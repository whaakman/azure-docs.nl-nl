---
title: Media Analytics op de Media Services-platform | Microsoft Docs
description: Overzicht van de openbare preview van Media Analytics, een verzameling spraak- en computer vision-services op grote schaal, compliance, beveiliging en wereldwijd bereik
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: 0c090499ee6ae2aaaa14af18c3c38db28ecb2f3e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57884724"
---
# <a name="media-analytics-on-the-media-services-platform"></a>Media Analytics op de Media Services-platform 

## <a name="overview"></a>Overzicht
Meer organisaties zijn video gebruiken als het gewenste medium om te trainen van hun werknemers, houd contact met hun klanten en documenteren van zakelijke functies. Cloud computing kunt u opslaat, streamen en grote mediabestanden. Maar als bibliotheek van video-inhoud van een bedrijf groeit, er moet een even efficiënte manier van het extraheren van inzichten uit de inhoud. 

Azure Media Services biedt om af te spelen op deze groeiende behoefte, Azure Media Analytics. Media Analytics is een verzameling spraakonderdelen en visuele onderdelen waarmee organisaties en bedrijven gemakkelijker inzichten kunnen verkrijgen op basis van hun video's, waarvoor een actie kan worden uitgevoerd. Gebouwd met behulp van de kernonderdelen van de Media Services-platform, kunnen Media Analytics mediaverwerking op schaal op dag één verwerkt.

Met Media Analytics, kunnen ontwikkelaars snel geavanceerde functionaliteit voor video brengen in toepassingen. Het biedt bedrijfsomgevingen met de volledige schaal, naleving, beveiliging en wereldwijd bereik vereist voor grote organisaties.

Het volgende diagram toont Media Analytics en andere belangrijke onderdelen van het Media Services-platform. 

![VoD-werkstroom](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Media Analytics-mediaprocessoren produceren MP4- of JSON-bestanden. Als een Mediaprocessor een MP4-bestand produceert, kunt u het bestand progressief downloaden. Als een Mediaprocessor een JSON-bestand produceert, kunt u het bestand downloaden uit Azure Blob storage. 

## <a name="media-analytics-services"></a>Media Analytics-services

### <a name="indexer"></a>Indexer
Met Azure Media Indexer, kunt u doorzoekbare inhoud en het genereren van ondertitels sporen te wissen. Vergeleken met de vorige versie, heeft Azure Media Indexer 2-Preview-indexering en sneller bredere taal ondersteunen. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees, Portugees en Arabisch. Zie voor gedetailleerde informatie en voorbeelden, [video's verwerken met Azure Media Indexer 2](media-services-process-content-with-indexer2.md).
### <a name="hyperlapse"></a>Hyperlapse
Microsoft Hyperlapse combineert video stabilization en timelapsevideo mogelijkheid om te maken van snelle, verbruikbare video's van uw inhoud lange vorm. Naast het maken van timelapsevideo video, kunt u Hyperlapse stabiele video's van schokkerige video's die zijn vastgelegd via mobiele telefoons en camcorders maken. Zie voor gedetailleerde informatie en voorbeelden, [Hyperlapse media-bestanden met Azure Media Hyperlapse](media-services-hyperlapse-content.md).
### <a name="motion-detector"></a>Bewegingsherkenning
Bewegingsherkenning kunt u beweging detecteren in een video met een stilstaande achtergrond. Dit maakt het mogelijk om te controleren voor fout-positieven op bewegingsgebeurtenissen van surveillancecamera's gedetecteerd. Zie voor gedetailleerde informatie en voorbeelden, [Bewegingsdetectie voor Azure Media Analytics](media-services-motion-detection.md).
### <a name="face-detector"></a>Gezichtsherkenning
Met behulp van Gezichtsherkenning, kunt u gezichten van mensen en hun emoties, met inbegrip van blijdschap, verdriet en verrassing detecteren. Dit heeft diverse nuttige branche toepassingen, verderop, waaronder aggregeren en analyseren van reacties van personen hebt deelgenomen aan een gebeurtenis. Zie voor gedetailleerde informatie en voorbeelden, [gezichten en emoties detectie voor Azure Media Analytics](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Video-overzicht
Samenvatting van de video kunt u bij het maken van samenvattingen van lange video's door het automatisch selecteren interessante codefragmenten in de bronvideo. Deze mogelijkheid is handig als u geven een kort overzicht van wat wilt u kunt verwachten in een lange video. Zie voor gedetailleerde informatie en voorbeelden, [met Azure Media Video Thumbnails om te maken van video-overzicht](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Optische tekenherkenning
Met Azure Media OCR (optische tekenherkenning), kunt u de tekstinhoud in video's converteren naar bewerkbare, doorzoekbaar digitale tekst. U kunt vervolgens de extractie van nuttige metagegevens van de video signaal van uw media automatiseren.
### <a name="scalable-face-redaction"></a>Schaalbare gezichten onherkenbaar maken
Azure Media Redactor is een mediaverwerkingsprogramma van Media Analytics biedt schaalbare gezichten onherkenbaar maken in de cloud. Met behulp van gezichten onherkenbaar maken, kunt u uw video om gezichten van geselecteerde personen onherkenbaar te wijzigen. Het is raadzaam de gezichten onherkenbaar maken-service gebruiken nieuwsmedia of als de openbare veiligheid is betrokken. Pas een paar minuten beeldmateriaal met meerdere gezichten uur redigeer handmatig, maar met deze service gezichten onherkenbaar maken duurt een paar eenvoudige stappen. Zie voor meer informatie de [Redigeer gezichten met Azure Media Analytics](media-services-face-redaction.md) artikel.
### <a name="content-moderation"></a>Content Moderation
Azure Content Moderator kunt u geautomatiseerd beheer gebruiken voor uw video's. U wilt bijvoorbeeld detecteren van mogelijk erotische en ongepaste inhoud in video's en bekijk de gemarkeerde inhoud door uw teams menselijk toezicht. Handmatig toezicht houden op video's voor ongewenste inhoud is een tijd in beslag nemen en dure taak. Met deze service en de bijbehorende programma's combineren u geautomatiseerd beheer met human-in-the-loop mogelijkheden voor de beste resultaten, efficiënte en rendabele wijze. Zie voor meer informatie, de [verwerken van uw video's met Azure Content Moderator](media-services-content-moderation.md) artikel.

## <a name="common-scenarios"></a>Algemene scenario's
Media Analytics-organisaties kan helpen en ondernemingen vindt nieuwe inzichten uit video en meer grote volumes van video-inhoud effectief te beheren. Hier volgen enkele scenario's:

* **Callcenters**. Zelfs met de komst van sociale media vergemakkelijken klant callcenters nog steeds een groot percentage van de klantenservice transacties. In deze audiogegevens gecodeerd, is een grote hoeveelheid klantgegevens die kan worden geanalyseerd voor het bereiken van hogere klanttevredenheid. Organisaties kunnen met behulp van Media Indexer, haal de tekst en maak zoekindexen en dashboards. Vervolgens kunnen ze intelligence rond algemene klachten, bronnen van klachten en andere relevante gegevens extraheren.
* **Door gebruikers gegenereerde inhoudstoezicht**. Veel organisaties hebben van nieuws mediakanalen aan de politie afdelingen, openbare-portals die door gebruikers gegenereerde media zoals video's en afbeeldingen te accepteren. Het volume van de inhoud kan piekbelastingen vanwege onverwachte gebeurtenissen. In deze scenario's is het moeilijk om uit te voeren van effectieve handmatige beoordelingen van inhoud voor geschiktheid. Klanten kunnen zijn afhankelijk van de service beheer van inhoud kunt richten op de inhoud die van toepassing is.
* **Toezicht**. Met de groei in het gebruik van IP-camera's afkomstig is een groeiende inventarisatie van toezicht video. Handmatig controleren toezicht video is het tijd intensieve en gevoelig voor menselijke fouten. Media Analytics biedt services, zoals bewegingsdetectie, gezichtsdetectie en Hyperlapse om het proces van het controleren, beheren en afgeleiden eenvoudiger maken.

## <a name="media-analytics-media-processors"></a>Media Analytics-mediaprocessoren
Deze sectie geeft een lijst van de media-processors van Media Analytics en laat zien hoe u .NET of REST gebruiken voor het ophalen van een mediaobject processor (MP).

### <a name="mp-names"></a>MP-namen
* Azure Media Indexer 2-Preview
* Azure Media Indexer
* Azure Media Hyperlapse
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR
* Azure Media Content Moderator

### <a name="net"></a>.NET
De volgende functie neemt een van de opgegeven Management Pack-namen en retourneert een Management Pack-object.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a>REST
Aanvraag:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

Reactie:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a>Demo’s
Zie [demo's van Azure Media Analytics](https://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
Zie [Media Services Analytics aankondiging](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
