---
title: Azure Disk-opslag overzicht van de schijf voor Linux-VM's beheerd | Microsoft Docs
description: Overzicht van Azure voor beheerde schijven die de storage-accounts voor u verwerkt bij het gebruik van virtuele Linux-machines
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 18dc1bd2eea232d0c2eb73d496dd4bd9d2d5016e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707801"
---
# <a name="introduction-to-azure-managed-disks"></a>Inleiding tot Azure beheerde schijven

Een door Azure beheerde schijf is een virtuele harde schijf (VHD). U kunt deze beschouwen als een fysieke schijf in een on-premises server maar gevirtualiseerde. Azure-beheerde schijven worden opgeslagen als pagina-blobs een willekeurige i/o-opslagobject in Azure zijn. Een beheerde schijf 'beheerde' omdat deze een abstractie via de pagina-blobs, blob-containers en Azure storage-accounts genoemd. Met beheerde schijven, u moet doen, is de schijf inrichten en Azure zorgt voor de rest.

Wanneer u selecteert om Azure te gebruiken beheerde schijven met uw workloads, Azure maakt en beheert de schijf voor u. De beschikbare typen schijven ultra schijven (Preview), premium SSD-schijven (SSD), standard-SSD, en standaard harde schijven (HDD). Zie voor meer informatie over elke afzonderlijke schijftype [een schijf selecteren voor IaaS-VM's](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selecteer een schijftype voor IaaS-VM 's](disks-types.md)
