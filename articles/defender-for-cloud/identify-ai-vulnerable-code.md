---
title: Identify vulnerabilities on AI code repositories
description: Learn how to use the cloud security explorer to identify code repositories with Generative AI vulnerabilities and set up Azure OpenAI.
ms.topic: how-to
ms.date: 04/15/2024
# customer intent: As a user, I want to learn how to identify all code repositories within my environment, that contain known Generative AI vulnerabilities and set up Azure OpenAI so that I can assess their security posture.
---

# Identify vulnerabilities on AI code repositories

Defender for Cloud provides a comprehensive view of your cloud environment, including the generative artificial intelligence (GenAI) code repositories within your environment that contain known GenAI vulnerabilities and set up Azure OpenAI. By using the cloud security explorer, you can all of these repositories that exist in your environment and assess their security posture.

## Prerequisites

- You need a Microsoft Azure subscription. If you don't have an Azure subscription, you can [sign up for a free subscription](https://azure.microsoft.com/pricing/free-trial/).

- [Enable Defender for Cloud on your Azure subscription](connect-azure-subscription.md).

- Enable [Defender Cloud Security Posture Management (CSPM)](tutorial-enable-cspm-plan.md) on your Azure subscription.

- Have at least one [Azure OpenAI resource](../ai-studio/how-to/create-azure-ai-resource.md), with at least one [model deployment](../ai-studio/how-to/deploy-models-openai.md) connected to it via Azure AI Studio.

## Identify code repositories with vulnerabilities

If you have multiple GenAI code repositories within your environment, you can use the cloud security explorer to identify them.

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Search for and select **Microsoft Defender for Cloud** > **Cloud Security Explorer**.

1. Select the **GenAI vulnerable code repositories that provision Azure OpenAI** query template.

    :::image type="content" source="media/identify-ai-vulnerable-code/gen-ai-vulnerable-code-query.png" alt-text="Screenshot that shows where to locate the GenAI vulnerable code repositories query." lightbox="media/identify-ai-vulnerable-code/gen-ai-vulnerable-code-query.png":::

1. Select **Search**.

1. Select a result to review its details.

    :::image type="content" source="media/identify-ai-vulnerable-code/vulnerable-results.png" alt-text="Screenshot that shows a sample of results for the query." lightbox="media/identify-ai-vulnerable-code/vulnerable-results.png":::

1. Select a node to review the findings.

    :::image type="content" source="media/identify-ai-vulnerable-code/vulnerable-results-details.png" alt-text="Screenshot that shows the details of the selected node." lightbox="media/identify-ai-vulnerable-code/vulnerable-results-details.png":::

1. In the insights section, select a CVE ID from the drop-down menu.

1. Select **Open the vulnerability page**.

1. [Remediate the recommendation](implement-security-recommendations.md#remediate-recommendations).

## Next step

> [!div class="nextstepaction"]
> [Identify GenAI vulnerable container images](identify-ai-container-image.md)
