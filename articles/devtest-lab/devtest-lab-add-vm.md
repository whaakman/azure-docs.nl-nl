---
title: Een virtuele machine toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een virtuele machine aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: v-craic
ms.openlocfilehash: 5d9fb235eae381003d96895eae9eff86c711e372
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Een virtuele machine toevoegen aan een lab in Azure DevTest Labs
Als u al hebt [gemaakt van uw eerste VM](devtest-lab-create-first-vm.md), waarschijnlijk werd ook een vooraf geladen [marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Nu, als u wilt de volgende virtuele machines toevoegen aan uw testomgeving, kunt u ook kiezen een *base* dat ofwel een [aangepaste installatiekopie](devtest-lab-create-template.md) of een [formule](devtest-lab-manage-formulas.md). Deze zelfstudie leert u met de Azure portal een virtuele machine toevoegen aan een lab in DevTest Labs.

Dit artikel ziet u ook de artefacten beheren voor een virtuele machine in uw testomgeving.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van een virtuele machine aan een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer in de lijst van labs in het lab waarin u wilt maken van de virtuele machine.  
1. Op de testomgeving **overzicht** blade Selecteer **+ toevoegen**.  

    ![VM-knop toevoegen](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Op de **kiezen een base** blade, selecteert u een basis voor de virtuele machine.
1. Op de **virtuele machine** blade een naam voor de nieuwe virtuele machine in de **virtuele-machinenaam** in het tekstvak.

    ![VM-labblade](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Voer een **gebruikersnaam** die administrator-bevoegdheden op de virtuele machine wordt verleend.  
1. Als u gebruiken van een wachtwoord opgeslagen wilt in uw [geheime store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), selecteer **een opgeslagen geheim**, en geef de waarde van een sleutel die overeenkomt met uw geheime (wachtwoord). Anders, voer een wachtwoord in het veld met het label **typt u een waarde**.
1. De **virtuele machine schijftype** wordt bepaald welk type opslagschijf is toegestaan voor de virtuele machines in de testomgeving.
1. Selecteer **grootte van virtuele machine** en selecteer een van de vooraf gedefinieerde items die de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine maken opgeven.
1. Selecteer **artefacten** - uit de lijst met artefacten - en selecteer de artefacten die u wilt toevoegen aan de basisinstallatiekopie configureren.
    **Opmerking:** als u niet vertrouwd met DevTest Labs bent of configureren van artefacten, naar verwijzen de [een bestaande artefact toevoegen aan een virtuele machine](#add-an-existing-artifact-to-a-vm) sectie en ga vervolgens terug hier na voltooiing.
1. Selecteer **geavanceerde instellingen** om de netwerkopties en opties van de virtuele machine te configureren. 

   Als u wilt een vervaldatum-optie is ingesteld, kies het pictogram Agenda om op te geven van een datum op waarop de virtuele machine automatisch worden verwijderd.  Standaard wordt de virtuele machine nooit verlopen. 
1. Als u wilt weergeven of kopiëren van de Azure Resource Manager-sjabloon, raadpleegt u de [opslaan Azure Resource Manager-sjabloon](#save-azure-resource-manager-template) sectie en hier terug wanneer voltooid.
1. Selecteer **maken** opgegeven virtuele machine toevoegen aan het lab.
1. De blade testlab toont de status van het maken van de VM - eerst als **maken**, klikt u vervolgens als **met** nadat de virtuele machine is gestart.

> [!NOTE]
> [Toevoegen van een claimable VM](devtest-lab-add-claimable-vm.md) ziet u hoe u de virtuele machine claimable zodat deze beschikbaar voor gebruik door elke gebruiker in de testomgeving.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Een bestaande artefact toevoegen aan een virtuele machine
U kunt bestaande artefacten toevoegen tijdens het maken van een virtuele machine. Elke lab bevat artefacten uit de openbare DevTest Labs-Artefactopslagplaats evenals artefacten die u hebt gemaakt en toegevoegd aan uw eigen Artefactopslagplaats.

* Azure DevTest Labs *artefacten* kunt u opgeven *acties* die worden uitgevoerd wanneer de virtuele machine is ingericht, zoals Windows PowerShell-scripts uitvoeren, Bash opdrachten uit te voeren en het installeren van software.
* Artefacten *parameters* kunt u het artefact voor uw specifieke scenario aanpassen

Voor het detecteren van het maken van artefacten, Zie het artikel [informatie over het ontwerpen van uw eigen artefacten voor gebruik met DevTest Labs](devtest-lab-artifact-author.md).

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer in de lijst van labs, de testomgeving met de virtuele machine die u wilt werken.  
1. Selecteer **mijn virtuele machines**.
1. Selecteer de gewenste virtuele machine.
1. Selecteer **artefacten**. 
1. Selecteer **toepassen artefacten**.
1. Op de **toepassen artefacten** blade, selecteert u het artefact die u wilt toevoegen aan de virtuele machine.
1. Op de **toevoegen artefact** blade, voert u de vereiste parameterwaarden en eventuele optionele parameters die u nodig hebt.  
1. Selecteer **toevoegen** het artefact toevoegen en Ga terug naar de **toepassen artefacten** blade.
1. Doorgaan met het toevoegen van artefacten nodig zijn voor uw virtuele machine.
1. Nadat u uw artefacten hebt toegevoegd, kunt u [de volgorde waarin de artefacten zijn uitgevoerd wijzigen](#change-the-order-in-which-artifacts-are-run). U kunt ook teruggaan naar [weergeven of wijzigen van een artefact](#view-or-modify-an-artifact).
1. Wanneer u klaar bent met het toevoegen van artefacten selecteren **toepassen**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Wijzig de volgorde waarin artefacten worden uitgevoerd
Standaard worden de acties van de artefacten uitgevoerd in de volgorde waarin ze worden toegevoegd aan de virtuele machine. De volgende stappen laten zien hoe de volgorde waarin de artefacten worden uitgevoerd te wijzigen.

1. Aan de bovenkant van de **toepassen artefacten** blade, selecteer de koppeling die aangeeft hoeveel artefacten die zijn toegevoegd aan de virtuele machine.
   
    ![Aantal artefacten toegevoegd aan de virtuele machine](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Op de **geselecteerd artefacten** blade slepen en neerzetten van de artefacten in de gewenste volgorde. **Opmerking:** als u problemen ondervindt bij het slepen van het artefact, zorgt u ervoor dat u vanaf de linkerkant van het artefact sleept. 
1. Selecteer **OK** wanneer u gereed bent.  

## <a name="view-or-modify-an-artifact"></a>Weergeven of wijzigen van een artefact
De volgende stappen laten zien hoe weergeven of wijzigen van de parameters van een artefact:

1. Aan de bovenkant van de **toepassen artefacten** blade, selecteer de koppeling die aangeeft hoeveel artefacten die zijn toegevoegd aan de virtuele machine.
   
    ![Aantal artefacten toegevoegd aan de virtuele machine](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Op de **geselecteerd artefacten** blade, selecteer de artefacten die u wilt weergeven of bewerken.  
1. Op de **toevoegen artefact** blade, controleert alle wijzigingen die nodig zijn en selecteer **OK** sluiten de **toevoegen artefact** blade.
1. Selecteer **OK** sluiten de **geselecteerd artefacten** blade.

## <a name="save-azure-resource-manager-template"></a>Azure Resource Manager-sjabloon opslaan
Een Azure Resource Manager-sjabloon biedt een declaratieve manier om een herhaalbare implementatie te definiëren. De volgende stappen wordt uitgelegd hoe de Azure Resource Manager-sjabloon opslaan voor de virtuele machine wordt gemaakt.
Wanneer opgeslagen, kunt u de Azure Resource Manager-sjabloon [implementeren van nieuwe virtuele machines met Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Op de **virtuele machine** blade Selecteer **ARM-sjabloon weergeven**.
2. Op de **weergave Azure Resource Manager-sjabloon** blade, selecteert u de sjabloontekst.
3. De geselecteerde tekst naar het Klembord kopiëren.
4. Selecteer **OK** sluiten de **weergave Azure Resource Manager-sjabloon blade**.
5. Open een teksteditor.
6. In de sjabloontekst van het Klembord te plakken.
7. Sla het bestand voor later gebruik.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Volgende stappen
* Wanneer de virtuele machine is gemaakt, kunt u verbinden met de virtuele machine door te selecteren **Connect** op de blade van de VM.
* Meer informatie over hoe [aangepaste artefacten maken voor uw DevTest Labs VM](devtest-lab-artifact-author.md).
* Verken de [DevTest Labs Azure Resource Manager QuickStart sjablonengalerie](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
