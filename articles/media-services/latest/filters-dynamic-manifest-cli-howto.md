---
title: Schalen van Media gereserveerde eenheden - Azure | Microsoft Docs
description: In dit onderwerp wordt een overzicht van het vergroten/verkleinen Media verwerken met Azure Media Services.
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
ms.date: 11/26/2018
ms.author: juliako
ms.openlocfilehash: 9099429097efb17629e88318430f004f0f763cc5
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336862"
---
# <a name="creating-filters-with-cli"></a>Filters maken met CLI 

Als uw inhoud levert aan klanten (Live-evenementen of Video on Demand streaming), moet uw client mogelijk meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-asset. Azure Media Services kunt u accountfilters en filters actief voor uw inhoud definiëren. Zie voor meer informatie, [Filters en dynamische manifesten](filters-dynamic-manifest-overview.md).

Dit onderwerp wordt beschreven hoe u een filter voor een on-Demand Video asset configureren en te maken met CLI voor Media Services v3 [Accountfilters](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) en [Asset Filters](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="prerequisites"></a>Vereisten 

- Installeer en gebruik de CLI lokaal. Voor dit artikel dient u gebruik te maken van Azure CLI, versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Momenteel werken niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref)-opdrachten in Azure Cloud Shell. U wordt aangeraden de CLI lokaal te gebruiken.
- [Een Azure Media Services-account maken](create-account-cli-how-to.md). Zorg ervoor dat u de naam van de resourcegroep en de naam van de Media Services-account. 
- Beoordeling [Filters en dynamische manifesten](filters-dynamic-manifest-overview.md).

## <a name="define-a-filter"></a>Definieer een filter 

Het volgende voorbeeld definieert de track selectie voorwaarden die zijn toegevoegd aan het laatste manifest. Dit filter bevat alle audionummers die Engels met EG 3 en de video nummers die bitrate in de 0-1000000 hebben bereik.

Filters zijn gedefinieerd in REST, bevatten de 'Eigenschappen' wrapper JSON-object.  

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
                "property": "Language",
                "value": "en",
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

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @C:\tracks.json
```

Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Asset-filters maken

De volgende [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) opdracht maakt u een filter asset met filter bijhouden selecties die waren [eerder hebt gedefinieerd](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @C:\tracks.json
```

Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Volgende stap

[Stream-video 's](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
