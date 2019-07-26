---
title: De nauw keurigheid van de detectie in Azure Active Directory Identity Protection verbeteren (vernieuwd) | Microsoft Docs
description: De nauw keurigheid van de detectie in Azure Active Directory Identity Protection verbeteren (vernieuwd).
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32bb8de7970fc167a6a95e9d9c3c71e4e1dc0150
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333950"
---
# <a name="how-to-improve-the-detection-accuracy"></a>Procedure: De nauw keurigheid van de detectie verbeteren 

Identiteits beveiliging biedt mechanismen om feedback te geven aan Azure AD op de risico detecties in uw omgeving. Als u feedback wilt geven, kunt u de status van de gedetecteerde Risk ante gebruiker of aanmeldings gebeurtenis bevestigen. Micro soft gebruikers deze feedback om actie te ondernemen op huidige risico detecties en de nauw keurigheid van toekomstige detecties te verbeteren. 

## <a name="what-is-detection"></a>Wat is detectie?

Detectie is het proces van het identificeren van verdachte activiteiten in combi natie met uw gebruikers accounts. De verdachte activiteiten die Azure AD kan detecteren, worden [risico gebeurtenissen](../reports-monitoring/concept-risk-events.md)genoemd. Het detectie proces is gebaseerd op adaptieve machine learning algoritmen en heuristieks om risico gebeurtenissen voor gebruikers te detecteren.

De detectie resultaten worden gebruikt om te bepalen of gebruikers en aanmeldingen risico lopen. 

## <a name="how-can-i-improve-the-detection-accuracy"></a>Hoe kan ik de nauw keurigheid van de detectie verbeteren?

Omdat detectie een geautomatiseerd proces is, is het mogelijk dat Azure AD meldt dat er valse positieven zijn. U kunt de nauw keurigheid van de detectie verbeteren door Azure AD feedback te geven over de detectie resultaten.

Er zijn drie manieren om de nauw keurigheid van de detectie te verbeteren: Bevestig het aanmelden, bevestig het aanmelden en sluit het risico van de gebruiker. U kunt dit doen in de volgende rapporten:

- **Rapport Risk ante aanmeldingen:** In het rapport Risk ante aanmeldingen kunt u bevestigen dat aanmeldingen veilig of aangetast zijn
- **Rapport Risk ante gebruikers-** In het rapport Risk ante gebruikers kunt u het gebruikers risico sluiten 

Uw feedback wordt verwerkt door Azure AD om de nauw keurigheid van de detectie resultaten te verbeteren. Normaal gesp roken geeft u feedback als onderdeel van een gebruikers risico of onderzoek naar een aanmeldings risico. Zie voor meer informatie [Risk ante gebruikers en aanmeldingen onderzoeken](howto-investigate-risky-users-signins.md).

## <a name="confirm-compromised"></a>Inbreuk bevestigen

Het bevestigen van een aanmeldings gebeurtenis als gesignaleerde signalen naar Azure AD dat de aanmelding niet is geautoriseerd door de eigenaar van de identiteit. Wanneer u ' aangetast bevestigen ' selecteert, wordt door Azure AD

- Verhoog het gebruikers risico van de betrokken gebruiker tot hoog.
- Help voor het optimaliseren van de machine learning die risico gebeurtenissen detecteert
- Extra maat regelen nemen om uw organisatie verder te beveiligen

Een aangetaste aanmelding bevestigen:

- **Het rapport Risk ante** aanmeldingen: met deze optie kunt u een aangetaste aanmelding voor een of meer aanmeldings gebeurtenissen bevestigen.

   ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/07.png)

- **De detail weergave van het rapport Risk ante** aanmeldingen: met deze optie kunt u een aangetast account voor de geselecteerde aanmeldings gebeurtenis bevestigen in het rapport Risk ante aanmeldingen. 

   ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/04.png)
 
## <a name="confirm-safe"></a>Veiligheid bevestigen

Het bevestigen van een aanmeldings gebeurtenis als veilige signalen naar Azure AD dat de aanmelding **is** geautoriseerd door de respectieve identiteits eigenaar. Wanneer u "veilig bevestigen" selecteert, zal Azure AD:

- De risico bijdrage van de geselecteerde aanmeldingen van de gebruiker herstellen
- De onderliggende risico gebeurtenissen sluiten
- Help voor het optimaliseren van de machine learning die risico gebeurtenissen detecteert
- Extra maat regelen nemen om uw organisatie verder te beveiligen
 
Een kluis aanmelding bevestigen in:

- **Het rapport Risk ante** aanmeldingen: met deze optie kunt u een veilige aanmelding voor een of meer aanmeldings gebeurtenissen bevestigen.

   ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/08.png)

- **De detail weergave van het rapport Risk ante** aanmeldingen: met deze optie kunt u een kluis aanmelding voor de geselecteerde aanmeldings gebeurtenis bevestigen in het rapport Risk ante aanmeldingen. 

   ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/05.png)

## <a name="dismiss-user-risk"></a>Gebruikersrisico negeren

Als u al herstel acties hebt uitgevoerd voor een risico gebruiker of denkt dat ze onterecht zijn gemarkeerd als riskant, kunt u het risico van een gebruiker negeren. Als u het risico van een gebruiker afmist, wordt de gebruiker teruggezet naar een niet-Risk ante status. Alle afgelopen Risk ante aanmeldingen en risico gebeurtenissen voor de geselecteerde gebruiker worden genegeerd.

U kunt het gemelde risico van de gebruiker in:

- **Het rapport Risk ante gebruikers** : met deze optie kunt u het gebruikers risico voor een of meer geselecteerde gebruikers negeren.

   ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/02.png)

- **De weer gave Details** : met deze optie kunt u het gebruikers risico voor de geselecteerde gebruiker in het gebruikers risico rapport negeren. 

   ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/01.png)

**Wat u moet weten:**

- U kunt deze actie niet ongedaan maken.
- Het kan enkele minuten duren voordat deze actie is voltooid. Daarom moet u de aanvraag niet opnieuw verzenden.
- U kunt deze actie alleen uitvoeren als AD de referenties van de gebruiker beheert. 

## <a name="best-practices"></a>Aanbevolen procedures

Als u het risico van een gebruiker uitschakelt, is het een goed idee om ze te deblokkeren als ze zijn geblokkeerd door het beleid voor gebruikers Risico's en niet zelf kunnen worden hersteld omdat het opnieuw instellen van wacht woorden en/of MFA is ingeschakeld. In dit geval is het raadzaam om ervoor te zorgen dat de gebruiker vervolgens registreert voor wacht woord opnieuw instellen en MFA zodat ze zelf risico gebeurtenissen zelf kunnen herstellen.

## <a name="next-steps"></a>Volgende stappen

Zie overzicht van de [Azure AD Identity Protection](overview-v2.md)voor een overzicht van Azure AD Identity Protection.
