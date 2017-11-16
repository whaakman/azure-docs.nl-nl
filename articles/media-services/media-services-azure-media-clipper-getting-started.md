---
title: Aan de slag met Azure Media openen | Microsoft Docs
description: Aan de slag met Azure Media openen, knipt een hulpprogramma voor het bouwen van media van activa
services: media-services
keywords: clip; subclip; codering; media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8a4f2c79131664ca0d078fa58c6a75b54243e705
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
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
- `language`{OPTIONEEL, tekenreeks}: taal de taal van de widget wordt ingesteld. Als niet wordt opgegeven, probeert de widget voor lokalisatie van de berichten op basis van de taal van de browser. Als er geen taal is gedetecteerd in de browser, wordt de widget wordt standaard ingesteld op Engels. Zie de sectie ondersteunde talen voor meer informatie.
- `languages`{OPTIONEEL, JSON}: de parameter talen vervangt de standaard-woordenlijst van talen door een aangepaste woordenlijst gedefinieerd door de gebruiker. Zie de sectie ondersteunde talen voor meer informatie.
- `extraLanguages`(Optioneel, JSON): de parameter extraLanaguages nieuwe talen toevoegen aan de standaardwoordenlijst. Zie de sectie ondersteunde talen voor meer informatie.

## <a name="typescript-definition"></a>Machineschrift definitie
Een [machineschrift](https://www.typescriptlang.org/) definitiebestand voor de openen vindt [hier](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media openen API
In dit gedeelte worden de API-gebied geleverd door het openen.

- `load(assets)`: een lijst met activa in het deelvenster activa laadt (mag niet worden gebruikt samen met `assetsPanelLoaderCallback`). Zie dit [artikel](media-services-azure-media-clipper-load-assets.md) voor meer informatie over het laden van de activa in de openen.
- `setLogLevel(level)`: Hiermee stelt u het logboekregistratieniveau moet worden weergegeven in de browser-console. Mogelijke waarden zijn: `info`, `warn`, `error`.
- `setHeight(height)`: Hiermee stelt u de totale hoogte van de widget in pixels (minimumhoogte is 600 px zonder activa deelvenster en 850 px met het deelvenster activa).
- `version`: de versie van de widget opgehaald.

## <a name="configuring-azure-media-clipper"></a>Configureren van Azure Media openen
Zie de volgende stappen voor het configureren van Azure Media openen:
- [Laden van assets in Azure Media openen](media-services-azure-media-clipper-load-assets.md)
- [Aangepaste sneltoetsen configureren](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Verzenden van taken van de openen knippen](media-services-azure-media-clipper-submit-job.md)

## <a name="supported-languages"></a>Ondersteunde talen
De widget openen is beschikbaar in 18 talen. Als u wilt de widget-taal instellen, moet u de `language` parameter tijdens de initialisatie. In de gewenste taal codetekenreeks doorgeven in de volgende lijst:
- Chinees (Vereenvoudigd): zh-hans
- Chinees (Traditioneel): zh-hant
- Tsjechisch: cs
- Nederlands, Vlaams: nl
- Engels: nl
- Frans: fr
- Duits: de
- Hongaars: hu
- Italiaans: het
- Japans: ja
- Koreaans: ko
- Pools: pl
- Portugees (Brazilië): pt-br
- Portugees (Portugal): pt-pt
- Russisch: ru
- Spaans: es
- Zweeds: sv
- Turks: tr

Als u wilt een aangepaste woordenlijst instellen of uitbreiden van de standaard taalwoordenlijst, definieert u de `languages` of `extraLanguages` parameter, respectievelijk. Doorgeven in een aangepaste woordenlijst met de volgende JSON-indeling:

```javascript
{
      "{language-code}":
          "{message-id}": "{message}"
          ...
      }
      ...
}
```

Bijvoorbeeld, definieert het volgende voorbeeld de gelokaliseerde tekenreeksen Engelse:

```javascript
export default {
  'VideoPlayer.noPreview': 'No video preview',
  'VideoPlayer.loadAsset': 'You must provide a valid asset',
  'AssetsPanel.name': 'Name',
  'AssetsPanel.type': 'Asset type',
  'AssetsPanel.actions': 'Actions',
  'AssetsPanel.loading': 'Loading...',
  'AssetsPanel.duration': 'Duration',
  'AssetsPanel.resolution': 'Resolution',
  'AssetsPanel.pluralFiles': '{0} assets',
  'AssetsPanel.searchFiles': 'Search assets',
  'AssetsPanel.showTypes': 'Show:',
  'AssetsPanel.typesInfo': 'Rendered assets are actual MP4 files. Dynamic manifest filters are filters applied to a parent asset\'s video segment playlist.',
  'AssetsPanel.filterTypes': 'Filters',
  'AssetsPanel.assetTypes': 'Assets',
  'AssetsPanel.assetsAll': 'All',
  'AssetsPanel.addAsset': 'Add asset to the end',
  'AssetsPanel.addFilter': 'Add filter to the timeline',
  'AssetsPanel.invalidAsset': 'The metadata of this asset is not compatible with the other assets in the timeline',
  'AssetsPanel.addAssetWarning': 'Subclipping on assets with different resolutions may cause resolution autoscaling.',
  'AssetsPanel.live': 'LIVE',
  'AssetsPanel.unknown': 'UNKNOWN',
  'AssetsPanel.minimGapNotMet': 'The asset duration must be greater than the minimum clip duration ({0} seconds)',
  'VideoPlayer.openAdvancedSettings': 'Advanced settings',
  'VideoPlayer.singleBitrate': 'Single-bitrate MP4 (rendered)',
  'VideoPlayer.multiBitrate': 'Multi-bitrate MP4 (rendered)',
  'VideoPlayer.dynamicManifest': 'Dynamic manifest filter',
  'VideoPlayer.ErrorWithMessage': 'There was an error in the video player, code {0}, message: {1}',
  'Common.cancel': 'Cancel',
  'Common.OK': 'OK',
  'AdvancedSettings.framerate': 'Frame rate',
  'Dropdown.select': 'Select an option...',
  'InputAsset.RemoveInput': 'Remove source',
  'Zoom.startTime': 'Start time',
  'Zoom.endTime': 'End time',
  'VideoPlayer.subclips': 'Subclips:',
  'VideoPlayer.length': 'Clip length:',
  'Accordion.scrollLeft': 'Scroll to the left',
  'Accordion.scrollRight': 'Scroll to the right',
  'AdvancedSettings.title': 'Advanced settings',
  'AdvancedSettings.subclipName': 'Subclip name',
  'AdvancedSettings.subclipType': 'Subclipping mode',
  'AdvancedSettings.includeAudioTracks': 'Include audio tracks',
  'AdvancedSettings.subclipTypeInfo': 'Single-bitrate and multi-bitrate MP4s are frame accurate rendered assets. Dynamic manifest filters are group-of-pictures (GOP) accurate filters applied to a parent asset. Creating filters does not create a new asset and does not require encoding. Subclipping jobs on live assets are valid as long as their mark times are within the archive window of the parent asset. Filters are valid as long as the parent asset exists and mark times are within its archive window.',
  'AdvancedSettings.frameRateInfo': 'We autodetect frame rate under most scenarios. however, If we cannot autodetect, choose a frame rate from the dropdown for the selected asset(s).',
  'AdvancedSettings.frameRateError': 'Unable to determine frame rate',
  'AdvancedSettings.subclipNameInfo': 'Choose a name for your subclip.',
  'AdvancedSettings.singleAudioTrack': '1 audio track selected',
  'AdvancedSettings.allAudioTracks': 'All audio tracks selected',
  'AdvancedSettings.someAudioTracks': '{0} audio tracks selected',
  'AdvancedSettings.includeAllAudioTracks': 'Include all audio tracks',
  'AssetsPanel.loadingError': 'Failed to retreive assets from server.',
  'AssetsPanel.retry': 'Retry?',
  'CommandBar.prevFrameTitle': 'Back up one frame',
  'CommandBar.prevKeyFrameTitle': 'Back up one GOP',
  'CommandBar.cleanJob': 'Remove all assets',
  'CommandBar.cleanJobTitle': 'Remove all assets from timeline',
  'CommandBar.cleanJobMessage': 'This will empty all video clips from your timeline.',
  'CommandBar.update': 'Update filter',
  'CommandBar.createFilter': 'Create filter',
  'CommandBar.submit': 'Submit subclipper job',
  'CommandBar.jobErrorTitle': 'Operation failed',
  'CommandBar.jobErrorMessage': 'Your subclip failed to submit. Please try again.',
  'CommandBar.markInTitle': 'Set in at playhead',
  'CommandBar.markInPosition': 'Mark in timecode',
  'CommandBar.markOutTitle': 'Set out at playhead',
  'CommandBar.markOutPosition': 'Mark out timecode',
  'CommandBar.nextFrameTitle': 'Advance one frame',
  'CommandBar.nextKeyFrameTitle': 'Advance one GOP',
  'CommandBar.play': 'Play video',
  'CommandBar.pause': 'Pause video',
  'CommandBar.playPreviewTitle': 'Play subclip preview',
  'CommandBar.pausePreviewTitle': 'Pause subclip preview',
  'CommandBar.redoTitle': 'Redo last action',
  'CommandBar.removeAsset': 'Remove current asset',
  'CommandBar.undoTitle': 'Undo last action',
  'VideoPlayer.errorTitle': 'Error',
  'VideoPlayer.errorMessage': 'There was an error loading the selected asset.',
  'Timeline.markIn': 'Mark in bracket',
  'Timeline.markOut': 'Mark out bracket',
  'Timeline.playHead': 'Play head',
};
```