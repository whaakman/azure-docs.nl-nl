---
title: Een aanbieding publiceren | Azure Marketplace
description: API voor het publiceren van de opgegeven aanbieding.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 117a4e5e238e754524ff813ce25ebc1105e2153c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64934984"
---
<a name="publish-an-offer"></a>Een aanbieding publiceren
================

Start het publicatieproces voor de opgegeven aanbieding. Deze aanroep is een langdurige bewerking.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

<a name="uri-parameters"></a>URI-parameters
--------------

|  **Naam**      |    **Beschrijving**                               |  **Gegevenstype** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Uitgever-ID, bijvoorbeeld `contoso`      |   String       |
|  offerId       | Aanbiedings-id                                 |   String       |
|  API-versie   | Meest recente versie van de API                        |   Date         |
|  |  |


<a name="header"></a>Header
------

|  **Naam**        |    **Waarde**          |
|  --------        |    ---------          |
|  Content-Type    | `application/json`    |
|  Autorisatie   |  `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="request"></a>Aanvraag

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>Eigenschappen van de aanvraag hoofdtekst

|  **Naam**               |   **Beschrijving**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  e-mailmeldingen    | Door komma's gescheiden lijst met e-mailadressen om te worden geïnformeerd over de voortgang van de publicatiebewerking. |
|  |  |


### <a name="response"></a>Antwoord

   `Operation-Location: /api/operations/contoso$56615b67-2185-49fe-80d2-c4ddf77bb2e8$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>Antwoordkoptekst

|  **Naam**             |    **Waarde**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Operation-Location    | De URL die kan worden doorzocht om te bepalen van de huidige status van de bewerking.    |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code** |  **Beschrijving**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted` -De aanvraag is geaccepteerd. Het antwoord bevat een locatie die kan worden gebruikt voor het bijhouden van de bewerking die is gestart. |
| 400   | `Bad/Malformed request` -De antwoordtekst voor fout kan bevatten meer informatie.                                                               |
| 422   | `Un-processable entity` -Geeft aan dat de entiteit moet worden gepubliceerd niet valideren.                                                        |
| 404   | `Not found` -De entiteit die is opgegeven door de client bestaat niet.                                                                              |
|  |  |
