---
title: Configureren van instellingen voor Azure Marketplace-installatiekopie in Azure DevTest Labs | Microsoft Docs
description: "Configureren welke Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van een virtuele machine in Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 5f888c9d92a9164cc7d3d1aed66c29a724b365d7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Configureren van instellingen voor Azure Marketplace-installatiekopie in Azure DevTest Labs
DevTest Labs ondersteunt maken VMs op basis van Azure Marketplace-installatiekopieën, afhankelijk van hoe u Azure Marketplace-installatiekopieën hebt geconfigureerd in uw testomgeving. Dit artikel laat zien hoe u kunt opgeven die, indien aanwezig, Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving. Dit zorgt ervoor dat uw team alleen toegang heeft tot de Marketplace-installatiekopieën die ze nodig hebben. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Selecteer welke Azure Marketplace-installatiekopieën zijn toegestaan bij het maken van een virtuele machine
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
3. Selecteer de gewenste testomgeving uit de lijst van labs. 
4. Selecteer op de labblade **configuratie en het beleid**.
5. Op het lab **configuratie en het beleid** blade onder **basissen voor virtuele Machine**, selecteer **Marketplace-installatiekopieën**.
6. Geef op of alle gekwalificeerde Azure Marketplace-installatiekopieën beschikbaar voor gebruik als een basis van een nieuwe virtuele machine. Als u selecteert **Ja**, en vervolgens alle Azure Marketplace-installatiekopieën die voldoen aan de volgende criteria zijn toegestaan in de testomgeving:
   
   * De installatiekopie wordt gemaakt van één VM **en**
   * De installatiekopie van het Azure Resource Manager gebruikt om in te richten virtuele machines, **en**
   * De installatiekopie van het vereist aanschaffen van extra licentieabonnement geen
     
    Als u wilt geen afbeeldingen worden toegestaan, of u opgeven wilt welke afbeeldingen kunnen worden gebruikt, selecteert u **Nee**.
     
     ![Optie voor het toestaan van alle Marketplace-installatiekopieën worden gebruikt als basis van installatiekopieën voor virtuele machines](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Als u selecteert **Nee** naar de vorige stap, de **toegestane installatiekopieën/Selecteer alle** selectievakje is ingeschakeld. 
   Gebruik deze optie samen met het zoekvak om snel te selecteren of selectie opheffen van alle items in de lijst weergegeven.
   * Selecteer de Azure Marketplace-installatiekopieën die u toestaan voor het maken van de virtuele machine afzonderlijk wilt door het bijbehorende selectievakje van elke installatiekopie.
   * Selecteer niets uit de lijst als u niet wilt dat alle Azure Marketplace-installatiekopieën worden gebruikt in de testomgeving.
   
    ![U kunt opgeven welke Azure Marketplace-installatiekopieën kunnen worden gebruikt als basis van installatiekopieën voor virtuele machines](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer u de manier waarop Azure Marketplace-installatiekopieën zijn toegestaan bij het maken van een virtuele machine hebt geconfigureerd, wordt de volgende stap is het [een virtuele machine toevoegen aan uw testomgeving](devtest-lab-add-vm-with-artifacts.md).

