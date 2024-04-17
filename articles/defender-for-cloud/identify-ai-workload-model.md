---
title: Discover generative AI applications
description: Learn how to use the cloud security explorer to determine which AI workloads and models are running in your environment.
ms.topic: how-to
ms.date: 04/17/2024
# customer intent: As a user, I want to learn how to identify AI workloads and models in my environment so that I can assess their security posture.
---

# Discover generative AI applications

Defender for Cloud provides a comprehensive view of your cloud environment, including the generative Artificial Intelligence (AI) workloads and models running in your environment. By using the cloud security explorer, you can identify the AI workloads and models that are running in your environment and assess their security posture.

## Prerequisites

- You need a Microsoft Azure subscription. If you don't have an Azure subscription, you can [sign up for a free subscription](https://azure.microsoft.com/pricing/free-trial/).

- [Enable Defender for Cloud on your Azure subscription](connect-azure-subscription.md).

- Enable [Defender Cloud Security Posture Management (CSPM)](tutorial-enable-cspm-plan.md) on your Azure subscription.

- Have at least one [Azure OpenAI resource](../ai-studio/how-to/create-azure-ai-resource.md), with at least one [model deployment](../ai-studio/how-to/deploy-models-openai.md) connected to it via Azure AI Studio.

## Discover workloads and models

If you have multiple generative AI workloads and models running in your environment, you can use the cloud security explorer to identify them. 

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Search for and select **Microsoft Defender for Cloud** > **Cloud Security Explorer**.

1. Select the **AI workloads and models in use** query template.

    :::image type="content" source="media/identify-ai-workload-model/ai-workload-query.png" alt-text="Screenshot that shows where to locate the AI workloads and models in use query template in the Cloud Security Explorer page." lightbox="media/identify-ai-workload-model/ai-workload-query.png":::

1. Select **Search**.

1. Select a result to review its details.

    :::image type="content" source="media/identify-ai-workload-model/result-details.png" alt-text="Screenshot of the results of the query with one of the results selected and the results detail pane open." lightbox="media/identify-ai-workload-model/result-details.png":::

1. Select a node to review the findings.

    :::image type="content" source="media/identify-ai-workload-model/additional-resource-details.png" alt-text="Screenshot of the results with a different resource selected and its results are displayed." lightbox="media/identify-ai-workload-model/additional-resource-details.png":::

With the information provided in the results, you can determine deployed models and specific model versions that are running on your resources.

## Next step

> [!div class="nextstepaction"]
> [Explore risks to generative AI applications](explore-ai-risk.md)
