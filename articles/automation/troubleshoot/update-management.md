---
title: Problemen oplossen met updatebeheer
description: Meer informatie over het oplossen van problemen met updatebeheer
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 9bcc871ecc9413f02545e6aec4caa6342d563b44
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66474568"
---
# <a name="troubleshooting-issues-with-update-management"></a>Oplossen van problemen met updatebeheer

Dit artikel worden oplossingen voor het oplossen van problemen die worden uitgevoerd op bij het gebruik van updatebeheer.

Er is een agent probleemoplosser voor Hybrid Worker-agent om te bepalen van het onderliggende probleem. Zie voor meer informatie over de probleemoplosser voor [problemen oplossen update-agent](update-agent-issues.md). Voor alle andere problemen, Zie de gedetailleerde informatie hieronder over mogelijke problemen.

## <a name="general"></a>Algemeen

### <a name="components-enabled-not-working"></a>Scenario: De onderdelen voor de oplossing voor beheer van updates zijn ingeschakeld en nu deze virtuele machine wordt geconfigureerd

#### <a name="issue"></a>Probleem

U blijven ziet het volgende bericht op een virtuele machine 15 minuten na de onboarding:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door de volgende redenen:

1. Communicatie terug naar het Automation-Account wordt geblokkeerd.
2. De virtuele machine onboarding wordt uitgevoerd kan afkomstig zijn uit een gekloonde machine die niet Sysprep met de Microsoft Monitoring Agent is geïnstalleerd is.

#### <a name="resolution"></a>Oplossing

1. Ga naar, [netwerkplanning](../automation-hybrid-runbook-worker.md#network-planning) voor meer informatie over welke adressen en poorten moeten worden toegestaan voor het beheer van updates om te werken.
2. Als een gekloonde installatiekopie:
   1. In uw Log Analytics-werkruimte, verwijdert u de virtuele machine van de opgeslagen zoekopdracht voor de Bereikconfiguratie `MicrosoftDefaultScopeConfig-Updates` als deze wordt weergegeven. Opgeslagen zoekopdrachten kunnen u vinden onder **algemene** in uw werkruimte.
   2. Voer `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force` uit.
   3. Voer `Restart-Service HealthService` opnieuw starten de `HealthService`. Dit wordt opnieuw maken van de sleutel en een nieuwe UUID genereren.
   4. Als dit niet werkt, sysprep de installatiekopie van het eerste en de MMA-agent installeren na de gebeurtenis.

### <a name="multi-tenant"></a>Scenario: U ontvangt een foutmelding gekoppelde abonnement bij het maken van een update-implementatie voor de machines in een andere Azure-tenant.

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het maken van een update-implementatie voor de machines in een andere Azure-tenant:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op wanneer u een update-implementatie met virtuele Azure-machines in een andere tenant opgenomen in een update-implementatie maakt.

#### <a name="resolution"></a>Oplossing

U moet de volgende oplossing gebruiken om op te halen ze gepland. U kunt de [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet met de switch `-ForUpdate` een planning maken en gebruiken de [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet en door te geven de machines in de andere tenant om de `-NonAzureComputer` parameter. Het volgende voorbeeld toont een voorbeeld van hoe u dit doet:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

### <a name="nologs"></a>Scenario: Machines niet worden weergegeven in de portal onder beheer van updates

#### <a name="issue"></a>Probleem

U kunt uitvoeren in de volgende scenario's:

* Uw programma's machine **niet geconfigureerd** uit de weergave beheer van updates van een virtuele machine

* Uw machines ontbreken in de weergave beheer van updates van uw Automation-Account

* U machines hebt die als **niet beoordeeld** onder **naleving**, maar ziet u heartbeat-gegevens in Azure Monitor-logboeken voor de Hybrid Runbook Worker, maar niet voor updatebeheer.

#### <a name="cause"></a>Oorzaak

Dit kan worden veroorzaakt door problemen met de lokale configuratie of onjuist geconfigureerde scopeconfiguratie.

De Hybrid Runbook Worker moet mogelijk opnieuw worden geregistreerd en opnieuw geïnstalleerd.

Mogelijk hebt u een quotum gedefinieerd in de werkruimte die is bereikt en stoppen gegevens worden opgeslagen.

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat uw computer is rapporteren aan de juiste werkruimte. Controleer of welke werkruimte uw machine rapporteert aan. Zie voor instructies over hoe u kunt dit controleren, [of agents verbonden zijn met Log Analytics](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Controleer vervolgens of dat dit is de werkruimte die is gekoppeld aan uw Azure Automation-account. U kunt dit controleren, gaat u naar uw Automation-Account en klikt u op **gekoppelde werkruimte** onder **gerelateerde Resources**.

* Controleer of de computers weergegeven in uw Log Analytics-werkruimte. Voer de volgende query uit in uw Log Analytics-werkruimte die is gekoppeld aan uw Automation-Account. Als u uw computer in de queryresultaten niet ziet, wordt uw computer is geen heartbeat waargenomen, wat betekent dat er is waarschijnlijk een probleem lokale configuratie. U kunt de probleemoplosser voor uitvoeren [Windows](update-agent-issues.md#troubleshoot-offline) of [Linux](update-agent-issues-linux.md#troubleshoot-offline) afhankelijk van het besturingssysteem, of u kunt [de agent opnieuw installeren](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Als uw computer weergegeven in de queryresultaten wordt, dan u zeer moet de scopeconfiguratie die is opgegeven in het volgende opsommingsteken.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```

* Gecontroleerd op configuratieproblemen voor bereik. [De scopeconfiguratie](../automation-onboard-solutions-from-automation-account.md#scope-configuration) bepaalt welke machines krijgen geconfigureerd voor de oplossing. Als uw computer wordt weergegeven in uw werkruimte, maar niet wordt weergegeven u moet de configuratie van de scope wilt richten op de machines te configureren. Zie voor meer informatie hoe u dit doet, [Onboarding van machines in de werkruimte](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* Als de bovenstaande stappen het probleem niet wordt opgelost, volgt u de stappen in [Windows Hybrid Runbook Worker implementeren](../automation-windows-hrw-install.md) opnieuw installeren van de Hybrid Worker voor Windows of [een Hybrid Runbook Worker in Linux implementeren](../automation-linux-hrw-install.md) voor Linux.

* Voer de volgende query in uw werkruimte. Als u het resultaat ziet `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` hebt u een quotum dat is gedefinieerd in de werkruimte die is bereikt en is gestopt met de gegevens worden opgeslagen. Navigeer in uw werkruimte naar **gebruik en geraamde kosten** > **gegevensvolumebeheer** en controleren van uw quotum aanvragen of te verwijderen van de quota die u hebt.

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

## <a name="windows"></a>Windows

Als u problemen ondervindt tijdens een poging voor de Onboarding van de oplossing op een virtuele machine, controleert u de **Operations Manager** logboek voor systeemgebeurtenissen onder **logboeken toepassingen en Services** op de lokale computer voor gebeurtenissen met de gebeurtenis-ID **4502** en gebeurtenis-bericht met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

De volgende sectie worden specifieke foutberichten en een mogelijke oplossing voor elk gemarkeerd. Zie voor andere onboarding problemen, [oplossen onboarding van oplossingen](onboarding.md).

### <a name="machine-already-registered"></a>Scenario: Machine is al geregistreerd bij een ander account

#### <a name="issue"></a>Probleem

U ontvangt de volgende strekking weergegeven:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Oorzaak

De machine is al vrijgegeven aan een andere werkruimte voor updatebeheer.

#### <a name="resolution"></a>Oplossing

Voer het opruimen van oude artefacten op de computer door [verwijderen van de hybride runbookgroep](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) en probeer het opnieuw.

### <a name="machine-unable-to-communicate"></a>Scenario: Machine is kan niet communiceren met de service

#### <a name="issue"></a>Probleem

U ontvangt een van de volgende foutberichten:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Oorzaak

Er zijn mogelijk een proxy, de gateway of de firewall blokkeert de netwerkcommunicatie.

#### <a name="resolution"></a>Oplossing

Controleer uw netwerken en zorg ervoor dat de juiste poorten en -adressen zijn toegestaan. Zie [vereisten voor de](../automation-hybrid-runbook-worker.md#network-planning), voor een lijst met poorten en -adressen die zijn vereist voor het beheer van updates en Hybrid Runbook Workers.

### <a name="unable-to-create-selfsigned-cert"></a>Scenario: Kan geen zelfondertekend certificaat maken

#### <a name="issue"></a>Probleem

U ontvangt een van de volgende foutberichten:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Oorzaak

De Hybrid Runbook Worker is niet een zelfondertekend certificaat genereren

#### <a name="resolution"></a>Oplossing

Controleer of systeemaccount leestoegang heeft tot map **C:\ProgramData\Microsoft\Crypto\RSA** en probeer het opnieuw.

### <a name="failed-to-start"></a>Scenario: Ziet u een virtuele machine is niet gestart in een update-implementatie

#### <a name="issue"></a>Probleem

Een virtuele machine heeft de status **kan niet worden gestart** voor een virtuele machine. Wanneer u de specifieke details voor de machine bekijkt, ziet u de volgende fout:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan optreden vanwege een van de volgende redenen:

* De machine bestaat niet meer.
* De machine is ingeschakeld, uitgeschakeld en niet bereikbaar.
* De machine heeft een probleem met de netwerkverbinding en de hybrid worker op de computer niet bereikbaar is.
* Er is een update voor de Microsoft Monitoring Agent die de SourceComputerId gewijzigd
* De update-uitvoering zijn mogelijk beperkt als u de taaklimiet van 2000 gelijktijdige in een Automation-Account bereikt. Elke implementatie wordt beschouwd als een taak en elke computer in een aantal update-implementaties als een taak. Elke andere automation-taak of update implementatie momenteel wordt uitgevoerd in de telling van uw Automation-Account voor de taaklimiet voor gelijktijdige.

#### <a name="resolution"></a>Oplossing

Bij het gebruik van toepassing [dynamische groepen](../automation-update-management.md#using-dynamic-groups) voor de update-implementaties.

* Controleer of de machine nog steeds bestaat en bereikbaar is. Als deze niet bestaat, uw implementatie bewerken en verwijderen van de machine.
* Zie de sectie over [netwerkplanning](../automation-update-management.md#ports) voor een lijst met poorten en -adressen die zijn vereist voor het beheer van updates en controleer of de computer aan deze vereisten voldoet.
* Voer de volgende query in Log Analytics om te zoeken machines in uw omgeving waarvan `SourceComputerId` gewijzigd. Zoeken naar computers die dezelfde `Computer` waarde, maar verschillende `SourceComputerId` waarde. Als u de betrokken computers hebt gevonden, moet u de update-implementaties die doel van deze machines en verwijderen en opnieuw toevoegen van de machines bewerken zodat de `SourceComputerId` weerspiegelt de juiste waarde.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

### <a name="hresult"></a>Scenario: Machine wordt weergegeven als niet beoordeeld en ziet u een uitzondering van HResult

#### <a name="issue"></a>Probleem

U machines hebt die als **niet beoordeeld** onder **naleving**, en u ziet een bericht van uitzondering eronder.

#### <a name="cause"></a>Oorzaak

Windows Update of WSUS worden is niet correct geconfigureerd op de machine. Updatebeheer maakt gebruik van Windows Update of WSUS om te voorzien van de updates die nodig zijn, de status van de patch en de resultaten van patches die zijn geïmplementeerd. Zonder deze gegevens kan updatebeheer niet correct rapporteren van de patches die zijn vereist of geïnstalleerd.

#### <a name="resolution"></a>Oplossing

Dubbelklik op de uitzondering in rood om te zien van de gehele uitzonderingsbericht weergegeven. Bekijk de volgende tabel voor mogelijke oplossingen of acties te ondernemen:

|Uitzondering  |Oplossing of actie  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Zoeken naar de relevante foutcode in [Windows update code foutenlijst](https://support.microsoft.com/help/938205/windows-update-error-code-list) naar aanvullende informatie vinden over de oorzaak van de uitzondering.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Deze fouten zijn problemen met de netwerkverbinding. Zorg ervoor dat uw computer de juiste netwerkverbinding met updatebeheer heeft. Zie de sectie over [netwerkplanning](../automation-update-management.md#ports) voor een lijst met poorten en -adressen die vereist zijn.        |
|`0x8024001E`| De updatebewerking is niet voltooid omdat de service of het systeem werd afgesloten.|
|`0x8024002E`| Windows Update-service is uitgeschakeld.|
|`0x8024402C`     | Als u van een WSUS-server gebruikmaakt, controleert u of de registerwaarden voor `WUServer` en `WUStatusServer` onder de registersleutel `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` hebt u de juiste WSUS-server.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Zorg ervoor dat de Windows Update-service (wuauserv) wordt uitgevoerd en niet is uitgeschakeld.        |
|Andere algemene uitzondering     | Voer een zoekopdracht uit op het internet voor de mogelijke oplossingen en werken met uw lokale IT-ondersteuning.         |

Controleren van de `windowsupdate.log` kunt u proberen om te bepalen, evenals de mogelijke oorzaak. Zie voor meer informatie over het lezen van het logboek [over het lezen van het bestand WindowsUpdate.log](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

U kunt ook downloaden en uitvoeren de [Windows Update-probleemoplosser](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) om te controleren of er problemen met Windows Update op de computer zijn.

> [!NOTE]
> De [Windows Update-probleemoplosser](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) is voor Windows-clients, maar het werkt op Windows Server wordt ook aangegeven.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Scenario: Update-uitvoering niet kan worden gestart

#### <a name="issue"></a>Probleem

Een update wordt uitgevoerd niet starten op een Linux-machine.

#### <a name="cause"></a>Oorzaak

De Hybrid Worker in Linux is beschadigd.

#### <a name="resolution"></a>Oplossing

Maak een kopie van het volgende logboekbestand en handhaven voor het oplossen van problemen:

```bash
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Scenario: Update-uitvoering wordt gestart, maar er een fout optreedt

#### <a name="issue"></a>Probleem

Een update-uitvoering wordt gestart, maar er een fout optreedt tijdens de uitvoering.

#### <a name="cause"></a>Oorzaak

Mogelijke oorzaken zijn:

* Pakketbeheer is niet in orde
* Specifieke pakketten kunnen leiden tot problemen met cloud op basis van patches
* Andere redenen

#### <a name="resolution"></a>Oplossing

Als er fouten optreden tijdens het bijwerken uitvoeren nadat deze is in Linux is gestart, controleert u de taak uitvoer van de geïnfecteerde computer in de uitvoering. Mogelijk vindt u specifieke foutberichten van Pakketbeheer van de computer die u kunt onderzoeken en actie ondernemen. Updatebeheer is de package manager in orde voor geslaagde update-implementaties zijn vereist.

In sommige gevallen pakketupdates kunnen leiden tot problemen met Update Management te voorkomen dat een update-implementatie niet worden voltooid. Als u dat ziet, hebt u deze handmatig installeren of deze pakketten uitsluiten van toekomstige update-uitvoeringen zelf.

Als u een probleem met een patch niet kunt oplossen, maakt u een kopie van het volgende logboekbestand en handhaven **voordat** de volgende update-implementatie is gestart voor het oplossen van problemen:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
