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
ms.date: 05/08/2019
ms.author: juliako
ms.openlocfilehash: 01b386c820a09af0e616698aabc58a886c30bb09
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550935"
---
# <a name="transforms-and-jobs"></a>Transformaties en taken

In dit onderwerp vindt u meer informatie over [transformeert](https://docs.microsoft.com/rest/api/media/transforms) en [taken](https://docs.microsoft.com/rest/api/media/jobs) en wordt uitgelegd van de relatie tussen deze entiteiten. 

## <a name="overview"></a>Overzicht 

### <a name="transformsjobs-workflow"></a>Transformaties/taken werkstroom

Het volgende diagram toont de werkstroom transformaties/taken.

![Transformaties](./media/encoding/transforms-jobs.png)

#### <a name="typical-workflow"></a>Standaardwerkstroom

1. Een transformatie maken 
2. Verzenden van taken onder deze transformatie 
3. Lijst met transformaties 
4. Een transformatie te verwijderen als u niet van plan bent om deze te gebruiken in de toekomst. 

#### <a name="example"></a>Voorbeeld

Stel dat u wilt extraheren van het eerste beeld van al uw video's als een miniatuurafbeelding: zijn de stappen die u moet uitvoeren: 

1. Het recept of de regel voor het verwerken van uw video's definiëren: "het eerste frame van de video gebruiken als de miniatuur". 
2. Voor elke video zou u de service zien: 
    1. Waar vind ik die video  
    2. Waar de miniatuurafbeelding van uitvoer schrijven. 

Een **transformeren** helpt u bij het maken van één keer het recept (stap 1) en het verzenden van taken met behulp van deze recept (stap 2).

> [!NOTE]
> Eigenschappen van **transformeren** en **taak** die van de datum/tijd zijn altijd in UTC-notatie zijn.

## <a name="transforms"></a>Transformaties

Gebruik **transformeert** het configureren van algemene taken voor het coderen of video's analyseren. Elke **transformeren** beschrijft een recept of een workflow van taken voor het verwerken van uw video- of audio-bestanden. Een enkele transformatie kunt meer dan één regel toepassen. Een transformatie kan bijvoorbeeld opgeven dat elke video worden gecodeerd naar een MP4-bestand op een bepaalde bitrate en dat een miniatuurafbeelding van het eerste frame van de video worden gegenereerd. U zou een TransformOutput-vermelding voor elke regel die u wilt opnemen in uw transformatie toevoegen. Voorinstellingen kunt u de transformatie vertellen hoe de invoer media-bestanden moeten worden verwerkt.

### <a name="viewing-schema"></a>Schema weergeven

Voorinstellingen zijn in Media Services v3, sterk getypeerde entiteiten in de API zelf. U vindt de definitie van de 'schema' voor deze objecten in [Open API-specificatie (of Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01). U kunt ook de vooraf ingestelde definities weergeven (zoals **StandardEncoderPreset**) in de [REST-API](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset), [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset?view=azure-dotnet) (of andere SDK-referentiedocumentatie voor Media Services v3).

### <a name="creating-transforms"></a>Het maken van transformaties

U kunt maken van transformaties met behulp van REST, CLI, of gebruik een van de gepubliceerde SDK's. De Media Services v3 die API van Azure Resource Manager wordt gestuurd, zodat u ook Resource Manager-sjablonen kunt maken en implementeren voor gegevensstromen in Media Services-account. Op rollen gebaseerd toegangsbeheer kan worden gebruikt om toegang tot transformaties vergrendelen.

### <a name="updating-transforms"></a>Transformaties bijwerken

Als u nodig hebt om bij te werken uw [transformeren](https://docs.microsoft.com/rest/api/media/transforms), gebruikt u de **bijwerken** bewerking. Het is bedoeld voor het aanbrengen van wijzigingen in de beschrijving of de prioriteiten van de onderliggende TransformOutputs. Het verdient aanbeveling dat dergelijke updates worden uitgevoerd wanneer alle lopende taken zijn voltooid. Als u van plan bent te herschrijven het recept, moet u een nieuwe transformatie te maken.

### <a name="transform-object-diagram"></a>Diagram van object transformeren

Het volgende diagram toont de **transformeren** object en de objecten hierin wordt verwezen naar de relaties afleiding inclusief. De grijze pijlen geven een type dat de verwijzingen van de taak en de groene pijl klasse afleiding relaties weergeven.<br/>Klik op de afbeelding om deze in volledig formaat weer te geven.  

<a href="./media/api-diagrams/transform-large.png" target="_blank"><img src="./media/api-diagrams/transform-small.png"></a> 

## <a name="jobs"></a>Taken

Een **taak** is van de werkelijke aanvraag voor Azure Media Services om toe te passen de **transformeren** aan een bepaalde invoer video of audio-inhoud. Zodra de transformatie is gemaakt, kunt u taken met behulp van Media Services-API's of een van de gepubliceerde SDK's kunt indienen. De **taak** bevat informatie zoals de locatie van de video-invoer en de locatie voor de uitvoer. U kunt de locatie van uw video met invoer opgeven: HTTPS-URL's, SAS-URL's of [activa](https://docs.microsoft.com/rest/api/media/assets).  

### <a name="job-input-from-https"></a>Taakinvoer van HTTPS

Gebruik [taak invoer van HTTPS](job-input-from-http-how-to.md) als er al uw inhoud toegankelijk is via een URL is en u hoeft voor het opslaan van het bronbestand in Azure (bijvoorbeeld importeren uit S3). Deze methode is ook geschikt als u de inhoud in Azure Blob-opslag hebben, maar niet nodig is voor het bestand zich in een Asset. Deze methode ondersteunt momenteel alleen een enkel bestand voor invoer.

### <a name="asset-as-job-input"></a>Asset als Taakinvoer

Gebruik [Asset als Taakinvoer](job-input-from-local-file-how-to.md) als de inhoud van de invoer al in een actief is of de inhoud wordt opgeslagen in lokaal bestand. Het is ook een goede optie als u van plan bent om te publiceren kan het invoeractivum voor streamen of downloaden (Stel dat u wilt publiceren van de mp4 gedownload, maar ook wilt doen van spraak naar tekst of face detection). Deze methode biedt ondersteuning voor meerdere bestanden assets (bijvoorbeeld MBR sets die lokaal zijn gecodeerd streaming).

### <a name="checking-job-progress"></a>Taakvoortgang controleren

De voortgang en status van taken kunnen worden verkregen door de bewaking van gebeurtenissen met Event Grid. Zie voor meer informatie, [bewaken van gebeurtenissen via EventGrid](job-state-events-cli-how-to.md).

### <a name="updating-jobs"></a>Taken bijwerken

De Update-bewerking op de [taak](https://docs.microsoft.com/rest/api/media/jobs) entiteit kan worden gebruikt om te wijzigen de *beschrijving*, en de *prioriteit* eigenschappen nadat de taak is verzonden. Een wijziging in de *prioriteit* eigenschap werkt alleen als de taak nog steeds in een in de wachtrij staat is. Als de taak verwerking is gestart of is voltooid, heeft prioriteit wijzigen geen effect.

### <a name="job-object-diagram"></a>Taakdiagram-object

Het volgende diagram toont de **taak** object en de objecten hierin wordt verwezen naar de relaties afleiding inclusief.<br/>Klik op de afbeelding om deze in volledig formaat weer te geven.  

<a href="./media/api-diagrams/job-large.png" target="_blank"><img src="./media/api-diagrams/job-small.png"></a> 

## <a name="configure-media-reserved-units"></a>Configureren van gereserveerde Media-eenheden

Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 gereserveerde Media-eenheden (groepsbeleidsinstelling). Als u meer dan 10 S3 groepsbeleidsinstelling nodig hebt, opent u een ondersteuning ticket met de [Azure-portal](https://portal.azure.com/).

Zie voor meer informatie, [met CLI mediaverwerking schalen](media-reserved-units-cli-how-to.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="see-also"></a>Zie ook

* [Foutcodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
* [Filters, bestellen, wisselbestand van Media Services-entiteiten](entities-overview.md)

## <a name="next-steps"></a>Volgende stappen

- Controleer voordat u begint met het ontwikkelen, [ontwikkelen met Media Services v3 API's](media-services-apis-overview.md) (bevat informatie over het verkrijgen van toegang tot API's voor naamgeving, enz.)
- Bekijk deze zelfstudies:

    - [Zelfstudie: Video’s uploaden, coderen en streamen met behulp van .NET](stream-files-tutorial-with-api.md)
    - [Zelfstudie: Analyseren van video's met Media Services v3 met behulp van .NET](analyze-videos-tutorial-with-api.md)
