---
title: Limieten en grenzen - QnA Maker
titleSuffix: Azure Cognitive Services
description: Uitgebreide lijst met limieten voor QnA Maker.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 76024266a8ae3eb03e910e140184a07ae363e881
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49384939"
---
# <a name="qna-maker-limits"></a>QnA Maker-limieten
Uitgebreide lijst met limieten voor QnA Maker.

## <a name="knowledge-bases"></a>Knowledge Bases

* Maximum aantal knowledge bases op basis van [categorielimieten Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Azure Search tier** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximum aantal gepubliceerde knowledge bases toegestaan (maximum aantal indexen--1 (gereserveerd voor test)|2|14|49|199|199|2999|

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
* Lengte van de antwoordtekst: 250000
* Lengte van de vraagtekst: 1000
* Lengte van de metagegevens van sleutel/waarde-tekst: 100
* Ondersteunde tekens voor de naam van de metagegevens: letters, cijfers en _  
* Ondersteunde tekens voor de metagegevenswaarde: alles behalve: en | 
* Lengte van bestandsnaam: 200
* Ondersteunde bestandsindelingen: ".tsv", '.pdf', '.txt', ".docx", '.xlsx'.
* Maximum aantal alternatieve vragen: 100
* Maximum aantal paren met vraag-antwoord: afhankelijk van de [Azure Search tier](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits) gekozen 

## <a name="create-knowledge-base-call-limits"></a>Limieten voor Knowledge base-aanroep maken:
Deze vertegenwoordigen de limieten voor elk maken knowledge base-actie. dat wil zeggen, te klikken op *maken KB* of de CreateKnowledgeBase-API aan te roepen.
* Maximum aantal alternatieve vragen per antwoord: 100
* Maximum aantal URL's: 10
* Maximum aantal bestanden: 10

## <a name="update-knowledge-base-call-limits"></a>Knowledge base-aanroep limieten bijwerken
Dit zijn de limieten voor elke update-actie. dat wil zeggen, te klikken op *opslaan en trainen* of de UpdateKnowledgeBase-API aan te roepen.
* Lengte van de bronnaam van elke: 300
* Maximum aantal alternatieve vragen toegevoegd of verwijderd: 100
* Maximum aantal metagegevensvelden toegevoegd of verwijderd: 10
* Maximum aantal URL's die kunnen worden vernieuwd: 5
