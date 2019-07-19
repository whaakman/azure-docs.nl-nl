---
title: Een Recovery Services kluis verwijderen die is geconfigureerd voor de Azure Site Recovery-service
description: Meer informatie over het verwijderen van een Recovery Services kluis die is geconfigureerd voor Azure Site Recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: a13dee2010688b02fd86fb05900826470a7d7a08
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876042"
---
# <a name="delete-a-site-recovery-services-vault"></a>Een Site Recovery Services-kluis verwijderen

Afhankelijkheden kunnen verhinderen dat u een Azure Site Recovery kluis verwijdert. De acties die u moet uitvoeren, zijn afhankelijk van het Site Recovery scenario. Zie [een back-upkluis verwijderen in azure](../backup/backup-azure-delete-vault.md)als u een kluis wilt verwijderen die wordt gebruikt in azure backup.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-a-site-recovery-vault"></a>Een Site Recovery kluis verwijderen 
Als u de kluis wilt verwijderen, volgt u de aanbevolen stappen voor uw scenario.
### <a name="azure-vms-to-azure"></a>Azure-VM's naar Azure

1. Verwijder alle beveiligde Vm's door de stappen in [beveiliging uitschakelen voor een VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-azure-vm-azure-to-azure)te volgen.
2. Verwijder de kluis.

### <a name="vmware-vms-to-azure"></a>VMware-VM’s naar Azure

1. Verwijder alle beveiligde Vm's door de stappen in [beveiliging uitschakelen voor een VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure)te volgen.

2. Verwijder alle beleids regels voor replicatie door de stappen in [een replicatie beleid verwijderen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy)te volgen.

3. Verwijder verwijzingen naar vCenter door de stappen in [een vCenter-Server verwijderen](vmware-azure-manage-vcenter.md#delete-a-vcenter-server)te volgen.

4. Verwijder de configuratie server door de stappen uit te voeren in [een configuratie server buiten gebruik stellen](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server).

5. Verwijder de kluis.


### <a name="hyper-v-vms-with-vmm-to-azure"></a>Virtuele Hyper-V-machines (met VMM) naar Azure
1. Verwijder alle beveiligde Vm's door de stappen in[beveiliging uitschakelen voor een Hyper-V-VM (met VMM)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario)te volgen.

2. Koppel & Verwijder alle replicatie beleidsregels door te bladeren naar uw kluis-> **site Recovery infra structuur** -> **voor het System Center VMM** -> -**replicatie beleid**

3.  Verwijder verwijzingen naar VMM-servers door de stappen in de [registratie van een verbonden VMM-server ongedaan te maken](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Verwijder de kluis.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Virtuele Hyper-V-machines (zonder Virtual Machine Manager) naar Azure
1. Verwijder alle beveiligde Vm's door de stappen in [beveiliging uitschakelen voor een hyper-v-virtuele machine (hyper-v naar Azure) te](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure)volgen.

2. Koppel & Verwijder alle replicatie beleidsregels door te bladeren naar uw kluis-> **site Recovery infra structuur** -> voor het**replicatie beleid** **van Hyper-V-sites** -> 

3. Verwijder verwijzingen naar Hyper-V-servers door de stappen in de [registratie van een hyper-v-host](site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site)op te volgen.

4. Verwijder de Hyper-V-site.

5. Verwijder de kluis.


## <a name="use-powershell-to-force-delete-the-vault"></a>Power shell gebruiken om het verwijderen van de kluis af te dwingen 

> [!Important]
> Als u het product test en geen ervaring hebt met het verlies van gegevens, kunt u de methode Force Delete gebruiken om snel de kluis en alle bijbehorende afhankelijkheden te verwijderen.
> De Power shell-opdracht verwijdert alle inhoud van de kluis en kan **niet onomkeerbaar**zijn.

Als u de Site Recovery kluis wilt verwijderen, zelfs als er beveiligde items zijn, gebruikt u deze opdrachten:

    Connect-AzAccount

    Select-AzSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzRecoveryServicesVault -Name "vaultname"

    Remove-AzRecoveryServicesVault -Vault $vault

Meer informatie over [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault)en [Remove-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault).
