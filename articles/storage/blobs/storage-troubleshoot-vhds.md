---
title: Schijven zijn gekoppeld aan Azure-VM's oplossen | Microsoft Docs
description: Vindt u koppelingen naar bronnen voor probleemoplossing voor de virtuele machine van Azure virtuele harde schijven (VHD's).
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: bf1aa75399f28b8dd1732bc9ec09c15e46132939
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414163"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Schijven zijn gekoppeld aan Azure-VM's oplossen 

Virtuele Azure-Machines (VM's), is afhankelijk van virtuele Hardeschijven (VHD's) voor de schijf van het besturingssysteem en eventuele gegevensschijven. VHD's worden opgeslagen als pagina-blobs in een of meer Azure Storage-accounts. In dit artikel verwijst naar de inhoud voor veelvoorkomende problemen die met VHD's optreden kunnen van de probleemoplossing. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Fouten bij het verwijderen van de opslag voor een virtuele machine oplossen

In bepaalde gevallen kan u een fout optreden tijdens het verwijderen van een opslagresource wanneer een virtuele machine in een Resource Manager-implementatie bevat VHD's die zijn gekoppeld. Zie voor hulp bij het oplossen van dit probleem, een van de volgende artikelen: 

  * Op Linux-VM's: [fouten bij het verwijderen van opslag in Resource Manager-implementatie](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Op Windows-VM's: [fouten bij het verwijderen van opslag in Resource Manager-implementatie](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Onverwacht opnieuw opstarten van virtuele machines met gekoppelde VHD's oplossen

Als er onverwacht opnieuw opstarten van een virtuele machine met een groot aantal gekoppelde VHD's, ziet u een van de volgende artikelen:

  * Op Linux-VM's: [onverwacht opnieuw wordt opgestart van VM's met gekoppelde VHD's](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Op Windows-VM's: [onverwacht opnieuw wordt opgestart van VM's met gekoppelde VHD's](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
