<properties
    pageTitle="Azure MFA cloud vs server | Microsoft Azure"
    description="Choose the multi-factor authentication secutiry solution that is right for you by asking what am i trying to secure and where are my users located.  Then choose cloud, MFA Server or AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Choose the Azure Multi-Factor Authentication solution for you

Because there are several flavors of Azure Multi-Factor Authentication (MFA), we must answer a few questions to figure out which version is the proper one to use.  Those questions are:

-   [What am I trying to secure](#what-am-i-trying-to-secure)
-   [Where are the users located](#where-are-the-users-located)
- [What features do I need?](#what-featured-do-i-need)

The following sections provide guidance on determining each of these answers.

## <a name="what-am-i-trying-to-secure"></a>What am I trying to secure?

To determine the correct two-step verification solution, first we must answer the question of what are you trying to secure with a second method of authentication.  Is it an application that is in Azure?  Or a remote access system?  By determining what we are trying to secure, we can answer the question of where Multi-Factor Authentication needs to be enabled.  


What are you trying to secure| Multi-Factor Authentication in the cloud|Multi-Factor Authentication Server
------------- | :-------------: | :-------------: |
First-party Microsoft apps|● |● |
SaaS apps in the app gallery|● |● |
IIS applications published through Azure AD App Proxy|● |● |
IIS applications not published through Azure AD App Proxy | |● |
Remote access such as VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Where are the users located

Next, looking at where our users are located helps to determine the correct solution to use, whether in the cloud or on-premises using the MFA Server.



User Location| Multi-Factor Authentication in the cloud|Multi-Factor Authentication Server
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD and on-premises AD using federation with AD FS|● |● |
Azure AD and on-premises AD using DirSync, Azure AD Sync, Azure AD Connect - no password sync|● |● |
Azure AD and on-premises AD using DirSync, Azure AD Sync, Azure AD Connect - with password sync|● | |
On-premises Active Directory| |● |

## <a name="what-features-do-i-need"></a>What features do I need?

The following table is a comparison of the features that are available with Multi-Factor Authentication in the cloud and with the Multi-Factor Authentication Server.

 | Multi-Factor Authentication in the cloud | Multi-Factor Authentication Server
------------- | :-------------: | :-------------: |
Mobile app notification as a second factor | ● | ● |
Mobile app verification code as a second factor | ● | ●
Phone call as second factor | ● | ●
One-way SMS as second factor | ● | ●
Two-way SMS as second factor |  | ●
Hardware Tokens as second factor |  | ●
App passwords for clients that don’t support MFA | ● |  
Admin control over authentication methods | ● | ●
PIN mode |  | ●
Fraud alert | ● | ●
MFA Reports | ● | ●
One-Time Bypass |  | ●
Custom greetings for phone calls | ● | ●
Customizable caller ID for phone calls | ● | ●
Trusted IPs | ● | ●
Remember MFA for trusted devices  | ● |  
Conditional access | ● | ●
Cache |  | ●

Now that we have determined whether to use cloud multi-factor authentication or the MFA Server on-premises, we can get started setting up and using Azure Multi-Factor Authentication. **Select the icon that represents your scenario!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
