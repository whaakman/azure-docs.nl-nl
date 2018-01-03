---
title: Uw eerste virtuele machine maken in een testomgeving in Azure DevTest Labs | Microsoft Docs
description: Informatie over het maken van uw eerste virtuele machine in een testomgeving in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: v-craic
ms.openlocfilehash: c807789dc2b47fe3632fc4ecf597904d8b0bbf6f
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Uw eerste virtuele machine maken in een testomgeving in Azure DevTest Labs

Wanneer u in eerste instantie DevTest Labs openen en u wilt maken van uw eerste virtuele machine, wordt waarschijnlijk hiervoor met behulp van een vooraf geladen [base marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Later in het moet ook mogelijk om te kiezen uit een [aangepaste installatiekopie en een formule](devtest-lab-add-vm.md) bij het maken van meer virtuele machines. 

Deze zelfstudie leert u met de Azure portal uw eerste virtuele machine toevoegen aan een lab in DevTest Labs.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van uw eerste virtuele machine aan een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer in de lijst van labs in het lab waarin u wilt maken van de virtuele machine.  
1. Op de testomgeving **overzicht** blade Selecteer **+ toevoegen**.  

    ![VM-knop toevoegen](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Op de **kiezen een base** blade, selecteer een marketplace-installatiekopie voor de virtuele machine.
1. Op de **virtuele machine** blade een naam voor de nieuwe virtuele machine in de **virtuele-machinenaam** in het tekstvak.

    ![VM-labblade](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Voer een **gebruikersnaam** die administrator-bevoegdheden op de virtuele machine wordt verleend.  
1. Voer een wachtwoord in het veld met het label **typt u een waarde**.
1. De **virtuele machine schijftype** wordt bepaald welk type opslagschijf is toegestaan voor de virtuele machines in de testomgeving.
1. Selecteer **grootte van virtuele machine** en selecteer een van de vooraf gedefinieerde items die de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine maken opgeven.
1. Selecteer **artefacten** - uit de lijst met artefacten - en selecteer de artefacten die u wilt toevoegen aan de basisinstallatiekopie configureren.
    **Opmerking:** als u niet vertrouwd met DevTest Labs bent of configureren van artefacten, naar verwijzen de [een bestaande artefact toevoegen aan een virtuele machine](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) sectie en ga vervolgens terug hier na voltooiing.
1. Selecteer **maken** opgegeven virtuele machine toevoegen aan het lab.

   De blade testlab toont de status van het maken van de VM - eerst als **maken**, klikt u vervolgens als **met** nadat de virtuele machine is gestart.

## <a name="next-steps"></a>Volgende stappen
* Wanneer de virtuele machine is gemaakt, kunt u verbinden met de virtuele machine door te selecteren **Connect** op de blade van de VM.
* Bekijk [een virtuele machine toevoegen aan een lab](devtest-lab-add-vm.md) voor gedetailleerde informatie over het toevoegen van de volgende virtuele machines in uw testomgeving.
* Verken de [DevTest Labs Azure Resource Manager QuickStart sjablonengalerie](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
