---
title: De configuratie aanmelden met de Microsoft Authenticator-app (preview) - Azure Active Directory
description: Aanmelden bij Azure AD met behulp van de Microsoft Authenticator-app zonder uw wachtwoord (openbare preview)
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
ms.openlocfilehash: 3125de0d1fd784b30c000bb287b457397c0fbebb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703020"
---
# <a name="passwordless-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>De configuratie aanmelden via telefoon met de Microsoft Authenticator-app (preview-versie)

De Microsoft Authenticator-app kan worden gebruikt voor aanmelding bij een Azure AD-account zonder een wachtwoord te gebruiken. Vergelijkbaar met de technologie van [Windows Hello voor bedrijven](/windows/security/identity-protection/hello-for-business/hello-identity-verification), de Microsoft Authenticator gebruikmaakt van verificatie op basis van een sleutel waarmee de referenties van een gebruiker die is gekoppeld aan een apparaat en maakt gebruik van een biometrische of PINCODE.

![Voorbeeld van een browser aanmelding voor gebruiker de aanmelding goedkeuren](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

In plaats van Zie een prompt om een wachtwoord na het invoeren van een gebruikersnaam, een persoon die is ingeschakeld aanmelden via telefoon in de Microsoft Authenticator-app een bericht om aan te geven om te tikken een getal in de app te zien. In de app, de gebruiker moet overeenkomen met het aantal, kies goedkeuren en geef vervolgens de PINCODE of biometrische, klikt u vervolgens de verificatie wordt voltooid.

## <a name="enable-my-users"></a>Mijn gebruikers inschakelen

Voor preview-versie, moet een beheerder eerst via powershell om gebruik van de referentie op die in de tenant een beleid toevoegen. Raadpleeg de sectie 'Problemen met bekende' voordat u deze stap.

### <a name="tenant-prerequisites"></a>Tenant-vereisten

* Azure Active Directory
* Eindgebruikers die zijn ingeschakeld voor Azure multi-factor Authentication
* Gebruikers kunnen hun apparaten registreren

### <a name="steps-to-enable"></a>Stappen voor het inschakelen

Volg de stappen in het artikel [zonder wachtwoord uitproberen aanmelding inschakelen voor Azure AD](howto-authentication-passwordless-enable.md#enable-new-passwordless-authentication-methods), om in te schakelen zonder wachtwoord uitproberen verificatiemethoden in uw directory.

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>Hoe kan mijn eindgebruikers aanmelden via telefoon inschakelen?

Er is geen manier om af te dwingen van gebruikers om te maken of gebruiken van deze nieuwe referentie voor de openbare preview. Een eindgebruiker wordt alleen zonder wachtwoord uitproberen aanmelding optreden als een beheerder hun tenant is ingeschakeld en de Microsoft Authenticator-app als u wilt aanmelden via telefoon inschakelen door de gebruiker is bijgewerkt.

> [!NOTE]
> Deze mogelijkheid is in de app sinds maart 2017, dus er is een mogelijkheid is dat wanneer het beleid is ingeschakeld voor een tenant, kunt u de volgende deze stroom onmiddellijk. Houd er rekening mee en bereid uw gebruikers om deze wijziging.
>

1. Schrijf u in de Azure multi-factor Authentication
1. Meest recente versie van de Microsoft Authenticator geïnstalleerd op apparaten met iOS 8.0 of hoger of Android 6.0 of hoger.
1. Account voor werk- of schoolaccount met pushmeldingen toegevoegd aan de app. Documentatie voor eindgebruikers kunt u vinden op [ https://aka.ms/authappstart ](https://aka.ms/authappstart).

Zodra de gebruiker de MFA-account met pushmeldingen in de Microsoft Authenticator-app ingesteld heeft, kunnen ze de stappen in het artikel [Meld u aan met uw telefoon, niet uw wachtwoord](../user-help/microsoft-authenticator-app-phone-signin-faq.md) om de telefoon aanmelden registratie te voltooien.

## <a name="known-issues"></a>Bekende problemen

### <a name="ad-fs-integration"></a>AD FS Integration

Wanneer een gebruiker heeft de Microsoft Authenticator zonder wachtwoord uitproberen referentie ingeschakeld, wordt altijd verificatie voor die gebruiker standaard een melding voor goedkeuring verzonden. Deze logica wordt voorkomen dat gebruikers in een hybride-tenant niet worden doorgestuurd naar AD FS voor aanmelding bij verificatie zonder dat de gebruiker een extra stap te klikken op "Uw wachtwoord gebruiken in plaats daarvan." Dit proces wordt ook een on-premises-beleid voor voorwaardelijke toegang en stromen van Pass through-verificatie overslaan. De uitzondering op dit proces is als een login_hint is opgegeven, een gebruiker wordt automatisch doorgestuurd naar AD FS en overslaan van de optie voor het gebruik van de referentie op die de configuratie.

### <a name="azure-mfa-server"></a>Azure MFA server

Eindgebruikers die zijn ingeschakeld voor MFA via on-premises Azure MFA-server van een organisatie kan nog steeds maken en gebruiken van een eenmalige configuratie telefoon aanmelden referentie. Als de gebruiker probeert om meerdere installaties (5 +) van de Microsoft Authenticator met de referenties van een upgrade uitvoert, kan deze wijziging resulteert in een fout.  

### <a name="device-registration"></a>Device Registration service

Een van de vereisten voor het maken van deze referentie nieuw, sterk is dat het apparaat waarop deze zich bevindt in de Azure AD-tenant aan een afzonderlijke gebruiker is geregistreerd. Vanwege beperkingen van de registratie van apparaten, kan een apparaat slechts in één tenant worden geregistreerd. Deze limiet betekent dat slechts één account voor werk of school in de Microsoft Authenticator-app kan worden ingeschakeld voor de aanmelding via telefoon.

## <a name="next-steps"></a>Volgende stappen

[Wat is het zonder wachtwoord uitproberen?](concept-authentication-passwordless.md)

[Meer informatie over het registreren van apparaten](../devices/overview.md#getting-devices-in-azure-ad)

[Meer informatie over Azure multi-factor Authentication](../authentication/howto-mfa-getstarted.md)
