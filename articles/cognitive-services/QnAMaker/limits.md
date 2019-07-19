---
title: Limieten en grenzen - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker heeft meta-limieten voor het delen van de knowledge base en de service. Het is belangrijk dat u uw knowledge base binnen de grenzen om te testen en publiceren.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 07/18/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b366352d19b3f1e03e32e5fbddf0cb2816fa1ba3
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320294"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>QnA Maker kennisdatabase limieten en grenzen

QnA Maker grenzen die hieronder worden aangegeven, zijn een combi natie van de [Azure Search prijs categorie limieten](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity) en de [QnA Maker prijs categorie limieten](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/). U moet beide sets limieten kennen om inzicht te krijgen in het aantal kennissen dat u per resource kunt maken en hoe groot elke kennis database kan groeien.

## <a name="knowledge-bases"></a>Kennis bases

Het maximum aantal kennis grondslagen is gebaseerd op de limieten van [Azure Search lagen](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity).

|**Azure Search tier** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maxi maal aantal gepubliceerde kennis grondslagen toegestaan|2|14|49|199|199|2,999|

 Als uw laag bijvoorbeeld 15 toegestane indexen heeft, kunt u 14 Knowledge bases publiceren (1 index per gepubliceerde kennis basis). De vijftiende index `testkb`,, wordt gebruikt voor alle kennis grondslagen voor ontwerpen en testen. 

## <a name="extraction-limits"></a>Extractie limieten

### <a name="maximum-number-of-files"></a>Maximum aantal bestanden

Het maximum aantal bestanden dat kan worden geëxtraheerd en de maximale bestands grootte is gebaseerd op uw **[QnA Maker prijs categorie limieten](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** .

### <a name="maximum-number-of-deep-links-from-url"></a>Maximum aantal diep gaande koppelingen van URL

Het maximum aantal diep gaande koppelingen dat kan worden verkend voor het uitpakken van QnAs van een URL-pagina is **20**.

## <a name="metadata-limits"></a>Limieten voor metagegevens

Het maximum aantal meta gegevens velden per Knowledge Base is gebaseerd op de limieten van uw **[Azure Search-laag](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)** .

|**Azure Search tier** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximale metagegevensvelden per QnA Maker-service (voor alle kB's)|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Limieten voor Knowledge Base-inhoud
Algemene beperkingen met betrekking tot de inhoud in het knowledge base:
* Lengte van antwoord tekst: 25,000
* Lengte van de vraag tekst: 1000
* Lengte van meta gegevens sleutel/waarde: 100
* Ondersteunde tekens voor naam van meta gegevens: Letters, cijfers en _  
* Ondersteunde tekens voor de meta gegevens waarde: Alle behalve: en | 
* Lengte van bestands naam: 200
* Ondersteunde bestandsindelingen: ".tsv", '.pdf', '.txt', ".docx", '.xlsx'.
* Maximum aantal alternatieve vragen: 300
* Maximum aantal antwoord paren vraag: Is afhankelijk van de gekozen limieten voor het **[Azure Search-niveau](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits)** . Een vraag-en-antwoord-paar is een document op uw Azure Search-index. 

## <a name="create-knowledge-base-call-limits"></a>Limieten voor Knowledge base-aanroep maken:
Deze vertegenwoordigen de limieten voor elk maken knowledge base-actie. dat wil zeggen, te klikken op *maken KB* of de CreateKnowledgeBase-API aan te roepen.
* Maximum aantal alternatieve vragen per antwoord: 300
* Maximum aantal Url's: 10
* Maximum aantal bestanden: 10

## <a name="update-knowledge-base-call-limits"></a>Knowledge base-aanroep limieten bijwerken
Dit zijn de limieten voor elke update-actie. dat wil zeggen, te klikken op *opslaan en trainen* of de UpdateKnowledgeBase-API aan te roepen.
* Lengte van elke bron naam: 300
* Maximum aantal alternatieve vragen dat is toegevoegd of verwijderd: 300
* Maximum aantal meta gegevens velden dat is toegevoegd of verwijderd: 10
* Maximum aantal Url's dat kan worden vernieuwd: 5

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het wijzigen van service lagen:

* [QnA Maker](how-to/upgrade-qnamaker-service.md#upgrade-qna-maker-management-sku): Als u meer bron bestanden of grotere documenten in uw Knowledge Base nodig hebt, moet u de prijs categorie van QnA Maker service upgraden naar uw huidige laag.
* [App service](how-to/upgrade-qnamaker-service.md#upgrade-app-service): Als uw Knowledge Base meer aanvragen van uw client-app nodig heeft, moet u de prijs categorie van uw app service bijwerken.
* [Azure Search](how-to/upgrade-qnamaker-service.md#upgrade-azure-search-service): Als u van plan bent een groot aantal Knowledge bases te hebben, moet u uw Azure Search service prijs categorie bijwerken.
