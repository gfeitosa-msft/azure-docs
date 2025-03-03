---
title: 'Tutorial: Azure AD SSO integration with BenSelect'
description: Learn how to configure single sign-on between Azure Active Directory and BenSelect.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/27/2021
ms.author: jeedes
---

# Tutorial: Azure AD SSO integration with BenSelect

In this tutorial, you'll learn how to integrate BenSelect with Azure Active Directory (Azure AD). When you integrate BenSelect with Azure AD, you can:

* Control in Azure AD who has access to BenSelect.
* Enable your users to be automatically signed-in to BenSelect with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* BenSelect single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

* BenSelect supports **IDP** initiated SSO.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add BenSelect from the gallery

To configure the integration of BenSelect into Azure AD, you need to add BenSelect from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **BenSelect** in the search box.
1. Select **BenSelect** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

 Alternatively, you can also use the [Enterprise App Configuration Wizard](https://portal.office.com/AdminPortal/home?Q=Docs#/azureadappintegration). In this wizard, you can add an application to your tenant, add users/groups to the app, assign roles, as well as walk through the SSO configuration as well. [Learn more about Microsoft 365 wizards.](/microsoft-365/admin/misc/azure-ad-setup-guides)

## Configure and test Azure AD SSO for BenSelect

Configure and test Azure AD SSO with BenSelect using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in BenSelect.

To configure and test Azure AD SSO with BenSelect, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure BenSelect SSO](#configure-benselect-sso)** - to configure the single sign-on settings on application side.
    1. **[Create BenSelect test user](#create-benselect-test-user)** - to have a counterpart of B.Simon in BenSelect that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **BenSelect** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

1. On the **Basic SAML Configuration** section, perform the following step:

    In the **Reply URL** text box, type a URL using the following pattern:
    `https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`

	> [!NOTE]
	> The value is not real. Update the value with the actual Reply URL. Contact [BenSelect Client support team](mailto:support@selerix.com) to get the value. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

1. BenSelect application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration. The following screenshot shows the list of default attributes.

	![Screenshot shows User Attributes with default attributes such as givenname user.givenname and emailaddress user.mail.](common/edit-attribute.png)

1. Click on the **Edit** icon to edit the **Name identifier value**.

	![Screenshot shows the User Attributes & Claims pane with the Edit icon called out.](media/benselect-tutorial/mail-prefix1.png)

1. On the **Manage user claims** section, perform the following steps:

	![Screenshot shows Manage user claims where you can enter the values described in this step.](media/benselect-tutorial/mail-prefix2.png)

	a. Select **Transformation** as a **Source**.

	b. In the **Transformation** dropdown list, select **ExtractMailPrefix()**.

	c. In the **Parameter 1** dropdown list, select **user.userprincipalname**.

	d. Click **Save**.

1. On the **Set up single sign-on with SAML** page, in the **SAML Signing Certificate** section,  find **Certificate (Raw)** and select **Download** to download the certificate and save it on your computer.

	![The Certificate download link](common/certificateraw.png)

1. On the **Set up BenSelect** section, copy the appropriate URL(s) based on your requirement.

	![Copy configuration URLs](common/copy-configuration-urls.png)

### Create an Azure AD test user

In this section, you'll create a test user in the Azure portal called B.Simon.

1. From the left pane in the Azure portal, select **Azure Active Directory**, select **Users**, and then select **All users**.
1. Select **New user** at the top of the screen.
1. In the **User** properties, follow these steps:
   1. In the **Name** field, enter `B.Simon`.  
   1. In the **User name** field, enter the username@companydomain.extension. For example, `B.Simon@contoso.com`.
   1. Select the **Show password** check box, and then write down the value that's displayed in the **Password** box.
   1. Click **Create**.

### Assign the Azure AD test user

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to BenSelect.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **BenSelect**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you're expecting any role value in the SAML assertion, in the **Select Role** dialog, select the appropriate role for the user from the list and then click the **Select** button at the bottom of the screen.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure BenSelect SSO

To configure single sign-on on **BenSelect** side, you need to send the downloaded **Certificate (Raw)** and appropriate copied URLs from Azure portal to [BenSelect support team](mailto:support@selerix.com). They set this setting to have the SAML SSO connection set properly on both sides.

> [!NOTE]
> You need to mention that this integration requires the SHA256 algorithm (SHA1 is not supported) to set the SSO on the appropriate server like app2101 etc.

### Create BenSelect test user

In this section, you create a user called Britta Simon in BenSelect. Work with [BenSelect support team](mailto:support@selerix.com) to add the users in the BenSelect platform. Users must be created and activated before you use single sign-on.

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options.

* Click on Test this application in Azure portal and you should be automatically signed in to the BenSelect for which you set up the SSO.

* You can use Microsoft My Apps. When you click the BenSelect tile in the My Apps, you should be automatically signed in to the BenSelect for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure BenSelect you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Defender for Cloud Apps](/cloud-app-security/proxy-deployment-aad).
