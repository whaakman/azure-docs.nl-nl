---
title: De nieuwe ervaring voor waarschuwingen in de Azure-Monitor verkennen | Microsoft Docs
description: Begrijpen hoe de nieuwe eenvoudige en schaalbare waarschuwingen optreden in Azure maakt ontwerpen, weergeven en waarschuwingen eenvoudiger beheren
author: manishsm-msft
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: mamit
ms.custom: ''
ms.openlocfilehash: c6252ab063ee074951f098e7814ea1cfec68a8b9
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="the-new-alerts-experience-in-azure-monitor"></a>De nieuwe waarschuwingen optreden in de Azure-Monitor

## <a name="overview"></a>Overzicht
Waarschuwingen heeft nieuwe ervaring. De oudere waarschuwingen ervaring nu op het tabblad waarschuwingen (klassiek is). De nieuwe ervaring van waarschuwingen heeft de volgende voordelen via de ervaring voor de waarschuwingen (klassiek):

 - **Scheiding van Fired waarschuwingen en waarschuwing regels** - waarschuwing regels (de definitie van de voorwaarde die een waarschuwing activeert) en waarschuwingen deze gebeurtenis wordt gestart (een exemplaar van de waarschuwingsregel starten) van elkaar worden onderscheiden, zodat de operationele en configuratie-weergaven worden gescheiden.
 - **Een uniforme ontwerpmogelijkheden** - alle waarschuwingen maken voor de metrische gegevens, logboeken en de activiteit zich aanmelden via Azure Monitor, Log Analytics en Application Insights is op één plek. 
 - **Weergave gestart logboekanalyse waarschuwingen in Azure portal** -kunt u nu ook Zie logboekanalyse waarschuwingen in uw abonnement gestart. Eerder waren deze in een afzonderlijke portal. 
 - **Betere werkstroom** : de nieuwe waarschuwingen ontwerpen handleidingen ervaring van de gebruiker het configuratieproces van een waarschuwingsregel dit het eenvoudiger om de juiste objecten maakt voor het ophalen van waarschuwingen op te detecteren.
 

De volgende secties wordt beschreven, in meer detail de werking van de nieuwe ervaring.

## <a name="alert-rules-terminology"></a>Waarschuwingsregels terminologie
De nieuwe ervaring voor waarschuwingen maakt gebruik van de volgende concepten voor het scheiden van de waarschuwingsregel en waarschuwing geactiveerd objecten tijdens de ontwerpmogelijkheden samen te voegen tussen de verschillende typen waarschuwingen.

- **Doelresource** -doel is een Azure-resource. Doelbron definieert het bereik en signalen die beschikbaar zijn voor waarschuwingen. Voorbeeld van doelen: een virtuele machine, een opslagaccount, een virtuele-machineschaalset, een werkruimte voor logboekanalyse of een Application Insights-resource.

- **Criteria** - Criteria is een combinatie van signaal en logica wordt toegepast op een doelbron. Voorbeelden: Percentage CPU > 70%, serverreactietijd > 4 ms, aantal resultaten van een logboek query > 100 enzovoort. 

- **Signaal** - signalen worden gegenereerd door de doelbron en van verschillende typen kunnen zijn. **Metriek**, **activiteitenlogboek**, **Application Insights**, en **logboek** ondersteunde signaaltypen zijn.

- **Logica** -gebruiker gedefinieerde logica wilt controleren of het signaal binnen bereik en-waarden verwacht.  
 
- **Actie** : een specifieke actie uitgevoerd wanneer de waarschuwing wordt geactiveerd. Bijvoorbeeld: e-mailen een e-mailadres of het aanroepen van een webhook-URL. Meerdere acties kunnen optreden wanneer een waarschuwing wordt geactiveerd. Deze waarschuwingen ondersteuning voor groepen in te grijpen.  
 
- **Waarschuwingsregel** -de voorwaarde die het activeren van de waarschuwing. De waarschuwingsregel bevat het doel en de Criteria voor waarschuwingen. De waarschuwingsregel kan zich in een ingeschakeld of de status van een uitgeschakeld.
 
    > [!NOTE]
    > Dit wijkt af van waarschuwingen (klassiek) ervaring waar de waarschuwing vertegenwoordigt de regel en de waarschuwing gestarte en kan dus een van statussen voor waarschuwing, actief of uitgeschakeld.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Één plaats is voor de waarschuwingen bekijken en beheren
Het doel van de ervaring voor de waarschuwingen is de één plaats naar uw Azure waarschuwingen bekijken en beheren. De volgende subsecties worden de functies van elke afzonderlijke scherm van de nieuwe ervaring beschreven.

### <a name="alerts-overview-page"></a>Overzichtspagina van waarschuwingen
**Monitor - waarschuwingen** overzichtspagina geaggregeerde wordt samenvatting weergegeven van alle gestarte waarschuwingen en totaal geconfigureerd/ingeschakeld regels voor waarschuwingen. U ziet ook een lijst met alle gestarte waarschuwingen. De statistische functies wijzigen van de abonnementen of filterparameters updates en de waarschuwingen lijst wordt gestart.

> [!NOTE]
> Gestarte waarschuwingen weergegeven in waarschuwingen zijn beperkt tot ondersteunde metrische gegevens en logboekbestanden activiteitswaarschuwingen; Overzicht van Azure Monitor toont het aantal gestarte waarschuwingen, inclusief verbindingen in oudere Azure-waarschuwingen

 ![overzicht van waarschuwingen](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Waarschuwingsregels management
**Monitor - waarschuwingen > regels** één pagina voor het beheren van alle regels voor waarschuwingen over uw Azure-abonnementen. Geeft een lijst van alle waarschuwingsregels (ingeschakeld of uitgeschakeld) en kunnen worden gesorteerd op basis van doelresources, resourcegroepen, regelnaam of status. Waarschuwingsregels kunnen ook worden uitgeschakeld/ingeschakeld en worden bewerkt via deze pagina.  

 ![waarschuwingen-regels](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Één waarschuwing ervaring via alle bewaking bronnen
In de ervaring van de nieuwe waarschuwingen, waarschuwingen worden geschreven in een consistente manier ongeacht de bewakingsservice of type aangegeven. Alle waarschuwingen geactiveerd en gerelateerde details zijn beschikbaar in één pagina.  
 
Een taak drie stappen voor het ontwerpen van een waarschuwing is waar de gebruiker eerst een doel voor de waarschuwing kiest gevolgd door het juiste signaal selecteren en vervolgens de logica worden toegepast op het signaal als onderdeel van de waarschuwingsregel op te geven. Deze vereenvoudigde ontwerpproces vereist niet langer dat gebruikers weten de bewaking bron- of signalen ondersteund voordat u een Azure-resource. De algemene ontwerpmogelijkheden automatisch filtert de lijst met beschikbare signalen op basis van de doelbron geselecteerd en het maken van een waarschuwing logica leidt

U kunt meer informatie over het maken van de volgende typen waarschuwingen [hier](monitor-alerts-unified-usage.md).
- Metrische waarschuwingen
- Logboek waarschuwingen (Log Analytics)
- Logboek waarschuwingen (activiteitenlogboeken)
- Logboek waarschuwingen (Application Insights)

 

## <a name="alert-types-supported"></a>Waarschuwingstypen ondersteund


| **Signaaltype** | **Bron van de monitor** | **Beschrijving** | 
|-------------|----------------|-------------|
| Gegevens | Monitor voor Azure | Ook wel [ **bijna de Real Time metriek waarschuwingen**](monitoring-near-real-time-metric-alerts.md), deze metrische waarschuwingen ondersteunen metrische voorwaarden zo vaak als 1 min. evalueren en toestaan voor meerdere metrische regels. Er is een lijst met ondersteunde resourcetypen beschikbaar [hier](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). Oudere metriek waarschuwingen gedefinieerd [hier](monitoring-overview-alerts.md#alerts-in-different-azure-services) worden niet ondersteund in de nieuwe ervaring waarschuwingen. U vindt deze onder waarschuwingen (klassiek)|
| Logboeken  | Log Analytics | Meldingen ontvangen of geautomatiseerde acties uitgevoerd wanneer een zoekquery logboek over metrische gegevens en/of gebeurtenis gegevens aan bepaalde criteria voldoet.|
| Activiteitenlogboek | Activiteitenlogboeken | Deze categorie bevat de records van alle Create, Update, en acties die worden uitgevoerd via het geselecteerde doel (resource/resourcegroep groep /-abonnement) verwijderen. |
| Logboeken  | Health service-Logboeken | Niet ondersteund in de gebruikerservaring van waarschuwingen.   |
| Logboeken  | Application Insights | Deze rubriek bevat logboeken met de details van de prestaties van uw toepassing. Analytics-query kunt u de voorwaarden voor welke acties moeten worden genomen: op basis van de toepassingsgegevens definiëren. |
| Gegevens | Application Insights | Niet ondersteund in de gebruikerservaring van waarschuwingen. U vindt deze onder waarschuwingen (klassiek) |
| Beschikbaarheidstests | Application Insights | Niet ondersteund in de gebruikerservaring van waarschuwingen. U vindt deze onder waarschuwingen (klassiek) |


## <a name="next-steps"></a>Volgende stappen
- [Informatie over het gebruik van de nieuwe ervaring voor waarschuwingen maken, weergeven en beheren van waarschuwingen](monitor-alerts-unified-usage.md)
- [Meer informatie over logboek waarschuwingen in de gebruikerservaring van waarschuwingen](monitor-alerts-unified-log.md)
- [Meer informatie over metrische waarschuwingen in de gebruikerservaring van waarschuwingen](monitoring-near-real-time-metric-alerts.md)
- [Meer informatie over activiteit logboek waarschuwingen in de gebruikerservaring van waarschuwingen](monitoring-activity-log-alerts-new-experience.md)
