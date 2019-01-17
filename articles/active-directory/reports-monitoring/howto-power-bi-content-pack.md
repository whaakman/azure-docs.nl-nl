---
title: Azure Active Directory Power BI Content Pack gebruiken | Microsoft Docs
description: In dit artikel leest u hoe u Azure Active Directory Power BI Content Pack gebruikt.
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c2d121106218c0965cd8f4e07776cf8d2578543f
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354158"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Het gebruik van de Azure Active Directory Power BI-inhoudspakket

|  |
|--|
|Op dit moment maakt het inhoudspakket voor Microsoft Azure AD Power BI gebruik van de Azure AD Graph-API's om gegevens op te halen uit uw Azure AD-tenant. Hierdoor ziet u mogelijk enkele verschillen tussen de gegevens die beschikbaar zijn in het inhoudspakket en de gegevens die worden opgehaald met de [Microsoft Graph-API's voor rapportage](concept-reporting-api.md). |
|  |

De Power BI content pack voor Azure Active Directory (Azure AD) bevat vooraf gemaakte rapporten om u te helpen begrijpen hoe uw gebruikers vast en gebruiken van Azure AD-functies. Hiermee kunt u meer inzicht krijgen in alle activiteiten in uw directory, met behulp van de krachtige visualisatie-ervaring in Power BI. U kunt ook uw eigen dashboard maken en deze delen met iedereen in uw organisatie. 

## <a name="prerequisites"></a>Vereisten

U moet een Azure AD premium (P1/P2)-licentie voor het gebruik van het inhoudspakket. Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden.

## <a name="install-the-content-pack"></a>Het inhoudspakket installeren

Bekijk de [snelstartgids](quickstart-install-power-bi-content-pack.md) voor het installeren van de Azure AD Power BI-inhoudspakket.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Rapporten die zijn opgenomen in deze versie van Azure AD-logboeken-inhoudspakket

De volgende rapporten zijn opgenomen in de Azure AD Power BI-inhoudspakket. De rapporten bevatten gegevens uit de **afgelopen 30 dagen**.

**App-gebruik en trends rapport**:  Dit rapport geeft u inzicht in de toepassingen die worden gebruikt in uw organisatie. U kunt een lijst van de populairste toepassingen of te begrijpen hoe een toepassing die u onlangs hebt uitgerold in uw organisatie wordt gebruikt. Hiermee kunt u bij te houden en gebruik na verloop van tijd te verbeteren.

**Aanmeldingen per locatie en gebruikers**: Dit rapport bevat de gegevens op alle aanmeldingen uitgevoerd met behulp van Azure. Met dit rapport, u kunt inzoomen op afzonderlijke aanmeldingen en vragen beantwoorden zoals:

- Waar deze gebruiker aanmelden uit?
- Welke gebruiker heeft de meeste aanmeldingen en vanaf welke locatie? 
- Is het aanmelden gelukt?  
 
U kunt ook resultaten filteren door een specifieke datum of locatie te selecteren.

**Unieke gebruikers per app**:  Dit rapport bevat een overzicht van alle unieke gebruikers met behulp van een bepaalde app. Alleen gebruikers die zijn opgenomen "*is*" aangemeld bij een toepassing.

**Apparaataanmeldingen**: Dit rapport kunt u de verschillende apparaatprofielen die worden gebruikt binnen uw organisatie begrijpen en te bepalen voor apparaten op basis van gebruik. Het biedt gegevens over het type besturingssysteem en browsers die worden gebruikt om aan te melden voor toepassingen, samen met gedetailleerde informatie over de gebruikers met inbegrip van:

- Gebruikersnaam
- IP-adres
- Locatie 
- Aanmeldingsstatus 

**SSPR-trechter**: Dit rapport kunt u begrijpen hoe de SSPR-hulpprogramma wordt gebruikt binnen uw organisatie. U kunt bekijken hoeveel wachtwoord opnieuw instellen van wachtwoorden zijn verzonden via het hulpprogramma SSPR en hoe vaak is gelukt. U kunt ook verdiepen in de problemen met wachtwoorden opnieuw instellen van wachtwoorden en te begrijpen waarom bepaalde fouten is opgetreden. 

## <a name="customize-azure-ad-activity-content-pack"></a>Het inhoudspakket Azure AD-activiteit aanpassen

**Visualisatie wijzigen**:  U kunt een rapportvisualisatie wijzigen door te klikken op **rapport bewerken** en selecteer de gewenste visualisatie.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/09.png) 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/10.png) 

**Aanvullende velden opnemen**:  U kunt een veld toevoegen aan het rapport of eruit verwijderen door het visuele element dat u wilt toevoegen of verwijderen van het veld te selecteren. U kunt bijvoorbeeld het veld 'sign-in status' toevoegen aan de tabelweergave, zoals hieronder weergegeven. 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/11.png) 

**Visualisaties aan het dashboard vastmaken**:  U kunt uw dashboard aanpassen door te nemen van uw eigen visualisaties aan het rapport en vast te maken aan het dashboard. 

![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/13.png) 
 
**Het dashboard deelt**: U kunt het dashboard ook delen met de gebruikers in uw organisatie. Nadat u het rapport deelt, kunnen gebruikers de velden die u hebt geselecteerd in het rapport zien.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Een dagelijkse vernieuwen van uw Power BI-rapport plannen

Als u een dagelijkse vernieuwen van uw Power BI-rapport plannen, gaat u naar **gegevenssets** > **instellingen** > **vernieuwen plannen** en in te stellen, zoals hieronder weergegeven.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>Bijwerken naar nieuwere versie van het inhoudspakket

Als u uw inhoudspakket bijwerken naar een nieuwere versie wilt:

- Download het nieuwe inhoudspakket en instellen met behulp van de instructies in dit artikel.

- Als u dit hebt ingesteld, gaat u naar **gegevensbron** > **instellingen** > **gegevensbronreferenties** en voer uw referenties opnieuw.

    ![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/16.png) 

Zodra u hebt gecontroleerd dat de nieuwe versie van het inhoudspakket werkt zoals verwacht, kunt u de oude versie verwijderen als die nodig zijn voor het verwijderen van de onderliggende rapporten en gegevenssets die zijn gekoppeld aan dat inhoudspakket.

## <a name="troubleshoot-content-pack-errors"></a>Inhoudspakketfouten oplossen

Als u werkt met het inhoudspakket, is het mogelijk dat u in de volgende fouten uitvoert: 

- [Vernieuwen is mislukt](#refresh-failed) 
- [Gegevensbronreferenties bijwerken is mislukt](#failed-to-update-data-source-credentials) 
- [Het importeren van gegevens is te lang duurt](#data-import-is-too-slow) 

Voor algemene hulp met Power BI raadpleegt u deze [Help-artikelen](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

### <a name="refresh-failed"></a>Vernieuwen is mislukt 
 
**Hoe deze fout is opgehaald**: Stuur een e-mail met Power BI of mislukte status in de geschiedenis vernieuwen. 


| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Fout bij fouten kunnen worden veroorzaakt wanneer de referenties van de gebruikers die verbinding maken met het inhoudspakket is opnieuw ingesteld, maar niet bijgewerkt in de instellingen van het inhoudspakket vernieuwen. | In Power BI, zoek de gegevensset die overeenkomt met het dashboard activiteitenlogboeken van Azure AD (**Azure Active Directory-activiteitenlogboek registreert**), kies planning vernieuwen en voer vervolgens uw Azure AD-referenties. |
| Vernieuwen kan mislukken vanwege problemen met de gegevens in het onderliggende inhoudspakket. | [Een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
### <a name="failed-to-update-data-source-credentials"></a>Gegevensbronreferenties bijwerken is mislukt 
 
**Hoe deze fout is opgehaald**: In Power BI, wanneer u verbinding met het inhoudspakket van Azure AD-activiteit Logboeken maakt. 

| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Gebruiker die de verbinding is niet een globale beheerder of beveiligingslezer of een beveiligingsbeheerder. | Gebruik een account dat een globale beheerder of beveiligingslezer of een beveiligingsbeheerder voor toegang tot de inhoudspakketten. |
| Uw tenant is niet een Premium-tenant of beschikt niet over ten minste één gebruiker met Premium-licentie bestand. | [Een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
### <a name="data-import-is-too-slow"></a>Het importeren van gegevens is te langzaam verlopen 
 
**Hoe deze fout is opgehaald**: In Power BI, nadat u verbinding maken met uw inhoudspakket het importeren van gegevens wordt gestart op uw dashboard voorbereiden voor Azure AD-activiteit Logboeken. U ziet het bericht: **Gegevens importeren...**  zonder eventuele verdere wordt uitgevoerd.  

| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Afhankelijk van de grootte van uw tenant, kan deze stap tussen een paar minuten duren tot 30 minuten. | Als het bericht wordt niet gewijzigd in het dashboard worden weergegeven binnen een uur [een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|
  
## <a name="next-steps"></a>Volgende stappen

* [Installeren van Power BI-inhoudspakket](quickstart-install-power-bi-content-pack.md).
* [Wat zijn Azure AD-rapporten? ](overview-reports.md).
