---
title: Aan de slag met de registratie van de gecombineerde voor Azure AD SSPR en meervoudige verificatie (preview) - Azure Active Directory
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
ms.openlocfilehash: 3baf2690ae07b87bb4d5dba30fcd20f62a1a4506
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280567"
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
> Nadat u hebt ingeschakeld gecombineerd registratie, gebruikers die registreren of bevestigen dat hun telefoonnummer of mobiele app via de nieuwe ervaring voor ze kunt gebruiken voor multi-factor Authentication en SSPR, als deze methoden zijn ingeschakeld in de multi-factor Authentication en de self-service voor Wachtwoordherstel beleidsregels. Als u deze vervolgens uitschakelt, wordt gebruikers die gaat u naar de SSPR-registratie in de vorige pagina op `https:/aka.ms/ssprsetup` moeten multi-factor authentication uitvoeren voordat ze krijgen de pagina tot toegang.

Als u de Site aan de lijst van zonetoewijzingen hebt geconfigureerd in Internet Explorer, hebben de volgende sites zich in dezelfde zone:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="next-steps"></a>Volgende stappen

[Beschikbare methoden voor meervoudige verificatie en Self-service voor Wachtwoordherstel](concept-authentication-methods.md)

[Self-service voor wachtwoord opnieuw instellen configureren](howto-sspr-deployment.md)

[Azure Multi-Factor Authentication configureren](howto-mfa-getstarted.md)

[Het oplossen van gecombineerde security info registratie](howto-registration-mfa-sspr-combined-troubleshoot.md)