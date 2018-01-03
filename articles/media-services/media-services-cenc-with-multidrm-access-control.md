---
title: 'CENC met multi-DRM en toegangsbeheer: een verwijzing naar ontwerpen en implementeren op Azure en Azure Media Services | Microsoft Docs'
description: Meer informatie over een licentie voor Microsoft Smooth Streaming Client overdragen Kit.
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
ms.openlocfilehash: cac1513d805e01f2c9633b3b318ad6808027904e
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC met multi-DRM en toegangsbeheer: een verwijzing naar ontwerpen en implementeren op Azure en Azure Media Services
 
## <a name="introduction"></a>Inleiding
Ontwerpen en bouwen van een digitale rechten (DRM) management subsysteem voor een boven de hoogste (OTT) of online streaming oplossing is een complexe taak. Operators/online uitbesteden video providers doorgaans deze taak voor gespecialiseerde DRM serviceproviders. Het doel van dit document is een verwijzing naar ontwerpen en implementeren van een end-to-end DRM-subsysteem in een OTT of online-streaming oplossing presenteren.

De betreffende lezers voor dit document zijn engineers die in DRM subsystemen met OTT of online streaming/Graphic; oplossingen of -lezers die geïnteresseerd in DRM subsystemen zijn werken. De veronderstelling is dat lezers bekend met ten minste één van de technologieën DRM op de markt, zoals PlayReady en Widevine, FairPlay en Adobe Access bent.

In deze discussie van DRM opnemen we ook common encryption (CENC) met multi-DRM. Een belangrijke trend in online streaming en de branche OTT is het gebruik van CENC met meerdere systeemeigen DRM op verschillende clientplatforms. Deze trend is een verschuiving van de vorige versie die een enkele DRM en de client SDK voor verschillende clientplatforms gebruikt. Wanneer u CENC met multi systeemeigen DRM gebruikt, zowel PlayReady als Widevine zijn versleuteld volgens de [Common Encryption (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) specificatie.

De voordelen van CENC met multi-DRM zijn dat deze:

* Versleuteling kosten vermindert omdat een versleutelingsproces één wordt gebruikt om verschillende doelplatforms met de systeemeigen DRM's.
* Beperkt de kosten van het versleutelde activa te beheren omdat slechts één exemplaar van de versleutelde activa nodig is.
* Elimineert licentieverlening kosten, omdat de systeemeigen DRM-client meestal vrije ruimte op het systeemeigen platform is DRM-client.

Microsoft is een actieve organisator van deze actie van DASH en CENC samen met enkele belangrijke zakelijke spelers. Azure Media Services biedt ondersteuning voor DASH en CENC. Zie de volgende blogs voor recente aankondigingen:

*  [Google Widevine-services voor het leveren van licenties in Azure Media Services aankondigen](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
* [Azure Media Services wordt toegevoegd voor Google Widevine-verpakking voor het leveren van een multi-DRM-stream](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)  

### <a name="overview-of-this-article"></a>Overzicht van dit artikel
De doelstellingen van dit artikel zijn naar:

* Geef een referentie-ontwerp van een DRM-subsysteem die gebruikmaakt van CENC met multi-DRM.
* Geef een referentie-implementatie op een Azure/Media Services-platform.
* Bespreek sommige onderwerpen ontwerpen en implementeren.

De term 'multi-DRM' in het artikel bevat informatie over de volgende producten:

* Microsoft PlayReady
* Google Widevine
* FairPlay van Apple 

De volgende tabel geeft een overzicht van de systeemeigen platform/systeemeigen app en browsers die door elke DRM ondersteund.

| **Clientplatform** | **Systeemeigen DRM-ondersteuning** | **Browser-app** | **Streaming-indelingen** |
| --- | --- | --- | --- |
| **Smart tv, operator STB's, OTT STB 's** |PlayReady voornamelijk, en/of Widevine en/of andere |Linux, Opera, WebKit, andere |Verschillende indelingen |
| **Windows 10-apparaten (Windows-PC, Windows-tablets, Windows Phone, Xbox)** |PlayReady |MS IE11-Edge/EME<br/><br/><br/>Universele Windows-Platform |STREEPJE (voor HLS, PlayReady wordt niet ondersteund)<br/><br/>DASH, Smooth Streaming (voor HLS, PlayReady wordt niet ondersteund) |
| **Android-apparaten (telefoon, tablet, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X-clients en Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


Gezien de huidige status van de implementatie voor elke DRM van een service doorgaans wil implementeren twee of drie DRM's om ervoor te zorgen dat u de typen van eindpunten in de beste manier oplossen.

Er is een afweging tussen de complexiteit van de logica van de service en de complexiteit bij de client tot een bepaalde mate van de gebruikerservaring op verschillende clients.

Houd er rekening mee zodat uw selectie:

* PlayReady is systeemeigen geïmplementeerd in elke Windows-apparaat op een Android-apparaten en beschikbaar via software-SDK's op vrijwel elk platform.
* Widevine is systeemeigen geïmplementeerd in elke Android-apparaat, Chrome en sommige andere apparaten.
* FairPlay is beschikbaar op iOS en Mac OS-clients of via iTunes.

Er zijn twee opties voor een typische multi-DRM:

* PlayReady en Widevine
* PlayReady en Widevine FairPlay

## <a name="a-reference-design"></a>Een referentie-ontwerp
Deze sectie bevat een verwijzing ontwerp dat is agnostisch ten opzichte van de technologieën die wordt gebruikt om dit te implementeren.

Een DRM-subsysteem kan de volgende onderdelen bevatten:

* Sleutelbeheer
* DRM verpakking
* Levering van DRM-licentie
* Recht controleren
* Verificatie/autorisatie
* Speler
* Oorsprong/content delivery network (CDN)

Het volgende diagram illustreert de op hoog niveau interactie tussen de onderdelen in een DRM-subsysteem:

![DRM-subsysteem met CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Het ontwerp heeft drie basic lagen:

* Een back-office-laag (zwart) is niet extern worden weergegeven.
* Een laag DMZ (donker blauw) bevat alle eindpunten die toegankelijk zijn vanaf het publiek.
* Een openbare internetlaag (lichte blauw) bevat het CDN en spelers met verkeer via het openbare internet.

Ook moet er een inhoudsbeheer-hulpprogramma voor het besturingselement DRM-beveiliging, ongeacht of het statische of dynamische versleuteling. De invoer voor DRM-codering zijn onder andere:

* MBR video-inhoud
* Inhoudssleutel
* URL's aanschaf van licentie

Dit is de stroom op hoog niveau tijdens het afspelen van:

* De gebruiker is geverifieerd.
* Een verificatietoken is gemaakt voor de gebruiker.
* DRM beveiligde inhoud (manifest) wordt gedownload naar de speler.
* Windows media player verzendt een aanvraag voor het aanschaf van licentie met licentieservers samen met een sleutel-ID en een verificatietoken.

Het volgende hoofdstuk beschrijft uitgebreid het ontwerp van Sleutelbeheer.

| **ContentKey naar asset** | **Scenario** |
| --- | --- |
| 1-op-1 |Het meest eenvoudige geval. Dit biedt de beste controle. Maar resulteert deze benadering in het algemeen in de hoogste kosten van de licentie-levering. Ten minste is één licentie vereist voor alle beveiligde activa. |
| 1-op-veel |U kunt dezelfde inhoudssleutel voor meerdere activa. Bijvoorbeeld, voor alle activa in een logische groep, kunt zoals een genre of de subset van een genre (of film gen), u een enkele inhoudssleutel. |
| Veel-op-1 |Meerdere inhoudssleutels nodig zijn voor elk actief. <br/><br/>Als u moet de beveiliging van dynamische CENC met multi-DRM voor MPEG DASH en dynamische AES-128-versleuteling voor HLS wilt toepassen, moet u bijvoorbeeld twee afzonderlijke inhoud sleutels. Elke sleutel moet een eigen ContentKeyType. (Voor de inhoudssleutel gebruikt voor dynamische CENC beveiliging, de ContentKeyType.CommonEncryption gebruiken. Voor de inhoudssleutel voor dynamische AES-128-versleuteling wordt gebruikt, gebruik ContentKeyType.EnvelopeEncryption.)<br/><br/>Als een ander voorbeeld kunt CENC beveiliging DASH-inhoud, in theorie, u een inhoudssleutel de videostream en een andere inhoud sleutel voor het beveiligen van de audiostroom te beschermen. |
| Veel-op-veel |De combinatie van de vorige twee scenario's. Een set van inhoud sleutels wordt gebruikt voor elk van de meerdere elementen in de groep met dezelfde activa. |

Een andere belangrijke factor in overweging moet nemen is het gebruik van permanente en niet-persistente licenties.

Waarom deze overwegingen belangrijk zijn?

Als u een openbare cloud voor de levering van de licentie gebruikt, hebben permanent en niet-persistente licenties een directe invloed op licentie leveringskosten. De volgende twee verschillende ontwerp gevallen dienen ter illustratie van:

* Maandabonnement: gebruik een permanente licentie en de toewijzing voor inhoud sleutel aan asset 1-op-veel. Bijvoorbeeld, voor alle de kinderen films gebruiken we een enkele inhoudssleutel voor versleuteling. In dit geval:

    Totaal aantal licenties die zijn aangevraagd voor alle kinderen films/apparaat = 1

* Maandabonnement: gebruik een niet-persistente licentie en 1 op 1 toewijzing tussen inhoudssleutel en asset. In dit geval:

    Totaal aantal licenties die zijn aangevraagd voor alle kinderen films/apparaat = [aantal films gevolgde] x [aantal sessies]

De twee verschillende ontwerpen resulteren in zeer andere licentiegroepen aanvraag patronen. De verschillende patronen ertoe leiden dat andere licentiegroepen levering kosten als service voor het leveren van licenties is opgegeven door een openbare cloud zoals Media Services.

## <a name="map-design-to-technology-for-implementation"></a>Ontwerp worden toegewezen aan de technologie voor implementatie
Vervolgens wordt is het ontwerp van de algemene toegewezen aan technologieën op het platform Azure/Media Services door te geven welke technologie die u wilt gebruiken voor elke bouwsteen.

De volgende tabel ziet de toewijzing.

| **Bouwstenen** | **Technologie** |
| --- | --- |
| **Player** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Id-provider (IDP)** |Azure Active Directory (Azure AD) |
| **Beveiligingstokenservice (STS)** |Azure AD |
| **Werkstroom van DRM-beveiliging** |Dynamische Media Services-beveiliging |
| **Levering van DRM-licentie** |* Media Services-licentie leveringsmethode (PlayReady, Widevine, FairPlay) <br/>* Axinom licentieserver <br/>* Aangepaste PlayReady-licentieserver |
| **Oorsprong** |Media Services-streaming-eindpunt |
| **Sleutelbeheer** |Niet nodig zijn voor de referentie-implementatie |
| **Inhoudsbeheer** |Een C#-consoletoepassing |

Met andere woorden, worden zowel IDP als STS gebruikt met Azure AD. De [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/) wordt gebruikt voor Windows media player. Ondersteunen zowel Media Services en mediaspeler DASH en CENC met multi-DRM.

Het volgende diagram toont de algemene structuur en stroom met de vorige technologie-toewijzing:

![CENC op mediaservices](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Als u dynamische versleuteling van CENC instelt, wordt het hulpprogramma voor inhoud gebruikt de volgende invoer:

* Open inhoud
* Inhoudssleutel uit de sleutel genereren/management
* URL's aanschaf van licentie
* Een lijst met gegevens van Azure AD

Dit is de uitvoer van het hulpprogramma voor inhoudsbeheer:

* ContentKeyAuthorizationPolicy bevat de specificatie op hoe licentie levering controleert of een JSON Web Token (JWT) en DRM-licentie-specificaties.
* AssetDeliveryPolicy bevat specificaties op het streaming-indeling, DRM-beveiliging en URL's aanschaf van licentie.

Dit is de stroom tijdens runtime:

* Bij verificatie van de gebruiker wordt een JWT gegenereerd.
* Een van de claims in de JWT is een claim voor groepen die het groepsobject ID EntitledUserGroup bevat. Deze claim wordt gebruikt voor het doorgeven van de controle van rechten.
* Windows media player het manifest van de client van CENC beveiligde inhoud wordt gedownload en identificeert het volgende:
   * Sleutel-ID.
   * De inhoud is CENC beveiligd.
   * URL's aanschaf van licentie.
* Windows media player wordt een licentieaanvraag verkrijgen op basis van de browser/DRM ondersteund. In de licentie overname aanvraag, de sleutel-ID en de JWT worden ook verzonden. De service voor het leveren van licenties controleert of de JWT en de claims voordat deze de vereiste licentie uitgeeft.

## <a name="implementation"></a>Implementatie
### <a name="implementation-procedures"></a>Procedures voor implementatie
Implementatie omvat de volgende stappen uit:

1. Bereid test activa. Een test die video naar een multi-bitrate coderen/pakket gefragmenteerd MP4 in Media Services. Deze actief is *niet* DRM beveiligde. DRM-beveiliging wordt gedaan door dynamische beveiliging later.

2. Maak een sleutel-ID en een inhoudssleutel (eventueel van een sleutel seed). In dit geval wordt de sleutelbeheersysteem is niet nodig omdat slechts één sleutel-ID en sleutel zijn vereist voor een aantal test activa.

3. Gebruik het Media Services-API om multi-DRM licentie levering van services voor de test-asset te configureren. Als u aangepaste licentieservers door uw bedrijf of leveranciers van uw bedrijf in plaats van de licentie-services in een Media Services gebruikt, kunt u deze stap overslaan. Wanneer u een licentie levering configureert, kunt u licentie overname URL's in de stap. De API voor Media Services is nodig om op te geven van bepaalde gedetailleerde configuraties, zoals autorisatie beleidsbeperking en licentie antwoord sjablonen voor services van andere DRM-licenties. Op dit moment biedt de Azure-portal niet de benodigde gebruikersinterface voor deze configuratie. Zie voor informatie van de API-niveau en voorbeeldcode [gebruik PlayReady en/of Widevine dynamic common encryption](media-services-protect-with-playready-widevine.md).

4. De Media Services-API gebruiken voor het configureren van het leveringsbeleid voor Assets voor de test-asset. Zie voor informatie van de API-niveau en voorbeeldcode [gebruik PlayReady en/of Widevine dynamic common encryption](media-services-protect-with-playready-widevine.md).

5. Maken en configureren van een Azure AD-tenant in Azure.

6. Maak een paar gebruikersaccounts en groepen in uw Azure AD-tenant. Ten minste een 'Gebruiker het recht'-groep maken en een gebruiker toevoegen aan deze groep. Gebruikers in deze groep doorgeven de controle van de rechten in licentie overname. Gebruikers die niet in deze groep geeft de controle van de verificatie is mislukt en kunnen niet geen licentie. Lidmaatschap van deze groep 'Gebruiker het recht' is een claim vereiste groepen in de JWT uitgegeven door Azure AD. U kunt deze vereiste claim in de stap opgeven bij het configureren van multi-DRM-services voor het leveren van licenties.

7. Een ASP.NET MVC-app voor het hosten van uw video player maken. Deze ASP.NET-app is beveiligd met gebruikersverificatie met Azure AD-tenant. Juiste claims worden opgenomen in de toegangstokens verkregen na verificatie van gebruiker. We raden aan OpenID Connect API voor deze stap. De volgende NuGet-pakketten installeren:

   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Install-Package Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

8. Een speler maken met behulp van de [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/). Gebruik de [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) om op te geven welke DRM-technologie gebruiken op verschillende DRM-platforms.

9. De volgende tabel toont de testmatrix.

    | **DRM** | **Browser** | **Resultaat van het gebruiksrecht heeft gebruiker** | **Resultaat van unentitled gebruiker** |
    | --- | --- | --- | --- |
    | **PlayReady** |Microsoft Edge of Internet Explorer 11 op Windows 10 |Voltooid |Mislukt |
    | **Widevine** |Chrome op Windows 10 |Voltooid |Mislukt |
    | **FairPlay** |NOG TE BEPALEN | | |

Zie voor meer informatie over het instellen van Azure AD voor een ASP.NET MVC-speler app [een Azure Media Services OWIN MVC-gebaseerde app integreren met Azure Active Directory en beperken van de belangrijkste inhouddistributie op basis van claims JWT](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Zie voor meer informatie [JWT-token verificatie in Azure Media Services en dynamische versleuteling](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).  

Voor informatie over Azure AD:

* U vindt informatie voor ontwikkelaars in de [ontwikkelaarshandleiding Azure Active Directory](../active-directory/active-directory-developers-guide.md).
* U vindt informatie voor beheerders in [uw Azure AD-tenant-directory beheren](../active-directory/active-directory-administer.md).

### <a name="some-issues-in-implementation"></a>Sommige problemen in uitvoering
Gebruik de volgende informatie voor probleemoplossing voor hulp bij problemen met implementatie.

* De verlener URL met eindigen moet '/'. De doelgroep moet de player client-id op. Ook toe te voegen "/" aan het einde van de uitgevers-URL.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    In de [JWT Decoder](http://jwt.calebb.net/), ziet u **aud** en **iss**, zoals weergegeven in de JWT:

    ![JWT](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)

* Machtigingen voor de toepassing in Azure AD toevoegen aan de **configureren** tabblad van de toepassing. Machtigingen zijn vereist voor elke toepassing, zowel lokaal als geïmplementeerde versie.

    ![Machtigingen](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)

* Gebruik de juiste verlener wanneer u dynamische CENC beveiliging instelt.

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    De volgende werkt niet:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    De GUID is de Azure AD-tenant-ID. De GUID vindt u in de **eindpunten** pop-upmenu van de Azure-portal.

* Verleen het lidmaatschap van claims van bevoegdheden. Zorg dat het volgende in het manifestbestand voor Azure AD-toepassing: 

    'groupMembershipClaims': 'Alle' (de standaardwaarde is null)

* Stel de juiste TokenType bij het maken van beperking vereisten.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Omdat u ondersteuning toevoegen voor JWT (Azure AD) naast SWT (ACS), is de standaardwaarde TokenType TokenType.JWT. Als u SWT/ACS gebruikt, moet u het token instellen op TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Aanvullende onderwerpen voor implementatie
Deze sectie worden enkele aanvullende onderwerpen in de ontwerp- en implementatiestappen behandeld.

### <a name="http-or-https"></a>HTTP of HTTPS?
De ASP.NET MVC-toepassing player moet ondersteunen het volgende:

* Gebruikersverificatie via Azure AD, die zich onder HTTPS.
* JWT-uitwisseling tussen de client en de Azure AD, die zich onder HTTPS.
* DRM licentie overname door de client, die onder HTTPS zijn ingesteld moet als de levering van de licentie wordt geleverd door Media Services. De productpakket PlayReady verplichten niet HTTPS voor de levering van licentie. Als de licentieserver PlayReady buiten Media Services, kunt u HTTP of HTTPS.

De ASP.NET-toepassing player maakt gebruik van HTTPS als een best practice zodat mediaspeler op een HTTPS-pagina. Echter, HTTP verdient de voorkeur voor streaming, dus u moet rekening houden met de uitgifte van gemengde inhoud.

* Gemengde inhoud toegestaan niet in de browser. Maar invoegtoepassingen zoals Silverlight en de invoegtoepassing voor OSMF vloeiend en streepje toe te staan. Gemengde inhoud is een beveiligingsprobleem vanwege de bedreiging van de mogelijkheid om te injecteren schadelijke JavaScript, waardoor de klantgegevens risico's kunnen lopen. Browsers deze mogelijkheid standaard geblokkeerd. De enige manier om dit oplossen door het is aan de serverzijde (oorsprong) doordat alle domeinen (onafhankelijk van HTTPS of HTTP). Dit is waarschijnlijk niet een goed idee ofwel.
* Vermijd gemengde inhoud. De spelertoepassing en de mediaspeler moet HTTP of HTTPS gebruiken. Tijdens het afspelen van gemengde inhoud wordt in de tech silverlightSS moet u een waarschuwing gemengde inhoud uitschakelen. De technische flashSS verwerkt gemengde inhoud zonder een waarschuwing gemengde inhoud.
* Als uw streaming-eindpunt is gemaakt voordat augustus 2014, niet HTTPS wordt ondersteund. In dit geval maken en gebruiken van een nieuwe streaming-eindpunt voor HTTPS.

In de implementatie van de referentie voor DRM beveiligde inhoud, de toepassing en de streaming zijn onder HTTPS. Windows media player komt niet voor inhoud openen moet verificatie of een licentie, zodat u kunt via HTTP of HTTPS gebruiken.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory sleutelrollover ondertekenen
Sleutelrollover ondertekening is een belangrijk punt om rekening in uw implementatie. Als u deze negeert, het systeem klaar uiteindelijk werkt niet volledig binnen zes weken maximaal.

Azure AD gebruikt industrienormen om een vertrouwensrelatie tussen zichzelf en toepassingen die gebruikmaken van Azure AD. Azure AD gebruikt met name een ondertekeningssleutel die uit een openbare en persoonlijke sleutelpaar bestaat. Azure AD maakt een beveiligingstoken dat informatie over de gebruiker bevat, zijn ondertekend door Azure AD met een persoonlijke sleutel voordat deze terug naar de toepassing wordt verzonden. Om te verifiëren dat het token geldig en oorsprong van Azure AD is, moet de toepassing van het token handtekening valideren. De toepassing maakt gebruik van de openbare sleutel die worden weergegeven door Azure AD dat is opgenomen in het document met federatieve metagegevens van de tenant. Deze openbare sleutel en de ondertekeningssleutel waarvan deze is afgeleid, is hetzelfde voor alle tenants worden gebruikt in Azure AD.

Zie voor meer informatie over Azure AD-sleutelrollover [belangrijke informatie over het ondertekenen van sleutelrollover in Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Tussen de [openbaar / persoonlijk sleutelpaar](https://login.microsoftonline.com/common/discovery/keys/):

* De persoonlijke sleutel wordt gebruikt door Azure AD voor het genereren van een JWT.
* De openbare sleutel wordt gebruikt door een toepassing zoals DRM licentie levering van services in een Media Services om te controleren of de JWT.

Azure AD draait om veiligheidsredenen periodiek het certificaat (elke zes weken). In het geval van inbreuk op de beveiliging, kan de sleutelrollover elk gewenst moment optreden. Daarom moeten de services voor het leveren van licenties in Media Services bijwerken van de openbare sleutel gebruikt als Azure AD het sleutelpaar draait. Anders token in Media Services-verificatie is mislukt en er is geen licentie wordt verleend.

Als u deze service instelt, instellen u TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument bij het configureren van DRM-services voor het leveren van licenties.

Hier volgt de JWT-stroom:

* Azure AD geeft de JWT met de huidige persoonlijke sleutel voor een geverifieerde gebruiker.
* Wanneer u een speler ziet een CENC met multi-DRM beveiligde inhoud, zoekt deze eerst de JWT uitgegeven door Azure AD.
* Windows media player verzendt een aanvraag voor de aanschaf van licentie met de JWT naar services voor het leveren van licenties in Media Services.
* De huidige/geldige openbare sleutel van Azure AD de services voor het leveren van licenties in Media Services gebruiken om te controleren of de JWT alvorens licenties.

DRM-services voor het leveren van licenties controleren altijd voor de huidige/geldige openbare sleutel van Azure AD. De openbare sleutel die wordt aangeboden door Azure AD is de sleutel die wordt gebruikt om te controleren van een JWT uitgegeven door Azure AD.

Wat gebeurt er als de sleutelrollover gebeurt nadat Azure AD een JWT genereert maar voordat de JWT wordt verzonden door spelers op DRM licentie levering van services in een Media Services voor verificatie?

Omdat een sleutel kan te allen tijde worden vernieuwd, is meer dan één geldige openbare sleutel altijd beschikbaar zijn in het document met federatieve metagegevens. Media Services-licentie levering kunt u elk van de sleutels die zijn opgegeven in het document. Omdat één sleutel kan snel worden teruggedraaid, een ander mogelijk worden de vervanging ervan, enzovoort.

### <a name="where-is-the-access-token"></a>Waar bevindt zich het toegangstoken?
Als u kijken hoe een web-app een API-app onder aanroept [toepassingsidentiteit met OAuth 2.0-client referenties grant](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), de authenticatiestroom is als volgt:

* Een gebruiker zich aanmeldt bij Azure AD in de webtoepassing. Zie voor meer informatie [webbrowser webtoepassing](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
* Het eindpunt van de autorisatie Azure AD leidt de gebruikersagent terug naar de client-toepassing met een autorisatiecode. De gebruikersagent retourneert de autorisatiecode op de clienttoepassing omleidings-URI.
* De webtoepassing moet te verkrijgen van een toegangstoken zodat deze kan worden geverifieerd met de web-API en ophalen van de gewenste resource. Het doet een aanvraag voor het eindpunt van het Azure AD-token en biedt de referentie, client-ID en web-API van toepassings-ID-URI. Dit geeft de autorisatiecode om aan te tonen dat de gebruiker toestemming gegeven deze.
* Azure AD verifieert de toepassing en retourneert een JWT-toegangstoken dat wordt gebruikt voor de web-API aanroepen.
* Het geretourneerde JWT-toegangstoken worden in de webtoepassing via HTTPS gebruikt de JWT-tekenreeks met een aanduiding 'Bearer' in de header 'Autorisatie' van de aanvraag toevoegen aan de web-API. De web-API wordt vervolgens de JWT valideert. Als validatie geslaagd is, wordt de gewenste resource.

In deze stroom van de identiteit toepassing vertrouwt de web-API of de webtoepassing de gebruiker heeft geverifieerd. Om deze reden wordt dit patroon van een vertrouwde subsysteem genoemd. De [autorisatie stroomdiagram](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) wordt beschreven hoe authorization-code-grant stroom werkt.

Licentie verkrijgen met de tokenbeperking volgt hetzelfde patroon vertrouwde subsysteem. De service voor het leveren van licenties in Media Services is de web-API-resource, of de 'back-end resource' die een webtoepassing toegang moet hebben. Waar is dus het toegangstoken?

Het toegangstoken is verkregen van Azure AD. Na een geslaagde verificatie wordt een autorisatiecode geretourneerd. De autorisatiecode wordt vervolgens gebruikt, samen met de client-ID en de app-sleutel voor het uitwisselen van voor het toegangstoken. Het toegangstoken is gebruikt voor toegang tot een 'aanwijzer'-toepassing die verwijst naar of vertegenwoordigt de Media Services-service voor het leveren van licenties.

Als u wilt registreren en de aanwijzer-app in Azure AD configureren, moet u de volgende stappen uitvoeren:

1. In de Azure AD-tenant:

   * Een toepassing (resource) met de URL aanmelding https://[resource_name].azurewebsites.net/ toevoegen. 
   * Toevoegen van een app-ID met de URL-https://[aad_tenant_name].onmicrosoft.com/[resource_name].

2. Voeg een nieuwe sleutel voor de resource-app.

3. Het manifestbestand van de app bijwerken zodat de eigenschap groupMembershipClaims de waarde 'groupMembershipClaims heeft': 'Alle'.

4. In de Azure AD-app die naar de player web-app in de sectie verwijst **machtigingen voor andere toepassingen**, de bron-app die is toegevoegd in stap 1 toevoegen. Onder **overgedragen machtigingen**, selecteer **toegang [resource_name]**. Deze optie zorgt ervoor dat de web-app-machtiging voor het maken van toegangstokens die toegang de bron-app tot. Als u met Visual Studio en de Azure-web-app ontwikkelt, moet u dit doen voor de lokale en geïmplementeerde versie van de web-app.

De JWT uitgegeven door Azure AD is het toegangstoken dat wordt gebruikt voor toegang tot de bron van de wijzer.

### <a name="what-about-live-streaming"></a>Wat vindt live streamen?
De bespreking van de vorige gericht op activa op aanvraag. Wat vindt live streamen?

U kunt precies dezelfde ontwerp en implementatie beveiligen live streaming in Media Services door de asset die zijn gekoppeld aan een programma als VOD activa behandelen.

In het bijzonder om live te streamen in Media Services, moet u een kanaal maken en vervolgens een programma onder het kanaal te maken. Voor het maken van het programma, moet u een asset met het live archief voor het programma te maken. Toepassing de dezelfde setup/verwerking voor CENC met multi-DRM-beveiliging van de live-inhoud voor de activa alsof deze een activum VOD voordat u begint met het programma.

### <a name="what-about-license-servers-outside-media-services"></a>Hoe zit het licentieservers buiten Media Services?
Klanten geïnvesteerd vaak in een serverfarm licentie zich in hun eigen Datacenter of een gehost door DRM-providers. Met Media Services Content Protection, kunt u gebruiken in de hybride-modus. Inhoud kunnen worden gehost en dynamisch in Media Services beveiligd terwijl DRM-licenties worden geleverd door servers buiten Media Services. Houd rekening met de volgende wijzigingen in dit geval:

* STS moet tokens die worden geaccepteerd en kunnen worden gecontroleerd door de serverfarm licentie uitgeven. De Widevine-licentieservers geleverd door Axinom vereisen bijvoorbeeld dat een specifieke JWT die een recht-bericht bevat. Daarom moet u een STS uitgeven van een dergelijke JWT hebben. Voor informatie over dit type implementatie, gaat u naar de [documentatiecentrum van Azure](https://azure.microsoft.com/documentation/) en Zie [gebruik Axinom Widevine-licenties te leveren met Azure Media Services](media-services-axinom-integration.md).
* U moet niet langer license levering-service (ContentKeyAuthorizationPolicy) configureren in een Media Services. U moet de licentievoorwaarden overname URL's opgeven (voor PlayReady, Widevine en FairPlay) wanneer u AssetDeliveryPolicy voor het instellen van CENC met multi-DRM configureert.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Wat gebeurt er als ik wil een aangepaste STS gebruiken?
Een klant kunt kiezen om een aangepaste STS gebruiken voor het JWTs. Redenen zijn:

* STS biedt geen ondersteuning voor de IDP gebruikt door de klant. In dit geval een aangepaste STS mogelijk een optie.
* De klant mogelijk meer flexibele strikter besturingselement of STS integreren met de klant abonnee factureringssysteem. Bijvoorbeeld, een operator MVPD biedt mogelijk meerdere OTT abonnee pakketten, zoals premium, basic, en sport. De operator wilt overeenkomen met de claims in een token met een abonnee pakket zodat alleen de inhoud in een specifiek pakket beschikbaar zijn gemaakt. In dit geval een aangepaste STS biedt de benodigde flexibiliteit en controle.

Wanneer u een aangepaste STS, moet twee worden gewijzigd:

* Wanneer u de service voor het leveren van licenties voor een asset configureert, moet u de beveiligingssleutel gebruikt voor verificatie door de aangepaste STS in plaats van de huidige sleutel van Azure AD opgeven. (Volg meer informatie.) 
* Wanneer een JTW-token wordt gegenereerd, een beveiligingssleutel is opgegeven in plaats van de persoonlijke sleutel van de huidige X509 certificaat in Azure AD.

Er zijn twee soorten beveiligingssleutels:

* Symmetrische sleutel: dezelfde sleutel te genereren en te controleren van een JWT wordt gebruikt.
* Asymmetrische sleutel: een openbaar / persoonlijk sleutelpaar in een X509 certificaat wordt gebruikt met een persoonlijke sleutel voor het versleutelen/genereren van een JWT en met de openbare sleutel om te controleren of het token.

> [!NOTE]
> Als u gebruikmaakt van .NET Framework / C# als uw platform voor ontwikkeling, het X509 certificaat dat wordt gebruikt voor een asymmetrische beveiligingssleutel moet een sleutellengte van ten minste 2048 hebben. Dit is een vereiste van de klasse System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework. Anders wordt is de volgende uitzondering opgetreden:

> IDX10630: System.IdentityModel.Tokens.X509AsymmetricSecurityKey voor het ondertekenen van mag niet kleiner dan '2048-bits zijn.

## <a name="the-completed-system-and-test"></a>Het voltooide systeem en de test
Deze sectie wordt u begeleid bij de volgende scenario's in het voltooide end-to-end-systeem zodat u een eenvoudige beeld van het gedrag hebben kunt voordat u een account krijgen:

* Als u een niet-geïntegreerde scenario moet:

    * Ofwel niet zijn beveiligd of DRM beveiligde maar zonder tokenverificatie (een licentie verlenen aan degene die deze heeft aangevraagd), kunt u deze testen zonder de aanmelding voor video activa in Media Services die worden gehost. Overschakelen naar HTTP als uw videostreaming via HTTP.

* Als u een end-to-end geïntegreerde scenario moet:

    * Voor video activa onder dynamische DRM-beveiliging in Media Services met de tokenverificatie en JWT die worden gegenereerd door Azure AD, moet u aan te melden.

Zie voor de webtoepassing player en de aanmeldingspagina [deze website](https://openidconnectweb.azurewebsites.net/).

### <a name="user-sign-in"></a>Gebruikersaanmelding
Als u wilt testen van het end-to-end geïntegreerde DRM-systeem, moet u een account gemaakt of toegevoegd.

Welk account?

Hoewel Azure oorspronkelijk alleen toegang verleende aan Microsoft-accountgebruikers, wordt toegang nu aan gebruikers van beide systemen toegestaan. Alle Azure-eigenschappen vertrouwt nu Azure AD voor verificatie en Azure AD verifieert gebruikers organisatie. Een federatieve relatie is gemaakt waarbij Azure AD vertrouwt voor het Microsoft-account klantidentiteitssysteem om klantgebruikers te verifiëren. Azure AD kan als gevolg hiervan gastaccounts Microsoft ook als systeemeigen Azure AD-accounts verifiëren.

Omdat Azure AD het domein van Microsoft-account vertrouwt, kunt u alle accounts uit een van de volgende domeinen met aangepaste Azure AD-tenant en het gebruik van de account aan te melden toevoegen:

| **Domeinnaam** | **Domein** |
| --- | --- |
| **Domein van de aangepaste Azure AD-tenant** |somename.onmicrosoft.com |
| **Bedrijfsdomein** |Microsoft.com |
| **Domein van Microsoft-account** |Outlook.com, live.com, hotmail.com |

U kunt contact opnemen met een van de auteurs van een account gemaakt of toegevoegd voor u.

De volgende schermafbeeldingen weergeven verschillende aanmeldingspagina's die worden gebruikt door andere domeinaccounts:

**Aangepaste Azure AD-tenant domeinaccount**: domein van de aangepaste aanmeldingspagina van de aangepaste Azure AD-tenant.

![Aangepaste Azure AD-tenant domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft-domeinaccount met smartcard**: aangepast door Microsoft zakelijke pagina IT-medewerkers met tweeledige verificatie.

![Aangepaste Azure AD-tenant domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-account**: de aanmeldingspagina van de Microsoft-account voor consumenten.

![Aangepaste Azure AD-tenant domeinaccount](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="use-encrypted-media-extensions-for-playready"></a>Versleutelde Media-extensies voor PlayReady gebruiken
Op een moderne browser met versleuteld Media extensies (EME) voor PlayReady-ondersteuning, zoals Internet Explorer 11 op Windows 8.1 of hoger en Microsoft Edge-browser op Windows 10 is PlayReady de onderliggende DRM voor EME.

![Gebruik EME voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Het donkere player-gebied is omdat PlayReady beveiliging voorkomt u dat een schermopname maken van een beveiligde video aanbrengen.

De volgende schermafbeelding ziet u de player-invoegtoepassingen en Microsoft Security Essentials (muis) / EME ondersteunen:

![Player-invoegtoepassingen voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME in Microsoft Edge en Internet Explorer 11 op Windows 10 kunt [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) moet worden aangeroepen op Windows 10-apparaten die deze functie ondersteunen. PlayReady SL3000 Hiermee ontgrendelt u de stroom van uitgebreide premium-inhoud (4K-Kopregel) en nieuwe inhoud leveringsmodel (voor verbeterde inhoud).

Als u wilt richten op de Windows-apparaten, is PlayReady de enige DRM in de hardware beschikbaar is op Windows-apparaten (PlayReady SL3000). Een streaming-service met PlayReady via EME of via een toepassing universele Windows-Platform en bieden een hogere videokwaliteit door PlayReady SL3000 dan een andere DRM. Normaal gesproken inhoud maximaal 2 K loopt via Chrome of Firefox en inhoud van 4 K stroomt via Microsoft Edge/Internet Explorer 11 of een Universal Windows Platform-toepassing op hetzelfde apparaat. Het bedrag is afhankelijk van service-instellingen en implementatie.

#### <a name="use-eme-for-widevine"></a>Gebruik EME voor Widevine
Google Widevine is op een moderne browser met EME/Widevine-ondersteuning, zoals Chrome 41 + op Windows 10, Windows 8.1, Mac OS x Yosemite en Chrome op Android 4.4.4 de DRM achter EME.

![Gebruik EME voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Widevine die niet voorkomen dat u een schermopname maken van een beveiligde video maken.

![Player-invoegtoepassingen voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="unentitled-users"></a>Unentitled gebruikers
Als een gebruiker geen lid is van de groep 'Gebruikers het recht', overgedragen niet de gebruiker de controle recht. De service multi-DRM-licenties wordt vervolgens weigert de aangevraagde licentie uitgeven, zoals wordt weergegeven. De gedetailleerde beschrijving is ' licentie ophalen is mislukt, ' die is ontworpen.

![Unentitled gebruikers](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="run-a-custom-security-token-service"></a>Voer een aangepaste beveiligingstokenservice
Als u een aangepaste STS uitvoert, wordt de JWT uitgegeven door de aangepaste STS met behulp van een symmetrische of een asymmetrische sleutel.

De volgende Schermafbeelding toont een scenario waarbij een symmetrische sleutel (met behulp van Chrome):

![Aangepaste STS met een symmetrische sleutel](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

De volgende Schermafbeelding toont een scenario met een asymmetrische sleutel via een X509 van het certificaat (met behulp van een moderne browser van Microsoft):

![Aangepaste STS met een asymmetrische sleutel](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

In beide van de vorige gevallen wordt blijft verificatie van de gebruiker hetzelfde. Dit vindt plaats via Azure AD. Het enige verschil is dat JWTs zijn uitgegeven door de aangepaste STS in plaats van Azure AD. Wanneer u dynamische CENC beveiliging configureert, geeft de licentie delivery Servicebeperking het type van JWT, een symmetrische of een asymmetrische sleutel.

## <a name="summary"></a>Samenvatting
Dit document besproken CENC met multi systeemeigen DRM en toegangsbeheer via tokenverificatie, het ontwerp en de implementatie met behulp van Azure Media Services en Media Player.

* Het ontwerp van een verwijzing is waarin alle benodigde onderdelen in een DRM/CENC-subsysteem worden gepresenteerd.
* De implementatie van een verwijzing is op Azure Media Services en Media Player weergegeven.
* Sommige onderwerpen die direct betrokken zijn bij het ontwerp en de implementatie zijn ook besproken.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
