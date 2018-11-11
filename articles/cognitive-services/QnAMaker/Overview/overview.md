---
title: Wat is QnA Maker?
titleSuffix: Azure Cognitive Services
description: QnA Maker is een cloud-API-service die aangepaste machine learning intelligence toepast op een gebruikersvraag in natuurlijke taal om daarop het beste antwoord te kunnen geven.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: bd859183a13e0f8a21cdd2eabb464b718e949464
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212213"
---
# <a name="what-is-qna-maker"></a>Wat is QnA Maker?

QnA Maker is een cloudgebaseerde API-service voor het maken van een vraag- en antwoordlaag over uw gegevens in gespreksstijl. 

Met QnA Maker kunt u een kennisbank (KB) maken van uw semi-gestructureerde inhoud, zoals URL's naar FAQ's (veelgestelde vragen), producthandleidingen, ondersteuningsdocumenten en aangepaste vragen en antwoorden. De QnA Maker-service beantwoordt de in natuurlijke taal gestelde vragen van uw gebruikers door deze te koppelen aan de best mogelijke antwoorden uit de QnA's in uw kennisbank.

In de eenvoudig te gebruiken [webportal](https://qnamaker.ai) kunt u uw service maken, beheren, trainen en publiceren zonder ontwikkelaarservaring. Wanneer de service is gepubliceerd op een eindpunt, kan een clienttoepassing zoals een chatbot de conversatie met een gebruiker beheren om vragen te krijgen en erop te reageren met antwoorden. 

![Overzicht](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Belangrijke QnA Maker-processen

QnA Maker biedt twee belangrijke services voor uw gegevens:

* **Extractie**: gestructureerde vraag-antwoordgegevens worden geëxtraheerd uit gestructureerde en semi-gestructureerde [gegevensbronnen](../Concepts/data-sources-supported.md), zoals veelgestelde vragen en producthandleidingen. Deze extractie kan worden gedaan bij het [maken](https://aka.ms/qnamaker-docs-createkb) van de KB of later, als onderdeel van het bewerkingsproces.

* **Matching**: nadat uw kennisbank [is getraind en getest](https://aka.ms/qnamaker-docs-trainkb), [publiceert](https://aka.ms/qnamaker-docs-publishkb) u deze. Dit maakt een eindpunt mogelijk voor uw QnA Maker-knowledge base, die u vervolgens kunt gebruiken in uw bot of clientapp. Dit eindpunt accepteert een gebruikersvraag en reageert met het beste antwoord in de knowledge base, samen met een betrouwbaarheidsscore voor de match.

```JSON
{
    "answers": [
        {
            "questions": [
                "How do I share a knowledge base with other?"
            ],
            "answer": "Sharing works at the level of a QnA Maker service, i.e. all knowledge bases in the services will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base)how to collaborate on a knowledge base.",
            "score": 70.95,
            "id": 4,
            "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
            "metadata": []
        }
    ]
}

```

## <a name="qna-maker-architecture"></a>QnA Maker-architectuur

De architectuur van QnA Maker bestaat uit de volgende twee onderdelen:

1. **QnA Maker-beheerservices**: de beheerervaring voor een QnA Maker-knowledge base die alle fasen omvat, van het maken tot en met het bijwerken, trainen en publiceren. Deze activiteiten kunnen worden uitgevoerd via de [portal](https://qnamaker.ai) of de [beheer-API's](https://aka.ms/qnamaker-v4-apis). 

2. De **QnA Maker-gegevens en -runtime**: deze is geïmplementeerd in uw Azure-abonnement in de door u opgegeven regio. Uw KB-inhoud wordt opgeslagen in [Azure Search](https://azure.microsoft.com/services/search/) en het eindpunt wordt geïmplementeerd als een [app-service](https://azure.microsoft.com/services/app-service/). U kunt er ook voor kiezen om een ​​[Application Insights](https://azure.microsoft.com/services/application-insights/)-bron te implementeren voor analyse.

![Architectuur](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Belangrijke functies van de service

- Een complete **no-code**-ervaring voor het [maken van een FAQ-bot](https://aka.ms/qnamaker-docs-create-faqbot).
- **Er is geen netwerkbeperking voor voorspellingen**. Betaal voor het hosten van de service en niet voor het aantal transacties. Bekijk de [prijzenpagina](https://aka.ms/qnamaker-docs-pricing) voor meer informatie.
- **Schalen naar behoefte**. Kies de juiste SKU’s van de afzonderlijke componenten die bij uw scenario passen. Zie hoe u [capaciteit kiest](https://aka.ms/qnamaker-docs-capacity) voor uw QnA Maker-service.
- **Volledige gegevenscompliance**. De onderdelen van de voorspellingsservice worden geïmplementeerd in uw Azure-abonnement en binnen hun nalevingsgrens.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een QnA Maker-service maken](../how-to/set-up-qnamaker-service-azure.md)
