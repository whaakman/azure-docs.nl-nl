---
title: Aan de slag met de registratie van de gecombineerde voor Azure AD SSPR en MFA (preview) - Azure Active Directory
description: Schakel multi-factor Authentication van Azure AD gecombineerd en Self-service voor wachtwoord opnieuw instellen van inschrijving (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25450d49fd32adf12ac6c8a71671a9cb796b06c4
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317439"
---
# <a name="enable-combined-security-information-registration-preview"></a>Schakel gecombineerd security informatie registratie (preview)

Voordat u de nieuwe ervaring inschakelt, lees dan het artikel [security informatie registratie (preview) gecombineerd](concept-registration-mfa-sspr-combined.md) zodat u inzicht in de functionaliteit en de impact van deze functie.

![Gecombineerde security informatie registratie verbeterde ervaring voor het aanvragen van meer informatie bij het aanmelden. Voorbeeld ziet u Microsoft Authenticator-app registreren als eerste methode.](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Registratie van de gecombineerde security informatie voor Azure multi-factor Authentication en Azure AD Self-service voor wachtwoord opnieuw instellen is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="enable-combined-registration"></a>Gecombineerde registratie inschakelen

Volg de volgende stappen voor het gecombineerde registratie inschakelen:

1. Aanmelden bij de Azure-portal als globale beheerder of Gebruikerbeheerder.
2. Blader naar **Azure Active Directory** > **gebruikersinstellingen** > **beheren van instellingen voor toegang tot deelvenster preview-functies**.
3. Onder **gebruikers kunnen gebruiken preview-functies voor het registreren en beheren van beveiligingsgegevens - vernieuwen**, kiezen om in te schakelen voor een **geselecteerde** groep van gebruikers of voor **alle** gebruikers.

![De gecombineerde security info preview-ervaring voor alle gebruikers in de Azure AD-portal inschakelen](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> Vanaf maart van 2019 de opties telefoongesprek zich niet beschikbaar voor gebruikers voor MFA en Self-service voor Wachtwoordherstel in vrij/proefversie van Azure AD-tenants. SMS-berichten worden niet beïnvloed door deze wijziging. Telefonische oproep blijft beschikbaar voor gebruikers in betaalde Azure AD-tenants. Deze wijziging geldt alleen voor gratis/proefversie van Azure AD-tenants.

> [!NOTE]
> Nadat u gecombineerde registratie inschakelen, gebruikers registreren of bevestigen dat hun telefoonnummer of mobiele app via de nieuwe ervaring voor ze kunt gebruiken voor MFA en SSPR, als deze methoden zijn ingeschakeld in het beleid voor MFA en Self-service voor Wachtwoordherstel. Als u deze vervolgens uitschakelt, wordt gebruikers die gaat u naar de SSPR-registratie in de vorige pagina op `https:/aka.ms/ssprsetup` moeten multi-factor authentication uitvoeren voordat ze krijgen de pagina tot toegang.

Als u de site aan de lijst van zonetoewijzingen hebt geconfigureerd in Internet Explorer op de volgende websites moeten zich in dezelfde zone:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="next-steps"></a>Volgende stappen

[Beschikbare methoden voor MFA en Self-service voor Wachtwoordherstel](concept-authentication-methods.md)

[Self-service voor wachtwoord opnieuw instellen configureren](howto-sspr-deployment.md)

[Azure multi-factor Authentication configureren](howto-mfa-getstarted.md)

[Het oplossen van gecombineerde security info registratie](howto-registration-mfa-sspr-combined-troubleshoot.md)