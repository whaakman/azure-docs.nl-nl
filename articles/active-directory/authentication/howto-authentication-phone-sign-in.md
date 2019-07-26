---
title: Aanmelding zonder wacht woord met de Microsoft Authenticator-app (preview)-Azure Active Directory
description: Aanmelden bij Azure AD met behulp van de app Microsoft Authenticator zonder uw wacht woord te gebruiken (open bare preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.custom: seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c464874708c7b93ec5620cc9ae253912ce1a4790
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68357136"
---
# <a name="passwordless-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>Aanmelden met een wacht woord met de Microsoft Authenticator-app (open bare preview)

De Microsoft Authenticator-app kan worden gebruikt om u aan te melden bij een Azure AD-account zonder gebruik te maken van een wacht woord. Net als de technologie van [Windows hello voor bedrijven](/windows/security/identity-protection/hello-for-business/hello-identity-verification)gebruikt de Microsoft Authenticator verificatie op basis van een sleutel om een gebruikers referentie in te scha kelen die is gekoppeld aan een apparaat en gebruikmaakt van biometrisch of pincode.

![Voor beeld van een browser aanmelding waarin de gebruiker wordt gevraagd om de aanmelding goed te keuren](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

In plaats van een prompt weer te geven voor een wacht woord na het invoeren van een gebruikers naam, wordt door een persoon die zich heeft aangemeld voor aanmelding bij de app van de Microsoft Authenticator, een bericht met de melding dat de gebruiker op een nummer in de app kan tikken. In de app moet de gebruiker overeenkomen met het nummer. Kies goed keuren en geef vervolgens hun pincode of biometrische op. vervolgens wordt de verificatie voltooid.

## <a name="enable-my-users"></a>Mijn gebruikers inschakelen

### <a name="tenant-prerequisites"></a>Tenant vereisten

* Azure Active Directory
* Eind gebruikers die zijn ingeschakeld voor Azure multi-factor Authentication
* Gebruikers kunnen hun apparaten registreren

### <a name="steps-to-enable"></a>Stappen om in te scha kelen

Volg de stappen in het artikel [aanmelden zonder wacht woord inschakelen voor Azure AD](howto-authentication-passwordless-enable.md#enable-new-passwordless-authentication-methods)om authenticatie methoden met een wacht woord in te scha kelen in uw Directory.

> [!NOTE]
> Als u deze functie eerder hebt ingeschakeld voor uw Tenant met behulp van een Power shell-script, wordt door het instellen van een nieuw beleid voor gebruikers en groepen het bestaande beleid voor de hele Tenant overschreven. 
>

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>Hoe kunnen mijn eind gebruikers zich aanmelden via de telefoon?

Voor een open bare preview is het niet mogelijk om gebruikers af te dwingen om deze nieuwe referentie te maken of te gebruiken. Een eind gebruiker ondervindt alleen aanmelding zonder wacht woord wanneer een beheerder hun Tenant heeft ingeschakeld en de gebruiker de Microsoft Authenticator-app heeft bijgewerkt om de aanmelding via de telefoon in te scha kelen.

> [!NOTE]
> Deze functie is sinds maart 2017 aanwezig in de app, zodat gebruikers deze stroom direct kunnen ondervinden wanneer het beleid is ingeschakeld voor een Tenant. Stel uw gebruikers op de hoogte en bereid ze voor op deze wijziging.
>

1. Registreren in azure multi-factor Authentication
1. Nieuwste versie van Microsoft Authenticator geïnstalleerd op apparaten met iOS 8,0 of hoger, of Android 6,0 of hoger.
1. Werk-of school account met push meldingen toegevoegd aan de app. Documentatie voor eind gebruikers vindt u op [https://aka.ms/authappstart](https://aka.ms/authappstart).

Zodra de gebruiker het MFA-account met push meldingen heeft ingesteld in de app Microsoft Authenticator, kunnen ze de stappen in het artikel [Aanmelden met uw telefoon en niet uw wacht woord](../user-help/microsoft-authenticator-app-phone-signin-faq.md) volgen om de registratie van de telefoon te volt ooien.

## <a name="known-issues"></a>Bekende problemen

### <a name="ad-fs-integration"></a>Integratie van AD FS

Wanneer een gebruiker de Microsoft Authenticator wacht woordloze referentie heeft ingeschakeld, zal verificatie voor die gebruiker altijd standaard een melding verzenden voor goed keuring. Deze logica voor komt dat gebruikers in een hybride Tenant worden doorgestuurd naar ADFS voor aanmeldings verificatie zonder dat de gebruiker een extra stap hoeft te doen om op ' uw wacht woord gebruiken ' te klikken. Met dit proces worden ook alle on-premises beleids regels voor voorwaardelijke toegang en Pass Through-verificatie stromen genegeerd. De uitzonde ring op dit proces is als er een login_hint is opgegeven, een gebruiker wordt automatisch doorgestuurd naar AD FS en de optie voor het gebruik van wacht woordloze referenties overs Laan.

### <a name="azure-mfa-server"></a>Azure MFA-server

Eind gebruikers die zijn ingeschakeld voor MFA via de on-premises Azure MFA-server van een organisatie, kunnen nog steeds één wacht woord voor eenmalige aanmelding voor een mobiele telefoon maken en gebruiken. Als de gebruiker meerdere installaties (5 +) van de Microsoft Authenticator met de referentie probeert bij te werken, kan deze wijziging een fout veroorzaken.  

### <a name="device-registration"></a>Apparaatregistratie

Een van de vereisten om deze nieuwe, sterke referentie te maken, is dat het apparaat waar het zich bevindt, is geregistreerd in de Azure AD-Tenant, naar een afzonderlijke gebruiker. Als gevolg van beperkingen voor apparaatregistratie, kan een apparaat alleen worden geregistreerd in één Tenant. Deze limiet betekent dat er slechts één werk-of school account in de Microsoft Authenticator-app kan worden ingeschakeld voor aanmelding via de telefoon.

## <a name="next-steps"></a>Volgende stappen

[Wat is een wacht woord?](concept-authentication-passwordless.md)

[Meer informatie over apparaatregistratie](../devices/overview.md#getting-devices-in-azure-ad)

[Meer informatie over Azure multi-factor Authentication](../authentication/howto-mfa-getstarted.md)
