---
title: Exporteren en verwijderen van gegevens
titleSuffix: Azure Cognitive Services
description: Language Understanding Intelligent Service (LUIS) inhoud van de klant om de service blijft behouden, maar de gebruiker LUIS heeft volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens. Dit kan worden gedaan via de LUIS-webportal of de programmatische LUIS-API.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: add526d8c09d2aa557ddd645e9016cb9c3b225a3
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328194"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exporteren en verwijderen van uw klantgegevens in Language Understanding (LUIS) in Cognitive Services

## <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag
Language Understanding Intelligent Service (LUIS) inhoud van de klant om de service blijft behouden, maar de gebruiker LUIS heeft volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens. Dit kan worden gedaan via het web LUIS [portal](luis-reference-regions.md) of de [programmatische LUIS-API's](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Inhoud van de klant is versleuteld opgeslagen in Microsoft regionale Azure storage en bevat:

- Account-inhoud van gebruikers die zijn verzameld tijdens de registratie
- Trainingsgegevens vereist voor het bouwen van de modellen (dat wil zeggen intentie en entiteiten)
- Query's van gebruikers aangemeld tijdens runtime voor het verbeteren van de gebruiker-modellen
  - Gebruikers kunnen query logboekregistratie uitschakelen door toe te voegen `&log=false` aan de aanvraag om details [hier](troubleshooting.md#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens
LUIS-gebruikers hebben volledig beheer over de gebruikersinhoud van een, via de LUIS-webportal of de LUIS-API's voor programmatische verwijderen. De volgende tabel bevat koppelingen ter ondersteuning van met beide:

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](luis-how-to-account-settings.md) | [Link](luis-how-to-start-new-app.md#delete-app) | [Link](luis-how-to-start-new-app.md#delete-app) | [Link](luis-how-to-start-new-app.md#delete-app) |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant
LUIS-gebruikers hebben volledig beheer tot de gegevens in de portal, maar deze moet worden geëxporteerd via de LUIS-API's voor programmatische. De volgende tabel bevat koppelingen ter ondersteuning van met de uitvoer van de gegevens via de programmatische LUIS-API:

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS-regio's verwijzen naar](./luis-reference-regions.md)
