---
title: Overzicht van Azure Disk Storage Managed disks voor Windows-Vm's | Microsoft Docs
description: Overzicht van Azure Managed disks, waarmee de opslag accounts voor u worden verwerkt wanneer u Azure Windows-Vm's gebruikt
services: virtual-machines-windows,storage
author: roygara
ms.service: virtual-machines-windows
ms.workload: storage
ms.tgt_pltfrm: vm-windows
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: fb1ee8556935b141dfee6a18c96ecafb476aa584
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "64725816"
---
# <a name="introduction-to-azure-managed-disks"></a>Inleiding tot Azure Managed disks

Een Azure Managed disk is een virtuele harde schijf (VHD). U kunt dit beschouwen als een fysieke schijf op een on-premises server, maar is gevirtualiseerd. Azure Managed disks worden opgeslagen als pagina-blobs, een wille keurig i/o-opslag object in Azure. Er wordt een beheerde schijf ' beheerd ' aangeroepen omdat het een abstractie is boven pagina-blobs, Blob-containers en Azure Storage-accounts. Met Managed disks hoeft u alleen maar de schijf in te richten en Azure zorgt voor de rest.

Wanneer u ervoor kiest om Azure Managed disks te gebruiken met uw workloads, wordt de schijf door Azure gemaakt en beheerd. De beschik bare typen schijven zijn ultra Solid-state drives (SSD), Premium-SSD, Standard-SSD en standaard harde schijven (HDD). Zie voor meer informatie over elk afzonderlijk schijf type [een schijf type selecteren voor IaaS vm's](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selecteer een schijf type voor IaaS Vm's](disks-types.md)