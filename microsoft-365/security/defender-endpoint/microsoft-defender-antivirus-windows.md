---
title: Microsoft Defender Antivirus in Windows
description: Learn how to manage, configure, and use Microsoft Defender Antivirus, built-in antimalware and antivirus protection.
keywords: Microsoft Defender Antivirus, windows defender, antimalware, scep, system center endpoint protection, system center configuration manager, virus, malware, threat, detection, protection, security
ms.service: defender-endpoint
ms.localizationpriority: high
ms.date: 12/05/2023
ms.topic: conceptual
author: denisebmsft
ms.author: deniseb
ms.reviewer: mkaminska, yonghree, pahuijbr
manager: dansimp
ms.custom: nextgen
ms.subservice: ngp
ms.collection: 
- m365-security
- tier2
- mde-ngp
search.appverid: met150
---

# Microsoft Defender Antivirus in Windows

**Applies to:**

- Microsoft Defender for Endpoint Plans 1 and 2
- Microsoft Defender for Business
- Microsoft Defender Antivirus

**Platforms**
- Windows 

Microsoft Defender Antivirus is available in Windows 10 and Windows 11, and in versions of Windows Server.

Microsoft Defender Antivirus is a major component of your next-generation protection in Microsoft Defender for Endpoint. This protection brings together machine learning, big-data analysis, in-depth threat resistance research, and the Microsoft cloud infrastructure to protect devices (or endpoints) in your organization. Microsoft Defender Antivirus is built into Windows, and it works with Microsoft Defender for Endpoint to provide protection on your device and in the cloud.

## Compatibility with other antivirus products

If you're using a non-Microsoft antivirus/antimalware product on your device, you might be able to run Microsoft Defender Antivirus in passive mode alongside the non-Microsoft antivirus solution. It depends on the operating system used and whether your device is onboarded to Defender for Endpoint. To learn more, see [Microsoft Defender Antivirus compatibility](microsoft-defender-antivirus-compatibility.md).

## Microsoft Defender Antivirus processes and services

The following table summarizes Microsoft Defender Antivirus processes and services. You can view them in Task Manager in Windows.

| Process or service | Where to view its status |
|--|--|
| **Microsoft Defender Antivirus Core service** <br/>(`MdCoreSvc`) | - **Processes** tab: `Antimalware Core Service` <br/>- **Details** tab: `MpDefenderCoreService.exe` <br/>- **Services** tab: `Microsoft Defender Core Service` | 
| **Microsoft Defender Antivirus service** <br/>(`WinDefend`) | - **Processes** tab: `Antimalware Service Executable` <br/>- **Details** tab: `MsMpEng.exe` <br/>- **Services** tab: `Microsoft Defender Antivirus` | 
| **Microsoft Defender Antivirus Network Realtime Inspection service** <br/>(`WdNisSvc`) |  - **Processes** tab: `Microsoft Network Realtime Inspection Service` <br/>- **Details** tab: `NisSrv.exe` <br/>- **Services** tab: `Microsoft Defender Antivirus Network Inspection Service` | 
| **Microsoft Defender Antivirus command-line utility** | - **Processes** tab: N/A <br/>- **Details** tab: `MpCmdRun.exe` <br/>- **Services** tab: N/A  | 
| **Microsoft Security Client Policy Configuration Tool** | - **Processes** tab: N/A <br/>- **Details** tab: `ConfigSecurityPolicy.exe` <br/>- **Services** tab: N/A | 

For [Microsoft Endpoint Data Loss Prevention](/purview/endpoint-dlp-getting-started) (Endpoint DLP), the following table summarizes processes and services. You can view them in Task Manager in Windows.

| Process or service | Where to view its status |
|--|--|
| **Microsoft Endpoint DLP service** <br/>(`MDDlpSvc`) | - **Processes** tab: `MpDlpService.exe` <br/>- **Details** tab: `MpDlpService.exe` <br/>- **Services** tab: `Microsoft Data Loss Prevention Service` |
| **Microsoft Endpoint DLP command-line utility**  | - **Processes** tab: N/A <br/>- **Details** tab: `MpDlpCmd.exe` <br/>- **Services** tab: N/A |

## Microsoft Defender Core service

To enhance your endpoint security experience, Microsoft is releasing the Microsoft Defender Core service to help with the stability and performance of Microsoft Defender Antivirus. For customers who are using Microsoft Endpoint Data Loss Prevention in the small, medium, and enterprise business sectors, Microsoft is splitting the codebase to its own service.  

The Microsoft Defender Core service is releasing with [Microsoft Defender Antivirus platform version 4.18.23110.2009](microsoft-defender-antivirus-updates.md#october-2023-platform-418231002009--engine-11231002009).

- Rollout begins in November 2023 to prerelease customers, with plans to release to all enterprise customers in the coming months.

- Enterprise customers should allow the following URLs:
   - `*.events.data.microsoft.com`
   - `*.endpoint.security.microsoft.com`
   - `*.ecs.office.com` 

- If you're using [Application Control for Windows](/windows/security/application-security/application-control/windows-defender-application-control/wdac), or you're running non-Microsoft antivirus or endpoint detection and response software, make sure to add the processes mentioned earlier to your allow list. 

- Consumers need not take any actions to prepare. 

## Comparing active mode, passive mode, and disabled mode

The following table describes what to expect when Microsoft Defender Antivirus is in active mode, passive mode, or disabled.

| Mode | What happens |
|---|---|
| Active mode | In active mode, Microsoft Defender Antivirus is used as the primary antivirus app on the device. Files are scanned, threats are remediated, and detected threats are listed in your organization's security reports and in your Windows Security app. |
| Passive mode | In passive mode, Microsoft Defender Antivirus is not used as the primary antivirus app on the device. Files are scanned, and detected threats are reported, but threats are not remediated by Microsoft Defender Antivirus. <br/><br/> **IMPORTANT**: Microsoft Defender Antivirus can run in passive mode only on endpoints that are onboarded to Microsoft Defender for Endpoint. See [Requirements for Microsoft Defender Antivirus to run in passive mode](microsoft-defender-antivirus-compatibility.md#requirements-for-microsoft-defender-antivirus-to-run-in-passive-mode). |
| Disabled or uninstalled | When disabled or uninstalled, Microsoft Defender Antivirus is not used. Files are not scanned, and threats are not remediated. In general, we do not recommend disabling or uninstalling Microsoft Defender Antivirus. |

To learn more, see [Microsoft Defender Antivirus compatibility](microsoft-defender-antivirus-compatibility.md).

## Check the state of Microsoft Defender Antivirus on your device

You can use one of several methods, such as the Windows Security app or Windows PowerShell, to check the state of Microsoft Defender Antivirus on your device.

> [!IMPORTANT]
> Beginning with [platform version 4.18.2208.0 and later](microsoft-defender-antivirus-updates.md#monthly-platform-and-engine-versions): If a server has been onboarded to Microsoft Defender for Endpoint, the "Turn off Windows Defender" [group policy](configure-endpoints-gp.md#update-endpoint-protection-configuration) setting will no longer completely disable Windows Defender Antivirus on Windows Server 2012 R2 and later. Instead, it will place it into passive mode. In addition, the [tamper protection](prevent-changes-to-security-settings-with-tamper-protection.md) feature will allow a switch to active mode but not to passive mode.
> 
> - If "Turn off Windows Defender" is already in place before onboarding to Microsoft Defender for Endpoint, there will be no change and Defender Antivirus will remain disabled.
> - To switch Defender Antivirus to passive mode, even if it was disabled before onboarding, you can apply the [ForceDefenderPassiveMode configuration](switch-to-mde-phase-2.md#set-microsoft-defender-antivirus-to-passive-mode-on-windows-server) with a value of `1`. To place it into active mode, switch this value to `0` instead.
> 
> Note the modified logic for `ForceDefenderPassiveMode` when tamper protection is enabled: Once Microsoft Defender Antivirus is toggled to active mode, tamper protection will prevent it from going back into passive mode even when `ForceDefenderPassiveMode` is set to `1`.

### Use the Windows Security app to check the status of Microsoft Defender Antivirus

1. On your Windows device, select the **Start** menu, and begin typing `Security`. Then open the Windows Security app in the results.

2. Select **Virus & threat protection**.

3. Under **Who's protecting me?**, choose **Manage Providers**.

You'll see the name of your antivirus/antimalware solution on the security providers page.

### Use PowerShell to check the status of Microsoft Defender Antivirus

1. Select the **Start** menu, and begin typing `PowerShell`. Then open Windows PowerShell in the results.

2. Type `Get-MpComputerStatus`.

3. In the list of results, look at the **AMRunningMode** row.

   - **Normal** means Microsoft Defender Antivirus is running in active mode.

   - **Passive mode** means Microsoft Defender Antivirus running, but is not the primary antivirus/antimalware product on your device. Passive mode is only available for devices that are onboarded to Microsoft Defender for Endpoint and that meet certain requirements. To learn more, see [Requirements for Microsoft Defender Antivirus to run in passive mode](microsoft-defender-antivirus-compatibility.md#requirements-for-microsoft-defender-antivirus-to-run-in-passive-mode).

   - **EDR Block Mode** means Microsoft Defender Antivirus is running and [Endpoint detection and response (EDR) in block mode](edr-in-block-mode.md), a capability in Microsoft Defender for Endpoint, is enabled. Check the **ForceDefenderPassiveMode** registry key. If its value is 0, it is running in normal mode; otherwise, it is running in passive mode.

   - **SxS Passive Mode** means Microsoft Defender Antivirus is running alongside another antivirus/antimalware product, and [limited periodic scanning is used](limited-periodic-scanning-microsoft-defender-antivirus.md).

> [!TIP]
> To learn more about the Get-MpComputerStatus PowerShell cmdlet, see the reference article [Get-MpComputerStatus](/powershell/module/defender/get-mpcomputerstatus).

> [!TIP]
> **Performance tip** Due to a variety of factors (examples listed below) Microsoft Defender Antivirus, like other antivirus software, can cause performance issues on endpoint devices. In some cases, you might need to tune the performance of Microsoft Defender Antivirus to alleviate those performance issues. Microsoft's **Performance analyzer** is a PowerShell command-line tool that helps determine which files, file paths, processes, and file extensions might be causing performance issues; some examples are:
>
> - Top paths that impact scan time
> - Top files that impact scan time
> - Top processes that impact scan time
> - Top file extensions that impact scan time
> - Combinations – for example:
>   - top files per extension
>   - top paths per extension
>   - top processes per path
>   - top scans per file
>   - top scans per file per process
>
> You can use the information gathered using Performance analyzer to better assess performance issues and apply remediation actions. 
> See: [Performance analyzer for Microsoft Defender Antivirus](tune-performance-defender-antivirus.md).
>

## Get your antivirus/antimalware platform updates

It's important to keep Microsoft Defender Antivirus (or any antivirus/antimalware solution) up to date. Microsoft releases regular updates to help ensure that your devices have the latest technology to protect against new malware and attack techniques. To learn more, see [Manage Microsoft Defender Antivirus updates and apply baselines](microsoft-defender-antivirus-updates.md).

> [!TIP]
> If you're looking for Antivirus related information for other platforms, see:
> - [Set preferences for Microsoft Defender for Endpoint on macOS](mac-preferences.md)
> - [Microsoft Defender for Endpoint on Mac](microsoft-defender-endpoint-mac.md)
> - [macOS Antivirus policy settings for Microsoft Defender Antivirus for Intune](/mem/intune/protect/antivirus-microsoft-defender-settings-macos)
> - [Set preferences for Microsoft Defender for Endpoint on Linux](linux-preferences.md)
> - [Microsoft Defender for Endpoint on Linux](microsoft-defender-endpoint-linux.md)
> - [Configure Defender for Endpoint on Android features](android-configure.md)
> - [Configure Microsoft Defender for Endpoint on iOS features](ios-configure-features.md)

## See also

- [Performance analyzer for Microsoft Defender Antivirus](tune-performance-defender-antivirus.md)
- [Microsoft Defender Antivirus management and configuration](configuration-management-reference-microsoft-defender-antivirus.md)
- [Evaluate Microsoft Defender Antivirus protection](evaluate-microsoft-defender-antivirus.md)
- [Exclusions for Microsoft Defender for Endpoint and Microsoft Defender Antivirus](defender-endpoint-antivirus-exclusions.md)
[!INCLUDE [Microsoft Defender for Endpoint Tech Community](../../includes/defender-mde-techcommunity.md)]
