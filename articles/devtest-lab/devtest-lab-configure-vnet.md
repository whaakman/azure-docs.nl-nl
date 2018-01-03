---
title: Een virtueel netwerk configureren in Azure DevTest Labs | Microsoft Docs
description: Informatie over het configureren van een bestaand virtueel netwerk en subnet en deze gebruiken in een virtuele machine met Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: v-craic
ms.openlocfilehash: 21daa8ff756ee30c6d454d49af7db20afe488c47
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Een virtueel netwerk configureren in Azure DevTest Labs
Zoals wordt beschreven in het artikel [een virtuele machine toevoegen aan een lab](devtest-lab-add-vm.md), wanneer u een virtuele machine in een testomgeving maken kunt u een geconfigureerde virtueel netwerk. U wilt bijvoorbeeld toegang tot uw corpnet-bronnen van uw virtuele machines met behulp van het virtuele netwerk dat is geconfigureerd met ExpressRoute of site-naar-site VPN.

In dit artikel wordt uitgelegd hoe u uw bestaande virtuele netwerk in de instellingen van het virtuele netwerk een lab toevoegen zodat deze beschikbaar is om te kiezen bij het maken van virtuele machines.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Een virtueel netwerk voor een testomgeving met behulp van de Azure-portal configureren
De volgende stappen doorlopen en toe te voegen een bestaand virtueel netwerk (subnet) tot een lab zodat deze kan worden gebruikt bij het maken van een virtuele machine in de testomgeving dezelfde. 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer de gewenste testomgeving uit de lijst van labs. 
1. Selecteer in het lab hoofdvenster **configuratie en het beleid**.

    ![Toegang tot de configuratie en het beleid in het lab](./media/devtest-lab-configure-vnet/policies-menu.png)
1. In de **externe bronnen** sectie **virtuele netwerken**. Een lijst met virtuele netwerken die zijn geconfigureerd voor het huidige lab wordt weergegeven en de standaard virtuele netwerk gemaakt voor uw testomgeving. 
1. Selecteer **+ toevoegen**.
   
    ![Een bestaand virtueel netwerk toevoegen aan uw testomgeving](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
1. Op de **virtueel netwerk** deelvenster **[Selecteer virtueel netwerk]**.
   
    ![Selecteer een bestaand virtueel netwerk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
1. Op de **virtueel netwerk kiezen** deelvenster, selecteer de gewenste virtuele netwerk. Een lijst wordt weergegeven met alle van de virtuele netwerken die in dezelfde regio in het abonnement als de testomgeving.
1. Na het selecteren van een virtueel netwerk, keert u terug naar de **virtueel netwerk** deelvenster. Selecteer het subnet in de lijst onderaan.

    ![Subnetlijst met](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Het deelvenster Lab Subnet wordt weergegeven.

    ![Lab subnet deelvenster](./media/devtest-lab-configure-vnet/lab-subnet.png)
     
   - Geef een **labsubnetnaam**.
   - Om een moet worden gebruikt in een lab maken van VM-subnet selecteert **gebruik bij het maken van de virtuele machine**.
   - Om in te schakelen een [openbaar IP-adres gedeeld](devtest-lab-shared-ip.md), selecteer **Schakel gedeelde openbare IP-adres**.
   - Om openbare IP-adressen in een subnet selecteert **openbare IP-maken toestaan**.
   - In de **maximum aantal virtuele machines per gebruiker** veld, geef het maximum aantal virtuele machines per gebruiker voor elk subnet. Als u wilt dat een onbeperkt aantal virtuele machines, laat u dit veld leeg.
1. Selecteer **OK** te sluiten van het deelvenster Lab Subnet.
1. Selecteer **opslaan** te sluiten van het virtuele netwerk deelvenster.

Nu dat het virtuele netwerk is geconfigureerd, kan worden geselecteerd bij het maken van een virtuele machine. Voor informatie over het maken van een virtuele machine en geef een virtueel netwerk, Raadpleeg het artikel [een virtuele machine toevoegen aan een lab](devtest-lab-add-vm.md). 

Azure [documentatie van uw virtuele netwerk](https://docs.microsoft.com/azure/virtual-network) vindt u meer informatie over het gebruik van vnet's, inclusief het instellen en beheren van een VNet en verbinden met uw on-premises netwerk.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u de gewenste virtuele netwerk hebt toegevoegd aan uw testomgeving, de volgende stap is het [een virtuele machine toevoegen aan uw testomgeving](devtest-lab-add-vm.md).

