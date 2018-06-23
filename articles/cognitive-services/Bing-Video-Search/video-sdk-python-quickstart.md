---
title: Video zoeken SDK Python Quick Start | Microsoft Docs
description: Instellingen voor de consoletoepassing Video Search SDK.
titleSuffix: Azure Video Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 02/15/2018
ms.author: v-gedod
ms.openlocfilehash: 1c4769a6ca3391fa595cc078651beff330bbfd60
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345707"
---
# <a name="video-search-sdk-python-quickstart"></a>Video zoeken SDK Python Quick Start

De Bing installatiekopie Search SDK bevat de functionaliteit van de REST-API voor web-query's en parseren resultaten.

De [broncode voor Python Bing Video Search SDK voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) beschikbaar is op de Git-Hub.


## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen
Als u dit nog geen hebt, installeert u Python. De SDK is compatibel met Python 2.7 3.3, 3.4, 3.5 en 3.6.

De algemene aanbevelingen voor de ontwikkeling van een Python is met een [virtuele omgeving](https://docs.python.org/3/tutorial/venv.html). Installeren en het initialiseren van de virtuele omgeving met de [venv module](https://pypi.python.org/pypi/virtualenv). Virtualenv voor Python 2.7 installeren.
```
python -m venv mytestenv
```
Bing-video's zoeken SDK-afhankelijkheden installeren:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```
## <a name="video-search-client"></a>Video zoeken client
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Invoer toevoegen:
```
subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Maak een instantie van de `CognitiveServicesCredentials`, en te maken van de client:
```
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Zoek video's (SwiftKey) en controleer of het aantal resultaten. Afdrukken `ID`, `name` en `URL` van eerste video resultaat.
```
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

try:
    video_result = client.videos.search(query="SwiftKey")
    print("Search videos for query \"SwiftKey\"")

    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")

except Exception as err:
    print("Encountered exception. {}".format(err))

```
Video's voor (Bellevue aanhangwagen) die is gratis kort en 1080p oplossing zoeken. Controleer of het aantal resultaten en afdrukken `ID`, `name` en `URL` van eerste video resultaat.
```
def video_search_with_filtering(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        video_result = client.videos.search(
            query="Bellevue Trailer",
            pricing=VideoPricing.free,  # Can use the str "free" too
            length=VideoLength.short,   # Can use the str "short" too
            resolution=VideoResolution.hd1080p  # Can use the str "hd1080p" too
        )
        print("Search videos for query \"Bellevue Trailer\" that is free, short and 1080p resolution")

        if video_result.value:
            first_video_result = video_result.value[0]
            print("Video result count: {}".format(len(video_result.value)))
            print("First video id: {}".format(first_video_result.video_id))
            print("First video name: {}".format(first_video_result.name))
            print("First video url: {}".format(first_video_result.content_url))
        else:
            print("Didn't see any video result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

Trends resultaten krijgt. Controleer of de tegels banner en categorieën:
```
def video_trending(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_result = client.videos.trending()
        print("Search trending video")

        # Banner tiles
        if trending_result.banner_tiles:
            first_banner_tile = trending_result.banner_tiles[0]
            print("Banner tile count: {}".format(len(trending_result.banner_tiles)))
            print("First banner tile text: {}".format(first_banner_tile.query.text))
            print("First banner tile url: {}".format(first_banner_tile.query.web_search_url))
        else:
            print("Couldn't find banner tiles!")

        # Categorires
        if trending_result.categories:
            first_category = trending_result.categories[0]
            print("Category count: {}".format(len(trending_result.categories)))
            print("First category title: {}".format(first_category.title))
            if first_category.subcategories:
                first_subcategory = first_category.subcategories[0]
                print("Subcategory count: {}".format(len(first_category.subcategories)))
                print("First subcategory title: {}".format(first_subcategory.title))
                if first_subcategory.tiles:
                    first_tile = first_subcategory.tiles[0]
                    print("Subcategory tile count: {}".format(len(first_subcategory.tiles)))
                    print("First tile text: {}".format(first_tile.query.text))
                    print("First tile url: {}".format(first_tile.query.web_search_url))
                else:
                    print("Couldn't find subcategory tiles!")
            else:
                print("Couldn't find subcategories!")
        else:
            print("Couldn't find categories!")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Video's zoeken naar (Bellevue aanhangwagen) en zoek vervolgens naar gedetailleerde informatie van de eerste video.
```
def video_detail(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        video_result = client.videos.search(query="Bellevue Trailer")
        first_video_result = video_result.value[0]

        video_details = client.videos.details(
            query="Bellevue Trailer",
            id=first_video_result.video_id,
            modules=[VideoInsightModule.all]  # Can use ["all"] too
        )
        print("Search detail for video id={}, name={}".format(
            first_video_result.video_id,
            first_video_result.name
        ))

        if video_details.video_result:
            print("Expected Video id: {}".format(video_details.video_result.video_id))
            print("Expected Video name: {}".format(video_details.video_result.name))
            print("Expected Video url: {}".format(video_details.video_result.content_url))
        else:
            print("Couldn't find expected video")

        if video_details.related_videos.value:
            first_related_video = video_details.related_videos.value[0]
            print("Related video count: {}".format(len(video_details.related_videos.value)))
            print("First related video id: {}".format(first_related_video.video_id))
            print("First related video name: {}".format(first_related_video.name))
            print("First related video content url: {}".format(first_related_video.content_url))
        else:
            print("Couldn't find any related video!")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
## <a name="next-steps"></a>Volgende stappen

[Cognitieve Services Python SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

