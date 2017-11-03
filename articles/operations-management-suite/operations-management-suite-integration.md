---
title: Integratie met Operations Management Suite (OMS) | Microsoft Docs
description: Naast de standaard functies van OMS gebruikt, kunt u deze integreren met andere toepassingen en services voor het leveren van een hybride beheeromgeving om aangepaste beheerscenario uniek is voor uw omgeving of voor een aangepaste management ervaring voor uw klanten.  Dit artikel bevat een overzicht van de verschillende opties voor het integreren met OMS en koppelingen naar artikelen gedetailleerde technische informatie verstrekt.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fc5f3a8a-77f7-4103-bd7e-744c15ffcca7
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 7a24df6f2c3b2c091d1b66b8b9c0a61035ffde11
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="integrating-with-operations-management-suite-oms"></a>Integratie met Operations Management Suite (OMS)
Operations Management Suite is een oplossing van Microsoft cloud-gebaseerde IT management waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur.  Naast de standaard functies van OMS gebruikt, kunt u deze integreren met andere toepassingen en services voor het leveren van een hybride beheeromgeving om aangepaste beheerscenario uniek is voor uw omgeving of voor een aangepaste management ervaring voor uw klanten.  Dit artikel bevat een overzicht van de verschillende opties voor het integreren met OMS-services en koppelingen naar artikelen gedetailleerde technische informatie verstrekt. 

## <a name="log-analytics"></a>Log Analytics
Management-gegevens die door logboekanalyse verzameld wordt opgeslagen in een opslagplaats die wordt gehost in Azure.  Alle gegevens die zijn opgeslagen in de opslagplaats is beschikbaar in het logboek van zoekopdrachten die voorzien in snel analyse op zeer grote hoeveelheden gegevens.  De Integratievereisten van uw mogelijk voor het vullen van de opslagplaats met nieuwe gegevens beschikbaar maakt voor analyse, of om gegevens in de opslagplaats om een nieuwe visualisatie of integreren met een ander management-hulpprogramma te extraheren.

Elk gegeven van gegevens in de bibliotheek is opgeslagen als een record.  Wanneer u de opslagplaats vullen, moet u gebruikers met het recordtype dat uw oplossing worden gebruikt en een beschrijving van de eigenschappen opgeven.  Wanneer u gegevens ophalen, moet u deze informatie over de gegevens waarmee u werkt.

![De OMS-opslagplaats in te vullen](media/operations-management-suite-integration/repository.png)

### <a name="populate-the-log-analytics-repository"></a>Vullen van de opslagplaats Log Analytics
Er zijn meerdere methoden voor het invullen van de OMS-opslagplaats.  De methode die u gebruikt, is afhankelijk van de factoren zoals waar de brongegevens zich bevindt, de indeling van de gegevens en welke clients die u wilt ondersteunen.  Wanneer gegevens worden opgeslagen in de opslagplaats, maken het maakt niet uit hoe deze is verzameld.

De volgende secties worden de verschillende opties voor het invullen van de OMS-opslagplaats.

#### <a name="connected-sources-and-data-sources"></a>Verbonden bronnen en gegevensbronnen
Verbonden bronnen zijn de locaties waar gegevens worden opgehaald voor de OMS-opslagplaats.  Gegevensbronnen en oplossingen wordt uitgevoerd op verbonden bronnen en definieer de specifieke gegevens die worden verzameld.  Als uw toepassing gegevens naar een van deze gegevensbronnen schrijft, kunt klikt u vervolgens u deze door het configureren van de gegevensbron.  Bijvoorbeeld, als uw toepassing Syslog-gebeurtenissen gemaakte, kunnen vervolgens zij worden verzameld door de Syslog-gegevensbron op een Linux-agent.

* [Gegevensbronnen in Log Analytics](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Oplossingen
Oplossingen voor uitbreiden de mogelijkheden van OMS.  Een oplossing kan verzamelen van gegevens van de verbonden bron of deze mogelijk analyse uitvoeren op records die al in de opslagplaats is verzameld.  Elke oplossing geleverd door Microsoft heeft een afzonderlijke artikel dat biedt de details over de gegevens die worden verzameld.

* [Oplossingen in Log Analytics](../log-analytics/log-analytics-add-solutions.md)

#### <a name="http-data-collector-api"></a>Gegevensverzamelaar voor HTTP-API
Log Analytics HTTP Data Collector API is een REST-API waarmee u JSON-gegevens toevoegen aan de opslagplaats logboekanalyse.  Wanneer u een toepassing hebt die geen gegevens via een van de andere gegevensbronnen of oplossingen biedt, kunt u deze API gebruikmaken.  Het kan worden gebruikt voor het vullen van de opslagplaats vanaf elke client die de API kan aanroepen en niet afhankelijk van de planning voor het verzamelen van een gegevensbron of oplossing.

* [Log Analytics HTTP-gegevensverzamelaar API](../log-analytics/log-analytics-data-collector-api.md)

### <a name="retrieve-data-from-the-log-analytics-repository"></a>Gegevens ophalen uit de opslagplaats Log Analytics
Er zijn meerdere methoden voor het ophalen van gegevens uit de opslagplaats OMS.  U kunt gebruikers wilt ophalen van gegevens met behulp van de OMS-console en hen voorzien van verschillende soorten visualisaties en analyse.  U kunt ook de gegevens ophalen uit een extern proces, zoals een andere oplossing voor het beheer.

#### <a name="log-searches"></a>Logboek zoekopdrachten
Alle gegevens die zijn opgeslagen in de OMS-opslagplaats is beschikbaar via logboek zoekopdrachten.  Gebruikers kunnen hun eigen ad hoc analyse uitvoeren in de OMS-console of maak een dashboard met een visualisatie in voor een bepaald logboek zoekopdracht.  Oplossingen kunnen aangepaste weergaven met visualisaties op basis van vooraf gedefinieerde zoekopdrachten bevatten.  Kunt u de Search-API van het logboek voor toegang tot gegevens in de OMS-opslagplaats uit een externe toepassing of management-hulpprogramma.  

* [Logboek van zoekopdrachten in Log Analytics](../log-analytics/log-analytics-log-searches.md)
* [Log Analytics logboek search REST-API](../log-analytics/log-analytics-log-search-api.md)
* [Log Analytics-cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)

#### <a name="custom-views"></a>Aangepaste weergaven
De ontwerper kunt u aangepaste weergaven maken in de OMS-console die gebruikers voorzien van visualisaties en analyse van de gegevens in uw oplossing.  Elke weergave bevat een tegel die wordt weergegeven op de hoofdpagina van de console en een willekeurig aantal visualisatie delen die zijn gebaseerd op het logboek van zoekopdrachten die u definieert.

* [Log Analytics-ontwerper](../log-analytics/log-analytics-view-designer.md)

#### <a name="power-bi"></a>Power BI
Log Analytics kunt automatisch gegevens exporteren uit de OMS-opslagplaats in Power BI zodat u van de vorm van visualisaties en hulpmiddelen voor analyse gebruikmaken kunt.  Hiermee deze export worden uitgevoerd volgens een schema zodat de gegevens is actueel. 

* [Log Analytics-gegevens exporteren naar Power BI](../log-analytics/log-analytics-powerbi.md)

## <a name="automation"></a>Automatisering
OMS kunt automatiseren processen om te reageren op de verzamelde gegevens of naar andere beheerfuncties uitvoeren.  Kan het verzamelen van gegevens van uw toepassing en plaats deze in de OMS-opslagplaats of u de correctie van een bekend probleem in reactie op gegevens in de opslagplaats gevonden kan automatiseren. 

![Automatisering](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks
PowerShell-scripts en werkstromen van Runbooks in Azure Automation worden uitgevoerd in de Azure-cloud.  U kunt deze gebruiken voor het beheren van resources in Azure of andere bronnen die toegankelijk zijn vanuit de cloud.  Runbooks kunnen ook worden uitgevoerd in een lokaal datacenter met Hybrid Runbook Worker.  U kunt een runbook starten vanuit de Azure-portal of vanuit externe processen met behulp van een aantal methoden zoals PowerShell of de Automation-API.

* [Een runbook starten in Azure Automation](../automation/automation-starting-a-runbook.md)
* [Azure Automation-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx)
* [REST-API voor Automation](https://msdn.microsoft.com/library/mt662285.aspx)
* [Automation .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Waarschuwingen
Regels voor waarschuwingen automatisch logboek zoekopdrachten volgens een schema uitgevoerd.  Als de resultaten aan bepaalde criteria voldoen kan de resulterende waarschuwing starten van een runbook in Azure Automation of aanroepen van een webhook die met een extern proces kan starten.  Beide van deze antwoorden kunnen details van de waarschuwing met inbegrip van de gegevens die worden geretourneerd in de zoekopdracht logboek opnemen.

* [Waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts.md)
* [Waarschuwing log Analytics-API](../log-analytics/log-analytics-api-alerts.md)

## <a name="backup-and-site-recovery"></a>Back-up en siteherstel
Azure back-up- en Site Recovery-services bieden voor uw Ondernemingsgegevens beveiligen en de beschikbaarheid van servers en toepassingen.  U kunt gebruikmaken van deze services om uit te voeren van scenario's zoals back-up bieden voor uw toepassing of u een failover van een virtuele machine start.

* [Azure Backup-cmdlets](https://msdn.microsoft.com/library/mt619253.aspx)
* [Azure Site Recovery REST-API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
* [Azure Site Recovery-Cmdlets](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Aangepaste oplossingen
U kunt integratie logica verpakken in een aangepaste oplossing in uw werkruimte of in de werkruimte van de klant uit te voeren.  Uw oplossing kan elk van de methoden integratie in dit artikel naast de andere bronnen voor een volledige beheerscenario bevatten.  De resources in de oplossing zijn verpakt zo dat wanneer de oplossing wordt verwijderd, worden alle resources die deze gemaakt verwijderd uit de OMS-werkruimte en de Azure-abonnement.

Uw oplossing kan bijvoorbeeld een Automation-runbook om te verzamelen en gegevens verwerken en vervolgens de Log Analytics-opslagplaats met behulp van de API van HTTP-Data Collector te vullen.  U kunt ook een aangepaste weergave die geeft en analyseert de verzamelde gegevens opnemen.  

* Maken van aangepaste oplossingen (binnenkort)    

## <a name="next-steps"></a>Volgende stappen
* Verwijzing naar de [OMS SDK](operations-management-suite-sdk.md) voor technische informatie over het automatiseren van OMS-services.  

