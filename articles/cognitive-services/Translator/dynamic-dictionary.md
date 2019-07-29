---
title: Dynamische woorden lijst-Translator Text-API
titleSuffix: Azure Cognitive Services
description: De functie voor dynamische woorden lijsten van het Translator Text-API gebruiken.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: f586702350b38c6d546767f64a8e2f6115b59817
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595288"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Een dynamische woorden lijst gebruiken

Als u al weet wat de vertaling is die u wilt Toep assen op een woord of een woord groep, kunt u deze als aantekeningen opgeven in de aanvraag. De dynamische woorden lijst is alleen veilig voor samengestelde samen stellingen, zoals eigen namen en product namen.

**Syntaxis**

< msTrans: Dictionary Translator = "vertaling van woord groep" > frase </msTrans: Dictionary >

**Vereiste**

* De `From` talen `To` en moeten verschillend zijn. 
* U moet de `From` para meter in uw API-Vertaal aanvraag toevoegen in plaats van de functie voor automatische detectie te gebruiken. 

**Voor beeld: nl-de:**

Bron invoer: Het woord < msTrans: woorden lijst vertaling\"=\"wordomatic > woord of zin </msTrans: Dictionary > een vermelding van een woorden lijst is.

Doel uitvoer: Das Wort "wordomatic" ist Wörterbucheintrag.

Deze functie werkt op dezelfde manier met en zonder HTML-modus.

De functie moet spaarzaam worden gebruikt. De juiste en veel betere manier om de vertaling aan te passen, is door aangepaste Translator te gebruiken. Aangepaste vertaler maakt volledig gebruik van context-en statistische kansen. Als u trainings gegevens hebt of kunt maken waarin uw werk of zin in de context wordt weer gegeven, krijgt u veel betere resultaten. Meer informatie over Custom Translator vindt u op [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).
