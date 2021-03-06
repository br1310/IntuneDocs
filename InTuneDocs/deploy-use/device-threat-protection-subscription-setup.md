---
# required metadata

title: Setup your subscription with Lookout | Microsoft Docs
description: This topics provides details on how to configure Lookout device threat protection.
keywords:
author: andredm7
ms.author: andredm
manager: angrobe
ms.date: 12/19/2016
ms.topic: article
ms.prod:
ms.service: microsoft-intune
ms.technology:
ms.assetid: 8477a2f1-2e1d-4d42-8bcb-e1181cc900bb

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: sandera
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: intune-classic

---

# Set up your subscription for Lookout device threat protection

[!INCLUDE[classic-portal](../includes/classic-portal.md)]

The following steps are required to set up Lookout device threat protection:

| #        |Step  |
| ------------- |:-------------|
| 1 | [Collect Azure AD information](#collect-azure-ad-information) |
| 2 | [Configure your subscription](#configure-your-subscription) |
| 3 | [Configure enrollment groups](#configure-enrollment-groups) |
| 4 | [Configure state sync](#configure-state-sync) |
| 5 | [Configure error report email recipient information](#configure-error-report-email-recipient-information) |
| 6 | [Configure enrollment settings](#configure-enrollment-settings) |
| 7 | [Configure email notifications](#configure-email-notifications) |
| 8 | [onfigure threat classification](#configure-threat-classification) |
| 1 | [Watching enrollment](#watching-enrollment) |


> [!IMPORTANT]
> An existing Lookout Mobile Endpoint Security tenant that is not already associated with your Azure AD tenant cannot be used for the integration with Azure AD and Intune. Contact Lookout support to create a new Lookout Mobile Endpoint Security tenant. Use the new tenant to onboard your Azure AD users.

## Collect Azure AD information
Your Lookout Mobility Endpoint Security tenant will be associated with your Azure AD subscription to integrate Lookout with Intune. To enable your Lookout device threat protection service subscription, Lookout support (enterprisesupport@lookout.com) needs the following information:  

* **Azure AD Tenant ID**
* **Azure AD Group Object ID** for **full** Lookout console access
* **Azure AD Group Object ID** for **restricted** Lookout console access (optional)

Use the following steps to gather the information you need to give to the Lookout support team.

1. Sign in to the [Azure AD management portal](https://manage.windowsazure.com) and select your subscription. 
  ![screenshot of the Azure AD page showing the name of the tenant](../media/mtp/aad_tenant_name.png)
2. When you choose the name of your subscription, the resulting URL includes the subscription ID.  If you have any issues finding your subscription ID, see this [Microsoft support article](https://support.office.com/en-us/article/Find-your-Office-365-tenant-ID-6891b561-a52d-4ade-9f39-b492285e2c9b?ui=en-US&rs=en-US&ad=US) for tips on finding your subscription ID. 
3. Find your Azure AD Group ID. The Lookout console supports 2 levels of access:  
  * **Full Access:** The Azure AD admin can create a group for users that will have Full Access and optionally create a group for users that will have Restricted Access.  Only users in these groups will be able to login to the **Lookout console**.
  * **Restricted Access:** The users in this group will have no access to several configuration and enrollment related modules of the Lookout console, and have read-only access to the **Security Policy** module of the Lookout console.  

  For more details on the permissions, read [this article](https://personal.support.lookout.com/hc/en-us/articles/114094105653) on the Lookout website.

  The **Group Object ID** is on the **Properties** page of the group in the **Azure AD management console**.

  ![screenshot of the properties page with GroupID field highlighted](../media/mtp/aad_group_object_id.png)

4. Once you have gathered this information, contact Lookout support (email: enterprisesupport@lookout.com). Lookout Support will work with your primary contact to onboard your subscription and create your Lookout Enterprise account, using the information that you collected.

## Configure your subscription
1. After Lookout support creates your Lookout Enterprise account, an email from Lookout is sent to the primary contact for your company with a link to the login url:https://aad.lookout.com/les?action=consent.

2.  The first login to the Lookout console must be by with a user account with the Azure AD role of Global Admin to register your Azure AD tenant. Later, sign in doesn't this level of Azure AD privilege. A consent page is displayed. Choose **Accept** to complete the registration.

  ![screenshot of the first time login page of the Lookout console](../media/mtp/lookout_mtp_initial_login.png)
  Once you have accepted and consented, you are redirected to the Lookout Console.

  See [troubleshooting Lookout integration](https://docs.microsoft.com/intune/troubleshoot/troubleshooting-lookout-integration) for help with login problems.

3.  In the [Lookout Console](https://aad.lookout.com), from the **System** module, choose the **Connectors** tab, and select **Intune**.

  ![screenshot of the Lookout console with the connectors tab open, and Intune option highlighted](../media/mtp/lookout_mtp_setup-intune-connector.png)

4.  Go **Connectors** > **Connection Settings** and specify the **Heartbeat Frequency** in minutes.

  ![screenshot of the connection settings tab with showing heartbeat frequency configured](../media/mtp/lookout-mtp-connection-settings.png)

## Configure enrollment groups
1. As a best practice, create an Azure AD security group in the [Azure AD management portal](https://manage.windowsazure.com) containing a small number of users to test Lookout integration.

  All the Lookout-supported, Intune-enrolled devices of users in an enrollment group in Azure AD that are identified and supported are enrolled and eligible for activation in Lookout device threat protection.

2. In the [Lookout Console](https://aad.lookout.com), from the **System** module, choose the **Connectors** tab, and select **Enrollment Management** to define a set of users whose devices should be enrolled with Lookout. Add the Azure AD security group **Display Name** for enrollment.

  ![screenshot of the Intune connector enrollment page](../media/mtp/lookout-mtp-enrollment.png)

  >[!IMPORTANT]
  > The  **Display Name** is case sensitive as shown the in the **Properties** of the security group in the Azure portal. As shown in the image below, the **Display Name** of the security group is camel case while the title is all lower case. In the Lookout console match the **Display Name** case for the security group.
  >![screenshot of the Azure portal, Azure Active Directory service, properties page](../media/mtp/aad-group-display-name.png)

  The best practice is to use the default (5 minutes) for the increment of time to check for new devices.

  **Current limitations:**
  * Lookout cannot validate group display names.  Ensure the **DISPLAY NAME** field in the Azure portal exactly matches the Azure AD security group.
  * Creating nest groups is not supported.  Azure AD security groups used in Lookout must contain users only. They cannot contain other groups.

3.  Once a group is added, the next time a user opens the Lookout for Work app on their supported device, the device is activated in Lookout.

4.  Once you are satisfied with your results, extend enrollment to additional user groups.

## Configure state sync
In the **State Sync** option, specify the type of data that should be sent to Intune.  Both device status and threat status are required for the Lookout Intune integration to work correctly.  These are enabled by default.

## Configure error report email recipient information
In the **Error Management** option, enter the email address that should receive the error reports.

![screenshot of the Intune connector error management page](../media/mtp/lookout-mtp-connector-error-notifications.png)

## Configure enrollment settings
In the **System** module, on the **Connectors** page, specify the number of days before a device is considered as disconnected.  Disconnected devices are considered as non-compliant and will be blocked from accessing your company applications based on the Intune conditional access policies. You can specify values between 1 and 90 days.

![](../media/mtp/lookout-console-enrollment-settings.png)

## Configure email notifications
If you want to receive email alerts for threats, sign in to the [Lookout console](https://aad.lookout.com) with the user account that should receive notifications. On the **Preferences** tab of the **System** module, choose the threat levels that should  notifications and set them to **ON**. Save your changes.

![screenshot of the preferences page with the user account displayed](../media/mtp/lookout-mtp-email-notifications.png)
If you no longer want to receive email notifications, set the notifications to **OFF** and save your changes.

### Configure threat classification
Lookout device threat protection classifies mobile threats of various types. The [Lookout threat classifications](http://personal.support.lookout.com/hc/en-us/articles/114094130693) have default risk levels associated with them. These can be changed at any time to suit your company requirements.

![screenshot of the policy page showing threat and classifications](../media/mtp/lookout-mtp-threat-classification.png)

>[!IMPORTANT]
> Risk levels are an important aspect of device threat protection because the Intune integration calculates device compliance according to these risk levels at runtime. The Intune administrator sets a rule in policy to identify a device as non-compliant if the device has an active threat with a minimum level of **High**, **Medium**, or **Low**. The threat classification policy in Lookout device threat protection directly drives the device compliance calculation in Intune.

## Watching enrollment
Once the setup is complete, Lookout device threat protection starts to poll Azure AD for devices that correspond to the specified enrollment groups.  You can find information about the devices enrolled on the Devices module.  The initial status for devices is shown as pending.  The device status changes once the Lookout for Work app is installed, opened, and activated on the device.  For details on how to get the Lookout for Work app pushed to the device, see the [Configure and deploy Lookout for work apps](configure-and-deploy-lookout-for-work-apps.md) topic.
## Next steps
[Enable Lookout MTP connection Intune](enable-lookout-mtp-connection-in-intune.md)
