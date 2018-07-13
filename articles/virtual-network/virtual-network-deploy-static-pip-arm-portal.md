---
title: Een virtuele machine maken met een statisch openbaar IP-adres - Azure portal | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statisch openbaar IP-adres met behulp van de Azure portal.
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
ms.openlocfilehash: 524293f9a1ded73ee7cb6ba4f53208a9f9c54ffa
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38670981"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Een virtuele machine maken met een statisch openbaar IP-adres met behulp van de Azure portal

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Een virtuele machine maken met een statisch openbaar IP-adres

Voor het maken van een virtuele machine met een statisch openbaar IP-adres in Azure portal, moet u de volgende stappen uitvoeren:

1. Navigeer via een browser naar de [Azure Portal](https://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op de bovenste links hoek van de portal, **een resource maken**>>**Compute**>**Windows Server 2012 R2 Datacenter**.
3. In de **een implementatiemodel selecteren** Selecteer **Resource Manager** en klikt u op **maken**.
4. In de **basisbeginselen** deelvenster, voert u de informatie van de virtuele machine als volgt, en klik vervolgens op **OK**.
   
    ![Azure portal - basisbeginselen](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. In de **Kies een grootte** deelvenster, klikt u op **Standard A1** als volgt te werk en klik op **Selecteer**.
   
    ![Azure-portal: Kies een grootte](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. In de **instellingen** deelvenster, klikt u op **openbaar IP-adres**, vervolgens in de **openbare IP-adres maken** deelvenster onder **toewijzing**, klikt u op  **Statische** als volgt. En klik vervolgens op **OK**.
   
    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. In de **instellingen** deelvenster, klikt u op **OK**.
8. Controleer de **samenvatting** deelvenster als volgt te werk en klik op **OK**.
   
    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. U ziet de nieuwe tegel in uw dashboard.
   
    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Nadat de virtuele machine is gemaakt, de **instellingen** deelvenster ziet er als volgt:
    
    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

## <a name="set-ip-addresses-within-the-operating-system"></a>IP-adressen binnen het besturingssysteem instellen

U moet het openbare IP-adres toegewezen aan een virtuele machine van Azure binnen het besturingssysteem van de virtuele machine nooit handmatig toewijzen. Het verdient aanbeveling dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een virtuele machine, toewijst tenzij die nodig zijn, bijvoorbeeld wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-portal.md). Als u het particuliere IP-adres binnen het besturingssysteem handmatig instellen wilt, controleert u of deze hetzelfde adres als de privé IP-adres toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of het verlies van connectiviteit met de virtuele machine. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen.

## <a name="next-steps"></a>Volgende stappen

Al het netwerkverkeer kan stromen naar en van de virtuele machine die in dit artikel hebt gemaakt. U kunt inkomende en uitgaande beveiligingsregels binnen een netwerkbeveiligingsgroep die het verkeer dat naar en van de netwerkinterface, het subnet of beide stromen kan beperken definiëren. Zie voor meer informatie over netwerkbeveiligingsgroepen, [overzicht van netwerkbeveiligingsgroepen](security-overview.md).