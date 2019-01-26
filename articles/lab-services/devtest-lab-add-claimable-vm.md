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
ms.date: 01/25/2019
ms.author: spelluru
ms.openlocfilehash: 5ff0a70d8b370655df1eef55dac68804a1db406b
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55082143"
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Maken en beheren van claimbare virtuele machines in Azure DevTest Labs
U een claimbare virtuele machine toevoegen aan een lab op een vergelijkbare manier met de manier waarop u [toevoegen van een standard VM](devtest-lab-add-vm.md) – in een *basis* dat wil zeggen een een [aangepaste installatiekopie](devtest-lab-create-template.md), [formule](devtest-lab-manage-formulas.md) , of [Marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Deze zelfstudie leidt u stapsgewijs door de Azure-portal een claimbare virtuele machine toevoegen aan een lab in DevTest Labs en geeft de processen dat voor het claimen en de virtuele machine aanvrager een gebruiker heeft gevolgd.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van een claimbare virtuele machine aan een lab in Azure DevTest Labs
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

> [!NOTE]
> Als u een lab-virtuele machines via implementeert [Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md), kunt u claimbare virtuele machines maken door in te stellen de **allowClaim** eigenschap op ' True ' in de sectie met eigenschappen.


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
