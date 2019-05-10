---
title: Verificatiestromen (Microsoft Authentication Library) | Azure
description: Meer informatie over de verificatie stromen/verleent die wordt gebruikt door de Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39f323c2ac86e8d42319b3d99221f6c20beff3e4
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406660"
---
# <a name="authentication-flows"></a>Verificatiestromen

Dit artikel beschrijft de verschillende verificatie-stromen geleverd door Microsoft Authentication Library (MSAL).  Deze stromen kunnen worden gebruikt in tal van verschillende toepassingsscenario's.

| Stroom | Description | Gebruikt in|  
| ---- | ----------- | ------- | 
| [Interactief](#interactive) | Hiermee wordt het token opgehaald via een interactieve proces dat de gebruiker wordt gevraagd om referenties via een browser of een pop-upvenster. | [Bureaubladapps](scenario-desktop-overview.md), [mobiele apps](scenario-mobile-overview.md) |
| [Impliciete goedkeuring voor](#implicit-grant) | Hiermee kunnen tokens ophalen zonder dat er een back-end server referentie exchange wordt uitgevoerd. Hiermee wordt de app te melden bij de gebruiker, sessie en het verkrijgen van tokens, voor andere web-API's in de client JavaScript-code.| [Toepassingen met één pagina (SPA)](scenario-spa-overview.md) |
| [Autorisatiecode](#authorization-code) | In apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's gebruikt. Hiermee kunt u zich aanmelden en API-toegang tot uw mobiele en bureaublad-apps toe te voegen. | [Bureaubladapps](scenario-desktop-overview.md), [mobiele apps](scenario-mobile-overview.md), [Web-Apps](scenario-web-app-call-api-overview.md) | 
| [On-behalf-of](#on-behalf-of) | Een toepassing wordt aangeroepen voor een service of web-API, die op zijn beurt moet een andere service/web-API aanroepen. Het idee is dat de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen doorgegeven. | [Web-API's](scenario-web-api-call-api-overview.md) |
| [Clientreferenties](#client-credentials) | Hebt u toegang tot web gehoste bronnen met behulp van de identiteit van een toepassing. Doorgaans gebruikt voor interactie met server-naar-server die moeten worden uitgevoerd op de achtergrond, zonder directe interactie met een gebruiker. | [daemon-apps](scenario-daemon-overview.md) |
| [Apparaatcode](#device-code) | Hiermee kunnen gebruikers zich aanmelden bij de invoer beperkte apparaten zoals een smart-tv's, IoT-apparaat of een printer. | [Desktop/Mobile apps](scenario-desktop-acquire-token.md#command-line-tool-without-web-browser) |
| [Geïntegreerde Windows-verificatie](scenario-desktop-acquire-token.md#integrated-windows-authentication) | U kunt toepassingen op het domein of Azure AD lid zijn van computers om een token op de achtergrond (zonder uw tussenkomst van de gebruikersinterface van de gebruiker) te verkrijgen.| [Desktop/Mobile apps](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Gebruikersnaam en wachtwoord](scenario-desktop-acquire-token.md#username--password) | Kan een toepassing aan te melden bij de gebruikers hun wachtwoord direct kan verwerken. Deze stroom wordt niet aanbevolen. | [Desktop/mobile apps](scenario-desktop-acquire-token.md#username--password) | 

## <a name="interactive"></a>Interactief
MSAL ondersteunt de mogelijkheid om interactief de gebruiker vragen om hun referenties aanmelden en een token met behulp van deze referenties verkrijgen.

![Interactieve stroom](media/msal-authentication-flows/interactive.png)

Lees het volgende voor meer informatie over het gebruik van MSAL.NET aan te schaffen interactief tokens op specifieke platforms:
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [Universeel Windows-platform](msal-net-uwp-considerations.md)

Lees voor meer informatie over interactieve aanroepen in MSAL.js [gedrag in de interactieve aanvragen MSAL.js vragen](msal-js-prompt-behavior.md)

## <a name="implicit-grant"></a>Impliciete toewijzing

MSAL ondersteunt de [OAuth 2 impliciete stroom verlenen](v2-oauth2-implicit-grant-flow.md), waarmee de app ophalen van tokens uit de Microsoft identity-platform zonder uit te voeren van een back-endserver uitwisseling van verificatiegegevens. Hiermee wordt de app te melden bij de gebruiker, sessie en het verkrijgen van tokens, voor andere web-API's in de client JavaScript-code.

![Impliciete stroom](media/msal-authentication-flows/implicit-grant.svg)

Veel moderne web-apps worden gebouwd als één pagina client-side '-toepassingen die zijn geschreven met behulp van JavaScript- of een beveiligd-WACHTWOORDVERIFICATIE framework zoals Angular, Vue.js en React.js. Deze toepassingen worden uitgevoerd in een webbrowser en hebben verschillende kenmerken dan traditionele serverzijde webtoepassingen. Het Microsoft identity-platform kan één pagina toepassingen gebruikers aanmelden en tokens voor toegang tot back-endservices of web-API's met behulp van de impliciete flow verkrijgen. De impliciete stroom kan de toepassing om op te halen van ID-tokens te vertegenwoordigen de geverifieerde gebruiker ook toegangstokens die nodig zijn voor de beveiligde API's aanroepen.

Deze authenticatiestroom bevat geen toepassingsscenario's met behulp van platformoverschrijdende JavaScript-frameworks zoals Electron en React-Native, omdat hiervoor meer mogelijkheden voor interactie met de systeemeigen platforms.

## <a name="authorization-code"></a>Autorisatiecode
MSAL ondersteunt de [OAuth 2-autorisatiecode verlenen](v2-oauth2-auth-code-flow.md), die kan worden gebruikt in apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's. Hiermee kunt u zich aanmelden en API-toegang tot uw mobiele en bureaublad-apps toe te voegen. 

Wanneer gebruikers zich aanmelden bij de web-apps (web sites), ontvangt de web-App een autorisatiecode.  De autorisatiecode wordt gebruikt om een token voor het aanroepen van web-API's te verkrijgen. In ASP.NET / ASP.NET core web-apps, het enige doel van `AcquireTokenByAuthorizationCode` is het toevoegen van een token aan de tokencache zodat deze kan vervolgens worden gebruikt door de toepassing (doorgaans in de controllers) die alleen een token verkrijgen voor het gebruik van een API `AcquireTokenSilent`.

![-Autorisatiecodestroom](media/msal-authentication-flows/authorization-code.png)

1. Een autorisatiecode die wordt ingewisseld voor een toegangstoken aanvragen.
2. Gebruikt het toegangstoken voor het aanroepen van een web-API.

### <a name="considerations"></a>Overwegingen
- De autorisatiecode kan slechts één keer worden gebruikt om te wisselen van een token. Probeer niet om een token meerdere keren met de dezelfde autorisatiecode (als deze expliciet door de standard-protocolspecificatie verboden) te verkrijgen. Als je de code meerdere keren opzet, of omdat u niet op de hoogte dat een framework ook het allemaal voor u doet zijn, krijgt u een fout opgetreden: `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- Als u een ASP.NET/ASP.NET Core-toepassing schrijft, wordt dit kan gebeuren dat als u het framework ASP.NET/Core niet die zien u hebt de autorisatiecode al ingewisseld. Hiervoor moet u contact opnemen `context.HandleCodeRedemption()` -methode van de `AuthorizationCodeReceived` gebeurtenis-handler.

- Vermijd het delen van het toegangstoken met ASP.NET, dat mogelijk te voorkomen dat incrementele toestemming correct gebeurt.  Zie voor meer informatie, [uitgeven #693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693).

## <a name="on-behalf-of"></a>On-behalf-of

MSAL ondersteunt de [OAuth 2 op-andere gebruikers-of-verificatiestroom](v2-oauth2-on-behalf-of-flow.md).  Deze stroom wordt gebruikt wanneer een toepassing wordt aangeroepen voor een service of web-API, die op zijn beurt moet een andere service/web-API aanroepen. Het idee is dat de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen doorgegeven. Voor de middelste laag service voor geverifieerde aanvragen versturen naar de downstream-service, moet voor het beveiligen van een toegangstoken van de Microsoft identity-platform, namens de gebruiker.

![Namens-stroom](media/msal-authentication-flows/on-behalf-of.png)

1. Een toegangstoken opgehaald voor de Web-API
2. Een client (Web, desktop, mobiel, Single-page toepassing) Hiermee wordt een beveiligde Web-API, het toegangstoken toe te voegen als een bearer-token in de header voor verificatie van de HTTP-aanvraag. De Web-API verifieert de gebruiker.
3. Als de client de Web-API aanroept, vraagt de Web-API een ander token op-andere gebruikers-of de gebruiker.  
4. De beveiligde Web-API maakt gebruik van dit token aan te roepen een downstream Web-API op-andere gebruikers-of de gebruiker.  De Web-API kan tokens ook later aanvragen voor andere downstream-API's (maar nog steeds namens de gebruiker).

## <a name="client-credentials"></a>Clientreferenties

MSAL ondersteunt de [clientreferenties die OAuth 2 flow](v2-oauth2-client-creds-grant-flow.md). Deze stroom kunt u toegang tot web gehoste bronnen met behulp van de identiteit van een toepassing. Dit type verlenen wordt vaak gebruikt voor interactie met server-naar-server die moeten worden uitgevoerd op de achtergrond, zonder directe interactie met een gebruiker. Dergelijke toepassingen worden vaak aangeduid als daemons of service-accounts. 

De referenties van de client verlenen flow kunnen inhoud een webservice (vertrouwelijke client) voor het gebruik van zijn eigen referenties, in plaats van een gebruiker imiteren om te verifiëren bij het aanroepen van een andere webservice. In dit scenario wordt de client is doorgaans een middelste laag webservice, een daemon-service of een website. Voor een hoger niveau van zekerheid kan de Microsoft identity-platform ook de aanroepende service een certificaat (in plaats van een gedeelde geheim genoemd) gebruikt als referentie.

> [!NOTE]
> De vertrouwelijke clientstroom is niet beschikbaar op de mobiele platforms (UWP, Xamarin.iOS en Xamarin.Android), aangezien deze alleen openbare clienttoepassingen ondersteunen.  Openbare clienttoepassingen weet niet hoe om te bewijzen de identiteit van de toepassing met de id-Provider. Een beveiligde verbinding kan worden bereikt op web-app of web-API-back-ends door het implementeren van een certificaat.

MSAL.NET ondersteunt twee typen clientreferenties. Deze clientreferenties moeten worden geregistreerd bij Azure AD. De referenties worden doorgegeven aan de constructors van de vertrouwelijke client-toepassing in uw code.

### <a name="application-secrets"></a>Toepassingsgeheimen 

![Vertrouwelijke client met een wachtwoord](media/msal-authentication-flows/confidential-client-password.png)

1. Een token met behulp van het geheim/wachtwoordreferenties toepassing krijgt.
2. Het token wordt gebruikt om aanvragen van de resource te maken.

### <a name="certificates"></a>Certificaten 

![Vertrouwelijke client met certificaat](media/msal-authentication-flows/confidential-client-certificate.png)

1. Een token met de certificaatreferenties verkrijgt.
2. Het token wordt gebruikt om aanvragen van de resource te maken.

Deze clientreferenties moeten worden:
- Geregistreerd bij Azure AD.
- Doorgegeven bij het samenstellen van de vertrouwelijke client-toepassing in uw code.


## <a name="device-code"></a>Apparaatcode
MSAL ondersteunt de [OAuth 2 apparaat codestroom](v2-oauth2-device-code.md), waarmee gebruikers zich aanmelden bij de invoer beperkte apparaten zoals een smart-tv's, IoT-apparaat of een printer. Interactieve verificatie met Azure AD moet een webbrowser. De stroom van het apparaat kan de gebruiker een ander apparaat (bijvoorbeeld een andere computer of een mobiele telefoon) aan te melden bij het interactief waar het apparaat of het besturingssysteem geen een webbrowser biedt gebruiken.

Met behulp van de stroom van het apparaat, krijgt de toepassing tokens via een proces in twee stappen is speciaal ontworpen voor deze apparaten/OS. Voorbeelden van dergelijke toepassingen worden toepassingen die worden uitgevoerd op iOT-apparaten of de opdrachtregelprogramma's (CLI). 

![Stroom voor apparaatcode](media/msal-authentication-flows/device-code.png)

1. Wanneer de verificatie van de gebruiker is vereist, de app biedt een code en vraag de gebruiker om een ander apparaat (zoals een smartphone internetverbinding) gebruiken om te navigeren naar een URL (bijvoorbeeld https://microsoft.com/devicelogin), waarbij de gebruiker wordt gevraagd de code op te geven. Dat klaar, de webpagina de gebruiker via een normale verificatie-ervaring leidt, met inbegrip van toestemming wordt gevraagd en meervoudige verificatie, indien nodig.

2. Bij een geslaagde verificatie, de opdrachtregel-app ontvangt de vereiste tokens via een back-kanaal en gebruikt voor het uitvoeren van de web-API-aanroepen nodig is.

### <a name="constraints"></a>Beperkingen

- De stroom van apparaat is alleen beschikbaar in openbare clienttoepassingen.
- De instantie doorgegeven bij het maken van de openbare clienttoepassing moet zijn:
  - tenants (van het formulier `https://login.microsoftonline.com/{tenant}/` waar `{tenant}` is de GUID die de tenant-ID of een domein dat is gekoppeld aan de tenant vertegenwoordigt.
  - of een werk- en schoolaccounts-accounts (`https://login.microsoftonline.com/organizations/`).
- Persoonlijke Microsoft-accounts worden nog niet ondersteund door de Azure AD v2.0-eindpunt (u kunt geen gebruiken de `/common` of `/consumers` tenants).

## <a name="integrated-windows-authentication"></a>Geïntegreerde Windows-verificatie
MSAL biedt ondersteuning voor geïntegreerde Windows-verificatie (IWA) voor bureaublad of mobiele toepassingen die worden uitgevoerd op een domein of Azure AD toegevoegde Windows-computer. IWA gebruikt, kunnen deze toepassingen een token op de achtergrond (zonder uw tussenkomst van de gebruikersinterface van de gebruiker) ophalen. 

![Geïntegreerde Windows-verificatie](media/msal-authentication-flows/integrated-windows-authentication.png)

1. Krijgt een token met behulp van geïntegreerde Windows-verificatie.
2. Het token wordt gebruikt om aanvragen van de resource te maken.

### <a name="constraints"></a>Beperkingen

Biedt ondersteuning voor IWA **federatieve** alleen voor gebruikers.  Gebruikers in een Active Directory gemaakt en ondersteund door Azure Active Directory. Gebruikers die rechtstreeks in Azure AD, zonder AD back-ups worden gemaakt (**beheerd** gebruikers) deze authenticatiestroom niet gebruiken. Deze beperking heeft geen invloed op de [gebruikersnaam en wachtwoord stroom](#usernamepassword).

IWA is bedoeld voor apps die zijn geschreven voor .NET Framework en .NET Core Universal Windows Platform-platforms.

IWA komt niet overslaan voor MFA (Multi-factor authentication). Als MFA is geconfigureerd, worden de IWA kan mislukken als er een MFA-controle is nodig omdat MFA tussenkomst van de gebruiker vereist. Dit is een lastige. IWA is niet-interactieve, maar tweeledige verificatie (2FA) is vereist voor interactie met de gebruiker. U doet geen controle over wanneer de id-provider aanvraagt 2FA moet worden uitgevoerd, de tenant-beheerder heeft. Uit onze observaties is 2FA is vereist wanneer u zich vanaf een ander land als niet is verbonden via VPN met een bedrijfsnetwerk, en soms zelfs aanmeldt wanneer die zijn verbonden via VPN. Niet had verwacht een deterministische set regels, Azure Active Directory maakt gebruik van AI voor steeds meer als 2FA vereist is. U moet terugvallen op een gebruiker krijgt (https://aka.ms/msal-net-interactive) als IWA is mislukt.

De instantie doorgegeven bij het maken van de openbare clienttoepassing moet zijn:
- tenants (van het formulier `https://login.microsoftonline.com/{tenant}/` waar `tenant` is de guid die de tenant-ID of een domein dat is gekoppeld aan de tenant vertegenwoordigt.
- voor een werk- en schoolaccounts (`https://login.microsoftonline.com/organizations/`). Persoonlijke Microsoft-accounts worden niet ondersteund (u kunt geen gebruiken `/common` of `/consumers` tenants).

Omdat IWA een stroom op de achtergrond is:
- de gebruiker van uw toepassing moet eerder zijn toegestaan dat de toepassing wordt gebruikt. 
- of de tenant-beheerder moet eerder hebben ingestemd met alle gebruikers in de tenant om het gebruik van de toepassing.
- Dit betekent dat:
    - kunt u als ontwikkelaar hebt gedrukt de **verlenen** knop op de Azure-portal voor uzelf. 
    - of een tenantbeheerder heeft gedrukt de **Grant/revoke-beheerderstoestemming voor {tenantdomein}** knop in de **API-machtigingen** tabblad van de registratie van de toepassing (Zie [machtigingen toevoegen toegang tot web-API's](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis))
    - of u een manier om gebruikers toestemming geven voor de toepassing hebt opgegeven (Zie [aanvragen van toestemming van de individuele gebruiker](v2-permissions-and-consent.md#requesting-individual-user-consent))
    - of u een manier voor de tenantbeheerder toestemming voor de toepassing hebt opgegeven (Zie [beheerderstoestemming](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant))

De stroom IWA is ingeschakeld voor .NET-desktop-, .NET Core- en Windows Universal Platform-apps. Op .NET Core is alleen de overbelasting houdend met de gebruikersnaam is beschikbaar als het .NET Core-platform geen vragen de gebruikersnaam voor het besturingssysteem stellen.
  
Zie voor meer informatie over toestemming [v2.0 machtigingen en toestemming](v2-permissions-and-consent.md).

## <a name="usernamepassword"></a>Gebruikersnaam en wachtwoord 
MSAL ondersteunt de [verlenen van OAuth 2-referenties voor wachtwoord van resource-eigenaar](v2-oauth-ropc.md), waarmee een toepassing aan te melden bij de gebruikers hun wachtwoord direct kan verwerken. In uw desktop-toepassing kunt u de stroom van de gebruikersnaam en wachtwoord op de achtergrond een token ophalen. Er is geen gebruikersinterface is vereist bij het gebruik van de toepassing.

![Stroom van de gebruikersnaam en wachtwoord](media/msal-authentication-flows/username-password.png)

1. Een token ontvangt door de gebruikersnaam en het wachtwoord te sturen naar de id-provider.
2. Een web-API met behulp van het token aanroept.

> [!WARNING]
> Deze stroom is **niet aanbevolen** omdat hiervoor een hoge mate van blootstelling van vertrouwen en gebruiker.  U moet deze stroom alleen gebruiken als andere, beter te beveiligen, stromen kunnen niet worden gebruikt. Zie voor meer informatie over dit probleem [in dit artikel](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

De gewenste stroom voor het verkrijgen van een token op de achtergrond op Windows-domein-machines is [geïntegreerde Windows-verificatie](#integrated-windows-authentication). Anders, kunt u ook gebruiken [codestroom van apparaat](#device-code)

Hoewel dit handig in sommige gevallen (DevOps-scenario's), is als u wilt gebruiken van gebruikersnaam en wachtwoord in interactieve scenario's waarin u uw eigen gebruikersinterface hebt opgeven, moet u echt Denk na over hoe u afstappen van het. Met behulp van gebruikersnaam en wachtwoord, u zijn geven tot een aantal dingen:
- Core-tenants van moderne identiteit: wachtwoord opgehaald gevangen, opnieuw afgespeeld. Omdat we dit concept van een share-geheim die kan worden onderschept hebben.
Dit is niet compatibel met de configuratie.
- gebruikers hoeven te doen MFA niet mogelijk om te melden bij (wat er geen tussenkomst van de is)
- Gebruikers niet mogelijk om u te eenmalige aanmelding

### <a name="constraints"></a>Beperkingen

Naast de [geïntegreerde Windows-verificatie beperkingen](#integrated-windows-authentication), de volgende beperkingen zijn ook van toepassing:

- De gebruikersnaam en wachtwoord-stroom is niet compatibel met voorwaardelijke toegang en verificatie met meerdere factoren: Als gevolg hiervan, als uw app wordt uitgevoerd in een Azure AD-tenant waar de tenantbeheerder is multi-factor authentication wordt vereist, niet u deze stroom gebruiken. Veel organisaties doen dat.
- Dit werkt alleen voor werk en schoolaccounts (niet MSA)
- De stroom is beschikbaar voor .NET-desktop- en .NET core, maar niet voor universele Windows-Platform.

### <a name="azure-ad-b2c-specifics"></a>Azure AD B2C-specificaties

Lees voor meer informatie over het gebruik van MSAL.NET en Azure AD B2C [ROPC gebruiken met Azure AD B2C (MSAL.NET)](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c).
