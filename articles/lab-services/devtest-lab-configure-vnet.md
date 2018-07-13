---
title: Een virtueel netwerk configureren in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het configureren van een bestaand virtueel netwerk en een subnet, en deze gebruiken in een virtuele machine met Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 0141ea8a88c0322e6f56cbea56d3a43c923769af
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38687785"
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Een virtueel netwerk configureren in Azure DevTest Labs
Zoals uitgelegd in het artikel [een virtuele machine toevoegen aan een lab](devtest-lab-add-vm.md), wanneer u een virtuele machine in een testomgeving maken, kunt u een geconfigureerde virtueel netwerk. Bijvoorbeeld, moet u mogelijk toegang tot uw bedrijfsnetwerk-resources van uw virtuele machines met behulp van het virtuele netwerk dat is geconfigureerd met ExpressRoute of site-naar-site VPN.

In dit artikel wordt uitgelegd hoe u uw bestaande virtuele netwerk in Virtual Network-instellingen van een lab toevoegen, zodat deze beschikbaar zijn om te kiezen bij het maken van virtuele machines.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Een virtueel netwerk voor een testomgeving met behulp van de Azure-portal configureren
De volgende stappen helpen u bij het toevoegen van een bestaand virtueel netwerk (en het subnet) aan een lab zodat deze kan worden gebruikt bij het maken van een virtuele machine in het hetzelfde lab. 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer de gewenste lab in de lijst met labs. 
1. Selecteer in het hoofdvenster van de testomgeving, **configuratie en het beleid**.

    ![Toegang tot de configuratie en het beleid van het lab](./media/devtest-lab-configure-vnet/policies-menu.png)
1. In de **externe bronnen** sectie, selecteer **virtuele netwerken**. Een lijst met virtuele netwerken die zijn geconfigureerd voor de huidige testomgeving wordt weergegeven en de standaard virtueel netwerk gemaakt voor uw testomgeving. 
1. Selecteer **+ Toevoegen**.
   
    ![Een bestaand virtueel netwerk toevoegen aan uw lab](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
1. Op de **virtueel netwerk** venster **[Selecteer het virtuele netwerk]**.
   
    ![Selecteer een bestaand virtueel netwerk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
1. Op de **virtueel netwerk kiezen** deelvenster, selecteert u de gewenste virtuele netwerk. Een lijst wordt weergegeven met alle van de virtuele netwerken die zich in dezelfde regio in het abonnement als de testomgeving.
1. Na het selecteren van een virtueel netwerk en u keert terug naar de **virtueel netwerk** deelvenster. Selecteer het subnet in de lijst aan de onderkant.

    ![Lijst met subnetten](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Het deelvenster Lab-Subnet wordt weergegeven.

    ![Lab subnet deelvenster](./media/devtest-lab-configure-vnet/lab-subnet.png)
     
   - Geef een **Lab subnetnaam**.
   - Als u wilt toestaan dat een subnet moet worden gebruikt in een lab maken van VM, selecteer **gebruiken bij het maken van virtuele machine**.
   - Om in te schakelen een [gedeeld openbaar IP-adres](devtest-lab-shared-ip.md), selecteer **inschakelen gedeeld openbaar IP-adres**.
   - Voor het openbare IP-adressen toestaan in een subnet, selecteert u **maken van openbare IP-toestaan**.
   - In de **maximum aantal virtuele machines per gebruiker** veld, het maximum aantal VM's per gebruiker voor elk subnet opgeven. Als u een onbeperkt aantal virtuele machines wilt, laat u dit veld leeg.
1. Selecteer **OK** te sluiten van het deelvenster Lab-Subnet.
1. Selecteer **opslaan** te sluiten van het virtuele netwerk deelvenster.

Nu dat het virtuele netwerk is geconfigureerd, kan worden geselecteerd bij het maken van een virtuele machine. Voor informatie over het maken van een virtuele machine en geef een virtueel netwerk, Raadpleeg het artikel [een virtuele machine toevoegen aan een lab](devtest-lab-add-vm.md). 

Azure [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network) vindt u meer informatie over het gebruik van vnet's, waaronder het instellen en beheren van een VNet en verbinden met uw on-premises netwerk.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u de gewenste virtuele netwerk hebt toegevoegd aan uw lab, de volgende stap is het [een VM toevoegen aan uw testomgeving](devtest-lab-add-vm.md).

