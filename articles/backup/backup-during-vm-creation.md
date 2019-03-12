---
title: Azure-VM back-up inschakelen tijdens het maken van
description: Klik hier voor meer informatie over het back-up van virtuele Azure-machine inschakelen tijdens het maken.
services: backup, virtual-machines
author: rayne-wiselman
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: fd2beaa39f03d4f2342c94bf1cd8b8aea7440e62
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780440"
---
# <a name="enable-backup-when-you-create-an-azure-virtual-machine"></a>Back-up inschakelen bij het maken van een virtuele machine van Azure

De Azure Backup-service gebruiken voor back-up van virtuele Azure-machines (VM's). Virtuele machines back-ups volgens een schema dat is opgegeven in een back-upbeleid en herstelpunten worden gemaakt op basis van de back-ups. Herstelpunten worden opgeslagen in Recovery Services-kluizen.

Dit artikel wordt uitgelegd hoe u back-up niet inschakelen terwijl u een virtuele machine (VM) maakt in Azure portal.  

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Als u nog niet hebt aangemeld bij uw account, aanmelden bij de [Azure-portal](https://portal.azure.com).
 
## <a name="create-a-vm-with-backup-configured"></a>Een virtuele machine maken met back-up geconfigureerd 

1. Selecteer in de linkerbovenhoek van Azure portal **nieuw**.

1. Selecteer **Compute**, en selecteer vervolgens een installatiekopie van de virtuele machine.

1. Voer de gegevens voor de virtuele machine. De gebruikersnaam en het wachtwoord dat u opgeeft wordt gebruikt voor aanmelding bij de virtuele machine. Wanneer u klaar bent, selecteert u **OK**. 

1. Selecteer een grootte voor de VM.  

1. Onder **instellingen** > **back-up**, selecteer **ingeschakeld** om de back-configuratieinstellingen te openen.

   - Selecteer voor het accepteren van de standaardwaarden, **OK** op de **instellingen** pagina. U gaat vervolgens naar de **samenvatting** pagina om te maken van de virtuele machine. Stap 6-8 overslaan.
   - De back-upconfiguratie om waarden te wijzigen, de volgende stappen te volgen.  

1. Maak of Selecteer een Recovery Services-kluis voor het opslaan van de back-ups van de virtuele machine. Als u een Recovery Services-kluis maakt, kunt u een resourcegroep voor de kluis.  

    ![Back-configuratie-instellingen in de pagina voor het maken van virtuele machine](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > De resourcegroep voor de Recovery Services-kluis kan afwijken van de resourcegroep voor de virtuele machine.  

1. Een back-upbeleid is standaard geselecteerd voor u om de VM te beveiligen. Een back-upbeleid geeft aan hoe vaak back-momentopnamen worden gemaakt en hoe lang de back-ups. 

   - Kunt u het standaardbeleid accepteren, of u kunt maken of Selecteer een ander back-upbeleid. 
   - Als u wilt bewerken in de back-upbeleid, selecteert u **back-upbeleid** en de waarden te wijzigen.  

1. Wanneer u klaar bent met instellen van de waarden van de back-upconfiguratie, selecteert u **OK** op de **instellingen** pagina.  

1. Op de **samenvatting** pagina nadat de validatie is verstreken, selecteer **maken** te maken van een virtuele machine die gebruikmaakt van de geconfigureerde instellingen voor back-up. 

## <a name="start-a-backup-after-creating-the-vm"></a>Een back-up na het maken van de virtuele machine starten 

Zelfs als u een back-upbeleid voor de virtuele machine hebt geconfigureerd, is het raadzaam om te maken van een eerste back-up. 

Nadat de VM-sjabloon voor het maken is voltooid, gaat u naar **Operations** in het menu aan de linkerkant en selecteer **back-up** om de details van de back-up voor de virtuele machine weer te geven. U kunt deze pagina om te gebruiken:

- Trigger een on-demand back-up.
- Een volledige virtuele machine of alle bijbehorende schijven herstellen.
- Bestanden herstellen vanaf een virtuele machine back-up.
- Wijzig het back-upbeleid dat is gekoppeld aan de virtuele machine.  

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Resource Manager-sjabloon gebruiken om te implementeren van een beveiligde virtuele machine

De vorige stappen wordt uitgelegd hoe de Azure portal gebruiken voor een virtuele machine maken en deze in een Recovery Services-kluis beveiligen. Voor het snel een of meer virtuele machines implementeren en ze beschermen in een Recovery Services-kluis, ziet u de sjabloon [een Windows-machine implementeert en back-up inschakelen](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

### <a name="which-vm-images-support-backup-configuration-during-vm-creation"></a>Welke VM-installatiekopieën ondersteuning voor back-upconfiguratie tijdens het maken van virtuele machine?

De volgende core installatiekopieën die zijn gepubliceerd door Microsoft worden ondersteund voor het back-up inschakelen tijdens het maken van virtuele machine. Voor andere virtuele machines, kunt u back-up inschakelen nadat de virtuele machine is gemaakt. Zie voor meer informatie, [inschakelen back-up nadat de virtuele machine wordt gemaakt](quick-backup-vm-portal.md).

- **Windows** -Windows Server 2016 Datacenter, Core voor Windows Server 2016 Datacenter, Windows Server 2012 Datacenter, Windows Server 2012 R2 Datacenter, Windows Server 2008 R2 SP1 
- **Ubuntu** -Ubuntu Server 17.10, Ubuntu Server 17.04, Ubuntu Server 16.04 (TNS), Ubuntu Server 14.04 (TNS) 
- **Red Hat** -RHEL 6.7, 6,8, 6,9, 7.2, 7.3, 7.4 
- **SUSE** - SUSE Linux Enterprise Server 11 SP4, 12 SP2, 12 SP3 
- **Debian** -Debian 8, Debian 9 
- **CentOS** - CentOS 6.9, CentOS 7.3 
- **Oracle Linux** - Oracle Linux 6.7, 6.8, 6.9, 7.2, 7.3 
 
### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>Zijn de back-up kosten opgenomen in de VM-kosten? 

Nee. Back-kosten zijn gescheiden van de kosten van een virtuele machine. Zie voor meer informatie over prijzen voor backup [prijzen voor Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Welke machtigingen zijn vereist om in te schakelen van back-up op een virtuele machine? 

Als u een VM-Inzender bent, kunt u back-up op de virtuele machine inschakelen. Als u een aangepaste rol, moet u de volgende machtigingen voor de back-up op de virtuele machine inschakelen: 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Als uw Recovery Services-kluis en de virtuele machine hebt verschillende resourcegroepen bevinden, zorg er dan voor dat u hebt schrijfmachtigingen in de resourcegroep voor de Recovery Services-kluis.  

## <a name="next-steps"></a>Volgende stappen 

Nu u uw virtuele machine hebt beveiligd, Zie de volgende artikelen voor meer informatie over het beheren en virtuele machines herstellen:

- [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md) 
- [Virtuele machines herstellen](backup-azure-arm-restore-vms.md) 
