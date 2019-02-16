---
title: Azure Data Box-Gateway beperkt | Microsoft Docs
description: Beschrijving van systeemlimieten en aanbevolen grootten voor de Microsoft Azure Data Box-Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/14/2019
ms.author: alkohli
ms.openlocfilehash: 34aac9a589516ace080906095ef3c14b34469bbd
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311185"
---
# <a name="azure-data-box-gateway-limits-preview"></a>Azure Data Box-Gateway-limieten (Preview)


Houd rekening met deze limieten bij het implementeren en uw Microsoft Azure Data Box-Gateway-oplossing werken. 

> [!IMPORTANT] 
> Data Box Gateway is in de previewfase. Lees de [gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert. 


## <a name="data-box-gateway-service-limits"></a>Gegevens in het Gateway-service-limieten

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-gateway-device-limits"></a>Gegevens in het Gateway-apparaat-limieten

De volgende tabel beschrijft de limieten voor de gegevens in het Gateway-apparaat.

| Description | Value |
|---|---|
|Nee. van bestanden per apparaat |100 miljoen <br> Limiet is ongeveer 25 miljoen bestanden voor elke 2 TB aan schijfruimte met maximale limiet op 100 miljoen |
|Nee. van shares per apparaat |24 |
|Nee. van shares per Azure-opslagcontainer |1 |
|Maximale bestandsgrootte die zijn geschreven naar een share|Maximale bestandsgrootte is voor een virtueel apparaat 2 TB 500 GB. <br> De maximale grootte wordt verhoogd met de grootte van de gegevensschijf in de voorgaande verhouding totdat een maximum van 5 TB is bereikt. |

## <a name="azure-storage-limits"></a>Limieten voor Azure-opslag

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Onder voorbehoud het uploaden van gegevens

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Azure-account grootte en object maximale grootte opslag

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]

## <a name="azure-object-size-limits"></a>De maximale grootte Azure-object

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Volgende stappen

- [Voorbereidingen voor de implementatie van Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
