---
title: Microsoft Translator tekst API Dynamic woordenlijst | Microsoft Docs
description: Het gebruik van de functie dynamische woordenlijst van de API van Microsoft Translator tekst.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a18348c9786669ac41c4e149577d97cd631d5531
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345830"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-microsoft-translator-text-api"></a>Het gebruik van de functie dynamische woordenlijst van de API van Microsoft Translator tekst

Als u de vertaling die u wilt toepassen op een woord of woordgroep al weet, kunt u deze als aantekeningen in de aanvraag opgeven. Alleen is de dynamische woordenlijst voor samengestelde woorden zoals namen van de juiste en product veilig. 

**Syntaxis:** 

< mstrans:dictionary vertaling = 'vertaling van zinsnede' > woordgroep < / mstrans:dictionary >

**Voorbeeld: nl-nl:**

Bron van de invoer: het woord < mstrans:dictionary vertaling =\"wordomatic\"> woord of woordgroep < / mstrans:dictionary > is een dictionary-vermelding.

Uitvoer als doel: Das Wort 'wordomatic' ist een Wörterbucheintrag.

Deze functie werkt op dezelfde manier met en zonder HTML-modus. 

De functie moet spaarzaam worden gebruikt. De juiste en ver betere manier van het aanpassen van de vertaling is via de Microsoft Translator Hub. De Hub maakt volledig gebruik van de context en statistische kansen. Als u hebt of betaalbare trainingsgegevens waarin uw werk- of woordgroep in context maken, kunt u veel betere resultaten krijgt. U vindt meer informatie over de hub aan [ http://hub.microsofttranslator.com ](http://hub.microsofttranslator.com).

