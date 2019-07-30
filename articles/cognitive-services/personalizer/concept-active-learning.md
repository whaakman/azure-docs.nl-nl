---
title: Actief leren-persoonlijker
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: 8c1579be3d11ae14ca45ee861de2d4f705e5d62c
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663713"
---
# <a name="active-learning-and-learning-policies"></a>Actief leren en leren beleid 

Wanneer uw toepassing de classificatie-API aanroept, ontvangt u een rang schikking van de inhoud. Bedrijfs logica kan deze positie gebruiken om te bepalen of de inhoud moet worden weer gegeven voor de gebruiker. Wanneer u de gerangschikte inhoud weergeeft, is dit een _actieve_ positie gebeurtenis. Wanneer uw toepassing deze geclassificeerde inhoud niet weergeeft, is dat een niet- _actieve_ positie gebeurtenis. 

De gegevens van de actieve positie gebeurtenis worden teruggestuurd naar Personaler. Deze informatie wordt gebruikt om het model door te lopen via het huidige leer beleid.

## <a name="active-events"></a>Actieve gebeurtenissen

Actieve gebeurtenissen moeten altijd worden weer gegeven aan de gebruiker en de belonings oproep moet worden geretourneerd om de learning-lus te sluiten. 

### <a name="inactive-events"></a>Inactieve gebeurtenissen 

Inactieve gebeurtenissen mogen het onderliggende model niet wijzigen, omdat de gebruiker geen kans heeft gekregen te kiezen uit de gerangschikte inhoud.

## <a name="dont-train-with-inactive-rank-events"></a>Niet trainen met inactieve positie gebeurtenissen 

Voor sommige toepassingen moet u mogelijk de Rank API aanroepen zonder te weten of de resultaten door de toepassing worden weer gegeven aan de gebruiker. 

Dit gebeurt wanneer:

* U kunt een bepaalde gebruikers interface vooraf renderen die de gebruiker al dan niet kan zien. 
* Uw toepassing maakt mogelijk gebruik van een voorspellende personalisatie waarbij rang gesprekken worden gedaan met minder realtime-context en de uitvoer ervan kan of mogen niet worden gebruikt door de toepassing. 

### <a name="disable-active-learning-for-inactive-rank-events-during-rank-call"></a>Actief leren uitschakelen voor inactieve rangorde gebeurtenissen tijdens een classificatie oproep

Als u automatisch leren wilt uitschakelen, roept `learningEnabled = False`u de rang orde aan.

Leren voor een inactieve gebeurtenis wordt impliciet geactiveerd als u een beloning voor de positie verzendt.

## <a name="learning-policies"></a>Leer beleid

Met het leer beleid wordt het specifieke *Hyper parameters* van de model training bepaald. Twee modellen van dezelfde gegevens, getraind op basis van een ander leer beleid, gedragen zich op een andere manier.

### <a name="importing-and-exporting-learning-policies"></a>Trainings beleid importeren en exporteren

U kunt Learning-beleids bestanden importeren en exporteren vanuit het Azure Portal. Zo kunt u bestaande beleids regels opslaan, testen, vervangen en deze in uw broncode beheer archiveren als artefacten voor toekomstige Naslag informatie en controle.

### <a name="learning-policy-settings"></a>Instellingen voor het leer beleid

De instellingen in het **trainings beleid** zijn niet bedoeld om te worden gewijzigd. Wijzig de instellingen alleen wanneer u begrijpt hoe deze van invloed zijn op persoonlijkere. Als u instellingen wijzigt zonder deze kennis, zullen er neven effecten ontstaan, waaronder het ongeldig maken van Personaler-modellen.

### <a name="comparing-effectiveness-of-learning-policies"></a>Effectiviteit van het leer beleid vergelijken

U kunt vergelijken hoe verschillende leer beleid zou worden uitgevoerd op eerdere gegevens in persoonlijke logboeken door [offline](concepts-offline-evaluation.md)-evaluaties uit te voeren.

[Upload uw eigen trainings beleid](how-to-offline-evaluation.md) om te vergelijken met het huidige leer beleid.

### <a name="discovery-of-optimized-learning-policies"></a>Detectie van geoptimaliseerde leer beleid

Personaler kan een meer geoptimaliseerd leer beleid maken wanneer een [offline-evaluatie](how-to-offline-evaluation.md)wordt uitgevoerd. Een meer geoptimaliseerd leer beleid, dat wordt weer gegeven om betere beloningen te hebben in een offline-evaluatie, levert betere resultaten op wanneer u online gebruikt in Personaler.

Nadat u een geoptimaliseerd trainings beleid hebt gemaakt, kunt u dit rechtstreeks op persoonlijker Toep assen zodat het huidige beleid direct wordt vervangen, of u kunt het opslaan voor verdere evaluatie en in de toekomst besluiten of u dit later wilt negeren, opslaan of Toep assen.