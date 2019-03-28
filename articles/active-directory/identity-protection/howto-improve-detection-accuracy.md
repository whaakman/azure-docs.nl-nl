---
title: Over het verbeteren van de nauwkeurigheid van de detectie in Azure Active Directory Identity Protection (vernieuwd) | Microsoft Docs
description: Klik hier voor meer informatie over het verbeteren van de nauwkeurigheid van de detectie in Azure Active Directory Identity Protection (vernieuwd).
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7724d69a9294b420ca061d5ad26ad64826372203
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517671"
---
# <a name="how-to-improve-the-detection-accuracy"></a>Procedure: De nauwkeurigheid van de detectie te verbeteren 

Identity Protection biedt mechanismen om feedback te geven aan Azure AD op risico's detecties in uw omgeving. Als u feedback wilt, kunt u de status van gedetecteerde riskante gebruiker of aanmelden gebeurtenis bevestigen. Gebruikers van Microsoft deze feedback naar actie ondernemen voor de huidige risico detecties en de nauwkeurigheid van toekomstige detecties worden verbeterd. 


## <a name="what-is-detection"></a>Wat is detectie?

Detectie is het proces van het identificeren van verdachte activiteiten in combinatie met uw gebruikersaccounts. De verdachte activiteiten kan worden gedetecteerd door Azure AD worden genoemd [risicogebeurtenis](../reports-monitoring/concept-risk-events.md). Het detectieproces is gebaseerd op adaptieve machine learning-algoritmes en -methodieken voor het detecteren van risicogebeurtenissen voor gebruikers.

De resultaten worden gebruikt om te bepalen of gebruikers en aanmeldingen risico lopen. 


## <a name="how-can-i-improve-the-detection-accuracy"></a>Hoe kan ik de nauwkeurigheid van de detectie verbeteren?

Omdat detectie een geautomatiseerd proces is is het mogelijk dat Azure AD-fout-positieven rapporten. U kunt de nauwkeurigheid van de detectie verbeteren door het geven van feedback aan Azure AD met betrekking tot de resultaten.

Er zijn drie manieren om de nauwkeurigheid van de detectie te: bevestig waarmee is geknoeid aanmelden, de bevestiging hebt gestuurd veilige aanmelding en gebruikersrisico negeren. U kunt dit doen vanaf de volgende rapporten:

- **Rapport riskante aanmeldingen -** In het rapport riskante aanmeldingen, kunt u controleren of aanmeldingen veilige of waarmee is geknoeid zijn

- **Rapport riskante gebruikers -** In het rapport riskante gebruikers, kunt u sluiten gebruikersrisico 

Uw feedback wordt verwerkt door Azure AD voor het verbeteren van de nauwkeurigheid van de resultaten. Normaal gesproken feedback u geven als onderdeel van een gebruikersrisico of aanmeldingsrisico onderzoek. Zie voor meer informatie, [onderzoeken riskante gebruikers en aanmeldingen](howto-investigate-risky-users-signins.md).


## <a name="confirm-compromised"></a>Inbraak bevestigen

Bevestiging van een gebeurtenis aanmelden als geknoeid geeft aan Azure AD dat de aanmelding is niet gemachtigd door de eigenaar van de identiteit. Wanneer u selecteert "bevestigen aangetast", wordt Azure AD

- Het gebruikersrisico van de betrokken gebruiker in hoog verhogen.

- Helpen met het optimaliseren van de machine learning die risicogebeurtenissen gedetecteerd
 
- Aanvullende maatregelen bescherming van uw organisatie uitvoeren



Bevestig als een verdachte aanmelding:

- **Het rapport riskante aanmeldingen** -deze optie kunt u een verdachte aanmelding voor een of meer Aanmeldingsgebeurtenissen controleren.

    ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/07.png)

- **De detailweergave van het rapport riskante aanmeldingen** -deze optie kunt u ter bevestiging een verdacht account voor de geselecteerde aanmelding gebeurtenis in het rapport riskante aanmeldingen. 

    ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/04.png)


 
## <a name="confirm-safe"></a>Veiligheid bevestigen


Bevestiging van een gebeurtenis aanmelden als veilig signalen en Azure AD dat de aanmelding **is** geautoriseerd door de eigenaar van de desbetreffende identiteit. Wanneer u de "bevestigen veilige" selecteert, wordt Azure AD:

- Herstellen van de bijdrage van de gebruiker risico's van de geselecteerde aanmeldingen

- Sluit de onderliggende risicogebeurtenissen

- Helpen met het optimaliseren van de machine learning die risicogebeurtenissen gedetecteerd

- Aanvullende maatregelen bescherming van uw organisatie uitvoeren
 

Bevestig als een veilige aanmelding in:

- **Het rapport riskante aanmeldingen** -deze optie kunt u een veilige aanmelding voor een of meer Aanmeldingsgebeurtenissen controleren.

    ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/08.png)

- **De detailweergave van het rapport riskante aanmeldingen** -deze optie kunt u ter bevestiging een veilige aanmelding voor de geselecteerde aanmelding gebeurtenis in het rapport riskante aanmeldingen. 

    ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/05.png)




## <a name="dismiss-user-risk"></a>Gebruikersrisico negeren

Als u herstelacties voor een gebruiker risico al hebt gemaakt, of u denkt dat ze ten onrechte zijn gemarkeerd als riskant dat, kunt u risico's van een gebruiker sluiten. Negeren van risico's van een gebruiker, wordt de gebruiker hersteld naar een niet-riskante staat. Alle eerdere riskante aanmeldingen en risico's worden gebeurtenissen voor de geselecteerde gebruiker worden gesloten.


U kunt het gebruikersrisico van gerapporteerde sluiten:

- **Het rapport riskante gebruikers** : deze optie kunt u voor het verwijderen van het gebruikersrisico voor een of meer geselecteerde gebruikers.

    ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/02.png)

- **De weergave Details** -deze optie kunt u voor het verwijderen van het gebruikersrisico voor de geselecteerde gebruiker in de gebruikersrapport van de risico's. 

    ![Gebruikersrisico negeren](./media/howto-improve-detection-accuracy/01.png)


**Wat u moet weten:**

- U kunt deze actie niet ongedaan maken.

- Het kan enkele minuten voor deze actie worden uitgevoerd, daarom moet u niet opnieuw een aanvraag verzendt duren.

- U kunt deze actie alleen uitvoeren als de referenties van de gebruiker worden beheerd in AD. 



## <a name="best-practices"></a>Aanbevolen procedures

Verwijderen van een gebruiker risico is een manier om de blokkering op te heffen als ze zijn geblokkeerd door het beleid voor gebruikersrisico's en kan niet zelf herstellen vanwege hoeven niet opnieuw instellen van wachtwoorden en/of MFA is ingeschakeld. In dit geval is het is raadzaam om te controleren of de gebruiker vervolgens -registers voor wachtwoord opnieuw instellen en MFA zodat ze kunnen zelf herstellen eventuele toekomstige risicogebeurtenissen zijn.


## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview-v2.md).


