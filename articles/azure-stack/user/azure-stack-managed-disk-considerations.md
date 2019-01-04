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
ms.openlocfilehash: 4bad339ca704f14f57c120db5e731d0dbfeb8d59
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793430"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack beheerde schijven: Verschillen en overwegingen
In dit artikel bevat een overzicht van de bekende verschillen tussen Azure Stack Managed Disks en Managed Disks voor Azure. Zie voor meer informatie over belangrijke verschillen tussen Azure Stack en Azure, de [belangrijke overwegingen met betrekking tot](azure-stack-considerations.md) artikel.

Beheerde schijven vereenvoudigt u Schijfbeheer voor IaaS-VM's door het beheer van de [opslagaccounts](/azure/azure-stack/azure-stack-manage-storage-accounts) die zijn gekoppeld aan de VM-schijven.

> [!Note]  
> Beheerde schijven in Azure Stack is beschikbaar via 1808.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Overzichtskaart: Beheerde schijf verschillen

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
|Schijfgrootte  |Azure Premium-schijf: P4 (32 GiB) naar P80 (32 TiB)<br>Azure Standard-SSD-schijf: E10 (128 GiB) naar E80 (32 TiB)<br>Azure Standard harde schijf: S4 (32 GiB) naar S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1024 giB |
|Exemplaar van de momentopname schijven|Momentopname maken van Azure beheerde schijven zijn gekoppeld aan een actieve virtuele machine die wordt ondersteund|Nog niet ondersteund |
|Schijven prestaties analytische |Cumulatieve metrische gegevens en per schijf ondersteunde metrische gegevens |Nog niet ondersteund |
|Migratie      |Hulpprogramma voor het migreren van bestaande niet-beheerde Azure Resource Manager VM's zonder de noodzaak om de virtuele machine opnieuw te bieden  |Nog niet ondersteund |

> [!Note]  
> Beheerde schijven IOPs en doorvoer in Azure Stack is een maximum aantal in plaats van een ingerichte nummer, dat mogelijk beïnvloed door de hardware- en workloads die worden uitgevoerd in Azure Stack.


## <a name="metrics"></a>Metrische gegevens
Er zijn ook verschillen met metrische gegevens over opslag:
- Met Azure Stack onderscheiden niet de transactiegegevens in metrische opslaggegevens intern of extern netwerkbandbreedte.
- Azure Stack-transactiegegevens in metrische gegevens van storage bevat geen virtuele machine toegang tot de gekoppelde schijven.


## <a name="api-versions"></a>API-versies
Azure Stack Managed Disks ondersteunt de volgende API-versies:
- 2017-03-30

## <a name="known-issues"></a>Bekende problemen
Na het toepassen van de 1809 bijwerken, kunnen de volgende problemen optreden bij het implementeren van virtuele machines met Managed Disks:

   - Als het abonnement is gemaakt vóór de update 1808, een virtuele machine met Managed Disks kan mislukken met een interne fout. Los de fout op door deze stappen voor elk abonnement uit te voeren:
      1. Ga in de tenantportal naar **abonnementen** en zoek het abonnement. Klik op **Resourceproviders**, klikt u vervolgens op **Microsoft.Compute**, en klik vervolgens op **opnieuw registreren**.
      2. Onder hetzelfde abonnement, gaat u naar **Access Control (IAM)**, en Controleer **Azure Stack – beheerde schijf** wordt vermeld.
   - Als u een omgeving met meerdere tenants hebt geconfigureerd, kan virtuele machines implementeren in een abonnement dat is gekoppeld aan een gast-map mislukken met een interne fout. Volg deze stappen om op te lossen de fout, [in dit artikel](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) opnieuw configureren van elk van de Gast-mappen.


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over virtuele machines van Azure Stack](azure-stack-compute-overview.md)
