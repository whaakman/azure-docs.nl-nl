---
title: Azure-VM back-up inschakelen tijdens het maken van
description: Zie de stappen voor het virtuele Azure-machine back-up inschakelen tijdens het maken.
services: backup, virtual-machines
author: rayne-wiselman
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup, virtual-machines
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: 518d171c96b9c4f9bf3e195a7130f4c022b7ad07
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879873"
---
# <a name="enable-backup-during-azure-virtual-machine-creation"></a>Back-up inschakelen tijdens het maken van virtuele Azure-machine 

De Azure Backup-service biedt een interface voor het maken en configureren van back-ups naar de cloud. Bescherm uw gegevens door te nemen van back-ups, herstelpunten, met regelmatige tussenpozen genoemd. Gebruik Azure Backup om herstelpunten te maken die kunnen worden opgeslagen in geografisch redundante kluizen van Recovery Services. Dit artikel wordt uitgelegd hoe u back-up inschakelen tijdens het maken van een virtuele machine (VM) in Azure portal.  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Als u niet al aangemeld bij uw account bent, aanmelden bij de [Azure-portal](http://portal.azure.com).
 
## <a name="create-virtual-machine-with-backup-configured"></a>Virtuele machine maken met back-up geconfigureerd 

1. Klik in de linkerbovenhoek van Azure portal op **nieuw**. 

2. Selecteer **Compute**, en selecteer vervolgens een installatiekopie van de virtuele machine.   

3. Geef de informatie van de virtuele machine op. De gebruikersnaam en wachtwoord wordt aanmelden bij de virtuele machine gebruikt. Na het voltooien klikt u op **OK**. 

4. Selecteer een grootte voor de VM.  

5. Onder **instellingen > back-up**, klikt u op **ingeschakeld** om back-configuratie-instellingen. U kunt de standaardwaarden accepteren en klikt u op **OK** op de instellingenpagina om door te gaan naar de pagina overzicht om de VM te maken. Als u wijzigen van de waarden wilt, volgt u de volgende stappen.  

6. Maak of Selecteer een Recovery Services-kluis waarin de back-ups van de virtuele machine. Als u een recovery services-kluis maakt, kunt u een resourcegroep voor de kluis.  

    ![Back-upconfiguratie in vm-pagina maken](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > De resourcegroep voor de Recovery Services-kluis kan afwijken van de resourcegroep voor de virtuele machine.  
    > 
    > 

7. Een back-upbeleid is standaard geselecteerd voor u snel de virtuele machine te beveiligen. Een back-upbeleid bepaalt hoe vaak de back-momentopnamen worden gemaakt en hoe lang de back-upkopieën bewaren. Kunt u het standaardbeleid accepteren, of u kunt maken of Selecteer een ander back-upbeleid. Als u wilt bewerken in de back-upbeleid, selecteert u **back-upbeleid** en de waarden van het beleid wijzigen.  

8. Wanneer u tevreden met de waarden van de back-upconfiguratie, in de pagina met instellingen bent, klikt u op **OK**.  

9. Op de pagina overzicht nadat de validatie is verstreken, klikt u op **maken** te maken van een virtuele machine die gebruikmaakt van de geconfigureerde instellingen voor back-up. 

## <a name="initiate-a-backup-after-creating-the-vm"></a>Een back-up na het maken van de virtuele machine starten 

Hoewel het back-upbeleid is gemaakt, is het raadzaam om een eerste back-up maken. De details van de back-up bekijken voor de virtuele machine eenmaal is voltooid-sjabloon maken van de virtuele machine van de **Operations** instellen op het menu links, klikt u op **back-up**. U kunt dit gebruiken om te activeren van een on-demand back-up, herstel een volledige virtuele machine of alle schijven, bestanden herstellen vanuit back-up van virtuele machine of het back-upbeleid dat is gekoppeld aan de virtuele machine wijzigen.  

## <a name="using-a-resource-manager-template-to-deploy-a-protected-vm"></a>Gebruik van Resource Manager-sjabloon voor het implementeren van een beveiligde virtuele machine

De vorige stappen wordt uitgelegd hoe u Azure portal gebruiken voor het maken van een virtuele machine en deze beveiligen via een Recovery Services-kluis. Als u wilt snel een of meer virtuele machines implementeren en ze naar een Recovery Services-kluis beveiligen, raadpleegt u de sjabloon [een Windows-machine implementeert en back-up inschakelen](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

### <a name="which-vm-images-enable-backup-at-the-time-of-vm-creation"></a>Welke VM-installatiekopieën back-up inschakelen op het moment van de VM wordt gemaakt? 

De volgende lijst core-installatiekopieën die zijn gepubliceerd door Microsoft worden ondersteund voor het back-up inschakelen tijdens het maken van virtuele machine. Voor andere virtuele machines, kunt u back-up inschakelen nadat de virtuele machine is gemaakt. Meer informatie [inschakelen back-up nadat de virtuele machine is gemaakt](quick-backup-vm-portal.md) 

- **Windows** -Windows Server 2016 Datacenter, Windows Server 2016 Datacenter-core, Windows Server 2012 DataCenter, Windows Server 2012 R2 DataCenter, Windows Server 2008 R2 SP1 
- **Ubuntu** -Ubuntu Server 1710, Ubuntu-Server versie 1704, UUbuntu Server 1604(LTS), 1404(LTS) Ubuntu-Server 
- **Red Hat** -RHEL 6.7, 6,8, 6,9, 7.2, 7.3, 7.4 
- **SUSE** -SP4 van SUSE Linux Enterprise Server 11, 12 SP2 en 12 SP3 
- **Debian** -Debian 8, Debian 9 
- **CentOS** - CentOS 6.9, CentOS 7.3 
- **Oracle Linux** -Oracle Linux 6.7, 6,8, 6,9, 7.2, 7.3 
 
### <a name="is-backup-cost-included-in-the-vm-cost"></a>Zijn kosten voor back-up opgenomen in de VM-kosten? 

Nee, back-kosten zijn afzonderlijke of distinct, van kosten voor virtuele machines. Zie voor meer informatie over prijzen voor backup, de [prijzen voor Backup site](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Welke machtigingen zijn vereist om in te schakelen van back-up op een virtuele machine? 

Als u een inzender voor virtuele machines, kunt u back-up op de virtuele machine. Als u van een aangepaste rol gebruikmaakt, moet u de volgende machtigingen om in te schakelen is back-up op de virtuele machine. 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Als uw recovery services-kluis en de virtuele machine hebt verschillende resourcegroepen bevinden, zorg er dan voor dat u hebt schrijfmachtigingen in de resourcegroep van recovery services-kluis.  

## <a name="next-steps"></a>Volgende stappen 

Nu u uw virtuele machine hebt beveiligd, Zie de volgende artikelen voor meer informatie over VM-beheertaken, en hoe u virtuele machines herstellen. 

- [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md) 
- [Virtuele machines herstellen](backup-azure-arm-restore-vms.md) 
