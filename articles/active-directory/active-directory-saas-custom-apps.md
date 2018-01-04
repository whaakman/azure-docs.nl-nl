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
ms.date: 07/20/2017
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cedba7397e29cb397560c65a2408cd27442ec01c
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Configuring single sign-on to applications that are not in the Azure Active Directory application gallery (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
Dit artikel is over een functie waarmee beheerders voor het configureren van eenmalige aanmelding tot toepassingen die niet aanwezig in de app-galerie van Azure Active Directory *zonder code te schrijven*. Deze functie werd uitgebracht van technical preview op 18 November 2015 en is opgenomen in [Azure Active Directory Premium](active-directory-editions.md). Als u in plaats daarvan-handleiding voor ontwikkelaars over het integreren van aangepaste apps met Azure AD via code zoekt, Zie [verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md).

De Azure Active Directory-toepassingsgalerie biedt een overzicht van toepassingen die bekend zijn bij het ondersteunen van een vorm van eenmalige aanmelding bij Azure Active Directory, zoals beschreven in [in dit artikel](active-directory-appssoaccess-whatis.md). Als u (als een IT infrastructuurspecialist of system integrator in uw organisatie) hebt gevonden dat de toepassing die u wilt verbinden, u kunt aan de slag door Volg de stapsgewijze instructies die zijn gepresenteerd in de Azure-beheerportal voor eenmalige aanmelding inschakelen.

Klanten met [Azure Active Directory Premium](active-directory-editions.md) licenties ook krijgen deze aanvullende mogelijkheden:

* Selfservice integratie van alle toepassingen die ondersteuning biedt voor SAML 2.0 id-providers (Serviceprovider geïnitieerde of IdP gestart)
* Integratie van een webtoepassing met een op basis van een HTML-aanmeldingspagina met selfservice [eenmalige aanmelding op basis van wachtwoorden](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Verbinding van toepassingen die gebruikmaken van het protocol SCIM voor gebruikers inrichten selfservice ([hier beschreven](active-directory-scim-provisioning.md))
* Koppelingen toevoegen aan elke toepassing in de [Office 365 app linksboven](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of de [deelvenster Azure AD access](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

Dit kunnen bijvoorbeeld niet alleen SaaS-toepassingen die u gebruikt, maar hebben niet nog is geïntegreerde naar de galerie van Azure AD-toepassing, maar van derden webtoepassingen die uw organisatie is geïmplementeerd op servers die u, in de cloud of on-premises beheert.

Deze mogelijkheden, ook wel bekend als *app integratie sjablonen*, geeft u op standaarden gebaseerde verbindingspunten voor apps die ondersteuning bieden voor SAML, SCIM of verificatie op basis van formulieren en flexibele opties en instellingen voor compatibiliteit met een groot aantal toepassingen bevatten. 

## <a name="adding-an-unlisted-application"></a>Een niet-vermelde toepassing toevoegen
Als u wilt verbinding maken met een toepassing met een app-sjabloon voor integratie, meld u aan bij de Azure-beheerportal met administrator-account van uw Azure Active Directory en blader naar de **Active Directory > [Directory] > toepassingen** sectie **toevoegen**, en vervolgens **toevoegen van een toepassing uit de galerie**. 

![][1]

U kunt in de app-galerie toevoegen een niet-vermelde app met behulp van de **aangepaste** categorie aan de linkerkant of door te selecteren de **een niet-vermelde toepassing toevoegen** koppeling die in de zoekresultaten wordt weergegeven als de gewenste app is niet gevonden. Na het invoeren van een naam voor uw toepassing, kunt u de opties voor eenmalige aanmelding en het gedrag configureren. 

**Snelle tip**: als een best practice, gebruikt u de zoekfunctie controleert u of de toepassing al in de galerie met toepassingen bestaat. Als de app wordt gevonden en de beschrijving 'eenmalige aanmelding' en vervolgens de toepassing noemt wordt al voor federatieve eenmalige aanmelding ondersteund. 

![][2]

Toevoegen van een toepassing op deze manier biedt een zeer vergelijkbare ervaring met de beschikbaar voor vooraf geïntegreerde toepassingen. Als u wilt starten, selecteert u **configureren Single Sign-On**. Het volgende scherm geeft de volgende drie opties voor het configureren van eenmalige aanmelding op, die worden beschreven in de volgende secties.

![][3]

## <a name="azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding
Selecteer deze optie voor het configureren van SAML-verificatie voor de toepassing. Dit is vereist dat de toepassingsondersteuning SAML 2.0, en u informatie te over het gebruik van de SAML-mogelijkheden van de toepassing verzamelen dient voordat u doorgaat. Na het selecteren van **volgende**, wordt u gevraagd drie verschillende URL's die overeenkomt met de SAML-eindpunten voor de toepassing invoeren. 

![][4]

Dit zijn:

* **Meld u op de URL (Serviceprovider geïnitieerde alleen)** – wanneer de gebruiker naar aanmelden bij deze toepassing gaat. Als de toepassing is geconfigureerd voor het uitvoeren van service provider geïnitieerde eenmalige aanmelding op vervolgens wanneer een gebruiker naar deze URL navigeert, doet de serviceprovider de benodigde omleiding naar Azure AD om te verifiëren en meld u aan de gebruiker in. Als dit veld is ingevuld, klikt u vervolgens Azure AD gebruikt deze URL om de toepassing van Office 365 en de Azure AD-Toegangsvenster te starten. Als dit veld wordt weggelaten, wordt de id-provider in plaats daarvan door Azure AD uitvoeren wordt-geïnitieerd aanmelding wanneer de app wordt gestart vanuit Office 365, het Azure AD-Toegangsvenster of vanuit de Azure AD eenmalige aanmeldings-URL (copyable op het tabblad Dashboard).
* **URL-verlener** -de uitgevers-URL moet een unieke identificatie van de toepassing voor welke één aanmelding wordt geconfigureerd. Dit is de waarde die Azure AD teruggestuurd naar de toepassing als wordt de **doelgroep** wordt verwacht dat de parameter van het SAML-token en de toepassing te valideren. Deze waarde wordt ook weergegeven als de **entiteit-ID** in een SAML-metagegevens geleverd door de toepassing. Raadpleeg de documentatie van de toepassing SAML voor meer informatie over wat het entiteit-ID is of waarde van de doelgroep is. Hieronder volgt een voorbeeld van hoe de URL van de doelgroep in het SAML-token geretourneerd naar de toepassing wordt weergegeven:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Antwoord-URL** -de antwoord-URL is waar de toepassing voor het ontvangen van het SAML-token verwacht. Dit wordt ook wel de **Assertion Consumer Service (ACS) URL**. Controleer de SAML-documentatie voor meer informatie over de SAML-token antwoord of ACS de URL van de toepassing.
  Nadat deze zijn ingevoerd, klikt u op **volgende** om door te gaan naar het volgende scherm. Dit scherm bevat informatie over wat er worden geconfigureerd aan de kant van de toepassing moet te kunnen worden geaccepteerd van een SAML-token van Azure AD. 

![][5]

Waarden die zijn vereist variëren afhankelijk van de toepassing, dus controleer SAML-documentatie van de toepassing voor meer informatie. De **aanmelding** en **afmeldingsaanvragen** URL beide worden omgezet in het hetzelfde eindpunt, het eindpunt van SAML verwerken van aanvragen voor uw exemplaar van Azure AD is-service. De URL-verlener is de waarde die wordt weergegeven als de 'uitgever' in het SAML-token verleend aan de toepassing. 

Nadat u uw toepassing is geconfigureerd, klikt u op **volgende** knop en vervolgens de **Complete** om het dialoogvenster te sluiten. 

## <a name="assigning-users-and-groups-to-your-saml-application"></a>Gebruikers en groepen toewijzen aan uw SAML-toepassing
Als uw toepassing is geconfigureerd voor het gebruik van Azure AD als een op basis van SAML-identiteitsprovider, is het bijna klaar om te testen. Als een beveiligingscontrole geeft Azure AD geen token van een zodat ze zich aanmeldt bij de toepassing, tenzij ze gebruikmaken van Azure AD toegang hebben gekregen. Gebruikers kunnen toegang verleend rechtstreeks of via een groep waarvan ze lid zijn van. 

Als u wilt een gebruiker of groep toewijzen aan uw toepassing, klikt u op de **gebruikers toewijzen** knop. Selecteer de gebruiker of groep die u wilt toewijzen en selecteer vervolgens de **toewijzen** knop. 

![][6]

Een gebruiker toewijzen kunt Azure AD het uitgeven van een token voor de gebruiker, evenals een tegel voor deze toepassing worden weergegeven in het deelvenster van de gebruiker toegang veroorzaakt. De tegel van een toepassing wordt ook weergegeven in het startprogramma voor toepassingen van Office 365 als de gebruiker via de Office 365. 

U kunt uploaden een logo in tegel voor de toepassing met de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 

### <a name="customizing-the-claims-issued-in-the-saml-token"></a>De uitgegeven claims in het SAML-token aanpassen
Wanneer een gebruiker zich bij de toepassing verifieert, doet Azure AD een SAML-token bij de app die informatie (of claims) bevat over de gebruiker die uniek wordt geïdentificeerd. Dit omvat standaard gebruikersnaam, e-mailadres, de voornaam en achternaam van de gebruiker. 

U kunt weergeven of bewerken van de claims die wordt verzonden in het SAML-token naar de toepassing onder de **kenmerken** tabblad. 

![][7]

Er zijn twee mogelijke redenen waarom moet u mogelijk de uitgegeven claims in het SAML-token bewerken: •het toepassing is geschreven naar het vereisen van een andere set claimregels URI's of claimwaarden •Your toepassing is geïmplementeerd op een manier die vereist dat de NameIdentifier beweert te zijn iets anders dan de gebruikersnaam (AKA UPN-naam) opgeslagen in Azure Active Directory. 

Voor informatie over het toevoegen en bewerken van claims voor deze scenario's, Bekijk dit [artikel op claims aanpassing](active-directory-saml-claims-customization.md). 

### <a name="testing-the-saml-application"></a>De SAML-toepassing testen
Zodra de SAML-URL's en het certificaat zijn geconfigureerd in Azure AD en in de toepassing, gebruikers of groepen zijn toegewezen aan de toepassing in Azure, en de claims hebt bekeken en bewerkt, indien nodig, wordt de gebruiker zich aanmeldt bij de toepassing gereed is. 

Om te testen, gewoon sign-in het Azure AD-Toegangsvenster op https://myapps.microsoft.com met een gebruikersaccount die u hebt toegewezen aan de toepassing en klik vervolgens op de tegel voor de toepassing op ere van het proces voor eenmalige aanmelding. U kunt ook rechtstreeks naar de aanmeldings-URL voor de toepassing en aanmelden bladeren daar. 

Zie voor foutopsporing tips, [artikel op fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen](active-directory-saml-debugging.md) 

## <a name="password-single-sign-on"></a>Eenmalige aanmelding wachtwoord
Selecteer deze optie voor het configureren van [op basis van wachtwoorden eenmalige aanmelding](active-directory-appssoaccess-whatis.md) voor een webtoepassing met een HTML-aanmeldingspagina. Op basis van wachtwoorden SSO, ook bekend als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden tot webtoepassingen die geen ondersteuning voor identiteitsfederatie. Het is ook nuttig voor scenario's waarin meerdere gebruikers één account moeten, zoals delen op uw organisatie sociale media app accounts. 

Na het selecteren van **volgende**, wordt u gevraagd naar de URL van de toepassing web gebaseerde aanmeldingspagina opgeven. Houd er rekening mee dat dit de pagina met de gebruikersnaam en wachtwoord invoervelden moet zijn. Zodra u hebt ingevoerd, wordt in Azure AD een proces voor het parseren van de aanmeldingspagina voor een gebruikersnaam invoeren en een wachtwoord invoeren gestart. Als het proces niet geslaagd is, klikt u vervolgens leidt het u door een alternatief proces van het installeren van een Browseruitbreiding (vereist Internet Explorer, Chrome of Firefox) waarmee u de velden handmatig vastleggen.

Zodra de aanmeldingspagina wordt vastgelegd, gebruikers en groepen kunnen worden toegewezen en referentie beleidsregels kunnen worden ingesteld net als normale [wachtwoord SSO apps](active-directory-appssoaccess-whatis.md).

Opmerking: U kunt een logo in tegel voor de toepassing met uploaden de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing. 

## <a name="existing-single-sign-on"></a>Bestaande eenmalige aanmelding
Selecteer deze optie om een koppeling toevoegen aan een toepassing als uw organisatie Azure AD-Toegangsvenster of Office 365-portal. U kunt deze koppelingen wilt toevoegen aan aangepaste web-apps die momenteel gebruikmaken van Azure Active Directory Federation Services (of een andere federatieservice) gebruiken in plaats van Azure AD voor verificatie. Of u kunt dieptekoppelingen toevoegen aan de specifieke SharePoint-pagina's of andere webpagina's die u zojuist hebt wilt op van uw gebruikers toegang panelen worden weergegeven. 

Na het selecteren van **volgende**, wordt u gevraagd de URL van de toepassing koppelen aan invoeren. Zodra de voltooid, gebruikers en groepen kunnen worden toegewezen aan de toepassing die ervoor zorgt de toepassing wordt weergegeven dat de [Office 365 app linksboven](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of de [Azure AD-Toegangsvenster](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) voor gebruikers.

Opmerking: U kunt een logo in tegel voor de toepassing met uploaden de **Logo uploaden** knop op de **configureren** tabblad voor de toepassing.

## <a name="related-articles"></a>Gerelateerde artikelen
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Het aanpassen van uitgegeven Claims in het SAML-Token voor vooraf geïntegreerde Apps](active-directory-saml-claims-customization.md)
* [Het oplossen van problemen op basis van SAML eenmalige aanmelding](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
