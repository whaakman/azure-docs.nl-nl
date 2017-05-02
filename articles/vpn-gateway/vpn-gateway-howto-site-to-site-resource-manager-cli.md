---
title: 'Uw on-premises netwerk verbinden met een virtueel Azure-netwerk: site-naar-site-VPN: CLI | Microsoft Docs'
description: Stappen voor het maken van een IPSec-verbinding van uw on-premises netwerk met een virtueel Azure-netwerk via het openbare internet. Deze stappen helpen u een cross-premises site-naar-site-VPN-gatewayverbinding te maken met CLI.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c3563f3a3fa46d40ba02fe97b3b0ebe3c45caddd
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Een virtueel netwerk maken met een site-naar-site-VPN-verbinding met CLI

In dit artikel leest u hoe u de Azure CLI gebruikt om een site-naar-site-VPN-gatewayverbinding te maken vanaf uw on-premises netwerk naar het VNet. De stappen in dit artikel zijn van toepassing op het Resource Manager-implementatiemodel. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager - CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassiek - klassieke portal](vpn-gateway-site-to-site-create.md)
> 
>


![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-connection-diagram.png)

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Zie [Overzicht van VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over VPN-gateways.

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint, of aan de volgende criteria is voldaan:

* U hebt vastgesteld dat u wilt werken met het Resource Manager-implementatiemodel. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Een compatibel VPN-apparaat en iemand die dit kan configureren. Zie [Over VPN-apparaten](vpn-gateway-about-vpn-devices.md) voor meer informatie over compatibele VPN-apparaten en -apparaatconfiguratie.
* Een extern gericht openbaar IPv4-adres voor het VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
* Als u de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken. Wanneer u deze configuratie maakt, moet u de IP-adresbereikvoorvoegsels opgeven die Azure naar uw on-premises locatie doorstuurt. Geen van de subnetten van uw on-premises netwerk kan overlappen met de virtuele subnetten waarmee u verbinding wilt maken. 
* De meest recente versie van de CLI-opdrachten (2.0 of hoger). Zie [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) voor meer informatie over het installeren van de CLI-opdrachten.

### <a name="example-values"></a>Voorbeeldwaarden

U kunt de volgende waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.12.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.12.0.0/24 
GatewaySubnet           = 10.12.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24 
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli
az login
```

Als u meer dan één Azure-abonnement hebt, worden alle abonnementen voor het account weergegeven.

```azurecli
Az account list --all
```

Geef het abonnement op dat u wilt gebruiken.

```azurecli
Az account set --subscription <replace_with_your_subscription_id>
```

## <a name="2-create-a-resource-group"></a>2. Een resourcegroep maken

In het volgende voorbeeld wordt een resourcegroep met de naam 'TestRG1' gemaakt op de locatie 'eastus'. Als u al een resourcegroep hebt in de regio waarin u uw VNet wilt maken, kunt u desgewenst ook deze gebruiken.

```azurecli
az group create -n TestRG1 -l eastus
```

## <a name="VNet"></a>3. Een virtueel netwerk maken

Als u nog geen virtueel netwerk hebt, maakt u er een. Controleer bij het maken van een virtueel netwerk of de adresruimten die u opgeeft, niet overlappen met adresruimten in uw on-premises netwerk. 

In het volgende voorbeeld worden een virtueel netwerk met de naam 'TestVNet1' en een subnet met de naam 'Subnet1' gemaakt.

```azurecli
az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.12.0.0/16 -l eastus --subnet-name Subnet1 --subnet-prefix 10.12.0.0/24
```

## 4. <a name="gwsub"></a>Het gatewaysubnet maken

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Voor deze configuratie hebt u ook een gatewaysubnet nodig. De virtuele netwerkgateway gebruikt een gatewaysubnet dat de IP-adressen bevat die door de VPN-gatewayservices worden gebruikt. Wanneer u een gatewaysubnet maakt, moet dit de naam GatewaySubnet krijgen. Als u een andere naam kiest, maakt u wel een subnet, maar wordt dit door Azure niet als een gatewaysubnet behandeld.

De grootte van het gatewaysubnet dat u opgeeft, is afhankelijk van de VPN-gatewayconfiguratie die u wilt maken. Het is mogelijk om een klein gatewaysubnet van /29 te maken, maar we raden u aan een groter subnet met meer adressen te maken door /27 of /28 te selecteren. Met een groter gatewaysubnet beschikt u over voldoende IP-adressen voor mogelijke toekomstige configuraties.


```azurecli
az network vnet subnet create --address-prefix 10.12.255.0/27 -n GatewaySubnet -g TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. De lokale netwerkgateway maken

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. U geeft de site een naam waarmee Azure hiernaar kan verwijzen en geeft vervolgens het IP-adres op van het on-premises VPN-apparaat waarmee u verbinding maakt. U geeft ook de IP-adresvoorvoegsels op die via de VPN-gateway worden doorgestuurd naar het VPN-apparaat. De adresvoorvoegsels die u opgeeft, zijn de voorvoegsels die zich in uw on-premises netwerk bevinden. Als uw on-premises netwerk verandert, kunt u de voorvoegsels eenvoudig bijwerken.

Gebruik de volgende waarden:

* Het *--gateway-ip-address* is het IP-adres van uw on-premises VPN-apparaat. Het VPN-apparaat kan zich niet achter een NAT bevinden.
* De *--local-address-prefixes* zijn uw on-premises adresruimten.

In het volgende voorbeeld wordt geïllustreerd hoe u een lokale netwerkgateway met meerdere adresvoorvoegsels toevoegt:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Een openbaar IP-adres aanvragen

Vraag een openbaar IP-adres aan dat moet worden toegewezen aan uw virtuele VPN-gateway. Dit is het IP-adres dat u configureert voor het maken van verbinding met uw VPN-apparaat.

De virtuele netwerkgateway voor het Resource Manager-implementatiemodel ondersteunt momenteel alleen openbare IP-adressen met behulp van de dynamische toewijzingsmethode. Dit betekent echter niet dat het IP-adres verandert. Het IP-adres van de VPN-gateway verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het openbare IP-adres van de virtuele netwerkgateway verandert niet wanneer de grootte van uw VPN-gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer er andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd. 

```azurecli
az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. De VPN-gateway maken

Maak de VPN-gateway van het virtuele netwerk. Het maken van een VPN-gateway kan 45 minuten of langer duren.

Gebruik de volgende waarden:

* Het *--gateway-type* voor een site-naar-site-configuratie is *VPN*. Het gatewaytype is altijd specifiek voor de configuratie die u implementeert. Zie [Soorten gateways](vpn-gateway-about-vpn-gateway-settings.md#gwtype) voor meer informatie.
* Het *--vpn-type* kan *RouteBased* (in sommige documentatie een 'dynamische gateway' genoemd) of *PolicyBased* (in sommige documentatie een 'statische gateway' genoemd) zijn. De instelling is specifiek voor de vereisten van het apparaat waarmee u verbinding maakt. Zie [Informatie over VPN-gatewayconfiguratie-instellingen](vpn-gateway-about-vpn-gateway-settings.md#vpntype) voor meer informatie over soorten VPN-gateways.
* De *--sku* kan Basic, Standard of HighPerformance zijn. Voor bepaalde SKU's gelden configuratiebeperkingen. Zie [Gateway-SKU's](vpn-gateway-about-vpngateways.md#gateway-skus) voor meer informatie.

Na het uitvoeren van deze opdracht wordt er geen feedback of uitvoer weergegeven. Het duurt ongeveer 45 minuten om een gateway te maken.

```azurecli
az network vnet-gateway create -n VNet1GW --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku Standard --no-wait 
```

## <a name="VPNDevice"></a>8. Uw VPN-apparaat configureren

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]
 Gebruik het volgende voorbeeld om het openbare IP-adres van uw virtuele netwerkgateway te vinden en vervang de waarden door uw eigen waarden. De lijst met openbare IP-adressen wordt in een gemakkelijk leesbare tabelindeling weergegeven.

  ```azurecli
  az network public-ip list -g TestRG1 -o table
  ```

## <a name="CreateConnection"></a>9. De VPN-verbinding maken

Maak de site-naar-site-VPN-verbinding tussen de gateway van uw virtuele netwerk en het on-premises VPN-apparaat. Let vooral op de waarde van de gedeelde sleutel. Deze moet overeenkomen met de geconfigureerde waarde van de gedeelde sleutel voor het VPN-apparaat.

```azurecli
az network vpn-connection create -n VNet1toSite2 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Na een korte tijd wordt de verbinding tot stand gebracht.

## <a name="toverify"></a>10. De VPN-verbinding controleren

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)] 

Zie [Een VPN-gatewayverbinding controleren](vpn-gateway-verify-connection-resource-manager.md) als u een andere methode wilt gebruiken om uw verbinding te controleren.

## <a name="common-tasks"></a>Algemene taken

### <a name="to-view-local-network-gateways"></a>Lokale netwerkgateways bekijken

```azurecli
az network local-gateway list --resource-group TestRG1
```

### <a name="modify"></a>IP-adresvoorvoegsels wijzigen voor de gateway van een lokaal netwerk
Volg de onderstaande instructies als u de voorvoegsels voor de gateway van een lokaal netwerk wilt wijzigen. Elke keer dat u een wijziging aanbrengt, moet de volledige lijst met voorvoegsels worden opgegeven, niet alleen de voorvoegsels die u wilt wijzigen.

- **Als u een verbinding hebt opgegeven**, gebruikt u het onderstaande voorbeeld. Geef de volledige lijst met voorvoegsels op, die bestaat uit de bestaande voorvoegsels en de voorvoegsels die u wilt toevoegen. In dit voorbeeld zijn 10.0.0.0/24 en 20.0.0.0/24 al aanwezig. We voegen de voorvoegsels 30.0.0.0/24 en 40.0.0.0/24 toe.

  ```azurecli
  az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 -n VNet1toSite2 -g TestRG1
  ```

- **Als u geen verbinding hebt opgegeven**, gebruikt u dezelfde opdracht die u gebruikt om een lokale netwerkgateway te maken. U kunt deze opdracht ook gebruiken om het gateway-IP-adres voor het VPN-apparaat bij te werken. Gebruik deze opdracht alleen als u nog geen verbinding hebt. In dit voorbeeld zijn 10.0.0.0/24, 20.0.0.0/24, 30.0.0.0/24 en 40.0.0.0/24 aanwezig. We geven alleen de voorvoegsels op die we willen houden. In dit geval 10.0.0.0/24 en 20.0.0.0/24.

  ```azurecli
  az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
  ```

### <a name="modifygwipaddress"></a>Het IP-adres van de gateway wijzigen in dat van een lokale netwerkgateway

Het IP-adres in deze configuratie is het IP-adres van het VPN-apparaat waarmee u verbinding maakt. Als het IP-adres van het VPN-apparaat verandert, kunt u de waarde wijzigen. Het IP-adres kan ook worden gewijzigd als er al een gatewayverbinding is.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 -n Site2 -g TestRG1
```

Controleer of de IP-adresvoorvoegsels zijn opgenomen als u het resultaat weergeeft.

  ```azurecli
  "localNetworkAddressSpace": { 
    "addressPrefixes": [ 
      "10.0.0.0/24", 
      "20.0.0.0/24", 
      "30.0.0.0/24" 
    ] 
  }, 
  "location": "eastus", 
  "name": "Site2", 
  "provisioningState": "Succeeded",  
  ```

### <a name="to-view-the-virtual-network-gateway-public-ip-address"></a>Het openbare IP-adres van de virtuele netwerkgateway bekijken

Gebruik het volgende voorbeeld om het openbare IP-adres van de gateway van uw virtuele netwerk te vinden. De lijst met openbare IP-adressen wordt in een gemakkelijk leesbare tabelindeling weergegeven.

```azurecli
az network public-ip list -g TestRG1 -o table
```

### <a name="to-verify-the-shared-key-values"></a>De gedeelde sleutelwaarden controleren

Controleer of de waarde van de gedeelde sleutel gelijk is aan de waarde die u voor de configuratie van uw VPN-apparaat hebt gebruikt. Als dat niet het geval is, voert u de verbinding opnieuw uit met de waarde van het apparaat of werkt u het apparaat bij met de waarde die is geretourneerd. De waarden moeten overeenkomen.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 -g TestRG1
```

## <a name="next-steps"></a>Volgende stappen

*  Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.
* Voor meer informatie over BGP raadpleegt u [BGP Overview](vpn-gateway-bgp-overview.md) (BGP-overzicht) en [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (BGP configureren).
* Zie [Geforceerde tunneling configureren](vpn-gateway-forced-tunneling-rm.md) voor meer informatie over geforceerde tunneling.
* Zie [Azure CLI](https://docs.microsoft.com/cli/azure/network) voor een lijst met Azure CLI-opdrachten voor netwerken.
