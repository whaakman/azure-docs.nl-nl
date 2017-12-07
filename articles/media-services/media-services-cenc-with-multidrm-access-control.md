---
title: 'CENC met Multi-DRM en toegangsbeheer: een verwijzing naar ontwerpen en implementeren op Azure en Azure mediaservices | Microsoft Docs'
description: "Meer informatie over het in de Microsoft® Smooth Streaming Client overdragen Kit licentieverlening voor."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: 50bcb71cd4f52386e9ea428fc124ac30ae9a862b
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC met Multi-DRM en Access Control: een referentieontwerp en -implementatie in Azure en Azure Media Services
 
## <a name="introduction"></a>Inleiding
Het is bekend dat het een complexe taak ontwerpen en bouwen van een DRM-subsysteem voor een OTT is of online streamingoplossing. En het is gebruikelijk voor operators/online video providers voor het uitbesteden van dit onderdeel gespecialiseerde DRM serviceproviders. Het doel van dit document is een verwijzing naar ontwerpen en implementeren van end-to-end DRM subsysteem in OTT of online-streaming oplossing presenteren.

De betreffende lezers van dit document zijn engineers werken in DRM-subsysteem van OTT online streaming/meerdere-screen oplossingen of eventuele geïnteresseerd in DRM-subsysteem lezers. De veronderstelling is dat lezers bekend met ten minste één van de technologieën DRM op de markt, zoals PlayReady, Widevine, FairPlay of Adobe Access bent.

Door DRM opnemen we ook (Common Encryption) CENC met multi-DRM. Een belangrijke trend in online streaming en OTT industrie is met CENC met meerdere-native-DRM op verschillende clientplatforms, namelijk een verschuiving van de vorige trend van het gebruik van een enkele DRM en de client-SDK voor verschillende clientplatforms. Als u CENC met meerdere native DRM, zowel PlayReady als Widevine zijn versleuteld volgens de [Common Encryption (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) specificatie.

De voordelen van CENC met multi-DRM zijn als volgt:

1. Vermindert versleuteling kosten omdat de verwerking van een enkele versleuteling wordt gebruikt die gericht is op verschillende platforms met de systeemeigen DRM's;
2. Beperkt de kosten van het versleutelde activa te beheren omdat slechts één exemplaar van de versleutelde activa is vereist;
3. Elimineert kosten licentieverlening sinds de systeemeigen DRM-client meestal vrije ruimte op het systeemeigen platform is DRM-client.

Microsoft is een actieve organisator van deze actie van DASH en CENC samen met enkele belangrijke zakelijke spelers. Microsoft Azure Media Services heeft is het bieden van ondersteuning van DASH en CENC. Zie voor recente aankondigingen van Mingfei blogs: [aangekondigd Google Widevine-licentie levering van services in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/), en [Azure Media Services wordt toegevoegd voor Google Widevine-verpakking voor het leveren van multi-DRM stroom](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Overzicht van dit artikel
Het doel van dit artikel omvat het volgende:

1. Bevat een verwijzing van DRM-subsysteem met CENC met multi-DRM;
2. Biedt de implementatie van een verwijzing op Microsoft Azure/Azure Media Services-platform;
3. Sommige onderwerpen ontwerp- en implementatiestappen besproken.

In het artikel "multi-DRM" heeft betrekking op het volgende:

1. Microsoft PlayReady
2. Google Widevine
3. FairPlay van Apple 

De volgende tabel geeft een overzicht van de systeemeigen platform/systeemeigen app en browsers die door elke DRM ondersteund.

| **Clientplatform** | **Systeemeigen DRM-ondersteuning** | **Browser-App** | **Streaming-indelingen** |
| --- | --- | --- | --- |
| **Smart tv, operator STB's, OTT STB 's** |PlayReady voornamelijk, en/of Widevine en/of andere |Linux, Opera, WebKit, andere |Verschillende indelingen |
| **Windows 10-apparaten (Windows-PC, Windows-Tablets, Windows Phone, Xbox.)** |PlayReady |MS IE11-Edge/EME<br/><br/><br/>UWP |STREEPJE (voor HLS, PlayReady wordt niet ondersteund)<br/><br/>DASH, Smooth Streaming (HLS voor, PlayReady wordt niet ondersteund) |
| **Android-apparaten (telefoon, Tablet, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X-clients en Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


Gezien de huidige status van implementatie voor elke DRM, een service, wilt waarschijnlijk 2 of 3 DRM's om ervoor te zorgen dat u de typen van de eindpunten van-adres in de beste manier te implementeren.

Er is een afweging tussen de complexiteit van de logica van de service en de complexiteit bij de client tot een bepaalde mate van de gebruikerservaring op verschillende clients.

Houd er rekening mee deze feiten zodat uw selectie:

* PlayReady is systeemeigen geïmplementeerd in elke Windows-apparaat op een Android-apparaten en beschikbaar via software-SDK's op vrijwel elk platform
* Widevine is systeemeigen geïmplementeerd in elke Android-apparaat, Chrome en sommige andere apparaten
* FairPlay is beschikbaar op iOS en Mac OS-clients of via iTunes.

Zo een typische multi-DRM zijn zou:

* Optie 1: PlayReady en Widevine
* Optie 2: PlayReady, Widevine en FairPlay

## <a name="a-reference-design"></a>Een referentie-ontwerp
In deze sectie biedt we een reference model dat is agnostisch ten opzichte van technologieën die worden gebruikt om dit te implementeren.

Een DRM-subsysteem kan de volgende onderdelen bevatten:

1. Sleutelbeheer
2. DRM verpakking
3. Levering van DRM-licentie
4. Recht controleren
5. Verificatie/autorisatie
6. Speler
7. Oorsprong/CDN

Het volgende diagram illustreert de hoog niveau interactie tussen de onderdelen in een DRM-subsysteem.

![DRM-subsysteem met CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Er zijn drie basic 'lagen' in het ontwerp:

1. Back-office-laag (in zwart) die extern; niet worden weergegeven
2. 'DMZ' laag (blauw) met alle de openbare; internetgerichte eindpunten
3. Openbare internetlaag (lichte blauw) met CDN en spelers met verkeer via openbaar Internet.

Er moet een inhoudsbeheer hulpprogramma voor het beheren van DRM-beveiliging, ongeacht het statische of dynamische versleuteling is. De invoer voor DRM-codering moeten worden opgenomen:

1. MBR video-inhoud;
2. Inhoudssleutel;
3. URL's aanschaf van licentie.

Tijdens het afspelen is de stroom van hoog niveau:

1. Gebruiker is geverifieerd;
2. Verificatietoken is gemaakt voor de gebruiker.
3. DRM beveiligde inhoud (manifest) wordt gedownload naar de speler;
4. Player verzendt overname licentieaanvraag met licentieservers samen met sleutel-ID en -autorisatie-token.

Voordat u doorgaat naar het volgende onderwerp, een aantal woorden over het ontwerp van Sleutelbeheer.

| **ContentKey-Asset:** | **Scenario** |
| --- | --- |
| 1-op-1 |Het meest eenvoudige geval. Dit biedt de beste controle. Maar resulteert dit in het algemeen in de hoogste kosten van de licentie-levering. Aanvraag is vereist voor alle beveiligde activa op ten minste één licentie. |
| 1-op-veel |U kunt dezelfde inhoudssleutel voor meerdere activa. Bijvoorbeeld: voor alle activa in een logische groep zoals een genre of een subset van genre (of film gen), u kunt een enkel inhoudssleutel. |
| Veel-op-1 |Meerdere inhoudssleutels nodig zijn voor elk actief. <br/><br/>Als u moet de beveiliging van dynamische CENC met multi-DRM voor MPEG DASH en dynamische AES-128-versleuteling voor HLS wilt toepassen, moet u bijvoorbeeld twee afzonderlijke inhoud sleutels, elk met een eigen ContentKeyType. (Voor de inhoudssleutel gebruikt voor dynamische CENC beveiliging, ContentKeyType.CommonEncryption moet worden gebruikt, terwijl voor de inhoudssleutel gebruikt voor dynamische AES-128-versleuteling, ContentKeyType.EnvelopeEncryption moet worden gebruikt.)<br/><br/>Een ander voorbeeld: bij CENC bescherming van DASH-inhoud, in theorie, een inhoudssleutel kan worden gebruikt voor het beveiligen van videostream en een andere inhoudssleutel audiostroom beveiligen. |
| Veel-op - veel |Combinatie van de bovenstaande twee scenario's: één set van inhoud sleutels worden gebruikt voor elk van de meerdere activa in dezelfde asset 'groep'. |

Een andere belangrijke factor in overweging moet nemen is het gebruik van permanente en niet-permanente licenties.

Waarom deze overwegingen belangrijk zijn?

Ze hebben direct gevolgen voor licentie leveringskosten als u de openbare cloud voor de levering van licentie gebruiken. De volgende twee verschillende ontwerp gevallen ter illustratie van laten we eens:

1. Maandabonnement: permanente licentie en 1-op-veel inhoud sleutel aan asset toewijzing gebruiken. Bijvoorbeeld voor alle kinderen films gebruiken we een enkele inhoudssleutel voor versleuteling. In dit geval:

    Totaal aantal licenties van # aangevraagd voor alle kinderen films/apparaat = 1
2. Maandabonnement: niet-permanente licentie en 1 op 1 toewijzing tussen inhoudssleutel en asset gebruiken. In dit geval:

    Totaal aantal licenties van # aangevraagd voor alle kinderen films/apparaat = [# films gevolgde] x [# sessies]

Zoals u gemakkelijk zien kunt, resulteren de twee verschillende ontwerpen in zeer andere licentiegroepen aanvraag patronen daarom licentie levering kosten als service voor het leveren van licenties is opgegeven door een openbare cloud zoals Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Ontwerp van de toewijzing aan technologie voor implementatie
Vervolgens toewijzen we onze algemene ontwerp aan technologieën op Microsoft Azure/Azure Media Services-platform, door te geven welke technologie die u wilt gebruiken voor elke bouwsteen.

De volgende tabel ziet u de toewijzing:

| **Bouwstenen** | **Technologie** |
| --- | --- |
| **Player** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **ID-Provider (IDP)** |Azure Active Directory |
| **Beveiligde beveiligingstokenservice (STS)** |Azure Active Directory |
| **Werkstroom van DRM-beveiliging** |Azure Media Services dynamische beveiliging |
| **Levering van DRM-licentie** |1. Azure Media Services-licentie leveringsmethode (PlayReady, Widevine, FairPlay), of <br/>2. Axinom-licentieserver <br/>3. Aangepaste PlayReady-licentieserver |
| **Oorsprong** |Azure Media Services-Streaming-eindpunt |
| **Sleutelbeheer** |Niet nodig zijn voor de referentie-implementatie |
| **Inhoudbeheer** |Een C#-consoletoepassing |

Met andere woorden, zowel Identity-Provider (IDP) en Secure Token Service (STS) worden Azure AD. Voor player, gebruiken we [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/). Zowel Azure Media Services en Azure Media Player ondersteunen DASH en CENC met multi-DRM.

Het volgende diagram toont de algemene structuur en stroom met de bovenstaande technologie-toewijzing.

![CENC op AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Om het instellen van dynamische CENC versleuteling, maakt het hulpprogramma inhoudsbeheer gebruik van de volgende invoer:

1. Open inhoud;
2. Inhoudssleutel uit de sleutel genereren/management;
3. URL's aanschaf van licentie;
4. Een lijst met gegevens uit Azure AD.

De uitvoer van het hulpprogramma inhoudsbeheer zijn:

1. ContentKeyAuthorizationPolicy met de specificatie op hoe licentie levering controleert of een JWT-token en DRM licentie specificaties;
2. AssetDeliveryPolicy met specificaties op het streaming-indeling, DRM-beveiliging en URL's aanschaf van licentie.

Tijdens runtime, de gegevensstroom is als hieronder:

1. Bij verificatie van de gebruiker wordt een JWT-token gegenereerd;
2. Een van de claims in de JWT-token is 'groepen' claim met de object-ID van 'EntitledUserGroup'. Deze claim wordt gebruikt voor het doorgeven van 'recht selectievakje'.
3. Player downloads client manifest van een CENC beveiligde inhoud en 'ziet' het volgende:

   1. sleutel-ID
   2. de inhoud is beveiligd, CENC
   3. URL's aanschaf van licentie.
4. Player wordt een licentieaanvraag verkrijgen op basis van de browser/DRM ondersteund. In de aanvraag aanschaf van licentie ID sleutel en de JWT-token wordt ook worden verzonden. Service voor het leveren van licenties controleert de JWT-token en de claims alvorens de licentie nodig.

## <a name="implementation"></a>Implementatie
### <a name="implementation-procedures"></a>Procedures voor implementatie
De implementatie bevat de volgende stappen uit:

1. Test activa voorbereiden: een test-video naar multi-bitrate coderen/pakket gefragmenteerd MP4 in Azure Media Services. Dit asset is niet beveiligd door DRM. DRM-beveiliging worden door dynamische beveiliging later uitgevoerd.
2. Maken van sleutel-ID en inhoud sleutel (optioneel van sleutel seed). Voor onze doel sleutelbeheersysteem is niet nodig omdat we zijn omgaan met slechts één set van sleutel-ID en sleutel voor een aantal test activa;
3. AMS API gebruiken om multi-DRM licentie levering van services voor de test-asset te configureren. Als u aangepaste licentieservers door uw bedrijf of leveranciers van uw bedrijf in plaats van de licentie-services in Azure Media Services gebruikt, kunt u deze stap overslaan en licentie overname URL's in de stap voor het configureren van de licentie levering opgeven. AMS API is nodig om op te geven gedetailleerde configuraties, zoals autorisatie beleidsbeperking, licentie antwoord sjablonen voor services van andere DRM-licenties, enzovoort. Op dit moment is biedt de Azure-portal nog geen de benodigde gebruikersinterface voor deze configuratie. U kunt zoeken naar API-niveau info en voorbeeldcode in het volgende artikel: [met behulp van PlayReady en/of Widevine Dynamic Common Encryption](media-services-protect-with-playready-widevine.md).
4. AMS API gebruiken voor het configureren van het leveringsbeleid voor Assets voor de test-asset. U kunt zoeken naar API-niveau info en voorbeeldcode in het volgende artikel: [met behulp van PlayReady en/of Widevine Dynamic Common Encryption](media-services-protect-with-playready-widevine.md).
5. Maken en configureren van een Azure Active Directory-tenant in Azure.
6. Maken van een paar gebruikersaccounts en groepen in de Azure Active Directory-tenant: u moet ten minste maken 'EntitledUser' groeperen en een gebruiker toevoegen aan deze groep. Gebruikers in deze groep geeft recht controleren in de aanschaf van licentie en gebruikers die niet in deze groep om door te geven van verificatie mislukt en een licentie kan niet worden. Een lid van deze groep 'EntitledUser' is een claim vereiste 'groepen' in de JWT-token dat is uitgegeven door Azure AD. Deze vereiste claim moet worden opgegeven bij het configureren van multi-DRM licentie levering van services stap.
7. Een ASP.NET MVC-app die als host de video-speler fungeert maken. Deze ASP.NET-app worden beveiligd met gebruikersverificatie met de Azure Active Directory-tenant. Juiste claims worden opgenomen in de toegangstokens verkregen na verificatie van gebruiker. OpenID Connect API wordt aanbevolen voor deze stap. U moet de volgende NuGet-pakketten installeren:
   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Install-Package Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
8. Maken van een speler met [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/). [Azure Media-Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) kunt u opgeven welke DRM-technologie gebruiken op verschillende DRM-platform.
9. Testmatrix:

| **DRM** | **Browser** | **Resultaat van het gebruiksrecht heeft gebruiker** | **Resultaat van de gebruiker niet gemachtigd** |
| --- | --- | --- | --- |
| **PlayReady** |MS rand of IE11 op Windows 10 |Voltooid |Mislukt |
| **Widevine** |Chrome op Windows 10 |Voltooid |Mislukt |
| **FairPlay** |NOG TE BEPALEN | | |

George Trifonov van Azure Media Services-Team blog gedetailleerde stappen bij het instellen van Azure Active Directory voor een ASP.NET MVC player-app die is geschreven: [integreren Azure Media Services OWIN MVC op basis van de app met Azure Active Directory en beperken van de sleutel inhouddistributie op basis van claims JWT](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George heeft ook een blog geschreven op [JWT-token verificatie in Azure Media Services en dynamische versleuteling](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).  

Voor informatie over Azure Active Directory:

* U vindt informatie voor ontwikkelaars in [Azure Active Directory-handleiding voor ontwikkelaars](../active-directory/active-directory-developers-guide.md).
* U vindt informatie voor beheerders in [beheren van uw Azure AD-Directory](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Sommige gotchas in uitvoering
Er zijn enkele 'gotchas' in de implementatie. Ideale geval kunt de volgende lijst met 'gotchas' u het oplossen van problemen als u problemen ondervindt.

1. **Certificaatverlener** URL moet eindigen met **'/'**.  

    **Doelgroep** moet de client-ID van de player-toepassing en u moet ook toevoegen **'/'** aan het einde van de uitgevers-URL.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    In [JWT Decoder](http://jwt.calebb.net/), ziet u **aud** en **iss** als onder in de JWT-token:

    ![Eerste gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)
2. Machtigingen toevoegen aan de toepassing in AAD (op het tabblad configureren van de toepassing). Dit is vereist voor elke toepassing (lokale en geïmplementeerde versie).

    ![Tweede gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)
3. Gebruik de juiste verlener bij het instellen van dynamische CENC beveiliging:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Het volgende zal niet werken:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    De GUID is de AAD-tenant-ID. De GUID vindt u in de pop-up eindpunten in Azure-portal.
4. Verleen het lidmaatschap van claims van bevoegdheden. Zorg ervoor dat in het manifestbestand van AAD-toepassing, hebben we het volgende

    'groupMembershipClaims': 'All', (de standaardwaarde is null)
5. Juiste TokenType instellen bij het maken van de vereisten van de beperking.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Sinds de ondersteuning van JWT (AAD) naast SWT (ACS) toe te voegen, is de standaardwaarde TokenType TokenType.JWT. Als u SWT/ACS gebruikt, moet u instellen op TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Aanvullende onderwerpen voor implementatie
Naast we wordt naar de schijf uss sommige extra onderwerpen in onze ontwerpen en implementeren.

### <a name="http-or-https"></a>HTTP of HTTPS?
De ASP.NET MVC-spelertoepassing moesten moet ondersteunen het volgende:

1. Gebruikersverificatie via Azure AD die moet worden onder HTTPS;
2. JWT-token exchange tussen client en Azure AD die moet worden onder HTTPS;
3. DRM licentie overname door de client is nodig om te worden onder HTTPS als levering van de licentie wordt verstrekt door Azure Media Services. Natuurlijk schrijft PlayReady productpakket niet voor HTTPS voor de levering van licentie. Als de licentieserver PlayReady buiten Azure Media Services is, kan via HTTP of HTTPS worden gebruikt.

Daarom wordt de ASP.NET-toepassing player HTTPS gebruikt als een best practice. Dit betekent dat de mediaspeler Azure worden uitgevoerd in een HTTPS-pagina. Voor streaming we liever HTTP, daarom moet rekening houden met gemengde inhoud probleem.

1. Browser kan geen gemengde inhoud. Maar invoegtoepassingen zoals Silverlight en OSMF-invoegtoepassing voor smooth en STREEPJES zijn toegestaan. Gemengde inhoud is een beveiligingsprobleem - dit wordt veroorzaakt door de bedreiging van de mogelijkheid om te injecteren schadelijke JS die ertoe leiden de gegevens van de klant dat kunnen kunnen lopen.  Browsers dit standaard blokkeren en tot nu toe de enige manier om dit oplossen door op de server (oorsprong), zodat alle domeinen (ongeacht https of http) is. Dit is waarschijnlijk niet een goed idee ofwel.
2. Vermijd we gemengde inhoud: beide gebruik van HTTP of beide gebruik van HTTPS. Tijdens het afspelen van gemengde inhoud wordt silverlightSS tech vereist een gemengde inhoud waarschuwing uit te schakelen. technische flashSS verwerkt gemengde inhoud zonder gemengde inhoud waarschuwing.
3. Als uw streaming-eindpunt is gemaakt voordat augustus 2014, wordt deze geen ondersteuning voor HTTPS. In dit geval Maak en gebruik van een nieuwe streaming-eindpunt voor HTTPS.

In de implementatie verwijzing voor DRM beveiligde inhoud, zich toepassing en streaming onder HTTTPS. Voor inhoud openen hoeft Windows media player geen verificatie of licentie, hoeft u de vrijheid om te gebruiken via HTTP of HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory sleutelrollover ondertekenen
Dit is een belangrijk punt om rekening van uw implementatie. Als u geen rekening dit in uw implementatie houden, niet het voltooide systeem uiteindelijk meer volledig binnen maximaal 6 weken.

Azure AD maakt gebruik van industriestandaard vertrouwensrelatie tussen zichzelf en toepassingen die gebruikmaken van Azure AD. Azure AD gebruikt met name een ondertekeningssleutel die uit een openbare en persoonlijke sleutelpaar bestaat. Als u Azure AD maakt een beveiligingstoken dat informatie over de gebruiker bevat, wordt dit token is ondertekend door Azure AD met behulp van de persoonlijke sleutel voordat deze terug naar de toepassing wordt verzonden. Om te verifiëren dat het token geldig en daadwerkelijk oorsprong van Azure AD is, moet de toepassing van het token handtekening met de openbare sleutel die worden weergegeven door Azure AD dat is opgenomen in het document met federatieve metagegevens van de tenant valideren. Deze openbare sleutel, en de ondertekeningssleutel waarvan deze is afgeleid, is hetzelfde voor alle tenants worden gebruikt in Azure AD.

Gedetailleerde informatie over Azure AD-sleutelrollover vindt u in het document: [belangrijke informatie over de overschakeling van de sleutel ondertekening in Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Tussen de [openbaar / persoonlijk sleutelpaar](https://login.microsoftonline.com/common/discovery/keys/),

* De persoonlijke sleutel wordt gebruikt door Azure Active Directory voor het genereren van een JWT-token
* De openbare sleutel wordt gebruikt door een toepassing zoals DRM licentie levering van Services in AMS om te controleren of de JWT-token

Voor beveiliging doel draait Azure Active Directory periodiek dit certificaat (elke 6 weken). In geval van inbreuk op de beveiliging, kan de sleutelrollover elk gewenst moment optreden. Daarom moeten de services voor het leveren van licenties in AMS bijwerken van de openbare sleutel gebruikt als Azure AD het sleutelpaar draait, anders de verificatie van de token in AMS mislukt en er is geen licentie wordt verleend.

Dit wordt bereikt door in te stellen TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument bij het configureren van DRM-services voor het leveren van licenties.

De stroom van JWT-token is als hieronder:

1. Azure AD geeft de JWT-token met de huidige persoonlijke sleutel voor een geverifieerde gebruiker;
2. Wanneer u een speler ziet een CENC met multi-DRM beveiligde inhoud, zoekt het eerst de JWT-token dat is uitgegeven door Azure AD.
3. Windows media player verzendt licentie overname aanvraag met de JWT-token naar de services voor het leveren van licenties in AMS;
4. De services voor het leveren van licenties in AMS wordt de huidige/geldige openbare sleutel van Azure AD gebruiken om te controleren of de JWT-token alvorens licenties.

DRM-services voor het leveren van licentie wordt altijd controleren voor de huidige/geldige openbare sleutel van Azure AD. De openbare sleutel die wordt aangeboden door Azure AD worden de sleutel die wordt gebruikt voor het controleren van een JWT-token dat is uitgegeven door Azure AD.

Wat gebeurt er als de sleutelrollover treedt op nadat de AAD JWT-token wordt gegenereerd, maar voordat de JWT token is verzonden door spelers naar DRM licentie levering van services in AMS voor verificatie?

Omdat een sleutel wordt mogelijk teruggedraaid te allen tijde, is altijd meer dan één geldige openbare sleutel beschikbaar is in het document met federatieve metagegevens. Azure Media Services-licentie levering kunt u elk van de sleutels die zijn opgegeven in het document, omdat één sleutel kan snel worden teruggedraaid, een ander kan worden de vervanging ervan, enzovoort.

### <a name="where-is-the-access-token"></a>Waar kan ik de toegang tot Token?
Als u kijken hoe een web-app een API-app onder aanroept [toepassingsidentiteit met OAuth 2.0-Client referenties Grant](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), de authenticatiestroom is als hieronder:

1. Een gebruiker is aangemeld bij Azure AD in de webtoepassing (Zie de [webbrowser webtoepassing](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
2. Het eindpunt van de autorisatie Azure AD leidt de gebruikersagent terug naar de client-toepassing met een autorisatiecode. De gebruikersagent retourneert autorisatiecode op de clienttoepassing omleidings-URI.
3. De webtoepassing moet te verkrijgen van een toegangstoken zodat deze kan worden geverifieerd met de web-API en ophalen van de gewenste resource. Dit doet een aanvraag naar Azure AD-tokeneindpunt, met de referenties, client-ID en een web-API van toepassings-ID-URI. Dit geeft de autorisatiecode om aan te tonen dat de gebruiker heeft ingestemd.
4. Azure AD verifieert de toepassing en retourneert een JWT-toegangstoken dat wordt gebruikt voor de web-API aanroepen.
5. Het geretourneerde JWT-toegangstoken worden in de webtoepassing via HTTPS gebruikt de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag toevoegen aan de web-API. De web-API valideert de JWT-token, en als validatie geslaagd is, retourneert de gewenste resource.

In deze stroom "toepassings-id" vertrouwt de web-API dat de webtoepassing de gebruiker heeft geverifieerd. Om deze reden wordt dit patroon van een vertrouwde subsysteem genoemd. De [diagram op deze pagina](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) wordt beschreven hoe de autorisatiecode stroom works verlenen.

Licentie overname met tokenbeperking volgt er hetzelfde vertrouwde subsysteem patroon. En de service voor het leveren van licenties in Azure Media Services is het web API-resource, de 'back-end voor resource' een webtoepassing toegang moet hebben. Waar is dus het toegangstoken?

We zijn immers toegangstoken ophalen uit Azure AD. Na een geslaagde verificatie, wordt autorisatiecode geretourneerd. De autorisatiecode wordt vervolgens gebruikt, samen met client-ID en app sleutel voor het uitwisselen van toegangstoken. En het toegangstoken voor toegang tot een 'aanwijzer' toepassing verwijst of vertegenwoordiging van Azure Media Services-service voor het leveren van licenties.

We moeten registreren en de 'aanwijzer'-app in Azure AD configureren door de onderstaande stappen te volgen:

1. In de Azure AD-tenant

   * een toepassing (resource) met de aanmeldings-URL toevoegen:

   https://[resource_name].azurewebsites.NET/ en

   * App-ID-URL:

   https://[aad_tenant_name].onmicrosoft.com/[resource_name];
2. Voeg een nieuwe sleutel voor de resource-app
3. Het manifestbestand van de app bijwerken zodat de eigenschap groupMembershipClaims de volgende waarde heeft: 'groupMembershipClaims': 'All',  
4. Toevoegen in de Azure AD-app die verwijst naar de player-web-app in de sectie 'machtigingen voor andere toepassingen', de bron-app die is toegevoegd in stap 1 hierboven. Controleer onder 'gemachtigd' vinkje 'Toegang [resource_name]'. Dit geeft de web-app-machtiging voor het maken van toegangstokens voor toegang tot de bron-app. U moet dit doen voor zowel lokale als geïmplementeerde versie van de web-app als u met Visual Studio en Azure-web-app ontwikkelt.

De JWT-token dat is uitgegeven door Azure AD is daarom inderdaad het toegangstoken voor toegang tot deze resource 'aanwijzer'.

### <a name="what-about-live-streaming"></a>Wat vindt Live Streaming?
In het bovenstaande is onze bespreking is gericht op activa op aanvraag. Wat vindt live streamen?

Goed nieuws is dat u precies hetzelfde ontwerp en implementatie kunt gebruiken voor het beveiligen van live streamen in Azure Media Services door te behandelen de asset die zijn gekoppeld aan een programma als een 'VOD actief'.

Het is met name bekend dat hiervoor live streaming in Azure Media Services, moet u een kanaal en vervolgens een programma onder het kanaal maken. Voor het maken van het programma, moet u een asset met het live archief voor het programma te maken. Om te kunnen bieden CENC met multi-DRM-beveiliging van de live-inhoud, alle u moet doen, is het toepassen van dezelfde setup/verwerking naar de asset alsof het een activum' VOD' voordat u begint met het programma.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Hoe zit het licentieservers buiten Azure Media Services?
Vaak kunnen klanten hebben geïnvesteerd in licentie server-farm zich in hun eigen gegevens centreren of gehost door DRM-providers. Gelukkig Azure Media Services Content Protection kunt u in de modus hybride werken: de inhoud die worden gehost en dynamisch in Azure Media Services beveiligd terwijl DRM-licenties worden geleverd door servers buiten Azure Media Services. Er zijn in dit geval wordt de volgende overwegingen van wijzigingen:

1. De Secure Token Service moet tokens die worden geaccepteerd en kunnen worden gecontroleerd door de serverfarm licentie uitgeven. De Widevine-licentieservers geleverd door Axinom vereisen bijvoorbeeld dat een JWT-token met 'recht bericht'. Daarom moet u een STS het uitgeven van dergelijke JWT-token hebben. De auteurs een dergelijke implementatie hebt voltooid en u kunt de details vinden in het volgende document in [documentatiecentrum van Azure](https://azure.microsoft.com/documentation/): [Axinom Widevine-licenties te leveren met Azure Media Services met behulp van](media-services-axinom-integration.md).
2. U moet niet langer license levering-service (ContentKeyAuthorizationPolicy) configureren in Azure Media Services. Wat u moet doen is het bieden van de licentie overname URL's (voor PlayReady, Widevine en FairPlay) wanneer u AssetDeliveryPolicy bij het instellen van CENC met multi-DRM configureert.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Wat gebeurt er als ik wil een aangepaste STS gebruiken?
Er zijn een aantal oorzaken hebben die een klant kan een aangepaste STS (Secure Token Service) gebruiken voor het ontwikkelen van JWT-tokens. Sommige hiervan zijn:

1. De identiteit Provider (IDP) die wordt gebruikt door de klant biedt geen ondersteuning voor STS. In dit geval een aangepaste STS mogelijk een optie.
2. De klant moet mogelijk meer flexibele of strikter besturingselement in STS met van klant abonnee factureringssysteem integreren. Een operator MVPD bieden bijvoorbeeld OTT abonnee pakketten zoals premium, basic, sport, enzovoort. De operator kunt overeenkomen met de claims in een token met een abonnee pakket zodat alleen de inhoud in het juiste pakket beschikbaar wordt gesteld. In dit geval een aangepaste STS biedt de benodigde flexibiliteit en controle.

Twee moet worden gewijzigd wanneer u een aangepaste STS:

1. Bij het configureren van de service voor het leveren van licenties voor een asset die u wilt opgeven van de beveiligingssleutel gebruikt voor verificatie door de aangepaste STS (meer details hieronder) in plaats van de huidige sleutel van Azure Active Directory.
2. Wanneer een JTW-token wordt gegenereerd, een beveiligingssleutel is opgegeven in plaats van de persoonlijke sleutel van de huidige X509 certificaat in Azure Active Directory.

Er zijn twee soorten beveiligingssleutels:

1. Symmetrische sleutel: dezelfde sleutel wordt gebruikt voor zowel genereren en controleren van een JWT-token
2. Asymmetrische sleutel: een openbaar / persoonlijk sleutelpaar in een X509 certificaat met persoonlijke sleutel voor het versleutelen/genereren van een JWT-token en de openbare sleutel voor het controleren van het token wordt gebruikt.

#### <a name="tech-note"></a>Technische opmerking
Als u gebruikmaakt van .NET Framework / C# als uw platform voor ontwikkeling, het X509 certificaat dat wordt gebruikt voor asymmetrische beveiligingssleutel sleutellengte ten minste 2048 moet hebben. Dit is een vereiste van de klasse System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework. Anders worden de volgende uitzondering gegenereerd:

IDX10630: System.IdentityModel.Tokens.X509AsymmetricSecurityKey voor het ondertekenen van mag niet kleiner dan '2048-bits zijn.

## <a name="the-completed-system-and-test"></a>Het voltooide systeem en de test
We doorlopen een paar scenario's in het voltooide end-to-end-systeem zodat lezers een eenvoudige 'afbeelding' van het gedrag hebben kunnen voordat u een aanmeldingsaccount.

De webtoepassing player en de aanmelding vindt [hier](https://openidconnectweb.azurewebsites.net/).

Als wat u moet 'niet geïntegreerde' scenario: video activa gehost in Azure Media Services die zijn beide niet beveiligd of DRM beveiligde maar zonder tokenverificatie (een licentie verlenen aan degene die deze aanvragen), kunt u deze testen zonder aanmelding (door het overschakelen naar HTTP als uw videostreaming via HTTP).

Als wat u moet de geïntegreerde scenario end-to-end: video activa ligt onder de dynamische DRM-beveiliging in Azure Media Services met tokenverificatie en JWT-token wordt gegenereerd door Azure AD, moet u aan te melden.

### <a name="user-login"></a>Gebruikersaanmelding
Testen van het end-to-end geïntegreerde DRM-systeem, moet u een ' account ' gemaakt of toegevoegd.

Welk account?

Hoewel Azure oorspronkelijk alleen toegang verleende aan Microsoft-accountgebruikers, staat deze nu toegang verleend aan gebruikers van beide systemen. Dit wordt mogelijk gemaakt door alle Azure-eigenschappen de Azure AD te laten vertrouwen voor verificatie, door Azure AD bedrijfsgebruikers te laten verifiëren en door een federatieve relatie te creëren waarbij Azure AD het klantidentiteitssysteem voor Microsoft-accounts vertrouwt om klantgebruikers te verifiëren. Als gevolg hiervan kan Azure AD zowel Microsoft-gastaccounts als systeemeigen Azure AD-accounts verifiëren.

Omdat Azure AD wordt vertrouwd domein van de Microsoft-Account (MSA), kunt u alle accounts uit een van de volgende domeinen met aangepaste Azure AD-tenant en het gebruik van de account om aan te melden toevoegen:

| **Domeinnaam** | **Domein** |
| --- | --- |
| **Domein van de aangepaste Azure AD-tenant** |somename.onmicrosoft.com |
| **Bedrijfsdomein** |Microsoft.com |
| **Microsoft-Account (MSA)-domein** |Outlook.com, live.com, hotmail.com |

U kunt contact opnemen met een van de auteurs van een account gemaakt of toegevoegd voor u.

Hieronder ziet u de schermafbeeldingen van andere aanmeldingspagina's die worden gebruikt door andere domeinaccounts.

**Aangepaste Azure AD-tenant domeinaccount**: In dit geval u ziet dat de aangepaste aanmeldingspagina van het aangepaste domein van de Azure AD-tenant.

![Aangepaste Azure AD-tenant domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft-domeinaccount met smartcard**: In dit geval u ziet dat de aanmeldingspagina aangepast door Microsoft zakelijke IT-medewerkers met tweeledige verificatie.

![Aangepaste Azure AD-tenant domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-Account (MSA)**: In dit geval ziet u de aanmeldingspagina van Microsoft-Account voor consumenten.

![Aangepaste Azure AD-tenant domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Versleutelde Media-extensies gebruiken voor PlayReady
Op een moderne browser met versleuteld Media extensies (EME) voor PlayReady-ondersteuning, zoals Internet Explorer 11 op Windows 8.1 en omhoog en Microsoft Edge-browser op Windows 10 is PlayReady de onderliggende DRM voor EME.

![Met behulp van EME voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Het donkere player-gebied is vanwege het feit dat PlayReady-beveiliging wordt voorkomen dat een schermopname van beveiligde video aanbrengen.

Het volgende scherm ziet u de player-invoegtoepassingen en de muis/EME ondersteuning.

![Met behulp van EME voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME in de Microsoft Edge en Internet Explorer 11 op Windows 10 kunt aanroepen van [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) op Windows 10-apparaten die deze functie ondersteunen. PlayReady SL3000 Hiermee ontgrendelt u de stroom van de inhoud van de uitgebreide premium (4K, Kopregel, enzovoort) en nieuwe inhoud leveringsmodel (vroege venster voor verbeterde inhoud).

Richt u op de Windows-apparaten: PlayReady is de enige DRM in de hardware beschikbaar is op Windows-apparaten (PlayReady SL3000). Een streaming-service met PlayReady via EME of via een UWP-toepassing en bieden een hogere videokwaliteit met PlayReady SL3000 dan een andere DRM. Normaal gesproken loopt 2 kB inhoud via Chrome of Firefox en 4K-inhoud via Microsoft Edge/IE11 of een UWP-toepassing op hetzelfde apparaat (afhankelijk van de service-instellingen en implementatie).

#### <a name="using-eme-for-widevine"></a>Met behulp van EME voor Widevine
Google Widevine is op een moderne browser met EME/Widevine-ondersteuning, zoals Chrome 41 + op Windows 10, Windows 8.1, Mac OS x Yosemite en Chrome op Android 4.4.4 de DRM achter EME.

![Met behulp van EME voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

U ziet dat Widevine voorkomt niet dat een schermopname van beveiligde video aanbrengen.

![Met behulp van EME voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Geen recht gebruikers
Als een gebruiker is geen lid van groep 'Gebruikers het recht', de gebruiker kan niet worden doorgegeven 'recht selectievakje' en de service multi-DRM-licenties weigert de aangevraagde licentie uitgeven, zoals hieronder wordt weergegeven. De gedetailleerde beschrijving is ' licentie ophalen is mislukt ", die is ontworpen.

![Unentitled gebruikers](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Aangepaste Secure Token Service uitgevoerd
Voor het scenario van het uitvoeren van aangepaste Secure Token Service (STS), is de JWT-token uitgegeven door de aangepaste STS met zowel symmetrisch als asymmetrisch sleutel.

De aanvraag van het gebruik van de symmetrische sleutel (met behulp van Chrome):

![Aangepaste STS uitgevoerd](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

De aanvraag van het gebruik van asymmetrische sleutel via een X509 van het certificaat (met behulp van Microsoft moderne browser).

![Aangepaste STS uitgevoerd](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

In beide bovenstaande gevallen wordt blijft verificatie van de gebruiker hetzelfde – via Azure AD. Het enige verschil is dat de JWT-tokens worden uitgegeven door de aangepaste STS in plaats van Azure AD. Bij het configureren van dynamische CENC beveiliging, de beperking van de service voor het leveren van licenties geeft het type van JWT-token zowel symmetrisch als asymmetrisch sleutel.

## <a name="summary"></a>Samenvatting
In dit document besproken CENC met meerdere native-DRM en toegangsbeheer via tokenverificatie: het ontwerp en de uitvoering ervan met behulp van Azure, Azure Media Services en Azure Media Player.

* Een referentie-ontwerp wordt weergegeven waarin de vereiste onderdelen in een subsysteem DRM/CENC;
* De implementatie van een verwijzing op Azure, Azure Media Services en Azure Media Player.
* Sommige onderwerpen die direct betrokken zijn bij het ontwerp en de implementatie worden ook besproken.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
