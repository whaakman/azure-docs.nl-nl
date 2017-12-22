---
title: Azure Active Directory Power BI Content Pack gebruiken | Microsoft Docs
description: In dit artikel leest u hoe u Azure Active Directory Power BI Content Pack gebruikt.
services: active-directory
author: MarkusVi
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: 
ms.topic: get-started-article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/06/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 503b3f4c576382d8ce965d1f90aadda32c819a0b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Azure Active Directory Power BI Content Pack gebruiken

Voor u als IT-beheerder is het enorm belangrijk om te weten hoe uw gebruikers functies van Azure Active Directory implementeren en gebruiken. Deze informatie is vereist om uw IT-infrastructuur en communicatie te plannen om zo het gebruik van AAD-functies te vergroten en het rendement te optimaliseren. Power BI Content Pack voor Azure Active Directory biedt u de mogelijkheid om uw gegevens verder te analyseren, zodat u begrijpt hoe u deze gegevens kunt gebruiken voor het verzamelen van inzichten die nog beter laten zien hoe Azure Active Directory wordt ingezet voor de verschillende toepassingsmogelijkheden die een belangrijke rol spelen in uw organisatie.  Met de integratie van Azure Active Directory-API's in Power BI kunt u eenvoudig de vooraf samengestelde inhoudspakketten downloaden en met behulp van de krachtige visualisatie-opties van Power BI inzicht verkrijgen in alle activiteiten binnen uw Azure Active Directory. U kunt uw eigen dashboard maken en dit dashboard vervolgens eenvoudig delen met andere personen binnen uw organisatie. 

In dit onderwerp krijgt u stapsgewijze instructies voor het installeren en gebruiken van het inhoudspakket in uw omgeving.

## <a name="installation"></a>Installeren  

**Power BI Content Pack installeren:**

1. Meld u aan bij [Power BI](https://app.powerbi.com/groups/me/getdata/services) met uw Power BI-account (dit is hetzelfde account als uw O365- of Azure AD-account).

2. Selecteer onderaan in het navigatiedeelvenster aan de linkerkant de optie **Gegevens ophalen**.

    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. Klik in het vak **Services** op **Ophalen**.
   
    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  Zoek naar **Azure Active Directory**.

    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  Als u daarom wordt gevraagd, typt u de id van uw Azure AD-tenant en klikt u vervolgens op **Volgende**.

    > [!TIP] 
    > Als u een snelle manier zoekt om de Tenant-ID voor uw Office 365 / Azure AD-tenant op te halen, kunt u zich aanmelden bij de Azure AD-portal, inzoomen op de map en de **map-ID** op de [ **eigenschappenpagina** kopiëren](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties).

    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  Klik op **Aanmelden**. 
 
    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  Voer uw gebruikersnaam en wachtwoord in en klik vervolgens op **Aanmelden**.
 
    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  Klik in het volgende dialoogvenster op **Accepteren**.
 
9.  Wanneer het dashboard Activiteitenlogboeken van Azure Active Directory is gemaakt, klikt u erop.
 
    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

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
 
![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

**Aanvullende velden opnemen**: u kunt een veld toevoegen aan het rapport of eruit verwijderen door het visuele element te selecteren waaraan u het veld wilt toevoegen of waaruit u het veld wilt verwijderen. In het onderstaande voorbeeld voeg ik het veld Sign-in Status toe aan de tabelweergave. 
 
![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

**Visualisaties vastmaken aan uw dashboard**: U kunt het dashboard aanpassen en uw eigen visualisaties toevoegen aan het rapport en deze vervolgens vastmaken aan het dashboard. In het onderstaande voorbeeld heb ik een nieuw filter met de naam Sign-in Status toegevoegd en opgenomen in het rapport. Daarnaast heb ik de visualisatie gewijzigd van een staafdiagram in een lijndiagram en heb ik dit nieuwe visuele element vastgemaakt aan het dashboard.

![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


**Uw dashboard delen**: als u de gewenste inhoud hebt gemaakt, kunt u het dashboard delen met gebruikers in uw organisatie. Houd er rekening mee dat wanneer u het rapport deelt deze gebruikers de velden kunnen zien die u hebt geselecteerd in het rapport.
 
![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Dagelijks vernieuwen van het Power BI-rapport plannen

Als u het Power BI-rapport dagelijks automatisch wilt bijwerken, gaat u naar **Gegevenssets > Instellingen > Geplande vernieuwing** en geeft u de onderstaande instellingen op.
 
![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a>Bijwerken naar een nieuwere versie van het inhoudspakket

Als u uw inhoudspakket wilt bijwerken naar een nieuwere versie:

- Download het nieuwe inhoudspakket en stel het in volgens de instructies in dit artikel.

- Als dat is gebeurd, gaat u naar **Gegevensbron > Instellingen > Gegevensbronreferenties** en voert u uw referenties opnieuw in, zoals hieronder wordt weergegeven.

    ![Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

Zodra de nieuwe versie van het inhoudspakket zonder problemen werkt, kunt u de oude versie desgewenst verwijderen door de onderliggende rapporten en gegevenssets te verwijderen die aan dat inhoudspakket zijn gekoppeld.

## <a name="still-having-issues"></a>Nog steeds problemen? 

Raadpleeg onze [handleiding voor het oplossen van problemen](active-directory-reporting-troubleshoot-content-pack.md). Voor algemene hulp met Power BI raadpleegt u deze [Help-artikelen](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor een overzicht van de rapportage.
