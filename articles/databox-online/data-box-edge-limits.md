---
title: Azure Data Box Edge beperkt | Microsoft Docs
description: Beschrijft systeemlimieten en aanbevolen grootten voor de rand van het Microsoft Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 02/05/2019
ms.author: alkohli
ms.openlocfilehash: 30e0c37d3d0c03e77b6dab9c06c0a50bff27e8bc
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967301"
---
# <a name="azure-data-box-edge-limits-preview"></a>Limieten voor Azure Data Box Edge (Preview)

Houd rekening met deze limieten bij het implementeren en uw Microsoft Azure Data Box Edge-oplossing werken.

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert.


## <a name="data-box-edge-service-limits"></a>Gegevens in het Edge-Servicelimieten

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-edge-device-limits"></a>Gegevenslimieten in het Edge-apparaat

De volgende tabel beschrijft de limieten voor de gegevens in het Edge-apparaat.

| Description | Value |
|---|---|
|Nee. van bestanden per apparaat |100 miljoen |
|Nee. van shares per apparaat |24 |
|Nee. van shares per container |1 |
|Maximale bestandsgrootte die zijn geschreven naar een share| 5 TB |

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
