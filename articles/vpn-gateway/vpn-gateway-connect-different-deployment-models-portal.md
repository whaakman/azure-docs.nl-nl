---
title: 'Klassieke virtuele netwerken verbinden met Azure Resource Manager VNets: Portal | Microsoft Docs'
description: Informatie over het maken van een VPN-verbinding tussen het klassieke vnet's en Resource Manager-VNets met behulp van VPN-Gateway en de portal
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/23/2017
ms.author: cherylmc
ms.openlocfilehash: 2100b2b8710207ddb5d1848f11f4d6133f1dfd91
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Verbinding maken met virtuele netwerken vanuit verschillende implementatiemodellen via de portal

In dit artikel leest u hoe klassieke VNets op Resource Manager VNets om toe te staan van de resources die zich in de afzonderlijke implementatiemodellen om te communiceren met elkaar verbinden. De stappen in dit artikel voornamelijk gebruikt de Azure-portal, maar u kunt ook maken met deze configuratie met behulp van de PowerShell door het artikel in deze lijst te selecteren.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Een klassiek VNet verbinding te maken met een Resource Manager VNet is vergelijkbaar met het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. U kunt een verbinding tussen VNets die tot verschillende abonnementen behoren en in verschillende regio's maken. U kunt ook VNets die u al verbindingen met on-premises netwerken hebt verbinden, zolang de gateway die is geconfigureerd met dynamische of op basis van route. Zie voor meer informatie over verbindingen tussen VNets de [Veelgestelde vragen over VNet-naar-VNet](#faq) aan het einde van dit artikel. 

Als uw vnet's zich in dezelfde regio, mogelijk wilt u in plaats daarvan deze met behulp van VNet-Peering te verbinden. Bij VNet-peering wordt geen VPN-gateway gebruikt. Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie. 

### <a name="before"></a>Voordat u begint

* Deze stappen wordt ervan uitgegaan dat beide VNets al zijn gemaakt. Als u van dit artikel wijze van oefening maakt gebruikmaakt en geen VNets hebt, moet u er koppelingen zijn in de stappen waarmee u kunt ze maken.
* Controleer of de adresbereiken voor de VNets niet met elkaar overlappen, of overlapt met een van de bereiken voor andere verbindingen die de gateways kunnen worden aangesloten op.
* Installeer de meest recente PowerShell-cmdlets voor Resource Manager en Service Management (klassiek). In dit artikel gebruiken we de Azure portal- en PowerShell. PowerShell is vereist voor het maken van de verbinding van het klassieke VNet naar het Resource Manager VNet. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. 

### <a name="values"></a>Voorbeeldinstellingen

U kunt deze waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel.

**Klassieke VNet**

VNet-naam = ClassicVNet <br>
Adresruimte = 10.0.0.0/24 <br>
Subnet-1 = 10.0.0.0/27 <br>
Resourcegroep ClassicRG = <br>
Locatie VS-West = <br>
GatewaySubnet 10.0.0.32/28 = <br>
Lokale site RMVNetLocal = <br>

**Resource Manager VNet**

VNet-naam = RMVNet <br>
Adresruimte = 192.168.0.0/16 <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet 192.168.0.0/26 = <br>
Resourcegroep RG1 = <br>
Locatie VS-Oost = <br>
Naam van virtuele-netwerkgateway RMGateway = <br>
Gatewaytype = VPN <br>
VPN-type = op Route gebaseerd <br>
Naam van gateway openbare IP-adres rmgwpip = <br>
Lokale netwerkgateway ClassicVNetLocal = <br>
Verbindingsnaam RMtoClassic =

### <a name="connection-overview"></a>Overzicht van de verbinding

Voor deze configuratie kunt u een VPN-gatewayverbinding maken via een VPN-IPsec/IKE-tunnel tussen de virtuele netwerken. Zorg ervoor dat geen van uw VNet-bereiken overlappen elkaar of met een van de lokale netwerken waarmee ze verbinding maken.

De volgende tabel toont een voorbeeld van hoe de VNets voorbeeld en lokale sites worden gedefinieerd:

| Virtual Network | Adresruimte | Regio | Maakt verbinding met lokale netwerksite op. |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |VS - west | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |VS - oost |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>Sectie 1: de klassieke VNet-instellingen configureren

In deze sectie maakt u het lokale netwerk (lokale site) en de virtuele netwerkgateway voor het klassieke VNet. Als u niet een klassiek VNet hebt en deze stappen als oefening maakt uitvoert, kunt u een VNet maken met behulp van [in dit artikel](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) en de [voorbeeld](#values) waarden voor de instellingen van hierboven.

Wanneer u de portal een klassiek virtueel netwerk maken, moet u navigeren naar de pagina virtuele netwerk met behulp van de volgende stappen, anders is de optie voor het maken van een klassiek virtueel netwerk niet wordt weergegeven:

1. Klik op '+' op de pagina 'New' te openen.
2. Typ in het veld 'Search de marketplace', 'Virtueel netwerk'. Als u in plaats daarvan selecteert netwerk -> virtueel netwerk, krijgt u niet de optie voor het maken van een klassiek VNet.
3. Zoek 'Virtueel netwerk' in de geretourneerde lijst en klikt u erop om het virtuele netwerk pagina te openen. 
4. Selecteer op de pagina virtueel netwerk 'Classic' voor het maken van een klassiek VNet. 

Als u al een VNet met een VPN-gateway, controleert u of de gateway dynamisch. Als het statisch, moet u eerst de VPN-gateway verwijderen en vervolgens doorgaan.

De schermafbeeldingen dienen alleen als voorbeeld. Zorg ervoor dat de waarden vervangt door uw eigen of gebruik de [voorbeeld](#values) waarden.

### 1. <a name="local"></a>Configureren van de lokale site

Open de [Azure-portal](https://ms.portal.azure.com) en meld u aan met uw Azure-account.

1. Navigeer naar **alle resources** en zoek de **ClassicVNet** in de lijst.
2. Op de **overzicht** pagina in de **VPN-verbindingen** sectie, klikt u op de **Gateway** afbeelding voor het maken van een gateway.

    ![Een VPN-gateway configureren](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "een VPN-gateway configureren")
3. Op de **nieuwe VPN-verbinding** pagina voor **verbindingstype**, selecteer **Site-naar-site**.
4. Voor **lokale site**, klikt u op **vereiste instellingen configureren**. Hiermee opent u de **lokale site** pagina.
5. Op de **lokale site** pagina, maakt u een naam voor het verwijzen naar het Resource Manager VNet. Bijvoorbeeld 'RMVNetLocal'.
6. Als de VPN-gateway voor het Resource Manager VNet al een openbaar IP-adres heeft, gebruikt de waarde voor de **IP-adres van VPN-gateway** veld. Als u deze stappen uitvoert als oefening of een virtuele netwerkgateway voor uw Resource Manager VNet nog geen hebt, kunt u een tijdelijke aanduiding voor IP-adres. Zorg ervoor dat de tijdelijke aanduiding voor IP-adres een geldige indeling gebruikt. Later kunt vervangen u de tijdelijke aanduiding voor IP-adres door het openbare IP-adres van de virtuele netwerkgateway van Resource Manager.
7. Voor **Client adresruimte**, de waarden voor het virtuele netwerk-IP-adresruimten gebruiken voor het Resource Manager VNet. Deze instelling wordt gebruikt om op te geven van de adresruimten om naar het Resource Manager virtuele netwerk te routeren.
8. Klik op **OK** de waarden opslaan en terugkeren naar de **nieuwe VPN-verbinding** pagina.

### <a name="classicgw"></a>2. De gateway van het virtuele netwerk maken

1. Op de **nieuwe VPN-verbinding** pagina de **gateway onmiddellijk maken** selectievakje en klikt u op **optionele gatewayconfiguratie** openen de **Gateway configuratie** pagina. 

    ![Open gateway-configuratiepagina](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "configuratiepagina Open-gateway")
2. Klik op **Subnet - vereiste instellingen configureren** openen de **subnet toevoegen** pagina. De **naam** al is geconfigureerd met de vereiste waarde **GatewaySubnet**.
3. De **-adresbereik** verwijst naar het bereik voor het gatewaysubnet. Hoewel u een gatewaysubnet met een/29 maken kunt-adresbereik (3-adressen), wordt aangeraden een gatewaysubnet maken die meer IP-adressen bevat. Dit geschikt is voor toekomstige configuraties waarvoor meer beschikbare IP-adressen. Gebruik indien mogelijk, / 27 of /28. Als u deze stappen wijze van oefening maakt, kunt u verwijzen naar de [voorbeeld](#values) waarden. Klik op **OK** het gatewaysubnet maken.
4. Op de **gatewayconfiguratie** pagina **grootte** verwijst naar het gateway-SKU. Selecteer de gateway-SKU voor uw VPN-gateway.
5. Controleer of de **Type routering** is **dynamische**, klikt u vervolgens op **OK** om terug te keren naar de **nieuwe VPN-verbinding** pagina.
6. Op de **nieuwe VPN-verbinding** pagina, klikt u op **OK** om te beginnen met het maken van uw VPN-gateway. Maken van een VPN-gateway kan tot 45 minuten duren.

### <a name="ip"></a>3. Kopieer de virtuele netwerkgateway openbare IP-adres

Nadat de virtuele netwerkgateway is gemaakt, kunt u het IP-adres van de gateway weergeven. 

1. Navigeer naar de klassieke VNet en klik op **overzicht**.
2. Klik op **VPN-verbindingen** om de VPN-verbindingen pagina te openen. U kunt het openbare IP-adres weergeven op de pagina VPN-verbindingen. Dit is het openbare IP-adres toegewezen aan uw virtuele netwerkgateway. 
3. Noteer of kopieer het IP-adres. U gebruikt in latere stappen wanneer u met uw configuratie-instellingen van Resource Manager LAN gateway werkt. U kunt ook de status van uw gatewayverbindingen weergeven. Let op de lokale netwerksite die u hebt gemaakt, wordt vermeld als 'Verbinding maken'. De status wordt gewijzigd nadat u uw verbindingen hebt gemaakt.
4. Sluit de pagina na het kopiëren van de IP-adres van de gateway.

## <a name="rmvnet"></a>Sectie 2: de Resource Manager VNet-instellingen configureren

In deze sectie maakt u de virtuele netwerkgateway en de lokale netwerkgateway voor uw Resource Manager VNet. Als u een Resource Manager VNet niet hebt en deze stappen als oefening maakt uitvoert, kunt u een VNet maken met behulp van [in dit artikel](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) en de [voorbeeld](#values) waarden voor de instellingen van hierboven.

De schermafbeeldingen dienen alleen als voorbeeld. Zorg ervoor dat de waarden vervangt door uw eigen of gebruik de [voorbeeld](#values) waarden.

### <a name="1-create-a-gateway-subnet"></a>1. Een gatewaysubnet maken

Voordat u een virtuele netwerkgateway maakt, moet u eerst het gatewaysubnet maken. Een gatewaysubnet met CIDR-telling van/28 of groter maken. (/ 27, / 26, enz.)

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="creategw"></a>2. De gateway van een virtueel netwerk maken

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>3. Een lokale netwerkgateway maken

De lokale netwerkgateway Hiermee geeft u het adresbereik en het openbare IP-adres die zijn gekoppeld aan uw klassieke VNet en de virtuele netwerkgateway.

Als u deze stappen als oefening uitvoert, raadpleegt u deze instellingen:

| Virtual Network | Adresruimte | Regio | Maakt verbinding met lokale netwerksite op. |Gateway openbare IP-adres|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |VS - west | RMVNetLocal (192.168.0.0/16) |Het openbare IP-adres dat is toegewezen aan de ClassicVNet-gateway|
| RMVNet | (192.168.0.0/16) |VS - oost |ClassicVNetLocal (10.0.0.0/24) |Het openbare IP-adres dat is toegewezen aan de gateway RMVNet.|

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>Sectie 3: de klassieke VNet lokale site-instellingen wijzigen

In deze sectie vervangt u de tijdelijke aanduiding voor IP-adres dat u hebt gebruikt bij het opgeven van de lokale site-instellingen met het IP-adres van de Resource Manager-VPN-gateway. Deze sectie wordt de klassieke (SM) PowerShell-cmdlets gebruikt.

1. Ga in de Azure-portal naar het klassieke virtuele netwerk.
2. Klik op de pagina voor het virtuele netwerk op **overzicht**.
3. In de **VPN-verbindingen** sectie, klikt u op de naam van uw lokale site in de afbeelding.

    ![VPN-verbindingen](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN-verbindingen")
4. Op de **Site-naar-site VPN-verbindingen** pagina, klikt u op de naam van de site.

    ![Site-name](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "lokale sitenaam")
5. Klik op de verbindingspagina voor uw lokale site op de naam van de lokale site openen de **lokale site** pagina.

    ![Open-local-site](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "lokale site openen")
6. Op de **lokale site** pagina, vervangen door de **IP-adres van VPN-gateway** met het IP-adres van de Resource Manager-gateway.

    ![Gateway-ip-adres](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "Gateway IP-adres")
7. Klik op **OK** bijwerken van het IP-adres.

## <a name="RMtoclassic"></a>Sectie 4: Resource Manager naar klassieke verbinding maken

In deze stap configureert u de verbinding van het Resource Manager VNet met het klassieke VNet met de Azure portal.

1. In **alle resources**, Ga naar de lokale netwerkgateway. In ons voorbeeld wordt de lokale netwerkgateway is **ClassicVNetLocal**.
2. Klik op **configuratie** en controleer of de waarde van de IP-adres de VPN-gateway voor het klassieke VNet. Indien nodig bijwerken en klik vervolgens op **opslaan**. De pagina te sluiten.
3. In **alle resources**, klikt u op de lokale netwerkgateway.
4. Klik op **verbindingen** om de pagina verbindingen te openen.
5. Op de **verbindingen** pagina, klikt u op  **+**  een verbinding toevoegen.
6. Op de **verbinding toevoegen** pagina en de naam van de verbinding. Bijvoorbeeld 'RMtoClassic'.
7. **Site-naar-Site** is al ingeschakeld op deze pagina.
8. Selecteer de virtuele netwerkgateway die u wilt koppelen aan deze site.
9. Maak een **gedeelde sleutel**. Deze sleutel wordt ook gebruikt in de verbinding die u vanuit het klassieke VNet naar het Resource Manager VNet maakt. U kunt de sleutel te genereren of er een bedenken. In ons voorbeeld gebruiken we 'abc123', maar u kunt (en moeten) gebruikmaken van iets meer complexe.
10. Klik op **OK** om de verbinding te maken.

##<a name="classictoRM"></a>Sectie 5 - van klassiek naar Resource Manager-verbinding maken

In deze stap configureert u de verbinding van het klassieke VNet naar het Resource Manager VNet. Deze stappen moet PowerShell. U kunt deze verbinding maken in de portal. Zorg ervoor dat u hebt gedownload en geïnstalleerd voor de klassieke (SM) en de Resource Manager (RM) PowerShell-cmdlets.

### <a name="1-connect-to-your-azure-account"></a>1. Verbinding maken met uw Azure-account

Open de PowerShell-console met verhoogde rechten en aanmelden bij uw Azure-account. De volgende cmdlet vraagt u om uw Azure-Account voor de aanmeldingsreferenties. Na het aanmelden, worden de instellingen van uw account worden gedownload zodat ze beschikbaar voor Azure PowerShell zijn.

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

Uw Azure-Account voor het gebruik van de klassieke PowerShell-cmdlets (SM) toevoegen. U kunt de volgende opdracht gebruiken om dit te doen:

```powershell
Add-AzureAccount
```

### <a name="2-view-the-network-configuration-file-values"></a>2. De waarden van de netwerk-configuratie weergeven

Wanneer u een VNet in de Azure portal maakt, is de volledige naam die gebruikmaakt van Azure niet zichtbaar in de Azure-portal. Een VNet dat wordt weergegeven in de Azure portal 'ClassicVNet' naam kan bijvoorbeeld een veel langer naam hebben in het configuratiebestand van het netwerk. De naam kan als volgt uitzien: 'Groep ClassicRG ClassicVNet'. In deze stappen voor het downloaden van het configuratiebestand van het netwerk en de waarden weergeven.

Maak een map op de computer en exporteer vervolgens het netwerkconfiguratiebestand naar de map. In dit voorbeeld wordt het netwerkconfiguratiebestand geëxporteerd naar C:\AzureNet.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Open het bestand met een teksteditor en de naam voor het klassieke VNet weergeven. De namen in het configuratiebestand van het netwerk gebruiken bij het uitvoeren van PowerShell-cmdlets.

- VNet-namen worden vermeld als **VirtualNetworkSite name =**
- De namen van sites worden vermeld als **LocalNetworkSite name =**

### <a name="3-create-the-connection"></a>3. De verbinding maken

De gedeelde sleutel instellen en het maken van de verbinding van het klassieke VNet naar het Resource Manager VNet. U kunt de gedeelde sleutel met de portal niet instellen. Zorg ervoor dat u deze stappen terwijl u bent aangemeld met de klassieke versie van de PowerShell-cmdlets uitvoeren. Gebruiken om dit te doen **Add-AzureAccount**. Anders u zich niet in te stellen de '-AzureVNetGatewayKey'.

- In dit voorbeeld **- VNetName** is de naam van het klassieke VNet zoals gevonden in het configuratiebestand van uw netwerk. 
- De **- LocalNetworkSiteName** is de naam die u hebt opgegeven voor de lokale site als gevonden in het configuratiebestand van uw netwerk.
- De **- SharedKey** is een waarde die u genereren en opgeven. In dit voorbeeld hebben we gebruikt *abc123*, maar u kunt iets ingewikkelders genereren. Het belangrijkste is dat de waarde die u hier opgeeft, dezelfde waarde die u hebt opgegeven moet bij het maken van de Resource Manager naar klassieke verbinding.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

##<a name="verify"></a>Sectie 6 - Controleer of uw verbindingen

U kunt de verbindingen controleren met behulp van de Azure-portal of PowerShell. Bij de verificatie, moet u mogelijk Wacht een minuut of twee als de verbinding wordt gemaakt. Als een verbinding geslaagd is, wordt de status van de status van de connectiviteit gewijzigd van 'Verbinding maken' in 'Verbonden'.

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Om te controleren of de verbinding van uw klassieke VNet naar uw VNet Resource Manager

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

###<a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Om te controleren of de verbinding van uw Resource Manager VNet met het klassieke VNet

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
