---
title: Maken en beheren van claimbare virtuele machines in een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een claimbare virtuele machine aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2018
ms.author: spelluru
ms.openlocfilehash: 3bfb674fa66f0701a099d237f4e760453c7b6a6e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232124"
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Maken en beheren van claimbare virtuele machines in Azure DevTest Labs
U een claimbare virtuele machine toevoegen aan een lab op een vergelijkbare manier met de manier waarop u [toevoegen van een standard VM](devtest-lab-add-vm.md) – in een *basis* dat wil zeggen een een [aangepaste installatiekopie](devtest-lab-create-template.md), [formule](devtest-lab-manage-formulas.md) , of [Marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Deze zelfstudie leidt u stapsgewijs door de Azure-portal een claimbare virtuele machine toevoegen aan een lab in DevTest Labs en geeft de processen dat voor het claimen en de virtuele machine aanvrager een gebruiker heeft gevolgd.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van een claimbare virtuele machine aan een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer in de lijst met labs, in het lab waarin u wilt de claimbare virtuele machine maken.  
1. Op van het lab **overzicht** venster **+ toevoegen**.  

    ![VM-knop toevoegen](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Op de **kiest u een base** gebied, selecteert u een basis voor de virtuele machine.
1. In de **virtuele machine** deelvenster, voer een naam voor de nieuwe virtuele machine in de **virtuele-machinenaam** in het tekstvak.

    ![Deelvenster voor Lab-virtuele machine](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Voer een **gebruikersnaam** die administrator-bevoegdheden op de virtuele machine wordt verleend.  
1. Als u gebruiken van een wachtwoord opgeslagen wilt uw [geheime store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), selecteer **een geheim opgeslagen**, en geeft u de waarde van een sleutel die overeenkomt met uw geheim (wachtwoord). Voer anders een wachtwoord in het tekstveld met het label **typt u een waarde**.
1. De **VM-schijftype** bepaalt welk opslagtype voor de schijf is toegestaan voor de virtuele machines in het lab.
1. Selecteer **grootte van virtuele machine** en selecteer een van de vooraf gedefinieerde items die u opgeeft de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine te maken.
1. Selecteer **artefacten** en in de lijst van artefacten, selecteren en configureren van de artefacten die u wilt toevoegen aan de basisinstallatiekopie. Als u geen ervaring hebt met DevTest Labs of configureren van artefacten, raadpleegt u de [een bestaande artefact toevoegen aan een virtuele machine](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) uit en keer vervolgens hier terug wanneer u klaar bent.
1. Selecteer **geavanceerde instellingen** voor het configureren van de virtuele machine netwerkmogelijkheden en opties. Onder **opties Claim**, kiest u **Ja** naar de machine claimbare maken.

  ![Kies op de virtuele machine claimbare maken.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Als u wilt weergeven of de Azure Resource Manager-sjabloon te kopiëren, raadpleegt u de [opslaan Azure Resource Manager-sjabloon](devtest-lab-add-vm.md#save-azure-resource-manager-template) uit en retourneren hier als u klaar bent.
1. Selecteer **maken** opgegeven virtuele machine toevoegen aan het lab.

   De status van het maken van de virtuele machine wordt weergegeven, als eerste **maken**, klikt u vervolgens als **met** als de virtuele machine al is gestart.

> [!NOTE]
> Als u een lab-virtuele machines via implementeert [Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md), kunt u claimbare virtuele machines maken door in te stellen de **allowClaim** eigenschap op ' True ' in de sectie met eigenschappen.
>
>

## <a name="using-a-claimable-vm"></a>Met behulp van een claimbare virtuele machine

Een gebruiker kan een virtuele machine in de lijst 'Claimbare virtuele machines' claim op een van deze stappen:

* In de lijst met 'Claimbare virtuele machines' aan de onderkant van van het lab "Overview" deelvenster, met de rechtermuisknop op een van de virtuele machines in de lijst en kies **Claim machine**.

 ![Vragen om een specifieke claimbare virtuele machine.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* Kies aan de bovenkant van het deelvenster 'Overzicht' **een Claim**. Een willekeurige virtuele machine wordt toegewezen uit de lijst met claimbare virtuele machines.

 ![Vragen om alle claimbare virtuele machines.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Nadat een gebruiker claims van een virtuele machine, wordt verplaatst van in de lijst met 'Mijn virtuele machines' en is niet langer claimbare door een andere gebruiker.

## <a name="unclaim-a-vm"></a>Aanvrager van een virtuele machine

Wanneer een gebruiker klaar is met een geclaimde virtuele machine en het beschikbaar maken voor iemand anders wil, kunnen ze de geclaimde VM terugkeren naar de lijst van claimbare virtuele machines op een van deze stappen:

- In de lijst 'Mijn virtuele machines', met de rechtermuisknop op een van de virtuele machines in de lijst met – of Selecteer het weglatingsteken (...) – en kies **Unclaim**.

  ![Aanvrager van een virtuele machine in de lijst met virtuele machine.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM2.png)

- Selecteer een virtuele machine naar het deelvenster beheer openen en selecteer vervolgens in de lijst 'Mijn virtuele machines', **Unclaim** in de bovenste menubalk.

  ![Aanvrager van een virtuele machine in het deelvenster van de VM-beheer.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM.png)

Wanneer een gebruiker een virtuele machine unclaims, hebben ze niet meer machtigingen voor die specifieke lab-VM.

### <a name="transferring-the-data-disk"></a>Overdracht van de gegevensschijf
Als een claimbare virtuele machine heeft een gegevensschijf die is gekoppeld aan een gebruiker en het unclaims, de gegevensschijf blijft van toepassing op de virtuele machine. Wanneer claims een andere gebruiker vervolgens dat de virtuele machine, die nieuwe gebruiker claims de gegevensschijf, evenals de virtuele machine.

Dit staat bekend als 'overdragen van de gegevensschijf'. De gegevensschijf wordt vervolgens beschikbaar in de lijst met de nieuwe gebruiker **Moje datové disky** kunnen beheren.

![Aanvrager gegevensschijven.](./media/devtest-lab-add-vm/devtestlab-unclaim-datadisks.png)



## <a name="next-steps"></a>Volgende stappen
* Zodra deze is gemaakt, kunt u verbinding met de virtuele machine met selecteren **Connect** in het deelvenster Beheer.
* Verken de [DevTest Labs Azure Resource Manager-sjabloon snelstartgalerie](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
