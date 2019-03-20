---
title: Azure Media Clipper lokalisatie configureren | Microsoft Docs
description: Meer informatie over Azure Media Clipper ondersteunde talen en lokalisatieondersteuning voor
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 41fd483a0b78619db6d018cf4b2ab7933a7c3031
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58015255"
---
# <a name="configure-localization"></a>Lokalisatie configureren 

Azure Media Clipper is beschikbaar in 18 talen. Als u wilt de taal van de widget instellen, moet u definiëren de `language` parameter tijdens de initialisatie. In de gewenste taal code-tekenreeks doorgeven in de volgende lijst:
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

Als u een aangepaste woordenlijst instellen of uitbreiden van de standaard-woordenlijst, definieert u de `languages` of `extraLanguages` parameter, respectievelijk. Doorgeven in een aangepaste woordenlijst met de volgende JSON-indeling:

```javascript
{
    '{language-code}': {
        '{message-id}': '{message}',
        ...
    }
    ...
}
```

Bijvoorbeeld, definieert het volgende voorbeeld de gelokaliseerde tekenreeksen in de Engelse:

```javascript
{
    'en': {
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
        'AssetsPanel.loadingError': 'Failed to retrieve assets from server.',
        'AssetsPanel.retry': 'Retry?',
        'CommandBar.prevFrameTitle': 'Move one frame backwards',
        'CommandBar.prevKeyFrameTitle': 'Move one GOP backwards',
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
        'CommandBar.nextFrameTitle': 'Move one frame forward',
        'CommandBar.nextKeyFrameTitle': 'Move one GOP forward',
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
        'Timeline.playHead': 'Play head'
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende stappen voor het configureren van Azure Media Clipper:
- [Het laden van assets in Azure Media Clipper](media-services-azure-media-clipper-load-assets.md)
- [Aangepaste toetscombinaties configureren](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Kniptaken uit de Clipper verzenden](media-services-azure-media-clipper-submit-job.md)
