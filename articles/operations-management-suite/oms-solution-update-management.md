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
ms.date: 03/06/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 18aa17f6af7fe492f3875e1af7cb06b613f171af
ms.lasthandoff: 04/03/2017


---
# <a name="update-management-solution-in-oms"></a>De oplossing voor updatebeheer in OMS
Met de oplossing voor updatebeheer in OMS kunt u updates voor uw Windows- en Linux-computers beheren.  U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en de procedure voor het installeren van vereiste updates voor servers te starten. 

## <a name="prerequisites"></a>Vereisten
* De oplossing biedt alleen ondersteuning voor updatebeoordelingen op Windows Server 2008 en hoger en update-implementaties op Windows Server 2012 en hoger.  Server Core- en nanoserverinstallatieopties worden niet ondersteund.
* Windows-clientbesturingssystemen worden niet ondersteund.  
* Windows-agents moeten worden geconfigureerd om te communiceren met een WSUS-server (Windows Server Update Services of toegang hebben tot Microsoft Update.  
  
  > [!NOTE]
  > De Windows-agent kan niet tegelijkertijd door System Center Configuration Manager worden beheerd.  
  > 
  > 
* Linux-agents moeten toegang hebben tot een opslagplaats voor updates.  De OMS-agent voor Linux kan worden gedownload van [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Configuratie
Voer de volgende stappen om de oplossing Updatebeheer toe te voegen aan uw OMS-werkruimte en Linux-agents toe te voegen. Windows-agents worden automatisch toegevoegd zonder extra configuratie.

> [!NOTE]
> Als u deze oplossing inschakelt zal elke Windows-computer die verbonden is met uw OMS-werkruimte automatisch worden geconfigureerd als Hybrid Runbook Worker zodat de runbooks die in deze oplossing zijn opgenomen, worden ondersteund.  De computer wordt echter niet geregistreerd bij alle Hybrid Worker-groepen die u mogelijk al hebt gedefinieerd in uw Automation-account.  Deze kan aan een Hybrid Runbook Worker-groep in uw Automation-account worden toegevoegd voor ondersteuning van Automation-runbooks als u maar hetzelfde account gebruikt voor zowel de oplossing als het lidmaatschap van de Hybrid Runbook Worker-groep.  Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.   

1. Voeg de oplossing Updatebeheer toe aan uw OMS-werkruimte met het volgens de procedure die is beschreven in [OMS-oplossingen toevoegen](../log-analytics/log-analytics-add-solutions.md) vanaf de Galerie van oplossingen.  
2. Selecteer in de OMS-portal de optie **Instellingen** en vervolgens **Verbonden bronnen**.  Noteer de **Werkruimte-ID** en ofwel de **primaire sleutel**, ofwel de **secundaire sleutel**.
3. Voer de volgende stappen uit voor elk Linux-computer.
   
   a.    Installeer de nieuwste versie van de OMS-agent voor Linux door de volgende opdrachten uit te voeren.  Vervang <Workspace ID> door de werkruimte-ID en <Key> door de primaire of de secundaire sleutel.
   
        cd ~
        wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID>  -s <PrimaryKey> -d opinsights.azure.com 

   b. U kunt de agent verwijderen met de procedure die wordt beschreven in het gedeelte [De OMS-agent verwijderen in Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#uninstalling-the-oms-agent-for-linux).  

## <a name="management-packs"></a>Management packs
Als de beheergroep van uw System Center Operations Manager is verbonden met uw OMS-werkruimte, worden de volgende management packs geïnstalleerd in Operations Manager wanneer u deze oplossing toevoegt. Er is geen configuratie of onderhoud van deze management packs vereist. 

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Implementatie MP bijwerken

Zie [Operations Manager koppelen aan Log Analytics](../log-analytics/log-analytics-om-agents.md) voor meer informatie over de manier waarop uw management packs voor oplossingen worden bijgewerkt.

## <a name="data-collection"></a>Gegevensverzameling
### <a name="supported-agents"></a>Ondersteunde agents
De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Windows-agents en start de installatie van de vereiste updates. |
| Linux-agents |Ja |De oplossing verzamelt informatie over systeemupdates van Linux-agents. |
| Beheergroep Operations Manager |Ja |De oplossing verzamelt informatie over systeemupdates van agents in een verboden beheergroep.<br>Er is geen directe verbinding van de Operations Manager-agent naar Log Analytics vereist. Gegevens worden doorgestuurd van de beheergroep naar de OMS-opslag. |
| Azure Storage-account |Nee |Azure Storage bevat geen informatie over systeemupdates. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie
Voor elke beheerde Windows-computer wordt twee keer per dag een scan uitgevoerd.  Wanneer een update is geïnstalleerd, wordt de informatie ervan binnen 15 minuten bijgewerkt.  

Voor elke beheerde Linux-computer wordt elke drie uur een scan uitgevoerd.  

## <a name="using-the-solution"></a>De oplossing gebruiken
Wanneer u de oplossing Updatebeheer toevoegt aan uw OMS-werkruimte wordt de tegel **Updatebeheer** toegevoegd aan uw OMS-dashboard. Deze tegel toont een aantal en de grafische voorstelling van het aantal computers in uw omgeving dat momenteel systeemupdates vereist.<br><br>
![Tegel overzicht updatebeheer](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Update-evaluaties weergeven
Klik op de tegel **Updatebeheer** om het dashboard **Updatebeheer** te openen. Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat maximaal tien items die overeenkomen de criteria voor het opgegeven bereik en het tijdsbereik van die kolom. U kunt een zoekactie die alle records retourneert, uitvoeren op het logboek door onderaan in de kolom te klikken op **Alles bekijken** of door te klikken op de kolomkoptekst.

| Kolom | Beschrijving |
| --- | --- |
| **Ontbrekende updates computers** | |
| Essentiële of beveiligingsupdates |Toont de top tien van computers waarop updates ontbreken, gesorteerd op het aantal ontbrekende updates. Klik op een computernaam om een zoekactie die alle updaterecords retourneert voor die computer, uit te voeren op het logboek. |
| Essentiële of beveiligingsupdates ouder dan 30 dagen |Identificeert het aantal computers waarop essentiële of beveiligingsupdates ontbreken, gegroepeerd op de duur sinds de update werd gepubliceerd. Klik op een van de gegevens om een zoekactie die alle ontbrekende en essentiële updates retourneert, uit te voeren op het logboek. |
| **Vereiste ontbrekende updates** | |
| Essentiële of beveiligingsupdates |Toont een lijst van classificaties van updates die ontbreken op computers, gesorteerd op het aantal computers waarop updates in de categorie ontbreken. Klik op een classificatie om een zoekactie die alle updaterecords retourneert voor die classificatie, uit te voeren op het logboek. |
| **Update-implementaties** | |
| Update-implementaties |Het aantal momenteel geplande update-implementaties en de duur tot de volgende geplande uitvoering.  Klik op de tegel om planningen, momenteel uitgevoerd en voltooide updates weer te geven of om een nieuwe implementatie te plannen. |

<br>  
![Dashboard overzicht updatebeheer](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Weergave dashboard computer updatebeheer](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Weergave dashboard pakket updatebeheer](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Updates installeren
Zodra updates zijn beoordeeld voor alle Windows-computers in uw omgeving, kunt u de vereiste updates installeren door een *Update-implementatie* te maken.  Een Update-implementatie is een geplande installatie van de vereiste updates voor een of meer Windows-computers.  U geeft de datum en tijd op voor de implementatie, samen met een computer of een groep computers die moet worden opgenomen.  

Updates worden geïnstalleerd door runbooks in Azure Automation.  U kunt deze runbooks niet weergeven en ze vereisen geen configuratie.  Wanneer een Update-implementatie wordt gemaakt, maakt het een planning waarmee een masterupdate-runbook voor de in de planning opgenomen computers start op het opgegeven tijdstip.  Deze master-runbook start een onderliggend runbook op elke Windows-agent die installatie van de vereiste updates uitvoert.  

Voor virtuele machines die zijn gemaakt op basis van de on-demand RHEL-installatiekopieën (Red Hat Enterprise Linux) die beschikbaar zijn in Azure Marketplace, zijn ze geregistreerd voor toegang tot de [Red Hat Update Infrastructure (RHUI)](../virtual-machines/linux/update-infrastructure-redhat.md) die is geïmplementeerd in Azure.  Andere Linux-distributies moeten volgens de ondersteunde methoden worden bijgewerkt vanuit de online distro-bestandsopslagplaats.  

### <a name="viewing-update-deployments"></a>Update-implementaties weergeven
Klik op de tegel **Update-implementatie** om de lijst met bestaande Update-implementaties weer te geven.  Ze zijn gegroepeerd op status – **Gepland**, **Wordt uitgevoerd** en **voltooid**.<br><br> ![Pagina Planning update-implementaties](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

De eigenschappen die worden weergegeven voor elke Update-implementatie worden beschreven in de volgende tabel.

| Eigenschap | Beschrijving |
| --- | --- |
| Naam |Naam van de Update-implementatie. |
| Planning |Type planning.  *Eenmalig* is momenteel de enige mogelijke waarde. |
| Begintijd |Datum en tijdstip waarop de Update-implementatie moet starten volgens de planning. |
| Duur |Het aantal minuten dat de update-implementatie mag worden uitgevoerd.  Als niet alle updates binnen deze tijd zijn geïnstalleerd, moeten de resterende updates wachten tot de volgende update-implementatie. |
| Servers |Het aantal computers dat is beïnvloed door de update-implementatie. |
| Status |Huidige status van de update-implementatie.<br><br>Mogelijke waarden zijn:<br>-    Niet gestart<br>- Wordt uitgevoerd<br>- Voltooid |

Klik op een update-implementatie om het detailscherm ervan te zien dat de kolommen in de volgende tabel bevat.  Deze kolommen worden niet ingevuld als de Update-implementatie nog niet is gestart.<br>

| Kolom | Beschrijving |
| --- | --- |
| **Computerresultaten** | |
| Voltooid |Toont het aantal computers in de update-implementatie op status.  Klik op een status voor het uitvoeren van een logboekzoekopdracht die alle updaterecords met die status retourneert voor de update-implementatie. |
| Installatiestatus computer |Toont de computers die betrokken zijn bij de update-implementatie en het percentage updates dat is geïnstalleerd. Klik op een van de gegevens om een zoekactie die alle ontbrekende en essentiële updates retourneert, uit te voeren op het logboek. |
| **Instantieresultaten bijwerken** | |
| Installatiestatus instantie |Toont een lijst van classificaties van updates die ontbreken op computers, gesorteerd op het aantal computers waarop updates in de categorie ontbreken. Klik op een computer om een zoekactie die alle updaterecords retourneert voor die computer, uit te voeren op het logboek. |

<br><br> ![Overzicht van de resultaten van de update-implementatie](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Een update-implementatie maken
Maak een nieuwe update-implementatie door bovenaan op het scherm te klikken op de knop **Toevoegen** om de pagina **Nieuwe update-implementatie** te openen.  U moet waarden voor de eigenschappen opgeven in de volgende tabel.

| Eigenschap | Beschrijving |
| --- | --- |
| Naam |Unieke naam voor het identificeren van de update-implementatie. |
| Tijdzone |Tijdzone die moet worden gebruikt voor de begintijd. |
| Begintijd |Datum en tijdstip waarop de update-implementatie start. |
| Duur |Het aantal minuten dat de update-implementatie mag worden uitgevoerd.  Als niet alle updates binnen deze tijd zijn geïnstalleerd, moeten de resterende updates wachten tot de volgende update-implementatie. |
| Computers |Namen van computers of computergroepen die moeten worden opgenomen in de update-implementatie.  Selecteer een of meer gegevens in de vervolgkeuzelijst. |

<br><br> ![Pagina Nieuwe update-implementatie](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Tijdsbereik
Het bereik van de gegevens die worden geanalyseerd in de Updatebeheeroplossing is standaard van alle verbonden beheergroepen die de afgelopen dag zijn gegenereerd. 

Om het tijdbereik van de gegevens te wijzigen, selecteert u **Gegevens op basis van** bovenaan op het dashboard. U kunt records selecteren die de afgelopen 7 dagen, 1 dag of 6 uur zijn gemaakt of bijgewerkt. U kunt ook **Aangepast** selecteren en een aangepast datumbereik opgeven.<br><br> ![Optie aangepast tijdsbereik](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Log Analytics-records
Met de oplossing Updatebeheer worden twee typen records gemaakt in de OMS-opslagplaats.

### <a name="update-records"></a>Records bijwerken
Een record met het type **Bijwerken** wordt gemaakt voor elke update die is geïnstalleerd of vereist is op elke computer. Updaterecords hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
| --- | --- |
| Type |*Update* |
| SourceSystem |De bron die de installatie van de update heeft goedgekeurd.<br>Mogelijke waarden zijn:<br>- Microsoft Update<br>- Windows Update<br>-    SCCM<br>- Linux-servers (opgehaald van pakketbeheer) |
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

<br>
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
| Alle computers met ontbrekende updates |Type=Update UpdateState=Needed Optional=false &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |
| Ontbrekende updates voor computer "COMPUTER01.contoso.com" (vervangen door de naam van uw eigen computer) |Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" &#124; select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate |
| Alle computers met ontbrekende essentiële of beveiligingsupdates |Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") |
| Essentiële of beveiligingsupdates die benodigd zijn door machines wanneer updates handmatig worden toegepast |Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual &#124; Distinct Computer} &#124; Distinct KBID |
| Foutgebeurtenissen voor machines met ontbrekende essentiële of beveiligingsupdates |Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false &#124; Distinct Computer} |
| Alle computers met ontbrekende updatepakketten |Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |
| Afzonderlijke ontbrekende updates op alle computers |Type=Update UpdateState=Needed Optional=false &#124; Distinct Title |
| WSUS-computerlidmaatschap |Type=UpdateSummary &#124; measure count() by WSUSServer |
| Configuratie automatische updates |Type=UpdateSummary &#124; measure count() by WindowsUpdateSetting |
| Computers met automatische update uitgeschakeld |Type=UpdateSummary WindowsUpdateSetting=Manual |
| Lijst van alle Linux-machines waarvoor een pakketupdate beschikbaar is |Type=Update and OSType=Linux and UpdateState!="Not needed" &#124; measure count() by Computer |
| Lijst van alle Linux-machines waarvoor een pakketupdate beschikbaar is die de essentiële of beveiligingskwetsbaarheid oplost |Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") &#124; measure count() by Computer |
| Lijst met alle pakketten waarvoor een update beschikbaar is |Type=Update and OSType=Linux and UpdateState!="Not needed" |
| Lijst met alle pakketten waarvoor een update beschikbaar die essentiële of beveiligingskwetsbaarheid oplost |Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") |
| Lijst van alle "Ubuntu"-machines waarvoor een update beschikbaar is |Type=Update and OSType=Linux and OSName = Ubuntu &#124; measure count() by Computer |

## <a name="next-steps"></a>Volgende stappen
* Gebruik Logboekzoekopdrachten in [Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde updategegevens weer te geven.
* [Maak uw eigen dashboards](../log-analytics/log-analytics-dashboards.md) die de updatecompatibiliteit voor uw beheerde computers weergeven.
* [Maak waarschuwingen](../log-analytics/log-analytics-alerts.md) wanneer wordt vastgesteld dat er essentiële updates ontbreken op de computers of als automatische updates is uitgeschakeld voor een computer.  


