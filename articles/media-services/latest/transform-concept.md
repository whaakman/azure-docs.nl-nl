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
ms.openlocfilehash: b755e0573098d3dbed1bea18a40af634be609f76
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34272077"
---
# <a name="transforms-and-jobs"></a>Transformaties en taken

## <a name="overview"></a>Overzicht 

De nieuwste versie van de Azure Media Services REST-API (v3) introduceert een nieuwe resource sjablonen werkstroom voor codering en/of het analyseren van video's, aangeroepen een **transformeren**. **Transformeert** kan worden gebruikt voor het configureren van algemene taken voor codering of analying video's. Elke **transformeren** wordt een eenvoudige taken voor het verwerken van uw bestanden video of audio-werkstroom beschreven. 

De **transformeren** object is het recept en een **taak** is de werkelijke aanvraag met Azure Media Services om toe te passen die **transformeren** naar een opgegeven invoer video of audio-inhoud. De **taak** bevat informatie zoals de locatie van de invoervideo en de locatie voor de uitvoer. U kunt de locatie van uw video met opgeven: http (s) URL's, SAS-URL's of een pad naar bestanden lokaal of in Azure Blob-opslag. U kunt maximaal 100 transformaties in uw Azure Media Services-account en verzenden van taken onder deze transformaties. U kunt vervolgens zich abonneren op gebeurtenissen zoals wijzigingen in de taak de status, met behulp van meldingen, die rechtstreeks te met het raster voor Azure Event notification system integreren. 

Aangezien deze API wordt aangedreven door Azure Resource Manager, kunt u de Resource Manager-sjablonen maken en implementeren van transformaties in uw Media Services-account gebruiken. Toegangsbeheer op basis van rollen kan ook worden ingesteld op het niveau van de resource in deze API, zodat u toegang tot specifieke bronnen zoals transformaties vergrendelen.

## <a name="transform-definition"></a>Definitie van transformatie

De volgende tabel ziet u de eigenschappen van de transformatie en geeft de definities.

|Naam|Type|Beschrijving|
|---|---|---|
|Id|tekenreeks|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|tekenreeks|De naam van de resource.|
|Properties.created |tekenreeks|De UTC-datum en tijd waarop de transformatie is gemaakt, in ' jjjj-MM-ssZ ' indeling.|
|Properties.Description |tekenreeks|Een optionele uitgebreide beschrijving van de transformatie.|
|properties.lastModified |tekenreeks|De UTC-datum en tijd waarop de transformatie laatst is bijgewerkt, in ' jjjj-MM-ssZ ' indeling.|
|Properties.outputs |TransformOutput]|Een matrix met een of meer TransformOutputs die moet worden gegenereerd door de transformatie.|
|type|tekenreeks|Het type van de resource.|

Zie voor de volledige definitie [transformeert](https://docs.microsoft.com/rest/api/media/transforms).

## <a name="job-definition"></a>Taakdefinitie

De volgende tabel ziet u de eigenschappen van taak en geeft de definities.

|Naam|Type|Beschrijving|
|---|---|---|
|Id|tekenreeks|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|tekenreeks|De naam van de resource.|
|Properties.created |tekenreeks|De UTC-datum en tijd waarop de transformatie is gemaakt, in ' jjjj-MM-ssZ ' indeling.|
|Properties.Description |tekenreeks|Een optionele uitgebreide beschrijving van de taak.|
|properties.lastModified |tekenreeks|De UTC-datum en tijd waarop de transformatie laatst is bijgewerkt, in ' jjjj-MM-ssZ ' indeling.|
|Properties.outputs |JobOutput []: [] JobOutputAsset |De uitvoer voor de taak.|
|Properties.Priority |Prioriteit |Prioriteit waarmee de taak moet worden verwerkt. Taken met een hogere prioriteit worden verwerkt voordat taken met lagere prioriteit. Als dat niet is ingesteld, de standaardwaarde is normaal.
|Properties.state |JobState |De huidige status van de taak.
|type|tekenreeks|Het type van de resource.|

Zie voor de volledige definitie [taken](https://docs.microsoft.com/rest/api/media/jobs).

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
