---
title: Live gaan | Microsoft Docs
description: De Go Live API initieert de aanbieding live aanbieding proces.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: c7d643f0c7885e64636a107d22ce332b1ba9371c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809456"
---
<a name="go-live"></a>Go Live
=======

Deze API begint het proces voor het pushen van een app naar productie. Met deze bewerking wordt meestal langlopende. Deze aanroep maakt gebruik van de meldingenlijst e-mailadres van de [publiceren](./cloud-partner-portal-api-publish-offer.md) API-bewerking.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>URI-parameters
--------------

|  **Naam**      |   **Beschrijving**                                                           | **Gegevenstype** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Uitgever-ID voor de aanbieding moet worden opgehaald, bijvoorbeeld `contoso`       |  Reeks       |
| offerId        | Id van de aanbieding om op te halen bieden                                   |  Reeks       |
| API-versie    | Meest recente versie van de API                                                   |  Date         |
|  |  |  |


<a name="header"></a>Koptekst
------

|  **Naam**       |     **Waarde**       |
|  ---------      |     ----------      |
| Inhoudstype    | `application/json`  |
| Autorisatie   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="response"></a>Antwoord

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Antwoordkoptekst

|  **Naam**             |      **Waarde**                                                            |
|  --------             |      ----------                                                           |
| Bewerking-locatie    |  URL naar de query om te bepalen van de huidige status van de bewerking            |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code** |  ** Beschrijving **                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted` -De aanvraag is geaccepteerd. Het antwoord bevat een locatie voor het volgen van de status van de bewerking. |
|  400     | `Bad/Malformed request` -Aanvullende foutinformatie vindt u in de hoofdtekst van de reactie. |
|  404     |  `Not found` -De opgegeven entiteit bestaat niet.                                       |
|  |  |
