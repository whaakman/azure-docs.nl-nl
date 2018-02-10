---
title: De nieuwe waarschuwingen (Preview)-ervaring in Azure Monitor verkennen | Microsoft Docs
description: Begrijpen hoe de nieuwe eenvoudige en schaalbare waarschuwingen optreden in Azure maakt ontwerpen, weergeven en waarschuwingen eenvoudiger beheren
author: manishsm-msft
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: mamit
ms.custom: 
ms.openlocfilehash: 316dcd53509897a6efc387749ca6f9ec268cb7ac
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="explore-the-new-alerts-preview-experience-in-azure-monitor"></a>De nieuwe waarschuwingen (Preview)-ervaring in Azure Monitor verkennen

## <a name="overview"></a>Overzicht
 Ervaring in Azure is een nieuwe waarschuwingen bekijken en bijgewerkte functionaliteit. Deze nieuwe ervaring is beschikbaar via de **waarschuwingen (Preview)** tabblad onder Azure bewaken. Hier volgen enkele van de voordelen van het gebruik van de nieuwe ervaring voor waarschuwingen (Preview):

 - **Scheiding van Fired waarschuwingen en waarschuwing regels** - waarschuwing In de ervaring Alerts(Preview) regels (de definitie van de voorwaarde die een waarschuwing activeert) en waarschuwingen deze gebeurtenis wordt gestart (een exemplaar van de waarschuwingsregel starten) van elkaar worden onderscheiden, zodat de operationele en configuratieweergaven worden gescheiden.
 - **Een uniforme ontwerpmogelijkheden voor metrische gegevens en logboekbestanden waarschuwingen** : de nieuwe waarschuwingen (Preview) ontwerpen handleidingen ervaring van de gebruiker het configuratieproces van een waarschuwingsregel dit het eenvoudiger om de juiste objecten maakt voor het ophalen van waarschuwingen op te detecteren.
 - **Weergave gestart logboekanalyse waarschuwingen in Azure portal** -ervaring In de waarschuwingen (Preview), kunt u nu ook Zie logboekanalyse waarschuwingen in uw abonnement gestart.  
 - **Uniforme ervaring voor activiteit logboek waarschuwingen** -u kunt nu de activiteit logboek waarschuwingen rechtstreeks vanuit maken **Monitor** > **waarschuwingen (Preview)**. Eerder kunt u deze alleen via **Monitor** > **activiteitenlogboek**.

De volgende secties wordt beschreven, in meer detail de werking van de nieuwe ervaring.

## <a name="taxonomy"></a>Taxonomie
De ervaring Alerts(preview) gebruikt volgende concepten voor het scheiden van de waarschuwingsregel en waarschuwing geactiveerd objecten tijdens de ontwerpmogelijkheden samen te voegen tussen de verschillende typen waarschuwingen.

- **Doelresource** -doel is een Azure-resource. Doelbron definieert het bereik en signalen die beschikbaar zijn voor waarschuwingen. Voorbeeld van doelen: een virtuele machine, een opslagaccount, een virtuele-machineschaalset, een werkruimte voor logboekanalyse of oplossing.

- **Criteria** - Criteria is een combinatie van signaal en logica wordt toegepast op een doelbron. Voorbeelden: Percentage CPU > 70%, serverreactietijd > 4 ms, aantal resultaten van een logboek query > 100 enzovoort. 

- **Signaal** - signalen worden gegenereerd door de doelbron en van verschillende typen kunnen zijn. Deze preview ondersteunt **metriek**, **activiteitenlogboek**, **Application Insights** en **logboek** als signaaltypen.

- **Logica** -gebruiker gedefinieerde logica wilt controleren of het signaal binnen bereik en-waarden verwacht.  
 
- **Actie** -specifieke gebeld verzonden naar een ontvanger van een melding (bijvoorbeeld e-mailen een adres of ze naar een webhook-URL). Meldingen kunnen meestal meerdere acties activeren. De Waarschuwingstypen in deze preview kunt ondersteuningsgroepen actie ondersteund.  
 
- **Waarschuwingsregel** -de definitie van een voorwaarde die de waarschuwing wilt activeren. In dit voorbeeld bevat waarschuwingsregel het doel en de Criteria voor waarschuwingen. De waarschuwingsregel kan zich in een ingeschakeld of de status van een uitgeschakeld.
 
- **Deze gebeurtenis wordt gestart waarschuwing** -gemaakt wanneer er een ingeschakelde waarschuwingsregel is gestart. Het gestarte waarschuwing object mag Fired of niet-omgezette status heeft.

    > [!NOTE]
    > Dit wijkt af van de huidige waarschuwingen ervaring waar de waarschuwing vertegenwoordigt de regel en de waarschuwing gestarte en kan dus een van statussen voor waarschuwing, actief of uitgeschakeld.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Één plaats is voor de waarschuwingen bekijken en beheren
Het doel van de ervaring voor de waarschuwingen (Preview) is de één plaats naar uw Azure waarschuwingen bekijken en beheren. De volgende subsecties worden de functies van elke afzonderlijke scherm van de nieuwe ervaring beschreven.

### <a name="alerts-preview-overview-page"></a>Overzichtspagina van waarschuwingen (preview)
**Monitor - waarschuwingen (Preview)** overzichtspagina geaggregeerde wordt samenvatting weergegeven van alle gestarte waarschuwingen en totaal geconfigureerd/ingeschakeld regels voor waarschuwingen. U ziet ook een lijst met alle gestarte waarschuwingen. De statistische functies wijzigen van de abonnementen of filterparameters updates en de waarschuwingen lijst wordt gestart.

> [!NOTE]
> Gestarte waarschuwingen weergegeven in waarschuwingen (Preview) zijn beperkt tot ondersteunde metrische gegevens en logboekbestanden waarschuwingen; Azure Monitor krijgt de telling van fire waarschuwingen, inclusief verbindingen in oudere Azure-waarschuwingen

 ![overzicht van waarschuwingen preview](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Waarschuwingsregels management
**Monitor - waarschuwingen (Preview) > regels** één pagina voor het beheren van alle regels voor waarschuwingen over uw Azure-abonnementen. Geeft een lijst van alle waarschuwingsregels (ingeschakeld of uitgeschakeld) en kunnen worden gesorteerd op basis van doelresources, resourcegroepen, regelnaam of status. Waarschuwingsregels kunnen ook worden uitgeschakeld/ingeschakeld en worden bewerkt via deze pagina.  

 ![waarschuwingen-preview-regels](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Één waarschuwing ervaring via alle bewaking bronnen
In de waarschuwingen (Preview)-ervaring, waarschuwingen worden geschreven in een consistente manier ongeacht de bewakingsservice of type aangegeven. Alle waarschuwingen geactiveerd en gerelateerde details zijn beschikbaar in één pagina.  
 
Een taak drie stappen voor het ontwerpen van een waarschuwing is waar de gebruiker eerst een doel voor de waarschuwing kiest gevolgd door het juiste signaal selecteren en vervolgens de logica worden toegepast op het signaal als onderdeel van de waarschuwingsregel op te geven. Deze vereenvoudigde ontwerpproces vereist niet langer dat gebruikers weten de bewaking bron- of signalen ondersteund voordat u een Azure-resource. De algemene ontwerpmogelijkheden automatisch filtert de lijst met beschikbare signalen op basis van de doelbron geselecteerd en het maken van een waarschuwing logica leidt

U kunt meer informatie over het maken van de volgende typen waarschuwingen [hier](monitor-alerts-unified-usage.md).
- Metrische waarschuwingen (bijna de Real Time metriek waarschuwingen in de huidige ervaring genoemd)
- Logboek waarschuwingen (Log Analytics)
- Logboek waarschuwingen (activiteitenlogboeken)
- Logboek waarschuwingen (Application Insights)

 

## <a name="alert-types-supported-in-this-preview"></a>Waarschuwingstypen die worden ondersteund in deze preview


| **Signaaltype** | **Bron van de monitor** | **Beschrijving** | 
|-------------|----------------|-------------|
| Gegevens | Monitor voor Azure | Aangeroepen [ **bijna de Real Time metriek waarschuwingen** ](monitoring-near-real-time-metric-alerts.md) in de huidige gebruikerservaring deze metrische waarschuwingen ondersteunen metrische voorwaarden zo vaak als 1 min. evalueren en toestaan voor meerdere metrische regels. Er is een lijst met ondersteunde resourcetypen beschikbaar [hier](monitoring-near-real-time-metric-alerts.md#what-resources-can-i-create-near-real-time-metric-alerts-for). Andere metriek waarschuwingen gedefinieerd [hier](monitoring-overview-alerts.md#alerts-in-different-azure-services) worden niet ondersteund in de ervaring voor de waarschuwingen (Preview).|
| Logboeken  | Log Analytics | Meldingen ontvangen of geautomatiseerde acties uitgevoerd wanneer een zoekquery logboek over metrische gegevens en/of gebeurtenis gegevens aan bepaalde criteria voldoet.|
| Logboeken  | Activiteitenlogboeken | Deze categorie bevat de records van alle Create, Update, en acties die worden uitgevoerd via het geselecteerde doel (resource/resourcegroep groep /-abonnement) verwijderen. |
| Logboeken  | Health service-Logboeken | Niet ondersteund in de gebruikerservaring van waarschuwingen (Preview).   |
| Logboeken  | Application Insights | Deze rubriek bevat logboeken met de details van de prestaties van uw toepassing. Analytics-query kunt u de voorwaarden voor welke acties moeten worden genomen: op basis van de toepassingsgegevens definiëren. |
| Gegevens | Application Insights | Niet ondersteund in de gebruikerservaring van waarschuwingen (Preview). |
| Beschikbaarheidstests | Application Insights | Niet ondersteund in de gebruikerservaring van waarschuwingen (Preview). |


## <a name="next-steps"></a>Volgende stappen
- [Informatie over het gebruik van de nieuwe ervaring voor waarschuwingen (Preview) te maken, weergeven en beheren van waarschuwingen](monitor-alerts-unified-usage.md)
- [Meer informatie over logboek waarschuwingen in de gebruikerservaring van waarschuwingen (Preview)](monitor-alerts-unified-log.md)
- [Meer informatie over metrische waarschuwingen in de gebruikerservaring van waarschuwingen (Preview)](monitoring-near-real-time-metric-alerts.md)
- [Meer informatie over activiteit logboek waarschuwingen in de gebruikerservaring van waarschuwingen (Preview)](monitoring-activity-log-alerts-new-experience.md)
