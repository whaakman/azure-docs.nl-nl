---
title: Primaire Vernieuwingstoken (PRT) en Azure AD - Azure Active Directory
description: Wat is de rol van en hoe beheer ik de primaire Refresh Token (PRT) in Azure Active Directory?
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e195a93209875b9eabfaa2ad00772281922443c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476119"
---
# <a name="what-is-a-primary-refresh-token"></a>Wat is een primaire Refresh Token?

Een primaire Refresh Token (PRT) is een belangrijke artefact van Azure AD-verificatie op Windows 10, iOS en Android-apparaten. Het is een JSON Web Token (JWT) speciaal wordt verleend aan Microsoft eerste partij token brokers voor eenmalige aanmelding (SSO) inschakelen via de toepassingen die worden gebruikt op deze apparaten. In dit artikel bieden we meer informatie over hoe een PRT is uitgegeven, gebruikt en beveiligd op Windows 10-apparaten.

In dit artikel wordt ervan uitgegaan dat u al bekend bent met de statussen van de ander apparaat beschikbaar zijn in Azure AD en hoe eenmalige aanmelding werkt in Windows 10. Zie het artikel voor meer informatie over apparaten in Azure AD [wat is Apparaatbeheer in Azure Active Directory?](overview.md)

## <a name="key-terminology-and-components"></a>Belangrijke terminologie en onderdelen

De volgende Windows-onderdelen een belangrijke rol spelen bij het aanvragen en het gebruik van een PRT:

* **Cloud-verificatieprovider** (CloudAP): CloudAP is de provider van moderne verificatie voor Windows zich aanmeldt, die controleert of gebruikers die zich aanmelden met een Windows 10-apparaat. CloudAP biedt een raamwerk invoegtoepassing die identiteit providers gebruikmaken kunnen van Windows-verificatie inschakelen met behulp van de referenties die id-provider.
* **Accountmanager web** (WAM): WAM is de standaard-resourcetokenbroker op Windows 10-apparaten. WAM biedt ook een raamwerk invoegtoepassing die identiteit providers kunnen bouwen op en eenmalige aanmelding inschakelen voor hun toepassingen die afhankelijk zijn van deze id-provider.
* **Azure AD-CloudAP invoegtoepassing**: Een Azure AD specifieke-invoegtoepassing die is gebouwd op het framework CloudAP, die verifieert met Azure AD tijdens het aanmelden bij Windows-gebruikersreferenties.
* **Azure AD-WAM invoegtoepassing**: Een Azure AD specifieke-invoegtoepassing die is gebouwd op de WAM-framework, waarmee de eenmalige aanmelding voor toepassingen die afhankelijk van Azure AD voor verificatie zijn.
* **Dsreg**: Een Azure AD specifiek onderdeel op Windows 10, die verantwoordelijk is voor het registratieproces van het apparaat voor alle apparaatstatussen.
* **Trusted Platform Module** (TPM): Een TPM is een hardwareonderdeel dat is ingebouwd in een apparaat, die beveiliging op basis van hardware-functies voor gebruikers- en -geheimen biedt. Meer informatie vindt u in het artikel [Trusted Platform Module-Technologieoverzicht](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

## <a name="what-does-the-prt-contain"></a>Wat bevat de PRT?

Een PRT bevat claims in het algemeen worden opgenomen in een Azure AD-vernieuwingstoken. Bovendien zijn sommige apparaat-specifieke claims in de PRT opgenomen. Ze zijn als volgt:

* **Apparaat-ID**: Een PRT wordt verleend aan een gebruiker op een specifiek apparaat. De apparaat-ID claim `deviceID` bepaalt u het apparaat de PRT is uitgegeven voor de gebruiker op. Deze claim wordt later aan tokens die zijn verkregen via de PRT uitgegeven. De claim apparaat-ID wordt gebruikt om autorisatie voor voorwaardelijke toegang op basis van apparaatstatus of naleving te bepalen.
* **Sessiesleutel**: De sessiesleutel is een gecodeerde symmetrische sleutel, die worden gegenereerd door de Azure AD authentication-service als onderdeel van de PRT uitgegeven. De sessiesleutel fungeert als het bewijs van bezit wanneer een PRT wordt gebruikt voor het verkrijgen van tokens voor andere toepassingen.

### <a name="can-i-see-whats-in-a-prt"></a>Kan ik zien wat er in een PRT?

Een PRT is een niet-toegankelijke blob van Azure AD, waarvan de inhoud niet bekend is dat alle clientonderdelen verzonden. U kunt geen zien wat er in een PRT.

## <a name="how-is-a-prt-issued"></a>Hoe wordt een PRT verleend?

Registratie van het apparaat is een vereiste voor verificatie op basis van apparaten in Azure AD. Een PRT wordt verleend aan gebruikers alleen op geregistreerde apparaten. Zie het artikel voor meer informatie over het registreren van apparaten, [Windows Hello voor bedrijven en Device Registration service](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-device-registration). Tijdens de registratie van apparaten genereert het onderdeel dsreg twee sets met cryptografische sleutelparen:

* Apparaatsleutel (dkpub/dkpriv)
* Transportsleutel (tkpub/tkpriv)

De persoonlijke sleutels zijn afhankelijk van het apparaat TPM als het apparaat een TPM geldig is en correct werken, heeft terwijl de openbare sleutels worden verzonden naar Azure AD tijdens het registratieproces van het apparaat. Deze sleutels worden gebruikt voor het valideren van de apparaatstatus tijdens PRT aanvragen.

De PRT is uitgegeven tijdens de verificatie van de gebruiker op een Windows 10-apparaat in twee scenario's:

* **Azure AD-domein** of **toegevoegd aan Hybrid Azure AD**: Een PRT is uitgegeven tijdens de Windows-aanmelding wanneer een gebruiker zich met de referenties van hun organisatie aanmeldt. Een PRT is uitgegeven met referenties voor alle Windows 10 wordt ondersteund, bijvoorbeeld wachtwoord- en Windows Hello voor bedrijven. In dit scenario is Azure AD CloudAP invoegtoepassing de primaire instantie voor de PRT.
* **Azure AD geregistreerd apparaat**: Wanneer een gebruiker een tweede werkaccount op hun Windows 10-apparaat toevoegt, wordt er een PRT weergegeven. Gebruikers kunnen een account toevoegen aan Windows 10 op twee verschillende manieren:  
   * Toevoegen van een account via de **Gebruik dit account overal op dit apparaat** vragen na het aanmelden bij een app (bijvoorbeeld Outlook)
   * Toe te voegen vanuit een account **instellingen** > **Accounts** > **toegang tot werk of School** > **verbinden**

De invoegtoepassing Azure AD WAM is de primaire instantie voor de PRT in geregistreerd bij Azure AD device-scenario's, omdat Windows-aanmelding niet gebeurt er met deze Azure AD-account.

> [!NOTE]
> 3e partij id-providers moeten ondersteuning bieden voor de WS-Trust-protocol om in te schakelen PRT uitgifte op Windows 10-apparaten. Zonder WS-Trust, PRT kan niet worden uitgegeven aan gekoppelde gebruikers in hybride Azure AD-domein of Azure AD apparaten

## <a name="what-is-the-lifetime-of-a-prt"></a>Wat is de levensduur van een PRT?

Nadat het is uitgegeven, wordt een PRT is geldig gedurende 14 dagen en continu wordt verlengd, zolang de gebruiker actief gebruikmaakt van het apparaat.  

## <a name="how-is-a-prt-used"></a>Hoe wordt een PRT gebruikt?

Een PRT wordt gebruikt door twee belangrijke onderdelen in Windows:

* **Azure AD-CloudAP invoegtoepassing**: Tijdens Windows zich heeft aangemeld vraagt de invoegtoepassing Azure AD CloudAP een PRT van Azure AD met behulp van de referenties die zijn opgegeven door de gebruiker. Er worden ook de PRT om in te schakelen in de cache aanmelden wanneer de gebruiker heeft geen toegang tot een internetverbinding opgeslagen.
* **Azure AD-WAM invoegtoepassing**: Wanneer gebruikers proberen toegang tot toepassingen, de Azure AD WAM-invoegtoepassing maakt gebruik van de PRT voor SSO inschakelen op Windows 10. Azure AD-WAM-invoegtoepassing maakt gebruik van de PRT om aan te vragen van vernieuwen en toegang tot tokens voor toepassingen die afhankelijk van WAM voor token aanvragen zijn. Bovendien kunnen door de PRT injecteren in browseraanvragen eenmalige aanmelding in browsers. Browser eenmalige aanmelding in Windows 10 (standaard) wordt ondersteund in Microsoft Edge en Chrome (via de Windows 10-Accounts of Office Online-extensie).

## <a name="how-is-a-prt-renewed"></a>Hoe wordt een PRT vernieuwd?

Een PRT wordt vernieuwd in twee verschillende methoden:

* **Azure AD-CloudAP invoegtoepassing elke 4 uur**: De invoegtoepassing CloudAP vernieuwt de PRT elke 4 uur tijdens de aanmelding bij Windows. Als de gebruiker heeft geen internetverbinding gedurende die tijd, wordt CloudAP invoegtoepassing de PRT vernieuwen nadat het apparaat is verbonden met internet.
* **Azure AD-WAM invoegtoepassing tijdens app token aanvragen**: De invoegtoepassing WAM kan eenmalige aanmelding op Windows 10-apparaten door in te schakelen op de achtergrond token aanvragen voor toepassingen. De invoegtoepassing WAM kan de PRT vernieuwen tijdens deze token aanvragen op twee verschillende manieren:
   * Een app WAM aanvragen voor een toegangstoken op de achtergrond, maar er is geen vernieuwingstoken beschikbaar zijn voor die app. In dit geval WAM de PRT gebruikt om te vragen van een token voor de app en wordt een nieuwe PRT in in het antwoord.
   * Een app WAM aanvragen voor een toegangstoken, maar de PRT is ongeldig of Azure AD vereist extra autorisatie (bijvoorbeeld Azure multi-factor Authentication). In dit scenario WAM initieert een interactieve aanmelding vereisen dat de gebruiker te verifiëren of aanvullende verificatie opgeven en een nieuwe PRT wordt verleend op verificatie is geslaagd.

### <a name="key-considerations"></a>Belangrijkste overwegingen

* Een PRT is alleen uitgegeven en vernieuwd tijdens de verificatie van de systeemeigen app. Een PRT is niet vernieuwd of uitgegeven tijdens een browsersessie.
* In Azure AD toegevoegd en hybride Azure AD gekoppelde apparaten, de invoegtoepassing CloudAP is de primaire instantie voor een PRT. Als een PRT tijdens een tokenaanvraag WAM gebaseerde is vernieuwd, wordt de PRT verzonden naar CloudAP-invoegtoepassing, die controleert of de geldigheid van de PRT met Azure AD voordat u deze accepteert.

## <a name="how-is-the-prt-protected"></a>Hoe worden de PRT beveiligd?

Een PRT wordt beveiligd door deze op het apparaat de gebruiker is aangemeld bij binding. Azure AD en Windows 10 PRT beveiliging inschakelen via de volgende methoden:

* **Tijdens de eerste aanmelding**: Tijdens de eerste zich heeft aangemeld, is een PRT uitgegeven door het ondertekenen van aanvragen met behulp van de apparaatsleutel cryptografisch gegenereerd tijdens de registratie van het apparaat. Op een apparaat met een TPM geldig is en correct werken, wordt de sleutel van het apparaat beveiligd door de TPM te voorkomen dat schadelijke toegang. Een PRT is niet uitgegeven als de bijbehorende handtekening van de apparaat-sleutel kan niet worden gevalideerd.
* **Tijdens het token aanvragen en -verlenging**: Wanneer een PRT is uitgegeven, problemen Azure AD een versleutelde sessiesleutel ook met het apparaat. Het is versleuteld met de openbaar vervoer sleutel (tkpub) gegenereerd en verzonden naar Azure AD als onderdeel van device Registration service. Deze sessiesleutel kan alleen worden ontsleuteld door de persoonlijke transportsleutel (tkpriv) dat is beveiligd door de TPM. De sessiesleutel is de sleutel bewijs van eigendom (POP) voor alle aanvragen die worden verzonden naar Azure AD.  De sessiesleutel ook wordt beveiligd door de TPM en geen andere onderdelen van het besturingssysteem kunt openen. Token aanvragen of aanvragen tot vernieuwing van PRT veilig zijn ondertekend door deze sessiesleutel via de TPM en daarom kunnen worden geknoeid. Azure AD, worden alle aanvragen van het apparaat die niet zijn ondertekend door de bijbehorende sessiesleutel ongeldig.

Door het beveiligen van deze sleutels met de TPM, kwaadwillende actoren de sleutels niet kunnen stelen noch de PRT replay elders als de TPM niet toegankelijk is, zelfs als een aanvaller het apparaat fysiek in bezit hebben heeft.  Met behulp van een TPM aanzienlijk verhoogt de beveiliging van Azure AD-join, toegevoegd aan Hybrid Azure AD join, en dus Azure AD ingeschreven apparaten tegen diefstal van referenties. Voor prestaties en betrouwbaarheid is TPM 2.0 de aanbevolen versie voor alle Azure AD device registration-scenario's in Windows 10.

### <a name="how-are-app-tokens-and-browser-cookies-protected"></a>Hoe worden app-tokens en browsercookies beveiligd?

**App-tokens**: Wanneer een app token via WAM aanvraagt, wordt Azure AD geeft een vernieuwingstoken en een toegangstoken. Echter WAM alleen het toegangstoken geretourneerd naar de app en het vernieuwingstoken dat in de cache beveiligt door het versleutelen met de sleutel van de gebruiker data protection application programming interface (DPAPI). Het vernieuwingstoken dat door aanvragen te ondertekenen met de sessiesleutel WAM veilig gebruikt voor het verder access tokens uitgeven. De DPAPI-sleutel wordt beveiligd door een Azure AD op basis van symmetrische sleutel in Azure AD zelf. Wanneer het apparaat het gebruikersprofiel met de DPAPI-sleutel te ontsleutelen moet, biedt Azure AD de DPAPI-sleutel versleuteld met de sessiesleutel, welke invoegtoepassing CloudAP TPM voor het ontsleutelen van aanvragen. Deze functionaliteit zorgt voor consistentie in het beveiligen van vernieuwingstokens en voorkomt u toepassingen implementeert eigen methoden voor beveiliging.  

**Browsercookies**: In Windows 10, Azure AD biedt ondersteuning voor eenmalige aanmelding in Internet Explorer en Microsoft Edge browser systeemeigen of in de Google Chrome via de extensie voor Windows 10-accounts. De beveiliging is gebouwd niet alleen voor het beveiligen van de cookies, maar ook de eindpunten die de cookies die worden verzonden. Browsercookies zijn dezelfde manier beveiligd een PRT is, door het gebruik van de sessiesleutel cookies beveiligen en ondertekenen.

Wanneer een gebruiker de interactie van een browser start, roept de browser (of de uitbreiding) een COM-native client-host. De systeemeigen client-host zorgt ervoor dat de pagina van een van de toegestane domeinen. De browser kan verzenden, dat andere parameters voor de systeemeigen client host, met inbegrip van een nonce, maar de systeemeigen client-host garandeert voor validatie van de hostnaam. De host native client vraagt een PRT-cookie van CloudAP-invoegtoepassing, die wordt gemaakt en tekent het met de TPM-beveiligde sessiesleutel. Als de PRT-cookie is ondertekend door de sessiesleutel, kan deze worden geknoeid. Deze PRT-cookie is opgenomen in de aanvraagheader voor Azure AD voor het valideren van het apparaat die afkomstig van zijn is. Als de Chrome-browser, kunt alleen de uitbreiding expliciet is gedefinieerd in het manifest van de host van de systeemeigen client deze zo wordt voorkomen dat willekeurige extensies deze aanvragen worden aangeroepen. Nadat Azure AD wordt de cookie PRT gevalideerd, geeft deze een sessiecookie aan de browser. Deze sessiecookie bevat ook de dezelfde sessiesleutel uitgegeven met een PRT. Tijdens de volgende aanvragen, wordt de sessiesleutel gevalideerd effectief binding van de cookie op het apparaat en zo wordt voorkomen dat replays van een andere locatie.

## <a name="when-does-a-prt-get-an-mfa-claim"></a>Wanneer een PRT een MFA-claim krijgen?

Een PRT krijgen een claim multi-factor authentication (MFA) in specifieke scenario's. Wanneer een PRT gebaseerd op meervoudige verificatie wordt gebruikt om aan te vragen van tokens voor toepassingen, worden de MFA-claim wordt overgebracht naar de app-tokens. Deze functionaliteit biedt een naadloze ervaring voor gebruikers door te voorkomen dat de MFA-controle voor elke app die vereist is. Een PRT krijgen een MFA-claim in de volgende manieren:

* **Aanmelden met Windows Hello voor bedrijven**: Windows Hello voor bedrijven vervangt wachtwoorden, en maakt gebruik van cryptografische sleutels voor sterke verificatie met twee factoren. Windows Hello voor bedrijven is specifiek voor een gebruiker op een apparaat, en zelf MFA is vereist om in te richten. Wanneer een gebruiker zich aanmeldt met Windows Hello voor bedrijven, krijgt de PRT van de gebruiker een MFA-claim. In dit scenario geldt ook voor gebruikers die zich heeft aangemeld met smartcards als smartcardverificatie een MFA-claim van AD FS produceert.
   * Als Windows Hello voor bedrijven wordt beschouwd als meervoudige verificatie, wordt de MFA-claim bijgewerkt wanneer de PRT zelf wordt vernieuwd, zodat de MFA-duur voortdurend uitbreiden wordt wanneer gebruikers zich aanmelden met WIndows Hello voor bedrijven
* **MFA tijdens WAM interactieve aanmelding in**: Tijdens een tokenaanvraag via WAM als een gebruiker vereist is voor MFA voor toegang tot de app is de PRT dat is vernieuwd tijdens deze interactie met een MFA-claim gedrukt.
   * In dit geval is de MFA-claim niet voortdurend bijgewerkt, zodat de MFA-duur is gebaseerd op de levensduur ingesteld op de map.
* **MFA tijdens het registreren van apparaten**: Als een beheerder de apparaatinstellingen van hun is geconfigureerd in Azure AD [MFA om apparaten te registreren vereisen](device-management-azure-portal.md#configure-device-settings), de gebruiker moet doen MFA om de registratie te voltooien. Tijdens dit proces is de PRT die wordt verleend aan de gebruiker de MFA-claim verkregen tijdens de registratie. Deze mogelijkheid geldt alleen voor de gebruiker die de join-bewerking, niet voor andere gebruikers die zich komt aanmelden bij dat apparaat.
   * Net als bij de WAM interactieve aanmelding, de MFA-claim is niet voortdurend bijgewerkt, zodat de MFA-duur is gebaseerd op de levensduur ingesteld op de map.

Windows 10 onderhoudt een gepartitioneerde lijst PRTs voor elke referentie. Er is dus een PRT voor elk van Windows Hello voor bedrijven, wachtwoord of smartcard. Deze partitioneren zorgt ervoor dat MFA claims zijn geïsoleerd op basis van de referentie gebruikt en niet door elkaar tijdens token aanvragen.

## <a name="how-is-a-prt-invalidated"></a>Hoe een PRT ongeldig?

Een PRT is ongeldig in de volgende scenario's:

* **Ongeldige gebruiker**: Als een gebruiker wordt verwijderd of uitgeschakeld in Azure AD, wordt hun PRT is ongeldig en kan niet worden gebruikt om op te halen van tokens voor toepassingen. Als een verwijderde of een uitgeschakelde gebruiker al aangemeld bij een apparaat hebt, zou in de cache aanmelden aanmelden, totdat CloudAP op de hoogte van de ongeldige status is. Zodra CloudAP wordt vastgesteld dat de gebruiker ongeldig is, blokkeert het daaropvolgende aanmeldingen. Een ongeldige gebruiker wordt automatisch geblokkeerd van Meld op nieuwe apparaten waarvoor geen hun referenties in de cache opgeslagen.
* **Ongeldige apparaatnaam**: Als een apparaat wordt verwijderd of uitgeschakeld in Azure AD, wordt de PRT verkregen op het apparaat is ongeldig en kan niet worden gebruikt om op te halen van tokens voor andere toepassingen. Als een gebruiker is al aangemeld bij een ongeldige-apparaat, kunnen blijven om dit te doen. Maar alle tokens op het apparaat zijn ongeldig en wordt de gebruiker heeft geen eenmalige aanmelding in resources vanaf dat apparaat.
* **Wachtwoordwijziging**: Nadat een gebruiker het wachtwoord wijzigt, wordt de PRT verkregen met het vorige wachtwoord ongeldig gemaakt door Azure AD. Leidt de gebruiker aan een nieuwe PRT voor wachtwoordwijzigingen. Deze invalidatie kan op twee verschillende manieren gebeuren:
   * Als de gebruiker zich aanmeldt bij Windows met het nieuwe wachtwoord, wordt CloudAP verwijdert de oude PRT en vraagt een Azure AD om uit te geven van een nieuwe PRT met het nieuwe wachtwoord. Als de gebruiker heeft geen internetverbinding, het nieuwe wachtwoord kan niet worden gevalideerd, wordt Windows kan vereisen dat de gebruiker hun oude wachtwoord in te voeren.
   * Als een gebruiker is aangemeld met het oude wachtwoord of het wachtwoord gewijzigd nadat u zich bij Windows aanmeldt, wordt voor alle tokenaanvragen WAM op basis van de oude PRT gebruikt. In dit scenario wordt de gebruiker wordt gevraagd om u te verifiëren tijdens de WAM tokenaanvraag en een nieuwe PRT wordt verleend.
* **Problemen met TPM**: TPM van een apparaat kan soms falter of mislukken, waardoor er ontoegankelijkheid van sleutels die zijn beveiligd door de TPM. In dit geval wordt het apparaat niet in staat is van een PRT ophalen of bij het aanvragen van tokens met behulp van een bestaande PRT zoals het bezit zijn van de cryptografische sleutels niet kan bewijzen. Als gevolg hiervan is een bestaande PRT ongeldig gemaakt door Azure AD. Wanneer Windows 10 is een fout detecteert, maken het van een herstel-stroom voor het registreren van het apparaat opnieuw met de nieuwe cryptografische sleutels. Met hybride Azure Ad-join, net als bij de registratie, wordt het herstel op de achtergrond gebeurt zonder invoer van de gebruiker. Voor Azure AD-domein of Azure AD apparaten ingeschreven, moet het herstel worden uitgevoerd door een gebruiker met beheerdersrechten op het apparaat. In dit scenario wordt de recovery-stroom gestart door een Windows-prompt waarmee de gebruiker om te kunnen herstellen van het apparaat.

## <a name="detailed-flows"></a>Gedetailleerde stromen

De volgende diagrammen ziet u de onderliggende details in uitgeven, vernieuwen en met behulp van een PRT om aan te vragen van een toegangstoken voor een toepassing. Bovendien deze stappen ook wordt beschreven hoe de hiervoor genoemde beveiligingsmechanismen tijdens deze interacties worden toegepast.

### <a name="prt-issuance-during-first-sign-in"></a>Uitgifte van PRT tijdens de eerste aanmelding

![Uitgifte van PRT tijdens de eerste aanmelding gedetailleerde stroom](./media/concept-primary-refresh-token/prt-initial-sign-in.png)

> [!NOTE]
> In Azure AD gekoppelde apparaten, deze exchange synchroon gebeurt voor het uitgeven van een PRT voordat de gebruiker zich bij Windows aanmelden kan. In hybride Azure AD gekoppelde apparaten, on-premises Active Directory is de primaire-instantie. De gebruiker wordt dus alleen wachten totdat ze een TGT om aan te melden, aanschaffen kunnen, terwijl de uitgifte van PRT asynchroon verloopt. In dit scenario geldt niet voor Azure AD ingeschreven apparaten, zoals aanmelding maakt geen gebruik van Azure AD-referenties.

| Stap | Description |
| :---: | --- |
| A | Gebruiker voert zijn wachtwoord in het teken in de gebruikersinterface. LogonUI geeft de referenties in een buffer auth aan LSA, wat op zijn beurt intern aan CloudAP doorgegeven. CloudAP verzendt deze aanvraag naar de CloudAP-invoegtoepassing. |
| B | De invoegtoepassing CloudAP initieert een aanvraag van de realm detectie voor het identificeren van de id-provider voor de gebruiker. Als de tenant van de gebruiker heeft de installatie van een federation-provider, retourneert Azure AD uitwisseling van de metagegevens van de federatieprovider (MEX)-eindpunt. Als dit niet het geval is, retourneert Azure AD dat de gebruiker wordt beheerd die aangeeft dat de gebruiker kan verifiëren met Azure AD. |
| C | Als de gebruiker wordt beheerd, ontvangt CloudAP de nonce van Azure AD. Als de gebruiker is gefedereerd, vraagt CloudAP invoegtoepassing een SAML-token van de federation-provider met de referenties van de gebruiker. Zodra deze ontvangt, wordt het SAML-token, vraagt het een nonce van Azure AD. |
| D | CloudAP invoegtoepassing vormt van de verificatieaanvraag met referenties van de gebruiker, nonce en een broker-bereik, zich van de aanvraag door de sleutelwaarde (dkpriv) en verzendt ze naar Azure AD. In een federatieve omgeving CloudAP-invoegtoepassing maakt gebruik van het SAML-token dat is geretourneerd door de federation-provider in plaats van de gebruiker de referenties. |
| E | Azure AD valideert de referenties van de gebruiker, de nonce en handtekening van het apparaat, verifieert u dat het apparaat is geldig in de tenant en problemen met de versleutelde PRT. Samen met de PRT geeft Azure AD ook een symmetrische sleutel, de sessiesleutel versleuteld door Azure AD met behulp van de sleutel Transport (tkpub) genoemd. Bovendien worden de sessiesleutel is ook opgenomen in de PRT. Deze sessiesleutel fungeert als de sleutel bewijs van eigendom (PoP) voor volgende aanvragen met de PRT. |
| F | De versleutelde sleutel van PRT en sessie doorgegeven CloudAP invoegtoepassing aan CloudAP. CloudAP aanvragen van de TPM voor het ontsleutelen van de sessiesleutel met behulp van de sleutel Transport (tkpriv) en opnieuw versleutelen met behulp van de sleutel van TPM. CloudAP slaat de versleutelde sessiesleutel in de cache, samen met de PRT. |

### <a name="prt-renewal-in-subsequent-logons"></a>PRT vernieuwen in de volgende aanmeldingen

![PRT vernieuwen in de volgende aanmeldingen](./media/concept-primary-refresh-token/prt-renewal-subsequent-logons.png)

| Stap | Description |
| :---: | --- |
| A | Gebruiker voert zijn wachtwoord in het teken in de gebruikersinterface. LogonUI geeft de referenties in een buffer auth aan LSA, wat op zijn beurt intern aan CloudAP doorgegeven. CloudAP verzendt deze aanvraag naar de CloudAP-invoegtoepassing. |
| B | Als de gebruiker eerder heeft aangemeld bij de gebruiker, wordt Windows starten in de cache opgeslagen Meld u aan en valideert de referenties in om de gebruiker zich aanmeldt. Elke 4 uur, initieert de invoegtoepassing CloudAP PRT vernieuwing het asynchroon. |
| C | De invoegtoepassing CloudAP initieert een aanvraag van de realm detectie voor het identificeren van de id-provider voor de gebruiker. Als de tenant van de gebruiker heeft de installatie van een federation-provider, retourneert Azure AD uitwisseling van de metagegevens van de federatieprovider (MEX)-eindpunt. Als dit niet het geval is, retourneert Azure AD dat de gebruiker wordt beheerd die aangeeft dat de gebruiker kan verifiëren met Azure AD. |
| D | Als de gebruiker is gefedereerd, vraagt CloudAP invoegtoepassing een SAML-token van de federation-provider met de referenties van de gebruiker. Zodra deze ontvangt, wordt het SAML-token, vraagt het een nonce van Azure AD. Als de gebruiker wordt beheerd, ontvangt CloudAP rechtstreeks de nonce van Azure AD. |
| E | CloudAP invoegtoepassing bouwt de verificatieaanvraag met referenties van de gebruiker, nonce en de bestaande PRT, ondertekent het verzoek met de sessiesleutel en verzendt ze naar Azure AD. In een federatieve omgeving CloudAP-invoegtoepassing maakt gebruik van het SAML-token dat is geretourneerd door de federation-provider in plaats van de gebruiker de referenties. |
| F | Azure AD valideert de handtekening van de sleutel sessie door deze te vergelijken op basis van de sessiesleutel ingesloten in de PRT, valideert de nonce en controleert of het apparaat is geldig in de tenant en een nieuwe PRT problemen. Zoals eerder, wordt de PRT opnieuw samen met de sessiesleutel versleuteld met Transport-sleutel (tkpub). |
| G | De versleutelde sleutel van PRT en sessie doorgegeven CloudAP invoegtoepassing aan CloudAP. CloudAP vraagt de TPM voor het ontsleutelen van de sessiesleutel met behulp van de sleutel Transport (tkpriv) en opnieuw versleutelen met behulp van de sleutel van TPM. CloudAP slaat de versleutelde sessiesleutel in de cache, samen met de PRT. |

### <a name="prt-usage-during-app-token-requests"></a>PRT gebruik tijdens de app-token aanvragen

![PRT gebruik tijdens de app-token aanvragen](./media/concept-primary-refresh-token/prt-usage-app-token-requests.png)

| Stap | Description |
| :---: | --- |
| A | Een toepassing (bijvoorbeeld Outlook, OneNote enzovoort) initieert een aanvraag voor een toegangstoken aan WAM. WAM, vraagt op zijn beurt de WAM van Azure AD-invoegtoepassing voor het afhandelen van het token aan te vragen. |
| B | Als een vernieuwingstoken voor de toepassing al beschikbaar is, wordt deze door WAM van Azure AD-invoegtoepassing gebruikt om aan te vragen van een toegangstoken. Om te bewijzen van de binding van apparaat, ondertekent WAM invoegtoepassing de aanvraag met de sessiesleutel. Azure AD de sessiesleutel valideert en problemen met een toegangstoken en een nieuw vernieuwingstoken voor de app, versleuteld met de sessiesleutel. WAM invoegtoepassing vraagt Cloud Azië en Stille Oceaan-invoegtoepassing voor het ontsleutelen van de tokens die, op zijn beurt, de TPM met behulp van de sessiesleutel vraagt, wat resulteert in WAM invoegtoepassing aan zowel de tokens ontsleutelen. WAM-invoegtoepassing biedt vervolgens in het toegangstoken voor de toepassing, terwijl deze opnieuw het vernieuwingstoken dat met DPAPI versleutelt en slaat ze op in een eigen cache  |
| C |  Als een vernieuwingstoken voor de toepassing niet beschikbaar is, WAM van Azure AD-invoegtoepassing maakt gebruik van de PRT om aan te vragen van een toegangstoken. Voor bewijs van bezit, ondertekent WAM invoegtoepassing de aanvraag met de PRT met de sessiesleutel. Azure AD valideert de handtekening van de sleutel sessie door deze te vergelijken op basis van de sessiesleutel ingesloten in de PRT, wordt gecontroleerd of het apparaat geldig is en problemen met een toegangstoken en een vernieuwingstoken voor de toepassing. bovendien een nieuwe PRT (op basis van de vernieuwingscyclus), kan geven in Azure AD al deze versleuteld met de sessiesleutel. |
| D | WAM invoegtoepassing vraagt Cloud Azië en Stille Oceaan-invoegtoepassing voor het ontsleutelen van de tokens die, op zijn beurt, de TPM met behulp van de sessiesleutel vraagt, wat resulteert in WAM invoegtoepassing aan zowel de tokens ontsleutelen. WAM-invoegtoepassing biedt alleen de toegangstoken uit aan de toepassing vervolgens terwijl deze opnieuw het vernieuwingstoken dat met DPAPI versleutelt en slaat ze op in een eigen cache. WAM-invoegtoepassing gebruikt het vernieuwingstoken voortaan voor deze toepassing. WAM invoegtoepassing geeft ook weer de nieuwe PRT tot Cloud-Azië en Stille Oceaan-invoegtoepassing, die valideert de PRT met Azure AD in de eigen cache wordt bijgewerkt. Cloud-Azië en Stille Oceaan-invoegtoepassing maakt gebruik van de nieuwe PRT voortaan. |
| E | WAM biedt het zojuist uitgegeven toegangstoken WAM, biedt deze op zijn beurt terug naar de aanroepende toepassing|

### <a name="browser-sso-using-prt"></a>Browser eenmalige aanmelding met behulp van PRT

![Browser eenmalige aanmelding met behulp van PRT](./media/concept-primary-refresh-token/browser-sso-using-prt.png)

| Stap | Description |
| :---: | --- |
| A | Gebruiker meldt zich aan bij Windows met hun referenties om toegang te krijgen van een PRT. Zodra de gebruiker de browser opent, wordt uit het register van browser (of uitbreiding), de URL's geladen. |
| B | Wanneer een gebruiker wordt geopend de aanmeldings-URL van een Azure AD, valideert de browser of de extensie u de URL met de resultaten opgehaald uit het register. Als ze overeenkomen, roept de browser de systeemeigen client-host op voor het ophalen van een token. |
| C | De systeemeigen client-host valideert dat de URL's deel uitmaken van de Microsoft id-providers (Microsoft-account of Azure AD), een nonce verzonden via de URL worden uitgepakt en Hiermee wordt een aanroep naar CloudAP invoegtoepassing om op te halen van een cookie PRT. |
| D | De invoegtoepassing CloudAP maakt de cookie PRT, meld u aan met de sessiesleutel van de TPM-gebonden en deze verzenden naar de native client-host. Als de cookie is ondertekend door de sessiesleutel, kan deze worden geknoeid. |
| E | De systeemeigen client-host, wordt deze cookie PRT terug naar de browser, waaronder als onderdeel van de aanvraagheader x-ms-RefreshTokenCredential en aanvraag tokens aangeroepen vanuit Azure AD. |
| F | Azure AD valideert de handtekening van de sleutel sessie op de cookie PRT, valideert de nonce, wordt gecontroleerd of het apparaat geldig in de tenant is en problemen met een ID-token voor de webpagina wordt weergegeven en een versleutelde sessiecookie voor de browser. |

## <a name="next-steps"></a>Volgende stappen

Zie het artikel voor meer informatie over het oplossen van problemen met betrekking tot PRT [probleemoplossing hybride Azure Active Directory gekoppelde Windows 10 en Windows Server 2016-apparaten](troubleshoot-hybrid-join-windows-current.md).
