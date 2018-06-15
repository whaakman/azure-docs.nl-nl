---
title: Problemen met virtuele machines met gekoppelde VHD's op Azure Linux VM's onverwacht opnieuw wordt opgestart | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van virtuele Linux-machines onverwacht opnieuw wordt opgestart.
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-linux
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 29aecf748ddda280e6f42be0bb3c986d90a3d7f9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34364417"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Problemen met virtuele machines met gekoppelde VHD's onverwacht opnieuw wordt opgestart

Als een Azure-virtuele Machine (VM) een groot aantal gekoppelde VHD's die zich in hetzelfde opslagaccount heeft, misschien u overschrijdt de schaalbaarheidsdoelen voor een afzonderlijke opslagaccount, waardoor de virtuele machine op de computer onverwacht opnieuw worden opgestart. Controleer de minuut metrische gegevens voor het opslagaccount (**TotalRequests**/**TotalIngress**/**TotalEgress**) pieken die groter is dan de schaalbaarheidsdoelen voor een opslagaccount. Zie [metrische gegevens tonen een toename in PercentThrottlingError](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) voor hulp bij het bepalen of beperking van uw opslagaccount heeft plaatsgevonden.

In het algemeen elke afzonderlijke invoer of uitvoerbewerking op een VHD van een virtuele Machine wordt omgezet in **downloaden pagina** of **pagina plaatsen** bewerkingen op de onderliggende paginablob. Daarom kunt u het geschatte aantal IOPS dat voor uw omgeving om af te stemmen hoeveel VHD's die u kunt hebben in een één opslagaccount op basis van het specifieke gedrag van uw toepassing. Microsoft raadt 40 of minder schijven in een enkel opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md) voor meer informatie over schaalbaarheidsdoelen voor storage-accounts, met name het totale percentage en de totale bandbreedte van het type opslagaccount u gebruikt.

Als u de van schaalbaarheidsdoelen voor uw opslagaccount overschreden, plaatst u uw VHD's in meerdere opslagaccounts te verminderen van de activiteit in elke rekening.
