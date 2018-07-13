---
title: Privé-IP-adressen voor VM's (klassiek) - Azure-portal configureren | Microsoft Docs
description: Informatie over het configureren van privé IP-adressen voor virtuele machines (klassiek) met behulp van de Azure portal.
services: virtual-network
documentationcenter: na
author: genlin
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
ms.openlocfilehash: 1aae74d8077a75e5ab556703b1c1531f540bbdb4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38698561"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Privé IP-adressen voor een virtuele machine (klassiek) met behulp van de Azure-portal configureren

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [beheren van een statisch privé IP-adres in het Resource Manager-implementatiemodel](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De voorbeeldstappen die volgen verwachten dat een eenvoudige omgeving al gemaakt. Als u uitvoeren van de stappen wilt zoals ze worden weergegeven in dit document, eerst samenstellen van de testomgeving die wordt beschreven [een vnet maken](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statisch privé IP-adres opgeven bij het maken van een virtuele machine
Het maken van een virtuele machine met de naam *DNS01* in de *FrontEnd* subnet van een VNet met de naam *TestVNet* met een statisch privé IP-adres van *192.168.1.101*, voltooid de volgende stappen uit:

1. Navigeer in een browser naar https://portal.azure.com en meld u, indien nodig, aan met uw Azure-account.
2. Selecteer **nieuw** > **Compute** > **Windows Server 2012 R2 Datacenter**, zoals u ziet de **een implementatiemodelselecteren** lijst met al wordt weergegeven **klassieke**, en selecteer vervolgens **maken**.
   
    ![Virtuele machine maken in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. Onder **maken VM**, voer de naam van de virtuele machine wordt gemaakt (*DNS01* in het scenario), wordt het lokale administrator-account en wachtwoord.
   
    ![Virtuele machine maken in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Selecteer **optionele configuratie** > **netwerk** > **Virtueelnetwerk**, en selecteer vervolgens **TestVNet** . Als **TestVNet** is niet beschikbaar is, zorg ervoor dat u gebruikt de *VS-midden* locatie en de testomgeving die wordt beschreven aan het begin van dit artikel hebt gemaakt.
   
    ![Virtuele machine maken in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. Onder **netwerk**, zorg ervoor dat het geselecteerde subnet *FrontEnd*en selecteer vervolgens **IP-adressen**onder **IP-adrestoewijzing** Selecteer **statische**, en voer vervolgens *192.168.1.101* voor **IP-adres** zoals hieronder wordt weergegeven.
   
    ![Virtuele machine maken in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Selecteer **OK** onder **IP-adressen**, selecteer **OK** onder **netwerk**, en selecteer vervolgens **OK** onder **Optionele configuratie**.
7. Onder **maken VM**, selecteer **maken**. U ziet de tegel weergegeven in uw dashboard:
   
    ![Virtuele machine maken in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische privé IP-adresgegevens voor een virtuele machine ophalen
Als u wilt weergeven met de statische privé IP-adresgegevens voor de virtuele machine gemaakt met de bovenstaande stappen, voer de onderstaande stappen.

1. Selecteer in de Azure-portal **door alles bladeren** > **virtuele machines (klassiek)** > **DNS01** > **alle instellingen voor** > **IP-adressen** en ziet u de toewijzing van IP-adressen en IP-adres zoals hieronder wordt weergegeven.
   
    ![Virtuele machine maken in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Het verwijderen van een statisch privé IP-adres van een virtuele machine

Onder **IP-adressen**, selecteer **dynamische** aan de rechterkant van **IP-adrestoewijzing**, selecteer **opslaan**, en selecteer vervolgens  **Ja**, zoals wordt weergegeven in de volgende afbeelding:
   
    ![Create VM in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statisch privé IP-adres toevoegen aan een bestaande virtuele machine

1. Onder **IP-adressen**, eerder weergegeven, selecteer **statische** aan de rechterkant van **IP-adrestoewijzing**.
2. Type *192.168.1.101* voor **IP-adres**, selecteer **opslaan**, en selecteer vervolgens **Ja**.

## <a name="set-ip-addresses-within-the-operating-system"></a>IP-adressen binnen het besturingssysteem instellen

Het verdient aanbeveling dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een virtuele machine, toewijst tenzij die nodig zijn. Als u het particuliere IP-adres binnen het besturingssysteem handmatig instelt, zorg ervoor dat deze hetzelfde adres als de privé IP-adres toegewezen aan de Azure-VM of verliest u connectiviteit met de virtuele machine. U moet het openbare IP-adres toegewezen aan een virtuele machine van Azure binnen het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [gereserveerde openbare IP-adres](virtual-networks-reserved-public-ip.md) adressen.
* Meer informatie over [instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adressen.
* Raadpleeg de [gereserveerd IP-REST-API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).

