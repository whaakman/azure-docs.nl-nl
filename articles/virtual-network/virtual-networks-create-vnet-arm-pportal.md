---
title: Een virtueel netwerk maken met Azure Portal | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met Azure Portal | Resource Manager.
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/8/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: ba7a67b8ae57da165f45bd3552a3dfac5f4ef64b
ms.openlocfilehash: c7257faad9a41174fb1d65e04c99cd96a8af3ea9


---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Een virtueel netwerk maken met Azure Portal

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure heeft twee implementatiemodellen: Azure Resource Manager en klassiek. Microsoft raadt aan resources te maken via het Resource Manager-implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md) voor meer informatie over de verschillen tussen de twee modellen.
 
In dit artikel wordt uitgelegd hoe u een VNet kunt maken via het Resource Manager-implementatiemodel met behulp van Azure Portal. U kunt via Resource Manager ook een VNet maken met andere hulpprogramma's. Bovendien kunt u een VNet maken via het klassieke implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [CLI](virtual-networks-create-vnet-arm-cli.md)
- [Sjabloon](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klassiek)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klassiek)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (klassiek)](virtual-networks-create-vnet-classic-cli.md)


[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voltooi de volgende stappen om een virtueel netwerk te maken met Azure Portal:

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. Klik op **Nieuw** > **Netwerken** > **Virtueel netwerk**, zoals wordt weergegeven in de volgende afbeelding:

    ![Nieuw virtueel netwerk](./media/virtual-network-create-vnet-arm-pportal/1.png)

3. Op de blade **Virtueel netwerk** die wordt weergegeven, moet u *Resource Manager* selecteren en op **Maken** klikken, zoals weergegeven in de volgende afbeelding:

    ![Virtueel netwerk](./media/virtual-network-create-vnet-arm-pportal/2.png)
    
4. Op de blade **Virtueel netwerk maken** die wordt weergegeven, voert u *TestVNet* in voor **Naam**, *192.168.0.0/16* voor **Adresruimte**, *FrontEnd* voor **Subnetnaam**, *192.168.1.0/24* voor **Subnet-adresbereik** en *TestRG* voor **Resourcegroep**. Selecteer vervolgens uw **abonnement** en een **locatie**, en klik op de knop **Maken**, zoals wordt weergegeven in de volgende afbeelding:

    ![Virtueel netwerk maken](./media/virtual-network-create-vnet-arm-pportal/3.png)

    U kunt ook een bestaande resourcegroep selecteren. Lees het artikel [Overzicht van Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie over resourcegroepen. U kunt bovendien een andere locatie selecteren. Lees het artikel [Azure-regio's](https://azure.microsoft.com/regions) voor meer informatie over Azure-locaties en -regio's.

5. Via de portal kunt u slechts één subnet maken wanneer u een VNet maakt. Voor dit scenario moet een tweede subnet worden gemaakt nadat de VNet is gemaakt. Klik op **Alle resources** en vervolgens op **TestVNet** op de blade **Alle resources** om het tweede subnet te maken, zoals wordt weergegeven in de volgende afbeelding:

    ![Gemaakte VNet](./media/virtual-network-create-vnet-arm-pportal/4.png)

6. Op de blade **TestVNet** die wordt weergegeven, klikt u op **Subnet** en vervolgens op **+ Subnet**. Voer *back-end* in voor **Naam** en *192.168.2.0/24* voor **Adresbereik** op de blade **Subnet toevoegen**, en klik daarna op **OK**, zoals wordt weergegeven in de volgende afbeelding:

    ![Subnet toevoegen](./media/virtual-network-create-vnet-arm-pportal/5.png)

7. De twee subnetten worden weergegeven zoals in de volgende afbeelding:
    
    ![Lijst met subnetten in VNet](./media/virtual-network-create-vnet-arm-pportal/6.png)

In dit artikel wordt uitgelegd hoe u voor testdoeleinden een virtueel netwerk maakt met twee subnetten. Voordat u een virtueel netwerk maakt voor productiegebruik, raden wij u aan de artikelen [Virtueel netwerkoverzicht](virtual-networks-overview.md) en [Virtueel netwerkplan en -ontwerp](virtual-network-vnet-plan-design-arm.md) te lezen om virtuele netwerken en alle instellingen volledig te begrijpen. 

## <a name="next-steps"></a>Volgende stappen

Leer hoe u de volgende verbindingen maakt:

- Verbinding van een virtuele machine (VM) met een virtueel netwerk: lees de artikelen [Een Windows VM maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) of [Een Linux-VM maken](../virtual-machines/virtual-machines-linux-quick-create-portal.md). In plaats van een VNet en subnet te maken via de stappen die in de artikelen worden beschreven, kunt u ook een bestaand VNet en subnet selecteren waarmee u een VM wilt verbinden.
- Verbinding van het virtuele netwerk met andere virtuele netwerken: lees het artikel [VNets verbinden](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Verbinding van het virtuele netwerk met een on-premises netwerk via een site-naar-site virtueel particulier netwerk (VPN) of een ExpressRoute-circuit. Lees hiervoor de artikelen [Een VNet verbinden met een on-premises netwerk via een site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) en [Een VNet koppelen aan een ExpressRoute-circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).


<!--HONumber=Jan17_HO1-->


