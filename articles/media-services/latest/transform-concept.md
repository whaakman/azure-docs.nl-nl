---
title: Transformeert en taken in Azure mediaservices | Microsoft Docs
description: Wanneer u Media Services gebruikt, moet u een transformatiebestand om te beschrijven de voorschriften of specificaties voor het verwerken van uw video's maken. In dit artikel biedt een overzicht van wat transformatie is en het gebruik ervan.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: d256d87548d54951cb77beffb88bba26a1a3de49
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="transforms-and-jobs"></a>Transformaties en taken

## <a name="overview"></a>Overzicht 

De nieuwste versie van de Azure Media Services REST-API (v3) introduceert een nieuwe resource sjablonen werkstroom voor codering en/of het analyseren van video's, aangeroepen een **transformeren**. **Transformeert** kan worden gebruikt voor het configureren van algemene taken voor codering of analying video's. Elke **transformeren** wordt een eenvoudige taken voor het verwerken van uw bestanden video of audio-werkstroom beschreven. 

De **transformeren** object is het recept en een **taak** is de werkelijke aanvraag met Azure Media Services om toe te passen die **transformeren** naar een opgegeven invoer video of audio-inhoud. De **taak** bevat informatie zoals de locatie van de video invoer en de locatie voor de uitvoer. U kunt de locatie van uw video met opgeven: http (s) URL's, SAS-URL's of een pad naar bestanden lokaal of in Azure Blob-opslag. U kunt maximaal 100 transformaties in uw Azure Media Services-account en verzenden van taken onder deze transformaties. U kunt vervolgens zich abonneren op gebeurtenissen zoals wijzigingen in de taak de status, met behulp van meldingen, die rechtstreeks te met het raster voor Azure Event notification system integreren. 

Aangezien deze API wordt aangedreven door Azure Resource Manager, kunt u de Resource Manager-sjablonen maken en implementeren van transformaties in uw Media Services-account gebruiken. Toegangsbeheer op basis van rollen kan ook worden ingesteld op het niveau van de resource in deze API, zodat u toegang tot specifieke bronnen zoals transformaties vergrendelen.

## <a name="typical-workflow-and-example"></a>Voorbeeld van en gangbare werkstroom

1. Maak een transformatie 
2. Verzenden van taken onder deze transformatie 
3. Lijst met transformaties 
4. Een transformatie verwijderen als u niet van plan bent te gebruiken deze 'recept' in de toekomst. 

Stel dat u wilt uitpakken van het eerste frame van uw video's als een miniatuur – zijn de stappen die u wilt uitvoeren: 

1. Definieer de regel voor de verwerking – bijvoorbeeld, het eerste frame van de video gebruiken als de miniatuur 
2. Klik voor elke video die u als invoer voor de service hebt, moet u de service zien: 
    1. Waar vind ik die video en 
    2. Waar u de uitvoer – bijvoorbeeld de miniatuurafbeelding schrijven 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Stroom videobestanden](stream-files-dotnet-quickstart.md)
