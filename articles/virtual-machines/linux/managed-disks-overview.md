---
title: Azure Disk-opslag overzicht van de schijf voor Linux-VM's beheerd | Microsoft Docs
description: Overzicht van Azure voor beheerde schijven die de storage-accounts voor u verwerkt bij het gebruik van virtuele Linux-machines
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/11/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: d8394121019338b53c87f7229d0ccf52fbdf8f21
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327024"
---
# <a name="introduction-to-azure-managed-disks"></a>Inleiding tot Azure beheerde schijven

Een door Azure beheerde schijf is een virtuele harde schijf (VHD). U kunt deze beschouwen als een fysieke schijf in een on-premises server maar gevirtualiseerde. Azure-beheerde schijven worden opgeslagen als pagina-blobs een willekeurige i/o-opslagobject in Azure zijn. Een beheerde schijf 'beheerde' omdat deze een abstractie via de pagina-blobs, blob-containers en Azure storage-accounts genoemd. Met beheerde schijven, u moet doen, is de schijf inrichten en Azure zorgt voor de rest.

Wanneer u selecteert om Azure te gebruiken beheerde schijven met uw workloads, Azure maakt en beheert de schijf voor u. De beschikbare typen schijven ultra schijven (Preview), premium SSD-schijven (SSD), standard-SSD, en standaard harde schijven (HDD). Zie voor meer informatie over elke afzonderlijke schijftype [een schijf selecteren voor IaaS-VM's](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selecteer een schijftype voor IaaS-VM 's](disks-types.md)
