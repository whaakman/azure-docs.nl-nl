---
title: Zoeken in volledige tekst toevoegen aan Azure Blob Storage - Azure Search
description: Tekstinhoud verkennen in Azure Blob-opslag voor Azure Search indexeren in code met behulp van de HTTP REST-API.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/01/2019
author: mgottein
manager: cgronlun
ms.author: magottei
ms.custom: seodec2018
ms.openlocfilehash: b7e7ecd2a82a8d64967288def9c6ede7a292f72a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759388"
---
# <a name="searching-blob-storage-with-azure-search"></a>Blob-opslag doorzoeken met Azure Search

Doorzoeken op de verschillende typen inhoud die zijn opgeslagen in Azure Blob-opslag is een probleem lastig om op te lossen. U kunt echter indexeren en zoeken van de inhoud van uw Blobs in een paar klikken met behulp van Azure Search. Zoeken in Blob-opslag is vereist voor het inrichten van een Azure Search-service. De verschillende Servicelimieten en -Prijscategorieën van Azure Search kunnen u vinden op de [pagina met prijzen](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Wat is Azure Search?
[Azure Search](https://aka.ms/whatisazsearch) is een search-service waarmee u eenvoudig voor ontwikkelaars om toe te voegen robuuste zoeken in volledige tekst ervaringen voor web- en mobiele toepassingen. Een service, Azure Search elimineert u de noodzaak voor het beheren van een zoekinfrastructuur bij aanbieding een [uptime van 99,9% SLA](https://aka.ms/azuresearchsla).

## <a name="index-and-search-enterprise-document-formats"></a>Indexeren en zoeken in de opmaak van de enterprise-documenten
Met ondersteuning voor [document extractie](https://aka.ms/azsblobindexer) in Azure Blob-opslag, kunt u de volgende inhoud indexeren:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

Door de tekst en metagegevens extraheren uit deze bestandstypen, kunt u zoeken in meerdere indelingen met een eenvoudige query uitvoeren. 

## <a name="search-through-your-blob-metadata"></a>De blobmetagegevens doorzoeken
Een veelvoorkomend scenario waarmee u eenvoudig om te sorteren via blobs van elk type inhoud is om te indexeren van zowel de aangepaste metagegevens als de eigenschappen voor elke blob. Op deze manier wordt informatie voor alle blobs worden geïndexeerd, ongeacht het documenttype. Vervolgens kunt u doorgaan om te sorteren, filteren en facet voor alle inhoud van de Blob-opslag.

[Meer dat informatie over het indexeren van blob-metagegevens.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Afbeeldingen zoeken
Zoeken in volledige tekst, meervoudige navigatie en sorteren mogelijkheden van Azure Search kunnen nu worden toegepast op de metagegevens van de installatiekopieën die zijn opgeslagen in blobs.

Cognitieve zoekopdrachten omvat installatiekopie verwerking vaardigheden zoals [optische tekenherkenning (OCR)](cognitive-search-skill-ocr.md) en identificatie van [visuele kenmerken](cognitive-search-skill-image-analysis.md) die maken het mogelijk om te indexeren van de visuele inhoud die is gevonden in elk afbeelding.

## <a name="index-and-search-through-json-blobs"></a>Index en zoek via JSON-blobs
Azure Search kan worden geconfigureerd om uit te pakken gestructureerde inhoud gevonden in de blobs die JSON bevatten. Azure Search kan lezen JSON-blobs en parseren van de gestructureerde inhoud naar de juiste velden van een Azure Search-document. Azure Search kan ook rekening houden met blobs die een matrix met JSON-objecten bevatten en toewijzen van elk element in een afzonderlijk Azure Search-document.

JSON parseren is niet op dit moment worden geconfigureerd via de portal. [Meer informatie over JSON parseren in Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Snel starten
Azure Search kan worden toegevoegd aan BLOB's rechtstreeks vanuit de portal Blob storage-pagina.

![](./media/search-blob-storage-integration/blob-blade.png)

Klik op **Azure Search toevoegen** om te starten van een stroom kunt u een bestaande Azure Search-service selecteren of een nieuwe service maken. Als u een nieuwe service maakt, wordt u genavigeerd buiten uw Storage-account-portal-ervaring. U kunt Ga terug naar de portal op de pagina opslag en opnieuw selecteren de **Azure Search toevoegen** optie, waarin u de bestaande service kunt selecteren.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de indexeerfunctie Azure Search Blob in de volledige [documentatie](https://aka.ms/azsblobindexer).
