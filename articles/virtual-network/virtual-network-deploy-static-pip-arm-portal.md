---
title: Een virtuele machine maken met statische openbare IP-adres - Azure-portal | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statische openbare IP-adres met de Azure portal.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
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
ms.openlocfilehash: 50ae4d6e8c275db16f811a2a1a063eda441f150b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Een virtuele machine maken met een statische openbare IP-adres met de Azure portal

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel wordt behandeld met het implementatiemodel van Resource Manager, die Microsoft voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel aanbeveelt.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Een virtuele machine maken met een statische openbare IP-adres

Als een virtuele machine maken met een statische openbare IP-adres in de Azure portal, moet u de volgende stappen uitvoeren:

1. Navigeer via een browser naar de [Azure Portal](https://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op de bovenste linkerbenedenhoek weergegeven van de portal op **maken van een resource**>>**Compute**>**Windows Server 2012 R2 Datacenter**.
3. In de **een implementatiemodel selecteren** Selecteer **Resource Manager** en klik op **maken**.
4. In de **basisbeginselen** deelvenster Voer als volgt de VM-gegevens in en klik vervolgens op **OK**.
   
    ![Azure portal - basisbeginselen](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. In de **een grootte kiezen** deelvenster, klikt u op **A1 standaard** als volgt en klik op **Selecteer**.
   
    ![Azure-portal - een grootte kiezen](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. In de **instellingen** deelvenster klikt u op **openbaar IP-adres**, klik dan in de **openbare IP-adres maken** deelvenster onder **toewijzing**, klikt u op  **Statische** als als volgt. En klik vervolgens op **OK**.
   
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. In de **instellingen** deelvenster, klikt u op **OK**.
8. Controleer de **samenvatting** deelvenster als volgt en klik op **OK**.
   
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Let op de nieuwe tegel in uw dashboard.
   
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Zodra de virtuele machine is gemaakt, de **instellingen** deelvenster ziet er als volgt:
    
    ![Azure-portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

## <a name="set-ip-addresses-within-the-operating-system"></a>Instellen van IP-adressen binnen het besturingssysteem

U moet het openbare IP-adres is toegewezen aan een virtuele machine van Azure in het besturingssysteem van de virtuele machine nooit handmatig toewijzen. Het raadzaam dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een VM, toewijst tenzij nodig, zoals wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-portal.md). Als u de privé IP-adres in het besturingssysteem handmatig instelt, moet u zorgen dat het hetzelfde adres als de privé IP-adres is toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of u verbinding met de virtuele machine kunt verliezen. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen.

## <a name="next-steps"></a>Volgende stappen

Netwerkverkeer kan stromen naar en van de virtuele machine gemaakt in dit artikel. Binnenkomende en uitgaande beveiligingsregels voor verbindingen binnen een netwerkbeveiligingsgroep die het verkeer dat naar en van de netwerkinterface, het subnet of beide stromen kan beperken, kunt u definiëren. Zie voor meer informatie over netwerkbeveiligingsgroepen, [netwerk groep beveiligingsoverzicht](security-overview.md).