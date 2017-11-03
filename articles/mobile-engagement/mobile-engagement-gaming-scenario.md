---
title: Azure Mobile Engagement-implementatie voor Gaming-App
description: Gaming-app-scenario voor het implementeren van Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0ca35a3d634db8eb5c63afacba046a35b8a3e7ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a>Mobile Engagement met Gaming-App implementeren
## <a name="overview"></a>Overzicht
Een gaming-opstarten heeft een nieuwe visserij gebaseerd role play/strategie gameapps gestart. Het spel is actief en werkend gedurende 6 maanden. Dit spel is een grote voltooid en heeft miljoenen downloads en de bewaarperiode is zeer hoog in vergelijking met andere opstarten game apps. Tijdens de vergadering elk kwartaal revisie akkoord belanghebbenden dat ze nodig hebben om te verhogen gemiddelde omzet per gebruiker (ARPU). Premium in de game-pakketten zijn beschikbaar als speciale aanbiedingen. Deze game packs kunnen gebruikers de vormgeving en prestaties van hun visserij regels en stelen of tackles in het spel bijwerken. Er zijn echter pakket verkoop zeer lage. Zodat zij eerst bepalen voor het analyseren van de gebruikerservaring met een hulpprogramma voor analyse en voor het ontwikkelen van een advies vervolgens geavanceerde programma om te verhogen met behulp van verkoop segmentering.

Op basis van de [Azure Mobile Engagement - instructiehandleiding met aanbevolen procedures](mobile-engagement-getting-started-best-practices.md) ze bouwen engagement-strategie.

## <a name="objectives-and-kpis"></a>Doelstellingen en KPI 's
Voldoen aan de belangrijkste belanghebbenden voor het spel. Alle overeenstemming worden bereikt over een hoofddoel - premium pakket verkoop verhogen met 15%. Ze maken deze doelstelling Business Key Performance Indicators (KPI's) naar een meting en het station

* Op welk niveau van het spel worden deze pakketten gekocht?
* Wat is de omzet per gebruiker, per sessie, per week en per maand?
* Wat zijn de typen favoriete aankoop?

Deel 1 van de [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) wordt uitgelegd hoe de doelstellingen en KPI's definiëren. 

De Mobile Product Manager maakt met de zakelijke KPI's nu gedefinieerd, Betrokkenheids-KPI's om te bepalen van de nieuwe gebruiker trends en te bewaren.

* Controleren van de retentie en gebruiken in de volgende intervallen: dagelijks, 2 dagen, wekelijks, maandelijks en elke 3 maanden
* Actieve gebruikers
* De classificatie van apps in het archief

Op basis van de aanbevelingen van het IT-team, zijn de volgende technische KPI's toegevoegd aan de volgende vragen beantwoorden:

* Wat is er een pad naar de gebruiker (welke bezochte hoeveel gebruikers besteden aan het)
* Aantal crashes of fouten aangetroffen per sessie
* Welke versies van het besturingssysteem worden uitgevoerd voor Mijn gebruikers?
* Wat is de gemiddelde grootte van het scherm voor Mijn gebruikers?
* Wat voor soort verbinding met internet hebt mijn gebruikers?

De Mobile Product Manager geeft de gegevens die ze nodig heeft en waar deze zich bevindt in haar playbook voor elke KPI.

## <a name="engagement-program-and-integration"></a>Betrokkenheidsprogramma en integratie
Voordat u een geavanceerde betrokkenheidsprogramma bouwt, moet de mobiele Project directeur die verantwoordelijk is voor het project een grondige kennis van hoe en wanneer u producten worden verbruikt door de gebruikers hebben.

Na drie maanden, de mobiele Project directeur voldoende gegevens om te verbeteren, zijn in-app-push notification-verkoop is verzameld. Hij leert die:

* De eerste aankoop gebeurt doorgaans op het niveau 14. Gedurende 90% van de gevallen is de aankoop nieuwe legendarische wapens voor $3.
* 80% van de gevallen aankopen van gebruikers die u hebt aangebracht kopen, doorgaan met het product en meer.
* Gebruikers die zijn geslaagd voor het niveau van 20, start te besteden aan meer dan $10/ week.
* Gebruikers vaak pakketten op niveau 16, 24 en 32 premium aanschaffen.

Dankzij deze analyse besluit de Mobile Project directeur specifieke push notification-reeksen te verhogen in app verkoop maken. Hij maakt drie push reeksen die hij aanroept: Welkom programma, verkoop-programma en niet-actieve programma. Raadpleeg voor meer informatie de [hulpmiddelen marketing en verkoop](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
