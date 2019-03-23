---
title: CLI gebruiken voor het schalen van gereserveerde Media-eenheden - Azure | Microsoft Docs
description: Dit onderwerp leest hoe u met CLI mediaverwerking schalen met Azure Media Services.
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
ms.date: 03/21/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 3f0d3c5748afaac2544232fc1ff84316d9eb7347
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351062"
---
# <a name="scaling-media-processing"></a>Mediaverwerking schalen

Azure Media Services kunt u mediaverwerking schalen in uw account door het beheer van gereserveerde Media-eenheden (groepsbeleidsinstelling). De meest recent gebruikte bepaalt de snelheid waarmee uw multimedia standaardtaken voor de verwerking worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2**, of **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**). 

Naast het opgeven van de gereserveerde-eenheidstype, kunt u opgeven voor het inrichten van uw account met gereserveerde eenheden. Op basis van het aantal ingerichte, gereserveerde eenheden wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account. Bijvoorbeeld, als uw account vijf gereserveerde eenheden, heeft en vervolgens vijf mediataken gelijktijdig zo lang zal worden uitgevoerd als er zijn taken die moeten worden verwerkt. De resterende taken in de wachtrij wordt gewacht en wordt u opgehaald voor het verwerken van sequentieel worden verwerkt wanneer een actieve taak is voltooid. Als een account heeft geen alle gereserveerde eenheden worden ingericht, klikt u vervolgens taken opgehaald sequentieel worden verwerkt. In dit geval afhankelijk de wachttijd tussen één taak is voltooid en de volgende begin van de beschikbaarheid van resources in het systeem.

## <a name="choosing-between-different-reserved-unit-types"></a>Kiezen tussen verschillende gereserveerde-eenheidstypen

De volgende tabel kunt u een beslissing te nemen bij de keuze tussen verschillende snelheden voor codering. Het bevat ook enkele benchmark gevallen op [een video die u kunt downloaden](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) het uitvoeren van uw eigen tests:

|RU-type|Scenario|Voorbeeld van resultaten voor de [7 min 1080 p video](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z)|
|---|---|---|
| **S1**|Single-bitrate codering. <br/>Bestanden op SD of onder oplossingen, tijd niet gevoelige, lage kosten.|Codering op single bitrate SD resolutie MP4-bestand met behulp van 'H264 Single Bitrate SD 16 x 9' duurt tien minuten.|
| **S2**|Single-bitrate en meerdere bitrate codering.<br/>Normaal gebruik voor zowel SD en HD-codering.|Codering met "H264 Single-Bitrate 720p" vooraf duurt ongeveer 8 minuten.<br/><br/>Codering met ' H264 Multiple Bitrate 720p "vooraf ingestelde duurt ongeveer 16,8 minuten.|
| **S3**|Single-bitrate en meerdere bitrate codering.<br/>Volledige HD en 4K resolutie van video's. Tijd gevoelige en snellere doorlooptijd codering.|Codering met "H264 Single-Bitrate 1080p" vooraf duurt ongeveer vier minuten.<br/><br/>Codering met ' H264 Multiple Bitrate 1080p "vooraf ingestelde duurt ongeveer 8 minuten.|

## <a name="considerations"></a>Overwegingen

* S3-eenheidstype wordt voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, sterk aanbevolen.
* Als de gedeelde groep wordt gebruikt, dat wil zeggen, zonder eventuele gereserveerde eenheden, klikt u vervolgens uw coderen taken hebben dezelfde prestaties net als bij de S1-ME. Er is geen bovengrens voor de tijd dat uw taken kunnen besteden aan de status in wachtrij echter en op elk moment maximaal slechts één taak wordt uitgevoerd.

De rest van het artikel ziet u hoe u [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) voor het schalen van de meest recent gebruikte.

> [!NOTE]
> Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 groepsbeleidsinstelling. Als u meer dan 10 S3 groepsbeleidsinstelling nodig hebt, opent u een ondersteuning ticket met de [Azure-portal](https://portal.azure.com/).
>
> U kunt de Azure-portal op dit moment niet gebruiken om andere v3-resources te beheren. Gebruik de [REST-API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref), of een van de ondersteunde [SDK's](developers-guide.md).

## <a name="prerequisites"></a>Vereisten 

[Een Azure Media Services-account maken](create-account-cli-how-to.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>Gereserveerde schaal Media-eenheden met CLI

Voer de opdracht `mru` uit.

De volgende [az ams-account mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) sets gereserveerde Media-eenheden op de 'amsaccount' rekening met de opdracht de **aantal** en **type** parameters.

```azurecli
az account set mru -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Billing

Er worden kosten berekend op basis van het aantal, het type en de hoeveelheid tijd die zijn ingericht met de meest recent gebruikte in uw account. Kosten van toepassing ongeacht of u taken uitvoeren of niet. Voor een gedetailleerde uitleg, Zie de sectie Veelgestelde vragen over van de [prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/) pagina.   

## <a name="next-step"></a>Volgende stap

[Video's analyseren](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Zie ook

* [Quota en beperkingen](limits-quotas-constraints.md)
* [Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
