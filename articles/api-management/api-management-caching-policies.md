---
title: Azure API Management cachebeleidsregels | Microsoft Docs
description: Meer informatie over de cachebeleidsregels beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: 52bdeb5fe517430497c57a5c34b822df5933e3ff
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608882"
---
# <a name="api-management-caching-policies"></a>Cachebeleidsregels van API Management
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleid. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CachingPolicies"></a> Cachebeleidsregels  
  
-   Antwoord cachebeleidsregels  
  
    -   [Ophalen uit de cache](api-management-caching-policies.md#GetFromCache) -cache uitvoeren opzoeken en retourneren een geldige in cache opgeslagen reacties indien beschikbaar.  
    -   [Store cache](api-management-caching-policies.md#StoreToCache) -antwoorden op basis van de opgegeven cache-control-configuratie in de cache opslaat.  
  
-   Waarde cachebeleidsregels  

    -   [Waarde niet ophalen uit de cache](#GetFromCacheByKey) -sleutel een item in de cache ophalen. 
    -   [Waarde in de cache Store](#StoreToCacheByKey) -Store van een item in de cache op sleutel. 
    -   [Waarde verwijderen uit de cache](#RemoveCacheByKey) -verwijderen van een item in de cache op sleutel.  
  
##  <a name="GetFromCache"></a> Ophalen van cache  
 Gebruik de `cache-lookup` beleid om uit te voeren cache opzoeken en retourneren een geldige in de cache opgeslagen reactie indien beschikbaar. Dit beleid kan worden toegepast in gevallen waar inhoud van de reactie statisch gedurende een bepaalde periode blijft. Reactiecaching verbruikt minder bandbreedte en verwerking van vereisten die zijn opgelegd voor de back-end web server- en lagere latentie waargenomen door de API-consument.  
  
> [!NOTE]
>  Dit beleid moet zijn gekoppeld aan een [Store cache](api-management-caching-policies.md#StoreToCache) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" cache-preference="prefer-external | external | internal" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">  
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
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true" cache-preference="internal" >  
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
 In dit voorbeeld laat zien hoe het configureren van API Management antwoord Cacheduur die overeenkomt met de antwoorden in cache opslaan van de back-endservice zoals opgegeven door de back-ups service `Cache-Control` richtlijn. Zie voor een demonstratie van configureren en gebruiken van dit beleid, [Cloud Cover aflevering 177: Meer functies van API Management met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en spoel vooruit naar 25:25.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Zie voor meer informatie, [beleidsexpressies](api-management-policy-expressions.md) en [contextvariabele](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|opzoeken van de cache|Root-element.|Ja|  
|afhankelijk van koptekst|Start in cache opslaan van antwoorden per waarde van de opgegeven header, zoals accepteren, accepteren-tekenset, Accept-Encoding, Accept-taal, autorisatie, verwachte, van de Host, If-Match.|Nee|  
|variëren-van-query-parameter|Start in cache opslaan van antwoorden per waarde van de opgegeven queryparameters. Voer één of meerdere parameters. Gebruik puntkomma als scheidingsteken. Als er niets wordt opgegeven, worden alle queryparameters worden gebruikt.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
| Name                           | Description                                                                                                                                                                                                                                                                                                                                                 | Vereist | Standaard           |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| toestaan-privé-reactie-caching | Als de waarde `true`, kunt opslaan in cache van aanvragen die een autorisatie-header bevatten.                                                                                                                                                                                                                                                                        | Nee       | false             |
| cache-voorkeur               | Kiezen uit de volgende waarden van het kenmerk:<br />- `internal` de ingebouwde API Management-cache gebruiken<br />- `external` de externe-cache gebruiken, zoals beschreven in [een externe Azure-Cache gebruiken voor Redis in Azure API Management](api-management-howto-cache-external.md),<br />- `prefer-external` om te gebruiken externe cache als geconfigureerd of de interne cache anders. | Nee       | `prefer-external` |
| downstream-caching-type        | Dit kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -geen - downstream opslaan in cache is niet toegestaan.<br />-persoonlijke - downstream in een privécache is toegestaan.<br />-openbare - persoonlijke en gedeelde downstream opslaan in cache is toegestaan.                                                                                                          | Nee       | geen              |
| moet revalidate                | Als downstream opslaan in cache is ingeschakeld. dit kenmerk schakelt in of uit de `must-revalidate` cache-control instructie in antwoorden van een gateway.                                                                                                                                                                                                                      | Nee       | true              |
| door ontwikkelaars verschillen              | Ingesteld op `true` cache antwoorden per [abonnementssleutel](https://docs.microsoft.com/azure/api-management/api-management-subscriptions#what-is-subscriptions).                                                                                                                                                                                                                                                                                                         | Ja      |         False          |
| variëren-in-ontwikkelaars-groepen       | Ingesteld op `true` cache antwoorden per [gebruikersgroep](https://docs.microsoft.com/azure/api-management/api-management-howto-create-groups).                                                                                                                                                                                                                                                                                                             | Ja      |       False            |  

### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend  
-   **Beleid bereiken:** Bewerking, product-API  
  
##  <a name="StoreToCache"></a> Store voor cache  
 De `cache-store` beleid in de cache opgeslagen reacties op basis van de opgegeven cache-instellingen. Dit beleid kan worden toegepast in gevallen waar inhoud van de reactie statisch gedurende een bepaalde periode blijft. Reactiecaching verbruikt minder bandbreedte en verwerking van vereisten die zijn opgelegd voor de back-end web server- en lagere latentie waargenomen door de API-consument.  
  
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
 In dit voorbeeld laat zien hoe het configureren van API Management antwoord Cacheduur die overeenkomt met de antwoorden in cache opslaan van de back-endservice zoals opgegeven door de back-ups service `Cache-Control` richtlijn. Zie voor een demonstratie van configureren en gebruiken van dit beleid, [Cloud Cover aflevering 177: Meer functies van API Management met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en spoel vooruit naar 25:25.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Zie voor meer informatie, [beleidsexpressies](api-management-policy-expressions.md) en [contextvariabele](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|cache-store|Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
| Name             | Description                                                                                                                                                                                                                                                                                                                                                 | Vereist | Standaard           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| duur         | Time-to-live van de vermeldingen in de cache in seconden opgegeven.                                                                                                                                                                                                                                                                                                   | Ja      | N/A               |  

### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** uitgaande    
-   **Beleid bereiken:** Bewerking, product-API  
  
##  <a name="GetFromCacheByKey"></a> Haal uit cache  
 Gebruik de `cache-lookup-value` beleid voor het opzoeken van de cache door sleutel uitvoeren en een in cache opgeslagen waarde retourneren. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door met behulp van een beleidsexpressie voor een.  
  
> [!NOTE]
>  Dit beleid moet zijn gekoppeld aan een [Store waarde in de cache](#StoreToCacheByKey) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value to use if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to"  
    cache-preference="prefer-external | external | internal" />  
```  
  
### <a name="example"></a>Voorbeeld  
 Zie voor meer informatie en voorbeelden van dit beleid [aangepast opslaan in cache in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|cache-lookup-waarde|Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
| Name             | Description                                                                                                                                                                                                                                                                                                                                                 | Vereist | Standaard           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| cache-voorkeur | Kiezen uit de volgende waarden van het kenmerk:<br />- `internal` de ingebouwde API Management-cache gebruiken<br />- `external` de externe-cache gebruiken, zoals beschreven in [een externe Azure-Cache gebruiken voor Redis in Azure API Management](api-management-howto-cache-external.md),<br />- `prefer-external` om te gebruiken externe cache als geconfigureerd of de interne cache anders. | Nee       | `prefer-external` |
| standaardwaarde:    | Een waarde die wordt toegewezen aan de variabele als de sleutel opzoeken van de cache heeft geresulteerd in een ontbreekt. Als dit kenmerk niet opgegeven is, `null` is toegewezen.                                                                                                                                                                                                           | Nee       | `null`            |
| sleutel              | Waarde van de sleutel te gebruiken in de zoekopdracht in de cache.                                                                                                                                                                                                                                                                                                                       | Ja      | N/A               |
| naam van de variabele    | Naam van de [contextvariabele](api-management-policy-expressions.md#ContextVariables) de looked van waarde wordt toegewezen aan, als de zoekopdracht is geslaagd. Als lookup in een ontbreekt resulteert, de variabele wordt toegewezen aan de waarde van de `default-value` kenmerk of `null`, als de `default-value` kenmerk wordt weggelaten.                                       | Ja      | N/A               |  

### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
-   **Beleid bereiken:** globale, API, bewerking, product  
  
##  <a name="StoreToCacheByKey"></a> Store-waarde in de cache  
 De `cache-store-value` cacheopslag uitvoert op sleutel. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door met behulp van een beleidsexpressie voor een.  
  
> [!NOTE]
>  Dit beleid moet zijn gekoppeld aan een [waarde niet ophalen uit de cache](#GetFromCacheByKey) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cache-store-value key="cache key value" value="value to cache" duration="seconds" cache-preference="prefer-external | external | internal" />  
```  
  
### <a name="example"></a>Voorbeeld  
 Zie voor meer informatie en voorbeelden van dit beleid [aangepast opslaan in cache in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|cache-store-waarde|Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
| Name             | Description                                                                                                                                                                                                                                                                                                                                                 | Vereist | Standaard           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| cache-voorkeur | Kiezen uit de volgende waarden van het kenmerk:<br />- `internal` de ingebouwde API Management-cache gebruiken<br />- `external` de externe-cache gebruiken, zoals beschreven in [een externe Azure-Cache gebruiken voor Redis in Azure API Management](api-management-howto-cache-external.md),<br />- `prefer-external` om te gebruiken externe cache als geconfigureerd of de interne cache anders. | Nee       | `prefer-external` |
| duur         | Waarde wordt in de cache worden opgeslagen voor de waarde voor de opgegeven duur, in seconden opgegeven.                                                                                                                                                                                                                                                                                 | Ja      | N/A               |
| sleutel              | De waarde van de cache-sleutel worden opgeslagen onder.                                                                                                                                                                                                                                                                                                                   | Ja      | N/A               |
| waarde            | De waarde in de cache opgeslagen.                                                                                                                                                                                                                                                                                                                                     | Ja      | N/A               |
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
-   **Beleid bereiken:** globale, API, bewerking, product  
  
###  <a name="RemoveCacheByKey"></a> Waarde verwijderen uit de cache  
De `cache-remove-value` Hiermee verwijdert u een item in de cache is geïdentificeerd door de sleutel. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door met behulp van een beleidsexpressie voor een.  
  
#### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
  
<cache-remove-value key="cache key value" cache-preference="prefer-external | external | internal"  />  
  
```  
  
#### <a name="example"></a>Voorbeeld  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|cache-remove-waarde|Root-element.|Ja|  
  
#### <a name="attributes"></a>Kenmerken  
  
| Name             | Description                                                                                                                                                                                                                                                                                                                                                 | Vereist | Standaard           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| cache-voorkeur | Kiezen uit de volgende waarden van het kenmerk:<br />- `internal` de ingebouwde API Management-cache gebruiken<br />- `external` de externe-cache gebruiken, zoals beschreven in [een externe Azure-Cache gebruiken voor Redis in Azure API Management](api-management-howto-cache-external.md),<br />- `prefer-external` om te gebruiken externe cache als geconfigureerd of de interne cache anders. | Nee       | `prefer-external` |
| sleutel              | De sleutel van de eerder in de cache waarde die moet worden verwijderd uit de cache.                                                                                                                                                                                                                                                                                        | Ja      | N/A               |
  
#### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
-   **Beleid bereiken:** globale, API, bewerking, product  

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie werken met beleidsregels voor:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)   
