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
ms.openlocfilehash: ea3f26d70c4a4ce07c988612890687504a4cf5ac
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594675"
---
<a name="go-live"></a>Go Live
=======

Deze API begint het proces voor het pushen van een app naar productie. Met deze bewerking wordt meestal langlopende. Deze aanroep maakt gebruik van de meldingenlijst e-mailadres van de [publiceren](./cloud-partner-portal-api-publish-offer.md) API-bewerking.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>URI-parameters
--------------

|  **Naam**      |   **Beschrijving**                                                           | **Gegevenstype** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Uitgever-ID voor de aanbieding moet worden opgehaald, bijvoorbeeld `contoso`       |  String       |
| offerId        | Id van de aanbieding om op te halen bieden                                   |  String       |
| API-versie    | Meest recente versie van de API                                                   |  Date         |
|  |  |  |


<a name="header"></a>Header
------

|  **Naam**       |     **Waarde**       |
|  ---------      |     ----------      |
| Content-Type    | `application/json`  |
| Autorisatie   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="response"></a>Antwoord

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Antwoordkoptekst

|  **Naam**             |      **Waarde**                                                            |
|  --------             |      ----------                                                           |
| Operation-Location    |  URL naar de query om te bepalen van de huidige status van de bewerking            |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code** |  **Beschrijving**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted` -De aanvraag is geaccepteerd. Het antwoord bevat een locatie voor het volgen van de status van de bewerking. |
|  400     | `Bad/Malformed request` -Aanvullende foutinformatie vindt u in de hoofdtekst van de reactie. |
|  404     |  `Not found` -De opgegeven entiteit bestaat niet.                                       |
|  |  |
