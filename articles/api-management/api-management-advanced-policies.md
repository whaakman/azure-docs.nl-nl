---
title: Azure API Management Geavanceerde beleidsregels | Microsoft Docs
description: Meer informatie over de geavanceerde beleidsregels beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: b8c181282dd28582a8fb02f611424ffd608fd1ec
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="api-management-advanced-policies"></a>API Management Geavanceerde beleidsregels
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AdvancedPolicies"></a>Geavanceerde beleidsregels  
  
-   [Transportbesturing](api-management-advanced-policies.md#choose) - voorwaardelijk is van toepassing op basis van de resultaten van de evaluatie van Booleaanse waarde beleidsverklaringen [expressies](api-management-policy-expressions.md).  
-   [Doorsturen van aanvraag](#ForwardRequest) -stuurt de aanvraag naar de back-endservice.
-   [Gelijktijdigheid beperken](#LimitConcurrency) -voorkomt u dat beleid uit door meer dan het opgegeven aantal aanvragen wordt uitgevoerd op een tijdstip ingesloten.
-   [Logboek naar Event Hub](#log-to-eventhub) -berichten in de opgegeven indeling verzendt naar een Event Hub gedefinieerd door een entiteit berichtenlogboek. 
-   [Antwoord model](#mock-response) -pipeline-uitvoering wordt afgebroken en retourneert een mocked antwoord rechtstreeks aan de aanroeper.
-   [Probeer](#Retry) -uitvoering van de ingesloten beleidsverklaringen pogingen als en pas de voorwaarde wordt voldaan. Uitvoering wordt herhaald op de opgegeven tijdsintervallen en tot het opgegeven aantal nieuwe pogingen.  
-   [Antwoord retourneren](#ReturnResponse) -pipeline-uitvoering wordt afgebroken en retourneert het opgegeven antwoord rechtstreeks naar de aanroeper. 
-   [Eenzijdige aanvraag verzenden](#SendOneWayRequest) -verzendt een aanvraag naar de opgegeven URL zonder te wachten op reactie.  
-   [Aanvraag verzenden](#SendRequest) -verzendt een aanvraag naar de opgegeven URL.  
-   [HTTP-proxy ingesteld](#SetHttpProxy) -kunt u de aanvragen van de route die zijn doorgestuurd via een HTTP-proxy.  
-   [Stel aanvraagmethode](#SetRequestMethod) -kunt u de HTTP-methode voor een aanvraag wijzigen.  
-   [Statuscode ingesteld](#SetStatus) -wijzigingen van de HTTP-statuscode in de opgegeven waarde.  
-   [Variabele instellen](api-management-advanced-policies.md#set-variable) -zich blijft voordoen een waarde in een benoemde [context](api-management-policy-expressions.md#ContextVariables) variabele voor latere toegang.  
-   [Tracering](#Trace) -voegt een tekenreeks in de [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) uitvoer.  
-   [Wacht](#Wait) -wacht voor ingesloten [Verzendaanvraag](api-management-advanced-policies.md#SendRequest), [waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey), of [transportbesturing](api-management-advanced-policies.md#choose) beleid om te voltooien voordat u doorgaat.  
  
##  <a name="choose"></a>Controlestroom  
 De `choose` beleid van toepassing is ingesloten beleid op basis van het resultaat van evaluatie van Booleaanse expressies, vergelijkbaar met een if-then-else of een switch instructies in een programmeertaal maken.  
  
###  <a name="ChoosePolicyStatement"></a>Beleidsverklaring  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements to be applied if none of the above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 Het beleid voor toegangsbeheer stroom moet ten minste één bevatten `<when/>` element. De `<otherwise/>` element is optioneel. Voorwaarden `<when/>` elementen worden geëvalueerd in de volgorde van de weergave in het beleid. Beleid voor instructie (s) die tussen de eerste `<when/>` element met kenmerk gelijk is aan voorwaarde `true` worden toegepast. Beleidsregels die tussen de `<otherwise/>` element, indien aanwezig, wordt toegepast als alle van de `<when/>` voorwaarde elementkenmerken zijn `false`.  
  
### <a name="examples"></a>Voorbeelden  
  
####  <a name="ChooseExample"></a>Voorbeeld  
 Het volgende voorbeeld toont een [variabele instellen](api-management-advanced-policies.md#set-variable) beleid en twee beleidsregels voor beheer-stroom.  
  
 De variabele beleid instellen in de sectie inkomende en genereert een `isMobile` Booleaanse [context](api-management-policy-expressions.md#ContextVariables) variabele die is ingesteld op true als de `User-Agent` aanvraag-header bevat de tekst `iPad` of `iPhone`.  
  
 Het eerste beleid voor toegangsbeheer stroom is ook in de sectie inkomende en voorwaardelijk wordt een van twee [querytekenreeksparameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) beleidsregels, afhankelijk van de waarde van de `isMobile` context-variabele.  
  
 Het tweede beleid voor toegangsbeheer stroom is in de sectie uitgaande en voorwaardelijk geldt de [XML converteren naar JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) beleid wanneer `isMobile` is ingesteld op `true`.  
  
```xml  
<policies>  
    <inbound>  
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
        <base />  
        <choose>  
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>true</value>  
                </set-query-parameter>  
            </when>  
            <otherwise>  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>false</value>  
                </set-query-parameter>  
            </otherwise>  
        </choose>  
    </inbound>  
    <outbound>  
        <base />  
        <choose>  
            <when condition="@(context.GetValueOrDefault<bool>("isMobile"))">  
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>  
            </when>  
        </choose>  
    </outbound>  
</policies>  
```  
  
#### <a name="example"></a>Voorbeeld  
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
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|Kies|Hoofdelement.|Ja|  
|Wanneer|De voorwaarde moet worden gebruikt voor de `if` of `ifelse` delen van de `choose` beleid. Als de `choose` beleid heeft meerdere `when` secties, ze worden opeenvolgend geëvalueerd. Eenmaal de `condition` van een wanneer element resulteert in `true`, niet verder `when` voorwaarden worden geëvalueerd.|Ja|  
|anders|Bevat het fragment beleid moet worden gebruikt als geen van de `when` voorwaarden worden geëvalueerd tot `true`.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|  
|---------------|-----------------|--------------|  
|conditie = "Boole-expressie &#124; Constante Booleaanse'|De Boole-expressie of een constante geëvalueerd wanneer de die `when` beleidsverklaring wordt geëvalueerd.|Ja|  
  
###  <a name="ChooseUsage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  
  
##  <a name="ForwardRequest"></a>Doorsturen van aanvraag  
 De `forward-request` beleid stuurt de binnenkomende aanvraag naar de back-endservice die is opgegeven in de aanvraag [context](api-management-policy-expressions.md#ContextVariables). De back-end-service-URL is opgegeven in de API [instellingen](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) en kan worden gewijzigd met behulp van de [back-endservice ingesteld](api-management-transformation-policies.md) beleid.  
  
> [!NOTE]
>  Verwijderen van deze resultaten van Groepsbeleid in de aanvraag niet wordt doorgestuurd naar de back-end worden-service en het beleid in de sectie uitgaande geëvalueerd onmiddellijk na de geslaagde voltooiing van het beleid in de sectie inkomende.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a>Voorbeelden  
  
#### <a name="example"></a>Voorbeeld  
 Het volgende beleid van de API-niveau verzendt alle aanvragen naar de back endservice met een time-outinterval van 60 seconden.  
  
```xml  
<!-- api level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="60"/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Voorbeeld  
 Het beleid voor deze bewerking gebruikt de `base` element worden overgenomen van het back-end-beleid van het bovenliggende niveau API-bereik.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <base/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Voorbeeld  
 Deze bewerking beleid expliciet stuurt alle aanvragen naar de back endservice met een time-out van 120 en het is niet overgenomen van het bovenliggende niveau back-end-API-beleid.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note the absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Voorbeeld  
 Dit beleid op het niveau bewerking doorstuurt geen aanvragen voor de back-endservice.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding to backend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|voorwaarts-aanvraag|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|time-out = 'integer'|De time-outinterval in seconden voordat de aanroep naar de back endservice is mislukt.|Nee|300 seconden|  
|Volg omleidingen = "true &#124; False"|Hiermee geeft u op of omleidingen vanaf de back-endservice worden gevolgd door de gateway of geretourneerd naar de aanroeper.|Nee|onwaar|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** back-end  
-   **Beleid scopes:** alle scopes  
  
##  <a name="LimitConcurrency"></a>Limiet gelijktijdigheid van taken  
 De `limit-concurrency` ingesloten beleid uit door meer dan het opgegeven aantal aanvragen wordt uitgevoerd op een bepaald moment wordt verhinderd door beleid. Bij meer dan dit aantal, mislukt nieuwe aanvragen onmiddellijk met statuscode voor 429 te veel aanvragen.
  
###  <a name="LimitConcurrencyStatement"></a>Beleidsverklaring  
  
```xml  
<limit-concurrency key="expression" max-count="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a>Voorbeelden  
  
#### <a name="example"></a>Voorbeeld  
 Het volgende voorbeeld laat zien hoe u wilt beperken het aantal aanvragen worden doorgestuurd naar een back-end op basis van de waarde van een variabele context.
 
```xml  
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|    
|limiet gelijktijdigheid|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|--------------|  
|sleutel|Een tekenreeks. Een expressie toegestaan. Hiermee geeft u het bereik gelijktijdigheid van taken. Kan worden gedeeld door meerdere beleidsregels.|Ja|N/A|  
|maximum aantal|Een geheel getal. Hiermee geeft u het maximale aantal aanvragen die zijn toegestaan in te voeren van het beleid.|Ja|N/A|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  

##  <a name="log-to-eventhub"></a>Logboek naar Event Hub  
 De `log-to-eventhub` beleid verzendt berichten in de opgegeven indeling naar een Event Hub gedefinieerd door een entiteit berichtenlogboek. Als de naam al aangeeft, wordt het beleid wordt gebruikt voor het opslaan van de geselecteerde aanvraag of antwoord contextinformatie voor online of offline-analyse.  
  
> [!NOTE]
>  Zie voor een stapsgewijze handleiding voor het configureren van een event hub en vastleggen van gebeurtenissen, [het registreren van API Management-gebeurtenissen met Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string to be logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a>Voorbeeld  
 Elke tekenreeks kan worden gebruikt als de waarde worden geregistreerd in Event Hubs. In dit voorbeeld is de datum en tijd, implementatie-servicenaam, aanvraag-id, IP-adres en de naam van de bewerking voor alle binnenkomende oproepen worden geregistreerd in de event hub berichtenlogboek geregistreerd bij de `contoso-logger` id.  
  
```xml  
<policies>  
  <inbound>  
    <log-to-eventhub logger-id ='contoso-logger'>  
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )   
    </log-to-eventhub>  
  </inbound>  
  <outbound>          
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|logboek voor eventhub|Hoofdelement. De waarde van dit element is de tekenreeks aan te melden naar uw event hub.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|  
|---------------|-----------------|--------------|  
|logboek-id|De id van het logboek geregistreerd bij uw API Management-service.|Ja|  
|partitie-id|Hiermee geeft u de index van de partitie waarin berichten worden verzonden.|Optioneel. Dit kenmerk kan niet worden gebruikt als `partition-key` wordt gebruikt.|  
|Partitiesleutel|Hiermee geeft u de waarde voor de partitietoewijzing van de wordt gebruikt wanneer berichten worden verzonden.|Optioneel. Dit kenmerk kan niet worden gebruikt als `partition-id` wordt gebruikt.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  

##  <a name="mock-response"></a>Mock-antwoord  
De `mock-response`, als de naam al aangeeft, wordt gebruikt voor het model van de API's en bewerkingen. Het normale pipeline-uitvoering wordt afgebroken en retourneert een mocked antwoord aan de aanroeper. Het beleid probeert altijd te retourneren van reacties van hoogste betrouwbaarheid. Deze verkiest antwoord inhoud voorbeelden, indien beschikbaar. Het voorbeeld antwoorden van schema's, genereert wanneer schema's worden geleverd en voorbeelden niet. Als geen van beide voorbeelden of schema's zijn gevonden, worden de antwoorden met geen inhoud geretourneerd.
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a>Voorbeelden  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|nagebootste-antwoord|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|--------------|  
|statuscode in|Hiermee geeft u de statuscode van antwoord en wordt gebruikt om bijbehorende voorbeeld of schema te selecteren.|Nee|200|  
|type inhoud|Hiermee geeft u `Content-Type` headerwaarde antwoord en wordt gebruikt om bijbehorende voorbeeld of schema te selecteren.|Nee|None|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** binnenkomende, uitgaande, bij fouten  
  
-   **Beleid scopes:** alle scopes

##  <a name="Retry"></a>Probeer het opnieuw  
 De `retry` beleid de onderliggende beleidsregels eenmaal wordt uitgevoerd en vervolgens probeert om opnieuw de uitvoering ervan totdat de nieuwe poging `condition` wordt `false` of probeer het opnieuw `count` leeg.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
  
<retry  
    condition="boolean expression or literal"  
    count="number of retry attempts"  
    interval="retry interval in seconds"  
    max-interval="maximum retry interval in seconds"  
    delta="retry interval delta in seconds"  
    first-fast-retry="boolean expression or literal">  
        <!-- One or more child policies. No restrictions -->  
</retry>  
  
```  
  
### <a name="example"></a>Voorbeeld  
 In het volgende voorbeeld wordt opnieuw geprobeerd doorsturen van aanvraag maximaal tien keer met een algoritme exponentiële probeer het opnieuw. Aangezien `first-fast-retry` is ingesteld op false, alle pogingen zijn onderworpen aan de algoritme exponentiële probeer het opnieuw.  
  
```xml  
  
<retry  
    condition="@(context.Response.StatusCode == 500)"  
    count="10"  
    interval="10"  
    max-interval="100"  
    delta="10"  
    first-fast-retry="false">  
        <forward-request />  
</retry>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|retry|Hoofdelement. Kan geen ander beleid bevatten als de onderliggende elementen.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|voorwaarde|Een boolean letterlijke waarde of [expressie](api-management-policy-expressions.md) opgeven als nieuwe pogingen moeten worden gestopt (`false`) of vervolg (`true`).|Ja|N/A|  
|aantal|Een positief getal dat aangeeft het maximale aantal nieuwe pogingen om te proberen.|Ja|N/A|  
|interval|Een positief getal in seconden, geven de wachtinterval tussen het opnieuw probeert.|Ja|N/A|  
|Max-interval|Een positief getal in seconden voor het opgeven van de maximale wachttijd interval tussen de pogingen. Wordt gebruikt voor het implementeren van een algoritme exponentiële probeer het opnieuw.|Nee|N/A|  
|delta|Een positief getal in seconden, de wachttijd interval verhoging opgeven. Wordt gebruikt voor het implementeren van de algoritmen lineaire en exponentieel probeer het opnieuw.|Nee|N/A|  
|eerste-fast-probeer het opnieuw|Indien ingesteld op `true` , de eerste nieuwe poging wordt onmiddellijk uitgevoerd.|Nee|`false`|  
  
> [!NOTE]
>  Wanneer alleen de `interval` is opgegeven, **vaste** interval voor nieuwe pogingen worden uitgevoerd.  
>  Wanneer alleen de `interval` en `delta` zijn opgegeven, een **lineaire** interval opnieuw proberen algoritme wordt gebruikt, waarbij de tijd tussen nieuwe pogingen wordt berekend op basis van de volgende formule - `interval + (count - 1)*delta`.  
>  Wanneer de `interval`, `max-interval` en `delta` zijn opgegeven, **exponentiële** interval opnieuw proberen algoritme wordt toegepast, waarbij de wachttijd tussen de pogingen groeit exponentieel van de waarde van `interval` naar de waarde `max-interval` volgens de volgende forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Houd er rekening mee dat informatie over het gebruik van onderliggende beleidsbeperkingen wordt overgenomen door dit beleid.  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  
  
##  <a name="ReturnResponse"></a>Antwoord retourneren  
 De `return-response` beleid annuleert pipeline-uitvoering en retourneert een standaardwaarde of een aangepast antwoord naar de aanroeper. Standaardantwoord is `200 OK` met geen hoofdcode. Aangepast antwoord kan worden opgegeven via de instructies voor een context variabele of beleid. Wanneer beide zijn opgegeven, wordt de reactie die is opgenomen in de context-variabele voordat het naar de aanroeper wordt geretourneerd door de beleidsverklaringen gewijzigd.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|Return-antwoord|Hoofdelement.|Ja|  
|set-header|Een [set-header](api-management-transformation-policies.md#SetHTTPheader) beleidsverklaring.|Nee|  
|set-instantie|Een [set hoofdtekst](api-management-transformation-policies.md#SetBody) beleidsverklaring.|Nee|  
|status instellen|Een [status instellen](api-management-advanced-policies.md#SetStatus) beleidsverklaring.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|  
|---------------|-----------------|--------------|  
|naam van een antwoord variabele|De naam van de variabele context waarnaar wordt verwezen vanuit, bijvoorbeeld een upstream [aanvragen verzenden](api-management-advanced-policies.md#SendRequest) beleid en met een `Response` object|Optioneel.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  
  
##  <a name="SendOneWayRequest"></a>Eenzijdige aanvraag verzenden  
 De `send-one-way-request` beleid voor de opgegeven aanvraag verzendt naar de opgegeven URL zonder te wachten op reactie.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a>Voorbeeld  
 Dit beleid voorbeeld toont een voorbeeld van het gebruik van de `send-one-way-request` beleid een bericht te verzenden naar toegestane chatruimten als de HTTP-antwoordcode groter dan of gelijk zijn aan 500 is. Zie voor meer informatie over dit voorbeeld [met behulp van externe services van de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|een-manier-verzoek om te verzenden|Hoofdelement.|Ja|  
|url|De URL van de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|  
|Methode|De HTTP-methode voor de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|  
|koptekst|Aanvraag-header. Meerdere headeronderdelen voor meerdere aanvraagheaders gebruiken.|Nee|  
|hoofdtekst|De aanvraagtekst.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|modus = 'tekenreeks'|Hiermee wordt bepaald of dit een nieuwe aanvraag of een kopie van de huidige aanvraag is. In de uitgaande modus, modus = kopiëren de aanvraagtekst niet geïnitialiseerd.|Nee|Nieuw|  
|naam|Hiermee geeft u de naam van de header moet worden ingesteld.|Ja|N/A|  
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de header is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -onderdrukking - vervangt de waarde van de bestaande koptekst.<br />de waarde van de bestaande header vervangen - skip - niet.<br />-toevoeg - de waarde toegevoegd aan de bestaande headerwaarde.<br />-delete - verwijdert de header van de aanvraag.<br /><br /> Als de waarde `override` opnemen van meerdere vermeldingen met dezelfde naam resulteert in de koptekst wordt ingesteld in overeenstemming met alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  
  
##  <a name="SendRequest"></a>Aanvraag verzenden  
 De `send-request` beleid voor de opgegeven aanvraag verzendt naar de opgegeven URL wacht niet langer dan de ingestelde time-out-waarde.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a>Voorbeeld  
 In dit voorbeeld ziet een manier om te controleren of de verwijzing naar een token met een server voor autorisatie. Zie voor meer informatie over dit voorbeeld [met behulp van externe services van de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request to Token Server to validate token (see RFC 7662) -->  
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">  
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>  
    <set-method>POST</set-method>  
    <set-header name="Authorization" exists-action="override">  
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>  
    </set-header>  
    <set-header name="Content-Type" exists-action="override">  
      <value>application/x-www-form-urlencoded</value>  
    </set-header>  
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>  
  </send-request>  
  
  <choose>  
        <!-- Check active property in response -->  
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
            <!-- Return 401 Unauthorized with http-problem payload -->  
            <return-response response-variable-name="existing response variable">  
                <set-status code="401" reason="Unauthorized" />  
                <set-header name="WWW-Authenticate" exists-action="override">  
                    <value>Bearer error="invalid_token"</value>  
                </set-header>  
            </return-response>  
        </when>  
    </choose>  
  <base />  
</inbound>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|aanvragen verzenden|Hoofdelement.|Ja|  
|url|De URL van de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|  
|Methode|De HTTP-methode voor de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|  
|koptekst|Aanvraag-header. Meerdere headeronderdelen voor meerdere aanvraagheaders gebruiken.|Nee|  
|hoofdtekst|De aanvraagtekst.|Nee|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|modus = 'tekenreeks'|Hiermee wordt bepaald of dit een nieuwe aanvraag of een kopie van de huidige aanvraag is. In de uitgaande modus, modus = kopiëren de aanvraagtekst niet geïnitialiseerd.|Nee|Nieuw|  
|antwoord-variabele-name = 'tekenreeks'|Als deze niet aanwezig is, `context.Response` wordt gebruikt.|Nee|N/A|  
|time-out = 'integer'|De time-outinterval in seconden voordat de aanroep naar de URL is mislukt.|Nee|60|  
|fout negeren|Indien true en de aanvraag resulteert in een fout opgetreden:<br /><br /> -Als de naam van een antwoord variabele bevat een null-waarde is opgegeven.<br />-Als de naam van een antwoord variabele is niet opgegeven, context. Aanvraag wordt niet bijgewerkt.|Nee|onwaar|  
|naam|Hiermee geeft u de naam van de header moet worden ingesteld.|Ja|N/A|  
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de header is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -onderdrukking - vervangt de waarde van de bestaande koptekst.<br />de waarde van de bestaande header vervangen - skip - niet.<br />-toevoeg - de waarde toegevoegd aan de bestaande headerwaarde.<br />-delete - verwijdert de header van de aanvraag.<br /><br /> Als de waarde `override` opnemen van meerdere vermeldingen met dezelfde naam resulteert in de koptekst wordt ingesteld in overeenstemming met alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  
  
##  <a name="SetHttpProxy"></a>Set HTTP-proxy  
 De `proxy` beleid kunt u op route-aanvragen worden doorgestuurd naar de back-ends via een HTTP-proxy. Alleen HTTP (niet HTTPS) wordt ondersteund tussen de gateway en de proxy. Basic- en NTLM-verificatie.
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a>Voorbeeld  
Let op het gebruik van [eigenschappen](api-management-howto-properties.md) als waarden van de gebruikersnaam en wachtwoord om te voorkomen dat gevoelige informatie op te slaan in het beleidsdocument.  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|Proxy|Hoofdelement|Ja|  

### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|URL = 'tekenreeks'|Proxy-URL in de vorm van http://host:port.|Ja|N/A|  
|gebruikersnaam = 'tekenreeks'|De gebruikersnaam moet worden gebruikt voor verificatie met de proxy.|Nee|N/A|  
|wachtwoord = 'tekenreeks'|Wachtwoord moet worden gebruikt voor verificatie met de proxy.|Nee|N/A|  

### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
  
-   **Beleid scopes:** alle scopes  

##  <a name="SetRequestMethod"></a>Set-aanvraagmethode  
 De `set-method` beleid kunt u de HTTP-aanvraagmethode voor een aanvraag wijzigen.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a>Voorbeeld  
 Dit voorbeeld-beleid die gebruikmaakt van de `set-method` beleid toont een voorbeeld van een bericht verzenden naar toegestane chatruimten als de HTTP-antwoordcode groter dan of gelijk zijn aan 500 is. Zie voor meer informatie over dit voorbeeld [met behulp van externe services van de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|set-methode|Hoofdelement. De waarde van het element bevat de HTTP-methode.|Ja|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, bij fouten  
  
-   **Beleid scopes:** alle scopes  
  
##  <a name="SetStatus"></a>Set-statuscode  
 De `set-status` beleid wordt de HTTP-statuscode ingesteld op de opgegeven waarde.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a>Voorbeeld  
 In dit voorbeeld laat zien hoe een 401-respons retourneren als het verificatietoken ongeldig is. Zie voor meer informatie [met behulp van externe services van de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)  
  
```xml  
<choose>  
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
    <return-response response-variable-name="existing response variable">  
      <set-status code="401" reason="Unauthorized" />  
      <set-header name="WWW-Authenticate" exists-action="override">  
        <value>Bearer error="invalid_token"</value>  
      </set-header>  
    </return-response>  
  </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|status instellen|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|code = 'integer'|De HTTP-statuscode te retourneren.|Ja|N/A|  
|reden = 'tekenreeks'|Een beschrijving van de reden voor het retourneren van de statuscode.|Ja|N/A|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** uitgaand, back-end op fout  
-   **Beleid scopes:** alle scopes  

##  <a name="set-variable"></a>Variabele instellen  
 De `set-variable` beleid declareert een [context](api-management-policy-expressions.md#ContextVariables) variabele en hieraan een waarde die is opgegeven een [expressie](api-management-policy-expressions.md) of een letterlijke tekenreeks. Als de expressie een letterlijke reeks bevat wordt geconverteerd naar een tekenreeks en het type van de waarde niet `System.String`.  
  
###  <a name="set-variablePolicyStatement"></a>Beleidsverklaring  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <a name="set-variableExample"></a>Voorbeeld  
 Het volgende voorbeeld wordt een variabele beleid instellen in de sectie binnenkomend. Deze variabele beleid instellen maakt een `isMobile` Booleaanse [context](api-management-policy-expressions.md#ContextVariables) variabele die is ingesteld op true als de `User-Agent` aanvraag-header bevat de tekst `iPad` of `iPhone`.  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|variabele instellen|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|  
|---------------|-----------------|--------------|  
|naam|De naam van de variabele.|Ja|  
|waarde|De waarde van de variabele. Dit kan een expressie of een letterlijke waarde zijn.|Ja|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
-   **Beleid scopes:** alle scopes  
  
###  <a name="set-variableAllowedTypes"></a>Toegestane typen  
 Expressies in de `set-variable` beleid moet een van de volgende eenvoudige typen retourneren.  
  
-   System.Boolean  
-   System.SByte  
-   System.Byte  
-   System.UInt16  
-   System.UInt32  
-   System.UInt64  
-   System.Int16  
-   System.Int32  
-   System.Int64  
-   System.Decimal  
-   System.Single  
-   System.Double  
-   System.Guid  
-   System.String  
-   System.Char  
-   System.DateTime  
-   System.DateTime  
-   System.Byte?  
-   System.UInt16?  
-   System.UInt32?  
-   System.UInt64?  
-   System.Int16?  
-   System.Int32?  
-   System.Int64?  
-   System.Decimal?  
-   System.Single?  
-   System.Double?  
-   System.Guid?  
-   System.String?  
-   System.Char?  
-   System.DateTime?  

##  <a name="Trace"></a>Tracering  
 De `trace` beleid wordt toegevoegd een tekenreeks in de [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) uitvoer. Het beleid wordt uitgevoerd, alleen wanneer tracering wordt geactiveerd, dat wil zeggen `Ocp-Apim-Trace` aanvraagheader aanwezig is en is ingesteld op `true` en `Ocp-Apim-Subscription-Key` aanvraagheader aanwezig is en een geldige sleutel die is gekoppeld aan het beheerdersaccount bevat.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|tracering|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|bron|Letterlijke tekenreeks relevant zijn voor de traceringsviewer en de bron van het bericht op te geven.|Ja|N/A|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Beleid secties:** inkomend, uitgaand back-end op fout  
  
-   **Beleid scopes:** alle scopes  
  
##  <a name="Wait"></a>Wacht  
 De `wait` beleid bijbehorende directe onderliggende beleidsregels parallel worden uitgevoerd en wordt gewacht op alle of een van de bijbehorende directe onderliggende beleidsregels te voltooien voordat deze is voltooid. Het beleid voor de wachttijd kan hebben als het beleid direct onderliggende [Verzendaanvraag](api-management-advanced-policies.md#SendRequest), [waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey), en [transportbesturing](api-management-advanced-policies.md#choose) beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a>Voorbeeld  
 In het volgende voorbeeld zijn er twee `choose` beleid als beleid direct onderliggende van de `wait` beleid. Elk van deze `choose` beleid parallel worden uitgevoerd. Elke `choose` beleid probeert op te halen van een waarde in de cache. Als er een cache ontbreekt, wordt een back-endservice aangeroepen om de waarde. In dit voorbeeld de `wait` beleid niet wordt voltooid totdat alle van de bijbehorende directe onderliggende beleidsregels hebt voltooid, omdat de `for` kenmerk is ingesteld op `all`.   In dit voorbeeld de variabelen context (`execute-branch-one`, `value-one`, `execute-branch-two`, en `value-two`) zijn gedeclareerd buiten het bereik van dit voorbeeldbeleid.  
  
```xml  
<wait for="all">  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-one="])">  
      <cache-lookup-value key="key-one" variable-name="value-one" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-one="))">  
          <send-request mode="new" response-variable-name="value-one">  
            <set-url>https://backend-one</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-two="])">  
      <cache-lookup-value key="key-two" variable-name="value-two" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-two="))">  
          <send-request mode="new" response-variable-name="value-two">  
            <set-url>https://backend-two</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
</wait>  
  
```  
  
### <a name="elements"></a>Elementen  
  
|Element|Beschrijving|Vereist|  
|-------------|-----------------|--------------|  
|Wacht|Hoofdelement. Alleen onderliggende elementen bevat mogelijk `send-request`, `cache-lookup-value`, en `choose` beleid.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Kenmerk|Beschrijving|Vereist|Standaard|  
|---------------|-----------------|--------------|-------------|  
|voor|Hiermee wordt bepaald of de `wait` beleid wacht tot alle directe onderliggende beleidsregels is voltooid of alleen bestaan. Toegestane waarden zijn:<br /><br /> -   `all`-Wacht totdat alle directe onderliggende beleidsregels om te voltooien<br />-een - wachten op een directe onderliggende beleid om te voltooien. Zodra het eerste directe onderliggende beleid is voltooid, de `wait` beleid is voltooid en de uitvoering van een ander beleid direct onderliggende is beëindigd.|Nee|all|  
  
### <a name="usage"></a>Gebruik  
 
Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend, uitgaand back-end  
-   **Beleid scopes:** alle scopes  
  
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, werken met beleid:
+ [Beleidsregels in API Management](api-management-howto-policies.md) 
+ [Beleidsexpressies](api-management-policy-expressions.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   
