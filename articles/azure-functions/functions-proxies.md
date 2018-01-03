---
title: Werken met proxy's in Azure Functions | Microsoft Docs
description: Overzicht van het gebruik van Azure Functions-proxy 's
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: alkarche
ms.openlocfilehash: dd022b189783f2d8c6209a6cd656704ff144bfd6
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="work-with-azure-functions-proxies"></a>Werken met Azure Functions-proxy 's

In dit artikel wordt uitgelegd hoe configureren en werken met Azure Functions-proxy's. Met deze functie kunt u eindpunten op de functie-app die zijn geïmplementeerd door een andere resource. U kunt deze proxy's gebruiken om een grote API in meerdere functie-apps (zoals in een microservice-architectuur), terwijl u nog steeds één API-gebied voor clients.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE] 
> Standaardfuncties facturering geldt voor proxy-uitvoeringen. Zie voor meer informatie [prijzen van Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

## <a name="create"></a>Maken van een proxy

Deze sectie wordt beschreven hoe u een proxy in de portal functies maken.

1. Open de [Azure-portal], en gaat u naar de functie-app.
2. Selecteer in het linkerdeelvenster **nieuwe proxy**.
3. Geef een naam voor de proxy.
4. Het eindpunt dat wordt weergegeven op deze functie-app configureren door te geven de **Routesjabloon** en **HTTP-methoden**. Deze parameters zich gedragen volgens de regels voor [HTTP-triggers].
5. Stel de **back-end-URL** naar een ander eindpunt. Dit eindpunt kan een functie in een andere functie-app, of andere API kan zijn. De waarde niet hoeft te worden statische en deze kan verwijzen naar [toepassingsinstellingen] en [parameters van de aanvraag van de oorspronkelijke client].
6. Klik op **Create**.

De proxy bestaat nu als een nieuw eindpunt op functie-app. Vanuit het clientperspectief van een is het equivalent zijn aan een HttpTrigger in Azure Functions. U kunt uw nieuwe proxy uitproberen door te kopiëren van de Proxy-URL en testen van het met uw favoriete HTTP-client.

## <a name="modify-requests-responses"></a>Aanvragen en antwoorden wijzigen

U kunt met Azure Functions-proxy's, aanvragen en antwoorden van de back-end wijzigen. Deze transformaties kunnen variabelen gebruiken zoals gedefinieerd in [variabelen gebruiken].

### <a name="modify-backend-request"></a>De back-end-aanvraag wijzigen

Standaard wordt de back-end-aanvraag geïnitialiseerd als een kopie van de oorspronkelijke aanvraag. Naast het instellen van de back-end-URL, kunt u wijzigingen aanbrengt aan de HTTP-methode, kopteksten en queryreeksparameters. De gewijzigde waarden kunnen verwijzen naar [toepassingsinstellingen] en [parameters van de aanvraag van de oorspronkelijke client].

Er is momenteel geen portal ervaring voor het wijzigen van de back-end-aanvragen. Voor informatie over het toepassen van deze mogelijkheid van *proxies.json*, Zie [definiëren van een object requestOverrides].

### <a name="modify-response"></a>Het antwoord worden gewijzigd

Het antwoord van de client wordt standaard geïnitialiseerd als een kopie van het back-end-antwoord. U kunt wijzigingen aanbrengen statuscode, reden, kopteksten en hoofdtekst van het antwoord. De gewijzigde waarden kunnen verwijzen naar [toepassingsinstellingen], [parameters van de aanvraag van de oorspronkelijke client], en [parameters uit het antwoord van de back-end].

Er is momenteel geen portal ervaring voor het wijzigen van antwoorden. Voor informatie over het toepassen van deze mogelijkheid van *proxies.json*, Zie [definiëren van een object responseOverrides].

## <a name="using-variables"></a>Variabelen gebruiken

De configuratie voor een proxy hoeft niet statisch. U kunt het gebruik van variabelen in de aanvraag van de oorspronkelijke client, de back-end antwoord of toepassingsinstellingen voorwaarde.

### <a name="request-parameters"></a>Aanvraagparameters verwijzing

U kunt de aanvraagparameters gebruiken als invoer voor de eigenschap van de back-end-URL of als onderdeel van het wijzigen van aanvragen en antwoorden. Sommige parameters kunnen afhankelijk zijn van de Routesjabloon die opgegeven in de base proxyconfiguratie en anderen kunnen afkomstig zijn uit de eigenschappen van de binnenkomende aanvraag.

#### <a name="route-template-parameters"></a>Route-Sjabloonparameters
Parameters die worden gebruikt in de Routesjabloon zijn beschikbaar voor worden verwezen met de naam. De parameternamen moeten tussen accolades ({}).

Bijvoorbeeld, als een proxy heeft een Routesjabloon zoals `/pets/{petId}`, de URL van de back-end kan de waarde van bevatten `{petId}`als in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Als het Routesjabloon wordt beëindigd in een jokerteken, zoals `/api/{*restOfPath}`, de waarde `{restOfPath}` is een tekenreeksrepresentatie van de resterende padsegmenten van de binnenkomende aanvraag.

#### <a name="additional-request-parameters"></a>Aanvullende aanvraagparameters
Naast de sjabloonparameters route kunnen configuratiewaarden in de volgende waarden worden gebruikt:

* **{request.method}** : De HTTP-methode die wordt gebruikt op de oorspronkelijke aanvraag.
* **{request.headers. \<HeaderName is opgeslagen\>}**: een header die kan worden gelezen vanaf de oorspronkelijke aanvraag. Vervang  *\<HeaderName is opgeslagen\>*  met de naam van de header die u wilt lezen. Als de header niet in de aanvraag opgenomen is, wordt de waarde een lege tekenreeks zijn.
* **{request.querystring. \<ParameterName\>}**: een queryreeksparameter opgeven die kan worden gelezen vanaf de oorspronkelijke aanvraag. Vervang  *\<ParameterName\>*  met de naam van de parameter die u wilt lezen. Als de parameter niet in de aanvraag opgenomen is, wordt de waarde een lege tekenreeks zijn.

### <a name="response-parameters"></a>Verwijzing naar back-end antwoord parameters

Antwoord parameters kunnen worden gebruikt als onderdeel van het wijzigen van het antwoord op de client. De volgende waarden kunnen worden gebruikt in configuratiewaarden:

* **{backend.response.statusCode}** : De HTTP-statuscode die op de back-end-antwoord wordt geretourneerd.
* **{backend.response.statusReason}** : De HTTP-reden dat wordt geretourneerd van het back-end-antwoord.
* **{backend.response.headers. \<HeaderName is opgeslagen\>}**: een koptekst die kan worden gelezen uit het antwoord van de back-end. Vervang  *\<HeaderName is opgeslagen\>*  met de naam van de header die u wilt lezen. Als de header niet in het antwoord opgenomen is, wordt de waarde een lege tekenreeks zijn.

### <a name="use-appsettings"></a>Toepassingsinstellingen-verwijzing

U kunt ook verwijzen naar [toepassingsinstellingen gedefinieerd voor de functie-app](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) door de naam van de instelling tussen procenttekens (%).

Bijvoorbeeld, een back-end-URL van *https://%ORDER_PROCESSING_HOST%/api/orders* "% ORDER_PROCESSING_HOST %" vervangen door de waarde van de instelling ORDER_PROCESSING_HOST zou hebben.

> [!TIP] 
> Toepassingsinstellingen voor back-end-hosts gebruiken wanneer u meerdere implementaties of testomgeving. Op die manier kunt u ervoor zorgen dat u altijd met de juiste back-end voor deze omgeving praten zijn.

## <a name="advanced-configuration"></a>Geavanceerde configuratie

De proxy's die u configureert, worden opgeslagen in een *proxies.json* bestand, dat in de hoofdmap van een functie app-map bevindt zich. U kunt handmatig bewerken van dit bestand en implementeren als onderdeel van uw app als u een van de [implementatiemethoden](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) die functies worden ondersteund. De functie Azure Functions-proxy's moet [ingeschakeld](#enable) het bestand moet worden verwerkt. 

> [!TIP] 
> Als u geen hebt ingesteld een van de methoden voor het implementeren, kunt u ook werken met de *proxies.json* bestand in de portal. Ga naar de functie-app, selecteer **platformfuncties**, en selecteer vervolgens **App Service-Editor**. Door dit te doen, kunt u de volledige structuur van uw app functie weergeven en brengt wijzigingen.

*Proxies.JSON* wordt gedefinieerd door een object proxy's, die bestaan uit benoemde proxy's en de bijbehorende definities. Eventueel, als uw editor ondersteunt, kunt u verwijzen naar een [JSON-schema](http://json.schemastore.org/proxies) voor de codevoltooiing. Een voorbeeld van een bestand kan er als volgt uitzien:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Elke proxy heeft een beschrijvende naam, zoals *proxy1* in het voorgaande voorbeeld. Het bijbehorende proxy definitie van object wordt gedefinieerd door de volgende eigenschappen:

* **matchCondition**: vereist--een object voor het definiëren van de aanvragen die de uitvoering van deze proxy activeren. Deze twee eigenschappen die worden gedeeld met bevat [HTTP-triggers]:
    * _methoden_: een matrix van de HTTP-methoden die de proxy reageert op. Als deze niet is opgegeven, wordt de proxy reageert op HTTP-methoden worden op de route.
    * _route_: vereist--definieert het Routesjabloon beheren die aanvragen URL's van uw proxy reageert op. In tegenstelling tot in de HTTP-triggers is er geen standaardwaarde.
* **backendUri**: de URL van de back-end-resource waarnaar de aanvraag via proxy moet. Deze waarde kan verwijzen naar toepassingsinstellingen en parameters van de aanvraag van de oorspronkelijke client. Als deze eigenschap niet opgenomen is, wordt Azure Functions reageert met een HTTP 200 OK.
* **requestOverrides**: een object dat transformaties de back-end-aanvraag bepaalt. Zie [definiëren van een object requestOverrides].
* **responseOverrides**: een object dat transformaties aan de reactie van de client bepaalt. Zie [definiëren van een object responseOverrides].

> [!NOTE] 
> De *route* eigenschap in de Azure Functions-proxy's worden niet door de *routePrefix* eigenschap van de configuratie van de host functie-App. Als u wilt opnemen een voorvoegsel zoals `/api`, moeten worden opgenomen in de *route* eigenschap.

### <a name="requestOverrides"></a>Een object requestOverrides definiëren

Het object requestOverrides definieert wijzigingen aangebracht aan de aanvraag toen de back-end-bron wordt aangeroepen. Het object wordt gedefinieerd door de volgende eigenschappen:

* **backend.Request.Method**: de HTTP-methode die wordt gebruikt voor het aanroepen van de back-end.
* **backend.Request.QueryString. \<ParameterName\>**: een queryreeksparameter opgeven die kan worden ingesteld voor het aanroepen van de back-end. Vervang  *\<ParameterName\>*  met de naam van de parameter die u wilt instellen. Als de lege tekenreeks is opgegeven, wordt de parameter niet opgenomen in de back-end-aanvraag.
* **backend.Request.headers. \<HeaderName is opgeslagen\>**: een header die kan worden ingesteld voor het aanroepen van de back-end. Vervang  *\<HeaderName is opgeslagen\>*  met de naam van de header die u wilt instellen. Als u een lege tekenreeks opgeeft, wordt de koptekst niet opgenomen in de back-end-aanvraag.

Waarden kunnen verwijzen naar toepassingsinstellingen en parameters van de aanvraag van de oorspronkelijke client.

Een voorbeeldconfiguratie kan er als volgt uitzien:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>Een object responseOverrides definiëren

Het object requestOverrides definieert wijzigingen die zijn aangebracht aan de reactie die wordt doorgegeven aan de client. Het object wordt gedefinieerd door de volgende eigenschappen:

* **response.statusCode**: de HTTP-statuscode moet worden geretourneerd naar de client.
* **response.statusReason**: de HTTP-reden wordt geretourneerd naar de client.
* **Response.BODY**: de tekenreeksrepresentatie van de instantie moet worden geretourneerd naar de client.
* **Response.headers. \<HeaderName is opgeslagen\>**: een header die kan worden ingesteld voor het antwoord op de client. Vervang  *\<HeaderName is opgeslagen\>*  met de naam van de header die u wilt instellen. Als u een lege tekenreeks opgeeft, wordt de koptekst niet opgenomen in het antwoord.

Waarden kunnen verwijzen naar toepassingsinstellingen, de parameters van de aanvraag van de oorspronkelijke client en de parameters van het back-end-antwoord.

Een voorbeeldconfiguratie kan er als volgt uitzien:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> In dit voorbeeld wordt de antwoordtekst is ingesteld rechtstreeks, dus er is geen `backendUri` eigenschap nodig is. Het voorbeeld ziet hoe u Azure Functions-proxy's kunt gebruiken voor mocking API's.

## <a name="enable"></a>Azure Functions-proxy's inschakelen

Proxy's worden nu standaard ingeschakeld. Als u een oudere versie van de preview proxy's gebruikt en proxy's uitgeschakeld, moet u handmatig inschakelen proxy's eenmaal in de volgorde voor proxy's uit te voeren.

1. Open de [Azure-portal], en gaat u naar de functie-app.
2. Selecteer **werken app-instellingen**.
3. Switch **inschakelen Azure Functions-proxy's (preview)** naar **op**.

U kunt ook hier retourneren voor het bijwerken van de proxy-runtime, nieuwe functies beschikbaar komen.

[Azure-portal]: https://portal.azure.com
[HTTP-triggers]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
[definiëren van een object requestOverrides]: #requestOverrides
[definiëren van een object responseOverrides]: #responseOverrides
[toepassingsinstellingen]: #use-appsettings
[variabelen gebruiken]: #using-variables
[parameters van de aanvraag van de oorspronkelijke client]: #request-parameters
[parameters uit het antwoord van de back-end]: #response-parameters
