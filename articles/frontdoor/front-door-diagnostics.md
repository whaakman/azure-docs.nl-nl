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
ms.openlocfilehash: 643ae03a9350868b172d99b2af7ce23e34fedf47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997995"
---
# <a name="monitoring-metrics-for-front-door"></a>Metrische gegevens controleren voor de voordeur

U kunt uw belangrijke metrische gegevens voor het valideren van de configuratie van de voordeur, samen met het gebruik, status, status en prestaties of de deur bewaken met behulp van Azure voordeur Service.

## <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn een functie voor bepaalde Azure-resources waar u prestatiemeteritems in de portal bekijken kunt. Voor de voordeur, zijn de volgende metrische gegevens zijn beschikbaar:

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Dimensies | Beschrijving |
| --- | --- | --- | --- | --- |
| RequestCount | Aantal aanvragen | Count | httpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal aanvragen van clients geleverd door de voordeur.  |
| RequestSize | Aanvraaggrootte | Bytes | httpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal bytes dat als aanvragen van clients naar de voordeur worden verzonden. |
| ResponseSize | Antwoordgrootte | Bytes | httpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Het aantal bytes dat als antwoorden vanaf de voordeur aan clients worden verzonden. |
| TotalLatency | Totale latentie | Milliseconden | httpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | De tijd die wordt berekend op basis van wanneer de clientaanvraag is ontvangen door voordeur totdat de client de laatste byte van de reactie van de voordeur bevestigd. |
| BackendRequestCount | Aantal back-end-aanvragen | Count | httpStatus</br>HttpStatusGroup</br>Back-end | Het aantal aanvragen dat is verzonden vanaf de voordeur back-ends. |
| BackendRequestLatency | Latentie van aanvraag van de back-end | Milliseconden | Back-end | De tijd die wordt berekend op basis van wanneer de aanvraag is verzonden door de voordeur naar de back-end totdat de voordeur de laatste byte van de reactie ontvangen van de back-end. |
| BackendHealthPercentage | Percentage van de back-Endstatus | Procent | Back-end</br>Hosts met de status | Het percentage van geslaagde statusrapporten tests vanaf de voordeur back-ends. |
| WebApplicationFirewallRequestCount | Aantal Web Application Firewall-aanvragen | Count | PolicyName</br>RuleName</br>Bewerking | Het aantal aanvragen van clients verwerkt door de laag voor Toepassingsbeveiliging van voordeur. |


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
- Informatie over [de werking van de voordeur](front-door-routing-architecture.md).
