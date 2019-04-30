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
ms.openlocfilehash: aa88bf3bd6c5037b41c09e9ffe47921f1b9dc9be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60937426"
---
## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Over het maken van een klassiek VNet in Azure portal
Volg deze stappen voor het maken van een klassiek die vnet op basis van het voorgaande scenario.

1. Navigeer in een browser naar http://portal.azure.com en meld u, indien nodig, aan met uw Azure-account.
2. Klik op **Een resource maken** > **Netwerken** > **Virtueel netwerk**. U ziet dat de **een implementatiemodel selecteren** lijst met al wordt weergegeven **klassieke**. 3. Klik op **maken** zoals wordt weergegeven in de volgende afbeelding.
   
    ![VNet aanmaken in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
4. Op de **virtueel netwerk** deelvenster, type de **naam** van het VNet, en klik vervolgens op **adresruimte**. Uw adres schijfruimte-instellingen voor het VNet en het eerste subnet configureren en klik vervolgens op **OK**. De volgende afbeelding toont de CIDR-blok-instellingen voor ons scenario.
   
    ![Adresruimte deelvenster](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
5. Klik op **resourcegroep** en selecteer een resourcegroep voor het toevoegen van het VNet of klikt u op **nieuwe resourcegroep maken** het VNet toevoegen aan een nieuwe resourcegroep. De volgende afbeelding ziet u de resource groepsinstellingen voor een nieuwe resourcegroep met de naam **TestRG**. Zie [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
   
    ![Deelvenster voor resource-groep maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
6. Wijzig zo nodig de instellingen van het **Abonnement** en de **Locatie** voor uw VNet. 
7. Als u VNet niet als tegel wilt laten weergeven in het **Startboard** schakelt u **Vastmaken aan Startboard** uit. 
8. Klik op **maken** en Let op de tegel met de naam **virtueel netwerk aanmaken** zoals wordt weergegeven in de volgende afbeelding.
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
9. Wacht tot het VNet moet worden gemaakt en wanneer u de tegel ziet, klikt u erop om toe te voegen meer subnetten.
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
10. U ziet de **configuratie** voor uw VNet, zoals wordt weergegeven. 
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
11. Klik op **subnetten** > **toevoegen**, typt u een **naam** en geef een **adresbereik (CIDR-blok)** voor uw subnet, en vervolgens Klik op **OK**. De volgende afbeelding ziet u de instellingen voor onze huidige scenario.
    
    ![VNet aanmaken in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

