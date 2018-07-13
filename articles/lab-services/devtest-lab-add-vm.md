---
title: Een virtuele machine toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een virtuele machine aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: 9ddf44ef933270c08b42f67387866cd7a3b34719
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004076"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Een virtuele machine toevoegen aan een lab in Azure DevTest Labs
Als u al hebt [maken van uw eerste virtuele machine](devtest-lab-create-first-vm.md), waarschijnlijk werd een vooraf geladen [marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Nu, als u wilt de volgende virtuele machines toevoegen aan uw lab, u kunt ook een *basis* dat ofwel een [aangepaste installatiekopie](devtest-lab-create-template.md) of een [formule](devtest-lab-manage-formulas.md). In deze zelfstudie begeleidt u stapsgewijs door de Azure portal een virtuele machine toevoegen aan een lab in DevTest Labs.

In dit artikel leest u ook hoe voor het beheren van de artefacten voor een virtuele machine in uw testomgeving.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van een virtuele machine aan een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer in de lijst met labs, in het lab waarin u wilt maken van de virtuele machine.  
1. Op van het lab **overzicht** venster **+ toevoegen**.  

    ![VM-knop toevoegen](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Op de **kiest u een base** deelvenster, selecteert u een basis voor de virtuele machine.
1. Op de **virtuele machine** deelvenster, voer een naam voor de nieuwe virtuele machine in de **virtuele-machinenaam** in het tekstvak.

    ![Deelvenster voor Lab-virtuele machine](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Voer een **gebruikersnaam** die administrator-bevoegdheden op de virtuele machine wordt verleend.  
1. Als u gebruiken van een wachtwoord opgeslagen wilt een [Azure sleutelkluis](devtest-lab-store-secrets-in-key-vault.md), selecteer **een geheim opgeslagen**, en geeft u de waarde van een sleutel die overeenkomt met uw geheim (wachtwoord). Voer anders een wachtwoord in het tekstveld met het label **typt u een waarde**. Zie voor meer informatie over het opslaan van geheimen in een key vault en het gebruik ervan bij het maken van de labresources, [Store geheimen in Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md).
1. De **VM-schijftype** bepaalt welk opslagtype voor de schijf is toegestaan voor de virtuele machines in het lab.
2. Selecteer **grootte van virtuele machine** en selecteer een van de vooraf gedefinieerde items die u opgeeft de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine te maken.
3. Selecteer **artefacten** en - uit de lijst met artefacten - selecteren en configureren van de artefacten die u wilt toevoegen aan de basisinstallatiekopie.
    **Opmerking:** als u geen ervaring hebt met DevTest Labs of configureren van artefacten, raadpleegt u de [een bestaande artefact toevoegen aan een virtuele machine](#add-an-existing-artifact-to-a-vm) uit en keer vervolgens hier terug wanneer u klaar bent.
4. Selecteer **geavanceerde instellingen** voor het configureren van de virtuele machine netwerkmogelijkheden en opties. 

   Als u wilt een vervaldatum-optie is ingesteld, kiest u het kalenderpictogram om op te geven van een datum op waarop de virtuele machine automatisch worden verwijderd.  Standaard wordt de virtuele machine nooit verlopen. 
1. Als u wilt weergeven of de Azure Resource Manager-sjabloon te kopiëren, raadpleegt u de [opslaan Azure Resource Manager-sjabloon](#save-azure-resource-manager-template) uit en retourneren hier als u klaar bent.
1. Selecteer **maken** opgegeven virtuele machine toevoegen aan het lab.
1. Het lab-deelvenster geeft de status van het maken van de VM - eerst als **maken**, klikt u vervolgens als **met** als de virtuele machine al is gestart.

> [!NOTE]
> [Toevoegen van een claimbare virtuele machine](devtest-lab-add-claimable-vm.md) ziet u hoe u de virtuele machine claimbare zodat deze beschikbaar voor gebruik door een gebruiker in het lab.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Een bestaande artefact toevoegen aan een virtuele machine
Tijdens het maken van een virtuele machine, kunt u bestaande artefacten toevoegen. Elk lab bevat artefacten uit de openbare DevTest Labs-Artefactopslagplaats, evenals de artefacten die u hebt gemaakt en toegevoegd aan uw eigen Artefactopslagplaats.

* Azure DevTest Labs *artefacten* kunt u opgeven *acties* die worden uitgevoerd wanneer de virtuele machine is ingericht, zoals het uitvoeren van Windows PowerShell-scripts, Bash-opdrachten uitvoeren en installeren van software.
* Artefact *parameters* kunt u het artefact voor uw specifieke scenario aanpassen

Om te ontdekken hoe u artefacten maken, Zie het artikel [informatie over het ontwerpen van uw eigen artefacten voor gebruik met DevTest Labs](devtest-lab-artifact-author.md).

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer in de lijst met labs, in het lab met de virtuele machine die u wilt werken.  
1. Selecteer **mijn virtuele machines**.
1. Selecteer de gewenste virtuele machine.
1. Selecteer **artefacten beheren**. 
1. Selecteer **toepassen artefacten**.
1. Op de **toepassen artefacten** deelvenster, selecteert u het artefact dat u wilt toevoegen aan de virtuele machine.
1. Op de **toevoegen artefact** deelvenster, voert u de vereiste parameterwaarden en eventuele optionele parameters die u nodig hebt.  
1. Selecteer **toevoegen** het artefact toevoegen en terugkeren naar de **toepassen artefacten** deelvenster.
1. Doorgaan met het toevoegen van artefacten, zoals die nodig zijn voor uw virtuele machine.
1. Als u uw artefacten hebt toegevoegd, kunt u [wijzigt de volgorde waarin de artefacten zijn uitgevoerd](#change-the-order-in-which-artifacts-are-run). U kunt ook teruggaan naar [weergeven of wijzigen van een artefact](#view-or-modify-an-artifact).
1. Wanneer u klaar bent met het artefacten toe te voegen, selecteert u **toepassen**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Wijzig de volgorde waarin artefacten worden uitgevoerd
De acties van de artefacten worden standaard uitgevoerd in de volgorde waarin ze worden toegevoegd aan de virtuele machine. De volgende stappen laten zien hoe u de volgorde waarin de artefacten worden uitgevoerd.

1. Aan de bovenkant van de **toepassen artefacten** deelvenster, selecteert u de koppeling met het nummer van de artefacten die zijn toegevoegd aan de virtuele machine.
   
    ![Aantal artefacten toegevoegd aan de virtuele machine](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Op de **geselecteerd artefacten** deelvenster slepen en neerzetten van de artefacten in de gewenste volgorde. **Opmerking:** als u problemen ondervindt met het slepen van het artefact, zorgt u ervoor dat u vanaf de linkerkant van het artefact sleept. 
1. Selecteer **OK** wanneer u gereed bent.  

## <a name="view-or-modify-an-artifact"></a>Weergeven of wijzigen van een artefact
De volgende stappen laten zien hoe u weergeven of wijzigen van de parameters van een artefact:

1. Aan de bovenkant van de **toepassen artefacten** deelvenster, selecteert u de koppeling met het nummer van de artefacten die zijn toegevoegd aan de virtuele machine.
   
    ![Aantal artefacten toegevoegd aan de virtuele machine](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Op de **geselecteerd artefacten** deelvenster, selecteert u de artefacten die u wilt weergeven of bewerken.  
1. Op de **toevoegen artefact** deelvenster, controleert alle wijzigingen die nodig zijn, en selecteer **OK** sluiten de **toevoegen artefact** deelvenster.
1. Selecteer **OK** sluiten de **geselecteerd artefacten** deelvenster.

## <a name="save-azure-resource-manager-template"></a>Azure Resource Manager-sjabloon opslaan
Een Azure Resource Manager-sjabloon biedt een declaratieve manier om een herhaalbare implementatie te definiëren. De volgende stappen wordt uitgelegd hoe u om op te slaan van de Azure Resource Manager-sjabloon voor de virtuele machine wordt gemaakt.
Nadat het is opgeslagen, kunt u de Azure Resource Manager-sjabloon [implementeren van nieuwe virtuele machines met Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Op de **virtuele machine** venster **ARM-sjabloon weergeven**.
2. Op de **weergave Azure Resource Manager-sjabloon** deelvenster, selecteert u de sjabloontekst.
3. De geselecteerde tekst naar het Klembord kopiëren.
4. Selecteer **OK** sluiten de **Azure Resource Manager-sjabloon weergeven deelvenster**.
5. Open een teksteditor.
6. Plak de sjabloontekst van het Klembord.
7. Sla het bestand voor later gebruik.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Nadat de virtuele machine is gemaakt, kunt u verbinding met de virtuele machine met selecteren **Connect** in het deelvenster van de virtuele machine.
* Meer informatie over het [aangepaste artefacten maken voor uw VM van DevTest Labs](devtest-lab-artifact-author.md).
* Verken de [DevTest Labs Azure Resource Manager QuickStart-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
