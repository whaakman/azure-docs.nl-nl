---
title: Overzicht van Azure Disk Storage Managed disk voor Linux-Vm's | Microsoft Docs
description: Overzicht van Azure Managed disks, waarmee de opslag accounts voor u worden verwerkt wanneer Linux Vm's worden gebruikt
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 18dc1bd2eea232d0c2eb73d496dd4bd9d2d5016e
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "64707801"
---
# <a name="introduction-to-azure-managed-disks"></a>Inleiding tot Azure Managed disks

Een Azure Managed disk is een virtuele harde schijf (VHD). U kunt dit beschouwen als een fysieke schijf op een on-premises server, maar is gevirtualiseerd. Azure Managed disks worden opgeslagen als pagina-blobs, een wille keurig i/o-opslag object in Azure. Er wordt een beheerde schijf ' beheerd ' aangeroepen omdat het een abstractie is boven pagina-blobs, Blob-containers en Azure Storage-accounts. Met Managed disks hoeft u alleen maar de schijf in te richten en Azure zorgt voor de rest.

Wanneer u ervoor kiest om Azure Managed disks te gebruiken met uw workloads, wordt de schijf door Azure gemaakt en beheerd. De beschik bare typen schijven zijn ultra disks (preview), Premium-schijven (Solid-state drives), standaard SSD en standaard harde schijven (HDD). Zie voor meer informatie over elk afzonderlijk schijf type [een schijf type selecteren voor IaaS vm's](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selecteer een schijf type voor IaaS Vm's](disks-types.md)
