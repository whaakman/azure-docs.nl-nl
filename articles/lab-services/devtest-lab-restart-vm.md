---
title: Opnieuw opstarten van een virtuele machine in een testomgeving in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het opnieuw opstarten van een virtuele machine in Azure DevTest Labs
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
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 24a17ce09bee1097b0418ad4e20990d359b3e084
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Opnieuw opstarten van een virtuele machine in een testomgeving in Azure DevTest Labs
U kunt snel en eenvoudig een virtuele machine opnieuw in DevTest Labs volgens de stappen in dit artikel. Overweeg het volgende voordat u een virtuele machine opnieuw opstart:

- De virtuele machine moet worden uitgevoerd voor de functie opnieuw worden ingeschakeld.
- Als een gebruiker is verbonden met een actieve virtuele machine wanneer ze opnieuw uitvoert, moeten ze opnieuw naar de virtuele machine nadat deze back-up is gestart.
- Als een artefact wordt toegepast wanneer u de virtuele machine opnieuw opstart, ontvangt u een waarschuwing dat het artefact mogelijk niet toegepast. 

    ![Waarschuwing opnieuw te starten tijdens het toepassen van artefacten](./media/devtest-lab-restart-vm/devtest-lab-restart-vm-apply-artifacts.png)


   > [!NOTE]
   > Als de virtuele machine tijdens het toepassen van een artefact tot stilstand is gekomen, kunt u de functie van de virtuele machine opnieuw opstarten als een mogelijke manier om het probleem te verhelpen.
   >
   >

## <a name="steps-to-restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Stappen voor het starten van een virtuele machine in een testomgeving in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer in de lijst van labs in het lab waarin de virtuele machine die u wilt starten.  
1. Selecteer in het linkerdeelvenster **mijn virtuele Machines**. 
1. Selecteer een actieve virtuele machine uit de lijst van virtuele machines.
1. Selecteer aan de bovenkant van het VM-management-deelvenster **opnieuw**.  

    ![Opnieuw opstarten van VM-knop](./media/devtest-lab-restart-vm/devtest-lab-restart-vm.png)

1. De status van de herstart controleren door het selecteren van de **meldingen** bovenin rechts van het venster.

    ![De statuscontrole van de virtuele machine opnieuw opstarten](./media/devtest-lab-restart-vm/devtest-lab-restart-notification.png)

U kunt een actieve virtuele machine ook opnieuw starten door het selecteren van het weglatingsteken (...) in de lijst met **mijn virtuele Machines**.

![VM starten via beletselteken](./media/devtest-lab-restart-vm/devtest-lab-restart-elipses.png)

## <a name="next-steps"></a>Volgende stappen
* Wanneer hij gestart is, u kunt de verbinding met de virtuele machine door te selecteren **Connect** op de het deelvenster Beheer.
* Verken de [DevTest Labs Azure Resource Manager-sjabloon snelstartgalerie](https://github.com/Azure/azure-devtestlab/tree/master/Samples)
