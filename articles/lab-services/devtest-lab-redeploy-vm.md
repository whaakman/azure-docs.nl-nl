---
title: Opnieuw implementeren van een virtuele machine in een lab in Azure DevTest Labs | Microsoft Docs
description: Informatie over het implementeren van een virtuele machine (overstappen van het ene Azure-knooppunt naar een andere) in Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: 273b0f1105d8b71b90a06e2627e201b97f12a754
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095392"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Een virtuele machine in een lab in Azure DevTest Labs opnieuw implementeren
Als u geen verbinding met een virtuele machine (VM) in een testomgeving via een verbinding met extern bureaublad maken, de virtuele machine opnieuw implementeren en probeer het opnieuw conencting toe. Wanneer u een virtuele machine opnieuw implementeren, wordt de virtuele machine in DevTest Labs verplaatst vanaf het knooppunt waarop deze wordt uitgevoerd naar een nieuw knooppunt in de Azure-infrastructuur. Vervolgens wordt de virtuele machine gestart en behoud van alle configuratie-opties en bijbehorende resources. Deze functie bespaart u de tijd besteed aan bij het oplossen van uw verbinding met extern bureaublad of de toepassing toegang tot Windows-VM's in het lab. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Stappen voor het implementeren van een virtuele machine in een testomgeving 
Als u wilt implementeren op een virtuele machine in een lab in Azure DevTest Labs, moet u de volgende stappen uitvoeren: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer in de lijst met labs, de testomgeving met de virtuele machine die u wilt implementeren.  
4. Selecteer in het linkerdeelvenster **mijn virtuele Machines**. 
5. Selecteer in de lijst met virtuele machines, een virtuele machine.
6. Selecteer in de virtuele Machine-pagina voor uw virtuele machine, **opnieuw implementeren** onder **OPERATIONS** in het menu links.

    ![Opnieuw implementeren](media/devtest-lab-redeploy-vm/redeploy.png)
7. Lees de informatie op de pagina en selecteer **opnieuw implementeren** knop. 9. Controleer de status van de bewerking opnieuw implementeren in de **meldingen** venster.

    ![Status opnieuw implementeren](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het formaat van een virtuele machine in Azure DevTest Labs, Zie [vergroten of verkleinen van een virtuele machine](devtest-lab-resize-vm.md).


