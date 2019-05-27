---
title: Aan de slag met de registratie van de gecombineerde voor Azure AD SSPR en meervoudige verificatie (preview) - Azure Active Directory
description: Schakel multi-factor Authentication van Azure AD gecombineerd en Self-service voor wachtwoord opnieuw instellen van inschrijving (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: cafe81a1b5ab6d26e71eff05e355d2ed79c4d5e7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119386"
---
# <a name="enable-combined-security-information-registration-preview"></a>Schakel gecombineerd security informatie registratie (preview)

Voordat u de nieuwe ervaring inschakelt, lees dan het artikel [security informatie registratie (preview) gecombineerd](concept-registration-mfa-sspr-combined.md) om te controleren of u informatie over de functionaliteit en de effecten van deze functie.

![Gecombineerde informatie registratie verbeterde ervaring](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Gecombineerde security informatie registratie voor meervoudige verificatie en selfservice voor wachtwoord opnieuw instellen van Azure Active Directory (Azure AD) is een openbare preview-functie van Azure AD. Zie [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews) voor meer informatie.|
|     |

## <a name="enable-combined-registration"></a>Gecombineerde registratie inschakelen

Voer deze stappen voor het gecombineerde registratie inschakelen:

1. Aanmelden bij de Azure-portal als globale beheerder of Gebruikerbeheerder.
2. Ga naar **Azure Active Directory** > **gebruikersinstellingen** > **beheren van instellingen voor toegang tot deelvenster preview-functies**.
3. Onder **gebruikers kunnen gebruiken preview-functies voor het registreren en beheren van beveiligingsgegevens - vernieuwen**, kiezen om in te schakelen voor een **geselecteerde** groep van gebruikers of voor **alle** gebruikers.

   ![De gecombineerde security info preview-ervaring voor alle gebruikers inschakelen](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> Vanaf maart 2019 biedt zijn de opties telefoongesprek niet beschikbaar voor multi-factor Authentication en SSPR-gebruikers in vrij/proefversie van Azure AD-tenants. SMS-berichten worden niet beïnvloed door deze wijziging. De opties telefoongesprek wordt nog steeds beschikbaar voor gebruikers met betaalde Azure AD-tenants.

> [!NOTE]
> Nadat u hebt ingeschakeld gecombineerd registratie, gebruikers die registreren of bevestigen dat hun telefoonnummer of mobiele app via de nieuwe ervaring voor ze kunt gebruiken voor multi-factor Authentication en SSPR, als deze methoden zijn ingeschakeld in de multi-factor Authentication en de self-service voor Wachtwoordherstel beleidsregels. Als u deze vervolgens uitschakelt, wordt gebruikers die gaat u naar de SSPR-registratie in de vorige pagina op `https://aka.ms/ssprsetup` moeten multi-factor authentication uitvoeren voordat ze krijgen de pagina tot toegang.

Als u de Site aan de lijst van zonetoewijzingen hebt geconfigureerd in Internet Explorer, hebben de volgende sites zich in dezelfde zone:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Beleid voor voorwaardelijke toegang voor gecombineerde registratie

Beveiligen wanneer en hoe gebruikers registreren voor Azure multi-factor Authentication en Self-service voor wachtwoord opnieuw instellen nu mogelijk is met de acties van de gebruiker in het beleid voor voorwaardelijke toegang is. Deze preview-functie is beschikbaar voor organisaties die hebben ingeschakeld de [gecombineerd registratie preview](../authentication/concept-registration-mfa-sspr-combined.md). Deze functionaliteit kan worden ingeschakeld in een organisatie waar ze willen dat gebruikers zich registreren voor Azure multi-factor Authentication en SSPR vanuit een centrale locatie zoals een vertrouwde netwerklocatie tijdens HR-onboarding. Zie het artikel voor meer informatie over het maken van vertrouwde locaties voor de voorwaardelijke toegang [wat is er met de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang?](../conditional-access/location-condition.md#named-locations)

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Maak een beleid om te vereisen dat de registratie van een vertrouwde locatie bevindt

Het volgende beleid van toepassing op alle geselecteerde gebruikers, die probeert te registreren met behulp van de gecombineerde registratie-ervaring en de toegang geblokkeerd, tenzij deze verbinding vanaf een locatie die is gemarkeerd als vertrouwd netwerk maakt.

![Maken van een CA-beleid voor het beheren van beveiliging info registratie](media/howto-registration-mfa-sspr-combined/conditional-access-register-security-info.png)

1. In de **Azure-portal**, blader naar **Azure Active Directory** > **voorwaardelijke toegang**
1. Selecteer **Nieuw beleid**.
1. Voer in het vak Naam een naam op voor dit beleid. Bijvoorbeeld, **gecombineerd Security Info registratie in vertrouwde netwerken**
1. Onder **toewijzingen**, klikt u op **gebruikers en groepen**, en selecteer de gebruikers en groepen die u wilt dat dit beleid wilt toepassen

   > [!WARNING]
   > Gebruikers moeten worden ingeschakeld voor de [gecombineerd registratie preview](../authentication/howto-registration-mfa-sspr-combined.md).

1. Onder **Cloud-apps of acties**, selecteer **gebruikersacties**, Controleer **registreren beveiligingsgegevens (preview)**
1. Onder **voorwaarden** > **locaties**
   1. Configureer **Ja**
   1. Opnemen **elke locatie**
   1. Uitsluiten **alle vertrouwde locaties**
   1. Klik op **gedaan** op de blade locaties
   1. Klik op **gedaan** op de blade voorwaarden
1. Onder **besturingselementen voor toegang** > **verlenen**
   1. Klik op **toegang blokkeren**
   1. Klik vervolgens op **selecteren**
1. Stel **beleid inschakelen** naar **op**
1. Klik vervolgens op **maken**

## <a name="next-steps"></a>Volgende stappen

[Beschikbare methoden voor meervoudige verificatie en Self-service voor Wachtwoordherstel](concept-authentication-methods.md)

[Self-service voor wachtwoord opnieuw instellen configureren](howto-sspr-deployment.md)

[Azure multi-factor Authentication configureren](howto-mfa-getstarted.md)

[Het oplossen van gecombineerde security info registratie](howto-registration-mfa-sspr-combined-troubleshoot.md)

[Wat is de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang?](../conditional-access/location-condition.md)
