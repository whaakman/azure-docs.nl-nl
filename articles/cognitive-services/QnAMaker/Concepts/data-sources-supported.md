---
title: Gegevensbronnen ondersteund - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Gegevensbronnen die worden ondersteund
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: b888846056fd60f37cdb1da85904fa14ffe79a39
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345314"
---
# <a name="data-sources"></a>Gegevensbronnen 
QnA Maker kan automatisch vraag combinaties van algemene inhoud semi-gestructureerde indelingen zoals veelgestelde vragen en producthandleidingen extraheren. Inhoud kan ook worden toegevoegd aan de knowledge base van gestructureerde bestanden.

## <a name="plain-faq-pages"></a>Gewone Veelgestelde vragen over pagina 's
Dit is de meest voorkomende type van de pagina met veelgestelde vragen, waarin de antwoorden onmiddellijk volgt u de vragen in dezelfde pagina. 

![Eenvoudige pagina met veelgestelde vragen](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>Veelgestelde vragen over pagina's met sectie koppelingen 
In dit type pagina met veelgestelde vragen, vragen samen worden samengevoegd en zijn gekoppeld aan de antwoorden die zich in verschillende secties op dezelfde pagina.

 ![Pagina met sectie koppeling Veelgestelde vragen](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>Veelgestelde vragen over pagina's met koppelingen naar verschillende pagina 's 
Dit type pagina met veelgestelde vragen is vergelijkbaar met een sectie gekoppelde pagina met veelgestelde vragen, behalve dat de koppelingen naar een andere pagina omleiden. QnA Maker verkent alle gekoppelde pagina's om op te halen van de bijbehorende antwoorden.

 ![Veelgestelde vragen over de dieptekoppeling naar pagina](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Producthandleidingen

Meestal is een handmatige richtlijnen materiaal die wordt meegestuurd met een product. Zo kunt u de gebruiker instellen, gebruiken, onderhouden en oplossen van het product. Wanneer QnA Maker een handmatige verwerkt, pakt deze koppen en onderverdelingen als vragen en de volgende inhoud, zoals antwoorden. Een voorbeeld bekijken [hier](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> Uitpakken van werkt het beste op handleidingen die een tabel met inhoud en/of een indexpagina en een duidelijke structuur met hiërarchische koppen hebben.


## <a name="structured-data-format-through-file-upload"></a>De indeling gestructureerde gegevens via het bestand is geüpload

Gestructureerde bestanden zoals .tsv, XLSX met opgemaakte kolommen kunnen ook worden geüpload naar QnA Maker tijdens het maken van het knowledge base. U kunt ook uploaden bestanden van de **instellingen** tabblad van een knowledge base

| Vraag  | Antwoord  | Metagegevens                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | `Key1:Value1\|Key2:Value2` |
| Question2 | Answer2 |      `Key:Value`           |
Eventuele extra kolommen in het bronbestand worden genegeerd.

## <a name="structured-data-format-through-import"></a>De indeling gestructureerde gegevens via importeren
Importeren van een knowledge base, vervangt de inhoud van de bestaande knowledge base. Invoer vereist een gestructureerde .tsv-bestand met informatie van gegevensbron. Deze informatie helpt QnA Maker groep de combinaties van de vraag en het kenmerk ze naar een bepaalde gegevensbron.

| Vraag  | Antwoord  | Bron| Metagegevens                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1|`Key1:Value1\|Key2:Value2` |
| Question2 | Answer2 | Redactioneel|    `Key:Value`       |

## <a name="editorial"></a>Redactioneel
Als u geen bestaande inhoud voor het vullen van de knowledge base, kunt u ook toevoegen ze redactioneel in QnA Maker Knowledge base. Informatie over het bijwerken van uw knowledge base [hier](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een QnA Maker-service instellen](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Zie ook 

[QnA Maker-overzicht](../Overview/overview.md)