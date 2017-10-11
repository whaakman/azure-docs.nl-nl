---
title: Een claimable virtuele machine toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een claimable virtuele machine aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: 98950d72e90b0e178bae2fffa7644fd824a25eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Een claimable virtuele machine toevoegen aan een lab in Azure DevTest Labs
U een claimable virtuele machine toevoegen aan een lab op een vergelijkbare manier met de manier waarop u [toevoegen van een standaard virtuele machine](devtest-lab-add-vm.md) – Selecteer in een *base* dat ofwel een [aangepaste installatiekopie](devtest-lab-create-template.md), [formule](devtest-lab-manage-formulas.md), of [Marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Deze zelfstudie wordt u begeleid bij met de Azure portal een claimable virtuele machine toevoegen aan een lab in DevTest Labs en ziet u dat een gebruiker om de virtuele machine via het proces.

> [!NOTE]
> Als u virtuele machines lab via implementeert [Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md), kunt u claimable virtuele machines maken door in te stellen de **allowClaim** eigenschap op true in de sectie met eigenschappen.
>
>

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van een claimable VM aan een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer in de lijst van labs in het lab waarin u wilt maken van de claimable VM.  
1. Op de testomgeving **overzicht** blade Selecteer **+ toevoegen**.  

    ![VM-knop toevoegen](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Op de **kiezen een base** blade, selecteert u een basis voor de virtuele machine.
1. Op de **virtuele machine** blade een naam voor de nieuwe virtuele machine in de **virtuele-machinenaam** in het tekstvak.

    ![VM-labblade](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Voer een **gebruikersnaam** die administrator-bevoegdheden op de virtuele machine wordt verleend.  
1. Als u gebruiken van een wachtwoord opgeslagen wilt in uw [geheime store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), selecteer **een opgeslagen geheim**, en geef de waarde van een sleutel die overeenkomt met uw geheime (wachtwoord). Anders, voer een wachtwoord in het veld met het label **typt u een waarde**.
1. De **virtuele machine schijftype** wordt bepaald welk type opslagschijf is toegestaan voor de virtuele machines in de testomgeving.
1. Selecteer **grootte van virtuele machine** en selecteer een van de vooraf gedefinieerde items die de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine maken opgeven.
1. Selecteer **artefacten** uit de lijst van artefacten en selecteer de artefacten die u wilt toevoegen aan de basisinstallatiekopie configureren. Als u niet vertrouwd met DevTest Labs bent of configureren van artefacten, naar verwijzen de [een bestaande artefact toevoegen aan een virtuele machine](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) sectie en ga vervolgens terug hier na voltooiing.
1. Selecteer **geavanceerde instellingen** om de netwerkopties en opties van de virtuele machine te configureren. Onder **opties Claim**, kies **Ja** op de machine claimable maken.

  ![Wilt u de VM claimable maken.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Als u wilt weergeven of kopiëren van de Azure Resource Manager-sjabloon, raadpleegt u de [opslaan Azure Resource Manager-sjabloon](devtest-lab-add-vm.md#save-azure-resource-manager-template) sectie en hier terug wanneer voltooid.
1. Selecteer **maken** opgegeven virtuele machine toevoegen aan het lab.
1. De blade testlab toont de status van het maken van de VM - eerst als **maken**, klikt u vervolgens als **met** nadat de virtuele machine is gestart.


## <a name="using-a-claimable-vm"></a>Met behulp van een claimable VM

Een gebruiker kan een virtuele machine uit de lijst met 'Claimable virtuele machines' claim op een van de volgende stappen uit:

* In de lijst met 'Claimable virtuele machines' onderaan de blade overzicht van de testomgeving, met de rechtermuisknop op een van de virtuele machines in de lijst en kies **Claim machine**.

 ![Aanvragen van een specifieke claimable virtuele machine.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* Aan de bovenkant van de **overzicht** blade kiezen **een Claim**. Een willekeurige virtuele machine wordt toegewezen uit de lijst met claimable virtuele machines.

 ![Aanvraag geen claimable VM.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Nadat een gebruiker claims van een virtuele machine, omhoog verplaatst naar hun lijst met 'Mijn virtuele machines' en is niet langer claimable door een andere gebruiker.

## <a name="next-steps"></a>Volgende stappen
* Wanneer de virtuele machine is gemaakt, kunt u verbinden met de virtuele machine door te selecteren **Connect** op de blade van de VM.
* Verken de [sjablonengalerie DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)
