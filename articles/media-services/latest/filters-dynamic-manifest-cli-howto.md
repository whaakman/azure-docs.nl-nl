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
ms.date: 11/26/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 471277433f0fc9a54a28baa158f1e20f1efb613f
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000511"
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

Het volgende voorbeeld definieert de track selectie voorwaarden die zijn toegevoegd aan het laatste manifest. Dit filter bevat alle audionummers die EG-3 en de video nummers die bitrate in de 0-1000000 hebben bereik.

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
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @c:\tracks.json
```

Als u de Azure Cloud Shell gebruikt, moet u het bestand uploaden naar de Cloud Shell (zoeken naar de knop uploaden/downloaden van bestanden aan de bovenkant van de shell-venster). U kunt vervolgens verwijzen naar het bestand als volgt:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Asset-filters maken

De volgende [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) opdracht maakt u een filter asset met filter bijhouden selecties die waren [eerder hebt gedefinieerd](#define-a-filter). 

> [!TIP]
> Zie de informatie over het opgeven van de locatie van de bestandsnaam in de vorige sectie.

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Volgende stap

[Stream-video 's](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
