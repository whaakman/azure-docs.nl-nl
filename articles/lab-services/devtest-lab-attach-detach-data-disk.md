---
title: Koppelen of ontkoppelen van een gegevensschijf aan een virtuele machine in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het koppelen of ontkoppelen van een gegevensschijf aan een virtuele machine in Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 9616bf38-7db8-4915-a32a-e4f40a7a56ad
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 2e168867ed342fb0b0545b5fdc330ba790f78de0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243552"
---
# <a name="attach-or-detach-a-data-disk-to-a-virtual-machine-in-azure-devtest-labs"></a>Koppelen of ontkoppelen van een gegevensschijf aan een virtuele machine in Azure DevTest Labs
[Azure Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) beheert de storage-accounts die zijn gekoppeld aan de gegevensschijven van de virtuele machine. Een gebruiker wordt een nieuwe gegevensschijf aan een virtuele machine, Hiermee geeft u het type en de hoeveelheid schijfruimte die nodig is, en Azure maakt en beheert de schijf automatisch. De gegevensschijf kan vervolgens worden losgekoppeld van de virtuele machine en een hoger op dezelfde virtuele machine, of zijn gekoppeld aan een andere virtuele machine die deel uitmaakt van dezelfde gebruiker opnieuw gekoppeld.

Deze functionaliteit is handig voor het beheren van opslag- of software buiten elke afzonderlijke virtuele machine. Als de opslag of de software al in een gegevensschijf bestaat, kan worden eenvoudig die zijn gekoppeld, losgekoppeld, en opnieuw gekoppeld aan elke virtuele machine die eigendom is van de gebruiker die eigenaar is van die gegevensschijf.

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Voordat u een gegevensschijf aan een virtuele machine koppelen, controleert u de volgende tips:

- De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie, [grootten voor virtuele machines](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).
- U kunt alleen een gegevensschijf koppelen aan een virtuele machine die wordt uitgevoerd. Zorg ervoor dat de virtuele machine wordt uitgevoerd voordat u probeert een gegevensschijf koppelen.

### <a name="attach-a-new-disk"></a>Een nieuwe schijf koppelen
Volg deze stappen voor het maken en een nieuwe beheerde gegevensschijf koppelen aan een virtuele machine in Azure DevTest Labs.

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer de gewenste lab in de lijst met labs. 
1. In de lijst van **mijn virtuele machines**, selecteert u een actieve virtuele machine.
1. Selecteer in het menu aan de linkerkant, **schijven**.

    ![Selecteer de gegevensschijven voor een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png)
1. Kies **koppelen nieuwe** te maken van een nieuwe gegevensschijf koppelen aan de virtuele machine.

    ![Nieuwe gegevensschijf koppelen aan een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new.png)
1. Voltooi de **nieuwe schijf koppelen** deelvenster door in te voeren van een naam-gegevensschijf, type en grootte.

    ![Vul het formulier "nieuwe schijf koppelen"](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new-form.png)
1. Selecteer **OK**.

Na enkele ogenblikken wordt de nieuwe gegevensschijf wordt gemaakt en gekoppeld aan de virtuele machine en wordt weergegeven in de lijst met **GEGEVENSSCHIJVEN** voor die VM.

### <a name="attach-an-existing-disk"></a>Een bestaande schijf koppelen
Volg deze stappen voor het koppelen van een bestaande schijf van de beschikbare gegevens aan een actieve VM. 

1. Selecteer een actieve virtuele machine waarvoor u een gegevensschijf wilt maken.
1. Selecteer in het menu aan de linkerkant, **schijven**.
1. Selecteer **bestaande koppelen** een beschikbare gegevensschijf koppelen aan de virtuele machine.

    ![Bestaande gegevensschijf koppelen aan een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing2.png)

1. Uit de **bestaande schijf koppelen** deelvenster, klik op OK.

    ![Bestaande gegevensschijf koppelen aan een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing.png)

Na enkele ogenblikken de gegevensschijf is gekoppeld aan de virtuele machine en wordt weergegeven in de lijst met **GEGEVENSSCHIJVEN** voor die VM.

## <a name="detach-a-data-disk"></a>Een gegevensschijf ontkoppelen
Wanneer u een gegevensschijf die gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Bezig met ontkoppelen wordt de schijf van de virtuele machine verwijderd, maar blijft in de opslag voor later gebruik.

Als u de bestaande gegevens op de schijf opnieuw gebruiken wilt, kunt u het koppelen aan dezelfde virtuele machine of aan een andere naam.

### <a name="detach-from-the-vms-management-pane"></a>Loskoppelen van het deelvenster van de VM-management
1. Selecteer in de lijst van virtuele machines een virtuele machine waarvoor een gegevensschijf die is gekoppeld.
1. Selecteer in het menu aan de linkerkant, **schijven**.

    ![Selecteer de gegevensschijven voor een virtuele machine](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png) 
1. In de lijst van **GEGEVENSSCHIJVEN**, selecteert u de gegevensschijf die u wilt loskoppelen.
1. Selecteer **loskoppelen** vanaf de bovenkant van deelvenster met details van de schijf.

    ![Een gegevensschijf ontkoppelen](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk2.png)
1. Selecteer **Ja** om te bevestigen dat u wilt loskoppelen van de gegevensschijf.

De schijf is losgekoppeld en is beschikbaar om te koppelen aan een andere virtuele machine. 
### <a name="detach-from-the-labs-main-pane"></a>Loskoppelen van het hoofdvenster van de testomgeving
1. Selecteer in het hoofdvenster van uw lab, **Moje datové disky**.

    ![Toegang tot de gegevensschijven van de testomgeving](./media/devtest-lab-attach-detach-data-disk/devtest-lab-my-data-disks.png)
1. Met de rechtermuisknop op de gegevensschijf die u wilt loskoppelen – of Selecteer het weglatingsteken (...) – en kies **loskoppelen**.

    ![Een gegevensschijf ontkoppelen](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk.png)
1. Selecteer **Ja** om te bevestigen dat u wilt loskoppelen.

   > [!NOTE]
   > Als een gegevensschijf al is losgekoppeld, kunt u kiezen om deze te verwijderen uit de lijst met beschikbare gegevensschijven hiervoor **verwijderen**.
   >
   >

## <a name="upgrade-an-unmanaged-data-disk"></a>Upgrade van een niet-beheerde gegevensschijf
Als u een bestaande virtuele machine die gebruikmaakt van niet-beheerde gegevensschijven hebt, kunt u eenvoudig de virtuele machine voor het gebruik van beheerde schijven converteren. Dit proces converteert zowel schijf met het besturingssysteem en eventuele gekoppelde gegevensschijven.

Als u een niet-beheerde gegevensschijf bijwerken, volg de stappen in dit artikel voor [loskoppelen van de gegevensschijf](#detach-a-data-disk) van een niet-beheerde virtuele machine. Vervolgens [de schijf opnieuw koppelen](#attach-an-existing-disk) naar een beheerde virtuele machine automatisch bijwerken van de gegevens schijf van niet-beheerde beheerd.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Informatie over het beheren van gegevensschijven voor [claimbare virtuele machines](devtest-lab-add-claimable-vm.md#unclaim-a-vm).

