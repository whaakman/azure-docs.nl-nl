---
title: Uw eerste virtuele machine maken in een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het maken van uw eerste virtuele machine in een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 93ce9feaf52282b9477d49eaf270d6d89dca7811
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232525"
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Uw eerste virtuele machine maken in een lab in Azure DevTest Labs

Wanneer u in eerste instantie toegang DevTest Labs tot en uw eerste virtuele machine wilt maken, zult u waarschijnlijk doen dit met een vooraf geladen [base marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Later zal ook het mogelijk om te kiezen uit een [aangepaste installatiekopie en een formule](devtest-lab-add-vm.md) bij het maken van meer virtuele machines. 

In deze zelfstudie begeleidt u stapsgewijs door de Azure portal naar uw eerste virtuele machine toevoegen aan een lab in DevTest Labs.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van uw eerste virtuele machine aan een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer in de lijst met labs, in het lab waarin u wilt maken van de virtuele machine.  
1. Op van het lab **overzicht** Selecteer **+ toevoegen**.  

    ![VM-knop toevoegen](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Op de **kiest u een base** blade, selecteer een marketplace met installatiekopieën voor de virtuele machine.
1. Op de **virtuele machine** blade, voer een naam voor de nieuwe virtuele machine in de **virtuele-machinenaam** in het tekstvak.

    ![Lab VM-beheerblade](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Voer een **gebruikersnaam** die administrator-bevoegdheden op de virtuele machine wordt verleend.  
1. Geef een wachtwoord in het tekstveld met het label **typt u een waarde**.
1. De **VM-schijftype** bepaalt welk opslagtype voor de schijf is toegestaan voor de virtuele machines in het lab.
1. Selecteer **grootte van virtuele machine** en selecteer een van de vooraf gedefinieerde items die u opgeeft de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine te maken.
1. Selecteer **artefacten** en - uit de lijst met artefacten - selecteren en configureren van de artefacten die u wilt toevoegen aan de basisinstallatiekopie.
    **Opmerking:** als u geen ervaring hebt met DevTest Labs of configureren van artefacten, raadpleegt u de [een bestaande artefact toevoegen aan een virtuele machine](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) uit en keer vervolgens hier terug wanneer u klaar bent.
1. Selecteer **maken** opgegeven virtuele machine toevoegen aan het lab.

   De labblade geeft de status van het maken van de VM - eerst als **maken**, klikt u vervolgens als **met** als de virtuele machine al is gestart.

## <a name="next-steps"></a>Volgende stappen
* Nadat de virtuele machine is gemaakt, kunt u verbinding met de virtuele machine met selecteren **Connect** op de blade van de virtuele machine.
* Bekijk [een virtuele machine toevoegen aan een lab](devtest-lab-add-vm.md) voor meer informatie over het toevoegen van de volgende virtuele machines in uw testomgeving.
* Verken de [DevTest Labs Azure Resource Manager QuickStart-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
