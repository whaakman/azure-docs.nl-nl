---
title: Back-up van virtuele machine van Azure inschakelen tijdens het maken van | Microsoft Docs
description: Zie de stappen voor het inschakelen van back-up van de virtuele machine in Azure tijdens het maken.
services: backup, virtual-machines
documentationcenter: 
author: markgalioto
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: 4041fc555fe4b61d10f84236dcae5156c6282fd3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="enable-backup-during-azure-virtual-machine-creation"></a>Back-up tijdens het maken van de virtuele machine van Azure inschakelen 

De Azure Backup-service biedt een interface voor het maken en configureren van de back-ups naar de cloud. Uw gegevens beschermen door back-ups, herstelpunten, aangeroepen met regelmatige tussenpozen. Gebruik Azure Backup om herstelpunten te maken die kunnen worden opgeslagen in geografisch redundante kluizen van Recovery Services. Dit artikel wordt uitgelegd hoe u back-up inschakelt tijdens het maken van een virtuele machine (VM) in de Azure portal.  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Als u niet al in aangemeld bij uw account, meld u bij de [Azure-portal](http://portal.azure.com).
 
## <a name="create-virtual-machine-with-backup-configured"></a>Virtuele machine maken met back-up is geconfigureerd 

1. Klik in de linkerbovenhoek van de Azure portal op **nieuw**. 

2. Selecteer **Compute**, en selecteer vervolgens een installatiekopie van de virtuele machine.   

3. Geef de informatie van de virtuele machine op. De gebruikersnaam en het opgegeven wachtwoord wordt aan te melden bij de virtuele machine gebruikt. Na het voltooien klikt u op **OK**. 

4. Selecteer een grootte voor de VM.  

5. Onder **instellingen > back-**, klikt u op **ingeschakeld** online zetten van de back-configuratie-instellingen. U kunt de standaardwaarden accepteren en klik op **OK** op de instellingenpagina om door te gaan naar de pagina overzicht maken van de virtuele machine. Als u wijzigen de waarden wilt, volgt u de volgende stappen.  

6. Maak of Selecteer een Recovery Services-kluis waarin de back-ups van de virtuele machine. Als u een recovery services-kluis maakt, kunt u een resourcegroep voor de kluis.  

    ![De pagina back-upconfiguratie in virtuele machine maken](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > De resourcegroep voor de Recovery Services-kluis kan afwijken van de resourcegroep voor de virtuele machine.  
    > 
    > 

7. Een back-upbeleid is standaard geselecteerd voor u snel de virtuele machine te beveiligen. Een back-upbeleid bepaalt hoe vaak de back-momentopnamen worden gemaakt en hoe lang deze back-upkopieën bewaren. U kunt het standaardbeleid accepteren of u kunt maken of Selecteer een andere back-upbeleid. Als de back-upbeleid bewerken, selecteert u **back-upbeleid** en de waarden van het beleid wijzigen.  

8. Wanneer u tevreden over de waarden van de back-upconfiguratie, in de pagina-instelling bent klikt u op **OK**.  

9. Op de overzichtspagina als validatie is geslaagd, klikt u op **maken** voor het maken van een virtuele machine die gebruikmaakt van de geconfigureerde instellingen voor back-up. 

## <a name="initiate-a-backup-after-creating-the-vm"></a>Een back-up te starten na het maken van de virtuele machine 

Hoewel het beleid van de back-up is gemaakt, is het raadzaam om een eerste back-up te maken. De back-up om details te bekijken voor de virtuele machine eenmaal is voltooid-sjabloon maken van de virtuele machine van de **Operations** instellen op in het menu links, klikt u op **back-**. U kunt dit activeert een-op-verzoek, een volledige virtuele machine of alle schijven te herstellen, bestanden terugzetten vanaf back-up van de VM of wijzigen van de back-upbeleid gekoppeld aan de virtuele machine.  

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

### <a name="which-vm-images-enable-backup-at-the-time-of-vm-creation"></a>Back-up welke VM-installatiekopieën inschakelen op het moment van de virtuele machine maken? 

De volgende lijst met core installatiekopieën die zijn gepubliceerd door Microsoft worden ondersteund voor het inschakelen van back-up tijdens het maken van VM. U kunt back-up voor andere VM's inschakelen wanneer de virtuele machine is gemaakt. Meer informatie [back-up nadat de virtuele machine wordt gemaakt](quick-backup-vm-portal.md) 

- **Windows** -datacenter van Windows Server 2016, Windows Server 2016 Data Center core, Windows Server 2012 DataCenter, Windows Server 2012 R2 DataCenter, Windows Server 2008 R2 SP1 
- **Ubuntu** - Ubuntu Server 1710, Ubuntu Server 1704, UUbuntu Server 1604(LTS), Ubuntu Server 1404(LTS) 
- **RedHat** -RHEL 6.7, 6,8, 6,9, 7.2, 7.3, 7.4 
- **SUSE** - SUSE Linux Enterprise Server 11 SP4, 12 SP2, 12 SP3 
- **Debian** -Debian 8, 9 Debian 
- **CentOS** - CentOS 6.9, CentOS 7.3 
- **Oracle Linux** - Oracle Linux 6.7, 6.8, 6.9, 7.2, 7.3 
 
### <a name="is-backup-cost-included-in-the-vm-cost"></a>Back-kosten die zijn opgenomen in de VM-kosten is? 

Nee, back-kosten zijn afzonderlijke of distinct, van kosten van de virtuele machines. Zie voor meer informatie over back-prijzen de [website prijzen voor back-](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Machtigingen die nodig zijn om back-up op een virtuele machine? 

Als u een virtuele machine-medewerker bent, kunt u back-up op de virtuele machine inschakelen. Als u een aangepaste beveiligingsrol gebruikt, moet u de volgende machtigingen tot stellen back-up op de virtuele machine. 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Als de recovery services-kluis en de virtuele machine hebt verschillende resourcegroepen, moet dat u schrijfmachtigingen hebt in de resourcegroep recovery services-kluis.  

## <a name="next-steps"></a>Volgende stappen 

Nu u uw virtuele machine hebt beveiligd, gaat u naar de volgende artikelen voor meer informatie over de VM-beheertaken en het herstellen van virtuele machines. 

- [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md) 
- [Virtuele machines herstellen](backup-azure-arm-restore-vms.md) 
