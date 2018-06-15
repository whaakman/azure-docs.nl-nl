---
title: Problemen met schijven zijn gekoppeld aan Azure Virtual machines | Microsoft Docs
description: Azure Blob-opslag is ontworpen voor het opslaan van grote hoeveelheden ongestructureerde objectgegevens, zoals tekst of binaire gegevens. Uw toepassingen hebben toegang tot objecten in Blob-opslag vanuit PowerShell of via Azure CLI, vanuit code via Azure Storage-clientbibliotheken of via REST.
services: storage
author: genlin
manager: cshepard
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 766062b085c359499046151f337921a51d948715
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34362703"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Problemen met schijven zijn gekoppeld aan Azure Virtual machines 

Azure virtuele Machines (VM's) zijn afhankelijk van virtuele harde schijven (VHD's) voor de besturingssysteemschijf en alle gekoppeld gegevensschijven. VHD's worden opgeslagen als pagina-blobs in een of meer Azure Storage-accounts. In dit artikel verwijst naar het oplossen van inhoud voor veelvoorkomende problemen die zich met VHD's voordoen kunnen. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Problemen oplossen van fouten bij het verwijderen van opslag voor een virtuele machine

In bepaalde gevallen kan u een fout optreden tijdens het verwijderen van een opslagresource wanneer een virtuele machine in een implementatie van Resource Manager bevat virtuele harde schijven die zijn gekoppeld. Zie een van de volgende artikelen voor hulp bij het oplossen van dit probleem: 

  * Op Linux VM's: [fouten bij het verwijderen van opslag in de implementatie van Resource Manager](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Op Windows-VM's: [fouten bij het verwijderen van opslag in de implementatie van Resource Manager](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Problemen met virtuele machines met gekoppelde VHD's onverwacht opnieuw wordt opgestart

Als u een virtuele machine met een groot aantal gekoppelde VHD's onverwacht opnieuw wordt opgestart, ziet u een van de volgende artikelen:

  * Op Linux VM's: [onverwacht opnieuw wordt opgestart van virtuele machines met gekoppelde VHD's](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Op Windows-VM's: [onverwacht opnieuw wordt opgestart van virtuele machines met gekoppelde VHD's](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
