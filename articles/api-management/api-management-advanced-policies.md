---
title: Azure API Management Geavanceerde beleidsregels | Microsoft Docs
description: Meer informatie over de geavanceerde beleidsregels beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 478b1352d0bf2eec9af62e519b50e61dea6cc8fc
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158428"
---
# <a name="api-management-advanced-policies"></a>API Management Geavanceerde beleidsregels
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleid. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).

##  <a name="AdvancedPolicies"></a> Geavanceerde beleidsregels

-   [Controlestroom](api-management-advanced-policies.md#choose) - voorwaardelijk is van toepassing op basis van de resultaten van de evaluatie van Booleaanse waarde beleidsverklaringen [expressies](api-management-policy-expressions.md).
-   [Doorsturen van aanvraag](#ForwardRequest) -stuurt de aanvraag naar de back-endservice.
-   [Beperken van gelijktijdigheid](#LimitConcurrency) -voorkomt dat tussen beleid van door meer dan het opgegeven aantal aanvragen dat wordt uitgevoerd op een tijdstip.
-   [Bij Log to Eventhub](#log-to-eventhub) -berichten in de opgegeven indeling verzendt naar een Event Hub gedefinieerd door een Logger-entiteit.
-   [Gesimuleerde een antwoord](#mock-response) -pipeline-uitvoering wordt afgebroken en retourneert een gesimuleerd antwoord rechtstreeks naar de oproepende functie.
-   [Probeer](#Retry) -uitvoering van de ingesloten beleidsverklaringen pogingen als en totdat de voorwaarde wordt voldaan. Uitvoering wordt herhaald op de opgegeven tijdsintervallen en tot het opgegeven aantal nieuwe pogingen.
-   [Antwoord retourneren](#ReturnResponse) -pipeline-uitvoering wordt afgebroken en retourneert het opgegeven antwoord rechtstreeks naar de oproepende functie.
-   [Verzenden van One-Way-request](#SendOneWayRequest) -zendt een verzoek naar de opgegeven URL zonder te wachten op een reactie.
-   [Aanvraag verzenden](#SendRequest) -zendt een verzoek naar de opgegeven URL.
-   [HTTP-proxy instellen](#SetHttpProxy) -kunt u de route doorgestuurd aanvragen via een HTTP-proxy.
-   [Aanvraagmethode ingesteld](#SetRequestMethod) -kunt u de HTTP-methode voor een aanvraag wijzigen.
-   [Instellen van de statuscode](#SetStatus) -verandert de HTTP-statuscode in de opgegeven waarde.
-   [Variabele instellen](api-management-advanced-policies.md#set-variable) -zich blijft voordoen een waarde in een benoemd exemplaar [context](api-management-policy-expressions.md#ContextVariables) variabele om later toegang te krijgen.
-   [Tracering](#Trace) -voegt een tekenreeks in de [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) uitvoer.
-   [Wacht](#Wait) -wacht voor ingesloten [Verzendaanvraag](api-management-advanced-policies.md#SendRequest), [waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey), of [controlestroom](api-management-advanced-policies.md#choose) beleid om te voltooien voordat u doorgaat.

##  <a name="choose"></a> Controlestroom
 De `choose` beleid van toepassing is ingesloten beleid overzichten op basis van het resultaat van evaluatie van Booleaanse expressies, vergelijkbaar met een if-dan-anders of een switch in een programmeertaal maken.

###  <a name="ChoosePolicyStatement"></a> Beleidsverklaring

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

 Beleid voor de stroom moet ten minste één bevatten `<when/>` element. De `<otherwise/>` element is optioneel. Voorwaarden `<when/>` elementen worden geëvalueerd in de volgorde van de weergave in het beleid. Beleid voor instructie (s) tussen de eerste `<when/>` element met kenmerk gelijk is aan voorwaarde `true` wordt toegepast. Beleid tussen de `<otherwise/>` -element, indien aanwezig, worden toegepast als u alle van de `<when/>` element voorwaardekenmerken zijn `false`.

### <a name="examples"></a>Voorbeelden

####  <a name="ChooseExample"></a> Voorbeeld
 Het volgende voorbeeld ziet u een [variabele instellen](api-management-advanced-policies.md#set-variable) beleid en de twee beleidsregels voor beheer stroom.

 De variabele beleid instellen in de sectie inbound en genereert een `isMobile` Booleaanse [context](api-management-policy-expressions.md#ContextVariables) variabele die is ingesteld op true als de `User-Agent` aanvraag header bevat de tekst `iPad` of `iPhone`.

 Het eerste besturingselement stroom beleid is ook in de sectie inbound en voorwaardelijk van toepassing is een van de twee [queryreeks-parameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) beleidsregels, afhankelijk van de waarde van de `isMobile` contextvariabele.

 Het tweede besturingselement stroom-beleid in de sectie outbound en voorwaardelijk past de [XML converteren naar JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) beleid wanneer `isMobile` is ingesteld op `true`.

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
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>
            </when>
        </choose>
    </outbound>
</policies>
```

#### <a name="example"></a>Voorbeeld
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

### <a name="elements"></a>Elementen

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|Kies|Root-element.|Ja|
|wanneer|De voorwaarde moet worden gebruikt voor de `if` of `ifelse` onderdelen van de `choose` beleid. Als de `choose` beleid heeft meerdere `when` secties worden deze geëvalueerd aan sequentieel worden verwerkt. Zodra de `condition` van een wanneer element wordt geëvalueerd als `true`, er verder geen `when` voorwaarden worden geëvalueerd.|Ja|
|anders|Bevat het fragment beleid moet worden gebruikt als geen van de `when` voorwaarden worden geëvalueerd tot `true`.|Nee|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|
|---------------|-----------------|--------------|
|voorwaarde = "Booleaanse expressie &#124; Booleaanse constante"|De Booleaanse expressie of een constante geëvalueerd wanneer de die `when` beleidsverklaring wordt geëvalueerd.|Ja|

###  <a name="ChooseUsage"></a> Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="ForwardRequest"></a> Doorsturen van aanvraag
 De `forward-request` beleid stuurt de inkomende aanvraag naar de back endservice die is opgegeven in de aanvraag [context](api-management-policy-expressions.md#ContextVariables). De URL van de back-end-service is opgegeven in de API [instellingen](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) en kan worden gewijzigd met behulp van de [instellen van back-endservice](api-management-transformation-policies.md) beleid.

> [!NOTE]
>  Bezig met verwijderen van deze resultaten van Groepsbeleid in de aanvraag niet wordt doorgestuurd naar de back-end worden-service en het beleid in de sectie outbound geëvalueerd onmiddellijk na de voltooiing van de beleidsregels in de sectie inbound.

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
 Maakt gebruik van dit beleid voor de bewerking op de `base` element overnemen van het back endbeleid van het bovenliggende bereik voor API.

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
 Dit beleid op het niveau van bewerking stuurt alle aanvragen naar de back endservice met een time-out van 120 expliciet en neemt geen informatie over het bovenliggende niveau back-end-API-beleid.

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
 Dit beleid op het niveau van bewerking wordt niet doorsturen van aanvragen naar de back-endservice.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|zone voor forward-aanvraag|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|timeout="integer"|De time-outinterval in seconden voordat de aanroep naar de back endservice is mislukt.|Nee|300 seconden|
|Volg-omleidingen = "true &#124; false"|Geeft aan of omleidingen van de back-endservice worden gevolgd door de gateway of geretourneerd naar de aanroeper.|Nee|onwaar|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** back-end
-   **Beleid bereiken:** alle bereiken

##  <a name="LimitConcurrency"></a> Limiet voor gelijktijdigheid
 De `limit-concurrency` beleid voorkomt dat ingesloten beleid door meer dan het opgegeven aantal aanvragen op elk gewenst moment uitvoeren. Bij meer dan dit aantal, mislukt nieuwe aanvragen onmiddellijk met de statuscode van 429 te veel aanvragen.

###  <a name="LimitConcurrencyStatement"></a> Beleidsverklaring

```xml
<limit-concurrency key="expression" max-count="number">
        <!— nested policy statements -->
</limit-concurrency>
```

### <a name="examples"></a>Voorbeelden

#### <a name="example"></a>Voorbeeld
 Het volgende voorbeeld laat zien hoe u het aantal aanvragen die worden doorgestuurd naar een back-end op basis van de waarde van een contextvariabele te beperken.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|limiet voor gelijktijdigheid|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|--------------|
|sleutel|Een tekenreeks. De expressie toegestaan. Hiermee geeft u het bereik van gelijktijdigheid. Kan worden gedeeld door meerdere beleidsregels.|Ja|n.v.t.|
|maximum aantal|Een geheel getal zijn. Hiermee geeft u een maximum aantal aanvragen die zijn toegestaan in te voeren van het beleid.|Ja|n.v.t.|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="log-to-eventhub"></a> Log to Event Hub
 De `log-to-eventhub` beleid-berichten in de opgegeven indeling verzendt naar een Event Hub gedefinieerd door een Logger-entiteit. Als de naam al aangeeft, wordt het beleid wordt gebruikt voor het opslaan van de geselecteerde aanvraag of antwoord contextinformatie voor online of offline-analyse.

> [!NOTE]
>  Zie voor een stapsgewijze handleiding voor het configureren van een event hub en vastleggen van gebeurtenissen, [hoe u zich aanmeldt API Management-gebeurtenissen met Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">
  Expression returning a string to be logged
</log-to-eventhub>

```

### <a name="example"></a>Voorbeeld
 Een willekeurige tekenreeks kan worden gebruikt als de waarde moet zijn aangemeld met Event Hubs. In dit voorbeeld is de datum en tijd, implementatie-servicenaam, aanvraag-id, ip-adres, en de naam van de bewerking voor alle inkomende aanroepen worden geregistreerd in de event hub Logger geregistreerd bij de `contoso-logger` id.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|logboek naar eventhub|Root-element. De waarde van dit element is de tekenreeks aan te melden met uw event hub.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|
|---------------|-----------------|--------------|
|logger-id|De id van het logboek geregistreerd met uw API Management-service.|Ja|
|partitie-id|Hiermee geeft u de index van de partitie waarnaar moeten worden verzonden.|Optioneel. Dit kenmerk kan niet worden gebruikt als `partition-key` wordt gebruikt.|
|Partitiesleutel|Hiermee geeft u de waarde voor de partitietoewijzing wanneer berichten worden verzonden.|Optioneel. Dit kenmerk kan niet worden gebruikt als `partition-id` wordt gebruikt.|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="mock-response"></a> Mock-antwoord
De `mock-response`, als de naam aangeeft, wordt gebruikt voor het model van API's en bewerkingen. Het normale pijplijnuitvoering wordt afgebroken en retourneert een gesimuleerd antwoord voor de oproepende functie. Het beleid altijd wordt geprobeerd om te retourneren van reacties van de hoogste kwaliteit. Verkiest het antwoord inhoud voorbeelden, wanneer beschikbaar. Deze voorbeeld-antwoorden van schema's, genereert wanneer schema's worden verstrekt en voorbeelden niet zijn. Als geen van beide voorbeelden of schema's worden gevonden, worden de antwoorden met geen inhoud geretourneerd.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|nagebootste-antwoord|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|--------------|
|status-code|Hiermee geeft u antwoordstatuscode en wordt gebruikt voor het selecteren van de bijbehorende voorbeeld of schema.|Nee|200|
|inhoudstype|Hiermee geeft u `Content-Type` headerwaarde antwoord en wordt gebruikt voor het selecteren van de bijbehorende voorbeeld of schema.|Nee|Geen|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** binnenkomende, uitgaande, bij fout

-   **Beleid bereiken:** alle bereiken

##  <a name="Retry"></a> Probeer het opnieuw
 De `retry` beleid voor de onderliggende beleidsregels één keer wordt uitgevoerd en vervolgens probeert om opnieuw de uitvoering ervan tot en met de nieuwe poging `condition` wordt `false` of probeer het opnieuw `count` is leeg.

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
 In het volgende voorbeeld wordt opnieuw uitgevoerd aanvraag doorsturen maximaal tien keer met behulp van een algoritme exponentiële nieuwe pogingen. Omdat `first-fast-retry` is ingesteld op false, alle nieuwe pogingen zijn afhankelijk van het algoritme exponentiële nieuwe pogingen.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|opnieuw proberen|Root-element. Kan geen ander beleid bevatten als de onderliggende elementen.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|voorwaarde|Een letterlijke Booleaanse waarde of [expressie](api-management-policy-expressions.md) op te geven als nieuwe pogingen moeten worden gestopt (`false`) of vervolg (`true`).|Ja|n.v.t.|
|aantal|Een positief getal opgeven van het maximum aantal nieuwe pogingen om te proberen.|Ja|n.v.t.|
|interval|Een positief getal in seconden op te geven het wachtinterval tussen het opnieuw probeert.|Ja|n.v.t.|
|max-interval|Een positief getal in seconden op te geven de maximale Wacht-interval tussen de pogingen. Het wordt gebruikt voor het implementeren van een algoritme exponentiële nieuwe pogingen.|Nee|n.v.t.|
|delta|Een positief getal in seconden op te geven de verhoging van de interval voor wait. Het wordt gebruikt voor het implementeren van de algoritmen lineaire en exponentiële nieuwe pogingen.|Nee|n.v.t.|
|eerste-fast-opnieuw proberen|Indien ingesteld op `true` , de eerste nieuwe poging onmiddellijk wordt uitgevoerd.|Nee|`false`|

> [!NOTE]
>  Wanneer alleen de `interval` is opgegeven, **vaste** interval voor nieuwe pogingen worden uitgevoerd.
> Wanneer alleen de `interval` en `delta` zijn opgegeven, een **lineaire** interval opnieuw proberen algoritme wordt gebruikt, waarbij de wachttijd tussen nieuwe pogingen wordt berekend op basis van de volgende formule - `interval + (count - 1)*delta`.
> Wanneer de `interval`, `max-interval` en `delta` zijn opgegeven, **exponentieel** interval opnieuw proberen algoritme wordt toegepast, waar de wachttijd tussen de pogingen exponentieel toeneemt van de waarde van `interval` naar de waarde `max-interval` op basis van de volgende formule - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Houd er rekening mee dat onderliggende beperkingen voor het gebruik van beleid, worden overgenomen door dit beleid.

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="ReturnResponse"></a> Antwoord retourneren
 De `return-response` beleid pijplijnuitvoering wordt afgebroken en retourneert een standaard of aangepast antwoord voor de oproepende functie. Standaardantwoord is `200 OK` met geen hoofdtekst. Aangepast antwoord kan worden opgegeven via een context variabele of beleid voor instructies. Wanneer beide zijn opgegeven, wordt het antwoord dat is opgenomen in de contextvariabele gewijzigd door de beleidsinstructies voordat het wordt geretourneerd voor de oproepende functie.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|Retour-antwoord|Root-element.|Ja|
|set-header|Een [set-header](api-management-transformation-policies.md#SetHTTPheader) beleidsverklaring.|Nee|
|hoofdtekst instellen|Een [hoofdtekst instellen](api-management-transformation-policies.md#SetBody) beleidsverklaring.|Nee|
|set-status|Een [set-status](api-management-advanced-policies.md#SetStatus) beleidsverklaring.|Nee|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|
|---------------|-----------------|--------------|
|antwoord-variabele-naam|De naam van de contextvariabele waarnaar wordt verwezen, bijvoorbeeld een upstream [Verzendaanvraag](api-management-advanced-policies.md#SendRequest) beleid en die een `Response` object|Optioneel.|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="SendOneWayRequest"></a> Een manier aanvraag verzenden
 De `send-one-way-request` beleid voor de opgegeven aanvraag verzendt naar de opgegeven URL zonder te wachten op een reactie.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<send-one-way-request mode="new | copy">
  <url>...</url>
  <method>...</method>
  <header name="" exists-action="override | skip | append | delete">...</header>
  <body>...</body>
  <authentication-certificate thumbprint="thumbprint" />
</send-one-way-request>

```

### <a name="example"></a>Voorbeeld
 Dit beleid voorbeeld ziet u een voorbeeld van het gebruik van de `send-one-way-request` beleid voor het verzenden van een bericht naar een Slack chatruimte als de HTTP-responscode groter dan of gelijk aan 500 is. Zie voor meer informatie over dit voorbeeld [gebruik van externe services via de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|een-manier-verzoek om te verzenden|Root-element.|Ja|
|url|De URL van de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|
|methode|De HTTP-methode voor de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|
|koptekst|De aanvraagheader. Gebruik meerdere headeronderdelen voor meerdere aanvraagheaders.|Nee|
|hoofdtekst|De aanvraagtekst.|Nee|
|certificaat voor clientverificatie|[Certificaat voor clientverificatie](api-management-authentication-policies.md#ClientCertificate)|Nee|


### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|modus = 'tekenreeks'|Bepaalt of dit een nieuwe aanvraag of een kopie van de huidige aanvraag is. In de uitgaande modus, modus = kopie hoofdtekst van de aanvraag niet geïnitialiseerd.|Nee|Nieuw|
|naam|Hiermee geeft u de naam van de header moet worden ingesteld.|Ja|n.v.t.|
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de koptekst is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -override - vervangt de waarde van de bestaande koptekst.<br />-skip: is geen vervanging voor de bestaande waarde voor header.<br />-append - de waarde wordt toegevoegd aan de bestaande waarde voor header.<br />-delete - verwijdert de header van de aanvraag.<br /><br /> Als de waarde `override` meerdere vermeldingen met dezelfde naam opnemen resulteert in de koptekst wordt ingesteld op basis van alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="SendRequest"></a> Aanvraag verzenden
 De `send-request` beleid verzendt de opgegeven aanvraag naar de opgegeven URL niet langer dan de ingestelde waarde voor time-out voor wachten.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error
="false|true">
  <set-url>...</set-url>
  <set-method>...</set-method>
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>
  <set-body>...</set-body>
  <authentication-certificate thumbprint="thumbprint" />
</send-request>

```

### <a name="example"></a>Voorbeeld
 In dit voorbeeld toont één manier om te controleren of de verwijzing naar een token met een autorisatie-server. Zie voor meer informatie over dit voorbeeld [gebruik van externe services via de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).

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
            <return-response>
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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|verzonden aanvragen|Root-element.|Ja|
|url|De URL van de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|
|methode|De HTTP-methode voor de aanvraag.|Er is geen als modus = kopiëren; anders Ja.|
|koptekst|De aanvraagheader. Gebruik meerdere headeronderdelen voor meerdere aanvraagheaders.|Nee|
|hoofdtekst|De aanvraagtekst.|Nee|
|certificaat voor clientverificatie|[Certificaat voor clientverificatie](api-management-authentication-policies.md#ClientCertificate)|Nee|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|modus = 'tekenreeks'|Bepaalt of dit een nieuwe aanvraag of een kopie van de huidige aanvraag is. In de uitgaande modus, modus = kopie hoofdtekst van de aanvraag niet geïnitialiseerd.|Nee|Nieuw|
|antwoord-variabele-name = 'tekenreeks'|De naam van de contextvariabele die een antwoordobject voor ontvangen. Als de variabele niet bestaat, deze na een geslaagde uitvoering van het beleid wordt gemaakt en worden daardoor toegankelijk via [ `context.Variable` ](api-management-policy-expressions.md#ContextVariables) verzameling.|Ja|n.v.t.|
|timeout="integer"|De time-outinterval in seconden voordat de aanroep naar de URL is mislukt.|Nee|60|
|fout negeren|Als true en de resultaten van de aanvraag in een fout opgetreden:<br /><br /> -Als antwoord-variabele-naam bevat een null-waarde is opgegeven.<br />-Als de naam van een reactie variabele is niet opgegeven, context. Aanvraag wordt niet bijgewerkt.|Nee|onwaar|
|naam|Hiermee geeft u de naam van de header moet worden ingesteld.|Ja|n.v.t.|
|Er bestaat actie|Hiermee geeft u op welke actie moet worden uitgevoerd wanneer de koptekst is al opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -override - vervangt de waarde van de bestaande koptekst.<br />-skip: is geen vervanging voor de bestaande waarde voor header.<br />-append - de waarde wordt toegevoegd aan de bestaande waarde voor header.<br />-delete - verwijdert de header van de aanvraag.<br /><br /> Als de waarde `override` meerdere vermeldingen met dezelfde naam opnemen resulteert in de koptekst wordt ingesteld op basis van alle vermeldingen (die wordt vermeld meerdere keren); alleen de vermelde waarden worden ingesteld in het resultaat.|Nee|overschrijven|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="SetHttpProxy"></a> HTTP-proxy instellen
 De `proxy` beleid kunt u doorgestuurd naar de back-ends via een HTTP-proxy voor het routeren van aanvragen. Alleen HTTP (niet HTTPS) wordt tussen de gateway en de proxy ondersteund. Basic- en NTLM-verificatie.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|Proxy|Root-element|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|URL = "tekenreeks"|Proxy-URL in de vorm van http://host:port.|Ja|n.v.t.|
|gebruikersnaam = 'tekenreeks'|De gebruikersnaam op die moet worden gebruikt voor verificatie met de proxy.|Nee|n.v.t.|
|wachtwoord = 'tekenreeks'|Het wachtwoord moet worden gebruikt voor verificatie met de proxy.|Nee|n.v.t.|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend

-   **Beleid bereiken:** alle bereiken

##  <a name="SetRequestMethod"></a> Set-aanvraagmethode
 De `set-method` beleid kunt u de HTTP-aanvraagmethode voor een aanvraag wijzigen.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<set-method>METHOD</set-method>

```

### <a name="example"></a>Voorbeeld
 Dit voorbeeld-beleid die gebruikmaakt van de `set-method` beleid toont een voorbeeld van een bericht verzenden naar een Slack chatruimte als de HTTP-responscode groter dan of gelijk aan 500 is. Zie voor meer informatie over dit voorbeeld [gebruik van externe services via de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|set-methode|Root-element. De waarde van het element Hiermee geeft u de HTTP-methode.|Ja|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, bij fout

-   **Beleid bereiken:** alle bereiken

##  <a name="SetStatus"></a> Set-statuscode
 De `set-status` beleid wordt de HTTP-statuscode ingesteld op de opgegeven waarde.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<set-status code="" reason=""/>

```

### <a name="example"></a>Voorbeeld
 In dit voorbeeld laat zien hoe een 401-respons retourneren als het verificatietoken ongeldig is. Zie voor meer informatie, [externe services van de Azure API Management-service gebruiken](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|set-status|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|code="integer"|De HTTP-statuscode om terug te keren.|Ja|n.v.t.|
|reden = 'tekenreeks'|Een beschrijving van de reden voor het retourneren van de statuscode.|Ja|n.v.t.|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** uitgaand, back-end, bij fout
-   **Beleid bereiken:** alle bereiken

##  <a name="set-variable"></a> Variabele instellen
 De `set-variable` beleid declareert een [context](api-management-policy-expressions.md#ContextVariables) variabele en hieraan een waarde die is opgegeven een [expressie](api-management-policy-expressions.md) of een letterlijke tekenreeks zijn. Als de expressie een letterlijke reeks bevat wordt geconverteerd naar een tekenreeks en het type van de waarde `System.String`.

###  <a name="set-variablePolicyStatement"></a> Beleidsverklaring

```xml
<set-variable name="variable name" value="Expression | String literal" />
```

###  <a name="set-variableExample"></a> Voorbeeld
 Het volgende voorbeeld ziet u een variabele beleid instellen in de sectie inbound. Deze variabele instellen-beleid maakt een `isMobile` Booleaanse [context](api-management-policy-expressions.md#ContextVariables) variabele die is ingesteld op true als de `User-Agent` aanvraag header bevat de tekst `iPad` of `iPhone`.

```xml
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />
```

### <a name="elements"></a>Elementen

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|variabele instellen|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|
|---------------|-----------------|--------------|
|naam|De naam van de variabele.|Ja|
|waarde|De waarde van de variabele. Dit kan een expressie of een letterlijke waarde zijn.|Ja|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end op fout
-   **Beleid bereiken:** alle bereiken

###  <a name="set-variableAllowedTypes"></a> Toegestane typen
 Expressies die worden gebruikt de `set-variable` beleid moet een van de volgende fundamentele typen retourneren.

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
-   System.TimeSpan
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

##  <a name="Trace"></a> Tracering
 De `trace` beleid wordt toegevoegd een tekenreeks in de [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) uitvoer. Het beleid wordt uitgevoerd alleen wanneer tracering is geactiveerd, dat wil zeggen `Ocp-Apim-Trace` aanvraagheader aanwezig is en is ingesteld op `true` en `Ocp-Apim-Subscription-Key` aanvraagheader aanwezig is en bevat geen geldige sleutel die is gekoppeld aan het beheerdersaccount.

### <a name="policy-statement"></a>Beleidsverklaring

```xml

<trace source="arbitrary string literal">
    <!-- string expression or literal -->
</trace>

```

### <a name="elements"></a>Elementen

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|Tracering|Root-element.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|source|Letterlijke tekenreeks zinvol is voor de traceringsviewer voor en de bron van het bericht op te geven.|Ja|n.v.t.|

### <a name="usage"></a>Gebruik
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .

-   **Beleid secties:** inkomend, uitgaand back-end op fout

-   **Beleid bereiken:** alle bereiken

##  <a name="Wait"></a> Wacht
 De `wait` beleid wordt voorgeschreven beleid direct onderliggende parallel worden uitgevoerd, en wacht tot alle of een van de direct onderliggende beleidsregels om te voltooien voordat het klaar is. Het beleid wacht kan hebben als het beleid direct onderliggende [Verzendaanvraag](api-management-advanced-policies.md#SendRequest), [waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey), en [controlestroom](api-management-advanced-policies.md#choose) beleid.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<wait for="all|any">
  <!--Wait policy can contain send-request, cache-lookup-value,
        and choose policies as child elements -->
</wait>

```

### <a name="example"></a>Voorbeeld
 In het volgende voorbeeld zijn er twee `choose` beleid direct onderliggende beleid van de `wait` beleid. Elk van deze `choose` beleid wordt parallel uitgevoerd. Elke `choose` beleid probeert op te halen van een waarde in de cache. Als er een cache ontbreekt, wordt een back-endservice waarmee de waarde genoemd. In dit voorbeeld de `wait` beleid niet wordt voltooid totdat alle beleidsregels voor de direct onderliggende hebt voltooid, omdat de `for` kenmerk is ingesteld op `all`.   In dit voorbeeld de contextvariabelen (`execute-branch-one`, `value-one`, `execute-branch-two`, en `value-two`) buiten het bereik van dit voorbeeldbeleid zijn gedefinieerd.

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

|Element|Description|Vereist|
|-------------|-----------------|--------------|
|Wacht|Root-element. Als alleen onderliggende elementen kan bevatten `send-request`, `cache-lookup-value`, en `choose` beleid.|Ja|

### <a name="attributes"></a>Kenmerken

|Kenmerk|Description|Vereist|Normaal|
|---------------|-----------------|--------------|-------------|
|gedurende|Hiermee bepaalt u of de `wait` beleid moet wachten voor alle directe onderliggende beleidsregels om te worden voltooid of slechts één. Toegestane waarden zijn:<br /><br /> -   `all` -Wacht voor alle directe onderliggende beleidsregels om te voltooien<br />-een - wachten op elk beleid direct onderliggende om te voltooien. Als het eerste beleid van de direct onderliggende is voltooid, de `wait` beleid is voltooid en uitvoering van een ander beleid direct onderliggende wordt beëindigd.|Nee|alle|

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand back-end
-   **Beleid bereiken:** alle bereiken

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie werken met beleidsregels voor:
+ [Beleid in API Management](api-management-howto-policies.md)
+ [Beleidsexpressies](api-management-policy-expressions.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)
