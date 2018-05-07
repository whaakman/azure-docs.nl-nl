---
title: Problemen met virtuele machines met gekoppelde VHD's op Azure VM's van Windows onverwacht opnieuw wordt opgestart | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van VM's van Windows onverwacht opnieuw wordt opgestart.
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-windows
author: tamram
manager: jeconnoc
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/01/2018
ms.author: tamram
ms.openlocfilehash: 2057ffe2d5ec61e1192ff12291114c43dc2cc049
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Problemen met virtuele machines met gekoppelde VHD's onverwacht opnieuw wordt opgestart

Als een Azure-virtuele Machine (VM) een groot aantal gekoppelde VHD's die zich in hetzelfde opslagaccount heeft, misschien u overschrijdt de schaalbaarheidsdoelen voor een afzonderlijke opslagaccount, waardoor de virtuele machine op de computer onverwacht opnieuw worden opgestart. Controleer de minuut metrische gegevens voor het opslagaccount (**TotalRequests**/**TotalIngress**/**TotalEgress**) pieken die groter is dan de schaalbaarheidsdoelen voor een opslagaccount. Zie [metrische gegevens tonen een toename in PercentThrottlingError](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) voor hulp bij het bepalen of beperking van uw opslagaccount heeft plaatsgevonden.

In het algemeen elke afzonderlijke invoer of uitvoerbewerking op een VHD van een virtuele Machine wordt omgezet in **downloaden pagina** of **pagina plaatsen** bewerkingen op de onderliggende paginablob. Daarom kunt u het geschatte aantal IOPS dat voor uw omgeving om af te stemmen hoeveel VHD's die u kunt hebben in een één opslagaccount op basis van het specifieke gedrag van uw toepassing. Microsoft raadt 40 of minder schijven in een enkel opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md) voor meer informatie over schaalbaarheidsdoelen voor storage-accounts, met name het totale percentage en de totale bandbreedte van het type opslagaccount u gebruikt.

Als u de van schaalbaarheidsdoelen voor uw opslagaccount overschreden, plaatst u uw VHD's in meerdere opslagaccounts te verminderen van de activiteit in elke rekening.
