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
ms.date: 02/03/2019
ms.author: juliako
ms.openlocfilehash: e84f74fe4678a65a33c9cc728f290e7c905b2261
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55743732"
---
# <a name="transforms-and-jobs"></a>Transformaties en taken
 
Gebruik [transformeert](https://docs.microsoft.com/rest/api/media/transforms) het configureren van algemene taken voor het coderen of video's analyseren. Elke **transformeren** beschrijft een recept of een workflow van taken voor het verwerken van uw video- of audio-bestanden. Een enkele transformatie kunt meer dan één regel toepassen. Een transformatie kan bijvoorbeeld opgeven dat elke video worden gecodeerd naar een MP4-bestand op een bepaalde bitrate en dat een miniatuurafbeelding van het eerste frame van de video worden gegenereerd. U zou een TransformOutput-vermelding voor elke regel die u wilt opnemen in uw transformatie toevoegen. In Media Services-account met behulp van de Media Services v3-API, of een van de gepubliceerde SDK's kunt u transformaties maken. De Media Services v3 die API van Azure Resource Manager wordt gestuurd, zodat u ook Resource Manager-sjablonen kunt maken en implementeren voor gegevensstromen in Media Services-account. Op rollen gebaseerd toegangsbeheer kan worden gebruikt om toegang tot transformaties vergrendelen.

De Update-bewerking op de [transformeren](https://docs.microsoft.com/rest/api/media/transforms) entiteit bestemd is voor aanbrengen in de beschrijving of de prioriteiten van de onderliggende TransformOutputs wijzigingen. Het verdient aanbeveling dat dergelijke updates worden uitgevoerd wanneer alle lopende taken zijn voltooid. Als u van plan bent te herschrijven het recept, moet u een nieuwe transformatie te maken.

Een [taak](https://docs.microsoft.com/rest/api/media/jobs) is van de werkelijke aanvraag voor Azure Media Services om toe te passen de **transformeren** aan een bepaalde invoer video of audio-inhoud. Zodra de transformatie is gemaakt, kunt u taken met behulp van Media Services-API's of een van de gepubliceerde SDK's kunt indienen. De **taak** bevat informatie zoals de locatie van de video-invoer en de locatie voor de uitvoer. U kunt de locatie van uw video met invoer opgeven: HTTPS-URL's, SAS-URL's of [activa](https://docs.microsoft.com/rest/api/media/assets). De voortgang en status van taken kunnen worden verkregen door de bewaking van gebeurtenissen met Event Grid. Zie voor meer informatie, [bewaken van gebeurtenissen via EventGrid](job-state-events-cli-how-to.md).

De Update-bewerking op de [taak](https://docs.microsoft.com/rest/api/media/jobs) entiteit kan worden gebruikt om te wijzigen de *beschrijving*, en de *prioriteit* eigenschappen nadat de taak is verzonden. Een wijziging in de *prioriteit* eigenschap werkt alleen als de taak nog steeds in een in de wachtrij staat is. Als de taak verwerking is gestart of is voltooid, heeft prioriteit wijzigen geen effect.

> [!NOTE]
> Eigenschappen van **transformeren** en **taak** die van de datum/tijd zijn altijd in UTC-notatie zijn.

## <a name="typical-workflow"></a>Standaardwerkstroom

1. Een transformatie maken 
2. Verzenden van taken onder deze transformatie 
3. Lijst met transformaties 
4. Een transformatie te verwijderen als u niet van plan bent om deze te gebruiken in de toekomst. 

### <a name="example"></a>Voorbeeld

Stel dat u wilt extraheren van het eerste beeld van al uw video's als een miniatuurafbeelding: zijn de stappen die u moet uitvoeren: 

1. Het recept of de regel voor het verwerken van uw video's definiëren: "het eerste frame van de video gebruiken als de miniatuur". 
2. Voor elke video zou u de service zien: 
    1. Waar vind ik die video  
    2. Waar de miniatuurafbeelding van uitvoer schrijven. 

Een **transformeren** helpt u bij het maken van één keer het recept (stap 1) en het verzenden van taken met behulp van deze recept (stap 2).

## <a name="paging"></a>Zoekresultaten oproepen

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen en streamen van video 's](stream-files-tutorial-with-api.md)
