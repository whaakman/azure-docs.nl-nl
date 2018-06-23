---
title: Exporteren en verwijderen van klantgegevens - LUIS - cognitieve Azure-Services || Microsoft Docs
description: Verwijzing voor exporteren en verwijderen van gegevens van de klant van Language Understanding-service (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: v-geberr;
ms.openlocfilehash: f684b8ab875e2fbb774dc4a29bce25be41b24e6d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345762"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exporteren en verwijderen van uw klantgegevens in Language Understanding (LUIS) in cognitieve Services

## <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant aanvraag gegevensfuncties
Language Understanding Intelligent Service (LUIS) behouden klant inhoud om de service, maar de gebruiker LUIS heeft volledig beheer over weer te geven, exporteren en verwijderen van hun gegevens. Dit kan worden gedaan via het web LUIS [portal](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-regions) of de [LUIS programmatische API's](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Inhoud van de klant wordt opgeslagen in Microsoft regionale Azure-opslag versleuteld en omvat:

- Account gebruikersinhoud verzameld tijdens de registratie
- Trainingsgegevens vereist voor het bouwen van de modellen (dat wil zeggen bedoeling & entiteiten)
- Gebruikersquery vastgelegd tijdens runtime te verbeteren van de gebruiker modellen
  - Gebruikers kunnen queryregistratie uitschakelen door toe te voegen `&log=false` details op de aanvraag [hier](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-resources-faq#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens
LUIS gebruikers volledige toegang hebben tot de gebruikersinhoud van een, via de webportal LUIS of de LUIS programmatische API's verwijderen. De volgende tabel bevat koppelingen te helpen met beide:

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's voor eindgebruikers** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-account-settings) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exporteren van klantgegevens
LUIS gebruikers hebben volledig beheer om de gegevens in de portal weer te geven, maar deze moet worden geëxporteerd via de LUIS programmatische API's. De volgende tabel bevat koppelingen te helpen met de geëxporteerde gegevens via de LUIS programmatische API's:

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's voor eindgebruikers** |
| --- | --- | --- | --- | --- |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS regio's verwijzen naar](./luis-reference-regions.md)
