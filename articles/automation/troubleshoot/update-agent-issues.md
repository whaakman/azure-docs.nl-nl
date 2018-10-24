---
title: Inzicht in de resultaten van de agent in beheer van Azure-updates
description: Informatie over het oplossen van problemen met de updatebeheer-agent.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/10/2018
ms.topic: conceptual
ms.service: automation
ms.component: update-management
manager: carmonm
ms.openlocfilehash: 1b258d115e7d9962ecab4b93dbcd98d13f5977c7
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956677"
---
# <a name="understand-the-agent-check-results-in-update-management"></a>Inzicht in de resultaten van de agent in beheer van updates

Mogelijk zijn er veel oorzaken waardoor uw niet-Azure-machine wordt niet weergegeven **gereed** in beheer van updates. U kunt de status van een Hybrid Worker-agent om te bepalen van het onderliggende probleem controleren in Update Management. In dit artikel wordt beschreven hoe u de probleemoplosser uitvoert vanuit de Azure-portal en in offlinescenario's.

## <a name="start-the-troubleshooter"></a>De probleemoplosser voor starten

Door te klikken op de **oplossen** koppeling onder de **gereedheid voor Update-Agent** kolom in de portal, start u de **Update-Agent oplossen** pagina. Deze pagina ziet u problemen met de agent en een koppeling naar dit artikel helpt u bij het oplossen van uw problemen.

![de pagina van de VM-lijst](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> De controles moeten de virtuele machine worden uitgevoerd. Als de virtuele machine wordt niet uitgevoerd krijgt u een knop **Start de virtuele machine**.

Op de **Update-Agent oplossen** pagina, klikt u op **uitvoeren controleert**, de probleemoplosser voor starten. Maakt gebruik van de probleemoplosser voor [RunCommand-](../../virtual-machines/windows/run-command.md) een script uitvoeren op de machine om te controleren of de afhankelijkheden die de agent heeft. Wanneer de probleemoplosser voor voltooid is, wordt het resultaat van de controles.

![Pagina oplossen](../media/update-agent-issues/troubleshoot-page.png)

Als u klaar bent, worden de resultaten worden geretourneerd in het venster. De [Controleer gedeelte](#pre-requisistes-checks) bevatten informatie over wat elke controle zoekt.

![Update-agent controleert op pagina](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Controles van vereisten

### <a name="operating-system"></a>Besturingssysteem

De OS-controle alleen gecontroleerd of de Hybrid Runbook Worker wordt uitgevoerd een van de volgende besturingssystemen:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Ondersteunt alleen bijwerken evaluaties.         |
|Windows Server 2008 R2 SP1 en hoger     |.NET framework 4.5 of hoger is vereist. ([.NET Framework downloaden](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 of hoger is vereist. ([WMF 4.0 downloaden](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1 wordt aanbevolen voor hogere mate van betrouwbaarheid.  ([Downloaden van WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) en 7 (x64)      | Linux-agents moeten toegang hebben tot een opslagplaats voor updates. Patch toepassen op classificatie gebaseerde vereist 'yum' om terug te keren beveiligingsgegevens die CentOS heeft geen buiten het vak.         |
|Red Hat Enterprise 6 (x86/x64) en 7 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Ubuntu 14.04 LTS en 16.04 LTS (x86/x64)      |Linux-agents moeten toegang hebben tot een opslagplaats voor updates.         |

### <a name="net-45"></a>.NET 4.5

De .NET framework-controle alleen gecontroleerd als het systeem heeft een minimum van [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) aanwezig zijn.

### <a name="wmf-51"></a>WMF 5.1

De controle van WMF, controleert u of als het systeem heeft de vereiste versie van Windows Management Framework. [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) is de laagste versie die wordt ondersteund. Het is raadzaam dat u installeert [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616) voor verbeterde betrouwbaarheid van de Hybrid Runbook Worker.

### <a name="tls-12"></a>TLS 1.2

Deze controle bepaalt als u TLS 1.2 uw communicatie te versleutelen. TLS 1.0 wordt niet meer ondersteund door het platform en het verdient aanbeveling dat clients gebruik van TLS 1.2 om te communiceren met updatebeheer.

## <a name="connectivity-checks"></a>Controles voor connectiviteit

### <a name="registration-endpoint"></a>Registratie-eindpunt

Deze controle bepaalt als de agent correct met de agent-service communiceren kan.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met het eindpunt voor clientregistratie toestaan. Zie voor een lijst van adressen en poorten te openen, [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="operations-endpoint"></a>Operations-eindpunt

Deze controle bepaalt als de agent correct met de taak-Runtime-gegevens-Service communiceren kan.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met de taak Runtime Data-Service toestaan. Zie voor een lijst van adressen en poorten te openen, [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning)

## <a name="vm-service-health-checks"></a>Statuscontroles van VM-service

### <a name="monitoring-agent-service-status"></a>Status van agent-service controleren

Deze controle wordt bepaald of de Microsoft Monitoring Agent, `HealthService` wordt uitgevoerd op de machine.

Zie voor meer informatie over het oplossen van de service, [The Microsoft Monitoring Agent wordt niet uitgevoerd](hybrid-runbook-worker.md#mma-not-running).

Als u wilt de Microsoft Monitoring Agent installeren, Zie [installeren en configureren van de Microsoft Monitoring Agent](/log-analytics/log-analytics-concept-hybrid.md#install-and-configure-agent)

### <a name="monitoring-agent-service-events"></a>Gebeurtenissen van de agent-service controleren

Deze controle bepaalt als er een is `4502` in de Operations Manager-gebeurtenislogboek op de computer in de afgelopen 24 uur.

Zie voor meer informatie over deze gebeurtenis, de [problemen oplossen met](hybrid-runbook-worker.md#event-4502) voor deze gebeurtenis.

## <a name="access-permissions-checks"></a>Machtigingen van toegangscontrole

### <a name="machinekeys-folder-access"></a>Toegang tot als een map

De controle van toegang tot de Crypto-map bepaalt als lokale systeemaccount toegang tot heeft `C:\ProgramData\Microsoft\Crypto\RSA`

## <a name="troubleshoot-offline"></a>Offline oplossen

U kunt de probleemoplosser voor offline gebruiken op een Hybrid Runbook Worker door het script lokaal worden uitgevoerd. Het script [problemen oplossen-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration) is te vinden in de PowerShell Gallery. Een voorbeeld van de uitvoer van dit script wordt weergegeven in het volgende voorbeeld:

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

Aanvullende problemen met de Hybrid Runbook Workers, Zie [problemen oplossen - Hybrid Runbook Workers](hybrid-runbook-worker.md)
