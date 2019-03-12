---
title: Azure voordeur Service - metrische gegevens en logboekregistratie | Microsoft Docs
description: In dit artikel krijgt u inzicht in de verschillende metrische gegevens en Logboeken openen die ondersteuning biedt voor Azure voordeur Service
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: d409d451385ba1f88189b12cf372845e70a87429
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57726367"
---
# <a name="monitoring-metrics-for-front-door"></a>Metrische gegevens controleren voor de voordeur

Met behulp van Azure voordeur Service, kunt u resources kunt controleren op de volgende manieren:
* [Logs](#diagnostic-logging): Logboeken toestaan voor prestaties, toegang en andere gegevens worden opgeslagen of gebruikt vanaf een resource voor bewakingsdoeleinden.

* [Metrische gegevens](#metrics): Application Gateway biedt momenteel zeven metrische gegevens om prestatiemeteritems weer te geven.

## <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn een functie voor bepaalde Azure-resources waar u prestatiemeteritems in de portal bekijken kunt. Voor de voordeur, zijn de volgende metrische gegevens zijn beschikbaar:

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Dimensies | Description |
| --- | --- | --- | --- | --- |
| RequestCount | Aantal aanvragen | Count | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal aanvragen van clients geleverd door de voordeur.  |
| RequestSize | Aanvraaggrootte | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal bytes dat als aanvragen van clients naar de voordeur worden verzonden. |
| ResponseSize | Antwoordgrootte | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal bytes dat als antwoorden vanaf de voordeur aan clients worden verzonden. |
| TotalLatency | Totale latentie | Milliseconden | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | De tijd die wordt berekend op basis van wanneer de clientaanvraag is ontvangen door voordeur totdat de client de laatste byte van de reactie van de voordeur bevestigd. |
| BackendRequestCount | Aantal back-end-aanvragen | Count | HttpStatus</br>HttpStatusGroup</br>Back-end | Het aantal aanvragen dat is verzonden vanaf de voordeur back-ends. |
| BackendRequestLatency | Latentie van aanvraag van de back-end | Milliseconden | Back-end | De tijd die wordt berekend op basis van wanneer de aanvraag is verzonden door de voordeur naar de back-end totdat de voordeur de laatste byte van de reactie ontvangen van de back-end. |
| BackendHealthPercentage | Percentage van de back-Endstatus | Procent | Back-end</br>BackendPool | Het percentage van geslaagde statusrapporten tests vanaf de voordeur back-ends. |
| WebApplicationFirewallRequestCount | Aantal Web Application Firewall-aanvragen | Count | PolicyName</br>RuleName</br>Bewerking | Het aantal aanvragen van clients verwerkt door de laag voor Toepassingsbeveiliging van voordeur. |

## <a name="activity-log"></a>Activiteitenlogboeken

Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw voordeur. Met activiteitenlogboeken kunt u bepalen de ' wat, wie, en wanneer ' voor (PUT, POST, DELETE schrijfbewerkingen) die wordt gemaakt op de voordeur.

> [!NOTE]
> Activiteitenlogboeken bevatten geen lees-bewerkingen (GET) of bewerkingen die zijn uitgevoerd in Azure Portal of met behulp van de oorspronkelijke beheer-API's.

U kunt activiteitenlogboeken in uw voordeur toegang of toegang tot logboeken van al uw Azure-resources in Azure Monitor. 

Activiteitenlogboeken weergeven:

1. Selecteer uw exemplaar van de voordeur.
2. Klik op **Activiteitenlogboek**.

    ![activiteitenlogboek](./media/front-door-diagnostics/activity-log.png)

3. Selecteer het gewenste filterbereik en klik op **Toepassen**.

## <a name="diagnostic-logging"></a>Diagnostische logboeken
Diagnoselogboeken bieden uitgebreide informatie over bewerkingen en fouten die belangrijk zijn voor zowel controles als het oplossen van problemen. Diagnoselogboeken verschillen van activiteitenlogboeken. Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure-resources. Diagnoselogboeken bieden inzicht in bewerkingen die door de resources zelf zijn uitgevoerd. Meer informatie over [diagnostische logboeken van Azure Monitor](..\azure-monitor\platform\diagnostic-logs-overview.md). 

Diagnostische logboeken configureren voor uw voordeur:

1. Selecteer uw exemplaar van de APIM-service.
2. Klik op **Diagnostische instellingen**.

    ![diagnostische logboeken](./media/front-door-diagnostics/diagnostic-log.png)

3. Klik op **Diagnostische gegevens inschakelen**. U kunt diagnostische logboeken samen met metrische gegevens naar een opslagaccount archiveren, ze naar een Event Hub streamen of ze naar Azure Monitor-logboeken verzenden. 

Azure voordeur Service biedt momenteel diagnostische logboeken (ingedeeld in batches per uur) over afzonderlijke API-aanvragen waarbij elk item in het volgende schema:

| Eigenschap  | Description |
| ------------- | ------------- |
| ClientIp | Het IP-adres van de client die de aanvraag heeft ingediend. |
| ClientPort | De IP-poort van de client die de aanvraag heeft ingediend. |
| HttpMethod | HTTP-methode die wordt gebruikt door de aanvraag. |
| HttpStatusCode | De HTTP-statuscode is geretourneerd door de proxy. |
| HttpStatusDetails | De resulterende status van de aanvraag. Betekenis van deze tekenreekswaarde kan worden gevonden op een tabel met de Status. |
| HttpVersion | Het type van de aanvraag of de verbinding. |
| RequestBytes | De grootte van het bericht van de HTTP-aanvraag in bytes, met inbegrip van de aanvraagheaders en hoofdtekst van de aanvraag. |
| RequestUri | De URI van de aanvraag ontvangen. |
| ResponseBytes | Bytes verzonden door de back-endserver als het antwoord.  |
| RoutingRuleName | De naam van de regel voor doorsturen die aan de aanvraag. |
| SecurityProtocol | De versie van het TLS/SSL-protocol gebruikt door de aanvraag of null als er geen versleuteling. |
| timeTaken | De hoeveelheid tijd die de actie heeft uitgevoerd, in milliseconden. |
| UserAgent | Het browsertype dat de client heeft gebruikt |
| TrackingReference | De unieke referentie-tekenreeks die een aanvraag die is geleverd door de voordeur, identificeert ook als de header X-Azure-verwijzing naar de client verzonden. Vereist voor het zoeken naar gegevens in de logboeken openen voor een specifieke aanvraag. |

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).
