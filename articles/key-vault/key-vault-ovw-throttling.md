---
title: Azure Key Vault throttling guidance
description: Key Vault throttling limits the number of concurrent calls to prevent overuse of resources.
services: key-vault
author: msmbaldwin
manager: rkarlin

ms.service: key-vault
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mbaldwin

---

# Azure Key Vault throttling guidance

Throttling is a process you initiate that limits the number of concurrent calls to the Azure service to prevent overuse of resources. Azure Key Vault (AKV) is designed to handle a high volume of requests. If an overwhelming number of requests occurs, throttling your client's requests helps maintain optimal performance and reliability of the AKV service.

Throttling limits vary based on the scenario. For example, if you are performing a large volume of writes, the possibility for throttling is higher than if you are only performing reads.

## How does Key Vault handle its limits?

Service limits in Key Vault prevent misuse of resources and ensure quality of service for all of Key Vault's clients. When a service threshold is exceeded, Key Vault limits any further requests from that client for a period of time. When this happens, Key Vault returns HTTP status code 429 (Too many requests), and the requests fail. Also, failed requests that return a 429 count towards the throttle limits tracked by Key Vault. 

Key Vault was originally designed to be used to store and retrieve your secrets at deployment time.  The world has evolved, and Key Vault is being used at run-time to store and retrieve secrets, and often apps and services want to use Key Vault like a database.  Our current throughput limits do not currently support extremely high throughput rates.

Key Vault was originally created with the limits specified in [Azure Key Vault service limits](key-vault-service-limits.md).  To maximize your Key Vault through put rates, here are some recommended guidelines/best practices for maximizing your throughput:
1. Ensure you have throttling in place.  Client must honor exponential back-off policies for 429's and ensure you are doing retries as per the guidance below.
1. Divide your Key Vault traffic amongst multiple vaults and different regions.   Use a separate vault for each security/availability domain.   If you have 5 apps, each in 2 regions, then we recommend 10 vaults each containing the secrets unique to app and region.  A subscription-wide limit for all transaction types is five times the individual key vault limit. For example, HSM-other transactions per subscription are limited to 5,000 transactions in 10 seconds per subscription. Consider caching the secret within your service or app to also reduce the RPS directly to key vault and/or handle burst based traffic.  You can also divide your traffic amongst different regions to minimize latency and use a different subscription/vault.  Do not send more than the subscription limit to the Key Vault service in a single Azure region.
1. Cache the secrets you retrieve from Azure Key Vault in memory, and reuse from memory whenever possible.  Re-read from Azure Key Vault only when the cached copy stops working (e.g. because it got rotated at the source). 
1. Key Vault is designed for your own services secrets.   If you are storing your customers' secrets (especially for high-throughput key storage scenarios), consider putting the keys in a database or storage account with encryption, and storing just the master key in Azure Key Vault.  A sample for this is coming soon.
1. Encrypt, wrap, and verify  public-key operations can be performed with no access to Key Vault - This not only reduces risk of throttling but also improves reliability, as long as you properly cache the public key material
1. If you use Key Vault to store credentials for a service, check if that service supports AAD Authentication to authenticate directly. This reduces the load on Key Vault, improves reliability and simplifies your code since Key Vault can now use the AAD token.  Many services have moved to using AAD Auth.  See the current list at [Services that support managed identities for Azure resources](../active-directory/managed-identities-azure-resources/services-support-managed-identities#azure-services-that-support-managed-identities-for-azure-resources.md).
1. Consider staggering your load/deployment over a longer period of time to stay under the current RPS limits
1. If your app comprises multiple nodes that need to read the same secret(s), then consider using a fan out pattern, where one entity reads the secret from Key Vault, and fans out to all nodes.   Cache the retrieved secrets only in memory.
If you find that the above still does not meet your needs, please fill out the below table and contact us to determine what additional capacity can be added (example put below for illustrative purposes only)

| Vault name | Vault Region | Object type (Secret, Key, or Cert) | Operation(s) – Create, Update, Delete, etc* | Key Type | Key Length or Curve | HSM key?| Steady state RPS needed | Peak RPS needed |
|--|--|--|--|--|--|--|--|--|
| https://mykeyvault.vault.azure.net/ | | Key | Sign | EC | P-256 | No | 200 | 1000 |

*Key Vault Operations:
https://docs.microsoft.com/en-us/rest/api/keyvault/key-operations

If additional capacity is approved, please note the following as result of the capacity increases:
1. Data consistency model changes.  Once a vault is whitelisted with additional throughput capacity, the Key Vault service data consistency guarantee changes (necessary to meet higher volume RPS since the underlying Azure Storage service cannot keep up).  In a nutshell:
  1. Without whitelisting: The Key Vault service will reflect the results of a write operation (eg. SecretSet, CreateKey) immediately in subsequent calls (eg. SecretGet, KeySign).
  1. With whitelist: The Key Vault service will reflect the results of a write operation (eg. SecretSet, CreateKey) within 60 seconds in subsequent calls (eg. SecretGet, KeySign).
1. Client code must honor back-off policy for 429 retries. The client code calling the Key Vault service must not immediately retry Key Vault requests when it receives a 429 response code.  The Azure Key Vault throttling guidance published here recommends applying exponential backoff when receiving a 429 Http response code.

If you have a valid business case for higher throttle limits, please contact us.

## How to throttle your app in response to service limits

The following are **best practices** you should implement when your service is throttled:
- Reduce the number of operations per request.
- Reduce the frequency of requests.
- Avoid immediate retries. 
    - All requests accrue against your usage limits.

When you implement your app's error handling, use the HTTP error code 429 to detect the need for client-side throttling. If the request fails again with an HTTP 429 error code, you are still encountering an Azure service limit. Continue to use the recommended client-side throttling method, retrying the request until it succeeds.

Code that implements exponential backoff is shown below. 
```
    public sealed class RetryWithExponentialBackoff
    {
        private readonly int maxRetries, delayMilliseconds, maxDelayMilliseconds;

        public RetryWithExponentialBackoff(int maxRetries = 50,
            int delayMilliseconds = 200,
            int maxDelayMilliseconds = 2000)
        {
            this.maxRetries = maxRetries;
            this.delayMilliseconds = delayMilliseconds;
            this.maxDelayMilliseconds = maxDelayMilliseconds;
        }

        public async Task RunAsync(Func<Task> func)
        {
            ExponentialBackoff backoff = new ExponentialBackoff(this.maxRetries,
                this.delayMilliseconds,
                this.maxDelayMilliseconds);
            retry:
            try
            {
                await func();
            }
            catch (Exception ex) when (ex is TimeoutException ||
                ex is System.Net.Http.HttpRequestException)
            {
                Debug.WriteLine("Exception raised is: " +
                    ex.GetType().ToString() +
                    " –Message: " + ex.Message +
                    " -- Inner Message: " +
                    ex.InnerException.Message);
                await backoff.Delay();
                goto retry;
            }
        }
    }

    public struct ExponentialBackoff
    {
        private readonly int m_maxRetries, m_delayMilliseconds, m_maxDelayMilliseconds;
        private int m_retries, m_pow;

        public ExponentialBackoff(int maxRetries, int delayMilliseconds,
            int maxDelayMilliseconds)
        {
            m_maxRetries = maxRetries;
            m_delayMilliseconds = delayMilliseconds;
            m_maxDelayMilliseconds = maxDelayMilliseconds;
            m_retries = 0;
            m_pow = 1;
        }

        public Task Delay()
        {
            if (m_retries == m_maxRetries)
            {
                throw new TimeoutException("Max retry attempts exceeded.");
            }
            ++m_retries;
            if (m_retries < 31)
            {
                m_pow = m_pow << 1; // m_pow = Pow(2, m_retries - 1)
            }
            int delay = Math.Min(m_delayMilliseconds * (m_pow - 1) / 2,
                m_maxDelayMilliseconds);
            return Task.Delay(delay);
        }
    }
```


Using this code in a client C\# application is straightforward. The following example shows how, using the HttpClient class.

```csharp
public async Task<Cart> GetCartItems(int page)
{
    _apiClient = new HttpClient();
    //
    // Using HttpClient with Retry and Exponential Backoff
    //
    var retry = new RetryWithExponentialBackoff();
    await retry.RunAsync(async () =>
    {
        // work with HttpClient call
        dataString = await _apiClient.GetStringAsync(catalogUrl);
    });
    return JsonConvert.DeserializeObject<Cart>(dataString);
}
```

Remember that this code is suitable only as a proof of concept. 

### Recommended client-side throttling method

On HTTP error code 429, begin throttling your client using an exponential backoff approach:

1. Wait 1 second, retry request
2. If still throttled wait 2 seconds, retry request
3. If still throttled wait 4 seconds, retry request
4. If still throttled wait 8 seconds, retry request
5. If still throttled wait 16 seconds, retry request

At this point, you should not be getting HTTP 429 response codes.

## See also

For a deeper orientation of throttling on the Microsoft Cloud, see [Throttling Pattern](https://docs.microsoft.com/azure/architecture/patterns/throttling).

