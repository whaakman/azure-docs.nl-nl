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
ms.openlocfilehash: 16e0fc493a257504e2708336e05c30b36d4bea15
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="the-new-alerts-experience-in-azure-monitor"></a>De nieuwe waarschuwingen optreden in de Azure-Monitor

## <a name="overview"></a>Overzicht

> [!NOTE]
> In dit artikel beschrijft nieuwe waarschuwingen. Oudere klassieke Azure-waarschuwingen worden beschreven in [klassiek overzicht van waarschuwingen](monitoring-overview-alerts.md). 
>
>

Waarschuwingen heeft nieuwe ervaring. De oudere waarschuwingen ervaring nu op het tabblad waarschuwingen (klassiek is). De nieuwe ervaring van waarschuwingen heeft de volgende voordelen via de ervaring voor de waarschuwingen (klassiek):

-   **Voor een betere notification system**: alle nieuwere waarschuwingen gebruiken [actiegroepen]( https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups), die groepen meldingen en acties die opnieuw kunnen worden gebruikt in meerdere waarschuwingen worden genoemd.  Gebruik geen actiegroepen klassieke metrische waarschuwingen en oudere Log Analytics-waarschuwingen. 
- **Een uniforme ontwerpmogelijkheden** - alle waarschuwingen maken voor de metrische gegevens, logboeken en de activiteit zich aanmelden via Azure Monitor, Log Analytics en Application Insights is op één plek. 
- **Weergave gestart logboekanalyse waarschuwingen in Azure portal** -kunt u nu ook Zie logboekanalyse waarschuwingen in uw abonnement gestart. Eerder waren deze in een afzonderlijke portal. 
- **Scheiding van Fired waarschuwingen en waarschuwing regels** - waarschuwing regels (de definitie van de voorwaarde die een waarschuwing activeert) en waarschuwingen deze gebeurtenis wordt gestart (een exemplaar van de waarschuwingsregel starten) van elkaar worden onderscheiden, zodat de operationele en configuratie-weergaven worden gescheiden.
- **Betere werkstroom** : de nieuwe waarschuwingen ontwerpen handleidingen ervaring van de gebruiker het configuratieproces van een waarschuwingsregel dit het eenvoudiger om de juiste objecten maakt voor het ophalen van waarschuwingen op te detecteren.
 
Nieuwere metrische waarschuwingen hebben specifiek de volgende verbeteringen:
-   **Verbeterde latentie**: nieuwere metrische waarschuwingen zo vaak als elke één minuut kunnen uitvoeren. Oudere metrische waarschuwingen wordt altijd uitgevoerd met een frequentie van 5 minuten. Logboek waarschuwingen nog steeds een langer is dan 1 minuut vertraging vanwege de tijd vergt om op te nemen van de logboeken is. 
-   **Ondersteuning voor multidimensionale metrische gegevens**: U kunt een waarschuwing op dimensionale metrische gegevens, zodat u kunt een interessante segment van de metrische gegevens controleren.
-   **Meer controle over metrische voorwaarden**: U kunt uitgebreidere waarschuwingsregels definiëren. De nieuwere waarschuwingen bewaken van de maximale, minimale gemiddelde en totale waarden van de metrische gegevens ondersteunen.
-   **Bewaking van meerdere metrische gegevens gecombineerd**: U kunt meerdere metrische gegevens (momenteel maximaal twee metrische gegevens) met een enkele regel bewaken. Er wordt een waarschuwing gegeven als beide metrische gegevens in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
-   **Metrische gegevens uit logboeken** (beperkte openbare preview): sommige logboek bij Log Analytics-gegevens nu kunnen worden uitgepakt en geconverteerd worden naar Azure Monitor metrische gegevens en vervolgens een melding op net als andere metrische gegevens. 



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

 
## <a name="alerts-supported-in-new-experience"></a>Waarschuwingen die worden ondersteund in de nieuwe ervaring
Waarschuwingen zijn beschikbaar in verschillende Azure-services controleren. Voor meer informatie over het gebruik van deze services [Raadpleeg dit artikel](./monitoring-overview.md). Hier vindt u een uitsplitsing van de Waarschuwingstypen in Azure en wat wordt momenteel ondersteund door de nieuwe ervaring voor waarschuwingen. 


| **Signaaltype** | **Bron van de monitor** | **Beschrijving** | 
|-------------|----------------|-------------|
| Gegevens | Monitor voor Azure | Ook wel [near-realtime metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md), ze ondersteunen metrische voorwaarden zo vaak als 1 minuut evalueren en toestaan voor meerdere metrische en multidimensionale metrische regels. Er is een lijst met ondersteunde resourcetypen beschikbaar [hier](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). |
| Gegevens | Monitor voor Azure | [Oudere klassieke metrische waarschuwingen](monitoring-overview-alerts.md) worden niet ondersteund in de nieuwe ervaring voor waarschuwingen. U vindt deze onder waarschuwingen (klassiek) in de Azure portal. De klassieke waarschuwingen ondersteuning voor bepaalde typen metrische gegevens die nog niet is verplaatst naar de nieuwe waarschuwingen. Zie voor een volledige lijst [ondersteund metrische gegevens](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-supported-metrics)
| Logboeken  | Log Analytics | Meldingen ontvangen of geautomatiseerde acties uitgevoerd wanneer een zoekquery logboek over metrische gegevens en/of gebeurtenis gegevens aan bepaalde criteria voldoet. Oudere Log Analytics-waarschuwingen zijn nog steeds beschikbaar, maar zijn [wordt gekopieerd naar de nieuwe ervaring](monitoring-alerts-extend.md). Bovendien een [voorbeeld bekijken van *logboekanalyse Logboeken als metrische gegevens* ](monitoring-alerts-extend-tool.md) is beschikbaar. De preview kunt u bepaalde typen logboeken nemen en deze converteren naar metrische gegevens, waar u kunt vervolgens een melding op deze met de nieuwe ervaring voor waarschuwingen. Het voorbeeld is handig als u niet-Azure-logboeken die u wilt ophalen naast systeemeigen Azure Monitor metrische gegevens hebt. |
| Activiteitenlogboek | Activiteitenlogboeken (Algemeen) | Bevat de records van alle maken, bijwerken en verwijderen acties uitgevoerd via het geselecteerde doel (resource/resourcegroep groep /-abonnement). |
| Activiteitenlogboek  | Service Health | Niet ondersteund in de nieuwe ervaring voor waarschuwingen. Zie [activiteit logboek waarschuwingen maken op servicemeldingen](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Logboeken  | Application Insights | Logboeken met de details van de prestaties van uw toepassing bevat. Analytics-query kunt u de voorwaarden voor welke acties moeten worden genomen: op basis van de toepassingsgegevens definiëren. |
| Gegevens | Application Insights | Niet ondersteund in de nieuwe ervaring voor waarschuwingen. Zie [metrische waarschuwingen](../application-insights/app-insights-alerts.md) |
| Webtests voor beschikbaarheid | Application Insights | Niet ondersteund in de gebruikerservaring van waarschuwingen.  Zie [Web test waarschuwingen](../application-insights/app-insights-monitor-web-app-availability.md). Beschikbaar voor een website geïnstrumenteerd om gegevens te verzenden naar Application Insights. U ontvangt een melding wanneer de beschikbaarheid of reactiesnelheid van een website lager dan de verwachtingen is. |




## <a name="next-steps"></a>Volgende stappen
- [Informatie over het gebruik van de nieuwe ervaring voor waarschuwingen maken, weergeven en beheren van waarschuwingen](monitor-alerts-unified-usage.md)
- [Meer informatie over logboek waarschuwingen in de gebruikerservaring van waarschuwingen](monitor-alerts-unified-log.md)
- [Meer informatie over metrische waarschuwingen in de gebruikerservaring van waarschuwingen](monitoring-near-real-time-metric-alerts.md)
- [Meer informatie over activiteit logboek waarschuwingen in de gebruikerservaring van waarschuwingen](monitoring-activity-log-alerts-new-experience.md)
