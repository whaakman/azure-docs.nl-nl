---
title: Azure API Management-beperking beleid | Microsoft Docs
description: Meer informatie over de beperking toegangsbeleid beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: f9872ee033d8c0bed215f8b37d64395e5dcd534c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-access-restriction-policies"></a>Beperking van API Management toegangsbeleid
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AccessRestrictionPolicies"></a>Softwarerestrictiebeleid toegang  
  
-   [Controle van HTTP-header](api-management-access-restriction-policies.md#CheckHTTPHeader) -afdwingt bestaan en/of de waarde van een HTTP-Header.  
-   [Aanroepfrequentie per abonnement](api-management-access-restriction-policies.md#LimitCallRate) -API wordt voorkomen dat gebruik wordt bereikt door het beperken van de aanroepfrequentie per abonnement op basis van een.  
-   [Aanroepfrequentie per sleutel](#LimitCallRateByKey) -API wordt voorkomen dat gebruik wordt bereikt door het beperken van de aanroepfrequentie per sleutel op basis van een.  
-   [Aanroeper IP-adressen beperken](api-management-access-restriction-policies.md#RestrictCallerIPs) -Filters (kunt/weigert) aanroepen van bepaalde IP-adressen en/of -adresbereiken.  
-   [Gebruiksquotum per abonnement instellen](api-management-access-restriction-policies.md#SetUsageQuota) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van per abonnement afdwingen.  
-   [Gebruiksquotum instellen door sleutel](#SetUsageQuotaByKey) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van de sleutel per afdwingen.  
-   [Valideren van JWT](api-management-access-restriction-policies.md#ValidateJWT) -afdwingt bestaan en de geldigheid van een JWT opgehaald uit een opgegeven HTTP-koptekst of een opgegeven queryparameter.  
  
##  <a name="CheckHTTPHeader"></a>Controleer de HTTP-header  
 Gebruik de `check-header` beleid af te dwingen dat een aanvraag een opgegeven HTTP-header heeft. U kunt eventueel controleren of de header is een specifieke waarde of het selectievakje voor een bereik van toegestane waarden. Als de controle mislukt, wordt het beleid wordt beëindigd aanvraagverwerking en retourneert het HTTP-status code en de fout bericht opgegeven door het beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|controle-header|Hoofdelement.|Ja|  
|waarde|Toegestane waarde voor HTTP-header. Als meerdere elementen van de waarde worden opgegeven, de controle wordt beschouwd als geslaagd als een van de waarden een overeenkomst.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|kan de niet-controle-foutbericht|Het foutbericht te retourneren in de HTTP-antwoordtekst als de header bestaat niet of een ongeldige waarde heeft. Dit bericht moet de speciale tekens escape hebben.|Ja|N.v.t.|  
|Kan controle httpcode|HTTP-statuscode te retourneren als de header bestaat niet of een ongeldige waarde heeft.|Ja|N.v.t.|  
|header-naam|De naam van de HTTP-Header om te controleren.|Ja|N.v.t.|  
|negeren geval|Kan worden ingesteld op True of False. Als is ingesteld op True aanvraag wordt genegeerd wanneer de waarde voor header met de set van acceptabele waarden vergeleken.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** binnenkomende, uitgaande  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="LimitCallRate"></a>Aanroepfrequentie per abonnement  
 De `rate-limit` API gebruikspieken op basis van per abonnement wordt verhinderd door het beperken van de aanroepfrequentie met een opgegeven aantal gedurende een opgegeven periode. Wanneer dit beleid wordt geactiveerd. de aanroeper ontvangt een `429 Too Many Requests` statuscode van antwoord.  
  
> [!IMPORTANT]
>  Dit beleid kan slechts één keer per beleidsdocument worden gebruikt.  
>   
>  [Beleidsexpressies](api-management-policy-expressions.md) kan niet worden gebruikt in een van de kenmerken van het beleid voor dit beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|opgegeven limiet|Hoofdelement.|Ja|  
|api|Voeg een of meer van deze elementen te leggen frequentielimiet aanroep op API's binnen het product. Producten en -API aanroepen snelheid limieten onafhankelijk worden toegepast.|Nee|  
|bewerking|Een of meer van deze elementen te leggen van de frequentielimiet van een aanroep op bewerkingen binnen een API toevoegen. Product, API en bewerking aanroepen snelheid limieten onafhankelijk worden toegepast.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|naam|De naam van de API voor de frequentielimiet wordt toegepast.|Ja|N.v.t.|  
|oproepen|Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval die is opgegeven in de `renewal-period`.|Ja|N.v.t.|  
|vernieuwingsperiode|De tijd in seconden waarna het quotum wordt opnieuw ingesteld.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
  
-   **Beleid scopes:** product  
  
##  <a name="LimitCallRateByKey"></a>Aanroepfrequentie per sleutel  
 De `rate-limit-by-key` API gebruikspieken op basis van de sleutel per wordt verhinderd door het beperken van de aanroepfrequentie met een opgegeven aantal gedurende een opgegeven periode. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door een beleidsexpressie met. Optionele incrementele voorwaarde kan worden toegevoegd om op te geven welke aanvragen naar de limiet moeten worden geteld. Wanneer dit beleid wordt geactiveerd. de aanroeper ontvangt een `429 Too Many Requests` statuscode van antwoord.  
  
 Zie voor meer informatie over en voorbeelden van dit beleid, [geavanceerde aanvraagbeperking met Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Dit beleid kan slechts één keer per beleidsdocument worden gebruikt.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a>Voorbeeld  
 In het volgende voorbeeld wordt de frequentielimiet ingevoerd door de aanroeper IP-adres.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|opgegeven limiet|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|oproepen|Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval die is opgegeven in de `renewal-period`.|Ja|N.v.t.|  
|tegenpartij sleutel|De sleutel moet worden gebruikt voor het frequentielimietbeleid.|Ja|N.v.t.|  
|verhoging voorwaarde|De Boole-expressie opgeven als de aanvraag moet worden geteld voor het quotum (`true`).|Nee|N.v.t.|  
|vernieuwingsperiode|De tijd in seconden waarna het quotum wordt opnieuw ingesteld.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="RestrictCallerIPs"></a>Aanroeper IP-adressen beperken  
 De `ip-filter` beleid filtert (kunt/weigert) aanroepen van bepaalde IP-adressen en/of -adresbereiken.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|IP-filter|Hoofdelement.|Ja|  
|Adres|Hiermee geeft u een enkel IP-adres waarop u wilt filteren.|Ten minste één `address` of `address-range` element is vereist.|  
|adresbereik van = 'adres' naar 'adres' =|Hiermee geeft u een bereik met IP-adres waarop u wilt filteren.|Ten minste één `address` of `address-range` element is vereist.|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|adresbereik van = 'adres' naar 'adres' =|Een bereik van IP-adressen wilt toestaan of weigeren van toegang voor.|Vereist wanneer de `address-range` element wordt gebruikt.|N.v.t.|  
|IP-filteractie = ' toestaan dat &#124; verbieden"|Geeft aan of aanroepen moeten worden toegestaan of niet voor het opgegeven IP-adressen en de bereiken.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="SetUsageQuota"></a>Gebruiksquotum per abonnement instellen  
 De `quota` beleid zorgt ervoor dat een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van per abonnement.  
  
> [!IMPORTANT]
>  Dit beleid kan slechts één keer per beleidsdocument worden gebruikt.  
>   
>  [Beleidsexpressies](api-management-policy-expressions.md) kan niet worden gebruikt in een van de kenmerken van het beleid voor dit beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|quotum|Hoofdelement.|Ja|  
|api|Voeg een of meer van deze elementen te leggen een quotum op API's binnen het product. Product- en API-quota worden onafhankelijk toegepast.|Nee|  
|bewerking|Een of meer van deze elementen te leggen een quotum op bewerkingen binnen een API toevoegen. Product, API en bewerking quota's worden onafhankelijk van elkaar toegepast.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|naam|De naam van de API of de bewerking waarvoor het quotum van toepassing.|Ja|N.v.t.|  
|Bandbreedte|Het maximum aantal kilobytes is toegestaan tijdens het tijdsinterval die is opgegeven in de `renewal-period`.|Beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven.|N.v.t.|  
|oproepen|Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval die is opgegeven in de `renewal-period`.|Beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven.|N.v.t.|  
|vernieuwingsperiode|De tijd in seconden waarna het quotum wordt opnieuw ingesteld.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
-   **Beleid scopes:** product  
  
##  <a name="SetUsageQuotaByKey"></a>Gebruiksquotum instellen door sleutel  
 De `quota-by-key` beleid zorgt ervoor dat een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van de per-sleutel. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door een beleidsexpressie met. Optionele incrementele voorwaarde kan worden toegevoegd om op te geven welke aanvragen naar het quotum moeten worden geteld.  
  
 Zie voor meer informatie over en voorbeelden van dit beleid, [geavanceerde aanvraagbeperking met Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Dit beleid kan slechts één keer per beleidsdocument worden gebruikt.  
>   
>  [Beleidsexpressies](api-management-policy-expressions.md) kan niet worden gebruikt in een van de kenmerken van het beleid voor dit beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a>Voorbeeld  
 In het volgende voorbeeld wordt het quotum ingevoerd door de aanroeper IP-adres.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|quotum|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|Bandbreedte|Het maximum aantal kilobytes is toegestaan tijdens het tijdsinterval die is opgegeven in de `renewal-period`.|Beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven.|N.v.t.|  
|oproepen|Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval die is opgegeven in de `renewal-period`.|Beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven.|N.v.t.|  
|tegenpartij sleutel|De sleutel moet worden gebruikt voor het quotumbeleid.|Ja|N.v.t.|  
|verhoging voorwaarde|De Boole-expressie opgeven als de aanvraag moet worden geteld voor het quotum (`true`)|Nee|N.v.t.|  
|vernieuwingsperiode|De tijd in seconden waarna het quotum wordt opnieuw ingesteld.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="ValidateJWT"></a>JWT valideren  
 De `validate-jwt` beleid zorgt ervoor dat bestaan en de geldigheid van een JWT opgehaald uit beide een opgegeven HTTP-Header of een opgegeven queryparameter.  
  
> [!IMPORTANT]
>  De `validate-jwt` beleid vereist dat de `exp` geregistreerde claim inlcuded in de JWT-token is tenzij `require-expiration-time` kenmerk is opgegeven en ingesteld op `false`.  
> De `validate-jwt` beleid ondersteunt HS256 en RS256 ondertekenen algoritmen. HS256 moet de sleutel worden opgegeven voor inline in het beleid in het formulier base64-gecodeerd. De sleutel heeft voor RS256 om via een Open ID configuratie-eindpunt.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<validate-jwt   
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"   
    failed-validation-httpcode="http status code to return on failure"   
    failed-validation-error-message="error message to return on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of the claim as it appears in the token" match="all|any" separator="separator character in a multi-valued claim">
      <value>claim value as it is expected to appear in the token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a>Voorbeelden  
  
#### <a name="azure-mobile-services-token-validation"></a>Validatie van Azure Mobile Services-tokens  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a>Validatie van Azure Active Directory-tokens  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a>Validatie van Azure Active Directory B2C-tokens  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-to-operations-based-on-token-claims"></a>Toegang verlenen aan bewerkingen op basis van claims token  
 Dit voorbeeld ziet u hoe u de [JWT valideren](api-management-access-restriction-policies.md#ValidateJWT) beleid vooraf toegang verlenen aan bewerkingen op basis van claims token. Zie voor een demonstratie van configureren en gebruiken van dit beleid [Cloud hebben betrekking op aflevering 177: meer API-beheerfuncties met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit tot 13:50. Snel doorsturen naar 15:00 voor een overzicht van het beleid dat is geconfigureerd in de beleidseditor en vervolgens naar 18:50 voor een demonstratie van een bewerking aanroepen vanuit de ontwikkelaarsportal zowel met als zonder de vereiste verificatietoken.  
  
```xml  
<!-- Copy the following snippet into the inbound section at the api (or higher) level to pre-authorize access to operations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|valideren jwt|Hoofdelement.|Ja|  
|doelgroepen|Bevat een lijst van toegestane doelgroep claims die gebruikt op het token worden kunnen. Als meerdere doelgroep waarden aanwezig zijn, wordt elke waarde wordt geprobeerd totdat alle (in dat geval validatie mislukt) zijn uitgeput of tot er één lukt. Ten minste één doelgroep moet worden opgegeven.|Nee|  
|ondertekening-sleutels van uitgever|Een lijst met Base64-gecodeerd beveiligingssleutels gebruikt om ondertekende tokens te valideren. Als meerdere beveiligingssleutels aanwezig zijn, wordt elke sleutel wordt geprobeerd totdat alle (in dat geval validatie mislukt) zijn uitgeput of tot er één lukt (dit is nuttig voor de overschakeling van token). De belangrijkste elementen hebben een optioneel `id` kenmerk dat wordt gebruikt voor het vergelijken van `kid` claim.|Nee|  
|uitgevers van certificaten|Een lijst met toegestane principals die het token heeft uitgegeven. Als meerdere verlener waarden aanwezig zijn, wordt elke waarde wordt geprobeerd totdat alle (in dat geval validatie mislukt) zijn uitgeput of tot er één lukt.|Nee|  
|openid-config|Het element dat wordt gebruikt voor het opgeven van een compatibele configuratie-eindpunt voor Open-ID is waar ondertekenen van sleutels en uitgever kan worden verkregen.|Nee|  
|vereist claims|Bevat een lijst met claims aanwezig zijn op het token als geldig beschouwd als deze wordt verwacht. Wanneer de `match` kenmerk is ingesteld op `all` elke claimwaarde in het beleid moet aanwezig zijn in het token voor de validatie mislukt. Wanneer de `match` kenmerk is ingesteld op `any` ten minste één claim moet aanwezig zijn in het token voor de validatie mislukt.|Nee|  
|zumo hoofdsleutel|Hoofdsleutel voor tokens die zijn uitgegeven door Azure Mobile Services|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|tijdverschil|TimeSpan. Biedt een aantal kleine eenheidsprofiel voor het geval van het token verloopt claim aanwezig in het token is en na de huidige datum valt / tijd.|Nee|0 seconden|  
|kan de niet-validatie-foutbericht|Het foutbericht te retourneren in de HTTP-antwoordtekst als de JWT niet kan gevalideerd worden. Dit bericht moet de speciale tekens escape hebben.|Nee|Standaardfoutbericht, is afhankelijk van validatieprobleem, bijvoorbeeld 'JWT niet aanwezig."|  
|kan geen validatie httpcode|HTTP-statuscode te retourneren als de JWT niet gevalideerd worden.|Nee|401|  
|header-naam|De naam van de HTTP-header van het token.|Ofwel `header-name` of `query-paremeter-name` moet worden opgegeven, maar niet beide.|N.v.t.|  
|id|De `id` -kenmerk uit voor de `key` element kunt u opgeven van de tekenreeks die wordt vergeleken met `kid` claim in het token (indien aanwezig) om erachter te komen met de juiste sleutel moet worden gebruikt voor validatie van handtekening.|Nee|N.v.t.|  
|Overeenkomst|De `match` -kenmerk uit voor de `claim` element geeft aan of de waarde van elke claim in het beleid aanwezig zijn in het token voor de validatie moet mislukt. Mogelijke waarden zijn:<br /><br /> -                          `all`-de waarde van elke claim in het beleid moet aanwezig zijn in het token voor de validatie mislukt.<br /><br /> -                          `any`-ten minste één claimwaarde moet aanwezig zijn in het token voor de validatie mislukt.|Nee|all|  
|query-paremeter-naam|De naam van de de queryparameter van het token.|Ofwel `header-name` of `query-paremeter-name` moet worden opgegeven, maar niet beide.|N.v.t.|  
|vereisen verlooptijd vallen|Booleaanse waarde. Hiermee geeft u op of een claim vervaldatum in het token is vereist.|Nee|waar|
|vereisen schema|De naam van het token schema, bijvoorbeeld 'Bearer'. Wanneer dit kenmerk is ingesteld, kan het beleid zorgt ervoor dat het opgegeven schema is aanwezig in de waarde van de autorisatie-header.|Nee|N.v.t.|
|vereisen ondertekend-tokens|Booleaanse waarde. Geeft aan of een token vereist om te worden ondertekend.|Nee|waar|  
|Scheidingsteken|De tekenreeks. Hiermee geeft u een scheidingsteken (bijvoorbeeld ",") moet worden gebruikt voor een set waarden extraheren uit een claim meerdere waarden.|Nee|N.v.t.| 
|url|Open ID configuratie eindpunt-URL op waar de metagegevens van de Open-ID-configuratie kan worden verkregen. Gebruik de volgende URL voor Azure Active Directory: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` waarbij u de naam van uw directory-tenant, bijvoorbeeld vervangt `contoso.onmicrosoft.com`.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, werken met beleid:

+ [Beleidsregels in API Management](api-management-howto-policies.md)
+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   