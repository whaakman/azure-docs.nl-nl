---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 815a217526117325ff80759701141b2b4d148514
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Het maken van een klassiek VNet en in de Azure portal
Volg deze stappen voor het maken van een klassiek die vnet op basis van het voorgaande scenario.

1. Navigeer in een browser naar http://portal.azure.com en meld u, indien nodig, aan met uw Azure-account.
2. Klik op **Een resource maken** > **Netwerken** > **Virtueel netwerk**. U ziet dat de **een implementatiemodel selecteren** lijst al staat **klassieke**. 3. Klik op **maken** zoals weergegeven in de volgende afbeelding.
   
    ![VNet aanmaken in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
4. Op de **virtueel netwerk** deelvenster, typ de **naam** van het VNet en klik op **adresruimte**. Uw adresruimte instellingen configureren voor het VNet en het eerste subnet en klik vervolgens op **OK**. De volgende afbeelding toont de CIDR-blok-instellingen voor ons scenario.
   
    ![Adresruimte deelvenster](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
5. Klik op **resourcegroep** en selecteer een resourcegroep om toe te voegen van het VNet of klik op **nieuwe resourcegroep maken** het VNet toevoegen aan een nieuwe resourcegroep. De volgende afbeelding ziet u de resource groepsinstellingen voor een nieuwe resourcegroep aangeroepen **TestRG**. Zie [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
   
    ![Deelvenster resource maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
6. Wijzig zo nodig de instellingen van het **Abonnement** en de **Locatie** voor uw VNet. 
7. Als u VNet niet als tegel wilt laten weergeven in het **Startboard** schakelt u **Vastmaken aan Startboard** uit. 
8. Klik op **maken** en Let op de tegel met de naam **virtueel netwerk aanmaken** zoals weergegeven in de volgende afbeelding.
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
9. Wacht tot het VNet worden gemaakt en wanneer u de tegel ziet, klikt u op om toe te voegen meer subnetten.
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
10. U ziet de **configuratie** voor uw VNet, zoals wordt weergegeven. 
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
11. Klik op **subnetten** > **toevoegen**, typ een **naam** en geef een **-adresbereik (CIDR-blok)** voor uw subnet, en vervolgens Klik op **OK**. De volgende afbeelding ziet de instellingen voor onze huidige scenario.
    
    ![VNet aanmaken in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

