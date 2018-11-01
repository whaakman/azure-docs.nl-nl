---
title: Dynamisch uitsluiten van wachtwoorden in Azure AD
description: Zwakke wachtwoorden van uw omgeving met Azure AD dynamisch uitsluiten van wachtwoorden blokkeren
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 4c5fead0a7f4634a8f5ee005114d24cae9a2590f
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739818"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Onjuiste wachtwoorden in uw organisatie te elimineren

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord en de lijst met aangepaste uitgesloten wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Toonaangevende bedrijven vertellen u niet hetzelfde wachtwoord gebruiken op meerdere plaatsen, zodat complexe, en niet kunt u eenvoudig zoals/Password123. Hoe kunnen organisaties garanderen dat hun gebruikers richtlijnen volgen? Hoe kunnen ze ervoor zorgen dat gebruikers worden niet met behulp van algemene wachtwoorden of wachtwoorden die bekend is dat ze worden opgenomen in recente gegevensschendingen?

## <a name="global-banned-password-list"></a>Lijst met uitgesloten wachtwoorden globale

Microsoft stelt altijd alles in het werk om cybercriminelen een stap voor te blijven. Het Azure AD Identity Protection-team zoeken daarom voortdurend voor wachtwoorden voor vaak gebruikte en waarmee is geknoeid. Ze vervolgens blokkeren deze wachtwoorden op dat te veel voorkomt in wat de lijst met algemene uitgesloten wachtwoorden heet worden geacht. Cybercriminelen maken gebruik van vergelijkbare strategieën in hun aanvallen, dus Microsoft niet publiceert de inhoud van deze lijst openbaar. Deze kwetsbare wachtwoorden worden geblokkeerd voordat ze een echte bedreiging voor klanten van Microsoft. Zie voor meer informatie over de inspanningen de [Microsoft Security Intelligence Report](https://www.microsoft.com/security/intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Voorbeeld: Aangepaste verboden lijst met wachtwoorden

Sommige organisaties kunt voor een beveiligingsstap verder door hun eigen aanpassingen boven op de lijst met uitgesloten wachtwoorden globale toe te voegen in wat Microsoft de lijst met aangepaste uitgesloten wachtwoorden. Zakelijke klanten, zoals Contoso kiest u kan voor het blokkeren van varianten van hun merknamen, bedrijfsspecifieke termen of andere items.

De aangepaste uitgesloten lijst van wachtwoorden en de mogelijkheid om in te schakelen van Active Directory van on-premises integratie wordt beheerd met behulp van de Azure portal.

![Wijzigen van de aangepaste lijst met uitgesloten wachtwoorden onder verificatiemethoden in de Azure-portal](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>On-premises hybride scenario 's

Beveiligen van accounts alleen in de cloud is het handig, maar in veel organisaties hebben hybride scenario's zoals on-premises Windows Server Active Directory. Het is mogelijk voor het installeren van de beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory (preview) agents on-premises naar de lijsten met uitgesloten wachtwoorden met uw bestaande infrastructuur uitbreiden. Nu gebruikers en beheerders die wijzigt, instellen of opnieuw instellen van wachtwoorden is on-premises zijn vereist om te voldoen aan het beleid voor dezelfde als gebruikers alleen in de cloud.

## <a name="how-does-the-banned-password-list-work"></a>Hoe werkt de lijst met uitgesloten wachtwoorden

De lijst met uitgesloten wachtwoorden komt overeen met wachtwoorden in de lijst met het converteren van de tekenreeks naar hoofdletters en kleine letters vergeleken aan de bekende uitgesloten wachtwoorden binnen een afstand van het bewerken van 1 met zoeken bij benadering.

Voorbeeld: Het word-wachtwoord voor een organisatie is geblokkeerd
   - Een gebruiker wil hun wachtwoord ingesteld op 'P@ssword' die wordt geconverteerd naar 'wachtwoord' en omdat het een variant van het wachtwoord wordt geblokkeerd.
   - Een beheerder wil instellen dat een wachtwoord gebruikers '/ 'Password123! die geconverteerd naar '/ 'password123! en omdat het een variant van het wachtwoord wordt geblokkeerd.

Telkens wanneer een gebruiker wordt opnieuw ingesteld of Azure AD wachtwoord die hiervandaan stroomt het via deze procedure om te bevestigen dat deze zich niet op de lijst met uitgesloten wachtwoorden. Deze controle is opgenomen in hybride scenario's die gebruikmaken van self-service voor wachtwoord opnieuw instellen en synchronisatie van wachtwoordhashes, pass-through-verificatie.

## <a name="license-requirements"></a>Licentievereisten

|   | Azure AD-wachtwoordbeveiliging met de lijst met uitgesloten wachtwoorden globale | Azure AD-wachtwoordbeveiliging met de lijst met aangepaste uitgesloten wachtwoorden|
| --- | --- | --- |
| Cloudgebruikers | Azure AD Free | Azure AD Basic |
| Gebruikers die zijn gesynchroniseerd vanaf on-premises Windows Server Active Directory | Azure AD Premium P1 of P2 | Azure AD Premium P1 of P2 |

Extra licentie-informatie, inclusief kosten, vindt u op de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Wat gebruikers zien

Wanneer een gebruiker probeert een wachtwoord opnieuw wordt ingesteld op iets dat zou worden geblokkeerd, zien ze de volgende strekking weergegeven:

Helaas komt bevat uw wachtwoord een woord, woordgroep of patroon dat uw wachtwoord gemakkelijk te raden maakt. Probeer het opnieuw met een ander wachtwoord.

## <a name="next-steps"></a>Volgende stappen

* [De lijst met aangepaste uitgesloten wachtwoorden configureren](howto-password-ban-bad.md)
* [Azure AD inschakelen protection agents on-premises wachtwoord](howto-password-ban-bad-on-premises-deploy.md)
