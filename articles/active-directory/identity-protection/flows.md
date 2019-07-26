---
title: Aanmeld ervaring met Azure AD Identity Protection | Microsoft Docs
description: Hiermee wordt een overzicht gegeven van de gebruikers ervaring wanneer de identiteits beveiliging een gebruiker heeft verminderd of heeft opgelost of wanneer multi-factor Authentication is vereist door een beleid.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e513027eed44ec7649f41f8786882aed8511bc6
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335489"
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Aanmeld ervaring met Azure AD Identity Protection

Met Azure Active Directory Identity Protection kunt u het volgende doen:

* vereisen dat gebruikers zich registreren voor multi-factor Authentication
* Risk ante aanmeldingen en gemanipuleerde gebruikers verwerken

De reactie van het systeem op deze problemen heeft gevolgen voor de aanmeldings ervaring van een gebruiker, omdat het rechtstreeks aanmelden door een gebruikers naam en een wacht woord op te geven niet meer mogelijk is. Er zijn extra stappen vereist om een gebruiker veilig weer in bedrijf te krijgen.

In dit artikel vindt u een overzicht van de aanmeldings ervaring van een gebruiker voor alle gevallen die zich kunnen voordoen.

**Multi-factor authentication**

* Multi-factor Authentication-registratie

**Meld u aan als risico**

* Risk ante aanmeld herstel
* Risk ante aanmelding geblokkeerd
* Multi-factor Authentication-registratie tijdens een Risk ante aanmelding

**Gebruiker met risico**

* Beschadigd account herstel
* Het account is geblokkeerd

## <a name="multi-factor-authentication-registration"></a>Multi-factor Authentication-registratie
De beste gebruikers ervaring voor zowel de beschadigde account herstel stroom als de Risk ante aanmeldings stroom is wanneer de gebruiker zelf herstel kan doen. Als gebruikers zijn geregistreerd voor multi-factor Authentication, hebben ze al een telefoon nummer dat is gekoppeld aan het account dat kan worden gebruikt voor het door geven van beveiligings problemen. Er is geen rol voor Help Desk of beheerder nodig voor het herstellen van inbreuk op het account. Daarom wordt het ten zeerste aanbevolen om uw gebruikers te laten registreren voor multi-factor Authentication. 

Beheerders kunnen een beleid instellen dat gebruikers nodig hebben om hun accounts in te stellen voor aanvullende beveiligings verificatie. Met dit beleid kunnen gebruikers multi-factor Authentication-registratie tot wel veer tien dagen overs Laan. De respijt periode van 14 dagen kan niet worden geconfigureerd.

**De multi-factor Authentication-registratie bestaat uit drie stappen:**

1. In de eerste stap krijgt de gebruiker een melding over de vereiste om het account in te stellen voor multi-factor Authentication. 
   
    ![Herstel](./media/flows/140.png "Herstel")
2. Als u multi-factor Authentication wilt instellen, moet u het systeem laten weten hoe u contact met u wilt opnemen.
   
    ![Herstel](./media/flows/141.png "Herstel")
3. Het systeem verzendt een uitdaging naar u en u moet reageren.
   
    ![Herstel](./media/flows/142.png "Herstel")

## <a name="risky-sign-in-recovery"></a>Risk ante aanmeld herstel
Wanneer een beheerder een beleid heeft geconfigureerd voor aanmeldings Risico's, worden de betrokken gebruikers hiervan op de hoogte gesteld wanneer ze zich proberen aan te melden. 

**De Risk ante aanmeldings stroom bestaat uit twee stappen:** 

1. De gebruiker wordt op de hoogte gesteld dat er iets ongebruikelijk is gedetecteerd over hun aanmelding, zoals het aanmelden vanaf een nieuwe locatie, apparaat of app. 
   
    ![Herstel](./media/flows/120.png "Herstel")
2. De gebruiker is verplicht hun identiteit te bewijzen door een beveiligings vraag op te lossen. Als de gebruiker is geregistreerd voor multi-factor Authentication, moeten ze een beveiligings code naar hun telefoon nummer afronden. Aangezien dit slechts een Risk ante aanmelding is en geen gemanipuleerd account is, hoeft de gebruiker het wacht woord in deze stroom niet te wijzigen. 
   
    ![Herstel](./media/flows/121.png "Herstel")

## <a name="risky-sign-in-blocked"></a>Risk ante aanmelding geblokkeerd
Beheerders kunnen er ook voor kiezen om een beleid voor aanmeldings Risico's in te stellen om gebruikers bij het aanmelden te blok keren, afhankelijk van het risico niveau. Als u de blok kering wilt opheffen, moeten eind gebruikers contact opnemen met een beheerder of een Help Desk of kunnen ze zich aanmelden vanaf een bekende locatie of apparaat. Het automatisch herstellen van het oplossen van multi-factor Authentication is in dit geval geen optie.

![Herstel](./media/flows/200.png "Herstel")

## <a name="compromised-account-recovery"></a>Beschadigd account herstel
Wanneer er een beveiligings beleid voor gebruikers Risico's is geconfigureerd, moeten gebruikers die voldoen aan het gebruikers risico niveau dat is opgegeven in het beleid (en daarom wordt aangenomen dat er is geknoeid) door de gebruiker de herstel stroom voor het geval van inbreuk door lopen voordat ze zich kunnen aanmelden. 

**De herstel stroom voor gebruikers met een inbreuk heeft drie stappen:**

1. De gebruiker wordt op de hoogte gesteld dat hun account beveiliging risico loopt vanwege verdachte activiteiten of gelekte referenties.
   
    ![Herstel](./media/flows/101.png "Herstel")
2. De gebruiker is verplicht hun identiteit te bewijzen door een beveiligings vraag op te lossen. Als de gebruiker is geregistreerd voor multi-factor Authentication, kan deze zichzelf herstellen als deze niet kan worden hersteld. Ze moeten een beveiligings code naar hun telefoon nummer afronden. 
   
   ![Herstel](./media/flows/110.png "Herstel")
3. Ten slotte wordt de gebruiker gedwongen het wacht woord te wijzigen, omdat iemand anders mogelijk toegang heeft gehad tot hun account. 
   Scherm afbeeldingen van deze ervaring vindt u hieronder.
   
   ![Herstel](./media/flows/111.png "Herstel")

## <a name="compromised-account-blocked"></a>Het account is geblokkeerd
Als u een gebruiker wilt ophalen die is geblokkeerd door een beveiligings beleid voor gebruikers risico is gedeblokkeerd, moet de gebruiker contact opnemen met een beheerder of de Help Desk. Het automatisch herstellen van het oplossen van multi-factor Authentication is in dit geval geen optie.

![Herstel](./media/flows/104.png "Herstel")

## <a name="reset-password"></a>Wachtwoord opnieuw instellen
Als gebruikers die zijn aangetast niet worden aangemeld, kan een beheerder een tijdelijk wacht woord voor hen maken. De gebruikers moeten hun wacht woord wijzigen tijdens de volgende aanmelding.

![Herstel](./media/flows/160.png "Herstel")

## <a name="see-also"></a>Zie ook

* [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) 
