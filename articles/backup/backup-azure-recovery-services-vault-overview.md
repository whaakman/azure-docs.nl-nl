---
title: Overzicht van Recovery Services-kluizen
description: Een overzicht en vergelijking van de Recovery Services-kluizen en Azure Backup-kluizen.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 8/10/2018
ms.author: markgal
ms.openlocfilehash: 697245a6465fc15c943acefa4b820982fbaecb55
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887595"
---
# <a name="recovery-services-vaults-overview"></a>Overzicht van Recovery Services-kluizen

In dit artikel beschrijft de functies van een Recovery Services-kluis. Een Recovery Services-kluis is een opslagentiteit in Azure met gegevens. De gegevens zijn meestal kopieën van gegevens of configuratie-informatie voor virtuele machines (VM's), werkbelastingen, servers of werkstations. U kunt de Recovery Services-kluizen gebruiken voor het opslaan van back-upgegevens voor verschillende Azure-services, zoals virtuele machines van IaaS (Linux of Windows) en Azure SQL-databases. Recovery Services-kluizen ondersteuning van System Center DPM, Windows Server, Azure Backup Server en meer. Recovery Services-kluizen maken het eenvoudig om uw back-upgegevens te ordenen, terwijl de beheertaken minimaal zijn.

Binnen een Azure-abonnement, kunt u maximaal 500 Recovery Services-kluizen per abonnement per regio maken.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Het vergelijken van Recovery Services-kluizen en back-upkluizen

Als u nog steeds back-upkluizen hebt, worden ze automatisch bijgewerkt naar Recovery Services-kluizen. Per November 2017, zijn alle back-upkluizen bijgewerkt naar Recovery Services-kluizen.

Recovery Services-kluizen zijn gebaseerd op het Azure Resource Manager-model van Azure, terwijl de Backup-kluizen zijn gebaseerd op het Azure Service Manager-model. Wanneer u een back-upkluis naar een Recovery Services-kluis upgraden, blijft de back-upgegevens behouden tijdens en na het upgradeproces. Recovery Services-kluizen bieden functies niet beschikbaar voor back-upkluizen, zoals:

- **Verbeterde mogelijkheden om gegevens te beveiligen back-up**: met Recovery Services-kluizen, Azure Backup biedt mogelijkheden voor beveiliging ter bescherming van de cloud back-ups. De beveiligingsfuncties of beveiligen van uw back-ups en veilig terugzetten van gegevens, zelfs als de productie- en back-up-servers zijn aangetast. [Meer informatie](backup-azure-security-feature.md)

- **Centrale bewaking voor uw hybride IT-omgeving**: met Recovery Services-kluizen, kunt u controleren niet alleen uw [Azure IaaS VM's](backup-azure-manage-vms.md) , maar ook uw [on-premises activa](backup-azure-manage-windows-server.md#manage-backup-items) vanuit een centrale portal. [Meer informatie](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rollen gebaseerd toegangsbeheer (RBAC)**: RBAC biedt uiterst gedetailleerd toegangsbeheer management in Azure. [Azure biedt verschillende ingebouwde rollen](../role-based-access-control/built-in-roles.md), en Azure Backup heeft drie [ingebouwde rollen voor het beheren van herstelpunten](backup-rbac-rs-vault.md). Recovery Services-kluizen zijn compatibel met RBAC, dat voorkomt back-up dat en herstellen van toegang tot een gedefinieerde set gebruikersrollen. [Meer informatie](backup-rbac-rs-vault.md)

- **Alle configuraties van virtuele Azure-Machines beveiligen**: Recovery Services-kluizen beveiligen van virtuele machines met inbegrip van Premium-schijven, Managed Disks en versleutelde virtuele machines op basis van Resource Manager. Een Backup-kluis upgraden naar een Recovery Services-kluis biedt u de mogelijkheid om te upgraden van uw Service Manager-VM's naar Resource Manager gebaseerde virtuele machines. Tijdens het upgraden van de kluis, kunt u uw herstelpunten op basis van een Service Manager-virtuele machine en configureer beveiliging voor de upgrade is uitgevoerd (Resource Manager-functionaliteit) virtuele machines. [Meer informatie](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Instant herstel voor IaaS-VM's**: met behulp van Recovery Services-kluizen, u kunt bestanden en mappen van een IaaS-VM herstellen zonder het herstellen van de volledige virtuele machine, waardoor sneller worden hersteld. Instant herstel voor IaaS-VM's is beschikbaar voor zowel Windows als Linux-VM's. [Meer informatie](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Beheren van uw Recovery Services-kluizen in de portal
Maken en beheren van Recovery Services-kluizen in de Azure-portal is eenvoudig, want de Backup-service kan worden geïntegreerd in andere Azure-services. Deze integratie betekent dat u kunt maken of beheren van een Recovery Services-kluis *in de context van de doelservice*. Bijvoorbeeld, als u wilt weergeven van de herstelpunten voor een virtuele machine, selecteer de virtuele machine en klikt u op **back-up** in het menu Acties.

![Recovery services vault-gegevens virtuele machine](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context-vm.png)

Als de virtuele machine geen back-up geconfigureerd, wordt klikt u vervolgens gevraagd u aan back-up configureren. Als de back-up is geconfigureerd, ziet u back-informatie over de virtuele machine, waaronder een lijst met herstelpunten.  

![Recovery services vault-gegevens virtuele machine](./media/backup-azure-recovery-services-vault-overview/vm-recovery-point-list.png)

In het vorige voorbeeld **ContosoVM** is de naam van de virtuele machine. **ContosoVM-demovault** is de naam van de Recovery Services-kluis. U hoeft niet de naam van de Recovery Services-kluis waarin de herstelpunten te herinneren, kunt u toegang tot deze informatie van de virtuele machine.  

Als een Recovery Services-kluis meerdere servers beveiligt, is deze mogelijk meer logische om te kijken naar de Recovery Services-kluis. U kunt zoeken naar alle Recovery Services-kluizen in het abonnement en maak een keuze uit de lijst.

De volgende secties bevatten koppelingen naar artikelen waarin wordt uitgelegd hoe u een Recovery Services-kluis in elk type activiteit.

> [!NOTE]
> Recovery Services-kluis kan niet worden gemaakt met dezelfde naam als deze binnen 24 uur is verwijderd. Gebruik een andere resourcenaam op of kies een andere resourcegroep of probeer het opnieuw na 24 uur.

### <a name="back-up-data"></a>Back-up van gegevens
- [Back-up van een Azure-VM](backup-azure-vms-first-look-arm.md)
- [Back-up van Windows Server of Windows-werkstation](backup-try-azure-backup-in-10-mins.md)
- [Back-up van DPM-workloads naar Azure](backup-azure-dpm-introduction.md)
- [Voorbereiden op back-up van workloads met Azure Backup Server](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Herstelpunten beheren
- [Back-ups van virtuele Azure-machine beheren](backup-azure-manage-vms.md)
- [Bestanden en mappen beheren](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Gegevens herstellen vanuit de kluis
- [Afzonderlijke bestanden herstellen van een Azure-VM](backup-azure-restore-files-from-vm.md)
- [Een Azure-VM herstellen](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>De kluis beveiligen
- [Beveiliging van de cloud back-upgegevens in Recovery Services-kluizen](backup-azure-security-feature.md)



## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende artikelen voor:</br>
[Back-up van een IaaS-VM](backup-azure-arm-vms-prepare.md)</br>
[Back-up van een Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[Back-up van een Windows-Server](backup-configure-vault.md)
