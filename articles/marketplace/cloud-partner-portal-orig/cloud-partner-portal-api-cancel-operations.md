---
title: API-bewerking annuleren | Microsoft Docs
description: Bewerkingen worden geannuleerd.
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
ms.openlocfilehash: 18f00391beded0744c80eab73bb1efe1c6ab8dbc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60625037"
---
<a name="cancel-operation"></a>Bewerking annuleren 
=================

Deze API wordt geannuleerd voor een bewerking die momenteel wordt uitgevoerd op de aanbieding. Gebruik de [operations API ophalen](./cloud-partner-portal-api-retrieve-operations.md) om op te halen een `operationId` om door te geven tot deze API. Annulering is meestal een synchrone bewerking, maar in sommige complexe scenario's mogelijk een nieuwe bewerking worden gevraagd om u te annuleren van een bestaande. In dit geval bevat de HTTP-antwoordtekst locatie van de bewerking die moet worden gebruikt om de status opvragen.

U kunt een door komma's gescheiden lijst met e-mailadressen opgeven met de aanvraag worden, en de API deze adressen over de voortgang van de bewerking.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

<a name="uri-parameters"></a>URI-parameters
--------------

|  **Naam**    |      **Beschrijving**                                  |    **Gegevenstype**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Uitgever-ID, bijvoorbeeld: `contoso`         |   String          |
| offerId      |  Aanbiedings-id                                     |   String          |
| API-versie  |  Huidige versie van API                               |    Date           |
|  |  |  |


<a name="header"></a>Header
------

|  **Naam**              |  **Waarde**         |
|  ---------             |  ----------        |
|  Content-Type          |  application/json  |
|  Autorisatie         |  Bearer-TOKEN van uw |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="request"></a>Aanvraag

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>Eigenschappen van de aanvraag hoofdtekst

|  **Naam**                |  **Beschrijving**                                               |
|  --------                |  ---------------                                               |
|  e-mailmeldingen     | Door komma's gescheiden lijst met e-mailadressen in om te worden geïnformeerd over de voortgang van de publicatiebewerking. |
|  |  |


### <a name="response"></a>Antwoord

  `Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Antwoordkoptekst

|  **Naam**             |    **Waarde**                       |
|  ---------            |    ----------                      |
| Operation-Location    | URL, om te bepalen van de huidige status van de bewerking kan worden opgevraagd. |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code**  |  **Beschrijving**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | OK. De aanvraag is verwerkt en de bewerking synchroon is geannuleerd. |
|  202      | Geaccepteerd. De aanvraag is verwerkt en de bewerking wordt momenteel wordt geannuleerd. Locatie van de annuleringsbewerking wordt geretourneerd in de antwoordkop. |
|  400      | Ongeldige/verkeerd ingedeeld aanvraag. De antwoordtekst voor fout kan bevatten meer informatie.  |
|  403      | Access Forbidden. De client heeft geen toegang tot de naamruimte die is opgegeven in de aanvraag. |
|  404      | Niet gevonden. De opgegeven entiteit bestaat niet. |
|  |  |
