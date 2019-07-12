---
title: Dynamisch uitsluiten van wachtwoorden - Azure Active Directory
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
ms.openlocfilehash: c043b2ed1a626e362d7edd1a83429aa14046f8ac
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703060"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Onjuiste wachtwoorden in uw organisatie te elimineren

Toonaangevende bedrijven vertellen u niet hetzelfde wachtwoord gebruiken op meerdere plaatsen, zodat complexe, en niet kunt u eenvoudig, zoals '/ 'Password123. Hoe kunnen organisaties garanderen dat hun gebruikers best practice-richtlijnen volgen? Hoe kunnen ze ervoor zorgen dat gebruikers worden niet met behulp van zwakke wachtwoorden, of zelfs variaties op zwakke wachtwoorden?

De eerste stap voor het hebben van sterke wachtwoorden is begeleiding te leveren aan uw gebruikers. De huidige richtlijnen van Microsoft over dit onderwerp kunt u vinden op de volgende koppeling:

[Wachtwoord voor Microsoft-richtlijnen](https://www.microsoft.com/research/publication/password-guidance)

Met goede richtlijnen is belangrijk, maar ook met dat we weten dat veel gebruikers wordt nog steeds terechtkomen zwakke wachtwoorden te kiezen. Beveiliging van Azure AD-wachtwoord beveiligt uw organisatie door detecteren en blokkeren van bekende zwakke wachtwoorden en de bijbehorende varianten, evenals aanvullende zwakke voorwaarden die specifiek voor uw organisatie zijn (optioneel) blokkeren.

Zie voor meer informatie over de inspanningen de [Microsoft Security Intelligence Report](https://www.microsoft.com/security/operations/security-intelligence-report).

## <a name="global-banned-password-list"></a>Lijst met uitgesloten wachtwoorden globale

Het Azure AD Identity Protection-team analyseert voortdurend telemetrie van Azure AD security-gegevens op zoek voor vaak gebruikte wachtwoorden op zwakke of waarmee is geknoeid, of meer in het bijzonder, de weak baseren termen die vaak als basis voor zwakke wachtwoorden gebruikt. Wanneer deze voorwaarden zijn zwakke worden gevonden, worden ze toegevoegd aan de lijst met algemene uitgesloten wachtwoorden. De inhoud van de lijst met algemene uitgesloten wachtwoorden zijn niet gebaseerd op een externe gegevensbron. De lijst met uitgesloten wachtwoorden globale is volledig op de lopende resultaten van Azure AD security Telemetrie en -analyse gebaseerd.

Telkens wanneer een nieuw wachtwoord is gewijzigd of opnieuw voor een gebruiker in een tenant in Azure AD instellen, wordt de huidige versie van de lijst met uitgesloten wachtwoorden globale gebruikt als sleutel bij het valideren van de kracht van het wachtwoord invoeren. Deze validatie resulteert in veel sterkere wachtwoorden voor alle klanten van Azure AD.

> [!NOTE]
> Cybercriminelen maken gebruik van vergelijkbare strategieën in hun aanvallen. Daarom wordt de inhoud van deze lijst niet openbaar gepubliceerd Microsoft.

## <a name="custom-banned-password-list"></a>Aangepaste lijst met wachtwoorden verboden

Sommige organisaties willen beveiliging nog verder verbeteren door het toevoegen van hun eigen aanpassingen boven op de lijst met uitgesloten wachtwoorden globale in wat Microsoft de lijst met aangepaste uitgesloten wachtwoorden. Microsoft raadt aan dat voorwaarden toegevoegd aan deze lijst voornamelijk op de organisatie-specifieke voorwaarden zoals gericht zijn:

- Merknamen
- Productnamen
- Locaties (bijvoorbeeld, zoals een hoofdkantoor)
- Bedrijfsspecifiek intern voorwaarden
- Afkortingen die specifiek bedrijf, wat betekent dat hebben.

Zodra de voorwaarden worden toegevoegd aan de lijst met aangepaste uitgesloten wachtwoorden, wordt deze toegevoegd aan de lijst algemene uitgesloten wachtwoorden bij het valideren van wachtwoorden.

> [!NOTE]
> De lijst met aangepaste uitgesloten wachtwoorden is beperkt tot maximaal 1000 voorwaarden met. Het is niet ontworpen voor het blokkeren van zeer grote lijsten van wachtwoorden. Om volledig gebruik maken van de voordelen van de lijst met aangepaste uitgesloten wachtwoorden, wordt aangeraden dat u eerst controleren en begrijpen van de algoritme van de evaluatie van wachtwoord (Zie [hoe wachtwoorden worden geëvalueerd](concept-password-ban-bad.md#how-are-passwords-evaluated)) vóór het toevoegen van nieuwe voorwaarden naar de aangepaste lijst met uitgesloten. Informatie over hoe de algoritme werkt in uw onderneming op efficiënte wijze detecteren en blokkeren van grote aantallen zwakke wachtwoorden en de bijbehorende varianten kunt.

Bijvoorbeeld: u kunt een klant met de naam 'Contoso', die is gebaseerd in Londen bevinden, en Hiermee kunt u een product met de naam 'Widget'. Voor een klant normaal zou het zijn verspilling, evenals minder veilig is om te proberen te blokkeren van bepaalde variaties van deze voorwaarden zoals:

- "Contoso!1"
- "Contoso@London"
- "ContosoWidget"
- "!Contoso"
- "LondonHQ"
- ...etcetera

In plaats daarvan is het nog veel meer efficiënte en veilige moet worden geblokkeerd dat alleen de belangrijkste basis termen:

- "Contoso"
- "Londen"
- "Widget"

Het algoritme voor gegevensvalidatie gebruikt wachtwoord wordt vervolgens automatisch blokkeren zwakke varianten en combinaties van de bovenstaande.

De aangepaste uitgesloten lijst van wachtwoorden en de mogelijkheid om in te schakelen van Active Directory van on-premises integratie wordt beheerd met behulp van de Azure portal.

![De lijst met aangepaste uitgesloten wachtwoorden onder verificatiemethoden aanpassen](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="password-spray-attacks-and-third-party-compromised-password-lists"></a>Wachtwoord spray aanvallen en het wachtwoord is verdacht van derden lijsten

Een sleutel voordeel van Azure AD wachtwoord beveiliging is om te beschermen tegen aanvallen van wachtwoord spray. De meeste wachtwoord spray aanvallen Probeer niet om aan te vallen alle opgegeven afzonderlijke account omdat u dergelijk gedrag aanzienlijk verbeterd, evenals de kans op detectie, accountvergrendeling of andere manier via meer dan een paar keer. De meeste wachtwoord spray aanvallen is daarom zijn afhankelijk van het indienen van slechts een klein aantal van de bekende zwakste wachtwoorden voor elk van de accounts in een onderneming. Deze techniek kan de aanvaller snel zoeken naar een eenvoudig verdacht account terwijl u zich op hetzelfde moment voorkomen van potentiële detectie drempelwaarden.

Azure AD-wachtwoordbeveiliging is ontworpen om efficiënt blokkeert alle bekende zwakke wachtwoorden die kunnen worden gebruikt bij op basis van echte security telemetrische gegevens zoals gezien door Azure AD-wachtwoord spray aanvallen.  Microsoft is zich bewust van websites van derden die het inventariseren van miljoenen wachtwoorden die in vorige openbaar bekende beveiligingslekken zijn aangetast. Het is gebruikelijk voor producten van derden wachtwoord validatie worden gebaseerd op een brute-force vergelijking op basis van die miljoenen wachtwoorden. Microsoft vindt dat dergelijke technieken niet de beste manier zijn om de algehele Wachtwoordsterkte gegeven van de typische strategieën wordt gebruikt door aanvallers van wachtwoord spray verbeteren.

> [!NOTE]
> De lijst met uitgesloten wachtwoorden globale niet is van Microsoft gebaseerd gesteld op alle gegevens van derden bronnen, met inbegrip van het wachtwoord is verdacht lijsten.

Hoewel de algemene uitgesloten lijst met Microsoft klein in vergelijking met sommige lijsten van derden bulksgewijs is, de effecten van de beveiliging worden versterkt door het feit dat het afkomstig is uit de praktijk security telemetrie over de echte wachtwoorden spray aanvallen, plus het feit dat de Microsoft algoritme voor gegevensvalidatie gebruikt wachtwoord maakt gebruik van slim zoeken bij benadering die overeenkomt met technieken. Het eindresultaat is dat deze wordt efficiënt detecteren en blokkeren van miljoenen van de meest voorkomende zwakke wachtwoorden worden gebruikt in uw onderneming. Klanten die de organisatie-specifieke voorwaarden toevoegen aan de lijst met uitgesloten wachtwoorden aangepaste profiteren ook van dezelfde algoritme.

## <a name="on-premises-hybrid-scenarios"></a>On-premises hybride scenario 's

Beveiligen van accounts alleen in de cloud is het handig, maar in veel organisaties hebben hybride scenario's zoals on-premises Windows Server Active Directory. Het is mogelijk voor het installeren van de beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory-agents on-premises naar de lijsten met uitgesloten wachtwoorden met uw bestaande infrastructuur uitbreiden. Nu gebruikers en beheerders die wijzigt, instellen of opnieuw instellen van wachtwoorden is on-premises zijn vereist om te voldoen aan het beleid voor dezelfde als gebruikers alleen in de cloud.

## <a name="how-are-passwords-evaluated"></a>Hoe worden wachtwoorden geëvalueerd

Wanneer een gebruiker wordt gewijzigd of hun wachtwoord opnieuw instellen, wordt het nieuwe wachtwoord voor de kracht en complexiteit gecontroleerd door te valideren op basis van de globale en de lijst met aangepaste uitgesloten wachtwoorden (indien deze is geconfigureerd).

Zelfs als het wachtwoord van een gebruiker een uitgesloten wachtwoorden bevat, kan het wachtwoord nog steeds worden geaccepteerd als het algehele wachtwoord anders sterk genoeg is. Een nieuwe ingestelde wachtwoord gaat via de volgende stappen uit om de algehele sterkte om te bepalen als kan deze moet worden geaccepteerd of geweigerd vast te stellen.

### <a name="step-1-normalization"></a>Stap 1: Normalisering

Een nieuw wachtwoord gaat eerst via een normalisatieproces. Deze techniek kan voor een kleine set met uitgesloten wachtwoorden worden toegewezen aan een veel grotere set van potentieel zwakke wachtwoorden.

Normalisering bestaat uit twee delen.  Eerste, allemaal hoofdletters letters worden omgezet in kleine letters.  Tweede, algemene teken vervangingen worden uitgevoerd, bijvoorbeeld:  

| Oorspronkelijke letter  | Vervangen letter |
| --- | --- |
| '0'  | -o ' |
| '1'  | 'l' |
| '$'  | de ' |
| '\@'  | 'a' |

Voorbeeld: Stel dat het wachtwoord 'leeg' is niet toegestaan en een gebruiker probeert het wachtwoord te wijzigen 'Bl@nK'. Hoewel 'Bl@nk' is niet expliciet is uitgesloten, converteert het normalisatieproces dit wachtwoord "blank", die een uitgesloten wachtwoorden.

### <a name="step-2-check-if-password-is-considered-banned"></a>Stap 2: Controleer of het wachtwoord wordt beschouwd als verboden

#### <a name="fuzzy-matching-behavior"></a>Fuzzy overeenkomende gedrag

Zoeken bij benadering wordt gebruikt op de genormaliseerde wachtwoord om te bepalen of deze een wachtwoord op ofwel de globale gevonden bevat of de aangepaste lijsten met wachtwoorden verboden. Het overeenkomende proces is gebaseerd op een afstand van het bewerken van één (1) vergelijking.  

Voorbeeld: Stel dat het wachtwoord "abcdef" is niet toegestaan en een gebruiker wil hun wachtwoord wijzigen in een van de volgende:

'abcdeg'    *(laatste teken gewijzigd van 'f' op 'g')* 'abcdefg'   *' (g' toegevoegd aan het einde)* 'abcde'     *(afsluitende 'f' is verwijderd vanaf eind)*

Elk van de bovenstaande wachtwoorden komt de uitgesloten wachtwoorden "abcdef" specifiek niet overeen. Echter, omdat elk voorbeeld binnen een afstand van het bewerken van 1 van de uitgesloten term "abcdef", ze alle beschouwd als "abcdef" van een overeenkomst.

#### <a name="substring-matching-on-specific-terms"></a>De subtekenreeks die overeenkomt met (op specifieke voorwaarden)

Subtekenreeksen op het genormaliseerde wachtwoord wordt gebruikt om te controleren of de gebruiker eerst's en achternaam en de naam van de tenant (Houd er rekening mee dat overeenkomt met de naam van tenant is niet uitgevoerd bij het valideren van wachtwoorden op een Active Directory-domeincontroller).

Voorbeeld: Stel dat we beschikken over een gebruiker, Pol, die hun wachtwoord te "P0l123fb" wil. Na normaliseren, zou dit wachtwoord "pol123fb" worden. Subtekenreeksen vindt dat het wachtwoord van de gebruiker eerst de naam "Pol" bevat. Hoewel 'P0l123fb' niet specifiek op een lijst met uitgesloten wachtwoorden is, subtekenreeksen 'Pol' gevonden in het wachtwoord. Dit wachtwoord zou dus worden geweigerd.

#### <a name="score-calculation"></a>Berekening van de risicoscore

De volgende stap is het identificeren van alle exemplaren van de uitgesloten wachtwoorden in genormaliseerde nieuwe wachtwoord van de gebruiker. Vervolgens:

1. Elke uitgesloten wachtwoorden die is gevonden in het wachtwoord van een gebruiker ontvangt één punt.
2. Alle resterende unieke tekens krijgt één punt.
3. Een wachtwoord moet ten minste vijf (5) punten om te worden geaccepteerd.

Voor de volgende twee voorbeelden laten we wordt ervan uitgegaan dat Contoso is met behulp van Azure AD-wachtwoord beveiliging 'contoso' in de lijst met aangepaste heeft. We gaan ook wordt ervan uitgegaan dat 'leeg' in de lijst met algemene.

Voorbeeld: een gebruiker het wachtwoord wijzigt naar "C0ntos0Blank12"

Na normaliseren wordt dit wachtwoord 'contosoblank12'. Het overeenkomende proces wordt gevonden dat dit wachtwoord twee uitgesloten wachtwoorden bevat: contoso en leeg. Dit wachtwoord wordt vervolgens een score gegeven:

[contoso] + [lege] + [1] + [2] = 4 punten omdat dit wachtwoord onder vijf (5) verwijst is, wordt dit geweigerd.

Voorbeeld: een gebruiker wijzigt het wachtwoord op 'ContoS0Bl@nkf9! ".

Na normaliseren wordt dit wachtwoord "contosoblankf9!". Het overeenkomende proces wordt gevonden dat dit wachtwoord twee uitgesloten wachtwoorden bevat: contoso en leeg. Dit wachtwoord wordt vervolgens een score gegeven:

[contoso] + [lege] + [v] + [9] + [!] = 5 punten omdat dit wachtwoord ten minste vijf (5) verwijst is, is deze geaccepteerd.

   > [!IMPORTANT]
   > Houd er rekening mee dat het algoritme uitgesloten wachtwoorden, samen met de globale lijst kunt en veranderen op elk gewenst moment in Azure op basis van continue beveiligingsanalyse en onderzoek. Voor de on-premises DC-agentservice bijgewerkte algoritmen wordt pas van kracht nadat de software van de agent DC opnieuw te installeren.

## <a name="license-requirements"></a>Licentievereisten

|   | Azure AD-wachtwoordbeveiliging met de lijst met uitgesloten wachtwoorden globale | Azure AD-wachtwoordbeveiliging met de lijst met aangepaste uitgesloten wachtwoorden|
| --- | --- | --- |
| Cloudgebruikers | Azure AD Free | Azure AD Premium P1 of P2 |
| Gebruikers die zijn gesynchroniseerd vanaf on-premises Windows Server Active Directory | Azure AD Premium P1 of P2 | Azure AD Premium P1 of P2 |

> [!NOTE]
> On-premises Windows Server Active Directory-gebruikers die niet zijn gesynchroniseerd met Azure Active Directory gebruik ook de voordelen van Azure AD-wachtwoordbeveiliging op basis van bestaande licentieverlening voor gesynchroniseerde gebruikers.

Extra licentie-informatie, inclusief kosten, vindt u op de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Wat gebruikers zien

Wanneer een gebruiker probeert een wachtwoord opnieuw wordt ingesteld op iets dat zou worden geblokkeerd, zien ze de volgende strekking weergegeven:

Helaas komt bevat uw wachtwoord een woord, woordgroep of patroon dat uw wachtwoord gemakkelijk te raden maakt. Probeer het opnieuw met een ander wachtwoord.

## <a name="next-steps"></a>Volgende stappen

- [De lijst met aangepaste uitgesloten wachtwoorden configureren](howto-password-ban-bad.md)
- [Azure AD inschakelen protection agents on-premises wachtwoord](howto-password-ban-bad-on-premises-deploy.md)
