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
ms.openlocfilehash: d810b8be496bbfd1c5bd88d8221e77dd5b76c889
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622960"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Het gebruik van de Azure Active Directory Power BI-inhoudspakket

|  |
|--|
|Op dit moment maakt het inhoudspakket voor Microsoft Azure AD Power BI gebruik van de Azure AD Graph-API's om gegevens op te halen uit uw Azure AD-tenant. Hierdoor ziet u mogelijk enkele verschillen tussen de gegevens die beschikbaar zijn in het inhoudspakket en de gegevens die worden opgehaald met de [Microsoft Graph-API's voor rapportage](concept-reporting-api.md). |
|  |

De Power BI content pack voor Azure Active Directory (Azure AD) bevat vooraf gemaakte rapporten om u te helpen begrijpen hoe uw gebruikers vast en gebruiken van Azure AD-functies. Hiermee kunt u meer inzicht krijgen in alle activiteiten in uw directory, met behulp van de krachtige visualisatie-ervaring in Power BI. U kunt ook uw eigen dashboard maken en deze delen met iedereen in uw organisatie. 

## <a name="prerequisites"></a>Vereisten

U moet een Azure AD premium (P1/P2)-licentie voor het gebruik van het inhoudspakket. 

## <a name="install-the-content-pack"></a>Het inhoudspakket installeren

Bekijk de [snelstartgids](quickstart-install-power-bi-content-pack.md) voor het installeren van de Azure AD Power BI-inhoudspakket.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Rapporten die zijn opgenomen in deze versie van Azure AD-logboeken-inhoudspakket

De volgende rapporten zijn opgenomen in de Azure AD Power BI-inhoudspakket. De rapporten bevatten gegevens uit de **afgelopen 30 dagen**.

**App-gebruik en trends rapport**: in dit rapport geeft u inzicht in de toepassingen die worden gebruikt in uw organisatie. U kunt een lijst van de populairste toepassingen of te begrijpen hoe een toepassing die u onlangs hebt uitgerold in uw organisatie wordt gebruikt. Hiermee kunt u bij te houden en gebruik na verloop van tijd te verbeteren.

**Aanmeldingen per locatie en gebruikers**: in dit rapport bevat gegevens over alle aanmeldingen uitgevoerd met behulp van Azure. Met dit rapport, u kunt inzoomen op afzonderlijke aanmeldingen en vragen beantwoorden zoals:

- Waar deze gebruiker aanmelden uit?
- Welke gebruiker heeft de meeste aanmeldingen en vanaf welke locatie? 
- Is het aanmelden gelukt?  
 
U kunt ook resultaten filteren door een specifieke datum of locatie te selecteren.

**Unieke gebruikers per app**: dit rapport bevat een overzicht van alle unieke gebruikers met behulp van een bepaalde app. Alleen gebruikers die zijn opgenomen "*is*" aangemeld bij een toepassing.

**Apparaataanmeldingen**: met dit rapport kunt u de verschillende apparaatprofielen die worden gebruikt binnen uw organisatie begrijpen en te bepalen voor apparaten op basis van gebruik. Het biedt gegevens over het type besturingssysteem en browsers die worden gebruikt om aan te melden voor toepassingen, samen met gedetailleerde informatie over de gebruikers met inbegrip van:

- Gebruikersnaam
- IP-adres
- Locatie 
- Aanmeldingsstatus 

**SSPR-trechter**: met dit rapport kunt u inzicht in hoe de SSPR-hulpprogramma wordt gebruikt binnen uw organisatie. U kunt bekijken hoeveel wachtwoord opnieuw instellen van wachtwoorden zijn verzonden via het hulpprogramma SSPR en hoe vaak is gelukt. U kunt ook verdiepen in de problemen met wachtwoorden opnieuw instellen van wachtwoorden en te begrijpen waarom bepaalde fouten is opgetreden. 

## <a name="customize-azure-ad-activity-content-pack"></a>Het inhoudspakket Azure AD-activiteit aanpassen

**Visualisatie wijzigen**: u kunt een rapportvisualisatie wijzigen door te klikken op **Rapport bewerken** en de gewenste visualisatie te selecteren.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/09.png) 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/10.png) 

**Aanvullende velden opnemen**: u kunt een veld toevoegen aan het rapport of eruit verwijderen door het visuele element te selecteren waaraan u het veld wilt toevoegen of waaruit u het veld wilt verwijderen. U kunt bijvoorbeeld het veld 'sign-in status' toevoegen aan de tabelweergave, zoals hieronder weergegeven. 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/11.png) 

**Visualisaties aan het dashboard vastmaken**: U kunt uw dashboard aanpassen door te nemen van uw eigen visualisaties aan het rapport en vast te maken aan het dashboard. 

![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/13.png) 
 
**Het dashboard deelt**: U kunt ook het dashboard delen met de gebruikers in uw organisatie. Nadat u het rapport deelt, kunnen gebruikers de velden die u hebt geselecteerd in het rapport zien.
 
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

## <a name="still-having-issues"></a>Nog steeds problemen? 

Raadpleeg onze [handleiding voor het oplossen van problemen](troubleshoot-content-pack.md). Voor algemene hulp met Power BI raadpleegt u deze [Help-artikelen](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
 
 
## <a name="next-steps"></a>Volgende stappen

* [Installeren van Power BI-inhoudspakket](quickstart-install-power-bi-content-pack.md).
* [Fouten met inhoudspakketten oplossen](troubleshoot-content-pack.md).
* [Wat zijn Azure AD-rapporten? ](overview-reports.md).
