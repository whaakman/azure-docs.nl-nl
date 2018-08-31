---
title: Uitzondering Management - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die beschikbaar zijn in de Threat Modeling Tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: ce748be7f11d440e656e4af5cdd3cee3bbc9e313
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43302146"
---
# <a name="security-frame-exception-management--mitigations"></a>Beveiliging-Frame: Uitzonderingsbeheer | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF - kan geen serviceDebug knooppunt in het configuratiebestand](#servicedebug)</li><li>[WCF - kan geen serviceMetadata knooppunt in het configuratiebestand](#servicemetadata)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat afhandeling van uitzonderingen correct wordt uitgevoerd in de ASP.NET-Web-API ](#exception)</li></ul> |
| **Web-App** | <ul><li>[Beveiligingsdetails van de in foutberichten niet zichtbaar ](#messages)</li><li>[Pagina voor foutafhandeling standaard implementeren ](#default)</li><li>[Implementatiemethode voor instellen naar de handelsversie in IIS](#deployment)</li><li>[Uitzonderingen zou moeten veilig mislukken](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF - kan geen serviceDebug knooppunt in het configuratiebestand

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_debug_information) |
| **Stappen** | Windows Communication Framework (WCF) services kan worden geconfigureerd om informatie over foutopsporing weer te geven. Fouten opsporen in informatie mag niet worden gebruikt in een productieomgeving. De `<serviceDebug>` tag bepaalt of de functie voor foutopsporing-informatie voor een WCF-service is ingeschakeld. Als het kenmerk includeExceptionDetailInFaults is ingesteld op true, informatie over de uitzondering van de toepassing wordt geretourneerd naar clients. Aanvallers kunnen de aanvullende informatie die zij krijgen vanuit het opsporen van fouten in de uitvoer voor het koppelen van aanvallen die zijn gericht op het framework, een database of andere resources die worden gebruikt door de toepassing. |

### <a name="example"></a>Voorbeeld
Het volgende configuratiebestand bevat de `<serviceDebug>` tag: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Informatie over foutopsporing in de service uitschakelen. Dit kan worden bewerkstelligd door het verwijderen van de `<serviceDebug>` tag van het configuratiebestand van uw toepassing. 

## <a id="servicemetadata"></a>WCF - kan geen serviceMetadata knooppunt in het configuratiebestand

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Algemeen, NET Framework 3 |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_service_enumeration) |
| **Stappen** | Wanneer gegevens over een service openbaar beschikbaar komen, kan aanvallers met waardevolle inzicht in hoe ze misbruik van de service maken kunnen bieden. De `<serviceMetadata>` code kan de functie voor het publiceren van metagegevens. De metagegevens van de service kan gevoelige gegevens bevatten die niet openbaar toegankelijk moet zijn. Ten minste, kunt u alleen vertrouwde gebruikers toegang tot de metagegevens en ervoor zorgen dat die onnodige gegevens niet weergegeven. Nog beter, de mogelijkheid voor het publiceren van metagegevens volledig uitschakelen. Een veilige WCF-configuratie bevat niet de `<serviceMetadata>` tag. |

## <a id="exception"></a>Zorg ervoor dat afhandeling van uitzonderingen correct wordt uitgevoerd in de ASP.NET-Web-API

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC 5, 6 MVC |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Afhandeling van uitzonderingen in ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [Model voor validatie in ASP.NET Web-API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Stappen** | Standaard worden meeste niet-onderschepte uitzonderingen in ASP.NET-Web-API vertaald naar een HTTP-antwoord met de statuscode `500, Internal Server Error`|

### <a name="example"></a>Voorbeeld
Voor het beheren van de statuscode die is geretourneerd door de API `HttpResponseException` kan worden gebruikt, zoals hieronder weergegeven: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a>Voorbeeld
Voor verdere controle van het antwoord van de uitzondering, de `HttpResponseMessage` klasse kan worden gebruikt, zoals hieronder wordt weergegeven: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
Om af te vangen onverwerkte uitzonderingen die niet van het type `HttpResponseException`, uitzondering Filters kunnen worden gebruikt. Uitzondering filters implementeren de `System.Web.Http.Filters.IExceptionFilter` interface. De eenvoudigste manier om het schrijffilter is een uitzondering is worden afgeleid van de `System.Web.Http.Filters.ExceptionFilterAttribute` klasse en de OnException-methode overschrijven. 

### <a name="example"></a>Voorbeeld
Hier volgt een filter dat wordt geconverteerd `NotImplementedException` uitzonderingen in de HTTP-statuscode `501, Not Implemented`: 
```csharp
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

Er zijn verschillende manieren voor het registreren van een Web-API uitzonderingsfilter:
- Door de actie
- Door de netwerkcontroller
- wereldwijd

### <a name="example"></a>Voorbeeld
Als u wilt de filter toepassen op een specifieke actie, moet u het filter toevoegen als een kenmerk met de actie: 
```csharp
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a>Voorbeeld
Het filter toepassen op alle acties die op een `controller`, het filter toevoegen als een kenmerk voor de `controller` klasse: 

```csharp
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Voorbeeld
Als u wilt het filter wereldwijd toepassen op alle Web-API-domeincontrollers, een exemplaar van het filter toevoegen de `GlobalConfiguration.Configuration.Filters` verzameling. Uitzondering filters in deze verzameling zijn van toepassing op elke Web-API-controller-actie. 
```csharp
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Voorbeeld
Voor de validatie, kan de modelstatus worden doorgegeven aan CreateErrorResponse methode, zoals hieronder weergegeven: 
```csharp
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

Controleer de koppelingen in de sectie Verwijzingen voor meer informatie over het afhandelen van uitzonderlijke en modelvalidatie van het in ASP.Net-Web-API 

## <a id="messages"></a>Beveiligingsdetails van de in foutberichten niet zichtbaar

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Algemene foutberichten zijn rechtstreeks naar de gebruiker opgegeven zonder op te nemen van gegevens voor gevoelige toepassingen. Voorbeelden van gevoelige gegevens zijn:</p><ul><li>Servernamen</li><li>Verbindingsreeksen</li><li>Gebruikersnamen</li><li>Wachtwoorden</li><li>SQL-procedures</li><li>Details van dynamische SQL-fouten</li><li>Stack-trace en regels code</li><li>Variabelen die zijn opgeslagen in het geheugen</li><li>Station en de maplocaties</li><li>Toepassing installeren punten</li><li>Host-configuratie-instellingen</li><li>De details van andere interne toepassing</li></ul><p>Alle fouten in een toepassing overlapping en bieden algemene foutberichten, evenals aangepaste fouten in IIS inschakelen wordt voorkomen dat openbaarmaking van informatie. SQL Server-database en verwerking, onder andere architecturen voor foutafhandeling van .NET-uitzonderingen zijn met name uitgebreide en zeer nuttig zijn om een kwaadwillende gebruiker profilering van uw toepassing. Niet rechtstreeks de inhoud van een klasse die is afgeleid van de .NET-uitzonderingsklasse weer te geven, en zorg ervoor dat de juiste afhandeling van uitzonderingen hebben zodat een onverwachte uitzondering per ongeluk rechtstreeks naar de gebruiker is niet gegenereerd.</p><ul><li>Geef algemene foutberichten rechtstreeks aan de gebruiker die abstracte opslag specifieke gegevens rechtstreeks in de uitzondering/foutbericht</li><li>De inhoud van een .NET-uitzonderingsklasse niet rechtstreeks naar de gebruiker weergeven</li><li>Alle foutberichten afvangen en indien van toepassing informeren over de gebruiker via een algemene foutmelding verzonden naar de toepassingsclient</li><li>De inhoud van de uitzonderingsklasse niet rechtstreeks naar de gebruiker, met name de geretourneerde waarde van blootstellen `.ToString()`, of de waarden van eigenschappen van het bericht of StackTrace. Veilig Meld deze informatie en een meer onschuldig bericht aan de gebruiker weergeven</li></ul>|

## <a id="default"></a>Pagina voor foutafhandeling standaard implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Dialoogvenster van de instellingen voor ASP.NET-foutpagina's bewerken](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Stappen** | <p>Wanneer een ASP.NET-toepassing mislukt en zorgt ervoor dat een HTTP/1.x 500 Interne serverfout opgetreden of een functieconfiguratie (zoals Aanvraagfiltering) voorkomt dat een pagina worden weergegeven, kunt u een foutbericht dat wordt gegenereerd. Beheerders kunnen kiezen of een bericht van de toepassing moet worden weergegeven voor de client, de gedetailleerd foutbericht naar de client of het gedetailleerde foutbericht op dat alleen localhost. De <customErrors> tag op in het bestand web.config heeft drie modi:</p><ul><li>**Op:** geeft aan dat de aangepaste fouten zijn ingeschakeld. Als geen kenmerk defaultRedirect is opgegeven, zien gebruikers een algemene fout. De aangepaste fouten worden weergegeven met de externe clients en op de lokale host</li><li>**Uitgeschakeld:** geeft aan dat de aangepaste fouten zijn uitgeschakeld. De gedetailleerde ASP.NET-fouten worden weergegeven met de externe clients en op de lokale host</li><li>**Ingesteld op RemoteOnly:** geeft aan dat aangepaste fouten worden alleen weergegeven op de externe clients, en dat de ASP.NET-fouten worden weergegeven op de lokale host. Dit is de standaardwaarde</li></ul><p>Open de `web.config` -bestand voor de toepassing/site en zorg ervoor dat de tag een heeft `<customErrors mode="RemoteOnly" />` of `<customErrors mode="On" />` gedefinieerd.</p>|

## <a id="deployment"></a>Implementatiemethode voor instellen naar de handelsversie in IIS

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [implementatie-Element (ASP.NET-instellingenschema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Stappen** | <p>De `<deployment retail>` switch is bedoeld voor gebruik door IIS productieservers. Deze switch wordt gebruikt om u te helpen toepassingen uitvoeren met de best mogelijke prestaties en de geringst mogelijke beveiliging gegevens lekken door het uitschakelen van de toepassing de mogelijkheid voor het genereren van trace-uitvoer op een pagina, uitschakelen van de mogelijkheid om gedetailleerde foutberichten weer te geven eindgebruikers en de switch foutopsporing uit te schakelen.</p><p>Opties die ontwikkelaars zijn gericht, zoals is mislukt, switches en vaak aanvragen traceren en foutopsporing, zijn ingeschakeld tijdens het ontwikkelen van active. Het verdient aanbeveling dat de implementatiemethode op een productieserver naar de handelsversie worden ingesteld. Open het bestand machine.config en zorg ervoor dat `<deployment retail="true" />` blijven ingesteld op true.</p>|

## <a id="fail"></a>Uitzonderingen zou moeten veilig mislukken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Veilig mislukt](https://www.owasp.org/index.php/Fail_securely) |
| **Stappen** | Toepassing zou moeten veilig mislukken. Elke methode een Booleaanse waarde retourneert, op basis van welke bepaalde beslissing is genomen, hebt uitzonderingsblok zorgvuldig gemaakt. Er zijn veel van de logische fouten vanwege die kneep beveiligingsproblemen in, wanneer het uitzonderingsblok ondoordacht wordt geschreven.|

### <a name="example"></a>Voorbeeld
```csharp
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check to enable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
De bovenstaande methode retourneert altijd waar, als het geval is een uitzondering. Als de eindgebruiker een onjuist gevormde URL, die de browser respecteert biedt, maar de `Uri()` constructor niet, dit een uitzondering genereert en het slachtoffer gaat u naar de URL geldig, maar onjuist gevormd. 