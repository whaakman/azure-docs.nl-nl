---
title: Koppelen of ontkoppelen van een gegevensschijf aan een virtuele machine in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het koppelen of ontkoppelen van een gegevensschijf aan een virtuele machine in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 9616bf38-7db8-4915-a32a-e4f40a7a56ad
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: v-craic
ms.openlocfilehash: c1f83097fe4e5da3a46f693d37b001dbb6831527
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="attach-or-detach-a-data-disk-to-a-virtual-machine-in-azure-devtest-labs"></a>Koppelen of ontkoppelen van een gegevensschijf aan een virtuele machine in Azure DevTest Labs
[Azure-beheerde schijven](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) beheert de storage-accounts die zijn gekoppeld aan de gegevensschijven van de virtuele machine. Een gebruiker wordt een nieuwe gegevens schijf aan een VM, geeft u het type en de grootte van de schijf die nodig is en Azure maakt en beheert de schijf automatisch. De gegevensschijf kan vervolgens worden losgekoppeld van de virtuele machine en een vervolgens opnieuw later dezelfde virtuele machine, of zijn gekoppeld aan een andere virtuele machine die tot dezelfde gebruiker behoort.

Deze functionaliteit is handig voor het beheren van opslag- of software buiten elke afzonderlijke virtuele machine. Als de software of de opslag al in een gegevensschijf bestaat, kan deze worden eenvoudig gekoppeld, losgekoppeld en opnieuw worden gekoppeld aan een VM dat eigendom is van de gebruiker die eigenaar is van die gegevensschijf.

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Voordat u een gegevensschijf aan een VM koppelen, controleert u de volgende tips:

- De omvang van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie [grootten voor virtuele machines](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).
- U kunt alleen een gegevensschijf koppelen aan een virtuele machine die wordt uitgevoerd. Zorg ervoor dat de VM wordt uitgevoerd voordat u probeert een gegevensschijf koppelen.

### <a name="attach-a-new-disk"></a>Een nieuwe schijf koppelen
Volg deze stappen voor het maken en een nieuwe beheerde gegevensschijf koppelen aan een virtuele machine in Azure DevTest Labs.

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer de gewenste testomgeving uit de lijst van labs. 
1. In de lijst met **mijn virtuele machines**, selecteert u een actieve virtuele machine.
1. Selecteer in het menu aan de linkerkant **schijven**.

    ![Selecteer gegevensschijven die voor een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png)
1. Kies **Attach nieuwe** naar een nieuwe gegevensschijf maken en deze te koppelen aan de virtuele machine.

    ![Nieuwe gegevensschijf koppelen aan een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new.png)
1. Voltooi de **nieuwe schijf koppelen** deelvenster door te voeren van een naam voor de schijf, het type en de grootte.

    ![Vul het formulier 'koppelen van de nieuwe schijf'](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new-form.png)
1. Selecteer **OK**.

Na enkele ogenblikken de nieuwe gegevensschijf wordt gemaakt en gekoppeld aan de virtuele machine en wordt weergegeven in de lijst met **GEGEVENSSCHIJVEN** voor die VM.

### <a name="attach-an-existing-disk"></a>Een bestaande schijf koppelen
Volg deze stappen voor het opnieuw koppelen van een bestaande schijf van de beschikbare gegevens naar een actieve virtuele machine. 

1. Selecteer een actieve virtuele machine waarvoor u wilt een gegevensschijf opnieuw koppelen.
1. Selecteer in het menu aan de linkerkant **schijven**.
1. Selecteer **koppelen bestaande** een beschikbare gegevensschijf koppelen aan de virtuele machine.

    ![Bestaande gegevensschijf koppelen aan een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing2.png)

1. Van de **bestaande schijf koppelen** deelvenster, klik op OK.

    ![Bestaande gegevensschijf koppelen aan een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing.png)

Na enkele ogenblikken de gegevensschijf is gekoppeld aan de virtuele machine en wordt weergegeven in de lijst met **GEGEVENSSCHIJVEN** voor die VM.

## <a name="detach-a-data-disk"></a>Een gegevensschijf ontkoppelen
Wanneer u een gegevensschijf die gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Loskoppelen van de schijf van de virtuele machine verwijderd, maar blijft in de opslag voor later gebruik.

Als u wilt de bestaande gegevens op de schijf opnieuw gebruiken, kunt u het opnieuw op dezelfde virtuele machine of op een andere naam.

### <a name="detach-from-the-vms-management-pane"></a>Loskoppelen van de VM management deelvenster
1. Selecteer een virtuele machine met een gegevensschijf gekoppeld in uw lijst met virtuele machines.
1. Selecteer in het menu aan de linkerkant **schijven**.

    ![Selecteer gegevensschijven die voor een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png) 
1. In de lijst met **GEGEVENSSCHIJVEN**, selecteer de gegevensschijf die u wilt loskoppelen.
1. Selecteer **Detach** vanaf de bovenkant van het detailvenster van de schijf.

    ![Een gegevensschijf ontkoppelen](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk2.png)
1. Selecteer **Ja** om te bevestigen dat u wilt loskoppelen van de gegevensschijf.

De schijf is losgekoppeld en is beschikbaar om te koppelen aan een andere virtuele machine. 
### <a name="detach-from-the-labs-main-pane"></a>Los te koppelen van het hoofdvenster de testomgeving
1. Selecteer in het hoofdvenster van uw lab **mijn gegevensschijven**.

    ![Toegang tot het lab gegevensschijven](./media/devtest-lab-attach-detach-data-disk/devtest-lab-my-data-disks.png)
1. Met de rechtermuisknop op de gegevensschijf die u wilt loskoppelen – of Selecteer het weglatingsteken (...) – Kies **Detach**.

    ![Een gegevensschijf ontkoppelen](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk.png)
1. Selecteer **Ja** om te bevestigen dat u wilt loskoppelen.

   > [!NOTE]
   > Als een gegevensschijf al is ontkoppeld, kunt u kiezen uit de lijst met beschikbare gegevensschijven verwijderen door te selecteren **verwijderen**.
   >
   >

## <a name="upgrade-an-unmanaged-data-disk"></a>Upgrade van een niet-beheerde gegevensschijf
Als u een bestaande virtuele machine die gebruikmaakt van niet-beheerde gegevensschijven hebt, kunt u eenvoudig de virtuele machine voor het gebruik van beheerde schijven converteren. Dit proces converteert zowel schijf met het besturingssysteem en eventuele aangesloten gegevensschijven.

Upgrade van een niet-beheerde gegevensschijf moet u de stappen in dit artikel voor [loskoppelen van de gegevensschijf](#detach-a-data-disk) van een niet-beheerde virtuele machine. Vervolgens [koppelt u de schijf opnieuw](#attach-an-existing-disk) aan een beheerde VM automatisch bijwerken van de gegevens schijf van niet-begeleid naar beheerd.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Informatie over het beheren van gegevensschijven voor [claimable virtuele machines](devtest-lab-add-claimable-vm.md#unclaim-a-vm).

