---
title: 'Klassieke virtuele netwerken verbinden met Azure Resource Manager VNets: PowerShell | Microsoft Docs'
description: Informatie over het maken van een VPN-verbinding tussen het klassieke vnet's en Resource Manager-VNets met VPN-Gateway en PowerShell
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: da5bddba3a1fad74b2ee08fd2f34d1b01c7345c8
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Virtuele netwerken van verschillende implementatiemodellen verbinden met PowerShell



In dit artikel leest u hoe klassieke VNets op Resource Manager VNets om toe te staan van de resources die zich in de afzonderlijke implementatiemodellen om te communiceren met elkaar verbinden. De stappen in dit artikel PowerShell gebruiken, maar u kunt ook maken met deze configuratie met de Azure portal door te selecteren van het artikel in deze lijst.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Een klassiek VNet verbinding te maken met een Resource Manager VNet is vergelijkbaar met het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. U kunt een verbinding tussen VNets die tot verschillende abonnementen behoren en in verschillende regio's maken. U kunt ook VNets die u al verbindingen met on-premises netwerken hebt verbinden, zolang de gateway die is geconfigureerd met dynamische of op basis van route. Zie voor meer informatie over verbindingen tussen VNets de [Veelgestelde vragen over VNet-naar-VNet](#faq) aan het einde van dit artikel. 

Als uw vnet's zich in dezelfde regio, mogelijk wilt u in plaats daarvan deze met behulp van VNet-Peering te verbinden. Bij VNet-peering wordt geen VPN-gateway gebruikt. Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie. 

## <a name="before"></a>Voordat u begint

De volgende stappen maakt u de instellingen die nodig zijn voor het configureren van een dynamisch of op route gebaseerde gateway voor elk VNet en een VPN-verbinding tussen de gateways maken. Deze configuratie biedt geen ondersteuning voor statische of op beleid gebaseerde gateways.

### <a name="pre"></a>Vereisten

* Beide VNets zijn al gemaakt.
* De adresbereiken voor de VNets die niet met elkaar overlappen of overlapt met een van de bereiken voor de gateways mogen worden verbonden met andere verbindingen.
* U kunt de meest recente PowerShell-cmdlets hebt geïnstalleerd. Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview) voor meer informatie. Zorg ervoor dat u de Service Management (SM) en de Resource Manager (RM)-cmdlets installeren. 

### <a name="exampleref"></a>Voorbeeldinstellingen

U kunt deze waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel.

**Klassieke VNet-instellingen**

VNet-naam = ClassicVNet <br>
Locatie VS-West = <br>
Adresruimten voor virtueel netwerk 10.0.0.0/24 = <br>
Subnet-1 = 10.0.0.0/27 <br>
GatewaySubnet 10.0.0.32/29 = <br>
De naam van het lokale netwerk RMVNetLocal = <br>
GatewayType DynamicRouting =

**Resource Manager VNet-instellingen**

VNet-naam = RMVNet <br>
Resourcegroep RG1 = <br>
Virtueel netwerk-IP-adresruimten 192.168.0.0/16 = <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet 192.168.0.0/26 = <br>
Locatie VS-Oost = <br>
Het openbare IP-gatewaynaam gwpip = <br>
Lokale netwerkgateway ClassicVNetLocal = <br>
Naam van de virtuele netwerkgateway RMGateway = <br>
Gateway-IP-adresconfiguratie gwipconfig =

## <a name="createsmgw"></a>Sectie 1: de klassieke VNet configureren
### <a name="1-download-your-network-configuration-file"></a>1. Het configuratiebestand van het netwerk downloaden
1. Aanmelden bij uw Azure-account in de PowerShell-console met verhoogde rechten. De volgende cmdlet vraagt u om uw Azure-Account voor de aanmeldingsreferenties. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell. De SM PowerShell-cmdlets kunt u dit deel van de configuratie te voltooien.

  ```powershell
  Add-AzureAccount
  ```
2. Uw Azure-netwerk-configuratiebestand exporteren door de volgende opdracht uit te voeren. U kunt de locatie van het bestand om te exporteren naar een andere locatie indien nodig wijzigen.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. Open het .xml-bestand dat u hebt gedownload om het te bewerken. Zie voor een voorbeeld van het configuratiebestand van het netwerk, de [Network-configuratieschema](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="2-verify-the-gateway-subnet"></a>2. Controleer of het gatewaysubnet
In de **VirtualNetworkSites** element een gatewaysubnet toevoegen aan uw VNet als heeft geen al is gemaakt. Als u werkt met het configuratiebestand van het netwerk, het gatewaysubnet moet de naam 'GatewaySubnet' of Azure kan niet herkennen en gebruiken als een gatewaysubnet.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Voorbeeld:**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="3-add-the-local-network-site"></a>3. De lokale netwerksite toevoegen
De lokale netwerksite die u toevoegt, geeft de RM-VNet waarmee u verbinding wilt maken. Voeg een **LocalNetworkSites** element aan het bestand als een nog niet bestaat. Op dit moment in de configuratie mag de VPNGatewayAddress geldig openbaar IP-adres omdat we de gateway nog niet hebt gemaakt voor het Resource Manager VNet. Als we de gateway hebt gemaakt, vervangen we dit tijdelijke aanduiding voor IP-adres met de juiste openbare IP-adres dat is toegewezen aan de RM-gateway.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="4-associate-the-vnet-with-the-local-network-site"></a>4. Het VNet koppelen aan de lokale netwerksite op.
In deze sectie wordt de lokale netwerksite die u wilt verbinding maken met het VNet opgeven. In dit geval is het Resource Manager VNet dat u eerder naar verwezen. Zorg ervoor dat de namen overeenkomen. Deze stap maakt een gateway niet. Geeft aan van het lokale netwerk die de gateway maakt verbinding met.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="5-save-the-file-and-upload"></a>5. Sla het bestand en uploaden
Sla het bestand vervolgens importeren naar Azure door de volgende opdracht uit te voeren. Zorg ervoor dat u het bestandspad dat nodig is voor uw omgeving.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Hier ziet u een vergelijkbaar resultaat weergegeven dat het importeren is gelukt.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="6-create-the-gateway"></a>6. De gateway maken

Raadpleeg voordat u dit voorbeeld uitvoert, het netwerk configuratiebestand dat u hebt gedownload voor de exacte namen die Azure verwacht te zien. Het configuratiebestand van het netwerk bevat de waarden voor uw klassieke virtuele netwerken. Soms worden de namen voor klassieke vnet's zijn gewijzigd in het configuratiebestand van het netwerk bij het maken van klassieke VNet-instellingen in de Azure portal vanwege de verschillen in de implementatiemodellen. Als u de Azure-portal gebruikt voor het maken van een klassiek VNet met de naam 'Klassieke VNet' en het in een resourcegroep met de naam 'ClassicRG' hebt gemaakt, kunt u voor de naam die is opgenomen in het configuratiebestand van het netwerk, wordt geconverteerd naar groep klassieke VNet ClassicRG. Gebruik aanhalingstekens rond de waarde bij het opgeven van de naam van een VNet dat spaties bevat.


Gebruik het volgende voorbeeld voor het maken van een gateway voor dynamische routering:

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

U kunt de status van de gateway controleren met behulp van de **Get-AzureVNetGateway** cmdlet.

## <a name="creatermgw"></a>Sectie 2: de RM-VNet-gateway configureren
Volg de volgende instructies voor het maken van een VPN-gateway voor het VNet RM. De stappen totdat niet worden gestart nadat u het openbare IP-adres voor de gateway van het klassieke VNet hebt opgehaald. 

1. Aanmelden bij uw Azure-account in de PowerShell-console. De volgende cmdlet vraagt u om uw Azure-Account voor de aanmeldingsreferenties. Na het aanmelden, worden de instellingen van uw account worden gedownload zodat ze beschikbaar voor Azure PowerShell zijn.

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  Een lijst met uw Azure-abonnementen ophalen als u meer dan één abonnement hebt.

  ```powershell
  Get-AzureRmSubscription
  ```
   
  Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Maak een lokale netwerkgateway. In een virtueel netwerk verwijst de lokale netwerkgateway doorgaans naar uw on-premises locatie. In dit geval wordt verwijst de lokale netwerkgateway naar de klassieke VNet. Geef het een naam waarmee Azure kunt hiernaar wordt verwezen, en ook het adresruimtevoorvoegsel opgeven. Azure gebruikt het IP-adresvoorvoegsel dat u opgeeft om te bepalen welk verkeer naar uw on-premises locatie moet worden verzonden. Als u de informatie hier later aanpassen moet voordat u de gateway maakt kunt u de waarden wijzigen en het voorbeeld opnieuw uitvoeren.
   
   **-Naam** is de naam die u toewijzen wilt voor het verwijzen naar de lokale netwerkgateway.<br>
   **-AddressPrefix** is de adresruimte voor het klassieke VNet.<br>
   **-GatewayIpAddress** het openbare IP-adres van het klassieke VNet-gateway. Zorg ervoor dat het volgende voorbeeld om de juiste IP-adres weer te wijzigen.<br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. Een openbaar IP-adres aan worden toegewezen aan de virtuele netwerkgateway voor het Resource Manager VNet aanvragen. U kunt het IP-adres dat u wilt gebruiken niet zelf opgeven. Het IP-adres wordt dynamisch toegewezen aan de virtuele netwerkgateway. Dit betekent echter niet dat het IP-adres verandert. De enige keer dat de wijziging van virtueel netwerk gateway IP-adres is wanneer de gateway is verwijderd en opnieuw gemaakt. Wordt niet gewijzigd via vergroten of verkleinen, opnieuw wordt ingesteld, of een andere interne onderhoud of upgrades worden uitgevoerd van de gateway.

  In deze stap wordt ook een variabele die wordt gebruikt in een latere stap instellen.

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. Controleer of het virtuele netwerk een gatewaysubnet. Als er geen gatewaysubnet bestaat, Voeg een. Zorg ervoor dat het gatewaysubnet heet *GatewaySubnet*.
5. Ophalen van het subnet voor de gateway wordt gebruikt door de volgende opdracht uit te voeren. In deze stap wordt ook een variabele moet worden gebruikt in de volgende stap instellen.
   
   **-Naam** is de naam van uw VNet Resource Manager.<br>
   **-ResourceGroupName** is van de resourcegroep die het VNet is gekoppeld. Het gatewaysubnet moet al bestaan voor dit VNet en moet de naam *GatewaySubnet* goed te laten werken.<br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. De configuratie van IP-adressering voor de gateway maken. De gatewayconfiguratie bepaalt welk subnet en openbaar IP-adres moeten worden gebruikt. Gebruik het volgende voorbeeld om de gatewayconfiguratie te maken.

  In deze stap de **- SubnetId** en **- PublicIpAddressId** parameters moeten worden doorgegeven de eigenschap id van het subnet en IP-adres objecten, respectievelijk. U kunt een eenvoudige tekenreeks niet gebruiken. Deze variabelen worden ingesteld in de stap voor het aanvragen van een openbare IP-adres en de stap voor het ophalen van het subnet.

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. Maak de virtuele netwerkgateway van Resource Manager door de volgende opdracht uit te voeren. De `-VpnType` moet *RouteBased*. Het duurt 45 minuten of langer voor de gateway te maken.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. Kopieer het openbare IP-adres als de VPN-gateway is gemaakt. U gebruikt deze wanneer u de LAN-instellingen voor het klassieke VNet configureren. U kunt de volgende cmdlet gebruiken voor het ophalen van het openbare IP-adres. Het openbare IP-adres wordt vermeld in de geretourneerde als *IpAddress*.

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="localsite"></a>Sectie 3: de klassieke VNet lokale site-instellingen wijzigen

In deze sectie kunt samenwerken u met het klassieke VNet. U Vervang de tijdelijke aanduiding voor IP-adres dat u hebt gebruikt bij het opgeven van de lokale site-instellingen waarmee u verbinding maken met de Resource Manager VNet-gateway. 

1. Exporteer het configuratiebestand van het netwerk.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Met een teksteditor, wijzig de waarde voor VPNGatewayAddress. Vervang de tijdelijke aanduiding voor IP-adres met het openbare IP-adres van de Resource Manager-gateway en sla de wijzigingen.

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. Importeer het gewijzigde configuratie netwerkbestand naar Azure.

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <a name="connect"></a>Sectie 4: Maak een verbinding tussen de gateways
Maken van een verbinding tussen de gateways is PowerShell vereist. Mogelijk moet u uw Azure-Account voor het gebruik van de klassieke versie van de PowerShell-cmdlets toevoegen. Gebruiken om dit te doen **Add-AzureAccount**.

1. Stel uw gedeelde sleutel in de PowerShell-console. Raadpleeg voordat u de cmdlets uitvoert, het netwerk configuratiebestand dat u hebt gedownload voor de exacte namen die Azure verwacht te zien. Bij het opgeven van de naam van een VNet dat spaties bevat, gebruik de waarde op tussen aanhalingstekens.<br><br>In het volgende voorbeeld **- VNetName** is de naam van het klassieke VNet en **- LocalNetworkSiteName** is de naam die u hebt opgegeven voor de lokale netwerksite. De **- SharedKey** is een waarde die u genereren en opgeven. In het voorbeeld hebben we 'abc123' gebruikt, maar u kunt genereren en gebruikmaken van iets meer complexe. Het belangrijkste is dat de waarde die u hier opgeeft, dezelfde waarde die u in de volgende stap moet opgeeft bij het maken van de verbinding. Het retourtype moet worden weergegeven **Status: geslaagd**.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. De VPN-verbinding maken met de volgende opdrachten:
   
  Stel de variabelen in.

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  Maak de verbinding. U ziet dat de **- ConnectionType** IPsec, niet Vnet2Vnet is.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="verify"></a>Sectie 5: Controleer of uw verbindingen

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Om te controleren of de verbinding van uw klassieke VNet naar uw VNet Resource Manager

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Om te controleren of de verbinding van uw Resource Manager VNet met het klassieke VNet

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

