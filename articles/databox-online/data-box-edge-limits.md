---
title: Azure Data Box Edge beperkt | Microsoft Docs
description: Beschrijft systeemlimieten en aanbevolen grootten voor de rand van het Microsoft Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: a687398032adffe5e787ef1469abe0b2627a38fe
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403536"
---
# <a name="azure-data-box-edge-limits"></a>Azure Data Box-Edge-limieten

Houd rekening met deze limieten bij het implementeren en uw Microsoft Azure Data Box Edge-oplossing werken.

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
