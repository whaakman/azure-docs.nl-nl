---
title: Ophalen biedt API | Microsoft Docs
description: API wordt een overzicht van aanbiedingen onder een publisher-naamruimte opgehaald.
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
ms.openlocfilehash: cf6e2fa296d3d9f152edf22685b930586b4de524
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024803"
---
<a name="retrieve-offers"></a>Biedt ophalen
===============

Hiermee haalt een overzicht van aanbiedingen onder een publisher-naamruimte.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`


<a name="uri-parameters"></a>URI-parameters
--------------

| ** De naam van **        |  **Beschrijving**                         |  **Gegevenstype** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | Uitgever-ID, bijvoorbeeld `contoso` |   Reeks    |
|  API-versie     | Meest recente versie van de API                    |    Date        |
|  |  |


<a name="header"></a>Header
------

|  **Naam**        |         **Waarde**       |
|  --------------- |       ----------------  |
|  Inhoudstype    | `application/json`      |
|  Autorisatie   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="response"></a>Antwoord

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Eigenschappen van de hoofdtekst van reactie

|  **Naam**       |       **Beschrijving**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | Geeft het type aanbieding                                                                                           |
|  publisherId    | ID die een unieke identificatie van de uitgever                                                                      |
|  status         | De status van de aanbieding. Zie voor een lijst van mogelijke waarden, [status van het voorstel](#offer-status) hieronder.                         |
|  id             | De GUID die de aanbieding in de naamruimte van de uitgever wordt aangeduid.                                                    |
|  versie        | Huidige versie van het product. De versie-eigenschap kan niet worden gewijzigd door de client. Het wordt verhoogd na elke publiceren. |
|  definitie     | Bevat een overzichtsweergave van de werkelijke definitie van de werkbelasting. Als u een uitgebreide definitie, gebruikt de [ophalen specifieke aanbiedings](./cloud-partner-portal-api-retrieve-specific-offer.md) API. |
|  changedTime    | UTC-tijd waarop de aanbieding voor het laatst is gewijzigd                                                                              |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code**  |  **Beschrijving**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK` -De aanvraag is verwerkt en de aanbiedingen onder de uitgever zijn geretourneerd naar de client.  |
|  400      | `Bad/Malformed request` -De antwoordtekst voor fout bevat mogelijk meer informatie.                                    |
|  403      | `Forbidden` -De client heeft geen toegang tot de opgegeven naamruimte.                                          |
|  404      | `Not found` -De opgegeven entiteit bestaat niet.                                                                 |
|  |  |


### <a name="offer-status"></a>Status van aanbieding

|  **Naam**                    | **Beschrijving**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | Aanbieding is nog nooit gepubliceerd.                  |
|  NotStarted                  | Aanbieding is nieuw, maar is niet gestart.                 |
|  WaitingForPublisherReview   | Aanbieding wacht op goedkeuring van de uitgever.         |
|  In uitvoering                     | Verzending van de aanbieding wordt verwerkt.             |
|  Geslaagd                   | Verzending van de aanbieding is verwerkt.       |
|  Geannuleerd                    | Verzending van de aanbieding is geannuleerd.                   |
|  Mislukt                      | Verzending van de aanbieding is mislukt.                         |
|  |  |
