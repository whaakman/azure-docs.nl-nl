---
title: SaaS-apps voor B2B-samenwerking configureren in Azure Active Directory | Microsoft Docs
description: Voorbeelden van code en PowerShell voor Azure Active Directory B2B-samenwerking
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 149a493f7b369415f0a2726dd6a576f0195c13d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>SaaS-apps voor B2B-samenwerking configureren

Azure Active Directory (Azure AD) B2B-samenwerking werkt met de meeste apps die zijn geïntegreerd met Azure AD. In deze sectie doorlopen we instructies voor het configureren van een aantal populaire SaaS-apps voor gebruik met Azure AD B2B.

Voordat u de instructies van de app-specifiek bekijkt, hier een aantal regels van miniatuur te volgen:

* Voor de meeste van de apps moet gebruikersinstellingen handmatig gebeuren. Dat wil zeggen, moeten gebruikers handmatig in de app ook worden gemaakt.

* Voor apps die ondersteuning bieden voor automatische installatie, zoals Dropbox, worden afzonderlijke uitnodigingen gemaakt vanuit apps. Gebruikers moeten ervoor dat elke uitnodiging te accepteren.

* In de kenmerken van de gebruiker om te beperken eventuele problemen met vervormde gebruikersprofielschijf (UDP) in gastgebruikers, altijd ingesteld **gebruikers-id** naar **user.mail**.


## <a name="dropbox-business"></a>Dropbox Business

Om gebruikers aan te melden met hun organisatie, moet u handmatig Business Dropbox voor het gebruik van Azure AD als een id-provider Security Assertion Markup Language (SAML) configureren. Als Dropbox bedrijven is niet geconfigureerd om dit te doen, kan deze vragen of anders kunnen gebruikers zich aanmelden bij het gebruik van Azure AD.

1. Selecteer om de Dropbox-Business-app met Azure AD toe **bedrijfstoepassingen** in het linkerdeelvenster en klik vervolgens op **toevoegen**.

  ![De knop "Toevoegen" op de pagina van de Enterprise-toepassingen](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. In de **een toepassing toevoegen** venster invoeren **dropbox** in het zoekvak en selecteer vervolgens **Dropbox voor bedrijven** in de lijst met resultaten.

  ![Zoeken naar 'dropbox' op het toevoegen van een toepassingspagina](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. Op de **eenmalige aanmelding** pagina **eenmalige aanmelding** in het linkerdeelvenster en voer vervolgens **user.mail** in de **gebruikers-id** vak. (Het ingesteld als de UPN standaard.)

  ![Eenmalige aanmelding voor de app configureren](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. Selecteer voor het downloaden van het certificaat moet worden gebruikt voor de configuratie van Dropbox, **DropBox configureren**, en selecteer vervolgens **SAML aanmelding op Service-URL met eenmalige** in de lijst.

  ![Downloaden van het certificaat voor Dropbox-configuratie](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. Aanmelden bij Dropbox met de aanmeldings-URL van de **eenmalige aanmelding** pagina.

  ![De aanmeldingspagina Dropbox](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. Selecteer in het menu **beheerconsole**.

  ![De koppeling 'Admin Console' in het menu Dropbox](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. In de **verificatie** dialoogvenster, **meer**, upload het certificaat en klik vervolgens op de **aanmelden URL** Voer het SAML één aanmeldings-URL.

  ![De koppeling 'Meer' in het dialoogvenster voor samengevouwen verificatie](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![De 'aanmelden URL' in het dialoogvenster voor uitgebreide verificatie](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. Voor het configureren van automatische gebruikersinstellingen in de Azure portal, selecteer **inrichten** selecteren in het linkerdeelvenster **automatische** in de **modus inrichting** vak en selecteer vervolgens **autoriseren**.

  ![Automatisch gebruikers inrichten in de Azure portal configureren](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

Nadat de gast of lid gebruikers zijn ingesteld in de app Dropbox, ontvangen ze een uitnodiging voor een afzonderlijke van Dropbox. Voor het gebruik van eenmalige aanmelding Dropbox, moeten de genodigden de uitnodiging accepteren door te klikken op een koppeling in het.

## <a name="box"></a>Box
U kunt gebruikers vak Gast om gebruikers te verifiëren met hun Azure AD-account met behulp van de federatieserver die gebaseerd op het SAML-protocol. In deze procedure kunt u metagegevens uploaden naar Box.com.

1. De Box-app uit de zakelijke apps toevoegen.

2. Eenmalige aanmelding configureren in de volgende volgorde:

  ![Eenmalige aanmelding vak configureren](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 a. In de **aanmelden URL** Zorg dat de aanmeldings-URL juist is ingesteld voor het selectievakje in de Azure portal. Deze URL is de URL van uw tenant Box.com. Deze naam moet voldoen aan de naamgevingsconventie *https://.box.com*.  
 De **id** geldt niet voor deze app maar nog steeds wordt weergegeven als een verplicht veld.

 b. In de **gebruikers-id** Voer **user.mail** (voor eenmalige aanmelding voor gastaccounts).

 c. Onder **SAML-certificaat voor ondertekening van**, klikt u op **nieuw certificaat maken**.

 d. Om te beginnen met het configureren van uw tenant Box.com voor het gebruik van Azure AD als id-provider, downloadt u het metagegevensbestand en vervolgens opslaan in de lokale schijf.

 e. Het metagegevensbestand aan het vak ondersteuning doorsturen team eenmalige aanmelding voor u te configureren.

3. Selecteer voor Azure AD automatische gebruikersinstellingen, in het linkerdeelvenster **inrichten**, en selecteer vervolgens **autoriseren**.

  ![Azure AD verbinding maken met het selectievakje toestaan](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Zoals Dropbox genodigden, moeten het selectievakje genodigden hun uitnodiging van de app vak inwisselen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Gebruikerseigenschappen B2B-samenwerking](active-directory-b2b-user-properties.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
* [B2B-samenwerking uitnodigingen delegeren](active-directory-b2b-delegate-invitations.md)
* [Dynamische groepen en B2B-samenwerking](active-directory-b2b-dynamic-groups.md)
* [B2B-samenwerking code en PowerShell-voorbeelden](active-directory-b2b-code-samples.md)
* [B2B-samenwerking gebruikerstokens](active-directory-b2b-user-token.md)
* [Gebruikersclaims voor B2B-samenwerking toewijzing](active-directory-b2b-claims-mapping.md)
* [Office 365 extern delen](active-directory-b2b-o365-external-user.md)
* [Huidige beperkingen voor B2B-samenwerking](active-directory-b2b-current-limitations.md)
