---
title: Activate Azure resource roles in PIM
description: Learn how to activate your Azure resource roles in Microsoft Entra Privileged Identity Management (PIM).
author: barclayn
manager: amycolannino
ms.service: entra-id-governance
ms.topic: how-to
ms.date: 02/06/2024
ms.subservice: privileged-identity-management
ms.author: barclayn
ms.reviewer: rianakarim
ms.custom: pim

---

# Activate my Azure resource roles in Privileged Identity Management

Use Microsoft Entra Privileged Identity Management (PIM), to allow eligible role members for Azure resources to schedule activation for a future date and time. They can also select a specific activation duration within the maximum (configured by administrators).

This article is for members who need to activate their Azure resource role in Privileged Identity Management.

>[!NOTE]
>As of March 2023, you may now activate your assignments and view your access directly from blades outside of PIM in the Azure portal. Read more [here](pim-resource-roles-activate-your-roles.md#activate-with-azure-portal).

>[!IMPORTANT]
>When a role is activated, Microsoft Entra PIM temporarily adds active assignment for the role. Microsoft Entra PIM creates active assignment (assigns user to a role) within seconds. When deactivation (manual or through activation time expiration) happens, Microsoft Entra PIM removes the active assignment within seconds as well.
>
>Application may provide access based on the role the user has. In some situations, application access may not immediately reflect the fact that user got role assigned or removed. If application previously cached the fact that user does not have a role – when user tries to access application again, access may not be provided. Similarly, if application previously cached the fact that user has a role – when role is deactivated, user may still get access. Specific situation depends on the application’s architecture. For some applications, signing out and signing back in may help get access added or removed.

## Activate a role

[!INCLUDE [portal updates](~/includes/portal-update.md)]

When you need to take on an Azure resource role, you can request activation by using the **My roles** navigation option in Privileged Identity Management.

>[!NOTE]
> PIM is now available in the Azure mobile app (iOS | Android) for Microsoft Entra ID and Azure resource roles. Easily activate eligible assignments, request renewals for ones that are expiring, or check the status of pending requests. [Read more below](#activate-pim-roles-using-the-azure-mobile-app)


1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) as at least a [Privileged Role Administrator](~/identity/role-based-access-control/permissions-reference.md#privileged-role-administrator).

1. Browse to **Identity governance** > **Privileged Identity Management** > **My roles**.

    ![My roles page showing roles you can activate](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Select **Azure resource roles** to see a list of your eligible Azure resource roles.

    ![My roles - Azure resource roles page](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png)

1. In the **Azure resource roles** list, find the role you want to activate.

    ![Azure resource roles - My eligible roles list](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Select **Activate** to open the Activate page.

    ![The opened Activate pane with scope, start time, duration, and reason](./media/pim-resource-roles-activate-your-roles/azure-role-eligible-activate.png)

1. If your role requires multifactor authentication, select **Verify your identity before proceeding**. You only have to authenticate once per session.

1. Select **Verify my identity** and follow the instructions to provide additional security verification.

    ![Screen to provide security verification such as a PIN code](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. If you want to specify a reduced scope, select **Scope** to open the Resource filter pane.

    It's a best practice to only request access to the resources you need. On the Resource filter pane, you can specify the resource groups or resources that you need access to.

    ![Activate - Resource filter pane to specify scope](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. If necessary, specify a custom activation start time. The member would be activated after the selected time.

1. In the **Reason** box, enter the reason for the activation request.

1. Select **Activate**.

    >[!NOTE]
    >If the [role requires approval](pim-resource-roles-approval-workflow.md) to activate, a notification will appear in the upper right corner of your browser informing you the request is pending approval.

## Activate a role with ARM API

Privileged Identity Management supports Azure Resource Manager (ARM) API commands to manage Azure resource roles, as documented in the [PIM ARM API reference](/rest/api/authorization/role-eligibility-schedule-requests). For the permissions required to use the PIM API, see [Understand the Privileged Identity Management APIs](pim-apis.md).

To activate an eligible Azure role assignment and gain activated access, use the [Role Assignment Schedule Requests - Create REST API](/rest/api/authorization/role-assignment-schedule-requests/create?tabs=HTTP) to create a new request and specify the security principal, role definition, requestType = SelfActivate and scope. To call this API, you must have an eligible role assignment on the scope. 

Use a GUID tool to generate a unique identifier that will be used for the role assignment identifier. The identifier has the format: 00000000-0000-0000-0000-000000000000. 

Replace {roleAssignmentScheduleRequestName} in the below PUT request with the GUID identifier of the role assignment. 

For more details on managing eligible roles for Azure resources, see this [PIM ARM API tutorial](/rest/api/authorization/privileged-role-assignment-rest-sample?source=docs#activate-an-eligible-role-assignment). 

The following is a sample HTTP request to activate an eligible assignment for an Azure role.

### Request

````HTTP
PUT https://management.azure.com/providers/Microsoft.Subscription/subscriptions/dfa2a084-766f-4003-8ae1-c4aeb893a99f/providers/Microsoft.Authorization/roleAssignmentScheduleRequests/{roleAssignmentScheduleRequestName}?api-version=2020-10-01
````

### Request body

````JSON
{ 
"properties": { 
   "principalId": "a3bb8764-cb92-4276-9d2a-ca1e895e55ea", 
   "roleDefinitionId": "/subscriptions/dfa2a084-766f-4003-8ae1-c4aeb893a99f/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608", 
   "requestType": "SelfActivate", 
   "linkedRoleEligibilityScheduleId": "b1477448-2cc6-4ceb-93b4-54a202a89413", 
   "scheduleInfo": { 
       "startDateTime": "2020-09-09T21:35:27.91Z", 
       "expiration": { 
           "type": "AfterDuration", 
           "endDateTime": null, 
           "duration": "PT8H" 
       } 
   }, 
   "condition": "@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:ContainerName] StringEqualsIgnoreCase 'foo_storage_container'", 
   "conditionVersion": "1.0" 
 } 
} 
````

### Response

Status code: 201

````HTTP
{ 
  "properties": { 
    "targetRoleAssignmentScheduleId": "c9e264ff-3133-4776-a81a-ebc7c33c8ec6", 
    "targetRoleAssignmentScheduleInstanceId": null, 
    "scope": "/subscriptions/dfa2a084-766f-4003-8ae1-c4aeb893a99f", 
    "roleDefinitionId": "/subscriptions/dfa2a084-766f-4003-8ae1-c4aeb893a99f/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608", 
    "principalId": "a3bb8764-cb92-4276-9d2a-ca1e895e55ea", 
    "principalType": "User", 
    "requestType": "SelfActivate", 
    "status": "Provisioned", 
    "approvalId": null, 
    "scheduleInfo": { 
      "startDateTime": "2020-09-09T21:35:27.91Z", 
      "expiration": { 
        "type": "AfterDuration", 
        "endDateTime": null, 
        "duration": "PT8H" 
      } 
    }, 
    "ticketInfo": { 
      "ticketNumber": null, 
      "ticketSystem": null 
    }, 
    "justification": null, 
    "requestorId": "a3bb8764-cb92-4276-9d2a-ca1e895e55ea", 
    "createdOn": "2020-09-09T21:35:27.91Z", 
    "condition": "@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:ContainerName] StringEqualsIgnoreCase 'foo_storage_container'", 
    "conditionVersion": "1.0", 
    "expandedProperties": { 
      "scope": { 
        "id": "/subscriptions/dfa2a084-766f-4003-8ae1-c4aeb893a99f", 
        "displayName": "Pay-As-You-Go", 
        "type": "subscription" 
      }, 
      "roleDefinition": { 
        "id": "/subscriptions/dfa2a084-766f-4003-8ae1-c4aeb893a99f/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608", 
        "displayName": "Contributor", 
        "type": "BuiltInRole" 
      }, 
      "principal": { 
        "id": "a3bb8764-cb92-4276-9d2a-ca1e895e55ea", 
        "displayName": "User Account", 
        "email": "user@my-tenant.com", 
        "type": "User" 
      } 
    } 
  }, 
  "name": "fea7a502-9a96-4806-a26f-eee560e52045", 
  "id": "/subscriptions/dfa2a084-766f-4003-8ae1-c4aeb893a99f/providers/Microsoft.Authorization/RoleAssignmentScheduleRequests/fea7a502-9a96-4806-a26f-eee560e52045", 
  "type": "Microsoft.Authorization/RoleAssignmentScheduleRequests" 
} 
````

## View the status of your requests

You can view the status of your pending requests to activate.

1. Open Microsoft Entra Privileged Identity Management.

1. Select **My requests** to see a list of your Microsoft Entra role and Azure resource role requests.

    ![My requests - Azure resource page showing your pending requests](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Scroll to the right to view the **Request Status** column.

## Cancel a pending request

If you do not require activation of a role that requires approval, you can cancel a pending request at any time.

1. Open Microsoft Entra Privileged Identity Management.

1. Select **My requests**.

1. For the role that you want to cancel, select the **Cancel** link.

    When you select Cancel, the request will be canceled. To activate the role again, you will have to submit a new request for activation.

   ![My request list with Cancel action highlighted](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## Deactivate a role assignment

When a role assignment is activated, you'll see a **Deactivate** option in the PIM portal for the role assignment. Also, you can't deactivate a role assignment within five minutes after activation.

## Activate with Azure portal

Privileged Identity Management role activation has been integrated into the Billing and Access Control (AD) extensions within the Azure portal. Shortcuts to Subscriptions (billing) and Access Control (AD) allow you to activate PIM roles directly from these blades.

From the Subscriptions blade, select “View eligible subscriptions” in the horizontal command menu to check your eligible, active, and expired assignments. From there, you can activate an eligible assignment in the same pane.

   ![Screenshot of view eligible subscriptions on the Subscriptions page.](./media/pim-resource-roles-activate-your-roles/view-subscriptions-1.png)

   ![Screenshot of view eligible subscriptions on the Cost Management: Integration Service page.](./media/pim-resource-roles-activate-your-roles/view-subscriptions-2.png)

In Access control (IAM) for a resource, you can now select “View my access” to see your currently active and eligible role assignments and activate directly.

   ![Screenshot of current role assignments on the Measurement page.](./media/pim-resource-roles-activate-your-roles/view-my-access.png)

By integrating PIM capabilities into different Azure portal blades, this new feature allows you to gain temporary access to view or edit subscriptions and resources more easily.

## Activate PIM roles using the Azure mobile app

PIM is now available in the Microsoft Entra ID and Azure resource roles mobile apps in both iOS and Android.

1.	To activate an eligible Microsoft Entra role assignment, start by downloading the Azure mobile app ([iOS](https://apps.apple.com/us/app/microsoft-azure/id1219013620) | [Android](https://play.google.com/store/apps/details?id=com.microsoft.azure)). You can also download the app by selecting **Open in mobile**  from Privileged Identity Management > My roles > Microsoft Entra roles.

    :::image type="content" source="./media/pim-resource-roles-activate-your-roles/download-mobile-app.png" alt-text="Screenshot shows how to download the mobile app." lightbox="./media/pim-resource-roles-activate-your-roles/download-mobile-app.png":::

1. Open the Azure mobile app and sign in. Click on the ‘Privileged Identity Management’ card and select **My Azure Resource roles** to view your eligible and active role assignments.

    :::image type="content" source="./media/pim-resource-roles-activate-your-roles/mobile-select-role.png" alt-text="Screenshot of the mobile app showing privileged identity managementand the user's roles" lightbox="./media/pim-resource-roles-activate-your-roles/mobile-select-role.png":::

1. Select the role assignment and click on **Action > Activate** under the role assignment details. Complete the steps to active and fill in any required details before clicking **Activate** at the bottom.

    :::image type="content" source="./media/pim-resource-roles-activate-your-roles/mobile-activate-role.png" alt-text="Screenshot of the mobile app showing the validation process has completed. The image shows an Activate button " lightbox="./media/pim-resource-roles-activate-your-roles/mobile-activate-role.png":::

1. View the status of your activation requests and your role assignments under ‘My Azure Resource roles’.

    :::image type="content" source="./media/pim-resource-roles-activate-your-roles/mobile-activation-processing.png" alt-text="Screenshot of the mobile app showing the activation in progress message." lightbox="./media/pim-resource-roles-activate-your-roles/mobile-activation-processing.png":::

## Next steps

- [Extend or renew Azure resource roles in Privileged Identity Management](pim-resource-roles-renew-extend.md)
- [Activate my Microsoft Entra roles in Privileged Identity Management](pim-how-to-activate-role.md)
