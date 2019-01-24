---
title: Assets laden in Azure Media Clipper | Microsoft Docs
description: Stappen voor het laden van assets in Azure Media Clipper
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 415f58d9c6880bc7ef9ba6b6ba3f575fae382f04
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54806881"
---
# <a name="loading-assets-into-azure-media-clipper"></a>Het laden van assets in Azure Media Clipper
Activa kunnen worden geladen in de Azure Media Clipper door twee methoden:
1. Statisch doorgeven in een bibliotheek met activa
2. Een lijst met activa via API dynamisch genereren

## <a name="statically-load-videos-into-clipper"></a>Statisch video's laden in Clipper
Laad statisch video's in de Clipper zodat eindgebruikers clips bouwen zonder video's selecteren in het deelvenster asset selecteren.

In dit geval geeft u in een vaste set voorzieningen aan de Clipper. Elke asset bevat een AMS-asset/filter-ID, naam, gepubliceerde streaming-URL. Indien van toepassing, een verificatietoken voor beveiliging van inhoud of een matrix van de miniatuur van URL's kunnen worden doorgegeven. Als een doorgegeven, worden de miniaturen ingevuld in de interface. In scenario's waarin de bibliotheek asset statische en klein, kunt u doorgeven in het contract asset voor alle activa in de bibliotheek.

> [!NOTE]
> Assets worden toegevoegd wanneer het statisch laden van assets in de Clipper, **rechtstreeks naar de tijdlijn** en de **asset deelvenster wordt niet gerenderd**. Het eerste activum wordt toegevoegd aan de tijdlijn en de rest van de activa zijn gestapeld aan de rechterkant van de tijdlijn).

Een statische asset om bibliotheek te laden, gebruikt u de **laden** methode om door te geven in een JSON-weergave van elk actief. Het volgende codevoorbeeld ziet u de JSON-weergave voor een asset.

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
> Het verdient keten de methode load() aanroepen met de methode ready(handler) zoals wordt weergegeven in het voorgaande voorbeeld. Het vorige voorbeeld zorgt ervoor dat de widget gereed is voordat het laden van de activa.

> [!NOTE]
> Voor de miniatuur van URL's naar behoren werkt in de tijdlijn van de Clipper ze gelijkmatig moeten worden verdeeld over de video (op basis van de duur) en in chronologische volgorde binnen de matrix. U kunt de volgende vooraf gedefinieerde JSON-fragment gebruiken als een voorbeeld-verwijzing voor het genereren van afbeeldingen met de 'Media Encoder Standard' processor:

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

## <a name="dynamically-load-videos-in-clipper"></a>Video's in Clipper dynamisch laden
Dynamisch video's in de Clipper waarmee eindgebruikers kunnen video's selecteren in het deelvenster asset selectie knippen tegen laden.

U kunt ook activa dynamisch via een retouraanroep laden. In scenario's waarbij activa dynamisch worden gegenereerd of de bibliotheek is groot, moet u via de callback laden. Als u wilt laden asset dynamisch, moet u de optionele onLoadAssets retouraanroepfunctie implementeren. Deze functie wordt doorgegeven in de Clipper tijdens de initialisatie. De omgezette assets moeten voldoen aan de dezelfde opdracht als statisch geladen assets. Het volgende codevoorbeeld ziet u de methode, de verwachte invoer en de verwachte uitvoer.

> [!NOTE]
> Wanneer het dynamisch laden van assets in de Clipper, assets worden weergegeven in de **asset selectie deelvenster**.

```javascript
// Video Assets Pane Callback
    //
    // Filter Parameters:
    // - search: string value term that will be used in the back-end to filter assets by name.
    // - skip: int value used for pagination in the back-end that allows skipping a number of assets in the response.
    // - take: int value used for pagination in the back-end that allows defining the number of assets to include in the response.
    // - type: ('filter', 'asset') value that will be used in the back-end to filter assets by type.
    //
    // Returns: a Promise object that, when resolved, returns an object containing an array of assets (input contract)
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