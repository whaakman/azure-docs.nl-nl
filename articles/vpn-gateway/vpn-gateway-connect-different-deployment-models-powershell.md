---
title: 'Klassieke virtuele netwerken en Azure Resource Manager VNets verbinden: PowerShell | Microsoft Docs'
description: Maak een VPN-verbinding tussen het klassieke vnet's en Resource Manager VNets met VPN-Gateway en PowerShell.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/17/2018
ms.author: cherylmc
ms.openlocfilehash: cf7726d017afd579b1eb227ec0fd3b9710395de6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58082259"
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Virtuele netwerken van verschillende implementatiemodellen verbinden met PowerShell

Dit artikel helpt u klassieke en Resource Manager VNets verbinden zodat de resources die zich in afzonderlijke implementatiemodellen om te communiceren met elkaar verbinden. De stappen in dit artikel wordt PowerShell gebruikt, maar u kunt ook deze configuratie met behulp van de Azure-portal door te selecteren van het artikel in deze lijst maken.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Een klassieke VNet verbinden met een Resource Manager-VNet is vergelijkbaar met het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. U kunt een verbinding tussen VNets die zich in verschillende abonnementen en in verschillende regio's maken. U kunt ook verbinding maken vnet's die al verbindingen met on-premises netwerken zijn, zolang als de gateway die ze zijn geconfigureerd met dynamische of op basis van route. Zie voor meer informatie over verbindingen tussen VNets de [Veelgestelde vragen over VNet-naar-VNet](#faq) aan het einde van dit artikel. 

Als u nog geen een virtuele netwerkgateway en niet wilt maken, kunt u in plaats daarvan daarmee verbinding maken met uw vnet's met behulp van VNet-Peering. Bij VNet-peering wordt geen VPN-gateway gebruikt. Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie.

## <a name="before"></a>Voordat u begint

De volgende stappen helpen u bij de instellingen die nodig zijn voor het configureren van een dynamisch of een op route gebaseerde gateway voor elk VNet en een VPN-verbinding tussen de gateways maken. Deze configuratie biedt geen ondersteuning voor statische of beleid gebaseerde gateways.

### <a name="pre"></a>Vereisten

* Beide VNets zijn al gemaakt. Als u maken van een virtueel netwerk van resource manager wilt, raadpleegt u [maken van een resourcegroep en een virtueel netwerk](../virtual-network/quick-create-powershell.md#create-a-resource-group-and-a-virtual-network). Zie voor het maken van een klassiek virtueel netwerk, [maken van een klassiek VNet](https://docs.microsoft.com/azure/virtual-network/create-virtual-network-classic).
* De adresbereiken voor de VNets niet met elkaar overlappen of overlapt met een van de bereiken voor andere verbindingen die de gateways kunnen worden verbonden met.
* U kunt de meest recente PowerShell-cmdlets hebben geïnstalleerd. Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. Zorg ervoor dat u zowel de SM (Service Management) en de Resource Manager (DB)-cmdlets installeren. 

### <a name="exampleref"></a>Voorbeeldinstellingen

U kunt deze waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel.

**Klassieke VNet-instellingen**

VNet-naam = ClassicVNet <br>
Locatie VS-West = <br>
Virtual Network-adresruimten 10.0.0.0/24 = <br>
Subnet-1 = 10.0.0.0/27 <br>
GatewaySubnet 10.0.0.32/29 = <br>
Naam van lokale RMVNetLocal = <br>
GatewayType = DynamicRouting

**Resource Manager-VNet-instellingen**

VNet-naam = RMVNet <br>
Resourcegroep RG1 = <br>
Virtueel netwerk-IP-adresruimten = 192.168.0.0/16 <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Locatie VS-Oost = <br>
Gatewaynaam van openbare IP-gwpip = <br>
Lokale netwerkgateway ClassicVNetLocal = <br>
Naam van de virtuele netwerkgateway RMGateway = <br>
Gateway-IP-adresconfiguratie gwipconfig =

## <a name="createsmgw"></a>Sectie 1: de klassieke VNet configureren
### <a name="1-download-your-network-configuration-file"></a>1. Download het bestand van de configuratie van netwerk
1. Meld u aan bij uw Azure-account in de PowerShell-console met verhoogde rechten. De volgende cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell. De klassieke SM (Service Management) Azure PowerShell-cmdlets worden gebruikt in deze sectie.

   ```azurepowershell
   Add-AzureAccount
   ```

   Uw Azure-abonnement ophalen.

   ```azurepowershell
   Get-AzureSubscription
   ```

   Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken.

   ```azurepowershell
   Select-AzureSubscription -SubscriptionName "Name of subscription"
   ```
2. Uw Azure-netwerk-configuratiebestand exporteren met de volgende opdracht. U kunt de locatie van het bestand om te exporteren naar een andere locatie indien nodig wijzigen.

   ```azurepowershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
3. Open het .xml-bestand dat u hebt gedownload om het te bewerken. Zie voor een voorbeeld van het netwerkconfiguratiebestand, de [Network-configuratieschema](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="2-verify-the-gateway-subnet"></a>2. Controleer of het gatewaysubnet
In de **VirtualNetworkSites** -element een gatewaysubnet toevoegen aan uw VNet, als er niet al is gemaakt. Als u werkt met het netwerkconfiguratiebestand, het gatewaysubnet moet de naam 'GatewaySubnet' of Azure kan niet worden herkend en deze gebruiken als een gateway-subnet.

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
De lokale netwerksite die u toevoegt, geeft de RM-VNet waarmee u verbinding wilt maken. Voeg een **LocalNetworkSites** element naar het bestand als een nog niet bestaat. Op dit moment kan de VPNGatewayAddress in de configuratie zijn van een geldig openbaar IP-adres omdat we nog de gateway voor het Resource Manager VNet nog niet hebt gemaakt. Zodra we de gateway hebt gemaakt, vervang we deze tijdelijke aanduiding voor IP-adres door het juiste openbare IP-adres dat is toegewezen aan de RM-gateway.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="4-associate-the-vnet-with-the-local-network-site"></a>4. Het VNet koppelen aan de lokale netwerksite
In deze sectie geven we de lokale netwerksite die u wilt verbinding maken met de VNet-naar. In dit geval is het Resource Manager VNet die u eerder hebt verwezen. Zorg ervoor dat de namen overeenkomen. Deze stap maakt geen een gateway. Het geeft het lokale netwerk waarmee de gateway verbinding te maken.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="5-save-the-file-and-upload"></a>5. Sla het bestand en upload
Sla het bestand vervolgens importeren naar Azure met de volgende opdracht. Zorg ervoor dat u het bestandspad zo nodig voor uw omgeving wijzigen.

```azurepowershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Hier ziet u een soortgelijk resultaat weergegeven dat het importeren is voltooid.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="6-create-the-gateway"></a>6. De gateway maken

Raadpleeg voordat u het volgende voorbeeld uitvoert, het netwerkconfiguratiebestand dat u hebt gedownload voor de exacte namen die Azure verwacht om te zien. Het netwerkconfiguratiebestand bevat de waarden voor uw klassieke virtuele netwerken. Soms worden de namen voor klassieke VNets zijn gewijzigd in het netwerkconfiguratiebestand bij het maken van klassieke VNet-instellingen in Azure portal vanwege de verschillen in de implementatiemodellen. Bijvoorbeeld, als u de Azure-portal gebruikt om te maken van een klassiek VNet met de naam 'Klassieke VNet' en deze in een resourcegroep met de naam 'ClassicRG' hebt gemaakt, wordt de naam die is opgenomen in het netwerkconfiguratiebestand geconverteerd naar groep klassieke VNet ClassicRG. Bij het opgeven van de naam van een VNet die spaties bevat, aanhalingstekens rond de waarde gebruiken.


Gebruik het volgende voorbeeld om te maken van een gateway voor dynamische routering:

```azurepowershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

U kunt de status van de gateway controleren met behulp van de **Get-AzureVNetGateway** cmdlet.

## <a name="creatermgw"></a>Sectie 2: de RM-VNet-gateway configureren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De vereisten wordt ervan uitgegaan dat u al hebt gemaakt een RM-VNet. In deze stap maakt maken u een VPN-gateway voor de RM-VNet. Deze stappen totdat niet worden gestart nadat u het openbare IP-adres voor het klassieke VNet-gateway hebt opgehaald. 

1. Aanmelden bij uw Azure-account in de PowerShell-console. De volgende cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, worden instellingen van uw account gedownload zodat ze beschikbaar voor Azure PowerShell zijn. U kunt eventueel de functie 'Try It' Azure Cloud Shell in de browser te starten.

   Als u Azure Cloud Shell gebruikt, gaat u verder met de volgende cmdlet:

   ```azurepowershell
   Connect-AzAccount
   ``` 
   Om te controleren dat u van het juiste abonnement gebruikmaakt, moet u de volgende cmdlet uitvoeren:  

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
   
   Als u meer dan één abonnement hebt, geeft u het abonnement dat u wilt gebruiken.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```
2. Maak een lokale netwerkgateway. In een virtueel netwerk verwijst de lokale netwerkgateway doorgaans naar uw on-premises locatie. In dit geval wordt de lokale netwerkgateway verwijst naar uw klassieke VNet. Geef deze een naam waarmee Azure hiernaar wordt verwezen, en ook het adresruimtevoorvoegsel opgeven. Azure gebruikt het IP-adresvoorvoegsel dat u opgeeft om te bepalen welk verkeer naar uw on-premises locatie moet worden verzonden. Als u de informatie hier later aanpassen moet voordat u de gateway maakt kunt u de waarden wijzigen en voert u het voorbeeld opnieuw uit.
   
   **-Naam** is de naam die u wilt toewijzen om te verwijzen naar de lokale netwerkgateway.<br>
   **-AddressPrefix** is de adresruimte voor uw klassieke VNet.<br>
   **-GatewayIpAddress** is het openbare IP-adres van het klassieke VNet-gateway. Zorg ervoor dat de volgende voorbeeldtekst "n.n.n.n" in overeenstemming met het juiste IP-adres.<br>

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name ClassicVNetLocal `
   -Location "West US" -AddressPrefix "10.0.0.0/24" `
   -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
   ```
3. Vraag een openbaar IP-adres worden toegewezen aan de virtuele netwerkgateway voor het Resource Manager-VNet. U kunt het IP-adres dat u wilt gebruiken niet zelf opgeven. Het IP-adres wordt dynamisch toegewezen aan de virtuele netwerkgateway. Dit betekent echter niet dat het IP-adres verandert. De enige keer dat het virtuele netwerk gateway IP-adres verandert is wanneer de gateway wordt verwijderd en opnieuw gemaakt. Het verandert vergroten of verkleinen, opnieuw in te stellen, of wanneer andere interne onderhoud of upgrades worden uitgevoerd van de gateway niet.

   In deze stap wordt ook een variabele die wordt gebruikt in een latere stap instellen.

   ```azurepowershell-interactive
   $ipaddress = New-AzPublicIpAddress -Name gwpip `
   -ResourceGroupName RG1 -Location 'EastUS' `
   -AllocationMethod Dynamic
   ```

4. Controleer of het virtuele netwerk heeft een gateway-subnet. Als er geen gatewaysubnet bestaat, Voeg een. Zorg ervoor dat het gatewaysubnet heet *GatewaySubnet*.
5. Het subnet dat wordt gebruikt voor de gateway met de volgende opdracht worden opgehaald. In deze stap wordt ook een variabele moet worden gebruikt in de volgende stap instellen.
   
   **-Naam** is de naam van uw Resource Manager-VNet.<br>
   **-ResourceGroupName** is de resourcegroep die het VNet is gekoppeld. Het gatewaysubnet moet al bestaan voor dit VNet en moet de naam *GatewaySubnet* goed te laten werken.<br>

   ```azurepowershell-interactive
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet `
   -VirtualNetwork (Get-AzVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
   ``` 

6. De configuratie van IP-adressering voor de gateway maken. De gatewayconfiguratie bepaalt welk subnet en openbaar IP-adres moeten worden gebruikt. Gebruik het volgende voorbeeld om de gatewayconfiguratie te maken.

   In deze stap is het **- SubnetId** en **- PublicIpAddressId** parameters moeten worden doorgegeven in de eigenschap id van het subnet en IP-adres-objecten, respectievelijk. U kunt een eenvoudige tekenreeks niet gebruiken. Deze variabelen worden ingesteld in de stap om aan te vragen van een openbaar IP-adres en de stap op te halen van het subnet.

   ```azurepowershell-interactive
   $gwipconfig = New-AzVirtualNetworkGatewayIpConfig `
   -Name gwipconfig -SubnetId $subnet.id `
   -PublicIpAddressId $ipaddress.id
   ```
7. Maak de virtuele netwerkgateway van Resource Manager met de volgende opdracht. De `-VpnType` moet *RouteBased*. Het kan duren voordat 45 minuten of langer voordat de gateway is gemaakt.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
   -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
   -IpConfigurations $gwipconfig `
   -EnableBgp $false -VpnType RouteBased
   ```
8. Kopieer het openbare IP-adres wanneer de VPN-gateway is gemaakt. U gebruikt deze bij het configureren van de lokale instellingen voor uw klassieke VNet. U kunt de volgende cmdlet gebruiken om op te halen van het openbare IP-adres. Het openbare IP-adres wordt vermeld in het rendement als *IpAddress*.

   ```azurepowershell-interactive
   Get-AzPublicIpAddress -Name gwpip -ResourceGroupName RG1
   ```

## <a name="localsite"></a>Sectie 3: de klassieke VNet lokale site-instellingen wijzigen

In deze sectie maakt werken u met het klassieke VNet. U kunt de tijdelijke aanduiding voor IP-adres dat u hebt gebruikt bij het opgeven van de lokale site-instellingen die wordt gebruikt om verbinding maken met de Resource Manager-VNet-gateway vervangen. Omdat u met het klassieke VNet werkt, moet u PowerShell lokaal is geïnstalleerd op uw computer, niet de Azure Cloud Shell TryIt gebruiken.

1. Het netwerkconfiguratiebestand exporteert.

   ```azurepowershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
2. Met een teksteditor, wijzig de waarde voor VPNGatewayAddress. Vervang de tijdelijke aanduiding voor IP-adres door het openbare IP-adres van de Resource Manager-gateway en sla de wijzigingen op.

   ```
   <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
   ```
3. Importeer het gewijzigde netwerkconfiguratiebestand naar Azure.

   ```azurepowershell
   Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
   ```

## <a name="connect"></a>Sectie 4: Maak een verbinding tussen de gateways
Het maken van een verbinding tussen de gateways is PowerShell vereist. Mogelijk moet u uw Azure-Account voor het gebruik van de klassieke versie van de PowerShell-cmdlets toevoegen. Om dit te doen, gebruik **Add-AzureAccount**.

1. Stel uw gedeelde sleutel in de PowerShell-console. Raadpleeg voordat u de cmdlets uitvoert, het netwerkconfiguratiebestand dat u hebt gedownload voor de exacte namen die Azure verwacht om te zien. Bij het opgeven van de naam van een VNet die spaties bevat, gebruikt u enkele aanhalingstekens rond de waarde.<br><br>In het volgende voorbeeld **- VNetName** is de naam van het klassieke VNet en **- LocalNetworkSiteName** is de naam die u hebt opgegeven voor de lokale netwerksite. De **- SharedKey** is een waarde die u genereert en opgeven. In het voorbeeld hebben we 'abc123' gebruikt, maar u kunt genereren en ingewikkeldere gebruiken. Het belangrijkste is dat de waarde die u hier opgeeft, dezelfde waarde die u in de volgende stap opgeeft bij het maken van de verbinding moet zijn. Moet worden weergegeven in het rendement **Status: Geslaagd**.

   ```azurepowershell
   Set-AzureVNetGatewayKey -VNetName ClassicVNet `
   -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
   ```
2. Maak de VPN-verbinding door het uitvoeren van de volgende opdrachten:
   
   Stel de variabelen in.

   ```azurepowershell-interactive
   $vnet01gateway = Get-AzLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
   $vnet02gateway = Get-AzVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
   ```
   
   Maak de verbinding. U ziet dat de **- ConnectionType** IPsec, niet Vnet2Vnet is.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
   -Location "East US" -VirtualNetworkGateway1 `
   $vnet02gateway -LocalNetworkGateway2 `
   $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```

## <a name="verify"></a>Sectie 5: Controleer uw verbindingen

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Om te controleren of de verbinding van uw klassieke VNet naar uw Resource Manager VNet

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Om te controleren of de verbinding van uw Resource Manager VNet naar uw klassieke VNet

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
