---
title: Fout tijdens verwerken van in Azure API Management-beleid | Microsoft Docs
description: Informatie over het reageren op fouten die tijdens de verwerking van aanvragen in Azure API Management optreden.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 6bc71c0745493d52128553a78a31c45a3bca30f8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="error-handling-in-api-management-policies"></a>Fout tijdens verwerken van in API Management-beleidsregels
Azure API Management kunnen publishers te reageren op fouten die optreden tijdens de verwerking van aanvragen naar de proxy door een `ProxyError` object. De `ProxyError` object kan worden geopend via de [context. LastError](api-management-policy-expressions.md#ContextVariables) eigenschap en kan worden gebruikt door het beleid in de `on-error` beleidssectie. Dit onderwerp bevat een verwijzing voor de fout verwerking mogelijkheden in Azure API Management.  
  
## <a name="error-handling-in-api-management"></a>Fout tijdens verwerken van in API Management  
 Beleid in Azure API Management zijn onderverdeeld in `inbound`, `backend`, `outbound`, en `on-error` secties zoals weergegeven in het volgende voorbeeld.  
  
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
  
 Tijdens het verwerken van een aanvraag, worden de ingebouwde stappen uitgevoerd samen met eventuele beleidsregels die zich binnen het bereik van de aanvraag. Als er een fout optreedt, onmiddellijk verwerken gaat u naar de `on-error` beleidssectie. De `on-error` beleidssectie kan worden gebruikt op een bereik en API-uitgevers aangepaste gedrag zoals logboekregistratie van de fout naar event hubs of maken van een nieuw antwoord om terug te keren naar de aanroeper kunnen configureren.  
  
> [!NOTE]
>  De `on-error` sectie is niet aanwezig in het beleid standaard. Om toe te voegen de `on-error` sectie aan een beleid, blader naar het gewenste beleid in de beleidseditor en toe te voegen. Zie voor meer informatie over het configureren van beleid [-beleid in API Management](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Als er geen `on-error` sectie aanroepfuncties 400 of 500 HTTP-antwoordberichten ontvangt als er een fout optreedt.  
  
### <a name="policies-allowed-in-on-error"></a>Beleid dat is toegestaan in op fout  
 De volgende beleidsregels kunnen worden gebruikt in de `on-error` beleidssectie.  
  
-   [Kies](api-management-advanced-policies.md#choose)  
-   [variabele instellen](api-management-advanced-policies.md#set-variable)  
-   [zoeken en vervangen](api-management-transformation-policies.md#Findandreplacestringinbody)  
-   [Return-antwoord](api-management-advanced-policies.md#ReturnResponse)  
-   [set-header](api-management-transformation-policies.md#SetHTTPheader)  
-   [set-methode](api-management-advanced-policies.md#SetRequestMethod)  
-   [status instellen](api-management-advanced-policies.md#SetStatus)  
-   [aanvragen verzenden](api-management-advanced-policies.md#SendRequest)  
-   [een-manier-verzoek om te verzenden](api-management-advanced-policies.md#SendOneWayRequest)  
-   [logboek voor eventhub](api-management-advanced-policies.md#log-to-eventhub)  
-   [JSON-to-xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
-   [XML-json-](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>LastError  
 Wanneer er een fout optreedt en besturingselement gaat u naar de `on-error` beleidssectie voor de fout wordt opgeslagen in [context. LastError](api-management-policy-expressions.md#ContextVariables) -eigenschap hebben die toegankelijk zijn voor het beleid in de `on-error` sectie en heeft de volgende eigenschappen.  
  
|Naam|Type|Beschrijving|Vereist|  
|----------|----------|-----------------|--------------|  
|Bron|Tekenreeks|De naam van het element waar de fout is opgetreden. Kan worden beleid of de naam van een ingebouwde pijplijn stap.|Ja|  
|Reden|Tekenreeks|Machine-vriendelijk foutcode die kan worden gebruikt in de foutafhandeling.|Nee|  
|Bericht|Tekenreeks|Er zijn foutomschrijving leesbare.|Ja|  
|Bereik|Tekenreeks|Naam van het bereik waarin de fout is opgetreden en mogelijk een van de 'global', 'product', 'api' of 'bewerking'|Nee|  
|Sectie|Tekenreeks|Naam van de sectie waar de fout is opgetreden en kan een van de 'inkomende', back-end', 'uitgaande' of 'on error'.|Nee|  
|Pad|Tekenreeks|Hiermee geeft u geneste beleid, bijvoorbeeld ' kiezen [3] / wanneer [2] '.|Nee|  
|PolicyId|Tekenreeks|Waarde van de `id` kenmerk toe, indien opgegeven door de klant op het beleid waar de fout is opgetreden|Nee|  
  
> [!NOTE]
>  Alle beleidsregels hebben een optioneel `id` kenmerk die kan worden toegevoegd aan het hoofdelement van het beleid. Als dit kenmerk aanwezig in een beleid is als een fout optreedt, de waarde van het kenmerk kan worden opgehaald met de `context.LastError.PolicyId` eigenschap.  
  
## <a name="predefined-errors-for-built-in-steps"></a>Vooraf gedefinieerde fouten voor de ingebouwde stappen  
 De volgende fouten zijn vooraf gedefinieerd voor foutvoorwaarden die tijdens het evalueren van de van de ingebouwde verwerkingsstappen optreden kunnen.  
  
|Bron|Voorwaarde|Reden|Bericht|  
|------------|---------------|------------|-------------|  
|configuratie|URI komt niet overeen met een Api of de bewerking|OperationNotFound|Kan niet overeen met de binnenkomende aanvraag voor een bewerking.|  
|Autorisatie|Abonnementssleutel niet opgegeven|SubscriptionKeyNotFound|Toegang is geweigerd vanwege een ontbrekende abonnementssleutel. Zorg ervoor dat abonnementssleutel bij aanvragen voor deze API.|  
|Autorisatie|De sleutelwaarde abonnement is ongeldig|SubscriptionKeyInvalid|De toegang is geweigerd vanwege ongeldige abonnements-sleutel. Zorg ervoor dat een geldige sleutel voor een actief abonnement opgeven.|  
  
## <a name="predefined-errors-for-policies"></a>Vooraf gedefinieerde fouten voor beleid  
 De volgende fouten zijn vooraf gedefinieerd voor fouten die tijdens de evaluatie van het beleid optreden kunnen.  
  
|Bron|Voorwaarde|Reden|Bericht|  
|------------|---------------|------------|-------------|  
|Frequentielimiet|Frequentielimiet is overschreden|RateLimitExceeded|Frequentielimiet is overschreden|  
|quotum|Quota overschreden|QuotaExceeded|Volumequotum buiten bereik. Quotum wordt aangevuld in xx:xx:xx. - of - Out van de quota voor bandbreedte. Quotum wordt aangevuld in xx:xx:xx.|  
|jsonp|De parameterwaarde retouraanroep is ongeldig (bevat verkeerde tekens)|CallbackParameterInvalid|Waarde van parameter callback {retouraanroep-parameter-name} is geen geldige JavaScript-id.|  
|IP-filter|Kan niet worden geparseerd aanroeper IP van aanvraag|FailedToParseCallerIP|IP-adres maken voor de aanroeper is mislukt. Toegang geweigerd.|  
|IP-filter|Aanroeper IP staat niet in lijst met toegestane|CallerIpNotAllowed|Aanroeper {IP-adres-} is niet toegestaan. Toegang geweigerd.|  
|IP-filter|Aanroeper IP staat in de lijst met geblokkeerde|CallerIpBlocked|Aanroeper IP-adres wordt geblokkeerd. Toegang geweigerd.|  
|controle-header|Vereiste header is niet opgenomen of waarde ontbreekt|HeaderNotFound|Header {header-name} is niet gevonden in de aanvraag. Toegang geweigerd.|  
|controle-header|Vereiste header is niet opgenomen of waarde ontbreekt|HeaderValueNotAllowed|De waarde van de header {headernaam} van {headerwaarde} is niet toegestaan. Toegang geweigerd.|  
|valideren jwt|Jwt-token ontbreekt in de aanvraag|TokenNotFound|De JWT is niet gevonden in de aanvraag. Toegang geweigerd.|  
|valideren jwt|Handtekeningvalidatie is mislukt|TokenSignatureInvalid|< bericht van jwt-bibliotheek\>. Toegang geweigerd.|  
|valideren jwt|Ongeldige doelgroep|TokenAudienceNotAllowed|< bericht van jwt-bibliotheek\>. Toegang geweigerd.|  
|valideren jwt|Ongeldige verlener|TokenIssuerNotAllowed|< bericht van jwt-bibliotheek\>. Toegang geweigerd.|  
|valideren jwt|Token is verlopen|TokenExpired|< bericht van jwt-bibliotheek\>. Toegang geweigerd.|  
|valideren jwt|Handtekeningsleutel is niet opgelost door de id|TokenSignatureKeyNotFound|< bericht van jwt-bibliotheek\>. Toegang geweigerd.|  
|valideren jwt|Er ontbreken vereiste claims in token|TokenClaimNotFound|JWT-token ontbreekt in de volgende claims: < c1\>, < c2\>,... Toegang geweigerd.|  
|valideren jwt|Claim waarden verschil|TokenClaimValueNotAllowed|Claimwaarde {claimnaam} van {claimwaarde} is niet toegestaan. Toegang geweigerd.|  
|valideren jwt|Andere validatiefouten|JwtInvalid|< bericht van jwt-bibliotheek\>|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, werken met beleid:

+ [Beleidsregels in API Management](api-management-howto-policies.md)
+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   