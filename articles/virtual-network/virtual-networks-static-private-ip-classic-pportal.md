---
title: "Privé IP-adressen voor virtuele machines (klassiek) - Azure-portal configureren | Microsoft Docs"
description: "Informatie over het configureren van privé IP-adressen voor virtuele machines (klassiek) met de Azure portal."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bde6de3495c2909b63b1f85e420a4ff5e7ac2c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Privé IP-adressen voor een virtuele machine (klassiek) met de Azure portal configureren

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [een statisch privé IP-adres in het Resource Manager-implementatiemodel beheren](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De volgende stappen uit het voorbeeld verwacht een eenvoudige omgeving al gemaakt. Als u wilt de stappen uitvoeren, zoals ze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in bouwen [een vnet maken](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statisch privé IP-adres opgeven bij het maken van een virtuele machine
Maken van een virtuele machine met de naam *DNS01* in de *FrontEnd* subnet van een VNet met de naam *TestVNet* met een statisch privé IP-adres van *192.168.1.101*, voer de volgende stappen uit:

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. Klik op **nieuw** > **Compute** > **Windows Server 2012 R2 Datacenter**, zoals u ziet de **een implementatiemodelselecteren** lijst al staat **klassieke**, en klik vervolgens op **maken**.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. In de **VM maken** blade, voer de naam van de virtuele machine moet worden gemaakt (*DNS01* in ons scenario), wordt het lokale administrator-account en wachtwoord.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Klik op **optionele configuratie** > **netwerk** > **virtueel netwerk**, en klik vervolgens op **TestVNet**. Als **TestVNet** is niet beschikbaar is, zorg ervoor dat u gebruikt de *VS-midden* locatie en de testomgeving beschreven aan het begin van dit artikel hebt gemaakt.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. In de **netwerk** blade, zorg ervoor dat het subnet geselecteerde *FrontEnd*, klikt u vervolgens op **IP-adressen**onder **IP-adrestoewijzing**klikt u op **statische**, en voer vervolgens *192.168.1.101* voor **IP-adres** zoals hieronder wordt weergegeven.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Klik op **OK** in de **IP-adressen** blade, klikt u vervolgens op **OK** in de **netwerk** blade en klik op **OK** in de **optionele configuratie** blade.
7. In de **VM maken** blade, klikt u op **maken**. Let op de tegel weergegeven in het dashboard.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische privé IP-adresgegevens voor een virtuele machine ophalen
Als u wilt weergeven in het statische privé IP-adresgegevens voor de virtuele machine gemaakt met de bovenstaande stappen, de onderstaande stappen worden uitgevoerd.

1. Klik in het Azure-Azure-portal op **door alles bladeren** > **virtuele machines (klassiek)** > **DNS01** > **alle instellingen** > **IP-adressen** en Let op de toewijzing van IP-adres en het IP-adres zoals hieronder wordt weergegeven.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statisch privé IP-adres van een virtuele machine verwijderen
Als u wilt verwijderen van het statische privé IP-adres van de virtuele machine die eerder is gemaakt, de volgende stappen uit te voeren.

1. Van de **IP-adressen** blade hierboven, klikt u op **dynamische** rechts van **IP-adrestoewijzing**, klikt u vervolgens op **opslaan**, en klik vervolgens op **Ja**.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statisch privé IP-adres toevoegen aan een bestaande virtuele machine
Als een statisch privé IP-adres aan de virtuele machine gemaakt met behulp van de bovenstaande stappen toevoegen, de volgende stappen uit te voeren:

1. Van de **IP-adressen** blade hierboven, klikt u op **statische** rechts van **IP-adrestoewijzing**.
2. Type *192.168.1.101* voor **IP-adres**, klikt u vervolgens op **opslaan**, en klik vervolgens op **Ja**.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [gereserveerde openbare IP-adres](virtual-networks-reserved-public-ip.md) adressen.
* Meer informatie over [instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adressen.
* Raadpleeg de [gereserveerd IP-REST-API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).

