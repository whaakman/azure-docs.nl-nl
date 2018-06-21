---
title: Dynamisch verboden wachtwoorden in Azure AD
description: BEen zwakke wachtwoorden van uw omgeving met Azure AD dynamisch verboden-wachtwoorden
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 89cbe386d87c6ccb81df7fabd86b197bb69e41e1
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295608"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Onjuiste wachtwoorden in uw organisatie elimineren

|     |
| --- |
| Azure AD-wachtwoordbeveiliging en de lijst met aangepaste verboden wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Toonaangevende bedrijven vertellen u niet hetzelfde wachtwoord gebruiken op meerdere plaatsen, zodat complexe, en niet kunt u eenvoudig zoals Password123. Hoe kunnen organisaties garanderen dat gebruikers richtlijnen volgen Hoe ze ervoor kunnen zorgen gebruikers worden niet met behulp van algemene wachtwoorden of wachtwoorden op dat bekend is dat ze worden opgenomen in recente schendingen van gegevens?

## <a name="global-banned-password-list"></a>Lijst met algemene verboden wachtwoorden

Microsoft werkt altijd om te blijven van één stap tevoren cyberbeveiliging criminelen. Daarom zoekt het team van Azure AD Identity Protection voortdurend wachtwoorden voor vaak gebruikte en waarmee is geknoeid. Ze blokkeren vervolgens deze wachtwoorden die worden geacht te vaak worden uitgevoerd in de lijst met algemene verboden wachtwoorden wordt aangeduid. Vergelijkbare strategieën Cyberbeveiliging criminelen ook gebruiken in hun aanvallen, daarom Microsoft publiceert niet de inhoud van deze lijst openbaar. Deze kwetsbaar wachtwoorden worden geblokkeerd voordat ze een echte bedreiging voor de klanten van Microsoft. Zie voor meer informatie over de huidige beveiligingsinspanningen, de [Microsoft Intelligence beveiligingsrapport](https://www.microsoft.com/security/intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Voorbeeld: Aangepaste verboden lijst met wachtwoorden

Sommige organisaties willen nemen beveiliging een stapje verder door hun eigen aanpassingen boven op de lijst met algemene verboden wachtwoorden toe te voegen in wat Microsoft de lijst met aangepaste verboden wachtwoorden roept. Enterprise-klanten zoals Contoso kiest u kan voor het blokkeren van varianten van hun merknaam, bedrijfsspecifieke termen of andere items.

De aangepaste uitgesloten wachtwoordenlijst en de mogelijkheid om in te schakelen op lokale Active Directory integratie wordt beheerd met de Azure portal.

![Wijzigen van de lijst met aangepaste verboden wachtwoorden onder verificatiemethoden in de Azure portal](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Lokale hybride scenario 's

Alleen in de cloud accounts beveiligen is het handig, maar in veel organisaties onderhouden hybride scenario's met inbegrip van de lokale Windows Server Active Directory. Het is mogelijk voor het installeren van Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory (preview) agents on-premises de wachtwoordenlijsten verboden aan uw bestaande infrastructuur uitbreiden. Nu gebruikers en beheerders die wijzigt, instellen of opnieuw instellen van wachtwoorden zijn lokale vereist om te voldoen aan de beleidsregels voor dezelfde wachtwoorden als gebruikers alleen in de cloud.

## <a name="how-does-the-banned-password-list-work"></a>Hoe werkt de lijst met uitgesloten wachtwoorden

De wachtwoordenlijst verboden komt overeen met wachtwoorden in de lijst door de tekenreeks converteren naar kleine letters en vergelijken het bekende verboden wachtwoorden binnen een afstand bewerken van 1 met fuzzy overeenkomende.

Voorbeeld: Het word-wachtwoord voor een organisatie is geblokkeerd
   - Een gebruiker wil hun wachtwoord ingesteld op 'P@ssword' die wordt geconverteerd naar 'password' en omdat het een variant van het wachtwoord is geblokkeerd.
   - Een beheerder wil instellen dat een wachtwoord gebruikers 'Password123!' die geconverteerd naar 'password123!' en omdat het een variant van het wachtwoord is geblokkeerd.

Telkens wanneer een gebruiker stelt of hun Azure AD-wachtwoord die wordt gebruikt door dit proces om te bevestigen dat deze zich niet op de wachtwoordenlijst verboden wijzigt. Deze controle is opgenomen in een hybride scenario's die gebruikmaken van zelf uw wachtwoord opnieuw instellen, wachtwoordhashsynchronisatie en Pass through-verificatie.

## <a name="license-requirements"></a>Licentievereisten

De voordelen van de lijst met algemene verboden wachtwoorden gelden voor alle gebruikers van Azure Active Directory (Azure AD).

De lijst met aangepaste verboden wachtwoorden vereist Azure AD Basic licenties.

Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory vereist Azure AD Premium-licenties. 

Aanvullende licentie-informatie, inclusief kosten, kunt u vinden op de [Azure Active Directory website prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Wat gebruikers zien

Wanneer een gebruiker probeert een wachtwoord opnieuw instellen met een probleem dat zou worden verboden, zien ze het volgende foutbericht weergegeven:

Helaas bevat uw wachtwoord een woord, woordgroep of patroon waardoor uw wachtwoord gemakkelijk te raden. Probeer het opnieuw met een ander wachtwoord.

## <a name="next-steps"></a>Volgende stappen

* [De lijst met aangepaste verboden wachtwoorden configureren](howto-password-ban-bad.md)
* [Azure AD inschakelen wachtwoord protection agents lokale](howto-password-ban-bad-on-premises.md)
