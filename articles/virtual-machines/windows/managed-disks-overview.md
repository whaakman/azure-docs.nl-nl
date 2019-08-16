---
title: Overzicht van Azure Disk Storage Managed disks voor Windows-Vm's | Microsoft Docs
description: Overzicht van Azure Managed disks, waarmee de opslag accounts voor u worden verwerkt wanneer u Azure Windows-Vm's gebruikt
author: roygara
ms.service: virtual-machines-windows
ms.topic: overview
ms.date: 08/15/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: c71d51580c064b5b2b0bbbbf7bf95d50aa40f72f
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515563"
---
# <a name="introduction-to-azure-managed-disks"></a>Inleiding tot Azure Managed disks

Een Azure Managed disk is een virtuele harde schijf (VHD). U kunt dit beschouwen als een fysieke schijf op een on-premises server, maar is gevirtualiseerd. Azure Managed disks worden opgeslagen als pagina-blobs, een wille keurig i/o-opslag object in Azure. Er wordt een beheerde schijf ' beheerd ' aangeroepen omdat het een abstractie is boven pagina-blobs, Blob-containers en Azure Storage-accounts. Met Managed disks hoeft u alleen maar de schijf in te richten en Azure zorgt voor de rest.

Wanneer u ervoor kiest om Azure Managed disks te gebruiken met uw workloads, wordt de schijf door Azure gemaakt en beheerd. De beschik bare typen schijven zijn ultra disk, Premium Solid State Drive (SSD), Standard-SSD en een standaard harde schijf (HDD). Zie voor meer informatie over elk afzonderlijk schijf type [een schijf type selecteren voor IaaS vm's](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selecteer een schijf type voor IaaS Vm's](disks-types.md)