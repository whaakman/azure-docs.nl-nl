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
ms.date: 01/25/2019
ms.author: spelluru
ms.openlocfilehash: 05e68e13ab5aa526362e71413c105340ad07426f
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55082075"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Een virtuele machine toevoegen aan een lab in Azure DevTest Labs
Als u al hebt [maken van uw eerste virtuele machine](devtest-lab-create-first-vm.md), waarschijnlijk werd een vooraf geladen [marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Nu, als u wilt de volgende virtuele machines toevoegen aan uw lab, u kunt ook een *basis* dat ofwel een [aangepaste installatiekopie](devtest-lab-create-template.md) of een [formule](devtest-lab-manage-formulas.md). In deze zelfstudie begeleidt u stapsgewijs door de Azure portal een virtuele machine toevoegen aan een lab in DevTest Labs.

In dit artikel leest u ook hoe voor het beheren van de artefacten voor een virtuele machine in uw testomgeving.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van een virtuele machine aan een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de **DEVOPS** sectie. Als u * (star) naast **DevTest Labs** in de **DEVOPS** sectie. Deze actie worden toegevoegd **DevTest Labs** naar het menu aan de linkerkant navigatie zodat u toegang hebben tot deze eenvoudig de volgende keer. Vervolgens kunt u **DevTest Labs** in het linkermenu van de navigatie.

    ![Alle services: Selecteer DevTest Labs](./media/devtest-lab-create-lab/all-services-select.png)
1. Selecteer in de lijst met labs, in het lab waarin u wilt maken van de virtuele machine.  
2. Op van het lab **overzicht** weergeeft, schakelt **+ toevoegen**.  

    ![VM-knop toevoegen](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)
1. Op de **kiest u een base** pagina, selecteert u een marketplace-installatiekopie voor de virtuele machine.
1. Op de **basisinstellingen** tabblad van de **virtuele machine** pagina, de volgende acties uitvoeren: 
    1. Voer een naam voor de virtuele machine in de **virtuele-machinenaam** in het tekstvak. In het tekstvak is voor u door een unieke naam voor het automatisch gegenereerde vooraf ingevuld. De naam overeenkomt met de gebruikersnaam van de in uw e-mailadres gevolgd door een unieke nummer 3 cijfers. Deze functie bespaart u de tijd te denken van een computernaam en typt u dit telkens wanneer u een virtuele machine. Als u wilt, kunt u dit veld automatisch gevuld met een naam van uw keuze overschrijven. Als u wilt overschrijven de automatisch ingevulde naam voor de virtuele machine, voer een naam in de **virtuele-machinenaam** in het tekstvak.
    2. Voer een **gebruikersnaam** die administrator-bevoegdheden op de virtuele machine wordt verleend. De **gebruikersnaam** voor de machine vooraf ingevuld met een unieke naam voor het automatisch gegenereerde is. De naam overeenkomt met de gebruikersnaam van de in uw e-mailadres. Deze functie bespaart u tijd om te beslissen over een gebruikersnaam, telkens wanneer u een nieuwe virtuele machine. Nogmaals, kunt u dit veld automatisch gevuld met een gebruikersnaam van uw keuze als u wilt onderdrukken. Als u wilt overschrijven de automatisch ingevulde waarde voor de gebruikersnaam van de, typt u een waarde in de **gebruikersnaam** in het tekstvak. Deze gebruiker is verleend **beheerder** bevoegdheden op de virtuele machine.
    3. Als u de eerste virtuele machine in het lab maakt, voert u een **wachtwoord** voor de gebruiker. Sla dit wachtwoord als een standaardwachtwoord in de Azure-sleutelkluis die is gekoppeld aan het lab, selecteer **opslaan als standaardwachtwoord**. Het standaardwachtwoord wordt opgeslagen in de key vault met de naam: **VmPassword**. Wanneer u probeert te maken van latere virtuele machines in het lab **VmPassword** wordt automatisch geselecteerd voor de **wachtwoord**. Als u wilt overschrijven de waarde, schakelt u de **een geheim opgeslagen** selectievakje en voer een wachtwoord. 

        U kunt ook geheimen eerst in de key vault opslaan en vervolgens worden gebruikt tijdens het maken van een virtuele machine in het lab. Zie voor meer informatie, [Store geheimen in een key vault](devtest-lab-store-secrets-in-key-vault.md). Selecteer voor het gebruik van het wachtwoord die zijn opgeslagen in de key vault, **een geheim opgeslagen**, en geeft u de waarde van een sleutel die overeenkomt met uw geheim (wachtwoord).
    4. In de **meer opties** sectie, selecteer **grootte wijzigen**. Selecteer een van de vooraf gedefinieerde items die u opgeeft de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine te maken.
    5. Selecteer **toevoegen of verwijderen van artefacten**. Selecteert en configureert u de artefacten die u wilt toevoegen aan de basisinstallatiekopie.
    **Opmerking:** Als u geen ervaring hebt met DevTest Labs of configureren van artefacten, raadpleegt u de [een bestaande artefact toevoegen aan een virtuele machine](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) uit en keer vervolgens hier terug wanneer u klaar bent.
2. Schakel over naar de **geavanceerde instellingen** tabblad aan de bovenkant en de volgende acties uitvoeren:
    1. Als u wilt wijzigen van het virtuele netwerk waarin de virtuele machine zich bevindt, selecteert u **wijzigen VNet**. 
    2. Als het subnet wijzigen, selecteert u **wijzigen van subnet**. 
    3. Geef op of het IP-adres van de virtuele machine is **openbare, persoonlijke of gedeelde**. 
    4. Als de virtuele machine automatisch verwijderen, geef de **datum en tijd**. 
    5. Als u de virtuele machine claimbare door een lab-gebruiker, schakelt u **Ja** voor **maken deze machine claimbare** optie. 
    6. Geef het aantal van de **exemplaren van virtuele machine** dat u wilt deze beschikbaar maakt voor uw labgebruikers. 
3. Selecteer **maken** opgegeven virtuele machine toevoegen aan het lab.

   De pagina lab geeft de status van het maken van de VM - eerst als **maken**, klikt u vervolgens als **met** als de virtuele machine al is gestart.

## <a name="add-an-existing-artifact-to-a-vm"></a>Een bestaande artefact toevoegen aan een virtuele machine
Tijdens het maken van een virtuele machine, kunt u bestaande artefacten toevoegen. Elk lab bevat artefacten uit de openbare DevTest Labs-Artefactopslagplaats, evenals de artefacten die u hebt gemaakt en toegevoegd aan uw eigen Artefactopslagplaats.

* Azure DevTest Labs *artefacten* kunt u opgeven *acties* die worden uitgevoerd wanneer de virtuele machine is ingericht, zoals het uitvoeren van Windows PowerShell-scripts, Bash-opdrachten uitvoeren en installeren van software.
* Artefact *parameters* kunt u het artefact voor uw specifieke scenario aanpassen

Om te ontdekken hoe u artefacten maken, Zie het artikel [informatie over het ontwerpen van uw eigen artefacten voor gebruik met DevTest Labs](devtest-lab-artifact-author.md).

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
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
1. Op de **geselecteerd artefacten** deelvenster slepen en neerzetten van de artefacten in de gewenste volgorde. **Opmerking:** Als u problemen ondervindt met het slepen van het artefact, zorg ervoor dat u vanaf de linkerkant van het artefact sleept. 
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

1. Op de **virtuele machine** venster **weergave Azure Resource Manager-sjabloon**.
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
