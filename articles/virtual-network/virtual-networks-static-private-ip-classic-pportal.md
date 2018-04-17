---
title: Privé IP-adressen voor virtuele machines (klassiek) - Azure-portal configureren | Microsoft Docs
description: Informatie over het configureren van privé IP-adressen voor virtuele machines (klassiek) met de Azure portal.
services: virtual-network
documentationcenter: na
author: genli
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7b81f0fc1d504a0cb3422e003aa4ad7986396652
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Privé IP-adressen voor een virtuele machine (klassiek) met de Azure portal configureren

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [een statisch privé IP-adres in het Resource Manager-implementatiemodel beheren](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De voorbeeldstappen volgen verwacht een eenvoudige omgeving al gemaakt. Als u wilt de stappen uitvoeren, zoals ze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in bouwen [een vnet maken](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statisch privé IP-adres opgeven bij het maken van een virtuele machine
Maken van een virtuele machine met de naam *DNS01* in de *FrontEnd* subnet van een VNet met de naam *TestVNet* met een statisch privé IP-adres van *192.168.1.101*, voltooid de volgende stappen uit:

1. Navigeer in een browser naar https://portal.azure.com en meld u, indien nodig, aan met uw Azure-account.
2. Selecteer **nieuw** > **Compute** > **Windows Server 2012 R2 Datacenter**, zoals u ziet de **een implementatiemodelselecteren** lijst al staat **klassieke**, en selecteer vervolgens **maken**.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. Onder **VM maken**, voer de naam van de virtuele machine moet worden gemaakt (*DNS01* in het scenario), wordt het lokale administrator-account en wachtwoord.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Selecteer **optionele configuratie** > **netwerk** > **virtueel netwerk**, en selecteer vervolgens **TestVNet** . Als **TestVNet** is niet beschikbaar is, zorg ervoor dat u gebruikt de *VS-midden* locatie en de testomgeving beschreven aan het begin van dit artikel hebt gemaakt.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. Onder **netwerk**, zorg ervoor dat het subnet geselecteerde *FrontEnd*, selecteer daarna **IP-adressen**onder **IP-adrestoewijzing** Selecteer **statische**, en voer vervolgens *192.168.1.101* voor **IP-adres** zoals hieronder wordt weergegeven.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Selecteer **OK** onder **IP-adressen**, selecteer **OK** onder **netwerk**, en selecteer vervolgens **OK** onder **Optionele configuratie**.
7. Onder **VM maken**, selecteer **maken**. Let op de tegel weergegeven in het dashboard:
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische privé IP-adresgegevens voor een virtuele machine ophalen
Als u wilt weergeven in het statische privé IP-adresgegevens voor de virtuele machine gemaakt met de bovenstaande stappen, de onderstaande stappen worden uitgevoerd.

1. Selecteer in de Azure-portal **door alles bladeren** > **virtuele machines (klassiek)** > **DNS01** > **alle instellingen** > **IP-adressen** en Let op de toewijzing van IP-adres en het IP-adres zoals hieronder wordt weergegeven.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statisch privé IP-adres van een virtuele machine verwijderen

Onder **IP-adressen**, selecteer **dynamische** rechts van **IP-adrestoewijzing**, selecteer **opslaan**, en selecteer vervolgens  **Ja**, zoals wordt weergegeven in de volgende afbeelding:
   
    ![Create VM in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statisch privé IP-adres toevoegen aan een bestaande virtuele machine

1. Onder **IP-adressen**, eerder weergegeven, selecteer **statische** rechts van **IP-adrestoewijzing**.
2. Type *192.168.1.101* voor **IP-adres**, selecteer **opslaan**, en selecteer vervolgens **Ja**.

## <a name="set-ip-addresses-within-the-operating-system"></a>Instellen van IP-adressen binnen het besturingssysteem

Het raadzaam dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een VM, toewijst tenzij nodig. Als u de privé IP-adres in het besturingssysteem handmatig instelt, moet u zorgen dat het hetzelfde adres als de privé IP-adres is toegewezen aan de Azure VM of u verbinding met de virtuele machine kunt verliezen. U moet het openbare IP-adres is toegewezen aan een virtuele machine van Azure in het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [gereserveerde openbare IP-adres](virtual-networks-reserved-public-ip.md) adressen.
* Meer informatie over [instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adressen.
* Raadpleeg de [gereserveerd IP-REST-API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).

