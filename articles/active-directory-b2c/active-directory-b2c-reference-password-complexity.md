---
title: Wachtwoordcomplexiteit - Azure Active Directory B2C | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de vereisten voor wachtwoordcomplexiteit voor wachtwoorden die worden geleverd door consumenten in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1d7874d7c8d04e3d3565cdfe2e52e49c538b3091
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795811"
---
# <a name="configure-complexity-requirements-for-passwords-in-azure-active-directory-b2c"></a>Vereisten voor wachtwoordcomplexiteit voor wachtwoorden configureren in Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C ondersteunt de complexiteitsvereisten voldoen voor wachtwoorden die worden geleverd door een eindgebruiker bij het maken van een account te wijzigen. Azure AD B2C gebruikt standaard `Strong` wachtwoorden. Azure AD B2C biedt ook ondersteuning voor configuratieopties voor het beheren van de complexiteit van wachtwoorden die klanten kunnen gebruiken.

## <a name="password-rule-enforcement"></a>Afdwinging van regels wordt wachtwoord

Tijdens het aanmelden of het wachtwoord opnieuw instellen, een eindgebruiker dient een wachtwoord dat voldoet aan de regels voor wachtwoordcomplexiteit. Regels voor wachtwoordcomplexiteit worden per beleid afgedwongen. Het is mogelijk dat één beleid vereisen dat een pincode van vier cijfers tijdens aanmelden terwijl een andere gebruikersstroom een tekenreeks van acht tekens vereist tijdens de registratie. Bijvoorbeeld, kunt u een beleid met verschillende wachtwoordcomplexiteit voor volwassenen dan voor kinderen.

Wachtwoordcomplexiteit wordt nooit tijdens het aanmelden afgedwongen. Gebruikers hun wachtwoord wijzigen, omdat het niet voldoet aan de vereiste voor het huidige complexiteit nooit gevraagd tijdens het aanmelden.

Wachtwoordcomplexiteit kan worden geconfigureerd in de volgende typen gebruikersstromen:

- Meld u aan of aanmelden gebruikersstroom
- Wachtwoord opnieuw instellen van beleid

Als u aangepaste beleidsregels gebruikt, kunt u ([wachtwoordcomplexiteit configureren in een aangepast beleid](active-directory-b2c-reference-password-complexity-custom.md)).

## <a name="configure-password-complexity"></a>Wachtwoordcomplexiteit configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map gebruikt met uw Azure AD B2C-tenant door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **gebruikersstromen**.
2. Selecteer de gebruikersstroom van een en klikt u op **eigenschappen**.
3. Onder **wachtwoordcomplexiteit**, de wachtwoordcomplexiteit voor deze gebruikersstroom te wijzigen **eenvoudige**, **sterke**, of **aangepaste**.

### <a name="comparison-chart"></a>Vergelijkingstabel

| Complexiteit | Description |
| --- | --- |
| Eenvoudig | Een wachtwoord dat ten minste 8 tot 64 tekens. |
| Sterk | Een wachtwoord dat ten minste 8 tot 64 tekens. Hiervoor 3 van 4 van kleine letters, hoofdletters, cijfers en symbolen. |
| Aangepast telefoonnummer | Deze optie biedt de meeste controle over de regels voor wachtwoordcomplexiteit.  Hierdoor kan een aangepaste lengte configureren.  Daarnaast kunt u wachtwoorden alleen-nummer (pincodes) accepteren. |

## <a name="custom-options"></a>Aangepaste opties

### <a name="character-set"></a>Tekenset

Hiermee kunt u gaat akkoord met alleen cijfers (pincodes) of de volledige-tekenset.

- **Alleen getallen** kunt cijfers alleen (0-9) bij het invoeren van een wachtwoord.
- **Alle** kunnen een letter, cijfer of symbool.

### <a name="length"></a>Lengte

Kunt u voor het beheren van de vereisten voor de lengte van het wachtwoord.

- **Minimale lengte** moet ten minste 4.
- **Maximale lengte** moet groter of gelijk zijn aan de minimale lengte en kan worden maximaal 64 tekens.

### <a name="character-classes"></a>Tekenklassen

Kunt u voor het beheren van de verschillende tekentypen in het wachtwoord gebruikt.

- **2 van 4: Kleine letter, een hoofdletter, een getal (0-9), symbool** zorgt ervoor dat het wachtwoord bevat ten minste twee tekentypen. Bijvoorbeeld, een getal en een kleine letter.
- **3 van 4: Kleine letter, een hoofdletter, een getal (0-9), symbool** zorgt ervoor dat het wachtwoord bevat ten minste twee tekentypen. Bijvoorbeeld, een getal, een kleine letter en een hoofdletter.
- **4 van 4: Kleine letter, een hoofdletter, een getal (0-9), symbool** zorgt ervoor dat het wachtwoord voor een speciale tekens bevat.

    > [!NOTE]
    > Vereisen van **4 van 4** kan leiden tot frustraties van eindgebruikers. Sommige onderzoek heeft aangetoond dat deze vereiste wachtwoord entropie niet worden verbeterd. Zie [richtlijnen voor het wachtwoord van het NIST](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
