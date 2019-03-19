---
title: Azure API Management-transformatiebeleid | Microsoft Docs
description: Meer informatie over de transformatiebeleid beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: apimpm
ms.openlocfilehash: 72348085a69746306e40029bc7473df271b60221
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58105281"
---
# <a name="api-management-transformation-policies"></a>API Management-beleidsregels voor transformatie
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleid. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).

##  <a name="TransformationPolicies"></a> Transformatiebeleid

-   [JSON converteren naar XML](api-management-transformation-policies.md#ConvertJSONtoXML) : zet aanvraag of antwoord body van JSON naar XML.

-   [XML converteren naar JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) : zet aanvraag of antwoord body van XML naar JSON.

-   [Zoeken en vervangen in de hoofdtekst van de tekenreeks](api-management-transformation-policies.md#Findandreplacestringinbody) - vindt u een aanvraag of antwoord subtekenreeks en vervangt deze door een andere subtekenreeks.

-   [URL's in de inhoud te maskeren](api-management-transformation-policies.md#MaskURLSContent) -herschrijft (maskers) koppelingen in het antwoord hoofdtekst zodat ze naar de equivalente koppeling via de gateway verwijzen.

-   [Instellen van back-endservice](api-management-transformation-policies.md#SetBackendService) -de back endservice voor een binnenkomende aanvraag wordt gewijzigd.

-   [Hoofdtekst instellen](api-management-transformation-policies.md#SetBody) -Hiermee stelt u de berichttekst voor binnenkomende en uitgaande aanvragen.

-   [Set HTTP-header](api-management-transformation-policies.md#SetHTTPheader) : een waarde toegewezen aan een bestaande antwoord en/of de aanvraagheader of voegt een nieuwe reactie en/of de aanvraag-header.

-   [Instellen van de queryreeks-parameter](api-management-transformation-policies.md#SetQueryStringParameter) - wordt toegevoegd, vervangt de waarde van of aanvraag queryreeks-parameter verwijderd.

-   [Herschrijven van URL's](api-management-transformation-policies.md#RewriteURL) -converteert van een aanvraag-URL van de openbare vorm aan het formulier werd verwacht door de webservice.

-   [Transformeer XML met behulp van een XSLT](api-management-transformation-policies.md#XSLTransform) -een XSL-transformatie is van toepassing op XML in de hoofdtekst van de aanvraag of antwoord.

##  <a name="ConvertJSONtoXML"></a> JSON converteren naar XML
 De `json-to-xml` beleid converteert de hoofdtekst van een aanvraag of antwoord van JSON naar XML.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false" parse-date="true | false"/>
```

### <a name="example"></a>Voorbeeld

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <outbound>
        <base />
        <json-to-xml apply="always" consider-accept-header="false" parse-date="false"/>
    </outbound>
</policies>
```

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|json-to-xml|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|toepassen|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> conversie - altijd - altijd van toepassing.<br />-inhoud type-json - converteren alleen als antwoord Content-Type-header geeft aan dat de aanwezigheid van JSON.|Ja|N/A|
|Houd rekening met-accepteren-header|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -true - conversie van toepassing als JSON is aangevraagd in de aanvraag Accept-header.<br />-ONWAAR - conversie altijd van toepassing.|Nee|true|
|parseren van datum|Als de waarde `false` datumwaarden gewoon tijdens transformatie worden gekopieerd|Nee|true|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** binnenkomende, uitgaande, bij fout

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="ConvertXMLtoJSON"></a> XML converteren naar JSON
 De `xml-to-json` beleid converteert de hoofdtekst van een aanvraag of antwoord van XML naar JSON. Dit beleid kan worden gebruikt om het moderniseren van API's op basis van XML-alleen back-end webservices.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>
```

### <a name="example"></a>Voorbeeld

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <outbound>
        <base />
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />
    </outbound>
</policies>
```

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|xml-to-json|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|type|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -javascript-vriendelijk - de geconverteerde JSON heeft een formulier beschrijvende voor JavaScript-ontwikkelaars.<br />de geconverteerde JSON weerspiegelt - direct - structuur van het oorspronkelijke XML-document.|Ja|N/A|
|toepassen|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -altijd - altijd converteren.<br />-inhoud-type-xml - converteren alleen als antwoord Content-Type-header geeft aan dat de aanwezigheid van XML.|Ja|N/A|
|Houd rekening met-accepteren-header|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -true - conversie van toepassing als XML is aangevraagd in de aanvraag Accept-header.<br />-ONWAAR - conversie altijd van toepassing.|Nee|true|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** binnenkomende, uitgaande, bij fout

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="Findandreplacestringinbody"></a> Zoeken en vervangen in de hoofdtekst van de tekenreeks
 De `find-and-replace` beleid vindt u een aanvraag of antwoord subtekenreeks en vervangt deze door een andere subtekenreeks.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<find-and-replace from="what to replace" to="replacement" />
```

### <a name="example"></a>Voorbeeld

```xml
<find-and-replace from="notebook" to="laptop" />
```

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|zoeken en vervangen|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|uit|De tekenreeks die moet worden gezocht.|Ja|N/A|
|tot|De vervangende tekenreeks. Geef een nul lengte van de vervangende tekenreeks als u wilt verwijderen van de zoektekenreeks.|Ja|N/A|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="MaskURLSContent"></a> Masker URL's in de inhoud
 De `redirect-content-urls` beleid herschrijft (maskers) koppelingen in de antwoordtekst zodat ze naar de equivalente koppeling via de gateway verwijzen. Gebruik in de sectie outbound opnieuw schrijven antwoord hoofdtekst koppelingen om ze verwijzen naar de gateway te maken. Gebruik in de sectie inbound voor een tegenovergestelde effect.

> [!NOTE]
>  Dit beleid verandert niet alle headerwaarden, zoals `Location` headers. Om headerwaarden te wijzigen, gebruikt u de [set-header](api-management-transformation-policies.md#SetHTTPheader) beleid.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<redirect-content-urls />
```

### <a name="example"></a>Voorbeeld

```xml
<redirect-content-urls />
```

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|redirect-content-urls|Root-element.|Ja|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="SetBackendService"></a> Back-end-service instellen
 Gebruik de `set-backend-service` beleid een binnenkomende aanvraag omleiden naar een andere back-end dan de versie die is opgegeven in de API-instellingen voor deze bewerking. Dit beleid wijzigt de back-end-service basis-URL van de inkomende aanvraag naar de versie die is opgegeven in het beleid.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<set-backend-service base-url="base URL of the backend service" />
```

of

```xml
<set-backend-service backend-id="identifier of the backend entity specifying base URL of the backend service" />
```

> [!NOTE]
> Back-end-entiteiten kunnen worden beheerd via beheer [API](https://docs.microsoft.com/en-us/rest/api/apimanagement/backend) en [PowerShell](https://www.powershellgallery.com/packages?q=apimanagement).

### <a name="example"></a>Voorbeeld

```xml
<policies>
    <inbound>
        <choose>
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">
                <set-backend-service base-url="http://contoso.com/api/8.2/" />
            </when>
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">
                <set-backend-service base-url="http://contoso.com/api/9.1/" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
In dit voorbeeld leidt het beleid instellen back-end-service aanvragen op basis van de versiewaarde in de query-tekenreeks doorgegeven aan een andere back-endservice dan degene die is opgegeven in de API.

In eerste instantie is de back-end-service basis-URL afgeleid van de instellingen voor de API. Dus de aanvraag-URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` wordt `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` waar `http://contoso.com/api/10.4/` is de back-end-service-URL opgegeven in de API-instellingen.

Wanneer de [< Kies\> ](api-management-advanced-policies.md#choose) beleidsverklaring wordt toegepast. de back-end-service basis-URL kan worden gewijzigd opnieuw toekennen aan `http://contoso.com/api/8.2` of `http://contoso.com/api/9.1`, afhankelijk van de waarde van de queryparameter van de versie-aanvraag. Bijvoorbeeld, als de waarde is `"2013-15"` de laatste aanvraag URL wordt `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.

Als meer transformatie van de aanvraag gewenste, andere is [transformatiebeleid](api-management-transformation-policies.md#TransformationPolicies) kan worden gebruikt. Bijvoorbeeld, om te verwijderen van de queryparameter versie nu dat de aanvraag wordt doorgestuurd naar een versie specifieke back-end, de [queryreeks-parameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) beleid kan worden gebruikt voor het versiekenmerk nu redundante verwijderen.

### <a name="example"></a>Voorbeeld

```xml
<policies>
    <inbound>
        <set-backend-service backend-id="my-sf-service" sf-partition-key="@(context.Request.Url.Query.GetValueOrDefault("userId","")" sf-replica-type="primary" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
In dit voorbeeld stuurt het beleid de aanvraag door naar een service fabric-back-end, met behulp van de gebruikers-id query-tekenreeks als de partitiesleutel en het gebruik van de primaire replica van de partitie.

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|set-backend-service|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|basis-url|Nieuwe back-end-service basis-URL.|Een van de `base-url` of `backend-id` moet aanwezig zijn.|N/A|
|backend-id|Id van de back-end om naar te routeren. (Back-end-entiteiten worden beheerd via [API](https://docs.microsoft.com/en-us/rest/api/apimanagement/backend) en [PowerShell](https://www.powershellgallery.com/packages?q=apimanagement).)|Een van de `base-url` of `backend-id` moet aanwezig zijn.|N/A|
|sf-partition-key|Alleen van toepassing wanneer de back-end een Service Fabric-service is en is opgegeven met behulp van back-end-id. Gebruikt voor het omzetten van een specifieke partitie van de name resolution-service.|Nee|N/A|
|sf-replica-type|Alleen van toepassing wanneer de back-end een Service Fabric-service is en is opgegeven met behulp van back-end-id. Bepaalt of de aanvraag moet gaan naar de primaire of secundaire replica van een partitie. |Nee|N/A|
|sf-resolve-condition|Alleen van toepassing wanneer de back-end een Service Fabric-service is. Als de aanroep van Service Fabric-back-end moet worden herhaald met een resolutie van nieuwe identificeren-voorwaarde.|Nee|N/A|
|sf-service-instance-name|Alleen van toepassing wanneer de back-end een Service Fabric-service is. Hiermee kunt u het wijzigen van de service-exemplaren tijdens runtime. |Nee|N/A|
|sf-listener-name|Alleen van toepassing wanneer de back-end een Service Fabric-service is en is opgegeven met behulp van back-end-id. Service Fabric Reliable Services kunt u meerdere listeners in een service maken. Dit kenmerk wordt gebruikt om te selecteren van een specifieke listener wanneer een back-end van de betrouwbare Service meer dan één listener heeft. Als dit kenmerk niet opgegeven is, probeert API Management te gebruiken van een listener zonder een naam. Een listener zonder naam is normaal voor Reliable Services die slechts één listener hebt. |Nee|N/A|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomende, back-end

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="SetBody"></a> Hoofdtekst instellen
 Gebruik de `set-body` beleid om in te stellen de berichttekst voor binnenkomende en uitgaande aanvragen. Voor toegang tot de berichttekst kunt u de `context.Request.Body` eigenschap of de `context.Response.Body`, afhankelijk van of het beleid is in de sectie binnenkomend of uitgaand.

> [!IMPORTANT]
>  Houd er rekening mee dat standaard bij het openen van het bericht hoofdtekst met behulp van `context.Request.Body` of `context.Response.Body`, de hoofdtekst van het oorspronkelijke bericht is verloren en moet worden ingesteld door de instantie terug in de expressie worden geretourneerd. Als u wilt behouden in de hoofdtekst, stel de `preserveContent` parameter `true` bij het openen van het bericht. Als `preserveContent` is ingesteld op `true` en een andere instantie wordt geretourneerd door de expressie de hoofdtekst van het geretourneerde wordt gebruikt.
> 
>  Houd er rekening mee de volgende overwegingen wanneer u de `set-body` beleid.
> 
> - Als u de `set-body` beleid om te retourneren van een nieuwe of bijgewerkte instantie die u niet nodig om in te stellen `preserveContent` naar `true` omdat u de inhoud van de nieuwe expliciet levert.
>   -   Behoud van de inhoud van een antwoord in de inkomende pijplijn zin geen omdat er nog geen reactie is.
>   -   Behoud van de inhoud van een aanvraag in de uitgaande pijplijn zin geen omdat de aanvraag is al is verzonden naar de back-end op dit moment.
>   -   Als dit beleid wordt gebruikt wanneer er geen hoofdtekst van het bericht, bijvoorbeeld in een inkomende GET, een uitzondering opgetreden.

 Zie voor meer informatie de `context.Request.Body`, `context.Response.Body`, en de `IMessage` secties in het [contextvariabele](api-management-policy-expressions.md#ContextVariables) tabel.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<set-body>new body value as text</set-body>
```

### <a name="examples"></a>Voorbeelden

#### <a name="literal-text-example"></a>Voorbeeld van de letterlijke tekst

```xml
<set-body>Hello world!</set-body>
```

#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a>Voorbeeld van de toegang tot de hoofdtekst als een tekenreeks. Houd er rekening mee dat we de hoofdtekst van de oorspronkelijke aanvraag worden behouden zodat we deze later in de pijplijn.

```xml
<set-body>
@{ 
    string inBody = context.Request.Body.As<string>(preserveContent: true); 
    if (inBody[0] =='c') { 
        inBody[0] = 'm'; 
    } 
    return inBody; 
}
</set-body>
```

#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accessing-it-later-in-the-pipeline-will-result-in-an-exception"></a>Voorbeeld van de toegang tot de hoofdtekst als een jobject is leeggemaakt. Houd er rekening mee dat sinds we niet van de hoofdtekst van de oorspronkelijke aanvraag reserveren zijn, er toegang toe hebben verderop in de pijplijn in een uitzondering resulteren zal.

```xml
<set-body> 
@{ 
    JObject inBody = context.Request.Body.As<JObject>(); 
    if (inBody.attribute == <tag>) { 
        inBody[0] = 'm'; 
    } 
    return inBody.ToString(); 
} 
</set-body>

```

#### <a name="filter-response-based-on-product"></a>Filteren op basis van product
 In dit voorbeeld laat zien hoe om uit te voeren met het filteren van inhoud door het verwijderen van elementen uit het antwoord ontvangen van de back endservice bij het gebruik van de `Starter` product. Zie voor een demonstratie van configureren en gebruiken van dit beleid, [Cloud Cover aflevering 177: Meer functies van API Management met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en spoel vooruit naar 34:30. Beginnen bij 31:50 voor een overzicht van [de donkere Sky prognose API](https://developer.forecast.io/) gebruikt voor deze demo.

```xml
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->
<choose>
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">
    <set-body>@{
        var response = context.Response.Body.As<JObject>();
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {
          response.Property (key).Remove ();
        }
        return response.ToString();
      }
    </set-body>
  </when>
</choose>
```

### <a name="using-liquid-templates-with-set-body"></a>Met behulp van Liquid sjablonen met hoofdtekst instellen
De `set-body` beleid kan worden geconfigureerd voor het gebruik van de [Liquid](https://shopify.github.io/liquid/basics/introduction/) templating taal het transformeren van de hoofdtekst van een aanvraag of antwoord. Dit kan zeer effectief zijn als u geheel nieuwe vorm geven de indeling van het bericht moet zijn.

> [!IMPORTANT]
> De implementatie van een vloeistof die wordt gebruikt de `set-body` beleid is geconfigureerd in de 'C#-modus'. Dit is vooral belangrijk bij het uitvoeren van handelingen zoals filteren. Een voorbeeld: met behulp van een filter vereist het gebruik van Pascal en kleine letters en C# datum bijvoorbeeld opmaak:
>
> {{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}

> [!IMPORTANT]
> Als u wilt correct binden aan een XML-instantie met de Liquid sjabloon, gebruiken een `set-header` beleid Content-Type instellen naar de application/xml, tekst/xml (of een type dat eindigt op + xml); voor een JSON-hoofdtekst moet application/json, tekst/json (of een type dat eindigt op + JSON).

#### <a name="convert-json-to-soap-using-a-liquid-template"></a>JSON converteren naar SOAP met behulp van een vloeistof sjabloon
```xml
<set-body template="liquid">
    <soap:Envelope xmlns="http://tempuri.org/" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <GetOpenOrders>
                <cust>{{body.getOpenOrders.cust}}</cust>
            </GetOpenOrders>
        </soap:Body>
    </soap:Envelope>
</set-body>
```

#### <a name="transform-json-using-a-liquid-template"></a>Met behulp van een vloeistof sjabloon JSON transformeren
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|hoofdtekst instellen|Root-element. Bevat de platte tekst of een expressies waarmee een instantie wordt geretourneerd.|Ja|

### <a name="properties"></a>Properties

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|sjabloon|Gebruikt de templating van die het beleid van de hoofdtekst van de set wordt uitgevoerd in de modus te wijzigen. Momenteel is de enige ondersteunde waarde:<br /><br />-liquid - het beleid van de hoofdtekst van de set wordt de liquid templating-engine gebruiken |Nee|Liquid|

Voor toegang tot informatie over de aanvraag en respons, kan de Liquid sjabloon worden verbonden met een context-object met de volgende eigenschappen: <br />
<pre>context.
    Request.
        Url
        Method
        OriginalMethod
        OriginalUrl
        IpAddress
        MatchedParameters
        HasBody
        ClientCertificates
        Headers

    Response.
        StatusCode
        Method
        Headers
Url.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString

OriginalUrl.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString
</pre>



### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="SetHTTPheader"></a> Set HTTP-header
 De `set-header` beleid wordt een waarde toegewezen aan een bestaande antwoord en/of de aanvraagheader of voegt een nieuwe reactie en/of de aanvraag-header.

 Voegt een lijst met HTTP-headers in een HTTP-bericht. Wanneer in een inkomende pijplijn geplaatst, wordt dit beleid de HTTP-headers voor de aanvraag wordt doorgegeven aan de doelservice. Wanneer in een uitgaande pijplijn geplaatst, wordt in dit beleid de HTTP-headers voor de reactie wordt verzonden naar de client van de gateway ingesteld.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<set-header name="header name" exists-action="override | skip | append | delete">
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->
</set-header>
```

### <a name="examples"></a>Voorbeelden

#### <a name="example"></a>Voorbeeld

```xml
<set-header name="some header name" exists-action="override">
    <value>20</value>
</set-header>
```

#### <a name="forward-context-information-to-the-backend-service"></a>Doorsturen van contextgegevens naar de back-endservice
 In dit voorbeeld laat zien hoe om toe te passen op de API-niveau contextinformatie naar de back endservice opgeven. Zie voor een demonstratie van configureren en gebruiken van dit beleid, [Cloud Cover aflevering 177: Meer functies van API Management met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en spoel vooruit naar 10:30. Bij 12:10 is er een demo van het aanroepen van een bewerking in de portal voor ontwikkelaars, waar u het beleid op het werk kunt zien.

```xml
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->
<set-header name="x-request-context-data" exists-action="override">
  <value>@(context.User.Id)</value>
  <value>@(context.Deployment.Region)</value>
</set-header>
```

 Zie voor meer informatie, [beleidsexpressies](api-management-policy-expressions.md) en [contextvariabele](api-management-policy-expressions.md#ContextVariables).

> [!NOTE]
> Meerdere waarden van een koptekst worden samengevoegd met een CSV-tekenreeks, bijvoorbeeld: `headerName: value1,value2,value3`
>
> Uitzonderingen bestaan uit gestandaardiseerde kopteksten, welke waarden:
> - komma's kunnen bevatten (`User-Agent`, `WWW-Authenticate`, `Proxy-Authenticate`),
> - datum kan bevatten (`Cookie`, `Set-Cookie`, `Warning`),
> - datum bevatten (`Date`, `Expires`, `If-Modified-Since`, `If-Unmodified-Since`, `Last-Modified`, `Retry-After`).
>
> In het geval van deze uitzonderingen, meerdere headerwaarden zal niet worden samengevoegd tot één tekenreeks en worden doorgegeven als afzonderlijke headers, bijvoorbeeld: `User-Agent: value1`
>`User-Agent: value2`
>`User-Agent: value3`

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|set-header|Root-element.|Ja|
|waarde|Hiermee geeft u de waarde van de header moet worden ingesteld. Voor meerdere headers met dezelfde naam voegt u extra `value` elementen.|Ja|

### <a name="properties"></a>Properties

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de koptekst is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -override - vervangt de waarde van de bestaande koptekst.<br />-skip: is geen vervanging voor de bestaande waarde voor header.<br />-append - de waarde wordt toegevoegd aan de bestaande waarde voor header.<br />-delete - verwijdert de header van de aanvraag.<br /><br /> Als de waarde `override` meerdere vermeldingen met dezelfde naam opnemen resulteert in de koptekst wordt ingesteld op basis van alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|
|naam|Hiermee geeft u de naam van de header moet worden ingesteld.|Ja|N/A|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="SetQueryStringParameter"></a> Queryreeks-setparameter
 De `set-query-parameter` beleid wordt toegevoegd, vervangt de waarde van, of verwijderingen vraag queryreeks-parameter. Kan worden gebruikt voor het doorgeven van parameters verwacht door de back endservice die zijn optioneel query of nooit die aanwezig zijn in de aanvraag.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<set-query-parameter name="param name" exists-action="override | skip | append | delete">
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->
</set-query-parameter>
```

### <a name="examples"></a>Voorbeelden

#### <a name="example"></a>Voorbeeld

```xml

<set-query-parameter>
  <parameter name="api-key" exists-action="skip">
    <value>12345678901</value>
  </parameter>
  <!-- for multiple parameters with the same name add additional value elements -->
</set-query-parameter>

```

#### <a name="forward-context-information-to-the-backend-service"></a>Doorsturen van contextgegevens naar de back-endservice
 In dit voorbeeld laat zien hoe om toe te passen op de API-niveau contextinformatie naar de back endservice opgeven. Zie voor een demonstratie van configureren en gebruiken van dit beleid, [Cloud Cover aflevering 177: Meer functies van API Management met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en spoel vooruit naar 10:30. Bij 12:10 is er een demo van het aanroepen van een bewerking in de portal voor ontwikkelaars, waar u het beleid op het werk kunt zien.

```xml
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->
<set-query-parameter name="x-product-name" exists-action="override">
  <value>@(context.Product.Name)</value>
</set-query-parameter>

```

 Zie voor meer informatie, [beleidsexpressies](api-management-policy-expressions.md) en [contextvariabele](api-management-policy-expressions.md#ContextVariables).

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|set-query-parameter|Root-element.|Ja|
|waarde|Hiermee geeft u de waarde van de query-parameter moet worden ingesteld. Voor meerdere queryparameters met dezelfde naam voegt u extra `value` elementen.|Ja|

### <a name="properties"></a>Properties

|Name|Description|Vereist|Standaard|
|----------|-----------------|--------------|-------------|
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de queryparameter is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -override - vervangt de waarde van de bestaande parameter.<br />-skip: de waarde van de bestaande query-parameter niet vervangen.<br />-append - de waarde wordt toegevoegd aan de bestaande waarde van de queryparameter.<br />-delete - verwijdert de queryparameter van de aanvraag.<br /><br /> Als de waarde `override` meerdere vermeldingen met dezelfde naam opnemen resulteert in de queryparameter wordt ingesteld op basis van alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|
|naam|Hiermee geeft u de naam van de query-parameter moet worden ingesteld.|Ja|N/A|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomende, back-end

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="RewriteURL"></a> Herschrijven van URL 's
 De `rewrite-uri` beleid omgezet in een aanvraag-URL van het formulier voor openbare van het formulier werd verwacht door de webservice, zoals wordt weergegeven in het volgende voorbeeld.

- Openbare URL- `http://api.example.com/storenumber/ordernumber`

- Aanvraag-URL: `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`

  Dit beleid kan worden gebruikt wanneer een menselijke en/of beschrijvende browser-URL moet worden omgezet in de URL-indeling verwacht door de webservice. Dit beleid moet alleen worden toegepast als een alternatieve URL-indeling, zoals nieuwe URL's, RESTful URL's, gebruikersvriendelijke URL's of SEO-vriendelijk-URL's die zijn uitsluitend structurele URL's die geen querytekenreeks bevatten en alleen het pad van de resource (in plaats daarvan bevatten om vrij te geven Nadat het schema en de instantie). Dit wordt vaak gedaan voor fraaie uiterlijk, bruikbaarheid of zoekmachine optimaliseringsdoeleinden (SEO).

> [!NOTE]
>  U kunt queryreeksparameters met behulp van het beleid alleen toevoegen. U kunt geen extra sjabloon padparameters toevoegen in de URL opnieuw schrijven.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />
```

### <a name="example"></a>Voorbeeld

```xml
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/put" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
<!-- Resulting URL will be /put?c=d -->
```
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/put" copy-unmatched-params="false" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
<!-- Resulting URL will be /put -->
```

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|herschrijven-uri|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Standaard|
|---------------|-----------------|--------------|-------------|
|sjabloon|De werkelijke web service-URL met een queryreeksparameters. Wanneer u expressies, is de hele waarde moet een expressie.|Ja|N/A|
|kopiëren-niet-overeenkomende parameters|Hiermee geeft u op of queryparameters in de inkomende aanvraag komt niet in de oorspronkelijke URL-sjabloon worden toegevoegd aan de URL die is gedefinieerd door de sjabloon opnieuw schrijven|Nee|true|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

##  <a name="XSLTransform"></a> Met behulp van een XSLT XML transformeren
 De `Transform XML using an XSLT` beleid geldt een XSL-transformatie voor XML in de hoofdtekst van de aanvraag of antwoord.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<xsl-transform>
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
        <xsl:output method="xml" indent="yes" />
        <xsl:param name="User-Agent" />
        <xsl:template match="* | @* | node()">
            <xsl:copy>
                <xsl:if test="self::* and not(parent::*)">
                    <xsl:attribute name="User-Agent">
                        <xsl:value-of select="$User-Agent" />
                    </xsl:attribute>
                </xsl:if>
                <xsl:apply-templates select="* | @* | node()" />
            </xsl:copy>
        </xsl:template>
    </xsl:stylesheet>
  </xsl-transform>
```

### <a name="example"></a>Voorbeeld

```xml
<policies>
  <inbound>
      <base />
  </inbound>
  <outbound>
      <base />
      <xsl-transform>
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />
            <!-- Copy all nodes directly-->
            <xsl:template match="node()| @*|*">
                <xsl:copy>
                    <xsl:apply-templates select="@* | node()|*" />
                </xsl:copy>
            </xsl:template>
        </xsl:stylesheet>
    </xsl-transform>
  </outbound>
</policies>
```

### <a name="elements"></a>Elementen

|Name|Description|Vereist|
|----------|-----------------|--------------|
|XSL-transformatie|Root-element.|Ja|
|Parameter|Gebruikt voor het definiëren van variabelen die worden gebruikt bij de transformatie|Nee|
|xsl:stylesheet|Stylesheet hoofdelement. Ga als volgt de standaard alle elementen en kenmerken die zijn gedefinieerd binnen [XSLT-specificatie](https://www.w3.org/TR/xslt)|Ja|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)
