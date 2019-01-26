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
ms.date: 01/24/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 9f95be8a9d75065deedd3bd7c92907145e966494
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913050"
---
# <a name="scaling-media-processing"></a>Mediaverwerking schalen

Azure Media Services kunt u mediaverwerking schalen in uw account door het beheer van gereserveerde Media-eenheden (groepsbeleidsinstelling). Zie voor een gedetailleerd overzicht [mediaverwerking schalen](../previous/media-services-scale-media-processing-overview.md). 

In dit artikel ziet u hoe u [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) voor het schalen van de meest recent gebruikte.

> [!NOTE]
> Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 groepsbeleidsinstelling. <br/>Als u meer dan 10 S3 groepsbeleidsinstelling nodig hebt, opent u een ondersteuning ticket met de [Azure-portal](https://portal.azure.com/).

## <a name="prerequisites"></a>Vereisten 

+ Installeer de [Azure CLI]( /cli/azure/install-azure-cli). In dit artikel is Azure CLI versie 2.0 of hoger vereist. Voer `az --version` uit om te zien welke versie u hebt. 

    U kunt ook de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) gebruiken.
+ [Een Azure Media Services-account maken](create-account-cli-how-to.md).

## <a name="scale-media-reserved-units-with-cli"></a>Gereserveerde schaal Media-eenheden met CLI

1. Voer de opdracht `login` uit. Voer deze opdracht ongeacht of u van de Azure cloudshell of de lokale CLI shell gebruikmaakt.

    ```azurecli
    az login
    ```
    
    Als de CLI uw standaardbrowser kan openen, gebeurt dat ook en wordt er een aanmeldingspagina gedownload. Anders moet u een browserpagina openen en volg de instructies op de opdrachtregel voor het invoeren van een autorisatiecode na het navigeren naar [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) in uw browser.
2. Voer de opdracht `mru` uit.

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
