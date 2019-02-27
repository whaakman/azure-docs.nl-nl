---
title: Back-up van een Azure-VM van de instellingen van de virtuele machine met de Azure Backup-service
description: Meer informatie over het back-up van een Azure VM met de Azure Backup-service
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: 16c7fe68e67094cef21d9efbffdccd614827ae6d
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56887468"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>Back-up van een Azure-VM van de instellingen van de virtuele machine

In dit artikel wordt uitgelegd hoe u back-up van virtuele Azure-machines met de [Azure Backup](backup-overview.md) service. U kunt back-up van virtuele Azure-machines met behulp van een aantal methoden:

- Eén Azure-VM: De instructies in dit artikel wordt beschreven hoe u back-up van een virtuele Azure-machine rechtstreeks vanuit de instellingen van de virtuele machine.
- Meerdere virtuele machines in Azure: U kunt een Recovery Services-kluis instellen en configureren van back-up voor meerdere Azure-VM's. Volg de instructies in [in dit artikel](backup-azure-arm-vms-prepare.md) voor dit scenario.

 

## <a name="before-you-start"></a>Voordat u begint

1. [Informatie over](backup-architecture.md#how-does-azure-backup-work) hoe back-up werkt, en [controleren](backup-support-matrix.md#azure-vm-backup-support) vereisten ondersteunen. 
2. [Bekijk een overzicht](backup-azure-vms-introduction.md) van Azure VM backup.

### <a name="azure-vm-agent-installation"></a>Azure VM-agent installeren

Als u wilt back-up van virtuele machines van Azure, installeert Azure Backup een uitbreiding op de VM-agent wordt uitgevoerd op de machine. Als uw virtuele machine vanuit een Azure marketplace-installatiekopie is gemaakt, wordt de agent worden uitgevoerd. In sommige gevallen, bijvoorbeeld als u een aangepaste VM maakt, of als u een virtuele machine migreren van on-premises. mogelijk moet u de agent handmatig installeren. 

- Als u de VM-agent handmatig installeren wilt, volgt u de instructies voor het [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) of [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) VM's. 
- Nadat de agent is geïnstalleerd, wanneer u back-up inschakelt, installeert Azure Backup de Backup-extensie voor de agent. Deze updates en patches van de extensie zonder tussenkomst van de gebruiker.

## <a name="back-up-from-azure-vm-settings"></a>Back-up van virtuele Azure-machine-instellingen


1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **alle services** en typ in het Filter, **virtuele machines**, en klik vervolgens op **virtuele machines**. 
3. Selecteer de virtuele machine die u back wilt-up maken van de lijst met virtuele machines.
4. Klik op het VM-menu **back-up**. 
5. In **Recovery Services-kluis**, doet u het volgende:
  - Als u al een kluis hebt, klikt u op **Selecteer een bestaande**, en selecteer een kluis.
  - Als u een kluis hebt, klikt u op **nieuw**. Geef een naam voor de kluis. Deze gemaakt in dezelfde regio en resourcegroep bevinden als de virtuele machine. U kunt deze instellingen niet wijzigen wanneer u back-up rechtstreeks vanuit de instellingen van de virtuele machine inschakelt.

  ![Wizard Back-up inschakelen](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. In **back-upbeleid kiezen**, doet u het volgende:

  - Laat het standaardbeleid. Dit back-ups van de virtuele machine eenmaal per dag om de tijd die is opgegeven en worden back-ups in de kluis gedurende 30 dagen bewaard.
  - Selecteer een bestaande back-upbeleid als u die hebt.
  - Een nieuw beleid maken en de beleidsinstellingen definiëren.  

  ![Back-upbeleid selecteren](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Klik op **back-up inschakelen**. Dit wordt het back-upbeleid gekoppeld aan de virtuele machine. 

    ![Knop Backup inschakelen](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. U kunt de voortgang van de configuratie in de portalmeldingen bijhouden.
9. Nadat de taak is voltooid, wordt in het VM-menu, klikt u op **back-up**. De back-upstatus wordt voor de virtuele machine, informatie over herstelpunten, taken die worden uitgevoerd en waarschuwingen die zijn uitgegeven.

  ![Back-upstatus](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Na het inschakelen van back-up, een eerste back-up wordt uitgevoerd. U kunt het onmiddellijk starten van de eerste back-up of wacht totdat deze wordt gestart in overeenstemming met de back-upschema.
    - Totdat de eerste back-up is voltooid, de **laatste back-up status** wordt weergegeven als **waarschuwing (eerste back-up in behandeling)**.
    - Als u wilt zien wanneer de volgende geplande back-up wordt uitgevoerd, klikt u op de naam van de back-upbeleid.
    
   

> [!NOTE]
> Azure Backup maakt een afzonderlijke resourcegroep (met uitzondering van de VM-resourcegroep) voor het opslaan van herstelpunten, met de naamgevingsindeling **AzureBackupRG_geography_number** (voorbeeld: AzureBackupRG_northeurope_1). U kunt deze resourcegroep mag niet vergrendelen.



## <a name="run-a-backup-immediately"></a>Direct uitvoeren van een back-up 

1. Klik om uit te voeren van een back-up onmiddellijk in het menu aan de virtuele machine op **back-up** > **back-up nu**.

    ![Back-up uitvoeren](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. In **nu back-up** het kalenderbesturingselement gebruiken om te selecteren tot wanneer het herstelpunt wordt bewaard > en **OK**.
  
    ![Dag van de back-upretentie](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. Portalmeldingen laat u weten dat de back-uptaak is geactiveerd. Voor het controleren van back-up wordt uitgevoerd, klikt u op **alle taken weergeven**.




## <a name="back-up-from-the-recovery-services-vault"></a>Back-up van de Recovery Services-kluis

Volg de instructies in dit artikel voor back-up inschakelen voor virtuele Azure-machines door het instellen van een Azure Backup Recovery Services-kluis en het inschakelen van back-up in de kluis.

## <a name="next-steps"></a>Volgende stappen

- Als u problemen met een van de procedures in dit artikel hebt, raadpleegt u de [problemen oplossen met](backup-azure-vms-troubleshoot.md).
- [Meer informatie over](backup-azure-manage-vms.md) beheren van uw back-ups.

