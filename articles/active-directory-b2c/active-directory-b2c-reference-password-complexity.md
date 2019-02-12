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
ms.openlocfilehash: 2c6f3d88aae99c419b2507f421cc4dfebb2c022b
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100199"
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
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
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
