---
title: Wat is de insluitende Reader-API?
titleSuffix: Azure Cognitive Services
description: Meer informatie over de insluitende Reader-API.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: f84804fbad07357706e786fc449fce9e42a5ec4a
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688332"
---
# <a name="what-is-immersive-reader"></a>Wat is Insluitende lezer?

De [insluitende lezer](https://www.onenote.com/learningtools) is een inclusief ontworpen hulp programma dat bewezen technieken implementeert om de Lees vaardigheid van opkomende lezers, taal kennisers en mensen te verbeteren, zoals Dyslexia.

U kunt de Insluitende lezer gebruiken in uw webtoepassing door de SDK voor de Insluitende lezer te gebruiken.

## <a name="what-does-immersive-reader-do"></a>Wat doet insluitende lezer?

De insluitende lezer is ontworpen om het lezen van iedereen toegankelijk te maken.

* Hiermee wordt inhoud in een minimale Lees weergave weer gegeven

  ![Insluitende lezer](./media/immersive-reader.png)

* Afbeeldingen van veelgebruikte woorden weer geven

  ![Afbeeldings woordenlijst](./media/picture-dictionary.png)

* De naam van zelfstandige naam woorden, werk woorden, bijvoegingen en bewerkings parameters

  ![Spraak onderdelen](./media/parts-of-speech.png)

* Hiermee wordt uw inhoud hardop voor u gelezen

  ![Hardop voor lezen](./media/read-aloud.png)

* Zet uw inhoud om in een andere taal

  ![Vertaling](./media/translation.png)

* Onderbreekt woorden in letter grepen

  ![Syllabification](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>Hoe werkt insluitende lezer?

De insluitende lezer is een zelfstandige web-app die, wanneer deze wordt aangeroepen met behulp van de Java script SDK van de insluitende lezer, boven `iframe`op uw bestaande web-app wordt weer gegeven via een. Wanneer u de API aanroept om de insluitende lezer te starten, geeft u de inhoud op die u wilt weer geven in de insluitende lezer. Onze SDK verwerkt het maken en de stijl van `iframe` de en communicatie met de back-end van de insluitende lezer, die de inhoud verwerkt voor onderdelen van spraak, tekst naar spraak, vertaling, enzovoort.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met Insluitende lezer:

* Ga naar de [Snelstartgids](./quickstart.md)
* De insluitende [Lezer-SDK op github](https://github.com/microsoft/immersive-reader-sdk) verkennen
* Lees de [referentie](./reference.md) voor de insluitende lezer-SDK