---
title: Beheer formules in Azure DevTest Labs virtuele machines maken | Microsoft Docs
description: Meer informatie over het bijwerken en verwijderen van Azure DevTest Labs formules
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: v-craic
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3dcd285761774c3cd1050976894f1f15db61b52c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="manage-azure-devtest-labs-formulas"></a>Azure DevTest Labs formules beheren

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

In dit artikel laat zien hoe een formule te maken van een base (aangepaste installatiekopie, Marketplace-installatiekopie of een andere formule) of een bestaande virtuele machine. In dit artikel helpt u ook bij het beheren van bestaande formules.

## <a name="create-a-formula"></a>Maak een formule
Iedereen met DevTest Labs *gebruikers* machtigingen kunnen maken van virtuele machines met een formule als basis is. Er zijn twee manieren om formules te maken: 

* Gebruik van een basis - als u wilt de eigenschappen van de formule definiëren.
* Van een bestaande lab VM - Gebruik deze optie wanneer u wilt maken van een formule op basis van de instellingen van een bestaande virtuele machine.

Zie voor meer informatie over het toevoegen van gebruikers en machtigingen [eigenaars en gebruikers toevoegen in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Maak een formule van een basis
De volgende stappen begeleiden u bij het proces van het maken van een formule van een aangepaste installatiekopie, Marketplace-installatiekopie of een andere formule.

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.

3. Selecteer de gewenste testomgeving uit de lijst van labs.  

4. Selecteer op de labblade **formules (herbruikbare bases)**.
   
    ![Menu formule](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. Op de **formules** blade Selecteer **+ toevoegen**.
   
    ![Een formule toevoegen](./media/devtest-lab-create-formulas/add-formula.png)

6. Op de **kiezen een base** blade, selecteer de base (aangepaste installatiekopie, Marketplace-installatiekopie of formule) van waaruit u wilt maken van de formule.
   
    ![Base lijst](./media/devtest-lab-create-formulas/base-list.png)

7. Op de **formule maken** blade, geef de volgende waarden:
   
    * **Formulenaam** -Voer een naam voor de formule. Deze waarde wordt weergegeven in de lijst met basisinstallatiekopieën wanneer u een virtuele machine maken. De naam wordt al gevalideerd als u deze typen, en als dat niet geldig, een bericht geeft aan de vereisten voor een geldige naam.
    * **Beschrijving** -Voer een duidelijke beschrijving voor de formule. Deze waarde is beschikbaar in het contextmenu van de formule bij het maken van een virtuele machine.
    * **Gebruikersnaam** -Voer een gebruikersnaam die wordt verleend administrator-bevoegdheden.
    * **Wachtwoord** : Geef - op of Selecteer in de vervolgkeuzelijst - een waarde die is gekoppeld aan het geheim (wachtwoord) dat u wilt gebruiken voor de opgegeven gebruiker. Zie voor meer informatie over de geheimen [Azure DevTest Labs: persoonlijke archief van de geheime](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).
    * **Virtuele machine schijftype** – Geef de harde schijf (harde schijf) of SSD (solid-state drive) om aan te geven welk type opslagschijf is toegestaan voor de virtuele machines ingericht met behulp van deze basisinstallatiekopie.
    * ** Virtuele machine grootte ** - Selecteer een van de vooraf gedefinieerde items die de processor-cores, RAM-geheugen en de grootte van de vaste schijf van de virtuele machine maken opgeven. 
    * **Artefacten** - wilt openen de **artefacten toevoegen** blade, waarin u gegevens kunt selecteren en configureren van de artefacten die u wilt toevoegen aan de basisinstallatiekopie. Zie voor meer informatie over artefacten [aangepaste artefacten gemaakt voor uw virtuele machine van Azure DevTest Labs](devtest-lab-artifact-author.md).
    * **Geavanceerde instellingen** - wilt openen de **Geavanceerd** blade waar u de volgende instellingen configureren:
        * **Virtueel netwerk** -Geef de gewenste virtuele netwerk.
        * **Subnet** -Geef de gewenste subnet.    
        * **IP-adresconfiguratie** -opgeven of u wilt dat de Public, Private of gedeelde IP-adressen. Zie voor meer informatie over gedeelde IP-adressen [Understand gedeelde IP-adressen in Azure DevTest Labs](./devtest-lab-shared-ip.md).
        * **Zorg dat deze machine claimable** -maken van een machine 'claimable' betekent dat deze wordt niet worden toegewezen eigendom op het moment van maken. In plaats daarvan zich lab gebruikers eigenaar ("claim") de machine zich in het lab-blade.     
    * **Afbeelding** -dit veld bevat de naam van de basisinstallatiekopie die u hebt geselecteerd op de vorige blade. 
     
       ![Formule maken](./media/devtest-lab-create-formulas/create-formula.png)

8. Selecteer **maken** om de formule te maken.

9. Wanneer de formule is gemaakt, wordt weergegeven in de lijst op de **formules** blade.

### <a name="create-a-formula-from-a-vm"></a>Maak een formule van een virtuele machine
De volgende stappen begeleiden u bij het proces van het maken van een formule op basis van een bestaande VM. 

> [!NOTE]
> Als u wilt een formule van een virtuele machine maakt, moet de virtuele machine zijn gemaakt na 30 maart 2016. 
> 
> 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
3. Selecteer de gewenste testomgeving uit de lijst van labs.  
4. Op de testomgeving **overzicht** blade, selecteert u de virtuele machine van waaruit u wilt maken van de formule.
   
    ![Virtuele machines Labs](./media/devtest-lab-create-formulas/my-vms.png)
5. Selecteer op de VM-blade **formule (op basis van herbruikbare) maken**.
   
    ![Formule maken](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. Op de **formule maken** blade, voer een **naam** en **beschrijving** voor uw nieuwe formule.
   
    ![Formule blade maken](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Selecteer **OK** om de formule te maken.

## <a name="modify-a-formula"></a>Een formule wijzigen
Volg deze stappen voor het wijzigen van een formule:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
3. Selecteer de gewenste testomgeving uit de lijst van labs.  
4. Selecteer op de labblade **formules (herbruikbare bases)**.
   
    ![Menu formule](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Op de **Lab formules** blade, selecteer de formule die u wilt wijzigen.
6. Op de **formule bijwerken** blade Breng de gewenste wijzigingen en selecteer **Update**.

## <a name="delete-a-formula"></a>Verwijderen van een formule
Volg deze stappen voor het verwijderen van een formule:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
3. Selecteer de gewenste testomgeving uit de lijst van labs.  
4. Op de testomgeving **instellingen** blade Selecteer **formules**.
   
    ![Menu formule](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Op de **Lab formules** blade, selecteer het weglatingsteken rechts van de formule die u wilt verwijderen.
   
    ![Menu formule](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Selecteer in het contextmenu van de formule, **verwijderen**.
   
    ![Formule contextmenu](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Selecteer **Ja** naar het bevestigingsvenster verwijderen.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten
* [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Volgende stappen
Nadat u een formule voor gebruik gemaakt hebt bij het maken van een virtuele machine, de volgende stap is het [een virtuele machine toevoegen aan uw testomgeving](devtest-lab-add-vm.md).

