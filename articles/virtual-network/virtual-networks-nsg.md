---
title: Netwerkbeveiligingsgroepen in Azure | Microsoft Docs
description: Leer hoe u de verkeersstroom binnen uw virtuele netwerken kunt isoleren en beheren met de gedistribueerde firewall in Azure met behulp van netwerkbeveiligingsgroepen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 20e850fc-6456-4b5f-9a3f-a8379b052bc9
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: fac6ee69b5f0377e0515ac9abeb28788cbef9b79
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="filter-network-traffic-with-network-security-groups"></a>Netwerkverkeer filteren met netwerkbeveiligingsgroepen

Een netwerkbeveiligingsgroep (Network Security Group, NSG) bevat een lijst met beveiligingsregels waarmee netwerkverkeer naar resources die zijn verbonden met virtuele Azure-netwerken (VNet) wordt toegestaan of geweigerd. NSG's kunnen worden gekoppeld aan subnetten, afzonderlijke virtuele machines (klassiek) of afzonderlijke netwerkinterfaces (NIC) die zijn gekoppeld aan VM’s (Resource Manager). Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. Verkeer kan verder worden beperkt door ook een NSG te koppelen aan een VM of NIC.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel komen beide modellen aan de orde, maar u wordt aangeraden voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

## <a name="nsg-resource"></a>NSG-resource
NSG's bevatten de volgende eigenschappen:

| Eigenschap | Beschrijving | Beperkingen | Overwegingen |
| --- | --- | --- | --- |
| Naam |Naam voor de NSG |Moet uniek zijn binnen de regio.<br/>Kan letters, cijfers, onderstrepingstekens, punten en afbreekstreepjes bevatten.<br/>Moet beginnen met een letter of cijfer.<br/>Moet eindigen op een letter, cijfer of onderstrepingsteken.<br/>Mag niet meer dan 80 tekens bevatten. |Omdat u mogelijk meerdere NSG's moet maken, is het raadzaam namen te gebruiken die aangeven wat het doel van de NSG is. |
| Regio |Azure-[regio](https://azure.microsoft.com/regions) waar de NSG wordt gemaakt. |NSG's kunnen alleen worden gekoppeld aan resources binnen dezelfde regio als de NSG. |Voor meer informatie over hoeveel NSG's u per regio kunt hebben, leest u het artikel over [Azure-limieten](../azure-subscription-service-limits.md#virtual-networking-limits-classic).|
| Resourcegroep |De [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups) waarin de NSG bestaat. |Hoewel een NSG bestaat in een resourcegroep, kan deze worden gekoppeld aan resources in elke willekeurige resourcegroep, mits de resource tot dezelfde Azure-regio als de NSG behoort. |Resourcegroepen worden gebruikt voor het beheren van meerdere resources tegelijk, als een implementatie-eenheid.<br/>U kunt de NSG desgewenst groeperen met resources waaraan deze is gekoppeld. |
| Regels |Regels voor binnenkomend of uitgaand verkeer die bepalen welk verkeer wordt toegestaan of geweigerd. | |Zie de sectie [NSG-regels](#Nsg-rules) in dit artikel. |

> [!NOTE]
> ACL's voor eindpunten en netwerkbeveiligingsgroepen worden niet ondersteund op een en hetzelfde VM-exemplaar. Als u een NSG wilt gebruiken en er al een ACL voor eindpunten is geïmplementeerd, moet u eerst de ACL voor eindpunten verwijderen. Voor informatie over het verwijderen van een ACL leest u het artikel [Toegangsbeheerlijsten (ACL's) voor eindpunten beheren met PowerShell](virtual-networks-acl-powershell.md).
> 

### <a name="nsg-rules"></a>NSG-regels
NSG-regels bevatten de volgende eigenschappen:

| Eigenschap | Beschrijving | Beperkingen | Overwegingen |
| --- | --- | --- | --- |
| **Naam** |Naam voor de regel. |Moet uniek zijn binnen de regio.<br/>Kan letters, cijfers, onderstrepingstekens, punten en afbreekstreepjes bevatten.<br/>Moet beginnen met een letter of cijfer.<br/>Moet eindigen op een letter, cijfer of onderstrepingsteken.<br/>Mag niet meer dan 80 tekens bevatten. |U kunt meerdere regels binnen een NSG hebben, dus zorg ervoor dat u duidelijk herkenbare namen gebruikt die het doel van de regel aangeven. |
| **Protocol** |Protocol waarop de regel van toepassing is. |TCP, UDP of * |Als u * als protocol gebruikt, omvat dit zowel ICMP (alleen oost-west-verkeer), als UDP en TCP en hebt u wellicht minder regels nodig.<br/>Tegelijkertijd is het gebruik van * mogelijk niet specifiek genoeg, zodat het wordt aanbevolen * alleen te gebruiken als dit echt nodig is. |
| **Bronpoortbereik** |Bronpoortbereik waarop de regel van toepassing is. |Eén poortnummer tussen 1 en 65535, een poortbereik (bijvoorbeeld 1-65535) of * (voor alle poorten). |Bronpoorten kunnen kortstondig zijn. Tenzij in uw clientprogramma een specifieke poort wordt gebruikt, gebruikt u in de meeste gevallen *.<br/>Gebruik zo veel mogelijk poortbereiken om te voorkomen dat u meerdere regels nodig hebt.<br/>Meerdere poorten of poortbereiken kunnen niet worden gegroepeerd met komma's. |
| **Doelpoortbereik** |Doelpoortbereik waarop de regel van toepassing is. |Eén poortnummer tussen 1 en 65535, een poortbereik (bijvoorbeeld 1-65535) of \* (voor alle poorten). |Gebruik zo veel mogelijk poortbereiken om te voorkomen dat u meerdere regels nodig hebt.<br/>Meerdere poorten of poortbereiken kunnen niet worden gegroepeerd met komma's. |
| **Bronadresvoorvoegsel** |Bronadresvoorvoegsel of tag waarop de regel van toepassing is. |Eén IP-adres (bijvoorbeeld 10.10.10.10), een IP-subnet (bijvoorbeeld 192.168.1.0/24), een [standaardtag](#default-tags) of * (voor alle adressen). |Overweeg het gebruik van bereiken, standaardtags en * om het aantal regels te verminderen. |
| **Doeladresvoorvoegsel** |Doeladresvoorvoegsel of tag waarop de regel van toepassing is. | Eén IP-adres (bijvoorbeeld 10.10.10.10), een IP-subnet (bijvoorbeeld 192.168.1.0/24), een [standaardtag](#default-tags) of * (voor alle adressen). |Overweeg het gebruik van bereiken, standaardtags en * om het aantal regels te verminderen. |
| **Richting** |Richting van het verkeer waarop de regel van toepassing is. |Binnenkomend of uitgaand. |Regels voor binnenkomend en uitgaand verkeer worden afzonderlijk verwerkt op basis van de richting. |
| **Prioriteit** |Regels worden in volgorde van prioriteit gecontroleerd. Zodra een regel van toepassing is, worden geen andere regels meer getest. | Getal tussen 100 en 4096. | Overweeg prioriteitsnummers voor regels te laten verspringen met 100 om ruimte over te laten voor nieuwe regels die u mogelijk in de toekomst wilt maken. |
| **Toegang** |Type toegang dat moet worden toegepast als de regel overeenkomt. | Toestaan of weigeren. | Onthoud dat het pakket wordt verwijderd als er geen regel wordt gevonden die het pakket toestaat. |

NSG's bevatten twee sets met regels: een voor binnenkomend verkeer en een voor uitgaand verkeer. De prioriteit voor een regel moet uniek zijn binnen elke set. 

![Verwerking van NSG-regels](./media/virtual-network-nsg-overview/figure3.png) 

In de vorige afbeelding ziet u hoe NSG-regels worden verwerkt.

### <a name="default-tags"></a>Standaardtags
Standaardtags zijn systeem-id's voor een bepaalde categorie IP-adressen. U kunt standaardtags gebruiken in de eigenschappen voor het **voorvoegsel voor het bronadres** en het **voorvoegsel voor het doeladres** van een regel. Er zijn drie standaardtags die u kunt gebruiken:

* **VirtualNetwork** (Resource Manager) (**VIRTUAL_NETWORK** voor klassiek): deze tag omvat de adresruimte van het virtuele netwerk (CIDR-bereiken die in Azure zijn gedefinieerd) en alle verbonden on-premises adresruimten en verbonden Azure VNet's (lokale netwerken).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** voor klassiek): met deze tag wordt de load balancer voor de infrastructuur van Azure aangeduid. De tag wordt omgezet in het IP-adres van een Azure-datacenter van waaruit statuscontroles van Azure worden uitgevoerd.
* **Internet** (Resource Manager) (**INTERNET** voor klassiek): met deze tag wordt de IP-adresruimte aangeduid die zich buiten het virtuele netwerk bevindt en bereikbaar is via internet. Dit bereik omvat ook de [openbare IP-adresruimte van Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="default-rules"></a>Standaardregels
Alle NSG's bevatten een set met standaardregels. De standaardregels kunnen niet worden verwijderd, maar omdat ze de laagste prioriteit hebben, kunnen ze worden overschreven door de regels die u maakt. 

De standaardregels kunnen verkeer als volgt toestaan en weigeren:
- **Virtueel netwerk:** Verkeer dat afkomstig is van en eindigt in een virtueel netwerk wordt toegestaan in zowel binnenkomende als uitgaande richting.
- **Internet:** Uitgaand verkeer is toegestaan, maar binnenkomend verkeer wordt geblokkeerd.
- **Load balancer:** Toestaan dat de load balancer van Azure de status van uw VM's en rolexemplaren controleert. Als u geen set met taakverdeling gebruikt, kunt u deze regel onderdrukken.

**Standaardregels voor binnenkomend verkeer**

| Name | Prioriteit | Bron-IP | Bronpoort | Doel-IP | Doelpoort | Protocol | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVNetInBound |65000 | VirtualNetwork | * | VirtualNetwork | * | * | Toestaan |
| AllowAzureLoadBalancerInBound | 65001 | AzureLoadBalancer | * | * | * | * | Toestaan |
| DenyAllInBound |65500 | * | * | * | * | * | Weigeren |

**Standaardregels voor uitgaand verkeer**

| Naam | Prioriteit | Bron-IP | Bronpoort | Doel-IP | Doelpoort | Protocol | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVnetOutBound | 65000 | VirtualNetwork | * | VirtualNetwork | * | * | Toestaan |
| AllowInternetOutBound | 65001 | * | * | Internet | * | * | Toestaan |
| DenyAllOutBound | 65500 | * | * | * | * | * | Weigeren |

## <a name="associating-nsgs"></a>NSG's koppelen
U kunt als volgt een NSG koppelen aan VM's, NIC's en subnetten, afhankelijk van het implementatiemodel dat u gebruikt:

* **VM (alleen klassiek):** beveiligingsregels worden toegepast op al het verkeer naar/van de virtuele machine. 
* **NIC (alleen Resource Manager):** beveiligingsregels worden toegepast op al het verkeer naar/van de NIC waaraan de NSG is gekoppeld. In een VM met meerdere NIC's kunt u verschillende NSG’s (of dezelfde NSG) toepassen op elke NIC afzonderlijk. 
* **Subnet (Resource Manager en klassiek):** beveiligingsregels worden toegepast op al het verkeer naar/van resources die zijn verbonden met het VNet.

U kunt verschillende NSG's koppelen aan een VM (of NIC, afhankelijk van het implementatiemodel) en het subnet waarmee een NIC of VM is verbonden. Beveiligingsregels worden toegepast op het verkeer op basis van prioriteit in elke NSG, in deze volgorde:

- **Binnenkomend verkeer**

  1. **NSG die is toegepast op het subnet:** als er een overeenkomende regel is voor de subnet-NSG die verkeer weigert, wordt het pakket verwijderd.

  2. **NSG die is toegepast op de NIC** (Resource Manager) of VM (klassiek): als een VM\NIC-NSG een overeenkomende regel heeft die verkeer weigert, worden pakketten verwijderd bij de VM\NIC, zelfs als een subnet-NSG een overeenkomende regel heeft die verkeer toestaat.

- **Uitgaand verkeer**

  1. **NSG die is toegepast op de NIC** (Resource Manager) of VM (klassiek): als een VM\NIC-NSG een overeenkomende regel heeft die verkeer weigert, worden pakketten verwijderd.

  2. **NSG die is toegepast op het subnet**: als een subnet-NSG een overeenkomende regel heeft die verkeer weigert, worden pakketten verwijderd, zelfs als een VM/NIC-NSG een overeenkomende regel heeft die verkeer toestaat.

> [!NOTE]
> U kunt slechts één NSG aan een subnet, VM of NIC koppelen, maar u kunt wel dezelfde NSG aan zo veel resources koppelen als u wilt.
>

## <a name="implementation"></a>Implementatie
U kunt NSG's implementeren in het klassieke implementatiemodel of het implementatiemodel van Resource Manager met behulp van de volgende hulpprogramma's:

| Implementatieprogramma | Klassiek | Resource Manager |
| --- | --- | --- |
| Azure Portal   | Ja | [Ja](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell     | [Ja](virtual-networks-create-nsg-classic-ps.md) | [Ja](virtual-networks-create-nsg-arm-ps.md) |
| Azure CLI **V1**   | [Ja](virtual-networks-create-nsg-classic-cli.md) | [Ja](virtual-networks-create-nsg-cli-nodejs.md) |
| Azure CLI **V2**   | Nee | [Ja](virtual-networks-create-nsg-arm-cli.md) |
| Azure Resource Manager-sjabloon   | Nee  | [Ja](virtual-networks-create-nsg-arm-template.md) |

## <a name="planning"></a>Planning
Voordat u NSG's implementeert, moet u de volgende vragen beantwoorden:

1. Voor welke soorten resources wilt binnenkomend of uitgaand verkeer filteren? U kunt resources zoals NIC's (Resource Manager), VM’s (klassiek), Cloud Services, omgevingen met toepassingsservices en VM- schaalsets verbinden. 
2. Zijn de resources waarvan u het binnenkomende/uitgaande verkeer wilt filteren verbonden met subnetten in bestaande VNet's?

Lees het artikel [Cloud Services en netwerkbeveiliging](../best-practices-network-security.md) voor meer informatie over het plannen van netwerkbeveiliging in Azure. 

## <a name="design-considerations"></a>Overwegingen bij het ontwerpen
Als u de antwoorden op de vragen in de sectie [Planning](#Planning) weet, leest u de volgende secties aandachtig door voordat u uw NSG's definieert:

### <a name="limits"></a>Limieten
Er gelden limieten voor het aantal NSG's dat u kunt hebben in een abonnement en het aantal regels per NSG. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).

### <a name="vnet-and-subnet-design"></a>VNET's en subnetten ontwerpen
Omdat NSG's kunnen worden toegepast op subnetten, kunt u het aantal NSG's minimaliseren door uw resources te  groeperen per subnet, en NSG's op subnetten toe te passen.  Als u besluit om NSG's op subnetten toe te passen, merkt u mogelijk dat bij het definiëren van bestaande VNET's en subnetten niet altijd rekening is gehouden met NSG's. U moet mogelijk nieuwe VNet's en subnetten definiëren ter ondersteuning van uw NSG-ontwerp en uw nieuwe resources implementeren met uw nieuwe subnetten. U kunt dan een migratiestrategie definiëren om bestaande resources te verplaatsen naar de nieuwe subnetten. 

### <a name="special-rules"></a>Speciale regels
Als u verkeer blokkeert dat is toegestaan door de volgende regels, kan uw infrastructuur niet communiceren met essentiële Azure-services:

* **Virtueel IP-adres van het hostknooppunt:** basisinfrastructuurservices zoals DHCP, DNS en statuscontrole worden geleverd via het gevirtualiseerde host-IP-adres 168.63.129.16. Dit openbare IP-adres is van Microsoft en is het enige gevirtualiseerde IP-adres dat in alle regio's wordt gebruikt voor dit doel. Dit IP-adres wordt toegewezen aan het fysieke IP-adres van de servercomputer (hostknooppunt) die fungeert als host voor de VM. Het hostknooppunt fungeert als de DHCP-relay, de recursieve DNS-omzetter en de bron voor de statuscontrole van de load balancer en de machine. Communicatie met dit IP-adres is geen aanval.
* **Licentieverlening (Key Management Service):** voor alle Windows installatiekopieën die op de VM’s machines worden uitgevoerd, is een licentie vereist. Hiervoor wordt een licentieaanvraag verstuurd naar de Key Management Service-hostservers waarop dergelijke query's worden afgehandeld. De uitgaande aanvraag wordt gedaan via poort. 1688.

### <a name="icmp-traffic"></a>ICMP-verkeer
In de huidige NSG-regels worden alleen de protocollen *TCP* en *UDP* ondersteund. Er is geen specifieke tag voor *ICMP*. ICMP-verkeer is echter toegestaan binnen een VNet door de standaardregel AllowVNetInBound waarmee verkeer van en naar een poort en protocol binnen de VNet wordt toegestaan.

### <a name="subnets"></a>Subnetten
* Houd rekening met het aantal benodigde lagen voor uw workload. Elke laag kan worden geïsoleerd met behulp van een subnet, waarbij een NSG wordt toegepast op het subnet. 
* Als u een subnet voor een VPN-gateway of ExpressRoute-circuit moet implementeren, mag u **geen** NSG op dat subnet toepassen. Als u dit wel doet, mislukken verbindingen tussen VNets of tussen lokale netwerkonderdelen onderling. 
* Als u een virtueel netwerkapparaat (Network Virtual Appliance, NVA) moet implementeren, verbindt u de NVA met een eigen subnet en maakt u door de gebruiker gedefinieerde routes (User-Defined Routes, UDR) naar en van de NVA. U kunt een NSG op subnetniveau implementeren om binnenkomend en uitgaand verkeer in dit subnet te filteren. Lees het artikel [User-defined routes](virtual-networks-udr-overview.md) (Door de gebruiker gedefinieerde routes) voor meer informatie over UDR’s.

### <a name="load-balancers"></a>Load balancers
* Overweeg regels voor taakverdeling en netwerkadresomzetting (Network Address Translation, NAT) voor elke load balancer die wordt gebruikt door elk van uw workloads. NAT-regels zijn gebonden aan een back-end-groep die NIC's (Resource Manager) of VM's/Cloud Services-rolexemplaren (klassiek) bevat. Overweeg een NSG te maken voor elke back-end-groep, zodat alleen verkeer wordt toegestaan dat is gedefinieerd in de regels die zijn geïmplementeerd in de load balancers. Als u een NSG maakt voor elke back-end-groep, zorgt u ervoor dat verkeer dat rechtstreeks naar de back-end-groep gaat, in plaats van via de load balancer, ook wordt gefilterd.
* In klassieke implementaties maakt u eindpunten waarop poorten van een load balancer worden afgestemd met poorten van uw VM's of rolexemplaren. U kunt ook uw eigen individuele openbare load balancer maken via Resource Manager. De doelpoort voor binnenkomend verkeer is de daadwerkelijke poort in de VM of het rolexemplaar, niet de poort die wordt weergegeven door een load balancer. De bronpoort en het bronadres voor de verbinding met de VM zijn een poort en adres op de externe computer op internet, en niet de poort en het adres die beschikbaar zijn gemaakt door de load balancer.
* Wanneer u NSG's maakt om verkeer te filteren dat afkomstig is van een interne load balancer (ILB), worden de bronpoort en het bronadresbereik toegepast van de computer waarvan de oproep afkomstig is en niet die van de load balancer. De doelpoort en het doeladresbereik zijn die van de doelcomputer, niet van de load balancer.

### <a name="other"></a>Overige
* ACL's voor eindpunten en NSG's worden niet ondersteund op een en hetzelfde VM-exemplaar. Als u een NSG wilt gebruiken en er al een ACL voor eindpunten is geïmplementeerd, moet u eerst de ACL voor eindpunten verwijderen. Voor meer informatie over het verwijderen van een eindpunt-ACT leest u het artikel [Eindpunt-ACL's beheren](virtual-networks-acl-powershell.md).
* In Resource Manager kunt u een NSG die aan een NIC is gekoppeld gebruiken voor VM's met meerdere NIC's om zo beheer (externe toegang) per NIC mogelijk te maken. Door unieke NSG's te koppelen aan elke NIC kunt u soorten verkeer over NIC's scheiden.
* Net als bij het gebruik van load balancers geldt dat u bij het filteren van verkeer van andere VNET's, het adresbereik van de bron van de externe computer moet gebruiken, en niet dat van de gateway waarmee de VNET's onderling worden verbonden.
* Veel Azure-services kunnen niet worden verbonden met VNet's. Als een Azure-resource niet met een VNet is verbonden, kunt u geen NSG gebruiken om verkeer naar de resource te filteren.  Raadpleeg de documentatie bij de services die u gebruikt om te bepalen of de service kan worden gebruikt in combinatie met een VNet.

## <a name="sample-deployment"></a>Voorbeeldimplementatie
Bekijk ter illustratie van de toepassing van de informatie in dit artikel een gangbaar scenario van een toepassing met twee lagen zoals in de volgende afbeelding:

![NSG's](./media/virtual-network-nsg-overview/figure1.png)

Zoals u ziet in het diagram, zijn de VM's *Web1* en *Web2* verbonden met het subnet *FrontEnd* en zijn de VM's *DB1* en *DB2* verbonden met het subnet *BackEnd*.  Beide subnetten behoren tot het VNET *TestVNET*. De toepassingsonderdelen worden elk uitgevoerd binnen een Azure-VM die is verbonden met een VNet. Voor het scenario gelden de volgende vereisten:

1. Scheiding van verkeer tussen de WEB- en DB-servers.
2. Taakverdelingsregels waarmee verkeer van de load balancer wordt doorgestuurd naar alle webservers op poort 80.
3. NAT-regels voor de load balancer waarmee verkeer dat via poort 50001 in de load balancer binnenkomt, wordt doorgestuurd naar poort 3389 op de VM WEB1.
4. Geen toegang tot de front-end- en back-end-VM's via internet, behalve vereisten 2 en 3.
5. Geen uitgaande internettoegang vanaf de WEB- of DB-servers.
6. Toegang vanaf het subnet FrontEnd naar poort 3389 van een webserver is toegestaan.
7. Toegang vanaf het subnet FrontEnd naar poort 3389 van een DB-server is toegestaan.
8. Toegang vanaf het subnet FrontEnd naar poort 1433 van alle DB-servers is toegestaan.
9. Scheiding van beheerverkeer (poort 3389) en databaseverkeer (poort 1433) op verschillende NIC's in DB-servers.

Vereisten 1-6 (behalve vereisten 3 en 4) zijn alle beperkt tot de subnetruimten. De volgende NSG's voldoen aan de voorgaande vereisten, terwijl het aantal vereiste NSG’s tot het minimum is beperkt:

### <a name="frontend"></a>FrontEnd
**Regels voor binnenkomend verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-HTTP-Internet | Toestaan | 100 | Internet | * | * | 80 | TCP |
| Allow-Inbound-RDP-Internet | Toestaan | 200 | Internet | * | * | 3389 | TCP |
| Deny-Inbound-All | Weigeren | 300 | Internet | * | * | * | TCP |

**Regels voor uitgaand verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All |Weigeren |100 | * | * | Internet | * | * |

### <a name="backend"></a>BackEnd
**Regels voor binnenkomend verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Weigeren | 100 | Internet | * | * | * | * |

**Regels voor uitgaand verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Weigeren | 100 | * | * | Internet | * | * |

De volgende NSG's worden gemaakt en gekoppeld aan NIC's in de volgende VM’s:

### <a name="web1"></a>WEB1
**Regels voor binnenkomend verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Internet | Toestaan | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Toestaan | 200 | Internet | * | * | 80 | TCP |

> [!NOTE]
> Het bronadresbereik voor de vorige regels is **Internet**, niet het virtuele IP-adres van de load balancer. De bronpoort is *, niet 500001. NAT-regels voor load balancers zijn niet hetzelfde als NSG-beveiligingsregels. NSG-beveiligingsregels hebben altijd betrekking op de bron van herkomst en de bestemming van het verkeer, **niet** op de load balancer ertussen. 
> 
> 

### <a name="web2"></a>WEB2
**Regels voor binnenkomend verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Inbound-RDP-Internet | Weigeren | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Toestaan | 200 | Internet | * | * | 80 | TCP |

### <a name="db-servers-management-nic"></a>DB-servers (beheer-NIC)
**Regels voor binnenkomend verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Front-end | Toestaan | 100 | 192.168.1.0/24 | * | * | 3389 | TCP |

### <a name="db-servers-database-traffic-nic"></a>DB-servers (NIC voor databaseverkeer)
**Regels voor binnenkomend verkeer**

| Regel | Toegang | Prioriteit | Bronadresbereik | Bronpoort | Doeladresbereik | Doelpoort | Protocol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-SQL-Front-end | Toestaan | 100 | 192.168.1.0/24 | * | * | 1433 | TCP |

Aangezien sommige NSG's zijn gekoppeld aan afzonderlijke NIC's, gelden de regels voor resources die zijn geïmplementeerd via Resource Manager. Regels worden gecombineerd voor subnet en NIC, afhankelijk van hoe ze zijn gekoppeld. 

## <a name="next-steps"></a>Volgende stappen
* [NSG's implementeren (Resource Manager)](virtual-networks-create-nsg-arm-pportal.md).
* [NSG's implementeren (klassiek)](virtual-networks-create-nsg-classic-ps.md).
* [NSG-logboeken beheren](virtual-network-nsg-manage-log.md).
* [Problemen met NSG’s oplossen] (virtual-network-nsg-troubleshoot-portal.md)
