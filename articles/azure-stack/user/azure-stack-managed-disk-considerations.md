---
title: Verschillen en overwegingen voor Managed Disks in Azure Stack | Microsoft Docs
description: Meer informatie over de verschillen en overwegingen bij het werken met beheerde schijven in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 2870bf8911c48ac4cbb442278f172b37a474152b
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078050"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack-beheerde schijven: De verschillen en overwegingen met betrekking tot
In dit artikel bevat een overzicht van de bekende verschillen tussen Azure Stack Managed Disks en Managed Disks voor Azure. Zie voor meer informatie over belangrijke verschillen tussen Azure Stack en Azure, de [belangrijke overwegingen met betrekking tot](azure-stack-considerations.md) artikel.

Beheerde schijven vereenvoudigt u Schijfbeheer voor IaaS-VM's door het beheer van de [opslagaccounts](/azure/azure-stack/azure-stack-manage-storage-accounts) die zijn gekoppeld aan de VM-schijven.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Overzichtskaart: beheerde schijf verschillen

| Functie | Azure (wereldwijd) | Azure Stack |
| --- | --- | --- |
|Versleuteling van Data-at-Rest |Azure Storage Service Encryption (SSE), Azure Disk Encryption (ADE)     |BitLocker-128-bits AES-versleuteling      |
|Installatiekopie          | Ondersteuning voor beheerde aangepaste installatiekopie |Nog niet ondersteund|
|Opties voor back-up |Ondersteuning voor Azure Backup-Service |Nog niet ondersteund |
|Opties voor herstel na noodgevallen |Ondersteuning voor Azure Site Recovery |Nog niet ondersteund|
|Schijftypen     |Premium SSD, Standard-SSD (Preview) en Standard HDD |Premium SSD, standaard harde schijven |
|Premium-schijven  |Volledig ondersteund |Kan worden ingericht, maar er is geen prestatielimiet voor of garanderen  |
|Premium-schijven IOPs  |Afhankelijk van de schijfgrootte  |2300 IOP's per schijf |
|Doorvoer van Premium-schijven |Afhankelijk van de schijfgrootte |145 MB/s per schijf |
|Maximale grootte van schijf  |4 TB       |1 TB       |
|Schijven prestaties analytische |Cumulatieve metrische gegevens en per schijf ondersteunde metrische gegevens |Nog niet ondersteund |
|Migratie      |Hulpprogramma voor het migreren van bestaande niet-beheerde Azure Resource Manager VM's zonder de noodzaak om de virtuele machine opnieuw te bieden  |Nog niet ondersteund |


## <a name="metrics"></a>Metrische gegevens
Er zijn ook verschillen met metrische gegevens over opslag:
- Met Azure Stack onderscheiden niet de transactiegegevens in metrische opslaggegevens intern of extern netwerkbandbreedte.
- Azure Stack-transactiegegevens in metrische gegevens van storage bevat geen virtuele machine toegang tot de gekoppelde schijven.


## <a name="api-versions"></a>API-versies
Azure Stack Managed Disks ondersteunt de volgende API-versies:
- 2017-03-30


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over virtuele machines van Azure Stack](azure-stack-compute-overview.md)
