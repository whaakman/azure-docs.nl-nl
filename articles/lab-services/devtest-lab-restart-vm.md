---
title: Opnieuw opstarten van een virtuele machine in een lab in Azure DevTest Labs | Microsoft Docs
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
ms.openlocfilehash: 0c11f5f43e0ea96a0b4bc9b4ecec30850c39b023
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231971"
---
# <a name="restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Opnieuw opstarten van een virtuele machine in een lab in Azure DevTest Labs
U kunt snel en eenvoudig een virtuele machine opnieuw in DevTest Labs door de stappen in dit artikel te volgen. Overweeg het volgende voordat u een virtuele machine opnieuw te starten:

- De virtuele machine moet worden uitgevoerd voor de functie opnieuw worden ingeschakeld.
- Als een gebruiker is verbonden met een actieve virtuele machine bij het uitvoeren van een opnieuw opstarten, ze moeten opnieuw verbinding maken met de virtuele machine nadat deze een back-up is gestart.
- Als een artefact wordt toegepast wanneer u de virtuele machine opnieuw opstarten, ontvangt u een waarschuwing dat het artefact niet kan worden toegepast. 

    ![Waarschuwing bij het opnieuw opstarten tijdens het toepassen van artefacten](./media/devtest-lab-restart-vm/devtest-lab-restart-vm-apply-artifacts.png)


   > [!NOTE]
   > Als de virtuele machine tijdens het toepassen van een artefact tot stilstand is gekomen, kunt u de functie van de virtuele machine opnieuw starten als een mogelijke manier om het probleem te verhelpen.
   >
   >

## <a name="steps-to-restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Stappen voor het opnieuw opstarten van een virtuele machine in een lab in Azure DevTest Labs
1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer in de lijst met labs, de testomgeving met de virtuele machine die u wilt starten.  
1. Selecteer in het linkerdeelvenster **mijn virtuele Machines**. 
1. Selecteer in de lijst met virtuele machines, een actieve virtuele machine.
1. Selecteer aan de bovenkant van het deelvenster van de virtuele machine management **opnieuw**.  

    ![Opnieuw opstarten van VM-knop](./media/devtest-lab-restart-vm/devtest-lab-restart-vm.png)

1. De status van het opnieuw opstarten controleren door het selecteren van de **meldingen** pictogram aan de bovenkant van het venster.

    ![De status van de virtuele machine opnieuw opstarten weer te geven](./media/devtest-lab-restart-vm/devtest-lab-restart-notification.png)

U kunt een actieve virtuele machine ook opnieuw starten door het bijbehorende beletselteken (...) selecteren in de lijst met **mijn virtuele Machines**.

![Opnieuw opstarten van virtuele machine via weglatingstekens](./media/devtest-lab-restart-vm/devtest-lab-restart-elipses.png)

## <a name="next-steps"></a>Volgende stappen
* Nadat het opnieuw is opgestart, u kunt verbinding maken met de virtuele machine door te selecteren **Connect** op de het deelvenster Beheer.
* Verken de [DevTest Labs Azure Resource Manager quickStart-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/Samples)
