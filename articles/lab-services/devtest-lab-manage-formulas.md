---
title: Formules in Azure DevTest Labs te maken van virtuele machines beheren | Microsoft Docs
description: Meer informatie over het bijwerken en verwijderen van Azure DevTest Labs-formules
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: d8f2ae64e2f8e694de5a7cf5aa9049e63998dca0
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57452654"
---
# <a name="manage-azure-devtest-labs-formulas"></a>Azure DevTest Labs formules beheren

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

In dit artikel ziet u hoe u een formule maken vanuit een base (aangepaste installatiekopie, Marketplace-installatiekopie of een andere formule) of een bestaande virtuele machine. In dit artikel begeleidt u ook bij het beheren van bestaande formules.

## <a name="create-a-formula"></a>Een formule maken
Iedereen met DevTest Labs *gebruikers* machtigingen kunnen maken van virtuele machines met behulp van een formule als basis is. Er zijn twee manieren om formules te maken: 

* Gebruik van een basis - als u wilt de eigenschappen van de formule definiëren.
* Vanuit een bestaande lab-VM - Gebruik deze sjabloon wanneer u wilt een formule maken op basis van de instellingen van een bestaande virtuele machine.

Zie voor meer informatie over het toevoegen van gebruikers en machtigingen [eigenaars en gebruikers toevoegen in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Een formule maken van een basis
De volgende stappen begeleiden u bij het proces voor het maken van een formule van een aangepaste installatiekopie, Marketplace-installatiekopie of een andere formule.

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.

3. Selecteer de gewenste lab in de lijst met labs.  

4. Selecteer op de pagina van het lab **formules (herbruikbare bases)**.
   
    ![Menu formule](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. Op de **formules** weergeeft, schakelt **+ toevoegen**.
   
    ![Een formule toevoegen](./media/devtest-lab-create-formulas/add-formula.png)

6. Op de **kiest u een base** pagina, selecteert u de base (aangepaste installatiekopie, Marketplace-installatiekopie of formule) van waaruit u wilt maken van de formule.
   
    ![Lijst met basis](./media/devtest-lab-create-formulas/base-list.png)

7. Op de **basisinstellingen** tabblad van de **formule maken** pagina, de volgende waarden opgeven:
   
    * **Naam van formule** -Voer een naam in voor uw formule. Deze waarde wordt weergegeven in de lijst met basisinstallatiekopieën bij het maken van een virtuele machine. De naam wordt gevalideerd als u deze typen en als dat niet geldig is, een bericht geeft aan de vereisten voor een geldige naam.
    * **Gebruikersnaam** -Voer een gebruikersnaam die wordt verleend beheerdersbevoegdheden.
    * **Wachtwoord** - Voer - of Selecteer in de vervolgkeuzelijst - u op een waarde die is gekoppeld aan de geheime sleutel (wachtwoord) die u wilt gebruiken voor de opgegeven gebruiker. Zie voor meer informatie over het opslaan van geheimen in een key vault en het gebruik ervan bij het maken van de labresources, [Store geheimen in Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md).
    * **VM-grootte** : Selecteer **grootte wijzigen** de grootte van de virtuele machine wijzigen. 
    * **Artefacten** : Selecteer **toevoegen of verwijderen van artefacten** pagina waarin selecteert en configureert u de artefacten die u wilt toevoegen aan de basisinstallatiekopie. Zie voor meer informatie over artefacten [maken van aangepaste artefacten voor uw virtuele machine van Azure DevTest Labs](devtest-lab-artifact-author.md).
8. Schakel over naar de **geavanceerde instellingen** tabblad, en geeft u de volgende waarden:
    - **Virtueel netwerk** : als u wilt wijzigen van het virtuele netwerk, selecteert u **wijzigen Vnet**. 
    - **Subnet** : als u wilt wijzigen van het subnet, selecteert u **Subnet wijzigen**. 
    - **IP-adresconfiguratie** -opgeven of u de openbare, persoonlijke of gedeelde IP-adressen wilt. Zie voor meer informatie over gedeelde IP-adressen, [begrijpen gedeelde IP-adressen in Azure DevTest Labs](./devtest-lab-shared-ip.md).
    - **Datum en tijd** : Geef de vervaldatum en tijd voor de virtuele machine, zodat de virtuele machine wordt automatisch verwijderd. 
    - **Zorg dat deze machine claimbare** -maken van een virtuele machine 'claimbare' betekent dat het zal niet worden toegewezen eigendom op het moment van maken. In plaats daarvan is labgebruikers mogelijk eigenaar ("claim") de machine op de pagina van het lab.     
    - **Aantal claimbare exemplaren** -opgeven hoeveel claimbare instanties dat u wilt maken. 
8. Selecteer **indienen** om de formule te maken.

9. Wanneer de formule is gemaakt, wordt weergegeven in de lijst op de **formules** pagina.

### <a name="create-a-formula-from-a-vm"></a>Een formule van een virtuele machine maken
De volgende stappen begeleiden u bij het proces voor het maken van een formule op basis van een bestaande virtuele machine. 

> [!NOTE]
> Voor het maken van een formule van een virtuele machine, moet de virtuele machine zijn gemaakt na 30 maart 2016. 
> 
> 

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer de gewenste lab in de lijst met labs.  
4. Op van het lab **overzicht** pagina, selecteert u de virtuele machine van waaruit u wilt maken van de formule.
   
    ![Labs VM 's](./media/devtest-lab-create-formulas/my-vms.png)
5. Selecteer op de pagina van de virtuele machine, **formule (op basis van herbruikbare) maken**.
   
    ![Formule maken](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. Op de **formule maken** pagina een **naam** en **beschrijving** voor uw nieuwe formule.
   
    ![Formule-pagina maken](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Selecteer **OK** om de formule te maken.

## <a name="modify-a-formula"></a>Een formule wijzigen
Volg deze stappen voor het wijzigen van een formule:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer de gewenste lab in de lijst met labs.  
4. Selecteer op de pagina van het lab **formules (herbruikbare bases)**.
   
    ![Menu formule](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Op de **Lab formules** pagina, selecteert u de formule die u wilt wijzigen.
6. Op de **Aktualizovat vzorec** pagina, de gewenste wijzigingen aanbrengen en selecteer **Update**.

## <a name="delete-a-formula"></a>Verwijderen van een formule
Als u wilt verwijderen van een formule, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer de gewenste lab in de lijst met labs.  
4. In het lab **instellingen** weergeeft, schakelt **formules**.
   
    ![Menu formule](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Op de **Lab formules** pagina, selecteer het weglatingsteken rechts van de formule die u wilt verwijderen.
   
    ![Menu formule](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Selecteer in het contextmenu van de formule, **verwijderen**.
   
    ![Formule contextmenu](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Selecteer **Ja** in het bevestigingsvenster verwijderen.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Gerelateerde blogberichten
* [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Volgende stappen
Nadat u een formule voor gebruik gemaakt hebt bij het maken van een virtuele machine, de volgende stap is het [een VM toevoegen aan uw testomgeving](devtest-lab-add-vm.md).

