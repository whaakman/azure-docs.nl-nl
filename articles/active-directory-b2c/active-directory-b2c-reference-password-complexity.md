---
title: Wachtwoordcomplexiteit - Azure AD B2C | Microsoft Docs
description: Het configureren van de complexiteitsvereisten voor wachtwoorden die worden geleverd door consumenten in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: 3906c9fa1def206a8f0a7e155949097242728c2f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: Complexiteitsvereisten voor wachtwoorden configureren

> [!NOTE]
> **Deze functie is in preview.**  Neem contact op met [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com) om uw testtenant met deze functie is ingeschakeld.  Niet testen dit op productie tenants.

Azure Active Directory B2C (Azure AD B2C) ondersteunt de complexiteitsvereisten voor wachtwoorden die worden geleverd door een eindgebruiker bij het maken van een account te wijzigen.  Azure AD B2C gebruikt standaard `Strong` wachtwoorden.  Azure AD B2C ondersteunt ook configuratieopties voor het beheren van de complexiteit van wachtwoorden die klanten kunnen gebruiken.

## <a name="when-password-rules-are-enforced"></a>Wanneer de wachtwoordregels worden afgedwongen

Tijdens de registratie of het wachtwoord opnieuw instellen, een eindgebruiker moet een wachtwoord opgeven die voldoet aan de regels voor wachtwoordcomplexiteit.  Regels voor wachtwoordcomplexiteit worden per beleid afgedwongen.  Het is mogelijk dat één beleid vereisen een pincode van vier cijfers tijdens registratie terwijl een ander beleid acht tekenreeks vereist tijdens de registratie.  Bijvoorbeeld, kunt u een beleid met verschillende wachtwoordcomplexiteit voor volwassenen dan voor kinderen.

Wachtwoordcomplexiteit wordt nooit tijdens het aanmelden afgedwongen.  Gebruikers hun wachtwoord wijzigen omdat deze niet voldoet aan de huidige complexiteit behoefte nooit gevraagd tijdens het aanmelden.

Hier zijn de soorten beleid waarop wachtwoordcomplexiteit kan worden geconfigureerd:

* Registreren of aanmelden
* Beleid voor wachtwoordherstel
* Aangepast beleid ([wachtwoordcomplexiteit configureren in het aangepaste beleid](active-directory-b2c-reference-password-complexity-custom.md))

## <a name="how-to-configure-password-complexity"></a>Wachtwoordcomplexiteit configureren

1. Volg deze stappen voor [gaat u naar Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
1. Open **registreren of aanmelden beleidsregels**.
1. Selecteer een beleid en klik op **bewerken**.
1. Open **wachtwoordcomplexiteit**.
1. Wijzigen van de wachtwoordcomplexiteit voor dit beleid wilt **eenvoudige**, **sterke**, of **aangepaste**.

### <a name="comparison-chart"></a>Vergelijkingsdiagram

| Complexiteit | Beschrijving |
| --- | --- |
| Simpel | Een wachtwoord dat ten minste 8 tot 64 tekens. |
| Sterk | Een wachtwoord dat ten minste 8 tot 64 tekens. Hiervoor moeten 3 van 4 van kleine letters, hoofdletters, cijfers en symbolen. |
| Aangepast telefoonnummer | Deze optie biedt de meeste controle over de regels voor wachtwoordcomplexiteit.  Kunt u een aangepaste lengte configureren.  Daarnaast kunt u wachtwoorden alleen-nummer (pincodes) worden geaccepteerd. |

## <a name="options-available-under-custom"></a>Beschikbare opties onder aangepast

### <a name="character-set"></a>Tekenset

Hiermee kunt u accepteert alleen cijfers (pincodes) of de volledige-tekenset.

* **Alleen cijfers** kunt cijfers alleen (0-9) bij het invoeren van een wachtwoord.
* **Alle** kunnen een letter, cijfer of symbool.

### <a name="length"></a>Lengte

Kunt u de lengtevereisten van het wachtwoord bepalen.

* **Minimale lengte** moet ten minste 4.
* **Maximale lengte** moet groter zijn dan of gelijk zijn aan de minimale lengte en kan worden maximaal 64 tekens lang zijn.

### <a name="character-classes"></a>Tekenklassen

Hiermee kunt u bepalen welke verschillende tekensets typen in het wachtwoord wordt gebruikt.

* **2 van 4: kleine letter, hoofdletter, cijfer (0-9), symbool** zorgt ervoor dat het wachtwoord bevat ten minste twee tekentypen. Een getal en een kleine letter.
* **3 van 4: kleine letter, hoofdletter, cijfer (0-9), symbool** zorgt ervoor dat het wachtwoord bevat ten minste twee tekentypen. Bijvoorbeeld: een getal, een kleine letter en een hoofdletter.
* **4 van 4: kleine letter, hoofdletter, cijfer (0-9), symbool** zorgt ervoor dat het wachtwoord voor een speciale tekens bevat.

    > [!NOTE]
    > Vereisen **4 van 4** kan leiden tot vervelend door eindgebruikers. Sommige onderzoek heeft aangetoond dat deze vereiste wachtwoord entropie niet worden verbeterd. Zie [NIST wachtwoord richtlijnen](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
