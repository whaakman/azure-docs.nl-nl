---
title: Risk ante gebruikers en aanmeldingen in Azure Active Directory identiteits beveiliging (vernieuwd) onderzoeken | Microsoft Docs
description: Meer informatie over het onderzoeken van Risk ante gebruikers en aanmeldingen in Azure Active Directory identiteits beveiliging (vernieuwd).
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: c89658e962654f005eaee5ceff220d5fb343e86e
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370321"
---
# <a name="how-to-investigate-risky-users-and-sign-ins"></a>Procedure: Riskante gebruikers en aanmeldingen onderzoeken 

Door gebruik te maken van rapporten met Risk ante aanmeldingen en Risk ante gebruikers kunt u het risico van uw omgeving onderzoeken en er inzicht in krijgen. Met de mogelijkheid om de Risk ante aanmeldingen en gebruikers te filteren en te sorteren, kunt u beter inzicht krijgen in mogelijke inbreuken in uw organisatie. 

## <a name="risky-users-report"></a>Rapport over riskante gebruikers

Met de informatie van het rapport Risk ante gebruikers kunt u antwoorden vinden op vragen zoals:

- Welke gebruikers zijn een hoog risico?
- Welke gebruikers hebben een risico status opgelost?

Uw eerste ingangs punt voor dit rapport is de sectie **onderzoek** op de pagina beveiliging.

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/01.png)

Het rapport Risk ante gebruikers heeft een standaard weergave waarin het volgende wordt weer gegeven:

- Name
- Status van risico
- Risiconiveau
- Details van risico
- Risico voor het laatst bijgewerkt
- type
- Status

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/03.png)

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/04.png)

In het dialoog venster kolommen kunt u extra velden weer geven of velden verwijderen die al worden weer gegeven.

Wanneer u op een item in de lijstweergave klikt, worden in een horizontale weergave alle beschikbare gegevens voor het item vermeld.

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/05.png)

In de detail weergave ziet u het volgende:

- Basisgegevens
- Recente riskante aanmeldingen
- Riskante gebeurtenissen die niet zijn gekoppeld aan een aanmelding
- Risicogeschiedenis

Daarnaast kunt u het volgende doen:

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/08.png)

- De snelkoppeling alle aanmeldingen weer geven om het rapport met aanmeldingen voor die gebruiker weer te geven.
- Bekijk alle Risk ante aanmeldingen om alle aanmeldingen voor die gebruiker weer te geven die als riskant zijn gemarkeerd.
- Stel het wacht woord van een gebruiker opnieuw in als u denkt dat de identiteit van de gebruiker is aangetast.
- Sluit gebruikers risico af als u denkt dat de actieve risico gebeurtenissen van een gebruiker vals-positief zijn. Zie het artikel [feedback geven over risico gebeurtenissen in azure AD Identity Protection](howto-provide-risk-event-feedback.md)voor meer informatie.

### <a name="filter-risky-users"></a>Risk ante gebruikers filteren

Als u de gerapporteerde gegevens wilt beperken tot een niveau dat geschikt is voor u, kunt u de Risk ante gebruikers gegevens filteren met behulp van de volgende standaard velden:

- Name
- Gebruikersnaam
- Status van risico
- Risiconiveau
- type
- Status

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/06.png)

Met het filter **naam** kunt u de naam of de User Principal Name (UPN) opgeven van de gebruiker die u bezorgt.

Met het filter **risico status** kunt u het volgende selecteren:

- Loopt risico
- Hersteld
- Verwijderd

Met het filter **risico niveau** kunt u het volgende selecteren:

- Hoog
- Gemiddeld
- Laag

Met het filter **type** kunt u het volgende selecteren:

- Gast
- Lid

Met het **status** filter kunt u het volgende selecteren:

- Verwijderd
- Actief

### <a name="download-risky-users-data"></a>Risk ante gebruikers gegevens downloaden

U kunt de Risk ante gebruikers gegevens downloaden als u deze buiten het Azure Portal wilt gebruiken. Als u op downloaden klikt, wordt er een CSV-bestand van de meest recente 2.500 records gemaakt. 

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/07.png)

U kunt de lijst weergave aanpassen door te klikken op de kolommen in de werk balk.
 
Hiermee kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.
 
Als u meer wilt weten over een Risk ante gebruiker, klikt u op de gegevens lade om deze uit te vouwen

## <a name="risky-sign-ins-report"></a>Rapport Riskante aanmeldingen

Met de informatie van het rapport Risk ante aanmeldingen kunt u antwoorden vinden op vragen zoals:

- Hoeveel geslaagde aanmeldingen heeft de afgelopen week anonieme IP-adres risico gebeurtenissen gehad?
- Welke gebruikers zijn in de afgelopen maand in het gedrang zijn bevestigd?
- Welke gebruikers hebben Risk ante aanmeldingen bij de Office 365-Portal?

Uw eerste ingangs punt voor dit rapport is de sectie **onderzoek** op de pagina beveiliging.

![Rapport Riskante aanmeldingen](./media/howto-investigate-risky-users-signins/02.png)

Het rapport Risk ante aanmeldingen heeft een standaard weergave waarin het volgende wordt weer gegeven:

- Date
- Gebruiker
- Toepassing
- Aanmeldingsstatus
- Status van risico
- Risico niveau (aggregatie)
- Risico niveau (realtime)
- Voorwaardelijke toegang
- MFA vereist  

![Rapport Riskante aanmeldingen](./media/howto-investigate-risky-users-signins/09.png)

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/11.png)

In het dialoog venster kolommen kunt u extra velden weer geven of velden verwijderen die al worden weer gegeven.

Wanneer u op een item in de lijstweergave klikt, worden in een horizontale weergave alle beschikbare gegevens voor het item vermeld.

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/12.png)

In de detail weergave ziet u het volgende:

- Basisgegevens
- Apparaatgegevens
- Risicogegevens
- MFA-info
- Voorwaardelijke toegang

Daarnaast kunt u het volgende doen:

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/13.png)

- Inbreuk bevestigen 
- Veiligheid bevestigen

Zie het artikel [feedback geven over risico gebeurtenissen in azure AD Identity Protection](howto-provide-risk-event-feedback.md)voor meer informatie.

### <a name="filter-risky-sign-ins"></a>Risk ante aanmeldingen filteren

Als u de gerapporteerde gegevens wilt beperken tot een niveau dat geschikt is voor u, kunt u de Risk ante gebruikers gegevens filteren met behulp van de volgende standaard velden:

- Gebruiker
- Toepassing
- Aanmeldingsstatus
- Status van risico
- Risico niveau (aggregatie)
- Risico niveau (realtime)
- Voorwaardelijke toegang
- Date
- Type risico niveau

![Rapport Riskante aanmeldingen](./media/howto-investigate-risky-users-signins/14.png)

Met het filter **naam** kunt u de naam of de User Principal Name (UPN) opgeven van de gebruiker die u bezorgt.

Met het filter **toepassing** kunt u de Cloud-app opgeven waartoe de gebruiker toegang probeerde te krijgen.

Met het filter **Aanmeldingsstatus** kunt u selecteren:

- Alle
- Geslaagd
- Mislukte poging

Met het filter **risico status** kunt u het volgende selecteren:

- Loopt risico
- Bevestigd (inbreuk)
- Bevestigd (veilig)
- Verwijderd
- Hersteld

Met het filter **risico niveau (aggregatie)** kunt u het volgende selecteren:

- Hoog
- Gemiddeld
- Laag

Met het filter **risico niveau (realtime)** kunt u het volgende selecteren:

- Hoog
- Gemiddeld
- Laag

Met het filter **voorwaardelijke toegang** kunt u het volgende selecteren:

- Alle
- Niet toegepast
- Geslaagd
- Mislukte poging

Met het filter **Datum** kunt u een tijdsbestek opgeven voor de geretourneerde gegevens.
Mogelijke waarden zijn:

- Afgelopen maand
- Afgelopen 7 dagen
- Afgelopen 24 uur
- Aangepaste tijdinterval

### <a name="download-risky-sign-ins-data"></a>Risk ante aanmeldingen gegevens downloaden

U kunt de gegevens voor Risk ante aanmeldingen downloaden als u deze buiten het Azure Portal wilt gebruiken. Als u op downloaden klikt, wordt er een CSV-bestand van de meest recente 2.500 records gemaakt. 

![Rapport over riskante gebruikers](./media/howto-investigate-risky-users-signins/15.png)

## <a name="next-steps"></a>Volgende stappen

Zie overzicht van de [Azure AD Identity Protection](overview-v2.md)voor een overzicht van Azure AD Identity Protection.
