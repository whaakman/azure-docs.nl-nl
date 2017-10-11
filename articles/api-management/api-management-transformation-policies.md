---
title: Azure API Management-claimtransformatiebeleidsinstellingen | Microsoft Docs
description: Meer informatie over de claimtransformatiebeleidsinstellingen beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7406a8ce-5f9c-4fae-9b0f-e574befb2ee9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: c2bed904b82c569b28a6e00d0cc9b49107c148dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-transformation-policies"></a>API Management-beleidsregels voor transformatie
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="TransformationPolicies"></a>Claimtransformatiebeleidsinstellingen  
  
-   [JSON converteren naar XML](api-management-transformation-policies.md#ConvertJSONtoXML) : zet aanvraag of antwoord body van JSON naar XML.  
  
-   [XML niet converteren naar JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) : zet aanvraag of antwoord body van XML naar JSON.  
  
-   [Zoeken en vervangen tekenreeks in de hoofdtekst](api-management-transformation-policies.md#Findandreplacestringinbody) - zoeken naar een subtekenreeks aanvraag of antwoord en wordt vervangen door een andere subtekenreeks.  
  
-   [URL's in de inhoud te maskeren](api-management-transformation-policies.md#MaskURLSContent) -koppelingen herschrijft (maskers) in het antwoord body zodat deze naar de equivalente koppeling via de gateway verwijzen.  
  
-   [Stel de back-endservice](api-management-transformation-policies.md#SetBackendService) -wijzigingen van de back-endservice voor de binnenkomende aanvraag.  
  
-   [Instantie ingesteld](api-management-transformation-policies.md#SetBody) -Hiermee stelt u de berichttekst voor binnenkomende en uitgaande aanvragen.  
  
-   [Set HTTP-header](api-management-transformation-policies.md#SetHTTPheader) : een waarde wordt toegewezen aan een bestaande antwoord en/of de aanvraag-header of voegt een nieuwe antwoord en/of de aanvraag-header.  
  
-   [Querytekenreeksparameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) - wordt toegevoegd, vervangt de waarde van of verwijderd van de aanvraag queryreeksparameter opgeven.  
  
-   [Herschrijven van URL](api-management-transformation-policies.md#RewriteURL) -converteert van een aanvraag-URL van de openbare vorm aan het formulier dat werd verwacht door de webservice.  
  
-   [XML met behulp van een XSLT transformeren](api-management-transformation-policies.md#XSLTransform) -geldt een XSL-transformatie voor XML in de hoofdtekst van de aanvraag of antwoord.  
  
##  <a name="ConvertJSONtoXML"></a>JSON naar XML converteren  
 De `json-to-xml` beleid converteert de hoofdtekst van een aanvraag of antwoord van JSON naar XML.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <json-to-xml apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|JSON-to-xml|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|toepassen|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> conversie - altijd - altijd van toepassing.<br />convert-inhoud type-json - alleen als response Content-Type-header aanwezigheid van JSON aangeeft.|Ja|N.v.t.|  
|Overweeg-accepteren-header|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -waar - conversie van toepassing als JSON is aangevraagd in aanvraag Accept-header.<br />-ONWAAR - conversie altijd van toepassing.|Nee|De waarde True|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** binnenkomende, uitgaande, bij fouten  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="ConvertXMLtoJSON"></a>XML niet converteren naar JSON  
 De `xml-to-json` beleid converteert de hoofdtekst van een aanvraag of antwoord van XML naar JSON. Dit beleid kan worden gebruikt voor API's op basis van de back-end van een alleen-XML-webservices moderniseren.  
  
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
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|XML-json-|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|type|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -javascript-vriendelijk - de geconverteerde JSON heeft een formulier beschrijvende voor ontwikkelaars van JavaScript.<br />de geconverteerde JSON weerspiegelt - direct - structuur van het oorspronkelijke XML-document.|Ja|N.v.t.|  
|toepassen|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -altijd - altijd converteren.<br />convert - inhoud-type-xml - alleen als response Content-Type-header aanwezigheid van XML aangeeft.|Ja|N.v.t.|  
|Overweeg-accepteren-header|Het kenmerk moet worden ingesteld op een van de volgende waarden.<br /><br /> -waar - conversie van toepassing als XML is aangevraagd in aanvraag Accept-header.<br />-ONWAAR - conversie altijd van toepassing.|Nee|De waarde True|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** binnenkomende, uitgaande, bij fouten  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="Findandreplacestringinbody"></a>Zoeken en vervangen tekenreeks in de hoofdtekst  
 De `find-and-replace` beleid vindt u een aanvraag of antwoord subtekenreeks en vervangen door een andere subtekenreeks.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<find-and-replace from="what to replace" to="replacement" />  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|zoeken en vervangen|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|Van|De tekenreeks om naar te zoeken.|Ja|N.v.t.|  
|tot|De vervangende tekenreeks. Geef de vervangende tekenreekslengte van nul als u wilt verwijderen van de zoekreeks.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="MaskURLSContent"></a>Masker URL's in de inhoud  
 De `redirect-content-urls` beleid herschrijft koppelingen in de hoofdtekst van antwoord (maskers) zodat deze naar de equivalente koppeling via de gateway verwijzen. Gebruik in de sectie uitgaande antwoord hoofdtekst koppelingen zodat deze verwijzen naar de gateway opnieuw te schrijven. Gebruik in de binnenkomende sectie voor een tegenovergestelde effect.  
  
> [!NOTE]
>  Dit beleid verandert niet een headerwaarden, zoals `Location` headers. Koptekst om waarden te wijzigen, gebruikt u de [set-header](api-management-transformation-policies.md#SetHTTPheader) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|Omleidings-inhoud-URL 's|Hoofdelement.|Ja|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** binnenkomende, uitgaande  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="SetBackendService"></a>Back-end-service instellen  
 Gebruik de `set-backend-service` beleid een inkomende aanvraag omleiden naar een andere back-end dan de opgegeven in de API-instellingen voor de bewerking. Dit beleid verandert de back-end service basis-URL van de inkomende aanvraag in de versie opgegeven in het beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<set-backend-service base-url="base URL of the backend service" />  
```  
  
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
In dit voorbeeld routeert set back-end service beleid aanvragen op basis van de versiewaarde in de queryreeks doorgegeven aan een andere back-endservice dan een opgegeven in de API.
  
In eerste instantie is de back-end service basis-URL afgeleid van de instellingen voor de API. Dus de aanvraag-URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` wordt `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` waar `http://contoso.com/api/10.4/` is de back-end-service-URL opgegeven in de API-instellingen.  
  
Wanneer de [< kiezen\> ](api-management-advanced-policies.md#choose) beleidsverklaring wordt toegepast. de back-end service basis-URL mag wijzigen opnieuw toekennen aan `http://contoso.com/api/8.2` of `http://contoso.com/api/9.1`, afhankelijk van de waarde van de queryparameter van de versie-aanvraag. Bijvoorbeeld, als de waarde is `"2013-15"` de laatste aanvraag URL wordt `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.  
  
Als verdere transformatie van de aanvraag gewenste, andere is [claimtransformatiebeleidsinstellingen](api-management-transformation-policies.md#TransformationPolicies) kan worden gebruikt. Om bijvoorbeeld te verwijderen van de version-queryparameter nu dat de aanvraag wordt gerouteerd naar een versie specifieke back-end van de [querytekenreeksparameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) beleid kan worden gebruikt voor het versiekenmerk nu redundante verwijderen.  
  
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
In dit voorbeeld stuurt het beleid voor de aanvraag door naar een service fabric back-end van de queryreeks userId gebruikt als de partitiesleutel en de primaire replica van de partitie.  

### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|set-back-end-service|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|basis-url|Nieuwe back-end service basis-URL.|Nee|N.v.t.|  
|back-end-id|Id van de back-end om naar te routeren.|Nee|N.v.t.|  
|SF partitiesleutel|Alleen van toepassing wanneer de back-end een Service Fabric-service is en wordt opgegeven met behulp van back-end-id. Gebruikt voor het omzetten van een specifieke partitie van de service voor naamomzetting.|Nee|N.v.t.|  
|SF-replica-type|Alleen van toepassing wanneer de back-end een Service Fabric-service is en wordt opgegeven met behulp van back-end-id. Bepaalt of de aanvraag moet gaan naar de primaire of secundaire replica van een partitie. |Nee|N.v.t.|    
|SF-resolve-voorwaarde|Alleen van toepassing wanneer de back-end een Service Fabric-service is. Voorwaarde te identificeren als de aanroep naar Service Fabric-back-end moet worden herhaald met nieuwe resolutie.|Nee|N.v.t.|    
|SF-service-exemplaar-name|Alleen van toepassing wanneer de back-end een Service Fabric-service is. Kan service-exemplaren tijdens runtime te wijzigen. |Nee|N.v.t.|    

### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** back-end voor binnenkomend verkeer  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="SetBody"></a>Set-instantie  
 Gebruik de `set-body` in te stellen van de berichttekst voor binnenkomende en uitgaande aanvragen. Voor toegang tot de berichttekst kunt u de `context.Request.Body` eigenschap of de `context.Response.Body`, afhankelijk van of het beleid is in de sectie binnenkomend of uitgaand.  
  
> [!IMPORTANT]
>  Opmerking die standaard bij het openen van het bericht met behulp van body `context.Request.Body` of `context.Response.Body`, de oorspronkelijke berichttekst verloren en wordt door te retourneren van de hoofdtekst terug in de expressie moet worden ingesteld. Instellen dat de inhoud van de hoofdtekst wordt bewaard, de `preserveContent` -parameter voor `true` bij het openen van het bericht. Als `preserveContent` is ingesteld op `true` en een andere instantie wordt geretourneerd door de expressie de hoofdtekst van het geretourneerde wordt gebruikt.  
>   
>  Houd rekening met de volgende overwegingen wanneer u de `set-body` beleid.  
>   
>  -   Als u de `set-body` beleid om te retourneren van een nieuwe of bijgewerkte instantie die u hoeft niet in te stellen `preserveContent` naar `true` omdat u de inhoud van de nieuwe expliciet levert.  
> -   Behouden van de inhoud van een reactie op in de inkomende pijplijn logisch niet omdat er nog geen reactie is.  
> -   Behouden van de inhoud van een aanvraag in de uitgaande pijplijn logisch niet omdat de aanvraag al is verzonden naar de back-end op dit moment.  
> -   Als dit beleid wordt gebruikt wanneer er geen berichttekst, bijvoorbeeld in een inkomende GET een uitzondering opgetreden.  
  
 Zie voor meer informatie de `context.Request.Body`, `context.Response.Body`, en de `IMessage` secties in de [Context variabele](api-management-policy-expressions.md#ContextVariables) tabel.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a>Voorbeelden  
  
#### <a name="literal-text-example"></a>Voorbeeld van de letterlijke tekst  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a>Voorbeeld van de toegang tot de instantie als een tekenreeks. Houd er rekening mee dat we bewaard de oorspronkelijke aanvraagtekst blijven zodat we deze later in de pijplijn.
  
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
  
#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accesing-it-later-in-the-pipeline-will-result-in-an-exception"></a>Voorbeeld van de toegang tot de instantie een JObject. Merk op dat omdat we niet reserveren van de oorspronkelijke aanvraagtekst, toegang tot het verderop in de pijplijn zal leiden tot een uitzondering.  
  
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
  
#### <a name="filter-response-based-on-product"></a>Reactie op basis van product filteren  
 Dit voorbeeld ziet u hoe u inhoud filteren van door gegevenselementen te verwijderen uit het antwoord ontvangen van de back-endservice bij gebruik van de `Starter` product. Zie voor een demonstratie van configureren en gebruiken van dit beleid [Cloud hebben betrekking op aflevering 177: meer API-beheerfuncties met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit te 34:30. Start op 31:50 voor een overzicht van [de donker Sky Forecast API](https://developer.forecast.io/) gebruikt voor deze demonstratie.  
  
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

### <a name="using-liquid-templates-with-set-body"></a>Vloeibare sjablonen gebruiken bij set-instantie 
De `set-body` beleid kan worden geconfigureerd voor het gebruik van de [vloeibare](https://shopify.github.io/liquid/basics/introduction/) templating taal transfom de hoofdtekst van een aanvraag of antwoord. Dit kan zeer effectief zijn als u moet de indeling van uw bericht volledig vorm zijn.

> [!IMPORTANT]
> De implementatie van vloeistof gebruikt in de `set-body` beleid is geconfigureerd in de 'C#-modus'. Dit is vooral belangrijk bij het uitvoeren van bewerkingen zoals filteren. Als u bijvoorbeeld met een datumfilter vereist het gebruik van Pascal hoofd- en C#-datum notatie, bijvoorbeeld:
>
> {{body.foo.startDateTime| Datum: 'yyyyMMddTHH:mm:ddZ'}}

> [!IMPORTANT]
> Gebruiken om correct binden aan een XML-hoofdtekst met de vloeibare sjabloon, een `set-header` beleid Content-Type instellen op application/xml, text/xml (of een type eindigend op + xml), een JSON-hoofdtekst, het application/json, text/json (of een type eindigend op + json) moet worden.

#### <a name="convert-json-to-soap-using-a-liquid-template"></a>JSON naar SOAP met een vloeibare sjabloon converteren
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

#### <a name="tranform-json-using-a-liquid-template"></a>Tranform JSON met een vloeibare sjabloon
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|set-instantie|Hoofdelement. Bevat de platte tekst of een expressies die een instantie retourneert.|Ja|  

### <a name="properties"></a>Eigenschappen  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|sjabloon|Gebruikt de modus templating die het beleid instellen instantie wordt uitgevoerd te wijzigen. Momenteel is de enige ondersteunde waarde:<br /><br />-vloeibare - de hoofdtekst-beleid instellen gebruikt de vloeibare templating-engine |Nee|vloeistof|  

Voor toegang tot informatie over de aanvraag en antwoord, kan de vloeibare sjabloon worden verbonden met een context-object met de volgende eigenschappen: <br />
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
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="SetHTTPheader"></a>Set HTTP-header  
 De `set-header` beleid wordt een waarde toegewezen aan een bestaande antwoord en/of de aanvraag-header of voegt een nieuwe antwoord en/of de aanvraag-header.  
  
 Voegt een lijst met HTTP-headers in een HTTP-bericht. Wanneer in een inkomende pijplijn geplaatst, wordt de HTTP-headers voor de aanvraag wordt doorgegeven aan de doelservice ingesteld in dit beleid. Wanneer in een uitgaande pijplijn geplaatst, wordt in dit beleid wordt de HTTP-headers voor de reactie wordt verzonden naar de gateway-client ingesteld.  
  
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
  
#### <a name="forward-context-information-to-the-backend-service"></a>Contextinformatie naar de back-endservice doorsturen  
 In dit voorbeeld laat zien hoe beleid op het niveau van de API op te geven contextinformatie voor de back-endservice toepassen. Zie voor een demonstratie van configureren en gebruiken van dit beleid [Cloud hebben betrekking op aflevering 177: meer API-beheerfuncties met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit tot en met 10:30. Bij 12:10 is er een demo voor het aanroepen van een bewerking in de portal voor ontwikkelaars, waar u het beleid op het werk kunt zien.  
  
```xml  
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 Zie voor meer informatie [beleidsexpressies](api-management-policy-expressions.md) en [Context variabele](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|set-header|Hoofdelement.|Ja|  
|waarde|Hiermee geeft u de waarde van de header moet worden ingesteld. Voor meerdere headers met dezelfde naam extra toevoegen `value` elementen.|Ja|  
  
### <a name="properties"></a>Eigenschappen  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de header is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -onderdrukking - vervangt de waarde van de bestaande koptekst.<br />de waarde van de bestaande header vervangen - skip - niet.<br />-toevoeg - de waarde toegevoegd aan de bestaande headerwaarde.<br />-delete - verwijdert de header van de aanvraag.<br /><br /> Als de waarde `override` opnemen van meerdere vermeldingen met dezelfde naam resulteert in de koptekst wordt ingesteld in overeenstemming met alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|  
|naam|Hiermee geeft u de naam van de header moet worden ingesteld.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="SetQueryStringParameter"></a>Querytekenreeksparameter instellen  
 De `set-query-parameter` beleid wordt toegevoegd, vervangt de waarde van, of verwijderingen querytekenreeksparameter aanvragen. Kan worden gebruikt voor het doorgeven van de query parameters verwacht door de back endservice die zijn optioneel of nooit voorkomen in de aanvraag.  
  
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
  
#### <a name="forward-context-information-to-the-backend-service"></a>Contextinformatie naar de back-endservice doorsturen  
 In dit voorbeeld laat zien hoe beleid op het niveau van de API op te geven contextinformatie voor de back-endservice toepassen. Zie voor een demonstratie van configureren en gebruiken van dit beleid [Cloud hebben betrekking op aflevering 177: meer API-beheerfuncties met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit tot en met 10:30. Bij 12:10 is er een demo voor het aanroepen van een bewerking in de portal voor ontwikkelaars, waar u het beleid op het werk kunt zien.  
  
```xml  
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 Zie voor meer informatie [beleidsexpressies](api-management-policy-expressions.md) en [Context variabele](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|query-set-parameter|Hoofdelement.|Ja|  
|waarde|Hiermee geeft u de waarde van de query-parameter moet worden ingesteld. Voor meerdere queryparameters met dezelfde naam extra toevoegen `value` elementen.|Ja|  
  
### <a name="properties"></a>Eigenschappen  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de queryparameter is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -onderdrukking - vervangt de waarde van de bestaande parameter.<br />-skip: de waarde van de bestaande query-parameter niet vervangen.<br />-toevoeg - voegt u de waarde met de waarde van de bestaande query.<br />de queryparameter verwijdert - delete - uit de aanvraag.<br /><br /> Als de waarde `override` opnemen van meerdere vermeldingen met dezelfde naam resulteert in de query-parameter worden ingesteld in overeenstemming met alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|  
|naam|Hiermee geeft u de naam van de queryparameter moet worden ingesteld.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** back-end voor binnenkomend verkeer  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
##  <a name="RewriteURL"></a>Herschrijven van URL 's  
 De `rewrite-uri` beleid omgezet in een aanvraag-URL van de openbare vorm het formulier dat werd verwacht door de webservice, zoals wordt weergegeven in het volgende voorbeeld.  
  
-   Openbare URL-`http://api.example.com/storenumber/ordernumber`  
  
-   Aanvraag-URL-`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`  
  
 Dit beleid kan worden gebruikt wanneer een menselijke en/of beschrijvende browser-URL moet worden omgezet in de URL-indeling verwacht door de webservice. Dit beleid moet alleen worden toegepast bij het blootstellen van een andere URL-indeling, zoals schone URL's, RESTful-URL's, gebruiksvriendelijke URL's of Zoekmachineoptimalisatie gebruiksvriendelijke URL's die zijn uitsluitend structurele URL's die niet een queryreeks bevatten en in plaats daarvan alleen het pad van de resource (na het schema en de autoriteit) bevatten. Dit wordt vaak gedaan voor fraaie uiterlijk, bruikbaarheid of zoekmachine optimaliseringsdoeleinden (Zoekmachineoptimalisatie).  
  
> [!NOTE]
>  U kunt alleen queryreeksparameters met het beleid toevoegen. U kunt geen extra Sjabloonparameters voor pad toevoegen in de URL herschrijven.  

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
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|Herschrijf-uri|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|sjabloon|De werkelijke web service-URL met een queryreeks-parameters. Wanneer u expressies gebruikt, is het gehele getal moet een expressie.|Ja|N.v.t.|  
|kopiëren niet-overeenkomende parameters|Hiermee geeft u op of de binnenkomende aanvraag niet aanwezig in de oorspronkelijke URL sjabloon queryparameters worden toegevoegd aan de URL die is gedefinieerd door de sjabloon opnieuw schrijven|Nee|De waarde True|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
  
-   **Beleid scopes:** product, API-bewerking  
  
##  <a name="XSLTransform"></a>XML met behulp van een XSLT transformeren  
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
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|XSL-transformatie|Hoofdelement.|Ja|  
|Parameter|Gebruikt voor het definiëren van variabelen die worden gebruikt in de transformatie|Nee|  
|xsl: Stylesheet|Hoofdelement opmaakmodel. Ga als volgt de standaard alle elementen en kenmerken die zijn gedefinieerd binnen [XSLT-specificatie](http://www.w3.org/TR/xslt)|Ja|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** binnenkomende, uitgaande  
  
-   **Beleid scopes:** wereldwijd, product, API, bewerking  
  
## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie werken met beleid [-beleid in API Management](api-management-howto-policies.md).  
