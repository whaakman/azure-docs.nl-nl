---
title: Verbindingsconfiguratie van virtuele machines met SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Verbindingsconfiguratie van virtuele machines voor het gebruik van SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90d920a501d27ca21b1c2b7b7f5491cebb2c2822
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233715"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Azure-VM's verbinden met HANA grote instanties

Zoals vermeld [SAP HANA (grote instanties) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture), de minimale implementatie van HANA grote instanties met het niveau van de SAP-toepassing in Azure uitziet:

![Azure VNet dat is verbonden met SAP HANA op Azure (grote instanties) en on-premises](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Op zoek dichter bij aan de kant van de Azure-netwerk, we de noodzaak van de volgende voordelen:

- De definitie van een Azure-netwerk waarin u wilt implementeren van de virtuele machines van de SAP-toepassingslaag.
- De definitie van een standaard-subnet in het Azure-netwerk dat is echt een waarin de virtuele machines zijn geïmplementeerd.
- Het Azure-netwerk dat gemaakt, moet ten minste één VM-subnet en een gatewaysubnet voor virtueel netwerk van Azure ExpressRoute hebben. Deze subnetten moeten de IP-adresbereiken opgegeven en die wordt beschreven in de volgende secties worden toegewezen.

Laten we eens iets dichter bij het maken van virtuele Azure-netwerk voor HANA grote instanties.

## <a name="create-the-azure-virtual-network-for-hana-large-instances"></a>Maken van de Azure-netwerk voor HANA grote instanties

>[!Note]
>Het Azure-netwerk voor HANA grote instanties moet worden gemaakt met behulp van het Azure Resource Manager-implementatiemodel. De oudere Azure-implementatiemodel, bekend als het klassieke implementatiemodel wordt niet ondersteund door de oplossing HANA grote instantie.

U kunt de Azure-portal, PowerShell, een Azure-sjabloon of de Azure CLI gebruiken om te maken van het virtuele netwerk. (Zie voor meer informatie, [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). In het volgende voorbeeld kijken we een virtueel netwerk dat gemaakt met behulp van de Azure-portal.

We zien hoe de definities van de virtuele netwerken in verband met wat we als verschillende IP-adresbereiken vermelden. Wanneer we praten over de **adresruimte**, bedoelen we de adresruimte die het Azure-netwerk kunnen gebruiken. Deze adresruimte is ook het adresbereik dat het virtuele netwerk wordt gebruikt voor het doorgeven van BGP-route. Dit **adresruimte** kunnen hier worden weergegeven:

![Adresruimte van een Azure-netwerk weergegeven in de Azure portal](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

In het vorige voorbeeld, met 10.16.0.0/16, is het Azure-netwerk een in plaats van grote en breed IP-adresbereik te gebruiken opgegeven. Alle IP-adresbereiken van de volgende subnetten in dit virtuele netwerk kunnen daarom hun bereiken hebben in deze adresruimte. Meestal niet aangeraden deze een grote adresbereik voor één virtueel netwerk in Azure. Maar laten we bekijken de subnetten die zijn gedefinieerd in de Azure-netwerk:

![Azure-netwerksubnetten en hun IP-adresbereiken](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

We kijken naar een virtueel netwerk met een eerste VM-subnet (hier 'standaard' genoemd) en een subnet met de naam 'GatewaySubnet'.

In de volgende sectie, verwijzen we naar het IP-adresbereik van het subnet, dat 'standaard' in de afbeeldingen als aangeroepen is **Azure VM-subnet IP-adresbereik**. Verwijzen we naar het IP-adresbereik van het gatewaysubnet als de **virtueel netwerk IP-adresbereik van gatewaysubnet**. 

In de vorige twee afbeeldingen, de **virtuele netwerkadresruimte** behandelt de **Azure VM-subnet IP-adresbereik** en de **virtueel netwerk IP-adresbereik van gatewaysubnet**. 

Als u met de IP-adresbereiken worden wilt besparen, kunt u beperken de **virtuele netwerkadresruimte** van een virtueel netwerk op de specifieke bereiken die worden gebruikt door elk subnet. U kunt definiëren de **virtuele netwerkadresruimte** van een virtueel netwerk als meerdere specifieke bereiken, zoals hier wordt weergegeven:

![Adresruimte van Azure-netwerk met twee spaties](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

In dit geval de **virtuele netwerkadresruimte** heeft twee spaties gedefinieerd. Ze zijn hetzelfde als de IP-adresbereiken die zijn gedefinieerd voor de **Azure VM-subnet IP-adresbereik** en de **virtueel netwerk IP-adresbereik van gatewaysubnet.**

U kunt een naamgevingsnorm die u wilt gebruiken voor deze tenant subnetten (VM-subnetten). Echter, **moet altijd er één en slechts één gateway-subnet voor elk virtueel netwerk** die verbinding maakt met de SAP HANA op Azure (grote instanties) ExpressRoute-circuit. En **deze gatewaysubnet heeft de naam 'GatewaySubnet'** om ervoor te zorgen dat de ExpressRoute-gateway correct is geplaatst.

> [!WARNING] 
> Het is essentieel dat het gatewaysubnet altijd worden met de naam 'GatewaySubnet'.

U kunt meerdere VM-subnetten en niet-aaneengesloten-adresbereiken. Deze adresbereiken moeten worden gedekt door de **virtuele netwerkadresruimte** van het virtuele netwerk. Ze kunnen zich in een geaggregeerde vorm of in een lijst met de exacte bereiken van de VM-subnetten en het gatewaysubnet.

Hieronder volgt een samenvatting van de belangrijke informatie over een Azure-netwerk die verbinding met HANA grote instanties maakt:

- U dient de **virtuele netwerkadresruimte** naar Microsoft bij het uitvoeren van een aanvankelijke implementatie van HANA grote instanties. 
- De **virtuele netwerkadresruimte** mag een groter bereik die betrekking heeft op het bereik voor de Azure VM-subnet IP-adresbereiken en het virtuele netwerk gateway-subnet IP-adresbereik.
- Of u kunt indienen meerdere adresbereiken die betrekking hebben op de verschillende IP-adresbereiken van VM-subnet IP-adresbereiken en het virtuele netwerk gateway IP-adresbereik.
- De gedefinieerde **virtuele netwerkadresruimte** wordt gebruikt voor het doorgeven van BGP-routering.
- De naam van het gatewaysubnet moet: **'GatewaySubnet'**.
- De adresruimte die wordt gebruikt als een filter aan de HANA grote instantie wilt toestaan of weigeren van verkeer naar de eenheden HANA grote instantie van Azure. Zorg ervoor dat het BGP routering van informatie van de virtuele Azure-netwerk en het IP-adresbereiken die zijn geconfigureerd voor het filteren op de overeenkomst van HANA grote instantie aan clientzijde. Anders kunnen er verbindingsproblemen optreden.
- Er zijn een aantal details van het gatewaysubnet die later worden besproken in de sectie **een virtueel netwerk verbinden met HANA grote instantie ExpressRoute.**



## <a name="different-ip-address-ranges-to-be-defined"></a>Verschillende IP-adresbereiken worden gedefinieerd 

We al enkele van de IP-adresbereiken die nodig zijn voor het implementeren van HANA grote instanties zijn geïntroduceerd. Maar er zijn meer IP-adresbereiken die ook belangrijk zijn. Hieronder volgen enkele meer informatie. De volgende IP-adressen moeten niet allemaal worden verzonden naar Microsoft. U moet echter ze definiëren voordat een aanvraag voor de eerste implementatie wordt verzonden:

- **Virtuele netwerkadresruimte**: geïntroduceerde eerder de **virtuele netwerkadresruimte** is het IP-adresbereik (of bereiken) dat u hebt toegewezen (of van plan bent om toe te wijzen) naar uw adresruimte-parameter in de virtuele Azure de netwerken die verbinding met de SAP HANA grote instantie-omgeving maken.

 U wordt aangeraden dat deze parameter adresruimte de waarde van een meerdere regels bestaat uit de Azure-VM-subnet meerdere bereiken en het bereik van de Azure-gateway-subnet, zoals wordt weergegeven in de vorige afbeeldingen is. Dit bereik mag niet overlappen met uw on-premises of server IP-adresgroep of ER P2P-adresbereiken. 
 
Hoe ontvang ik deze IP-adresbereiken? 

Uw bedrijfsnetwerk team of de service-provider moet een of meerdere IP-adres adresbereiken die niet worden gebruikt in uw netwerk bieden. Bijvoorbeeld, als uw Azure-VM-subnet 10.0.1.0/24 is en uw Azure-gateway-subnet 10.0.2.0/28 is, klikt u vervolgens de adresruimte van uw Azure-netwerk is raadzaam twee regels: 10.0.1.0/24 en 10.0.2.0/28. 

Hoewel de waarden voor de adres kunnen worden samengevoegd, wordt u aangeraden ze te vergelijken met de subnet-bereiken. Op deze manier kunt voorkomen u dat per ongeluk niet-gebruikte IP-adresbereiken in grotere adresruimten in de toekomst ergens anders in uw netwerk. **De adresruimte van het virtuele netwerk is een IP-adresbereik op dat worden verzonden naar Microsoft moet wanneer u om een initiële implementatie vraagt**.

- **Azure VM-subnet IP-adresbereik:** deze IP-adresbereik, zoals eerder besproken is degene die u hebt toegewezen (of van plan om toe te wijzen) naar de parameter voor het subnet van virtuele Azure-netwerk in uw Azure-netwerk die verbinding met SAP HANA grote instantie maakt -omgeving. Deze IP-adresbereik wordt gebruikt om IP-adressen toewijzen aan uw Azure VM's. De IP-adressen buiten dit bereik zijn toegestaan verbinding maken met uw SAP HANA grote instantie (s). Indien nodig, kunt u meerdere Azure-VM-subnetten. We raden aan een/24 CIDR-blokkering voor elke Azure-VM-subnet. Dit adresbereik moet een deel van de waarden die worden gebruikt in de adresruimte van Azure-netwerk. 

Hoe ontvang ik deze IP-adresbereik? 

Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat niet wordt gebruikt in uw netwerk bieden.

- **Virtueel netwerk IP-adresbereik gatewaysubnet:** , afhankelijk van de functies die u wilt gebruiken, de aanbevolen grootte is:
   - Ultra-performance ExpressRoute-gateway: / 26-Adresblok--vereist voor de klasse van SKU's Type II.
   - Co-existentie met VPN en ExpressRoute met behulp van een gateway met hoge prestaties ExpressRoute virtueel netwerk (of lager): / 27-Adresblok.
   - Alle andere gevallen: / 28-Adresblok. Dit adresbereik moet een deel van de waarden in de waarden 'VNet-adresruimte'. Dit adresbereik moet een deel van de waarden die worden gebruikt in de Azure-netwerk adresruimte waarden die u naar Microsoft verzenden. Hoe ontvang ik deze IP-adresbereik? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat momenteel niet wordt gebruikt in uw netwerk bieden. 

- **Adresbereik voor ER-P2P-connectiviteit:** dit is het IP-bereik voor uw SAP HANA grote instantie ExpressRoute (ER) P2P-verbinding. Dit bereik van IP-adressen moet een/29 CIDR-IP-adresbereik. Dit bereik mag niet overlappen met uw on-premises of andere Azure-IP-adresbereiken. Deze IP-adresbereik wordt gebruikt voor het instellen van de connectiviteit ER van uw virtuele ExpressRoute-gateway naar de SAP HANA grote instantie-servers. Hoe ontvang ik deze IP-adresbereik? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat momenteel niet wordt gebruikt in uw netwerk bieden. **Dit bereik is een IP-adresbereik op dat worden verzonden naar Microsoft moet tijdens het aanvragen van een aanvankelijke implementatie**.
  
- **Server IP-adresgroepbereik:** dit IP-adresbereik wordt gebruikt voor de afzonderlijke IP-adres toewijzen aan HANA grote instantie servers. De subnetgrootte van de aanbevolen is een/24 CIDR-blok. Indien nodig, kan het zijn kleiner, met slechts 64 IP-adressen. Uit dit bereik, worden de eerste 30 IP-adressen gereserveerd voor gebruik door Microsoft. Zorg ervoor dat voor dit account te maken wanneer u de grootte van het bereik te kiezen. Dit bereik mag niet overlappen met uw on-premises of andere Azure-IP-adressen. Hoe ontvang ik deze IP-adresbereik? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat momenteel niet wordt gebruikt in uw netwerk bieden. Een/24 (aanbevolen) unieke CIDR-blok moet worden gebruikt voor het toewijzen van de specifieke IP-adressen is nodig voor SAP HANA op Azure (grote instanties). **Dit bereik is een IP-adresbereik op dat worden verzonden naar Microsoft moet tijdens het aanvragen van een aanvankelijke implementatie**.
 
Hoewel u nodig hebt om te definiëren en plan de IP-adresbereiken die eerder zijn beschreven, moet u niet al deze naar Microsoft worden verzonden. De IP-adresbereiken die u nodig zijn voor de naam van Microsoft zijn:

- Azure-netwerk adres space(s)
- Adresbereik voor ER-P2P-connectiviteit
- IP-adresgroepbereik dat Server

Als u aanvullende virtuele netwerken die verbinding maken met HANA grote instanties wilt toevoegen, hebt u de nieuwe Azure-netwerk-adresruimte die u aan Microsoft toevoegt te verzenden. 

Hieronder volgt een voorbeeld van de verschillende bereiken en enkele voorbeeld-bereiken als u nodig hebt om te configureren en uiteindelijk aan Microsoft worden verstrekt. De waarde voor de adresruimte van Azure-netwerk wordt niet geaggregeerd in het eerste voorbeeld, maar is gedefinieerd in het bereik van de eerste Azure-VM-subnet IP-adresbereik en het virtuele netwerk gateway-subnet IP-adresbereik. 

U kunt meerdere VM-subnetten binnen het Azure-netwerk gebruiken wanneer u configureert en verzendt u de extra IP-adresbereiken van de aanvullende VM-subnetten als onderdeel van de adresruimte van Azure-netwerk.

![IP-adresbereiken in SAP HANA op Azure (grote instanties) minimale implementatie vereist](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

U kunt ook de gegevens die u naar Microsoft verzenden samenvoegen. In dat geval bevat de adresruimte van het Azure-netwerk alleen een spatie. Met behulp van de IP-adresbereiken in het vorige voorbeeld, de samengevoegde virtuele netwerkadresruimte kan er uitzien zoals in de volgende afbeelding:

![Tweede mogelijkheid van IP-adresbereiken in SAP HANA op Azure (grote instanties) minimale implementatie vereist](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Zoals u ziet in het voorbeeld, in plaats van twee kleinere bereiken die gedefinieerd van de adresruimte van het Azure-netwerk, hebben we een groter bereik die betrekking heeft op 4096 IP-adressen. Een grote definitie van de adresruimte blijven sommige vrij groot bereiken niet-gebruikte. Omdat het virtuele netwerk adresruimte waarde(n) voor doorgifte van BGP-route worden gebruikt, gebruik van de ongebruikte bereiken on-premises of ergens anders in uw netwerk, kan leiden tot problemen met routering. 

Daarom is het raadzaam dat u de adresruimte die nauw zijn uitgelijnd met de werkelijke subnet-adresruimte die u gebruikt. Indien nodig, zonder downtime op het virtuele netwerk, kunt u de waarden voor nieuwe adres altijd later toevoegen.
 
> [!IMPORTANT] 
> Elk IP-adresbereik in ER P2P, de server IP-adresgroep en de adresruimte van Azure-netwerk moet **niet** overlappen met elkaar of met andere bereik dat wordt gebruikt in uw netwerk. Elk moet discreet zijn. Als de twee voorgaande afbeeldingen weergeven, mag ze ook niet een subnet van een ander bereik. Als er overlapping optreden tussen bereiken, Azure virtual network kan geen verbinding maken met het ExpressRoute-circuit.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Vervolgstappen na de adresbereiken zijn gedefinieerd
Nadat de IP-adresbereiken zijn gedefinieerd, moeten de volgende dingen gebeuren:

1. De IP-adresbereiken voor de adresruimte van Azure-netwerk, de ER P2P-connectiviteit en server IP-adresgroepbereik, samen met andere gegevens die aan het begin van het document is aangeboden indienen. U kunt op dit moment ook starten om het virtuele netwerk en de VM-subnetten te maken. 
2. Een ExpressRoute-circuit wordt gemaakt door Microsoft tussen uw Azure-abonnement en de stempel HANA grote instantie.
3. Een tenantnetwerk is gemaakt op het stempel grote instantie door Microsoft.
4. Microsoft Hiermee configureert u netwerken in de SAP HANA op Azure (grote instanties)-infrastructuur om te accepteren van IP-adressen van uw Azure-netwerk-adresruimte die met HANA grote instanties communiceert.
5. Afhankelijk van de specifieke SAP HANA op Azure (grote instanties)-SKU die u hebt gekocht, Microsoft wijst een compute-eenheid in een tenantnetwerk toe, kan worden toegewezen en opslag koppelt en het besturingssysteem (SUSE of Red Hat Linux) wordt geïnstalleerd. IP-adressen voor deze eenheden zijn afkomstig uit de Server IP-Adresgroepbereik dat die u wilde verzenden naar Microsoft.

Microsoft biedt aan het einde van het implementatieproces start, de volgende gegevens in om u te:
- De informatie die nodig is om uw Azure virtuele netwerken verbinden met de ExpressRoute-circuit dat is verbonden virtuele netwerken van Azure met HANA grote instanties:
     - Autorisatie sleutel (s)
     - ExpressRoute PeerID
- Gegevens voor HANA grote instanties nadat u hebt toegang tot stand brengen van ExpressRoute-circuit en Azure-netwerk.

U vindt hier ook de volgorde van de verbinding te maken van HANA grote instanties in het document [SAP HANA op Azure (grote instanties) Setup](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Veel van de volgende stappen worden weergegeven in een voorbeeld van een implementatie in dat document. 

**Volgende stappen**

- Raadpleeg [een virtueel netwerk verbinden met HANA grote instantie ExpressRoute](hana-connect-vnet-express-route.md).
