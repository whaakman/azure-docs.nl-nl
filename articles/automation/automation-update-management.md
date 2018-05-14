---
title: Updatebeheer in Azure
description: Dit artikel is bedoeld om u te leren begrijpen hoe u deze oplossing kunt gebruiken voor het beheren van updates voor uw Windows- en Linux-computers.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/23/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: c095576ccce7e32850c3fb2daf8303a0d6e957bc
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="update-management-solution-in-azure"></a>Updatebeheer in Azure

De oplossing updatebeheer in Azure automation kunt u voor het beheren van updates voor uw Windows- en Linux-computers geïmplementeerd in Azure, on-premises omgevingen of andere cloudproviders besturingssysteem. U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en de procedure voor het installeren van vereiste updates voor servers beheren.

Updatebeheer voor virtuele machines kunt u rechtstreeks vanuit uw Azure Automation-account.
Zie [Updates voor meerdere virtuele machines in Azure beheren](manage-update-multi.md) voor informatie over het inschakelen van Updatebeheer voor virtuele machines vanaf uw Automation-account. U kunt ook updatebeheer inschakelen voor een enkele virtuele machine via de pagina van de virtuele machine in de Azure-portal. Dit scenario is beschikbaar voor zowel [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) en [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) virtuele machines.

## <a name="solution-overview"></a>Oplossingenoverzicht

Computers wordt beheerd door update management-gebruik de volgende configuraties voor het uitvoeren van implementaties beoordelen en bij te werken:

* Microsoft Monitoring agent voor Windows of Linux
* PowerShell Desired State Configuration (DSC) voor Linux
* Automation Hybrid Runbook Worker
* Microsoft Update of Windows Server Update Services voor Windows-computers

Het volgende diagram toont een conceptueel overzicht van het gedrag en de gegevensstroom van hoe de oplossing evalueert en beveiligingsupdates geldt voor alle verbonden Windows Server- en Linux-computers in een werkruimte.

![Processtroom management bijwerken](media/automation-update-management/update-mgmt-updateworkflow.png)

Nadat een computer een scan voor compatibiliteit van updates voert, verzendt de agent de informatie in bulk met logboekanalyse. Op Windows-computers wordt de nalevingsscan standaard elke twaalf uur uitgevoerd. Naast het schema voor scannen, wordt de scan voor compatibiliteit van updates binnen 15 minuten als Microsoft Monitoring Agent (MMA) opnieuw wordt opgestart voordat de installatie van update en na installatie van update gestart. Op Linux-computers wordt de nalevingsscan standaard elke drie uur uitgevoerd. Wanneer de MMA-agent opnieuw is opgestart, wordt er altijd binnen vijftien minuten een aanvullende nalevingsscan gestart.

De oplossing rapporteert hoe up-to-date de computer is op basis van de bron waarmee u in de huidige configuratie synchroniseert. Als de Windows-computer is geconfigureerd om te rapporteren aan WSUS, kunnen de resultaten afwijken van wat er wordt weergegeven in Microsoft Update, afhankelijk van wanneer WSUS voor het laatst is gesynchroniseerd met Microsoft Update. Dit is hetzelfde voor Linux-computers die zijn geconfigureerd voor rapportage aan een lokale opslagplaats ten opzichte van een openbare opslagplaats.

> [!NOTE]
> Updatebeheer is vereist op bepaalde URL's en poorten die worden ingeschakeld voor het correct rapporteren aan de service, Zie [Network planning voor Hybrid Workers](automation-hybrid-runbook-worker.md#network-planning) voor meer informatie over deze vereisten.

U kunt software-updates implementeren en installeren op computers die updates vereisen door daarvoor een planning in te stellen. Updates die zijn geclassificeerd als *optioneel*, worden niet opgenomen in het implementatiebereik voor Windows-computers. Alleen vereiste updates worden uitgevoerd. De geplande implementatie wordt gedefinieerd welke doelcomputers ontvangen de toepasselijke updates door het expliciet opgeven van computers of selecteren van een [computergroep](../log-analytics/log-analytics-computer-groups.md) die is gebaseerd op het logboek van zoekopdrachten van een bepaalde set van computers. U geeft ook een planning op voor het goedkeuren en toekennen van een bepaalde tijdsperiode waarin updates mogen worden geïnstalleerd. Updates worden geïnstalleerd door runbooks in Azure Automation. U kunt deze runbooks niet weergeven en ze vereisen geen configuratie. Wanneer een update-implementatie wordt gemaakt, wordt er een planning opgesteld waarmee een masterupdate-runbook voor de in de planning opgenomen computers start op het opgegeven tijdstip. Dit master-runbook start een onderliggend runbook op elke agent die installatie van de vereiste updates uitvoert.

Op de doelcomputers wordt gelijktijdig ook de implementatie uitgevoerd op de datum en tijd die zijn opgegeven in de update-implementatie. Er wordt eerst een scan uitgevoerd om te controleren of de updates nog steeds vereist zijn. Vervolgens worden ze geïnstalleerd. Voor de WSUS-clientcomputers als de updates niet worden goedgekeurd in WSUS, mislukt de implementatie-update.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Ondersteunde client-typen

De volgende tabel ziet u een lijst met ondersteunde besturingssystemen:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Ondersteunt alleen bijwerken beoordelingen         |
|Windows Server 2008 R2 SP1 en hoger     |Windows PowerShell 4.0 of hoger is vereist ([WMF 4.0 downloaden](https://www.microsoft.com/download/details.aspx?id=40855)).</br> Windows PowerShell 5.1 ([downloaden WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)) wordt aanbevolen voor een hogere mate van betrouwbaarheid.         |
|CentOS 6 (x86/x64) en 7 (x64)      | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Red Hat Enterprise 6 (x86/x64) en 7 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Ubuntu 12.04 TNS, 14.04 TNS, 16.04 LTS (x86/x64)      |Linux-agents moeten toegang hebben tot een opslagplaats voor updates.         |

### <a name="unsupported-client-types"></a>Niet-ondersteunde client-typen

De volgende tabel worden de besturingssystemen die worden niet ondersteund:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows-client     | Client-besturingssystemen (Windows 7, Windows 10, enzovoort) worden niet ondersteund.        |
|Windows Server 2016 Nano Server     | Niet ondersteund       |

### <a name="client-requirements"></a>Clientvereisten

#### <a name="windows"></a>Windows

Windows-agents moeten worden geconfigureerd om te communiceren met een Windows Server Update Services (WSUS)-server of toegang hebben tot Microsoft Update. Updatebeheer met System Center Configuration Manager kan worden gebruikt, voor meer informatie over de integratiescenario's gaat u naar [System Center Configuration Manager integreren met updatebeheer](oms-solution-updatemgmt-sccmintegration.md#configuration). De [Windows-agent](../log-analytics/log-analytics-agent-windows.md) is vereist. Deze agent wordt automatisch geïnstalleerd als u voorbereiden op een virtuele machine in Azure.

#### <a name="linux"></a>Linux

Voor Linux moet de machine naar een update-opslagplaats persoonlijke of openbare worden kan toegang hebben. Een OMS-Agent voor Linux geconfigureerd voor rapportage aan meerdere Log Analytics-werkruimten wordt niet ondersteund met deze oplossing.

Zie voor meer informatie over het installeren van de OMS-Agent voor Linux en download de nieuwste versie [Operations Management Suite-Agent voor Linux](https://github.com/microsoft/oms-agent-for-linux). Raadpleeg [Operations Management Suite-agent voor Windows](../log-analytics/log-analytics-windows-agent.md) voor meer informatie over het installeren van de OMS-agent voor Windows.

## <a name="permissions"></a>Machtigingen

Om te kunnen maken en beheren van de update-implementaties, moet u specifieke machtigingen. Voor meer informatie over deze machtigingen [rollen gebaseerd toegangsbeheer - updatebeheer](automation-role-based-access-control.md#update-management)

## <a name="solution-components"></a>Oplossingsonderdelen

Deze oplossing bestaat uit de volgende resources die zijn toegevoegd aan uw Automation-account en rechtstreeks verbonden agents of verbonden Operations Manager-beheergroepen.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-groepen

Nadat u deze oplossing hebt ingeschakeld, wordt elke Windows-computer die rechtstreeks zijn verbonden met uw werkruimte voor logboekanalyse automatisch geconfigureerd als een hybride Runbook Worker ter ondersteuning van de runbooks die zijn opgenomen in deze oplossing. Voor elke Windows-computer worden beheerd door de oplossing, wordt deze vermeld onder de pagina voor hybride worker groepen als een systeem hybrid worker-groep voor het Automation-account na de naamgevingsconventie *Hostname FQDN_GUID*. U kunt deze groepen met runbooks kan niet richt in uw account, anders die ze mislukken. Deze groepen zijn alleen bedoeld ter ondersteuning van de beheeroplossing.

U kunt de Windows-computers echter wel toevoegen aan een Hybrid Runbook Worker-groep in uw Automation-account voor ondersteuning van Automation-runbooks, mits u hetzelfde account gebruikt voor zowel de oplossing als het lidmaatschap van de Hybrid Runbook Worker-groep. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.

### <a name="management-packs"></a>Management packs

Als uw System Center Operations Manager-beheergroep met een werkruimte voor logboekanalyse is verbonden, worden de volgende management packs geïnstalleerd in Operations Manager. Deze management packs worden na het toevoegen van deze oplossing ook op rechtstreeks verbonden Windows-computers geïnstalleerd. Met deze management packs hoeft u niets te configureren of te beheren.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Implementatie MP bijwerken

Zie [Operations Manager koppelen aan Log Analytics](../log-analytics/log-analytics-om-agents.md) voor meer informatie over de manier waarop uw management packs voor oplossingen worden bijgewerkt.

### <a name="confirm-non-azure-machines-are-onboarded"></a>Niet-Azure-machines zijn vrijgegeven bevestigen

Om te bevestigen dat rechtstreeks verbonden machines communiceert Log Analytics na een paar minuten kunt u de volgende logboek zoekopdracht uitvoeren:

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table`
```

Op een Windows-computer, kunt u het volgende om te controleren of de verbinding van de agent met logboekanalyse bekijken:

1. Open Microsoft Monitoring Agent in het Configuratiescherm en op de **Azure Log Analytics** tabblad, de agent geeft een bericht weergegeven: **de Microsoft Monitoring Agent is verbonden met logboekanalyse** .
2. Open het Windows-gebeurtenislogboek, ga naar **Toepassings- en servicelogboeken\Operations Manager**, en zoek naar gebeurtenis-id 3000 en 5002 van de bronserviceconnector. Deze gebeurtenissen geven aan de computer met de werkruimte voor logboekanalyse is geregistreerd en configuratie ontvangt.

Als de agent kan niet communiceren met logboekanalyse en deze is geconfigureerd om te communiceren met internet via een firewall of proxyserver, controleert u de firewall of proxy-server correct is geconfigureerd aan de hand van [netwerkconfiguratie voor Windows-agent](../log-analytics/log-analytics-agent-windows.md) of [netwerkconfiguratie voor Linux-agent](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Als uw Linux-systemen zijn geconfigureerd om te communiceren met een proxy of Gateway OMS en u voorbereiden op deze oplossing, update de *proxy.conf* machtigingen voor het verlenen van de groep omiuser leesmachtigingen voor het bestand door het uitvoeren van de volgende opdrachten: `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Bij pas toegevoegde Linux-agents staat de status **Bijgewerkt** na het uitvoeren van een beoordeling. Dit proces kan maximaal zes uur duren.

Om te controleren op een Operations Manager-beheergroep communiceert met Log Analytics, Zie [Operations Manager-integratie valideren met logboekanalyse](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-log-analytics).

## <a name="data-collection"></a>Gegevensverzameling

### <a name="supported-agents"></a>Ondersteunde agents

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Windows-agents en start de installatie van de vereiste updates. |
| Linux-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Linux-agents en start de installatie van de vereiste updates op ondersteunde besturingssysteemversies. |
| Beheergroep Operations Manager |Ja |De oplossing verzamelt informatie over systeemupdates van agents in een verboden beheergroep.</br>Er is geen directe verbinding van de Operations Manager-agent naar Log Analytics vereist. Gegevens uit de beheergroep doorgestuurd naar de werkruimte voor logboekanalyse. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie

Voor elke beheerde Windows-computer wordt twee keer per dag een scan uitgevoerd. Om de vijftien minuten wordt de Windows-API aangeroepen voor de laatste updatetijd om op die manier te bepalen of de status is gewijzigd. Als dat het geval is, wordt er een nalevingsscan gestart. Voor elke beheerde Linux-computer wordt elke drie uur een scan uitgevoerd.

Het kan dertig minuten tot zes uur duren voordat er in het dashboard bijgewerkte gegevens worden weergegeven van de beheerder computers.

## <a name="viewing-update-assessments"></a>Update-evaluaties weergeven

Klik op de **updatebeheer** op uw automation-account om de status van uw apparaten weer te geven.

Deze weergave bevat informatie over uw machines ontbrekende updates, update-implementaties en geplande update-implementaties.

U kunt een zoekopdracht met logboekbestanden die informatie over de machine, update of implementatie geeft door het selecteren van het item in de lijst uitvoeren. Hiermee opent u de **logboek zoeken** pagina met een query voor het geselecteerde item.

## <a name="installing-updates"></a>Updates installeren

Wanneer de updates zijn beoordeeld voor alle Linux- en Windows-computers in uw werkruimte, kunt u de vereiste updates installeren door een *update-implementatie* te maken. Een Update-implementatie is een geplande installatie van vereiste updates voor een of meer computers. U geeft de datum en tijd op voor de implementatie, samen met een computer of een groep computers die moet worden opgenomen in het implementatiebereik. Zie [Computergroepen in Log Analytics](../log-analytics/log-analytics-computer-groups.md) voor meer informatie over computergroepen. Wanneer u computergroepen in de installatie van updates opneemt, wordt het lidmaatschap van slechts één keer geëvalueerd op het moment van schema maken. Toekomstige wijzigingen aan een groep worden niet weergegeven. U kunt dit probleem omzeilen door de geplande update-implementatie te verwijderen en deze opnieuw te maken.

> [!NOTE]
> Virtuele Windows-machines die via Azure Marketplace zijn geïmplementeerd, worden standaard ingesteld voor het automatisch ontvangen van updates van de Windows Update-service. Dit gedrag wordt niet gewijzigd na het toevoegen van deze oplossing of na het toevoegen van virtuele Windows-machines aan uw werkruimte. Als u niet actief updates met deze oplossing wordt het standaardgedrag beheert (automatisch updates van toepassing) van toepassing is.

Om te voorkomen dat updates buiten een onderhoudsperiode in Ubuntu worden toegepast, moet u het pakket Unattended-Upgrade opnieuw configureren en automatische updates uitschakelen. Zie [het onderwerp Automatic Updates in de Engelstalige Ubuntu Server Guide](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) voor meer informatie.

Voor virtuele machines die zijn gemaakt op basis van de on-demand RHEL-installatiekopieën (Red Hat Enterprise Linux) die beschikbaar zijn in Azure Marketplace, zijn ze geregistreerd voor toegang tot de [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) die is geïmplementeerd in Azure. Andere Linux-distributies moeten volgens de ondersteunde methoden worden bijgewerkt vanuit de online distro-bestandsopslagplaats.

## <a name="viewing-missing-updates"></a>Ontbrekende updates weergeven

Klik op **ontbrekende updates** om de lijst met updates die in uw machines ontbreken weer te geven. Elke update wordt weergegeven en toont informatie met betrekking tot het aantal machines waarvoor de update, het besturingssysteem en de koppeling voor meer informatie. Elke update kan worden geselecteerd en de **logboek zoeken** pagina wordt weergegeven en ziet u meer informatie over de updates.

## <a name="viewing-update-deployments"></a>Update-implementaties weergeven

Klik op de **Update-implementaties** tabblad om de lijst met bestaande Update-implementaties. Te klikken op een van de update-implementaties in de tabel wordt de **bijwerken implementatie uitgevoerd** pagina voor de implementatie van deze update.

![Overzicht van de resultaten van de update-implementatie](./media/automation-update-management/update-deployment-run.png)

## <a name="creating-an-update-deployment"></a>Een update-implementatie maken

Maak een nieuwe Update-implementatie door te klikken op de **schema-update-implementatie** knop aan de bovenkant van het scherm openen de **nieuwe implementatie-Update** pagina. U moet waarden opgeven voor de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
| --- | --- |
| Naam |Unieke naam voor het identificeren van de update-implementatie. |
|Besturingssysteem| Linux- of Windows|
| Machines bijwerken |Selecteer een opgeslagen zoekopdracht of Machine kiest uit de vervolgkeuzelijst en selecteert u afzonderlijke machines |
|Updateclassificaties|Selecteer de updateclassificaties die u nodig hebt|
|Updates die moeten worden uitgesloten|Voer alle KB om uit te sluiten zonder het voorvoegsel 'KB'|
|Planningsinstellingen|Selecteer de tijd om te starten en selecteer hetzij eenmaal of terugkerende voor het terugkeerpatroon|
| Onderhoudsvenster |Aantal minuten instellen voor updates. De waarde kan niet worden worden minder dan 30 minuten en niet meer dan 6 uur |

## <a name="update-classifications"></a>Updateclassificaties

De volgende tabellen bevatten een overzicht van de updateclassificaties in updatebeheer samen met een definitie voor elke classificatie.

### <a name="windows"></a>Windows

|Classificatie  |Beschrijving  |
|---------|---------|
|Essentiële updates     | Een update voor een specifiek probleem die zijn gericht op een kritieke fout niet gerelateerd.        |
|Beveiligingsupdates     | Een update voor een productspecifiek en beveiligingsgerelateerd probleem.        |
|Updatepakketten     | Een volledige reeks van hotfixes die samen zijn verpakt voor een gemakkelijke implementatie.        |
|Functiepakketten     | Nieuwe productfuncties die zijn gedistribueerd buiten een productrelease.        |
|Servicepacks     | Een volledige reeks van hotfixes die op een toepassing worden toegepast.        |
|Definitie-updates     | Een update voor antivirus- of andere definitiebestanden.        |
|Hulpprogramma's     | Een hulpprogramma of onderdeel dat u een of meer taken kunt voltooien.        |
|Updates     | Een update voor een toepassing of bestand dat momenteel is geïnstalleerd.        |

### <a name="linux"></a>Linux

|Classificatie  |Beschrijving  |
|---------|---------|
|Essentiële en beveiligingsupdates     | Updates voor een specifiek probleem of een productspecifiek en beveiligingsgerelateerd probleem.         |
|Andere Updates     | Alle andere updates die niet essentieel in aard of beveiligingsupdates.        |

## <a name="ports"></a>Poorten

De volgende adressen zijn vereist voor het beheer van updates. Communicatie met deze adressen wordt via poort 443 uitgevoerd.

|Openbare Azure  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

Voor meer informatie over de poorten die nodig zijn de hybride Runbook Worker, [Hybrid Worker-rol poorten](automation-hybrid-runbook-worker.md#hybrid-worker-role)

## <a name="search-logs"></a>Zoeken in Logboeken

Naast de informatie die beschikbaar zijn in de portal kunnen zoekopdrachten worden gedaan op basis van de logboeken. Met de **bijhouden** pagina openen, klikt u op **logboekanalyse**, Hiermee opent u de **logboek zoeken** pagina

### <a name="sample-queries"></a>Voorbeeldquery's

De volgende tabel bevat een voorbeeld-logboek zoekt bijwerkrecords die door deze oplossing worden verzameld:

| Query’s uitvoeren | Beschrijving |
| --- | --- |
|Update</br>&#124;waar UpdateState == 'Vereist' en een optionele == false</br>&#124;Computer, titel, KBID, classificatie, PublishedDate project |Alle computers met ontbrekende updates</br>Voeg een van de volgende opties om te beperken van het besturingssysteem:</br>Besturingssysteemtype = 'Windows'</br>Besturingssysteemtype == 'Linux' |
| Update</br>&#124;waar UpdateState == 'Vereist' en een optionele == false</br>&#124;waar Computer == "ContosoVM1.contoso.com"</br>&#124;Computer, titel, KBID, Product, PublishedDate project |Ontbrekende updates voor een specifieke computer (vervang de waarde door de naam van uw eigen computer)|
| Gebeurtenis</br>&#124;waar EventLevelName == "error" en elke Computer in ((Update &#124; waar (classificatie == 'Beveiligingsupdates' of classificatie == 'Essentiële Updates')</br>&#124;waar UpdateState == 'Vereist' en een optionele == false </br>&#124;afzonderlijke Computer)) |Foutgebeurtenissen voor machines met ontbrekende essentiële of beveiligingsupdates |
| Update</br>&#124;waar UpdateState == 'Vereist' en een optionele == false</br>&#124;afzonderlijke titel |Afzonderlijke ontbrekende updates op alle computers |
| UpdateRunProgress</br>&#124;waar InstallationStatus == "is mislukt" </br>&#124;overzicht van AggregatedValue count() door de Computer, de titel, UpdateRunName = |Computers met updates die niet in een update-uitvoering</br>Voeg een van de volgende opties om te beperken van het besturingssysteem:</br>Besturingssysteemtype = 'Windows'</br>Besturingssysteemtype == 'Linux' |
| Update</br>&#124;waar besturingssysteemtype == 'Linux'</br>&#124;waar UpdateState! = 'Niet nodig' en (classificatie == 'Essentiële Updates' of de indeling 'Beveiligingsupdates' ==)</br>&#124;overzicht van AggregatedValue = count() door Computer |Lijst met alle Linux machines, die een pakketupdate beschikbaar hebben, welke essentiële of beveiligingsupdates beveiligingslek adressen |
| UpdateRunProgress</br>&#124;waar UpdateRunName == "DeploymentName"</br>&#124;overzicht van AggregatedValue = count() door Computer|Computers die zijn bijgewerkt tijdens het uitvoeren van updates (vervang de waarde met de naam van uw update-implementatie) |

## <a name="integrate-with-system-center-configuration-manager"></a>Integreren met System Center Configuration Manager

Klanten die hebben geïnvesteerd in System Center Configuration Manager voor het beheren van pc's, servers en mobiele apparaten, rekenen ook op zijn kracht en volwassenheid bij het beheren van software-updates als onderdeel van hun software-updatebeheercyclus.

Zie voor meer informatie over de management-oplossing integreren met System Center Configuration Manager, [System Center Configuration Manager integreren met updatebeheer](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patching-linux-machines"></a>Linux-machines herstellen

De volgende secties worden de mogelijke problemen met het Linux-patches.

### <a name="package-exclusion"></a>Uitsluiting van pakket

Op sommige varianten Linux zoals Red Hat Enterprise Linux treedt OS-niveau upgrades via pakketten. Dit kan leiden tot beheer van updates wordt uitgevoerd wanneer het nummer van de OS-versie wordt gewijzigd. Omdat Management Update worden dezelfde methodes gebruikt pakketten bijwerken wanneer een beheerder zou lokaal op de Linux-computer, is dit gedrag is opzettelijk.

Om te voorkomen met het bijwerken van de versie van het besturingssysteem via beheer van updates wordt uitgevoerd, gebruikt u de **uitsluiting** functie.

In Red Hat Enterprise Linux, de naam van het pakket moeten worden uitgesloten zou zijn: redhat-release-server.x86_64

![Pakketten moeten worden uitgesloten voor Linux](./media/automation-update-management/linuxpatches.png)

### <a name="security-patches-not-being-applied"></a>Beveiligingspatches niet toegepast

Wanneer u updates implementeert naar een Linux-machine, kunt u de updateclassificaties selecteren. Dit filtert de updates die worden toegepast op computers die voldoen aan de opgegeven criteria. Dit filter is lokaal op de machine toegepast wanneer de update wordt geïmplementeerd. Omdat update verrijking Management bijwerken in de cloud uitvoert, kunnen sommige updates in de Update Management worden gemarkeerd als die van beveiliging invloed, hoewel de lokale computer geen die informatie heeft. Als gevolg hiervan, als u essentiële updates op een Linux-machine toepassen, mogelijk zijn er updates die niet zijn gemarkeerd als die beveiliging van invloed zijn op deze machine en kunnen niet worden toegepast. Management bijwerken kan echter nog steeds melden dat de machine als niet-compatibele omdat er aanvullende informatie over de desbetreffende update.

Implementeren van updates door updateclassificatie werkt mogelijk niet op openSUSE Linux vanwege de verschillende patch model dat wordt gebruikt.

## <a name="troubleshooting"></a>Problemen oplossen

Deze sectie bevat informatie over het oplossen van problemen met de oplossing Update Management.

Als u problemen ondervindt tijdens de onboarding van de oplossing of een virtuele machine, gaat u naar het gebeurtenislogboek **Toepassings- en servicelogboeken\Operations Manager** voor gebeurtenissen met gebeurtenis-id 4502 en het gebeurtenisbericht met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**. In de volgende tabel worden specifieke foutberichten weergegeven, plus een mogelijke oplossing voor elk van deze fouten.

| Bericht | Reden | Oplossing |
|----------|----------|----------|
| Kan de machine niet registreren voor patchbeheer,</br>Registratie is mislukt met uitzondering</br>System.InvalidOperationException: {"Message":"Machine is al</br>geregistreerd bij een ander account. "} | Machine is al vrijgegeven aan een andere werkruimte voor updatebeheer | Voer het opruimen van oude artefacten uit door [de hybride runbookgroep te verwijderen](automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|
| Kan geen Machine te registreren voor patchbeheer, de registratie is mislukt met uitzondering</br>System.Net.Http.HttpRequestException: Er is een fout opgetreden tijdens het verzenden van de aanvraag. ---></br>System.Net.WebException: De onderliggende verbinding</br>is gesloten: Er is een onverwachte fout</br>opgetreden tijdens het ontvangen. ---> System.ComponentModel.Win32Exception:</br>De client en server kunnen niet communiceren,</br>omdat ze geen gemeenschappelijk algoritme hebben | Communicatie wordt geblokkeerd door proxy/gateway/firewall | [Netwerkvereisten bekijken](automation-hybrid-runbook-worker.md#network-planning)|
| Kan de machine niet registreren voor patchbeheer,</br>Registratie is mislukt met uitzondering</br>Newtonsoft.Json.JsonReaderException: Fout tijdens het parseren van oneindig positieve waarde. | Communicatie wordt geblokkeerd door proxy/gateway/firewall | [Netwerkvereisten bekijken](automation-hybrid-runbook-worker.md#network-planning)|
| Het certificaat dat is doorgegeven door de service \<wsid\>. oms.opinsights.azure.com</br>is niet uitgegeven door een certificeringsinstantie</br>die wordt gebruikt voor Microsoft-services. Contact</br>de netwerkbeheerder om na te gaan of er een proxy wordt uitgevoerd waarmee</br>TLS/SSL-communicatie wordt onderschept. |Communicatie wordt geblokkeerd door proxy/gateway/firewall | [Netwerkvereisten bekijken](automation-hybrid-runbook-worker.md#network-planning)|
| Kan de machine niet registreren voor patchbeheer,</br>Registratie is mislukt met uitzondering</br>AgentService.HybridRegistration.</br>PowerShell.Certificates.CertificateCreationException:</br>Maken van een zelfondertekend certificaat is mislukt. ---></br>System.UnauthorizedAccessException: Toegang is geweigerd. | Genereren van zelfondertekend certificaat is mislukt | Controleer of het systeemaccount</br>leestoegang heeft tot de map:</br>**C:\ProgramData\Microsoft\**</br>** Crypto\RSA**|

## <a name="next-steps"></a>Volgende stappen

Blijven de zelfstudie voor informatie over het beheren van updates voor Windows-VM's.

> [!div class="nextstepaction"]
> [Updates en patches voor uw Windows Azure-VM's beheren](automation-tutorial-update-management.md)

* Gebruik Logboekzoekopdrachten in [Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde updategegevens weer te geven.
* [Maak waarschuwingen](../log-analytics/log-analytics-alerts.md) wanneer wordt vastgesteld dat er essentiële updates ontbreken op de computers of als automatische updates is uitgeschakeld voor een computer.
