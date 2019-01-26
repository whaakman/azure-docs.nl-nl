---
title: Veelgestelde vragen en bekende problemen met identity protection (vernieuwd) in Azure Active Directory | Microsoft Docs
description: Veelgestelde vragen en bekende problemen met identity protection (vernieuwd) in Azure Active Directory.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2019
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: c9660dccf4929cb62e78dae1956d01b0d3ba8fcb
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913830"
---
# <a name="faqs-and-known-issues-with-identity-protection-refreshed-in-azure-active-directory"></a>Veelgestelde vragen en bekende problemen met identity protection (vernieuwd) in Azure Active Directory


## <a name="dismiss-user-risk"></a>Gebruikersrisico negeren

**Negeren van gebruikersrisico** in de klassieke versie Identity Protection Hiermee stelt u de actor in de geschiedenis van risico's van de gebruiker in Identity Protection (vernieuwd) voor **Azure AD**.


**Negeren van gebruikersrisico** in Identity Protection (vernieuwd) Hiermee stelt u de actor in de geschiedenis van risico's van de gebruiker in Identity Protection (vernieuwd) voor **\<beheerdersnaam met een hyperlink naar de blade van de gebruiker\>**.


## <a name="risky-users-report"></a>Rapport over riskante gebruikers

Query's op de **gebruikersnaam** veld zijn hoofdlettergevoelig, terwijl de query's op de **naam** case-agnostische zijn.

Bij het omschakelen van **datums weergeven als** wordt ingeschakeld, wordt de **LAATST bijgewerkt risico** kolom. Klik op kolom toegewezen **kolommen** aan de bovenkant van het tabblad riskante gebruikers.

**Alle gebeurtenissen sluiten** in de klassieke versie Identity Protection de status van de risicogebeurtenissen naar stelt **gesloten (opgelost)**.

Als u probeert te krijgen tot het rapport riskante gebruikers door te klikken op **riskante gebruikers rapporteren** in een record aanmelden in het rapport riskante aanmeldingen ze soms kunnen aantonen **is iets fout gegaan. Probeer het opnieuw**. U lost dit probleem, klikt u op **toepassen** of **opnieuw** aan de bovenkant van het scherm en de riskante gebruiker (s) gegevens wordt gevuld.


## <a name="risky-sign-ins-report"></a>Rapport Riskante aanmeldingen

**Los** gebeurtenis wordt op een risico op de status ingesteld op **gebruikers zijn geslaagd voor MFA aangestuurd door de risico's gebaseerd beleid**.

**Opnieuw instellen van** in de **riskante aanmeldingen** rapport wist niet de waarde van de **risico gebeurtenistype**.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-event"></a>Waarom kan ik mijn eigen risiconiveaus voor elke risicogebeurtenis niet instellen?

Risiconiveaus in Identity Protection zijn gebaseerd op de nauwkeurigheid van de detectie en gebruikmaken van onze beheerde machine learning. Om aan te passen welke gebruikers ervaring worden aangeboden, beheerder kunt opnemen/uitsluiten bepaalde gebruikers/groepen uit het Gebruikersrisico en risico's beleid voor aanmelden.


### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>Waarom de locatie van een aanmelding niet overeenkomt met waar de gebruiker echt iemand aangemeld vanuit?

IP-geolocatie toewijzing is een uitdaging voor de hele branche. Als u denkt dat de locatie die worden vermeld in het aanmeldingenrapport komt niet overeen met de werkelijke locatie, kunt u contact opnemen voor de ondersteuning. 


### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>De werking van de feedbackmechanismen in Identity Protection

**Controleer of aangetast** (op een aanmelding) – informeert Azure AD Identity Protection dat de aanmelding is niet uitgevoerd door de eigenaar van de identiteit en geeft aan dat een inbreuk op.

- Na ontvangst van deze feedback gaan we de status van de risico's aanmelding en gebruiker om te **bevestigd aangetast** en risiconiveau naar **hoge**.

- Daarnaast bieden we de informatie voor ons machine learning-systemen voor toekomstige verbeteringen in de risico-evaluatie.

    > [!NOTE]
    > Als de gebruiker is al hersteld, moet u niet op **bevestigen aangetast** omdat deze de status van de risico's aanmelding en gebruiker om te worden verplaatst **bevestigd aangetast** en risiconiveau naar **hoge**.

**Bevestig veilige** (op een aanmelding bij): Azure AD Identity Protection informeert dat de aanmelding is uitgevoerd door de eigenaar van de identiteit en geeft niet aan een inbreuk op.

- Na ontvangst van deze feedback gaan we de aanmelding (niet de gebruiker) de status van risico **bevestigd veilig** en het risiconiveau voor **-**.

- Daarnaast bieden we de informatie voor ons machine learning-systemen voor toekomstige verbeteringen in de risico-evaluatie.

    > [!NOTE]
    > Als u denkt de gebruiker niet worden gecompromitteerd dat, gebruikt u **negeren gebruikersrisico** op het gebruikersniveau van de in plaats van **bevestigd veilig** op het niveau van de aanmelding. Een **gebruikersrisico negeren** op de gebruiker niveau sluit u het gebruikersrisico en alle riskante aanmeldingen en risicogebeurtenissen uit het verleden.



### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-events-are-shown-in-identity-protection"></a>Waarom zie ik een risicoscore van de gebruiker met een lage (of hoger), zelfs als er geen riskante aanmeldingen of risicogebeurtenissen worden weergegeven in Identity Protection?

De gebruiker opgegeven risico is cumulatief van aard en niet is verlopen, een gebruiker mogelijk een gebruikersrisico van laag of hierboven, zelfs als er zijn geen recente riskante aanmeldingen of risicogebeurtenissen die worden weergegeven in Identity Protection. Dit kan gebeuren als het alleen schadelijke activiteiten van een gebruiker die heeft plaatsgevonden na de periode waarvoor we de details van riskante aanmeldingen en risicogebeurtenissen opslaan. We verlopen gebruikersrisico niet omdat ongewenste actoren bekend is om te blijven in de omgeving van klanten meer dan 140 dagen achter een waarmee is geknoeid identiteit voor het afhandelen van hun aanval. Klanten kunnen risico tijdlijn van de gebruiker om te begrijpen waarom een gebruiker is risico lopen door te gaan naar aangeraden: `Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>Waarom een aanmelding hoeft een ' aanmelden (totaal) "risicoscore van hoge wanneer de detecties die zijn gekoppeld aan deze van de laag of Gemiddeld risico's zijn?

De maximale cumulatieve risicoscore kan worden gebaseerd op andere functies van de aanmelding of het feit dat meer dan één detectie gestart voor die aanmelding. En omgekeerd, een aanmelding mogelijk een aanmeldingsrisico (statistische) van gemiddeld, zelfs als de detecties die zijn gekoppeld aan de aanmelding van een hoog risico zijn. 
