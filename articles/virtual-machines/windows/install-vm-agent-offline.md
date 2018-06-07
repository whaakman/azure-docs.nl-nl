---
title: De Azure VM-Agent installeren in de offlinemodus | Microsoft Docs
description: Informatie over het installeren van de Azure VM-Agent in de offlinemodus.
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
ms.date: 05/18/2018
ms.author: genli
ms.openlocfilehash: df1bb64d0d753ae1853a2212c7c89d2e1613b8bc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657312"
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>De Azure VM-Agent installeren in de offlinemodus bevindt 

De Agent in Azure virtuele Machine (VM-Agent) biedt een handige functies, zoals lokale administrator-wachtwoord opnieuw instellen en het script worden gepusht. In dit artikel leest u hoe de VM-Agent voor een offline Windows virtuele machine (VM) te installeren. 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>Wanneer gebruikt u de VM-Agent in de offlinemodus bevindt

De VM-Agent installeren in de offlinemodus bevindt in de volgende scenario's:

- De geïmplementeerde virtuele machine van Azure beschikt niet over de VM-Agent is geïnstalleerd of de agent werkt niet.
- U bent vergeten het beheerderswachtwoord voor de virtuele machine of u geen toegang tot de virtuele machine.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>Het installeren van de VM-Agent in de offlinemodus bevindt

Gebruik de volgende stappen voor het installeren van de VM-Agent in de offlinemodus.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>Stap 1: De besturingssysteemschijf van de virtuele machine aan een andere virtuele machine als een gegevensschijf koppelen

1.  Verwijder de virtuele machine. Selecteer de **houden van de schijven** optie wanneer u de virtuele machine verwijdert.

2.  De OS-schijf als een gegevensschijf koppelen aan een andere virtuele machine (ook wel een _probleemoplosser_ VM). Zie voor meer informatie [een gegevensschijf koppelen aan een virtuele machine van Windows in de Azure portal](attach-managed-disk-portal.md).

3.  Verbinding maken met de probleemoplosser voor virtuele machine. Open **Computerbeheer** > **Schijfbeheer**. Bevestig dat de OS-schijf online is en dat de stationsletters zijn toegewezen aan de partities op schijf.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>Stap 2: De besturingssysteemschijf voor de installatie van de Azure VM-Agent wijzigen

1.  Een verbinding met extern bureaublad aanbrengen in de probleemoplosser voor virtuele machine.

2.  Op de OS-schijf die u hebt gekoppeld, bladert u naar de map \windows\system32\config. Kopieer alle bestanden in deze map als een back-up, als een terugdraaibewerking vereist is.

3.  Start de **Register-Editor** (regedit.exe).

4.  Selecteer de **HKEY_LOCAL_MACHINE** sleutel. Selecteer in het menu **bestand** > **Component laden**:

    ![Het volgende onderdeel laden](./media/install-vm-agent-offline/load-hive.png)

5.  Blader naar de map \windows\system32\config\SYSTEM op de schijf met het besturingssysteem die u hebt gekoppeld. Voer voor de naam van de component **BROKENSYSTEM**. De nieuwe registercomponent wordt weergegeven onder de **HKEY_LOCAL_MACHINE** sleutel.

6.  Blader naar de map \windows\system32\config\SOFTWARE op de schijf met het besturingssysteem die u hebt gekoppeld. Voer voor de naam van de hive-software, **BROKENSOFTWARE**.

7.  Als de VM-Agent niet werkt, back-up van de huidige configuratie.

    >[!NOTE]
    >Als de virtuele machine niet beschikt over de agent is geïnstalleerd, gaat u verder naar stap 8. 
      
    1. De naam van de map \windowsazure \windowsazure.old.

    2. De volgende registers exporteren:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Gebruik de bestaande bestanden op de probleemoplosser voor virtuele machine als een opslagplaats voor de VM-Agent-installatie. Voltooi de volgende stappen:

    1. Exporteren van de VM probleemoplosser voor de volgende subsleutels in registerindeling (.reg): 
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![De registersubsleutels exporteren](./media/install-vm-agent-offline/backup-reg.png)

    2. Bewerk de registerbestanden. In elk bestand, wijzigt u de Vermeldingswaarde **SYSTEM** naar **BROKENSYSTEM** (zoals weergegeven in de volgende afbeeldingen) en sla het bestand.

        ![Wijzig de subsleutel registerwaarden](./media/install-vm-agent-offline/change-reg.png)

    3. De Register-bestanden in de opslagplaats door te dubbelklikken op elk registerbestand importeren.

    4. Controleer of de volgende drie subsleutels zijn geïmporteerd in de **BROKENSYSTEM** hive:
        - WindowsAzureGuestAgent
        - WindowsAzureTelemetryService
        - RdAgent

9.  Kopieer de map van de VM-Agent van C:\windowsazure\packages naar de &lt;besturingssysteemschijf door u bijgevoegde&gt;: \windowsazure\packages.

    ![Kopieer de bestanden van de VM-Agent naar de OS-schijf](./media/install-vm-agent-offline/copy-package.png)
      
    >[!NOTE]
    >Kopieer niet de **logboeken** map. Nadat de service is gestart, worden nieuwe logboeken worden gegenereerd.

10.  Selecteer **BROKENSYSTEM**. Selecteer in het menu **bestand** > **component**.

11.  Selecteer **BROKENSOFTWARE**. Selecteer in het menu **bestand** > **component**.

12.  Loskoppelen van de besturingssysteemschijf en vervolgens de virtuele machine opnieuw te maken met behulp van de besturingssysteemschijf.

13.  Toegang tot de virtuele machine. U ziet dat de RdAgent wordt uitgevoerd en de logboeken worden gegenereerd.

Als u de virtuele machine met behulp van het implementatiemodel van Resource Manager implementatie hebt gemaakt, kunt u klaar bent.

### <a name="use-the-provisionguestagent-property-for-classic-vms"></a>Gebruik de eigenschap ProvisionGuestAgent voor klassieke virtuele machines

Als u de virtuele machine gemaakt met behulp van het klassieke model, de Azure PowerShell-module gebruiken om bij te werken de **ProvisionGuestAgent** eigenschap. De eigenschap informeert Azure dat de virtuele machine het VM-Agent is geïnstalleerd.

Om in te stellen de **ProvisionGuestAgent** eigenschap, voer de volgende opdrachten in Azure PowerShell:

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

- [Overzicht van Azure VM-Agent](../extensions/agent-windows.md)
- [Virtuele machine-extensies en functies voor Windows](extensions-features.md)
