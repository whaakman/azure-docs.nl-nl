---
title: Toevoegen van Azure Search naar Blob Storage | Microsoft Docs
description: Een index in code maken met behulp van de Azure Search HTTP REST-API.
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.openlocfilehash: 15469e8a2d28bdf00d6e8d8c9f823c51975ee90e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="searching-blob-storage-with-azure-search"></a>Blob-opslag doorzoeken met Azure Search

Zoeken in de verschillende typen inhoud opgeslagen in Azure Blob storage mag een moeilijk probleem kunt oplossen. U kunt echter indexeren en de inhoud van uw BLOB's zoeken in een paar muisklikken met behulp van Azure Search. Zoeken in de Blob-opslag is vereist voor het inrichten van een Azure Search-service. De verschillende Servicelimieten en Prijscategorieën van Azure Search kunnen u vinden op de [pagina met prijzen](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Wat is Azure Search?
[Azure Search](https://aka.ms/whatisazsearch) is een search-service waarmee u gemakkelijk voor ontwikkelaars om toe te voegen optreedt robuuste zoekopdracht in volledige tekst voor webtoepassingen en mobiele toepassingen. Als een service, Azure Search verwijdert de behoefte om een infrastructuur zoeken bij aanbieding te beheren een [uptime van 99,9% SLA](https://aka.ms/azuresearchsla).

## <a name="index-and-search-enterprise-document-formats"></a>Index en zoek de opmaak van de enterprise-documenten
Met ondersteuning voor [extractie document](https://aka.ms/azsblobindexer) in Azure Blob storage, kunt u de volgende inhoud indexeren:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

Door de tekst en metagegevens extraheren uit deze bestandstypen, kunt u meerdere indelingen met één query doorzoeken. 

## <a name="search-through-your-blob-metadata"></a>Het doorzoeken van de blobmetagegevens
Een veelvoorkomend scenario waarmee u gemakkelijk te sorteren via blobs van elk type inhoud is om te indexeren zowel aangepaste metagegevens en eigenschappen voor elke blob. Op deze manier wordt de informatie voor alle blobs ongeacht documenttype geïndexeerd. Vervolgens kunt u doorgaan met sorteren, te filteren en facet over alle inhoud van de Blob-opslag.

[Meer dat informatie over het indexeren van blob-metagegevens.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Zoeken naar afbeelding
Zoekopdracht in volledige tekst, meervoudige navigatie en sorteren mogelijkheden van Azure Search kunnen nu worden toegepast op de metagegevens van de installatiekopieën die zijn opgeslagen in blobs.

Als deze installatiekopieën vooraf worden verwerkt met behulp van de [Computer Vision-API](https://www.microsoft.com/cognitive-services/computer-vision-api) van cognitieve Services van Microsoft, is het mogelijk om de visual inhoud gevonden in elke installatiekopie inclusief OCR en handschriftherkenning te indexeren. We werken over het toevoegen van OCR en andere mogelijkheden voor de verwerking van de installatiekopie rechtstreeks naar Azure Search, als u geïnteresseerd in deze mogelijkheden kunnen bent een aanvraag indienen op onze [UserVoice](https://aka.ms/azsuv) of [Stuur ons een e-mail](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Index en zoek via JSON-blobs
Azure Search kan worden geconfigureerd voor het uitpakken van gestructureerde inhoud gevonden in de blobs die JSON bevatten. Azure Search kunt lezen JSON-blobs en de inhoud van het gestructureerde geparseerd in de juiste velden van een Azure Search-document. Azure Search kunt ook rekening houden met blobs die een matrix met JSON-objecten bevatten en elke element toewijzen aan een afzonderlijke Azure Search-document.

Bij het parseren van JSON kan niet op dit moment worden geconfigureerd via de portal. [Meer informatie over de JSON parseren in Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Snel starten
Azure Search kan worden toegevoegd aan BLOB's rechtstreeks vanuit de portal Blob-opslag-pagina.

![](./media/search-blob-storage-integration/blob-blade.png)

Klik op **toevoegen Azure Search** starten van een stroom kunt u een bestaande Azure Search-service selecteren of een nieuwe service maken. Als u een nieuwe service maakt, wordt u genavigeerd buiten portal ervaring voor uw opslagaccount. U kunt Ga terug naar de portal opslag-pagina en selecteer nogmaals het **toevoegen Azure Search** optie, waarin u de bestaande service kunt selecteren.

### <a name="next-steps"></a>Volgende stappen
Meer informatie over de Azure Blob-indexering voor zoeken in de volledige [documentatie](https://aka.ms/azsblobindexer).
