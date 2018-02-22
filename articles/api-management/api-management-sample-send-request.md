---
title: Met behulp van API Management-service voor het genereren van HTTP-aanvragen
description: Informatie over het gebruik van beleidsregels voor aanvraag en antwoord in API Management externe services van uw API niet aanroepen
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7f3cc81327d1d247fb8e19e256eafb009a5bf162
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Met behulp van externe services van de Azure API Management-service
De beleidsregels die beschikbaar zijn in Azure API Management-service kan een breed scala aan goed functioneren op basis van uitsluitend op de inkomende aanvraag, het uitgaande antwoord en basisinformatie over de configuratie kunnen doen. Echter, kunnen communiceren met externe services van API Management beleid wordt geopend veel meer mogelijkheden.

U hebt eerder hebt gezien hoe om te communiceren met de [Azure Event Hub-service voor logboekregistratie, bewaken en analytics](api-management-log-to-eventhub-sample.md). In dit artikel wordt gedemonstreerd beleidsregels waarmee u communiceren met een externe service op basis van HTTP. Dit beleid kunnen worden gebruikt voor activering van externe gebeurtenissen of voor het ophalen van informatie die wordt gebruikt om de oorspronkelijke aanvraag en antwoord op een bepaalde manier te bewerken.

## <a name="send-one-way-request"></a>Een-manier-verzoek om te verzenden
Mogelijk de meest eenvoudige externe interactie wordt de stijl die wordt gestart en vergeet van aanvraag waarmee een externe service op de hoogte gesteld van een soort belangrijke gebeurtenis. Het beleid voor toegangsbeheer stroom `choose` kan worden gebruikt voor het detecteren van elk soort voorwaarde dat u geïnteresseerd bent in.  Als de voorwaarde is voldaan, kunt u een externe HTTP-aanvraag met de [-één-manier-Verzendaanvraag](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) beleid. Dit kan een aanvraag naar een berichtensysteem zoals Hipchat of vertraging of een e-mailbericht API zoals SendGrid of MailChimp, of voor kritieke ondersteuningsaanvragen als PagerDuty. Alle deze berichtsystemen hebben eenvoudige HTTP-APIs die kunnen worden aangeroepen.

### <a name="alerting-with-slack"></a>Waarschuwingen met Slack
Het volgende voorbeeld laat zien hoe u een bericht te verzenden naar toegestane chatruimten als de HTTP-statuscode van antwoord groter dan of gelijk zijn aan 500 is. Een 500-bereikfout duidt op een probleem met de back-end API die zelf kan niet worden omgezet door de client van de API. Normaal gesproken een soort interventie op API Management-onderdeel.  

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

Vertraging heeft het begrip van inkomende web haken. Bij het configureren van een binnenkomende web haakje genereert Slack een speciale URL, zodat u een eenvoudige POST-aanvraag en het doorgeven van een bericht in de ongebruikt kanaal. De JSON-hoofdtekst die u maakt is gebaseerd op een indeling die is gedefinieerd door vertraging.

![Toegestane Web haakje](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Fire is en goed vergeten?
Er zijn bepaalde voor-en nadelen wanneer u een stijl fire en vergeet van aanvraag. Als voor een bepaalde reden het verzoek mislukken, en vervolgens de fout is niet gerapporteerd. In dit geval wordt de complexiteit van een secundaire fout reporting systeem en de extra kosten van het wachten op het antwoord dat niet gerechtvaardigd. Voor scenario's waar het is essentieel om te controleren van het antwoord, wordt de [aanvragen verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) beleid is een betere optie.

## <a name="send-request"></a>Aanvragen verzenden
De `send-request` beleid maakt gebruik van een externe service voor complexe verwerkingsfuncties uitvoeren en retourneren van gegevens naar de API management-service kunnen worden gebruikt voor verdere beleidsverwerking.

### <a name="authorizing-reference-tokens"></a>Verwijzingstokens autoriseren
Een belangrijke functie van API Management met het beveiligen van de back-end-bronnen. Als de autorisatie-server die wordt gebruikt door uw API maakt [JWT-tokens](http://jwt.io/) als onderdeel van de stroom OAuth2 als [Azure Active Directory](../active-directory/active-directory-aadconnect.md) komt, kunt u de `validate-jwt` beleid om de geldigheid van het token. Sommige autorisatieservers maken zogenaamde [verwijzen naar tokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) die zonder een retouraanroep de autorisatie-server kan niet worden geverifieerd.

### <a name="standardized-introspection"></a>Gestandaardiseerde introspection
In het verleden is er geen gestandaardiseerde manier om een verwijzingstoken met een server van de autorisatie te controleren. Echter een onlangs voorgestelde standaard [RFC 7662](https://tools.ietf.org/html/rfc7662) is gepubliceerd door de IETF die definieert hoe een resource-server kan de geldigheid van een token.

### <a name="extracting-the-token"></a>Het uitpakken van het token
De eerste stap is het token ophalen uit de autorisatie-header. Waarde van de header moet zijn geformatteerd met het `Bearer` autorisatieschema, een spatie en vervolgens het verificatietoken conform [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Er zijn helaas gevallen waarbij het autorisatieschema wordt weggelaten. Voor het account voor deze tijdens het parseren van API Management splitst de waarde van de header op een spatie en selecteert de laatste tekenreeks in de geretourneerde matrix van tekenreeksen. Dit biedt een oplossing voor onjuist ingedeeld autorisatie-header.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>Maken van de validatieaanvraag
Als een API Management heeft het verificatietoken, kan de aanvraag voor het valideren van het token maken om API Management. RFC 7662 roept deze introspection proces en vereist dat u `POST` een HTML-formulier op de resource introspection. Het HTML-formulier moet ten minste een sleutel-waardepaar bevatten met de sleutel `token`. Deze aanvraag naar de autorisatie-server moet ook zijn geverifieerd, om ervoor te zorgen dat kwaadwillende clients kunnen niet voor geldige-tokens sleepnetten gaat.

```xml
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
```

### <a name="checking-the-response"></a>Het antwoord controleren
De `response-variable-name` kenmerk wordt gebruikt om toegang te geven het geretourneerde antwoord. De naam zijn gedefinieerd in deze eigenschap kan worden gebruikt als een sleutel in de `context.Variables` woordenlijst voor toegang tot de `IResponse` object.

Uit het antwoordobject, kunt u de instantie ophalen en RFC 7622 API Management wordt uitgelegd dat het antwoord een JSON-object moet zijn en moet ten minste een eigenschap genaamd bevatten `active` dat een Booleaanse waarde. Wanneer `active` is ingesteld op true en vervolgens het token als geldig wordt aangemerkt.

### <a name="reporting-failure"></a>Reporting is mislukt
U kunt een `<choose>` beleid om te detecteren of het token ongeldig is en als dat het geval is, retourneert een 401-respons.

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

Als per [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) waarin wordt beschreven hoe `bearer` tokens moeten worden gebruikt, API Management ook retourneert een `WWW-Authenticate` header met de 401-respons. De WWW-verificatie is bedoeld om een client op het maken van een aanvraag goed geautoriseerde instrueren. Vanwege de grote verscheidenheid aan benaderingen mogelijk met de OAuth2-framework is het lastig om alle benodigde informatie te communiceren. Er zijn gelukkig pogingen uitgevoerd om te helpen [clients ontdekken hoe goed aanvragen naar een resource-server te autoriseren](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Definitieve oplossing
Aan het einde krijgt u het volgende beleid:

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

Dit is slechts een aantal voorbeelden van hoe de `send-request` beleid kan worden gebruikt om nuttige externe services integreren met het proces voor aanvragen en -antwoorden die door de API Management-service.

## <a name="response-composition"></a>Antwoord samenstelling
De `send-request` beleid kan worden gebruikt voor het verbeteren van een primaire aanvraag voor een back endsysteem, zoals u in het vorige voorbeeld hebt gezien, of deze kan worden gebruikt als een volledige vervangen voor van de back-end-aanroep. Met deze techniek kunt u gemakkelijk maken samengestelde bronnen die worden samengevoegd uit meerdere verschillende systemen.

### <a name="building-a-dashboard"></a>Een dashboard maken
Soms wilt u mogelijk gegevens die in meerdere back endsystemen, bijvoorbeeld voorkomt openbaar, om een dashboard. De KPI's afkomstig zijn van alle verschillende back-ends, maar u liever niet voorzien in directe toegang tot en het normaal zou zijn handig als u alle gegevens in één aanvraag kan worden opgehaald. Enkele van de back-end-gegevens moet wellicht enkele segmenteren en opdelen en eerst een beetje opschonen! Kunnen in de cache die samengestelde bron zou een nuttig zijn voor de back-end-belasting te verminderen, zoals u weet dat gebruikers hebben een gewoonte van hammering de toets F5 om te zien als hun underperforming metrische gegevens kan worden gewijzigd.    

### <a name="faking-the-resource"></a>De resource faking
De eerste stap voor het ontwikkelen van de bron van het dashboard is voor het configureren van een nieuwe bewerking in de Azure portal. Dit is een tijdelijke aanduiding voor bewerking gebruikt voor het configureren van een samenstelling beleid voor het bouwen van de dynamische resource.

![Dashboard-bewerking](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>De aanvragen
Wanneer de bewerking is gemaakt, kunt u een beleid specifiek voor deze bewerking kunt configureren. 

![Dashboard-bewerking](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

De eerste stap is queryparameters ophalen uit de binnenkomende aanvraag zodat u ze naar de back-end doorsturen kunt. In dit voorbeeld wordt het dashboard op basis van een bepaalde periode informatie wordt weergegeven en is daarom heeft een `fromDate` en `toDate` parameter. U kunt de `set-variable` beleid voor het ophalen van de informatie uit de aanvraag-URL.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Zodra u deze informatie hebt, kunt u aanvragen op alle back endsystemen. Elke aanvraag vormt een nieuwe URL met de parameterinformatie en roept de desbetreffende server en het antwoord worden opgeslagen in een context-variabele.

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Deze aanvragen worden uitgevoerd in de juiste volgorde niet ideaal is. 

### <a name="responding"></a>Reageert
Kan het antwoord samengesteld, kunt u de [return antwoord](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) beleid. De `set-body` element een expressie kunt gebruiken om een nieuwe samen te stellen `JObject` met alle onderdeel verklaringen ingesloten als eigenschappen.

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

Het volledige beleid ziet er als volgt uit:

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

U kunt het dashboard resource mogen worden opgeslagen voor ten minste een uur configureren in de configuratie van de tijdelijke aanduiding voor bewerking. 

## <a name="summary"></a>Samenvatting
Azure API Management-service biedt flexibele beleidsregels die u kunnen selectief worden toegepast op HTTP-verkeer en Hiermee samenstelling van back-end-services. Of u wilt uw API-gateway met waarschuwingen, functies, verificatie, validatie-mogelijkheden verbeteren of maken van nieuwe samengestelde resources op basis van meerdere back-end-services de `send-request` en bijbehorende beleid opent u een wereld van mogelijkheden.

## <a name="watch-a-video-overview-of-these-policies"></a>Bekijk een video-overzicht van deze beleidsregels
Voor meer informatie over de [-één-manier-Verzendaanvraag](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [aanvragen verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), en [return antwoord](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) beleidsregels die in dit artikel bevat de volgende video:

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

