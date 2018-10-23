---
title: Gegevensbronnen ondersteund - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker worden vraag-antwoord paren automatisch geëxtraheerd uit semi-gestructureerde inhoud zoals veelgestelde vragen over, producthandleidingen, richtlijnen, voor ondersteuningsdocumenten en beleidsregels die zijn opgeslagen als webpagina's, PDF-bestanden of bestanden van Microsoft Word-document. Inhoud kan ook worden toegevoegd aan de knowledge base van gestructureerde QnA inhoudsbestanden.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/25/2018
ms.author: tulasim
ms.openlocfilehash: 36d74c7fba472956d57344977de79a4cbfd2cf02
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648510"
---
# <a name="data-sources-for-qna-maker-content"></a>Gegevensbronnen voor QnA Maker-inhoud

QnA Maker worden vraag-antwoord paren automatisch geëxtraheerd uit semi-gestructureerde inhoud zoals veelgestelde vragen over, producthandleidingen, richtlijnen, voor ondersteuningsdocumenten en beleidsregels die zijn opgeslagen als webpagina's, PDF-bestanden of bestanden van Microsoft Word-document. Inhoud kan ook worden toegevoegd aan de knowledge base van gestructureerde QnA inhoudsbestanden. 

De onderstaande tabel bevat een overzicht van de typen inhoud en bestandsindelingen die worden ondersteund door de QnA Maker.

|Brontype|Inhoudstype| Voorbeelden|
|--|--|--|
|URL|Veelgestelde vragen (vaste, met de secties of met een startpagina onderwerpen)|[Veelgestelde vragen over de gewone](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), [Veelgestelde vragen over met koppelingen](https://www.microsoft.com/software-download/faq), [Veelgestelde vragen over met onderwerpen startpagina](https://support.microsoft.com/products/windows?os=windows-10)|
|PDF / DOC-BESTAND|Veelgestelde vragen over handmatige Product, Brochures, document, Folder-beleid, ondersteuning handleiding gestructureerde QnA, enzovoort.|[Gestructureerd QnA.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Bot%20Service%20Sample%20FAQ.docx), [voorbeeld Product Manual.pdf](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf), [voorbeeld semi-structured.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx), [wit paper.pdf voorbeeld](https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-stack-wortmann-bring-the-power-of-the-public-cloud-into-your-data-center/Azure_Stack_Wortmann_Bring_the_Power_of_the_Public_Cloud_into_Your_Data_Center.pdf)|
|Excel|Gestructureerd QnA-bestand (met inbegrip van RTF, HTML ondersteuning)|[Voorbeeld QnA FAQ.xls](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|Gestructureerd QnA-bestand|[Voorbeeld chit-chat.tsv](https://raw.githubusercontent.com/Microsoft/BotBuilder-PersonalityChat/master/CSharp/Datasets/Queries_Responses_Friendly_QnAMaker.tsv)|

## <a name="faq-urls"></a>Veelgestelde vragen over URL 's

Veelgestelde vragen over webpagina's in 3 verschillende vormen kunt ondersteuning voor QnA Maker: Plain Veelgestelde vragen over pagina's, veelgestelde vragen over pagina's met koppelingen, veelgestelde vragen over pagina's met een startpagina onderwerpen.

### <a name="plain-faq-pages"></a>Plain Veelgestelde vragen over pagina 's

Dit is de meest voorkomende type van de pagina met veelgestelde vragen, waarin de antwoorden onmiddellijk de vragen in dezelfde pagina volgen. 

Hieronder volgt een voorbeeld van een eenvoudige pagina met veelgestelde vragen:

![Eenvoudige pagina met veelgestelde vragen](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Veelgestelde vragen over pagina's met koppelingen 

In dit type pagina met veelgestelde vragen, vragen bij elkaar worden opgeteld en antwoorden die in andere gedeelten van dezelfde pagina of in verschillende pagina's zijn gekoppeld.

Hieronder volgt een voorbeeld van een pagina met koppelingen in de secties die zich op dezelfde pagina met veelgestelde vragen:

 ![Pagina met sectie koppeling Veelgestelde vragen](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>Veelgestelde vragen over pagina's met een startpagina onderwerpen

Dit type Veelgestelde vragen over heeft een startpagina bij met de onderwerpen waarin elk onderwerp is een koppeling naar de relevante vragen en antwoorden supereenvoudig in een andere pagina. Hier wordt de gekoppelde's om op te halen van de bijbehorende vragen en antwoorden verkend, QnA Maker.

Hieronder volgt een voorbeeld van een pagina met veelgestelde vragen waar een startpagina onderwerpen koppelingen naar Veelgestelde vragen over secties in verschillende pagina's bevat. 

 ![Veelgestelde vragen over de dieptekoppeling naar pagina](../media/qnamaker-concepts-datasources/topics-faq.png) 


## <a name="pdf-doc-files"></a>PDF / DOC-bestand

QnA Maker kunt semi-gestructureerde inhoud in een PDF-bestand of document bestand verwerken, en deze converteren naar vragen en antwoorden supereenvoudig. Een goede-bestand dat goed kan worden geëxtraheerd is een waar inhoud wordt georganiseerd in een gestructureerde vorm en wordt weergegeven in de goed gedefinieerde secties. De secties kunnen verder worden onderverdeeld in subsecties of subonderwerpen. Extractie werkt het beste op documenten waarvoor een duidelijke structuur met hiërarchische koppen.

QnA Maker geeft secties en subsecties en relaties in het bestand op basis van visuele aanwijzingen, zoals lettertype, grootte, tekenstijl nummering, kleuren, enzovoort. Semi-gestructureerde PDF-bestand of document bestanden mogelijk handleidingen, veelgestelde vragen over de richtlijnen, beleid, Brochures, folders en vele andere typen bestanden. Hieronder ziet u enkele documentbeveiligingsrisico voorbeeld van deze bestanden.

### <a name="product-manuals"></a>Producthandleidingen

Een handmatige is doorgaans richtlijnen materiaal die wordt meegestuurd met een product. Hiermee wordt de gebruiker wilt instellen, gebruikt, onderhouden en oplossen van het product. Wanneer de QnA Maker een handmatige processen, pakt deze de koppen en subkoppen als vragen en de volgende inhoud als antwoord. Bekijk een voorbeeld [hier](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

Hieronder volgt een voorbeeld van een handmatige met een index-pagina en hiërarchische inhoud

 ![Handmatige product-voorbeeld](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> Extractie werkt het beste op de handleidingen die een tabel met inhoud en/of een indexpagina en een duidelijke structuur met hiërarchische koppen hebben.

### <a name="brochures-guidelines-papers-and-other-files"></a>Brochures, richtlijnen, documenten en andere bestanden

Nog vele andere typen documenten kunnen ook worden verwerkt voor het genereren van QA-paren, mits ze een duidelijke structuur en indeling hebben. Deze omvatten: Brochures, richtlijnen, rapporten, wit papier, wetenschappelijke documenten, beleid, boeken, enzovoort. Bekijk een voorbeeld [hier](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Hieronder volgt een voorbeeld van een semi-gestructureerde doc, zonder een index:

 ![Azure Blob-opslag semi-gestructureerde doc-bestand](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>Gestructureerde QnA-Document

De notatie voor gestructureerde-antwoorden op beveiligingsvragen in DOC-bestand is in de vorm van wisselende vragen en antwoorden per regel één vraag per regel gevolgd door een antwoord in de volgende regel, zoals hieronder wordt weergegeven: 

```text
Question1

Answer1

Question2

Answer2
```

Hieronder volgt een voorbeeld van een gestructureerde QnA word-document:

 ![Gestructureerde QnA-document](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>Gestructureerde *TXT*, *TSV* en *XLS* bestanden

Vragen en antwoorden supereenvoudig in de vorm van gestructureerde *.txt*, *.tsv* of *xls* bestanden kunnen ook worden geüpload naar de QnA Maker te maken of uitbreiden van een kennisdatabase.  Dit kunnen bestaan uit tekst zonder opmaak of inhoud in de RTF- of HTML-code kunnen hebben. 

| Vraag  | Antwoord  | Metagegevens                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Eventuele extra kolommen in het bronbestand worden genegeerd.

Hieronder volgt een voorbeeld van een gestructureerde QnA *xls* bestand met de HTML-inhoud:

 ![Gestructureerde QnA in excel](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>Indeling van gestructureerde gegevens via importeren

Importeren van een kennisdatabase wordt vervangen door de inhoud van de bestaande knowledge base. Invoer vereist een gestructureerde .tsv-bestand met informatie van de gegevensbron. Deze informatie helpt QnA Maker groep van de vraag-antwoord-paren en ze het kenmerk met een bepaalde gegevensbron.

| Vraag  | Antwoord  | Bron| Metagegevens                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Redactioneel|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Redactioneel in knowledge base toevoegen

Als u geen bestaande inhoud voor het vullen van de knowledge base, kunt u vragen en antwoorden supereenvoudig redactioneel toevoegen in de QnA Maker Knowledge base. Meer informatie over het bijwerken van uw knowledge base [hier](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een QnA Maker-service instellen](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van QnA Maker](../Overview/overview.md)
