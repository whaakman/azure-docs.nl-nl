---
title: API-overwegingen | Azure Marketplace
description: Versiebeheer, foutafhandeling en autorisatie problemen bij het gebruik van de marketplace API's.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 6bf27db27daee50f78552344ae1b2b116d48a5c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935579"
---
# <a name="api-considerations"></a>API-overwegingen


<a name="api-versioning"></a>API-versiebeheer
--------------

Mogelijk zijn er meerdere versies van de API die beschikbaar op hetzelfde moment zijn. Clients dient aan te geven welke versie ze willen gebruiken aanroepen door te geven de `api-version` parameter als onderdeel van de query-tekenreeks.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

Het antwoord op een aanvraag met een onbekende of ongeldige API-versie is een HTTP-code 400. Deze fout wordt de verzameling van bekende API-versies in de hoofdtekst van antwoord geretourneerd.

``` json
    {
        "error”: { 
            "code":"InvalidAPIVersion",
            "message":"Invalid api version. Allowed values are [2016-08-01-preview]"
        }
    }
```            

<a name="errors"></a>Fouten
------

De API reageert op fouten met de bijbehorende HTTP-statuscodes en, optioneel, als u meer informatie in het antwoord dat is geserialiseerd als JSON.
Wanneer u een foutbericht, met name een 400-klasse-fout ontvangt, probeer niet opnieuw de aanvraag voor het oplossen van de onderliggende oorzaak. Bijvoorbeeld, in het bovenstaande voorbeeldantwoord Herstel dit de API-versie-parameter alvorens de aanvraag.

<a name="authorization-header"></a>Autorisatie-header
--------------------

Voor alle API's in deze referentie, moet u de autorisatie-header, samen met het bearer-token ophalen uit Azure Active Directory (Azure AD) doorgeven. Deze header is vereist voor het ontvangen van een geldige reactie; Als deze niet aanwezig is, een `401 Unauthorized` fout wordt geretourneerd. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
