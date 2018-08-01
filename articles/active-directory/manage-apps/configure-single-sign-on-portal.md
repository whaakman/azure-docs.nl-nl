---
title: Eenmalige aanmelding management voor bedrijfs-apps in Azure Active Directory | Microsoft Docs
description: Instellingen voor eenmalige aanmelding voor zakelijke apps binnen uw organisatie beheren in Azure Active Directory-toepassingsgalerie
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: 81b959a08f55f13fd0bcadce32b65ba64f9bb270
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365488"
---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Beheren van eenmalige aanmelding voor zakelijke apps

In dit artikel wordt beschreven hoe u de [Azure-portal](https://portal.azure.com) voor het beheren van instellingen voor eenmalige aanmelding voor zakelijke toepassingen. Zakelijke apps zijn apps die zijn geïmplementeerd en gebruikt binnen uw organisatie. In dit artikel geldt met name voor apps die zijn toegevoegd vanuit de [Azure Active Directory-toepassingsgalerie](what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps-in-the-portal"></a>Uw apps zoeken in de portal
Alle enterprise-apps die zijn geconfigureerd voor eenmalige aanmelding kunnen worden weergegeven en beheerd in Azure portal. De toepassingen kunnen u vinden in de **alle Services** &gt; **bedrijfstoepassingen** sectie van de portal. 

![Blade voor bedrijfstoepassingen](./media/configure-single-sign-on-portal/enterprise-apps-blade.png)

Selecteer **alle toepassingen** om een lijst met alle apps die zijn geconfigureerd weer te geven. Selecteren van een app, geeft de resources voor die app, waar rapporten kunnen worden weergegeven voor die app en een aantal instellingen kan worden beheerd.

Selecteer voor het beheren van instellingen voor eenmalige aanmelding, **eenmalige aanmelding**.

![Toepassingsblade-resource](./media/configure-single-sign-on-portal/enterprise-apps-sso-blade.png)

## <a name="single-sign-on-modes"></a>Modi voor eenmalige aanmelding
**Eenmalige aanmelding** begint met een **modus** menu, waardoor de modus voor één aanmelding moet worden geconfigureerd. De beschikbare opties zijn onder andere:

* **SAML gebaseerde aanmelding** -deze optie is beschikbaar als de toepassing ondersteunt volledig federatieve eenmalige aanmelding met Azure Active Directory met het protocol SAML 2.0, WS-Federation, of OpenID connect-protocollen.
* **Wachtwoord gebaseerde aanmelding** -deze optie is beschikbaar als Azure AD wachtwoord formulier invullen voor deze toepassing ondersteunt.
* **Gekoppelde aanmelding** -voorheen bekend als 'Bestaande single sign-on', deze optie kan beheerders om een koppeling naar deze toepassing in van de gebruiker Azure AD-Toegangsvenster of Office 365 startprogramma voor toepassingen.

Zie voor meer informatie over deze modi [hoe eenmalige aanmelding met Azure Active Directory-werk](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML gebaseerde aanmelding
De **SAML gebaseerde aanmelding** optie is onderverdeeld in vier secties:

### <a name="domains-and-urls"></a>Domeinen en URL 's
Dit is waar alle informatie over het domein en de URL's van de toepassing worden toegevoegd aan uw Azure AD-directory. Alle invoer vereist voor het maken van eenmalige aanmelding work-app rechtstreeks op het scherm worden weergegeven terwijl alle optionele invoer kunnen worden bekeken door het selecteren van de **geavanceerde URL-instellingen weergeven** selectievakje. De volledige lijst van ondersteunde invoer bevat:

* **aanmeldings-URL** – wanneer de gebruiker zich aanmeldt bij de toepassing gaat. Als de toepassing is geconfigureerd voor het uitvoeren van service provider-geïnitieerde eenmalige aanmelding, wanneer een gebruiker deze URL wordt geopend, is de service-provider wordt omgeleid naar Azure AD om te verifiëren en meld u aan de gebruiker. 
  * Als dit veld is ingevuld, gebruikt Azure AD de URL van de toepassing starten vanuit Office 365 en Azure AD-Toegangsvenster.
  * Als dit veld wordt weggelaten, klikt u vervolgens of Azure AD in plaats daarvan voert identiteit serviceprovider is geïnitieerd aanmelding wanneer de app wordt gestart vanuit het Office 365, de Azure AD-Toegangsvenster van de Azure AD eenmalige aanmeldings-URL.
* **Id** -deze URI moet uniek zijn voor de toepassing waarvoor eenmalige aanmelding wordt geconfigureerd. Dit is de waarde die Azure AD terug naar de toepassing als de parameter van de doelgroep van het SAML-token stuurt en de toepassing wordt verwacht om te valideren. Deze waarde wordt ook weergegeven als de entiteit-ID in een SAML-metagegevens geleverd door de toepassing.
* **Antwoord-URL** -de antwoord-URL is waar de toepassing verwacht voor het ontvangen van het SAML-token. Dit is ook de URL Assertion Consumer Service (ACS) genoemd. Nadat deze zijn ingevoerd, klikt u op volgende om door te gaan naar het volgende scherm. Dit scherm bevat informatie over wat er worden geconfigureerd aan de toepassing moet zodat het accepteren van een SAML-token van Azure AD.
* **Relaystatus** -de relaystatus van de is een optionele parameter waarmee de toepassing zien waar u het doorsturen van gebruikers nadat de verificatie is voltooid. De waarde is doorgaans een geldige URL op de toepassing, maar sommige toepassingen anders dit veld gebruiken (Zie van de app één aanmeldings-documentatie voor meer informatie). De mogelijkheid om in te stellen van de relaystatus van de is een nieuwe functie die uniek is voor de nieuwe Azure portal.

### <a name="user-attributes"></a>Gebruikerskenmerken
Dit is waar beheerders kunt weergeven en bewerken van de kenmerken die worden verzonden in het SAML-token die Azure AD aan de toepassing van elke verleent keer dat gebruikers zich aanmelden.

Het alleen bewerkbare kenmerk ondersteund is de **gebruikers-id** kenmerk. De waarde van dit kenmerk is het veld in Azure AD die uniek voor elke gebruiker in de toepassing. Bijvoorbeeld als de app is geïmplementeerd met behulp van de 'e-mailadres' als de gebruikersnaam en de unieke id, klikt u vervolgens de waarde zou worden ingesteld op het veld 'user.mail' in Azure AD.

### <a name="saml-signing-certificate"></a>SAML-handtekeningcertificaat
In deze sectie worden de details van het certificaat dat Azure AD gebruikt voor het ondertekenen van de SAML-tokens die zijn verleend aan de toepassing telkens wanneer de gebruiker wordt geverifieerd. Hierdoor kan de eigenschappen van het huidige certificaat moet worden gecontroleerd, met inbegrip van de vervaldatum.

### <a name="application-configuration"></a>Toepassingsconfiguratie
De laatste sectie vindt u de documentatie en/of de besturingselementen die zijn vereist voor het configureren van de toepassing zelf op het gebruik van Azure Active Directory als id-provider.

De **toepassing configureren** vervolgmenu bevat nieuwe beknopte, ingesloten instructies voor het configureren van de toepassing. Dit is een andere nieuwe functie uniek is voor de nieuwe Azure portal.

> [!NOTE]
> Zie voor een compleet voorbeeld van ingesloten documentatie, de Salesforce.com-toepassing. Documentatie voor Aanvullende apps wordt voortdurend toegevoegd.
> 
> 

![Ingesloten docs](./media/configure-single-sign-on-portal/enterprise-apps-blade-embedded-docs.png)

## <a name="password-based-sign-on"></a>Wachtwoord gebaseerde aanmelding
Als dit wordt ondersteund voor de toepassing, selecteren de SSO-modus op basis van wachtwoorden en klikken op **opslaan** direct configureert u eenmalige aanmelding op basis van wachtwoorden. Zie voor meer informatie over het implementeren van eenmalige aanmelding op basis van wachtwoorden [hoe eenmalige aanmelding met Azure Active Directory-werk](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

![Wachtwoord gebaseerde aanmelding](./media/configure-single-sign-on-portal/enterprise-apps-blade-password-sso.png)

## <a name="linked-sign-on"></a>Gekoppelde aanmelding
Als dit wordt ondersteund voor de toepassing, kunt selecteren de gekoppelde SSO-modus u voert u de URL die u wilt omleiden naar wanneer gebruikers deze app op de Azure AD-Toegangsvenster of Office 365. Zie voor meer informatie over gekoppelde SSO (voorheen bekend als 'bestaande eenmalige aanmelding'), [hoe eenmalige aanmelding met Azure Active Directory-werk](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

![Gekoppelde aanmelding](./media/configure-single-sign-on-portal/enterprise-apps-blade-linked-sso.png)

## <a name="feedback"></a>Feedback

We hopen dat u, zoals het gebruik van de verbeterde ervaring voor Azure AD. Houd uw feedback! Plaats uw feedback en ideeën voor verbetering van de **-beheerportal** sectie van onze [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  We enthousiast bent over het bouwen van coole nieuwe dingen elke dag en gebruik de informatie voor de shape en definiëren wat we hierna bouwen.

