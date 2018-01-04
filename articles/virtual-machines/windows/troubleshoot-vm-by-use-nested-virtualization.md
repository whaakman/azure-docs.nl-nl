---
title: Virtuele machine van Azure problemen oplossen door het gebruik van geneste virtualisatie in Azure | Microsoft Docs
description: Het oplossen van een probleem virtuele machine van Azure met behulp van geneste virtualisatie in Azure
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: genli
ms.openlocfilehash: b50d139f62ef773fb5b9bb2638e40f8154e80979
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-a-problem-azure-vm-by-using-nested-virtualization-in-azure"></a>Virtuele machine van Azure problemen oplossen door het gebruik van geneste virtualisatie in Azure

In dit artikel laat zien hoe een geneste virtualisatieomgeving maken in Microsoft Azure, zodat u de schijf van het probleem VM op de Hyper-V-host (herstel-VM koppelen kunt) voor het oplossen van problemen.

## <a name="prerequisite"></a>Vereiste

Het probleem VM koppelen, moet de herstel-VM voldoen aan de volgende vereisten:

-   De herstel-VM moet zich in dezelfde locatie als het probleem VM.

-   De herstel-VM moet zich in dezelfde resourcegroep bevinden als het probleem VM.

-   De herstel-VM moet hetzelfde type Opslagaccount (Standard of Premium) gebruiken als het probleem VM.

## <a name="step-1-create-a-recovery-vm-and-install-hyper-v-role"></a>Stap 1: Maken van een herstel-VM en Hyper-V-functie installeren

1.  Maak een nieuwe virtuele machine voor herstel:

    -  Besturingssysteem: Windows Server 2016 Datacenter

    -  Grootte: Een reeks V3 met ten minste twee cores die ondersteuning voor geneste-virtualisatie. Zie voor meer informatie [introductie van de nieuwe Dv3 en Ev3 VM-grootte](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).

    -  Dezelfde locatie, Storage-Account en resourcegroep als het probleem VM.

    -  Selecteer het hetzelfde type als het probleem VM (Standard of Premium).

2.  Na de herstel-VM wordt gemaakt, extern bureaublad met de herstel-VM.

3.  Selecteer in Serverbeheer **beheren** > **functies en onderdelen toevoegen**.

4.  In de **installatietype** sectie **op basis van functie of onderdeel gebaseerde installatie**.

5.  In de **Select doelserver** sectie, zorg ervoor dat de herstel-VM is geselecteerd.

6.  Selecteer de **Hyper-V-functie** > **onderdelen toevoegen**.

7.  Selecteer **volgende** op de **functies** sectie.

8.  Als een virtuele switch beschikbaar is, selecteert u deze. Selecteer anders **volgende**.

9.  Op de **migratie** sectie **volgende**

10. Op de **standaardarchieven** sectie **volgende**.

11. Schakel het selectievakje in om de server automatisch opnieuw opstarten indien nodig.

12. Selecteer **installeren**.

13. Toestaan dat de server voor het installeren van de Hyper-V-rol. Dit duurt enkele minuten en de server automatisch opnieuw wordt opgestart.

## <a name="step-2-create-the-problem-vm-on-the-recovery-vms-hyper-v-server"></a>Stap 2: Maak de virtuele machine van het probleem op de herstel-VM Hyper-V-server

1.  Noteer de naam van de schijf in de virtuele machine van het probleem en verwijder vervolgens het probleem VM. Zorg ervoor dat u alle gekoppelde schijven behouden. 

2.  Koppel de OS-schijf van uw probleem VM als een gegevensschijf van de herstel-VM.

    1.  Nadat het probleem virtuele machine is verwijderd, gaat u naar de herstel-VM.

    2.  Selecteer **schijven**, en vervolgens **gegevensschijf toevoegen**.

    3.  Selecteer de schijf het probleem van de virtuele machine en selecteer vervolgens **opslaan**.

3.  Na de schijf is gekoppeld, extern bureaublad naar de herstel-VM.

4.  Open Schijfbeheer (diskmgmt.msc). Zorg ervoor dat de schijf van het probleem VM is ingesteld op **Offline**.

5.  Open Hyper-V-beheer: In **Serverbeheer**, selecteer de **Hyper-V-functie**. Met de rechtermuisknop op de server en selecteer vervolgens de **Hyper-V-beheer**.

6.  In de Hyper-V-beheer met de rechtermuisknop op de herstel-VM en selecteer vervolgens **nieuw** > **virtuele Machine** > **volgende**.

7.  Typ een naam voor de virtuele machine en selecteer vervolgens **volgende**.

8.  Selecteer **generatie 1**.

9.  De opstartgeheugen instellen op 1024 MB of meer.

10. Indien van toepassing selecteert u de Hyper-V-netwerkswitch die is gemaakt. Anders te verplaatsen naar de volgende pagina.

11. Selecteer **later een virtuele harde schijf koppelen**.

    ![de afbeelding over het koppelen van een optie Later virtuele hardeschijf](./media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

12. Selecteer **voltooien** wanneer de virtuele machine wordt gemaakt.

13. Met de rechtermuisknop op de virtuele machine die u gemaakt en selecteer vervolgens **instellingen**.

14. Selecteer **IDE-Controller 0**, selecteer **harde schijf**, en klik vervolgens op **toevoegen**.

    ![de afbeelding over voegt nieuwe vaste schijf](./media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

15. In **fysieke harde schijf**, selecteert u de schijf van het probleem VM die u hebt gekoppeld aan de virtuele machine in Azure. Als er geen schijven die worden vermeld, controleert u als de schijf is ingesteld op offline met behulp van Schijfbeheer.

    ![de afbeelding over koppelt de schijf](./media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


17. Selecteer **toepassen**, en selecteer vervolgens **OK**.

18. Dubbelklik op de virtuele machine en start het programma.

19. U kunt nu werken op de virtuele machine als de lokale virtuele machine. Kunt u alle stappen die u moet volgen.

## <a name="step-3-re-create-your-azure-vm-in-azure"></a>Stap 3: Uw Azure virtuele machine in Azure opnieuw maken

1.  Nadat u de virtuele machine weer online hebt ontvangen, sluit u de virtuele machine in Hyper-V-beheer.

2.  Ga naar de [Azure-portal](https://portal.azure.com) en selecteer de herstel-VM > schijven, de naam van de schijf kopiëren. U gebruikt de naam in de volgende stap. Ontkoppel de vaste schijf van de herstel-VM.

3.  Ga naar **alle resources**, zoekt u de naam van de schijf en selecteer vervolgens de schijf.

     ![de afbeelding over doorzoekt de schijf](./media/troubleshoot-vm-by-use-nested-virtualization/search-disk.png)     

4. Klik op **virtuele machine maken**.

     ![de afbeelding over maakt vm van de schijf](./media/troubleshoot-vm-by-use-nested-virtualization/create-vm-from-vhd.png) 

U kunt ook Azure PowerShell gebruiken voor het maken van de virtuele machine van de schijf. Zie voor meer informatie [de nieuwe virtuele machine maken van een bestaande schijf met behulp van PowerShell](create-vm-specialized.md#create-the-new-vm). 

## <a name="next-steps"></a>Volgende stappen

Als u verbinding maakt met uw virtuele machine problemen ondervindt, raadpleegt u [problemen met RDP-verbindingen naar een Azure-virtuele machine](troubleshoot-rdp-connection.md). Zie voor problemen met de toegang tot toepassingen die worden uitgevoerd op de virtuele machine [oplossen verbindingsproblemen van toepassing op een Windows VM](troubleshoot-app-connection.md).
