---
title: Updatebeheer in Azure
description: In dit artikel is bedoeld om te begrijpen hoe de oplossing voor beheer van Azure-updates gebruiken voor het beheren van updates voor uw Windows- en Linux-computers.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 06/28/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e1bcae85c7078d817e30ec578ac12b2be13342c7
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129020"
---
# <a name="update-management-solution-in-azure"></a>Updatebeheer in Azure

U kunt de oplossing voor beheer van de Update in Azure Automation besturingssysteem om updates te beheren voor uw Windows- en Linux-computers die zijn geïmplementeerd in Azure, on-premises omgevingen of andere cloudproviders gebruiken. U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en de procedure voor het installeren van vereiste updates voor servers beheren.

U kunt updatebeheer inschakelen voor virtuele machines rechtstreeks vanuit uw Azure Automation-account. Zie voor meer informatie over hoe u updatebeheer inschakelt voor virtuele machines van uw Automation-account, [beheren van updates voor meerdere virtuele machines](manage-update-multi.md). U kunt ook updatebeheer inschakelen voor een enkele virtuele machine in het deelvenster van de virtuele machine in de Azure portal. Dit scenario is beschikbaar voor [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) en [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) virtuele machines.

## <a name="solution-overview"></a>Oplossingenoverzicht

Computers die beheerd worden door Management Update de volgende configuraties gebruiken dat u uitvoert en de update-implementaties:

* Microsoft Monitoring Agent (MMA) voor Windows of Linux
* PowerShell Desired State Configuration (DSC) voor Linux
* Automation Hybrid Runbook Worker
* Microsoft Update of Windows Server Update Services (WSUS) voor Windows-computers

Het volgende diagram toont een conceptueel overzicht van het gedrag en de gegevensstroom van hoe de oplossing evalueert en beveiligingsupdates geldt voor alle Windows Server- en Linux-computers in een werkruimte verbonden:

![Processtroom Management bijwerken](media/automation-update-management/update-mgmt-updateworkflow.png)

Nadat een computer een scan voor compatibiliteit van updates voert, verzendt de agent de informatie in bulk naar Azure-logboekanalyse. Op een Windows-computer, wordt de scan voor naleving standaard elke 12 uur uitgevoerd.

Naast het schema voor scannen, wordt de scan voor compatibiliteit van updates binnen 15 minuten als de MMA opnieuw wordt opgestart voordat de installatie van update en na installatie van update gestart.

Voor een Linux-computer, wordt de scan voor naleving standaard elke drie uur uitgevoerd. Als de agent MMA opnieuw wordt opgestart, wordt een scan voor naleving binnen 15 minuten gestart.

De oplossing rapporten hoe up-to-date de computer is gebaseerd op wat u de gegevensbron zijn geconfigureerd om te synchroniseren met. Als de Windows-computer is geconfigureerd om te rapporteren aan WSUS, afhankelijk van wanneer WSUS laatst gesynchroniseerd met Microsoft Update, wordt de resultaten afwijken van wat Microsoft-Updates wordt weergegeven. Dit is hetzelfde voor Linux-computers die zijn geconfigureerd voor rapportage aan een lokale opslagplaats in plaats van naar een openbare opslagplaats.

> [!NOTE]
> Updatebeheer vereist voor het correct rapporteren aan de service, bepaalde URL's en poorten die worden ingeschakeld. Zie voor meer informatie over deze vereisten, [Network planning voor Hybrid Workers](automation-hybrid-runbook-worker.md#network-planning).

U kunt software-updates implementeren en installeren op computers die updates vereisen door daarvoor een planning in te stellen. Updates die zijn geclassificeerd als *optioneel* niet zijn opgenomen in het bereik van de implementatie voor Windows-computers. Alleen de vereiste updates zijn opgenomen in het bereik van de implementatie. 

De geplande implementatie wordt gedefinieerd welke doelcomputers de toepasselijke updates ontvangen door computers expliciet op te geven of door het selecteren van een [computergroep](../log-analytics/log-analytics-computer-groups.md) dat gebaseerd op het logboek van zoekopdrachten van een specifieke set computers. U wordt ook een planning voor het goedkeuren en toewijzen van een bepaalde periode gedurende welke updates kunnen worden geïnstalleerd. 

Updates worden geïnstalleerd door runbooks in Azure Automation. U kunt deze runbooks niet weergeven en de runbooks geen configuratie vereist. Wanneer een update-implementatie wordt gemaakt, maakt de update-implementatie in een schema dat wordt een runbook master update op de opgegeven tijd voor de computers opgenomen gestart. Het master runbook wordt gestart van een onderliggend runbook voor elke agent voor installatie van vereiste updates uitvoeren.

Op de datum en tijd die is opgegeven in de update-implementatie, uitvoeren de doelcomputers de implementatie parallel. Een scan wordt uitgevoerd vóór de installatie om te controleren of de updates nog steeds vereist. Voor WSUS-clientcomputers als de updates niet worden goedgekeurd in WSUS, mislukt de implementatie-update.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Ondersteunde client-typen

De volgende tabel ziet u een lijst met ondersteunde besturingssystemen:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Ondersteunt alleen bijwerken beoordelingen.         |
|Windows Server 2008 R2 SP1 en hoger     |.NET framework 4.5 of hoger is vereist. ([.NET Framework downloaden](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 of hoger is vereist. ([WMF 4.0 downloaden](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1 wordt aanbevolen voor een hogere mate van betrouwbaarheid.  ([Downloaden WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) en 7 (x64)      | Linux-agents moeten toegang hebben tot een opslagplaats voor updates. Op basis van classificatie patchen vereist 'yum' van beveiligingsgegevens die CentOS heeft geen gebruiksklaar te retourneren.         |
|Red Hat Enterprise 6 (x86/x64) en 7 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Ubuntu 14.04 TNS en 16.04 LTS (x86/x64)      |Linux-agents moeten toegang hebben tot een opslagplaats voor updates.         |

### <a name="unsupported-client-types"></a>Niet-ondersteunde client-typen

De volgende tabel geeft een overzicht van de besturingssystemen die niet worden ondersteund:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows-client     | Client-besturingssystemen (zoals Windows 7 en Windows 10) worden niet ondersteund.        |
|Windows Server 2016 Nano Server     | Wordt niet ondersteund.       |

### <a name="client-requirements"></a>Clientvereisten

#### <a name="windows"></a>Windows

Windows-agents moeten worden geconfigureerd voor communicatie met WSUS-server of ze moeten toegang hebben tot Microsoft Update. U kunt met System Center Configuration Manager-updatebeheer. Zie voor meer informatie over scenario's voor integratie, [System Center Configuration Manager integreren met updatebeheer](oms-solution-updatemgmt-sccmintegration.md#configuration). De [Windows-agent](../log-analytics/log-analytics-agent-windows.md) is vereist. De agent wordt automatisch geïnstalleerd als u voorbereiden op Azure een virtuele machine.

#### <a name="linux"></a>Linux

Voor Linux moet de computer toegang hebben tot een update-opslagplaats. De opslagplaats update kan worden persoonlijke of openbare. TLS 1.1 of TLS 1.2 is vereist om te communiceren met updatebeheer. Een Agent Operations Management Suite (OMS) voor Linux dat geconfigureerd voor rapportage aan meerdere Log Analytics-werkruimten wordt niet ondersteund met deze oplossing.

Zie voor meer informatie over het installeren van de OMS-Agent voor Linux en download de nieuwste versie [Operations Management Suite-Agent voor Linux](https://github.com/microsoft/oms-agent-for-linux). Zie voor meer informatie over het installeren van de OMS-Agent voor Windows [Operations Management Suite-Agent voor Windows](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>Machtigingen

Als u wilt maken en beheren van de update-implementaties, moet u specifieke machtigingen. Zie voor meer informatie over deze machtigingen [op rollen gebaseerde toegang - updatebeheer](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Oplossingsonderdelen

De oplossing bestaat uit de volgende bronnen. De resources worden toegevoegd aan uw Automation-account. Ze zijn beide rechtstreeks verbonden zijn met agents of in een Operations Manager verbonden beheergroep.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-groepen

Nadat u deze oplossing hebt ingeschakeld, wordt elke Windows-computer die rechtstreeks verbonden met uw werkruimte voor logboekanalyse automatisch geconfigureerd als een hybride Runbook Worker ter ondersteuning van de runbooks die zijn opgenomen in deze oplossing.

Elke computer met Windows die wordt beheerd door de oplossing wordt vermeld in de **Hybrid worker-groepen** deelvenster als een **System hybrid worker-groep** voor het Automation-account. De oplossingen gebruiken de naamgevingsconventie *Hostname FQDN_GUID*. U kunt deze groepen met runbooks kan niet gericht in uw account. Deze mislukken als u probeert. Deze groepen zijn bedoeld ter ondersteuning van alleen de oplossing voor beheer.

U kunt de Windows-computers toevoegen aan een hybride Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks als u hetzelfde account voor de oplossing en het lidmaatschap van de Hybrid Runbook Worker gebruiken. Deze functionaliteit is toegevoegd in versie 7.2.12024.0 van de hybride Runbook Worker.

### <a name="management-packs"></a>Management packs

Als uw System Center Operations Manager-beheergroep met een werkruimte voor logboekanalyse is verbonden, worden de volgende management packs geïnstalleerd in Operations Manager. Deze management packs zijn ook op rechtstreeks verbonden zijn met Windows-computers geïnstalleerd als u de oplossing toevoegen. U hoeft niet te configureren of beheren van deze management packs.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Implementatie MP bijwerken

Zie voor meer informatie over hoe de oplossing management packs worden bijgewerkt, [Operations Manager verbinden met Log Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Controleer of de niet-Azure-machines vrijgegeven zijn

Om te bevestigen dat rechtstreeks verbonden machines Log Analytics na een paar minuten communiceert, kunt u een de volgende logboek zoekacties uitvoeren.

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Op een Windows-computer, kunt u de volgende informatie om te controleren of de agent-connectiviteit met logboekanalyse bekijken:

1. Open in het Configuratiescherm **Microsoft Monitoring Agent**. Op de **Azure Log Analytics** tabblad en de agent wordt het volgende bericht weergegeven: **de Microsoft Monitoring Agent is verbonden met logboekanalyse**.
2. Open het Windows-gebeurtenislogboek. Ga naar **toepassingen en Services Logs\Operations Manager** en zoek naar gebeurtenis-ID 3000 en gebeurtenis-ID 5002 van de bron **serviceconnector**. Deze gebeurtenissen geven aan dat de computer is geregistreerd bij de werkruimte voor logboekanalyse en configuratie ontvangt.

Als de agent kan niet met logboekanalyse communiceren en de agent is geconfigureerd voor communicatie met internet via een firewall of proxyserver, bevestigt u dat de firewall of proxy-server juist is geconfigureerd. Zie voor informatie over het controleren of de firewall of proxy-server correct is geconfigureerd, [netwerkconfiguratie voor Windows-agent](../log-analytics/log-analytics-agent-windows.md) of [netwerkconfiguratie voor Linux-agent](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Als uw Linux-systemen zijn geconfigureerd om te communiceren met een proxy of Gateway OMS en u onboarding bent deze oplossing, update de *proxy.conf* machtigingen voor het verlenen van de groep omiuser leesmachtigingen voor het bestand met behulp van de volgende opdrachten:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Toegevoegde Linux-agents weergeven status **bijgewerkt** nadat een beoordeling is uitgevoerd. Dit proces kan maximaal zes uur duren.

Om te bevestigen dat een Operations Manager-beheergroep met Log Analytics communiceert, Zie [valideren Operations Manager-integration met logboekanalyse](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-log-analytics).

## <a name="data-collection"></a>Gegevensverzameling

### <a name="supported-agents"></a>Ondersteunde agents

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing:

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Windows-agents en start vervolgens de installatie van vereiste updates. |
| Linux-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Linux-agents en start vervolgens de installatie van vereiste updates op ondersteunde distributies. |
| Beheergroep Operations Manager |Ja |De oplossing verzamelt informatie over systeemupdates van agents in een verboden beheergroep.<br/>Er is een rechtstreekse verbinding tussen de Operations Manager-agent met logboekanalyse niet vereist. Gegevens uit de beheergroep doorgestuurd naar de werkruimte voor logboekanalyse. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie

Een scan wordt uitgevoerd op twee keer per dag voor elke beheerde Windows-computer. Elke 15 minuten, de Windows-API wordt aangeroepen voor de query voor het laatst update om te bepalen of de status is gewijzigd. Als de status is gewijzigd, wordt een scan voor naleving gestart. 

Een scan wordt elke drie uur uitgevoerd voor elke Linux-computer beheerde.

Het kost tussen 30 minuten en 6 uur voor het dashboard bijgewerkte gegevens van beheerde computers wilt weergeven.

## <a name="viewing-update-assessments"></a>Beoordelingen van weergave-update

Selecteer in uw Automation-account **updatebeheer** om de status van uw apparaten weer te geven.

Deze weergave bevat gegevens over uw computers, ontbrekende updates, update-implementaties en geplande update-implementaties. In de **naleving kolom**, ziet u de laatste keer dat de machine is beoordeeld. In de **gereedheid van de AGENT bijwerken** kolom, kunt u zien als de status van de update-agent. Als er een probleem is, selecteert u de koppeling naar de documentatie die kan helpen bij het oplossen van problemen dat u meer te weten welke stappen nemen om het probleem te verhelpen.

Als u wilt uitvoeren in een logboek zoekquery waarmee informatie over de machine, update of implementatie, selecteert u het item in de lijst. De **logboek zoeken** deelvenster wordt geopend met een query voor het geselecteerde item:

![De standaardweergave Management bijwerken](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Updates installeren

Nadat de updates worden beoordeeld op alle Linux- en Windows-computers in uw werkruimte, kunt u de vereiste updates installeren door het maken van een *update-implementatie*. Een update-implementatie is een geplande installatie van vereiste updates voor een of meer computers. U geeft de datum en tijd voor de implementatie en een computer of groep computers die u wilt opnemen in het bereik van een implementatie. Zie [Computergroepen in Log Analytics](../log-analytics/log-analytics-computer-groups.md) voor meer informatie over computergroepen.

 Wanneer u computergroepen in de installatie van updates opneemt, wordt het lidmaatschap van slechts één keer geëvalueerd op het moment van schema maken. Wijzigingen aan een groep niet worden gereflecteerd. U kunt dit probleem omzeilen implementatie van de geplande update verwijderen en opnieuw maken.

> [!NOTE]
> Windows virtuele machines die zijn geïmplementeerd vanuit Azure Marketplace standaard ingesteld op automatische updates ontvangen van Windows Update-Service. Dit gedrag verandert niet wanneer u deze oplossing toevoegt of Windows virtuele machines aan uw werkruimte toevoegen. Als u geen actief updates beheren met behulp van deze oplossing, geldt het standaardgedrag (automatisch updates wilt toepassen).

Configureren om te voorkomen updates buiten een onderhoudsvenster op Ubuntu wordt toegepast, het Unattended-Upgrade-pakket voor automatische updates niet uitschakelen. Zie voor meer informatie over het configureren van het pakket [Automatische Updates-onderwerp in de handleiding Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Virtuele machines die zijn gemaakt op basis van installatiekopieën van het Red Hat Enterprise Linux (RHEL) op aanvraag die beschikbaar in Azure Marketplace zijn zijn geregistreerd voor toegang tot de [Red Hat Update-infrastructuur (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) die wordt geïmplementeerd in Azure. Alle Linux-distributie moet worden bijgewerkt vanuit de distributie bestanden online opslagplaats door de volgende ondersteunde methoden voor het distributiepunt.

## <a name="view-missing-updates"></a>Ontbrekende updates weergeven

Selecteer **ontbrekende updates** om de lijst met updates die in uw machines ontbreken weer te geven. Elke update wordt vermeld en kan worden geselecteerd. Informatie over het aantal machines waarvoor de update, het besturingssysteem en een koppeling voor meer informatie wordt weergegeven. De **logboek zoeken** deelvenster bevat meer informatie over de updates.

## <a name="view-update-deployments"></a>Weergave-update-implementaties

Selecteer de **implementaties bijwerken** tabblad om de lijst met bestaande update-implementaties. Selecteer een van de update-implementaties in de tabel openen de **bijwerken implementatie uitgevoerd** deelvenster voor die update-implementatie.

![Overzicht van de resultaten van de update-implementatie](./media/automation-update-management/update-deployment-run.png)

## <a name="create-or-edit-an-update-deployment"></a>Maken of bewerken van een update-implementatie

Voor het maken van een nieuwe update-implementatie selecteert **schema-update-implementatie**. De **nieuwe Update-implementatie** deelvenster wordt geopend. Geef waarden voor de eigenschappen die in de volgende tabel worden beschreven:

| Eigenschap | Beschrijving |
| --- | --- |
|Naam |Unieke naam voor het identificeren van de update-implementatie. |
|Besturingssysteem| Selecteer **Linux** of **Windows**.|
|Machines bijwerken |Selecteer een opgeslagen zoekopdracht of selecteer **Machine** uit de vervolgkeuzelijst en selecteer vervolgens afzonderlijke computers. |
|Updateclassificaties|Selecteer de updateclassificaties die u nodig hebt. CentOS ondersteunt dit niet uit het vak.|
|Updates die moeten worden uitgesloten|Voer de updates om uit te sluiten. Voor Windows, voert u het KB-artikel zonder de **KB** voorvoegsel. Voer de naam van het pakket voor Linux of een jokerteken gebruiken.  |
|Planningsinstellingen|Selecteer de tijd om te starten en selecteer vervolgens **eenmaal** of **terugkerend** voor het terugkeerpatroon.|| Onderhoudsvenster |Aantal minuten instellen voor updates. De waarde mag niet minder dan 30 minuten of langer dan 6 uur. |

## <a name="update-classifications"></a>Updateclassificaties

De volgende tabellen worden de updateclassificaties in het beheer van bijwerken met een definitie voor elke classificatie.

### <a name="windows"></a>Windows

|Classificatie  |Beschrijving  |
|---------|---------|
|Essentiële updates     | Een update voor een specifiek probleem die zijn gericht op een kritieke fout niet gerelateerd.        |
|Beveiligingsupdates     | Een update voor een productspecifiek en beveiligingsgerelateerd probleem.        |
|Updatepakketten     | Een volledige reeks van hotfixes die samen zijn verpakt voor een gemakkelijke implementatie.        |
|Functiepakketten     | Nieuwe productfuncties die zijn gedistribueerd buiten een productrelease.        |
|Servicepacks     | Een volledige reeks van hotfixes die op een toepassing worden toegepast.        |
|Definitie-updates     | Een update voor antivirus- of andere definitiebestanden.        |
|Hulpprogramma's     | Een hulpprogramma of onderdeel aan waarmee een of meer taken uitvoeren.        |
|Updates     | Een update aan voor een toepassing of bestand dat momenteel is geïnstalleerd.        |

### <a name="linux"></a>Linux

|Classificatie  |Beschrijving  |
|---------|---------|
|Essentiële en beveiligingsupdates     | Updates voor een specifiek probleem of een productspecifiek en beveiligingsgerelateerd probleem.         |
|Andere Updates     | Alle overige updates die niet essentieel zijn in aard of beveiligingsupdates niet.        |

Voor Linux-updatebeheer kunnen worden onderscheiden essentiële updates en beveiligingsupdates in de cloud terwijl beoordelingsgegevens vanwege gegevens verrijking wordt weergegeven in de cloud. Voor patch-doeleinden, updatebeheer gebruikgemaakt van classificatie gegevens beschikbaar op de computer. In tegenstelling tot andere distributies CentOS heeft geen deze informatie beschikbaar is buiten het vak. Als er CentOS-machines die zijn geconfigureerd op een manier om beveiligingsgegevens voor de volgende opdracht te retourneren, zich updatebeheer bij patch op basis van classificaties.

```bash
sudo yum -q --security check-update
```

Er is geen methode methode ondersteund voor het inschakelen van de beschikbaarheid van de systeemeigen classificatie-gegevens op CentOS. Op dit moment worden alleen best-effort-ondersteuning is beschikbaar voor klanten aan wie u mogelijk hebt ingeschakeld dit op hun eigen.

## <a name="ports"></a>Poorten

De volgende adressen zijn vereist voor het beheer van updates. -Communicatie met deze adressen vindt plaats via poort 443.

|Openbare Azure  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

Zie voor meer informatie over de poorten die vereist dat de hybride Runbook Worker [Hybrid Worker-rol poorten](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Het is raadzaam om de adressen die worden vermeld bij het definiëren van uitzonderingen te gebruiken. Voor IP-adressen die u downloaden via kunt de [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Dit bestand wordt wekelijks bijgewerkt en weerspiegelt de huidige geïmplementeerde bereiken en toekomstige wijzigingen in de IP-adresbereiken.

## <a name="search-logs"></a>Zoeken in Logboeken

Naast de details die beschikbaar zijn in de Azure-portal, kunt u de logboeken zoekacties doen. Selecteer op de oplossing's, **logboekanalyse**. De **logboek zoeken** deelvenster wordt geopend.

U kunt ook informatie over het aanpassen van de query of ze gebruiken van andere clients en meer in via: [Zoek API-documentatie voor logboekanalyse](
https://dev.loganalytics.io/).

### <a name="sample-queries"></a>Voorbeeldquery's

De volgende secties bieden voorbeeldquery logboek's voor bijwerkrecords die door deze oplossing worden verzameld:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Eén Azure VM Assessment query's (Windows)

Vervang de waarde VMUUID met de VM-GUID van de virtuele machine die u wilt zoeken. U vindt de VMUUID die moet worden gebruikt door de volgende query in logboekanalyse: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Ontbrekende updates samenvatting

```
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="missing-updates-list"></a>Lijst met ontbrekende updates

```
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

#### <a name="single-azure-vm-assessment-queries-linux"></a>Eén virtuele machine van Azure assessment query's (Linux)

Voor sommige Linux-distributies er is een [endianness](https://en.wikipedia.org/wiki/Endianness) komt niet overeen met de waarde VMUUID die afkomstig zijn van Azure Resource Manager en wat wordt opgeslagen in logboekanalyse. De volgende query controleert of er een overeenkomst op beide endianness. Vervang de waarden VMUUID met de big endian en little endian-indeling van de GUID voor de resultaten te retourneren. U vindt de VMUUID die moet worden gebruikt door de volgende query in logboekanalyse: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Ontbrekende updates samenvatting

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Lijst met ontbrekende updates

```
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

#### <a name="multi-vm-assessment-queries"></a>Multi-VM assessment query 's

##### <a name="computers-summary"></a>Samenvatting van computers

```
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
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeeedOtherUpdatesCount=sum(computersNeeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Ontbrekende updates samenvatting

```
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

```
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

```
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

## <a name="integrate-with-system-center-configuration-manager"></a>Integreren met System Center Configuration Manager

Klanten die in System Center Configuration Manager voor het beheren van pc's, servers en mobiele apparaten hebben geïnvesteerd is ook afhankelijk van de sterkte en de vervaldatum van Configuration Manager om te software-updates beheren. Configuration Manager maakt deel uit van de software update management (totaal)-cyclus.

Zie voor meer informatie over de management-oplossing integreren met System Center Configuration Manager, [System Center Configuration Manager integreren met updatebeheer](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patch-linux-machines"></a>Patch voor Linux-machines

De volgende secties worden de mogelijke problemen met het Linux-patches.

### <a name="unexpected-os-level-upgrades"></a>Onverwachte OS-niveau wordt bijgewerkt

OS-niveau upgrades optreden op sommige varianten Linux zoals Red Hat Enterprise Linux via pakketten. Dit kan leiden tot beheer van updates wordt uitgevoerd wanneer het nummer van de OS-versie wordt gewijzigd. Omdat Management Update worden dezelfde methodes gebruikt om bij te werken van pakketten die een beheerder lokaal op de Linux-computer wilt gebruiken, is dit gedrag is opzettelijk.

Om te voorkomen met het bijwerken van de versie van het besturingssysteem via beheer van updates wordt uitgevoerd, gebruikt de **uitsluiting** functie.

In Red Hat Enterprise Linux is de naam van het pakket moeten worden uitgesloten redhat-release-server.x86_64.

![Pakketten moeten worden uitgesloten voor Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Kritieke / beveiligingspatches worden niet toegepast

Wanneer u updates naar een Linux-machine implementeert, kunt u de updateclassificaties selecteren. Dit filtert de updates die worden toegepast op computers die voldoen aan de opgegeven criteria. Dit filter is lokaal op de machine toegepast wanneer de update wordt geïmplementeerd.

Omdat update verrijking Management bijwerken in de cloud uitvoert, sommige updates mogelijk worden gemarkeerd in beheer bijwerken als bestanden met impact van de beveiliging, zelfs als de lokale computer geen die informatie. Als gevolg hiervan, als u essentiële updates op een Linux-machine toepassen, kan er updates die niet zijn gemarkeerd als die van beveiliging van invloed op machine en de updates worden niet toegepast.

Update Management mogelijk nog steeds rapporteert echter dat de machine als niet-compatibele omdat er aanvullende informatie over de desbetreffende update.

Implementeren van updates door updateclassificatie werkt niet op CentOS gebruiksklaar. Voor SUSE, selecteren *alleen* 'andere updates, zoals de classificatie tot enige vorm van beveiliging leiden kan updates ook worden geïnstalleerd als beveiligingsupdates met betrekking tot zypper (Pakketbeheer) of de afhankelijkheden ervan eerst vereist zijn. Dit is een beperking van zypper. In sommige gevallen kunt u opnieuw uitvoeren om te controleren of de update-implementatie vereist Raadpleeg het logboek voor update.

## <a name="troubleshoot"></a>Problemen oplossen

Zie voor meer informatie over het oplossen van uw updatebeheer, [updatebeheer probleemoplossing](troubleshoot/update-management.md)

## <a name="next-steps"></a>Volgende stappen

Blijven de zelfstudie voor informatie over het beheren van updates voor uw virtuele machines van Windows.

> [!div class="nextstepaction"]
> [Updates en patches voor uw Windows Azure-VM's beheren](automation-tutorial-update-management.md)

* Logboek van zoekopdrachten in gebruiken [logboekanalyse](../log-analytics/log-analytics-log-searches.md) om gedetailleerde updategegevens weer te geven.
* [Waarschuwingen maken](../log-analytics/log-analytics-alerts.md) wanneer essentiële updates worden gedetecteerd als ontbrekend van computers of als een computer heeft de automatische updates uitgeschakeld.
