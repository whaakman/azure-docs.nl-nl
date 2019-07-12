---
title: Eenmalige aanmelding - niet-galerietoepassingen - Microsoft identity-platform | Microsoft Docs
description: Eenmalige aanmelding (SSO) configureren voor niet-galerietoepassingen in Microsoft identity-platform (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 05/08/2019
ms.author: celested
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: a72cb7bc7feeba984d568a0465d4f23a494496e8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807656"
---
# <a name="configure-single-sign-on-to-non-gallery-applications-in-microsoft-identity-platform"></a>Single sign-on bij niet-galerietoepassingen in Microsoft identity-platform configureren

In dit artikel gaat over een functie waarmee beheerders het configureren van eenmalige aanmelding voor toepassingen ontbreekt in de Microsoft identity-platform-app-galerie *zonder code te schrijven*.

Als u in plaats daarvan voor richtlijnen voor ontwikkelaars over het integreren van aangepaste apps met Azure AD via de programmacode, Zie [Verificatiescenario's voor Azure AD](../develop/authentication-scenarios.md).

De toepassingsgalerie van Microsoft identity-platform biedt een overzicht van toepassingen die bekend zijn bij het ondersteunen van een vorm van eenmalige aanmelding met Microsoft identity-platform, zoals beschreven in [in dit artikel](what-is-single-sign-on.md). Zodra u (als een IT-specialist of system integrator in uw organisatie) hebt gevonden die de toepassing dat u verbinding wilt maken, u kunt aan de slag door de stapsgewijze instructies die zijn gepresenteerd in Azure portal om in te schakelen eenmalige aanmelding.

De volgende mogelijkheden zijn ook beschikbaar is, op basis van uw licentieovereenkomst. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/active-directory/) voor meer informatie.

- Self-service-integratie van een toepassing die gebruikmaken van een moderne protocol, zoals [OpenId Connect/OAuth](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) aan haar gebruikers verifiëren en verkrijgen van tokens voor [Microsoft Graph](https://graph.microsoft.com).
- Self-service-integratie van toepassingen die ondersteuning biedt voor [Security Assertion Markup Language (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0) id-providers (SP geïnitieerde of IdP gestart)
- Self-service-integratie van een webtoepassing die een op HTML gebaseerde aanmeldingspagina opgeven met behulp is [wachtwoord gebaseerde SSO](what-is-single-sign-on.md#password-based-sso)
- Selfservice-verbinding van toepassingen die gebruikmaken van de [systeem voor meerdere domeinen Identity Management (SCIM)-protocol voor het inrichten van gebruikers](use-scim-to-provision-users-and-groups.md)
- Koppelingen toevoegen aan elke toepassing in de [startprogramma voor Office 365](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) of de [Azure AD-toegangspaneel](what-is-single-sign-on.md#linked-sign-on)

Deze mogelijkheden kunnen Self-service-integratie van een software als een dienst (SaaS)-toepassing die u gebruikt, bevatten, zelfs als er niemand heeft toegevoegd nog de toepassing naar de Azure AD-toepassingsgalerie. Een andere mogelijkheid is Self-service-integratie van een derde partij-webtoepassing die uw organisatie is geïmplementeerd op servers die u, in de cloud of on-premises beheert.

Ook wel bekend als *sjablonen voor app-integratie*, deze mogelijkheden bieden op standaarden gebaseerde verbindingspunten voor toepassingen die ondersteuning bieden voor SAML, SCIM of op formulieren gebaseerde verificatie. De mogelijkheden behoren flexibele opties en instellingen voor compatibiliteit met een groot aantal toepassingen.

## <a name="adding-an-unlisted-application"></a>Toevoegen van een niet-vermelde toepassing

Het Microsoft identity-platform biedt twee methoden voor het registreren van toepassingen.

Een toepassing die gebruikmaakt van een moderne protocol, zoals [OpenId Connect/OAuth](../develop/active-directory-v2-protocols.md) om de gebruikers te verifiëren is geregistreerd met behulp van de [portal voor appregistratie](../develop/quickstart-register-app.md).

Het registreren van toepassingen met behulp van alle andere typen [verificatiemechanismen ondersteund](what-is-single-sign-on.md), bijvoorbeeld de [SAML](../develop/single-sign-on-saml-protocol.md) protocol, gebruikt u de **bedrijfstoepassingen** blade om verbinding te maken Als u deze met het Microsoft identity-platform.

Als u wilt verbinding maken met een niet-vermelde toepassing met behulp van een app-integratie-sjabloon, moet u deze stappen uitvoeren:

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/) met uw Microsoft identity platform administrator-account.
1. Selecteer **bedrijfstoepassingen** > **nieuwe toepassing**.
1. (Optioneel maar aanbevolen) In de **toevoegen vanuit de galerie** het zoekvak, typ de weergavenaam van de toepassing. Als de toepassing wordt weergegeven in de lijst met zoekresultaten, selecteert u deze en de rest van deze procedure overslaan.
1. Selecteer **niet in de galerij toepassing**. De **uw eigen toepassing toevoegen** pagina wordt weergegeven.

   ![De pagina van uw eigen toepassing ziet u de toevoegen](./media/configure-single-sign-on-non-gallery-applications/add-your-own-application.png)

1. Voer de weergavenaam voor uw nieuwe toepassing.
1. Selecteer **Toevoegen**.

Door het toevoegen van een toepassing op deze manier kunt u een vergelijkbare ervaring met de beschikbare opgeven voor vooraf geïntegreerde toepassingen. Selecteer eerst **eenmalige aanmelding** uit van de toepassing zijbalk. De volgende pagina (**selecteert u een methode voor eenmalige aanmelding**) geeft de opties voor het configureren van eenmalige aanmelding:

- **SAML**
- **Op basis van wachtwoorden**
- **Gekoppelde**

![Selecteer een methode voor eenmalige aanmelding-pagina wordt weergegeven](./media/configure-single-sign-on-non-gallery-applications/select-a-single-sign-on-method.png)

Zie de volgende secties van dit artikel voor meer informatie over deze opties.

## <a name="saml-based-single-sign-on"></a>SAML gebaseerde eenmalige aanmelding

Selecteer de **SAML** optie voor het configureren van verificatie op basis van SAML voor de toepassing. (Deze optie vereist dat de toepassing SAML 2.0 ondersteunt.) De **instellen van eenmalige aanmelding met SAML** pagina wordt weergegeven.

![Toont de Set van eenmalige aanmelding met SAML-pagina](./media/configure-single-sign-on-non-gallery-applications/set-up-single-sign-on-with-saml.png)

Deze pagina bevat vijf verschillende koppen:

| Nummer van koptekst | De kopnaam van de | Zie voor een overzicht van deze kop: |
| --- | --- | --- |
| 1 | **Basic SAML-configuratie** | [Voer de SAML-basisconfiguratie](#enter-basic-saml-configuration) |
| 2 | **Gebruikerskenmerken en Claims** | [Controleren of de claims die zijn uitgegeven in het SAML-token aanpassen](#review-or-customize-the-claims-issued-in-the-saml-token) |
| 3 | **SAML-handtekeningcertificaat** | [Certificaat verlopen gegevens, de status en e-mailmelding](#review-certificate-expiration-data-status-and-email-notification) |
| 4 | **Instellen van \<toepassingsnaam >** | [Doeltoepassing instellen](#set-up-target-application) |
| 5 | **Eenmalige aanmelding testen met \<toepassingsnaam >** | [De SAML-toepassing testen](#test-the-saml-application) |

Nu informatie verzamelen over het gebruik van de SAML-mogelijkheden van de toepassing voordat u doorgaat. Voer de volgende secties voor het configureren van eenmalige aanmelding tussen de toepassing en Azure AD.

### <a name="enter-basic-saml-configuration"></a>Voer de SAML-basisconfiguratie

Als u Azure AD instelt, gaat u naar de **SAML-basisconfiguratie** kop en selecteer de **bewerken** pictogram (een potlood). U kunt handmatig de waarden invoeren of upload een bestand met metagegevens om op te halen van de waarde van de velden.

![Toont de pagina Basic SAML-configuratie](./media/configure-single-sign-on-non-gallery-applications/basic-saml-configuration.png)

De volgende twee velden die zijn vereist:

- **Identifier**. Deze waarde moet uniek zijn voor de toepassing waarvoor eenmalige aanmelding wordt geconfigureerd. U vindt deze waarde als de **verlener** -element in de **AuthnRequest** (SAML-aanvraag) die zijn verzonden door de toepassing. Deze waarde wordt ook weergegeven als de **entiteit-ID** in alle SAML-metagegevens geleverd door de toepassing. Controleren van de toepassing SAML-documentatie voor meer informatie over wat de **entiteit-ID** of **doelgroep** waarde is.

  De volgende code toont hoe de **id** of **verlener** wordt weergegeven in de SAML-aanvraag die de toepassing worden verzonden naar Azure AD:

  ```xml
  <samlp:AuthnRequest
  xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
  ID="id6c1c178c166d486687be4aaf5e482730"
  Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
  xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
  </samlp:AuthnRequest>
  ```

- **Antwoord-URL**. De antwoord-URL is waar de toepassing verwacht voor het ontvangen van het SAML-token. Deze URL wordt ook de assertion consumer service (ACS)-URL genoemd. Controleer de SAML-documentatie voor meer informatie over wat de token SAML-antwoord-URL of ACS-URL is van de toepassing.

  U kunt de volgende PowerShell-script gebruiken voor het configureren van meerdere antwoord-URL's.

  ```powershell
  $sp = Get-AzureADServicePrincipal -SearchString "<Exact app name>"
  $app = Get-AzureADApplication -SearchString "<Exact app name>"
  $urllist = New-Object "System.Collections.Generic.List[String]"
  $urllist.Add("<reply URL 1>")
  $urllist.Add("<reply URL 2>")
  $urllist.Add("<reply URL 3>")
  Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls $urllist
  Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls $urllist
  ```

De volgende drie velden zijn optioneel:

- **Aanmeldings-URL (SP geïnitieerde alleen)** . Deze waarde geeft aan waar de gebruiker zich aanmeldt bij deze toepassing gaat. Als de toepassing wordt uitgevoerd Serviceprovider geïnitieerde eenmalige aanmelding, wanneer een gebruiker naar deze URL navigeert wordt de SP de benodigde omleiding naar Azure AD om te verifiëren en meld u aan de gebruiker. Als u dit veld opgeeft, Azure AD maakt gebruik van deze URL om de toepassing uit Office 365 en Azure AD-Toegangsvenster te starten. Als u dit veld niet opgeeft, Azure AD in plaats daarvan voert IdP gestart door aanmelding bij toepassing is gestart uit Office 365, de Azure AD-Toegangsvenster of de URL van de Azure AD-eenmalige aanmelding (die u kunt kopiëren van de **Dashboard** pagina).

- **Relaystatus**. U kunt een relaystatus in SAML om te geven van de toepassing waar gebruikers na verificatie opgeven. De waarde is doorgaans een URL of URL-pad dat gebruikers naar een specifieke locatie in de toepassing leidt.

- **Afmeldings-URL van**. Deze waarde wordt gebruikt voor het verzenden van de SAML-afmelding reactie terug naar de toepassing.

Zie voor meer informatie, [eenmalige aanmelding in de SAML-protocol](../develop/single-sign-on-saml-protocol.md).

### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Controleren of de claims die zijn uitgegeven in het SAML-token aanpassen

Wanneer een gebruiker wordt geverifieerd op de toepassing, Azure AD geeft de toepassing een SAML-token met informatie (of claims) over de gebruiker die een unieke identificatie ze. Deze informatie omvat standaard, gebruikersnaam, e-mailadres, voornaam en achternaam van de gebruiker.

Als u wilt weergeven of bewerken van de claims verzonden in het SAML-token naar de toepassing:

- Ga naar de **gebruikerskenmerken en Claims** kop en selecteer de **bewerken** pictogram. De **gebruikerskenmerken en Claims** pagina wordt weergegeven.

![Geeft de gebruikerspagina kenmerken en claims](./media/configure-single-sign-on-non-gallery-applications/user-attributes-and-claims.png)

Mogelijk moet u de claims in het SAML-token is uitgegeven voor twee redenen bewerken:

- De toepassing vereist een andere set claimen van URI's of claimwaarden.
- Uw toepassing vereist de **naam id-waarde** beweren te zijn dan de gebruikersnaam (ook wel bekend als de user principal name) die zijn opgeslagen in Microsoft identity-platform.

Zie voor meer informatie, [het: In het SAML-token voor bedrijfstoepassingen uitgegeven claims aanpassen](../develop/active-directory-saml-claims-customization.md).

### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Certificaat verlopen gegevens, de status en e-mailmelding

Wanneer u een galerie of een toepassing buiten de galerie maakt, maakt Azure AD een toepassingsspecifieke-certificaat dat is drie jaar na de aanmaakdatum verloopt. U moet dit certificaat voor het instellen van de vertrouwensrelatie tussen Azure AD en de toepassing. Zie voor meer informatie over de opmaak van het certificaat, SAML-documentatie van de toepassing.

Uit Azure AD, kunt u downloaden van het actieve certificaat in Base64 of Raw-indeling rechtstreeks vanuit het hoofdvenster **instellen van eenmalige aanmelding met SAML** pagina. U kunt ook het actieve certificaat ophalen door het downloaden van het XML-bestand van de toepassing-metagegevens of met behulp van de App-URL voor federatieve metagegevens.

Als u wilt weergeven, maken of uw certificaten (actief of inactief) downloaden, gaat u naar de **SAML-handtekeningcertificaat** kop en selecteer de **bewerken** pictogram. De **SAML-handtekeningcertificaat** wordt weergegeven.

![Ziet u de SAML-pagina voor ondertekenen certificaat](./media/configure-single-sign-on-non-gallery-applications/saml-signing-certificate.png)

Controleer of dat het certificaat heeft:

- *De gewenste vervaldatum.* U kunt de vervaldatum voor maximaal drie jaar configureren in de toekomst.
- *De status van actieve voor het gewenste certificaat.* Als de status **inactief**, wijzigt de status van de **Active**. U wijzigt de status, met de rechtermuisknop op het gewenste certificaat rij en selecteer **certificaat activeren**.
- *De juiste optie voor ondertekening en het algoritme.*
- *De juiste melding e-mailadressen.* Wanneer het actieve certificaat in de buurt van de vervaldatum is, verzendt Azure AD een melding naar het e-mailadres dat is geconfigureerd in dit veld.  

Zie voor meer informatie, [beheren van certificaten voor federatieve eenmalige aanmelding](manage-certificates-for-federated-single-sign-on.md) en [geavanceerde opties in het SAML-token voor Certificaatondertekening](certificate-signing-options.md).

### <a name="set-up-target-application"></a>Doeltoepassing instellen

Ga naar de documentatie van de toepassing voor het configureren van de toepassing voor eenmalige aanmelding. Als u de documentatie zoekt, gaat u naar de **instellen \<toepassingsnaam >** kop en selecteert u een **Stapsgewijze instructies bekijken over**. De documentatie wordt weergegeven in de **aanmelding configureren** pagina. Die pagina begeleidt u bij het invullen van de **aanmeldings-URL**, **Azure AD-id**, en **afmeldings-URL van** waarden in de **instellen \<toepassingsnaam >** kop.

De vereiste waarden variëren op basis van de toepassing. Zie voor meer informatie, SAML-documentatie van de toepassing. De **aanmeldings-URL** en **afmeldings-URL van** waarden beide worden omgezet naar hetzelfde eindpunt, die het SAML-eindpunt voor afhandeling van aanvragen voor uw exemplaar van Azure AD. De **Azure AD-id** is de waarde van de **verlener** in het SAML-token dat is uitgegeven voor de toepassing.

### <a name="assign-users-and-groups-to-your-saml-application"></a>Gebruikers en groepen toewijzen aan uw toepassing SAML

Wanneer u uw toepassing in Azure AD gebruiken als een SAML gebaseerde id-provider hebt geconfigureerd, is het bijna klaar om te testen. Als een beveiligingscontrole geeft Azure AD alleen een token zodat een gebruiker zich aanmelden bij de toepassing als Azure AD heeft toegang verleend aan de gebruiker. Gebruikers kunnen toegang krijgen, rechtstreeks of via een groepslidmaatschap.

Een nieuwe gebruiker of groep toewijzen aan uw toepassing:

1. Selecteer in de zijbalk toepassing **gebruikers en groepen**. De  **\<toepassingsnaam >-gebruikers en groepen** pagina wordt weergegeven, waarin de huidige lijst met toegewezen gebruikers en groepen.
1. Selecteer **gebruikers toevoegen**. De **toewijzingen toevoegen** pagina wordt weergegeven.
1. Selecteer **gebruikers en groepen (\<nummer > geselecteerde)** . De **gebruikers en groepen** pagina wordt weergegeven, waarin een lijst met beschikbare gebruikers en groepen.
1. Typ of blader te vinden van de gebruiker of groep die u wilt toewijzen in de lijst.
1. Selecteer elke gebruiker of groep die u wilt toevoegen en selecteer vervolgens de **Selecteer** knop. De **gebruikers en groepen** pagina verdwijnt.
1. In de **toewijzingen toevoegen** weergeeft, schakelt **toewijzen**. De  **\<toepassingsnaam >-gebruikers en groepen** pagina wordt weergegeven met de extra gebruikers wordt weergegeven in de lijst.

   ![Toont de pagina voor gebruikers en groepen van toepassingen](./media/configure-single-sign-on-non-gallery-applications/application-users-and-groups.png)

In deze lijst, kunt u het volgende doen:

- Een gebruiker verwijderen.
- De rol bewerken.
- Hun referenties (gebruikersnaam en wachtwoord) bijwerken zodat de gebruiker bij de toepassing uit in het toegangsvenster van de gebruiker verifiëren kan.

U kunt bewerken of verwijderen van meerdere gebruikers of groepen tegelijk.

Een gebruiker toewijzen, kunt Azure AD om uit te geven van de gebruiker een token. Het is ook ervoor zorgt dat een tegel voor deze toepassing wordt weergegeven in het toegangsvenster van de gebruiker. De tegel van een toepassing worden ook weergegeven in het startprogramma voor Office 365-toepassingen als Office 365 wordt gebruikt door de gebruiker.

> [!NOTE]
> U kunt een logo in tegel voor het gebruik van de toepassing uploadt de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing.

### <a name="test-the-saml-application"></a>De SAML-toepassing testen

Voordat u de SAML-toepassing test, moet u hebben al de toepassing met Azure AD instellen en gebruikers of groepen toegewezen aan de toepassing. Als u wilt testen van de SAML-toepassing, selecteert u **eenmalige aanmelding**, die gaat u terug naar de **SAML gebaseerde aanmelding** pagina. (Als een andere methode voor eenmalige aanmelding van kracht is, selecteert u **modi voor eenmalige aanmelding wijzigen** > **SAML** te.) Klik in de **Test eenmalige aanmelding met \<toepassingsnaam >** kop, selecteer **Test**. Zie voor meer informatie, [fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md).

## <a name="password-single-sign-on"></a>Wachtwoord eenmalige aanmelding

Selecteer deze optie voor het configureren van [wachtwoord gebaseerde eenmalige aanmelding](what-is-single-sign-on.md) voor een webtoepassing met een HTML-aanmeldingspagina opgeven. Wachtwoord gebaseerde SSO, ook wel aangeduid als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden voor webtoepassingen die bieden geen ondersteuning voor identiteitsfederatie. Het is ook handig voor scenario's waarin meerdere gebruikers één account moeten, zoals om accounts voor sociale media-app van uw organisatie te delen.

Nadat u hebt geselecteerd **op basis van wachtwoorden**, u wordt gevraagd om in te voeren van de URL van de toepassing web gebaseerde aanmeldingspagina opgeven.

![Ziet u de aanmeldings-URL-pagina de aanmeldings-URL invoeren](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

Voer deze stappen:

1. Voer de URL. Deze tekenreeks moet de pagina met het invoerveld gebruikersnaam.
1. Selecteer **Opslaan**. Azure AD wil parseren van de aanmeldingspagina voor een gebruikersnaam invoeren en een wachtwoord invoeren.
1. Als het parseren van Azure AD de poging mislukt, selecteert u **configureren \<toepassingsnaam > instellingen voor wachtwoord eenmalige aanmelding** om weer te geven de **aanmelding configureren** pagina. (Als de poging is gelukt, u kunt geen rekening gehouden met de rest van deze procedure.)
1. Selecteer **aanmeldingsvelden handmatig detecteren**. Aanvullende instructies met een beschrijving van de handmatige detectie van aanmelding velden worden weergegeven.

   ![Handmatige configuratie van wachtwoord gebaseerde eenmalige aanmelding](./media/configure-single-sign-on-non-gallery-applications/password-configure-sign-on.png)

1. Selecteer **aanmeldingsvelden vastleggen**. Een pagina van de status vastleggen in een nieuw tabblad van het bericht dat wordt geopend **metagegevens vastleggen wordt momenteel uitgevoerd**.
1. Als de **toegang Configuratiescherm-uitbreiding vereist** vak wordt weergegeven in een nieuw tabblad, selecteert u **nu installeren** voor het installeren van de **mijn Apps beveiligde aanmelding extensie** browserextensie. (De browserextensie vereist Microsoft Edge, Chrome of Firefox.) Vervolgens installeert, starten en de extensie inschakelen en vernieuw de pagina van de status vastleggen.

   De uitbreiding van de browser opent vervolgens een ander tabblad die de opgegeven URL wordt weergegeven.

1. Op het tabblad met de ingevoerde URL, gaat u door het proces voor aanmelden. Vul de velden gebruikersnaam en wachtwoord in en probeert aan te melden. (U hebt niet het juiste wachtwoord opgeven.)

   Wordt u gevraagd om op te slaan van de vastgelegde aanmeldingsvelden.

1. Selecteer **OK**. Het tabblad wordt gesloten, de uitbreiding van de browser werkt op de statuspagina vastleggen met het bericht **metagegevens zijn bijgewerkt voor de toepassing**, en die browser tabblad ook wordt gesloten.
1. In de Azure AD **aanmelding configureren** weergeeft, schakelt **Oké, ik kan aanmelden bij de app is**.
1. Selecteer **OK**.

Na het vastleggen van de aanmeldingspagina, kunt u gebruikers en groepen toewijzen en u kunt een referentie beleid net als bij normale instellen [wachtwoord SSO-toepassingen](what-is-single-sign-on.md).

> [!NOTE]
> U kunt een logo in tegel voor het gebruik van de toepassing uploadt de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing.

## <a name="existing-single-sign-on"></a>Bestaande eenmalige aanmelding

Selecteer deze optie om een koppeling naar de toepassing in uw organisatie Azure AD-Toegangsvenster of Office 365-portal. U kunt deze methode koppelingen wilt toevoegen aan aangepaste web-apps die momenteel gebruikmaken van Active Directory Federation Services (of een andere federation-service) gebruiken in plaats van Azure AD voor verificatie. Of u dieptekoppelingen naar specifieke SharePoint-pagina's of andere webpagina's die u zojuist hebt wilt weergeven op de Toegangsdeelvensters van uw gebruikers kunt toevoegen.

Nadat u hebt geselecteerd **gekoppelde**, u wordt gevraagd om in te voeren van de URL van de toepassing om aan te koppelen. Typ de URL en selecteer **opslaan**. U kunt gebruikers en groepen toewijzen aan de toepassing, die zorgt ervoor de toepassing wordt weergegeven dat in de [startprogramma voor Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of de [Azure AD-toegangspaneel](end-user-experiences.md) voor gebruikers.

> [!NOTE]
> U kunt een logo in tegel voor het gebruik van de toepassing uploadt de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing.

## <a name="related-articles"></a>Verwante artikelen:

- [Procedure: In het SAML-token voor bedrijfstoepassingen uitgegeven claims aanpassen](../develop/active-directory-saml-claims-customization.md)
- [Fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md)
- [Microsoft identity-platform (voorheen Azure Active Directory voor ontwikkelaars)](../develop/index.yml)
