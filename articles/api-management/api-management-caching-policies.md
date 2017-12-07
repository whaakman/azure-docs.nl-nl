---
title: Azure API Management cachebeleidsregels | Microsoft Docs
description: Meer informatie over de cachebeleidsregels beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 488a4c4b7daf5c07ca5f6b6bb72464279658d372
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="api-management-caching-policies"></a>De cachebeleidsregels API Management
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CachingPolicies"></a>Cachebeleidsregels  
  
-   Antwoord cachebeleidsregels  
  
    -   [Ophalen uit de cache](api-management-caching-policies.md#GetFromCache) -cache uitvoeren opzoeken en retourneren een geldige in cache opgeslagen reacties indien beschikbaar.  
    -   [Store aan cache](api-management-caching-policies.md#StoreToCache) -antwoorden volgens de configuratie opgegeven cache opslaat in de cache.  
  
-   Waarde cachebeleidsregels  

    -   [Waarde niet ophalen uit de cache](#GetFromCacheByKey) -ophalen van een item in de cache per sleutel. 
    -   [Waarde opslaan in cache](#StoreToCacheByKey) -opslaan van een item in de cache per sleutel. 
    -   [Waarde verwijderen uit de cache](#RemoveCacheByKey) -verwijderen van een item in de cache per sleutel.  
  
##  <a name="GetFromCache"></a>Ophalen uit de cache  
 Gebruik de `cache-lookup` beleid voor het uitvoeren van de cache opzoeken en retourneren een geldige cacheantwoord indien beschikbaar. Dit beleid kan worden toegepast in gevallen waar antwoordinhoud statisch gedurende een periode blijft. Antwoord in cache opslaan minder bandbreedte en verwerking van vereisten opgelegd voor de back-end web server en lagere latentie waargenomen door de consument API.  
  
> [!NOTE]
>  Dit beleid moet zijn gekoppeld aan een [Store aan cache](api-management-caching-policies.md#StoreToCache) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">  
  <vary-by-header>Accept</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Accept-Charset</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Authorization</vary-by-header>  
  <!-- should be present when allow-private-response-caching is "true"-->  
  <vary-by-header>header name</vary-by-header>  
  <!-- optional, can repeated several times -->  
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>  
  <!-- optional, can repeated several times -->  
</cache-lookup>  
```  
  
### <a name="examples"></a>Voorbeelden  
  
#### <a name="example"></a>Voorbeeld  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true">  
            <vary-by-query-parameter>version</vary-by-query-parameter>  
        </cache-lookup>           
    </inbound>  
    <outbound>  
        <cache-store duration="seconds" />  
        <base />          
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Voorbeeld met behulp van beleidsexpressies  
 Dit voorbeeld ziet u hoe aan voor het configureren van API Management-antwoord in cache opslaan duur die overeenkomt met het antwoord in cache plaatsen van de back-endservice als opgegeven door de back-service `Cache-Control` richtlijn. Zie voor een demonstratie van configureren en gebruiken van dit beleid [Cloud hebben betrekking op aflevering 177: meer API-beheerfuncties met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit te 25:25.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Zie voor meer informatie [beleidsexpressies](api-management-policy-expressions.md) en [Context variabele](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|cache-lookup|Hoofdelement.|Ja|  
|door header Vary|De antwoorden per waarde voor de opgegeven header, zoals accepteren, Accept-Charset Accept-Encoding, Accept-Language autorisatie Expect, van de Host, cache beginnen op If-Match.|Nee|  
|variëren door-query-parameter|De cache beginnen op antwoorden per waarde voor opgegeven queryparameters. Voer één of meerdere parameters. Gebruik puntkomma als scheidingsteken. Als niets wordt opgegeven, worden alle queryparameters gebruikt.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|toestaan dat persoonlijke-antwoord-schrijfcache|Als de waarde `true`, kunt opslaan in cache van aanvragen met een autorisatie-header.|Nee|onwaar|  
|downstream-caching-type|Dit kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -none - downstream opslaan in cache is niet toegestaan.<br />-persoonlijke - downstream persoonlijke opslaan in cache is toegestaan.<br />-openbaar: persoonlijke en gedeelde downstream cache is toegestaan.|Nee|geen|  
|must-revalidate|Wanneer downstream caching is ingeschakeld dit kenmerk schakelt in of uit de `must-revalidate` cache-control-instructie in de antwoorden van de gateway.|Nee|waar|  
|door ontwikkelaars variëren|Ingesteld op `true` aan reacties van cache per sleutel van de ontwikkelaar.|Ja||  
|variëren-in-developer-groepen|Ingesteld op `true` aan reacties van cache per gebruikersrol.|Ja||  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
-   **Beleid scopes:** API, bewerking, product  
  
##  <a name="StoreToCache"></a>Aan het cachegeheugen opslaan  
 De `cache-store` beleid in de cache opgeslagen reacties volgens de opgegeven cache-instellingen. Dit beleid kan worden toegepast in gevallen waar antwoordinhoud statisch gedurende een periode blijft. Antwoord in cache opslaan minder bandbreedte en verwerking van vereisten opgelegd voor de back-end web server en lagere latentie waargenomen door de consument API.  
  
> [!NOTE]
>  Dit beleid moet zijn gekoppeld aan een [ophalen uit de cache](api-management-caching-policies.md#GetFromCache) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a>Voorbeelden  
  
#### <a name="example"></a>Voorbeeld  
  
```xml  
<policies>  
    <inbound>  
        <base />  
          <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">  
                <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->  
        </cache-lookup>  
    </inbound>  
    <outbound>  
        <base />  
            <cache-store duration="3600" />  
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Voorbeeld met behulp van beleidsexpressies  
 Dit voorbeeld ziet u hoe aan voor het configureren van API Management-antwoord in cache opslaan duur die overeenkomt met het antwoord in cache plaatsen van de back-endservice als opgegeven door de back-service `Cache-Control` richtlijn. Zie voor een demonstratie van configureren en gebruiken van dit beleid [Cloud hebben betrekking op aflevering 177: meer API-beheerfuncties met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit te 25:25.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Zie voor meer informatie [beleidsexpressies](api-management-policy-expressions.md) en [Context variabele](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|opslag van de cache|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|Duur|Time-to-live van de vermeldingen in cache in seconden opgegeven.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** uitgaand    
-   **Beleid scopes:** API, bewerking, product  
  
##  <a name="GetFromCacheByKey"></a>Waarde niet ophalen uit de cache  
 Gebruik de `cache-lookup-value` beleid om te zoeken in het cachegeheugen worden uitgevoerd met sleutel en retourneert een waarde in de cache. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door een beleidsexpressie met.  
  
> [!NOTE]
>  Dit beleid moet zijn gekoppeld aan een [waarde opslaan in cache](#StoreToCacheByKey) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value to use if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a>Voorbeeld  
 Zie voor meer informatie over en voorbeelden van dit beleid, [aangepast opslaan in cache in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|cache-lookup-waarde|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|standaard-waarde|Een waarde die wordt toegewezen aan de variabele als de sleutel zoeken in het cachegeheugen resulteerde in een ontbreekt. Als dit kenmerk niet is opgegeven, `null` is toegewezen.|Nee|`null`|  
|sleutel|Waarde van de sleutel moet worden gebruikt in de zoekopdracht in de cache.|Ja|N.v.t.|  
|naam variabele|Naam van de [context variabele](api-management-policy-expressions.md#ContextVariables) de looked up waarde wordt toegewezen als de zoekopdracht is geslaagd. Als lookup in een ontbreekt resulteert, de variabele wordt toegewezen aan de waarde van de `default-value` kenmerk of `null`, als de `default-value` kenmerk wordt weggelaten.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
-   **Beleid scopes:** algemeen API, bewerking, product  
  
##  <a name="StoreToCacheByKey"></a>Waarde opslaan in cache  
 De `cache-store-value` opslag van de cache per sleutel uitvoert. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door een beleidsexpressie met.  
  
> [!NOTE]
>  Dit beleid moet zijn gekoppeld aan een [waarde niet ophalen uit de cache](#GetFromCacheByKey) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cache-store-value key="cache key value" value="value to cache" duration="seconds" />  
```  
  
### <a name="example"></a>Voorbeeld  
 Zie voor meer informatie over en voorbeelden van dit beleid, [aangepast opslaan in cache in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|cache-store-waarde|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|Duur|Waarde wordt voor de van de opgegeven duurwaarde, in seconden opgegeven in de cache worden opgeslagen.|Ja|N.v.t.|  
|sleutel|De waarde van de cache-sleutel worden opgeslagen onder.|Ja|N.v.t.|  
|waarde|De waarde in de cache opgeslagen.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
-   **Beleid scopes:** algemeen API, bewerking, product  
  
###  <a name="RemoveCacheByKey"></a>Waarde uit cache verwijderen  
De `cache-remove-value` een geïdentificeerd door de sleutel in de cache-item wordt verwijderd. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door een beleidsexpressie met.  
  
#### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a>Voorbeeld  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|cache-remove-waarde|Hoofdelement.|Ja|  
  
#### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|sleutel|De sleutel van de waarde uit de cache worden verwijderd uit de cache.|Ja|N.v.t.|  
  
#### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
-   **Beleid scopes:** algemeen API, bewerking, product  

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, werken met beleid:

+ [Beleidsregels in API Management](api-management-howto-policies.md)
+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   
