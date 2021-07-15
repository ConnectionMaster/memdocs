---
# required metadata

title: Learn about using Windows Update for Business in Microsoft Intune - Azure | Microsoft Docs
description: Manage Windows 10 software updates by using Intune policy for Windows 10 update rings and Windows 10 feature updates for Windows Update for Business settings in Microsoft Intune.
keywords:
author: brenduns
ms.author: brenduns
manager: dougeby
ms.date: 04/09/2021
ms.topic: overview
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high
ms.technology:

# optional metadata

#ROBOTS:
#audience:

ms.reviewer: davidmeb; bryanke
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
#ms.custom:
ms.collection: M365-identity-device-management
---

# Manage Windows 10 software updates in Intune

Use Intune to manage the install of Windows 10 software updates from Windows Update for Business.

By using Windows Update for Business, you simplify the update management experience. You don't need to approve individual updates for groups of devices and can manage risk in your environments by configuring an update rollout strategy. Intune provides the ability to [configure update settings](windows-update-settings.md) on devices and gives you the ability to defer update installation. You can also prevent devices from installing features from new Windows versions to help keep them stable, while allowing those devices to continue installing updates for quality and security.

Intune stores only the update policy assignments, not the updates themselves. When you save a policy, Intune passes the configuration details to Windows Update which then determines which updates will be offered to each device. Devices access Windows Update directly for the updates.

Learn more about Windows 10 [*feature* and *quality* updates](/windows/deployment/update/get-started-updates-channels-tools#types-of-updates) in the Windows documentation.

## Policy types to manage updates

Intune provides the following policy types to manage updates, which you assign to groups of devices:

- **Windows 10 update ring**: This policy is a collection of settings that configures when Windows 10 updates get installed.

  Update ring policies are supported for devices that run Windows 10 version 1607 or later.

- **Windows 10 feature updates** *(public preview)*: This policy updates devices to the Windows version you specify, and then freezes the feature set version on those devices. This version freeze remains in place until you choose to update them to a later Windows version. While the feature version remains static, devices can continue to install quality and security updates that are available for their feature version.

  Feature updates policies are supported for devices that run Windows 10 version 1709 or later.

## Move from update ring deferrals to feature updates policy

When using Intune to manage Windows 10 updates, it’s possible to use both *update rings* policy with update deferrals, and *feature updates* policy to manage the updates you want to install on devices. If you’re using feature updates, we recommend you end use of deferrals as configured in your update rings policy. Combining update ring deferrals with feature updates policy can create complexity that might delay update installations. You can continue to use the user experience settings from update rings, as they don’t create issues when combined with feature updates policy.

While nothing prohibits use of both policy types to control which updates can install on a device, there is typically no advantage to doing so. When both policy types apply to a device, the conditions of both policy types must be met (be true) on the device before it’s offered an applicable update. This scenario can lead to updates not installing as expected due to a block by one of the policy types.

### Plan to transition

Plan to manage the change from using update ring deferrals to feature updates so that the Windows Update service can be ready to deploy the updates you expect.

- When Intune policies for Windows 10 updates are created or modified, Intune passes the policy details to Windows Update, which then determines the updates that are applicable for each device that’s assigned one or more update policies.

- The process to evaluate updates for devices can take up to 10 minutes to complete, and in some cases might take a bit longer.

- If a device starts a scan for updates *after* a deferral has been set to zero or removed for the device, but *before* Windows Update completes the processing of the feature updates policy, that device can be offered an update you didn’t plan for it to install.

Use the following process to ensure Windows Update has processed your feature updates policy before deferrals are removed.

#### Switch to feature updates policy

1. In the Microsoft Endpoint Manager admin center, create a [feature updates policy](../protect/windows-10-feature-updates.md) that configures your desired Windows version, and assign it to applicable devices.

   After the saved policy is assigned to devices, it will take a few minutes for Windows Update to process the policy.

2. View the [Windows 10 feature updates (Organizational)](../protect/windows-update-compliance-reports.md#use-the-windows-10-feature-updates-organizational-report) report for the feature update policy, and verify devices have a state of **OfferReady** before you proceed.  Once all devices show **OfferReady**, Windows Update has completed processing the policy.

3. After devices are verified to be in the **OfferReady** state you can safely reconfigure the [Windows 10 update ring policy](../protect/windows-10-update-rings.md) for that same set of devices to change the setting **Feature update deferral period (days)** to a value of **0**.


## Reporting on updates

To learn about report options for Windows 10 update ring policy and Windows 10 feature updates policy, see [Intune compliance reports for updates](windows-update-compliance-reports.md).

## Next steps

- [Use Windows 10 update rings in Intune](../protect/windows-10-update-rings.md)
- [Use Windows 10 feature updates in Intune](../protect/windows-10-feature-updates.md)
- For more information, see [Manage updates using Windows Update for Business](/windows/deployment/update/waas-manage-updates-wufb) in the Windows documentation.
