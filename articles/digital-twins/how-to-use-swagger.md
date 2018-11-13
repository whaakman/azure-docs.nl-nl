---
title: Meer informatie over het gebruik van Azure digitale dubbele Swagger | Microsoft Docs
description: Gebruik Azure digitale dubbele Swagger
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: adgera
ms.openlocfilehash: 0fae059db53668da2d71906bbe94e9dabae931bc
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578583"
---
# <a name="use-azure-digital-twins-swagger"></a>Gebruik Azure digitale dubbele Swagger

Elke ingerichte Azure digitale Twins-instantie bevat een eigen automatisch gegenereerde Swagger-referentiedocumentatie.

[Swagger](https://swagger.io/), of [OpenAPI](https://www.openapis.org/), koppelt zijn complexe API-informatie in een resource voor de interactieve en taal-neutraal verwijzing netwerk. Swagger bevat essentiële referentiemateriaal over de JSON-nettoladingen, HTTP-methoden, en specifieke te gebruiken voor het uitvoeren van bewerkingen op een API-eindpunten.

> [!IMPORTANT]
> Ondersteuning voor Swagger-verificatie is tijdelijk uitgeschakeld tijdens de openbare Preview.

## <a name="swagger-summary"></a>Samenvatting van swagger

Swagger biedt een interactieve samenvatting van uw API, waaronder:

* API- en modelgegevens.
* REST API-eindpunten die de vereiste aanvraag nettoladingen, headers, parameters, context paden en HTTP-methoden opgeeft.
* Testen van de API-functies.
* Voorbeeld van de informatie die wordt gebruikt om te valideren en controleer of HTTP-antwoorden.
* Informatie over foutcodes.

Swagger is een handig hulpmiddel om u te helpen bij het ontwikkelen en testen oproepen naar de API Management.

> [!TIP]
> Een voorbeeld van Swagger eerst wordt geleverd ter illustratie van de API-functie instellen.
> Deze wordt gehost op [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

U kunt toegang tot uw eigen gegenereerde Swagger-Management-API-documentatie op:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Aangepaste naam | Vervangen door |
| --- | --- |
| NaamExemplaar | De naam van uw Azure digitale Twins-exemplaar |
| yourLocation | Welke regio u uw exemplaar wordt gehost op server |

## <a name="reference-material"></a>Naslagmateriaal

Referentiemateriaal voor automatisch gegenereerde wordt uitgelegd belangrijke concepten en objectmodellen.

Een beknopt overzicht beschrijft de API.

![Swagger-bovenaan][1]

Core API objectmodellen worden ook weergegeven.

![Swagger-modellen][2]

U kunt elke vermelde objectmodel voor een meer gedetailleerd overzicht van belangrijke kenmerken selecteren.

![Swagger-model][3]

De gegenereerde Swagger-objectmodellen zijn handig om te zien van alle beschikbare Azure digitale dubbels [objecten en API's](./concepts-objectmodel-spatialgraph.md). Ontwikkelaars kunnen maken wanneer ze bij het bouwen van oplossingen op Azure digitale dubbels het gebruik van deze resource.

## <a name="endpoint-summary"></a>Samenvatting van eindpunt

Swagger bevat ook een uitgebreide overzicht van alle eindpunten waaruit de API.

Elke vermelde eindpunt omvat ook de informatie vereist aanvraag, zoals de:

* Vereiste parameters.
* Vereiste parametergegevenstypen.
* HTTP-methode voor toegang tot de resource.

![Swagger-eindpunten][4]

Selecteer elke resource een gedetailleerder overzicht.

## <a name="use-swagger-to-test-endpoints"></a>Gebruik Swagger om te testen eindpunten

Een van de krachtige functies van die swagger biedt is de mogelijkheid voor het testen van een API-eindpunt rechtstreeks via de UI-documentatie.

Nadat u een bepaald eindpunt selecteert, ziet u **Try it out in**.

![Probeer swagger][5]

Vouw deze sectie om de invoervelden voor elke vereiste en optionele parameter. Voer de waarden dienovereenkomstig en selecteer **Execute**.

![Er is geprobeerd swagger][6]

Nadat u de test uitvoert, kunt u de antwoordgegevens valideren.

## <a name="swagger-response-data"></a>Antwoordgegevens voor swagger

Elke vermelde eindpunt bevat ook de hoofdtekst van de antwoordgegevens voor het valideren van de ontwikkeling en tests. Deze voorbeelden zijn onder meer de statuscodes en JSON die u wilt zien voor geslaagde HTTP-aanvragen.

![Swagger-antwoord][7]

De voorbeelden zijn ook foutcodes zodat fouten opspoort of mislukt tests verbeteren.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0-autorisatie

Zie voor meer informatie over het testen van interactief aanvragen die zijn beveiligd met OAuth 2.0, de [officiële documentatie](https://swagger.io/docs/specification/authentication/oauth2/).

> [!NOTE]
> Ondersteuning voor OAuth 2.0-verificatie is tijdelijk uitgeschakeld tijdens de openbare Preview.

## <a name="next-steps"></a>Volgende stappen

Lees meer informatie over Azure digitale dubbels objectmodellen en de grafiek ruimtelijke intelligence lezen [digitale dubbels Azure begrijpen object modellen](./concepts-objectmodel-spatialgraph.md).

Lees voor meer informatie over verificatie met uw API Management, [verifiëren met API's](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG
