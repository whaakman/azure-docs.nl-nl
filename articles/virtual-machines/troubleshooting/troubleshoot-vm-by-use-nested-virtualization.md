---
title: Een probleem virtuele Azure-machine op te lossen met behulp van geneste virtualisatie in Azure | Microsoft Docs
description: Problemen oplossen met een probleem met virtuele Azure-machine met behulp van geneste virtualisatie in Azure
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: c84d015da907c8792f09d1d60e6bc8eddb7e2957
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005595"
---
# <a name="troubleshoot-a-problem-azure-vm-by-using-nested-virtualization-in-azure"></a>Een probleem virtuele Azure-machine op te lossen met behulp van geneste virtualisatie in Azure

In dit artikel laat zien hoe een geneste virtualisatie-omgeving maken in Microsoft Azure, zodat u de schijf van het probleem VM op de Hyper-V-host (redden VM koppelen kunt) voor het oplossen van problemen.

## <a name="prerequisites"></a>Vereisten

Voor het koppelen van de probleem-VM, moet de VM redden voldoen aan de volgende vereisten:

-   De redden virtuele machine moet zich in dezelfde locatie als de probleem-VM.

-   De redden virtuele machine moet zich in dezelfde resourcegroep bevinden als de probleem-VM.

-   De redden virtuele machine moet hetzelfde type Opslagaccount (Standard of Premium) gebruiken als de probleem-VM.

## <a name="step-1-create-a-rescue-vm-and-install-hyper-v-role"></a>Stap 1: Een redden virtuele machine maken en Hyper-V-functie installeren

1.  Maak een nieuwe redden VM:

    -  Besturingssysteem: Windows Server 2016 Datacenter

    -  Grootte: Elke V3-serie met ten minste twee cores die ondersteuning voor geneste virtualisatie. Zie voor meer informatie, [Maak kennis met de nieuwe Dv3- en Ev3 VM-grootten](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).

    -  Dezelfde locatie, Storage-Account en resourcegroep als de probleem-VM.

    -  Selecteer hetzelfde opslagtype als het probleem VM (Standard of Premium).

2.  Na de redden virtuele machine is gemaakt, met de redden virtuele machine via Extern bureaublad.

3.  Selecteer in Serverbeheer **beheren** > **functies en onderdelen toevoegen**.

4.  In de **installatietype** sectie, selecteer **op basis van functie of onderdeel gebaseerde installatie**.

5.  In de **Selecteer doelserver** sectie, zorg ervoor dat de VM redden is geselecteerd.

6.  Selecteer de **Hyper-V-functie** > **onderdelen toevoegen**.

7.  Selecteer **volgende** op de **functies** sectie.

8.  Als een virtuele switch beschikbaar is, selecteert u deze. Selecteer anders **volgende**.

9.  Op de **migratie** sectie, selecteer **volgende**

10. Op de **standaardarchieven** sectie, selecteer **volgende**.

11. Schakel het selectievakje in als u wilt de server automatisch opnieuw opstarten indien nodig.

12. Selecteer **Installeren**.

13. Toestaan dat de server voor het installeren van de Hyper-V-rol. Dit duurt een paar minuten en de server automatisch opnieuw wordt opgestart.

## <a name="step-2-create-the-problem-vm-on-the-rescue-vms-hyper-v-server"></a>Stap 2: De probleem-VM maken op redden van de virtuele machine Hyper-V-server

1.  Noteer de naam van de schijf in de probleem-VM en verwijder vervolgens de probleem-VM. Zorg ervoor dat u alle gekoppelde schijven. 

2.  Koppel de besturingssysteemschijf van de virtuele machine van uw probleem als een gegevensschijf van de VM redden.

    1.  Wanneer het probleem dat virtuele machine wordt verwijderd, gaat u naar de VM redden.

    2.  Selecteer **schijven**, en vervolgens **gegevensschijf toevoegen**.

    3.  Selecteer de probleem-VM-schijf en selecteer vervolgens **opslaan**.

3.  Nadat de schijf is gekoppeld, extern bureaublad met de VM redden.

4.  Hiermee opent u Schijfbeheer (diskmgmt.msc). Zorg ervoor dat de schijf van het probleem VM is ingesteld op **Offline**.

5.  Open Hyper-V-beheer: In **Serverbeheer**, selecteer de **Hyper-V-functie**. Met de rechtermuisknop op de server en selecteer vervolgens de **Hyper-V-beheer**.

6.  In de Hyper-V-beheer met de rechtermuisknop op de VM redden en selecteer vervolgens **nieuw** > **virtuele Machine** > **volgende**.

7.  Typ een naam voor de virtuele machine en selecteer vervolgens **volgende**.

8.  Selecteer **generatie 1**.

9.  Het opstartgeheugen ingesteld op 1024 MB of meer.

10. Indien van toepassing selecteert u de Hyper-V-netwerkswitch die is gemaakt. Anders verplaatsen naar de volgende pagina.

11. Selecteer **later een virtuele harde schijf koppelen**.

    ![afbeelding van het koppelen van een optie voor de virtuele hardeschijf Later de](media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

12. Selecteer **voltooien** wanneer de virtuele machine wordt gemaakt.

13. Met de rechtermuisknop op de virtuele machine die u hebt gemaakt, en selecteer vervolgens **instellingen**.

14. Selecteer **IDE-Controller 0**, selecteer **harde schijf**, en klik vervolgens op **toevoegen**.

    ![afbeelding van de nieuwe vaste schijf wordt toegevoegd](media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

15. In **fysieke harde schijf**, selecteert u de schijf van de virtuele machine die u hebt gekoppeld aan de virtuele Azure-machine van het probleem. Als u alle schijven die worden vermeld niet ziet, controleert u als de schijf is ingesteld op offline met behulp van Schijfbeheer.

    ![afbeelding van de koppelt de schijf](media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


17. Selecteer **Apply** en vervolgens **OK**.

18. Dubbelklik op de virtuele machine en start het programma.

19. U kunt nu op de virtuele machine werken als de on-premises VM. U kunt een oplossen van problemen met stappen die u moet volgen.

## <a name="step-3-re-create-your-azure-vm-in-azure"></a>Stap 3: Uw Azure-VM in Azure voor het opnieuw maken

1.  Nadat u de virtuele machine weer online is, sluit u de virtuele machine in Hyper-V-beheer.

2.  Ga naar de [Azure-portal](https://portal.azure.com) en selecteer de redden virtuele machine > schijven, de naam van de schijf kopiëren. U gebruikt de naam in de volgende stap. De vaste schijf van de VM redden loskoppelen.

3.  Ga naar **alle resources**, zoekt u de naam van de schijf en selecteer vervolgens de schijf.

     ![afbeelding van de zoekt naar de schijf](media/troubleshoot-vm-by-use-nested-virtualization/search-disk.png)     

4. Klik op **virtuele machine maken**.

     ![afbeelding van de maakt virtuele machine van de schijf](media/troubleshoot-vm-by-use-nested-virtualization/create-vm-from-vhd.png) 

U kunt ook Azure PowerShell gebruiken om te maken van de virtuele machine van de schijf. Zie voor meer informatie, [de nieuwe virtuele machine maken van een bestaande schijf met behulp van PowerShell](../windows/create-vm-specialized.md#create-the-new-vm). 

## <a name="next-steps"></a>Volgende stappen

Als u hebt met het maken van een verbinding met uw virtuele machine problemen, Zie [oplossen RDP-verbindingen met een Azure VM](troubleshoot-rdp-connection.md). Zie voor problemen met toegang tot toepassingen die worden uitgevoerd op de virtuele machine, [verbindingsproblemen van toepassing op een Windows-VM oplossen](troubleshoot-app-connection.md).
