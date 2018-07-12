---
title: 'Klassieke virtuele netwerken en Azure Resource Manager VNets verbinden: Portal | Microsoft Docs'
description: Stappen voor het klassieke en Resource Manager VNets met VPN-Gateway en de portal verbinden
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/13/2018
ms.author: cherylmc
ms.openlocfilehash: 5e51027455da1f8be34d99c79bc79bc37df57d14
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38721552"
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Verbinding maken met virtuele netwerken van verschillende implementatiemodellen met behulp van de portal

In dit artikel wordt beschreven hoe u klassieke VNets verbinden met Resource Manager VNets verbinden zodat de resources die zich in afzonderlijke implementatiemodellen met elkaar communiceren. De stappen in dit artikel voornamelijk gebruik van Azure portal, maar u kunt ook deze configuratie met behulp van PowerShell door het selecteren van het artikel in deze lijst maken.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Een klassieke VNet verbinden met een Resource Manager-VNet is vergelijkbaar met het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. U kunt een verbinding tussen VNets die zich in verschillende abonnementen en in verschillende regio's maken. U kunt ook verbinding maken vnet's die al verbindingen met on-premises netwerken zijn, zolang als de gateway die ze zijn geconfigureerd met dynamische of op basis van route. Zie voor meer informatie over verbindingen tussen VNets de [Veelgestelde vragen over VNet-naar-VNet](#faq) aan het einde van dit artikel. 

Als u nog geen een virtuele netwerkgateway en niet wilt maken, kunt u in plaats daarvan daarmee verbinding maken met uw vnet's met behulp van VNet-Peering. Bij VNet-peering wordt geen VPN-gateway gebruikt. Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie.

### <a name="before"></a>Voordat u begint

* Deze stappen wordt ervan uitgegaan dat beide Vnetten al zijn gemaakt. Als u in dit artikel bij wijze van oefening en geen vnet's hebt, moet u er koppelingen in de stappen voor het maken van deze zijn.
* Controleer of dat de adresbereiken voor de VNets niet met elkaar overlappen, of met een van de bereiken voor andere verbindingen die de gateways kunnen worden verbonden overlapt met.
* Installeer de meest recente PowerShell-cmdlets voor Resource Manager en Service Management (klassiek). In dit artikel gebruiken we Azure portal en PowerShell. PowerShell is vereist voor het verbinding maken tussen het klassieke VNet en het Resource Manager-VNet. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. 

### <a name="values"></a>Voorbeeldinstellingen

U kunt deze waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel.

**Klassieke VNet**

VNet-naam = ClassicVNet <br>
Adresruimte = 10.0.0.0/24 <br>
Subnetnaam Subnet-1 = <br>
Subnetadresbereik 10.0.0.0/27 = <br>
Abonnement = van het abonnement dat u wilt gebruiken <br>
Resourcegroep ClassicRG = <br>
Locatie VS-West = <br>
GatewaySubnet 10.0.0.32/28 = <br>
Lokale site RMVNetLocal = <br>

**Resource Manager VNet**

VNet-naam = RMVNet <br>
Adresruimte = 192.168.0.0/16 <br>
Resourcegroep RG1 = <br>
Locatie VS-Oost = <br>
Subnetnaam Subnet-1 = <br>
Adresbereik = 192.168.1.0/24 <br>
GatewaySubnet 192.168.0.0/26 = <br>
Gatewaynaam van het virtuele netwerk RMGateway = <br>
Gatewaytype VPN = <br>
VPN-type = op Route gebaseerd <br>
SKU VpnGw1 = <br>
Locatie VS-Oost = <br>
Virtueel netwerk RMVNet = <br> (de VPN-gateway in dit VNet koppelen) Eerste IP-configuratie rmgwpip = <br> (openbare IP-adres) Lokale netwerkgateway ClassicVNetLocal = <br>
Verbindingsnaam RMtoClassic =

### <a name="connectoverview"></a>Overzicht van verbindingen

Voor deze configuratie kunt u een VPN-gatewayverbinding maken via een IPsec/IKE VPN-tunnel tussen de virtuele netwerken. Zorg ervoor dat geen van de VNet-adresbereiken overlappen met elkaar of met een van de lokale netwerken waarmee ze verbinding maken.

De volgende tabel toont een voorbeeld van hoe het voorbeeld VNets en lokale sites worden gedefinieerd:

| Virtual Network | Adresruimte | Regio | Maakt verbinding met lokale netwerksite |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |VS - west | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |VS - oost |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>Sectie 1: de klassieke VNet-instellingen configureren

In deze sectie maakt u het klassieke VNet, het lokale netwerk (lokale site) en de virtuele netwerkgateway. De schermafbeeldingen dienen alleen als voorbeeld. Zorg ervoor dat u de waarden vervangt door uw eigen, of gebruik de [voorbeeld](#values) waarden.

### 1. <a name="classicvnet"></a>Een klassiek VNet maken

Als u niet beschikt over een klassieke VNet en deze stappen als oefening uitvoert, kunt u een VNet maken met behulp van [in dit artikel](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) en de [voorbeeld](#values) waarden voor de instellingen van boven.

Als u al een VNet met een VPN-gateway hebt, controleert u of dat de gateway dynamisch is. Als het statische, moet u de VPN-gateway eerst verwijderen voordat u naar doorgaat [configureren van de lokale site](#local).

1. Open de [Azure-portal](https://ms.portal.azure.com) en meld u aan met uw Azure-account.
2. Klik op **+ een resource maken** om de pagina 'Nieuw' te openen.
3. Typ in het veld 'Zoek in marketplace', 'Virtueelnetwerk'. Als u in plaats daarvan selecteert netwerk -> Virtueelnetwerk, krijgt u niet de optie voor het maken van een klassiek VNet.
4. Zoek 'Virtueelnetwerk' in de geretourneerde lijst en klikt u erop om de pagina Virtueelnetwerk te openen. 
5. Selecteer op de pagina virtueel netwerk 'Classic' te maken van een klassiek VNet. Als u hier de standaard, u wordt de afsluiting van met een Resource Manager VNet in plaats daarvan.

### 2. <a name="local"></a>De lokale site configureren

1. Navigeer naar **alle resources** en zoek de **ClassicVNet** in de lijst.
2. Op de **overzicht** pagina, in de **VPN-verbindingen** sectie, klikt u op **Gateway** om een gateway te maken.
  ![Een VPN-gateway configureren](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "een VPN-gateway configureren")
3. Op de **nieuwe VPN-verbinding** pagina voor **verbindingstype**, selecteer **Site-naar-site**.
4. Voor **lokale site**, klikt u op **vereiste instellingen configureren**. Hiermee opent u de **lokale site** pagina.
5. Op de **lokale site** pagina, maakt u een naam om te verwijzen naar het Resource Manager-VNet. Bijvoorbeeld: 'RMVNetLocal'.
6. Als de VPN-gateway voor het Resource Manager VNet al een openbaar IP-adres heeft, gebruikt de waarde voor de **IP-adres van VPN-gateway** veld. Als u deze stappen uitvoert als oefening, of een virtuele netwerkgateway voor uw Resource Manager VNet nog geen hebt, kunt u een tijdelijke aanduiding voor IP-adres. Zorg ervoor dat de tijdelijke aanduiding voor IP-adres maakt gebruik van een geldige indeling hebben. Later, vervangt u de tijdelijke aanduiding voor IP-adres door het openbare IP-adres van de virtuele netwerkgateway van Resource Manager.
7. Voor **Clientadresruimte**, gebruikt u de [waarden](#connectoverview) voor het virtuele netwerk-IP-adresruimten voor het Resource Manager-VNet. Deze instelling wordt gebruikt om op te geven van de adresruimten om te routeren naar het virtuele netwerk van Resource Manager. In het voorbeeld gebruiken we 192.168.0.0/16, het adresbereik van het RMVNet.
8. Klik op **OK** Sla de waarden op en gaat u terug naar de **nieuwe VPN-verbinding** pagina.

### <a name="classicgw"></a>3. De gateway van het virtuele netwerk maken

1. Op de **nieuwe VPN-verbinding** weergeeft, schakelt de **gateway onmiddellijk maken** selectievakje.
2. Klik op **Optionele gatewayconfiguratie** om de pagina **Gatewayconfiguratie** te openen.

  ![Open gateway-configuratiepagina](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "configuratiepagina Open-gateway")
3. Klik op **Subnet - vereiste instellingen configureren** openen de **subnet toevoegen** pagina. De **naam** is al geconfigureerd met de vereiste waarde: **GatewaySubnet**.
4. De **adresbereik** verwijst naar het bereik voor het gatewaysubnet. Hoewel u een gatewaysubnet met een/29 maken kunt-adresbereik (3-adressen), het is raadzaam een gatewaysubnet maken die meer IP-adressen bevat. Dit geschikt is voor toekomstige configuraties waarvoor meer beschikbare IP-adressen. Gebruik indien mogelijk /27 of/28. Als u deze stappen bij wijze van oefening, kunt u verwijzen naar de [voorbeeldwaarden](#values). Voor dit voorbeeld gebruiken we '10.0.0.32/28'. Klik op **OK** om het gatewaysubnet te maken.
5. Op de **gatewayconfiguratie** pagina **grootte** verwijst naar de gateway-SKU. Selecteer de gateway-SKU voor uw VPN-gateway.
6. Controleer of de **Routeringstype** is **dynamische**, klikt u vervolgens op **OK** om terug te keren naar de **nieuwe VPN-verbinding** pagina.
7. Op de **nieuwe VPN-verbinding** pagina, klikt u op **OK** om te beginnen met het maken van uw VPN-gateway. Het maken van een VPN-gateway kan tot 45 minuten duren.

### <a name="ip"></a>4. Kopieer de virtuele netwerkgateway openbaar IP-adres

Nadat de virtuele netwerkgateway is gemaakt, kunt u de IP-adres van de gateway bekijken. 

1. Navigeer naar uw klassieke VNet en klik op **overzicht**.
2. Klik op **VPN-verbindingen** om de VPN-verbindingen-pagina te openen. U kunt het openbare IP-adres weergeven op de pagina van de VPN-verbindingen. Dit is het openbare IP-adres toegewezen aan uw virtuele netwerkgateway. Noteer het IP-adres. U deze gebruiken in latere stappen wanneer u met uw configuratie-instellingen van Resource Manager LAN gateway werkt. 
3. U kunt de status van uw gatewayverbindingen weergeven. Let op de lokale netwerksite die u hebt gemaakt, wordt vermeld als 'Verbinding maken'. De status wordt gewijzigd nadat u uw verbindingen hebt gemaakt. U kunt deze pagina sluiten nadat u hebt de status weer te geven.

## <a name="rmvnet"></a>Sectie 2: de Resource Manager-VNet-instellingen configureren

In deze sectie maakt u een gateway van het virtuele netwerk en de lokale netwerkgateway maken voor uw Resource Manager-VNet. De schermafbeeldingen dienen alleen als voorbeeld. Zorg ervoor dat u de waarden vervangt door uw eigen, of gebruik de [voorbeeld](#values) waarden.

### <a name="1-create-a-virtual-network"></a>1. Een virtueel netwerk maken

**Voorbeelden van waarden:**

* VNet-naam = RMVNet <br>
* Adresruimte = 192.168.0.0/16 <br>
* Resourcegroep RG1 = <br>
* Locatie VS-Oost = <br>
* Subnetnaam Subnet-1 = <br>
* Adresbereik = 192.168.1.0/24 <br>


Als u een Resource Manager VNet niet hebt en deze stappen als oefening uitvoert, een virtueel netwerk maken met de stappen in [een virtueel netwerk maken](../virtual-network/quick-create-portal.md), met behulp van de voorbeelden van waarden.

### <a name="2-create-a-gateway-subnet"></a>2. Een gatewaysubnet maken

**Voorbeeldwaarde:** GatewaySubnet 192.168.0.0/26 =

Voordat u een virtuele netwerkgateway maakt, moet u eerst het gatewaysubnet maken. Een gatewaysubnet maken met CIDR-telling van/28 of groter (/ 27, / 26 etc.). Als u dit als onderdeel van oefening maakt, kunt u de voorbeelden van waarden.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="creategw"></a>3. De gateway van een virtueel netwerk maken

**Voorbeelden van waarden:**

* Gatewaynaam van het virtuele netwerk RMGateway = <br>
* Gatewaytype VPN = <br>
* VPN-type = op Route gebaseerd <br>
* SKU VpnGw1 = <br>
* Locatie VS-Oost = <br>
* Virtueel netwerk RMVNet = <br>
* Eerste IP-configuratie rmgwpip = <br>

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>4. Een lokale netwerkgateway maken

**Voorbeelden van waarden:** lokale netwerkgateway ClassicVNetLocal =

| Virtual Network | Adresruimte | Regio | Maakt verbinding met lokale netwerksite |Openbare IP-adres van gateway|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |VS - west | RMVNetLocal (192.168.0.0/16) |Het openbare IP-adres dat is toegewezen aan de gateway ClassicVNet|
| RMVNet | (192.168.0.0/16) |VS - oost |ClassicVNetLocal (10.0.0.0/24) |Het openbare IP-adres dat is toegewezen aan de gateway RMVNet.|

De lokale netwerkgateway geeft het adresbereik en het openbare IP-adres dat is gekoppeld aan uw klassieke VNet en de virtuele netwerkgateway. Als u deze stappen bij wijze van oefening doen, raadpleegt u de voorbeelden van waarden.

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>Sectie 3: de klassieke VNet lokale site-instellingen wijzigen

In deze sectie vervangt u de tijdelijke aanduiding voor IP-adres dat u hebt gebruikt bij de lokale site-instellingen met het IP-adres van de Resource Manager-VPN-gateway op te geven. In deze sectie maakt gebruik van de klassieke (SM) PowerShell-cmdlets.

1. Ga naar het klassieke virtuele netwerk in de Azure-portal.
2. Klik op de pagina voor het virtuele netwerk op **overzicht**.
3. In de **VPN-verbindingen** sectie, klikt u op de naam van uw lokale site in de afbeelding.

  ![VPN-verbindingen](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN-verbindingen")
4. Op de **Site-naar-site VPN-verbindingen** pagina, klikt u op de naam van de site.

  ![Naam van de site](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "naam lokale site")
5. Klik op de naam van de lokale site om te openen op de pagina voor uw lokale site, de **lokale site** pagina.

  ![Open-local-site](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "lokale site openen")
6. Op de **lokale site** pagina, vervangt de **IP-adres van VPN-gateway** met het IP-adres van de Resource Manager-gateway.

  ![Gateway-ip-adres](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "Gateway-IP-adres")
7. Klik op **OK** om bij te werken van het IP-adres.

## <a name="RMtoclassic"></a>Sectie 4: Resource Manager naar klassieke verbinding maken

In deze stap configureert u de verbinding van het Resource Manager VNet met het klassieke VNet met behulp van de Azure portal.

1. In **alle resources**, Ga naar de lokale netwerkgateway. In ons voorbeeld is het de lokale netwerkgateway **ClassicVNetLocal**.
2. Klik op **configuratie** en controleer of de waarde van de IP-adres de VPN-gateway voor het klassieke VNet. Indien nodig bijwerken en klik vervolgens op **opslaan**. Sluit de pagina.
3. In **alle resources**, klikt u op de lokale netwerkgateway.
4. Klik op **verbindingen** om de pagina verbindingen te openen.
5. Op de **verbindingen** pagina, klikt u op **+** om toe te voegen een verbinding.
6. Op de **verbinding toevoegen** pagina en de naam van de verbinding. Bijvoorbeeld: 'RMtoClassic'.
7. **Site-naar-Site** is al op deze pagina hebt geselecteerd.
8. Selecteer de virtuele netwerkgateway die u wilt koppelen aan deze site.
9. Maak een **gedeelde sleutel**. Deze sleutel wordt ook gebruikt in de verbinding die u van het klassieke VNet met het Resource Manager-VNet maken. U kunt de sleutel of er een bedenken. In ons voorbeeld gebruiken we 'abc123', maar u kunt gebruiken (aanbevolen) ingewikkeldere.
10. Klik op **OK** om de verbinding te maken.

## <a name="classictoRM"></a>Sectie 5: het klassieke model naar Resource Manager-verbinding maken

In deze stap configureert u de verbinding van het klassieke VNet naar het Resource Manager-VNet. Deze stappen is PowerShell vereist. U kunt deze verbinding in de portal maken. Zorg ervoor dat u hebt gedownload en geïnstalleerd zowel de klassieke (SM) als de Resource Manager (DB) PowerShell-cmdlets.

### <a name="1-connect-to-your-azure-account"></a>1. Verbinding maken met uw Azure-account

Open de PowerShell-console met verhoogde rechten en meld u aan bij uw Azure-account. Na het aanmelden, worden instellingen van uw account gedownload zodat ze beschikbaar voor Azure PowerShell zijn. De volgende cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-Account voor het Resource Manager-implementatiemodel:

```powershell
Connect-AzureRmAccount
```

Haal een lijst met uw Azure-abonnementen op.

```powershell
Get-AzureRmSubscription
```

Als u meer dan één abonnement hebt, geeft u het abonnement dat u wilt gebruiken.

```powershell
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```

Vervolgens Meld u aan bij de klassieke (Service Management) van de PowerShell-cmdlets gebruiken. Gebruik de volgende opdracht om toe te voegen van uw Azure-account voor het klassieke implementatiemodel:

```powershell
Add-AzureAccount
```

Haal een lijst van uw abonnementen. Deze stap kan nodig zijn bij het toevoegen van de Service Management-cmdlets, afhankelijk van uw Azure-module installeren.

```powershell
Get-AzureSubscription
```

Als u meer dan één abonnement hebt, geeft u het abonnement dat u wilt gebruiken.

```powershell
Select-AzureSubscription -SubscriptionName "Name of subscription"
```

### <a name="2-view-the-network-configuration-file-values"></a>2. Waarden voor het configuratiebestand netwerk weergeven

Wanneer u een VNet in Azure portal maakt, is de volledige naam die gebruikmaakt van Azure niet zichtbaar in de Azure portal. Een VNet dat wordt weergegeven om te worden met de naam 'ClassicVNet' in de Azure-portal kan bijvoorbeeld een veel langer naam hebben in het configuratiebestand van het netwerk. De naam ziet er ongeveer als volgt: 'Groep ClassicRG ClassicVNet'. In deze stappen die u kunt downloaden van het netwerkconfiguratiebestand en bekijk de waarden.

Maak een map op de computer en exporteer vervolgens het netwerkconfiguratiebestand naar de map. In dit voorbeeld wordt het netwerkconfiguratiebestand geëxporteerd naar C:\AzureNet.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Open het bestand met een teksteditor en de weergavenaam voor uw klassieke VNet. De namen in het netwerkconfiguratiebestand gebruiken bij het uitvoeren van uw PowerShell-cmdlets.

- VNet-namen worden weergegeven als **VirtualNetworkSite name =**
- Sitenamen worden weergegeven als **LocalNetworkSite name =**

### <a name="3-create-the-connection"></a>3. De verbinding maken

De gedeelde sleutel instellen en de verbinding maken tussen het klassieke VNet en het Resource Manager-VNet. U kunt de gedeelde sleutel met behulp van de portal niet instellen. Zorg ervoor dat u deze stappen terwijl u aangemeld met behulp van de klassieke versie van de PowerShell-cmdlets uitvoeren. Om dit te doen, gebruik **Add-AzureAccount**. Anders u pas weer om in te stellen de '-AzureVNetGatewayKey'.

- In dit voorbeeld **- VNetName** is de naam van het klassieke VNet zoals gevonden in het configuratiebestand van uw netwerk. 
- De **- LocalNetworkSiteName** is de naam die u hebt opgegeven voor de lokale site als gevonden in het configuratiebestand van uw netwerk.
- De **- SharedKey** is een waarde die u genereert en opgeven. In dit voorbeeld gebruikt we *abc123*, maar u kunt ingewikkeldere genereren. Het belangrijkste is dat de waarde die u hier opgeeft, dezelfde waarde die u hebt opgegeven bij het maken van uw Resource Manager naar klassieke verbinding moet zijn.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

##<a name="verify"></a>Sectie 6: de verbindingen controleren

U kunt uw verbindingen controleren met behulp van de Azure portal of PowerShell. Tijdens het controleren, moet u mogelijk Wacht een minuut of twee als de verbinding wordt gemaakt. Als een verbinding geslaagd is, verandert de status van de connectiviteit van 'Verbinding maken' in 'Verbonden'.

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Om te controleren of de verbinding van uw klassieke VNet naar uw Resource Manager VNet

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Om te controleren of de verbinding van uw Resource Manager VNet naar uw klassieke VNet

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
