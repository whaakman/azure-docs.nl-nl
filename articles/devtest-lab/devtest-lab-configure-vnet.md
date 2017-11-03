---
title: Een virtueel netwerk configureren in Azure DevTest Labs | Microsoft Docs
description: Informatie over het configureren van een bestaand virtueel netwerk en subnet en deze gebruiken in een virtuele machine met Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 19dbb1625f46f8864413dc538a96b2413bc6eea0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Een virtueel netwerk configureren in Azure DevTest Labs
Zoals wordt beschreven in het artikel [een VM met artefacten toevoegen aan een lab](devtest-lab-add-vm-with-artifacts.md), wanneer u een virtuele machine in een testomgeving maakt u een geconfigureerde virtueel netwerk kunt opgeven. Een scenario om dit te doen is als u moet toegang tot uw corpnet-bronnen van uw virtuele machines met behulp van het virtuele netwerk dat is geconfigureerd met ExpressRoute of site-naar-site VPN. De volgende secties ziet u hoe u uw bestaande virtuele netwerk in de instellingen van het virtuele netwerk een lab toevoegen zodat deze beschikbaar is om te kiezen bij het maken van virtuele machines.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Een virtueel netwerk voor een testomgeving met behulp van de Azure-portal configureren
De volgende stappen doorlopen en toe te voegen een bestaand virtueel netwerk (subnet) tot een lab zodat deze kan worden gebruikt bij het maken van een virtuele machine in de testomgeving dezelfde. 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
3. Selecteer de gewenste testomgeving uit de lijst van labs. 
4. Selecteer op de labblade **configuratie en het beleid**.
5. Op de testomgeving **configuratie en het beleid** blade Selecteer **virtuele netwerken**.
6. Op de **virtuele netwerken** blade ziet u een lijst met virtuele netwerken die zijn geconfigureerd voor het huidige lab, evenals de standaard virtuele netwerk dat is gemaakt voor uw testomgeving. 
7. Selecteer **+ toevoegen**.
   
    ![Een bestaand virtueel netwerk toevoegen aan uw testomgeving](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. Op de **virtueel netwerk** blade Selecteer **[Selecteer virtueel netwerk]**.
   
    ![Selecteer een bestaand virtueel netwerk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. Op de **virtueel netwerk kiezen** blade, selecteer de gewenste virtuele netwerk. De blade ziet u de virtuele netwerken die in dezelfde regio in het abonnement als de testomgeving.  
10. Na het selecteren van een virtueel netwerk, keert u terug naar de **virtueel netwerk** klikt u op het subnet in de lijst onderaan de blade.

    ![Subnetlijst met](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    De blade Lab Subnet wordt weergegeven.

    ![Subnet labblade](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. Geef een **labsubnetnaam**.
12. Om een moet worden gebruikt in een lab maken van VM-subnet selecteert **gebruik bij het maken van de virtuele machine**.
13. Om in te schakelen een [openbaar IP-adres gedeeld](devtest-lab-shared-ip.md), selecteer **Schakel gedeelde openbare IP-adres**.
14. Om openbare IP-adressen in een subnet selecteert **openbare IP-maken toestaan**.
15. In de **maximum aantal virtuele machines per gebruiker** veld, geef het maximum aantal virtuele machines per gebruiker voor elk subnet. Als u wilt dat een onbeperkt aantal virtuele machines, laat u dit veld leeg.
16. Selecteer **OK** sluit de blade Lab Subnet.
17. Selecteer **opslaan** te sluiten van het virtuele netwerk-blade.
18. Nu dat het virtuele netwerk is geconfigureerd, kan worden geselecteerd bij het maken van een virtuele machine. 
    Voor informatie over het maken van een virtuele machine en geef een virtueel netwerk, Raadpleeg het artikel [een VM met artefacten toevoegen aan een lab](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u de gewenste virtuele netwerk hebt toegevoegd aan uw testomgeving, de volgende stap is het [een virtuele machine toevoegen aan uw testomgeving](devtest-lab-add-vm-with-artifacts.md).

