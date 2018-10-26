---
title: Informatie over het gebruik van Azure digitale dubbele Swagger | Microsoft Docs
description: Het gebruik van Azure digitale dubbele Swagger
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: adgera
ms.openlocfilehash: 3bc365c204ab75a2f136c3e26c4b598b25f66114
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093486"
---
# <a name="how-to-use-azure-digital-twins-swagger"></a>Het gebruik van Azure digitale dubbele Swagger

Elke ingerichte Azure digitale Twins-instantie bevat een eigen automatisch gegenereerde Swagger-referentiedocumentatie.

[Swagger](https://swagger.io/) (of [OpenAPI](https://www.openapis.org/)) koppelt zijn complexe API-informatie in een resource voor de interactieve en taal-neutraal verwijzing netwerk. Swagger bevat specifiek, kritieke referentiemateriaal over de JSON-nettoladingen, HTTP-methoden, en specifieke te gebruiken voor het uitvoeren van bewerkingen op een API-eindpunten.

## <a name="swagger-summary"></a>Samenvatting van swagger

Swagger biedt een interactieve samenvatting van uw API, waaronder:

* Informatie over API- en -objectmodel.
* REST API-eindpunten op te geven vereist aanvraag nettoladingen, headers, parameters, context paden en HTTP-methoden.
* Testen van de API-functies.
* Voorbeeld van de antwoord-informatie voor het valideren en bevestigen van HTTP-antwoorden.
* Informatie over foutcodes.

Swagger is daarom een handig hulpmiddel om u te helpen bij het ontwikkelen en testen oproepen naar de API Management.

> [!TIP]
> Ter referentie, wordt een Swagger eerst in preview verstrekt ter illustratie van de API-functie instellen.
> Deze wordt gehost op [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

U kunt toegang tot uw eigen, gegenereerde, beheer-API Swagger-documentatie op:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *NaamExemplaar* | De naam van uw Azure digitale Twins-exemplaar |
| *yourLocation* | Welke regio u uw exemplaar wordt gehost op server |

## <a name="reference-material"></a>Naslagmateriaal

Referentiemateriaal voor automatisch gegenereerde wordt uitgelegd belangrijke concepten en objectmodellen.

Een beknopt overzicht van de API wordt beschreven:

![Swagger-bovenaan][1]

Core API objectmodellen worden ook vermeld:

![Swagger-modellen][2]

U kunt klikken in elke vermelde objectmodel voor een meer gedetailleerd overzicht van belangrijke kenmerken:

![Swagger-model][3]

De gegenereerde Swagger objectmodellen zijn handig om te zien van alle beschikbare Azure digitale dubbels [objecten en API's](./concepts-objectmodel-spatialgraph.md). Het is een fantastische bron voor ontwikkelaars kunnen gebruiken bij het bouwen van oplossingen op Azure digitale dubbels.

## <a name="endpoint-summary"></a>Samenvatting van eindpunt

Swagger bevat ook een uitgebreide overzicht van alle eindpunten waaruit de API.

Elke vermelde eindpunt bevat ook de vereiste aanvraag informatie zoals:

* Vereiste parameters.
* Vereiste parametergegevenstypen.
* De HTTP-methode voor toegang tot de resource.

![Swagger-eindpunten][4]

Elke resource kan worden geklikt om te zien van een gedetailleerder overzicht.

## <a name="using-swagger-to-test-endpoints"></a>Met behulp van Swagger voor het testen van eindpunten

Is een van de krachtige functies van Swagger biedt de mogelijkheid tot **Try it out in** of voor het testen van een API-eindpunt rechtstreeks via de UI-documentatie.

Nadat u hebt geklikt in een bepaald eindpunt, ziet u een **Try it out in** knop:

![Probeer swagger][5]

Deze sectie uitvouwen wordt invoervelden voor elke vereiste en optionele parameter. De waarden dienovereenkomstig invoeren en op **Execute**:

![Er is geprobeerd swagger][6]

Nadat de test is uitgevoerd, kunt u de antwoordgegevens valideren.

## <a name="swagger-response-data"></a>Antwoordgegevens voor swagger

Elke vermelde eindpunt bevat ook de hoofdtekst van de antwoordgegevens voor het valideren van de ontwikkeling en tests. Deze voorbeelden zijn onder meer de gewenste-statuscodes en JSON voor geslaagde HTTP-aanvragen.

![Swagger-antwoord][7]

De voorbeelden zijn ook foutcodes zodat fouten opspoort of mislukt tests verbeteren.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0-autorisatie

Als u wilt testen interactief aanvragen op basis van de API-resources die zijn beveiligd met OAuth 2.0, Zie de [officiële documentatie](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure digitale dubbels objectmodellen en ruimtelijke intelligence graph lezen [in dit artikel](./concepts-objectmodel-spatialgraph.md).

Lees voor meer informatie over verificatie met uw API Management, [verificatie met API's](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG
