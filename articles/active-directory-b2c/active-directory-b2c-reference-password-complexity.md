---
title: Wachtwoordcomplexiteit in Azure Active Directory B2C | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de vereisten voor wachtwoordcomplexiteit voor wachtwoorden die worden geleverd door consumenten in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b16ac10e10655bbc7e41d9336378228097ca19ff
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014717"
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: Vereisten voor wachtwoordcomplexiteit voor wachtwoorden configureren

> [!NOTE]
> **Deze functie is beschikbaar als openbare preview.**

Azure Active Directory B2C (Azure AD B2C) biedt ondersteuning voor de complexiteitsvereisten voldoen voor wachtwoorden die worden geleverd door een eindgebruiker bij het maken van een account te wijzigen.  Azure AD B2C gebruikt standaard `Strong` wachtwoorden.  Azure AD B2C biedt ook ondersteuning voor configuratieopties voor het beheren van de complexiteit van wachtwoorden die klanten kunnen gebruiken.

## <a name="when-password-rules-are-enforced"></a>Wanneer de wachtwoordregels worden afgedwongen

Tijdens het aanmelden of het wachtwoord opnieuw instellen, een eindgebruiker dient een wachtwoord dat voldoet aan de regels voor wachtwoordcomplexiteit.  Regels voor wachtwoordcomplexiteit worden per beleid afgedwongen.  Het is mogelijk dat één beleid vereisen dat een pincode van vier cijfers tijdens registratie terwijl een ander beleid een tekenreeks van acht tekens vereist tijdens de registratie.  Bijvoorbeeld, kunt u een beleid met verschillende wachtwoordcomplexiteit voor volwassenen dan voor kinderen.

Wachtwoordcomplexiteit wordt nooit tijdens het aanmelden afgedwongen.  Gebruikers hun wachtwoord wijzigen, omdat het niet voldoet aan de vereiste voor het huidige complexiteit nooit gevraagd tijdens het aanmelden.

Hier zijn de soorten beleid waar wachtwoordcomplexiteit kan worden geconfigureerd:

* Beleid voor registreren of aanmelden
* Beleid voor wachtwoord opnieuw instellen
* Aangepast beleid ([wachtwoordcomplexiteit configureren in het aangepaste beleid](active-directory-b2c-reference-password-complexity-custom.md))

## <a name="how-to-configure-password-complexity"></a>Wachtwoordcomplexiteit configureren

1. Open **registreren of aanmelden beleidsregels**.
2. Selecteer een beleid en klikt u op **bewerken**.
3. Open **wachtwoordcomplexiteit**.
4. Wijzigen van de wachtwoordcomplexiteit voor dit beleid **eenvoudige**, **sterke**, of **aangepaste**.

### <a name="comparison-chart"></a>Vergelijkingstabel

| Complexiteit | Beschrijving |
| --- | --- |
| Eenvoudig | Een wachtwoord dat ten minste 8 tot 64 tekens. |
| Sterk | Een wachtwoord dat ten minste 8 tot 64 tekens. Hiervoor 3 van 4 van kleine letters, hoofdletters, cijfers en symbolen. |
| Aangepast telefoonnummer | Deze optie biedt de meeste controle over de regels voor wachtwoordcomplexiteit.  Hierdoor kan een aangepaste lengte configureren.  Daarnaast kunt u wachtwoorden alleen-nummer (pincodes) accepteren. |

## <a name="options-available-under-custom"></a>Beschikbare opties onder aangepaste

### <a name="character-set"></a>Tekenset

Hiermee kunt u gaat akkoord met alleen cijfers (pincodes) of de volledige-tekenset.

* **Alleen getallen** kunt cijfers alleen (0-9) bij het invoeren van een wachtwoord.
* **Alle** kunnen een letter, cijfer of symbool.

### <a name="length"></a>Lengte

Kunt u voor het beheren van de vereisten voor de lengte van het wachtwoord.

* **Minimale lengte** moet ten minste 4.
* **Maximale lengte** moet groter of gelijk zijn aan de minimale lengte en kan worden maximaal 64 tekens.

### <a name="character-classes"></a>Tekenklassen

Kunt u voor het beheren van de verschillende tekentypen in het wachtwoord gebruikt.

* **2 van 4: kleine letter, een hoofdletter, een getal (0-9), symbool** zorgt ervoor dat het wachtwoord bevat ten minste twee tekentypen. Bijvoorbeeld, een getal en een kleine letter.
* **3 van 4: kleine letter, een hoofdletter, een getal (0-9), symbool** zorgt ervoor dat het wachtwoord bevat ten minste twee tekentypen. Bijvoorbeeld, een getal, een kleine letter en een hoofdletter.
* **4 van 4: kleine letter, een hoofdletter, een getal (0-9), symbool** zorgt ervoor dat het wachtwoord voor een speciale tekens bevat.

    > [!NOTE]
    > Vereisen van **4 van 4** kan leiden tot frustraties van eindgebruikers. Sommige onderzoek heeft aangetoond dat deze vereiste wachtwoord entropie niet worden verbeterd. Zie [richtlijnen voor het wachtwoord van het NIST](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
