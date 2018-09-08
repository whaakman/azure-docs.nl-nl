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
ms.openlocfilehash: f0e09e6dfce5d0ede525f461193866219b7f9429
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164729"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Azure-VM's verbinden met HANA grote instanties

Zoals vermeld [SAP HANA (grote instanties) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) de minimale implementatie van HANA grote instanties met het niveau van de SAP-toepassing in Azure hebt, zoals:

![Azure VNet dat is verbonden met SAP HANA op Azure (grote instanties) en on-premises](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Op zoek dichter bij aan de Azure-VNet, we de noodzaak van de volgende voordelen:

- De definitie van een Azure-VNet die u wilt worden gebruikt voor het implementeren van de virtuele machines van de SAP-toepassingslaag in.
- Dat automatisch betekent dat een standaard-subnet in het Azure-Vnet is gedefinieerd dat is echt de gebruikt voor het implementeren van de virtuele machines in.
- Het Azure-VNet dat gemaakt, moet beschikken over ten minste één VM-subnet en één ExpressRoute-Gateway-subnet. Deze subnetten moeten de IP-adresbereiken opgegeven en die wordt beschreven in de volgende secties worden toegewezen.

Dus laten we bekijken een en ander dichter bij het maken van Azure VNet voor HANA grote instanties

## <a name="creating-the-azure-vnet-for-hana-large-instances"></a>Het maken van het Azure VNet voor HANA grote instanties

>[!Note]
>Het Azure VNet voor HANA grote instantie moet worden gemaakt met het Azure Resource Manager-implementatiemodel. De oudere Azure-implementatiemodel, bekend als het klassieke implementatiemodel wordt niet ondersteund met de oplossing voor HANA grote instantie.

Het VNet kan worden gemaakt met de Azure portal, PowerShell, Azure-sjabloon of Azure CLI (Zie [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). In het volgende voorbeeld wordt behandeld in een VNet dat is gemaakt via Azure portal.

Als u in de definities van een Azure VNet via Azure portal, laten we bekijken enkele van de definities en hoe deze in verband met wat wij in de lijst van verschillende IP-adresbereiken. Als we praten over de **adresruimte**, bedoelen we de adresruimte die het Azure-VNet mag worden gebruikt. Deze adresruimte is ook het adresbereik dat de VNet wordt gebruikt voor het doorgeven van BGP-route. Dit **adresruimte** kunnen hier worden weergegeven:

![Adres van de ruimte van Azure VNet weergegeven in de Azure portal](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

In het geval is voorgaande met 10.16.0.0/16, is het Azure-VNet krijgt een in plaats van grote en breed IP-adresbereik te gebruiken. Betekent dat alle IP-adresbereiken van de volgende subnetten in dit VNet hun bereiken binnen die adresruimte kunnen hebben. Meestal bevelen we dergelijke grote adresbereik niet voor één VNet in Azure. Maar laten we nog een stapje verder aan, bekijken in de subnetten die zijn gedefinieerd in het Azure-VNet:

![Azure VNet-subnetten en hun IP-adresbereiken](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Zoals u ziet, kijken we naar een VNet met een eerste VM-subnet (hier 'default' genoemd) en een subnet met de naam 'GatewaySubnet'.
In de volgende sectie, verwijzen we naar het IP-adresbereik van het subnet, dat 'default' in de afbeeldingen als aangeroepen is **Azure VM-subnet IP-adresbereik**. In de volgende secties, verwijzen we naar het IP-adresbereik van het Gatewaysubnet als **VNet-Gateway-Subnet IP-adresbereik**. 

In het geval gedemonstreerd met de bovenstaande twee afbeeldingen ziet u dat de **VNet-adresruimte** behandelt, de **Azure VM-subnet IP-adresbereik** en de **VNet-Gateway-Subnet IP-adresbereik**. 

In andere gevallen waarin u wilt besparen of worden bepaald met de IP-adresbereiken, kunt u beperken de **VNet-adresruimte** van een VNet naar de specifieke bereiken door elk subnet wordt gebruikt. In dit geval kunt u definiëren de **VNet-adresruimte** van een VNet als meerdere specifieke bereiken, zoals hier wordt weergegeven:

![Azure VNet-adresruimte met twee spaties](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

In dit geval de **VNet-adresruimte** heeft twee spaties gedefinieerd. Deze twee spaties zijn identiek aan de IP-adresbereiken zijn gedefinieerd voor **Azure VM-subnet IP-adresbereik** en de **VNet-Gateway-Subnet IP-adresbereik.**

U kunt een naamgevingsnorm die u wilt gebruiken voor deze tenant subnetten (VM-subnetten). Echter, **moet altijd er één en slechts één gateway-subnet voor elke VNet** die verbinding maakt met de SAP HANA op Azure (grote instanties) ExpressRoute-circuit. En **deze gatewaysubnet moet altijd worden met de naam 'GatewaySubnet'** om te controleren of de juiste plaatsing van de ExpressRoute-gateway.

> [!WARNING] 
> Het is essentieel dat het gatewaysubnet altijd is met de naam 'GatewaySubnet'.

Meerdere VM-subnetten kunnen worden gebruikt, zelfs met behulp van niet-aaneengesloten-adresbereiken. Maar zoals eerder vermeld, wordt deze adresbereiken moeten worden gedekt door de **VNet-adresruimte** van het VNet in geaggregeerde vorm of in een lijst met de exacte bereiken van de VM-subnetten en het gatewaysubnet.

De belangrijke gegevens over een Azure-VNet die verbinding met HANA grote instanties maakt worden samengevat:

- U moet worden verzonden naar Microsoft moet de **VNet-adresruimte** bij het uitvoeren van een aanvankelijke implementatie van HANA grote instanties. 
- De **VNet-adresruimte** mag een groter bereik die betrekking heeft op het bereik van IP-adresbereiken op Azure VM-subnet en het VNet-Gateway-Subnet IP-adresbereik.
- Of u kunt indienen als **VNet-adresruimte** meerdere adresbereiken die betrekking hebben op de verschillende IP-adresbereiken van VM-subnet IP-adresbereiken en het VNet-Gateway-Subnet IP-adresbereik.
- De gedefinieerde **VNet-adresruimte** gebruikte doorgifte van de BGP-routering is.
- De naam van het gatewaysubnet moet: **'GatewaySubnet'.**
- De **VNet-adresruimte** wordt gebruikt als een filter aan de HANA grote instantie voor het toestaan of weigeren van verkeer naar de eenheden HANA grote instantie van Azure. Als de BGP-routering informatie van het Azure-VNet en het IP-adresbereiken is geconfigureerd voor het filteren aan de HANA grote instantie niet overeenkomen, kunnen problemen in de verbinding ontstaan.
- Er zijn een aantal details van het Gateway-subnet die zijn beschreven verder naar beneden in de sectie 'Een VNet verbinden met HANA grote instantie ExpressRoute'



## <a name="different-ip-address-ranges-to-be-defined"></a>Verschillende IP-adresbereiken worden gedefinieerd 

We al enkele van de IP-adresbereiken voor het implementeren van HANA grote instanties in eerdere secties geïntroduceerd. Maar er zijn enkele meer IP-adresbereiken, die belangrijk zijn. Hieronder volgen enkele verdere details. De volgende IP-adressen die niet allemaal moeten worden verzonden naar Microsoft moeten worden gedefinieerd voordat een aanvraag voor de eerste implementatie wordt verzonden:

- **VNet-adresruimte:** zoals al eerder hebt ingevoerd, is of zijn de IP-adresbereiken u hebt toegewezen adres (of wilt toewijzen) aan uw adresruimte-parameter in de virtuele Azure-netwerken (VNet) verbinding maken met de SAP HANA grote instantie-omgeving. Het verdient aanbeveling dat deze adresruimte-parameter de waarde van een meerdere regels bestaat uit de Azure VM-Subnet meerdere bereiken en het bereik van de Azure-Gateway-subnet is, zoals eerder weergegeven in de afbeeldingen. Dit bereik mag niet overlappen met uw on-premises of de Server IP-adresgroep of ER P2P-adresbereiken. Hoe u dit of deze IP-adresbereiken? Uw bedrijfsnetwerk team of de service-provider moet een of meerdere IP-adresbereiken, die is of niet worden gebruikt in uw netwerk bieden. Voorbeeld: Als uw Azure VM-Subnet (Zie eerdere) 10.0.1.0/24 is en het Subnet in uw Azure-Gateway (Zie de volgende) 10.0.2.0/28 is, klikt u vervolgens uw Azure VNet-adresruimte wordt aanbevolen om te worden twee regels; 10.0.1.0/24 en 10.0.2.0/28. Hoewel de adresruimte-waarden kunnen worden samengevoegd, het verdient aanbeveling die overeenkomt met deze naar de adresbereiken subnet om te voorkomen dat onbedoelde hergebruik van niet-gebruikte IP-adresbereiken in grotere adresruimten in de toekomst ergens anders in uw netwerk. **De VNET-adresruimte is een IP-adresbereik op dat worden verzonden naar Microsoft moet wanneer u wordt gevraagd een eerste implementatie**

- **Azure VM-subnet IP-adresbereik:** deze IP-adresbereik, zoals eerder al besproken is degene die u hebt toegewezen (of van plan om toe te wijzen) naar de Azure-VNet-subnet-parameter in uw Azure-VNET verbinding maken met de SAP HANA grote instantie-omgeving. Deze IP-adresbereik wordt gebruikt om IP-adressen toewijzen aan uw Azure VM's. De IP-adressen buiten dit bereik zijn toegestaan verbinding maken met uw SAP HANA grote instantie (s). Indien nodig, kunnen meerdere Azure-VM-subnetten worden gebruikt. Een/24 CIDR-blok door Microsoft wordt aanbevolen voor elke Azure-VM-Subnet. Dit adresbereik moet een deel van de waarden in de Azure-VNet-adresruimte. Hoe kan ik deze IP-adresbereik ophalen? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik op dat momenteel niet in uw netwerk gebruikt wordt bieden.

- **VNet-Gateway-Subnet IP-adresbereik:** , afhankelijk van de functies die u wilt gebruiken, de aanbevolen grootte is:
   - Ultra-performance ExpressRoute-gateway: / 26-Adresblok - vereist voor de klasse van SKU's Type II
   - Samen met VPN en ExpressRoute met behulp van een High-performance ExpressRoute-Gateway (of lager): / 27-Adresblok
   - Alle andere gevallen: / 28-Adresblok. Dit adresbereik moet een deel van de waarden in de waarden 'VNet-adresruimte'. Dit adresbereik moet een deel van de waarden die de waarden in Azure VNet-adresruimte die u nodig hebt om te verzenden naar Microsoft worden gebruikt. Hoe kan ik deze IP-adresbereik ophalen? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik op dat momenteel niet in uw netwerk gebruikt wordt bieden. 

- **Adresbereik voor ER-P2P-connectiviteit:** dit is het IP-bereik voor uw SAP HANA grote instantie ExpressRoute (ER) P2P-verbinding. Dit bereik van IP-adressen moet een/29 CIDR-IP-adresbereik. Dit bereik mag niet overlappen met uw on-premises of andere Azure-IP-adresbereiken. Deze IP-adresbereik wordt gebruikt voor het instellen van de connectiviteit ER vanuit uw Azure VNet ExpressRoute-Gateway naar de SAP HANA grote instantie-servers. Hoe kan ik deze IP-adresbereik ophalen? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik op dat momenteel niet in uw netwerk gebruikt wordt bieden. **Dit bereik is een IP-adresbereik op dat worden verzonden naar Microsoft moet wanneer u wordt gevraagd een eerste implementatie**
  
- **Server IP-Adresgroepbereik:** dit IP-adresbereik wordt gebruikt voor de afzonderlijke IP-adres toewijzen aan HANA grote instantie servers. De subnetgrootte van de aanbevolen is een/24 CIDR blokkeren - maar indien nodig kan zijn kleiner tot een minimum van het leveren van 64 IP-adressen. Uit dit bereik, worden de eerste 30 IP-adressen gereserveerd voor gebruik door Microsoft. Zorg ervoor dat dit feit rekening wordt gehouden bij het kiezen van de grootte van het bereik. Dit bereik mag niet overlappen met uw on-premises of andere Azure-IP-adressen. Hoe kan ik deze IP-adresbereik ophalen? Uw bedrijfsnetwerk team of de service-provider moet een IP-adresbereik dat momenteel niet in uw netwerk gebruikt wordt bieden. Een/24 (aanbevolen) unieke CIDR blokkeren om te worden gebruikt voor het toewijzen van de specifieke IP-adressen die nodig zijn voor SAP HANA op Azure (grote instanties). **Dit bereik is een IP-adresbereik op dat worden verzonden naar Microsoft moet wanneer u wordt gevraagd een eerste implementatie**
 
Hoewel u nodig hebt om te definiëren en de bovenstaande IP-adresbereiken plannen, moeten niet alle ze worden verzonden naar Microsoft. Om samen te vatten het bovenstaande, zijn de IP-adresbereiken die u moet de naam van Microsoft:

- Azure-VNet-adres Space(s)
- Adresbereik voor ER-P2P-connectiviteit
- IP-Adresgroepbereik dat Server

Toevoegen van extra vnet's die u moet verbinding maken met HANA grote instanties, moet u de nieuwe Azure VNet-adresruimte u aan Microsoft toevoegt te verzenden. 

Hieronder volgt een voorbeeld van de verschillende bereiken en enkele voorbeeld-bereiken als u wilt configureren en uiteindelijk naar Microsoft. Zoals u ziet, is de waarde voor de Azure-VNet-adresruimte wordt niet geaggregeerd in het eerste voorbeeld, maar is gedefinieerd in het bereik van de eerste Azure-VM-subnet IP-adresbereik en het VNet-Gateway-Subnet IP-adresbereik. Met behulp van meerdere VM-subnetten binnen het Azure-VNet zou moeten werken bereiken van de aanvullende VM-subnetten dienovereenkomstig door configureren en het verzenden van de extra IP-adres als onderdeel van de Azure-VNet-adresruimte.

![IP-adresbereiken in SAP HANA op Azure (grote instanties) minimale implementatie vereist](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

U hebt ook de mogelijkheid van de samenvoeging van de gegevens die u naar Microsoft verzenden. In dat geval zou de adresruimte van het Azure-VNet zijn alleen van een spatie. Met behulp van de IP-adresbereiken die eerder in dit voorbeeld gebruikt. Deze cumulatieve VNet-adresruimte kan er als volgt uitzien:

![Tweede mogelijkheid van IP-adresbereiken in SAP HANA op Azure (grote instanties) minimale implementatie vereist](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Zoals u ziet, in plaats van twee kleinere bereiken die de adresruimte van het Azure-VNet gedefinieerd hebben we een groter bereik die betrekking heeft op 4096 IP-adressen. Een grote definitie van de adresruimte blijven sommige vrij groot bereiken niet-gebruikte. Omdat de VNet-adresruimte waarde(n) voor doorgifte van BGP-route worden gebruikt, gebruik van de ongebruikte bereiken on-premises of ergens anders in uw netwerk, kan leiden tot problemen met routering. Het is dus raadzaam te houden van de adresruimte die nauw zijn uitgelijnd met de werkelijke subnet een adresruimte die wordt gebruikt. Indien nodig, zonder downtime op het VNet, kunt u nieuwe adresruimte waarden altijd later toevoegen.
 
> [!IMPORTANT] 
> Elk IP-adres bereik van ER-P2P, IP-servergroep, Azure VNet-adresruimte moet **niet** met elkaar overlappen of een ander bereik die ergens anders in uw netwerk wordt gebruikt; elk moet discreet zijn en als de twee afbeeldingen eerdere weergeven, zijn mogelijk niet een subnet van een ander bereik. Als er overlapping optreden tussen bereiken, het Azure-VNet kan geen verbinding maken met het ExpressRoute-circuit.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Vervolgstappen na de adresbereiken zijn gedefinieerd
Nadat de IP-adresbereiken zijn gedefinieerd, moeten de volgende activiteiten worden uitgevoerd:

1. De IP-adresbereiken indienen voor Azure VNet-adresruimte, ER P2P-connectiviteit en -Server IP-Adresgroepbereik, samen met andere gegevens die aan het begin van het document is aangeboden. Op dit moment, u ook kan worden gestart om het VNet en de VM-subnetten te maken. 
2. Een Express Route-circuit wordt gemaakt door Microsoft tussen uw Azure-abonnement en de stempel HANA grote instantie.
3. Een tenantnetwerk is gemaakt op het stempel grote instantie door Microsoft.
4. Microsoft Hiermee configureert u netwerken in de SAP HANA op Azure (grote instanties)-infrastructuur om te accepteren van IP-adressen van uw Azure VNet-adresruimte die communiceert met HANA grote instanties.
5. Afhankelijk van de specifieke SAP HANA op Azure (grote instanties) SKU hebt aangeschaft, Microsoft wijst een compute-eenheid in een tenantnetwerk, toewijzen en opslag koppelen en installeer het besturingssysteem (SUSE of Red Hat Linux). IP-adressen voor deze eenheden zijn afkomstig uit de servergroep voor IP-adresbereik u verzonden naar Microsoft.

Microsoft biedt aan het einde van het implementatieproces start, de volgende gegevens in om u te:
- De informatie die nodig is om uw Azure vnet (s) aan het ExpressRoute-circuit dat is verbonden Azure vnet's met HANA grote instanties verbinding te maken:
     - Autorisatie sleutel (s)
     - ExpressRoute PeerID
- Gegevens voor toegang tot HANA grote instanties nadat u ExpressRoute-circuit en Azure VNet tot stand gebracht.

U vindt hier ook de volgorde van de verbinding te maken van HANA grote instanties in het document [End-to-Setup voor SAP HANA grote instanties](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Veel van de volgende stappen worden weergegeven in een voorbeeld van een implementatie in dat document. 

**Volgende stappen**

- Raadpleeg [verbinden van een VNet met HANA grote instantie ExpressRoute](hana-connect-vnet-express-route.md).