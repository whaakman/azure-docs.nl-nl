---
title: Oplossing voor updatebeheer in Azure
description: In dit artikel is bedoeld om te begrijpen hoe u kunt de oplossing Update Management van Azure gebruiken om updates voor uw Windows- en Linux-computers te beheren.
services: automation
ms.service: automation
ms.subservice: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1af2117b1d12c98182434705181462fd7c9bebf4
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862944"
---
# <a name="update-management-solution-in-azure"></a>Oplossing voor updatebeheer in Azure

U kunt de oplossing Update Management in Azure Automation gebruiken voor het beheren van besturingssysteemupdates voor uw Windows- en Linux-computers in Azure, on-premises omgevingen en andere cloudproviders. U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en de procedure voor het installeren van vereiste updates voor servers beheren.

U kunt updatebeheer inschakelen voor virtuele machines rechtstreeks vanuit uw Azure Automation-account. Zie voor informatie over het inschakelen van updatebeheer voor virtuele machines vanaf uw Automation-account, [beheren van updates voor meerdere virtuele machines](manage-update-multi.md). U kunt ook de updatebeheer inschakelen voor een virtuele machine op de pagina van de virtuele machine in Azure portal. In dit scenario is beschikbaar voor [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) en [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) virtuele machines.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="solution-overview"></a>Oplossingenoverzicht

Computers die worden beheerd door de Update Management gebruiken de volgende configuraties voor het uitvoeren van de evaluatie en update-implementaties:

* Microsoft Monitoring Agent (MMA) voor Windows of Linux
* PowerShell Desired State Configuration (DSC) voor Linux
* Automation Hybrid Runbook Worker
* Microsoft Update of Windows Server Update Services (WSUS) voor Windows-computers

Het volgende diagram ziet u een conceptueel overzicht van het gedrag en de gegevensstroom van hoe de oplossing beoordeelt en beveiligingsupdates geldt voor alle verbonden Windows Server en Linux-computers in een werkruimte:

![Processtroom voor het beheer van bijwerken](./media/automation-update-management/update-mgmt-updateworkflow.png)

Updatebeheer kan worden gebruikt om systeemeigen Onboarding van machines in meerdere abonnementen in dezelfde tenant.

Zodra een CVE release is, duurt het 2-3 uur voor de patch voor Linux-machines voor evaluatie wordt weergegeven.  Het duurt 15-12 uur voor de patch om weer te geven voor de beoordeling nadat deze is vrijgegeven voor Windows-machines.

Nadat een computer is voltooid scannen voor Updatevereisten, stuurt de agent de informatie in bulk naar Azure Monitor-Logboeken. Op een Windows-computer, de nalevingsscan standaard elke 12 uur uitgevoerd.

Naast het schema voor scannen, wordt de scan voor naleving van updates binnen 15 minuten van de MMA opnieuw wordt gestart, voordat de installatie van de update en na installatie van update gestart.

Voor een Linux-computer wordt de nalevingsscan standaard elke drie uur uitgevoerd. Als de MMA-agent opnieuw is opgestart, wordt een nalevingsscan gestart binnen 15 minuten.

De oplossing rapporteert hoe up-to-date de computer is gebaseerd op de bron waarmee u bent geconfigureerd om te synchroniseren met. Als de Windows-computer is geconfigureerd om te rapporteren aan WSUS, afhankelijk van wanneer WSUS laatste synchronisatie met Microsoft Update, wordt de resultaten afwijken van wat Microsoft-Updates wordt weergegeven. Dit gedrag is hetzelfde voor Linux-computers die zijn geconfigureerd voor rapportage aan een lokale opslagplaats in plaats van naar een openbare opslagplaats.

> [!NOTE]
> Beheer van updates vereist voor het correct rapporteren aan de service, bepaalde URL's en poorten worden ingeschakeld. Zie voor meer informatie over deze vereisten, [netwerk planning voor Hybrid Workers](automation-hybrid-runbook-worker.md#network-planning).

U kunt software-updates implementeren en installeren op computers die updates vereisen door daarvoor een planning in te stellen. Updates die zijn geclassificeerd als *optioneel* niet zijn opgenomen in het implementatiebereik voor Windows-computers. Alleen vereiste updates zijn opgenomen in het implementatiebereik.

De geplande implementatie wordt gedefinieerd welke doelcomputers de updates worden geïmplementeerd, ontvangen door computers expliciet op te geven of door het selecteren van een [computergroep](../azure-monitor/platform/computer-groups.md) die gebaseerd op logboekzoekopdrachten van een specifieke set computers. U geeft ook een planning voor het goedkeuren en instellen van een bepaalde periode gedurende welke updates kunnen worden geïnstalleerd. Deze periode wordt het onderhoudsvenster genoemd. Tien minuten van het onderhoudsvenster is gereserveerd voor opnieuw opstarten als een herstart nodig en u hebt geselecteerd de optie voor de juiste opnieuw opstarten. Opnieuw opstarten wordt niet uitgevoerd als patches langer duurt dan verwacht en er minder dan tien minuten binnen het onderhoudsvenster is.

Updates worden geïnstalleerd door runbooks in Azure Automation. U kunt deze runbooks niet weergeven en de runbooks vereisen geen configuratie. Wanneer een update-implementatie wordt gemaakt, wordt een planning waarmee een masterupdate-runbook op de opgegeven tijd voor de opgenomen computers gestart door de update-implementatie gemaakt. De master-runbook start een onderliggend runbook op elke agent om de vereiste updates te installeren.

Op de datum en tijd die is opgegeven in de update-implementatie, uitvoeren de doelcomputers de implementatie parallel. Een scan wordt uitgevoerd vóór de installatie om te controleren of de updates nog steeds vereist. Voor WSUS-clientcomputers, als de updates niet zijn goedgekeurd in WSUS, mislukt de update-implementatie.

Met een apparaat dat is geregistreerd voor updatebeheer in meer dan één Log Analytics-werkruimten (multihoming) wordt niet ondersteund.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Ondersteunde client-typen

De volgende tabel ziet u een lijst met ondersteunde besturingssystemen:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Ondersteunt alleen bijwerken evaluaties.         |
|Windows Server 2008 R2 SP1 en hoger (inclusief Windows Server 2012- en 2016)    |.NET framework 4.5.1 of later is vereist. ([.NET Framework downloaden](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 of hoger is vereist. ([WMF 4.0 downloaden](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1 wordt aanbevolen voor hogere mate van betrouwbaarheid.  ([Download WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) en 7 (x64)      | Linux-agents moeten toegang hebben tot een opslagplaats voor updates. Patch toepassen op classificatie gebaseerde vereist 'yum' om terug te keren beveiligingsgegevens CentOS hoeft niet dezelfde kant. Zie voor meer informatie op classificatie gebaseerde patches op CentOS [Updateclassificaties op Linux](#linux-2)          |
|Red Hat Enterprise 6 (x86/x64) en 7 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Ubuntu 14.04 LTS, 16.04 LTS en 18.04 (x86/x64)      |Linux-agents moeten toegang hebben tot een opslagplaats voor updates.         |

### <a name="unsupported-client-types"></a>Niet-ondersteunde client-typen

De volgende tabel geeft een overzicht van de besturingssystemen die niet worden ondersteund:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows-client     | Client-besturingssystemen (zoals Windows 7 en Windows 10) worden niet ondersteund.        |
|Windows Server 2016 Nano Server     | Wordt niet ondersteund.       |

### <a name="client-requirements"></a>Clientvereisten

#### <a name="windows"></a>Windows

Windows-agents moeten worden geconfigureerd om te communiceren met een WSUS-server of ze moeten toegang hebben tot Microsoft Update. U kunt updatebeheer gebruiken met System Center Configuration Manager. Zie voor meer informatie over scenario's voor gegevensintegratie, [System Center Configuration Manager integreren met updatebeheer](oms-solution-updatemgmt-sccmintegration.md#configuration). De [Windows agent](../azure-monitor/platform/agent-windows.md) is vereist. De agent wordt automatisch geïnstalleerd als u onboarding van een virtuele machine van Azure.

#### <a name="linux"></a>Linux

Voor Linux, moet de computer toegang hebben tot een opslagplaats voor updates. De opslagplaats van de update kan privé of openbaar zijn. TLS 1.1 of TLS 1.2 is vereist om te communiceren met updatebeheer. Een Log Analytics-Agent voor Linux die geconfigureerd voor rapportage aan meer dan één Log Analytics-werkruimten wordt niet ondersteund met deze oplossing.

Zie voor meer informatie over het installeren van de Log Analytics-Agent voor Linux en de nieuwste versie te downloaden, [Log Analytics-Agent voor Linux](https://github.com/microsoft/oms-agent-for-linux). Zie voor meer informatie over het installeren van de Log Analytics-Agent voor Windows [Microsoft Monitoring Agent voor Windows](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>Machtigingen

Als u wilt maken en beheren-update-implementaties, moet u specifieke machtigingen. Zie voor meer informatie over deze machtigingen, [Role-based access - updatebeheer](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Oplossingsonderdelen

De oplossing bestaat uit de volgende resources. De resources worden toegevoegd aan uw Automation-account. Ze beide rechtstreeks verbonden agents of in een Operations Manager-verbonden beheergroep.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-groepen

Nadat u deze oplossing inschakelt, wordt een Windows-computer die rechtstreeks verbonden met uw Log Analytics-werkruimte automatisch geconfigureerd als Hybrid Runbook Worker voor ondersteuning van de runbooks die zijn opgenomen in deze oplossing.

Elke Windows-computer die wordt beheerd door de oplossing wordt vermeld in de **Hybrid worker-groepen** deelvenster als een **System hybrid worker-groep** voor het Automation-account. De oplossingen maken gebruik van de naamconventie *Hostname FQDN_GUID*. U kunt deze groepen met runbooks niet targeten in uw account. Ze mislukken als u probeert. Deze groepen zijn bedoeld ter ondersteuning van alleen de management-oplossing.

U kunt de Windows-computers toevoegen aan een Hybrid Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks als u hetzelfde account voor zowel de oplossing en het lidmaatschap van de Hybrid Runbook Worker gebruiken. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.

### <a name="management-packs"></a>Management packs

Als uw System Center Operations Manager-beheergroep is verbonden met een Log Analytics-werkruimte, worden de volgende management packs geïnstalleerd in Operations Manager. Deze management packs worden ook op rechtstreeks verbonden zijn met Windows-computers geïnstalleerd nadat u de oplossing toevoegt. U hoeft niet te configureren of beheren van deze management packs.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Implementatie MP bijwerken

> [!NOTE]
> Als u een beheergroep van Operations Manager 1807 waarop agents zijn geconfigureerd op het niveau van de beheergroep moet worden gekoppeld aan een werkruimte hebt, de huidige oplossing om op te halen ze worden weergegeven voor de onderdrukking is **IsAutoRegistrationEnabled** aan **Waar** in de **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init** regel.

Zie voor meer informatie over hoe management packs voor oplossingen worden bijgewerkt, [Operations Manager verbinden met Azure Monitor-logboeken](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Voor systemen waarop de Operations Manager-Agent, om te kunnen volledig worden beheerd door beheer van updates, de agent moet worden bijgewerkt naar de Microsoft Monitoring Agent. Zie voor informatie over het bijwerken van de agent, [upgrade uitvoeren van een Operations Manager-agent](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). Voor omgevingen met behulp van Operations Manager, is het vereist dat u System Center Operations Manager 2012 R2 UR 14 of hoger wordt uitgevoerd.

## <a name="onboard"></a>Updatebeheer inschakelen

Als u wilt herstellen van systemen, moet u de oplossing Update Management inschakelen. Er zijn veel manieren om onboarding machines updatebeheer. De volgende zijn de aanbevolen en manieren waarop u de oplossing ondersteund:

* [Van een virtuele machine](automation-onboard-solutions-from-vm.md)
* [Vanuit het bladeren door meerdere virtuele machines](automation-onboard-solutions-from-browse.md)
* [Via uw Automation-account](automation-onboard-solutions-from-automation-account.md)
* [Met een Azure Automation-runbook](automation-onboard-solutions.md)
  
### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Controleer of niet-Azure-machines zijn toegevoegd

Om te bevestigen dat rechtstreeks verbonden zijn met machines na een paar minuten zijn communicatie met Azure Monitor-Logboeken, kunt u uitvoeren een de volgende zoekopdrachten in Logboeken.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Op een Windows-computer, kunt u de volgende informatie om te controleren of agents verbonden zijn met Azure Monitor logboeken bekijken:

1. Open in het Configuratiescherm, **Microsoft Monitoring Agent**. Op de **Azure Log Analytics** tabblad en de agent het volgende bericht weergegeven: **De Microsoft Monitoring Agent is verbonden met Log Analytics**.
2. Open het Windows-gebeurtenislogboek. Ga naar **toepassings- en servicelogboeken\operations Manager** en zoek naar gebeurtenis-ID 3000 en 5002 van de gebeurtenis-ID van de bron **serviceconnector**. Deze gebeurtenissen geven aan dat de computer is geregistreerd bij de Log Analytics-werkruimte en of deze configuratie ontvangt.

Als de agent kan niet met communiceren Azure Monitor-logboeken en de agent is geconfigureerd om te communiceren met internet via een firewall of proxyserver, Controleer of de firewall of proxyserver correct is geconfigureerd. Zie voor informatie over het controleren of de firewall of proxyserver correct is geconfigureerd, [netwerkconfiguratie voor Windows-agent](../azure-monitor/platform/agent-windows.md) of [netwerkconfiguratie voor Linux-agent](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Als uw Linux-systemen zijn geconfigureerd om te communiceren met een proxy of Log Analytics-Gateway en u bent onboarding van deze oplossing, update de *proxy.conf* machtigingen voor het verlenen van de groep omiuser leesmachtigingen voor het bestand met behulp van de de volgende opdrachten:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Toegevoegde Linux-agents weer de status van **bijgewerkt** nadat een evaluatie is uitgevoerd. Dit proces kan maximaal zes uur duren.

Om te bevestigen dat een Operations Manager-beheergroep met Azure Monitor-logboeken communiceert, Zie [valideren Operations Manager-integratie met Azure Monitor logboeken](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor).

## <a name="data-collection"></a>Gegevensverzameling

### <a name="supported-agents"></a>Ondersteunde agents

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing:

| Verbonden bron | Ondersteund | Description |
| --- | --- | --- |
| Windows-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Windows-agents en start vervolgens de installatie van vereiste updates. |
| Linux-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Linux-agents en start vervolgens de installatie van vereiste updates op ondersteunde distributies. |
| Beheergroep Operations Manager |Ja |De oplossing verzamelt informatie over systeemupdates van agents in een verboden beheergroep.<br/>Een directe verbinding van Operations Manager-agent naar Logboeken van Azure Monitor is niet vereist. Gegevens uit de beheergroep doorgestuurd naar de Log Analytics-werkruimte. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie

Een scan wordt uitgevoerd twee keer per dag voor elke beheerde Windows-computer. Elke 15 minuten, de Windows-API wordt aangeroepen om op te vragen de laatste updatetijd om te bepalen of de status is gewijzigd. Als de status is gewijzigd, wordt er een nalevingsscan gestart.

Voor elke Linux-computer beheerde, wordt elke drie uur aan een scan uitgevoerd.

Duurt tussen 30 minuten en 6 uur voor het dashboard bijgewerkte gegevens van beheerde computers worden weergegeven.

Het gemiddelde gebruik van Azure Monitor Logboeken gegevens voor een virtuele machine met updatebeheer is ongeveer 25MB per maand. Deze waarde is slechts een schatting en kan worden gewijzigd op basis van uw omgeving. Het raadzaam dat u uw omgeving om te zien van de exacte syntaxis die u hebt gecontroleerd.

## <a name="viewing-update-assessments"></a>Update-evaluaties weergeven

Selecteer in uw Automation-account **updatebeheer** om de status van uw machines weer te geven.

In deze weergave bevat informatie over uw computers, met ontbrekende updates, update-implementaties en geplande update-implementaties. In de **naleving kolom**, ziet u de laatste keer dat de machine is beoordeeld. In de **gereedheid voor UPDATE-AGENT** kolom, u kunt zien als de status van de update-agent. Als er een probleem is, selecteert u de koppeling om te gaan naar ondersteunende documentatie waarmee kunt dat u meer te weten welke stappen moeten uitvoeren om het probleem te verhelpen.

Als u wilt een logboekzoekopdracht die informatie over de machine retourneert uitvoeren, selecteert update, of de implementatie, u het item in de lijst. De **zoeken in logboeken** deelvenster geopend met een query voor het geselecteerde item:

![Standaardweergave updatebeheer](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Updates installeren

Nadat updates zijn beoordeeld voor alle Linux- en Windows-computers in uw werkruimte, kunt u de vereiste updates installeren door het maken van een *update-implementatie*. Een update-implementatie is een geplande installatie van vereiste updates voor een of meer computers. U geeft de datum en tijd voor de implementatie en een computer of groep computers die u wilt opnemen in het bereik van een implementatie. Zie voor meer informatie over computergroepen, [computergroepen in Logboeken van Azure Monitor](../azure-monitor/platform/computer-groups.md).

 Wanneer u in uw update-implementatie computergroepen, wordt het lidmaatschap van slechts één keer geëvalueerd op het moment van schema maken. Wijzigingen aan een groep worden niet weergegeven. Om op te halen om deze [dynamische groepen](#using-dynamic-groups), deze groepen worden omgezet tijdens de implementatie en zijn gedefinieerd door een query.

> [!NOTE]
> Windows virtuele machines die zijn geïmplementeerd vanuit de Azure Marketplace standaard zijn ingesteld op automatische updates ontvangen van Windows Update-Service. Dit gedrag verandert niet wanneer u deze oplossing toevoegt of Windows virtuele machines aan uw werkruimte toevoegen. Als u geen actief updates beheren met behulp van deze oplossing, wordt het standaardgedrag (automatisch updates wilt toepassen) is van toepassing.

Om te voorkomen dat updates buiten een onderhoudsperiode in Ubuntu worden toegepast, de configuratie van het pakket Unattended-Upgrade automatische updates uitschakelen. Zie voor meer informatie over het configureren van het pakket [onderwerp Automatic Updates in de Ubuntu Server Guide](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Virtuele machines die zijn gemaakt via de on-demand Red Hat Enterprise Linux (RHEL)-installatiekopieën die beschikbaar in de Azure Marketplace zijn zijn geregistreerd voor toegang tot de [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) die geïmplementeerd in Azure. Andere Linux-distributies moet uit de opslagplaats van de distributie van online-bestand aan de hand van de ondersteunde distributiemethoden worden bijgewerkt.

Voor het maken van een nieuwe update-implementatie selecteert **update-implementatie plannen**. De **nieuwe Update-implementatie** deelvenster wordt geopend. Voer waarden in voor de eigenschappen die worden beschreven in de volgende tabel en klik vervolgens op **maken**:

| Eigenschap | Description |
| --- | --- |
| Name |Unieke naam voor het identificeren van de update-implementatie. |
|Besturingssysteem| Linux of Windows|
| Groepen om bij te werken (preview)|definieer een query op basis van een combinatie van abonnement, resourcegroepen, locaties en tags om een dynamische groep virtuele Azure-machines te bouwen voor opname in uw implementatie. Zie [Dynamische groepen](automation-update-management.md#using-dynamic-groups) voor meer informatie|
| Bij te werken computers |selecteer een opgeslagen zoekopdracht of geïmporteerde groep, of kies Computer in de vervolgkeuzelijst en selecteer de afzonderlijke computers. Als u **Computers** selecteert, wordt de gereedheid van de computer weergegeven in de kolom **GEREEDHEID VOOR UPDATE-AGENT**.</br> Zie [Computergroepen in Azure Monitorlogboeken](../azure-monitor/platform/computer-groups.md) voor meer informatie over de verschillende manieren waarop u computergroepen kunt maken in Azure Monitor-logboeken |
|Updateclassificaties|Selecteer de updateclassificaties die u nodig hebt|
|Updates opnemen/uitsluiten|Hiermee opent u de **opnemen/uitsluiten** pagina. Updates die moeten worden opgenomen of uitgesloten, worden op afzonderlijke tabbladen weergegeven. Zie [Werking van opname](automation-update-management.md#inclusion-behavior) voor meer informatie over hoe de opname wordt verwerkt |
|Planningsinstellingen|Selecteer de tijd om te starten, en selecteer een van beide eenmaal of terugkerende voor het terugkeerpatroon|
| Scripts die voorafgaan aan en scripts die volgen|Selecteer de scripts worden uitgevoerd vóór en na de implementatie|
| Onderhoudsvenster |Het aantal minuten instellen voor updates. De waarde mag niet kleiner zijn dan 30 minuten en niet meer dan 6 uur |
| Opnieuw opstarten van besturingselement| Bepaalt hoe vaak opnieuw opstarten moeten worden verwerkt. De volgende opties zijn beschikbaar:</br>Opnieuw opstarten indien nodig (standaard)</br>Altijd opnieuw opstarten</br>Nooit opnieuw opstarten</br>Alleen opnieuw opstarten - updates worden niet geïnstalleerd|

Update-implementaties kunnen ook programmatisch worden gemaakt. Zie voor meer informatie over het maken van een Update-implementatie met de REST-API, [configuraties van Software-Update - maken](/rest/api/automation/softwareupdateconfigurations/create). Er is ook een voorbeeldrunbook dat kan worden gebruikt om een wekelijkse Update-implementatie te maken. Zie voor meer informatie over dit runbook, [een wekelijkse update-implementatie voor een of meer virtuele machines in een resourcegroep maken](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

### <a name="multi-tenant"></a>Cross-tenant-Update-implementaties

Als u computers in een andere Azure-tenant rapporteren aan beheer van updates die u nodig hebt voor het vullen van hebt, moet u de volgende oplossing gebruiken om op te halen ze gepland. U kunt de [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet met de switch `-ForUpdate` een planning maken en gebruiken de [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet en door te geven de machines in de andere tenant om de `-NonAzureComputer` parameter. Het volgende voorbeeld toont een voorbeeld van hoe u dit doet:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="view-missing-updates"></a>Ontbrekende updates weergeven

Selecteer **ontbrekende updates** om de lijst met updates die niet aanwezig in uw machines zijn weer te geven. Elke update wordt weergegeven en kan worden geselecteerd. Informatie over het aantal machines waarvoor de update, het besturingssysteem en een koppeling voor meer informatie wordt weergegeven. De **zoeken in logboeken** deelvenster ziet u meer informatie over de updates.

## <a name="view-update-deployments"></a>Weergave update-implementaties

Selecteer de **Update Deployments** tabblad om de lijst met bestaande update-implementaties weer te geven. Selecteer een van de update-implementaties in de tabel om te openen de **Update-implementatie uitvoeren** deelvenster voor deze update-implementatie.

![Overzicht van de resultaten van de update-implementatie](./media/automation-update-management/update-deployment-run.png)

Als u een update-implementatie van de REST-API, Zie [Software Update-configuratie wordt uitgevoerd](/rest/api/automation/softwareupdateconfigurationruns).

## <a name="update-classifications"></a>Updateclassificaties

De volgende tabellen worden de updateclassificaties in Update Management met een definitie voor elke classificatie.

### <a name="windows"></a>Windows

|Classificatie  |Description  |
|---------|---------|
|Essentiële updates     | Een update voor een specifiek probleem die een kritieke bug niet-beveiliging.        |
|Beveiligingsupdates     | Een update voor een probleem met de productspecifieke, productspecifieke beveiliging.        |
|Updatepakketten     | Een volledige reeks van hotfixes die samen zijn verpakt voor een gemakkelijke implementatie.        |
|Functiepakketten     | Nieuwe productfuncties die zijn gedistribueerd buiten een productrelease.        |
|Servicepacks     | Een volledige reeks van hotfixes die op een toepassing worden toegepast.        |
|Definitie-updates     | Een update voor antivirus- of andere definitiebestanden.        |
|Hulpprogramma's     | Een hulpprogramma of onderdeel aan waarmee een of meer taken uitvoeren.        |
|Updates     | Een update voor een toepassing of bestand dat momenteel wordt geïnstalleerd.        |

### <a name="linux"></a>Linux

|Classificatie  |Description  |
|---------|---------|
|Essentiële en beveiligingsupdates     | Updates voor een specifiek probleem of een probleem met de productspecifieke, productspecifieke beveiliging.         |
|Andere Updates     | Alle overige updates die niet essentieel zijn in de aard of die niet-beveiligingsupdates.        |

Voor Linux, updatebeheer, kunnen onderscheid maken tussen essentiële updates en beveiligingsupdates in de cloud bij het weergeven van gegevens voor de evaluatie vanwege gegevensverrijking in de cloud. Het toepassen van patches, gebruikmaakt van updatebeheer classificatie gegevens beschikbaar is op de computer. In tegenstelling tot andere distributies CentOS heeft geen deze informatie beschikbaar buiten het vak. Hebt u CentOS-machines die zijn geconfigureerd op een manier om terug te keren van beveiligingsgegevens voor de volgende opdracht, zich updatebeheer voor het patchen van op basis van classificaties.

```bash
sudo yum -q --security check-update
```

Er is momenteel geen ondersteunde methode waarmee de beschikbaarheid van de systeemeigen classificatie-gegevens op CentOS. Op dit moment worden alleen best-effort-ondersteuning is beschikbaar voor klanten die mogelijk zijn ingeschakeld deze op hun eigen.

## <a name="firstparty-predownload"></a>Geavanceerde instellingen

Beheer van updates is afhankelijk van Windows Update voor Windows-Updates downloaden en installeren. Als gevolg hiervan respecteren we veel van de instellingen die door Windows Update. Als u instellingen voor niet-Windows-updates inschakelen, wordt deze updates ook updatebeheer kunnen beheren. Als u inschakelen updates downloaden wilt voordat er een update-implementatie optreedt, kunnen de update-implementaties gaat sneller en minder waarschijnlijk het onderhoudsvenster overschrijden.

### <a name="pre-download-updates"></a>Pre-updates downloaden

Als u wilt configureren updates automatisch downloaden in Groepsbeleid, kunt u instellen de [instelling Automatische Updates configureren](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#BKMK_comp5) naar **3**. Deze downloadt de updates die nodig zijn op de achtergrond, maar ze niet installeren. Dit houdt u updatebeheer in beheer van schema's, maar toestaan van updates voor het downloaden van buiten het onderhoudsvenster voor updatebeheer. Dit kan verhinderen dat **onderhoudsvenster is overschreden** fouten in de Update Management.

U kunt dit ook instellen met PowerShell, de volgende PowerShell ook uitvoeren op een systeem dat u de automatische download van updates wilt.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

### <a name="disable-automatic-installation"></a>Schakel de automatische installatie

Azure virtuele machines hebben automatische installatie van updates standaard ingeschakeld. Dit kan leiden tot updates moeten worden geïnstalleerd voordat u ze moet worden geïnstalleerd door beheer van updates te plannen. U kunt dit gedrag uitschakelen door in te stellen de `NoAutoUpdate` registersleutel in op `1`. De volgende PowerShell-codefragment toont één manier om dit te doen.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

### <a name="enable-updates-for-other-microsoft-products"></a>Updates voor andere Microsoft-producten inschakelen

Standaard biedt Windows Update alleen updates voor Windows. Als u inschakelt **updates ontvangen voor andere Microsoft-producten als ik Windows update**, krijgt u updates voor andere producten, met inbegrip van beveiligingspatches voor SQL Server of andere eerste software van derden. Deze optie kan niet worden geconfigureerd door Groepsbeleid. De volgende PowerShell ook uitvoeren op de systemen die u andere eerste partij patches inschakelen wilt op en beheer van updates wordt geacht deze instelling.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="third-party"></a> Externe patches voor Windows

Beheer van updates is afhankelijk van WSUS of Windows Update voor het patchen van ondersteunde Windows-systemen. Hulpprogramma's zoals [System Center Updates Publisher](/sccm/sum/tools/updates-publisher
) (Updates Publisher) kunt u aangepaste updates naar WSUS te publiceren. In dit scenario kunt updatebeheer patch-machines die gebruikmaken van WSUS als de update-opslagplaats met software van derden. Zie voor informatie over het configureren van Updates Publisher, [installeren Updates Publisher](/sccm/sum/tools/install-updates-publisher).

## <a name="ports"></a>Netwerken plannen

De volgende adressen zijn vereist voor het beheer van updates. Communicatie met deze adressen vindt plaats via poort 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

Zie voor meer informatie over de poorten die vereist dat de Hybrid Runbook Worker [Hybrid Worker-rol poorten](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Het is raadzaam om de adressen die worden vermeld bij het definiëren van uitzonderingen te gebruiken. Voor IP-adressen die u kunt downloaden de [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Dit bestand wordt wekelijks bijgewerkt, en weerspiegelt bereiken momenteel zijn geïmplementeerd en eventuele toekomstige wijzigingen in de IP-adresbereiken.

## <a name="search-logs"></a>Logboeken zoeken

Naast de details die beschikbaar zijn in Azure portal, kunt u zoeken op basis van de logboeken doen. Selecteer op de pagina's van de oplossing, **Log Analytics**. De **zoeken in logboeken** deelvenster wordt geopend.

U kunt ook meer informatie over het aanpassen van de query of het gebruik van verschillende clients en meer door naar de pagina:  [Log Analytics zoeken naar API-documentatie](
https://dev.loganalytics.io/).

### <a name="sample-queries"></a>Voorbeeldquery's

De volgende secties bevatten voorbeeld logboeken-query's voor updaterecords die worden verzameld door deze oplossing:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Enkele query's de evaluatie van de Azure-VM (Windows)

Vervang de waarde VMUUID met de VM-GUID van de virtuele machine die u query's. U vindt de VMUUID die moet worden gebruikt door de volgende query uit te voeren in Azure Monitor-Logboeken: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Ontbrekende updates samenvatting

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="missing-updates-list"></a>Lijst met ontbrekende updates

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

#### <a name="single-azure-vm-assessment-queries-linux"></a>Één virtuele machine van Azure evaluatie van de query's (Linux)

Voor sommige Linux-distributies, er is een [endianness](https://en.wikipedia.org/wiki/Endianness) komt niet overeen met de waarde VMUUID die afkomstig zijn van Azure Resource Manager en wat wordt opgeslagen in Azure Monitor-Logboeken. De volgende query uit controleert een overeenkomst op een van beide endianness. Vervang de waarden VMUUID door de big endian en weinig-endian-indeling van de GUID juist de resultaten worden geretourneerd. U vindt de VMUUID die moet worden gebruikt door de volgende query uit te voeren in Azure Monitor-Logboeken: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Ontbrekende updates samenvatting

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Lijst met ontbrekende updates

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

#### <a name="multi-vm-assessment-queries"></a>Query's multi-VM-evaluatie

##### <a name="computers-summary"></a>Samenvatting van computers

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Ontbrekende updates samenvatting

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="computers-list"></a>Lijst met computers

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

##### <a name="missing-updates-list"></a>Lijst met ontbrekende updates

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="using-dynamic-groups"></a>Met behulp van dynamische groepen (preview)

Updatebeheer biedt de mogelijkheid om u te richten op een dynamische groep virtuele Azure-machines voor update-implementaties. Deze groepen zijn gedefinieerd door een query, wanneer een update-implementatie begint, de leden van die groep worden geëvalueerd. Dynamische groepen werken niet met klassieke virtuele machines. Bij het definiëren van uw query, kunnen de volgende items samen worden gebruikt voor het vullen van de dynamische groep

* Abonnement
* Resourcegroepen
* Locaties
* Tags

![Groepen selecteren](./media/automation-update-management/select-groups.png)

Preview-versie van de resultaten van een dynamische groep, klikt u op de **Preview** knop. In dit voorbeeld worden het lidmaatschap van de op dat moment wordt in dit voorbeeld wij zoeken naar computers met de tag **rol** is gelijk aan **BackendServer**. Als u meer machines hebben deze tag toegevoegd, worden ze voor alle toekomstige implementaties op basis van die groep worden toegevoegd.

![Preview-groepen](./media/automation-update-management/preview-groups.png)

## <a name="integrate-with-system-center-configuration-manager"></a>Integreren met System Center Configuration Manager

Klanten die in System Center Configuration Manager voor het beheren van pc's, servers en mobiele apparaten hebben geïnvesteerd is ook afhankelijk van de kracht en volwassenheid van Configuration Manager zodat ze de software-updates beheren. Configuration Manager maakt deel uit van de software update management (som)-cyclus.

Zie voor meer informatie over het integreren van de oplossing voor beheer met System Center Configuration Manager, [System Center Configuration Manager integreren met updatebeheer](oms-solution-updatemgmt-sccmintegration.md).

## <a name="inclusion-behavior"></a>Opname-gedrag

Opname van de update kunt u opgeven van specifieke updates om toe te passen. Patches of pakketten die opgenomen zijn, zijn geïnstalleerd. Wanneer Patches of pakketten opgenomen worden en een classificatie ook is geselecteerd, worden de opgenomen items en de items die voldoen aan de classificatie geïnstalleerd.

Het is belangrijk te weten dat uitsluitingen insluitingen overschrijven. Bijvoorbeeld, als u een uitsluitingsregel van definieert `*`, en er zijn geen patches of pakketten worden geïnstalleerd als ze zijn al uitgesloten. Als het ontbreken van de machine nog steeds weergeven patches uitgesloten. Voor Linux-machines als een pakket opgenomen is, maar een afhankelijk pakket die is uitgesloten, heeft is het pakket niet geïnstalleerd.

## <a name="patch-linux-machines"></a>Patch voor Linux-machines

De volgende secties worden potentiële problemen met Linux-patches.

### <a name="unexpected-os-level-upgrades"></a>Onverwachte besturingssysteemniveau upgrades

Op sommige varianten van Linux, zoals Red Hat Enterprise Linux, optreden OS-niveau upgrades via pakketten. Dit kan leiden tot beheer van updates wordt uitgevoerd wanneer het versienummer van het besturingssysteem wordt gewijzigd. Omdat de Update Management maakt gebruik van dezelfde methoden om bij te werken van pakketten die een beheerder lokaal op de Linux-computer wilt gebruiken, is dit gedrag is opzettelijk.

Om te voorkomen dat het bijwerken van de versie van het besturingssysteem via beheer van updates wordt uitgevoerd, gebruikt u de **uitsluiting** functie.

De naam van het pakket om uit te sluiten is in Red Hat Enterprise Linux, Red Hat-release-server.x86_64.

![Pakketten om uit te sluiten voor Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Kritieke / beveiligingspatches worden niet toegepast

Wanneer u updates voor een Linux-machine implementeert, kunt u de updateclassificaties selecteren. Hiermee worden de updates die worden toegepast op de computer en die voldoen aan de opgegeven criteria gefilterd. Dit filter is lokaal toegepast op de computer wanneer de update wordt geïmplementeerd.

Omdat update verrijking updatebeheer in de cloud uitvoert, voor sommige updates kunnen bijvoorbeeld worden gemarkeerd in de Update Management dat gevolgen voor de beveiliging, zelfs als de lokale computer beschikt niet over die informatie. Als gevolg hiervan, als u essentiële updates op een Linux-machine toepassen, zijn er updates die niet zijn gemarkeerd als gevolgen voor de beveiliging op machine en de updates worden niet toegepast.

Updatebeheer kunnen echter nog steeds melden die machine als niet-compatibele omdat er extra informatie over de betreffende update.

Implementeren van updates op updateclassificatie werkt niet op CentOS buiten het vak. Voor de implementatie goed updates voor CentOS, selecteert u alle classificaties om te controleren of updates worden toegepast. Voor SUSE, selecteren *alleen* 'Andere updates' als de classificatie tot extra beveiliging leiden kan werkt ook wordt geïnstalleerd als beveiligingsupdates die betrekking hebben op zypper (Pakketbeheer) of de afhankelijkheden ervan eerst vereist zijn. Dit gedrag is een beperking van zypper. In sommige gevallen mogelijk u gevraagd de update-implementatie opnieuw uit te voeren. Om te controleren, controleert u de update-logboek.

## <a name="remove-a-vm-for-update-management"></a>Verwijderen van een virtuele machine voor updatebeheer

Een virtuele machine verwijderen uit het beheer van de Update:

* In uw Log Analytics-werkruimte, verwijdert u de virtuele machine van de opgeslagen zoekopdracht voor de Bereikconfiguratie `MicrosoftDefaultScopeConfig-Updates`. Opgeslagen zoekopdrachten kunnen u vinden onder **algemene** in uw werkruimte.
* Verwijder de [Microsoft Monitoring agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) of de [Log Analytics-agent voor Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Volgende stappen

Doorgaan naar de zelfstudie voor informatie over het beheren van updates voor uw virtuele machines van Windows.

> [!div class="nextstepaction"]
> [Updates en patches voor uw Windows Azure-VM's beheren](automation-tutorial-update-management.md)

* Gebruik logboekzoekopdrachten in [logboeken van Azure Monitor](../log-analytics/log-analytics-log-searches.md) om gedetailleerde updategegevens weer te geven.
* [Waarschuwingen maken](automation-tutorial-update-management.md#configure-alerts) voor de status van de implementatie.

* Zie voor meer informatie over de communicatie met updatebeheer via de REST-API, [configuraties van Software-Update](/rest/api/automation/softwareupdateconfigurations)
* Zie voor informatie over het oplossen van uw updatebeheer, [updatebeheer oplossen van problemen](troubleshoot/update-management.md)
