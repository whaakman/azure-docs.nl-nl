---
title: Overzicht van de Recovery Services-kluizen | Microsoft Docs
description: Een overzicht en de vergelijking tussen de Recovery Services-kluizen en Azure Backup-kluizen.
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/15/2017
ms.author: markgal;arunak;sogup
ms.openlocfilehash: ad685744595ab86def8d226d7c2fb5455af98da4
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="recovery-services-vaults-overview"></a>Overzicht van Recovery Services-kluizen

In dit artikel beschrijft de functies van een Recovery Services-kluis. Een Recovery Services-kluis is een entiteit opslag in Azure met gegevens. De gegevens zijn meestal kopieën van gegevens of configuratie-informatie voor virtuele machines (VM's), werkbelastingen, servers of werkstations. U kunt de Recovery Services-kluizen gebruiken voor het opslaan van back-upgegevens voor verschillende Azure-services zoals IaaS VM's (Linux of Windows) en Azure SQL-databases. Recovery Services-kluizen ondersteuning van System Center DPM, Windows Server back-upserver van Azure, en meer. Recovery Services-kluizen maken het eenvoudig om uw back-upgegevens te ordenen, terwijl de beheertaken minimaal zijn. 

Binnen een Azure-abonnement kunt u maximaal 25 Recovery Services-kluizen per regio.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Vergelijken Recovery Services-kluizen en back-upkluizen

Als u nog steeds Backup-kluizen hebt, worden ze automatisch bijgewerkt naar Recovery Services-kluizen. Door November 2017 zijn alle Backup-kluizen bijgewerkt naar een Recovery Services-kluizen. 

Recovery Services-kluizen zijn gebaseerd op het Azure Resource Manager-model van Azure, terwijl de Backup-kluizen zijn gebaseerd op de Azure Service Manager-model. Wanneer u een back-upkluis naar een Recovery Services-kluis bijwerkt, blijft de back-upgegevens intact tijdens en na het upgradeproces. Recovery Services-kluizen bieden, zoals functies die niet beschikbaar voor de Backup-kluizen:

- **Uitgebreide mogelijkheden om te helpen beveiligen back-upgegevens**: met Recovery Services-kluizen, Azure Backup biedt beveiligingsmogelijkheden voor het beveiligen van back-ups van de cloud. De beveiligingsfuncties of beveiligen van uw back-ups en veilig terugzetten van gegevens, zelfs als de productie- en back-up-servers worden getroffen. [Meer informatie](backup-azure-security-feature.md)

- **Centrale bewaking voor uw hybride IT-omgeving**: met Recovery Services-kluizen, kunt u controleren niet alleen uw [Azure IaaS VM's](backup-azure-manage-vms.md) , maar ook uw [lokale activa](backup-azure-manage-windows-server.md#manage-backup-items) vanuit een centrale portal. [Meer informatie](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Op rollen gebaseerde toegangsbeheer (RBAC)**: RBAC verfijnd toegangsbeheer management in Azure biedt. [Azure biedt diverse ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md), en de Azure Backup bevat drie [ingebouwde functies voor het beheren van herstelpunten](backup-rbac-rs-vault.md). Recovery Services-kluizen compatibel zijn met RBAC waarmee de beperkt back-up en herstel van toegang aan de gedefinieerde set van gebruikersrollen. [Meer informatie](backup-rbac-rs-vault.md)

- **Alle configuraties van Azure Virtual Machines beveiligen**: Recovery Services-kluizen beveiligen Resource Manager gebaseerde virtuele machines met inbegrip van Premium-schijven, schijven beheerd en virtuele machines versleuteld. Een back-upkluis upgraden naar een Recovery Services-kluis biedt u de mogelijkheid uw Service Manager-VM's bijwerken naar Resource Manager gebaseerde virtuele machines. Tijdens het upgraden van de kluis, kunt u uw herstelpunten op basis van Service Manager VM bewaren en configureer beveiliging voor de bijgewerkte (Resource Manager is ingeschakeld) virtuele machines. [Meer informatie](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Chatberichten terugzetten voor IaaS VM's**: met behulp van Recovery Services-kluizen, u kunt bestanden en mappen van een IaaS-VM herstellen zonder te herstellen van de hele virtuele machine, waardoor sneller worden hersteld. Chatberichten terugzetten voor IaaS VM's is beschikbaar voor zowel Windows als een virtuele Linux-machines. [Meer informatie](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>De Recovery Services-kluizen in de portal beheren
Maken en beheren van de Recovery Services-kluizen in de Azure portal is eenvoudig, omdat de Backup-service is geïntegreerd in het menu instellingen van Azure. Deze integratie zorgt ervoor dat u kunt maken of beheren van een Recovery Services-kluis *in de context van de doelservice*. Bijvoorbeeld als u wilt weergeven van de herstelpunten voor een virtuele machine, selecteert u deze en klikt u op **back-up** in het menu instellingen. De back-upgegevens die specifiek zijn voor die VM wordt weergegeven. In het volgende voorbeeld **ContosoVM** is de naam van de virtuele machine. **ContosoVM demovault** is de naam van de Recovery Services-kluis. U hoeft niet te onthouden van de naam van de Recovery Services-kluis die de herstelpunten opslaat, kunt u toegang tot deze informatie van de virtuele machine.  

![Recovery services-kluis details VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context.png)

Als meerdere servers worden beschermd met behulp van dezelfde Recovery Services-kluis, is het wellicht meer logische om te kijken naar de Recovery Services-kluis. U kunt zoeken naar een Recovery Services-kluizen worden in het abonnement en maak een keuze uit de lijst.

De volgende secties bevatten koppelingen naar artikelen waarin wordt uitgelegd hoe u een Recovery Services-kluis in elk type activiteit.

### <a name="back-up-data"></a>Back-up van gegevens
- [Back-up van een Azure VM](backup-azure-vms-first-look-arm.md)
- [Back-up van Windows Server of Windows-werkstation](backup-try-azure-backup-in-10-mins.md)
- [Back-up DPM workloads naar Azure](backup-azure-dpm-introduction.md)
- [Back-up werkbelastingen zijn die gebruikmaken van Azure Backup-Server voorbereiden](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Herstelpunten beheren
- [Back-ups van virtuele machine in Azure beheren](backup-azure-manage-vms.md)
- [Bestanden en mappen beheren](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Herstellen van gegevens uit de kluis
- [Afzonderlijke bestanden terugzetten vanuit een virtuele machine in Azure](backup-azure-restore-files-from-vm.md)
- [Een Azure-virtuele machine herstellen](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Beveiligen van de kluis
- [Back-upgegevens van de cloud in de Recovery Services-kluizen beveiligen](backup-azure-security-feature.md)



## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende artikelen voor:</br>
[Back-up van een IaaS VM](backup-azure-arm-vms-prepare.md)</br>
[Back-up van een Azure Backup-Server](backup-azure-microsoft-azure-backup.md)</br>
[Back-up van een Windows-Server](backup-configure-vault.md)
