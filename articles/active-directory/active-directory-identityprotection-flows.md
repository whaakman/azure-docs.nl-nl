---
title: Aanmelden-ervaringen met Azure AD Identity Protection | Microsoft Docs
description: Biedt een overzicht van de gebruikerservaring als Identity Protection is verholpen of hersteld van een gebruiker of als de multi-factor authentication-server is vereist voor een beleid.
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 558d1e04e352d5f33201bb15cb7a2dd1c18f524f
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Aanmelden-ervaring met Azure AD Identity Protection
U kunt met Azure Active Directory: Identity Protection:

* vereisen dat gebruikers zich registreren voor meervoudige verificatie
* riskant aanmeldingen en verdachte gebruikers verwerken

Het antwoord van het systeem op deze problemen invloed is op een gebruiker-aanmeldingservaring aanpast omdat alleen rechtstreeks aanmelden door op te geven van een gebruikersnaam en een wachtwoord zijn niet mogelijk meer. Er zijn extra stappen nodig om op te halen van een gebruiker veilig weer in bedrijf.

In dit onderwerp biedt een overzicht van de aanmeldingservaring van een gebruiker in alle gevallen die zich kunnen voordoen.

**Multi-factor authentication**

* Registratie voor meervoudige verificatie

**Meld u aan risico**

* Herstel voor riskante aanmelden
* Riskant aanmelden geblokkeerd
* Registratie van de multi-factor authentication-server tijdens een riskante aanmelden

**Gebruiker risico**

* Verdacht accountherstel
* Verdacht account geblokkeerd

## <a name="multi-factor-authentication-registration"></a>Registratie voor meervoudige verificatie
De beste gebruikerservaring voor zowel de verdacht account recovery stroom en de stroom voor riskante aanmelden is wanneer de gebruiker zelf kunt herstellen. Als gebruikers zijn geregistreerd voor multi-factor authentication, hebben ze al een telefoonnummer dat is gekoppeld aan hun account die kan worden gebruikt om door te geven van beveiligingsproblemen met zich mee. Geen help helpdesk of beheerder betrokkenheid is nodig voor het herstellen van inbreuk op het account. Dus is het raadzaam om op te halen van de gebruikers die zijn geregistreerd voor multi-factor authentication. 

Beheerders kunnen:

* een beleid instellen dat gebruikers moeten hun accounts instellen voor aanvullende beveiligingsverificatie. 
* bieden registratie voor meervoudige verificatie overslaan voor maximaal 30 dagen, als deze gebruikers geven een respijtperiode wilt voordat u registreert.

**De registratie van de multi-factor authentication-server heeft drie stappen:**

1. In de eerste stap van ontvangt de gebruiker een melding over de vereiste van het account voor multi-factor authentication instellen. 
   
    ![Herstel](./media/active-directory-identityprotection-flows/140.png "herstel")
2. Multi-factor authentication-server als u wilt instellen, moet u laten weten hoe u wilt verbinding worden gemaakt.
   
    ![Herstel](./media/active-directory-identityprotection-flows/141.png "herstel")
3. Het systeem verzendt een uitdaging voor u en u moet reageren.
   
    ![Herstel](./media/active-directory-identityprotection-flows/142.png "herstel")

## <a name="risky-sign-in-recovery"></a>Herstel voor riskante aanmelden
Wanneer een beheerder heeft een beleid voor aanmelden risico's geconfigureerd, wordt de betreffende gebruikers worden gewaarschuwd wanneer ze proberen om aan te melden. 

**De stroom voor riskante aanmelden bestaat uit twee stappen:** 

1. De gebruiker wordt geïnformeerd dat ongewone activiteit over hun aanmelden gevonden is, zoals het aanmelden vanaf een nieuwe locatie, apparaat of app. 
   
    ![Herstel](./media/active-directory-identityprotection-flows/120.png "herstel")
2. De gebruiker is vereist voor hun identiteit bewijzen door een beveiligingsvraag op te lossen. Als de gebruiker is geregistreerd voor multi-factor authentication moeten ze interactie een beveiligingscode naar hun telefoonnummer. Aangezien dit een zojuist een riskante aanmelden en geen account waarmee is geknoeid, wordt de gebruiker hoeft te wijzigen van het wachtwoord in deze stroom. 
   
    ![Herstel](./media/active-directory-identityprotection-flows/121.png "herstel")

## <a name="risky-sign-in-blocked"></a>Riskant aanmelden geblokkeerd
Beheerders kunnen ook kiezen een beleid voor aanmelden risico voorkomen dat gebruikers bij het aanmelden, afhankelijk van het risiconiveau instellen. Als u gedeblokkeerd, eindgebruikers moet contact opnemen met een netwerkbeheerder of de helpdesk of proberen ze aanmelden vanaf een vertrouwde locatie of het apparaat. Automatisch herstellen door het oplossen van multi-factor authentication-server kan niet worden gebruikt in dit geval.

![Herstel](./media/active-directory-identityprotection-flows/200.png "herstel")

## <a name="compromised-account-recovery"></a>Verdacht accountherstel
Wanneer een gebruiker risico-beveiligingsbeleid is geconfigureerd, gebruikers die voldoen aan de gebruiker risiconiveau opgegeven in het beleid (en zijn daarom aangenomen geknoeid) door de gebruiker inbreuk herstel stroom moet gaan voordat ze kunnen aanmelden. 

**De gebruiker inbreuk herstel-stroom heeft drie stappen:**

1. De gebruiker wordt geïnformeerd dat de accountbeveiliging van hun risico vanwege verdachte activiteiten loopt of referenties gelekte.
   
    ![Herstel](./media/active-directory-identityprotection-flows/101.png "herstel")
2. De gebruiker is vereist voor hun identiteit bewijzen door een beveiligingsvraag op te lossen. Als de gebruiker is geregistreerd voor multi-factor authentication kunnen ze zelf herstellen van wordt aangetast. Ze moet interactie een beveiligingscode naar hun telefoonnummer. 
   
   ![Herstel](./media/active-directory-identityprotection-flows/110.png "herstel")
3. Ten slotte wordt de gebruiker gedwongen hun wachtwoord wijzigen omdat iemand anders mogelijk toegang heeft gehad tot hun account. 
   Schermopnamen van deze ervaring zijn hieronder.
   
   ![Herstel](./media/active-directory-identityprotection-flows/111.png "herstel")

## <a name="compromised-account-blocked"></a>Verdacht account geblokkeerd
Als u een gebruiker die is geblokkeerd door een gebruiker risico beveiligingsbeleid gedeblokkeerd, moet de gebruiker contact op met een beheerder of helpdesk. Automatisch herstellen door het oplossen van multi-factor authentication-server kan niet worden gebruikt in dit geval.

![Herstel](./media/active-directory-identityprotection-flows/104.png "herstel")

## <a name="reset-password"></a>Wachtwoord opnieuw instellen
Als verdachte gebruikers hebben geen toegang tot het aanmelden, kan een beheerder een tijdelijk wachtwoord genereren voor deze. De gebruikers hebben hun wachtwoord wijzigen tijdens een volgende keer aanmelden.

![Herstel](./media/active-directory-identityprotection-flows/160.png "herstel")

## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md) 

