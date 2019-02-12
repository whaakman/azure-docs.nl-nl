---
title: Aan de slag met Azure Media Clipper | Microsoft Docs
description: Aan de slag met Azure Media Clipper, een hulpprogramma voor het bouwen van videoclips van AMS-middelen
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 02/10/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 772865f42dfef8734fb23022039f02910958af61
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992463"
---
# <a name="create-clips-with-azure-media-clipper"></a>Maak korte clips met Azure Media Clipper
Deze sectie leest u de eenvoudige stappen aan de slag met Azure Media Clipper. De volgende gedeelten vindt de details over het configureren van Azure Media Clipper.

- De volgende koppelingen voor Azure Media Player en Azure Media Clipper eerst toevoegen aan de kop van het document. Het is raadzaam om een versie van de Clipper en Azure Media Player expliciet opgeven in de URL's. Gebruik de nieuwste versie van deze resources niet in de productieomgeving, zoals ze onderhevig aan wijzigingen op aanvraag zijn.

```javascript
<!--Azure Media Player 2.1.4 or later is a prerequisite-->
<link href="//amp.azure.net/libs/amp/2.1.4/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amp/2.1.4/azuremediaplayer.min.js"></script>
<!--Azure Media Clipper script and stylesheet-->
<link href="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.min.js"></script>
```

- Voeg vervolgens de volgende klassen toe aan het div-element waarin u wilt de Clipper instantiëren.

```javascript
<div id="root" class="azure-subclipper" />
```

Als u wilt de donkere thema inschakelen, voegt u desgewenst de klasse donker-weergave:

```javascript
<div id="root" class="azure-subclipper dark-skin" />
```

- Vervolgens maken de Clipper met de volgende API-aanroep:

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

De parameters voor het aanroepen van de initialisatie-methode zijn:
- `selector` {VEREIST, tekenreeks}: CSS-selector van de overeenkomende HTML-element waarin het object moet worden gerenderd.
- `restVersion` {VEREIST, tekenreeks}: De Azure Media Services REST API-versie naar doel. De versie van de REST definieert de indeling van de uitvoer gegenereerd met de widget. Op dit moment wordt alleen 2.0 ondersteund.
- `submitSubclipCallback` {VEREIST, belofte} De callback-functie die wordt aangeroepen wanneer de knop "verzenden" van de widget wordt geklikt. De callback-functie goed is, de uitvoer gegenereerd met de widget (een taak render configuratie of de filterdefinitie van een). Zie voor meer informatie verzenden subclip retouraanroep.
- `logLevel` {OPTIONAL, {'info', 'warn', 'error'}}: Het niveau van logboekregistratie moet worden weergegeven in de console van de browser. Standaardwaarde: fout
- `minimumMarkerGap` {OPTIONEEL, int}: De minimale grootte van een subclip (in seconden). Opmerking: de waarde moet groter of gelijk zijn aan 6, dit is de standaardinstelling.
- `singleBitrateMp4Profile` {OPTIONEEL, JSON-object} Het single-bitrate mp4-profiel moet worden gebruikt voor de configuratie van de render gegenereerd met de widget. Als niet is opgegeven, wordt de [single-bitrate MP4-profiel standaard](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-single-bitrate-1080p).
- `multiBitrateMp4Profile` {OPTIONEEL, JSON-object} Het multi-bitrate mp4-profiel moet worden gebruikt voor de configuratie van gegenereerd met de widget weergegeven. Als niet is opgegeven, wordt de [standaard multi-bitrate MP4-profiel](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-multiple-bitrate-1080p).
- `keymap` {OPTIONEEL, json-object} Hierbij kan de sneltoetsen van de widget. Zie voor meer informatie, [aanpasbare sneltoetsen](media-services-azure-media-clipper-keyboard-shortcuts.md).
- `assetsPanelLoaderCallback` {OPTIONEEL, belofte} De callback-functie aangeroepen voor het laden van (asynchroon) een nieuwe pagina van de activa in het deelvenster activa telkens wanneer de gebruiker gaat naar de onderkant van het deelvenster scrollen. Zie voor meer informatie, Asset deelvenster loader terugbellen.
- `height` {OPTIONEEL, number} De totale hoogte van de widget (minimumhoogte is 600 px zonder activa deelvenster en 850 px met het deelvenster activa).
- `subclippingMode` (OPTIONAL, {'all', 'render', 'filter'}): De sub-clipping modus/modi toegestaan. De standaardwaarde is alles.
- `filterAssetsTypes` (Optioneel, bool): filterAssetsTypes kunt u de vervolgkeuzelijst filters in het deelvenster activa weergeven/verbergen. De standaardwaarde is true.
- `speedLevels` (Optioneel, matrix): speedLevels kunnen andere snelheidsniveaus voor de video speler instellen, Zie [documentatie voor Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/#amp.player.playbackspeedoptions) voor meer informatie.
- `resetOnJobDone` (Optioneel, bool): resetOnJobDone kunt Clipper opnieuw in te stellen de subclipper naar de oorspronkelijke status wanneer een taak is verzonden.
- `autoplayVideo` (Optioneel, bool): autoplayVideo kunt Clipper automatisch afspelen de video bij het laden. De standaardwaarde is true.
- `language` {String, optioneel,}: taal de taal van de widget wordt ingesteld. Indien niet opgegeven, probeert de widget voor lokalisatie van de berichten op basis van de browsertaal. Als er geen taal is gedetecteerd in de browser, wordt de widget standaard in het Engels. Zie voor meer informatie de [lokalisatie configureren](media-services-azure-media-clipper-localization.md) sectie.
- `languages` {OPTIONEEL, JSON}: de parameter talen wordt vervangen door de standaard-woordenlijst van talen met een aangepaste woordenlijst die is gedefinieerd door de gebruiker. Zie voor meer informatie de [lokalisatie configureren](media-services-azure-media-clipper-localization.md) sectie.
- `extraLanguages` (Optioneel, JSON): de parameter extraLanguages nieuwe talen toevoegen aan de standaardwoordenlijst. Zie voor meer informatie de [lokalisatie configureren](media-services-azure-media-clipper-localization.md) sectie.

## <a name="typescript-definition"></a>TypeScript-definitie
Een [TypeScript](https://www.typescriptlang.org/) definitiebestand voor de Clipper vindt [hier](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media Clipper API
In deze sectie worden de API-gebied geleverd door de Clipper.

- `ready(handler)`: biedt een manier om JavaScript uitgevoerd zodra de Clipper is volledig geladen en klaar om te worden gebruikt.
- `load(assets)`: een lijst van assets in de tijdlijn van de widget (mag niet worden gebruikt samen met assetsPanelLoaderCallback) laadt. Raadpleeg deze [artikel](media-services-azure-media-clipper-load-assets.md) voor meer informatie over het laden van assets in de Clipper.
- `setLogLevel(level)`: Hiermee stelt u het niveau van logboekregistratie moet worden weergegeven in de console van de browser. Mogelijke waarden zijn: `info`, `warn`, `error`.
- `setHeight(height)`: Hiermee stelt u de totale hoogte van de widget in pixels (minimumhoogte is 600 px zonder activa deelvenster en 850 px met het deelvenster activa).
- `version`: Hiermee wordt de versie van de widget opgehaald.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende stappen voor het configureren van Azure Media Clipper:
- [Het laden van assets in Azure Media Clipper](media-services-azure-media-clipper-load-assets.md)
- [Aangepaste toetscombinaties configureren](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Kniptaken uit de Clipper verzenden](media-services-azure-media-clipper-submit-job.md)
- [Lokalisatie configureren](media-services-azure-media-clipper-localization.md)