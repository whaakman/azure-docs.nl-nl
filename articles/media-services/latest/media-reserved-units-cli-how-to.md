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
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: dd587e5fc2082d1e496fbc05d5b25cf6692413bc
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713058"
---
# <a name="scaling-media-processing"></a>Mediaverwerking schalen

Azure Media Services kunt u mediaverwerking schalen in uw account door het beheer van gereserveerde Media-eenheden (groepsbeleidsinstelling). Zie voor een gedetailleerd overzicht [mediaverwerking schalen](../previous/media-services-scale-media-processing-overview.md). 

In dit artikel ziet u hoe u [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) voor het schalen van de meest recent gebruikte.

> [!NOTE]
> Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 groepsbeleidsinstelling. <br/>Als u meer dan 10 S3 groepsbeleidsinstelling nodig hebt, opent u een ondersteuning ticket met de [Azure-portal](https://portal.azure.com/).

## <a name="prerequisites"></a>Vereisten 

- Installeren en de CLI lokaal gebruikt, in dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Op dit moment niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) opdrachten werken in de Azure Cloud Shell. Het verdient de CLI lokaal gebruikt.

- [Een Media Services-account maken](create-account-cli-how-to.md).

## <a name="scale-media-reserved-units-with-cli"></a>Gereserveerde schaal Media-eenheden met CLI

De volgende [az ams-account mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) sets gereserveerde Media-eenheden op de 'amsaccount' rekening met de opdracht de **aantal** en **type** parameters.

```azurecli
az account set mru -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Billing

Er worden kosten berekend op basis van het aantal, het type en de hoeveelheid tijd die zijn ingericht met de meest recent gebruikte in uw account. Kosten van toepassing ongeacht of u taken uitvoeren of niet. Voor een gedetailleerde uitleg, Zie de sectie Veelgestelde vragen over van de [prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/) pagina.   

## <a name="next-step"></a>Volgende stap

[Video's analyseren](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
