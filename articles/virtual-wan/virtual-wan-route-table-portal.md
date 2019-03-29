---
title: Een Azure virtuele WAN virtuele hub routetabel - Azure portal maken | Microsoft Docs
description: Virtuele WAN virtuele hub routetabel sturen verkeer naar een virtueel netwerkapparaat met behulp van de portal.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to create a route table using the portal.
ms.openlocfilehash: 2c8b3b4671fd14f9b10b8491861ae2c652f0188b
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58580581"
---
# <a name="create-a-virtual-wan-hub-route-table-for-nvas-azure-portal"></a>Maak een virtueel WAN hub routetabel voor NVA's: Azure Portal

In dit artikel wordt beschreven hoe u verkeer van een hub naar een netwerk virtuele Appliance (NVA) sturen.

![Virtual WAN-diagram](./media/virtual-wan-route-table/vwanroute.png)

## <a name="before-you-begin"></a>Voordat u begint

Controleer of dat u voldoet aan de volgende criteria:

*  U hebt een Network Virtual Appliance (NVA). Een virtueel netwerkapparaat is een software van derden van uw keuze die doorgaans in een virtueel netwerk van Azure Marketplace is ingericht.

    * Een privé IP-adres moet worden toegewezen aan de netwerkinterface van de NVA.

    * De NVA is niet geïmplementeerd in de virtuele hub. Worden moet geïmplementeerd in een apart VNet.

    *  De NVA-VNet kan een of meerdere virtuele netwerken zijn verbonden. In dit artikel verwijzen we naar de NVA-VNet als een 'indirecte knooppunt VNet'. Deze VNets kunnen worden verbonden met het VNet NVA met behulp van VNet-peering.
*  U kunt VNets 2 hebt gemaakt. Ze worden gebruikt als knooppunt VNets.

    * Voor deze oefening worden de adresruimten van de VNet-knooppunt: VNet1: 10.0.2.0/24 en VNet2: 10.0.3.0/24. Als u informatie over het maken van een VNet, Zie [maken van een virtueel netwerk](../virtual-network/quick-create-portal.md).

    * Controleer of dat er zijn geen virtuele netwerkgateways in een van de VNets.
    * Deze vnet's vereisen voor deze configuratie, niet een gatewaysubnet.

## <a name="signin"></a>1. Aanmelden

Open een browser, ga naar [Azure Portal](https://portal.azure.com) en meld u aan met uw Azure-account.

## <a name="vwan"></a>2. Een virtueel WAN maken

Maak een virtueel WAN. Voor deze oefening, kunt u de volgende waarden:

* **Virtuele WAN-naam:** myVirtualWAN
* **Resourcegroep:** testRG
* **Locatie:** US - west

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Een hub maken

De hub maken. Voor deze oefening, kunt u de volgende waarden:

* **Locatie:** US - west
* **Naam:** westushub
* **Privé-adresruimte van hub:** 10.0.1.0/24

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="route"></a>4. Maken en toepassen van de routetabel van een hub

De hub bijwerken met een routetabel hub. Voor deze oefening, kunt u de volgende waarden:

* **Indirecte spoke VNet-adresruimten:** (VNet1 en VNet2) 10.0.2.0/24 en 10.0.3.0/24
* **DMZ-NVA network interface privé IP-adres:** 10.0.4.5

1. Navigeer naar uw virtuele WAN.
2. Klik op de hub die u wilt maken van een routetabel.
3. Klik op de **...** , en klik vervolgens op **bewerken virtuele hub**.
4. Op de **bewerken virtuele hub** pagina, schuif naar beneden en selecteer het selectievakje **tabel gebruiken voor de routering van**.
5. In de **als voorvoegsel voor bestemming** kolom de adresruimten toevoegen. In de **verzenden naar de volgende hop** kolom, het DMZ-NVA network interface particulier IP-adres toevoegen.
6. Klik op **bevestigen** bijwerken van de hub-resource met de instellingen van de tabel.

## <a name="connections"></a>5. De VNet-verbindingen maken

Maak een verbinding van elk indirecte knooppunt VNet (VNet1 en VNet2) naar de hub. Vervolgens maakt u een verbinding van de NVA-VNet naar de hub.

 Voor deze stap kunt u de volgende waarden:

| VNET-naam| Verbindingsnaam|
| --- | --- |
| VNet1 | testconnection1 |
| VNet2 | testconnection2 |
| NVAVNet | testconnection3 |

Herhaal de volgende procedure voor elke VNet waarmee u verbinding wilt maken.

1. Klik op de pagina voor uw virtuele WAN op **Virtuele netwerkverbinding**.
2. Klik op de pagina Virtuele netwerkverbinding op **+Verbinding toevoegen**.
3. Vul de volgende velden in op de pagina **Verbinding toevoegen**:

    * **Verbindingsnaam** - voer een naam in voor uw verbinding.
    * **Hubs** - selecteer de hub die u wilt koppelen aan deze verbinding.
    * **Abonnement** - controleer of het abonnement klopt.
    * **Virtueel netwerk** - selecteer het virtuele netwerk dat met deze hub wilt verbinden. Het virtuele netwerk mag geen bestaande virtuele netwerkgateway hebben.
4. Klik op **OK** om de verbinding te maken.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Virtual WAN de pagina [Overzicht van Virtual WAN](virtual-wan-about.md).