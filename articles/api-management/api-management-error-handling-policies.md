---
title: Foutafhandeling in Azure API Management-beleid | Microsoft Docs
description: Informatie over het reageren op fouten die tijdens de verwerking van aanvragen in Azure API Management optreden.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: apimpm
ms.openlocfilehash: 2bde63bb668188936b3dd3cf5ecbf3b8c604eb95
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564310"
---
# <a name="error-handling-in-api-management-policies"></a>Foutafhandeling in API Management-beleidsregels

Door op te geven een `ProxyError` -object, Azure API Management kunnen uitgevers om te reageren op fouten die tijdens het verwerken van aanvragen optreden. De `ProxyError` object toegankelijk is via de [context. LastError](api-management-policy-expressions.md#ContextVariables) eigenschap en kan worden gebruikt door het beleid in de `on-error` beleidssectie. Dit artikel bevat een verwijzing voor de fout afhandelen mogelijkheden in Azure API Management.  
  
## <a name="error-handling-in-api-management"></a>Foutafhandeling in API Management

 In Azure API Management-beleidsregels worden gedeeld in `inbound`, `backend`, `outbound`, en `on-error` secties, zoals wordt weergegeven in het volgende voorbeeld.  
  
```xml  
<policies>  
  <inbound>  
    <!-- statements to be applied to the request go here -->  
  </inbound>  
  <backend>  
    <!-- statements to be applied before the request is   
         forwarded to the backend service go here -->  
    </backend>  
    <outbound>  
      <!-- statements to be applied to the response go here -->  
    </outbound>  
    <on-error>  
        <!-- statements to be applied if there is an error   
             condition go here -->  
  </on-error>  
</policies>  
```  
  
Tijdens de verwerking van een aanvraag, ingebouwde stappen worden uitgevoerd, samen met eventuele beleidsregels die binnen het bereik van de aanvraag zijn. Als er een fout optreedt, onmiddellijk verwerken gaat u naar de `on-error` beleidssectie.  
De `on-error` beleidssectie kan worden gebruikt op een bereik. API-uitgevers kunnen aangepaste gedrag, zoals logboekregistratie van de fout naar eventhubs of het maken van een nieuw antwoord om terug te keren naar de aanroepende functie configureren.  
  
> [!NOTE]
>  De `on-error` sectie is niet aanwezig in het beleid standaard. Om toe te voegen de `on-error` sectie naar een door beleid, blader naar het gewenste beleid in de editor en toe te voegen. Zie voor meer informatie over het configureren van beleid [beleidsregels in API Management](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Als er geen `on-error` sectie aanroepers ontvangt 400 of 500 HTTP-antwoordberichten als er een fout optreedt.  
  
### <a name="policies-allowed-in-on-error"></a>Beleidsregels die zijn toegestaan in bij fout

 De volgende beleidsregels kunnen worden gebruikt in de `on-error` beleidssectie.  
  
-   [Kies](api-management-advanced-policies.md#choose)  
-   [set-variable](api-management-advanced-policies.md#set-variable)  
-   [zoeken en vervangen](api-management-transformation-policies.md#Findandreplacestringinbody)  
-   [return-response](api-management-advanced-policies.md#ReturnResponse)  
-   [set-header](api-management-transformation-policies.md#SetHTTPheader)  
-   [set-method](api-management-advanced-policies.md#SetRequestMethod)  
-   [set-status](api-management-advanced-policies.md#SetStatus)  
-   [send-request](api-management-advanced-policies.md#SendRequest)  
-   [send-one-way-request](api-management-advanced-policies.md#SendOneWayRequest)  
-   [log-to-eventhub](api-management-advanced-policies.md#log-to-eventhub)  
-   [json-to-xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
-   [xml-to-json](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>LastError

 Wanneer er een fout optreedt en besturingselement gaat u naar de `on-error` beleidssectie de fout wordt opgeslagen in [context. LastError](api-management-policy-expressions.md#ContextVariables) eigenschap, die kan worden geopend door een beleid in de `on-error` sectie. LastError heeft de volgende eigenschappen.  
  
| Name     | Type   | Description                                                                                               | Vereist |
|----------|--------|-----------------------------------------------------------------------------------------------------------|----------|
| Bron   | string | De naam van het element waar de fout is opgetreden. Mogelijk beleid of de naam van een ingebouwde pijplijn-stap.     | Ja      |
| Reden   | string | Machine-vriendelijk foutcode die kan worden gebruikt in foutafhandeling.                                       | Nee       |
| Bericht  | string | Leesbare foutbeschrijving.                                                                         | Ja      |
| Bereik    | string | Naam van het bereik waarin de fout is opgetreden en een van de 'global', 'product', "api" of 'bewerking' | Nee       |
| Sectie  | string | De naam van sectie waarin fout is opgetreden. Mogelijke waarden: "inkomende", 'back-end', "uitgaande" of "on error".       | Nee       |
| `Path`     | string | Hiermee geeft u geneste beleid, bijvoorbeeld ' kiezen [3] / wanneer [2] '.                                                        | Nee       |
| `PolicyId` | string | De waarde van de `id` kenmerk als opgegeven door de klant, in het beleid dat is waar de fout is opgetreden             | Nee       |

> [!TIP]
> U kunt de statuscode openen via context. Response.StatusCode.  

> [!NOTE]
> Alle beleidsregels hebben een optioneel `id` kenmerk dat kan worden toegevoegd aan het hoofdelement van het beleid. Als dit kenmerk in een beleid aanwezig is wanneer er een fout optreedt, de waarde van het kenmerk kan worden opgehaald met behulp van de `context.LastError.PolicyId` eigenschap.

## <a name="predefined-errors-for-built-in-steps"></a>Vooraf gedefinieerde fouten voor de ingebouwde stappen  
 De volgende fouten zijn vooraf gedefinieerd voor fouten die zich tijdens de evaluatie van de verwerkingsstappen die worden ingebouwd voordoen kunnen.  
  
| Bron        | Voorwaarde                                 | Reden                  | Bericht                                                                                                                |
|---------------|-------------------------------------------|-------------------------|------------------------------------------------------------------------------------------------------------------------|
| configuratie | URI komt niet overeen met elke API of bewerking | OperationNotFound       | Kan niet overeen met de binnenkomende aanvraag voor een bewerking.                                                                      |
| Autorisatie | De abonnementssleutel is niet opgegeven             | SubscriptionKeyNotFound | Toegang is geweigerd vanwege een ontbrekende abonnementssleutel. Zorg ervoor dat de abonnementssleutel bij aanvragen voor deze API. |
| Autorisatie | De sleutelwaarde abonnement is ongeldig         | SubscriptionKeyInvalid  | De toegang is geweigerd vanwege ongeldige abonnements-sleutel. Zorg ervoor dat u een geldige sleutel opgeven voor een actief abonnement.            |
  
## <a name="predefined-errors-for-policies"></a>Vooraf gedefinieerde fouten voor beleid  
 De volgende fouten zijn vooraf gedefinieerd voor fouten die zich tijdens de evaluatie van het beleid voordoen kunnen.  
  
| Bron       | Voorwaarde                                                       | Reden                    | Bericht                                                                                                                              |
|--------------|-----------------------------------------------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Frequentielimiet   | Limiet overschreden                                             | RateLimitExceeded         | Limiet wordt overschreden                                                                                                               |
| quota        | Quotum overschreden                                                  | QuotaExceeded             | Volumequotum buiten bereik. Quotum wordt aangevuld in xx:xx:xx. - of - Out van quota voor bandbreedte. Quotum wordt aangevuld in xx:xx:xx. |
| jsonp        | Waarde van de callback-parameter is ongeldig (bevat verkeerde tekens) | CallbackParameterInvalid  | Waarde van parameter callback {retouraanroep-parameter-name} is niet een geldige JavaScript-id.                                          |
| IP-filter    | Het parseren van de beller-IP-adres uit de aanvraag is mislukt                          | FailedToParseCallerIP     | Kan geen IP-adres instellen voor de oproepende functie. Toegang geweigerd.                                                                        |
| IP-filter    | Aanroeper IP staat niet in lijst met toegestane                                | CallerIpNotAllowed        | Aanroeper IP-adres {ip-adres} is niet toegestaan. Toegang geweigerd.                                                                        |
| IP-filter    | Aanroeper IP staat in de lijst met geblokkeerde Apps                                    | CallerIpBlocked           | IP-adres van oproepende functie is geblokkeerd. Toegang geweigerd.                                                                                         |
| Check-header | Vereiste header niet weergegeven of waarde ontbreekt               | HeaderNotFound            | Koptekst {header-name} is niet gevonden in de aanvraag. Toegang geweigerd.                                                                    |
| Check-header | Vereiste header niet weergegeven of waarde ontbreekt               | HeaderValueNotAllowed     | De waarde van de header {header-naam} van {headerwaarde} is niet toegestaan. Toegang geweigerd.                                                          |
| validate-jwt | Jwt-token ontbreekt in de aanvraag                                 | TokenNotFound             | De JWT is niet gevonden in de aanvraag. Toegang geweigerd.                                                                                         |
| validate-jwt | Handtekeningvalidatie is mislukt                                     | TokenSignatureInvalid     | < bericht van jwt-bibliotheek\>. Toegang geweigerd.                                                                                          |
| validate-jwt | Ongeldige doelgroep                                                | TokenAudienceNotAllowed   | < bericht van jwt-bibliotheek\>. Toegang geweigerd.                                                                                          |
| validate-jwt | Ongeldige uitgever                                                  | TokenIssuerNotAllowed     | < bericht van jwt-bibliotheek\>. Toegang geweigerd.                                                                                          |
| validate-jwt | Token is verlopen                                                   | TokenExpired              | < bericht van jwt-bibliotheek\>. Toegang geweigerd.                                                                                          |
| validate-jwt | Handtekeningsleutel is niet opgelost door ID                            | TokenSignatureKeyNotFound | < bericht van jwt-bibliotheek\>. Toegang geweigerd.                                                                                          |
| validate-jwt | Er ontbreken vereiste claims in token                          | TokenClaimNotFound        | JWT-token ontbreekt in de volgende claims: < c1\>, < c2\>,... Toegang geweigerd.                                                            |
| validate-jwt | Claim waarden verschil                                           | TokenClaimValueNotAllowed | Claimwaarde {claim-name} van {claimwaarde} is niet toegestaan. Toegang geweigerd.                                                             |
| validate-jwt | Andere validatiefouten                                       | JwtInvalid                | < bericht van jwt-bibliotheek\>                                                                                                          |

## <a name="example"></a>Voorbeeld

Als een API-beleid op:

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
    <on-error>
        <set-header name="ErrorSource" exists-action="override">
            <value>@(context.LastError.Source)</value>
        </set-header>
        <set-header name="ErrorReason" exists-action="override">
            <value>@(context.LastError.Reason)</value>
        </set-header>
        <set-header name="ErrorMessage" exists-action="override">
            <value>@(context.LastError.Message)</value>
        </set-header>
        <set-header name="ErrorScope" exists-action="override">
            <value>@(context.LastError.Scope)</value>
        </set-header>
        <set-header name="ErrorSection" exists-action="override">
            <value>@(context.LastError.Section)</value>
        </set-header>
        <set-header name="ErrorPath" exists-action="override">
            <value>@(context.LastError.Path)</value>
        </set-header>
        <set-header name="ErrorPolicyId" exists-action="override">
            <value>@(context.LastError.PolicyId)</value>
        </set-header>
        <set-header name="ErrorStatusCode" exists-action="override">
            <value>@(context.Response.StatusCode.ToString())</value>
        </set-header>
        <base />
    </on-error>
</policies>
```

en verzenden van een niet-gemachtigde aanvraag resulteert in het volgende antwoord:

![Niet-geautoriseerde foutbericht](media/api-management-error-handling-policies/error-response.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie werken met beleidsregels voor:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)   