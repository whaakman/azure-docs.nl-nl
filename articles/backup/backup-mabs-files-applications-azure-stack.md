---
title: Maak een back-up van Azure-Stack-bestanden en toepassingen | Microsoft Docs
description: Azure Backup gebruiken voor back-up en herstellen van Azure-Stack-bestanden en toepassingen naar uw Azure-Stack-omgeving.
services: backup
documentationcenter: ''
author: adiganmsft
manager: shivamg
editor: ''
keyword: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/11/2018
ms.author: adigan,markgal
ms.openlocfilehash: 19067b40e8e87c160515d13bb490e7c1604788b6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Back-up van bestanden en toepassingen op Azure-Stack
U kunt Azure back-up beveiligen (of back-up) bestanden en toepassingen op Azure-Stack. Als u wilt back-up van bestanden en toepassingen, Microsoft Azure Backup-Server te installeren als een virtuele machine uitgevoerd op Azure-Stack. U kunt alle toepassingen, die op elke server Azure-Stack in hetzelfde virtuele netwerk kunt beveiligen. Eenmaal u Azure Backup-Server hebt geïnstalleerd, voegt Azure-schijven voor een verhoging van de lokale opslag beschikbaar voor back-upgegevens op korte termijn. Azure Backup-Server gebruikmaakt van Azure-opslag op lange termijn.

> [!NOTE]
> Hoewel Azure Backup-Server en System Center Data Protection Manager (DPM) zijn vergelijkbaar, wordt DPM wordt niet ondersteund voor gebruik met Azure-Stack.
>


## <a name="azure-backup-server-protection-matrix"></a>Beveiligingsmatrix voor Azure Backup Server
Azure Backup-Server beveiligt de volgende Stack Azure VM-werkbelastingen.

| Beveiligde gegevensbron | Beveiliging en herstel |
| --------------------- | ----------------------- |
| Windows Server Semi-per jaar kanaal - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| WindowsServer 2016 - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| Windows Server 2012 R2 - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| WindowsServer 2012 - Entprise-Datacenter-standaard | Volumes, bestanden, mappen |
| Windows Server 2008 R2 - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2013 | Farm, database, frontend, webserver |
| SharePoint 2010 | Farm, database, frontend, webserver |


## <a name="install-azure-backup-server"></a>Azure Backup-Server installeren
Zie het artikel Azure Backup-Server installeren op een virtuele machine van Azure-Stack [voorbereiden van de back-up van werkbelastingen met Azure Backup-Server](backup-azure-microsoft-azure-backup.md). Voordat u installeren en configureren van Azure Backup-Server, worden rekening met het volgende:

### <a name="determining-size-of-virtual-machine"></a>Grootte van virtuele machine vaststellen
Azure Backup-Server op een virtuele machine van Azure-Stack uitgevoerd, gebruikt u de grootte A2 of hoger. Download voor hulp bij het kiezen van de grootte van een virtuele machine, de [Azure Stack VM-grootte Rekenmachine](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Virtuele netwerken op de Stack Azure virtuele machines
Alle virtuele machines die worden gebruikt in een Azure-Stack-werkbelasting moet behoren tot de dezelfde Azure-netwerk en Azure-abonnement.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Opslaan van back-upgegevens op lokale schijf en in Azure
Azure Backup-Server slaat de back-upgegevens op Azure schijven zijn gekoppeld aan de virtuele machine voor operationeel herstel. Zodra de schijven en de opslagruimte die zijn gekoppeld aan de virtuele machine, beheert back-upserver van Azure storage voor u. De hoeveelheid back-upgegevens opslag is afhankelijk van het aantal en grootte van de schijven die zijn gekoppeld aan elk [Stack Azure virtuele machine](../azure-stack/user/azure-stack-storage-overview.md). Elke grootte van de Stack van virtuele machine in Azure heeft het maximale aantal schijven dat kan worden gekoppeld aan de virtuele machine. A2 is bijvoorbeeld vier schijven. A3 is acht schijven. A4 is 16 schijven. Bepaalt het totale aantal back-upopslag opnieuw, de grootte en het aantal schijven.

> [!IMPORTANT]
> U moet **niet** operationele herstelgegevens (back-up) op schijven met Azure Backup-Server gekoppeld voor meer dan vijf dagen bewaren.
>

Back-upgegevens opslaan in Azure, vermindert back-upinfrastructuur op Azure-Stack. Als gegevens niet meer dan vijf dagen, moet worden opgeslagen in Azure.

Als u wilt back-gegevens opslaat in Azure, maken of gebruiken van een Recovery Services-kluis. Bij het voorbereiden van de back-up van de werkbelasting van de Azure Backup-Server, u [configureren van de Recovery Services-kluis](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault). Na de configuratie, elke keer die een back-uptaak wordt uitgevoerd, wordt er een herstelpunt gemaakt in de kluis. Elke Recovery Services-kluis bevat tot 9999 herstelpunten. Afhankelijk van het aantal herstelpunten die zijn gemaakt en hoe lang ze blijven behouden, kunt u back-upgegevens jaren behouden. U kan bijvoorbeeld maandelijks herstelpunten maken en deze gedurende vijf jaar te handhaven.
 
### <a name="using-sql-server"></a>Met behulp van SQL Server
Als u een externe SQL Server gebruikt voor de Azure Backup-Server-database wilt, selecteert u alleen een Stack van virtuele machine van Azure met SQL Server.

### <a name="azure-backup-server-vm-performance"></a>Azure virtuele machine back-up van Server-prestaties
Als gedeeld met andere virtuele machines, kunnen de grootte van de opslagruimte en IOPS-limieten invloed op de prestaties van de virtuele machine Azure Backup-Server. Daarom moet u een afzonderlijke opslagaccount voor de virtuele machine van Azure Backup-Server. De Azure Backup-agent op de Azure Backup-Server wordt uitgevoerd, heeft tijdelijke opslag voor:
    - eigen gebruik nodig (een cachelocatie)
    - gegevens die worden hersteld vanuit de cloud (lokaal faseringsgebied)
  
### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Backup tijdelijke schijfopslag configureren
Elke virtuele machine van Azure-Stack wordt geleverd met tijdelijke schijfruimte die beschikbaar is voor de gebruiker als volume `D:\`. Het lokale faseringsgebied dat Azure Backup nodig kan worden geconfigureerd dat het zich in `D:\`, en de cachelocatie kan worden geplaatst op `C:\`. Er is geen opslag moet verzinken weg van de gegevensschijven gekoppeld aan de virtuele machine van Azure Backup-Server op deze manier.

### <a name="scaling-deployment"></a>Implementatie schalen
Als u schalen van uw implementatie wilt, hebt u de volgende opties:
  - Opschalen: verhoog de grootte van de virtuele machine Azure Backup-Server op D-reeks uit een reeks en verhogen van de lokale opslag [per de instructies van de virtuele machine Azure Stack](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Gegevens overdragen: verzend oudere gegevens naar Azure Backup-Server en bewaar alleen de nieuwste gegevens op de opslag die is gekoppeld aan de Azure Backup-Server.
  - Uitschalen - toevoegen van meer back-up van Azure-Servers om de werkbelastingen te beveiligen.

## <a name="see-also"></a>Zie ook
Zie voor informatie over andere werkbelastingen beveiligen met behulp van Azure Backup-Server een van de volgende artikelen:
- [Back-up van SharePoint-farm](backup-azure-backup-sharepoint-mabs.md)
- [Back-up van SQL server](backup-azure-sql-mabs.md)
