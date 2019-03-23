---
title: Sessie Management - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
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
ms.openlocfilehash: e8f3cf3889b3f79e930630ff0e768a0c4875eec6
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361724"
---
# <a name="security-frame-session-management"></a>Beveiliging-Frame: Sessiebeheer
| Product/Service | Artikel |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[De juiste afmelden implementeren met behulp van ADAL methoden bij het gebruik van Azure AD](#logout-adal)</li></ul> |
| IoT-apparaat | <ul><li>[Beperkte levensduur voor de gegenereerde SaS-tokens gebruiken](#finite-tokens)</li></ul> |
| **Azure Document DB** | <ul><li>[Minimale levensduur van tokens voor gegenereerde brontokens gebruiken](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[Juiste afmelden WsFederation methoden gebruiken bij het gebruik van AD FS implementeren](#wsfederation-logout)</li></ul> |
| **Identiteitsserver** | <ul><li>[Implementeren in de juiste afmelden bij het gebruik van Identiteitsserver](#proper-logout)</li></ul> |
| **Web-App** | <ul><li>[Toepassingen die beschikbaar zijn via HTTPS moeten beveiligde cookies gebruiken](#https-secure-cookies)</li><li>[Alle HTTP-gebaseerde toepassing moet http alleen voor de definitie van de cookie opgeven](#cookie-definition)</li><li>[Bescherming tegen aanvallen van Cross-Site aanvragen kunnen worden vervalst (CSRF) op ASP.NET-webpagina 's](#csrf-asp)</li><li>[Instellen van de sessie voor de levensduur van inactiviteit](#inactivity-lifetime)</li><li>[Juiste afmelden van de toepassing implementeren](#proper-app-logout)</li></ul> |
| **Web-API** | <ul><li>[Bescherming tegen aanvallen van Cross-Site aanvragen kunnen worden vervalst (CSRF) op ASP.NET Web-API 's](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>De juiste afmelden implementeren met behulp van ADAL methoden bij het gebruik van Azure AD

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Als de toepassing, is afhankelijk van access token dat is uitgegeven door Azure AD, moet de gebeurtenis-handler voor afmelden aanroepen |

### <a name="example"></a>Voorbeeld
```csharp
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Voorbeeld
Het moet ook de gebruikerssessie door het aanroepen van Session.Abandon() methode vernietigen. Methode, ziet u veilige uitvoering van de gebruiker afmelden in:
```csharp
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <a id="finite-tokens"></a>Beperkte levensduur voor de gegenereerde SaS-tokens gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | SaS-tokens gegenereerd voor verificatie bij Azure IoT Hub moeten een beperkte verloopperiode hebben. Houd de levensduur van de SaS-tokens tot een minimum te beperken van de hoeveelheid tijd die ze op de opnieuw kunnen worden afgespeeld in het geval de tokens zijn aangetast.|

## <a id="resource-tokens"></a>Minimale levensduur van tokens voor gegenereerde brontokens gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Document DB | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Verlaag de duur van de resourcetoken naar een minimale waarde vereist. Resource-tokens hebben een geldige tijdsduur van de standaardwaarde van 1 uur.|

## <a id="wsfederation-logout"></a>Juiste afmelden WsFederation methoden gebruiken bij het gebruik van AD FS implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | ADFS | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Als de toepassing afhankelijk is uitgegeven door AD FS STS-token, moet de gebeurtenis-handler voor afmelden WSFederationAuthenticationModule.FederatedSignOut() methode naar de gebruiker afmelden aanroepen. Ook de huidige sessie moet worden vernietigd en de waarde voor de sessie-token moet worden opnieuw ingesteld en geneutraliseerd.|

### <a name="example"></a>Voorbeeld
```csharp
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes the user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at the specified security token service (STS) by using the WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of the current session and raises the appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at the specified security token service (STS) by using the WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <a id="proper-logout"></a>Implementeren in de juiste afmelden bij het gebruik van Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Meld u af IdentityServer3 federatieve](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Stappen** | IdentityServer ondersteunt de mogelijkheid om te federeren met externe id-providers. Wanneer een gebruiker zich afmeldt een upstream-id-provider, is het protocol dat wordt gebruikt, al naar gelang het mogelijk een melding ontvangen wanneer de gebruiker zich afmeldt. Hierdoor kan IdentityServer zijn om clients te verwittigen, zodat ze kunnen ook de gebruiker zich af. Raadpleeg de documentatie in de sectie Verwijzingen voor details over de implementatie.|

## <a id="https-secure-cookies"></a>Toepassingen die beschikbaar zijn via HTTPS moeten beveiligde cookies gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - on-premises |
| **Verwijzingen**              | [httpCookies Element (ASP.NET-instellingenschema)](https://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure eigenschap](https://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Stappen** | Cookies zijn normaal gesproken alleen toegankelijk is voor het domein waarvoor ze zijn binnen het bereik. Helaas, de definitie van 'domein' bevat geen het protocol, zodat cookies die zijn gemaakt via HTTPS toegankelijk via HTTP zijn. De "veilig" kenmerk geeft aan dat in de browser dat de cookie moet alleen worden beschikbaar gesteld via HTTPS. Zorg ervoor dat alle cookies ingesteld via HTTPS gebruiken de **beveiligde** kenmerk. De vereiste kan worden afgedwongen in het web.config-bestand door het kenmerk requireSSL instellen op true. Het is de aanpak van voorkeur omdat deze wordt afgedwongen de **beveiligde** kenmerk voor alle huidige en toekomstige cookies zonder de noodzaak om eventuele extra code hoeft te wijzigen.|

### <a name="example"></a>Voorbeeld
```csharp
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
De instelling wordt afgedwongen, zelfs als HTTP wordt gebruikt voor toegang tot de toepassing. Als HTTP wordt gebruikt voor toegang tot de toepassing, wordt de instelling van de toepassing omdat de cookies worden ingesteld met het kenmerk secure en de browser niet ze terug naar de toepassing stuurt.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Web Forms, MVC5 |
| **Kenmerken**              | EnvironmentType - on-premises |
| **Verwijzingen**              | N/A  |
| **Stappen** | Wanneer de web-App de Relying Party is en de id-provider ADFS-server is, beveiligde kenmerk van het FedAuth-token kan worden geconfigureerd door requireSSL instellen op waar in de `system.identityModel.services` sectie van web.config:|

### <a name="example"></a>Voorbeeld
```csharp
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <a id="cookie-definition"></a>Alle HTTP-gebaseerde toepassing moet http alleen voor de definitie van de cookie opgeven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Beveiligde Cookie-kenmerk](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Stappen** | Als u wilt het risico van de openbaarmaking van informatie met een aanval cross-site scripting (XSS), een nieuw kenmerk - httpOnly - geïntroduceerd om cookies te en wordt ondersteund door alle belangrijke browsers. Het kenmerk geeft aan dat een cookie niet toegankelijk zijn via een script is. Met behulp van cookies HttpOnly vermindert een web-App de mogelijkheid dat gevoelige informatie in de cookie kan worden gestolen via scripts en naar een kwaadwillende persoon-website verzonden. |

### <a name="example"></a>Voorbeeld
Alle HTTP-gebaseerde toepassingen die gebruikmaken van cookies moeten in de definitie van de cookie door het implementeren van de volgende configuratie in web.config HttpOnly opgeven:
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [FormsAuthentication.RequireSSL Property](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Stappen** | De waarde van de RequireSSL-eigenschap is ingesteld in het configuratiebestand voor een ASP.NET-toepassing met behulp van het kenmerk requireSSL van het configuratie-element. U kunt opgeven in het bestand Web.config voor uw ASP.NET-toepassing of SSL (Secure Sockets Layer) vereist is voor de cookie voor formulierverificatie terugkeren naar de server door het kenmerk requireSSL.|

### <a name="example"></a>Voorbeeld 
In het volgende voorbeeld wordt het kenmerk requireSSL ingesteld in het bestand Web.config.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5 |
| **Kenmerken**              | EnvironmentType - on-premises |
| **Verwijzingen**              | [Windows Identity Foundation (WIF) Configuration – Part II](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Stappen** | Om in te stellen httpOnly kenmerk voor FedAuth cookies, moet de kenmerkwaarde hideFromCsript worden ingesteld op True. |

### <a name="example"></a>Voorbeeld
Volgende configuratie ziet u de juiste configuratie:
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <a id="csrf-asp"></a>Bescherming tegen aanvallen van Cross-Site aanvragen kunnen worden vervalst (CSRF) op ASP.NET-webpagina 's

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Aanvraag voor cross-site kunnen worden vervalst (CSRF of XSRF) is een aanval waarbij een aanvaller acties in de beveiligingscontext van de vastgestelde sessie een andere gebruiker op een website kunt uitvoeren. Het doel is om te wijzigen of verwijderen van inhoud, als de betreffende website is afhankelijk van uitsluitend sessiecookies om te verifiëren van de aanvraag. Een aanvaller kan misbruik maken van dit beveiligingsprobleem met het ophalen van de browser een URL met een opdracht laden uit een kwetsbaar site waarop de gebruiker is aangemeld in een andere gebruiker. Er zijn veel manieren voor een aanvaller om dat te doen, zoals door die als host fungeert voor een andere website die wordt een resource van de server kwetsbaar wordt geladen, of voor het verkrijgen van de gebruiker op een koppeling te klikken. De aanval kan worden voorkomen als de server wordt een extra token naar de client verzonden, vereist dat de client om op te nemen dat token in alle toekomstige aanvragen en controleert of dat alle toekomstige aanvragen een token die betrekking hebben op de huidige sessie bevatten, zoals met behulp van de ASP.NET AntiForgeryToken of de weergavestatus. |

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [XSRF/CSRF preventie in ASP.NET MVC en webpagina 's](https://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Stappen** | Anti-CSRF- en ASP.NET MVC-formulieren - gebruiken de `AntiForgeryToken` Help-methode op weergaven; put een `Html.AntiForgeryToken()` in het formulier, bijvoorbeeld:|

### <a name="example"></a>Voorbeeld
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Voorbeeld
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Voorbeeld
Op hetzelfde moment biedt Html.AntiForgeryToken() de bezoeker een cookie __RequestVerificationToken, met dezelfde waarde als de willekeurige verborgen waarde hierboven wordt genoemd. Vervolgens voor het valideren van een binnenkomende formulier post de [ValidateAntiForgeryToken] filter toevoegen aan de doel-actiemethode. Bijvoorbeeld:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Filtr autorizace die controleert:
* De binnenkomende aanvraag heeft een cookie __RequestVerificationToken aangeroepen
* De binnenkomende aanvraag heeft een `Request.Form` vermelding __RequestVerificationToken
* Deze cookie en `Request.Form` waarden vergelijken, ervan uitgaande dat alle goed is, de aanvraag wordt verwerkt via die normaal werken. Maar als dit niet het geval is, klikt u vervolgens een Autorisatiefout met het bericht "een vereiste anti-vervalsingstoken is niet opgegeven of is ongeldig '. 

### <a name="example"></a>Voorbeeld
Anti-CSRF- en AJAX: Het formulier-token is een probleem voor AJAX-aanvragen, omdat een AJAX-aanvraag JSON-gegevens geen HTML-formuliergegevens worden verzonden. Eén oplossing is voor het verzenden van de tokens in een aangepaste HTTP-header. De volgende code Razor-syntax gebruikt voor het genereren van de tokens en vervolgens de tokens toegevoegd aan een AJAX-aanvraag. 
```csharp
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Voorbeeld
Wanneer u de aanvraag verwerkt, moet u de tokens ophalen uit de aanvraagheader. Vervolgens roept de methode AntiForgery.Validate voor het valideren van de tokens. De methode valideren, wordt er een uitzondering genereert als de tokens niet geldig zijn.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Profiteer van ASP.NET ingebouwde functies voor Fend uit aanvallen via Internet](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Stappen** | CSRF-aanvallen in webformulier op basis van toepassingen kunnen grotendeels worden opgevangen door ViewStateUserKey te stellen op een willekeurige tekenreeks varieert voor elke gebruiker - gebruikers-ID of beter nog, sessie-ID. Sessie-ID is een veel beter geschikt voor een aantal technische en sociale oorzaken hebben, omdat een sessie-ID onvoorspelbaar zijn, is een time-out optreedt en varieert op basis van per gebruiker.|

### <a name="example"></a>Voorbeeld
Dit is de code die u nodig hebt om in alle pagina's:
```csharp
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Instellen van de sessie voor de levensduur van inactiviteit

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [HttpSessionState.Timeout Property](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Stappen** | Sessietime-out vertegenwoordigt de gebeurtenis die plaatsvindt wanneer een gebruiker niet een actie op een website tijdens een interval uitvoeren is (gedefinieerd door de webserver). De gebeurtenis, aan serverzijde, de status van de gebruikerssessie wijzigen naar 'ongeldig' (bijvoorbeeld ' niet meer gebruikt') en dat de webserver te vernietigen (verwijderen van alle gegevens die erin zijn opgenomen). Het volgende codevoorbeeld wordt de time-out van sessie-kenmerk ingesteld op 15 minuten in het bestand Web.config.|

### <a name="example"></a>Voorbeeld
```XML 
<configuration>
  <system.web>
    <sessionState mode="InProc" cookieless="true" timeout="15" />
  </system.web>
</configuration>
```

## <a id="threat-detection"></a>Detectie van bedreigingen op SQL Azure inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Formulieren-element voor verificatie (ASP.NET-instellingenschema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Stappen** | De time-out van de cookie Formulierverificatieticket ingesteld op 15 minuten|

### <a name="example"></a>Voorbeeld
```XML
<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Web Forms, MVC5 |
| **Kenmerken**              | EnvironmentType - on-premises |
| **Verwijzingen**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Stappen** | Wanneer de web-App Relying Party is en AD FS de STS is, kan de levensduur van de verificatiecookies die - tokens FedAuth - worden ingesteld door de volgende configuratie in web.config:|

### <a name="example"></a>Voorbeeld
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use the code below to enable encryption-decryption of claims received from ADFS. Thumbprint value varies based on the certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a>Voorbeeld
De AD FS SAML uitgegeven claim ook de levensduur van token moet worden ingesteld op 15 minuten, door te voeren van de volgende powershell-opdracht op de ADFS-server:
```csharp
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Juiste afmelden van de toepassing implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Uitvoeren juiste afmelden van de toepassing wanneer de gebruiker op drukt knop Afmelden. Na de afmelding, moet toepassing vernietigen gebruikerssessie, en ook opnieuw instellen en cookie-outwaarde voor sessies, samen met opnieuw in te stellen en de waarde van de cookie verificatie nullifying ongeldig verklaard. Ook wanneer meerdere sessies zijn gekoppeld aan een enkele gebruikers-id, moeten die worden gezamenlijk afgesloten op time-out of afmelden op de server. Ten slotte ervoor te zorgen dat afmelden functionaliteit beschikbaar op elke pagina is. |

## <a id="csrf-api"></a>Bescherming tegen aanvallen van Cross-Site aanvragen kunnen worden vervalst (CSRF) op ASP.NET Web-API 's

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Aanvraag voor cross-site kunnen worden vervalst (CSRF of XSRF) is een aanval waarbij een aanvaller acties in de beveiligingscontext van de vastgestelde sessie een andere gebruiker op een website kunt uitvoeren. Het doel is om te wijzigen of verwijderen van inhoud, als de betreffende website is afhankelijk van uitsluitend sessiecookies om te verifiëren van de aanvraag. Een aanvaller kan misbruik maken van dit beveiligingsprobleem met het ophalen van de browser een URL met een opdracht laden uit een kwetsbaar site waarop de gebruiker is aangemeld in een andere gebruiker. Er zijn veel manieren voor een aanvaller om dat te doen, zoals door die als host fungeert voor een andere website die wordt een resource van de server kwetsbaar wordt geladen, of voor het verkrijgen van de gebruiker op een koppeling te klikken. De aanval kan worden voorkomen als de server wordt een extra token naar de client verzonden, vereist dat de client om op te nemen dat token in alle toekomstige aanvragen en controleert of dat alle toekomstige aanvragen een token die betrekking hebben op de huidige sessie bevatten, zoals met behulp van de ASP.NET AntiForgeryToken of de weergavestatus. |

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Voorkomen van aanvallen van de aanvraag voor Cross-Site kunnen worden vervalst (CSRF) in ASP.NET Web-API](https://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Stappen** | Anti-CSRF- en AJAX: Het formulier-token is een probleem voor AJAX-aanvragen, omdat een AJAX-aanvraag JSON-gegevens geen HTML-formuliergegevens worden verzonden. Eén oplossing is voor het verzenden van de tokens in een aangepaste HTTP-header. De volgende code Razor-syntax gebruikt voor het genereren van de tokens en vervolgens de tokens toegevoegd aan een AJAX-aanvraag. |

### <a name="example"></a>Voorbeeld
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Voorbeeld
Wanneer u de aanvraag verwerkt, moet u de tokens ophalen uit de aanvraagheader. Vervolgens roept de methode AntiForgery.Validate voor het valideren van de tokens. De methode valideren, wordt er een uitzondering genereert als de tokens niet geldig zijn.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a>Voorbeeld
Anti-CSRF- en ASP.NET MVC-formulieren - AntiForgeryToken Help-methode gebruiken voor weergaven. bijvoorbeeld, een Html.AntiForgeryToken() in het formulier plaatsen
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Voorbeeld
Het bovenstaande voorbeeld wordt uitvoer er ongeveer als volgt uit:
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Voorbeeld
Op hetzelfde moment biedt Html.AntiForgeryToken() de bezoeker een cookie __RequestVerificationToken, met dezelfde waarde als de willekeurige verborgen waarde hierboven wordt genoemd. Vervolgens voor het valideren van een binnenkomende formulier post de [ValidateAntiForgeryToken] filter toevoegen aan de doel-actiemethode. Bijvoorbeeld:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Filtr autorizace die controleert:
* De binnenkomende aanvraag heeft een cookie __RequestVerificationToken aangeroepen
* De binnenkomende aanvraag heeft een `Request.Form` vermelding __RequestVerificationToken
* Deze cookie en `Request.Form` waarden vergelijken, ervan uitgaande dat alle goed is, de aanvraag wordt verwerkt via die normaal werken. Maar als dit niet het geval is, klikt u vervolgens een Autorisatiefout met het bericht "een vereiste anti-vervalsingstoken is niet opgegeven of is ongeldig '.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | ID-Provider - ADFS, id-Provider - Azure AD |
| **Verwijzingen**              | [Een Web-API met afzonderlijke Accounts en lokale aanmelding in ASP.NET Web API 2.2 beveiligen](https://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Stappen** | Als de Web-API is beveiligd met behulp van OAuth 2.0, vervolgens wordt verwacht dat een bearer-token in autorisatie-header voor aanvraag en verleent toegang tot de aanvraag alleen als het token geldig is. In tegenstelling tot Cookieverificatie op basis van, doen browsers het bearer-tokens niet koppelen aan aanvragen. De aanvragende client moet expliciet koppelen het bearer-token in de aanvraagheader. Bearer-tokens worden daarom voor ASP.NET Web-API's beveiligd met behulp van OAuth 2.0, beschouwd als een beveiliging tegen CSRF-aanvallen. Houd er rekening mee dat als het MVC-gedeelte van de toepassing gebruikmaakt van formulierverificatie (dat wil zeggen, maakt gebruik van cookies), anti-vervalsingstokens moeten worden gebruikt door de MVC-web-app. |

### <a name="example"></a>Voorbeeld
De Web-API heeft op de hoogte worden gebracht vertrouwen alleen op bearer-tokens en niet op cookies. Het kan worden gedaan door de volgende configuratie in `WebApiConfig.Register` methode:

```csharp
config.SuppressDefaultHostAuthentication();
config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```

De methode SuppressDefaultHostAuthentication vertelt Web-API om de verificatie die plaatsvindt voordat de aanvraag de Web-API-pijplijn, door IIS of OWIN-middleware bereikt negeren. Op die manier kunnen we Web-API om te verifiëren met bearer-tokens beperken.
