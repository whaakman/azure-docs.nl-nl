---
title: Verwijderen van een testomgeving of een virtuele machine in een lab in Azure DevTest Labs | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een lab of de virtuele machine verwijderen in een testomgeving.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: spelluru
ms.openlocfilehash: 9634c70566aba21bdd28ee016c9fa94464ec9c1b
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956327"
---
# <a name="delete-a-lab-or-vm-in-a-lab-in-azure-devtest-labs"></a>Verwijderen van een testomgeving of een virtuele machine in een lab in Azure DevTest Labs
In dit artikel wordt beschreven hoe u een lab of de virtuele machine verwijderen in een testomgeving.

## <a name="delete-a-lab"></a>Verwijderen van een lab
Wanneer u een DevTest Labs-exemplaar van een resourcegroep verwijdert, worden de volgende acties uitgevoerd in de DevTest Labs-service: 

- Alle resources die automatisch zijn gemaakt op het moment dat lab gemaakt worden automatisch verwijderd. De resourcegroep zelf wordt niet verwijderd. Als u geen resources handmatig deze resourcegroep gemaakt had, verwijderen de service niet. 
- Alle virtuele machines in de testomgeving en resource-groepen die zijn gekoppeld aan deze VM's worden automatisch verwijderd. Wanneer u een virtuele machine in een testomgeving maakt, maakt de service resources (schijf, de netwerkinterface, openbare IP-adres, enzovoort) voor de virtuele machine in een afzonderlijke resourcegroep. Echter, als u aanvullende resources voor het handmatig in deze resourcegroepen maken, de DevTest Labs-service niet verwijderd die resources en de resourcegroep. 

Als u wilt verwijderen van een lab, moet u de volgende acties uitvoeren: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle resource** menu aan de linkerkant en selecteer **DevTest Labs** voor het type van de service en selecteer in het lab.

    ![Selecteer uw lab](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Op de **DevTest Lab** pagina, klikt u op **verwijderen** op de werkbalk. 

    ![Knop verwijderen](media/devtest-lab-delete-lab-vm/delete-button.png)
4. Op de **bevestiging** pagina de **naam** van uw lab, en selecteer **verwijderen**. 

    ![Bevestigen](media/devtest-lab-delete-lab-vm/confirm-delete.png)
5. De status van de bewerking Selecteer **meldingen** pictogram (de klok). 

    ![Meldingen](media/devtest-lab-delete-lab-vm/delete-status.png)

 
## <a name="delete-a-vm-in-a-lab"></a>Verwijderen van een virtuele machine in een testomgeving
Als ik een virtuele machine in een testomgeving verwijdert, worden sommige van de (niet alle) resources die zijn gemaakt op het moment van lab maken verwijderd. De volgende resources zijn niet verwijderd: 

-   Voor Key vault in de belangrijkste resourcegroep
-   Beschikbaarheid is ingesteld, netwerktaakverdeler, openbare IP-adres in de resourcegroep van de virtuele machine. Deze resources worden gedeeld door meerdere virtuele machines in een resourcegroep. 

Virtuele machine, de netwerkinterface en de schijf die is gekoppeld aan de virtuele machine worden verwijderd. 

Als u wilt verwijderen van een virtuele machine in een testomgeving, moet u de volgende acties uitvoeren: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle resource** menu aan de linkerkant en selecteer **DevTest Labs** voor het type van de service en selecteer in het lab.

    ![Selecteer uw lab](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Selecteer **... (ellips)**  voor de virtuele machine in de lijst met virtuele machines, en selecteer **verwijderen**. 

    ![Virtuele machine in het menu verwijderen](media/devtest-lab-delete-lab-vm/delete-vm-menu-in-list.png)
4. Op de **bevestiging** in het dialoogvenster, selecteer **Ok**. 
5. De status van de bewerking Selecteer **meldingen** pictogram (de klok). 

Verwijderen van een virtuele machine via de **pagina van de virtuele Machine**, selecteer **verwijderen** via de werkbalk zoals weergegeven in de volgende afbeelding:

![Virtuele machine van VM-pagina verwijderen](media/devtest-lab-delete-lab-vm/delete-from-vm-page.png) 


## <a name="next-steps"></a>Volgende stappen
Als u een lab maken wilt, raadpleegt u de volgende artikelen: 

- [Een lab maken](devtest-lab-create-lab.md)
- [Een virtuele machine toevoegen aan het lab](devtest-lab-add-vm.md)