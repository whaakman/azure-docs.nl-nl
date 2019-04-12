---
title: Azure Marketplace SEO-richtlijnen | Microsoft Docs
description: Biedt richtlijnen voor het maximaliseren van zoekmachines (SEO).
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: pbutlerm
ms.openlocfilehash: da7b59400baa4c964dff71fa1f842fede9d68df1
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59488846"
---
# <a name="azure-marketplace-seo-guidance"></a>Azure Marketplace SEO-richtlijnen

Dit artikel wordt uitgelegd hoe u kunt optimaliseren, zichtbaarheid van uw aanbieding via de zoekfunctie in de [Azure Marketplace](https://azuremarketplace.microsoft.com) en [AppSource](https://appsource.microsoft.com). 


## <a name="general-explanation-of-algorithm"></a>Algemene uitleg van het algoritme

Microsoft-marktplaatsen gebruikmaken van Azure Search voor het aansturen van de zoekmogelijkheden van de site. Het algoritme is gebaseerd op termijn frequentie – inverse document frequentie ([TF-IDF](https://en.wikipedia.org/wiki/Tf–idf)). De standaard [Lucene Analyzer](https://lucene.apache.org/core/) wordt gebruikt.

In het algemeen alle tekst velden, categorieën en industrieën en opgenomen in de weightage van de volgorde van relevantie. Vaktermen die zelden worden gebruikt door apps, maar vaak in uw app genereert een hogere score in de overeenkomst met zoeken. Met inbegrip van termen, zoals 'VM' zou weinig voordeel bieden dat 'Azure search' zou worden nog veel meer gespecialiseerd.
Hieronder vindt u de meest relevante velden om te overwegen.

 
|  Veld                   | Urgentie | Richtlijnen                                                                                            |
|  --------------------    | ----------                   | ---------------                                                                   |
| Naam van aanbieding:               |  Hoog      | Exacte of dicht bij een volledige overeenkomst met search query resulteert in hoge classificatie.                       |
| Naam van de uitgever           |  Hoog      | Exacte of dicht bij een volledige overeenkomst met search query resulteert in hoge classificatie.                       |
| Korte beschrijving        |  Middelgroot    | Naamgeving van apps en uitgever opgegeven namen wordt bijna een hoge ranking garanderen, kan dit niet het meest relevant. In dit geval is een korte beschrijving essentieel. De tekst behouden beknopt en het herstelpunt. Trefwoorden en verwachte zoektermen moeten worden opgenomen voor de beste resultaten.  Bijvoorbeeld "Dit is de beste Retail POS volledig gebaseerd op Dynamics 365" is minder effectief dan ' Retail POS (punt van verkoop) voor Dynamics 365 ".  | 
| Lange beschrijving         |  Laag       | Beschrijving biedt een manier om dieper. De beschrijvingen van de meest effectieve redelijke lengte en trefwoorden worden gebruikt.  Een aan de-punten beschrijvingen met trefwoorden profiteren meer dan lang lange tekst. Zorg ervoor dat belangrijkste termen, zoals 'IoT' zijn aanwezig in de beschrijving.  |
| Productcategorieën       | Middelgroot     |  Productcategorieën worden bepaald door een combinatie van opties van de uitgever en Microsoft. Selecteer deze categorieën op de juiste wijze, zodat gebruikers de apps in de juiste categorie eenvoudig kunnen vinden. |
|  |  |  |


## <a name="other-tips"></a>Andere Tips

-   Search stelt de zware gebruikersactiviteit opgehaald. Bepaalt de volgorde van overeenkomsten op basis van app-naam/uitgever. Korte beschrijving wordt het sleutelveld voor wanneer de zoekterm niet exact overeenkomt met de naam van uitgever/app is.
-   Documenten downloaden zijn niet opgenomen in weightage zoeken.
-   Uw apps daadwerkelijke aanschaf en het gebruik is van invloed op search ook classificeren. Bijvoorbeeld, krijgt twee gelijkwaardige apps waarbij een aanzienlijk meer gebruikers heeft een hogere ranking.
