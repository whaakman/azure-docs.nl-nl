---
title: Aangepaste caching in Azure API Management
description: Leer hoe u items in de cache door sleutel in Azure API Management
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 922ab731ccd76e6a1336d61abe4b0251e358beb7
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793540"
---
# <a name="custom-caching-in-azure-api-management"></a>Aangepaste caching in Azure API Management
Azure API Management-service heeft ingebouwde ondersteuning voor [HTTP-antwoorden in cache opslaan](api-management-howto-cache.md) met behulp van de bron-URL als de sleutel. De sleutel kan worden gewijzigd door aanvraagheaders met behulp van de `vary-by` eigenschappen. Dit is handig voor het hele HTTP-antwoorden (ook wel opmerkingen) in cache opslaan, maar soms is het nuttig om alleen cache een gedeelte van een weergave. De nieuwe [cache-lookup-waarde](/azure/api-management/api-management-caching-policies#GetFromCacheByKey) en [cache-store-waarde](/azure/api-management/api-management-caching-policies#StoreToCacheByKey) -beleid biedt u de mogelijkheid voor het opslaan en ophalen van willekeurige delen van gegevens vanuit beleidsdefinities. Deze mogelijkheid wordt ook waarde toegevoegd aan de eerder geïntroduceerd [Verzendaanvraag](/azure/api-management/api-management-advanced-policies#SendRequest) beleid omdat kunt u de antwoorden van externe services nu cache.

## <a name="architecture"></a>Architectuur
API Management-service wordt gebruikt een gedeelde per tenant-gegevens in de cache zodat, als u omhoog schalen naar meerdere eenheden krijgt u nog steeds toegang tot dezelfde gegevens in de cache. Als u werkt met een implementatie voor meerdere regio's zijn er echter onafhankelijk caches in elk van de regio's. Het is belangrijk dat u de cache niet behandelen als een gegevensarchief, waar de enige bron van een stukje informatie is. Als u hebt en u later besluit om te profiteren van de implementatie voor meerdere regio's, kunnen de klanten met gebruikers die mogelijk toegang tot die gegevens in de cache verliezen.

## <a name="fragment-caching"></a>Fragment opslaan in cache
Er zijn bepaalde gevallen waarbij een gedeelte van de gegevens die is duur om te bepalen en blijft het nog vers voor een redelijk tijdsbestek de bevatten, antwoorden worden geretourneerd. Een voorbeeld: Houd rekening met een service die zijn gemaakt door een luchtvaartmaatschappij waarmee informatie over Vluchtreserveringen, vlucht status, enzovoort. Als de gebruiker lid is van het programma van de punten luchtvaartmaatschappijen, ook het geval ook informatie met betrekking tot de huidige status en reiskosten samengevoegd. Deze informatie met betrekking tot gebruiker kan worden opgeslagen in een ander systeem, maar kan het wenselijk zijn opgenomen in antwoorden geretourneerd over de status van vertragingen van vluchten en reserveringen zijn. Dit kan worden gedaan met behulp van een proces genaamd fragment opslaan in cache. De primaire weergave kan worden geretourneerd vanaf de oorspronkelijke server met behulp van een soort token om aan te geven waar de gebruiker-gerelateerde informatie moet worden ingevoegd. 

Houd rekening met het volgende JSON-antwoord van een back-end-API.

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

En secundaire resource `/userprofile/{userid}` die vergelijkbaar is,

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

Moeten identificeren die de eindgebruiker is om te bepalen van de juiste informatie over te nemen, API Management. Dit mechanisme is afhankelijk van het-implementatie. Bijvoorbeeld: ik gebruik de `Subject` claimen van een `JWT` token. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

API Management-winkels de `enduserid` waarde in een contextvariabele voor later gebruik. De volgende stap is om te bepalen of een eerdere aanvraag al is opgehaald van de gebruikersgegevens en in de cache opgeslagen. Voor deze API Management maakt gebruik van de `cache-lookup-value` beleid.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Als er geen vermelding in de cache die overeenkomt met de sleutelwaarde en geen `userprofile` contextvariabele is gemaakt. API Management controleert of het succes van het gebruik van het opzoeken van de `choose` stroom beleid beheren.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

Als de `userprofile` contextvariabele bestaat niet en gaat API Management hebt om een HTTP-aanvraag te halen.

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

API Management maakt gebruik van de `enduserid` te maken van de URL van de gebruiker de resource-profiel. Als een API Management heeft het antwoord, haalt de platte tekst uit het antwoord en slaat deze weer in een contextvariabele.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

Om te voorkomen dat API Management deze HTTP-aanvraag opnieuw aanbrengen wanneer dezelfde gebruiker een andere aanvraag indient, kunt u opgeven voor het opslaan van het gebruikersprofiel in de cache.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

API Management wordt de waarde in de cache met behulp van de exacte dezelfde sleutel die API Management is oorspronkelijk heeft geprobeerd om op te halen met opgeslagen. De duur van die API Management kiest voor het opslaan van de waarde moet worden gebaseerd op hoe vaak de gegevens-wijzigingen en hoe fouttolerante gebruikers worden naar verouderde gegevens. 

Het is belangrijk om te profiteren van dat ophalen uit de cache nog steeds een out-of-process, netwerkaanvraag is en mogelijk nog steeds tientallen milliseconden aan de aanvraag toevoegen kunt. De voordelen worden geleverd bij het bepalen van dat de gebruikersprofielgegevens langer duurt dan die vanwege hoeft te database-query's of statistische gegevens uit meerdere back-ends.

De laatste stap in het proces is om bij te werken van het geretourneerde antwoord met informatie over de gebruikersprofielen.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

U kunt kiezen om op te nemen van de aanhalingstekens als onderdeel van het token dat, zelfs wanneer de vervangen niet wordt uitgevoerd, het antwoord nog steeds een geldige JSON is.  

Nadat u alle stappen samen combineren, is het eindresultaat een beleid dat lijkt op de volgende uitvoer.

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

Deze benadering in het cachegeheugen wordt voornamelijk gebruikt in web sites waarbij HTML wordt samengesteld aan de serverzijde zodat deze kan worden gerenderd als één pagina. Het kan ook nuttig in API's wanneer clients niet HTTP-caching aan clientzijde of het is niet wenselijk dat de verantwoordelijkheid plaatsen op de client zijn.

Dit dezelfde soort fragment caching kan ook worden uitgevoerd op de back-end-webservers met behulp van een Redis cache van de server, maar met behulp van de API Management-service om dit werk te doen is nuttig wanneer het in de cache-fragmenten afkomstig zijn uit verschillende back-ends dan de primaire antwoorden.

## <a name="transparent-versioning"></a>Transparante versiebeheer
Het is gebruikelijk om voor meerdere versies van de andere implementatie van een API voor het op elk gewenst moment worden ondersteund. Bijvoorbeeld voor de ondersteuning van verschillende omgevingen (ontwikkelen, testen, productie, enzovoort) of voor de ondersteuning van oudere versies van de API om tijd voor de API-consumenten te migreren naar nieuwere versies te geven. 

Een aanpak voor het afhandelen van dit, in plaats van dat ontwikkelaars van de client te wijzigen van de URL's van `/v1/customers` naar `/v2/customers` is om op te slaan in de profielgegevens van de consument welke versie van de API die ze momenteel wilt gebruiken en de URL van de juiste back-end aanroepen. Om te bepalen van de juiste back-end-URL voor het aanroepen van een bepaalde client, is het nodig om sommige configuratiegegevens op te vragen. Door deze configuratiegegevens caching, kunt API Management minimaliseren van de prestaties nadelig worden beïnvloed deze opzoektabel te doen.

De eerste stap is om te bepalen van de id die wordt gebruikt voor het configureren van de gewenste versie. Ik heb geselecteerd in het volgende voorbeeld om de versie om de productcode van het abonnement te koppelen. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

API Management biedt vervolgens een opzoeken van de cache om te zien of deze al de versie van de gewenste client opgehaald.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

API Management wordt vervolgens gecontroleerd om te zien als deze niet in de cache vinden is.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

Als API Management is niet wordt gevonden, wordt het door de API Management opgehaald.

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

Haal de hoofdtekst van antwoord uit het antwoord.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

Het Store terug in het cachegeheugen voor toekomstig gebruik.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

En tot slot werkt u de URL van de back-end om te selecteren van de versie van de gewenste door de client-service.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

Het volledige beleid is als volgt:

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

Inschakelen van de consumenten API transparant bepalen welke endversie van de back-die wordt gebruikt door clients zonder te hoeven bijwerken en opnieuw implementeren van clients is een elegante oplossing waarmee veel API-versiebeheer problemen-adressen.

## <a name="tenant-isolation"></a>Isolatie van tenants
Sommige bedrijven Maak in grotere, multitenant implementaties afzonderlijke groepen van tenants op verschillende implementaties van de back-end-hardware. Dit verkleint het aantal klanten die worden beïnvloed door een hardwareprobleem op de back-end. U kunt ook nieuwe versies van software om te worden geïmplementeerd in fasen. Deze architectuur back-end moet in het ideale geval transparant voor gebruikers van de API. Dit kan worden bereikt op een soortgelijke manier op transparante versiebeheer omdat deze is gebaseerd op dezelfde techniek van het manipuleren van de back-end-URL met behulp van de status van de configuratie per API-sleutel.  

In plaats van een gewenste versie van de API voor elke abonnementssleutel wordt geretourneerd, zou u een id die is gekoppeld aan een tenant aan de Hardwaregroep toegewezen retourneren. Deze id kan worden gebruikt om de juiste back-end-URL samen te stellen.

## <a name="summary"></a>Samenvatting
De vrijheid om te gebruiken van de Azure API management-cache voor het opslaan van elk soort gegevens kan efficiënte toegang tot de configuratiegegevens die invloed kunnen zijn op de manier waarop die een binnenkomende aanvraag is verwerkt. Het kan ook worden gebruikt voor het opslaan van fragmenten voor gegevens die antwoorden, geretourneerd vanuit een back-end-API kunnen verbeteren.
