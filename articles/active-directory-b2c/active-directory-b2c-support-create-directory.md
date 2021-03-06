<properties
    pageTitle="Azure Active Directory: Create tenant support topic | Microsoft Azure"
    description="Creating an Azure Active Directory tenant or an Azure Active Directory B2C tenant: Issues and resolutions"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="creating-an-azure-active-directory-azure-ad-tenant-or-azure-ad-b2c-tenant-issues-and-resolutions"></a>Creating an Azure Active Directory (Azure AD) tenant or Azure AD B2C tenant: Issues and resolutions

## <a name="creating-an-azure-ad-tenant"></a>Creating an Azure AD tenant

If you can't create an Azure AD tenant the first time, please try again. If the problem persists, contact Support.

## <a name="creating-an-azure-ad-b2c-tenant"></a>Creating an Azure AD B2C tenant

If you encounter issues during the [creation of an Azure AD B2C tenant](active-directory-b2c-get-started.md), try the following:
 
- If the Azure AD B2C tenant doesn't show up in your list of tenants, please try again.
- If the Azure AD B2C tenant does show up in your list of tenants and you receive an error message ("Could not complete the creation of the B2C tenant 'contosob2c'. Please visit this [link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) for more guidance."), delete the tenant that you just created and try again.
- Note that there are known issues when you delete an existing B2C tenant and re-create it with the same domain name. You have to create a B2C tenant with a different domain name.
- If these resolutions don't work for you, contact Support. Learn more about [how to file support requests for Azure AD B2C](active-directory-b2c-support.md).
