---
title: Inzicht in de resultaten van Windows-agent in beheer van Azure-updates
description: Informatie over het oplossen van problemen met de updatebeheer-agent.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: automation
ms.component: update-management
manager: carmonm
ms.openlocfilehash: 68bac54de4f6a9f16f54032b10031bcf7222c676
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969308"
---
# <a name="understand-the-windows-agent-check-results-in-update-management"></a>Inzicht in de resultaten van Windows-agent in beheer van updates

Er zijn diverse redenen waarom een Azure-machine kan niet worden weergegeven een **gereed** status in Azure updatebeheer. U kunt de status van een Hybrid Worker-agent om te bepalen van de onderliggende probleem controleren in Update Management. In dit artikel wordt beschreven hoe u de probleemoplosser voor het beheer van updates vanuit de Azure-portal en in offlinescenario's worden uitgevoerd.

## <a name="start-the-troubleshooter"></a>De probleemoplosser voor starten

In de Azure-portal, de **Update-Agent oplossen** pagina problemen met de agent wordt weergegeven. Er is een koppeling naar dit artikel om u te helpen bij het oplossen van problemen op de pagina. Naar de **Update-Agent oplossen** weergeeft, schakelt de **oplossen** herstelkoppeling in de **gereedheid voor Update-Agent** kolom.

![Beheerlijst met van virtuele machines bijwerken](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Om te controleren of de status van een agent, moet de virtuele machine worden uitgevoerd. Als de virtuele machine niet wordt uitgevoerd, een **Start de virtuele machine** knop wordt weergegeven.

Op de **Update-Agent oplossen** weergeeft, schakelt **controles uitvoeren** de probleemoplosser voor starten. Maakt gebruik van de probleemoplosser voor [opdracht uitvoeren](../../virtual-machines/windows/run-command.md) een script uitvoeren op de machine om te controleren of de afhankelijkheden voor clientagent. Wanneer de probleemoplosser voor voltooid is, wordt het resultaat van de controles.

![Pagina van de Update-Agent oplossen](../media/update-agent-issues/troubleshoot-page.png)

Resultaten worden weergegeven op de pagina wanneer ze klaar voor bent. De [controleert secties](#prerequisiste-checks) wat opgenomen in elke controle weergeven.

![Controles van de Update-Agent oplossen](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Controles van vereisten

### <a name="operating-system"></a>Besturingssysteem

De controle van het besturingssysteem wordt geverifieerd of de Hybrid Runbook Worker een van deze besturingssystemen wordt uitgevoerd:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008 R2 RTM, WindowsServer 2008 | Ondersteunt alleen bijwerken evaluaties.         |
|Windows Server 2008 R2 SP1 en hoger |.NET framework 4.5.1 of later is vereist. ([Downloaden van het .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 of hoger is vereist. ([Downloaden van Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1 wordt aanbevolen voor hogere mate van betrouwbaarheid.  ([Downloaden van Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |

### <a name="net-451"></a>.NET 4.5.1

De .NET Framework-controle wordt geverifieerd dat het systeem een minimum van heeft [.NET Framework 4.5.1](https://www.microsoft.com/download/details.aspx?id=30653) geïnstalleerd.

### <a name="wmf-51"></a>WMF 5.1

De WMF-controle wordt geverifieerd dat het systeem de vereiste versie van de Windows Management Framework (WMF heeft). [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) is de eerste ondersteunde versie. Het is raadzaam dat u installeert [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616) te vergroten van de betrouwbaarheid van de Hybrid Runbook Worker.

### <a name="tls-12"></a>TLS 1.2

Deze controle wordt bepaald of u TLS 1.2 uw communicatie te versleutelen. TLS 1.0 is niet meer ondersteund door het platform. U wordt aangeraden dat clients gebruik van TLS 1.2 om te communiceren met updatebeheer.

## <a name="connectivity-checks"></a>Controles voor connectiviteit

### <a name="registration-endpoint"></a>Eindpunt voor clientregistratie

Deze controle wordt bepaald of de agent correct met de agent-service communiceren kan.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met het eindpunt voor clientregistratie toestaan. Zie voor een lijst van adressen en poorten te openen, [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="operations-endpoint"></a>Operations-eindpunt

Deze controle wordt bepaald of de agent correct met de taak Runtime Data-Service communiceren kan.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met de taak Runtime Data-Service toestaan. Zie voor een lijst van adressen en poorten te openen, [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning).

## <a name="vm-service-health-checks"></a>Statuscontroles van VM-service

### <a name="monitoring-agent-service-status"></a>Status van agent-service controleren

Deze controle wordt bepaald of `HealthService`, de Microsoft Monitoring Agent, op de machine wordt uitgevoerd.

Zie voor meer informatie over het oplossen van de service, [The Microsoft Monitoring Agent wordt niet uitgevoerd](hybrid-runbook-worker.md#mma-not-running).

Als u wilt de Microsoft Monitoring Agent installeren, Zie [installeren en configureren van de Microsoft Monitoring Agent](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows).

### <a name="monitoring-agent-service-events"></a>Gebeurtenissen van de agent-service controleren

Deze controle wordt bepaald of een `4502` gebeurtenissen worden weergegeven in het Azure Operations Manager-gebeurtenislogboek op de computer in de afgelopen 24 uur.

Zie voor meer informatie over deze gebeurtenis, de [problemen oplossen met](hybrid-runbook-worker.md#event-4502) voor deze gebeurtenis.

## <a name="access-permissions-checks"></a>Machtigingen van toegangscontrole

### <a name="machinekeys-folder-access"></a>Toegang tot als een map

De toegangscontrole voor Crypto-map wordt bepaald of het lokale systeemaccount toegang tot C:\ProgramData\Microsoft\Crypto\RSA heeft.

## <a name="troubleshoot-offline"></a>Offline oplossen

U kunt de probleemoplosser voor op een Hybrid Runbook Worker offline gebruiken door het script lokaal worden uitgevoerd. U kunt het script, krijgen [problemen oplossen-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration), in de PowerShell Gallery. De uitvoer van dit script ziet er als volgt uit:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.1.7601 (Windows Server 2008 R2 SP1) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .Net Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             : 
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Microsoft Monitoring Agent, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Microsoft Monitoring Agent, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Volgende stappen

Zie meer problemen met de Hybrid Runbook Workers, [oplossen Hybrid Runbook Workers](hybrid-runbook-worker.md).
