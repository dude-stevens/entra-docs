---
title: 'Tutorial: Configure Contentstack for automatic user provisioning with Microsoft Entra ID'
description: Learn how to automatically provision and deprovision user accounts from Microsoft Entra ID to Contentstack.

author: twimmers
writer: twimmers
manager: jeedes
ms.assetid: 1898e52a-94da-4512-ab88-25ce81cd226b
ms.service: entra-id
ms.subservice: saas-apps

ms.topic: tutorial
ms.date: 02/12/2024
ms.author: thwimmer
---

# Tutorial: Configure Contentstack for automatic user provisioning

This tutorial describes the steps you need to perform in both Contentstack and Microsoft Entra ID to configure automatic user provisioning. When configured, Microsoft Entra ID automatically provisions and deprovisions users to [Contentstack](https://www.contentstack.com/) using the Microsoft Entra provisioning service. For important details on what this service does, how it works, and frequently asked questions, see [Automate user provisioning and deprovisioning to SaaS applications with Microsoft Entra ID](~/identity/app-provisioning/user-provisioning.md). 


## Supported capabilities
> [!div class="checklist"]
> * Create users in Contentstack.
> * Remove users in Contentstack when they do not require access anymore.
> * Keep user attributes synchronized between Microsoft Entra ID and Contentstack.
> * [Single sign-on](contentstack-tutorial.md) to Contentstack (recommended).

## Prerequisites

The scenario outlined in this tutorial assumes that you already have the following prerequisites:

* [A Microsoft Entra tenant](~/identity-platform/quickstart-create-new-tenant.md) 
* A user account in Microsoft Entra ID with [permission](~/identity/role-based-access-control/permissions-reference.md) to configure provisioning (for example, Application Administrator, Cloud Application administrator, Application Owner, or Global Administrator).
* A user account in Contentstack with at least User Admin permissions.

## Plan your provisioning deployment

* Learn about [how the provisioning service works](~/identity/app-provisioning/user-provisioning.md).
* Determine who will be in [scope for provisioning](~/identity/app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
* Determine what data to [map between Microsoft Entra ID and Contentstack](~/identity/app-provisioning/customize-application-attributes.md).

## Configure Contentstack to support provisioning with Microsoft Entra ID

Contact [Contentstack support](https://Contentstack.atlassian.net/servicedesk/customer/portal/2) to open a service request to enable Contentstack to support provisioning with Microsoft Entra ID. Ensure to provide the Contentstack network domain (e.g. acme.Contentstack.com) you want to enable user provisioning for. You will then get provided with the Tenant URL and Secret Token for authorization.

## Contentstack from the Microsoft Entra application gallery

Add Contentstack from the Microsoft Entra application gallery to start managing provisioning to Contentstack. If you have previously setup Contentstack for SSO, you can use the same application. However it's recommended that you create a separate app when testing out the integration initially. Learn more about adding an application from the gallery [here](~/identity/enterprise-apps/add-application-portal.md). 

## Define who will be in scope for provisioning 

The Microsoft Entra provisioning service allows you to scope who will be provisioned based on assignment to the application and or based on attributes of the user. If you choose to scope who will be provisioned to your app based on assignment, you can use the following [steps](~/identity/enterprise-apps/assign-user-or-group-access-portal.md) to assign users to the application. If you choose to scope who will be provisioned based solely on attributes of the user, you can use a scoping filter as described [here](~/identity/app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Start small. Test with a small set of users before rolling out to everyone. When scope for provisioning is set to assigned users, you can control this by assigning one or two users to the app. When scope is set to all users, you can specify an [attribute based scoping filter](~/identity/app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

* If you need more roles, you can [update the application manifest](~/identity-platform/howto-add-app-roles-in-apps.md) to add new roles.

## Configure automatic user provisioning to Contentstack 

This section guides you through the steps to configure the Microsoft Entra provisioning service to create, update, and disable users in Contentstack based on user assignments in Microsoft Entra ID.

<a name='to-configure-automatic-user-provisioning-for-Contentstack-in-azure-ad'></a>

### To configure automatic user provisioning for Contentstack in Microsoft Entra ID:

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) as at least a [Cloud Application Administrator](~/identity/role-based-access-control/permissions-reference.md#cloud-application-administrator).
1. Browse to **Identity** > **Applications** > **Enterprise applications**

	![Screenshot of Enterprise applications blade.](common/enterprise-applications.png)

1. In the applications list, select **Contentstack**.

	![Screenshot of the Contentstack link in the Applications list.](common/all-applications.png)

1. Select the **Provisioning** tab.

	![Screenshot of Provisioning tab.](common/provisioning.png)

1. Set the **Provisioning Mode** to **Automatic**.

	![Screenshot of Provisioning tab automatic.](common/provisioning-automatic.png)

1. Under the **Admin Credentials** section, input your Contentstack Tenant URL, Token Endpoint, Client Identifier and Client Secret. Click **Test Connection** to ensure Microsoft Entra ID can connect to M-Files. If the connection fails, ensure your M-Files account has Admin permissions and try again.

 	![Screenshot of Token.](./media/contentstack-provisioning-tutorial/test-connection.png)

1. In the **Notification Email** field, enter the email address of a person who should receive the provisioning error notifications and select the **Send an email notification when a failure occurs** check box.

	![Screenshot of Notification Email.](common/provisioning-notification-email.png)

1. Select **Save**.

1. Under the **Mappings** section, select **Synchronize Microsoft Entra users to Contentstack**.

1. Review the user attributes that are synchronized from Microsoft Entra ID to Contentstack in the **Attribute-Mapping** section. The attributes selected as **Matching** properties are used to match the user accounts in Contentstack for update operations. If you choose to change the [matching target attribute](~/identity/app-provisioning/customize-application-attributes.md), you need to ensure that the Contentstack API supports filtering users based on that attribute. Select the **Save** button to commit any changes.

   |Attribute|Type|Supported for filtering|Required by Contentstack|
   |---|---|---|---|
   |userName|String|&check;|&check;
   |name.givenName|String||
   |name.familyName|String||
   |active|Boolean||

1. Under the **Mappings** section, select **Synchronize Microsoft Entra groups to Contentstack**.

1. Review the group attributes that are synchronized from Microsoft Entra ID to Contentstack in the **Attribute-Mapping** section. The attributes selected as **Matching** properties are used to match the groups in Contentstack for update operations. Select the **Save** button to commit any changes.

   |Attribute|Type|Supported for filtering|Required by Contentstack
   |---|---|---|---|
   |displayName|String|&check;|&check;
   |members|Reference||

1. To configure scoping filters, refer to the following instructions provided in the [Scoping filter tutorial](~/identity/app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

1. To enable the Microsoft Entra provisioning service for Contentstack, change the **Provisioning Status** to **On** in the **Settings** section.

	![Screenshot of Provisioning Status Toggled On.](common/provisioning-toggle-on.png)

1. Define the users that you would like to provision to Contentstack by choosing the desired values in **Scope** in the **Settings** section.

	![Screenshot of Provisioning Scope.](common/provisioning-scope.png)

1. When you're ready to provision, click **Save**.

	![Screenshot of Saving Provisioning Configuration.](common/provisioning-configuration-save.png)

This operation starts the initial synchronization cycle of all users defined in **Scope** in the **Settings** section. The initial cycle takes longer to perform than subsequent cycles, which occur approximately every 40 minutes as long as the Microsoft Entra provisioning service is running. 

## Monitor your deployment

Once you've configured provisioning, use the following resources to monitor your deployment:

* Use the [provisioning logs](~/identity/monitoring-health/concept-provisioning-logs.md) to determine which users have been provisioned successfully or unsuccessfully
* Check the [progress bar](~/identity/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) to see the status of the provisioning cycle and how close it's to completion
* If the provisioning configuration seems to be in an unhealthy state, the application goes into quarantine. Learn more about quarantine states [here](~/identity/app-provisioning/application-provisioning-quarantine-status.md).

## More resources

* [Managing user account provisioning for Enterprise Apps](~/identity/app-provisioning/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Microsoft Entra ID?](~/identity/enterprise-apps/what-is-single-sign-on.md)

## Next steps

* [Learn how to review logs and get reports on provisioning activity](~/identity/app-provisioning/check-status-user-account-provisioning.md)
