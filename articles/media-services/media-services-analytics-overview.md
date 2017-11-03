---
title: Media Analytics op het Media Services-platform | Microsoft Docs
description: Overzicht van de openbare preview van Media Analytics, een verzameling spraakonderdelen en computer vision-services op enterprise schaal, naleving, beveiliging en globale bereiken
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2017
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: c0bbe6f80370515fa783b12757434897fe2221b6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="media-analytics-on-the-media-services-platform"></a>Media Analytics op het Media Services-platform
## <a name="overview"></a>Overzicht
Meer organisaties gebruiken video als het medium voor voorkeur voor het trainen van werknemers, hun klanten benaderen en documenteren bedrijfsfuncties. Cloud computing biedt een manier opslaan, stream en toegang tot deze grote mediabestanden. Maar als bibliotheek van video-inhoud van een bedrijf groeit, moet een even efficiënt middel insights extraheren uit de inhoud. 

Om deze groeiende behoefte op te lossen, biedt Azure Media Services Azure Media Analytics. Media Analytics is een verzameling spraakonderdelen en visuele onderdelen waarmee organisaties en bedrijven gemakkelijker inzichten kunnen verkrijgen op basis van hun video's, waarvoor een actie kan worden uitgevoerd. Met behulp van de kernonderdelen van het Media Services-platform gebouwd, kunnen Media Analytics media verwerken op grote schaal op dag één verwerken.

Met Media Analytics kunnen ontwikkelaars snel geavanceerde functionaliteit van de video doen in toepassingen. Het biedt bedrijfsomgevingen de volledige schaal, naleving, beveiliging en globale bereiken die vereist zijn voor grote organisaties.

Het volgende diagram toont de Media Analytics en andere belangrijke onderdelen van het Media Services-platform. 

![VoD-werkstroom](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Media Analytics-mediaprocessoren produceren MP4- of JSON-bestanden. Als een Mediaprocessor een MP4-bestand produceert, kunt u het bestand progressief downloaden. Als een Mediaprocessor een JSON-bestand produceert, kunt u het bestand downloaden uit Azure Blob-opslag. 

## <a name="media-analytics-services"></a>Media Analytics-services

### <a name="indexer"></a>Indexer
Met Azure Media Indexer, kunt u inhoud doorzoekbare en houdt gesloten ondertiteling te genereren. Azure Media Indexer 2 Preview heeft vergeleken met de vorige versie, een sneller indexering en breder taal ondersteunen. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees, Portugees en Arabisch. Zie voor gedetailleerde informatie en voorbeelden [video's verwerken met Azure Media Indexer 2](media-services-process-content-with-indexer2.md).
### <a name="hyperlapse"></a>Hyperlapse
Microsoft Hyperlapse combineert video stabilization en time-lapse mogelijkheid snel, verbruikbare video's van uw inhoud lange vorm maken. Naast het time-lapse video maken, kunt u Hyperlapse stabiele video's van beelden video's die zijn vastgelegd via mobiele telefoons en camcorders maken. Zie voor gedetailleerde informatie en voorbeelden [Hyperlapse media-bestanden met Azure Media Hyperlapse](media-services-hyperlapse-content.md).
### <a name="motion-detector"></a>Bewegingsherkenning
Bewegingsdetectie-detectie kunt u beweging in een video met een stilstaan achtergrond detecteren. Dit maakt het mogelijk om te controleren op valse positieven op motion gebeurtenissen gedetecteerd door bewakingscamera's te gebruiken. Zie voor gedetailleerde informatie en voorbeelden [Bewegingsdetectie voor Azure Media Analytics](media-services-motion-detection.md).
### <a name="face-detector"></a>Gezichtsherkenning
Face-detectie gebruikt, kunt u vlakken van gebruikers en hun emoties, inclusief gelukkig, sadness en onverwacht detecteren. Dit heeft verschillende nuttig toepassingen in sectoren verderop, met inbegrip van aggregeren en analyseren van reacties van mensen die een gebeurtenis. Zie voor gedetailleerde informatie en voorbeelden [gezichts- en emotion detectie voor Azure Media Analytics](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Samenvatting van de video
Samenvatting van de video kan helpen u bij het maken van samenvattingen van lange video's door interessante codefragmenten automatisch selecteren van de bronvideo. Dit is handig als u bieden een snel overzicht van wat u wilt kunt verwachten in een lange video. Zie voor gedetailleerde informatie en voorbeelden [gebruik Azure Media Video miniaturen voor het maken van de samenvatting van de video](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Optische tekenherkenning
Met Azure Media OCR (OCR), kunt u tekstinhoud in videobestanden converteren naar een bewerkbare, doorzoekbaar digitale tekst. U kunt vervolgens de extractie van zinvolle metagegevens van de video signaal van uw media automatiseren.
### <a name="scalable-face-redaction"></a>Schaalbare face redactie
Azure Media Redactor is een Mediaprocessor Media Analytics schaalbare face redactie in de cloud biedt. Face redactie gebruikt, kunt u uw video om te vervagen vlakken van geselecteerde gebruikers wijzigen. Het is raadzaam de service wilt gebruiken face redactie nieuws media of wanneer de openbare veiligheid is betrokken. Kunnen een paar minuten beeldmateriaal waarin meerdere vlakken uren handmatig Redigeren duren, maar met deze service face redactie duurt een paar eenvoudige stappen. Zie voor meer informatie de [Redigeren vlakken met Azure Media Analytics](media-services-face-redaction.md) artikel.

## <a name="common-scenarios"></a>Algemene scenario's
Organisaties kunnen helpen bij Media Analytics en ondernemingen verzamelen inzichten van video en meer effectief beheer van grote hoeveelheden video-inhoud. Hier volgen enkele scenario's:

* **Roep centers**. Zelfs met de komst van sociale media vergemakkelijken klantenservice aanroep afdelingen nog steeds een groot percentage van de klantenservice transacties. In deze audiogegevens gecodeerd, is een grote hoeveelheid klantgegevens die kan worden geanalyseerd om te zorgen voor hogere klanttevredenheid. Organisaties kunnen met behulp van Media Indexer Haal de tekst en maak zoekindexen en dashboards. Vervolgens kunnen ze intelligence rond de algemene klachten bronnen van klachten en andere relevante gegevens extraheren.
* **Gebruikers gegenereerde inhoud toezicht**. Veel organisaties hebben van nieuws media-uitgevers voor afdelingen politie, openbare-portals die gebruiker gegenereerde media zoals video's en afbeeldingen te accepteren. Het volume van de inhoud kunt pieken vanwege onverwachte gebeurtenissen. In deze scenario's is het moeilijk om uit te voeren effectieve handmatige beoordelingen van inhoud voor geschiktheid. Klanten kunnen zijn afhankelijk van de service inhoud toezicht te concentreren op inhoud die geschikt is.
* **Toezicht**. Met de groei in het gebruik van IP-camera's wordt geleverd een groeiende inventarisatie van toezicht video. Handmatig controleren toezicht video is tijd tijdrovende en foutgevoelige menselijke fouten. Media Analytics biedt services zoals bewegingsdetectie face detection en Hyperlapse waarmee het proces van controleren, beheren en afleidingen gemakkelijker te maken.

## <a name="media-analytics-media-processors"></a>Media Analytics-mediaprocessoren
Deze sectie geeft een lijst van de Media Analytics-mediaprocessoren en laat zien hoe u .NET of REST ophalen van een media-processor (MP)-object.

### <a name="mp-names"></a>MP-namen
* Preview van Azure Media Indexer 2
* Azure Media Indexer
* Azure Media Hyperlapse
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR

### <a name="net"></a>.NET
De volgende functie heeft een van de opgegeven MP-namen en een MP-object geretourneerd.

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

Antwoord:

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

## <a name="demos"></a>Demo 's
Zie [Azure Media Analytics demo's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
Zie [Media Services Analytics aankondiging](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
