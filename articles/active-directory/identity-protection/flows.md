---
title: Aanmelden-ervaringen met Azure AD Identity Protection | Microsoft Docs
description: Biedt een overzicht van de gebruikerservaring wanneer Identity Protection is verholpen of hersteld van een gebruiker of als de multi-factor Authentication-verificatie is vereist voor een beleid.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5b330a8e0eebc2a6f3701534fbc25848296694b1
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54912626"
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Aanmelden-ervaringen met Azure AD Identity Protection
Met Azure Active Directory Identity Protection, kunt u het volgende doen:

* vereisen dat gebruikers zich registreren voor meervoudige verificatie
* verwerking van riskante aanmeldingen en gebruikers waarmee is geknoeid

De reactie van het systeem op deze problemen heeft een invloed op van een gebruiker aanmelden ervaring omdat rechtstreeks aanmelden door te geven van een gebruikersnaam en een wachtwoord zijn niet mogelijk niet meer. Er zijn extra stappen vereist om op te halen van een gebruiker veilig terug in het bedrijf.

In dit artikel biedt een overzicht van de aanmelding van een gebruiker voor alle aanvragen die kunnen optreden.

**Multi-factor authentication**

* Registratie voor meervoudige verificatie

**Aanmelden bij de risico 's**

* Riskante aanmelding herstel
* Riskante aanmelding geblokkeerd
* Registratie voor meervoudige verificatie tijdens een riskante aanmelding

**Gebruikers die risico lopen**

* Verdacht account herstellen
* Verdacht account geblokkeerd

## <a name="multi-factor-authentication-registration"></a>Registratie voor meervoudige verificatie
De beste gebruikerservaring voor zowel de stroom van de recovery attent op accounts en de stroom voor riskante aanmelding is wanneer de gebruiker zelf kunt herstellen. Als gebruikers zijn geregistreerd voor meervoudige verificatie, hebben ze al een telefoonnummer dat is gekoppeld aan hun account die kan worden gebruikt om door te geven, beveiligingsproblemen met zich mee. Er is geen help helpdesk of beheerder betrokkenheid is nodig om te zetten van inbreuk op account. Het is dus zeer raadzaam om uw gebruikers geregistreerd voor meervoudige verificatie. 

Beheerders kunnen een beleid waarbij gebruikers zich voor het instellen van hun account voor aanvullende beveiligingsverificatie instellen. Dit beleid kan gebruikers multi-factor authentication-registratie overslaan gedurende 14 dagen. De 14-daagse respijtperiode voorbij kan niet worden geconfigureerd.

**De multi-factor authentication-registratie heeft drie stappen:**

1. In de eerste stap van ontvangt de gebruiker een melding over de vereiste voor het account voor multi-factor authentication in te stellen. 
   
    ![Herstel](./media/flows/140.png "herstel")
2. Als u multi-factor authentication instelt, moet u het systeem weten hoe u wilt verbinding worden gemaakt met.
   
    ![Herstel](./media/flows/141.png "herstel")
3. Het systeem dient een uitdaging voor u en u moet reageren.
   
    ![Herstel](./media/flows/142.png "herstel")

## <a name="risky-sign-in-recovery"></a>Riskante aanmelding herstel
Wanneer een beheerder heeft een beleid voor aanmelden risico's worden geconfigureerd, worden de betrokken gebruikers worden gewaarschuwd wanneer ze proberen aan te melden. 

**De stroom voor riskante aanmelding bestaat uit twee stappen:** 

1. De gebruiker op de hoogte gesteld dat er is ongewone activiteit is gedetecteerd over het aanmelden, zoals ze zich aanmelden vanaf een nieuwe locatie, apparaat of app. 
   
    ![Herstel](./media/flows/120.png "herstel")
2. De gebruiker is vereist voor hun identiteit bewijzen met het oplossen van een beveiligingsvraag. Als de gebruiker is geregistreerd voor multi-factor authentication die ze nodig hebben om af te ronden veroorzaken een beveiligingscode te ontvangen via het telefoonnummer. Omdat dit slechts een riskante aanmelding en niet op een account waarmee is geknoeid is, wordt de gebruiker hoeft te wijzigen van het wachtwoord in deze stroom. 
   
    ![Herstel](./media/flows/121.png "herstel")

## <a name="risky-sign-in-blocked"></a>Riskante aanmelding geblokkeerd
Beheerders kunnen ook kiezen om in te stellen van een beleid voor Aanmeldingsrisico om te voorkomen dat gebruikers bij het aanmelden, afhankelijk van het risiconiveau. Als u niet-geblokkeerd, eindgebruikers moet contact opnemen met een beheerder of de helpdesk of ze kunnen proberen ze zich aanmelden vanaf een vertrouwde locatie of het apparaat. Zelf herstellen door het oplossen van meervoudige verificatie kan niet worden gebruikt in dit geval.

![Herstel](./media/flows/200.png "herstel")

## <a name="compromised-account-recovery"></a>Verdacht account herstellen
Wanneer een beleid voor gebruikersrisico's beveiliging is geconfigureerd, dat is opgegeven in het beleid risico's door gebruikers die voldoen aan de gebruiker (en daarom wordt aangenomen dat aangetast) moet gaan via de gebruikersstroom voor het herstel van inbreuk op voordat deze zich kan aanmelden. 

**De gebruikersstroom voor inbreuk recovery heeft drie stappen:**

1. De gebruiker op de hoogte gesteld dat hun accountbeveiliging loopt het risico vanwege verdachte activiteiten of referenties gelekte.
   
    ![Herstel](./media/flows/101.png "herstel")
2. De gebruiker is vereist voor hun identiteit bewijzen met het oplossen van een beveiligingsvraag. De gebruiker is geregistreerd voor meervoudige verificatie kunnen zelf herstellen worden aangetast. Ze moeten veroorzaken ronde een beveiligingscode te ontvangen via het telefoonnummer. 
   
   ![Herstel](./media/flows/110.png "herstel")
3. Ten slotte wordt de gebruiker gedwongen hun wachtwoord wijzigen, omdat iemand anders toegang tot hun account heeft gehad. 
   Schermopnamen van deze ervaring staan hieronder.
   
   ![Herstel](./media/flows/111.png "herstel")

## <a name="compromised-account-blocked"></a>Verdacht account geblokkeerd
Als u een gebruiker die is geblokkeerd door een gebruiker risico beveiligingsbeleid gedeblokkeerd, moet de gebruiker contact op met een beheerder of helpdesk. Zelf herstellen door het oplossen van meervoudige verificatie kan niet worden gebruikt in dit geval.

![Herstel](./media/flows/104.png "herstel")

## <a name="reset-password"></a>Wachtwoord opnieuw instellen
Als verdachte gebruikers hebben geen toegang tot het aanmelden, kan een beheerder een tijdelijk wachtwoord genereren voor hen. De gebruikers hebben hun wachtwoord wijzigen tijdens een volgende aanmelding.

![Herstel](./media/flows/160.png "herstel")

## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) 

