<properties
    pageTitle="Azure API Management FAQ | Microsoft Azure"
    description="Learn the answers to common questions, patterns, and best practices in Azure API Management."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure API Management FAQs

Get the answers to common questions, patterns, and best practices for Azure API Management.

## <a name="frequently-asked-questions"></a>Frequently asked questions

-   [How can I ask the Microsoft Azure API Management team a question?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [What does it mean when a feature is in preview?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [How can I secure the connection between the API Management gateway and my back-end services?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [How do I copy my API Management service instance to a new instance?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Can I manage my API Management instance programmatically?](#can-i-manage-my-api-management-instance-programmatically)
-   [How do I add a user to the Administrators group?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Why is the policy that I want to add unavailable in the policy editor?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [How do I use API versioning in API Management?](#how-do-i-use-api-versioning-in-api-management)
-   [How do I set up multiple environments in a single API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Can I use SOAP with API Management?](#can-i-use-soap-with-api-management)
-   [Is the API Management gateway IP address constant? Can I use it in firewall rules?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Can I configure an OAuth 2.0 authorization server with AD FS security?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [What routing method does API Management use in deployments to multiple geographic locations?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Can I use an Azure Resource Manager template to create an API Management service instance?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Can I use a self-signed SSL certificate for a back end?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Why do I get an authentication failure when I try to clone a GIT repository?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Does API Management work with Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Can I move an API Management service from one subscription to another?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>How can I ask the Microsoft Azure API Management team a question?

You can contact us by using one of these options:

-   Post your questions in our [API Management MSDN forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Send an email to <apimgmt@microsoft.com>.
-   Send us a feature request in the [Azure feedback forum](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>What does it mean when a feature is in preview?

When a feature is in preview, it means that we're actively seeking feedback on how the feature is working for you. A feature in preview is functionally complete, but it's possible that we'll make a breaking change in response to customer feedback. We recommend that you don't depend on a feature that is in preview in your production environment. If you have any feedback on preview features, please let us know through one of the contact options in [How can I ask the Microsoft Azure API Management team a question?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>How can I secure the connection between the API Management gateway and my back-end services?

You have several options to secure the connection between the API Management gateway and your back-end services. You can:

-   Use HTTP basic authentication. For more information, see [Configure API settings](api-management-howto-create-apis.md#configure-api-settings).
- Use SSL mutual authentication as described in [How to secure back-end services by using client certificate authentication in Azure API Management](api-management-howto-mutual-certificates.md).
- Use IP whitelisting on your back-end service. If you have a Standard or Premium tier API Management instance, the IP address of the gateway remains constant. You can set your whitelist to allow this IP address. You can get the IP address of your API Management instance on the Dashboard in the Azure portal.
- Connect your API Management instance to an Azure Virtual Network. For more information, see [How to set up VPN connections in Azure API Management](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>How do I copy my API Management service instance to a new instance?

You have several options if you want to copy an API Management instance to a new instance. You can:

-   Use the backup and restore function in API Management. For more information, see [How to implement disaster recovery by using service backup and restore in Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
-   Create your own backup and restore feature by using the [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Use the REST API to save and restore the entities from the service instance that you want.
-   Download the service configuration by using Git, and then upload it to a new instance. For more information, see [How to save and configure your API Management service configuration by using Git](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Can I manage my API Management instance programmatically?

Yes, you can manage API Management programmatically by using:

-   The [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   The [Microsoft Azure ApiManagement Service Management Library SDK](http://aka.ms/apimsdk).
-   The [Service deployment](https://msdn.microsoft.com/library/mt619282.aspx) and [Service management](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>How do I add a user to the Administrators group?

Here's how you can add a user to the Administrators group:

1. Sign in to the [Azure portal](https://portal.azure.com).
2. Go to the resource group that has the API Management instance you want to update.
3. In API Management, assign the **Api Management Contributor** role to the user.

Now the newly added contributor can use Azure PowerShell [cmdlets](https://msdn.microsoft.com/library/mt613507.aspx). Here's how to sign in as an administrator:

1. Use the `Login-AzureRmAccount` cmdlet to sign in.
2. Set the context to the subscription that has the service by using `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Get a single sign-on URL by using `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Use the URL to access the admin portal.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Why is the policy that I want to add unavailable in the policy editor?

If the policy that you want to add appears dimmed or shaded in the policy editor, be sure that you are in the correct scope for the policy. Each policy statement is designed for you to use in specific scopes and policy sections. To review the policy sections and scopes for a policy, see the policy's Usage section in [API Management policies](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>How do I use API versioning in API Management?

You have a few options to use API versioning in API Management:

-   In API Management, you can configure APIs to represent different versions. For example, you might have two different APIs, MyAPIv1 and MyAPIv2. A developer can choose the version that the developer wants to use.
-   You also can configure your API with a service URL that doesn't include a version segment, for example, https://my.api. Then, configure a version segment on each operation's [Rewrite URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) template. For example, you can have an operation with a [URL template](api-management-howto-add-operations.md#url-template) called /resource and a [Rewrite URL](api-management-howto-add-operations.md#rewrite-url-template) template called /v1/Resource. You can change the version segment value separately for each operation.
-   If you'd like to keep a "default" version segment in the API's service URL, on selected operations, set a policy that uses the [Set backend service](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) policy to change the back-end request path.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>How do I set up multiple environments in a single API?

To set up multiple environments, for example, a test environment and a production environment, in a single API, you have two options. You can:

-   Host different APIs on the same tenant.
-   Host the same APIs on different tenants.

### <a name="can-i-use-soap-with-api-management"></a>Can I use SOAP with API Management?

[SOAP pass-through](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) support is now available. Administrators can import the WSDL of their SOAP service, and Azure API Management will create a SOAP front end. Developer portal documentation, test console, policies and analytics are all available for SOAP services.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Is the API Management gateway IP address constant? Can I use it in firewall rules?

At the Standard and Premium tiers, the public IP address (VIP) of the API Management tenant is static for the lifetime of the tenant, with some exceptions. The IP address changes in these circumstances:

-   The service is deleted and then re-created.
-   The service subscription is suspended (for example, for nonpayment) and then reinstated.
-   You add or remove Azure Virtual Network (you can use Virtual Network only at the Premium tier).

For multi-region deployments, the regional address changes if the region is vacated and then reinstated (you can use multi-region deployment only at the Premium tier).

Premium tier tenants that are configured for multi-region deployment are assigned one public IP address per region.

You can get your IP address (or addresses, in a multi-region deployment) on the tenant page in the Azure portal.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Can I configure an OAuth 2.0 authorization server with AD FS security?

To learn how to configure an OAuth 2.0 authorization server with Active Directory Federation Services (AD FS) security, see [Using ADFS in API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>What routing method does API Management use in deployments to multiple geographic locations?

API Management uses the [performance traffic routing method](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) in deployments to multiple geographic locations. Incoming traffic is routed to the closest API gateway. If one region goes offline, incoming traffic is automatically routed to the next closest gateway. Learn more about routing methods in [Traffic Manager routing methods](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Can I use an Azure Resource Manager template to create an API Management service instance?

Yes. See the [Azure API Management Service](http://aka.ms/apimtemplate) QuickStart templates.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Can I use a self-signed SSL certificate for a back end?

Yes. Here's how to use a self-signed Secure Sockets Layer (SSL) certificate for a back end:

1. Create a [Backend](https://msdn.microsoft.com/library/azure/dn935030.aspx) entity by using API Management.
2. Set the **skipCertificateChainValidation** property to **true**.
3. If you no longer want to allow self-signed certificates, delete the Backend entity, or set the **skipCertificateChainValidation** property to **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Why do I get an authentication failure when I try to clone a Git repository?

If you use Git Credential Manager, or if you're trying to clone a Git repository by using Visual Studio, you might run into a known issue with the Windows credentials dialog box. The dialog box limits password length to 127 characters, and it truncates the Microsoft-generated password. We are working on shortening the password. For now, please use Git Bash to clone your Git repository.

### <a name="does-api-management-work-with-azure-expressroute"></a>Does API Management work with Azure ExpressRoute?

Yes. API Management works with Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Can I move an API Management service from one subscription to another?

Yes. To learn how, see [Move resources to a new resource group or subscription](../resource-group-move-resources.md).
