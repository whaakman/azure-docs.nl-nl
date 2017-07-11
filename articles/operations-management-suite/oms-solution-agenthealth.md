---
title: Oplossing Status van agent in OMS | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u deze oplossing kunt gebruiken om de status te controleren van agents die rechtstreeks aan OMS of System Center Operations Manager rapporteren.
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/22/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: ca2316010f9508b6dc71ceff434988695063db0f
ms.contentlocale: nl-nl
ms.lasthandoff: 06/23/2017


---
<a id="agent-health-solution-in-oms" class="xliff"></a>

#  De oplossing Status van agent in OMS
De oplossing Status van agent in OMS geeft inzicht in de agents die niet reageren en de agents die wel operationele gegevens versturen. Het betreft alle agents die rechtstreeks aan de OMS-werkruimte rapporteren of aan een beheergroep van System Center Operations Manager die is verbonden met OMS.  U kunt ook bijhouden hoeveel agents er zijn geïmplementeerd en waar deze zich geografisch gezien bevinden. Bovendien kunt u query's uitvoeren om op de hoogte te blijven van de verdeling van agents over Azure, andere cloudomgevingen of on-premises.    

<a id="prerequisites" class="xliff"></a>

## Vereisten
U kunt deze oplossing alleen implementeren als u beschikt over ondersteunde [Windows-agents](../log-analytics/log-analytics-windows-agents.md) die rapporteren aan de OMS-werkruimte of aan een [Operations Manager-beheergroep](../log-analytics/log-analytics-om-agents.md) die is geïntegreerd met de OMS-werkruimte.    

<a id="solution-components" class="xliff"></a>

## Oplossingsonderdelen
Deze oplossing bestaat uit de volgende resources die worden toegevoegd aan uw werkruimte en rechtstreeks verbonden agents of verbonden Operations Manager-beheergroepen. 

<a id="management-packs" class="xliff"></a>

### Management packs
Als de beheergroep van uw System Center Operations Manager is verbonden met een OMS-werkruimte, worden de volgende management packs geïnstalleerd in Operations Manager.  Deze management packs worden na het toevoegen van deze oplossing ook op rechtstreeks verbonden Windows-computers geïnstalleerd. Met deze management packs hoeft u niets te configureren of te beheren. 

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack  (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Zie [Operations Manager koppelen aan Log Analytics](../log-analytics/log-analytics-om-agents.md) voor meer informatie over de manier waarop uw management packs voor oplossingen worden bijgewerkt.

<a id="configuration" class="xliff"></a>

## Configuratie
U kunt de oplossing Status van agent toevoegen aan uw OMS-werkruimte met behulp van de procedure die wordt beschreven in [Oplossingen toevoegen](../log-analytics/log-analytics-add-solutions.md). Er is geen verdere configuratie nodig.


<a id="data-collection" class="xliff"></a>

## Gegevensverzameling
<a id="supported-agents" class="xliff"></a>

### Ondersteunde agents
De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents | Ja | Er worden heartbeat-gebeurtenissen verzameld van direct verbonden Windows-agents.|
| Beheergroep System Center Operations Manager | Ja | Er worden elke 60 seconden heartbeat-gebeurtenissen verzameld van agents die rapporteren aan de beheergroep en deze worden vervolgens doorgestuurd naar Log Analytics. Er is geen directe verbinding tussen Operations Manager-agents en Log Analytics vereist. Gegevens van heartbeat-gebeurtenissen worden vanuit de beheergroep doorgestuurd naar de opslagplaats van Log Analytics.|

<a id="using-the-solution" class="xliff"></a>

## De oplossing gebruiken
Wanneer u de oplossing toevoegt aan uw OMS-werkruimte, wordt de tegel **Status van agent** toegevoegd aan uw OMS-dashboard. Op deze tegel ziet u het totale aantal agents en het aantal agents dat de afgelopen 24 uur niet heeft gereageerd.<br><br> ![De tegel Status van agent in het dashboard](./media/oms-solution-agenthealth/agenthealth-solution-tile-homepage.png)

Klik op de tegel **Status van agent** om het**** gelijknamige dashboard te openen.  Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat de tien belangrijkste gebeurtenissen naar aantal die overeenkomen met de criteria van die kolom voor de opgegeven periode. U kunt de volledige lijst met gebeurtenissen weergeven door linksonder elke kolom **Alles weergeven** te selecteren. Dit kan overigens ook door op de kolomkop te klikken.

| Kolom | Beschrijving | 
|--------|-------------|
| Agent count over time | Een trend van het aantal agents gedurende een periode van zeven dagen voor Linux- en Windows-agents.| 
| Count of unresponsive agents | Een lijst met agents die in de afgelopen 24 uur geen heartbeat hebben verzonden.| 
| Distribution by OS Type | Een visualisatie van het aantal Windows- en Linux-agents in uw omgeving.| 
| Distribution by Agent Version | Een visualisatie van de verschillende agentversies die zijn geïnstalleerd in uw omgeving en het aantal van elke versie.| 
| Distribution by Agent Category | Een visualisatie van de verschillende categorieën agents die heartbeat-gebeurtenissen verzenden: directe agents, OpsMgr-agents of OpsMgr Management Server.| 
| Distribution by Management Group | Een visualisatie van de verschillende SCOM-beheergroepen in uw omgeving.| 
| Geo-location of Agents | Een visualisatie van de verschillende landen waarin zich agents bevinden en het totale aantal agents dat in elk land is geïnstalleerd.| 
| Count of Gateways Installed | Het aantal servers waarop de OMS-gateway is geïnstalleerd en een lijst van deze servers.|

![Voorbeeld van het dashboard van de oplossing Status van agent](./media/oms-solution-agenthealth/agenthealth-solution-dashboard.png)  

<a id="log-analytics-records" class="xliff"></a>

## Log Analytics-records
De oplossing voegt één type record toe aan de OMS-opslagplaats.  

<a id="heartbeat-records" class="xliff"></a>

### Heartbeat-records
Er wordt een record van het type **Heartbeat** gemaakt.  Deze records hebben de eigenschappen uit de volgende tabel.  

| Eigenschap | Beschrijving |
| --- | --- |
| Type | *Heartbeat*|
| Category | De waarde is *Direct Agent*, *SCOM Agent* of *SCOM Management Server*.| 
| Computer | De naam van de computer.| 
| OSType | Windows- of Linux-besturingssysteem.| 
| OSMajorVersion | De primaire versie van het besturingssysteem.| 
| OSMinorVersion | De secundaire versie van het besturingssysteem.| 
| Versie | De versie van de OMS-agent of Operations Manager-agent.| 
| SCAgentChannel | De waarde is *Direct* en/of *SCManagementServer*.| 
| IsGatewayInstalled | Als de OMS-gateway is geïnstalleerd, is de waarde *true*, anders is de waarde *false*.| 
| ComputerIP | Het IP-adres van de computer.| 
| RemoteIPCountry | De geografische locatie waar de computer is geïmplementeerd.| 
| ManagementGroupName | De naam van de beheergroep van Operations Manager.| 
| SourceComputerId | De unieke ID van de computer.| 
| RemoteIPLongitude | De lengtegraad van geografische locatie van de computer.| 
| RemoteIPLatitude | De breedtegraad van de geografische locatie van de computer.| 

Elke agent die rapporteert aan een Operations Manager-beheerserver stuurt twee heartbeats, en de waarde van de eigenschap SCAgentChannel bevat zowel **Direct** als **SCManagementServer**, afhankelijk van de gegevensbronnen van Log Analytics en de oplossingen die zijn ingeschakeld in uw OMS-abonnement. Zoals eerder vermeld, worden gegevens van oplossingen rechtstreeks verzonden vanaf een Operations Manager-beheerserver naar de OMS-webservice of, vanwege de grote hoeveelheid gegevens die wordt verzameld op de agent, rechtstreeks vanaf de agent naar de OMS-webservice. Voor heartbeat-gebeurtenissen met de waarde **SCManagementServer** bestaat de waarde van ComputerIP uit het IP-adres van de beheerserver, aangezien de gegevens door deze server worden geüpload.  Voor heartbeats waarvan de eigenschap SCAgentChannel is ingesteld op **Direct**, is de waarde het openbare IP-adres van de agent.  

<a id="sample-log-searches" class="xliff"></a>

## Voorbeeldzoekopdrachten in logboeken
De volgende tabel bevat voorbeelden van zoekopdrachten in logboeken voor records die zijn verzameld met deze oplossing. 

| Query’s uitvoeren | Beschrijving |
| --- | --- |
| Type=Heartbeat &#124; distinct Computer |Het totale aantal agents | 
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-24HOURS |Het aantal agents dat de afgelopen 24 uur niet heeft gereageerd | 
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-15MINUTES |Het aantal agents dat de afgelopen 15 minuten niet heeft gereageerd | 
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer IN {Type=Heartbeat TimeGenerated>NOW-24HOURS &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Computers die online waren (in de afgelopen 24 uur) | 
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer NOT IN {Type=Heartbeat TimeGenerated>NOW-30MINUTES &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Het totale aantal agents dat offline was in de afgelopen 30 minuten (voor de afgelopen 24 uur) | 
| Type=Heartbeat &#124; measure countdistinct(Computer) by OSType |Een trend van het aantal agents in de tijd per type besturingssysteem| 
| Type=Heartbeat&#124;measure countdistinct(Computer) by OSType |Distribution by OS Type | 
| Type=Heartbeat&#124;measure countdistinct(Computer) by Version |Verdeling naar agent-versie | 
| Type=Heartbeat&#124;measure count() by Category |Verdeling naar agent-catgeorie | 
| Type=Heartbeat&#124;measure countdistinct(Computer) by ManagementGroupName | Verdeling naar beheergroep | 
| Type=Heartbeat&#124;measure countdistinct(Computer) by RemoteIPCountry |Geo-location of Agents |
| Type=Heartbeat IsGatewayInstalled=true&#124;Distinct Computer |Het aantal geïnstalleerde OMS-gateways | 

  
<a id="next-steps" class="xliff"></a>

## Volgende stappen

* Zie [Understanding alerts in Log Analytics](../log-analytics/log-analytics-alerts.md) voor meer informatie over het genereren van waarschuwingen van Log Analytics.
