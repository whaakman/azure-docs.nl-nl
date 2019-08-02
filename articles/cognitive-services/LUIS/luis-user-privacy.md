---
title: Gegevens exporteren & verwijderen-LUIS
titleSuffix: Azure Cognitive Services
description: Verwijder klant gegevens om te zorgen voor privacy en naleving.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: e234f88d6e735f33be253cacb373baef63c605c0
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559996"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exporteren en verwijderen van uw klantgegevens in Language Understanding (LUIS) in Cognitive Services

Verwijder klant gegevens om te zorgen voor privacy en naleving. 

## <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag
Language Understanding Intelligent Service (LUIS) inhoud van de klant om de service blijft behouden, maar de gebruiker LUIS heeft volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens. Dit kan worden gedaan via de LUIS- [](luis-reference-regions.md) webportal of de api's voor het [ontwerpen van Luis (ook wel bekend als programmatisch)](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Inhoud van de klant is versleuteld opgeslagen in Microsoft regionale Azure storage en bevat:

- Account-inhoud van gebruikers die zijn verzameld tijdens de registratie
- Trainings gegevens die nodig zijn voor het bouwen van de modellen
- Geregistreerde gebruikers query's die worden gebruikt door [actief leren](luis-concept-review-endpoint-utterances.md) om het model te verbeteren
  - Gebruikers kunnen query logboekregistratie uitschakelen door toe te voegen `&log=false` aan de aanvraag om details [hier](troubleshooting.md#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens
LUIS-gebruikers hebben volledige controle om gebruikers inhoud te verwijderen, hetzij via de LUIS-webportal of de LUIS authoring (ook wel programmatische-Api's). De volgende tabel bevat koppelingen ter ondersteuning van met beide:

| | **Gebruikersaccount** | **Toepassing** | **Voor beeld van Utterance (s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](luis-concept-data-storage.md#delete-an-account) | [Link](luis-how-to-start-new-app.md#delete-app) | [Link](luis-concept-data-storage.md#utterances-in-an-intent) | [Actieve Learning uitingen](luis-how-to-review-endpoint-utterances.md#disable-active-learning)<br>[Geregistreerde uitingen](luis-concept-data-storage.md#disable-logging-utterances) |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant
LUIS-gebruikers hebben volledige controle over het weer geven van de gegevens op de portal, maar moeten worden geëxporteerd via de Api's voor LUIS ontwerpen (ook wel bekend als programmatisch). De volgende tabel bevat koppelingen voor het exporteren van gegevens via de Api's van LUIS authoring (ook wel bekend als programmatisch):

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>Locatie van actief leren

Om [actief leren](luis-how-to-review-endpoint-utterances.md#enable-active-learning)in te scha kelen, worden de geregistreerde uitingen van gebruikers die zijn ontvangen op de gepubliceerde Luis-eind punten, opgeslagen in de volgende Azure-geografies:

* [Europa](#europe)
* [Australië](#australia)
* [Verenigde Staten](#united-states)

Met uitzonde ring van actieve leer gegevens (hieronder beschreven), volgt LUIS de [gegevensopslag methoden voor regionale Services](https://azuredatacentermap.azurewebsites.net/). 

### <a name="europe"></a>Europa

De [EU.Luis.ai](https://eu.luis.ai) -Portal en Europa-ontwerping (ook wel programmatische api's genoemd) worden gehost in de Europa-wereld van Azure. De eu.luis.ai-Portal en Europa-ontwerp (ook wel programmatische Api's genoemd) ondersteunen de implementatie van eind punten naar de volgende Azure-geografi:

* Europa
* Frankrijk
* Verenigd Koninkrijk

Wanneer u implementeert in deze Azure-grafieken, wordt de uitingen die door het eind punt van de eind gebruikers van uw app worden ontvangen, opgeslagen in de Europa-wereld van Azure voor actief onderwijs. U kunt actief leren uitschakelen. Zie voor het [uitschakelen van actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Zie [Utterance verwijderen](luis-how-to-review-endpoint-utterances.md#delete-utterance)om opgeslagen uitingen te beheren. 

### <a name="australia"></a>Australië

De [au.Luis.ai](https://au.luis.ai) -Portal en Australië-ontwerping (ook wel programmatische api's genoemd) worden gehost in de geografische regio van Azure. De au.luis.ai-Portal en Australië-ontwerping (ook wel programmatische Api's genoemd) ondersteunen de implementatie van eind punten naar de volgende Azure-geografies:

* Australië

Wanneer u implementeert in deze Azure-grafieken, wordt de uitingen die door het eind punt van de eind gebruikers van uw app worden ontvangen, opgeslagen in de geografische regio van Azure voor actief onderwijs. U kunt actief leren uitschakelen. Zie voor het [uitschakelen van actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Zie [Utterance verwijderen](luis-how-to-review-endpoint-utterances.md#delete-utterance)om opgeslagen uitingen te beheren. 

### <a name="united-states"></a>Verenigde Staten

De [Luis.ai](https://www.luis.ai) -portal en Verenigde Staten-ontwerp (ook wel programmatische api's genoemd) worden gehost in de Verenigde Staten Geografie van Azure. De luis.ai-Portal en Verenigde Staten-ontwerp (ook wel programmatische Api's genoemd) ondersteunen de implementatie van eind punten naar de volgende Azure-geografi:

* Azure-geografi niet ondersteund door de ontwerp regio's Europa of Australië

Wanneer u implementeert in deze Azure-grafieken, wordt de uitingen die door het eind punt van de eind gebruikers van uw app worden ontvangen, opgeslagen in de Verenigde Staten Geografie van Azure voor actief onderwijs. U kunt actief leren uitschakelen. Zie voor het [uitschakelen van actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Zie [Utterance verwijderen](luis-how-to-review-endpoint-utterances.md#delete-utterance)om opgeslagen uitingen te beheren. 


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS-regio's verwijzen naar](./luis-reference-regions.md)
