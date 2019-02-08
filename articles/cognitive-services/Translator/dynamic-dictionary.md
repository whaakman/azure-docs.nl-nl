---
title: Dynamische Dictionary - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Het gebruik van de functie dynamische woordenlijst van de Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 6140361d3cffaf7938aa8f24b6f0c3d27784af88
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55876008"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Het gebruik van de functie dynamische woordenlijst van de Translator Text-API

Als u de vertaling die u wilt toepassen op een woord of woordgroep al kent, kunt u deze opgeven als aantekeningen in de aanvraag. Alleen is de dynamische-woordenlijst voor samenstellingen, zoals de namen van de juiste en product veilig.

**Syntaxis:**

< mstrans:dictionary vertaling = "vertalingen van woorden" > woordgroep < / mstrans:dictionary >

**Voorbeeld: nl-nl:**

Invoer van de bron: Het woord < mstrans:dictionary vertaling =\"wordomatic\"> woord of woordgroep < / mstrans:dictionary > is een dictionary-vermelding.

Doeluitvoer: DAS Wort "wordomatic" is een Wörterbucheintrag.

Deze functie werkt op dezelfde manier met en zonder HTML-modus.

De functie moet spaarzaam worden gebruikt. De juiste en veel beter manier voor het aanpassen van de vertaling is met behulp van aangepaste Translator. Aangepaste Translator maakt volledig gebruik van context en statistische kansen. Als u hebt of trainingsgegevens waarin uw werk- of woordgroep in context kunt maken, kunt u veel betere resultaten krijgt. U vindt meer informatie over aangepaste Translator op [ http://aka.ms/CustomTranslator ](https://aka.ms/CustomTranslator).
