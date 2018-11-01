---
title: De Azure VM-Agent installeren in de offlinemodus | Microsoft Docs
description: Informatie over het installeren van de Azure VM-Agent in de offlinemodus bevindt.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 3caa4f2dbe36f86c9b15a83303e90b16d06c56fd
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419398"
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>De Azure VM-Agent installeren in de offlinemodus bevindt 

De Azure Virtual Machine Agent (VM-Agent) biedt handige functies, zoals lokale administrator-wachtwoord opnieuw instellen en script pushen. In dit artikel wordt beschreven hoe u voor het installeren van de VM-Agent voor een offline Windows-machine (VM). 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>Wanneer u de VM-Agent in de offlinemodus bevindt

De VM-Agent installeren in de offlinemodus bevindt in de volgende scenario's:

- De geïmplementeerde Azure-VM heeft de VM-Agent is geïnstalleerd of de agent werkt niet.
- U bent vergeten het beheerderswachtwoord voor de virtuele machine of u geen toegang tot de virtuele machine.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>Het installeren van de VM-Agent in de offlinemodus bevindt

Gebruik de volgende stappen voor het installeren van de VM-Agent in de offlinemodus bevindt.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>Stap 1: De besturingssysteemschijf van de virtuele machine koppelen aan een andere virtuele machine als gegevensschijf

1.  De virtuele machine verwijderen. Zorg ervoor dat u selecteert de **houden van de schijven** optie wanneer u de virtuele machine verwijdert.

2.  Koppel de besturingssysteemschijf als gegevensschijf aan een andere virtuele machine (ook wel een _probleemoplosser_ VM). Zie voor meer informatie, [een gegevensschijf koppelen aan een Windows-VM in Azure portal](../windows/attach-managed-disk-portal.md).

3.  Verbinding maken met de probleemoplosser voor virtuele machine. Open **Computerbeheer** > **Schijfbeheer**. Bevestig dat de besturingssysteemschijf online is en dat er stationsletters zijn toegewezen aan de partities op de schijf.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>Stap 2: De besturingssysteemschijf voor het installeren van de Azure VM-Agent wijzigen

1.  Een verbinding met extern bureaublad naar de probleemoplosser voor virtuele machine maken

2.  Blader op een schijf met het besturingssysteem die u hebt gekoppeld, naar de map \windows\system32\config. Kopieer alle bestanden in deze map als een back-up, als een terugdraaiactie moet uitgevoerd worden.

3.  Start de **Register-Editor** (regedit.exe).

4.  Selecteer de **HKEY_LOCAL_MACHINE** sleutel. Selecteer in het menu **bestand** > **Component laden**:

    ![De component laden](./media/install-vm-agent-offline/load-hive.png)

5.  Blader naar de map \windows\system32\config\SYSTEM op de schijf met het besturingssysteem die u hebt gekoppeld. Voer voor de naam van de component, **BROKENSYSTEM**. De nieuwe registercomponent wordt weergegeven onder de **HKEY_LOCAL_MACHINE** sleutel.

6.  Blader naar de map \windows\system32\config\SOFTWARE op de schijf met het besturingssysteem die u hebt gekoppeld. Voer voor de naam van de hive-software, **BROKENSOFTWARE**.

7. Als de gekoppelde besturingssysteemschijf de VM-agent is geïnstalleerd heeft, voert u een back-up van de huidige configuratie. Als er geen VM-agent is geïnstalleerd, verplaatst naar de volgende stap.
      
    1. Wijzig de map \windowsazure in \windowsazure.old.

    2. De volgende registers exporteren:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Gebruik de bestaande bestanden op de probleemoplosser voor virtuele machine als een opslagplaats voor de VM-Agent-installatie. Voltooi de volgende stappen:

    1. Exporteren van de probleemoplosser voor de virtuele machine de volgende registersubsleutels in registerindeling (.reg): 
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![De registersubsleutels exporteren](./media/install-vm-agent-offline/backup-reg.png)

    2. Bewerk de registerbestanden. In elk bestand, wijzig de waarde van de vermelding **SYSTEM** naar **BROKENSYSTEM** (zoals weergegeven in de volgende installatiekopieën) en sla het bestand. Houd er rekening mee de **ImagePath** van de huidige VM-agent. We moet de bijbehorende map kopiëren naar de gekoppelde besturingssysteemschijf. 

        ![Wijzigen van de subsleutel registerwaarden](./media/install-vm-agent-offline/change-reg.png)

    3. De Register-bestanden in de opslagplaats door te dubbelklikken op elke Registerbestand importeren.

    4. Controleer of de volgende drie subsleutels zijn geïmporteerd in de **BROKENSYSTEM** hive:
        - WindowsAzureGuestAgent
        - WindowsAzureTelemetryService
        - RdAgent

    5. Kopieer de installatiemap van de huidige VM-Agent naar de gekoppelde besturingssysteemschijf: 

        1.  Maak op een schijf met het besturingssysteem die u hebt gekoppeld, een map met de naam WindowsAzure in het pad naar de hoofdmap.

        2.  Ga naar C:\WindowsAzure op de probleemoplosser voor virtuele machine, zoek naar een map met de naam C:\WindowsAzure\GuestAgent_X.X.XXXX.XXX. Kopieer de map GuestAgent waaraan de hoogste versienummer van C:\WindowsAzure naar de map WindowsAzure in de gekoppelde besturingssysteemschijf. Als u niet zeker weet welke map moet worden gekopieerd, moet u alle GuestAgent-mappen kopiëren. De volgende afbeelding toont een voorbeeld van de GuestAgent-map die wordt gekopieerd naar de gekoppelde besturingssysteemschijf.

             ![Kopieer de map GuestAgent](./media/install-vm-agent-offline/copy-files.png)

9.  Selecteer **BROKENSYSTEM**. Selecteer in het menu **bestand** > **component**.

10.  Selecteer **BROKENSOFTWARE**. Selecteer in het menu **bestand** > **component**.

11.  Loskoppelen van de besturingssysteemschijf en vervolgens de virtuele machine opnieuw te maken met behulp van de besturingssysteemschijf.

12.  Toegang tot de virtuele machine. U ziet dat de RdAgent wordt uitgevoerd en de logboeken worden gegenereerd.

Als u de virtuele machine gemaakt met behulp van de Resource Manager-implementatiemodel, bent u klaar.

### <a name="use-the-provisionguestagent-property-for-classic-vms"></a>Gebruik de eigenschap ProvisionGuestAgent voor klassieke virtuele machines

Als u de virtuele machine gemaakt met behulp van het klassieke model, de Azure PowerShell-module gebruiken om te werken de **ProvisionGuestAgent** eigenschap. De eigenschap informeert Azure dat de virtuele machine het VM-Agent is geïnstalleerd.

Om in te stellen de **ProvisionGuestAgent** eigenschap, voer de volgende opdrachten uit in Azure PowerShell:

   ```powershell
   $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   $vm.VM.ProvisionGuestAgent = $true
   Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>
   ```

Voer vervolgens de `Get-AzureVM` opdracht. U ziet dat de **GuestAgentStatus** eigenschap is nu gevuld met gegevens:

   ```powershell
   Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus
   ```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure Virtual Machine Agent](../extensions/agent-windows.md)
- [Extensies voor virtuele machines en functies voor Windows](../extensions/features-windows.md)
