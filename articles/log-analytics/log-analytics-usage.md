---
title: Gegevensgebruik analyseren in Log Analytics | Microsoft Docs
description: U kunt het Gebruiksdashboard in Log Analytics gebruiken om na te gaan hoeveel gegevens er naar de OMS-service worden verzonden.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 668cde23cb717fcad52fd7823561d10735e6df1b


---
# <a name="analyze-data-usage-in-log-analytics"></a>Gegevensgebruik analyseren in Log Analytics
Met Log Analytics worden periodiek gegevens verzameld en verzonden naar de OMS-service.  U kunt het **Gebruiksdashboard in Log Analytics** gebruiken om na te gaan hoeveel gegevens er naar de OMS-service worden verzonden. Op het dashboard wordt ook weergegeven hoeveel gegevens er op basis van de oplossingen worden verzonden en hoe vaak er via uw servers gegevens worden verzonden.

> [!NOTE]
> Als u een gratis account hebt gemaakt, kunt u dagelijks maximaal 500 MB aan gegevens naar de OMS-service verzenden. Als de dagelijkse limiet is bereikt, stopt het analyseren van gegevens. Dit wordt aan het begin van de volgende dag hervat. In dit geval moet u ook alle gegevens opnieuw verzenden die niet zijn geaccepteerd of verwerkt via OMS.

Als u de dagelijkse gebruikslimiet hebt overschreden of als u deze limiet nadert, kunt u ervoor kiezen om een oplossing te verwijderen om de hoeveelheid gegevens die naar de OMS-service wordt verzonden, te verminderen. Zie [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) (Log Analytics-oplossingen toevoegen uit de oplossingengalerie) voor meer informatie over het verwijderen van oplossingen.

![gebruiksdashboard](./media/log-analytics-usage/usage-dashboard01.png)

Het **Log Analytics-gebruiksdashboard** bevat de volgende informatie:

- Gegevensvolume
    - Gegevensvolume gedurende een bepaalde periode (op basis van uw huidige tijdsbereik)
    - Gegevensvolume per oplossing
    - Gegevens die niet zijn gekoppeld aan een computer
- Computers
    - Computers waarmee gegevens worden verzonden
    - Computers zonder gegevens in de afgelopen 24 uur
- Aanbiedingen
    - Knooppunten voor Insight en Analytics
    - Knooppunten voor automatisering en beheer
    - Knooppunten voor beveiliging
- Prestaties
    - Benodigde tijd voor het verzamelen en indexeren van gegevens
- Lijst met query's

## <a name="to-work-with-usage-data"></a>Werken met gebruiksgegevens
1. Meld u met uw Azure-abonnement aan bij [Azure Portal](https://portal.azure.com) als u dit nog niet hebt gedaan.
2. Klik in het menu **Hub** op **Meer services** en typ in de lijst met resources op **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Log Analytics**.  
    ![Azure-hub](./media/log-analytics-usage/hub.png)
3. Op het **Log Analytics**-dashboard wordt een lijst met uw werkruimten weergegeven. Selecteer een werkruimte.
4. Klik op het dashboard *Werkruimte* op **Log Analytics-gebruik**.
5. Klik op het **Log Analytics-gebruiksdashboard** op **Tijd: afgelopen 24 uur** om het tijdsinterval te wijzigen.  
    ![tijdsinterval](./media/log-analytics-usage/time.png)
6. Bekijk de blades voor de gebruikscategorie waarin gebieden worden weergegeven waarin u bent geïnteresseerd. Kies een blade en klik vervolgens op een item op de blade om meer details weer te geven in [Zoeken in logboeken](log-analytics-log-searches.md).  
    ![voorbeeld van blade voor gegevensgebruik](./media/log-analytics-usage/blade.png)
7. Bekijk op het dashboard Zoeken in logboeken de resultaten die zijn geretourneerd na de zoekopdracht.  
    ![voorbeeld van gebruik van zoeken in logboeken](./media/log-analytics-usage/usage-log-search.png)


## <a name="next-steps"></a>Volgende stappen
* Raadpleeg [Log searches in Log Analytics](log-analytics-log-searches.md) (Zoekopdrachten in logboeken in Log Analytics) om gedetailleerde informatie te bekijken die is verzameld en verzonden naar OMS, per functie en per oplossing.



<!--HONumber=Jan17_HO1-->


