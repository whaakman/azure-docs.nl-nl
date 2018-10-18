---
title: Overzicht van de Web Language Model API
titleSuffix: Azure Cognitive Services
description: Web Language Model API in Microsoft Cognitive Services biedt de meest geavanceerde hulpprogramma's voor de verwerking van natuurlijke taal.
services: cognitive-services
author: piyushbehre
manager: cgronlun
ms.service: cognitive-services
ms.component: web-language-model
ms.topic: overview
ms.date: 08/12/2016
ms.author: pibehre
ROBOTS: NOINDEX
ms.openlocfilehash: 79b126fc33175b7cd6df96ab07cd7b726d6065a7
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389935"
---
# <a name="what-is-the-web-language-model-api-preview"></a>Wat is de Web Language Model API? (Preview)

> [!IMPORTANT]
> De preview van Web Language Model is op 9 augustus 2018 uit gebruik genomen. We raden u aan [Azure Machine Learning-tekstanalysemodules](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) te gebruiken voor tekstverwerking en -analyse.

De Web Language Model API van Microsoft is een op REST gebaseerde cloudservice die de meest geavanceerde hulpprogramma's voor de verwerking van natuurlijke taal biedt. Met behulp van deze API kan uw toepassing alle mogelijkheden van big data benutten door middel van taalmodellen die zijn getraind met corpora op webniveau en die door Bing zijn verzameld binnen het en-US-taalgebied.

Deze gestroomlijnde back-off N-gram-taalmodellen, die Markovketens tot orde 5 ondersteunen, zijn getraind met de volgende corpora:

- Hoofdtekst van webpagina's
- Titeltekst van webpagina's
- Ankertekst van webpagina's
- Querytekst voor zoeken op web

De Web Language Model API ondersteunt vier opzoekbewerkingen:

1. Gecombineerde (log10) waarschijnlijkheid van een reeks woorden.
2. Voorwaardelijke (log10) waarschijnlijkheid van één woord bij een reeks voorgaande woorden.
3. Lijst met woorden (aanvullingen) waarvan de kans groot is dat ze volgen op een gegeven reeks woorden.
4. Woordafbreking van tekenreeksen die geen spaties bevatten.

## <a name="getting-started"></a>Aan de slag

1. Abonneren op de service.
2. Download de [SDK](https://www.github.com/microsoft/cognitive-weblm-windows).
3. Voer de SDK-voorbeeldcode uit.
4. Raadpleeg de [API-referentie](https://westus.dev.cognitive.microsoft.com/docs/services/55de9ca4e597ed1fd4e2f104) voor volledige details over de eindpunten, met inbegrip van codefragmenten in diverse talen.

## <a name="underlying-technology"></a>Onderliggende technologie

Het volgende document bevat informatie over het ontwikkelen van deze taalmodellen en moet worden vermeld in onderzoekspublicaties die van deze service gebruikmaken:

- [Een overzicht van corpus en toepassingen voor Microsoft Web N-gram](http://research.microsoft.com/apps/pubs/default.aspx?id=130762), NAACL HLT 2010

Klik [hier](https://academic.microsoft.com/#/search?iq=And%28Ty%3D'0'%2CRId%3D2145833060%29&q=papers%20citing%20an%20overview%20of%20microsoft%20web%20n%20gram%20corpus%20and%20applications&filters=&from=0&sort=0) voor een huidige lijst van documenten waarin naar dit werk wordt verwezen.
