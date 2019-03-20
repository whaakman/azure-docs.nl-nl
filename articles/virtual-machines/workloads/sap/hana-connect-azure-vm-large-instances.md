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
ms.openlocfilehash: 2628cafada47b2602b195c44d4b6f2e6b16012ef
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58092766"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Azure-VM's verbinden met HANA grote instanties

Het artikel [wat is er SAP HANA op Azure (grote instanties)?](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) vermeldingen die de minimale implementatie van HANA grote instanties met het niveau van de SAP-toepassing in Azure uitziet:

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

In de vorige twee afbeeldingen, de **virtuele netwerkadresruimte** behandelt de **het subnet IP-adresbereik van het Azure-VM** en die van de virtuele netwerkgateway.

U kunt beperken de **virtuele netwerkadresruimte** op de specifieke bereiken die worden gebruikt door elk subnet. U kunt ook definiëren de **virtuele netwerkadresruimte** van een virtueel netwerk als meerdere specifieke bereiken, zoals hier wordt weergegeven:

![Adresruimte van Azure-netwerk met twee spaties](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

In dit geval de **virtuele netwerkadresruimte** heeft twee spaties gedefinieerd. Ze zijn hetzelfde als de IP-adresbereiken die zijn gedefinieerd voor het subnet IP-adresbereik van de Azure-VM en de virtuele netwerkgateway. 

U kunt een naamgevingsnorm die u wilt gebruiken voor deze tenant subnetten (VM-subnetten). Echter, **moet altijd er één en slechts één gateway-subnet voor elk virtueel netwerk** die verbinding maakt met de SAP HANA op Azure (grote instanties) ExpressRoute-circuit. En **deze gatewaysubnet heeft de naam 'GatewaySubnet'** om ervoor te zorgen dat de ExpressRoute-gateway correct is geplaatst.

> [!WARNING] 
> Het is essentieel dat het gatewaysubnet altijd worden met de naam 'GatewaySubnet'.

U kunt meerdere VM-subnetten en niet-aaneengesloten-adresbereiken. Deze adresbereiken moeten worden gedekt door de **virtuele netwerkadresruimte** van het virtuele netwerk. Ze kunnen zich op een geaggregeerde vorm. Ze kunnen ook worden in een lijst met de exacte bereiken van de VM-subnetten en het gatewaysubnet.

Hieronder volgt een samenvatting van de belangrijke informatie over een Azure-netwerk die verbinding met HANA grote instanties maakt:

- U dient de **virtuele netwerkadresruimte** naar Microsoft bij het uitvoeren van een aanvankelijke implementatie van HANA grote instanties. 
- De **virtuele netwerkadresruimte** mag een groter bereik die betrekking heeft op het bereik voor zowel het subnet IP-adresbereik van de Azure-VM en de virtuele netwerkgateway.
- Of u kunt indienen meerdere adresbereiken die betrekking hebben op de verschillende IP-adresbereiken van VM-subnet IP-adresbereiken en het virtuele netwerk gateway IP-adresbereik.
- De gedefinieerde **virtuele netwerkadresruimte** wordt gebruikt voor het doorgeven van BGP-routering.
- De naam van het gatewaysubnet moet zijn: **"GatewaySubnet"**.
- De adresruimte die wordt gebruikt als een filter aan de HANA grote instantie wilt toestaan of weigeren van verkeer naar de eenheden HANA grote instantie van Azure. De BGP-routering informatie van de Azure-netwerk en de IP-adresbereiken die zijn geconfigureerd voor het filteren aan de HANA grote instantie moet overeenkomen met. Anders kunnen er verbindingsproblemen optreden.
- Er zijn een aantal details van het gatewaysubnet die later worden besproken in de sectie **een virtueel netwerk verbinden met HANA grote instantie ExpressRoute.**



## <a name="different-ip-address-ranges-to-be-defined"></a>Verschillende IP-adresbereiken worden gedefinieerd 

We al enkele van de IP-adresbereiken die nodig zijn voor het implementeren van HANA grote instanties zijn geïntroduceerd. Maar er zijn meer IP-adresbereiken die ook belangrijk zijn. Hieronder volgen enkele meer informatie. De volgende IP-adressen moeten niet allemaal worden verzonden naar Microsoft. U moet echter ze definiëren voordat een aanvraag voor de eerste implementatie wordt verzonden:

- **Virtuele netwerkadresruimte**: De **virtuele netwerkadresruimte** is het IP-adresbereiken die u aan de parameter adresruimte in de Azure-netwerken toewijst. Deze netwerken verbinden met de SAP HANA grote instantie-omgeving.

  U wordt aangeraden dat de parameter van dit adresruimte een waarde met meerdere regels is. Deze moet bestaan uit het subnetbereik van de Azure-VM en het subnet meerdere bereiken van de Azure-gateway. Deze subnetbereik werd weergegeven in de vorige afbeeldingen. Deze mag niet overlappen met uw on-premises of server IP-adresgroep of ER P2P-adresbereiken. 
 
Hoe ontvang ik deze IP-adresbereiken? 

Uw bedrijfsnetwerk team of de service-provider moet een of meerdere IP-adres adresbereiken die niet worden gebruikt in uw netwerk bieden. Stel dat bijvoorbeeld, het subnet van uw Azure-VM is 10.0.1.0/24 en het subnet van uw Azure-gateway-subnet is 10.0.2.0/28.  U wordt aangeraden dat de adresruimte van uw Azure-netwerk de volgende twee regels is: 10.0.1.0/24 en 10.0.2.0/28. 

Hoewel de waarden voor de adres kunnen worden samengevoegd, wordt u aangeraden ze te vergelijken met de subnet-bereiken. Op deze manier kunt u per ongeluk voorkomen dat niet-gebruikte IP-adresbereiken in grotere adresruimten ergens anders in uw netwerk. **De adresruimte van het virtuele netwerk is een IP-adresbereik. Ze moeten worden verzonden naar Microsoft wanneer u om een initiële implementatie vraagt**.

- **Azure VM-subnet IP-adresbereik:** Deze IP-adresbereik is degene die u aan de Azure-netwerk subnet-parameter toewijst. Deze parameter is in uw Azure-netwerk en maakt verbinding met de SAP HANA grote instantie-omgeving. Deze IP-adresbereik wordt gebruikt om IP-adressen toewijzen aan uw Azure VM's. De IP-adressen buiten dit bereik zijn toegestaan verbinding maken met uw SAP HANA grote instantie (s). Indien nodig, kunt u meerdere Azure-VM-subnetten. We raden aan een/24 CIDR-blokkering voor elke Azure-VM-subnet. Dit adresbereik moet een deel van de waarden die worden gebruikt in de adresruimte van Azure-netwerk. 

Hoe ontvang ik deze IP-adresbereik? 

Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat niet wordt gebruikt in uw netwerk bieden.

- **Virtueel netwerk IP-adresbereik gatewaysubnet:** Afhankelijk van de functies die u wilt gebruiken, is de aanbevolen grootte:
   - Ultra-performance ExpressRoute-gateway: / 26-Adresblok--vereist voor de klasse van SKU's Type II.
   - Co-existentie met VPN en ExpressRoute met behulp van een gateway met hoge prestaties ExpressRoute virtueel netwerk (of lager): / 27-Adresblok.
   - Alle andere gevallen: / 28-Adresblok. Dit adresbereik moet een deel van de waarden in de waarden 'VNet-adresruimte'. Dit adresbereik moet een deel van de waarden die worden gebruikt in de Azure-netwerk adresruimte waarden die u naar Microsoft verzenden. Hoe ontvang ik deze IP-adresbereik? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat momenteel niet wordt gebruikt in uw netwerk bieden. 

- **Adresbereik voor ER-P2P-verbindingen:** Dit is het IP-bereik voor uw SAP HANA grote instantie ExpressRoute (ER) P2P-verbinding. Dit bereik van IP-adressen moet een/29 CIDR-IP-adresbereik. Dit bereik mag niet overlappen met uw on-premises of andere Azure-IP-adresbereiken. Deze IP-adresbereik wordt gebruikt voor het instellen van de connectiviteit ER van uw virtuele ExpressRoute-gateway naar de SAP HANA grote instantie-servers. Hoe ontvang ik deze IP-adresbereik? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat momenteel niet wordt gebruikt in uw netwerk bieden. **Dit bereik is een IP-adresbereik. Ze moeten worden verzonden naar Microsoft wanneer u om een initiële implementatie vraagt**.
  
- **Server IP-adresgroepbereik dat:** Deze IP-adresbereik wordt gebruikt om de afzonderlijke IP-adres toewijzen aan HANA grote instantie servers. De subnetgrootte van de aanbevolen is een/24 CIDR-blok. Indien nodig, kan het zijn kleiner, met slechts 64 IP-adressen. Uit dit bereik, worden de eerste 30 IP-adressen gereserveerd voor gebruik door Microsoft. Zorg ervoor dat voor dit account te maken wanneer u de grootte van het bereik te kiezen. Dit bereik mag niet overlappen met uw on-premises of andere Azure-IP-adressen. Hoe ontvang ik deze IP-adresbereik? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat momenteel niet wordt gebruikt in uw netwerk bieden. 

  **Dit bereik is een IP-adresbereik op dat worden verzonden naar Microsoft moet tijdens het aanvragen van een aanvankelijke implementatie**.
 
U moet definiëren en plan de IP-adresbereiken die eerder zijn beschreven. U moet echter niet al deze naar Microsoft worden verzonden. De IP-adresbereiken die u nodig zijn voor de naam van Microsoft zijn:

- Azure-netwerk adres space(s)
- Adresbereik voor ER-P2P-connectiviteit
- IP-adresgroepbereik dat Server

Als u aanvullende virtuele netwerken die verbinding maken met HANA grote instanties wilt toevoegen, hebt u de nieuwe Azure-netwerk-adresruimte die u aan Microsoft toevoegt te verzenden. 

Hieronder volgt een voorbeeld van de verschillende bereiken en enkele voorbeeld-bereiken als u wilt configureren en uiteindelijk naar Microsoft. De waarde voor de adresruimte van Azure-netwerk wordt niet geaggregeerd in het eerste voorbeeld. Het is echter van de bereiken van de eerste Azure-VM-subnet IP-adresbereik en het virtuele netwerk IP-adresbereik van gatewaysubnet gedefinieerd. 

U kunt meerdere VM-subnetten binnen het Azure-netwerk gebruiken wanneer u configureert en verzendt u de extra IP-adresbereiken van de aanvullende VM-subnetten als onderdeel van de adresruimte van Azure-netwerk.

![IP-adresbereiken in SAP HANA op Azure (grote instanties) minimale implementatie vereist](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

U kunt ook de gegevens die u naar Microsoft verzenden samenvoegen. In dat geval bevat de adresruimte van het Azure-netwerk alleen een spatie. Met behulp van de IP-adresbereiken in het vorige voorbeeld, de samengevoegde virtuele netwerkadresruimte kan er uitzien zoals in de volgende afbeelding:

![Tweede mogelijkheid van IP-adresbereiken in SAP HANA op Azure (grote instanties) minimale implementatie vereist](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

We hebben een groter bereik die betrekking heeft op 4096 IP-adressen in het voorbeeld, in plaats van twee kleinere bereiken die de adresruimte van het Azure-netwerk gedefinieerd. Een grote definitie van de adresruimte blijven sommige vrij groot bereiken niet-gebruikte. Omdat het virtuele netwerk adresruimte waarde(n) voor doorgifte van BGP-route worden gebruikt, gebruik van de ongebruikte bereiken on-premises of ergens anders in uw netwerk, kan leiden tot problemen met routering. 

Daarom is het raadzaam dat u de adresruimte die nauw zijn uitgelijnd met de werkelijke subnet-adresruimte die u gebruikt. Indien nodig, zonder downtime op het virtuele netwerk, kunt u de waarden voor nieuwe adres altijd later toevoegen.
 
> [!IMPORTANT] 
> Elk IP-adresbereik in ER P2P, de server IP-adresgroep en de adresruimte van Azure-netwerk moet **niet** overlappen met elkaar of met andere bereik dat wordt gebruikt in uw netwerk. Elk moet discreet zijn. Als de twee voorgaande afbeeldingen weergeven, mag ze ook niet een subnet van een ander bereik. Als er overlapping optreden tussen bereiken, Azure virtual network kan geen verbinding maken met het ExpressRoute-circuit.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Vervolgstappen na de adresbereiken zijn gedefinieerd
Nadat de IP-adresbereiken zijn gedefinieerd, moeten de volgende dingen gebeuren:

1. De IP-adresbereiken voor de adresruimte van Azure-netwerk, de ER P2P-connectiviteit en server IP-adresgroepbereik, samen met andere gegevens die aan het begin van het document is aangeboden indienen. U kunt op dit moment ook starten om het virtuele netwerk en de VM-subnetten te maken. 
2. Een ExpressRoute-circuit wordt gemaakt door Microsoft tussen uw Azure-abonnement en de stempel HANA grote instantie.
3. Een tenantnetwerk is gemaakt op het stempel grote instantie door Microsoft.
4. Microsoft Hiermee configureert u netwerken in de SAP HANA op Azure (grote instanties)-infrastructuur om te accepteren van IP-adressen van uw Azure-netwerk-adresruimte die met HANA grote instanties communiceert.
5. Afhankelijk van de specifieke SAP HANA op Azure (grote instanties)-SKU die u hebt gekocht, wijst Microsoft een compute-eenheid in een tenantnetwerk. Ook kan worden toegewezen en opslag koppelt, en het besturingssysteem (SUSE of Red Hat Linux) wordt geïnstalleerd. IP-adressen voor deze eenheden zijn afkomstig uit de Server IP-Adresgroepbereik dat die u wilde verzenden naar Microsoft.

Microsoft biedt aan het einde van het implementatieproces start, de volgende gegevens in om u te:
- De informatie die nodig is om uw Azure virtuele netwerken verbinden met de ExpressRoute-circuit dat is verbonden virtuele netwerken van Azure met HANA grote instanties:
     - Autorisatie sleutel (s)
     - ExpressRoute PeerID
- Gegevens voor HANA grote instanties nadat u hebt toegang tot stand brengen van ExpressRoute-circuit en Azure-netwerk.

U vindt hier ook de volgorde van de verbinding te maken van HANA grote instanties in het document [SAP HANA op Azure (grote instanties) Setup](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Veel van de volgende stappen worden weergegeven in een voorbeeld van een implementatie in dat document. 

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg [een virtueel netwerk verbinden met HANA grote instantie ExpressRoute](hana-connect-vnet-express-route.md).
