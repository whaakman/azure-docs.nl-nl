---
title: Limieten en grenzen - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker heeft meta-limieten voor het delen van de knowledge base en de service. Het is belangrijk dat u uw knowledge base binnen de grenzen om te testen en publiceren.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 51e494101b13b3020f79c9acfc4d853cee995d7b
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911419"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>QnA Maker kennisdatabase limieten en grenzen
Uitgebreide lijst met limieten voor QnA Maker.

## <a name="knowledge-bases"></a>Knowledge Bases

* Maximum aantal knowledge bases op basis van [categorielimieten Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Azure Search tier** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximum aantal gepubliceerde knowledge bases toegestaan|2|14|49|199|199|2,999|

 Als de laag 15 toegestane indexen heeft, kunt u bijvoorbeeld 14 knowledge bases (1-index per gepubliceerd knowledge base) publiceren. De vijftiende index `testkb`, wordt gebruikt voor alle knowledge bases voor het ontwerpen en testen. 

## <a name="extraction-limits"></a>Extractie limieten
* Maximum aantal bestanden die kunnen worden geëxtraheerd en maximale bestandsgrootte: Zie [QnAMaker prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)
* Maximum aantal deep-koppelingen die kunnen worden benaderd voor extractie van vragen en antwoorden supereenvoudig van veelgestelde vragen over het HTML-pagina's: 20

## <a name="metadata-limits"></a>Limieten voor metagegevens
* Maximum aantal metagegevensvelden per knowledge base op basis van [categorielimieten Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Azure Search tier** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximale metagegevensvelden per QnA Maker-service (voor alle kB's)|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Limieten voor Knowledge Base-inhoud
Algemene beperkingen met betrekking tot de inhoud in het knowledge base:
* De lengte van de antwoordtekst: 25,000
* De lengte van de vraagtekst: 1000
* De lengte van de metagegevens van sleutel/waarde-tekst: 100
* Ondersteunde tekens in voor de naam voor de metagegevens: Letters, cijfers en _  
* Ondersteunde tekens in voor de metagegevenswaarde: Overal behalve in: en | 
* De lengte van bestandsnaam: 200
* Ondersteunde bestandsindelingen: ".tsv", '.pdf', '.txt', ".docx", '.xlsx'.
* Maximum aantal alternatieve vragen: 100
* Maximum aantal paren met vraag-antwoord: Afhankelijk van de [Azure Search tier](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits) gekozen 

## <a name="create-knowledge-base-call-limits"></a>Limieten voor Knowledge base-aanroep maken:
Deze vertegenwoordigen de limieten voor elk maken knowledge base-actie. dat wil zeggen, te klikken op *maken KB* of de CreateKnowledgeBase-API aan te roepen.
* Maximum aantal alternatieve vragen per antwoord: 100
* Maximum aantal URL's: 10
* Maximum aantal bestanden: 10

## <a name="update-knowledge-base-call-limits"></a>Knowledge base-aanroep limieten bijwerken
Dit zijn de limieten voor elke update-actie. dat wil zeggen, te klikken op *opslaan en trainen* of de UpdateKnowledgeBase-API aan te roepen.
* De lengte van de bronnaam van elke: 300
* Maximum aantal alternatieve vragen toegevoegd of verwijderd: 100
* Maximum aantal metagegevensvelden toegevoegd of verwijderd: 10
* Maximum aantal URL's die kunnen worden vernieuwd: 5

## <a name="next-steps"></a>Volgende stappen

Informatie over wanneer en hoe u Servicelagen wijzigen:

* [QnA Maker](how-to/upgrade-qnamaker-service.md#upgrade-qna-maker-management-sku): wanneer u nodig hebt om meer vragen en antwoorden in uw knowledge base, buiten de huidige laag upgrade uw prijscategorie QnA Maker-service.
* [Search](how-to/upgrade-qnamaker-service.md#upgrade-app-service) : wanneer uw knowledge base moet fungeren meer aanvragen van uw client-app, upgrade van uw appservice-prijscategorie.
* [Appservice](how-to/upgrade-qnamaker-service.md#upgrade-azure-search-service): wanneer u van plan bent om veel knowledge bases, upgrade van uw Azure Search-service prijscategorie.