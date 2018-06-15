---
title: Limieten voor QnA Maker - cognitieve Azure-Services | Microsoft Docs
description: QnA Maker limieten
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 4d2bafad08ffab76743cb802733a5d2f01a97f06
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "35345948"
---
# <a name="qna-maker-limits"></a>QnA Maker limieten
Uitgebreide lijst met beperkingen in QnA Maker.

## <a name="knowledge-bases"></a>Kennis basissen

* Maximum aantal kennis basissen op basis van [limieten voor Azure Search-laag](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Azure Search-laag** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**HD S3**|
|---|---|---|---|---|---|----|
|Maximum aantal gepubliceerde kennis basissen toegestaan (Max indexen--1 (gereserveerd voor de test)|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Limieten voor uitpakken
* Maximum aantal bestanden dat kan worden uitgepakt en de maximum bestandsgrootte: Zie [QnAMaker prijzen](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* Maximum aantal deep-koppelingen die voor de extractie van QnAs kunnen worden benaderd vanuit Veelgestelde vragen over het HTML-pagina's: 20

## <a name="metadata-limits"></a>Limieten voor metagegevens
* Maximum aantal metagegevensvelden per knowledge base op basis van [limieten voor Azure Search-laag](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Azure Search-laag** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**HD S3**|
|---|---|---|---|---|---|----|
|Maximale metagegevensvelden per QnA Maker-service (in alle kB)|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Inhoud Knowledge Base-limieten
Algemene beperkingen met betrekking tot de inhoud in het knowledge base:
* Lengte van antwoordtekst: 250000
* Lengte van de vraagtekst: 1000
* Lengte van de metagegevens van sleutel/waarde-tekst: 100
* Ondersteunde tekens voor de naam van de metagegevens: letters, cijfers en _  
* Ondersteunde tekens voor metagegevenswaarde: alles behalve: en | 
* Lengte van de bestandsnaam: 200
* Ondersteunde bestandsindelingen: '.tsv', '.pdf', '.txt', ".docx", '.xlsx'.
* Maximum aantal alternatieve vragen: 100
* Maximum aantal vraag combinaties: afhankelijk van de [Azure Search laag](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) gekozen 

## <a name="create-knowledge-base-call-limits"></a>Knowledge base-aanroep grenzen maken:
Deze vertegenwoordigen de limieten voor elk maken kennisdatabase actie; dat wil zeggen, te klikken op *KB maken* of de CreateKnowledgeBase-API aanroept.
* Maximum aantal alternatieve vragen per antwoord: 100
* Maximum aantal URL's: 10
* Maximum aantal bestanden: 10

## <a name="update-knowledge-base-call-limits"></a>Knowledge base-aanroep limieten bijwerken
Dit zijn de limieten voor elke update-actie; dat wil zeggen, te klikken op *opslaan en trainen* of de UpdateKnowledgeBase-API aanroept.
* Lengte van de bronnaam van elke: 300
* Maximum aantal vragen alternatieve vragen toegevoegd of verwijderd: 100
* Maximum aantal metagegevensvelden toegevoegd of verwijderd: 10
* Maximum aantal URL's die kunnen worden vernieuwd: 5
