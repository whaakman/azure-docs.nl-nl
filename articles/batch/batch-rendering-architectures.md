---
title: Rendering van Azure-referentiearchitecturen - Azure Batch
description: Architecturen voor het gebruik van Azure Batch en andere Azure-services uit te breiden van een on-premises render farm door te sturen naar de cloud
services: batch
author: davefellows
manager: jeconnoc
ms.author: lahugh
ms.date: 08/13/2018
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: d5102ba94e2b7808a457df00a87b35ef7022c454
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53543492"
---
# <a name="reference-architectures-for-azure-rendering"></a>Referentiemateriaal voor architecturen voor Azure rendering

In dit artikel geeft een architectuur op hoog niveau diagrammen voor scenario's uit te breiden, of 'burst', een on-premises farm renderen naar Azure. De voorbeelden tonen verschillende opties voor Azure compute, netwerk- en -services.

## <a name="hybrid-with-nfs-or-cfs"></a>Hybride met NFS- of CFS

Het volgende diagram toont een hybride scenario met de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Met Azure ExpressRoute of VPN. Azure: Azure VNet.

* **Opslag** - invoer en uitvoer bestanden: NFS- of met behulp van Azure-VM's, gesynchroniseerd met on-premises opslag via Azure File Sync of RSync CFS.

  ![Cloudbursting - hybride oplossing met NFS- of CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs.png)

## <a name="hybrid-with-blobfuse"></a>Hybride met Blobfuse

Het volgende diagram toont een hybride scenario met de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Met Azure ExpressRoute of VPN. Azure: Azure VNet.

* **Opslag** - invoer en uitvoer bestanden: BLOB-opslag gekoppeld voor de compute-resources via Azure Blobfuse.

  ![Cloudbursting - hybride oplossing met Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hybride berekeningen en opslag

Het volgende diagram ziet u een volledig verbonden hybride scenario voor berekeningen en opslag en bevat de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Met Azure ExpressRoute of VPN. Azure: Azure VNet.

* **Opslag** -Cross-premises: Avere vFXT. Optionele archiveren van on-premises bestanden via Azure Data Box naar Blob-opslag.

  ![Cloudbursting - hybride berekeningen en opslag](./media/batch-rendering-architectures/hybrid-compute-storage.png)


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het gebruik van [renderen managers](batch-rendering-render-managers.md) met Azure Batch.

* Meer informatie over opties voor [Rendering in Azure](batch-rendering-service.md).