---
title: Verkenning - Personalizer
titleSuffix: Azure Cognitive Services
description: Met het verkennen kan Personalizer om door te gaan met het leveren van goede resultaten, zelfs als het gedrag van gebruikers wordt gewijzigd. Kiezen van een instelling voor verkennen, is een zakelijke beslissing over het deel van de interactie van gebruikers om te verkennen met, om te verbeteren van het model.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: edjez
ms.openlocfilehash: ebb59b6bb7c36f4558b2bd63d2d55fa95823c4c3
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722478"
---
# <a name="exploration-and-exploitation"></a>Verkennen en misbruik

Met het verkennen kan Personalizer om door te gaan met het leveren van goede resultaten, zelfs als het gedrag van gebruikers wordt gewijzigd.

Wanneer Personalizer een positie gesprek ontvangt, wordt een RewardActionID die ofwel:
* Maakt gebruik van misbruik zodat deze overeenkomen met de meest waarschijnlijke gedrag van gebruikers op basis van het huidige model voor machine learning.
* Gegevensverkenning, komt niet overeen met de actie die de hoogste kans in de rang heeft gebruikt.

<!--
Returning the most probable action is called *exploit* behavior. Returning a different action is called *exploration*.
-->
Personalizer gebruikt momenteel een algoritme met de naam *epsilon greedy* om te verkennen. 

## <a name="choosing-an-exploration-setting"></a>Het kiezen van een instelling verkennen

Configureren van het percentage van het verkeer moet worden gebruikt voor verkennen in Azure portal **instellingen** -pagina voor Personalizer. Deze instelling bepaalt het percentage van de positie aanroepen die onderzoek uitvoeren. 

Personalizer bepaalt of verkennen of misbruik te maken met deze kans op elke aanroep van de positie. Dit is anders dan het gedrag van sommige A / B-frameworks die een behandeling op specifieke gebruikers-id's te vergrendelen.

## <a name="best-practices-for-choosing-an-exploration-setting"></a>Aanbevolen procedures voor het kiezen van een instelling verkennen

<!--
@edjez - you say what not to do, but make no recommendations of what **to** do. 
-->

Kiezen van een instelling voor verkennen, is een zakelijke beslissing over het deel van de interactie van gebruikers om te verkennen met, om te verbeteren van het model. 

Een instelling van nul wordt veel van de voordelen van Personalizer negatief maken. Met deze instelling kan Personalizer geen gebruikersinteracties gebruikt voor het detecteren van betere gebruikersinteracties. Dit leidt tot model stagnatie, afwijking en uiteindelijk lagere prestaties.

Een instelling die is te hoog worden de voordelen van de training van gebruikersgedrag negatief moet worden gemaakt. Instellen op 100% betekent een willekeurig toepassen van constante en elk geleerde gedrag van gebruikers kan niet van invloed op het resultaat.

Het is belangrijk om niet te wijzigen van de werking van de toepassing op basis van of u ziet als Personalizer verkennen of misbruik van. Dit zou leiden tot learning vooroordelen die uiteindelijk van de mogelijke prestaties verlagen zou.

## <a name="next-steps"></a>Volgende stappen

[Bekrachtigingen](concepts-reinforcement-learning.md) 