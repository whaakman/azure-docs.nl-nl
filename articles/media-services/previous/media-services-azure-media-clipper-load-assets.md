---
title: Laden van assets in Azure Media openen | Microsoft Docs
description: Stappen voor het laden van assets in Azure Media openen
services: media-services
keywords: clip; subclip; codering; media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 6a479218ff8bd5addf4273b23c06380859e0ea08
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="loading-assets-into-azure-media-clipper"></a>Laden van assets in Azure Media openen
Activa kunnen worden geladen in de Azure Media openen door twee methoden:
1. Statisch doorgeven in een bibliotheek van elementen
2. Een lijst met activa via API dynamisch genereren

## <a name="statically-load-videos-into-clipper"></a>Statisch video's laden in openen
Statisch laden video's in de openen zodat eindgebruikers illustraties uitvoeren zonder dat video's in het deelvenster van de selectie actief te selecteren.

In dit geval geeft u in een vaste set van activa aan de openen. Elke asset bevat een AMS asset-/ filter-ID, naam, gepubliceerde streaming-URL. Indien van toepassing, een verificatietoken voor beveiliging van inhoud of een matrix van miniatuur URL's kunnen worden doorgegeven. Als doorgegeven, worden de miniaturen ingevuld in de interface. In scenario's waarin de bibliotheek asset statische en in kleine, kunt u in het contract asset doorgeven voor alle activa in de bibliotheek.

> [!NOTE]
> Bij het laden statisch activa in de openen, activa zijn toegevoegd **rechtstreeks aan de tijdlijn** en de **asset deelvenster wordt niet weergegeven**. De eerste asset wordt toegevoegd aan de tijdlijn en de rest van de activa aan de rechterkant van de tijdlijn gestapeld zijn).

Als u wilt een bibliotheek met statische elementen, gebruiken de **laden** methode doorgeven in een JSON-weergave van elk actief. Het volgende codevoorbeeld ziet u de JSON-weergave voor een asset.

```javascript
var assets = [
    {
      /* Required: represents the Azure Media Services asset Id when "type" === "asset"; otherwise, represents the dynamic manifest asset filter Id ("type" === "filter")  */
      "id": "my-asset-or-dynamic-manifest-asset-filter-id",
    
      /* Required: represents the asset name as shown in the Clipper interface */
      "name": "My Asset or Dynamic Manifest Asset Filter Name",
    
      /* Required: must be one of the following values: "asset" or "filter" */
      /* NOTE: "asset" type represents a rendered asset; "filter" type represents a dynamic manifest asset filter */
      "type": "asset",
    
      /* Required */
      "source": {
    
        /* Required: represents the asset streaming locator, the base Smooth Streaming URL */
        "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
    
        /* Optional: default value "application/vnd.ms-sstr+xml" */
        "type": "application/vnd.ms-sstr+xml",
    
        /* Required: If the asset has content protection applied, then you must include an array with the different protection types along with the token to request the license/key; otherwise, provide an empty array */
        "protectionInfo": [{
            "type": "AES",
            "authenticationToken": "Bearer aes-token-placeholder"
          },
          {
            "type": "PlayReady",
            "authenticationToken": "Bearer playready-token-placeholder"
          },
          {
            "type": "Widevine",
            "authenticationToken": "Bearer widevine-token-placeholder"
          },
          {
            "type": "FairPlay",
            "certificateUrl": "//example/path/to/myfairplay.der",
            "authenticationToken": "Bearer fairplay-token-placeholder"
          }
        ]
      },
    
      /* Optional: array containing thumbnail URLs for the video. */
      /* NOTE: For the thumbnail URLs to work as expected in the Clipper timeline they must be evenly distributed across the video (based on the duration) and in chronological order within the array. */
      "thumbnails": [
        "//example/path/thumbnail_001.jpg",
        "//example/path/thumbnail_002.jpg",
        "//example/path/thumbnail_003.jpg",
        "//example/path/thumbnail_004.jpg",
        "//example/path/thumbnail_005.jpg"
        ]
    }
];

var subclipper = new subclipper({
    selector: '#root',
    restVersion: '2.0',
    submitSubclipCallback: onSubmitSubclip,
});
subclipper.ready(function () {
    subclipper.load(assets);
});

```

> [!NOTE]
> Het is raadzaam de methode load() aanroepen met de methode ready(handler) zoals weergegeven in het voorgaande voorbeeld keten. Het vorige voorbeeld zorgt ervoor dat de widget gereed is voordat het laden van de activa.

> [!NOTE]
> Voor de miniatuur URL's werken zoals verwacht in de openen tijdlijn ze moeten worden gelijkmatig worden verdeeld over de video (op basis van de duur) en in chronologische volgorde binnen de matrix. Kunt u het volgende vooraf ingestelde JSON-codefragment als voorbeeld verwijzing voor het genereren van afbeeldingen met de 'Media Encoder Standard' processor:

```json
{
  "Start": "0",
  "Step": "00:00:05",
  "Range": "100%",
  "Type": "PngImage",
  "PngLayers": [
    {
      "Type": "PngLayer",
      "Width": 48,
      "Height": 26
    }
  ]
}
```

## <a name="dynamically-load-videos-in-clipper"></a>Dynamisch geladen video's in openen
Dynamisch video's in de openen zodat eindgebruikers video's te selecteren in het Configuratiescherm van de selectie actief naar bijsnijden tegen laden.

U kunt ook activa dynamisch via een retouraanroep laden. In scenario's waarbij activa dynamisch wordt gegenereerd of de bibliotheek groot is, moet u via de callback laden. Als u wilt laden asset dynamisch, moet u de optionele onLoadAssets retouraanroepfunctie implementeren. Deze functie is doorgegeven aan de openen tijdens de initialisatie. De opgelost activa moeten voldoen aan hetzelfde contract als statisch geladen activa. Het volgende codevoorbeeld ziet u de methodehandtekening, de verwachte invoer en de verwachte uitvoer.

> [!NOTE]
> Bij het laden dynamisch activa in de openen, activa worden weergegeven in de **asset selectie Configuratiescherm**.

```javascript
// Video Assets Pane Callback
    //
    // Filter Parameters:
    // - search: string value term that will be used in the back-end to filter assets by name.
    // - skip: int value used for pagination in the back-end that allows skipping a number of assets in the response.
    // - take: int value used for pagination in the back-end that allows defining the number of assets to include in the response.
    // - type: ('filter', 'asset') value that will be used in the back-end to filter assets by type.
    //
    // Returns: a Promise object that, when resolved, retuns an object containing an array of assets (input contract)
    //          that satisfies the filter parameters, plus optionally the total types of files available:
    // {
    //  total: 100,
    //  assets: [{...}],
    // }
    var onLoadAssets = function (search, skip, take, type) {
        var promise = new Promise(function (resolve, reject) {
            // TODO: implement the getAssetsFromBackend method to get the assets from the back-end using the filter parameters (search, skip, take, type).
            getAssetsFromBackend(search, skip, take, type)
                .then(function (assets) {
                    resolve({
                        total: assets.length,
                        assets: assets
                    });
                }).catch(function () {
                    reject(Error("error details"));
                });
        });
    
        return promise;
    };

    // Create widget instance:
    // - using a root element selector
    // - enabling the Video Assets panel by registering a callback in the 'assetsPanelLoaderCallback' option parameter.
    var widget = new subclipper({
        selector: '#root',

        // Enable the Video Assets panel in the widget to automatically load assets (input contract)
        assetsPanelLoaderCallback: onLoadAssets
    });

    // ...
    // The widget will automatically invoke the 'assetsPanelLoaderCallback' callback with the filter parameters specified by the user 
    // and load assets returned by the Promise into the Video Assets panel.
    // ...
```