---
title: Uitzondering Management - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die worden weergegeven in het hulpprogramma Threat Modeling
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 9a8e0154faccca356c7fb8ce93e43ce67cc0aae2
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="security-frame-exception-management--mitigations"></a>Beveiliging Frame: Uitzonderingsbeheer | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF - Do serviceDebug knooppunt niet opgenomen in het configuratiebestand](#servicedebug)</li><li>[WCF - Do serviceMetadata knooppunt niet opgenomen in het configuratiebestand](#servicemetadata)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de juiste uitzonderingsverwerking in ASP.NET Web API is uitgevoerd](#exception)</li></ul> |
| **Webtoepassing** | <ul><li>[Informatie over de beveiliging in foutberichten niet zichtbaar](#messages)</li><li>[Standaard-fout tijdens het verwerken van pagina implementeren](#default)</li><li>[Methode-implementatie instellen naar een handelsversie in IIS](#deployment)</li><li>[Uitzonderingen zou moeten veilig mislukken](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF - Do serviceDebug knooppunt niet opgenomen in het configuratiebestand

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | Framework WCF (Windows Communication) services kunnen worden geconfigureerd om informatie over foutopsporing weer te geven. Fouten opsporen informatie mag niet worden gebruikt in een productieomgeving. De `<serviceDebug>` tag bepaalt of de functie van de informatie foutopsporing is ingeschakeld voor een WCF-service. Als het kenmerk includeExceptionDetailInFaults is ingesteld op true, informatie over de uitzondering van de toepassing wordt geretourneerd naar clients. Aanvallers kunnen gebruikmaken van de aanvullende informatie ze krijgen van foutopsporing uitvoer om te koppelen van aanvallen die zijn gericht op het framework, database of andere bronnen worden gebruikt door de toepassing. |

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
Informatie voor foutopsporing in de service uitschakelen. Kan dit worden bereikt door het verwijderen van de `<serviceDebug>` tag uit het configuratiebestand van uw toepassing. 

## <a id="servicemetadata"></a>WCF - Do serviceMetadata knooppunt niet opgenomen in het configuratiebestand

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Algemeen, NET Framework 3 |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | Informatie over een service openbaar vrijgeeft, kan aanvallers waardevolle inzicht in hoe ze de service kunnen misbruiken bieden. De `<serviceMetadata>` code kan de functie voor het publiceren van metagegevens. Metagegevens van de service kan gevoelige gegevens bevatten die niet moet openbaar toegankelijk zijn. Ten minste staan alleen vertrouwde gebruikers toegang tot de metagegevens en ervoor te zorgen dat onnodige informatie is niet beschikbaar gemaakt. Nog beter de mogelijkheid voor het publiceren van metagegevens volledig uitschakelen. Een veilige WCF-configuratie bevat niet de `<serviceMetadata>` label. |

## <a id="exception"></a>Zorg ervoor dat de juiste uitzonderingsverwerking in ASP.NET Web API is uitgevoerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC 5, MVC 6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Afhandeling van uitzonderingen in ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [Model validatie in ASP.NET-Web-API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Stappen** | Standaard worden meest niet-onderschepte uitzonderingen in ASP.NET Web API vertaald in een HTTP-antwoord met de statuscode`500, Internal Server Error`|

### <a name="example"></a>Voorbeeld
Om te bepalen van de statuscode geretourneerd door de API `HttpResponseException` kan worden gebruikt, zoals hieronder wordt weergegeven: 
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
Voor verdere besturingselement in het antwoord uitzondering, de `HttpResponseMessage` klasse kan worden gebruikt, zoals hieronder wordt weergegeven: 
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
Om af te vangen onverwerkte uitzonderingen die niet van het type `HttpResponseException`, uitzondering Filters kunnen worden gebruikt. Uitzondering filters implementeren de `System.Web.Http.Filters.IExceptionFilter` interface. De eenvoudigste manier om te schrijven van een uitzonderingsfilter is worden afgeleid van de `System.Web.Http.Filters.ExceptionFilterAttribute` klasse en de methode OnException overschrijven. 

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

Er zijn verschillende manieren registreren van een uitzonderingsfilter Web-API:
- Door de actie
- Door de netwerkcontroller
- Globaal

### <a name="example"></a>Voorbeeld
Als u wilt het filter toepassen op een specifieke actie, moet u het filter toevoegen als een kenmerk met de actie: 
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
U kunt het filter globaal toepassen aan alle Web-API-controllers toe een exemplaar van het filter moet de `GlobalConfiguration.Configuration.Filters` verzameling. Uitzondering filters in deze verzameling van toepassing op elke actie Web API-controller. 
```csharp
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Voorbeeld
Voor de validatie, kan de modelstatus worden doorgegeven aan CreateErrorResponse methode, zoals hieronder wordt weergegeven: 
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

Controleer de koppelingen in de sectie Verwijzingen voor meer informatie over het afhandelen van uitzonderlijke en modelvalidatie van het in ASP.Net Web API 

## <a id="messages"></a>Informatie over de beveiliging in foutberichten niet zichtbaar

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Algemene foutberichten worden verstrekt rechtstreeks naar de gebruiker zonder met inbegrip van gegevens voor gevoelige toepassingen. Voorbeelden van gevoelige gegevens omvatten:</p><ul><li>Namen van server</li><li>Verbindingsreeksen</li><li>Gebruikersnamen</li><li>Wachtwoorden</li><li>SQL-procedures</li><li>Details van dynamische SQL-fouten</li><li>Stack-trace en de regels van code</li><li>Variabelen die zijn opgeslagen in het geheugen</li><li>Station en de map locaties</li><li>Toepassing installeren punten</li><li>Host-configuratie-instellingen</li><li>Andere details interne toepassing</li></ul><p>Alle fouten in een toepassing onderscheppen en algemene foutberichten bieden, evenals aangepaste fouten in IIS inschakelen wordt voorkomen dat openbaarmaking van informatie. SQL Server-database en .NET-uitzonderingen afhandelt, onder andere foutafhandeling architecturen, zijn vooral uitgebreide en zeer nuttig is voor kwaadwillende gebruikers profileren van uw toepassing. Niet rechtstreeks de inhoud van een klasse die is afgeleid van de uitzondering voor .NET-klasse weer te geven en zorg ervoor dat de juiste uitzonderingsverwerking hebben zodat een onverwachte uitzondering per ongeluk rechtstreeks naar de gebruiker is niet gegenereerd.</p><ul><li>Geef algemene foutberichten rechtstreeks naar de gebruiker die abstracte opslag specifieke details rechtstreeks in het bericht uitzonderingsfout/gevonden</li><li>De inhoud van een .NET-uitzonderingsklasse niet rechtstreeks naar de gebruiker weergeven</li><li>Alle foutberichten afvangen en eventueel informeren over de gebruiker via een algemeen foutbericht verzonden naar de toepassingsclient</li><li>De inhoud van de uitzonderingsklasse niet rechtstreeks aan de gebruiker, met name de retourwaarde van blootstellen `.ToString()`, of de waarden van eigenschappen van het bericht of StackTrace. Veilig Meld deze informatie en een meer onschuldig bericht aan de gebruiker weergeven</li></ul>|

## <a id="default"></a>Standaard-fout tijdens het verwerken van pagina implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Dialoogvenster voor instellingen van ASP.NET-foutpagina's bewerken](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Stappen** | <p>Wanneer een ASP.NET-toepassing mislukt en zorgt ervoor dat een HTTP/1.x 500 Interne serverfout opgetreden of de configuratie van een onderdeel (zoals Aanvraagfiltering) voorkomt dat een pagina worden weergegeven, kunt u een foutbericht wordt gegenereerd. Beheerders kunnen kiezen of er een bericht van de toepassing moet worden weergegeven voor de client, gedetailleerd foutbericht naar de client of gedetailleerd foutbericht alleen localhost. De <customErrors> -label in het bestand web.config heeft drie beschikbare modi:</p><ul><li>**Op:** geeft aan dat de aangepaste fouten zijn ingeschakeld. Als geen kenmerk defaultRedirect is opgegeven, zien gebruikers een algemene fout. De aangepaste fouten worden weergegeven op de externe clients en op de lokale host</li><li>**Uit:** geeft aan dat de aangepaste fouten zijn uitgeschakeld. De gedetailleerde ASP.NET-fouten worden weergegeven op de externe clients en op de lokale host</li><li>**RemoteOnly:** geeft aan dat aangepaste fouten worden alleen weergegeven op de externe clients en dat de ASP.NET-fouten worden weergegeven op de lokale host. Dit is de standaardwaarde</li></ul><p>Open de `web.config` -bestand voor de toepassing/site en zorg ervoor dat het label een heeft `<customErrors mode="RemoteOnly" />` of `<customErrors mode="On" />` gedefinieerd.</p>|

## <a id="deployment"></a>Methode-implementatie instellen naar een handelsversie in IIS

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [implementatie-Element (ASP.NET-instellingenschema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Stappen** | <p>De `<deployment retail>` switch is bedoeld voor gebruik door IIS productieservers. Deze switch wordt gebruikt om toepassingen worden uitgevoerd met de best mogelijke prestaties en het lekken van zo weinig mogelijk beveiliging informatie door het uitschakelen van de toepassing de mogelijkheid voor het genereren van traceringsuitvoer op een pagina voor het uitschakelen van de mogelijkheid om gedetailleerde foutberichten weer te geven eindgebruikers en de switch foutopsporing uit te schakelen.</p><p>Opties die ontwikkelaars zijn gericht, zijn zoals is mislukt, switches en vaak aanvragen tracering en foutopsporing zijn ingeschakeld tijdens het ontwikkelen van active. Het verdient aanbeveling dat de methode-implementatie op een productieserver naar een handelsversie wordt ingesteld. Open het bestand machine.config en zorg ervoor dat `<deployment retail="true" />` blijft is ingesteld op true.</p>|

## <a id="fail"></a>Uitzonderingen zou moeten veilig mislukken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Veilig mislukt](https://www.owasp.org/index.php/Fail_securely) |
| **Stappen** | Toepassing zou moeten veilig mislukken. Elke methode waarbij een Booleaanse waarde retourneert, op basis van welke bepaalde besluit wordt gemaakt, hebt uitzonderingsblok zorgvuldig gemaakt. Er zijn veel logische fouten als gevolg van die kneep beveiligingsproblemen in wanneer de uitzonderingsblok ondoordacht is geschreven.|

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
De bovenstaande methode retourneert True, wordt altijd als een uitzondering gebeurt. Als de eindgebruiker een verkeerd ingedeelde URL die de browser respecteert biedt, maar de `Uri()` constructor niet en het slachtoffer gaat u naar de URL geldig, maar een verkeerd ingedeelde dit genereert een uitzondering. 