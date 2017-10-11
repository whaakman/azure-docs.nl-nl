---
title: Een virtuele machine maken met statische openbare IP-adres - Azure-portal | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statische openbare IP-adres met de Azure portal.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 233e4eea8439320c1c7446e2c2b2e9d379351a3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Een virtuele machine maken met een statische openbare IP-adres met de Azure portal

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Sjabloon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel wordt behandeld met het implementatiemodel van Resource Manager, die Microsoft voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel aanbeveelt.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Een virtuele machine maken met een statische openbare IP-adres

Als een virtuele machine maken met een statische openbare IP-adres in de Azure portal, moet u de volgende stappen uitvoeren:

1. Navigeer via een browser naar de [Azure Portal](https://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op de bovenste linkerkant hoek van de portal, **nieuw**>>**Compute**>**Windows Server 2012 R2 Datacenter**.
3. In de **een implementatiemodel selecteren** Selecteer **Resource Manager** en klik op **maken**.
4. In de **basisbeginselen** blade, geef de VM-informatie, zoals hieronder wordt weergegeven en klik vervolgens op **OK**.
   
    ![Azure portal - basisbeginselen](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. In de **een grootte kiezen** blade, klikt u op **A1 standaard** zoals hieronder aangegeven, en klik vervolgens op **Selecteer**.
   
    ![Azure-portal - een grootte kiezen](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. In de **instellingen** blade, klikt u op **openbaar IP-adres**, klik dan in de **openbare IP-adres maken** blade onder **toewijzing**, klikt u op **statische** zoals hieronder wordt weergegeven. En klik vervolgens op **OK**.
   
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. In de **instellingen** blade, klikt u op **OK**.
8. Controleer de **samenvatting** blade, zoals hieronder wordt weergegeven en klik vervolgens op **OK**.
   
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Let op de nieuwe tegel in uw dashboard.
   
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Zodra de virtuele machine is gemaakt, de **instellingen** blade wordt weergegeven zoals hieronder wordt weergegeven
    
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

