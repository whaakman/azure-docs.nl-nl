---
title: De oplossing voor updatebeheer in OMS | Microsoft Docs
description: Dit artikel is bedoeld om u te leren begrijpen hoe u deze oplossing kunt gebruiken voor het beheren van updates voor uw Windows- en Linux-computers.
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/20/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: b0624bde9ab53231768beb0c832a4a49b21a8975
ms.lasthandoff: 04/20/2017


---
# <a name="update-management-solution-in-oms"></a>De oplossing voor updatebeheer in OMS
Met de oplossing voor updatebeheer in OMS kunt u updates voor uw Windows- en Linux-computers beheren.  U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en de procedure voor het installeren van vereiste updates voor servers te starten. 

## <a name="solution-components"></a>Oplossingsonderdelen

Op computers die worden beheerd met OMS, wordt gebruikgemaakt van de volgende zaken voor het uitvoeren van controles en het bijwerken van implementaties: 

* OMS-agent voor Windows of Linux
* PowerShell Desired State Configuration (DSC) voor Linux 
* Automation Hybrid Runbook Worker 
* Microsoft Update of Windows Server Update Services voor Windows-computers

In de volgende diagrammen ziet u een conceptueel overzicht van het gedrag en de gegevensstroom op basis waarvan de oplossing controleert en updates uitvoert voor alle Windows Server- en Linux-computers in een werkruimte.    

#### <a name="windows-server"></a>Windows Server
![Processtroom voor het beheren van Windows Server-updates](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Processtroom voor het beheren van Linux-updates](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

Wanneer op de computer is onderzocht of wordt voldaan aan de updatevereisten, stuurt de OMS-agent de informatie in bulk door naar OMS. Op Windows-computers wordt de nalevingsscan standaard elke twaalf uur uitgevoerd.  Naast de reguliere ingeplande scans wordt de updatenalevingsscan altijd binnen vijftien minuten gestart nadat de Microsoft Monitoring Agent (MMA) opnieuw is opgestart. De scan wordt ook vóór het installeren van updates en ná het installeren van updates uitgevoerd.  Op Linux-computers wordt de nalevingsscan standaard elke drie uur uitgevoerd. Wanneer de MMA-agent opnieuw is opgestart, wordt er altijd binnen vijftien minuten een aanvullende nalevingsscan gestart.  

De nalevingsinformatie wordt dan verwerkt en u krijgt er een overzicht van te zien in de dashboards van de oplossing. U kunt de informatie ook doorzoeken met door de gebruiker gedefinieerde of vooraf gedefinieerde query's.  De oplossing rapporteert hoe up-to-date de computer is op basis van de bron waarmee u in de huidige configuratie synchroniseert.  Als de Windows-computer is geconfigureerd om te rapporteren aan WSUS, kunnen de resultaten afwijken van wat er wordt weergegeven in Microsoft Update, afhankelijk van wanneer WSUS voor het laatst is gesynchroniseerd met Microsoft Update.  Ditzelfde geldt voor Linux-computers die zijn geconfigureerd voor rapportage aan een lokale in plaats van een openbare opslagplaats.   

U kunt software-updates implementeren en installeren op computers die updates vereisen door daarvoor een planning in te stellen.  Updates die zijn geclassificeerd als *optioneel*, worden niet opgenomen in het implementatiebereik voor Windows-computers. Alleen vereiste updates worden uitgevoerd.  In de implementatieplanning staat op welke doelcomputers de updates worden geïmplementeerd, ofwel door specifieke computers op te geven of door een [computergroep](../log-analytics/log-analytics-computer-groups.md) te selecteren die is gebaseerd op logboekzoekopdrachten binnen een bepaalde set computers.  U geeft ook een planning op voor het goedkeuren en toekennen van een bepaalde tijdsperiode waarin updates mogen worden geïnstalleerd.  Updates worden geïnstalleerd door runbooks in Azure Automation.  U kunt deze runbooks niet weergeven en ze vereisen geen configuratie.  Wanneer een update-implementatie wordt gemaakt, wordt er een planning opgesteld waarmee een masterupdate-runbook voor de in de planning opgenomen computers start op het opgegeven tijdstip.  Dit master-runbook start een onderliggend runbook op elke agent die installatie van de vereiste updates uitvoert.       

Op de doelcomputers wordt gelijktijdig ook de implementatie uitgevoerd op de datum en tijd die zijn opgegeven in de update-implementatie.  Er wordt eerst een scan uitgevoerd om te controleren of de updates nog steeds vereist zijn. Vervolgens worden ze geïnstalleerd.  Houd voor ogen dat de update-implementatie voor WSUS-clientcomputers mislukt als de updates niet zijn goedgekeurd in WSUS.  De resultaten van de toegepaste updates worden doorgestuurd naar OMS. Hier worden ze verwerkt en vervolgens wordt er een overzicht van weergegeven in de dashboards. U kunt er ook meer informatie over raadplegen door de gebeurtenissen te doorzoeken.     

## <a name="prerequisites"></a>Vereisten
* De oplossing biedt ondersteuning voor updatebeoordelingen op Windows Server 2008 en hoger en update-implementaties op Windows Server 2012 en hoger.  Server Core- en nanoserverinstallatieopties worden niet ondersteund.
* Windows-clientbesturingssystemen worden niet ondersteund.  
* Windows-agents moeten worden geconfigureerd om te communiceren met een WSUS-server (Windows Server Update Services of toegang hebben tot Microsoft Update.  
  
    > [!NOTE]
    > De Windows-agent kan niet tegelijkertijd door System Center Configuration Manager worden beheerd.  
    >
* CentOS 6 (x86/x64) en 7 (x64)
* Red Hat Enterprise 6 (x86/x64) en 7 (x64)
* SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)
* Ubuntu 12.04 LTS en nieuwer x86/x64  
* Linux-agents moeten toegang hebben tot een opslagplaats voor updates.  

    > [!NOTE]
    > OMS-agents voor Linux die zijn geconfigureerd om te rapporteren aan meerdere OMS-werkruimten, worden niet ondersteund in deze oplossing.  
    > 

Raadpleeg [Operations Management Suite-agent voor Linux](https://github.com/microsoft/oms-agent-for-linux) voor meer informatie over het installeren van de OMS-agent voor Linux en om de nieuwste versie te downloaden.  Raadpleeg [Operations Management Suite-agent voor Windows](../log-analytics/log-analytics-windows-agents.md) voor meer informatie over het installeren van de OMS-agent voor Windows.  

## <a name="solution-components"></a>Oplossingsonderdelen
Deze oplossing bestaat uit de volgende resources die zijn toegevoegd aan uw Automation-account en rechtstreeks verbonden agents of verbonden Operations Manager-beheergroepen. 

### <a name="management-packs"></a>Management packs
Als de beheergroep van uw System Center Operations Manager is verbonden met een OMS-werkruimte, worden de volgende management packs geïnstalleerd in Operations Manager.  Deze management packs worden na het toevoegen van deze oplossing ook op rechtstreeks verbonden Windows-computers geïnstalleerd. Met deze management packs hoeft u niets te configureren of te beheren. 

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Implementatie MP bijwerken

Zie [Operations Manager koppelen aan Log Analytics](../log-analytics/log-analytics-om-agents.md) voor meer informatie over de manier waarop uw management packs voor oplossingen worden bijgewerkt.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-groepen
Als u deze oplossing inschakelt, wordt elke Windows-computer die rechtstreeks is verbonden met uw OMS-werkruimte, automatisch geconfigureerd als Hybrid Runbook Worker, zodat de runbooks die in deze oplossing zijn opgenomen, worden ondersteund.  Voor elke Windows-computer die door de oplossing wordt beheerd, wordt deze vermeld op de blade Hybrid Runbook Worker-groepen van het Automation-account, met de naamconventie *Hostname FQDN_GUID*.  U kunt deze groepen niet targeten met runbooks uit uw account. De bewerking mislukt dan. Deze groepen zijn alleen bedoeld ter ondersteuning van de beheeroplossing.   

U kunt de Windows-computers echter wel toevoegen aan een Hybrid Runbook Worker-groep in uw Automation-account voor ondersteuning van Automation-runbooks, mits u hetzelfde account gebruikt voor zowel de oplossing als het lidmaatschap van de Hybrid Runbook Worker-groep.  Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.  

## <a name="configuration"></a>Configuratie
Voer de volgende stappen uit om de oplossing Update Management toe te voegen aan uw OMS-werkruimte en om te controleren of agents wel rapporteren. Windows-agents worden automatisch toegevoegd zonder extra configuratie. 

Er zijn drie verschillende manieren waarop u deze oplossing kunt toevoegen: vanuit Azure Marketplace in Azure Portal, door de aanbieding Automation en besturing of de Update Management-oplossing te selecteren, of vanuit de OMS-oplossingengalerie in uw OMS-werkruimte.  Als u al een Automation-account hebt en de OMS-werkruimte is gekoppeld aan dezelfde resourcegroep en regio, kunt u Automation en besturing selecteren om de configuratie te verifiëren. U installeert dan alleen de oplossing en deze wordt vervolgens in beide services geconfigureerd.  Als u de Update Management-oplossing selecteert in Azure Marketplace, ontstaat hetzelfde gedrag.  Als u in uw abonnement geen van beide services hebt geïmplementeerd, volgt u de stappen op de blade **Nieuwe oplossing maken** en bevestigt u dat u de andere vooraf geselecteerde aanbevolen oplossingen wilt installeren.  Optioneel kunt u ook de oplossing Update Management toevoegen aan uw OMS-werkruimte volgens de procedure die is beschreven in [OMS-oplossingen toevoegen](../log-analytics/log-analytics-add-solutions.md). Dit kan via de Oplossingengalerie.  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-to-oms"></a>Controleren of OMS-agents en Operations Manager-beheergroepen zijn verbonden met OMS

Als u wilt controleren of rechtstreeks verbonden OMS-agents voor Linux en Windows communiceren met OMS, voert u na enkele minuten de volgende logboekzoekopdracht uit:

* Linux - `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows - `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Op Windows-computers kunt u het volgende controleren om te bekijken of agents verbonden zijn met OMS:

1.  Open Microsoft Monitoring Agent in het Configuratiescherm en ga naar het tabblad **Azure-logboekanalyse (OMS)**. Het volgende bericht wordt weergegeven voor de agent: **The Microsoft Monitoring Agent has successfully connected to the Microsoft Operations Management Suite service**.   
2.  Open het Windows-gebeurtenislogboek, ga naar **Toepassings- en servicelogboeken\Operations Manager**, en zoek naar gebeurtenis-id 3000 en 5002 van de bronserviceconnector.  Deze gebeurtenissen geven aan of de computer is geregistreerd bij de OMS-werkruimte en of deze configuratie ontvangt.  

Als de agent niet kan communiceren met de OMS-service en is geconfigureerd voor communicatie met internet via een firewall of proxyserver, controleert u of de firewall of proxyserver correct is geconfigureerd. Dit doet u aan de hand van [Proxy- en firewallinstellingen configureren in Log Analytics](../log-analytics/log-analytics-proxy-firewall.md).
  
Bij pas toegevoegde Linux-agents staat de status **Bijgewerkt** na het uitvoeren van een beoordeling.  Dit proces kan maximaal zes uur duren. 

Als u wilt controleren of een Operations Manager-beheergroep communiceert met OMS, raadpleegt u [Validate Operations Manager Integration with OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms) (Operations Manager-integratie met OMS controleren).

## <a name="data-collection"></a>Gegevensverzameling
### <a name="supported-agents"></a>Ondersteunde agents
De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Windows-agents en start de installatie van de vereiste updates. |
| Linux-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Linux-agents en start de installatie van de vereiste updates op ondersteunde besturingssysteemversies. |
| Beheergroep Operations Manager |Ja |De oplossing verzamelt informatie over systeemupdates van agents in een verboden beheergroep.<br>Er is geen directe verbinding van de Operations Manager-agent naar Log Analytics vereist. Gegevens worden doorgestuurd van de beheergroep naar de OMS-opslag. |
| Azure Storage-account |Nee |Azure Storage bevat geen informatie over systeemupdates. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie
Voor elke beheerde Windows-computer wordt twee keer per dag een scan uitgevoerd. Om de vijftien minuten wordt de Windows-API aangeroepen voor de laatste updatetijd om op die manier te bepalen of de status is gewijzigd. Als dat het geval is, wordt er een nalevingsscan gestart.  Voor elke beheerde Linux-computer wordt elke drie uur een scan uitgevoerd. 

Het kan dertig minuten tot zes uur duren voordat er in het dashboard bijgewerkte gegevens worden weergegeven van de beheerder computers.   

## <a name="using-the-solution"></a>De oplossing gebruiken
Wanneer u de oplossing Updatebeheer toevoegt aan uw OMS-werkruimte wordt de tegel **Updatebeheer** toegevoegd aan uw OMS-dashboard. Deze tegel toont het aantal computers in uw omgeving, evenals een grafische voorstelling hiervan, en de bijbehorende updatenaleving.<br><br>
![Tegel overzicht updatebeheer](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>Update-evaluaties weergeven
Klik op de tegel **Updatebeheer** om het dashboard **Updatebeheer** te openen.<br><br> ![Dashboard overzicht updatebeheer](./media/oms-solution-update-management/update-management-dashboard.png)<br> 

Dit dashboard biedt een gedetailleerd overzicht van de updatestatus, ingesteld op type besturingssysteem en updateclassificatie (kritiek, beveiligingsgerelateerd of anders, zoals bij een definitie-update). De tegel **Update-implementaties** verwijst u naar de pagina Update-implementaties, waar u planningen, updates die momenteel worden uitgevoerd en voltooide implementaties kunt bekijken. U kunt hier ook nieuwe implementaties plannen.  

U kunt een logboekzoekopdracht uitvoeren voor het retourneren van alle records. Hiervoor klikt u op de specifieke tegel of voert u een query voor een bepaalde categorie en vooraf gedefinieerde criteria uit. Selecteer dan één optie uit de lijst die beschikbaar is in de kolom **Algemene updatequery's**.    

## <a name="installing-updates"></a>Updates installeren
Wanneer de updates zijn beoordeeld voor alle Linux- en Windows-computers in uw werkruimte, kunt u de vereiste updates installeren door een *update-implementatie* te maken.  Een update-implementatie is een geplande installatie van de vereiste updates voor een of meer computers.  U geeft de datum en tijd op voor de implementatie, samen met een computer of een groep computers die moet worden opgenomen in het implementatiebereik.  Zie [Computergroepen in Log Analytics](../log-analytics/log-analytics-computer-groups.md) voor meer informatie over computergroepen.  Als u computergroepen gebruikt voor de update-implementatie, wordt het groepslidmaatschap slechts één keer geëvalueerd (bij het opstellen van de planning).  Toekomstige wijzigingen aan een groep worden niet weergegeven.  U kunt dit probleem omzeilen door de geplande update-implementatie te verwijderen en deze opnieuw te maken. 

> [!NOTE]
> Virtuele Windows-machines die via Azure Marketplace zijn geïmplementeerd, worden standaard ingesteld voor het automatisch ontvangen van updates van de Windows Update-service.  Dit gedrag wordt niet gewijzigd na het toevoegen van deze oplossing of na het toevoegen van virtuele Windows-machines aan uw werkruimte.  Als u updates niet actief beheert met deze oplossing, wordt het standaardgedrag toegepast (automatisch updates toepassen).  

Voor virtuele machines die zijn gemaakt op basis van de on-demand RHEL-installatiekopieën (Red Hat Enterprise Linux) die beschikbaar zijn in Azure Marketplace, zijn ze geregistreerd voor toegang tot de [Red Hat Update Infrastructure (RHUI)](../virtual-machines/linux/update-infrastructure-redhat.md) die is geïmplementeerd in Azure.  Andere Linux-distributies moeten volgens de ondersteunde methoden worden bijgewerkt vanuit de online distro-bestandsopslagplaats.  

### <a name="viewing-update-deployments"></a>Update-implementaties weergeven
Klik op de tegel **Update-implementatie** om de lijst met bestaande Update-implementaties weer te geven.  Ze zijn gegroepeerd op status – **Gepland**, **Wordt uitgevoerd** en **voltooid**.<br><br> ![Pagina Planning update-implementaties](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

De eigenschappen die worden weergegeven voor elke Update-implementatie worden beschreven in de volgende tabel.

| Eigenschap | Beschrijving |
| --- | --- |
| Naam |Naam van de Update-implementatie. |
| Planning |Type planning.  Beschikbare opties zijn *één keer*, *elke week* en *elke maand*. |
| Begintijd |Datum en tijdstip waarop de Update-implementatie moet starten volgens de planning. |
| Duur |Het aantal minuten dat de update-implementatie mag worden uitgevoerd.  Als niet alle updates binnen deze tijd zijn geïnstalleerd, moeten de resterende updates wachten tot de volgende update-implementatie. |
| Servers |Het aantal computers dat is beïnvloed door de update-implementatie.  |
| Status |Huidige status van de update-implementatie.<br><br>Mogelijke waarden zijn:<br>- Niet gestart<br>- Wordt uitgevoerd<br>- Voltooid |

Selecteer een voltooide update-implementatie om het detailscherm ervan weer te geven. Dit scherm bevat de kolommen die u in de volgende tabel ziet.  Deze kolommen worden niet ingevuld als de Update-implementatie nog niet is gestart.<br><br> ![Overzicht van de resultaten van de update-implementatie](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Kolom | Beschrijving |
| --- | --- |
| **Weergave voor computers** | |
| Windows-computers |Toont het aantal Windows-computers in de update-implementatie op status.  Klik op een status voor het uitvoeren van een logboekzoekopdracht die alle updaterecords met die status retourneert voor de update-implementatie. |
| Linux-computers |Toont het aantal Linux-computers in de update-implementatie op status.  Klik op een status voor het uitvoeren van een logboekzoekopdracht die alle updaterecords met die status retourneert voor de update-implementatie. |
| Installatiestatus computer |Toont de computers die betrokken zijn bij de update-implementatie en het percentage updates dat is geïnstalleerd. Klik op een van de gegevens om een zoekactie die alle ontbrekende en essentiële updates retourneert, uit te voeren op het logboek. |
| **Weergave voor updates** | |
| Windows-updates |Toont een lijst met Windows-updates uit de update-implementatie en de installatiestatus per update.  Selecteer een update om een logboekzoekopdracht uit te voeren waarmee alle updaterecords voor die specifieke update worden geretourneerd. U kunt ook op de status klikken om een logboekzoekopdracht uit te voeren om alle updaterecords van de implementatie te retourneren. | 
| Linux-updates |Toont een lijst met Linux-updates uit de update-implementatie en de installatiestatus per update.  Selecteer een update om een logboekzoekopdracht uit te voeren waarmee alle updaterecords voor die specifieke update worden geretourneerd. U kunt ook op de status klikken om een logboekzoekopdracht uit te voeren om alle updaterecords van de implementatie te retourneren. | 

### <a name="creating-an-update-deployment"></a>Een update-implementatie maken
Maak een nieuwe update-implementatie door bovenaan op het scherm te klikken op de knop **Toevoegen** om de pagina **Nieuwe update-implementatie** te openen.  U moet waarden voor de eigenschappen opgeven in de volgende tabel.

| Eigenschap | Beschrijving |
| --- | --- |
| Naam |Unieke naam voor het identificeren van de update-implementatie. |
| Tijdzone |Tijdzone die moet worden gebruikt voor de begintijd. |
| Planningstype | Type planning.  Beschikbare opties zijn *één keer*, *elke week* en *elke maand*.  
| Begintijd |Datum en tijdstip waarop de update-implementatie start. **Opmerking:** als u direct moet implementeren, kan de eerste implementatie op zijn snelst een halfuur na de huidige tijd worden uitgevoerd. |
| Duur |Het aantal minuten dat de update-implementatie mag worden uitgevoerd.  Als niet alle updates binnen deze tijd zijn geïnstalleerd, moeten de resterende updates wachten tot de volgende update-implementatie. |
| Computers |Namen van computers of computergroepen die moeten worden opgenomen in de update-implementatie en die extra aandacht moeten krijgen.  Selecteer een of meer gegevens in de vervolgkeuzelijst. |

<br><br> ![Pagina Nieuwe update-implementatie](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Tijdsbereik
Het bereik van de gegevens die worden geanalyseerd in de Updatebeheeroplossing is standaard van alle verbonden beheergroepen die de afgelopen dag zijn gegenereerd. 

Om het tijdbereik van de gegevens te wijzigen, selecteert u **Gegevens op basis van** bovenaan op het dashboard. U kunt records selecteren die de afgelopen 7 dagen, 1 dag of 6 uur zijn gemaakt of bijgewerkt. U kunt ook **Aangepast** selecteren en een aangepast datumbereik opgeven.

## <a name="log-analytics-records"></a>Log Analytics-records
Met de oplossing Updatebeheer worden twee typen records gemaakt in de OMS-opslagplaats.

### <a name="update-records"></a>Records bijwerken
Een record met het type **Bijwerken** wordt gemaakt voor elke update die is geïnstalleerd of vereist is op elke computer. Updaterecords hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
| --- | --- |
| Type |*Update* |
| SourceSystem |De bron die de installatie van de update heeft goedgekeurd.<br>Mogelijke waarden zijn:<br>- Microsoft Update<br>- Windows Update<br>- SCCM<br>- Linux-servers (opgehaald van pakketbeheer) |
| Goedgekeurd |Hiermee geeft u op of de update is goedgekeurd voor installatie.<br> Voor Linux-servers is dit momenteel optioneel omdat patchen niet wordt beheerd door OMS. |
| Classificatie voor Windows |Classificatie van de update.<br>Mogelijke waarden zijn:<br>-    Toepassingen<br>- Essentiële updates<br>- Definitie-updates<br>- Functiepakketten<br>- Beveiligingsupdates<br>- Servicepacks<br>- Updatepakketten<br>- Updates |
| Classificatie voor Linux |Classificatie van de update.<br>Mogelijke waarden zijn:<br>- Essentiële updates<br>- Beveiligingsupdates<br>- Andere Updates |
| Computer |Naam van de computer. |
| InstallTimeAvailable |Hiermee geeft u op de installatietijd beschikbaar is van agents die dezelfde update hebben geïnstalleerd. |
| InstallTimePredictionSeconds |Geschatte installatietijd in seconden op basis van andere agents die dezelfde update hebben geïnstalleerd. |
| KBID |ID van het KB-artikel dat de update beschrijft. |
| ManagementGroupName |Naam van de beheergroep voor SCOM-agents.  Voor andere agents is dit AOI -<workspace ID>. |
| MSRCBulletinID |ID van het Microsoft-beveiligingsbulletin dat de update beschrijft. |
| MSRCSeverity |Ernst van het Microsoft-beveiligingsbulletin.<br>Mogelijke waarden zijn:<br>- Kritiek<br>- Belangrijk<br>- Gemiddeld |
| Optioneel |Hiermee geeft u op of de update optioneel is. |
| Product |Naam van het product waarvoor de update is.  Klik op **Weergeven** om het artikel in een browser te openen. |
| PackageSeverity |De ernst van de kwetsbaarheid die is opgelost in deze update, zoals gemeld door de Linux distro-verkopers. |
| PublishDate |Datum en tijd waarop de update is geïnstalleerd. |
| RebootBehavior |Hiermee geeft u op als de update opnieuw opstarten moet forceren.<br>Mogelijke waarden zijn:<br>- canrequestreboot<br>- neverreboots |
| RevisionNumber |Revisienummer van de update. |
| SourceComputerId |GUID voor het uniek identificeren van de computer. |
| TimeGenerated |Datum en tijd waarop de record het laatst werd bijgewerkt. |
| Titel |Titel van de update. |
| UpdateID |GUID voor het uniek identificeren van de update. |
| UpdateState |Hiermee geeft u op of de update is geïnstalleerd op deze computer.<br>Mogelijke waarden zijn:<br>- Geïnstalleerd - De update is geïnstalleerd op deze computer.<br>- Nodig - De update niet is geïnstalleerd en is vereist op deze computer. |

Wanneer u een zoekopdracht in logboeken uitvoert, waarmee records van het type **Bijwerken** retourneert, kunt u de weergave **Updates** selecteren. Hiermee wordt een set tegels weergegeven met een samenvatting van de updates die door de zoekopdracht worden geretourneerd. U kunt klikken op de gegevens in de tegels **Ontbrekende en toegepaste updates** en **Vereiste en optionele updates** om de weergave te beperken tot die set updates. Selecteer de weergave **Lijst** of **Tabel** om de individuele records te retourneren.<br> 

![Weergave update logboekzoekactie met Recordtype update](./media/oms-solution-update-management/update-la-view-updates.png)  

In de weergave **Tabel** kunt u klikken op de **KBID** voor elke record om een browser met het KB-artikel te openen. Hiermee kunt u snel meer lezen over de details van de specifieke update.<br> 

![Weergave tabel logboekzoekactie met tegels recordtype updates](./media/oms-solution-update-management/update-la-view-table.png)

In de weergave **Lijst** kunt u klikken op de koppeling **Weergave** naar de KBID om het KB-artikel te openen.<br>

![Weergave lijst logboekzoekactie met tegels recordtype updates](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>UpdateSummary records
Een record van het type **UpdateSummary** wordt gemaakt voor elke Windows-agentcomputer. Deze record wordt telkens bijgewerkt wanneer de computer wordt gescand op updates. **UpdateSummary**-records hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
| --- | --- |
| Type |UpdateSummary |
| SourceSystem |OpsManager |
| Computer |Naam van de computer. |
| CriticalUpdatesMissing |Het aantal essentiële updates dat ontbreekt op de computer. |
| ManagementGroupName |Naam van de beheergroep voor SCOM-agents. Voor andere agents is dit AOI -<workspace ID>. |
| NETRuntimeVersion |De versie van de .NET runtime die op de computer geïnstalleerd. |
| OldestMissingSecurityUpdateBucket |Bucket voor het categoriseren van de tijd sinds de oudste ontbrekende beveiligingsupdate op deze computer is gepubliceerd.<br>Mogelijke waarden zijn:<br>- Ouder<br>-    180 dagen geleden<br>- 150 dagen geleden<br>-    120 dagen geleden<br>- 90 dagen geleden<br>- 60dagen geleden<br>-    30 dagen geleden<br>-    Recent |
| OldestMissingSecurityUpdateInDays |Aantal dagen sinds de oudste ontbrekende beveiligingsupdate op deze computer is gepubliceerd. |
| OsVersion |De versie van het besturingssysteem dat op de computer is geïnstalleerd. |
| OtherUpdatesMissing |Het aantal overige updates dat ontbreekt op de computer. |
| SecurityUpdatesMissing |Het aantal beveiligingsupdates dat ontbreekt op de computer. |
| SourceComputerId |GUID voor het uniek identificeren van de computer. |
| TimeGenerated |Datum en tijd waarop de record het laatst werd bijgewerkt. |
| TotalUpdatesMissing |Het aantal updates dat ontbreekt op de computer. |
| WindowsUpdateAgentVersion |Versienummer van de Windows Update-agent op de computer. |
| WindowsUpdateSetting |De instelling voor hoe de computer belangrijke updates zal installeren.<br>Mogelijke waarden zijn:<br>- Uitgeschakeld<br>- Waarschuwen vóór installatie<br>- Geplande installatie |
| WSUSServer |URL van de WSUS-server als de computer is geconfigureerd om er een te gebruiken. |

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken
De volgende tabel biedt voorbeeldzoekopdrachten in logboeken voor updaterecords die worden verzameld door deze oplossing. 

| Query’s uitvoeren | Beschrijving |
| --- | --- |
|Servercomputers met Windows die moeten worden bijgewerkt |`Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false | measure count() by Computer` |
|Linux-servers die moeten worden bijgewerkt | `Type:Update OSType=Linux UpdateState!="Not needed" | measure count() by Computer` |
| Alle computers met ontbrekende updates |`Type=Update UpdateState=Needed Optional=false | select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Ontbrekende updates voor een specifieke computer (vervang de waarde door de naam van uw eigen computer) |`Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" | select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate` |
| Alle computers met ontbrekende essentiële of beveiligingsupdates |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates"`) |
| Essentiële of beveiligingsupdates die benodigd zijn door machines wanneer updates handmatig worden toegepast |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | Distinct Computer} | Distinct KBID` |
| Foutgebeurtenissen voor machines met ontbrekende essentiële of beveiligingsupdates |`Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false | Distinct Computer}` |
| Alle computers met ontbrekende updatepakketten |`Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed| select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Afzonderlijke ontbrekende updates op alle computers |`Type=Update UpdateState=Needed Optional=false | Distinct Title` |
| Windows-servercomputer met updates die zijn mislukt tijdens het uitvoeren van updates | `Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Title, UpdateRunName` |
| Linux-server met updates die zijn mislukt tijdens het uitvoeren van updates |`Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Product, UpdateRunName` |
| WSUS-computerlidmaatschap |`Type=UpdateSummary | measure count() by WSUSServer` |
| Configuratie automatische updates |`Type=UpdateSummary | measure count() by WindowsUpdateSetting` |
| Computers met automatische update uitgeschakeld |`Type=UpdateSummary WindowsUpdateSetting=Manual` |
| Lijst van alle Linux-machines waarvoor een pakketupdate beschikbaar is |`Type=Update and OSType=Linux and UpdateState!="Not needed" | measure count() by Computer` |
| Lijst van alle Linux-machines waarvoor een pakketupdate beschikbaar is die de essentiële of beveiligingskwetsbaarheid oplost |`Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") | measure count() by Computer` |
| Lijst met alle pakketten waarvoor een update beschikbaar is |Type=Update and OSType=Linux and UpdateState!="Not needed" |
| Lijst met alle pakketten waarvoor een update beschikbaar die essentiële of beveiligingskwetsbaarheid oplost |`Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates")` |
| Weergeven met welke update-implementaties computers zijn gewijzigd |`Type:UpdateRunProgress | measure Count() by UpdateRunName` |
|Computers die zijn bijgewerkt tijdens het uitvoeren van updates (vervang de waarde met de naam van uw update-implementatie) |`Type:UpdateRunProgress UpdateRunName="DeploymentName" | measure Count() by Computer` |
| Lijst van alle "Ubuntu"-machines waarvoor een update beschikbaar is |`Type=Update and OSType=Linux and OSName = Ubuntu &| measure count() by Computer` |

## <a name="troubleshooting"></a>Problemen oplossen 

Deze sectie bevat informatie over het oplossen van problemen met de oplossing Update Management.  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Hoe kan ik problemen met update-implementaties oplossen?
U kunt de resultaten bekijken van het runbook dat verantwoordelijk is voor het implementeren van de updates uit de geplande update-implementatie. Ga daarvoor naar de blade Taken van uw Automation-account dat is gekoppeld aan de OMS-werkruimte die deze oplossing ondersteunt.  Het runbook **Patch-MicrosoftOMSComputer** is een onderliggend runbook dat is gericht op een specifieke beheerde computer. Als u de gedetailleerde stroom bekijkt, komt u meer te weten over die implementatie.  In de uitvoer staat welke vereiste updates van toepassing zijn, wat de downloadstatus is en wat de installatiestatus is. Ook wordt er aanvullende informatie geboden.<br><br> ![De taakstatus van de update-implementatie](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

Zie [Automation-runbookuitvoer en -berichten](../automation/automation-runbook-output-and-messages.md) voor meer informatie.   
  
## <a name="next-steps"></a>Volgende stappen
* Gebruik Logboekzoekopdrachten in [Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde updategegevens weer te geven.
* [Maak uw eigen dashboards](../log-analytics/log-analytics-dashboards.md) die de updatecompatibiliteit voor uw beheerde computers weergeven.
* [Maak waarschuwingen](../log-analytics/log-analytics-alerts.md) wanneer wordt vastgesteld dat er essentiële updates ontbreken op de computers of als automatische updates is uitgeschakeld voor een computer.  


