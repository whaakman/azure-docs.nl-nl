---
title: Een Site Recovery-kluis verwijderen
description: Informatie over het verwijderen van een Azure Site Recovery-kluis op basis van de Site Recovery-scenario.
service: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: 77bc7e068629c5dbf650b39844912b1b7a6155b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="delete-a-site-recovery-vault"></a>Een Site Recovery-kluis verwijderen
Afhankelijkheden kunnen voorkomen dat u een Azure Site Recovery-kluis verwijderen. De acties die u moet nemen afhankelijk van het scenario met Site Recovery: VMware naar Azure, Hyper-V (met en zonder System Center Virtual Machine Manager) voor Azure en Azure Backup. Zie het verwijderen van een gebruikt in Azure Backup-kluis [verwijderen van een back-upkluis in Azure](../backup/backup-azure-delete-vault.md).



## <a name="delete-a-site-recovery-vault"></a>Een Site Recovery-kluis verwijderen 
Volg de aanbevolen stappen voor uw scenario voor het verwijderen van de kluis.

### <a name="vmware-vms-to-azure"></a>VMware-VM’s naar Azure

1. Verwijder alle virtuele machines beveiligd door de stappen in [Schakel de beveiliging voor een VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. Beleidsregels voor alle replicatie verwijderen door de stappen in [verwijderen van een beleid voor wachtwoordreplicatie](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Verwijzingen naar vCenter verwijderen door de stappen in [verwijderen van een vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. De configuratieserver verwijderen door de stappen in [een configuratieserver buiten gebruik stellen](site-recovery-vmware-to-azure-manage-configuration-server.md##delete-or-unregister-a-configuration-server).

5. Verwijder de kluis.


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>Hyper-V-machines (met Virtual Machine Manager) naar Azure
1. Verwijder alle virtuele machines beveiligd door de stappen in[Schakel de beveiliging voor een Hyper-V virtuele machines repliceren naar Azure met behulp van de System Center VMM voor Azure scenario](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).

2. Koppeling & verwijderen van alle beleidsregels voor replicatie door naar uw kluis te bladeren -> **Site Recovery-infrastructuur** -> **voor System Center VMM** -> **replicatie Beleid**

3.  Verwijzingen naar Virtual Machine Manager-servers verwijderen door de stappen in [Hef de registratie van een VMM-server verbonden](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Verwijder de kluis.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Hyper-V-machines (zonder Virtual Machine Manager) naar Azure
1. Verwijder alle virtuele machines beveiligd door de stappen in [Schakel de beveiliging voor een Hyper-V virtuele machine (Hyper-V naar Azure)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. Koppeling & verwijderen van alle beleidsregels voor replicatie door naar uw kluis te bladeren -> **Site Recovery-infrastructuur** -> **voor Hyper-V-Sites** -> **beleidsregels voor replicatie**

3. Verwijzingen naar Hyper-V-servers verwijderen door de stappen in [Hef de registratie van een Hyper-V-host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. De Hyper-V-site verwijderen.

5. Verwijder de kluis.


## <a name="use-powershell-to-force-delete-the-vault"></a>Gebruik PowerShell om af te dwingen de kluis verwijderen 

> [!Important]
> Als u het product test en niet betrokken zijn bij verlies van gegevens, gebruikt u de delete-methode force snel de kluis en de afhankelijkheden ervan verwijderen.
> De PowerShell-opdracht wordt de inhoud van de kluis verwijderd en is **niet omkeerbaar**.

Voor het verwijderen van de Site Recovery-kluis zelfs als er beveiligde items, moet u deze opdrachten gebruiken:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault