---
title: Azure AD-eenmalige aanmelding configureren voor toepassingen | Microsoft Docs
description: Informatie over hoe verbinding maken met apps aan Azure Active Directory met behulp van SAML en eenmalige aanmelding op basis van wachtwoorden voor selfservice
services: active-directory
author: barbkess
documentationcenter: na
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asmalser,luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 928e4811f15953313df167375043231478f5c11a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054871"
---
# <a name="configure-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Configureren van eenmalige aanmelding voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie

In dit artikel gaat over een functie waarmee beheerders het configureren van eenmalige aanmelding voor toepassingen die niet aanwezig in de app-galerie van Azure Active Directory *zonder code te schrijven*. Deze functie werd uitgebracht van technical preview op 18 November 2015 en is opgenomen in [Azure Active Directory Premium](fundamentals/active-directory-whatis.md). Als u in plaats daarvan voor richtlijnen voor ontwikkelaars over het integreren van aangepaste apps met Azure AD via de programmacode, Zie [Verificatiescenario's voor Azure AD](develop/authentication-scenarios.md).

De galerie met Azure Active Directory biedt een overzicht van toepassingen die bekend zijn bij het ondersteunen van een vorm van eenmalige aanmelding met Azure Active Directory, zoals beschreven in [in dit artikel](manage-apps/what-is-single-sign-on.md). Zodra u (als een IT-specialist of system integrator in uw organisatie) hebt gevonden die de toepassing dat u verbinding wilt maken, u kunt aan de slag door de stapsgewijze instructies die zijn gepresenteerd in Azure portal om in te schakelen eenmalige aanmelding.

Klanten met [Azure Active Directory Premium](fundamentals/active-directory-whatis.md) licentie krijgt ook deze aanvullende mogelijkheden:

* Self-service-integratie van toepassingen die ondersteuning biedt voor SAML 2.0 id-providers (SP geïnitieerde of IdP gestart)
* Self-service-integratie van een webtoepassing die een op HTML gebaseerde aanmeldingspagina opgeven met behulp is [wachtwoord gebaseerde SSO](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on)
* Selfservice-verbinding van toepassingen die het protocol SCIM voor het inrichten van gebruikers gebruiken ([die hier worden beschreven](manage-apps/use-scim-to-provision-users-and-groups.md))
* Koppelingen toevoegen aan elke toepassing in de [startprogramma voor Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of de [Azure AD-toegangspaneel](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users)

Dit kunnen bijvoorbeeld niet alleen SaaS-toepassingen die u gebruikt, maar zijn niet nog is toegevoegd aan de galerie met Azure AD-toepassing, maar van derden webtoepassingen die uw organisatie is geïmplementeerd op servers die u, in de cloud of on-premises beheert.

Deze mogelijkheden, ook wel bekend als *sjablonen voor app-integratie*, op standaarden gebaseerde verbindingspunten bieden voor apps die ondersteuning bieden voor SAML, SCIM of op formulieren gebaseerde verificatie en flexibele opties en instellingen voor opnemen compatibiliteit met een groot aantal toepassingen. 

## <a name="adding-an-unlisted-application"></a>Toevoegen van een niet-vermelde toepassing
Als u wilt verbinding maken met een toepassing met behulp van een app-integratie-sjabloon, moet u zich aanmelden bij de Azure-portal met uw Azure Active Directory administrator-account. Blader naar de **Active Directory > bedrijfstoepassingen > nieuwe toepassing > niet in de galerij toepassing** sectie, selecteer **toevoegen**, en vervolgens **toevoegen van een toepassing uit de galerie** .

  ![](./media/active-directory-saas-custom-apps/customapp1.png)

In de app-galerie, kunt u een niet-vermelde app toevoegen door het selecteren van de **niet in de galerij toepassing** tegel die in de lijst met zoekresultaten wordt weergegeven als de gewenste app is niet gevonden. Na het invoeren van een naam voor uw toepassing, kunt u de opties voor eenmalige aanmelding en het gedrag configureren. 

**Snelle tip**: als een best practice, kunt u de zoekfunctie gebruiken om te controleren om te zien als de toepassing bestaat al in de toepassingengalerie. Als de app wordt gevonden en de beschrijving eenmalige aanmelding noemt, wordt klikt u vervolgens de toepassing al ondersteund voor federatieve eenmalige aanmelding.

  ![](./media/active-directory-saas-custom-apps/customapp2.png)

Toevoegen van een toepassing op deze manier biedt een vergelijkbare ervaring met de beschikbaar voor vooraf geïntegreerde toepassingen. Selecteer eerst **configureren Single Sign-On** of klik op **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing. Het volgende scherm geeft de opties voor het configureren van eenmalige aanmelding. De opties worden in de volgende secties van dit artikel beschreven.
  
![](./media/active-directory-saas-custom-apps/customapp3.png)

## <a name="saml-based-single-sign-on"></a>Eenmalige aanmelding op basis van SAML
Selecteer deze optie om te configureren op basis van SAML-verificatie voor de toepassing. Dit is vereist dat de toepassing SAML 2.0 ondersteunt. U dient informatie te verzamelen over het gebruik van de SAML-mogelijkheden van de toepassing voordat u doorgaat. Voer de volgende secties voor het configureren van eenmalige aanmelding in tussen de toepassing en Azure AD.

### <a name="enter-basic-saml-configuration"></a>Voer de SAML-basisconfiguratie

Als u Azure AD instelt, voer de basisconfiguratie van SAML. U kunt handmatig de waarden invoeren of upload een bestand met metagegevens om op te halen van de waarde van de velden.

  ![Litware domein en URL 's](./media/active-directory-saas-custom-apps/customapp4.png)

- **Meld u URL (SP geïnitieerde alleen)** – wanneer de gebruiker naartoe om aan te melden bij deze toepassing. Als de toepassing is geconfigureerd voor het uitvoeren van service provider geïnitieerde eenmalige aanmelding, en wanneer een gebruiker naar deze URL navigeert, de serviceprovider naar Azure AD om te verifiëren en meld u aan de gebruiker in de benodigde omleiding doet. Als dit veld is ingevuld, klikt u vervolgens Azure AD gebruikt deze URL om de toepassing uit Office 365 en Azure AD-Toegangsvenster te starten. Als dit veld wordt weggelaten, wordt de id-provider in plaats daarvan door Azure AD uitvoeren wordt-aanmelding wordt gestart wanneer de app wordt gestart vanuit Office 365, de Azure AD-Toegangsvenster, of vanuit de Azure AD aanmeldings-URL met eenmalige (kopieerbare op het tabblad Dashboard).
- **Id** -moet uniek zijn voor de toepassing waarvoor eenmalige aanmelding wordt geconfigureerd. U vindt deze waarde als het element verlener in de AuthRequest (SAML-aanvraag) die is verzonden door de toepassing. Deze waarde wordt ook weergegeven als de **entiteit-ID** in alle SAML-metagegevens geleverd door de toepassing. Controleer de SAML-documentatie voor meer informatie over wat de entiteit-ID of doelgroep waarde is van de toepassing. 

    Hier volgt een voorbeeld van hoe de id of de uitgever wordt weergegeven in de SAML-aanvraag verzonden door de toepassing naar Azure AD:

    ```
    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>
    ```

- **Antwoord-URL** -de antwoord-URL is waar de toepassing verwacht voor het ontvangen van het SAML-token. Dit is ook de URL Assertion Consumer Service (ACS) genoemd. Controleer de SAML-documentatie voor meer informatie over wat de token SAML-antwoord-URL of ACS-URL is van de toepassing. 

    U kunt de volgende PowerShell-script gebruiken voor het configureren van meerdere afgestemd.

    ```PowerShell
    $sp = Get-AzureADServicePrincipal -SearchString "<Exact App  name>"
    $app = Get-AzureADApplication -SearchString "<Exact app name>"
    Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls "<ReplyURLs>"
    Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls "<ReplyURLs>"
    ```

Zie voor meer informatie, [SAML 2.0-verificatieaanvragen en antwoorden die ondersteuning biedt voor Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML)


### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Controleren of de claims die zijn uitgegeven in het SAML-token aanpassen

Wanneer een gebruiker wordt geverifieerd op de toepassing, wordt Azure AD een SAML-token naar de app die gegevens (of claims) bevat over de gebruiker die een unieke identificatie te geven. Dit omvat standaard gebruikersnaam, e-mailadres, voornaam en achternaam van de gebruiker. 

U kunt weergeven of bewerken van de claims die zijn verzonden in het SAML-token naar de toepassing onder de **kenmerken** tabblad.

  ![](./media/active-directory-saas-custom-apps/customapp7.png)

Er zijn twee redenen waarom u mogelijk de uitgegeven claims in het SAML-token bewerken:

- De toepassing is naar een andere set claim URI's is vereist of claimwaarden geschreven.
- Uw toepassing is geïmplementeerd op een manier die de claim NameIdentifier dan de gebruikersnaam (AKA gebruiker principal name) die zijn opgeslagen in Azure Active Directory vereist. 

Zie voor meer informatie, [in het SAML-token voor bedrijfstoepassingen uitgegeven claims aanpassen](./develop/active-directory-saml-claims-customization.md). 



### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Certificaat verlopen gegevens, de status en e-mailmelding

Wanneer u een galerie of een toepassing die niet in de galerij maakt, maakt Azure AD een toepassingsspecifieke-certificaat met een vervaldatum van drie jaar na de aanmaakdatum. U moet dit certificaat voor het instellen van de vertrouwensrelatie tussen Azure AD en de toepassing. Zie voor meer informatie over de opmaak van het certificaat, SAML-documentatie van de toepassing. 

U kunt het certificaat in Base64 of onbewerkte indeling downloaden uit Azure AD. Bovendien kunt u het certificaat krijgen door het downloaden van het XML-bestand van de toepassing-metagegevens of met behulp van de App-URL voor federatieve metagegevens.

  ![Certificaat](./media/active-directory-saas-custom-apps/certificate.png)

Controleer of dat het certificaat heeft:

- De gewenste vervaldatum. U kunt configureren dat de vervaldatum voor maximaal drie jaar.
- De status van actieve. Als de status niet actief is, wijzigt u de status actief. U wijzigt de status, Controleer **Active** en sla vervolgens de configuratie. 
- De juiste meldingse-mail. Als het actieve certificaat in de buurt van de vervaldatum is wordt Azure AD een melding verzenden naar het e-mailadres dat is geconfigureerd in dit veld.  

Zie voor meer informatie, [beheren van certificaten voor federatieve eenmalige aanmelding in Azure Active Directory](manage-apps/manage-certificates-for-federated-single-sign-on.md).

### <a name="set-up-target-application"></a>Doeltoepassing instellen

Ga naar de documentatie van de toepassing voor het configureren van de toepassing voor eenmalige aanmelding. Als u wilt zoeken in de documentatie, blader naar het einde van de configuratie voor SAML gebaseerde aanmelding-pagina en klik vervolgens op **configureren <application name>** . 

De vereiste waarden variëren op basis van de toepassing. Zie voor meer informatie, SAML-documentatie van de toepassing. De aanmeldings- en afmelden service-URL worden beide omgezet naar hetzelfde eindpunt, die het SAML-eindpunt voor afhandeling van aanvragen voor uw exemplaar van Azure AD. De SAML-entiteit-ID is de waarde die wordt weergegeven als de verlener in het SAML-token dat is uitgegeven voor de toepassing.


### <a name="assign-users-and-groups-to-your-saml-application"></a>Gebruikers en groepen toewijzen aan uw toepassing SAML

Nadat uw toepassing is geconfigureerd voor het gebruik van Azure AD als een SAML gebaseerde id-provider, is het bijna klaar om te testen. Als een besturingselement voor beveiliging, Azure AD een token zodat een gebruiker zich aanmeldt bij de toepassing, tenzij de Azure AD heeft toegang verleend aan de gebruiker niet uitgeven. Gebruikers kunnen toegang verleend rechtstreeks of via een groepslidmaatschap. 

Als u wilt een gebruiker of groep toewijzen aan uw toepassing, klikt u op de **gebruikers toewijzen** knop. Selecteer de gebruiker of groep die u wilt toewijzen en selecteer vervolgens de **toewijzen** knop.

  ![](./media/active-directory-saas-custom-apps/customapp6.png)

Een gebruiker toewijst, kunnen Azure AD om uit te geven van een token voor de gebruiker. Het is ook ervoor zorgt dat een tegel voor deze toepassing wordt weergegeven in het toegangsvenster van de gebruiker. De tegel van een toepassing wordt ook weergegeven in het startprogramma voor Office 365-toepassingen als Office 365 wordt gebruikt door de gebruiker. 

> [!NOTE] 
> U kunt een logo in tegel voor het gebruik van de toepassing uploadt de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 


### <a name="test-the-saml-application"></a>De SAML-toepassing testen

Voordat u de SAML-toepassing test, moet u hebben de toepassing met Azure AD instellen en gebruikers of groepen toegewezen aan de toepassing. Als u wilt testen van de SAML-aanvraag, Zie [fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](develop/howto-v1-debug-saml-sso-issues.md).

## <a name="password-single-sign-on"></a>Wachtwoord eenmalige aanmelding

Selecteer deze optie voor het configureren van [wachtwoord gebaseerde eenmalige aanmelding](manage-apps/what-is-single-sign-on.md) voor een webtoepassing met een HTML-aanmeldingspagina opgeven. Wachtwoord gebaseerde SSO, ook wel aangeduid als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden voor webtoepassingen die bieden geen ondersteuning voor identiteitsfederatie. Het is ook handig voor scenario's waarin meerdere gebruikers één account moeten, zoals om accounts voor sociale media-app van uw organisatie te delen. 

Na het selecteren van **volgende**, wordt u gevraagd om in te voeren van de URL van de toepassing web gebaseerde aanmeldingspagina opgeven. Houd er rekening mee dat dit moet de pagina waarop de gebruikersnaam en wachtwoord invoervelden. Nadat opgegeven, wordt Azure AD een proces voor het parseren van de aanmeldingspagina voor een gebruikersnaam invoeren en een wachtwoord invoeren. Als het proces niet geslaagd is, klikt u vervolgens leidt deze u door een ander proces van het installeren van een browserextensie (vereist Internet Explorer, Chrome of Firefox) waarmee u de velden handmatig vastleggen.

Zodra de aanmeldingspagina wordt vastgelegd, gebruikers en groepen kunnen worden toegewezen en referentie-beleidsregels kunnen worden ingesteld net als bij normale [wachtwoord SSO apps](manage-apps/what-is-single-sign-on.md).

> [!NOTE] 
> U kunt een logo in tegel voor het gebruik van de toepassing uploadt de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 
>

## <a name="existing-single-sign-on"></a>Bestaande eenmalige aanmelding
Selecteer deze optie om een koppeling naar een toepassing van uw organisatie Azure AD-Toegangsvenster of Office 365-Portal. U kunt deze koppelingen wilt toevoegen aan aangepaste web-apps die momenteel gebruikmaken van Azure Active Directory Federation Services (of een andere federation-service) gebruiken in plaats van Azure AD voor verificatie. Of u dieptekoppelingen naar specifieke SharePoint-pagina's of andere webpagina's die u zojuist hebt wilt weergeven op de Toegangsdeelvensters van uw gebruikers kunt toevoegen. 

Na het selecteren van **volgende**, wordt u gevraagd om in te voeren van de URL van de toepassing om aan te koppelen. Als voltooid, gebruikers en groepen kunnen worden toegewezen aan de toepassing, die zorgt ervoor de toepassing wordt weergegeven dat in de [startprogramma voor Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of de [Azure AD-toegangspaneel](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) voor gebruikers.

> [!NOTE] 
> U kunt een logo in tegel voor het gebruik van de toepassing uploadt de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 
>

## <a name="related-articles"></a>Gerelateerde artikelen

- [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
- [In het SAML-Token voor vooraf geïntegreerde Apps uitgegeven Claims aanpassen](active-directory-saml-claims-customization.md)
- [Oplossen van problemen met SAML gebaseerde eenmalige aanmelding](active-directory-saml-debugging.md)

