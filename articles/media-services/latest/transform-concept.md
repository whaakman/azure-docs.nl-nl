---
title: Transformaties en taken in Azure mediaservices | Microsoft Docs
description: Wanneer u Media Services gebruikt, moet u om een transformatie om te beschrijven van de regels of de specificaties voor het verwerken van uw video's te maken. In dit artikel biedt een overzicht van wat transformatie is en het gebruik ervan.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 214d4d3d11255e417f3df1e5f6e648b2a30225ea
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377305"
---
# <a name="transforms-and-jobs"></a>Transformaties en taken

## <a name="overview"></a>Overzicht 

De nieuwste versie van de Azure Media Services REST-API (v3) introduceert een nieuwe resource sjablonen werkstroom voor codering en/of het analyseren van de video's, met de naam een **transformeren**. **Transformeert** kan worden gebruikt voor het configureren van algemene taken voor het coderen of analying video's. Elke **transformeren** beschrijft een eenvoudige werkstroom van taken voor het verwerken van uw video- of audio-bestanden. 

De **transformeren** object is het recept en een **taak** is van de werkelijke aanvraag voor Azure Media Services om toe te passen die **transformeren** aan een bepaalde invoer video of audio-inhoud. De **taak** bevat informatie zoals de locatie van de invoervideo en de locatie voor de uitvoer. U kunt de locatie van uw video met opgeven: http (s) URL's, SAS-URL's of een pad op naar bestanden lokaal of in Azure Blob-opslag. U kunt maximaal 100 transformaties in uw Azure Media Services-account hebt en het verzenden van taken onder deze transformaties. U kunt vervolgens met de abonneren op gebeurtenissen zoals wijzigingen in de taak de status, met behulp van meldingen, die rechtstreeks met de Azure Event Grid-melding-systeem integreren. 

Aangezien deze API wordt aangestuurd door Azure Resource Manager, kunt u Resource Manager-sjablonen maken en implementeren van transformaties in Media Services-account gebruiken. Op rollen gebaseerd toegangsbeheer kunt ook instellen op het resourceniveau van de in deze API, zodat u kunt toegang tot bepaalde resources, zoals transformaties vergrendelen.

## <a name="transform-definition"></a>Definitie van transformatie

De volgende tabel ziet u de transformatie-eigenschappen en hun definities biedt.

|Naam|Type|Beschrijving|
|---|---|---|
|Id|tekenreeks|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|tekenreeks|De naam van de resource.|
|Properties.created |tekenreeks|De UTC-datum en tijd waarop de transformatie is gemaakt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.Description |tekenreeks|Een uitgebreide beschrijving van de transformatie.|
|properties.lastModified |tekenreeks|De UTC-datum en tijd wanneer de transformatie het laatst is bijgewerkt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.outputs |TransformOutput]|Een matrix met een of meer TransformOutputs die de transformatie te genereren.|
|type|tekenreeks|Het type van de resource.|

Zie voor de definitie van de volledige [transformeert](https://docs.microsoft.com/rest/api/media/transforms).

## <a name="job-definition"></a>Jobdefinitie

De volgende tabel ziet u de eigenschappen van de taak en biedt de definities.

|Naam|Type|Beschrijving|
|---|---|---|
|Id|tekenreeks|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|tekenreeks|De naam van de resource.|
|Properties.created |tekenreeks|De UTC-datum en tijd waarop de transformatie is gemaakt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.Description |tekenreeks|Een uitgebreide beschrijving van de taak.|
|properties.lastModified |tekenreeks|De UTC-datum en tijd wanneer de transformatie het laatst is bijgewerkt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.outputs |JobOutput []: JobOutputAsset] |De uitvoer voor de taak.|
|Properties.Priority |Prioriteit |De prioriteit die de taak moet worden verwerkt. Taken met een hogere prioriteit worden verwerkt voordat jobs met lagere prioriteit. Als dat niet is ingesteld, de standaardwaarde is normaal.
|Properties.state |JobState |De huidige status van de taak.
|type|tekenreeks|Het type van de resource.|

Zie voor de definitie van de volledige [taken](https://docs.microsoft.com/rest/api/media/jobs).

## <a name="typical-workflow-and-example"></a>Gebruikelijke werkstroom en voorbeeld

1. Maak een transformatie 
2. Verzenden van taken onder deze transformatie 
3. Lijst met transformaties 
4. Een transformatie te verwijderen als u niet van plan bent te gebruiken van deze 'recept"in de toekomst. 

Stel dat u wilt extraheren van het eerste beeld van al uw video's als een miniatuurafbeelding: zijn de stappen die u moet uitvoeren: 

1. Definieer de regel voor de verwerking van – bijvoorbeeld, het eerste frame van de video gebruiken als de miniatuur 
2. Klik voor elke video die u als invoer voor de service hebt, moet u de service zien: 
    1. Waar vind ik die video en 
    2. Waar u de uitvoer – bijvoorbeeld, de miniatuurafbeelding schrijven 

## <a name="next-steps"></a>Volgende stappen

[Stream-video 's](stream-files-dotnet-quickstart.md)
