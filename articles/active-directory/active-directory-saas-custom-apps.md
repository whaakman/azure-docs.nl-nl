---
title: Azure AD-eenmalige aanmelding configureren voor toepassingen | Microsoft Docs
description: Meer informatie over hoe self-service verbonden apps met Azure Active Directory met behulp van SAML- en eenmalige aanmelding op basis van wachtwoorden
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 0d42eb0c-6d3f-4557-9030-e88e86709a19
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/20/2018
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c115414fc6ae599342192196e256597c28e5aa6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293191"
---
# <a name="configure-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Eenmalige aanmelding tot toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie configureren

Dit artikel is over een functie waarmee beheerders voor het configureren van eenmalige aanmelding tot toepassingen die niet aanwezig in de app-galerie van Azure Active Directory *zonder code te schrijven*. Deze functie werd uitgebracht van technical preview op 18 November 2015 en is opgenomen in [Azure Active Directory Premium](active-directory-whatis.md). Als u in plaats daarvan-handleiding voor ontwikkelaars over het integreren van aangepaste apps met Azure AD via code zoekt, Zie [verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md).

De Azure Active Directory-toepassingsgalerie biedt een overzicht van toepassingen die bekend zijn bij het ondersteunen van een vorm van eenmalige aanmelding bij Azure Active Directory, zoals beschreven in [in dit artikel](manage-apps/what-is-single-sign-on.md). Als u (als een IT infrastructuurspecialist of system integrator in uw organisatie) hebt gevonden dat de toepassing die u wilt verbinden, u kunt aan de slag door de stapsgewijze instructies die zijn gepresenteerd in de Azure-portal voor eenmalige aanmelding inschakelen.

Klanten met [Azure Active Directory Premium](active-directory-whatis.md) licentie ook krijgen deze aanvullende mogelijkheden:

* Selfservice integratie van alle toepassingen die ondersteuning biedt voor SAML 2.0 id-providers (Serviceprovider geïnitieerde of IdP gestart)
* Integratie van een webtoepassing met een op basis van een HTML-aanmeldingspagina met selfservice [eenmalige aanmelding op basis van wachtwoorden](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on)
* Verbinding van toepassingen die gebruikmaken van het protocol SCIM voor gebruikers inrichten selfservice ([hier beschreven](manage-apps/use-scim-to-provision-users-and-groups.md))
* Koppelingen toevoegen aan elke toepassing in de [Office 365 app linksboven](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of de [deelvenster Azure AD access](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users)

Dit kunnen bijvoorbeeld niet alleen SaaS-toepassingen die u gebruikt, maar hebben niet nog is geïntegreerde naar de galerie van Azure AD-toepassing, maar van derden webtoepassingen die uw organisatie is geïmplementeerd op servers die u, in de cloud of on-premises beheert.

Deze mogelijkheden, ook wel bekend als *app integratie sjablonen*, geeft u op standaarden gebaseerde verbindingspunten voor apps die ondersteuning bieden voor SAML, SCIM of verificatie op basis van formulieren en flexibele opties en instellingen voor compatibiliteit met een groot aantal toepassingen bevatten. 

## <a name="adding-an-unlisted-application"></a>Een niet-vermelde toepassing toevoegen
Als u wilt verbinding maken met een toepassing met een app-integratie-sjabloon, moet u zich aanmelden bij de Azure-portal met de administrator-account van uw Azure Active Directory. Blader naar de **Active Directory > bedrijfstoepassingen > nieuwe toepassing > niet-galerie toepassing** sectie **toevoegen**, en vervolgens **een toepassing uit de galerie toevoegen** .

  ![](./media/active-directory-saas-custom-apps/customapp1.png)

In de app-galerie, kunt u een niet-vermelde app toevoegen door het selecteren van de **Non-galerie toepassing** tegel die in de zoekresultaten wordt weergegeven als de gewenste app is niet gevonden. Na het invoeren van een naam voor uw toepassing, kunt u de opties voor eenmalige aanmelding en het gedrag configureren. 

**Snelle tip**: als een best practice, gebruikt u de zoekfunctie controleert u of de toepassing al in de galerie met toepassingen bestaat. Als de app wordt gevonden en de beschrijving eenmalige aanmelding noemt, wordt de toepassing al voor federatieve eenmalige aanmelding ondersteund.

  ![](./media/active-directory-saas-custom-apps/customapp2.png)

Toevoegen van een toepassing op deze manier biedt een vergelijkbare ervaring met de beschikbaar voor vooraf geïntegreerde toepassingen. Als u wilt starten, selecteert u **configureren Single Sign-On** of klik op **eenmalige aanmelding** in de toepassing linkermenubalk menu. Het volgende scherm geeft de opties voor het configureren van eenmalige aanmelding. De opties worden in de volgende secties van dit artikel beschreven.
  
![](./media/active-directory-saas-custom-apps/customapp3.png)

## <a name="saml-based-single-sign-on"></a>Eenmalige aanmelding op basis van SAML
Selecteer deze optie voor het configureren van SAML-verificatie voor de toepassing. Dit is vereist dat de toepassing SAML 2.0 ondersteunt. U dient informatie te verzamelen over het gebruik van de SAML-mogelijkheden van de toepassing voordat u doorgaat. Voltooi de volgende secties voor het configureren van eenmalige aanmelding tussen de toepassing en de Azure AD.

### <a name="enter-basic-saml-configuration"></a>Voer de SAML-basisconfiguratie

Voer de basisconfiguratie van SAML voordat u Azure AD kunt instellen. U kunt handmatig invoeren van de waarden of uploaden van een bestand met metagegevens om op te halen van de waarde van de velden.

  ![Litware domein en de URL 's](./media/active-directory-saas-custom-apps/customapp4.png)

- **Meld u op de URL (Serviceprovider geïnitieerde alleen)** – wanneer de gebruiker naar aanmelden bij deze toepassing gaat. Als de toepassing is geconfigureerd voor het uitvoeren van service provider geïnitieerde eenmalige aanmelding, en wanneer een gebruiker naar deze URL navigeert, de serviceprovider doet de benodigde omleiding naar Azure AD om te verifiëren en meld u aan de gebruiker in. Als dit veld is ingevuld, klikt u vervolgens Azure AD gebruikt deze URL om de toepassing van Office 365 en de Azure AD-Toegangsvenster te starten. Als dit veld wordt weggelaten, wordt de id-provider in plaats daarvan door Azure AD uitvoeren wordt-aanmelding wordt gestart wanneer de app wordt gestart vanuit Office 365, het Azure AD-Toegangsvenster of vanuit de Azure AD één aanmeldings-URL (copyable op het tabblad Dashboard).
- **Id** -moet een unieke identificatie van de toepassing waarvoor eenmalige aanmelding wordt geconfigureerd. U vindt deze waarde als het element certificaatverlener in de AuthRequest (SAML-aanvraag) verzonden door de toepassing. Deze waarde wordt ook weergegeven als de **entiteit-ID** in een SAML-metagegevens geleverd door de toepassing. Controleer de SAML-documentatie voor meer informatie over wat de waarde van de entiteit-ID of doelgroep is van de toepassing. 

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

- **Antwoord-URL** -de antwoord-URL is waar de toepassing voor het ontvangen van het SAML-token verwacht. Dit is ook de URL van de verklaring Consumer Service (ACS) genoemd. Controleer de SAML-documentatie voor meer informatie over de SAML-token antwoord of ACS de URL van de toepassing. 

    U kunt de volgende PowerShell-script gebruiken voor het configureren van meerdere replyURLs.

    ```PowerShell
    $sp = Get-AzureADServicePrincipal -SearchString "<Exact App  name>"
    $app = Get-AzureADApplication -SearchString "<Exact app name>"
    Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls "<ReplyURLs>"
    Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls "<ReplyURLs>"
    ```

Zie voor meer informatie [SAML 2.0-verificatieaanvragen en -antwoorden die ondersteuning biedt voor Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML)


### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Bekijken of aanpassen van de uitgegeven claims in het SAML-token

Wanneer een gebruiker zich bij de toepassing verifieert, doet Azure AD een SAML-token bij de app die informatie (of claims) bevat over de gebruiker die uniek wordt geïdentificeerd. Dit omvat standaard gebruikersnaam, e-mailadres, de voornaam en achternaam van de gebruiker. 

U kunt weergeven of bewerken van de claims die wordt verzonden in het SAML-token naar de toepassing onder de **kenmerken** tabblad.

  ![](./media/active-directory-saas-custom-apps/customapp7.png)

Er zijn twee redenen waarom moet u mogelijk de uitgegeven claims in het SAML-token bewerken:

- De toepassing is geschreven naar een andere set claimregels URI's is vereist of claimwaarden.
- Uw toepassing is geïmplementeerd op een manier die de NameIdentifier claim iets anders dan de gebruikersnaam (AKA UPN-naam) opgeslagen in Azure Active Directory vereist. 

Zie voor meer informatie [uitgegeven claims in het SAML-token voor bedrijfstoepassingen aanpassen](./develop/active-directory-saml-claims-customization.md). 



### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Controleer de certificaatgegevens van verlopen, status en e-mailmeldingen

Wanneer u een galerie of een niet-galerie-toepassing maakt, maakt Azure AD een toepassingsspecifieke certificaat met een vervaldatum van 3 jaar na de datum van maken. U moet dit certificaat voor het instellen van de vertrouwensrelatie tussen Azure AD en de toepassing. Zie voor informatie over de indeling van het certificaat, SAML-documentatie van de toepassing. 

U kunt het certificaat in Base64 of onbewerkte indeling downloaden vanuit Azure AD. Bovendien kunt u het certificaat ophalen door het downloaden van het toepassingsbestand van de metagegevens-XML of met behulp van de App-URL voor federatieve metagegevens.

  ![Certificaat](./media/active-directory-saas-custom-apps/certificate.png)

Controleer of dat het certificaat heeft:

- De gewenste vervaldatum. U kunt de vervaldatum voor maximaal drie jaar.
- De status van actieve. Als de status niet actief is, moet u de status wijzigen in actief. U wijzigt de status, Controleer **Active** en sla de configuratie. 
- De juiste meldingse-mail. Wanneer het actieve certificaat in de buurt van de vervaldatum is, stuurt Azure AD een melding naar het e-mailadres dat is geconfigureerd in dit veld.  

Zie voor meer informatie [beheren van certificaten voor federatieve eenmalige aanmelding bij Azure Active Directory](manage-apps/manage-certificates-for-federated-single-sign-on.md).

### <a name="set-up-target-application"></a>Doeltoepassing instellen

Zoek de documentatie van de toepassing voor het configureren van de toepassing voor eenmalige aanmelding. Als u wilt zoeken in de documentatie, Ga naar het einde van de pagina op basis van SAML eenmalige aanmelding en klik vervolgens op **configureren <application name>** . 

De vereiste waarden zijn afhankelijk van de toepassing. Zie voor meer informatie SAML-documentatie van de toepassing. De aanmelding en afmelding plaatsvindt service-URL worden beide omgezet naar hetzelfde eindpunt dat het eindpunt van SAML verwerken van aanvragen voor uw exemplaar van Azure AD. De SAML-entiteit-ID is de waarde die wordt weergegeven als de uitgever in het SAML-token dat is uitgegeven aan de toepassing.


### <a name="assign-users-and-groups-to-your-saml-application"></a>Gebruikers en groepen toewijzen aan uw SAML-toepassing

Als uw toepassing is geconfigureerd voor het gebruik van Azure AD als een op basis van SAML-identiteitsprovider, is het bijna klaar om te testen. Als een beveiligingscontrole geeft Azure AD geen token van een zodat een gebruiker zich aanmeldt bij de toepassing, tenzij u Azure AD heeft toegang verleend aan de gebruiker. Gebruikers kunnen toegang verleend rechtstreeks of via een groepslidmaatschap. 

Als u wilt een gebruiker of groep toewijzen aan uw toepassing, klikt u op de **gebruikers toewijzen** knop. Selecteer de gebruiker of groep die u wilt toewijzen en selecteer vervolgens de **toewijzen** knop.

  ![](./media/active-directory-saas-custom-apps/customapp6.png)

Een gebruiker toewijzen kunt Azure AD het uitgeven van een token voor de gebruiker. Het is ook ervoor zorgt dat een tegel voor deze toepassing worden weergegeven in het deelvenster van de toegang van de gebruiker. De tegel van een toepassing wordt ook weergegeven in het startprogramma voor toepassingen van Office 365 als de gebruiker via de Office 365. 

> [!NOTE] 
> U kunt uploaden een logo in tegel voor de toepassing met de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 


### <a name="test-the-saml-application"></a>De SAML-toepassing testen

Voordat u de SAML-toepassing test, moet u de toepassing met Azure AD instellen en toegewezen gebruikers of groepen aan de toepassing.

  ![Testen](./media/active-directory-saas-custom-apps/testing.png)

Klik op de pagina voor eenmalige aanmelding op **Test SAML instellingen** onder de sectie domein en de URL's. Hiermee opent u een inhoudsvenster met instructies voor het testen van de toepassing.

1. Aanmelden bij de toepassing. Als de toepassing is geconfigureerd als de service provider geïnitieerde eenmalige aanmelding, wordt u omgeleid naar de URL met eenmalige aanmelding waarbij u de aanmeldingspagina kan initiëren. Als de toepassing is geconfigureerd als identiteit serviceprovider geïnitieerde eenmalige aanmelding, klikt u zijn aangemeld bij de toepassing.
2.  Als er een fout in uw bedrijf-aanmeldingspagina, kopieert u de fout en Ga terug naar Azure AD eenmalige aanmelding inhoudsvenster testen. Plak de volgende fout in het vak en klik op **ophalen Oplossingsstappen**. Als de fout zich op de pagina van de toepassing, moet u contact op met de leverancier van de toepassing en het delen van uw configuratie op Azure AD voor het valideren van de waarden. 
3.  Azure AD levert op basis van de fout, specifieke stappen over het oplossen van het probleem.

Zie voor meer informatie [fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-saml-debugging/?WT.mc_id=DMC_AAD_?WT.mc_id=UI_AAD_Configure_NonGalleryApps)


## <a name="password-single-sign-on"></a>Eenmalige aanmelding wachtwoord

Selecteer deze optie voor het configureren van [op basis van wachtwoorden eenmalige aanmelding](manage-apps/what-is-single-sign-on.md) voor een webtoepassing met een HTML-aanmeldingspagina. Op basis van wachtwoorden SSO, ook bekend als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden tot webtoepassingen die geen ondersteuning voor identiteitsfederatie. Het is ook nuttig voor scenario's waarin meerdere gebruikers één account moeten, zoals delen op uw organisatie sociale media app accounts. 

Na het selecteren van **volgende**, wordt u gevraagd naar de URL van de toepassing web gebaseerde aanmeldingspagina opgeven. Houd er rekening mee dat dit de pagina met de gebruikersnaam en wachtwoord invoervelden moet zijn. Zodra u hebt ingevoerd, wordt in Azure AD een proces voor het parseren van de aanmeldingspagina voor een gebruikersnaam invoeren en een wachtwoord invoeren gestart. Als het proces niet geslaagd is, klikt u vervolgens leidt het u door een alternatief proces van het installeren van een Browseruitbreiding (vereist Internet Explorer, Chrome of Firefox) waarmee u de velden handmatig vastleggen.

Zodra de aanmeldingspagina wordt vastgelegd, gebruikers en groepen kunnen worden toegewezen en referentie beleidsregels kunnen worden ingesteld net als normale [wachtwoord SSO apps](manage-apps/what-is-single-sign-on.md).

> [!NOTE] 
> U kunt uploaden een logo in tegel voor de toepassing met de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 
>

## <a name="existing-single-sign-on"></a>Bestaande eenmalige aanmelding
Selecteer deze optie om een koppeling toevoegen aan een toepassing als uw organisatie Azure AD-Toegangsvenster of Office 365-portal. U kunt deze koppelingen wilt toevoegen aan aangepaste web-apps die momenteel gebruikmaken van Azure Active Directory Federation Services (of een andere federatieservice) gebruiken in plaats van Azure AD voor verificatie. Of u kunt dieptekoppelingen toevoegen aan de specifieke SharePoint-pagina's of andere webpagina's die u zojuist hebt wilt op van uw gebruikers toegang panelen worden weergegeven. 

Na het selecteren van **volgende**, wordt u gevraagd de URL van de toepassing koppelen aan invoeren. Zodra de voltooid, gebruikers en groepen kunnen worden toegewezen aan de toepassing die ervoor zorgt de toepassing wordt weergegeven dat de [Office 365 app linksboven](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of de [Azure AD-Toegangsvenster](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) voor gebruikers.

> [!NOTE] 
> U kunt uploaden een logo in tegel voor de toepassing met de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 
>

## <a name="related-articles"></a>Verwante artikelen

- [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
- [Het aanpassen van uitgegeven Claims in het SAML-Token voor vooraf geïntegreerde Apps](active-directory-saml-claims-customization.md)
- [Het oplossen van problemen op basis van SAML eenmalige aanmelding](active-directory-saml-debugging.md)

