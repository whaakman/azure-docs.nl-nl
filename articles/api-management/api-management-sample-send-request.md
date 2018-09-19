---
title: Met behulp van API Management-service voor het genereren van HTTP-aanvragen
description: Informatie over het gebruik van de aanvraag- en -beleid in API Management naar externe services aanroepen vanuit uw API
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: fdcc230171006c6388e75b947e10a73fb953001a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294673"
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Externe services van de Azure API Management-service gebruiken
Het beleid dat beschikbaar is in Azure API Management-service kan een breed scala aan nuttig werk op basis van alleen de inkomende aanvraag, de uitgaande antwoord, en informatie over de standaardconfiguratie. Echter, de mogelijkheid om te communiceren met externe services van API Management beleid opent u veel meer mogelijkheden.

U hebt eerder hebt gezien hoe om te communiceren met de [Azure Event Hub-service voor logboekregistratie, bewaking en analyse](api-management-log-to-eventhub-sample.md). In dit artikel ziet u beleidsregels waarmee u kunt om te communiceren met een externe service op basis van HTTP. Dit beleid kunnen worden gebruikt voor het activeren van externe evenementen of voor het ophalen van informatie die wordt gebruikt voor het bewerken van de oorspronkelijke aanvraag en respons op een bepaalde manier.

## <a name="send-one-way-request"></a>Een-manier-verzoek om te verzenden
Mogelijk de eenvoudigste externe interactie is de stijl fire-and-forgetstromen van aanvraag waarmee een externe service om te worden geïnformeerd over een of andere belangrijke gebeurtenis. Beleid voor de stroom `choose` kan worden gebruikt voor het detecteren van welke aard dan ook van de voorwaarde die u geïnteresseerd bent in.  Als de voorwaarde is voldaan, kunt u een externe HTTP-aanvraag met de [-één-manier-Verzendaanvraag](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) beleid. Dit kan een aanvraag voor een berichtensysteem zoals Hipchat of Slack, of een e-mailbericht API, zoals SendGrid of MailChimp, of voor kritieke support-incidenten, iets zoals PagerDuty. Al deze berichtsystemen hebben eenvoudige HTTP-APIs die kunnen worden aangeroepen.

### <a name="alerting-with-slack"></a>Waarschuwingen met Slack
Het volgende voorbeeld ziet u hoe u een bericht verzenden naar een Slack chatruimte als de HTTP-antwoordstatuscode groter dan of gelijk aan 500 is. Een 500-bereikfout duidt op een probleem met de back-end API die de client van de API kan niet worden opgelost. Hiervoor wordt meestal een soort interventie op API Management-onderdeel.  

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

Slack is het begrip van inkomende webhooks. Bij het configureren van een binnenkomende webhook, wordt een speciale URL, waarmee u een eenvoudige POST-aanvraag doen en om door te geven van een bericht naar het Slack-kanaal in Slack gegenereerd. De JSON-hoofdtekst die u maakt, is gebaseerd op een indeling die is gedefinieerd door Slack.

![Slack-Webhook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Fire is en goed genoeg vergeten?
Er zijn bepaalde afwegingen bij het gebruik van een fire-and-forget-stijl van de aanvraag. Als voor een of andere reden, de aanvraag mislukt, en vervolgens de fout wordt niet gerapporteerd. In dit geval, is de complexiteit van een secundaire fout melden van systeem en de kosten voor extra prestaties wachten op het antwoord dat u hoeft niet gerechtvaardigd. Voor scenario's waarin het essentieel om te controleren of het antwoord, dan zal de [Verzendaanvraag](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) beleid is een betere optie.

## <a name="send-request"></a>Verzonden aanvragen
De `send-request` beleid maakt met een externe service voor de complexe verwerkingsfuncties uitvoeren en retourneren gegevens naar de API management-service die kunnen worden gebruikt voor verdere beleidsverwerking.

### <a name="authorizing-reference-tokens"></a>Naslaginformatie over tokens autoriseren
Een belangrijke functie van API Management beveiligt back-endresources. Als de autorisatieserver die wordt gebruikt door uw API maakt [JWT-tokens](http://jwt.io/) als onderdeel van de stroom OAuth2 als [Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md) heeft, kunt u de `validate-jwt` beleid om te controleren of de geldigheid van het token. Sommige autorisatieservers maken ook [verwijzen naar tokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) die zonder een retouraanroep de autorisatie-server kan niet worden geverifieerd.

### <a name="standardized-introspection"></a>Gestandaardiseerde introspection
In het verleden ligt en is er geen gestandaardiseerde manier controleren of u een referentie-token met een autorisatie-server. Maar een onlangs voorgestelde standaard [RFC 7662](https://tools.ietf.org/html/rfc7662) is gepubliceerd door de IETF waarmee wordt gedefinieerd hoe een resource-server kunt controleren of de geldigheid van een token.

### <a name="extracting-the-token"></a>Uitpakken van het token
De eerste stap is het token ophalen uit de autorisatie-header. De waarde voor header moet zijn geformatteerd met het `Bearer` autorisatieschema, een spatie en het Autorisatietoken volgens [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Er zijn helaas gevallen waar de autorisatieschema wordt weggelaten. Voor het account voor dit bij het parseren van API Management splitst de waarde van de header op een spatie en selecteert de laatste tekenreeks van de geretourneerde matrix met tekenreeksen. Dit biedt een oplossing voor de onjuist opgemaakte autorisatie-header.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>De validatie-aanvraag
Als een API Management heeft het Autorisatietoken, kunnen API Management maken van de aanvraag voor het valideren van het token. RFC 7662 roept deze introspection proces en vereist dat u `POST` een HTML-formulier aan de resource introspection. De HTML-formulier moet ten minste een sleutel/waarde-paar bevatten met de sleutel `token`. Deze aanvraag naar de autorisatie-server moet ook worden geverifieerd, om ervoor te zorgen dat schadelijke clients kunnen geen voor geldige-tokens sleepnetten gaat.

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
De `response-variable-name` kenmerk wordt gebruikt om toegang te geven het geretourneerde antwoord. De naam die is gedefinieerd in deze eigenschap kan worden gebruikt als een sleutel in de `context.Variables` woordenlijst voor toegang tot de `IResponse` object.

In het antwoordobject, kunt u de instantie ophalen en RFC 7622 API Management geeft aan dat het antwoord een JSON-object moet en moet ten minste een eigenschap met de naam bevatten `active` dat wil zeggen een Booleaanse waarde. Wanneer `active` is ingesteld op true en vervolgens het token als geldig wordt aangemerkt.

### <a name="reporting-failure"></a>Fout melden
U kunt een `<choose>` beleid om te detecteren of het token ongeldig is en als dit het geval is, retourneert een 401-respons.

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

Als per [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) waarin wordt beschreven hoe `bearer` tokens moeten worden gebruikt, API Management geeft ook een `WWW-Authenticate` -header met de 401-respons. De WWW-verificatie is bedoeld om te geven van een client op het maken van een goed gemachtigde aanvraag. Vanwege de grote verscheidenheid aan benaderingen mogelijk is met het OAuth2-framework is het moeilijk zijn om te communiceren, de benodigde gegevens. Gelukkig zijn er inspanningen uitgevoerd om te helpen [clients ontdekken hoe goed om aanvragen te autoriseren met een resource-server](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

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

Dit is slechts een aantal voorbeelden van hoe de `send-request` beleid kan worden gebruikt om nuttige externe services integreren in het proces voor aanvragen en antwoorden die worden gebruikt met de API Management-service.

## <a name="response-composition"></a>Samenstelling van de reactie
De `send-request` beleid kan worden gebruikt voor het verbeteren van een primaire aanvraag met een back endsysteem, zoals u in het vorige voorbeeld hebt gezien, of deze kan worden gebruikt als een volledige vervangen voor van de back-end-aanroep. Met deze techniek kunt u eenvoudig maken samengestelde resources die worden samengevoegd uit meerdere verschillende systemen.

### <a name="building-a-dashboard"></a>Het bouwen van een dashboard
Soms wilt u mogelijk om beschikbaar te stellen van de informatie die deel uitmaakt van meerdere back-endsystemen, bijvoorbeeld, om een dashboard te stimuleren. De KPI's zijn afkomstig uit alle verschillende back-ends, maar u liever niet wilt voorzien in directe toegang tot en het zou niet geweldig zijn als alle gegevens in een enkele aanvraag kan worden opgehaald. Sommige van de gegevens van de back-end moet mogelijk bepaalde segmenteren en op te delen en wat bij het opschonen van eerst! Geen samengestelde resource in de cache is een handig om de back-end-belasting te verminderen, zoals u weet dat gebruikers hebben een gewoonte van voorkomen de F5-toets om te kunnen bekijken als hun onder de maat presterende metrische gegevens kunnen worden gewijzigd.    

### <a name="faking-the-resource"></a>De resource faking
De eerste stap bij het bouwen van de dashboard-resource is het configureren van een nieuwe bewerking in Azure portal. Dit is een tijdelijke aanduiding voor bewerking gebruikt voor het configureren van een samenstelling beleid voor het bouwen van de dynamische-resource.

![Dashboard-bewerking](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>De aanvragen
Zodra de bewerking is gemaakt, kunt u een beleid specifiek voor deze bewerking kunt configureren. 

![Dashboard-bewerking](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

De eerste stap is om op te halen van alle queryparameters van de inkomende aanvraag, zodat u ze naar de back-end doorsturen kunt. In dit voorbeeld wordt het dashboard wordt weergegeven gegevens op basis van een bepaalde periode en daarom heeft een `fromDate` en `toDate` parameter. U kunt de `set-variable` beleid voor het extraheren van de informatie van de aanvraag-URL.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Zodra u deze informatie hebt, kunt u aanvragen naar de back-endsystemen. Elke aanvraag wordt een nieuwe URL met de parametergegevens en roept de desbetreffende server en slaat het antwoord in een contextvariabele.

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

Deze aanvragen worden uitgevoerd in de reeks, dit niet ideaal is. 

### <a name="responding"></a>Reageert
Kan het samengestelde antwoord, kunt u de [retour-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) beleid. De `set-body` element een expressie kunt gebruiken om samen te stellen een nieuwe `JObject` met alle van de component-representaties ingesloten als eigenschappen.

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

In de configuratie van de tijdelijke aanduiding-bewerking, kunt u de dashboard-resource in de cache opgeslagen ten minste een uur configureren. 

## <a name="summary"></a>Samenvatting
Azure API Management-service biedt flexibele beleidsregels die selectief kunnen worden toegepast op HTTP-verkeer en samenstelling van de back-endservices kunt. Of u wilt uw API-gateway met waarschuwingen van functies, controle, validatie-mogelijkheden verbeteren of nieuwe samengestelde bronnen op basis van meerdere back-endservices, maken de `send-request` en bijbehorende beleid openen in een wereld aan mogelijkheden.

