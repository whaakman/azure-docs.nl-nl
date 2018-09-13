---
title: Formaat van een virtuele machine in een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het formaat van een virtuele machine in Azure DevTest Labs
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
ms.openlocfilehash: 9b9a1839bf4b028aec13b764b4de66385de4189e
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646365"
---
# <a name="resize-a-vm-in-a-lab-in-azure-devtest-labs"></a>Formaat van een virtuele machine in een lab in Azure DevTest Labs
Een van de belangrijke functies van virtuele machines van Azure is dat Hiermee kunt u de grootte van een virtuele machine (VM) op basis van uw behoeften voor CPU, netwerk en prestaties van de schijf wijzigen. Azure DevTest Labs biedt ondersteuning voor deze functie voor virtuele machines in een testomgeving nu. De functie formaat voldoet aan het lab-beleid voor toegestane VM-grootten in de testomgeving. Dat wil zeggen, kunt u de grootte van een virtuele machine alleen toegestane grootten in de testomgeving. 


## <a name="steps-to-resize-a-vm-in-a-lab"></a>Stappen voor het formaat van een virtuele machine in een testomgeving 
Als u wilt het formaat van een virtuele machine in een lab in Azure DevTest Labs, moet u de volgende stappen uitvoeren: 

> [!NOTE]
> Als u verbonden bent met de virtuele machine via Extern bureaublad-sessiehost (RDP), sla uw werk en verbreken van de virtuele machine voordat u het formaat.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer in de lijst met labs, de testomgeving met de virtuele machine die u wilt vergroten of verkleinen.  
4. Selecteer in het linkerdeelvenster **mijn virtuele Machines**. 
5. Selecteer in de lijst met virtuele machines, een virtuele machine.
6. Selecteer **stoppen** op de werkbalk als de virtuele machine wordt uitgevoerd. Controleer de status van de bewerking in de **meldingen** venster. Wacht totdat de virtuele machine gestopt en sluit is de **meldingen** venster. 

    ![De virtuele machine stoppen](media/devtest-lab-resize-vm/stop-vm.png)
1. Selecteer in de virtuele Machine-pagina voor uw virtuele machine, **grootte** onder **instellingen** in het menu links.

    ![Grootte van menu](media/devtest-lab-resize-vm/size-menu.png)
1. In de **Kies een grootte** bladeren en selecteer een grootte voor de virtuele machine en klikt u op **Selecteer**.     
1. Controleer de status van de bewerking formaat wijzigen in de **meldingen** venster.

    ![Grootte van status](media/devtest-lab-resize-vm/resize-status.png)
10. De grootte is gewijzigd na de bewerking is geslaagd, sluit de **meldingen** venster. 
11. Selecteer **overzicht** in het menu aan de linkerkant en selecteer **opnieuw** op de werkbalk om het opnieuw opstarten van de virtuele machine. 

## <a name="next-steps"></a>Volgende stappen
Zie voor gedetailleerde informatie over het formaat wijzigen-functie wordt ondersteund door virtuele machines van Azure, [vergroten of verkleinen van virtuele machines](https://azure.microsoft.com/blog/resize-virtual-machines/).


