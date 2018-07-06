---
title: Exporteren en verwijderen van gegevens van de klant - LUIS - Azure Cognitive Services || Microsoft Docs
description: Referentie voor het exporteren en verwijderen van klantgegevens van Language Understanding-service (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: v-geberr;
ms.openlocfilehash: 8b4f9c17557b405bc5f4e78947952826724fc784
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865059"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exporteren en verwijderen van uw klantgegevens in Language Understanding (LUIS) in Cognitive Services

## <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag
Language Understanding Intelligent Service (LUIS) inhoud van de klant om de service blijft behouden, maar de gebruiker LUIS heeft volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens. Dit kan worden gedaan via het web LUIS [portal](luis-reference-regions.md) of de [programmatische LUIS-API's](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Inhoud van de klant is versleuteld opgeslagen in Microsoft regionale Azure storage en bevat:

- Account-inhoud van gebruikers die zijn verzameld tijdens de registratie
- Trainingsgegevens vereist voor het bouwen van de modellen (dat wil zeggen intentie en entiteiten)
- Query's van gebruikers aangemeld tijdens runtime voor het verbeteren van de gebruiker-modellen
  - Gebruikers kunnen query logboekregistratie uitschakelen door toe te voegen `&log=false` aan de aanvraag om details [hier](luis-resources-faq.md#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens
LUIS-gebruikers hebben volledig beheer over de gebruikersinhoud van een, via de LUIS-webportal of de LUIS-API's voor programmatische verwijderen. De volgende tabel bevat koppelingen ter ondersteuning van met beide:

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](luis-how-to-account-settings.md) | [Link](create-new-app.md#delete-app) | [Link](create-new-app.md#delete-app) | [Link](create-new-app.md#delete-app) |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant
LUIS-gebruikers hebben volledig beheer tot de gegevens in de portal, maar deze moet worden geëxporteerd via de LUIS-API's voor programmatische. De volgende tabel bevat koppelingen ter ondersteuning van met de uitvoer van de gegevens via de programmatische LUIS-API:

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **API's** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS-regio's verwijzen naar](./luis-reference-regions.md)
