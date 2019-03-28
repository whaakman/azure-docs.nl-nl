---
title: Veelgestelde vragen en bekende problemen met identity protection (vernieuwd) in Azure Active Directory | Microsoft Docs
description: Veelgestelde vragen en bekende problemen met identity protection (vernieuwd) in Azure Active Directory.
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
ms.date: 01/24/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 392b7a08d9422658c5620f60e9c1caca074bc85e
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521683"
---
# <a name="faqs-and-known-issues-with-identity-protection-refreshed-in-azure-active-directory"></a>Veelgestelde vragen en bekende problemen met identity protection (vernieuwd) in Azure Active Directory


## <a name="dismiss-user-risk-known-issues"></a>Bekende problemen gebruikersrisico negeren

**Negeren van gebruikersrisico** in de klassieke versie Identity Protection Hiermee stelt u de actor in de geschiedenis van risico's van de gebruiker in Identity Protection (vernieuwd) voor **Azure AD**.


**Negeren van gebruikersrisico** in Identity Protection (vernieuwd) Hiermee stelt u de actor in de geschiedenis van risico's van de gebruiker in Identity Protection (vernieuwd) voor **\<beheerdersnaam met een hyperlink naar de blade van de gebruiker\>**.

Er is een bekend probleem met huidige latentie in de gebruikersstroom voor het ontslag van risico's veroorzaken. Hebt u een 'beleid voor gebruikersrisico's ', stopt dit beleid toe te passen op gesloten gebruikers binnen enkele minuten van te klikken op 'Gebruikersrisico negeren'. Maar zijn er bekende vertragingen bij het vernieuwen van de 'risico-status' gesloten gebruikers UX. Als tijdelijke oplossing, vernieuw de pagina op het browserniveau van de om te zien dat de meest recente gebruiker 'Risico state'.


## <a name="risky-users-report-known-issues"></a>Riskante gebruikers rapporteren bekende problemen

Query's op de **gebruikersnaam** veld zijn hoofdlettergevoelig, terwijl de query's op de **naam** case-agnostische zijn.

Bij het omschakelen van **datums weergeven als** wordt ingeschakeld, wordt de **LAATST bijgewerkt risico** kolom. Klik op kolom toegewezen **kolommen** aan de bovenkant van het tabblad riskante gebruikers.

**Alle gebeurtenissen sluiten** in de klassieke versie Identity Protection de status van de risicogebeurtenissen naar stelt **gesloten (opgelost)**.


## <a name="risky-sign-ins-report-known-issues"></a>Rapport riskante aanmeldingen bekende problemen

**Los** gebeurtenis wordt op een risico op de status ingesteld op **gebruikers zijn geslaagd voor MFA aangestuurd door de risico's gebaseerd beleid**.


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
