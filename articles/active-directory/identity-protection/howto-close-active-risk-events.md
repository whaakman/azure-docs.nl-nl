---
title: Actieve risico gebeurtenissen in Azure Active Directory Identity Protection sluiten | Microsoft Docs
description: Meer informatie over de opties waarmee u actieve risico gebeurtenissen kunt sluiten.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5e24c12b72852ee7009533c8dc24d231fe636f2
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333998"
---
# <a name="how-to-close-active-risk-events"></a>Procedure: Actieve risicogebeurtenissen sluiten

Met [risico gebeurtenissen](../reports-monitoring/concept-risk-events.md)Azure Active Directory detecteert indica toren voor mogelijk gemanipuleerde gebruikers accounts. Als beheerder wilt u alle risico gebeurtenissen gesloten laten, zodat de betrokken gebruikers niet langer risico lopen.

In dit artikel vindt u een overzicht van de aanvullende opties die u hebt om actieve risico gebeurtenissen te sluiten.

## <a name="options-to-close-risk-events"></a>Opties voor het sluiten van risico gebeurtenissen 

De status van een risico gebeurtenis is **actief** of **gesloten**. Alle actieve risico gebeurtenissen dragen bij aan de berekening van een waarde met de naam gebruikers risico niveau. Het risico niveau van de gebruiker is een indicator (laag, gemiddeld, hoog) voor de kans dat een account is aangetast. 

Als u actieve risico gebeurtenissen wilt sluiten, hebt u de volgende opties:

- Vereisen dat wacht woord opnieuw wordt ingesteld met een beleid voor gebruikers Risico's
- Hand matig opnieuw instellen van wacht woorden
- Alle risico gebeurtenissen verwijderen 
- Afzonderlijke risico gebeurtenissen hand matig sluiten

## <a name="require-password-reset-with-a-user-risk-policy"></a>Vereisen dat wacht woord opnieuw wordt ingesteld met een beleid voor gebruikers Risico's

Door het [beleid voor voorwaardelijke toegang voor gebruikers Risico's](howto-user-risk-policy.md)te configureren, kunt u een wachtwoord wijziging vereisen als een opgegeven gebruikers risico niveau automatisch is gedetecteerd. 

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/13.png)

Bij het opnieuw instellen van een wacht woord worden alle actieve risico gebeurtenissen van de gerelateerde gebruiker gesloten en wordt de identiteit weer ingesteld op een veilige status. Het gebruik van een gebruikers risico beleid is de voorkeurs methode voor het sluiten van actieve risico gebeurtenissen omdat deze methode is geautomatiseerd. Er is geen interactie vereist tussen de betrokken gebruiker en Help Desk of beheerder.

Het gebruik van een gebruikers risico beleid is echter niet altijd van toepassing. Dit is bijvoorbeeld van toepassing op:

- Gebruikers die niet zijn geregistreerd voor multi-factor Authentication (MFA).
- Gebruikers met actieve risico gebeurtenissen die zijn verwijderd.
- Een onderzoek dat laat zien dat een gerapporteerde risico gebeurtenis is uitgevoerd door de legitieme gebruiker.

## <a name="manual-password-reset"></a>Hand matig opnieuw instellen van wacht woorden

Als het vereisen van een wacht woord opnieuw instellen met behulp van een gebruikers risico beleid geen optie is, kunt u alle risico gebeurtenissen voor een gebruiker sluiten met hand matig opnieuw instellen van het wacht woord.

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/04.png)

Het gerelateerde dialoog venster biedt twee verschillende methoden om een wacht woord opnieuw in te stellen:

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/05.png)

**Een tijdelijk wacht woord genereren** : door een tijdelijk wacht woord te genereren, kunt u direct een identiteit weer in een veilige staat zetten. Deze methode vereist interactie met de betrokken gebruikers omdat ze moeten weten wat het tijdelijke wacht woord is. U kunt bijvoorbeeld het nieuwe tijdelijke wacht woord verzenden naar een alternatief e-mail adres voor de gebruiker of naar de Manager van de gebruiker. Omdat het wacht woord tijdelijk is, wordt de gebruiker gevraagd het wacht woord te wijzigen tijdens de volgende aanmelding.

**De gebruiker verplichten om wacht woord opnieuw in te stellen** : als u wilt dat gebruikers hun wacht woorden opnieuw moeten instellen, kunnen ze zelf herstel zonder contact opnemen met de Help Desk of een beheerder. Net als in het geval van een beleid voor gebruikers Risico's is deze methode alleen van toepassing op gebruikers die zijn geregistreerd voor MFA. Voor gebruikers die nog niet zijn geregistreerd voor MFA, is deze optie niet beschikbaar.

## <a name="dismiss-all-risk-events"></a>Alle risico gebeurtenissen verwijderen

Als het opnieuw instellen van een wacht woord geen optie is, kunt u ook alle risico gebeurtenissen negeren. 

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/03.png)

Wanneer u op **alle gebeurtenissen negeren**klikt, worden alle gebeurtenissen gesloten en loopt de betrokken gebruiker geen risico meer. Omdat deze methode echter geen invloed heeft op het bestaande wacht woord, wordt de bijbehorende identiteit niet opnieuw in een veilige staat gebracht. De voorkeurs use-case voor deze methode is een verwijderde gebruiker met actieve risico gebeurtenissen. 

## <a name="close-individual-risk-events-manually"></a>Afzonderlijke risico gebeurtenissen hand matig sluiten

U kunt afzonderlijke risico gebeurtenissen hand matig sluiten. Door risico gebeurtenissen hand matig te sluiten, kunt u het risico niveau van de gebruiker verlagen. Normaal gesp roken worden risico gebeurtenissen hand matig worden gesloten in reactie op een gerelateerd onderzoek. Als er bijvoorbeeld een gesprek met een gebruiker laat zien dat een actieve risico gebeurtenis niet meer nodig is. 
 
Wanneer risico gebeurtenissen hand matig worden gesloten, kunt u een van de volgende acties uitvoeren om de status van een risico gebeurtenis te wijzigen:

![Acties](./media/howto-close-active-risk-events/06.png)

- **Oplossen** - als u na onderzoek van een risicogebeurtenis een juiste herstelactie buiten Identity Protection hebt ondernomen en van mening bent dat de risicogebeurtenis kan worden gesloten, markeert u de gebeurtenis als Opgelost. Een opgeloste risicogebeurtenis krijgt de status Gesloten. Bovendien draagt de risicogebeurtenis niet meer bij aan het gebruikersrisico.
- **Markeren als onjuist-positief** : in sommige gevallen kunt u een risico gebeurtenis onderzoeken en detecteren dat deze onjuist is gemarkeerd als riskant. U kunt het aantal dergelijke gevallen verminderen door de risico gebeurtenis te markeren als onwaar-positief. Dit helpt de algoritmen van machine learning om de classificatie van soort gelijke gebeurtenissen in de toekomst te verbeteren. De status van valse gebeurtenissen is om te worden gesloten en er worden geen bijdragen meer aan gebruikers Risico's.
- **Negeren** :  als u geen herstelactie heb uitgevoerd, maar wilt dat de risicogebeurtenis wordt verwijderd uit de actieve lijst, kunt u een risicogebeurtenis markeren als Negeren. De status van de gebeurtenis wordt dan Gesloten. Genegeerde gebeurtenissen dragen niet bij aan het gebruikersrisico. Deze optie dient alleen in ongebruikelijke omstandigheden te worden gebruikt.
- **Opnieuw activeren** : risico gebeurtenissen die hand matig zijn gesloten (door omzetten, fout positief of negeren), kunnen opnieuw worden geactiveerd, waarbij de gebeurtenis status weer actief wordt. Opnieuw geactiveerde risicogebeurtenissen dragen bij aan de berekening van het gebruikersrisiconiveau. Risico's die gesloten zijn via herstelacties (zoals het opnieuw instellen van een beveiligd wachtwoord), kunnen niet opnieuw worden geactiveerd.

## <a name="next-steps"></a>Volgende stappen

Zie overzicht van de [Azure AD Identity Protection](overview.md)voor een overzicht van Azure AD Identity Protection.
