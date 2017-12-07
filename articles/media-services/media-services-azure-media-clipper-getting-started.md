---
title: Aan de slag met Azure Media openen | Microsoft Docs
description: Aan de slag met Azure Media openen, een hulpprogramma voor het bouwen van videoclips uit AMS activa
services: media-services
keywords: clip; subclip; codering; media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: ac64d97aeeef6147aa62658c9ee440bf058f4db1
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Illustraties maken met Azure Media openen
Deze sectie leest u de eenvoudige stappen van aan de slag met Azure Media openen. De volgende gedeelten vindt de details over het configureren van Azure Media openen.

- De volgende koppelingen voor Azure Media Player en Azure Media openen eerst toevoegen aan uw document head. Het is raadzaam om een versie van de openen en de Azure Media Player expliciet op te geven in de URL's. Gebruik de nieuwste versie van deze resources niet in productie, omdat ze worden gewijzigd op aanvraag.

```javascript
<!--Azure Media Player 2.1.4 or later is a prerequisite-->
<link href="//amp.azure.net/libs/amp/2.1.4/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amp/2.1.4/azuremediaplayer.min.js"></script>
<!--Azure Media Clipper script and stylesheet-->
<link href="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.min.js"></script>
```

- Voeg vervolgens de volgende klassen waarin u wilt de openen instantiëren div-element.

```javascript
<div id="root" class="azure-subclipper" />
```

Voeg desgewenst de klasse donker skin zodat het donkere thema:

```javascript
<div id="root" class="azure-subclipper dark-skin" />
```

- Vervolgens exemplaar maken van de openen met de volgende API-aanroep:

```javascript
var subclipper = new subclipper({
    selector: '#root',
    logLevel: 'info',
    restVersion: '2.0',
    minimumMarkerGap: 6,
    submitSubclipCallback: onSubmitSubclip,
    singleBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with single H264Layers
                }, {
                    // Audio Codec
                }]
    },
    multiBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with multiple H264Layers
                }, {
                    // Audio Codec
                }]
    },
    keymap: {
      // See below for more info
    },
   // Enable the Video Assets panel in the widget to automatically load assets (input contract)
    assetsPanelLoaderCallback: onLoadAssets,
    height: 600,
    subclippingMode: 'all',
    filterAssetsTypes: true,
    speedLevels: [
        { name: '4x', value: 4.0 },
        { name: '3x', value: 3.0 },
        { name: '2x', value: 2.0 },
        { name: 'normal', value: 1.0 },
        { name: '0.75x', value: 0.75 },
        { name: '0.5x', value: 0.5 },
    ],
    resetOnJobDone: false,
    autoplayVideo: true,
    language: 'en'    
});
```

De parameters voor de initialisatie van de methodeaanroep zijn:
- `selector`{VEREIST, string}: CSS-selector van de overeenkomende HTML-element waar het object moet worden gerenderd.
- `restVersion`{VEREIST, string}: het Azure Media Services REST-API-versie naar doel. De versie van de REST definieert de indeling van de uitvoer wordt gegenereerd door de widget. Op dit moment wordt alleen 2.0 ondersteund.
- `submitSubclipCallback`{VEREIST, beloven} De callback-functie die wordt aangeroepen wanneer de knop 'verzenden' van het object wordt geklikt. De functie voor retouraanroepen verwachten uitvoer wordt gegenereerd door de widget (een configuratie van de taak weergeven of een filterdefinitie). Zie voor meer informatie verzenden subclip retouraanroep.
- `logLevel`{OPTIONEEL, {'info', 'waarschuwing', 'fout'}}: het logboekregistratieniveau moet worden weergegeven in de browser-console. Standaardwaarde: fout
- `minimumMarkerGap`{OPTIONEEL, int}: de minimumgrootte van een subclip (in seconden). Opmerking: de waarde moet groter zijn dan of gelijk zijn aan 6, dit is de standaardwaarde.
- `singleBitrateMp4Profile`{OPTIONEEL, JSON-object} Het single-bitrate mp4-profiel moet worden gebruikt voor de configuratie van de render-taak die worden gegenereerd door de widget. Als niet wordt opgegeven, wordt de [single-bitrate MP4-profiel standaard](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-single-bitrate-1080p).
- `multiBitrateMp4Profile`{OPTIONEEL, JSON-object} De multi bitrate mp4-profiel moet worden gebruikt voor weergeven die zijn gegenereerd door de widget in de configuratie. Als niet wordt opgegeven, wordt de [multi-bitrate MP4-profiel standaard](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-multiple-bitrate-1080p).
- `keymap`{OPTIONEEL, json-object} U kunt aanpassen van de sneltoetsen van het object. Zie voor meer informatie [aanpasbare sneltoetsen](media-services-azure-media-clipper-keyboard-shortcuts.md).
- `assetsPanelLoaderCallback`{OPTIONEEL, belofte} De callback-functie is aangeroepen om te laden (asynchroon) een nieuwe pagina van de activa in het deelvenster activa telkens wanneer de gebruiker naar de onderkant van het deelvenster schuift. Zie voor meer informatie Asset deelvenster loader retouraanroep.
- `height`{OPTIONEEL, number} De totale hoogte van de widget (minimumhoogte is 600 px zonder activa deelvenster en 850 px met het deelvenster activa).
- `subclippingMode`(Optioneel, {'all', 'weergeven', 'filter'}): de subclipping modus/modi toegestaan. De standaardwaarde is alles.
- `filterAssetsTypes`(Optioneel, bool): filterAssetsTypes kunt u de vervolgkeuzelijst filters in het deelvenster activa weergeven/verbergen. De standaardwaarde is true.
- `speedLevels`(Optioneel, array): speedLevels kunt u de instelling van de van de verschillende snelheidsniveaus voor de video-speler, Zie [Azure Media Player documentatie](http://amp.azure.net/libs/amp/latest/docs/#amp.player.playbackspeedoptions) voor meer informatie.
- `resetOnJobDone`(Optioneel, bool): resetOnJobDone kunt openen om in te stellen de subclipper naar de oorspronkelijke status wanneer een taak met succes wordt ingediend.
- `autoplayVideo`(Optioneel, bool): autoplayVideo kunt openen automatisch afspelen de video belasting. De standaardwaarde is true.
- `language`{OPTIONEEL, tekenreeks}: taal de taal van de widget wordt ingesteld. Als niet wordt opgegeven, probeert de widget voor lokalisatie van de berichten op basis van de taal van de browser. Als er geen taal is gedetecteerd in de browser, wordt de widget wordt standaard ingesteld op Engels. Zie voor meer informatie de [lokalisatie configureren](media-services-azure-media-clipper-localization.md) sectie.
- `languages`{OPTIONEEL, JSON}: de parameter talen vervangt de standaard-woordenlijst van talen door een aangepaste woordenlijst gedefinieerd door de gebruiker. Zie voor meer informatie de [lokalisatie configureren](media-services-azure-media-clipper-localization.md) sectie.
- `extraLanguages`(Optioneel, JSON): de parameter extraLanaguages nieuwe talen toevoegen aan de standaardwoordenlijst. Zie voor meer informatie de [lokalisatie configureren](media-services-azure-media-clipper-localization.md) sectie.

## <a name="typescript-definition"></a>Machineschrift definitie
Een [machineschrift](https://www.typescriptlang.org/) definitiebestand voor de openen vindt [hier](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media openen API
In dit gedeelte worden de API-gebied geleverd door het openen.

- `ready(handler)`: biedt een manier om JavaScript uitgevoerd zodra de openen is volledig geladen en klaar om te worden gebruikt.
- `load(assets)`: Hiermee wordt een lijst met activa in de widget tijdlijn (mag niet worden gebruikt samen met assetsPanelLoaderCallback) geladen. Zie dit [artikel](media-services-azure-media-clipper-load-assets.md) voor meer informatie over het laden van de activa in de openen.
- `setLogLevel(level)`: Hiermee stelt u het logboekregistratieniveau moet worden weergegeven in de browser-console. Mogelijke waarden zijn: `info`, `warn`, `error`.
- `setHeight(height)`: Hiermee stelt u de totale hoogte van de widget in pixels (minimumhoogte is 600 px zonder activa deelvenster en 850 px met het deelvenster activa).
- `version`: de versie van de widget opgehaald.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende stappen voor het configureren van Azure Media openen:
- [Laden van assets in Azure Media openen](media-services-azure-media-clipper-load-assets.md)
- [Aangepaste sneltoetsen configureren](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Verzenden van taken van de openen knippen](media-services-azure-media-clipper-submit-job.md)
- [Lokalisatie configureren](media-services-azure-media-clipper-localization.md)