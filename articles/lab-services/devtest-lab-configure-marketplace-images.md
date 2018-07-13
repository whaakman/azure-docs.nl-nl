---
title: Azure Marketplace-installatiekopie-instellingen configureren in Azure DevTest Labs | Microsoft Docs
description: Configureren welke Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van een virtuele machine in Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 90df853eeca89e61fa69e6967218c4c18274ac1b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38633033"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Azure Marketplace-installatiekopie-instellingen configureren in Azure DevTest Labs
DevTest Labs biedt ondersteuning voor het maken van het virtuele machines gebaseerd op Azure Marketplace-installatiekopieën, afhankelijk van hoe u Azure Marketplace-installatiekopieën hebt geconfigureerd om te worden gebruikt in uw testomgeving. Dit artikel leest u hoe u kunt opgeven die, indien van toepassing, de Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving. Dit zorgt ervoor dat uw team alleen toegang heeft tot de Marketplace-installatiekopieën die ze nodig hebben. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Selecteer welke Azure Marketplace-installatiekopieën zijn toegestaan bij het maken van een virtuele machine
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer de gewenste lab in de lijst met labs. 
4. Selecteer op de blade van de testomgeving **configuratie en het beleid**.
5. In het lab **configuratie en het beleid** blade onder **VM-basissen**, selecteer **Marketplace-installatiekopieën**.
6. Geef op of u wilt dat alle gekwalificeerde Azure Marketplace-installatiekopieën beschikbaar voor gebruik als basis van een nieuwe virtuele machine. Als u selecteert **Ja**, en vervolgens de Azure Marketplace-installatiekopieën die voldoen aan de volgende criteria zijn toegestaan in de testomgeving:
   
   * De installatiekopie wordt gemaakt van een enkele virtuele machine, **en**
   * De installatiekopie van het Azure Resource Manager gebruikt voor het inrichten van virtuele machines, **en**
   * De installatiekopie van het vereist aanschaffen van een extra licentie abonnement geen
     
    Als u wilt dat er geen afbeeldingen worden toegestaan, of als u opgeven wilt welke installatiekopieën kan worden gebruikt, selecteert u **Nee**.
     
     ![Optie voor het toestaan van alle Marketplace-installatiekopieën moet worden gebruikt als basisinstallatiekopieën voor virtuele machines](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Als u selecteert **Nee** naar de vorige stap, de **toegestane afbeeldingen/Selecteer alle** selectievakje is ingeschakeld. 
   Gebruik deze optie samen met het zoekvak kunt u snel geselecteerd of gedeselecteerd alle items in de lijst weergegeven.
   * Selecteer de Azure Marketplace-installatiekopieën die u toestaan voor het maken van virtuele machine afzonderlijk wilt door het controleren van het bijbehorende selectievakje van elke afbeelding.
   * Als u niet wilt dat alle Azure Marketplace-installatiekopieën in de testomgeving moet worden gebruikt, moet u niets selecteren in de lijst.
   
    ![U kunt opgeven welke Azure Marketplace-installatiekopieën kunnen worden gebruikt als basisinstallatiekopieën voor virtuele machines](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u hebt geconfigureerd hoe Azure Marketplace-installatiekopieën zijn toegestaan bij het maken van een virtuele machine, de volgende stap is het [een VM toevoegen aan uw testomgeving](devtest-lab-add-vm.md).

