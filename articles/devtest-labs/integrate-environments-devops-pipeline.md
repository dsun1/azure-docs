---
title: Integrate environments into Azure Pipelines
description: Learn how to integrate Azure DevTest Labs environments into your Azure DevOps continuous integration (CI) and continuous delivery (CD) pipelines. 
ms.topic: how-to
ms.date: 11/15/2021
---

# Integrate environments into your Azure DevOps CI/CD pipelines
You can use the Azure DevTest Labs Tasks extension that is installed in Azure DevOps Services (formerly known as Visual Studio Team Services) to easily integrate your continuous integration (CI)/ continuous delivery (CD) build-and-release pipeline with Azure DevTest Labs. These extensions make it easier to quickly deploy an [environment](devtest-lab-test-env.md) for a specific test task and then delete it when the test is finished. 

This article shows how to create and deploy an environment, then delete the environment, all in one complete pipeline. You would ordinarily perform each of these tasks individually in your own custom build-test-deploy pipeline. The extensions used in this article are in addition to these [create/delete DTL VM tasks](devtest-lab-integrate-ci-cd.md):

- Create an Environment
- Delete an Environment

## Prerequisites
Before you can integrate your CI/CD pipeline with Azure DevTest Labs, you must: 
1. Install [Azure DevTest Labs Tasks](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) extension from Visual Studio Marketplace. 
1. [Create a lab](devtest-lab-create-lab.md) and ensure configured to use **Public Environment**.

## Create a release definition & environment
To create the release definition, do the following:

1. In your Azure DevOps project, select **Releases** under the **Pipelines** section.
1. On the **Releases** tab, select **New pipeline**.  In the **Select a template** window on the right is a list of featured templates for common deployment patterns. 
1. For this pipeline, click **Empty Job** to start creating the environment to be used for development or testing purposes.
1. In the empty job, select **Tasks** in the toolbar, and then select **Stage 1**.

   :::image type="content" source="/media/integrate-environments-devops-pipeline/new-release-pipeline-stage.png" alt-text="Screenshot shows opening the pipeline release stage.":::

1. To add tasks, select the plus (+) sign in the **Agent job** section. A searchable list of available tasks appears. 
1. In the **Add task** window, search for `Azure DevTest Labs Create Environment`.
1. Select `Azure DevTest Labs Create Environment` task in the results, and select **Add**.
1. Select the task you just added. The **Azure DevTest Labs Create Environment (Preview)** window appears.

   :::image type="content" source="/media/integrate-environments-devops-pipeline/new-release-pipeline-environment.png" alt-text="Screenshot shows the fields needed for Azure Pipelines environment for Azure DevTest Labs.":::

2. On the **Tasks** tab, configure the environment as follows:

   |Field|Description|
   |-----|-----------|
   |**Azure RM Subscription**|Select a connection in the **Available Azure Service Connections** list, or create a more restricted permissions connection to your Azure subscription. For more information, see [Azure Resource Manager service endpoint](/azure/devops/pipelines/library/service-endpoints).|
   |**Lab Name**|Select the name of a lab you created earlier (see prerequisites). Manually entering the friendly name will cause failures, use the drop-down lists to select the information.|
   |**Repository Name**|Select the default repository, `Public Environment Repo`, or one containing the template you want to use. Manually entering the friendly name will cause failures, use the drop-down lists to select the information.|
   |**Template Name**|Select the name of the environment that you saved to your source code repository. Manually entering the friendly name will cause failures, use the drop-down lists to select the information.| 
   |**Environment Name**|Enter a name to uniquely identify the environment instance within the lab.  It must be unique within the lab.|
   |**Parameters File** & **Parameter overrides**|Use to pass custom parameters to the environment. Either or both can be used to set the parameter values. For example, you can these to pass the encrypted password.|

## Delete the environment
The final stage is to delete the Environment that you deployed in your Azure DevTest Labs instance. You would ordinarily delete the environment after you execute the dev tasks or run the tests that you need on the deployed resources.

In the release definition, select **Add tasks**, and then on the **Deploy** tab, add an **Azure DevTest Labs Delete Environment** task. Configure it as follows:

1. To delete the VM, see [Azure DevTest Labs Tasks](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks):
    1. For **Azure RM Subscription**, select a connection in the **Available Azure Service Connections** list, or create a more restricted permissions connection to your Azure subscription. For more information, see [Azure Resource Manager service endpoint](/azure/devops/pipelines/library/service-endpoints).
    2. For **Lab Name**, select the lab where the environment exists.
    3. For **Environment Name**, enter the name of the environment to be removed.
2. Enter a name for the release definition, and then save it.

## Next steps
See the following articles: 
- [Create multi-VM environments with Resource Manager templates](devtest-lab-create-environment-from-arm.md).
- Quickstart Resource Manager templates for DevTest Labs automation from the [DevTest Labs GitHub repository](https://github.com/Azure/azure-quickstart-templates).
- [VSTS Troubleshooting page](/azure/devops/pipelines/troubleshooting)
