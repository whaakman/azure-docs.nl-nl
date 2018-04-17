---
title: Privé IP-adressen voor virtuele machines - Azure-portal configureren | Microsoft Docs
description: Informatie over het configureren van privé IP-adressen voor virtuele machines met de Azure-portal.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff9d025980f80cb77246ea12dbf2e9bcedd73f86
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal"></a>Privé IP-adressen voor een virtuele machine met de Azure portal configureren

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
> * [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
> * [Azure-CLI](virtual-networks-static-private-ip-arm-cli.md)
> * [Azure-portal (klassiek)](virtual-networks-static-private-ip-classic-pportal.md)
> * [PowerShell (klassiek)](virtual-networks-static-private-ip-classic-ps.md)
> * [Azure CLI (klassiek)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het Resource Manager-implementatiemodel. U kunt ook [statisch privé IP-adres in het klassieke implementatiemodel beheren](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De volgende stappen in de steekproef verwacht een eenvoudige omgeving al gemaakt. Als u wilt de stappen uitvoeren, zoals ze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in bouwen [een virtueel netwerk maken](quick-create-portal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Het maken van een virtuele machine voor het testen van statisch privé IP-adressen
U kunt een statisch privé IP-adres niet instellen tijdens het maken van een virtuele machine in de implementatiemodus Resource Manager-met behulp van de Azure-portal. U moet eerst de virtuele machine maken en vervolgens de privé-IP statisch ingesteld.

Maken van een virtuele machine met de naam *DNS01* in de *FrontEnd* subnet van een VNet met de naam *TestVNet*, als volgt te werk:

1. Navigeer in een browser naar http://portal.azure.com en meld u, indien nodig, aan met uw Azure-account.
2. Klik op **maken van een resource** > **Compute** > **Windows Server 2012 R2 Datacenter**, zoals u ziet de **selecteert u een implementatie model** lijst al staat **Resource Manager**, en klik vervolgens op **maken**, zoals in de volgende afbeelding.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. In de **basisbeginselen** deelvenster, voer de naam van de virtuele machine te maken (*DNS01* in het scenario), wordt het lokale administrator-account en wachtwoord, zoals in de volgende afbeelding.
   
    ![Deelvenster basisbeginselen](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Zorg ervoor dat de **locatie** geselecteerd is *VS-midden*, klikt u vervolgens op **Selecteer een bestaande** onder **resourcegroep**, klikt u vervolgens op **resourcegroep** opnieuw in en klik vervolgens op *TestRG*, en klik vervolgens op **OK**.
   
    ![Deelvenster basisbeginselen](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. In de **een grootte kiezen** deelvenster **A1 standaard**, en klik vervolgens op **Selecteer**.
   
    ![Kies een deelvenster grootte](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. In de **instellingen** deelvenster, zorg ervoor dat de eigenschappen worden ingesteld met de volgende waarden en klik vervolgens op **OK**.
   
    -**Storage-account**: *vnetstorage*
   
   * **Netwerk**: *TestVNet*
   * **Subnet**: *FrontEnd*
     
     ![Kies een deelvenster grootte](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. In de **samenvatting** deelvenster, klikt u op **OK**. U ziet de volgende tegel weergegeven in het dashboard.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

Het raadzaam dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een VM, toewijst tenzij nodig, zoals wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-portal.md). Als u de privé IP-adres in het besturingssysteem handmatig instelt, moet u zorgen dat het hetzelfde adres als de privé IP-adres is toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of u verbinding met de virtuele machine kunt verliezen. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen. U moet het openbare IP-adres is toegewezen aan een virtuele machine van Azure in het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische privé IP-adresgegevens voor een virtuele machine ophalen
Als u wilt weergeven in het statische privé IP-adresgegevens voor de virtuele machine gemaakt met de bovenstaande stappen, de volgende stappen worden uitgevoerd.

1. Klik in de Azure-portal op **door alles bladeren** > **virtuele machines** > **DNS01** > **alle instellingen**  >  **Netwerkinterfaces** en klik vervolgens op de alleen-netwerkinterface vermeld.
   
    ![VM-tegel implementeren](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. In de **netwerkinterface** deelvenster, klikt u op **alle instellingen** > **IP-adressen** en Let op de **toewijzing** en  **IP-adres** waarden.
   
    ![VM-tegel implementeren](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statisch privé IP-adres toevoegen aan een bestaande virtuele machine
Als een statisch privé IP-adres aan de virtuele machine gemaakt met behulp van de bovenstaande stappen toevoegen, de volgende stappen uit:

1. Van de **IP-adressen** deelvenster hierboven, klikt u op **statische** onder **toewijzing**.
2. Type *192.168.1.101* voor **IP-adres**, en klik vervolgens op **opslaan**.
   
    ![Virtuele machine maken in Azure-portal](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Als na klikken **opslaan**, merkt u dat de toewijzing nog is ingesteld op **dynamische**, betekent dit het IP-adres dat u hebt getypt, is al in gebruik. Probeer een ander IP-adres.
> 
> 

Het raadzaam dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een VM, toewijst tenzij nodig, zoals wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-portal.md). Als u de privé IP-adres in het besturingssysteem handmatig instelt, moet u zorgen dat het hetzelfde adres als de privé IP-adres is toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of u verbinding met de virtuele machine kunt verliezen. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen. U moet het openbare IP-adres is toegewezen aan een virtuele machine van Azure in het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statisch privé IP-adres van een virtuele machine verwijderen
Als u wilt verwijderen van het statische privé IP-adres van de virtuele machine die eerder is gemaakt, voert u de volgende stap:

Van de **IP-adressen** deelvenster hierboven, klikt u op **dynamische** onder **toewijzing**, en klik vervolgens op **opslaan**.

## <a name="set-ip-addresses-within-the-operating-system"></a>Instellen van IP-adressen binnen het besturingssysteem

Het raadzaam dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een VM, toewijst tenzij nodig, zoals wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-portal.md). Als u de privé IP-adres in het besturingssysteem handmatig instelt, moet u zorgen dat het hetzelfde adres als de privé IP-adres is toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of u verbinding met de virtuele machine kunt verliezen. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen. U moet het openbare IP-adres is toegewezen aan een virtuele machine van Azure in het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het beheren van [IP-adresinstellingen](virtual-network-network-interface-addresses.md).

