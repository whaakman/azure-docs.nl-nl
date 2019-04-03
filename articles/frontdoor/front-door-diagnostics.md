---
title: Bewaking van metrische gegevens en Logboeken in Azure-Service voordeur | Microsoft Docs
description: Dit artikel beschrijft de verschillende metrische gegevens en Logboeken openen die ondersteuning biedt voor Azure voordeur Service
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
ms.openlocfilehash: 98aabf5330589bf80f1653bb2882c015a4bc133c
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862103"
---
# <a name="monitoring-metrics-and-logs-in-azure-front-door-service"></a>Metrische gegevens en Logboeken in Azure voordeur Service bewaken

Met behulp van Azure voordeur Service, kunt u resources kunt controleren op de volgende manieren:

- **Metrische gegevens**. Application Gateway biedt momenteel zeven metrische gegevens om prestatiemeteritems weer te geven.
- **Logs**. Activiteit- en diagnostische logboeken kunnen prestaties, toegang en andere gegevens worden opgeslagen of gebruikt vanaf een resource voor bewakingsdoeleinden.

### <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn een functie voor bepaalde Azure-resources die u kunt prestatiemeteritems bekijken in de portal. Hier volgen de beschikbare voordeur metrische gegevens:

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Dimensies | Description |
| --- | --- | --- | --- | --- |
| RequestCount | Aantal aanvragen | Count | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal aanvragen van clients geleverd door de voordeur.  |
| RequestSize | Aanvraaggrootte | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal bytes dat als aanvragen van clients naar de voordeur worden verzonden. |
| ResponseSize | Antwoordgrootte | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal bytes dat als antwoorden vanaf de voordeur aan clients worden verzonden. |
| TotalLatency | Totale latentie | Milliseconden | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | De tijd berekend op basis van de aanvraag van de client ontvangen door de voordeur totdat de client de laatste byte van de reactie van de voordeur bevestigd. |
| BackendRequestCount | Aantal back-end-aanvragen | Count | HttpStatus</br>HttpStatusGroup</br>Back-end | Het aantal aanvragen dat is verzonden vanaf de voordeur back-ends. |
| BackendRequestLatency | Latentie van aanvraag van de back-end | Milliseconden | Back-end | De tijd die wordt berekend op basis van wanneer de aanvraag is verzonden door de voordeur naar de back-end totdat de voordeur de laatste byte van de reactie ontvangen van de back-end. |
| BackendHealthPercentage | Percentage van de back-Endstatus | Procent | Back-end</br>BackendPool | Het percentage van geslaagde statusrapporten tests vanaf de voordeur back-ends. |
| WebApplicationFirewallRequestCount | Aantal Web Application Firewall-aanvragen | Count | PolicyName</br>RuleName</br>Bewerking | Het aantal aanvragen van clients verwerkt door de laag voor Toepassingsbeveiliging van voordeur. |

## <a name="activity-log"></a>Activiteitenlogboeken

Activiteitenlogboeken bieden informatie over de bewerkingen uitgevoerd op de voordeur Service. Ze bepalen ook wat, wie, en wanneer u voor alle schrijfbewerkingen (put, post of delete) die wordt gemaakt op de voordeur Service.

>[!NOTE]
>Activiteitenlogboeken bevatten geen lees (get)-bewerkingen. Ze ook niet-bewerkingen bevatten die u uitvoert met behulp van de Azure-portal of de oorspronkelijke Management-API.

Activiteitenlogboeken in uw Service voordeur of de logboeken van uw Azure-resources in Azure Monitor toegang. Activiteitenlogboeken weergeven:

1. Selecteer uw exemplaar van de voordeur.
2. Selecteer **activiteitenlogboek**.

    ![Activiteitenlogboek](./media/front-door-diagnostics/activity-log.png)

3. Kies een filteren bereik, en selecteer vervolgens **toepassen**.

## <a name="diagnostic-logging"></a>Diagnostische logboeken
Diagnoselogboeken bieden uitgebreide informatie over bewerkingen en fouten die belangrijk zijn voor controle en probleemoplossing. Diagnoselogboeken verschillen van activiteitenlogboeken.

Activiteitenlogboeken bieden inzicht in de bewerkingen uitgevoerd op Azure-resources. Diagnoselogboeken bieden inzicht in bewerkingen die de resources zelf zijn uitgevoerd. Zie voor meer informatie, [diagnostische logboeken van Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md).

![Diagnostische logboeken](./media/front-door-diagnostics/diagnostic-log.png)

Diagnostische logboeken configureren voor uw Service voordeur:

1. Selecteer uw Azure API Management-service.

2. Kies **diagnostische instellingen**.

3. Selecteer **diagnostische gegevens inschakelen**. Diagnostische logboeken samen met metrische gegevens naar een opslagaccount archiveren, ze naar een event hub streamen of ze verzenden naar Azure Monitor-Logboeken.

Voordeur Service biedt momenteel diagnostische logboeken (ingedeeld in batches per uur). Diagnoselogboeken bieden afzonderlijke API-aanvragen waarbij elk item in het volgende schema:

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

- [Maak een profiel van de voordeur](quickstart-create-front-door.md)
- [De werking van de voordeur](front-door-routing-architecture.md)
