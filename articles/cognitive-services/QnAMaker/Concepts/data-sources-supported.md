---
title: Gegevensbronnen ondersteund - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker kan automatisch vraag-antwoord paren extraheren uit algemene semi-gestructureerde inhoud indelingen, zoals veelgestelde vragen over en producthandleidingen. Inhoud kan ook worden toegevoegd aan de knowledge base uit gestructureerde bestanden.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/24/2018
ms.author: tulasim
ms.openlocfilehash: b9214d44061edad967212a1f904c2cdb6687dba2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039051"
---
# <a name="data-sources"></a>Gegevensbronnen 
QnA Maker kan automatisch vraag-antwoord paren extraheren uit algemene semi-gestructureerde inhoud indelingen, zoals veelgestelde vragen over en producthandleidingen. Inhoud kan ook worden toegevoegd aan de knowledge base uit gestructureerde bestanden.

## <a name="plain-faq-pages"></a>Plain Veelgestelde vragen over pagina 's
Dit is de meest voorkomende type van de pagina met veelgestelde vragen, waarin de antwoorden onmiddellijk de vragen in dezelfde pagina volgen. 

![Eenvoudige pagina met veelgestelde vragen](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>Veelgestelde vragen over pagina's met sectie koppelingen 
In dit type pagina met veelgestelde vragen, vragen bij elkaar worden opgeteld en zijn gekoppeld aan de antwoorden die zich in verschillende secties op dezelfde pagina.

 ![Pagina met sectie koppeling Veelgestelde vragen](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>Veelgestelde vragen over pagina's met koppelingen naar verschillende pagina 's 
Dit type van de pagina met veelgestelde vragen is vergelijkbaar met een sectie gekoppelde pagina met veelgestelde vragen, behalve dat de koppelingen naar een andere pagina omleiden. QnA Maker verkent de gekoppelde's om op te halen van de bijbehorende antwoorden.

 ![Veelgestelde vragen over de dieptekoppeling naar pagina](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Producthandleidingen

Een handmatige is doorgaans richtlijnen materiaal die wordt meegestuurd met een product. Hiermee wordt de gebruiker wilt instellen, gebruikt, onderhouden en oplossen van het product. Wanneer de QnA Maker een handmatige processen, pakt deze de koppen en subkoppen als vragen en de volgende inhoud als antwoord. Bekijk een voorbeeld [hier](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> Extractie werkt het beste op de handleidingen die een tabel met inhoud en/of een indexpagina en een duidelijke structuur met hiërarchische koppen hebben.


## <a name="structured-data-format-through-file-upload"></a>Indeling van gestructureerde gegevens via bestand uploaden

Gestructureerde bestanden zoals .tsv, .xlsx met opgemaakte kolommen kunnen ook met QnA Maker worden geüpload tijdens de ontwikkeling van knowledge base. U kunt bestanden ook uploaden de **instellingen** tabblad van een kennisdatabase

| Vraag  | Antwoord  | Metagegevens                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | ' Key1:Value1|Key2:Value2' |
| Question2 | Answer2 |      `Key:Value`           |
Eventuele extra kolommen in het bronbestand worden genegeerd.

## <a name="structured-data-format-through-import"></a>Indeling van gestructureerde gegevens via importeren
Importeren van een kennisdatabase wordt vervangen door de inhoud van de bestaande knowledge base. Invoer vereist een gestructureerde .tsv-bestand met informatie van de gegevensbron. Deze informatie helpt QnA Maker groep van de vraag-antwoord-paren en ze het kenmerk met een bepaalde gegevensbron.

| Vraag  | Antwoord  | Bron| Metagegevens                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1|' Key1:Value1|Key2:Value2' |
| Question2 | Answer2 | Redactioneel|    `Key:Value`       |

## <a name="editorial"></a>Redactioneel
Als u geen bestaande inhoud voor het vullen van de knowledge base, kunt u ook toevoegen ze redactioneel in QnA Maker Knowledge base. Meer informatie over het bijwerken van uw knowledge base [hier](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Persoonlijkheid aan antwoorden met chit chat Persona's toevoegen](../How-To/chit-chat-knowledge-base.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van QnA Maker](../Overview/overview.md)
