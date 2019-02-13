---
title: Dynamisch uitsluiten van wachtwoorden in Azure AD
description: Zwakke wachtwoorden van uw omgeving met Azure AD dynamisch uitsluiten van wachtwoorden blokkeren
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96e2c3cfd509c9b0b77d0db00add31b58a07ce6a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56206545"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Onjuiste wachtwoorden in uw organisatie te elimineren

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord en de lijst met aangepaste uitgesloten wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Toonaangevende bedrijven vertellen u niet hetzelfde wachtwoord gebruiken op meerdere plaatsen, zodat complexe, en niet kunt u eenvoudig zoals/Password123. Hoe kunnen organisaties garanderen dat hun gebruikers richtlijnen volgen? Hoe kunnen ze ervoor zorgen dat gebruikers worden niet met behulp van algemene wachtwoorden of wachtwoorden die bekend is dat ze worden opgenomen in recente gegevensschendingen?

## <a name="global-banned-password-list"></a>Lijst met uitgesloten wachtwoorden globale

Microsoft stelt altijd alles in het werk om cybercriminelen een stap voor te blijven. Het Azure AD Identity Protection-team zoeken daarom voortdurend voor wachtwoorden voor vaak gebruikte en waarmee is geknoeid. Ze vervolgens blokkeren deze wachtwoorden op dat te veel voorkomt in wat de lijst met algemene uitgesloten wachtwoorden heet worden geacht. Cybercriminelen maken gebruik van vergelijkbare strategieën in hun aanvallen, dus Microsoft niet publiceert de inhoud van deze lijst openbaar. Deze kwetsbare wachtwoorden worden geblokkeerd voordat ze een echte bedreiging voor klanten van Microsoft. Zie voor meer informatie over de inspanningen de [Microsoft Security Intelligence Report](https://www.microsoft.com/security/operations/security-intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Preview: Aangepaste lijst met verboden wachtwoorden

Sommige organisaties kunt voor een beveiligingsstap verder door hun eigen aanpassingen boven op de lijst met uitgesloten wachtwoorden globale toe te voegen in wat Microsoft de lijst met aangepaste uitgesloten wachtwoorden. Zakelijke klanten, zoals Contoso kiest u kan voor het blokkeren van varianten van hun merknamen, bedrijfsspecifieke termen of andere items.

De aangepaste uitgesloten lijst van wachtwoorden en de mogelijkheid om in te schakelen van Active Directory van on-premises integratie wordt beheerd met behulp van de Azure portal.

![Wijzigen van de aangepaste lijst met uitgesloten wachtwoorden onder verificatiemethoden in de Azure-portal](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>On-premises hybride scenario 's

Beveiligen van accounts alleen in de cloud is het handig, maar in veel organisaties hebben hybride scenario's zoals on-premises Windows Server Active Directory. Het is mogelijk voor het installeren van de beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory (preview) agents on-premises naar de lijsten met uitgesloten wachtwoorden met uw bestaande infrastructuur uitbreiden. Nu gebruikers en beheerders die wijzigt, instellen of opnieuw instellen van wachtwoorden is on-premises zijn vereist om te voldoen aan het beleid voor dezelfde als gebruikers alleen in de cloud.

## <a name="how-are-passwords-evaluated"></a>Hoe worden wachtwoorden geëvalueerd

Wanneer een gebruiker wordt gewijzigd of hun wachtwoord opnieuw instellen, wordt het nieuwe wachtwoord voor de kracht en complexiteit gecontroleerd door te valideren op basis van de globale en de lijst met aangepaste uitgesloten wachtwoorden (indien deze is geconfigureerd).

Zelfs als het wachtwoord van een gebruiker een uitgesloten wachtwoorden bevat, kan het wachtwoord nog steeds worden geaccepteerd als het algehele wachtwoord anders sterk genoeg is. Een nieuwe ingestelde wachtwoord gaat via de volgende stappen uit om de algehele sterkte om te bepalen als kan deze moet worden geaccepteerd of geweigerd vast te stellen.

### <a name="step-1-normalization"></a>Stap 1: Normalisering

Een nieuw wachtwoord gaat eerst via een normalisatieproces. Hierdoor een kleine set uitgesloten wachtwoorden worden toegewezen aan een veel grotere set van potentieel zwakke wachtwoorden.

Normalisering bestaat uit twee delen.  Eerste, allemaal hoofdletters letters worden omgezet in kleine letters.  Tweede, algemene teken vervangingen worden uitgevoerd, bijvoorbeeld:  

| Oorspronkelijke letter  | Vervangen letter |
| --- | --- |
| '0'  | -o ' |
| '1'  | 'l' |
| '$'  | de ' |
| '@'  | 'a' |

Voorbeeld: Stel dat het wachtwoord 'leeg' is niet toegestaan en een gebruiker probeert het wachtwoord te wijzigen 'Bl@nK'. Hoewel 'Bl@nk' is niet expliciet is uitgesloten, converteert het normalisatieproces dit wachtwoord "blank", die een uitgesloten wachtwoorden.

### <a name="step-2-check-if-password-is-considered-banned"></a>Stap 2: Controleer of het wachtwoord wordt beschouwd als verboden

#### <a name="fuzzy-matching-behavior"></a>Fuzzy overeenkomende gedrag

Zoeken bij benadering wordt gebruikt op de genormaliseerde wachtwoord om te bepalen of deze een wachtwoord op ofwel de globale gevonden bevat of de aangepaste lijsten met wachtwoorden verboden. Het overeenkomende proces is gebaseerd op een afstand van het bewerken van één (1) vergelijking.  

Voorbeeld: Stel dat het wachtwoord "abcdef" is niet toegestaan en een gebruiker wil hun wachtwoord wijzigen in een van de volgende:

'abcdeg'    *(laatste teken gewijzigd van 'f' op 'g')* 'abcdefg'   *' (g' toegevoegd aan het einde)* 'abcde'     *(afsluitende 'f' is verwijderd vanaf eind)*

Elk van de bovenstaande wachtwoorden komt de uitgesloten wachtwoorden "abcdef" specifiek niet overeen. Echter, omdat elk voorbeeld binnen een afstand van het bewerken van 1 van het verboden token "abcdef", ze alle beschouwd als "abcdef" van een overeenkomst.

#### <a name="substring-matching-on-specific-terms"></a>De subtekenreeks die overeenkomt met (op specifieke voorwaarden)

Subtekenreeksen op het genormaliseerde wachtwoord wordt gebruikt om te controleren of de gebruiker eerst's en achternaam en de naam van de tenant (Houd er rekening mee dat overeenkomt met de naam van tenant is niet uitgevoerd bij het valideren van wachtwoorden op een Active Directory-domeincontroller).

Voorbeeld: Stel dat we beschikken over een gebruiker Jan de Vries die wil opnieuw instellen van zijn/haar wachtwoord te 'J0hn123fb'. Na normaliseren, zou dit wachtwoord "john123fb" worden. Subtekenreeksen vindt dat het wachtwoord van de gebruiker eerst de naam 'John' bevat. Hoewel 'J0hn123fb' niet specifiek op een lijst met uitgesloten wachtwoorden is, subtekenreeksen 'John' gevonden in het wachtwoord. Dit wachtwoord zou dus worden geweigerd.

#### <a name="score-calculation"></a>Berekening van de risicoscore

De volgende stap is het identificeren van alle exemplaren van de uitgesloten wachtwoorden in genormaliseerde nieuwe wachtwoord van de gebruiker. Vervolgens:

1. Elke uitgesloten wachtwoorden die is gevonden in het wachtwoord van een gebruiker ontvangt één punt.
2. Alle resterende unieke tekens krijgt één punt.
3. Een wachtwoord moet ten minste 5 punten om te worden geaccepteerd.

Voor de volgende twee voorbeelden laten we wordt ervan uitgegaan dat Contoso is met behulp van Azure AD-wachtwoord beveiliging 'contoso' in de lijst met aangepaste heeft. We gaan ook wordt ervan uitgegaan dat 'leeg' in de lijst met algemene.

Voorbeeld: een gebruiker het wachtwoord wijzigt naar "C0ntos0Blank12"

Na normaliseren wordt dit wachtwoord 'contosoblank12'. Het overeenkomende proces wordt gevonden dat dit wachtwoord twee uitgesloten wachtwoorden bevat: contoso en leeg. Dit wachtwoord wordt vervolgens een score gegeven:

[contoso] + [lege] [1] = + [2] = 4 punten omdat dit wachtwoord onder 5 punten is, wordt dit geweigerd.

Voorbeeld: een gebruiker wijzigt het wachtwoord op 'ContoS0Bl@nkf9! ".

Na normaliseren wordt dit wachtwoord "contosoblankf9!". Het overeenkomende proces wordt gevonden dat dit wachtwoord twee uitgesloten wachtwoorden bevat: contoso en leeg. Dit wachtwoord wordt vervolgens een score gegeven:

[contoso] + [lege] + [v] + [9] + [!] = 5 punten omdat dit wachtwoord ten minste 5 punten is, het wordt geaccepteerd.

   > [!IMPORTANT]
   > Houd er rekening mee dat het algoritme uitgesloten wachtwoorden, samen met de globale lijst kunt en veranderen op elk gewenst moment in Azure op basis van continue beveiligingsanalyse en onderzoek. Voor de on-premises DC-agentservice bijgewerkte algoritmen wordt pas van kracht nadat de software van de agent DC opnieuw te installeren.

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
