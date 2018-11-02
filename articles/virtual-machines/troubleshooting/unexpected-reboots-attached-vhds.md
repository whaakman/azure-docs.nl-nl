---
title: Onverwacht opnieuw opstarten van virtuele machines met gekoppelde VHD's op Azure VM's oplossen | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van onverwacht opnieuw opstarten van virtuele machines.
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 6273087e28be8b784168d5808918d04d0e4cf303
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748150"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Onverwacht opnieuw opstarten van virtuele machines met gekoppelde VHD's oplossen

Als een Azure-virtuele Machine (VM) een groot aantal gekoppelde VHD's die zich in hetzelfde opslagaccount heeft, mag u overschrijden de schaalbaarheidsdoelen voor een afzonderlijke storage-account, waardoor de virtuele machine opnieuw op te starten onverwacht afgesloten. Controleer de minuut metrische gegevens voor het opslagaccount (**TotalRequests**/**TotalIngress**/**TotalEgress**) voor pieken vertoont die groter zijn dan de schaalbaarheidsdoelen voor een opslagaccount. Zie [metrische gegevens geven een toename in percentthrottlingerror aan](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) voor hulp bij het bepalen of beperking is opgetreden op uw storage-account.

In het algemeen elke afzonderlijke invoer of uitvoerbewerking op een VHD van een virtuele Machine wordt omgezet in het **opvragen pagina** of **pagina plaatsen** bewerkingen op de onderliggende pagina-blob. Daarom kunt u de geschatte IOP's voor uw omgeving om af te stemmen hoeveel VHD's die u kunt hebben in een enkel opslagaccount op basis van de specifieke gedrag van uw toepassing. Microsoft raadt 40 of minder schijven in een enkel opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md) voor meer informatie over de schaalbaarheidsdoelen voor storage-accounts, met name de totale aanvraagsnelheid en de totale bandbreedte van het type opslagaccount dat u gebruikt.

Als u de schaalbaarheidsdoelen voor uw opslagaccount overschrijden, plaatst u uw VHD's in meerdere opslagaccounts te verminderen van de activiteit in elke afzonderlijke account.
