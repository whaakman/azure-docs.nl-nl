---
title: Azure API Management beleid voor meerdere domeinen | Microsoft Docs
description: Meer informatie over het beleid tussen domeinen beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 590831454e8a18678e357b4824eb35a717d1fee0
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-cross-domain-policies"></a>API Management-beleid voor meerdere domeinen
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CrossDomainPolicies"></a>Cross-domeinbeleid  
  
-   [Aanroepen tussen domeinen toestaan](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -de API van Adobe Flash en Microsoft Silverlight op browser gebaseerde clients toegankelijk maakt.  
-   [CORS](api-management-cross-domain-policies.md#CORS) -cross-origin-resource delen (CORS) ondersteuning voor een bewerking of een API om toe te staan tussen domeinen aanroepen van clients op basis van een browser wordt toegevoegd.  
-   [JSONP](api-management-cross-domain-policies.md#JSONP) -JSON wordt toegevoegd met ondersteuning voor een bewerking of een API om toe te staan tussen domeinen aanroepen vanuit JavaScript-browser gebaseerde clients opvulling (JSONP).  
  
##  <a name="AllowCrossDomainCalls"></a>Aanroepen tussen domeinen toestaan  
 Gebruik de `cross-domain` beleid voor de API om toegankelijk te maken van Adobe Flash en Microsoft Silverlight op browser gebaseerde clients.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in the Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|verschillende domeinen|Hoofdelement. Onderliggende elementen moeten voldoen aan de [Adobe cross-domeinbeleid bestandsspecificatie](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).|Ja|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
-   **Beleid scopes:** globale  
  
##  <a name="CORS"></a>CORS  
 De `cors` beleid cross-origin-resource delen (CORS) ondersteuning voor een bewerking of een API om toe te staan tussen domeinen aanroepen van clients op basis van een browser wordt toegevoegd.  
  
 CORS kunt een browser en een server om te communiceren en bepalen of bepaalde cross-origin-aanvragen (d.w.z. XMLHttpRequests aanroepen van JavaScript aangebracht op een webpagina in andere domeinen) toestaan. Dit biedt meer flexibiliteit dan alleen toe te staan dezelfde-origin-aanvragen, maar is veiliger dan zodat alle cross-origin-aanvragen.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<cors allow-credentials="false|true">  
    <allowed-origins>  
        <origin>origin uri</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="number of seconds">  
        <method>http verb</method>  
    </allowed-methods>  
    <allowed-headers>  
        <header>header name</header>  
    </allowed-headers>  
    <expose-headers>  
        <header>header name</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="example"></a>Voorbeeld  
 In dit voorbeeld laat zien hoe ter ondersteuning van voorafgaan aan de vlucht aanvragen, zoals die met aangepaste headers of andere methoden dan GET en POST. Ter ondersteuning van aangepaste headers en aanvullende HTTP-termen, gebruikt u de `allowed-methods` en `allowed-headers` secties zoals weergegeven in het volgende voorbeeld.  
  
```xml  
<cors allow-credentials="true">  
    <allowed-origins>  
        <!-- Localhost useful for development -->  
        <origin>http://localhost:8080/</origin>  
        <origin>http://example.com/</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="300">  
        <method>GET</method>  
        <method>POST</method>  
        <method>PATCH</method>  
        <method>DELETE</method>  
    </allowed-methods>  
    <allowed-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
        <header>x-zumo-version</header>  
        <header>x-zumo-auth</header>  
        <header>content-type</header>  
        <header>accept</header>  
    </allowed-headers>  
    <expose-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|cors|Hoofdelement.|Ja|N.v.t.|  
|toegestane oorsprongen|Bevat `origin` elementen die de toegestane oorsprongen voor aanvragen van andere domeinen beschrijven. `allowed-origins`kan bevatten één `origin` element waarmee wordt aangegeven `*` om toe te staan een oorsprong, of één of meer `origin` elementen die een URI bevatten.|Ja|N.v.t.|  
|oorsprong|De waarde kan zijn `*` toegestaan om alle oorsprongen of een URI die een enkele oorsprong aangeeft. De URI moet een schema, host en poort bevatten.|Ja|Als de poort in een URI wordt weggelaten, wordt poort 80 wordt gebruikt voor HTTP en poort 443 voor HTTPS wordt gebruikt.|  
|toegestaan methoden|Dit element is vereist als methoden dan ophalen of POST zijn toegestaan. Bevat `method` elementen die opgeeft van de ondersteunde HTTP-termen.|Nee|Als u deze sectie is niet aanwezig is, worden GET en POST worden ondersteund.|  
|Methode|Hiermee geeft u een HTTP-term.|Ten minste één `method` element is vereist als de `allowed-methods` sectie aanwezig is.|N.v.t.|  
|toegestaan headers|Dit element bevat `header` elementen geven van namen van de headers die kunnen worden opgenomen in de aanvraag.|Nee|N.v.t.|  
|zichtbaar headers|Dit element bevat `header` elementen geven van namen van de headers die toegankelijk is door de client.|Nee|N.v.t.|  
|koptekst|Hiermee geeft u een header-naam.|Ten minste één `header` element is vereist in `allowed-headers` of `expose-headers` als de sectie aanwezig is.|N.v.t.|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|referenties toestaan|De `Access-Control-Allow-Credentials` header in het voorbereidende antwoord op de waarde van dit kenmerk worden ingesteld en van invloed zijn op de client de referenties in verschillende domeinen aanvragen indienen.|Nee|onwaar|  
|Preflight-resultaat--maximumleeftijd|De `Access-Control-Max-Age` header in het voorbereidende antwoord aan de waarde van dit kenmerk wordt ingesteld en van invloed zijn op de gebruikersagent cacheantwoord voorafgaan aan de vlucht.|Nee|0|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
-   **Beleid scopes:** API, bewerking  
  
##  <a name="JSONP"></a>JSONP  
 De `jsonp` beleid wordt JSON met ondersteuning voor opvulling (JSONP) toegevoegd aan een bewerking of een API om toe te staan tussen domeinen aanroepen vanuit JavaScript-clients op basis van een browser. JSONP is een methode in JavaScript-programma's voor gegevens van aanvragen van een server in een ander domein. JSONP omzeilt de beperking die wordt afgedwongen door de meeste webbrowsers waarop toegang tot webpagina's, in hetzelfde domein moet.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 Als u de methode zonder de parameter callback aanroepen? cb = XXX gewone JSON (zonder een wrapper-aanroep van functie) wordt geretourneerd.  
  
 Als u de parameter callback toevoegt `?cb=XXX` een JSONP-resultaat wordt geretourneerd, resultaten rond de callback-functie, zoals de oorspronkelijke JSON wrapping`XYZ('<json result goes here>');`  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|jsonp|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|callback parameternaam|De verschillende domeinen JavaScript functieaanroep voorafgegaan door de volledig gekwalificeerde domeinnaam waarin de functie zich bevindt.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** uitgaand  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, werken met beleid:

+ [Beleidsregels in API Management](api-management-howto-policies.md)
+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   