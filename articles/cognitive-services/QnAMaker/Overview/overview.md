---
title: Wat is QnA Maker?
titleSuffix: Azure Cognitive Services
description: Met QnA Maker kunt u een vraag- en antwoordservice maken op basis van uw semi-gestructureerde inhoud zoals FAQ-documenten of URL's en producthandleidingen.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: eba7355faf304721fdac4bfdb88f8a69ed41fcba
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038150"
---
# <a name="what-is-qna-maker"></a>Wat is QnA Maker?

Met [QnA Maker](https://qnamaker.ai) kunt u een vraag- en antwoordservice maken op basis van uw semi-gestructureerde inhoud zoals FAQ-documenten (veelgestelde vragen) of URL's en producthandleidingen. U kunt een model van vragen en antwoorden bouwen dat flexibel is voor vragen van gebruikers, en antwoorden biedt waarmee u een bot traint om ze op een natuurlijke, conversationele manier te gebruiken.

Dankzij een gebruikersvriendelijke grafische gebruikersinterface kunt u uw service maken, beheren, trainen en laten werken zonder dat u ontwikkelingservaring nodig hebt.

![Overzicht](../media/qnamaker-overview-learnabout/overview.png)

## <a name="highlights"></a>Hoogtepunten

- Een complete **no-code**-ervaring voor het [maken van een FAQ-bot](https://aka.ms/qnamaker-docs-create-faqbot).
- **Geen bandbreedtebeperking meer**. Betaal voor het hosten van de service en niet voor het aantal transacties. Bekijk de [prijzenpagina](https://aka.ms/qnamaker-docs-pricing) voor meer informatie.
- **Schaal volgens uw behoeften**. Kies de juiste SKU’s van de afzonderlijke componenten die bij uw scenario passen. Zie hoe u [capaciteit kiest](https://aka.ms/qnamaker-docs-capacity) voor uw QnA Maker-service.
- **Volledige gegevenscompliance**. De gegevens- en runtime-componenten worden geïmplementeerd in het Azure-abonnement van de gebruiker en binnen hun nalevingsgrens. Lees het gedeelte hieronder voor meer informatie.

## <a name="key-qna-maker-processes"></a>Belangrijke QnA Maker-processen

QnA Maker biedt twee belangrijke services voor uw gegevens:

* **Extractie**: gestructureerde vraag-antwoordgegevens worden geëxtraheerd uit semi-gestructureerde gegevensbronnen zoals veelgestelde vragen en producthandleidingen. Deze extractie wordt gedaan bij het creëren van de kennisbank. Maak [hier](https://aka.ms/qnamaker-docs-createkb) uw kennisbank.

* **Matching**: nadat uw kennisbank [is getraind en getest](https://aka.ms/qnamaker-docs-trainkb), [publiceert](https://aka.ms/qnamaker-docs-publishkb) u deze. Dit maakt een eindpunt mogelijk voor uw QnA Maker-kennisbank, dat u vervolgens kunt gebruiken in uw bot of app. Dit eindpunt accepteert een gebruikersvraag en reageert met de beste vraag/antwoord-match in de kennisbank, samen met een betrouwbaarheidsscore voor de match.

## <a name="qna-maker-architecture"></a>QnA Maker-architectuur

De QnA Maker-stack bestaat uit de volgende onderdelen:

1. **QnA Maker-beheerservices (besturingslaag)**: de managementervaring voor een QnA Maker-kennisbank, die de initiële creatie, het bijwerken, training en publicatie omvat. Deze activiteiten kunnen worden uitgevoerd via de [portal](https://qnamaker.ai) of de [beheer-API's](https://aka.ms/qnamaker-v4-apis). De beheerservices communiceren met de onderstaande runtime-component.

2. **QnA Maker-runtime (gegevenslaag)**: de gegevens en runtime worden in het Azure-abonnement van de gebruiker geïmplementeerd in een regio naar keuze. Klantvraag-/antwoordinhoud wordt opgeslagen in [Azure Search](https://azure.microsoft.com/services/search/)en de runtime wordt geïmplementeerd als een [app-service](https://azure.microsoft.com/services/app-service/). U kunt er eventueel ook voor kiezen om een ​​[Application Insights](https://azure.microsoft.com/services/application-insights/)-bron voor analyse te implementeren.

![Architectuur](../media/qnamaker-overview-learnabout/architecture.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een QnA Maker-service maken](../how-to/set-up-qnamaker-service-azure.md)
