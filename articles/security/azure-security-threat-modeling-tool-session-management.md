---
title: Sessie-Management - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
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
ms.openlocfilehash: 56471d8ef68eacacb3ecebad5056d7e7a9f3ca40
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-session-management--articles"></a>Beveiliging Frame: Sessiebeheer | Artikelen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[De juiste afmelding implementeren met behulp van ADAL methoden bij gebruik van Azure AD](#logout-adal)</li></ul> |
| IoT-apparaat | <ul><li>[Het gebruiken van beperkte levensduur voor gegenereerde SaS-tokens](#finite-tokens)</li></ul> |
| **Azure Documentdb** | <ul><li>[Minimale token levensduur voor tokens voor gegenereerde Resource gebruiken](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[Juiste afmelding WsFederation methoden gebruiken wanneer u AD FS implementeren](#wsfederation-logout)</li></ul> |
| **Identiteitsserver** | <ul><li>[Implementeren in de juiste afmelden bij gebruik van Identiteitsserver](#proper-logout)</li></ul> |
| **Webtoepassing** | <ul><li>[Toepassingen die beschikbaar zijn via HTTPS moeten beveiligde cookies gebruiken](#https-secure-cookies)</li><li>[Alle HTTP-gebaseerde toepassing moet http alleen voor de definitie van de cookie opgeven](#cookie-definition)</li><li>[Beperken dat Cross-Site aanvragen kunnen worden vervalst (CSRF) aanvallen op ASP.NET-webpagina 's](#csrf-asp)</li><li>[Sessie voor inactiviteit levensduur instellen](#inactivity-lifetime)</li><li>[Juiste afmelden van de toepassing implementeren](#proper-app-logout)</li></ul> |
| **Web-API** | <ul><li>[Beperken dat Cross-Site aanvragen kunnen worden vervalst (CSRF) aanvallen op ASP.NET Web-API 's](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>De juiste afmelding implementeren met behulp van ADAL methoden bij gebruik van Azure AD

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Als de toepassing op access token dat is uitgegeven door Azure AD vertrouwt, moet de afmelding gebeurtenis-handler aanroepen |

### <a name="example"></a>Voorbeeld
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Voorbeeld
Het moet ook de gebruikerssessie door het aanroepen van methode Session.Abandon() vernietigen. Methode, ziet u beveiligde implementatie van de gebruiker afmelden in:
```C#
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

## <a id="finite-tokens"></a>Het gebruiken van beperkte levensduur voor gegenereerde SaS-tokens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | SaS-tokens gegenereerd voor verificatie bij de Azure IoT Hub moeten een eindige verloopperiode hebben. Houd de levensduur van SaS-token tot een minimum te beperken van de hoeveelheid tijd die kunnen worden cookies voor het geval de tokens worden getroffen.|

## <a id="resource-tokens"></a>Minimale token levensduur voor tokens voor gegenereerde Resource gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Documentdb | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Het interval van resource token beperken tot een voorgeschreven minimumwaarde. Resource-tokens hebben een geldige timespan standaardwaarde van 1 uur.|

## <a id="wsfederation-logout"></a>Juiste afmelding WsFederation methoden gebruiken wanneer u AD FS implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | ADFS | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Als de toepassing afhankelijk is uitgegeven door AD FS STS-token, moet de gebeurtenis-handler voor afmelden WSFederationAuthenticationModule.FederatedSignOut() methode afmelden van de gebruiker aanroepen. Ook de huidige sessie moet worden vernietigd en de waarde voor de sessie-token te herstellen en geneutraliseerd.|

### <a name="example"></a>Voorbeeld
```C#
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

## <a id="proper-logout"></a>Implementeren in de juiste afmelden bij gebruik van Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [IdentityServer3 federatieve afmelden](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Stappen** | IdentityServer ondersteunt de mogelijkheid om te communiceren met externe id-providers. Wanneer een gebruiker zich afmeldt een upstream-id-provider, is afhankelijk van het protocol dat wordt gebruikt, het mogelijk een melding te ontvangen wanneer de gebruiker zich afmeldt. Kunt u IdentityServer ervan om clients te verwittigen zodat ze kunnen ook de gebruiker zich af. Raadpleeg de documentatie in de sectie Verwijzingen voor details over de implementatie.|

## <a id="https-secure-cookies"></a>Toepassingen die beschikbaar zijn via HTTPS moeten beveiligde cookies gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - OnPrem |
| **Verwijzingen**              | [httpCookies Element (ASP.NET-instellingenschema)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure-eigenschap](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Stappen** | Cookies zijn normaal gesproken alleen toegankelijk is voor het domein waarvoor ze zijn binnen het bereik. Helaas bevat de definitie van 'domein' geen het protocol zodat cookies die zijn gemaakt via HTTPS toegankelijk via HTTP zijn. Het kenmerk 'secure' geeft u aan de browser dat de cookie alleen beschikbaar moet worden gesteld via HTTPS. Zorg ervoor dat alle cookies moet worden ingesteld via HTTPS gebruikt de **beveiligde** kenmerk. Aan deze eis kan worden afgedwongen in het web.config-bestand door het kenmerk requireSSL in te stellen op true. De gewenste aanpak omdat deze wordt afgedwongen wordt de **beveiligde** kenmerk voor alle huidige en toekomstige cookies zonder aanvullende codewijzigingen aanbrengen.|

### <a name="example"></a>Voorbeeld
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
De instelling wordt afgedwongen, zelfs als HTTP is gebruikt voor toegang tot de toepassing. HTTP wordt gebruikt om toegang tot de toepassing, verbroken de instelling als de toepassing omdat de cookies worden ingesteld met het kenmerk secure en de browser ze niet terug naar de toepassing stuurt.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Webformulieren, MVC5 |
| **Kenmerken**              | EnvironmentType - OnPrem |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Wanneer de webtoepassing de Relying Party is en de IdP ADFS-server, beveiligde kenmerk van het FedAuth-token kan worden geconfigureerd door requireSSL in te stellen op True in `system.identityModel.services` sectie van web.config:|

### <a name="example"></a>Voorbeeld
```C#
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
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Beveiligde Cookie-kenmerk](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Stappen** | Zodat het risico van de openbaarmaking van informatie met een aanval cross-site scripting (XSS) van een nieuw kenmerk - httpOnly - om cookies te werd geïntroduceerd en wordt ondersteund door alle belangrijke browsers. Het kenmerk geeft u een cookie is niet toegankelijk via script. Met behulp van cookies HttpOnly verkleint een webtoepassing de kans dat gevoelige informatie in de cookie kan worden gestolen via scripts en naar de website van een aanvaller verzonden. |

### <a name="example"></a>Voorbeeld
Alle HTTP-gebaseerde toepassingen die gebruikmaken van cookies moeten in de definitie van het cookie door het implementeren van na de configuratie in web.config HttpOnly opgeven:
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
| **Van toepassing technologieën** | Web Forms |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [De eigenschap FormsAuthentication.RequireSSL](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Stappen** | De waarde van de eigenschap RequireSSL is ingesteld in het configuratiebestand voor een ASP.NET-toepassing met behulp van het kenmerk requireSSL van het configuratie-element. U kunt opgeven in het bestand Web.config voor uw ASP.NET-toepassing of SSL (Secure Sockets Layer) vereist is voor de cookie voor formulierverificatie terugkeren naar de server door het kenmerk requireSSL.|

### <a name="example"></a>Voorbeeld 
Het volgende codevoorbeeld stelt het kenmerk requireSSL in het bestand Web.config.
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
| **Kenmerken**              | EnvironmentType - OnPrem |
| **Verwijzingen**              | [Configuratie van Windows Identity Foundation (WIF) – deel II](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Stappen** | Om in te stellen kenmerk httpOnly voor FedAuth cookies, moet de kenmerkwaarde hideFromCsript worden ingesteld op True. |

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

## <a id="csrf-asp"></a>Beperken dat Cross-Site aanvragen kunnen worden vervalst (CSRF) aanvallen op ASP.NET-webpagina 's

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Aanvraagvervalsing op meerdere sites (CSRF of XSRF) is een type van een aanval waarbij een aanvaller acties in de beveiligingscontext van de vastgestelde sessie een andere gebruiker op een website kunt uitvoeren. Het doel is om te wijzigen of verwijderen van inhoud, als voor de betreffende website uitsluitend sessiecookies ontvangen request verifiëren. Een aanvaller kan deze kwetsbaarheid misbruiken met het ophalen van de browser een URL met een opdracht laden van een kwetsbaar site waarop de gebruiker is aangemeld in een andere gebruiker. Er zijn veel manieren voor aanvallers om u te doen, zoals door die als host fungeert voor een andere website die wordt een resource van de server kwetsbaar wordt geladen, of voor het verkrijgen van de gebruiker op een koppeling te klikken. De aanval kan worden voorkomen als de server een extra token naar de client verzendt vereist dat de client die token opnemen in alle toekomstige aanvragen en verifieert dat alle toekomstige aanvragen een token dat betrekking op de huidige sessie bevatten, heeft zoals met behulp van de ASP.NET AntiForgeryToken of ViewState. |

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [XSRF/CSRF voorkomen in ASP.NET MVC en webpagina 's](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Stappen** | Anti-CSRF en formulieren ASP.NET MVC - gebruiken de `AntiForgeryToken` Help-methode op weergaven; put een `Html.AntiForgeryToken()` in het formulier, bijvoorbeeld:|

### <a name="example"></a>Voorbeeld
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Voorbeeld
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Voorbeeld
Op hetzelfde moment biedt Html.AntiForgeryToken() de bezoeker een cookie met de naam __RequestVerificationToken met dezelfde waarde als de willekeurige verborgen waarde hierboven weergegeven. Vervolgens voegt u het filter [ValidateAntiForgeryToken] aan doelmethode actie voor het valideren van een binnenkomende Formulierbericht. Bijvoorbeeld:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Autorisatiefilter die of controleert:
* De binnenkomende aanvraag heeft een cookie met de naam __RequestVerificationToken
* De binnenkomende aanvraag heeft een `Request.Form` vermelding __RequestVerificationToken aangeroepen
* Deze cookie en `Request.Form` waarden overeenkomen, ervan uitgaande dat alle goed is, de aanvraag wordt verwerkt via die normaal werken. Maar als dit niet het geval is, klikt u vervolgens een Autorisatiefout met het bericht 'een vereiste anti-vervalsingstoken is niet opgegeven of is ongeldig'. 

### <a name="example"></a>Voorbeeld
Anti-CSRF en AJAX: het token van het formulier een probleem voor AJAX-aanvragen kan worden, omdat een AJAX-aanvraag JSON-gegevens, geen HTML-formuliergegevens kan verzenden. Eén oplossing is voor het verzenden van de tokens in een aangepaste HTTP-header. De volgende code Razor syntaxis wordt gebruikt voor het genereren van de tokens en vervolgens de tokens toegevoegd aan een AJAX-aanvraag. 
```C#
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
Wanneer u de aanvraag verwerkt, moet u de tokens extraheren uit de aanvraagheader. Vervolgens roept de methode AntiForgery.Validate voor het valideren van de tokens. De methode Validate genereert een uitzondering als de tokens niet geldig zijn.
```C#
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
| **Van toepassing technologieën** | Web Forms |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Profiteren van ASP.NET ingebouwde functies voor Fend uit aanvallen via Internet](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Stappen** | CSRF aanvallen in het webformulier op basis van toepassingen kunnen worden verholpen door ViewStateUserKey in te stellen op een willekeurige tekenreeks varieert voor elke gebruiker - gebruikers-ID of beter nog, de sessie-ID. Sessie-ID is veel beter geschikt zijn voor een aantal technische en sociale oorzaken hebben, omdat een sessie-ID onvoorspelbare, is een time-out optreedt en varieert op basis van een gebruiker.|

### <a name="example"></a>Voorbeeld
Dit is de code hoeft te hebben in alle pagina's:
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Sessie voor inactiviteit levensduur instellen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [De eigenschap HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Stappen** | Sessietime-out vertegenwoordigt de gebeurtenis die optreedt wanneer een gebruiker niet voert geen actie op een website tijdens een interval van (gedefinieerd door de webserver). De gebeurtenis aan serverzijde, de status van de gebruikerssessie wijzigen naar 'ongeldig' (bijvoorbeeld ' niet meer gebruikt') en dat de webserver vernietiging (verwijderen van alle gegevens die zijn opgenomen in de App). Het volgende codevoorbeeld wordt de time-out van sessie-kenmerk ingesteld op 15 minuten in het bestand Web.config.|

### <a name="example"></a>Voorbeeld
'''XML-code <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Web Forms |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Element vormt voor verificatie (ASP.NET-instellingenschema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Stappen** | De time-out van de cookie Formulierverificatieticket instelt op 15 minuten|

### <a name="example"></a>Voorbeeld
'''XML-code<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When the web application is Relying Party and ADFS is the STS, the lifetime of the authentication cookies - FedAuth tokens - can be set by the following configuration in web.config:|

### Example
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
De AD FS SAML uitgegeven claim ook de levensduur van token moet worden ingesteld op 15 minuten, door de volgende powershell-opdracht wordt uitgevoerd op de ADFS-server:
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Juiste afmelden van de toepassing implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Uitvoeren juiste afmelden van de toepassing als de gebruiker op drukt knop Afmelden. Bij het afmelden, moet toepassing vernietigen gebruikerssessie, en ook opnieuw instellen en cookie-outwaarde voor sessies, samen met opnieuw instellen en de waarde van de cookie verificatie nullifying ongeldig verklaard. Ook wanneer meerdere sessies zijn gekoppeld aan een enkele gebruikers-id, moeten ze worden gezamenlijk afgesloten aan de serverzijde op time-out of meld u af. Ten slotte zorgen ervoor dat afmelding functionaliteit beschikbaar op elke pagina. |

## <a id="csrf-api"></a>Beperken dat Cross-Site aanvragen kunnen worden vervalst (CSRF) aanvallen op ASP.NET Web-API 's

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Aanvraagvervalsing op meerdere sites (CSRF of XSRF) is een type van een aanval waarbij een aanvaller acties in de beveiligingscontext van de vastgestelde sessie een andere gebruiker op een website kunt uitvoeren. Het doel is om te wijzigen of verwijderen van inhoud, als voor de betreffende website uitsluitend sessiecookies ontvangen request verifiëren. Een aanvaller kan deze kwetsbaarheid misbruiken met het ophalen van de browser een URL met een opdracht laden van een kwetsbaar site waarop de gebruiker is aangemeld in een andere gebruiker. Er zijn veel manieren voor aanvallers om u te doen, zoals door die als host fungeert voor een andere website die wordt een resource van de server kwetsbaar wordt geladen, of voor het verkrijgen van de gebruiker op een koppeling te klikken. De aanval kan worden voorkomen als de server een extra token naar de client verzendt vereist dat de client die token opnemen in alle toekomstige aanvragen en verifieert dat alle toekomstige aanvragen een token dat betrekking op de huidige sessie bevatten, heeft zoals met behulp van de ASP.NET AntiForgeryToken of ViewState. |

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Het voorkomen van Aanvraagvervalsing op meerdere sites (CSRF) aanvallen in ASP.NET Web-API](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Stappen** | Anti-CSRF en AJAX: het token van het formulier een probleem voor AJAX-aanvragen kan worden, omdat een AJAX-aanvraag JSON-gegevens, geen HTML-formuliergegevens kan verzenden. Eén oplossing is voor het verzenden van de tokens in een aangepaste HTTP-header. De volgende code Razor syntaxis wordt gebruikt voor het genereren van de tokens en vervolgens de tokens toegevoegd aan een AJAX-aanvraag. |

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
Wanneer u de aanvraag verwerkt, moet u de tokens extraheren uit de aanvraagheader. Vervolgens roept de methode AntiForgery.Validate voor het valideren van de tokens. De methode Validate genereert een uitzondering als de tokens niet geldig zijn.
```C#
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
Anti-CSRF en formulieren ASP.NET MVC - gebruikmaken van de Help-methode AntiForgeryToken op weergaven; bijvoorbeeld, een Html.AntiForgeryToken() in het formulier plaatsen
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Voorbeeld
Het bovenstaande voorbeeld wordt ongeveer de volgende uitvoer:
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Voorbeeld
Op hetzelfde moment biedt Html.AntiForgeryToken() de bezoeker een cookie met de naam __RequestVerificationToken met dezelfde waarde als de willekeurige verborgen waarde hierboven weergegeven. Vervolgens voegt u het filter [ValidateAntiForgeryToken] aan doelmethode actie voor het valideren van een binnenkomende Formulierbericht. Bijvoorbeeld:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Autorisatiefilter die of controleert:
* De binnenkomende aanvraag heeft een cookie met de naam __RequestVerificationToken
* De binnenkomende aanvraag heeft een `Request.Form` vermelding __RequestVerificationToken aangeroepen
* Deze cookie en `Request.Form` waarden overeenkomen, ervan uitgaande dat alle goed is, de aanvraag wordt verwerkt via die normaal werken. Maar als dit niet het geval is, klikt u vervolgens een Autorisatiefout met het bericht 'een vereiste anti-vervalsingstoken is niet opgegeven of is ongeldig'.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | ID-Provider - ADFS, identiteitsprovider - Azure AD |
| **Verwijzingen**              | [Een Web-API met afzonderlijke Accounts en lokale aanmelding in ASP.NET Web-API 2.2 beveiligen](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Stappen** | Als de Web-API is beveiligd met OAuth 2.0, vervolgens een bearer-token in autorisatie-header voor aanvraag verwacht en verleent toegang tot de aanvraag alleen als het token geldig is. In tegenstelling tot op basis van Cookieverificatie koppel browsers geen bearer-tokens op aanvragen. De aanvragende client moet expliciet de bearer-token in de aanvraagheader te koppelen. Daarom voor ASP.NET Web API's beveiligd met OAuth 2.0-bearer-tokens worden beschouwd als bij de bescherming tegen aanvallen CSRF. Houd er rekening mee dat als het MVC-gedeelte van de toepassing gebruikmaakt van formulierverificatie (dat wil zeggen, maakt gebruik van cookies), ter voorkoming tokens moeten worden gebruikt door de MVC-web-app. |

### <a name="example"></a>Voorbeeld
De Web-API heeft om te worden gesteld te vertrouwen alleen op bearer-tokens en niet voor cookies. Het kan worden gedaan door de volgende configuratie in `WebApiConfig.Register` methode: '''C-Sharp code config. SuppressDefaultHostAuthentication(); configuratie. Filters.Add (nieuwe HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```
The SuppressDefaultHostAuthentication method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API to authenticate only using bearer tokens.