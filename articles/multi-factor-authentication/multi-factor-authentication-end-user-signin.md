<properties
    pageTitle="Azure MFA Signin experience with Azure Multi-Factor Authentication"
    description="This page will provide you guidance on where to go to see the various signin methods available with Azure MFA."
    keywords="user authentication, sign-in experience, sign-in with mobile phone, sign-in with office phone"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>The sign in experience with Azure Multi-Factor Authentication
> [AZURE.NOTE]  The following documentation provided on this page shows a typical sign-in experience.  For help with signing in see [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>What will your sign in experience be?
Depending on how you sign in and use multi-factor authentication, your experience will differ.  In this section we will provide information on what to expect when you sign in.  Choose the one that best describes what you are doing:


What are you doing?|Description
:------------- | :------------- |
[Signing in with mobile or office phone](#signing-in-with-mobile-or-office-phone) | This is what you can expect from signing in using your mobile or office phone.
[Signing in with the Microsoft Authenticator app using notification](#signing-in-with-the-microsoft-authenticator-app-using-notification) | This is what you can expect using the Microsoft Authenticator app with notifications.
[Signing in with the Microsoft Authenticator app using verification code](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|This is what you can expect using the Microsoft Authenticator thapp with a verification code.
[Signing in with an alternate method](#signing-in-with-an-alternate-method)|This will show you what to expect if you want to use an alternate method.

## <a name="signing-in-with-mobile-or-office-phone"></a>Signing in with mobile or office phone

The following information will describe the experience of using multi-factor authentication with your mobile or office phone.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>To sign in with a call to your office or mobile phone

- Sign in to an application or service such as Office 365 using your user name and password.
- Microsoft will call you.

![Microsoft calls](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Answer the phone and hit the # key.

![Answer](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- You should now be signed in.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Signing in with the Microsoft Authenticator app using notification

The following information will describe the experience of using multi-factor authentication with the Microsoft Authenticator app when you are sent a notification.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>To sign in with a notification sent the Microsoft Authenticator app

- Sign in to an application or service such as Office 365 using your user name and password.
- Microsoft will send a notification.

![Microsoft sends notification](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Answer the phone and hit the verify key.  If your company requires a PIN you will be asked for it here.

![Verify](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- You should now be signed in.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Signing in with the Microsoft Authenticator app using verification code

The following information will describe the experience of using multi-factor authentication with the Microsoft Authenticator app when you are using it with a verification code.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>To sign in using a verification code with the Microsoft Authenticator app

- Sign in to an application or service such as Office 365 using your user name and password.
- Microsoft will prompt you for a verification code.

![Enter verification code](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Open the Microsoft Authenticator app on your phone and enter the code in the box where you are signing in.

![Get code](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- You should now be signed in.


## <a name="signing-in-with-an-alternate-method"></a>Signing in with an alternate method


The following section will show you how to sign in with an alternate method when your primary method may not be available.

### <a name="to-sign-in-with-an-alternate-method"></a>To sign in with an alternate method

- Sign in to an application or service such as Office 365 using your user name and password.
- Select use a different verification option.  You will be present with a choice of different options. The number you see will be based on how many you have setup.

![Use alternate method](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Choose an alternate method and sign in.