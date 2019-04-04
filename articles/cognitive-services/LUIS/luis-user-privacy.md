---
title: Exporteren en verwijderen van gegevens
titleSuffix: Azure Cognitive Services
description: Verwijderen van klantgegevens om ervoor te zorgen, privacy en naleving.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: eb125133138c6de173fdeb90024a9e5d961a929d
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895153"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exporteren en verwijderen van uw klantgegevens in Language Understanding (LUIS) in Cognitive Services

Verwijderen van klantgegevens om ervoor te zorgen, privacy en naleving. 

## <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag
Language Understanding Intelligent Service (LUIS) inhoud van de klant om de service blijft behouden, maar de gebruiker LUIS heeft volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens. Dit kan worden gedaan via het web LUIS [portal](luis-reference-regions.md) of de [(ook wel bekend als programmatische) API's ontwerpen van LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Inhoud van de klant is versleuteld opgeslagen in Microsoft regionale Azure storage en bevat:

- Account-inhoud van gebruikers die zijn verzameld tijdens de registratie
- Trainingsgegevens vereist voor het bouwen van modellen
- Query's van gebruikers die worden gebruikt door het logboek geregistreerd [actief leren](luis-concept-review-endpoint-utterances.md) voor het verbeteren van het model
  - Gebruikers kunnen query logboekregistratie uitschakelen door toe te voegen `&log=false` aan de aanvraag om details [hier](troubleshooting.md#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens
LUIS-gebruikers hebben volledig beheer om een gebruiker inhoud via de LUIS-webportal of de LUIS ontwerpen (ook bekend als Programmatic)-API's te verwijderen. De volgende tabel bevat koppelingen ter ondersteuning van met beide:

| | **Gebruikersaccount** | **Toepassing** | **Voorbeeld Utterance(s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **Portal** | [Koppeling](luis-concept-data-storage.md#delete-an-account) | [Koppeling](luis-how-to-start-new-app.md#delete-app) | [Koppeling](luis-concept-data-storage.md#utterances-in-an-intent) | [Actief leren uitingen](luis-how-to-review-endpoint-utterances.md#disable-active-learning)<br>[Geregistreerde uitingen](luis-concept-data-storage.md#disable-logging-utterances) |
| **API's** | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant
LUIS-gebruikers hebben volledig beheer tot de gegevens in de portal, maar deze moet worden geëxporteerd via de LUIS ontwerpen (ook wel bekend als programmatische) API's. De volgende tabel bevat koppelingen ter ondersteuning van met de uitvoer van de gegevens via het ontwerpen van LUIS (ook wel bekend als programmatische) API's:

| | **Gebruikersaccount** | **Toepassing** | **Utterance(s)** | **Query's door eindgebruikers** |
| --- | --- | --- | --- | --- |
| **API's** | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Koppeling](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>Locatie van actief leren

Om in te schakelen [actief leren](luis-how-to-review-endpoint-utterances.md#enable-active-learning), geregistreerde gebruikers uitingen, ontvangen op de gepubliceerde LUIS-eindpunten worden opgeslagen in de volgende Azure-regio's:

* [Europa](#europe)
* [Australië](#australia)
* [Verenigde Staten](#united-states)

Met uitzondering van actief leren gegevens (hieronder beschreven), LUIS volgt de [data storage procedures voor regionale services](http://azuredatacentermap.azurewebsites.net/). 

### <a name="europe"></a>Europa

De [eu.luis.ai](https://eu.luis.ai) portal en Europa ontwerpen (ook wel bekend als programmatische API's) worden gehost in Azure van Europa Geografie. Ondersteuning voor implementatie van de eindpunten aan de volgende Azure-geografieën de eu.luis.ai-portal en Europa ontwerpen (ook wel bekend als programmatische API's):

* Europa
* Frankrijk
* Verenigd Koninkrijk

Bij het implementeren van deze Azure-geografieën, wordt de uitingen ontvangen door het eindpunt van de eindgebruikers van uw app worden opgeslagen in Azure van Europa geografisch gebied voor actief leren. U kunt uitschakelen actief leren, Zie [uitschakelen actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Zie voor het beheren van opgeslagen uitingen [verwijderen utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 

### <a name="australia"></a>Australië

De [au.luis.ai](https://au.luis.ai) portal en Australië ontwerpen (ook wel bekend als programmatische API's) worden gehost in Azure Australië Geografie. Ondersteuning voor implementatie van de eindpunten aan de volgende Azure-geografieën de au.luis.ai-portal en Australië ontwerpen (ook wel bekend als programmatische API's):

* Australië

Bij het implementeren van deze Azure-geografieën, worden de uitingen ontvangen door het eindpunt van de eindgebruikers van uw app opgeslagen in de Azure Australië geografisch gebied voor actief leren. U kunt uitschakelen actief leren, Zie [uitschakelen actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Zie voor het beheren van opgeslagen uitingen [verwijderen utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 

### <a name="united-states"></a>Verenigde Staten

De [luis.ai](https://www.luis.ai) portal en de Verenigde Staten ontwerpen (ook wel bekend als programmatische API's) worden gehost in Azure Verenigde Staten Geografie. Ondersteuning voor implementatie van de eindpunten aan de volgende Azure-geografieën de luis.ai-portal en de Verenigde Staten ontwerpen (ook wel bekend als programmatische API's):

* Azure-geografieën niet wordt ondersteund door de Europa of Australië regio's ontwerpen

Bij het implementeren van deze Azure-geografieën, worden de uitingen ontvangen door het eindpunt van de eindgebruikers van uw app opgeslagen in de Verenigde Staten geografisch gebied van Azure voor actief leren. U kunt uitschakelen actief leren, Zie [uitschakelen actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Zie voor het beheren van opgeslagen uitingen [verwijderen utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS-regio's verwijzen naar](./luis-reference-regions.md)
