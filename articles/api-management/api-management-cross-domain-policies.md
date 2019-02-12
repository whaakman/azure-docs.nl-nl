---
title: Azure API Management beleid voor meerdere domeinen | Microsoft Docs
description: Meer informatie over de andere domeinbeleidsregels beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: ecbc1af97ce5ed158138f2bcf47f5729842c0fe9
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098528"
---
# <a name="api-management-cross-domain-policies"></a>API Management-beleid voor meerdere domeinen
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleid. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="CrossDomainPolicies"></a> Beleid voor meerdere domeinen

- [Toestaan van aanroepen tussen domeinen](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -de API van Adobe Flash en Microsoft Silverlight-browser-gebaseerde clients toegankelijk maakt.
- [CORS](api-management-cross-domain-policies.md#CORS) -cross-origin resource sharing (CORS) ondersteuning voor een bewerking of een API voor het toestaan van aanroepen tussen domeinen van clients op basis van een browser wordt toegevoegd.
- [JSONP](api-management-cross-domain-policies.md#JSONP) -JSON met ondersteuning voor de opvulling (JSONP) naar een bewerking of een API voor het toestaan van aanroepen tussen domeinen van JavaScript-clients op basis van een browser wordt toegevoegd.

## <a name="AllowCrossDomainCalls"></a> Toestaan van aanroepen tussen domeinen
Gebruik de `cross-domain` beleid waardoor de API van Adobe Flash en Microsoft Silverlight-browser-gebaseerde clients toegankelijk.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<cross-domain>
    <!-Policy configuration is in the Adobe cross-domain policy file format,
        see https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->
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

|Name|Description|Vereist|
|----------|-----------------|--------------|
|meerdere domeinen|Root-element. Onderliggende elementen moeten voldoen aan de [Adobe tussen domeinen beleidsspecificatie bestand](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).|Ja|

### <a name="usage"></a>Gebruik
Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Beleid secties:** inkomend
- **Beleid bereiken:** globale

## <a name="CORS"></a> CORS
De `cors` beleid cross-origin resource sharing (CORS) ondersteuning voor een bewerking of een API voor het toestaan van aanroepen tussen domeinen van clients op basis van een browser wordt toegevoegd.

CORS kunt u een browser en een server om te communiceren en te bepalen of bepaalde cross-origin-aanvragen (dat wil zeggen XMLHttpRequests oproepen van JavaScript op een webpagina naar andere domeinen) toestaan. Dit biedt meer flexibiliteit dan alleen toestaan dat dezelfde-origin-aanvragen, maar is veiliger dan zodat alle cross-origin-aanvragen.

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
In dit voorbeeld ziet u hoe u vooraf flight ondersteuningsaanvragen, zoals die met aangepaste headers of andere methoden dan GET en POST. Ter ondersteuning van aangepaste kopteksten en aanvullende HTTP-termen, gebruikt u de `allowed-methods` en `allowed-headers` secties, zoals wordt weergegeven in het volgende voorbeeld.

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

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|cors|Root-element.|Ja|N/A|
|toegestane oorsprongen|Bevat `origin` elementen die worden beschreven van de toegestane oorsprongen voor interdomein-aanvragen. `allowed-origins` Eén mag `origin` element waarmee `*` om toe te staan een oorsprong, of één of meer `origin` -elementen die een URI bevatten.|Ja|N/A|
|oorsprong|De waarde kan zijn `*` om toe te staan alle oorsprongen of een URI die Hiermee geeft u een één oorsprong. De URI moet bevatten een schema, host en poort.|Ja|Als de poort in een URI wordt weggelaten, wordt poort 80 wordt gebruikt voor HTTP en poort 443 voor HTTPS wordt gebruikt.|
|toegestane methoden|Dit element is vereist als methoden dan ophalen of boeken zijn toegestaan. Bevat `method` elementen die de ondersteunde HTTP-termen opgeeft.|Nee|Als deze sectie niet aanwezig zijn is, worden GET en POST worden ondersteund.|
|method|Hiermee geeft u een HTTP-term.|Ten minste één `method` element is vereist als de `allowed-methods` sectie aanwezig is.|N/A|
|toegestane kopteksten|Dit element bevat `header` elementen namen van de headers die kunnen worden opgenomen in de aanvraag op te geven.|Nee|N/A|
|expose-headers|Dit element bevat `header` elementen namen van de headers die toegankelijk is door de client op te geven.|Nee|N/A|
|koptekst|Hiermee geeft u de naam van een header.|Ten minste één `header` element is vereist in `allowed-headers` of `expose-headers` als de sectie aanwezig is.|N/A|

### <a name="attributes"></a>Kenmerken

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|allow-credentials|De `Access-Control-Allow-Credentials` header in het voorbereidende antwoord op de waarde van dit kenmerk wordt ingesteld en van invloed zijn op de mogelijkheid van de client om referenties in meerdere domeinen aanvragen.|Nee|false|
|preflight-result-max-age|De `Access-Control-Max-Age` header in het voorbereidende antwoord op de waarde van dit kenmerk wordt ingesteld en cache vooraf flight antwoord gevolgen voor de gebruikersagent.|Nee|0|

### <a name="usage"></a>Gebruik
Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Beleid secties:** inkomend
- **Beleid bereiken:** globale, API, bewerking

## <a name="JSONP"></a> JSONP
De `jsonp` beleid wordt JSON met ondersteuning voor opvulling (JSONP) toegevoegd aan een bewerking of een API voor het toestaan van aanroepen tussen domeinen van de browser gebaseerde JavaScript-clients. JSONP is een methode in JavaScript-programma's voor gegevens van aanvragen van een server in een ander domein. JSONP omzeilt de beperking die wordt afgedwongen door de meeste webbrowsers waar toegang tot de webpagina's moet zich in hetzelfde domein bevinden.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<jsonp callback-parameter-name="callback function name" />
```

### <a name="example"></a>Voorbeeld

```xml
<jsonp callback-parameter-name="cb" />
```

Als u de methode zonder de parameter callback aanroept? cb = XXX plain JSON (zonder een wrapper-aanroep voor functie) wordt geretourneerd.

Als u de parameter callback toevoegt `?cb=XXX` JSONP resultaat wordt geretourneerd, wrapping van de oorspronkelijke JSON-resultaten rond de callback-functie, zoals `XYZ('<json result goes here>');`

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|jsonp|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|callback-parameter-name|De interdomein-JavaScript functieaanroep voorafgegaan door de volledig gekwalificeerde domeinnaam waarin de functie zich bevindt.|Ja|N/A|

### <a name="usage"></a>Gebruik
Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Beleid secties:** uitgaande
- **Beleid bereiken:** wereldwijd, product, API, bewerking

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie werken met beleidsregels voor:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)   
