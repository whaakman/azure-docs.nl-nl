---
title: Infrastructuur en de verbinding met SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Infrastructuur van de verbinding is vereist voor het gebruik van SAP HANA in Azure (grote exemplaren) configureren.
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 177627d8f72dbd04fb918ac7ece18321246a9c62
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="sap-hana-large-instances-infrastructure-and-connectivity-on-azure"></a>Infrastructuur voor SAP HANA (grote exemplaren) en de verbindingen van Azure 

Sommige definities tevoren voordat u deze handleiding leest. In [SAP HANA (grote exemplaren) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) er zijn twee verschillende soorten HANA grote exemplaar eenheden met geïntroduceerd:

- S72, S72m S144, S144m, S192 en S192m die naar verwezen als 'Type ik klasse' van SKU's.
- S384, S384m S384xm, S576, S768 en S960 die naar als de 'Type II class-instructie van SKU's verwezen.

De klasse-specificaties gaat worden gebruikt in de documentatie van de grote exemplaar HANA uiteindelijk verwijzen naar verschillende mogelijkheden en vereisten op basis van HANA grote exemplaar SKU's.

Er zijn andere definities die wordt vaak gebruikt:
- **Grote exemplaar stempel:** een hardware-infrastructuur stack die SAP HANA TDI gecertificeerd en toegewezen voor SAP HANA-exemplaren in Azure uitvoeren.
- **SAP HANA in Azure (grote exemplaren):** officiële taal voor de aanbieding in Azure uitvoeren HANA-exemplaren op SAP HANA TDI gecertificeerde hardware die wordt geïmplementeerd in grote exemplaar stempels in verschillende Azure-regio's. De verwante term **HANA grote exemplaar** kort voor SAP HANA in Azure (grote exemplaren) en veel is deze technische handleiding gebruikt.
 

Nadat de aankoop van SAP HANA in Azure (grote exemplaren) tussen u en Microsoft enterprise-accountteam is voltooid, wordt de volgende informatie door Microsoft vereist HANA grote exemplaar eenheden implementeren:

- Naam van de klant
- Zakelijke contactgegevens (inclusief e-mailadres en telefoonnummer)
- Technische contactgegevens (inclusief e-mailadres en telefoonnummer nummer)
- Technische contactpersoon netwerkgegevens (inclusief e-mailadres en telefoonnummer nummer)
- Azure-implementatie regio (VS-West, VS-Oost, Australië-Oost, Australië-Zuidoost, West-Europa en Noord-Europa vanaf juli 
- 2017)
- Bevestig SAP HANA op Azure (grote exemplaren) SKU (configuratie)
- Als al beschreven in het overzicht en architectuur voor grote HANA-exemplaren voor elke Azure-regio die wordt geïmplementeerd op:
    - Een/29 IP-adresbereik voor ER P2P-verbindingen die verbinding maken met Azure VNets HANA grote exemplaren
    - Een/24 CIDR-blok gebruikt voor de HANA grote exemplaren Server IP-adresgroep
- De IP-adres bereikwaarden gebruikt in het kenmerk VNet-adresruimte van elke Azure-VNet die verbinding met de grote HANA-exemplaren maakt
- Gegevens voor elk systeem HANA grote exemplaren:
  - Gewenste hostnaam - in het ideale geval met de volledig gekwalificeerde domeinnaam.
  - Gewenste IP-adres voor de eenheid HANA grote exemplaar buiten het bereik van de groep met IP-adres - Houd er rekening mee dat de eerste 30 IP-adressen in het bereik van de groep met IP-adressen zijn gereserveerd voor intern gebruik binnen grote HANA-exemplaren
  - De naam van de SAP HANA SID voor het SAP HANA-exemplaar (vereist voor het maken van de benodigde SAP HANA-gerelateerde schijfvolumes). De HANA SID is vereist voor het maken van de machtigingen voor <sidadm> op de NFS-volumes die zijn ophalen gekoppeld aan de eenheid HANA grote exemplaar. Dit wordt ook gebruikt als een van de onderdelen van de naam van de volumes op schijven die gekoppeld ophalen. Als u meer dan één HANA-exemplaar op de eenheid uitvoeren wilt, moet u voor een lijst met meerdere HANA SID's. Elke Hiermee haalt u een afzonderlijke set van volumes die zijn toegewezen.
  - De groeps-id die de hana sidadm gebruiker in de Linux-besturingssysteem heeft is vereist voor het maken van de benodigde SAP HANA-gerelateerde schijfvolumes. De SAP HANA-installatie wordt de groep sapsys meestal gemaakt met een groeps-id 1001. De gebruiker hana sidadm maakt deel uit van de groep
  - De gebruikersnaam die de gebruiker hana sidadm in de Linux-besturingssysteem heeft is vereist voor het maken van de benodigde SAP HANA-gerelateerde schijfvolumes. Als u meerdere exemplaren van HANA op de eenheid uitvoert, moet u de lijst alle de <sid>adm-gebruikers 
- Azure-abonnement-ID voor het Azure-abonnement aan welke SAP HANA op Azure HANA grote exemplaren gaat rechtstreeks zijn aangesloten. Dit abonnement-ID verwijst naar de Azure-abonnement, die wordt in rekening gebracht met de eenheid HANA grote exemplaar.

Nadat u de informatie opgeeft, wordt Microsoft richt SAP HANA in Azure (grote exemplaren) en retourneert de informatie nodig uw Azure VNets koppelen aan HANA grote exemplaren en de toegang tot de eenheden HANA grote exemplaar.

## <a name="connecting-azure-vms-to-hana-large-instances"></a>Verbinding maken met virtuele machines in Azure HANA grote exemplaren

Zoals vermeld [SAP HANA (grote exemplaren) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) de implementatie van de minimale van HANA grote exemplaren met de toepassingslaag SAP in Azure lijkt, zoals:

![Azure VNet verbinding met SAP HANA op Azure (grote exemplaren) en on-premises](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Zoek dichter bij de Azure-VNet-zijde, we de noodzaak van de volgende voordelen:

- De definitie van een Azure VNet dat wordt gebruikt voor het implementeren van de virtuele machines van de toepassingslaag SAP in.
- Dat automatisch betekent dat een subnet standaard in de Azure-Vnet is gedefinieerd, die is in feite een gebruikt voor het implementeren van de virtuele machines in.
- Het Azure VNet dat gemaakt, moet ten minste één VM-subnet en een ExpressRoute-Gateway-subnet. Deze subnetten moeten de IP-adresbereiken als opgegeven en die wordt beschreven in de volgende secties worden toegewezen.

Ja, bekijken we iets dichter bij het maken van het Azure VNet voor grote HANA-exemplaren

### <a name="creating-the-azure-vnet-for-hana-large-instances"></a>Maken van het Azure VNet voor grote HANA-exemplaren

>[!Note]
>Het Azure VNet voor grote HANA-exemplaar moet worden gemaakt met het Azure Resource Manager-implementatiemodel. Het oudere model van Azure-implementatie, bekend als het klassieke implementatiemodel wordt niet ondersteund met de oplossing voor grote HANA-exemplaar.

Het VNet kan worden gemaakt met de Azure-portal, PowerShell, Azure-sjabloon of Azure CLI (Zie [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). We bekijken in het volgende voorbeeld in een VNet gemaakt via de Azure portal.

Als we in de definities van een Azure VNet via de Azure portal bekijken, bekijken we over een aantal van de definities en hoe die betrekking hebben op wat wij in de lijst van verschillende IP-adresbereiken. Als we bespreken de **adresruimte**, bedoelen we de adresruimte die het Azure VNet mag worden gebruikt. Deze adresruimte is ook het adresbereik op dat het VNet wordt gebruikt voor het doorgeven van de BGP-route. Dit **adresruimte** kunnen hier worden weergegeven:

![Address Space van Azure VNet weergegeven in de Azure-portal](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Het Azure VNet opgegeven in het geval voorgaande met 10.16.0.0/16, een in plaats daarvan grote en wide IP-adresbereik te gebruiken. Betekent dat alle IP-adresbereiken van de volgende subnetten binnen dit VNet kunnen hun bereiken binnen die adresruimte. Meestal bevelen we dergelijke grote adresbereik niet voor één VNet in Azure. Maar we ophalen van een stapje verder kunt bekijken in de gedefinieerd in de Azure-VNet-subnetten:

![Azure VNet-subnetten en hun IP-adresbereiken](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Zoals u ziet, bekijken we een VNet met een eerste VM-subnet (hier 'default' genoemd) en een subnet met de naam 'GatewaySubnet'.
In de volgende sectie verwijzen we naar het IP-adresbereik van het subnet, 'default' is aangeroepen in de afbeeldingen als **Azure VM-subnet IP-adresbereik**. In de volgende secties verwijzen we naar het IP-adresbereik van het Gatewaysubnet als **VNet Gateway-Subnet IP-adresbereik**. 

In het geval blijkt uit de bovenstaande twee afbeeldingen ziet u dat de **VNet-adresruimte** behandelt, de **Azure VM-subnet IP-adresbereik** en de **VNet Gateway-Subnet IP-adresbereik**. 

In andere gevallen waarin u wilt besparen of specifiek met de IP-adresbereiken, kunt u beperken de **VNet-adresruimte** van een VNet-naar de specifieke bereiken door elk subnet wordt gebruikt. In dit geval kunt u de **VNet-adresruimte** van een VNet als meerdere specifieke bereiken als volgt te werk:

![Azure VNet-adresruimte met twee spaties](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

In dit geval de **VNet-adresruimte** heeft twee spaties gedefinieerd. Deze twee spaties zijn identiek aan de IP-adresbereiken gedefinieerd voor **Azure VM-subnet IP-adresbereik** en de **VNet Gateway-Subnet IP-adresbereik.**

U kunt naamgevingsnorm die u wilt gebruiken voor deze tenant subnetten (VM-subnetten). Echter, **altijd moet er één en slechts één gateway-subnet voor elk VNet** die is verbonden met de SAP HANA op Azure (grote exemplaren) ExpressRoute-circuit. En **deze gatewaysubnet moet altijd de naam 'GatewaySubnet'** om te controleren of de juiste plaatsing van de ExpressRoute-gateway.

> [!WARNING] 
> Het is essentieel dat het gatewaysubnet altijd de naam 'GatewaySubnet'.

Meerdere VM-subnetten kunnen worden gebruikt, zelfs met behulp van niet-aaneengesloten-adresbereiken. Maar zoals eerder vermeld, wordt deze adresbereiken moeten worden gedekt door de **VNet-adresruimte** van de VNet in geaggregeerde vorm of in een lijst met de exacte bereiken van de VM-subnetten en het gatewaysubnet.

Samenvatting van belangrijke fact over een Azure-VNet die verbinding met HANA grote exemplaren maakt:

- U moet verzenden naar Microsoft de **VNet-adresruimte** bij het uitvoeren van een aanvankelijke implementatie van grote HANA-exemplaren. 
- De **VNet-adresruimte** mag een groter bereik die betrekking heeft op het bereik van IP-adresbereiken op Azure VM-subnet en het VNet Gateway-Subnet IP-adresbereik.
- Of u kunt indienen als **VNet-adresruimte** meerdere bereiken die betrekking hebben op de verschillende IP-adresbereiken van VM-subnet IP-adresbereiken en het VNet Gateway-Subnet IP-adresbereik.
- De gedefinieerde **VNet-adresruimte** gebruikte doorgifte van de BGP-routering is.
- De naam van het gatewaysubnet moet: **'GatewaySubnet'.**
- De **VNet-adresruimte** wordt gebruikt als een filter aan de kant van de grote HANA-exemplaar voor het toestaan of weigeren van verkeer naar de eenheden HANA grote exemplaar van Azure. Als de BGP-routinggegevens van het Azure VNet en de IP-adresbereiken geconfigureerd voor het filteren van de zijde HANA grote exemplaar niet overeenkomen, kunnen problemen in de verbinding veroorzaakt.
- Er zijn een aantal details van het gatewaysubnet die zijn beschreven verderop in de sectie 'Verbinden van een VNet met HANA grote exemplaar ExpressRoute'



### <a name="different-ip-address-ranges-to-be-defined"></a>Verschillende IP-adresbereiken worden gedefinieerd 

We al enkele van de IP-adresbereiken die voor het implementeren van grote HANA-exemplaren in vorige gedeelten geïntroduceerd. Maar er zijn een aantal meer IP-adresbereiken, die belangrijk zijn. We doorlopen een meer gedetailleerde informatie. De volgende IP-adressen waarvan niet hoeven te worden verzonden naar Microsoft moeten worden gedefinieerd vóór het verzenden van een aanvraag voor de initiële implementatie:

- **VNet-adresruimte:** zoals al eerder zijn ingevoerd, is of zijn de IP-adres range(s) u hebt toegewezen (of wilt toewijzen) aan uw adresruimte parameter in de virtuele Azure-netwerken (VNet) verbinding maken met de grote exemplaar voor SAP HANA-omgeving. Het is raadzaam dat deze parameter adresruimte waarde met meerdere regels bestaat uit de Azure VM-Subnet meerdere bereiken en het bereik van de Azure-Gateway-subnet, zoals wordt weergegeven in de afbeeldingen eerder is. Dit bereik mogen elkaar niet overlappen met uw on-premises of Server IP-groep of ER P2P-adresbereiken. Hoe kunt u dit of deze IP-adresbereiken? Uw bedrijfsnetwerk team of service provider moet een of meerdere IP-adresbereiken, maar is of niet worden gebruikt in uw netwerk bieden. Voorbeeld: Als uw Azure VM-Subnet (Zie eerder) 10.0.1.0/24 is en uw Azure-Gateway-Subnet (Zie de volgende) 10.0.2.0/28, vervolgens uw Azure VNet-adresruimte is het raadzaam om te worden twee regels; 10.0.1.0/24 en 10.0.2.0/28. Hoewel de adresruimte-waarden kunnen worden samengevoegd, wordt u aangeraden overeenstemming brengen met de subnet-bereiken om te voorkomen dat per ongeluk hergebruik van niet-gebruikte IP-adresbereiken in grotere adresruimten in de toekomst ergens anders in uw netwerk. **De VNET-adresruimte is een IP-adresbereik op dat worden verzonden naar Microsoft moet tijdens het aanvragen van een aanvankelijke implementatie**

- **Azure VM-subnet IP-adresbereik:** dit IP-adresbereik, zoals eerder al besproken is die u hebt toegewezen (of abonnement toewijzen) aan de parameter Azure VNet subnet in uw Azure-VNET verbinding te maken met de grote exemplaar voor SAP HANA-omgeving. Dit IP-adresbereik wordt gebruikt voor het IP-adressen toewijzen aan uw Azure VM's. De IP-adressen buiten dit bereik zijn verbinding maken met uw grote SAP HANA-exemplaar (s) toegestaan. Indien nodig, kunnen meerdere Azure VM-subnetten kunnen worden gebruikt. Een/24 CIDR-blok door Microsoft wordt aanbevolen voor elke Azure VM-Subnet. Dit adresbereik moet een deel van de waarden die worden gebruikt in de Azure-VNet-adresruimte. Het ophalen van deze IP-adresbereik? Uw bedrijfsnetwerk team of service provider moet een IP-adresbereik op dat momenteel niet wordt gebruikt in uw netwerk bieden.

- **VNet-Gateway-Subnet IP-adresbereik:** , afhankelijk van de functies die u wilt gebruiken, zijn de aanbevolen grootte:
   - Ultra-performance ExpressRoute-gateway: / 26-Adresblok - vereist voor de klasse Type II van SKU's
   - Integratie met VPN- en ExpressRoute met behulp van een High-performance ExpressRoute-Gateway (of kleinere): / 27-Adresblok
   - Alle andere situaties: / 28-Adresblok. Dit adresbereik moet een deel van de waarden die worden gebruikt in de waarden 'VNet-adresruimte'. Dit adresbereik moet een deel van de waarden in de Azure VNet-adresruimte-waarden die u wilt verzenden naar Microsoft. Het ophalen van deze IP-adresbereik? Uw bedrijfsnetwerk team of service provider moet een IP-adresbereik op dat momenteel niet wordt gebruikt in uw netwerk bieden. 

- **-Adresbereik voor ER P2P-connectiviteit:** dit is het IP-bereik voor de verbinding met SAP HANA grote exemplaar ExpressRoute (ER) P2P. Dit bereik van IP-adressen moet een/29 CIDR IP-adresbereik. Dit bereik mogen elkaar niet overlappen met uw on-premises of andere Azure-IP-adresbereiken. Dit IP-adresbereik wordt gebruikt voor het instellen van de connectiviteit ER van uw Azure VNet ExpressRoute-Gateway aan de servers grote SAP HANA-exemplaar. Het ophalen van deze IP-adresbereik? Uw bedrijfsnetwerk team of service provider moet een IP-adresbereik op dat momenteel niet wordt gebruikt in uw netwerk bieden. **Dit bereik is een IP-adresbereik op dat worden verzonden naar Microsoft moet tijdens het aanvragen van een aanvankelijke implementatie**
  
- **Server IP-Adresgroepbereik:** dit IP-adresbereik is gebruikt voor de afzonderlijke IP-adres toewijzen aan HANA grote exemplaar servers. De subnetgrootte van de aanbevolen is een/24 CIDR blokkeren - maar indien nodig kan zijn kleinere tot een minimum van het bieden van 64 IP-adressen. In dit bereik, worden de eerste 30 IP-adressen gereserveerd voor gebruik door Microsoft. Zorg ervoor dat dit feit wordt verwerkt bij het kiezen van de grootte van het bereik. Dit bereik mogen elkaar niet overlappen met uw on-premises of andere Azure-IP-adressen. Het ophalen van deze IP-adresbereik? Uw bedrijfsnetwerk team of service provider moet een IP-adresbereik dat momenteel niet in uw netwerk gebruikt wordt bieden. Een/24 unieke CIDR (aanbevolen) blokkeren om te worden gebruikt voor het toewijzen van de specifieke IP-adressen die nodig zijn voor SAP HANA in Azure (grote exemplaren). **Dit bereik is een IP-adresbereik op dat worden verzonden naar Microsoft moet tijdens het aanvragen van een aanvankelijke implementatie**
 
Hoewel u moet definiëren en de bovenstaande IP-adresbereiken plannen, moeten ze niet alle naar Microsoft worden verzonden. Om samen te vatten het bovenstaande, zijn de IP-adresbereiken die u moet de naam van Microsoft:

- Azure VNet adres Space(s)
- Adresbereik voor ER P2P-connectiviteit
- Server IP-adresbereik van adresgroep

Aanvullende VNets die verbinding moeten maken met de grote exemplaren HANA toevoegt, moet u de nieuwe Azure VNet-adresruimte u naar Microsoft toevoegen wilt verzenden. 

Hieronder volgt een voorbeeld van de andere bereiken en enkele voorbeeld-bereiken als u wilt configureren en uiteindelijk naar Microsoft. Zoals u ziet, is de waarde voor de Azure-VNet-adresruimte wordt niet geaggregeerd in het eerste voorbeeld, maar is gedefinieerd in het bereik van de eerste Azure VM-subnet IP-adresbereik en het VNet Gateway-Subnet IP-adresbereik. Meerdere VM-subnetten binnen het Azure VNet zou werken dienovereenkomstig door configureren en het verzenden van de extra IP-adresbereiken van de aanvullende VM-subnetten voor als onderdeel van de Azure-VNet-adresruimte.

![IP-adresbereiken in SAP HANA vereist op minimale implementatie van Azure (grote exemplaren)](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Hebt u ook de mogelijkheid van de samenvoeging van de gegevens die u naar Microsoft verzenden. In dat geval zou de adresruimte van het Azure VNet alleen bevatten één spatie. Met behulp van de IP-adresbereiken die eerder in dit voorbeeld gebruikt. Deze cumulatieve VNet-adresruimte kan eruitzien als:

![Tweede mogelijkheid van IP-adresbereiken in SAP HANA vereist op minimale implementatie van Azure (grote exemplaren)](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Zoals u ziet, in plaats van twee kleinere bereiken die gedefinieerd van de adresruimte van het Azure VNet, hebben we een groter bereik die betrekking heeft op 4096 IP-adressen. Een grote definitie van de adresruimte laat de ongebruikte sommige in plaats daarvan grote gegevensreeksen. Omdat de VNet-adresruimte waarde(n) worden gebruikt voor het doorgeven van de BGP-route, kan informatie over het gebruik van de ongebruikte bereiken on-premises of elders in uw netwerk routering problemen veroorzaken. Het is dus raadzaam te houden van de adresruimte nauw uitgelijnd met de werkelijke subnetadresruimte gebruikt. Indien nodig, zonder uitvaltijd op het VNet, kunt u nieuwe waarden van de adresruimte altijd later toevoegen.
 
> [!IMPORTANT] 
> Elk IP-adres bereik van ER-P2P, IP-servergroep, Azure VNet-adresruimte moet **niet** met elkaar overlappen of een ander bereik ergens anders in uw netwerk gebruikt; elk moet discreet zijn en als de twee afbeeldingen eerdere weergeven niet mogelijk een subnet van een ander bereik. Als er worden overlappingen tussen bereiken optreden, de Azure-VNet kan geen verbinding maken met het ExpressRoute-circuit.

### <a name="next-steps-after-address-ranges-have-been-defined"></a>Vervolgstappen na de adresbereiken zijn gedefinieerd
Nadat de IP-adresbereiken zijn gedefinieerd, moeten de volgende activiteiten gebeuren:

1. De IP-adresbereiken voor Azure VNet-adresruimte, ER P2P-connectiviteit en Server IP-Adresgroepbereik, samen met andere gegevens die aan het begin van het document is aangeboden indienen. Op dit moment, u ook kan worden gestart om het VNet en de VM-subnetten te maken. 
2. Een Express Route-circuit is gemaakt door Microsoft tussen uw Azure-abonnement en de stempel HANA grote exemplaar.
3. Een tenantnetwerk wordt gemaakt op de grote exemplaar stempel door Microsoft.
4. Microsoft configureert netwerken in de SAP HANA op de infrastructuur van Azure (grote exemplaren) te accepteren van IP-adressen van uw Azure VNet-adresruimte die communiceert met grote HANA-exemplaren.
5. Afhankelijk van de specifieke SAP HANA op Azure (grote exemplaren) SKU hebt aangeschaft, Microsoft wijst een compute-eenheid in een tenantnetwerk toewijzen en opslag koppelen en installeer het besturingssysteem (SUSE of Red Hat Linux). IP-adressen voor deze eenheden worden uitgevoerd buiten de groep met IP-adresbereik u verzonden naar Microsoft.

Aan het einde van het implementatieproces biedt Microsoft de volgende gegevens u:
- De informatie die nodig zijn voor uw Azure-VNet(s) verbinding met het ExpressRoute-circuit dat verbinding Azure VNets met HANA grote exemplaren maakt:
     - Autorisatie sleutel (s)
     - ExpressRoute PeerID
- Gegevens voor toegang tot grote exemplaren HANA nadat u ExpressRoute-circuit en Azure VNet tot stand gebracht.

U kunt ook de volgorde van de grote exemplaren HANA verbinden in het document vinden [complete Setup voor SAP HANA grote exemplaren](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Veel van de volgende stappen worden weergegeven in een voorbeeldimplementatie in dat document. 


## <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>Verbinding maken met een VNet aan HANA grote exemplaar ExpressRoute

Als u alle IP-adresbereiken gedefinieerd en nu hebt u de gegevens terug van Microsoft, kunt u beginnen het VNet dat u hebt gemaakt voordat u verbinding met grote HANA-exemplaren. Zodra de Azure-VNet is gemaakt, moet een ExpressRoute-gateway op de VNet-naar de VNet koppelen aan het ExpressRoute-circuit dat verbinding met de klant-tenant met tijdstempel grote exemplaar maakt worden gemaakt.

> [!NOTE] 
> Deze stap duurt maximaal 30 minuten om uit te voeren, zoals de nieuwe gateway is gemaakt in het toegewezen Azure-abonnement en met het opgegeven Azure VNet verbonden.

Als er al een gateway bestaat, controleert u of het een ExpressRoute-gateway of niet is. Zo niet, de gateway moet worden verwijderd en opnieuw gemaakt als een ExpressRoute-gateway. Als een ExpressRoute-gateway al gemaakt is, omdat de Azure-VNet al met het ExpressRoute-circuit voor lokale connectiviteit verbonden is, gaat u verder naar de sectie VNets koppelen.

- Gebruik ofwel de (nieuw) [Azure-portal](https://portal.azure.com/), of PowerShell voor het maken van een ExpressRoute VPN-gateway is verbonden met uw VNet.
  - Als u de Azure-portal gebruikt, voegt u een nieuwe **virtuele netwerkgateway** en selecteer vervolgens **ExpressRoute** als het Gatewaytype.
  - Als u in plaats daarvan PowerShell kiest, eerst downloaden en gebruiken van de meest recente [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) om te controleren of een optimale ervaring. De volgende opdrachten maken een ExpressRoute-gateway. De tekst wordt voorafgegaan door een  _$_  zijn door de gebruiker gedefinieerde variabelen die moeten worden bijgewerkt met de specifieke informatie.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

In dit voorbeeld is de HighPerformance gateway SKU gebruikt. Uw opties zijn HighPerformance of UltraPerformance als de enige gateway-SKU's die worden ondersteund voor SAP HANA in Azure (grote exemplaren).

> [!IMPORTANT]
> Voor grote HANA-exemplaren van de SKU van het type S384, S384m S384xm, S576, S768 en S960 (Type II klasse SKU's), het gebruik van de Gateway-SKU UltraPerformance is verplicht.

### <a name="linking-vnets"></a>Vnet's koppelen

Nu dat het Azure VNet een ExpressRoute-gateway heeft, kunt u de autorisatie-informatie die is geleverd door Microsoft verbinding maken met de ExpressRoute-gateway aan de SAP HANA in Azure (grote exemplaren) ExpressRoute-circuit gemaakt voor deze connectiviteit. Deze stap kan worden uitgevoerd met de Azure-portal of PowerShell. De portal wordt aanbevolen, PowerShell-instructies zijn echter als volgt. 

- Uitvoeren van de volgende opdrachten voor elk VNet-gateway met behulp van een andere AuthGUID voor elke verbinding. De eerste twee items in het volgende script afkomstig zijn van de informatie die wordt geleverd door Microsoft. Er is ook de AuthGUID specifiek is voor elk VNet en de gateway. Dus, als u wilt toevoegen van een andere Azure-VNet, moet u een andere AuthID voor uw ExpressRoute-circuit dat verbinding HANA grote exemplaren in Azure maakt. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Als u verbinding maken met de gateway voor meerdere ExpressRoute-circuits die gekoppeld aan uw abonnement wilt zijn, moet u mogelijk meer dan één keer uitvoeren van deze stap. Bijvoorbeeld, gaat u waarschijnlijk verbinding maken met het hetzelfde VNet-Gateway aan de ExpressRoute-circuit dat het VNet naar uw on-premises netwerk verbindt.

## <a name="adding-more-ip-addresses-or-subnets"></a>Meer IP-adressen of subnetten toe te voegen

Gebruik de Azure-portal, PowerShell of CLI bij het toevoegen van meer IP-adressen of subnetten.

In dit geval is de aanbeveling het nieuwe IP-adresbereik toevoegen als nieuw bereik aan de VNet-adresruimte in plaats van een nieuw bereik geaggregeerde genereren. In beide gevallen moet u deze wijziging naar Microsoft connectiviteit buiten die nieuwe IP-adresbereik aan de eenheden grote HANA-exemplaar dat is toegestaan in de client verzenden. U kunt een aanvraag voor de ondersteuning van Azure ophalen van de nieuwe VNet adresruimte toegevoegd openen. Nadat u een bevestiging ontvangen, moet u de volgende stappen uitvoeren.

Zie het artikel voor informatie over het maken van een ander subnet van de Azure-portal [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), en voor het maken van PowerShell, Zie [maken van een virtueel netwerk met behulp van PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="adding-vnets"></a>Vnet's toe te voegen

Nadat u in eerste instantie verbinding maakt met een of meer Azure VNets, wilt u mogelijk extra velden die toegang SAP HANA in Azure (grote exemplaren tot) toevoegen. Eerst een ondersteuning van Azure-aanvraag verzendt, in die aanvraag zowel de specifieke informatie bevatten identificatie van de specifieke Azure-implementatie en de IP-adresbereiken ruimte van de Azure VNet-adresruimte. SAP HANA op Azure Service Management geeft vervolgens de benodigde informatie die u wilt verbinding maken met de extra vnet's en ExpressRoute. Voor elk VNet moet u een unieke sleutel autorisatie verbinding maken met het ExpressRoute-Circuit aan HANA grote exemplaren.

Stappen voor het toevoegen van een nieuwe Azure VNet:

1. Voltooid de eerste stap bij het voorbereidingsproces, Zie de _maken van Azure VNet_ sectie.
2. Voltooid de tweede stap van het voorbereidingsproces, Zie de _gatewaysubnet maken_ sectie.
3. De aanvullende VNets verbinden aan HANA grote exemplaar ExpressRoute-circuit, opent u een Azure-ondersteuningsaanvraag met informatie over de nieuwe VNet en vraagt u een nieuwe autorisatie-sleutel.
4. Wanneer een melding dat de autorisatie is voltooid, gebruik de Microsoft geleverde autorisatie-informatie voor het voltooien van de derde stap in het voorbereidingsproces, Zie de _VNets koppelen_ sectie.

## <a name="increasing-expressroute-circuit-bandwidth"></a>ExpressRoute-circuit bandbreedte vergroten

Neem contact op met de SAP HANA op Azure Service Management. Als u wordt aangeraden te verhogen van de bandbreedte van het SAP HANA op Azure (grote exemplaren) ExpressRoute-circuit, maakt u een aanvraag voor de ondersteuning van Azure. (U kunt een verhoging van het voor een enkel circuit bandbreedte maximaal 10 Gbps aanvragen.) U ontvangt vervolgens een melding nadat de bewerking voltooid is. geen verdere actie nodig is voor het inschakelen van deze hogere snelheid in Azure.

## <a name="adding-an-additional-expressroute-circuit"></a>Toevoegen van een extra ExpressRoute-circuit

Neem contact op met de SAP HANA op Azure Service Management als u wordt aangeraden dat een extra ExpressRoute-circuit is vereist, moet een aanvraag voor het maken van nieuwe ExpressRoute-circuit (en verbinding maken met het autorisatie-informatie ophalen) ondersteunen Azure. De adresruimte die gaat worden gebruikt voor de VNets die moet worden gedefinieerd voordat u de aanvraag, in volgorde voor SAP HANA op Azure-servicebeheer voor autorisatie.

Nadat het nieuwe circuit is gemaakt en de SAP HANA van Azure Service Management-configuratie voltooid is, gaat u ontvangen met de informatie die dat u nodig hebt om door te gaan. Volg de stappen die hierboven is opgegeven voor het maken en de nieuwe VNet verbinding te maken met deze aanvullende circuit. U kan geen verbinding maken met Azure VNets dit circuit aanvullende als ze al met een andere SAP HANA op Azure (grote exemplaar) ExpressRoute-circuit in dezelfde Azure-regio verbonden.

## <a name="deleting-a-subnet"></a>Verwijderen van een subnet

Als u wilt verwijderen van een VNet subnet, kan de Azure-portal, PowerShell of CLI worden gebruikt. Als uw Azure VNet IP-adres bereik/Azure VNet-adresruimte een cumulatieve bereik is, is er geen volgen voor u met Microsoft. Behalve dat het VNet is nog steeds de adresruimte van het BGP-route met het verwijderde subnet wordt doorgegeven. Als u het Azure VNet IP-adres bereik/Azure VNet-adresruimte gedefinieerd als meerdere IP-adresbereiken waarvan een is toegewezen aan uw verwijderde subnet, moet u die buiten uw VNet-adresruimte te verwijderen en vervolgens SAP HANA op Azure-servicebeheer te informeren verwijderen van de bereiken die SAP HANA in Azure (grote exemplaren) om te communiceren met is toegestaan.

Terwijl er nog specifieke, toegewezen Azure.com richtlijnen over het verwijderen van subnetten, het proces voor het verwijderen van subnetten de achterzijde van het proces is voor deze toe te voegen. Zie het artikel [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over het maken van subnetten.

## <a name="deleting-a-vnet"></a>Verwijderen van een VNet

Gebruik de Azure-portal, PowerShell of CLI bij het verwijderen van een VNet. SAP HANA op Azure Service Management verwijdert de bestaande goedkeuringen op de SAP HANA op Azure (grote exemplaren) ExpressRoute-circuit en verwijderen van het Azure VNet IP-adres bereik/Azure VNet-adresruimte voor de communicatie met grote HANA-exemplaren.

Zodra het VNet is verwijderd, opent u een aanvraag voor ondersteuning van Azure om de IP-adresbereiken ruimte worden verwijderd.

Terwijl er nog niet specifiek, speciale Azure.com richtlijnen over het verwijderen van vnet's, het proces voor het verwijderen van VNets is de achterzijde van het proces voor deze toe te voegen, die hierboven wordt beschreven. Zie de artikelen [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [maken van een virtueel netwerk met behulp van PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over het maken van VNets.

Om te controleren of dat alles is verwijderd, verwijdert u de volgende items:

- De ExpressRoute-verbinding, VNet-Gateway, openbare IP-VNet-Gateway en VNet

## <a name="deleting-an-expressroute-circuit"></a>Verwijderen van een ExpressRoute-circuit

Als u wilt verwijderen van een extra SAP HANA op Azure (grote exemplaren) ExpressRoute-circuit, een aanvraag voor de ondersteuning van Azure met SAP HANA op Azure Service Management openen en aanvragen dat het circuit moet worden verwijderd. U kunt binnen de Azure-abonnement verwijderen of behouden van de VNet indien nodig. Echter, moet u de verbinding tussen het HANA grote exemplaren ExpressRoute-circuit en de gekoppelde VNet gateway verwijderen.

Als u ook wilt verwijderen van een VNet, volgt u de instructies over het verwijderen van een VNet in de bovenstaande sectie.


