---
title: Verificatiestromen (Microsoft Authentication Library) | Azure
description: Meer informatie over de verificatiestromen en verleent die wordt gebruikt door de Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
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
ms.openlocfilehash: b7ba6ae188c098e85573503a1518ba65480d713a
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807210"
---
# <a name="authentication-flows"></a>Verificatiestromen

Dit artikel beschrijft de verschillende verificatie-stromen geleverd door Microsoft Authentication Library (MSAL).  Deze stromen kunnen worden gebruikt in tal van verschillende toepassingsscenario's.

| Stroom | Description | Gebruikt in|  
| ---- | ----------- | ------- | 
| [Interactief](#interactive) | Hiermee wordt het token opgehaald via een interactieve proces dat de gebruiker wordt gevraagd om referenties via een browser of een pop-upvenster. | [Bureaubladapps](scenario-desktop-overview.md), [mobiele apps](scenario-mobile-overview.md) |
| [Impliciete goedkeuring voor](#implicit-grant) | Hiermee kunnen tokens ophalen zonder dat er een back-endserver referentie exchange wordt uitgevoerd. Hiermee wordt de app te melden bij de gebruiker, sessie en het verkrijgen van tokens, voor andere web-API's, allemaal binnen de client JavaScript-code.| [Toepassingen met één pagina (SPA)](scenario-spa-overview.md) |
| [Autorisatiecode](#authorization-code) | In apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's gebruikt. Hiermee kunt u aanmelden en API-toegang toevoegen aan uw mobiele en bureaublad-apps. | [Bureaubladapps](scenario-desktop-overview.md), [mobiele apps](scenario-mobile-overview.md), [web-apps](scenario-web-app-call-api-overview.md) | 
| [On-behalf-of](#on-behalf-of) | Een toepassing wordt aangeroepen voor een service of web-API, die op zijn beurt moet een andere service worden aangeroepen of een web-API. Het idee is dat de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen doorgegeven. | [Web-API's](scenario-web-api-call-api-overview.md) |
| [Clientreferenties](#client-credentials) | Hebt u toegang tot web gehoste bronnen met behulp van de identiteit van een toepassing. Doorgaans gebruikt voor interactie met server-naar-server die moeten worden uitgevoerd op de achtergrond, zonder directe interactie met een gebruiker. | [daemon-apps](scenario-daemon-overview.md) |
| [Apparaatcode](#device-code) | Hiermee kunnen gebruikers zich aanmelden bij de invoer beperkte apparaten zoals een smart-tv's, IoT-apparaat of een printer. | [Desktop/mobile apps](scenario-desktop-acquire-token.md#command-line-tool-without-web-browser) |
| [Geïntegreerde Windows-verificatie](scenario-desktop-acquire-token.md#integrated-windows-authentication) | Hiermee kunnen computers toepassingen op het domein of Azure Active Directory (Azure AD) toegevoegd om een token op de achtergrond (zonder uw tussenkomst van de gebruikersinterface van de gebruiker) te verkrijgen.| [Desktop/mobile apps](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Gebruikersnaam en wachtwoord](scenario-desktop-acquire-token.md#username--password) | Kan een toepassing aan te melden bij de gebruikers hun wachtwoord direct kan verwerken. Deze stroom wordt niet aanbevolen. | [Desktop/mobile apps](scenario-desktop-acquire-token.md#username--password) | 

## <a name="interactive"></a>Interactief
MSAL ondersteunt de mogelijkheid om interactief de gebruiker vragen om hun referenties aanmelden en een token verkrijgen met behulp van deze referenties.

![Diagram van interactieve stroom](media/msal-authentication-flows/interactive.png)

Zie voor meer informatie over het gebruik van MSAL.NET aan te schaffen interactief tokens op specifieke platforms:
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [Universeel Windows-platform](msal-net-uwp-considerations.md)

Zie voor meer informatie over interactieve aanroepen in MSAL.js [vragen gedrag in de interactieve aanvragen MSAL.js](msal-js-prompt-behavior.md).

## <a name="implicit-grant"></a>Impliciete goedkeuring voor

MSAL ondersteunt de [OAuth 2 impliciete stroom verlenen](v2-oauth2-implicit-grant-flow.md), waarmee de app ophalen van tokens uit de Microsoft identity-platform zonder uit te voeren van een back-endserver uitwisseling van verificatiegegevens. Hiermee wordt de app te melden bij de gebruiker, sessie en het verkrijgen van tokens, voor andere web-API's, allemaal binnen de client JavaScript-code.

![Diagram van de impliciete stroom](media/msal-authentication-flows/implicit-grant.svg)

Veel moderne web-apps worden gebouwd als client-side, één pagina toepassingen, die zijn geschreven met behulp van JavaScript of een beveiligd-WACHTWOORDVERIFICATIE framework zoals Angular, Vue.js en React.js. Deze toepassingen worden uitgevoerd in een webbrowser, en hebben verschillende kenmerken dan traditionele serverzijde webtoepassingen. Het Microsoft identity-platform kan één pagina toepassingen voor gebruikers aanmelden en het verkrijgen van tokens voor toegang tot back-endservices of web-API's, met behulp van de impliciete stroom. De impliciete stroom kan de toepassing om op te halen van ID-tokens te vertegenwoordigen de geverifieerde gebruiker, en ook toegangstokens die nodig zijn voor de beveiligde API's aanroepen.

Deze authenticatiestroom geen toepassingsscenario's die gebruikmaken van platformoverschrijdende JavaScript-frameworks zoals Electron en React-Native, omdat ze meer mogelijkheden voor interactie met de systeemeigen platforms vereisen.

## <a name="authorization-code"></a>Autorisatiecode
MSAL ondersteunt de [OAuth 2-autorisatiecode verlenen](v2-oauth2-auth-code-flow.md). Deze machtiging kan worden gebruikt in apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's. Hiermee kunt u aanmelden en API-toegang toevoegen aan uw mobiele en bureaublad-apps. 

Wanneer gebruikers zich aanmelden voor webtoepassingen (websites), ontvangt de web-App een autorisatiecode.  De autorisatiecode wordt gebruikt om een token voor het aanroepen van web-API's te verkrijgen. In ASP.NET en ASP.NET Core web-apps, het enige doel van `AcquireTokenByAuthorizationCode` is het toevoegen van een token aan de tokencache. Het token kan vervolgens worden gebruikt door de toepassing (meestal in de domeincontrollers die alleen een token verkrijgen voor een API met behulp van `AcquireTokenSilent`).

![Diagram van autorisatiecodestroom](media/msal-authentication-flows/authorization-code.png)

In het voorgaande diagram wordt de toepassing:

1. Een autorisatiecode die wordt ingewisseld voor een toegangstoken aanvragen.
2. Gebruikt het toegangstoken voor het aanroepen van een web-API.

### <a name="considerations"></a>Overwegingen
- U kunt slechts één keer de autorisatiecode gebruiken om in te wisselen van een token. Probeer niet om een token meerdere keren met de dezelfde autorisatiecode (als deze expliciet door de standard-protocolspecificatie verboden) te verkrijgen. Als je de code meerdere keren opzet, of omdat u niet op de hoogte dat een framework ook het allemaal voor u doet zijn, krijgt u de volgende fout: `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- Als u een ASP.NET- of ASP.NET Core-toepassing schrijft, wordt dit kan gebeuren dat als u het framework niet die zien u hebt de autorisatiecode al ingewisseld. Hiervoor moet u contact opnemen de `context.HandleCodeRedemption()` -methode van de `AuthorizationCodeReceived` gebeurtenis-handler.

- Vermijd het delen van het toegangstoken met ASP.NET, dat mogelijk te voorkomen dat incrementele toestemming correct gebeurt. Zie voor meer informatie, [uitgeven #693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693).

## <a name="on-behalf-of"></a>On-behalf-of

MSAL ondersteunt de [OAuth 2 op-andere gebruikers-of-verificatiestroom](v2-oauth2-on-behalf-of-flow.md).  Deze stroom wordt gebruikt wanneer een toepassing een service of web-API, die op zijn beurt moet een andere service worden aangeroepen of een web-API aanroept. Het idee is dat de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen doorgegeven. Voor de middelste laag service voor geverifieerde aanvragen versturen naar de downstream-service, moet voor het beveiligen van een toegangstoken van de Microsoft identity-platform, namens de gebruiker.

![Diagram van on-namens-stroom](media/msal-authentication-flows/on-behalf-of.png)

In het vorige diagram:

1. De toepassing krijgt een toegangstoken voor de web-API.
2. Een client (web, desktop-, mobiele of één pagina toepassing) roept een beveiligde web-API, het toegangstoken toe te voegen als een bearer-token in de header voor verificatie van de HTTP-aanvraag. De web-API verifieert de gebruiker.
3. Als de client de web-API aanroept, vraagt de web-API een ander token op-andere gebruikers-of de gebruiker.  
4. De beveiligde web-API maakt gebruik van dit token aan te roepen een downstream web-API op-andere gebruikers-of de gebruiker.  De web-API kan ook later aanvragen van tokens voor andere downstream-API's (maar nog steeds namens de gebruiker).

## <a name="client-credentials"></a>Clientreferenties

MSAL ondersteunt de [clientreferenties die OAuth 2 flow](v2-oauth2-client-creds-grant-flow.md). Deze stroom kunt u toegang tot web gehoste bronnen met behulp van de identiteit van een toepassing. Dit type verlenen wordt vaak gebruikt voor interactie met server-naar-server die moeten worden uitgevoerd op de achtergrond, zonder directe interactie met een gebruiker. Dergelijke toepassingen worden vaak aangeduid als daemons of service-accounts. 

De referenties van de client verlenen flow kunnen inhoud een webservice (een vertrouwelijke client) voor het gebruik van zijn eigen referenties, in plaats van een gebruiker imiteren om te verifiëren bij het aanroepen van een andere webservice. In dit scenario wordt de client is doorgaans een middelste laag webservice, een daemon-service of een website. Voor een hoger niveau van zekerheid kan de Microsoft identity-platform ook de aanroepende service een certificaat (in plaats van een gedeelde geheim genoemd) gebruikt als referentie.

> [!NOTE]
> De vertrouwelijke clientstroom is niet beschikbaar op de mobiele platforms (UWP, Xamarin.iOS en Xamarin.Android), omdat deze alleen openbare clienttoepassingen ondersteunen. Openbare clienttoepassingen weet niet hoe om te bewijzen de identiteit van de toepassing met de id-Provider. Een beveiligde verbinding kan worden bereikt op web-app of web weer-API wordt beëindigd door het implementeren van een certificaat.

MSAL.NET ondersteunt twee typen clientreferenties. Deze clientreferenties moeten worden geregistreerd bij Azure AD. De referenties worden doorgegeven aan de constructors van de vertrouwelijke client-toepassing in uw code.

### <a name="application-secrets"></a>Toepassingsgeheimen 

![Diagram van vertrouwelijke client met een wachtwoord](media/msal-authentication-flows/confidential-client-password.png)

In het voorgaande diagram wordt de toepassing:

1. Krijgt een token met behulp van referenties voor het geheim of het wachtwoord van toepassing.
2. Het token wordt gebruikt om aanvragen van de resource te maken.

### <a name="certificates"></a>Certificaten 

![Diagram van vertrouwelijke client met certificaat](media/msal-authentication-flows/confidential-client-certificate.png)

In het voorgaande diagram wordt de toepassing:

1. Een token verkrijgt met behulp van de referenties van het computercertificaat.
2. Het token wordt gebruikt om aanvragen van de resource te maken.

Deze clientreferenties moeten worden:
- Geregistreerd bij Azure AD.
- Doorgegeven bij het samenstellen van de vertrouwelijke client-toepassing in uw code.


## <a name="device-code"></a>Apparaatcode
MSAL ondersteunt de [OAuth 2 apparaat codestroom](v2-oauth2-device-code.md), waarmee gebruikers zich aanmelden bij de invoer beperkte apparaten, zoals een smart-tv's, IoT-apparaat of een printer. Interactieve verificatie met Azure AD moet een webbrowser. De stroom van het apparaat kan de gebruiker een ander apparaat (bijvoorbeeld, een andere computer of een mobiele telefoon) aan te melden bij het interactief, waar het apparaat of het besturingssysteem geen een webbrowser biedt gebruiken.

Met behulp van de stroom van het apparaat, krijgt de toepassing tokens via een proces in twee stappen is speciaal ontworpen voor deze apparaten of besturingssystemen. Voorbeelden van dergelijke toepassingen zijn die wordt uitgevoerd op IoT-apparaten of de opdrachtregelprogramma's (CLI). 

![Diagram van de stroom van apparaat](media/msal-authentication-flows/device-code.png)

In het vorige diagram:

1. Wanneer de verificatie van de gebruiker is vereist, de app biedt een code en vraag de gebruiker om een ander apparaat (zoals een smartphone internetverbinding) gebruiken om te gaan naar een URL (bijvoorbeeld https://microsoft.com/devicelogin). De gebruiker wordt vervolgens gevraagd de code op te geven, en wordt uitgevoerd in een normale verificatie-ervaring, met inbegrip van toestemming wordt gevraagd en multi-factor authentication indien nodig.

2. Bij een geslaagde verificatie, de opdrachtregel-app ontvangt de vereiste tokens via een back-kanaal en gebruikt deze voor het uitvoeren van de web-API-aanroepen nodig is.

### <a name="constraints"></a>Beperkingen

- De stroom van apparaat is alleen beschikbaar in openbare clienttoepassingen.
- De instantie die wordt doorgegeven bij het maken van de openbare clienttoepassing moet een van de volgende:
  - Tenants (van het formulier `https://login.microsoftonline.com/{tenant}/` waar `{tenant}` is de GUID die de tenant-ID of een domein dat is gekoppeld aan de tenant).
  - voor een werk- en schoolaccounts (`https://login.microsoftonline.com/organizations/`).
- Persoonlijk Microsoft-account nog niet worden ondersteund door de Azure AD v2.0-eindpunt (u kunt geen gebruiken de `/common` of `/consumers` tenants).

## <a name="integrated-windows-authentication"></a>Geïntegreerde Windows-verificatie
MSAL biedt ondersteuning voor geïntegreerde Windows-verificatie (IWA) voor bureaublad of mobiele toepassingen die worden uitgevoerd op een domein of Azure AD toegevoegde Windows-computer. IWA gebruikt, kunnen deze toepassingen een token op de achtergrond (zonder uw tussenkomst van de gebruikersinterface van de gebruiker) ophalen. 

![Diagram van geïntegreerde Windows-verificatie](media/msal-authentication-flows/integrated-windows-authentication.png)

In het voorgaande diagram wordt de toepassing:

1. Krijgt een token met behulp van geïntegreerde Windows-verificatie.
2. Het token wordt gebruikt om aanvragen van de resource te maken.

### <a name="constraints"></a>Beperkingen

IWA biedt ondersteuning voor federatieve gebruikers alleen, wat betekent dat gebruikers in Active Directory gemaakt en ondersteund door Azure AD. Gebruikers rechtstreeks in Azure AD, zonder Active Directory een back-up gemaakt (beheerde gebruikers) kunnen niet deze authenticatiestroom gebruiken. Deze beperking niet van invloed op de [gebruikersnaam en wachtwoord stroom](#usernamepassword).

IWA is bedoeld voor apps die zijn geschreven voor .NET Framework en .NET Core Universal Windows Platform-platforms.

IWA overslaan geen multi-factor authentication. Als multi-factor authentication is geconfigureerd, worden de IWA kan mislukken als er een uitdaging meervoudige verificatie is vereist. Multi-factor authentication vereist tussenkomst van de gebruiker.

U bepalen niet wanneer de verificatie met twee factoren om te worden uitgevoerd voor het aanvragen van de id-provider. De tenant-beheerder heeft. Normaal gesproken is tweeledige verificatie vereist wanneer u zich aanmelden met een ander land, wanneer u bent niet verbonden via VPN met een bedrijfsnetwerk en soms zelfs als u bent verbonden via VPN. Azure AD maakt gebruik van AI voor steeds meer als tweeledige verificatie vereist is. Als IWA mislukt, u moet terugvallen op een [interactieve Gebruikersprompt] (#interactive).

De instantie die wordt doorgegeven bij het maken van de openbare clienttoepassing moet een van de volgende:
- Tenants (van het formulier `https://login.microsoftonline.com/{tenant}/` waar `tenant` is de guid die de tenant-ID of een domein dat is gekoppeld aan de tenant).
- voor een werk- en schoolaccounts (`https://login.microsoftonline.com/organizations/`). Persoonlijke Microsoft-accounts worden niet ondersteund (u kunt geen gebruiken `/common` of `/consumers` tenants).

Omdat IWA een stroom op de achtergrond is, moet een van de volgende waar zijn:
- de gebruiker van uw toepassing moet eerder zijn toegestaan dat de toepassing wordt gebruikt. 
- De tenantbeheerder moet eerder hebben ingestemd met alle gebruikers in de tenant om het gebruik van de toepassing.

Dit betekent dat een van de volgende geldt:
- U als ontwikkelaar hebt geselecteerd **verlenen** in Azure portal zelf.
- Een tenantbeheerder heeft geselecteerd **Grant/revoke-beheerderstoestemming voor {tenantdomein}** in de **API-machtigingen** tabblad van de registratie van de toepassing (Zie [machtigingen voor toegang tot web-API's toevoegen ](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)).
- U hebt opgegeven een manier om gebruikers toestemming geven voor de toepassing (Zie [aanvragen van toestemming van de individuele gebruiker](v2-permissions-and-consent.md#requesting-individual-user-consent)).
- U hebt opgegeven een manier voor de tenantbeheerder toestemming voor de toepassing (Zie [beheerderstoestemming](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)).

De stroom IWA is ingeschakeld voor .NET-desktop-, .NET Core- en Windows Universal Platform-apps. Op .NET Core is alleen de overbelasting houdend met de gebruikersnaam beschikbaar. De .NET Core-platform geen vragen stellen de gebruikersnaam voor het besturingssysteem.
  
Zie voor meer informatie over toestemming [v2.0 machtigingen en toestemming](v2-permissions-and-consent.md).

## <a name="usernamepassword"></a>Gebruikersnaam en wachtwoord 
MSAL ondersteunt de [verlenen van OAuth 2-referenties voor wachtwoord van resource-eigenaar](v2-oauth-ropc.md), waarmee een toepassing aan te melden bij de gebruikers hun wachtwoord direct kan verwerken. In uw desktop-toepassing kunt u de stroom van de gebruikersnaam en wachtwoord op de achtergrond een token ophalen. Er is geen gebruikersinterface is vereist bij het gebruik van de toepassing.

![Diagram van de stroom van de gebruikersnaam en wachtwoord](media/msal-authentication-flows/username-password.png)

In het voorgaande diagram wordt de toepassing:

1. Een token ontvangt door de gebruikersnaam en het wachtwoord te sturen naar de id-provider.
2. Een web-API aanroept met behulp van het token.

> [!WARNING]
> Deze stroom wordt niet aanbevolen. Een hoge mate van blootstelling van vertrouwen en de gebruiker is vereist.  U moet deze stroom alleen gebruiken als andere, beter te beveiligen, stromen kunnen niet worden gebruikt. Zie voor meer informatie, [wat is de oplossing voor het groeiende probleem van wachtwoorden?](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

De gewenste stroom voor het verkrijgen van een token op de achtergrond op het domein Windows-machines is [geïntegreerde Windows-verificatie](#integrated-windows-authentication). Anders, kunt u ook gebruiken [apparaat codestroom](#device-code).

Hoewel dit handig in sommige gevallen (DevOps-scenario's), is als u wilt gebruiken van gebruikersnaam en wachtwoord in interactieve scenario's waarin u uw eigen gebruikersinterface hebt opgeven, kunt u proberen om te voorkomen dat deze. Met behulp van gebruikersnaam en wachtwoord:
- Gebruikers die willen multi-factor Authentication zich niet aanmelden (wat er geen tussenkomst van de is).
- Gebruikers niet mogelijk om u te eenmalige aanmelding.

### <a name="constraints"></a>Beperkingen

Naast de [geïntegreerde Windows-verificatie beperkingen](#integrated-windows-authentication), de volgende beperkingen zijn ook van toepassing:

- De gebruikersnaam en wachtwoord-stroom is niet compatibel is met voorwaardelijke toegang en verificatie met meerdere factoren. Als gevolg hiervan, als uw app wordt uitgevoerd in een Azure AD-tenant waar de tenantbeheerder is multi-factor authentication wordt vereist, niet u deze stroom gebruiken. Veel organisaties doen dat.
- Dit werkt alleen voor werk en school-accounts (niet een Microsoft-accounts).
- De stroom is beschikbaar voor .NET-desktop- en .NET Core, maar niet voor universele Windows-Platform.

### <a name="azure-ad-b2c-specifics"></a>Azure AD B2C-specificaties

Zie voor meer informatie over het gebruik van MSAL.NET en Azure AD B2C [ROPC gebruiken met Azure AD B2C (MSAL.NET)](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c).
