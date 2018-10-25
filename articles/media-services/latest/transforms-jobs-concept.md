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
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: e13afe26d06f5b5b2dcf7eddf00f9ee481312b2c
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024253"
---
# <a name="transforms-and-jobs"></a>Transformaties en taken
 
Azure Media Services v3 introduceert een nieuwe resource sjablonen werkstroom voor een recept die u gebruiken om te coderen en/of het analyseren van uw video's wilt, met de naam [transformeert](https://docs.microsoft.com/rest/api/media/transforms). **Transformeert** kan worden gebruikt voor het configureren van algemene taken voor het coderen of video's analyseren. Elke **transformeren** beschrijft een recept of een workflow van taken voor het verwerken van uw video- of audio-bestanden. 

Een **taak** is van de werkelijke aanvraag voor Azure Media Services om toe te passen de **transformeren** aan een bepaalde invoer video of audio-inhoud. De **taak** bevat informatie zoals de locatie van de video-invoer en de locatie voor de uitvoer. U kunt de locatie van uw video met invoer opgeven: HTTPs-URL's, SAS-URL's of [Media Services-activa](https://docs.microsoft.com/rest/api/media/assets).  

## <a name="typical-workflow"></a>Standaardwerkstroom

1. Maak een transformatie 
2. Verzenden van taken onder deze transformatie 
3. Lijst met transformaties 
4. Een transformatie te verwijderen als u niet van plan bent om deze te gebruiken in de toekomst. 

Stel dat u wilt extraheren van het eerste beeld van al uw video's als een miniatuurafbeelding: zijn de stappen die u moet uitvoeren: 

1. Het recept of de regel voor het verwerken van uw video's definiëren: "het eerste frame van de video gebruiken als de miniatuur". 
2. Voor elke video zou u de service zien: 
    1. Waar vind ik die video  
    2. Waar de miniatuurafbeelding van uitvoer schrijven. 

Een **transformeren** helpt u bij het maken van één keer het recept (stap 1) en het verzenden van taken met behulp van deze recept (stap 2).

## <a name="transforms"></a>Transformaties

In Media Services-account met behulp van de API v3 rechtstreeks of via een van de gepubliceerde SDK's kunt u transformaties maken. De Media Services v3 die API van Azure Resource Manager wordt gestuurd, zodat u ook Resource Manager-sjablonen kunt maken en implementeren voor gegevensstromen in Media Services-account. Op rollen gebaseerd toegangsbeheer kan worden gebruikt om toegang tot transformaties vergrendelen.

### <a name="transform-definition"></a>Definitie van transformatie

De volgende tabel ziet u de eigenschappen van de transformatie en biedt de definities.

|Naam|Beschrijving|
|---|---|
|Id|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|De naam van de resource.|
|Properties.created |De UTC-datum en tijd waarop de transformatie is gemaakt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.Description |Een uitgebreide beschrijving van de transformatie.|
|properties.lastModified |De UTC-datum en tijd wanneer de transformatie het laatst is bijgewerkt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.outputs |Een matrix met een of meer TransformOutputs die de transformatie te genereren.|
|type|Het type van de resource.|

Zie voor de definitie van de volledige [transformeert](https://docs.microsoft.com/rest/api/media/transforms).

Zoals hierboven is uitgelegd, wordt er een transformatie helpt u bij het maken van een recept of de regel voor het verwerken van uw video's. Een enkele transformatie kunt meer dan één regel toepassen. Een transformatie kan bijvoorbeeld opgeven dat elke video worden gecodeerd naar een MP4-bestand op een bepaalde bitrate en dat een miniatuurafbeelding van het eerste frame van de video worden gegenereerd. U zou een TransformOutput-vermelding voor elke regel die u wilt opnemen in uw transformatie toevoegen.

> [!NOTE]
> Terwijl de definitie van de transformaties ondersteuning biedt voor een bewerking voor bijwerken, moet u ervoor zorgen dat Zorg ervoor dat alle lopende taken zijn voltooid voordat u een wijziging aanbrengt. U zou normaal gesproken een bestaande Transform als u wilt wijzigen van de beschrijving van de update, of wijzig de prioriteiten van de onderliggende TransformOutputs. Als u het recept herschrijven wilt, zou u een nieuwe transformatie maken.

## <a name="jobs"></a>Taken

Zodra een transformatie is gemaakt, kunt u taken met behulp van Media Services-API's of een van de gepubliceerde SDK's kunt indienen. De voortgang en status van taken kunnen worden verkregen door de bewaking van gebeurtenissen met Event Grid. Zie voor meer informatie, [bewaken van gebeurtenissen via EventGrid](job-state-events-cli-how-to.md ).

### <a name="jobs-definition"></a>De definitie van taken

De volgende tabel ziet u de eigenschappen van de taken en geeft de definities.

|Naam|Beschrijving|
|---|---|
|id|Volledig gekwalificeerde resource-ID voor de resource.|
|naam   |De naam van de resource.|
|properties.correlationData |De klant opgegeven wijzigingsmeldingen data korelace (onveranderbare) dat wordt geretourneerd als onderdeel van de taak en JobOutput staat. Zie voor meer informatie, [gebeurtenisschema's](media-services-event-schemas.md)<br/><br/>De eigenschap kan ook worden gebruikt voor multitenant-gebruik van Media Services. U kunt de tenant-id's plaatsen in de taken. |
|Properties.created |De UTC-datum en tijd waarop de taak is gemaakt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.Description |Optionele klant verstrekte beschrijving van de taak.|
|Properties.Input|De invoer voor de taak.|
|properties.lastModified    |De UTC-datum en tijd wanneer de taak het laatst is bijgewerkt, in ' jjjj-MM-ddTHH ' indeling.|
|Properties.outputs|De uitvoer voor de taak.|
|Properties.Priority    |De prioriteit die de taak moet worden verwerkt. Taken met een hogere prioriteit worden verwerkt voordat jobs met lagere prioriteit. Als dat niet is ingesteld, de standaardwaarde is normaal.|
|Properties.state   |De huidige status van de taak.|
|type   |Het type van de resource.|

Zie voor de definitie van de volledige [taken](https://docs.microsoft.com/rest/api/media/jobs).

> [!NOTE]
> Tijdens een updatebewerking ondersteuning biedt voor de definitie van de taken, zijn de enige eigenschappen die kunnen worden gewijzigd nadat de taak is verzonden de beschrijving en de prioriteit. Een wijziging in de prioriteit is bovendien werkt alleen als de taak nog steeds in een in de wachtrij staat is. Als de taak verwerking is gestart of is voltooid, heeft de prioriteit wijzigen geen effect.

### <a name="pagination"></a>Paginering

Taken paginering wordt ondersteund in Media Services v3.

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. 

Als er taken worden gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen doorgevoerd in de geretourneerde resultaten (als deze wijzigingen zijn in het gedeelte van de verzameling die niet zijn gedownload.) 

De volgende C# voorbeeld laat zien hoe om te inventariseren door de taken in het account.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Zie voor voorbeelden van REST [taken - lijst](https://docs.microsoft.com/rest/api/media/jobs/list)


## <a name="next-steps"></a>Volgende stappen

[Stream-video 's](stream-files-dotnet-quickstart.md)
