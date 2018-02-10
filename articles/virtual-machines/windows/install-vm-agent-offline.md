---
title: De Azure VM-Agent installeren in de offlinemodus | Microsoft Docs
description: Informatie over de installatie van de Azure VM-Agent in de offlinemodus.
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: genli
ms.openlocfilehash: b38bfaffad3e214f3b854715e4d8030cde432bae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="install-the-vm-agent-in-offline-mode-in-an-azure-windows-vm"></a>De VM-agent installeren in de offlinemodus bevindt in een Windows Azure VM 

VM-agent biedt nuttige functies zoals lokale administrator-wachtwoord opnieuw instellen en script push. In dit artikel laat zien hoe installeer de VM-agent voor een virtuele machine die offline is. 

## <a name="when-to-use-offline-mode"></a>Wanneer offlinemodus gebruiken

U moet de VM-agent installeren in de offlinemodus bevindt in het volgende scenario:

- U implementeert een Azure-virtuele machine die niet de VM-agent zijn geïnstalleerd of de VM-agent werkt niet.
- U het wachtwoord van de beheerder niet meer weet of u geen toegang tot de virtuele machine.

In dit scenario moet u de VM-agent installeren in de offlinemodus. 



## <a name="detailed-steps"></a>Gedetailleerde stappen

**Stap 1: De besturingssysteemschijf van de virtuele machine aan een andere virtuele machine als een gegevensschijf koppelen**

1.  Verwijder de virtuele machine. Zorg ervoor dat u selecteert de **houden van de schijven** optie als u dit doet.

2.  De OS-schijf als een gegevensschijf aan een andere virtuele machine (een probleemoplosser VM) te koppelen. Zie [How to attach a data disk to a Windows VM in the Azure portal](attach-managed-disk-portal.md) (Een gegevensschijf koppelen aan een virtuele Windows-machine in Azure Portal) voor meer informatie.

3.  Maak verbinding met de VM voor probleemoplossing. Open **Computerbeheer** > **Schijfbeheer**. Zorg dat de OS-schijf online is en dat de partities stationsletter is toegewezen.

**Stap 2: De besturingssysteemschijf voor de installatie van de VM-Agent wijzigen**

1.  Extern bureaublad verbinding maken met de problemen met virtuele machine.

2.  Op de schijf met het besturingssysteem door u bijgevoegde, bladert u naar **\windows\system32\config**. Kopieer de bestanden die als een back-up als een terugdraaibewerking vereist is.

3.  Start de Register-Editor (regedit.exe).

4.  Klik op de **HKEY_LOCAL_MACHINE** sleutel en selecteer vervolgens **bestand** > **Component laden** in het menu.

    ![Component laden](./media/install-vm-agent-offline/load-hive.png)

5.  Navigeer naar **\windows\system32\config\SYSTEM** schijf die u hebt gekoppeld in het besturingssysteem en typ BROKENSYSTEM als de naam van de component. Nadat u dit doet, ziet u het register hive onder **HKEY_LOCAL_MACHINE**.

6.  Navigeer naar **\windows\system32\config\SOFTWARE** op de OS-schijf u hebt gekoppeld, typt u BROKENSOFTWARE als de naam voor de component.

7.  Als er een actuele versie van de agent die niet werkt, voert u een back-up van de huidige configuratie. Als de virtuele machine niet de VM-agent is geïnstalleerd heeft, gaat u naar de volgende stap.  
      
    1. Wijzig de naam van de map \windowsazure naar \windowsazure.old

    2. De volgende registers exporteren
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE \BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Gebruik de bestaande bestanden op de VM, het oplossen van problemen als een opslagplaats voor de installatie van de VM-agent: 

    1. Exporteren van de VM probleemoplosser voor de volgende subsleutels in registerindeling (.reg): 

        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![De afbeelding over registersleutels exporteren](./media/install-vm-agent-offline/backup-reg.png)

    2. Deze drie registerbestanden bewerken, wijzig de **SYSTEM** vermelding die sleutel **BROKENSYSTEM**, en sla de bestanden.
        ![De afbeelding over registersleutels wijzigen](./media/install-vm-agent-offline/change-reg.png)
    3. Als u dubbelklikt op de registerbestanden om te importeren.
    4. Zorg ervoor dat de volgende sleutels zijn geïmporteerd in de component BROKENSYSTEM: WindowsAzureGuestAgent, WindowsAzureTelemetryService en RdAgent.

9.  Kopieer de map van de VM-agent van C:\windowsazure\packages naar de &lt;besturingssysteemschijf door u bijgevoegde&gt;: \windowsazure\packages.
    ![De afbeelding over bestanden kopiëren](./media/install-vm-agent-offline/copy-package.png)
      
    **Opmerking** Kopieer de map met Logboeken niet. Nadat de service wordt gestart, kunt u nieuwe logboeken wordt gegenereerd.

10.  Klik op BROKENSYSTEM en selecteer **bestand** > **component** in het menu.

11.  Klik op BROKENSOFTWARE en selecteer **bestand** > **component** in het menu.

12.  Ontkoppel de schijf en maak vervolgens de virtuele machine met behulp van de besturingssysteemschijf.

13.  Als u toegang de virtuele machine tot nu ziet u de RDAgent uitgevoerd en de logboeken ophalen gemaakt.

14. Als dit een virtuele machine gemaakt met behulp van het klassieke implementatiemodel, bent u klaar. Echter, als dit een virtuele machine gemaakt met het implementatiemodel van Resource Manager, moet ook u Azure PowerShell de eigenschap ProvisionGuestAgent bijwerken zodat deze Azure weet dat de virtuele machine de agent is geïnstalleerd. Voer hiervoor de volgende opdrachten in Azure PowerShell:

        $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        $vm.VM.ProvisionGuestAgent = $true
        Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>

    Als u **Get-AzureVM** nadat u deze stappen de **GuestAgentStatus** eigenschap moet worden ingevuld in plaats van een lege pagina die wordt weergegeven:

        Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure VM-Agent](agent-user-guide.md)
- [Virtuele machine-extensies en functies voor Windows](extensions-features.md)