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
ms.openlocfilehash: c72e10991e27f7d616703e635ee6e1a18122afc5
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078066"
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Uw eerste virtuele machine maken in een lab in Azure DevTest Labs

Wanneer u in eerste instantie toegang DevTest Labs tot en uw eerste virtuele machine wilt maken, zult u waarschijnlijk doen dit met een vooraf geladen [base marketplace-installatiekopie](devtest-lab-configure-marketplace-images.md). Later zal ook het mogelijk om te kiezen uit een [aangepaste installatiekopie en een formule](devtest-lab-add-vm.md) bij het maken van meer virtuele machines. 

In deze zelfstudie begeleidt u stapsgewijs door de Azure portal naar uw eerste virtuele machine toevoegen aan een lab in DevTest Labs.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Stappen voor het toevoegen van uw eerste virtuele machine aan een lab in Azure DevTest Labs
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

## <a name="next-steps"></a>Volgende stappen
* Nadat de virtuele machine is gemaakt, kunt u verbinding met de virtuele machine met selecteren **Connect** op de pagina van de virtuele machine.
* Bekijk [een virtuele machine toevoegen aan een lab](devtest-lab-add-vm.md) voor meer informatie over het toevoegen van de volgende virtuele machines in uw testomgeving.
* Verken de [DevTest Labs Azure Resource Manager QuickStart-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
