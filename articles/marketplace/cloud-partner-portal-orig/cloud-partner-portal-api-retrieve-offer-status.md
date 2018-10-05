---
title: Status van de aanbieding ophalen | Microsoft Docs
description: API haalt de huidige status van de aanbieding.
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
ms.openlocfilehash: 9233a5919ad86adcbb7947cd095945654ed015a7
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810011"
---
<a name="retrieve-offer-status"></a>Status van de aanbieding ophalen 
=====================

Hiermee haalt u de huidige status van de aanbieding.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

<a name="uri-parameters"></a>URI-parameters
--------------

|  **Naam**       |   **Beschrijving**                            |  **Gegevenstype** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  publisherId    | Uitgever-ID, bijvoorbeeld `Contoso`  |     Reeks     |
|  offerId        | GUID die de unieke identificatie van de aanbieding      |     Reeks     |
|  API-versie    | Meest recente versie van de API                        |     Date       |
|  |  |


<a name="header"></a>Koptekst
------

|  Naam           |  Waarde               |
|  -------------  | -------------------  |
|  Inhoudstype   |  `application/json`  |
|  Autorisatie  | `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="response"></a>Antwoord

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 day",
          "id": "displayprovision",
          "stepName": "Provisioning",
          "description": "Your virtual machine is being replicated in our production systems.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "displaypackage",
          "stepName": "Packaging and Lead Generation Registration",
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
      "notificationEmails": "jdoe@contoso.com"
  } 
```


### <a name="response-body-properties"></a>Eigenschappen van de hoofdtekst van reactie

|  **Naam**             |    **Beschrijving**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  status               | De status van de aanbieding. Zie voor een lijst van mogelijke waarden, [status van het voorstel](#offer-status) hieronder. |
|  berichten             | Matrix van berichten die zijn gekoppeld aan de aanbieding                                                    |
|  stappen                | Matrix van de stappen uit om de aanbieding via tijdens een aanbieding publiceren verloopt                      |
|  estimatedTimeFrame   | Schatting van lang die het duren zou om uit te voeren van deze stap in de aangepaste indeling                       |
|  id                   | Id van de stap                                                                         |
|  stepName             | Naam van de stap                                                                               |
|  description          | Beschrijving van de stap                                                                        |
|  status               | De status van de stap. Zie voor een lijst van mogelijke waarden, [stap status](#step-status) hieronder.    |
|  berichten             | Matrix van berichten met betrekking tot de stap                                                          |
|  processPercentage    | Percentage voltooid van de stap                                                              |
|  previewLinks         | *Op dit moment niet geïmplementeerd*                                                                    |
|  liveLinks            | *Op dit moment niet geïmplementeerd*                                                                    |
|  notificationEmails   | Door komma's gescheiden lijst met e-mailadressen om te worden geïnformeerd over de voortgang van de bewerking        |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code** |   **Beschrijving**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK` -De aanvraag is verwerkt en de huidige status van de aanbieding is geretourneerd. |
|  400     | `Bad/Malformed request` -De antwoordtekst voor fout bevat mogelijk meer informatie.                 |
|  404     | `Not found` -De opgegeven entiteit bestaat niet.                                                |
|  |  |


### <a name="offer-status"></a>Status van aanbieding

|  **Naam**                    |    **Beschrijving**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  NeverPublished              | Aanbieding is nog nooit gepubliceerd.                          |
|  NotStarted                  | Aanbieding is er nieuw en niet gestart.                            |
|  WaitingForPublisherReview   | Aanbieding wacht op goedkeuring van de uitgever.                 |
|  In uitvoering                     | Verzending van de aanbieding wordt verwerkt.                     |
|  Geslaagd                   | Verzending van de aanbieding is verwerkt.               |
|  Geannuleerd                    | Verzending van de aanbieding is geannuleerd.                           |
|  Mislukt                      | Verzending van de aanbieding is mislukt.                                 |
|  |  |


### <a name="step-status"></a>Stapstatus

|  **Naam**                    |    **Beschrijving**                           |
|  -------------------------   |  ------------------------------------------  |
|  NotStarted                  | Stap is niet gestart.                        |
|  Wordt uitgevoerd                  | Stap wordt uitgevoerd.                             |
|  WaitingForPublisherReview   | Stap wacht op goedkeuring van de uitgever.      |
|  WaitingForApproval          | Stap wacht op goedkeuring van proces.        |
|  Geblokkeerd                     | Stap is geblokkeerd.                             |
|  Afgewezen                    | Stap wordt geweigerd.                            |
|  Voltooien                    | Stap is voltooid.                            |
|  Geannuleerd                    | Stap is geannuleerd.                           |
|  |  |
