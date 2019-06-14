---
title: Over het onderzoeken van riskante gebruikers en aanmeldingen in Azure Active Directory identity protection (vernieuwd) | Microsoft Docs
description: Informatie over het onderzoeken van riskante gebruikers en aanmeldingen in Azure Active Directory identity protection (vernieuwd).
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.date: 01/25/2019
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 646119f223067fac5ee36574fb10273819251cce
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60295945"
---
# <a name="how-to-investigate-risky-users-and-sign-ins"></a>Procedure: Riskante gebruikers en aanmeldingen onderzoeken 


Met behulp van de rapporten over riskante aanmeldingen en riskante gebruikers kunt u onderzoek en krijg inzicht in de risico's in uw omgeving. Met de mogelijkheid om te filteren en sorteren van de riskante aanmeldingen en gebruikers, kunt u beter te begrijpen mogelijke indringers in uw organisatie. 


## <a name="risky-users-report"></a>Rapport riskante gebruikers

Met de informatie die wordt geleverd door het rapport riskante gebruikers, vindt u antwoorden op vragen zoals:

- Welke gebruikers zijn hoog risico?
- Welke gebruikers hebben de status van een risico van hersteld?



Uw eerste beginpunt voor dit rapport is de **onderzoeken** sectie op de pagina.

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/01.png)


Het rapport riskante gebruikers heeft een standaard-weergave waarin wordt weergegeven:

- Name

- Status gebruikersrisico

- Risiconiveau

- Details van de risico 's

- Laatst bijgewerkt risico

- Type

- Status
 

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/03.png)


U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/04.png)

Het dialoogvenster kolommen kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.

Wanneer u op een item in de lijstweergave klikt, worden in een horizontale weergave alle beschikbare gegevens voor het item vermeld.

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/05.png)


De details weer wordt weergegeven:

- Basisgegevens

- Recente riskante aanmeldingen

- Risicogebeurtenissen niet is gekoppeld aan een aanmelding

- Geschiedenis van risico



U kunt ook het volgende doen:

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/08.png)

- Bekijk alle aanmeldingen snelkoppeling om het rapport aanmeldingen voor die gebruiker weer te geven.

- Alle riskante aanmeldingen bekijken als u wilt weergeven van alle aanmeldingen voor die gebruiker die zijn gemarkeerd als riskant.

- Wachtwoord van een gebruiker opnieuw instellen als u van mening bent dat de identiteit van de gebruiker is aangetast.

- Gebruikersrisico negeren als u denkt dat de actieve risicogebeurtenissen van een gebruiker fout-positieven. Zie voor meer informatie, [over het verbeteren van de nauwkeurigheid van de detectie](howto-improve-detection-accuracy.md).



### <a name="filter-risky-users"></a>Riskante gebruikers filteren

Als u wilt beperken de gerapporteerde gegevens naar een niveau die bij u past, kunt u de gegevens van de riskante gebruiker met behulp van de volgende standaardvelden filteren:

- Name

- Gebruikersnaam

- Status gebruikersrisico

- Risiconiveau

- Type

- Status

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/06.png)



De **naam** filter kunt u de naam of de user principal name (UPN) van de gewenste gebruiker opgeven.


De **risico status** filter kunt u selecteren:

- Risico lopen
- Hersteld
- Gesloten


De **risiconiveau** filter kunt u selecteren:

- Hoog
- Gemiddeld
- Laag


De **Type** filter kunt u selecteren:

- Gast
- Lid

De **Status** filter kunt u selecteren:

- Verwijderen
- Actief


### <a name="download-risky-users-data"></a>Riskante gebruikers gegevens downloaden

Als u wilt werken met deze buiten de Azure-portal, kunt u de gegevens riskante gebruikers downloaden. Downloaden te klikken op maakt een CSV-bestand van de meest recente 2500 records. 

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/07.png)


U kunt de lijstweergave aanpassen door kolommen in de werkbalk te klikken.
 
Hiermee kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.
 
Voor meer informatie over een riskante gebruiker, klikt u op in de sectie van de gegevens uit te vouwen

 



## <a name="risky-sign-ins-report"></a>Rapport Riskante aanmeldingen

Met de informatie die wordt geleverd door het rapport riskante aanmeldingen, vindt u antwoorden op vragen zoals:

- Het aantal geslaagde aanmeldingen zijn er die anonieme risicogebeurtenissen voor IP-adres heeft in de afgelopen week?

- Welke gebruikers zijn bevestigd dat het gedrang komen in de afgelopen maand?

- Welke gebruikers waren riskante aanmeldingen bij de Office 365-portal?




Uw eerste beginpunt voor dit rapport is de **onderzoeken** sectie op de pagina.

![Rapport Riskante aanmeldingen](./media/howto-investigate-risky-users-signins/02.png)

Het rapport riskante aanmeldingen heeft een standaard-weergave waarin wordt weergegeven:

- Date

- Gebruiker

- Toepassing

- Aanmeldingsstatus

- Status gebruikersrisico

- Risiconiveau (totaal)

- Risiconiveau (realtime)

- Voorwaardelijke toegang

- MFA vereist  
 

![Rapport Riskante aanmeldingen](./media/howto-investigate-risky-users-signins/09.png)


U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/11.png)

Het dialoogvenster kolommen kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.

Wanneer u op een item in de lijstweergave klikt, worden in een horizontale weergave alle beschikbare gegevens voor het item vermeld.

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/12.png)


De details weer wordt weergegeven:

- Basisgegevens

- Apparaatgegevens

- Informatie over de risico 's

- MFA-info

- Voorwaardelijke toegang





U kunt ook het volgende doen:

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/13.png)

- Controleer of aangetast 

- Veilige bevestigen

Zie voor meer informatie, [over het verbeteren van de nauwkeurigheid van de detectie](howto-improve-detection-accuracy.md).




### <a name="filter-risky-sign-ins"></a>Riskante aanmeldingen filteren

Als u wilt beperken de gerapporteerde gegevens naar een niveau die bij u past, kunt u de gegevens van de riskante gebruiker met behulp van de volgende standaardvelden filteren:

- Gebruiker
- Toepassing
- Aanmeldingsstatus
- Status gebruikersrisico
- Risiconiveau (totaal)
- Risiconiveau (realtime)
- Voorwaardelijke toegang
- Date
- Niveau risicotype

![Rapport Riskante aanmeldingen](./media/howto-investigate-risky-users-signins/14.png)



De **naam** filter kunt u de naam of de user principal name (UPN) van de gewenste gebruiker opgeven.

De **toepassing** filter kunt u opgeven van de cloud-app die de gebruiker toegang probeert te krijgen.

Met het filter **Aanmeldingsstatus** kunt u selecteren:

- Alle
- Geslaagd
- Fout


De **risico status** filter kunt u selecteren:

- Risico lopen
- Bevestigd aangetast
- Veilige bevestigd
- Gesloten
- Hersteld


De **risiconiveau (totaal)** filter kunt u selecteren:

- Hoog
- Gemiddeld
- Laag

De **(realtime) risiconiveau** filter kunt u selecteren:

- Hoog
- Gemiddeld
- Laag


De **voorwaardelijke toegang** filter kunt u selecteren:

- Alle
- Niet toegepast
- Geslaagd
- Fout


Met het filter **Datum** kunt u een tijdsbestek opgeven voor de geretourneerde gegevens.
Mogelijke waarden zijn:

- Afgelopen maand
- Afgelopen 7 dagen
- Afgelopen 24 uur
- Aangepast tijdsinterval





### <a name="download-risky-sign-ins-data"></a>Riskante aanmeldingen gegevens downloaden

Als u wilt werken met deze buiten de Azure-portal, kunt u de gegevens riskante aanmeldingen downloaden. Downloaden te klikken op maakt een CSV-bestand van de meest recente 2500 records. 

![Rapport riskante gebruikers](./media/howto-investigate-risky-users-signins/15.png)



## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview-v2.md).
