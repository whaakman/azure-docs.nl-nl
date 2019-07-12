---
title: CLI gebruiken om filters te maken met Azure Media Services | Microsoft Docs
description: In dit onderwerp laat zien hoe CLI gebruiken om filters te maken met Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 26350479f0f066f45c143e1a35061b3a409de309
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786482"
---
# <a name="creating-filters-with-cli"></a>Filters maken met CLI 

Als uw inhoud levert aan klanten (Live-evenementen of Video on Demand streaming), moet uw client mogelijk meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-asset. Azure Media Services kunt u accountfilters en filters actief voor uw inhoud definiëren. 

Zie voor gedetailleerde beschrijving van deze functie en de scenario's waarin deze wordt gebruikt, [dynamische manifesten](filters-dynamic-manifest-overview.md) en [Filters](filters-concept.md).

Dit onderwerp wordt beschreven hoe u een filter voor een on-Demand Video asset configureren en te maken met CLI voor Media Services v3 [Accountfilters](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) en [Asset Filters](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

> [!NOTE]
> Controleer [presentationTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Vereisten 

- [Een Azure Media Services-account maken](create-account-cli-how-to.md). Zorg ervoor dat u de naam van de resourcegroep en de naam van de Media Services-account. 

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-filter"></a>Definieer een filter 

Het volgende voorbeeld definieert de track selectie voorwaarden die zijn toegevoegd aan het laatste manifest. Dit filter bevat alle audionummers die EG-3 en de video nummers die bitrate in de 0-1000000 hebben bereik.

> [!TIP]
> Als u van plan bent om te definiëren **Filters** in REST, ziet u dat u wilt opnemen van het 'Eigenschappen' wrapper JSON-object.  

```json
[
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Audio",
                "operation": "Equal"
            },
            {
                "property": "FourCC",
                "value": "EC-3",
                "operation": "NotEqual"
            }
        ]
    },
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Video",
                "operation": "Equal"
            },
            {
                "property": "Bitrate",
                "value": "0-1000000",
                "operation": "Equal"
            }
        ]
    }
]
```

## <a name="create-account-filters"></a>Accountfilters maken

De volgende [az ams-account-filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) opdracht maakt u een account-filter met filter bijhouden selecties die waren [eerder hebt gedefinieerd](#define-a-filter). 

Met de opdracht kunt u een optionele doorgeven `--tracks` parameter met de JSON voor de selecties bijhouden.  Gebruik @{file} JSON laden uit een bestand. Als u de Azure CLI lokaal gebruikt, geeft u het volledige bestandspad:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create-an-account-filter).

## <a name="create-asset-filters"></a>Asset-filters maken

De volgende [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) opdracht maakt u een filter asset met filter bijhouden selecties die waren [eerder hebt gedefinieerd](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create-an-asset-filter).

## <a name="associate-filters-with-streaming-locator"></a>Filters met Streaming-Locator gemaakt koppelen

U kunt een lijst met activa of account filters, die voor uw Streaming-Locator gemaakt gelden. De [dynamische Packager (Streaming-eindpunt)](dynamic-packaging-overview.md) deze lijst met filters samen met die de client Hiermee geeft u in de URL van toepassing is. Deze combinatie genereert een [dynamische Manifest](filters-dynamic-manifest-overview.md), die is gebaseerd op filters in de URL en filters die u op Streaming-Locator gemaakt opgeeft. U wordt aangeraden dat u deze functie gebruiken als u filters wilt toepassen, maar niet wilt weergeven van de filterfunctie namen in de URL.

De volgende CLI-code laat zien hoe u een Streaming-Locator te maken en opgeven `filters`. Dit is een optionele eigenschap waarmee een door spaties gescheiden lijst van asset filterfunctie namen en/of namen van filter.

```azurecli
az ams streaming-locator create -a amsAccount -g resourceGroup -n streamingLocatorName \
                                --asset-name assetName \                               
                                --streaming-policy-name policyName \
                                --filters filterName1 filterName2
                                
```

## <a name="stream-using-filters"></a>Stream met behulp van filters

Als u filters definiëren, kunnen uw clients deze gebruiken in de streaming-URL. Filters kunnen worden toegepast op adaptieve bitrate streamingprotocollen: Apple HTTP Live Streaming (HLS), MPEG-DASH en Smooth Streaming.

De volgende tabel ziet u enkele voorbeelden van URL's met filters:

|Protocol|Voorbeeld|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-step"></a>Volgende stap

[Stream-video 's](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
