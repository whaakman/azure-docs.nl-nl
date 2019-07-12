---
title: Een IPv6 dual stack-toepassing in Azure-netwerk - CLI implementeren
titlesuffix: Azure Virtual Network
description: In dit artikel toont hoe een toepassing IPv6 dual-stack in Azure-netwerk met behulp van Azure CLI implementeren.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 9e591bdf2ff0b6493f092d666d02c2614c907700
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798980"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure-virtual-network---cli-preview"></a>Implementeren van een IPv6 dual stack-toepassing in Azure-netwerk - CLI (Preview)

Dit artikel ziet u hoe u een dual-stack (IPv4 + IPv6)-toepassing in Azure met een dual-stack virtueel netwerk met een dual-stack-subnet, een load balancer met dual (IPv4 + IPv6) front-configuraties, VM's met NIC's waarvoor een dubbele IP-configuratie implementeert twee regels voor netwerkbeveiligingsgroepen en twee openbare IP-adressen.

> [!Important]
> IPv6 dual stack voor Azure Virtual Network is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als u nog geen abonnement op Azure hebt, maak dan nu een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u besluit te installeren en gebruiken Azure CLI lokaal in plaats daarvan, deze Quick Start, moet u het gebruik van Azure CLI versie 2.0.49 of hoger. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) voor installatie- of upgrade-informatie.

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van de IPv6 voor de functie Azure-netwerk, moet u uw abonnement met behulp van Azure PowerShell als volgt configureren:

```azurecli
az feature register --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
```
Het duurt maximaal 30 minuten voor de functieregistratie te voltooien. U kunt de registratiestatus van uw controleren door het uitvoeren van de volgende Azure CLI-opdracht:

```azurelci
az feature show --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
```
Nadat de registratie voltooid is, voert u de volgende opdracht uit:

```azurelci
az provider register --namespace Microsoft.Network
```
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u het virtuele netwerk van dual stack maken kunt, moet u een resourcegroep met [az-groep maken](/cli/azure/group). Het volgende voorbeeld wordt een resourcegroep met de naam *myRGDualStack* in de *eastus* locatie:

```azurecli
az group create \
--name DsResourceGroup01 \
--location eastus
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses-for-load-balancer"></a>IPv4 en IPv6-openbare IP-adressen voor load balancer maken
Voor toegang tot uw IPv4 en IPv6-eindpunten op het Internet, moet u IPv4 en IPv6-openbare IP-adressen voor de load balancer. Maak een openbaar IP-adres met [az network public-ip create](/cli/azure/network/public-ip). Het volgende voorbeeld wordt IPv4 en IPv6-openbare IP-adres met de naam *dsPublicIP_v4* en *dsPublicIP_v6* in de *myRGDualStack* resourcegroep:

```azurecli
# Create an IPV4 IP address
az network public-ip create \
--name dsPublicIP_v4  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4

# Create an IPV6 IP address
az network public-ip create \
--name dsPublicIP_v6  \
--resource-group DsResourceGroup01  \
--location eastus \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv6

```

## <a name="create-public-ip-addresses-for-vms"></a>Openbare IP-adressen maken voor virtuele machines

Voor externe toegang tot uw virtuele machines op het internet, moet u IPv4 openbare IP-adressen voor de virtuele machines. Maak een openbaar IP-adres met [az network public-ip create](/cli/azure/network/public-ip).

```azurecli
az network public-ip create \
--name dsVM0_remote_access  \
--resource-group DsResourceGroup01 \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4

az network public-ip create \
--name dsVM1_remote_access  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4
```

## <a name="create-basic-load-balancer"></a>Load balancer van het type Basic maken

In deze sectie maakt u twee front-end IP-adres (IPv4 en IPv6) en de back-end-adresgroep voor de load balancer configureren en maak vervolgens een Basic Load Balancer.

### <a name="create-load-balancer"></a>Load balancer maken

Maak de Basic Load Balancer met [az network lb maken](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) met de naam **dsLB** die een front-endgroep met de naam bevat **dsLbFrontEnd_v4**, een back endadresgroep met de naam  **dsLbBackEndPool_v4** dat is gekoppeld aan het openbare IP-adres van IPv4 **dsPublicIP_v4** die u in de vorige stap hebt gemaakt. 

```azurecli
az network lb create \
--name dsLB  \
--resource-group DsResourceGroup01 \
--sku Basic \
--location eastus \
--frontend-ip-name dsLbFrontEnd_v4  \
--public-ip-address dsPublicIP_v4  \
--backend-pool-name dsLbBackEndPool_v4
```

### <a name="create-ipv6-frontend"></a>IPv6-frontend maken

Maken van een IPv6-frontend-IP met [az network lb frontend-ip maken](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create). Het volgende voorbeeld wordt een front-end-IP-configuratie met de naam *dsLbFrontEnd_v6* en koppelt u de *dsPublicIP_v6* adres:

```azurepowershell-interactive
az network lb frontend-ip create \
--lb-name dsLB  \
--name dsLbFrontEnd_v6  \
--resource-group DsResourceGroup01  \
--public-ip-address dsPublicIP_v6

```

### <a name="configure-ipv6-back-end-address-pool"></a>IPv6-back-end-adresgroep configureren

Maken van een IPv6-adres voor back-end-pools met [az network lb address-pool maken](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create). Het volgende voorbeeld wordt een back-end-adrespool met de naam *dsLbBackEndPool_v6* om op te nemen van virtuele machines met IPv6-NIC-configuraties:

```azurecli
az network lb address-pool create \
--lb-name dsLB  \
--name dsLbBackEndPool_v6  \
--resource-group DsResourceGroup01
```

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. 

Gebruik [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) om een load balancer-regel te maken. Het volgende voorbeeld wordt de load balancer-regels met de naam *dsLBrule_v4* en *dsLBrule_v6* waarmee het verkeer op *TCP* poort *80* naar de IPv4 en IPv6 front-end IP-configuraties:

```azurecli
az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v4  \
--resource-group DsResourceGroup01  \
--frontend-ip-name dsLbFrontEnd_v4  \
--protocol Tcp  \
--frontend-port 80  \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v4


az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v6  \
--resource-group DsResourceGroup01 \
--frontend-ip-name dsLbFrontEnd_v6  \
--protocol Tcp  \
--frontend-port 80 \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v6

```

## <a name="create-network-resources"></a>Netwerkbronnen maken
Voordat u enkele VM's implementeert, moet u ondersteunende netwerkbronnen - beschikbaarheidsset, netwerkbeveiligingsgroep, virtuele netwerken en virtuele NIC's. 
### <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken
Ter verbetering van de beschikbaarheid van uw app in uw virtuele machines in een beschikbaarheidsset te plaatsen.

Maak een beschikbaarheidsset met behulp van [az vm availability-set create](https://docs.microsoft.com/cli/azure/vm/availability-set?view=azure-cli-latest). Het volgende voorbeeld wordt een beschikbaarheidsset met de naam *dsAVset*:

```azurecli
az vm availability-set create \
--name dsAVset  \
--resource-group DsResourceGroup01  \
--location eastus \
--platform-fault-domain-count 2  \
--platform-update-domain-count 2  
```

### <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken

Maak een netwerkbeveiligingsgroep voor de regels die binnenkomende en uitgaande communicatie in uw VNET bepalen.

#### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maak een netwerkbeveiligingsgroep met [az network nsg maken](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create)


```azurecli
az network nsg create \
--name dsNSG1  \
--resource-group DsResourceGroup01  \
--location eastus

```

#### <a name="create-a-network-security-group-rule-for-inbound-and-outbound-connections"></a>Een regel voor de netwerkbeveiligingsgroep voor binnenkomende en uitgaande verbindingen maken

Maak een regel voor netwerkbeveiligingsgroep zodat RDP-verbindingen via poort 3389, internet-verbinding via poort 80, en voor uitgaande verbindingen met [az network nsg-regel maken](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create).

```azurecli
# Create inbound rule for port 3389
az network nsg rule create \
--name allowRdpIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 100  \
--description "Allow Remote Desktop In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges 3389

# Create inbound rule for port 80
az network nsg rule create \
--name allowHTTPIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 200  \
--description "Allow HTTP In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges 80  \
--destination-address-prefixes "*"  \
--destination-port-ranges 80

# Create outbound rule

az network nsg rule create \
--name allowAllOut  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 300  \
--description "Allow All Out"  \
--access Allow  \
--protocol "*"  \
--direction Outbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges "*"
```


### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *dsVNET* met subnetten *dsSubNET_v4* en *dsSubNET_v6*:

```azurecli
# Create the virtual network
az network vnet create \
--name dsVNET \
--resource-group DsResourceGroup01 \
--location eastus  \
--address-prefixes "10.0.0.0/16" "ace:cab:deca::/48"

# Create a single dual stack subnet

az network vnet subnet create \
--name dsSubNET \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--address-prefixes "10.0.0.0/24" "ace:cab:deca:deed::/64" \
--network-security-group dsNSG1
```

### <a name="create-nics"></a>NIC's maken

Virtuele NIC's maken voor elke virtuele machine met [az network nic maken](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create). Het volgende voorbeeld wordt een virtuele NIC voor elke virtuele machine. Elke NIC heeft twee IP-configuraties (1 IPv4-configuratie, 1 IPv6-configuratie). Maken van de IPv6-configuratie met [az network nic ip-config maken](https://docs.microsoft.com/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-create).
 
```azurecli
# Create NICs
az network nic create \
--name dsNIC0  \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1  \
--vnet-name dsVNET  \
--subnet dsSubNet  \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4  \
--lb-name dsLB  \
--public-ip-address dsVM0_remote_access

az network nic create \
--name dsNIC1 \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4 \
--lb-name dsLB \
--public-ip-address dsVM1_remote_access

# Create IPV6 configurations for each NIC

az network nic ip-config create \
--name dsIp6Config_NIC0  \
--nic-name dsNIC0  \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name dsNIC1 \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB
```

### <a name="create-virtual-machines"></a>Virtuele machines maken

Maken van de virtuele machines met [az vm maken](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create). In het volgende voorbeeld worden twee VM's en de vereiste onderdelen van het virtuele netwerk gemaakt als deze nog niet bestaan. 

Virtuele machine maken *dsVM0* als volgt:

```azurecli
 az vm create \
--name dsVM0 \
--resource-group DsResourceGroup01 \
--nics dsNIC0 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest  
```
Virtuele machine maken *dsVM1* als volgt:

```azurecli
az vm create \
--name dsVM1 \
--resource-group DsResourceGroup01 \
--nics dsNIC1 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest 
```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>IPv6 dual-stack virtueel netwerk in Azure portal bekijken
U kunt het virtuele netwerk van IPv6 dual-stack in Azure-portal als volgt bekijken:
1. Voer in de zoekbalk van de portal, *dsVnet*.
2. Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteert u dit. Dit start de **overzicht** pagina van het virtuele netwerk met de naam van dual-stack *dsVnet*. Het virtuele netwerk van dual-stack ziet u de twee NIC's met zowel IPv4 als IPv6-configuraties die zich in de dual stack-subnet met de naam *dsSubnet*.

  ![IPv6 dual-stack virtueel netwerk in Azure](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

> [!NOTE]
> De IPv6 voor Azure-netwerk is beschikbaar in Azure portal in alleen-lezen voor deze preview-versie.


## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az-group-delete) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurecli
 az group delete --name DsRG1
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel, kunt u een Basic Load Balancer gemaakt met een dubbele front-end-IP-configuratie (IPv4 en IPv6). Hebt u ook een twee virtuele machines die opgenomen NIC's met dubbele IP-configuraties (IPV4 + IPv6) die zijn toegevoegd aan de back-endpool van de load balancer gemaakt. Zie voor meer informatie over IPv6-ondersteuning in virtuele netwerken van Azure, [wat IPv6 voor Azure Virtual Network is?](ipv6-overview.md)
