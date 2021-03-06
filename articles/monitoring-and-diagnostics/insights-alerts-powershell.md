<properties
    pageTitle="Use PowerShell to create alerts for Azure services | Microsoft Azure"
    description="Use PowerShell to create Azure alerts, which can trigger notifications or automation when the conditions you specify are met."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Use PowerShell to create alerts for Azure services

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Overview

This article shows you how to set up Azure alerts using PowerShell.  

You can receive an alert based on monitoring metrics for, or events on, your Azure services.

- **Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction. That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.    
- **Activity log events** - An alert can trigger on *every* event, or, only when a certain number of events occur.

You can configure an alert to do the following when it triggers:

- send email notifications to the service administrator and co-administrators
- send email to additional emails that you specify.
- call a webhook
- start execution of an Azure runbook (only from the Azure portal)

You can configure and get information about alert rules using

- [Azure portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [command-line interface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


For additional information, you can always type ```get-help``` and then the PowerShell command you want help on.

## <a name="create-alert-rules-in-powershell"></a>Create alert rules in PowerShell

1. Log in to Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Get a list of the subscriptions you have available. Verify that you are working with the right subscription. If not, set it to the right one using the output from `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  To list existing rules on a resource group, use the following command:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. To create a rule, you need to have several important pieces of information first. 
    - The **Resource ID** for the resource you want to set an alert for
    - The **metric definitions** available for that resource

    One way to get the Resource ID is to use the Azure portal. Assuming the resource is already created, select it in the portal. Then in the next blade, select *Properties* under the *Settings* section. The RESOURCE ID is a field in the next blade. Another way is to use the [Azure Resource Explorer](https://resources.azure.com/).

    An example resource id for a web app is

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    You can use `Get-AzureRmMetricDefinition` to view the list of all metric definitions for a specific resource.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    The following example generates a table with the metric Name and the Unit for that metric.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    A full list of available options for Get-AzureRmMetricDefinition is available by running Get-MetricDefinitions.


5. The following example sets up an alert on a web site resource. The alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. To create webhook or send email when an alert triggers, first create the email and/or webhooks. Then immediately create the rule afterwards with the -Actions tag and as shown in the following example. You cannot associate webhook or emails with already created rules via PowerShell.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. To create an alert that triggers on a specific condition in the activity log, use commands of the following form

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    The -OperationName corresponds to a type of event in the activity log. Examples include *Microsoft.Compute/virtualMachines/delete* and *microsoft.insights/diagnosticSettings/write*.

    You can use the PowerShell command [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) to obtain a list of possible operationNames. Alternately, you can use the Azure portal to query the Activity log and find specific past operations that you want to create an alert for. The operations shown in the graphic log view of the friendly names. Look in the JSON for the entry and pull out the OperationName value.   

8. Verify that your alerts have been created properly by looking at the individual rules.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Delete your alerts. These commands delete the rules created previously in this article.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Next steps

* [Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.
* Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).
* Learn more about [Azure Automation Runbooks](..\automation\automation-starting-a-runbook.md).
* Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) to collect detailed high-frequency metrics on your service.
* Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.
