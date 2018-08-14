---
title: Rendering van Azure - referentiearchitecturen
description: Architecturen voor het gebruik van Azure Batch en andere Azure-services uit te breiden van een on-premises render farm door te sturen naar de cloud
services: batch
author: davefellows
manager: jeconnoc
ms.author: danlep
ms.date: 08/13/2018
ms.topic: conceptual
ms.openlocfilehash: 0fe101ee6eb88094034b90c4d39f06ba509c9512
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "40099872"
---
# <a name="reference-architectures-for-azure-rendering"></a>Referentiemateriaal voor architecturen voor Azure rendering

In dit artikel geeft een architectuur op hoog niveau diagrammen voor scenario's uit te breiden, of 'burst', een on-premises farm renderen naar Azure. De voorbeelden tonen verschillende opties voor Azure compute, netwerk- en -services.

## <a name="hybrid-with-nfs-or-cfs"></a>Hybride met NFS- of CFS

Het volgende diagram toont een hybride scenario met de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Azure ExpressRoute of VPN. Azure: Azure VNet.

* **Opslag** - invoer en uitvoer bestanden: NFS- of CFS met behulp van Azure-VM's die zijn gesynchroniseerd met on-premises opslag via Azure File Sync of RSync.

  ![Cloudbursting - hybride oplossing met NFS- of CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs.png)

## <a name="hybrid-with-blobfuse"></a>Hybride met Blobfuse

Het volgende diagram toont een hybride scenario met de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Azure ExpressRoute of VPN. Azure: Azure VNet.

* **Opslag** - invoer en uitvoer bestanden: Blob storage, die rekenresources via Azure Blobfuse gekoppeld.

  ![Cloudbursting - hybride oplossing met Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hybride berekeningen en opslag

Het volgende diagram ziet u een volledig verbonden hybride scenario voor berekeningen en opslag en bevat de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Azure ExpressRoute of VPN. Azure: Azure VNet.

* **Opslag** -Cross-premises: Avere vFXT. Optionele archiveren van on-premises bestanden via Azure Data Box naar Blob-opslag.

  ![Cloudbursting - hybride berekeningen en opslag](./media/batch-rendering-architectures/hybrid-compute-storage.png)


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het gebruik van [renderen managers](batch-rendering-render-managers.md) met Azure Batch.

* Meer informatie over opties voor [Rendering in Azure](batch-rendering-service.md).