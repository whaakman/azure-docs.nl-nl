---
title: 'SaaS-toepassingen voor B2B-samenwerking: Azure Active Directory configureren | Microsoft Docs'
description: Code- en PowerShell-voorbeelden voor Azure Active Directory B2B-samenwerking
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c7a652ab4283321c5934b32fdc5478bb5d79e4a7
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58293127"
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>Configureren van SaaS-apps voor B2B-samenwerking

Active Directory (Azure AD) B2B-samenwerking van Azure werkt met de meeste apps die met Azure AD integreren. In deze sectie behandelen we instructies voor het configureren van enkele populaire SaaS-apps voor gebruik met Azure AD B2B.

Voordat u de app-specifieke instructies bekijkt, worden hier enkele vuistregels:

* Voor het merendeel van de apps moet gebruikersinstellingen handmatig gebeuren. Dat wil zeggen, moeten gebruikers handmatig in de app ook worden gemaakt.

* Voor apps die ondersteuning bieden voor automatische installatie, zoals Dropbox, worden afzonderlijke inschrijvingen gemaakt van de apps. Gebruikers moeten ervoor dat elke uitnodiging te accepteren.

* In de kenmerken van de gebruiker om te beperken van eventuele problemen met vervormde gebruiker profiel schijf (UDP) in gastgebruikers, altijd ingesteld **gebruikers-id** naar **user.mail**.


## <a name="dropbox-business"></a>Dropbox-bedrijven

Als u wilt dat gebruikers zich aanmelden met hun organisatieaccount, moet u handmatig de zakelijke Dropbox voor het gebruik van Azure AD als een id-provider Security Assertion Markup Language (SAML) configureren. Als Dropbox bedrijven niet is geconfigureerd om dit te doen, kan deze vragen of anders kunnen gebruikers zich aanmelden bij het gebruik van Azure AD.

1. Als u wilt de Dropbox-Business-app in Azure AD toevoegen, selecteert u **bedrijfstoepassingen** in het linkerdeelvenster en klik vervolgens op **toevoegen**.

   ![De knop 'Toevoegen' op de pagina van de Enterprise-toepassingen](media/configure-saas-apps/add-dropbox.png)

2. In de **toevoegen van een toepassing** venster, geef **dropbox** in het zoekvak en selecteer vervolgens **Dropbox voor bedrijven** in de lijst met resultaten.

   ![Zoeken naar "dropbox' op de toevoegen een toepassingspagina](media/configure-saas-apps/add-app-dialog.png)

3. Op de **eenmalige aanmelding** weergeeft, schakelt **eenmalige aanmelding** in het linkerdeelvenster en voer vervolgens **user.mail** in de **gebruikers-id** vak. (Deze wordt ingesteld als UPN standaard.)

   ![Configureren van eenmalige aanmelding voor de app.](media/configure-saas-apps/configure-app-sso.png)

4. Selecteer voor het downloaden van het certificaat moet worden gebruikt voor de configuratie van Dropbox, **DropBox configureren**, en selecteer vervolgens **SAML eenmalige aanmelding op Service-URL** in de lijst.

   ![Downloaden van het certificaat voor de configuratie van Dropbox](media/configure-saas-apps/download-certificate.png)

5. Aanmelden bij Dropbox met de aanmeldings-URL van de **eenmalige aanmelding** pagina.

   ![Schermopname van de aanmeldingspagina van Dropbox](media/configure-saas-apps/sign-in-to-dropbox.png)

6. Selecteer in het menu **-beheerconsole**.

   ![De koppeling '-beheerconsole' in het menu Dropbox](media/configure-saas-apps/dropbox-menu.png)

7. In de **verificatie** in het dialoogvenster, selecteer **meer**, upload het certificaat en klik vervolgens op de **aanmelden URL** voert u de SAML aanmeldings-URL met eenmalige.

   ![De koppeling 'Meer' in het dialoogvenster voor samengevouwen verificatie](media/configure-saas-apps/dropbox-auth-01.png)

   ![De 'aanmelden URL' in de uitgebreide dialoogvenster voor verificatie](media/configure-saas-apps/paste-single-sign-on-URL.png)

8. Voor het configureren van automatische gebruikersinstellingen in Azure portal, selecteert u **Provisioning** selecteren in het linkerdeelvenster **automatische** in de **inrichting modus** vak en selecteer vervolgens  **Autoriseren**.

   ![Automatisch gebruikers inrichten in Azure portal configureren](media/configure-saas-apps/set-up-automatic-provisioning.png)

Nadat de gast- of lid gebruikers zijn ingesteld in de Dropbox-app, krijgen ze een uitnodiging voor een afzonderlijke van Dropbox. Voor het gebruik van eenmalige aanmelding Dropbox, moeten de genodigden de uitnodiging accepteren door te klikken op een koppeling in het.

## <a name="box"></a>Box
U kunt gebruikers vak Gast om gebruikers te verifiëren met hun Azure AD-account met behulp van de Federatie die gebaseerd op het SAML-protocol kunt inschakelen. In deze procedure kunt u metagegevens uploaden naar Box.com.

1. De Box-app uit de enterprise-apps toevoegen.

2. Configureren van eenmalige aanmelding in de volgende volgorde:

   ![Schermopname van de configuratie voor eenmalige aanmelding-instellingen](media/configure-saas-apps/configure-box-sso.png)

   a. In de **aanmeldings-URL** vak, zorg ervoor dat de aanmeldings-URL is ingesteld op de juiste wijze voor Box in Azure portal. Deze URL is de URL van uw Box.com-tenant. Deze moet de naamconventie volgen *https://.box.com*.  
   De **id** geldt niet voor deze app, maar deze nog steeds wordt weergegeven als een verplicht veld.

   b. In de **gebruikers-id** Voer **user.mail** (voor eenmalige aanmelding voor Gast-accounts).

   c. Onder **SAML-handtekeningcertificaat**, klikt u op **nieuw certificaat maken**.

   d. Om te beginnen met het configureren van uw tenant Box.com voor het gebruik van Azure AD als id-provider, download het bestand met metagegevens en sla het op uw lokale schijf.

   e. Het bestand met metagegevens in het vak ondersteuning doorsturen-team en configureert u eenmalige aanmelding voor u.

3. Selecteer voor de instellingen van het Azure AD automatische gebruiker, in het linkerdeelvenster **Provisioning**, en selecteer vervolgens **autoriseren**.

   ![Azure AD om verbinding met Box te autoriseren](media/configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Vak genodigden moeten de uitnodiging van de Box-app inwisselen, zoals Dropbox genodigden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Dynamische groepen en B2B-samenwerking](use-dynamic-groups.md)
- [B2B-samenwerking-gebruikersclaims toewijzen](claims-mapping.md)
- [Office 365 extern delen](o365-external-user.md)

