---
title: 'Maak een verbinding tussen VNets: klassiek: Azure-portal | Microsoft Docs'
description: Verbinding maken met Azure-netwerken samen met behulp van PowerShell en Azure portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: cherylmc
ms.openlocfilehash: 6924d4eca52bfab8c90e7787bb8849b47df064db
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58112259"
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>Configureren van een VNet-naar-VNet-verbinding (klassiek)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Dit artikel helpt u bij het maken van een VPN-gatewayverbinding tussen virtuele netwerken. De virtuele netwerken kunnen zich in dezelfde of verschillende regio's bevinden en tot dezelfde of verschillende abonnementen behoren. De stappen in dit artikel zijn van toepassing op het klassieke implementatiemodel en Azure portal. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure-portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure-CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Verbinding maken tussen verschillende implementatiemodellen - Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Verbinding maken tussen verschillende implementatiemodellen - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![VNet-naar-VNet-connectiviteit-Diagram](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)

## <a name="about-vnet-to-vnet-connections"></a>Over VNet-naar-VNet-verbindingen

Een virtueel netwerk verbinden met een ander virtueel netwerk (VNet-naar-VNet) in het klassieke implementatiemodel met behulp van een VPN-gateway is vergelijkbaar met een virtueel netwerk verbinden met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden.

De VNets die u verbindt, kunnen zich in verschillende abonnementen en verschillende regio's. U kunt VNet naar VNet-communicatie met multi-site-configuraties combineren. Zo kunt u netwerktopologieën maken waarin cross-premises connectiviteit is gecombineerd met connectiviteit tussen virtuele netwerken.

![VNet-naar-VNet-verbindingen](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why"></a>Waarom virtuele netwerken koppelen?

U wilt virtuele netwerken wellicht koppelen om de volgende redenen:

* **Geografische redundantie en aanwezigheid tussen regio's**

  * U kunt uw eigen geo-replicatie of synchronisatie met beveiligde connectiviteit instellen zonder gebruik te maken van internetgerichte eindpunten.
  * Met Azure Load Balancer en Microsoft of derden clustering technologie, kunt u maximaal beschikbare werkbelasting met geografische redundantie over meerdere Azure-regio's instellen. Een belangrijk voorbeeld hiervan is het instellen van SQL Always On met beschikbaarheidsgroepen verspreid over meerdere Azure-regio's.
* **Regionale toepassingen met meerdere lagen met sterke isolatiegrens**

  * Binnen dezelfde regio, kunt u toepassingen met meerdere lagen instellen met meerdere VNets die zijn verbonden, samen met een sterke isolatie en beveiligde tussen laag-communicatie.
* **Cross-abonnement, de communicatie tussen organisatie in Azure**

  * Als u meerdere Azure-abonnementen hebt, kunt u workloads uit verschillende abonnementen samen veilig tussen virtuele netwerken.
  * U kunt binnen de organisatie-communicatie met beveiligde VPN-technologie in Azure inschakelen voor ondernemingen of serviceproviders.

Zie voor meer informatie over verbindingen tussen VNets de [Aandachtspunten bij VNet-naar-VNet](#faq) aan het einde van dit artikel.

### <a name="before-you-begin"></a>Voordat u begint

Voordat u deze oefening begint, download en installeer de nieuwste versie van de Azure SM (Service Management) PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. We gebruiken de portal voor het merendeel van de stappen, maar u moet PowerShell gebruiken om de verbindingen tussen de VNets te maken. U kunt geen verbindingen met Azure portal maken.

## <a name="plan"></a>Stap 1: De IP-adresbereiken plannen

Het is belangrijk om te bepalen van de bereiken die u gebruiken gaat om uw virtuele netwerken te configureren. Voor deze configuratie, moet u ervoor zorgen dat geen van de VNet-adresbereiken overlappen met elkaar of met een van de lokale netwerken waarmee ze verbinding maken.

De volgende tabel ziet u een voorbeeld van hoe u uw vnet's definiëren. Gebruik de bereiken als richtlijn alleen. Noteer de bereiken voor uw virtuele netwerken. U hebt deze informatie nodig voor de latere fasen.

**Voorbeeld**

| Virtual Network | Adresruimte | Regio | Maakt verbinding met lokale netwerksite |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |US - oost |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |US - west |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>Stap 2: de virtuele netwerken maken

Maakt u twee virtuele netwerken in de [Azure-portal](https://portal.azure.com). Zie voor de stappen voor het maken van klassieke virtuele netwerken, [maken van een klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). 

Wanneer een klassiek virtueel netwerk maken met de portal, moet u navigeren naar de pagina virtueel netwerk met behulp van de volgende stappen uit, anders wordt de optie voor het maken van een klassiek virtueel netwerk niet wordt weergegeven:

1. Klik op '+' op de pagina 'Nieuw' openen.
2. Typ in het veld 'Zoek in marketplace', 'Virtueelnetwerk'. Als u in plaats daarvan selecteert netwerk -> Virtueelnetwerk, krijgt u niet de optie voor het maken van een klassiek VNet.
3. Zoek 'Virtueelnetwerk' in de geretourneerde lijst en klikt u erop om de pagina Virtueelnetwerk te openen. 
4. Selecteer op de pagina virtueel netwerk 'Classic' te maken van een klassiek VNet. 

Als u van dit artikel bij wijze van oefening gebruikmaakt, kunt u de volgende voorbeeldwaarden gebruiken:

**Waarden voor TestVNet1**

Naam: TestVNet1<br>
Adresruimte: 10.11.0.0/16, 10.12.0.0/16 (optioneel)<br>
Subnetnaam: standaard<br>
Subnetadresbereik: 10.11.0.1/24<br>
Resourcegroep: ClassicRG<br>
Locatie: US - oost<br>
GatewaySubnet: 10.11.1.0/27

**Waarden voor TestVNet4**

Naam: TestVNet4<br>
Adresruimte: 10.41.0.0/16, 10.42.0.0/16 (optioneel)<br>
Subnetnaam: standaard<br>
Subnetadresbereik: 10.41.0.1/24<br>
Resourcegroep: ClassicRG<br>
Locatie: US - west<br>
GatewaySubnet: 10.41.1.0/27

**Bij het maken van uw vnet's, houd rekening met de volgende instellingen:**

* **Virtual Network-adresruimten** : op de pagina adresruimten voor virtueel netwerk, geef het adresbereik dat u wilt gebruiken voor het virtuele netwerk. Dit zijn de dynamische IP-adressen die worden toegewezen aan de virtuele machines en andere rolinstanties die u op dit virtuele netwerk implementeert.<br>De adresruimten die u selecteert mogen niet overlappen met de adresruimten voor het gebruik van de andere VNets of on-premises locaties die dit VNet verbinding wordt gemaakt.

* **Locatie** – wanneer u een virtueel netwerk, maakt u deze koppelen aan een Azure-locatie (regio). Bijvoorbeeld, als u wilt dat uw virtuele machines die zijn geïmplementeerd met het virtuele netwerk zich fysiek bevinden in VS-West, selecteert u die locatie. U kunt de locatie die is gekoppeld aan het virtuele netwerk nadat u dit hebt gemaakt niet wijzigen.

**Na het maken van uw VNets, kunt u de volgende instellingen toevoegen:**

* **Adresruimte** : extra adresruimte is niet vereist voor deze configuratie, maar u kunt extra adresruimte toevoegen na het maken van het VNet.

* **Subnetten** : extra subnetten zijn niet vereist voor deze configuratie, maar u mogelijk wilt u uw virtuele machines in een subnet dat is gescheiden van uw andere rolinstanties.

* **DNS-servers** – Geef de naam van de DNS-server en het IP-adres. Met deze instelling wordt geen DNS-server gemaakt. U kunt hiermee de DNS-servers opgeven die u wilt gebruiken voor de naamomzetting voor dit virtuele netwerk.

In deze sectie maakt u het verbindingstype, de lokale site configureren en maken van de gateway.

## <a name="localsite"></a>Stap 3: de lokale site configureren

Azure maakt gebruik van de instellingen die zijn opgegeven in de lokale netwerksite om te bepalen hoe het routeren van verkeer tussen de VNets. Elk VNet moet verwijzen naar de respectieve lokale netwerk die u wilt routeren van verkeer naar. U bepalen de naam die u wilt gebruiken om te verwijzen naar de lokale netwerksite. Het is raadzaam een beschrijvende gebruikt.

Bijvoorbeeld, TestVNet1 verbinding maakt met de site van een lokale netwerk die u maakt met de naam 'VNet4Local'. De instellingen voor VNet4Local bevatten de adresvoorvoegsels voor TestVNet4.

De lokale site voor elke VNet is het andere VNet. De volgende voorbeelden van waarden worden gebruikt voor de configuratie:

| Virtual Network | Adresruimte | Regio | Maakt verbinding met lokale netwerksite |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |US - oost |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |US - west |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Ga naar TestVNet1 in Azure portal. In de **VPN-verbindingen** sectie van de pagina, klikt u op **Gateway**.

    ![Er is geen gateway](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. Op de **nieuwe VPN-verbinding** weergeeft, schakelt **Site-naar-Site**.
3. Klik op **lokale site** open de pagina van de lokale site en de instellingen configureren.
4. Op de **lokale site** pagina en de naam van uw lokale site. In ons voorbeeld naam we de lokale site 'VNet4Local'.
5. Voor **IP-adres van VPN-gateway**, u kunt IP-adressen die u wilt gebruiken, zolang het is een geldige indeling hebben. Normaal gesproken gebruikt u het externe IP-adres voor een VPN-apparaat. Maar voor een klassieke VNet-naar-VNet-configuratie, u het openbare IP-adres dat is toegewezen aan de gateway voor uw VNet. Gezien het feit dat u de virtuele netwerkgateway is nog niet hebt gemaakt, kunt u een geldig openbaar IP-adres opgeven als een tijdelijke aanduiding.<br>Geen laat dit veld leeg: het is niet optioneel voor deze configuratie. In een latere stap, gaat u terug naar deze instellingen en ze met de bijbehorende IP-adressen van een virtueel netwerk-gateway configureren wanneer Azure dit genereert.
6. Voor **Clientadresruimte**, gebruikt u de adresruimte van het andere VNet. Raadpleeg uw planning voorbeeld. Klik op **OK** aan uw instellingen opslaan en terug te keren naar de **nieuwe VPN-verbinding** pagina.

    ![lokale site](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>Stap 4: de gateway van virtueel netwerk maken

Elk virtueel netwerk moet een virtuele netwerkgateway hebben. De virtuele netwerkgateway routes en verkeer wordt versleuteld.

1. Schakel op de pagina **Nieuwe VPN-verbinding** het selectievakje **Gateway onmiddellijk maken** in.
2. Klik op **Subnet, grootte en routingtype**. Op de **gatewayconfiguratie** pagina, klikt u op **Subnet**.
3. De naam van het gateway-subnet wordt automatisch ingevuld met de vereiste naam 'GatewaySubnet'. De **adresbereik** bevat de IP-adressen die zijn toegewezen aan de VPN-gateway-services. Sommige configuraties een gatewaysubnet van /29 toestaan, maar het is raadzaam een/28 of/27 te gebruiken voor toekomstige configuraties waarvoor meer IP-adressen voor de gateway-services. In onze voorbeeldinstellingen gebruiken we 10.11.1.0/27. De adresruimte aanpassen en klik vervolgens op **OK**.
4. Configureer de **grootte van de Gateway**. Deze instelling verwijst naar de [Gateway-SKU](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
5. Configureer de **Routeringstype**. Voor deze configuratie moet de routering Typ **dynamische**. U kunt het routeringstype dat later niet wijzigen, tenzij u de gateway verbreken en een nieuwe maken.
6. Klik op **OK**.
7. Op de **nieuwe VPN-verbinding** pagina, klikt u op **OK** om te beginnen met het maken van de virtuele netwerkgateway. Het maken van een gateway duurt vaak 45 minuten of langer, afhankelijk van de geselecteerde gateway-SKU.

## <a name="vnet4settings"></a>Stap 5: TestVNet4-instellingen configureren

Herhaal de stappen voor het [maken van een lokale site](#localsite) en [maken van de virtuele netwerkgateway](#gw) TestVNet4, vervangen door de waarden wanneer dat nodig configureren. Als u dit bij wijze van oefening doet, gebruikt u de [voorbeeldwaarden](#vnetvalues).

## <a name="updatelocal"></a>Stap 6: de lokale sites bijwerken

Nadat de virtuele netwerkgateways voor beide vnet's zijn gemaakt, moet u de lokale sites aanpassen **IP-adres van VPN-gateway** waarden.

|VNET-naam|Verbonden site|IP-adres van gateway|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|IP-adres van VPN-gateway voor TestVNet4|
|TestVNet4|VNet1Local|IP-adres van VPN-gateway voor TestVNet1|

### <a name="part-1---get-the-virtual-network-gateway-public-ip-address"></a>Deel 1: het virtuele netwerk openbare IP-adres ophalen

1. Zoek het virtuele netwerk in Azure portal.
2. Klik hier om het VNet **overzicht** pagina. Klik op de pagina in **VPN-verbindingen**, kunt u het IP-adres voor uw virtuele netwerkgateway weergeven.

   ![Openbare IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. Kopieer het IP-adres. U gebruikt deze in de volgende sectie.
4. Herhaal deze stappen voor TestVNet4

### <a name="part-2---modify-the-local-sites"></a>Deel 2: de lokale sites wijzigen

1. Zoek het virtuele netwerk in Azure portal.
2. Op het VNet **overzicht** pagina, klikt u op de lokale site.

   ![Lokale site die is gemaakt](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. Op de **Site-naar-Site VPN-verbindingen** pagina, klikt u op de naam van de lokale site die u wilt wijzigen.

   ![Lokale site openen](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Klik op de **lokale site** die u wilt wijzigen.

   ![site wijzigen](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. Update de **IP-adres van VPN-gateway** en klikt u op **OK** de instellingen op te slaan.

   ![Gateway IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Sluit de andere pagina's.
7. Herhaal deze stappen voor TestVNet4.

## <a name="getvalues"></a>Stap 7: de waarden ophalen uit het netwerkconfiguratiebestand

Wanneer u klassieke VNets in Azure portal maakt, is de naam die u niet de volledige naam die u voor PowerShell gebruikt. Bijvoorbeeld: een VNet dat wordt weergegeven op de naam **TestVNet1** in de portal voor een veel langere naam kunnen hebben in het configuratiebestand van het netwerk. De naam kan als volgt uitzien: **Groep ClassicRG TestVNet1**. Wanneer u uw verbindingen maakt, is het belangrijk dat u de waarden die u ziet in het configuratiebestand van het netwerk.

In de volgende stappen maakt u verbinding maken met uw Azure-account en downloaden en het netwerkconfiguratiebestand om op te halen van de waarden die vereist voor uw verbindingen zijn weergeven.

1. Download en installeer de nieuwste versie van de Azure SM (Service Management) PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

2. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

   ```powershell
   Connect-AzureRmAccount
   ```

   Controleer de abonnementen voor het account.

   ```powershell
   Get-AzureRmSubscription
   ```

   Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken.

   ```powershell
   Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```

   Gebruik vervolgens de volgende cmdlet uit uw Azure-abonnement toevoegen aan PowerShell voor het klassieke implementatiemodel.

   ```powershell
   Add-AzureAccount
   ```
3. Exporteren en het netwerkconfiguratiebestand weergeven. Maak een map op de computer en exporteer vervolgens het netwerkconfiguratiebestand naar de map. In dit voorbeeld wordt het netwerkconfiguratiebestand geëxporteerd naar **C:\AzureNet**.

   ```powershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
4. Open het bestand met een teksteditor en de namen voor uw vnet's en sites weergeven. Dit zijn de naam die u gebruikt wanneer u uw verbindingen gemaakt.<br>VNet-namen worden weergegeven als **VirtualNetworkSite name =**<br>Sitenamen worden weergegeven als **LocalNetworkSiteRef naam =**

## <a name="createconnections"></a>Stap 8: de VPN-gateway-verbindingen maken

Wanneer de vorige stappen zijn voltooid, kunt u de vooraf gedeelde IPsec/IKE-sleutels en maak de verbinding. Deze reeks stappen wordt PowerShell gebruikt. VNet-naar-VNet-verbindingen voor het klassieke implementatiemodel kunnen niet worden geconfigureerd in Azure portal.

In de voorbeelden ziet u de gedeelde sleutel is precies hetzelfde. De gedeelde sleutel moet altijd overeenkomen. Zorg ervoor dat de waarden in deze voorbeelden vervangt door de exacte namen voor uw vnet's en lokale netwerksites.

1. Maak de verbinding tussen TestVNet1 en TestVNet4.

   ```powershell
   Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
   -LocalNetworkSiteName '17BE5E2C_VNet4Local' -SharedKey A1b2C3D4
   ```
2. Maak de verbinding tussen TestVNet4 en TestVNet1.

   ```powershell
   Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
   -LocalNetworkSiteName 'F7F7BFC7_VNet1Local' -SharedKey A1b2C3D4
   ```
3. Wacht tot de verbindingen worden geïnitialiseerd. Zodra de gateway is geïnitialiseerd, is de Status 'Geslaagd'.

   ```
   Error          :
   HttpStatusCode : OK
   Id             :
   Status         : Successful
   RequestId      :
   StatusCode     : OK
   ```

## <a name="faq"></a>VNet-naar-VNet-overwegingen voor het klassieke vnet 's
* De virtuele netwerken kunnen zich in de dezelfde of verschillende abonnementen behoren.
* De virtuele netwerken kunnen zich in dezelfde of verschillende Azure-regio's (locaties) bevinden.
* Een cloudservice of een taakverdelingseindpunt kan geen virtuele netwerken overbruggen, zelfs niet als ze met elkaar zijn verbonden.
* Met elkaar verbinden van meerdere virtuele netwerken zijn vereist om een VPN-apparaten.
* VNet-naar-VNet ondersteunt het verbinden virtuele Azure-netwerken. Het ondersteunt niet verbinden van virtuele machines of cloudservices die niet zijn geïmplementeerd met een virtueel netwerk.
* VNet-naar-VNet-vereist gateways met dynamische routering. Azure gateways met statische routering worden niet ondersteund.
* U kunt virtuele-netwerkverbindingen tegelijk gebruiken met multi-site-VPN’s. Er is een maximum van 10 VPN-tunnels voor een VPN-gateway van het virtuele netwerk verbinding maken met andere virtuele netwerken of on-premises-sites.
* De adresruimten van de virtuele netwerken en on-premises lokale netwerksites mogen elkaar niet overlappen. Overlappende adresruimten zorgt ervoor dat het maken van virtuele netwerken of uploaden netcfg-configuratiebestanden mislukken.
* Redundante tunnels tussen een paar virtuele netwerken worden niet ondersteund.
* Alle VPN-tunnels voor het VNet, met inbegrip van P2S VPN's, delen de beschikbare bandbreedte voor de VPN-gateway en de dezelfde VPN-gateway een SLA voor uptime in Azure.
* VNet-naar-VNet-verkeer via de Azure-backbone wordt verzonden.

## <a name="next-steps"></a>Volgende stappen
Controleer uw verbindingen. Zie [een VPN-gatewayverbinding verifiëren](vpn-gateway-verify-connection-resource-manager.md).
