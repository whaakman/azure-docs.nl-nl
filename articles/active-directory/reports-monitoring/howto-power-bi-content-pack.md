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
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 2cfdd5b903b8ffd9702745df150fe578d42e5a11
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735829"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Azure Active Directory Power BI Content Pack gebruiken

|  |
|--|
|Op dit moment het Azure AD Power BI-inhoudspakket maakt gebruik van de Azure AD Graph-API's voor het ophalen van gegevens uit uw Azure AD-tenant. Als gevolg hiervan, ziet u mogelijk enkele verschillen tussen de gegevens die beschikbaar zijn in het inhoudspakket en de gegevens die worden opgehaald met behulp van de [Microsoft Graph-API's voor het melden van](concept-reporting-api.md). |
|  |

Als IT-beheerder moet u begrijpen hoe uw gebruikers vast en Azure Active Directory-functies gebruiken. Hiermee kunt u het plannen van uw IT-infrastructuur en communicatie met het-gebruik verhogen en optimaal gebruikmaken van Azure AD-functies. Power BI-inhoudspakket voor Azure Active Directory biedt u de mogelijkheid voor verdere analyse van uw gegevens voor het verzamelen van uitgebreidere inzicht in wat met uw directory gebeurt er. Met de integratie van Azure Active Directory-API's in Power BI, kunt u eenvoudig de vooraf gemaakte inhoudspakket downloaden en krijgt u inzicht in alle activiteiten binnen uw Azure Active Directory met behulp van de krachtige visualisatie-ervaring die Power BI biedt. U kunt uw eigen dashboard maken en dit dashboard vervolgens eenvoudig delen met andere personen binnen uw organisatie. 

## <a name="install-the-content-pack"></a>Het inhoudspakket installeren

Bekijk de [snelstartgids](quickstart-install-power-bi-content-pack.md) voor het installeren van de Azure AD Power BI-inhoudspakket.

## <a name="what-can-i-do-with-this-content-pack"></a>Wat kan ik doen met dit inhoudspakket?

Voordat we gaan bespreken wat u allemaal kunt doen met dit inhoudspakket, krijgt u eerst een kort overzicht van de verschillende rapporten in het inhoudspakket. De rapportgegevens hebben betrekking op de **afgelopen 30 dagen**.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Rapporten die zijn opgenomen in deze versie van het inhoudspakket met Azure Active Directory-logboeken

**App-gebruik en trends**: krijg een duidelijk beeld van de apps die worden gebruikt in uw organisatie en welke apps het meest worden gebruikt en wanneer. Gebruik dit rapport om inzicht te krijgen in de manier waarop een app die u onlangs hebt uitgerold in uw organisatie, wordt gebruikt of om vast te stellen welke apps populair zijn. Aan de hand van deze informatie kunt u het gebruik van de app verbeteren als deze momenteel niet wordt gebruikt.

**Aanmeldingen per locatie en gebruikers**: verzamel inzichten over alle aanmeldingen met behulp van Azure en krijg een beeld van de identiteit van de gebruikers. U kunt zo gedetailleerde informatie verkrijgen over afzonderlijke aanmeldingen en vragen beantwoorden zoals:

- Vanaf welke locatie hebben deze gebruikers zich aangemeld?
- Welke gebruiker heeft de meeste aanmeldingen en vanaf welke locatie? 
- Is het aanmelden gelukt?  
 
U kunt inzoomen op details door op een bepaalde datum of locatie te klikken.

**Unieke gebruikers per app**: bekijk een weergave van alle unieke gebruikers die een bepaalde app gebruiken. Het betreft hier alleen gebruikers die *het* gelukt is om zich aan te melden bij een toepassing.

**Apparaataanmeldingen**: bekijk gegevens over het type besturingssysteem en de browsers die door gebruikers in uw organisatie worden gebruikt, met inbegrip van de volgende gedetailleerde informatie over de gebruikers:

- Gebruikersnaam
- IP-adres
- Locatie 
- Aanmeldingsstatus 

Met dit rapport kunt u een beeld krijgen van de verschillende apparaatprofielen die worden gebruikt binnen uw organisatie en op basis daarvan beleidsregels voor apparaten instellen.

**SSPR-trechter**: bekijk informatie over de manier waarop wachtwoorden opnieuw worden ingesteld in uw organisatie. U kunt zien hoe vaak er is geprobeerd een wachtwoord opnieuw in te stellen via het hulpprogramma SSPR en hoe vaak dit is gelukt. Analyseer de SSPR-trechter om te achterhalen waarom het in bepaalde gevallen niet lukt om wachtwoorden opnieuw in te stellen met SSPR. Dit rapport geeft een beter beeld van de manier waarop het SSPR-hulpprogramma wordt ingezet binnen uw organisatie, zodat u de juiste beslissingen kunt nemen.

## <a name="customizing-azure-ad-activity-content-pack"></a>Het inhoudspakket Azure AD-activiteit aanpassen

**Visualisatie wijzigen**: u kunt een rapportvisualisatie wijzigen door te klikken op **Rapport bewerken** en de gewenste visualisatie te selecteren.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/09.png) 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/10.png) 

**Aanvullende velden opnemen**: u kunt een veld toevoegen aan het rapport of eruit verwijderen door het visuele element te selecteren waaraan u het veld wilt toevoegen of waaruit u het veld wilt verwijderen. In het onderstaande voorbeeld voeg ik het veld Sign-in Status toe aan de tabelweergave. 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/11.png) 

**Visualisaties vastmaken aan uw dashboard**: U kunt het dashboard aanpassen en uw eigen visualisaties toevoegen aan het rapport en deze vervolgens vastmaken aan het dashboard. In het onderstaande voorbeeld heb ik een nieuw filter met de naam Sign-in Status toegevoegd en opgenomen in het rapport. Daarnaast heb ik de visualisatie gewijzigd van een staafdiagram in een lijndiagram en heb ik dit nieuwe visuele element vastgemaakt aan het dashboard.

![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/12.png) 

![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/13.png) 
 

 


**Uw dashboard delen**: als u de gewenste inhoud hebt gemaakt, kunt u het dashboard delen met gebruikers in uw organisatie. Houd er rekening mee dat wanneer u het rapport deelt deze gebruikers de velden kunnen zien die u hebt geselecteerd in het rapport.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Dagelijks vernieuwen van het Power BI-rapport plannen

Als u het Power BI-rapport dagelijks automatisch wilt bijwerken, gaat u naar **Gegevenssets > Instellingen > Geplande vernieuwing** en geeft u de onderstaande instellingen op.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a>Bijwerken naar een nieuwere versie van het inhoudspakket

Als u uw inhoudspakket wilt bijwerken naar een nieuwere versie:

- Download het nieuwe inhoudspakket en stel het in volgens de instructies in dit artikel.

- Als dat is gebeurd, gaat u naar **Gegevensbron > Instellingen > Gegevensbronreferenties** en voert u uw referenties opnieuw in, zoals hieronder wordt weergegeven.

    ![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/16.png) 

Zodra de nieuwe versie van het inhoudspakket zonder problemen werkt, kunt u de oude versie desgewenst verwijderen door de onderliggende rapporten en gegevenssets te verwijderen die aan dat inhoudspakket zijn gekoppeld.

## <a name="still-having-issues"></a>Nog steeds problemen? 

Raadpleeg onze [handleiding voor het oplossen van problemen](troubleshoot-content-pack.md). Voor algemene hulp met Power BI raadpleegt u deze [Help-artikelen](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Volgende stappen

* [Installeren van Power BI-inhoudspakket](quickstart-install-power-bi-content-pack.md).
* [Inhoudspakketfouten oplossen](troubleshoot-content-pack.md).
* [Wat zijn Azure AD-rapporten? ](overview-reports.md).
