---
title: Azure-netwerk (VNet) planning and Design Guide | Microsoft Docs
description: Informatie over het plannen en ontwerpen van virtuele netwerken in Azure op basis van uw vereisten isolatie, connectiviteit en locatie.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2016
ms.author: jdial
ms.openlocfilehash: 9a0126235c9ff3fec05d7709bdee95ab4832a33b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="plan-and-design-azure-virtual-networks"></a>Plannen en ontwerpen van virtuele netwerken van Azure
Maken van een VNet om te experimenteren met eenvoudig genoeg is, maar waarschijnlijk hebt u meerdere VNets na verloop van tijd voor de ondersteuning van de productie-behoeften van uw organisatie gaat implementeren. Met een bepaalde planning en ontwerp, kunt u zich kan VNets implementeren en verbinding maken met de gewenste effectiever resources. Als u niet bekend bent met Vnetten, verdient het aanbeveling die u [meer informatie over VNets](virtual-networks-overview.md) en [implementeren](virtual-networks-create-vnet-arm-pportal.md) een voordat u doorgaat.

## <a name="plan"></a>Plannen
Een goed begrip van de Azure-abonnementen, regio's en resources van netwerk is essentieel voor succes. U kunt de lijst met overwegingen hieronder als uitgangspunt gebruiken. Zodra u deze overwegingen begrijpt, kunt u de vereisten voor het netwerkontwerp van uw definiëren.

### <a name="considerations"></a>Overwegingen
Voordat het beantwoorden van de planning hieronder vragen, moet u rekening houden met het volgende:

* Alles wat die u in Azure maakt bestaat uit een of meer resources. Een virtuele machine (VM) is een resource, de adapter netwerkinterface (NIC) die wordt gebruikt door een virtuele machine is een resource, het openbare IP-adres gebruikt door een NIC is een resource, het VNet de NIC is verbonden met een bron is.
* Maken van resources in een [Azure-regio](https://azure.microsoft.com/regions/#services) en -abonnement. Resources kunnen alleen worden verbonden met een virtueel netwerk dat in dezelfde regio bestaat en de resource-abonnement is in.
* U kunt virtuele netwerken met elkaar verbinden door gebruik te maken:
    * **[Virtueel netwerk peering](virtual-network-peering-overview.md)**: de virtuele netwerken moeten aanwezig zijn in dezelfde Azure-regio. Bandbreedte tussen resources in virtuele netwerken brengen is hetzelfde als wanneer de resources met hetzelfde virtuele netwerk zijn verbonden.
    * **Een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)**: de virtuele netwerken kunnen bestaan in dezelfde of verschillende Azure-regio's. Bandbreedte tussen resources in virtuele netwerken die zijn verbonden via een VPN-Gateway wordt beperkt door de bandbreedte van de VPN-Gateway.
* U kunt VNets verbinden naar uw on-premises netwerk via een van de [connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) beschikbaar in Azure.
* Verschillende bronnen kunnen worden gegroepeerd [resourcegroepen](../azure-resource-manager/resource-group-overview.md#resource-groups), waardoor het gemakkelijker voor het beheren van de bron als eenheid. Een resourcegroep kan resources uit meerdere regio's, bevatten, zolang de resources tot hetzelfde abonnement behoren.

### <a name="define-requirements"></a>Vereisten definiëren
Gebruik de vragen hieronder als een beginpunt voor het ontwerp van uw Azure-netwerk.    

1. Welke Azure locaties gebruikt u host VNets?
2. Moet u communicatie tussen deze Azure locaties?
3. Moet u communicatie tussen uw Azure-VNet(s) en uw lokale clientresources?
4. Hoeveel infrastructuur als een Service (IaaS) virtuele machines, cloud services-rollen en web-apps wilt die u voor uw oplossing nodig?
5. Moet u voor het isoleren van verkeer op basis van groepen met VM's (dat wil zeggen front-end-webservers en back-end-databaseservers)?
6. Moet u het verkeer met behulp van virtuele apparaten beheren?
7. Moeten gebruikers verschillende sets van machtigingen voor andere Azure-resources

### <a name="understand-vnet-and-subnet-properties"></a>VNet en de subneteigenschappen begrijpen
VNet en subnetten resources helpen definiëren een beveiligingsgrens voor werkbelastingen in Azure. Een VNet wordt gekenmerkt door een verzameling van adresruimten, gedefinieerd als CIDR-blokken.

> [!NOTE]
> Netwerkbeheerders bent bekend met CIDR-notatie. Als u niet bekend met CIDR bent, [meer informatie over het](http://whatismyipaddress.com/cidr).
>
>

Vnet's bevatten de volgende eigenschappen.

| Eigenschap | Beschrijving | Beperkingen |
| --- | --- | --- |
| **naam** |VNet-naam |Tekenreeks van maximaal 80 tekens. Kan bevatten letters, cijfers, onderstrepingstekens, punten of afbreekstreepjes. Moet beginnen met een letter of cijfer. Moet eindigen op een letter, cijfer of onderstrepingsteken. Kan bevat hoofdletter of kleine letters bestaan. |
| **location** |Azure-locatie (ook wel regio genoemd). |Moet een geldige Azure locaties. |
| **de addressSpace** |Verzameling van adresvoorvoegsels waaruit het VNet in CIDR-notatie. |Moet een matrix van geldig CIDR-adresblokken, met inbegrip van openbare IP-adresbereiken. |
| **subnetten** |Verzameling van subnetten die gezamenlijk de VNet |Zie de onderstaande subnet eigenschappentabel. |
| **dhcpOptions** |Object met een enkele vereiste eigenschap met de naam **dnsServers**. | |
| **dnsServers** |Matrix van DNS-servers die worden gebruikt door het VNet. Als er geen server is opgegeven, wordt Azure interne naamomzetting gebruikt. |Moet een matrix van maximaal 10 DNS-servers IP-adres. |

Een subnet is een onderliggende resource van een VNet en helpt bij het segmenten van adresruimten binnen een CIDR-blok met behulp van IP-adresvoorvoegsels definiëren. NIC's worden toegevoegd aan subnetten en verbonden met virtuele machines, tegelijk connectiviteit biedt voor verschillende werkbelastingen.

Subnetten bevatten de volgende eigenschappen.

| Eigenschap | Beschrijving | Beperkingen |
| --- | --- | --- |
| **naam** |Subnetnaam |Tekenreeks van maximaal 80 tekens. Kan bevatten letters, cijfers, onderstrepingstekens, punten of afbreekstreepjes. Moet beginnen met een letter of cijfer. Moet eindigen op een letter, cijfer of onderstrepingsteken. Kan bevat hoofdletter of kleine letters bestaan. |
| **location** |Azure-locatie (ook wel regio genoemd). |Moet een geldige Azure locaties. |
| **addressPrefix** |Één adresvoorvoegsel waaruit het subnet in CIDR-notatie |Moet een enkel CIDR-blok die deel uitmaakt van een van de VNet-adresruimten. |
| **networkSecurityGroup** |NSG wordt toegepast op het subnet | |
| **Migratiestatus** |De routetabel is toegepast op het subnet | |
| **ipConfigurations** |Verzameling van IP-configuratie-objecten die worden gebruikt door de NIC's die zijn verbonden met het subnet | |

### <a name="name-resolution"></a>Naamomzetting
Uw VNet gebruikt standaard [Azure verschafte naamomzetting](virtual-networks-name-resolution-for-vms-and-role-instances.md) omzetten van namen binnen het VNet en op het openbare Internet. Echter, als u uw vnet's verbinding met uw lokale datacenters, moet u bieden [uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md) voor de naamomzetting tussen uw netwerken.  

### <a name="limits"></a>Limieten
Bekijk de netwerken limieten in de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel om ervoor te zorgen dat uw ontwerp niet met een van de limieten conflicteert. Sommige limieten kunnen worden verhoogd door een ondersteuningsticket te openen.

### <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)
U kunt [Azure RBAC](../active-directory/role-based-access-built-in-roles.md) om te bepalen het niveau van andere gebruikers toegang tot verschillende resources in Azure hebben kunnen. Op die manier kunt u het werk dat door uw team op basis van hun behoeften scheiden.

Wat virtuele netwerken zijn betrokken gebruikers in de **Network Contributor** rol hebben volledige controle over virtuele Azure Resource Manager-netwerkbronnen. Op deze manier voor gebruikers in de **klassieke Network Contributor** rol hebben volledig beheer over de resources van klassieke virtuele netwerk.

> [!NOTE]
> U kunt ook [uw eigen rollen maken](../active-directory/role-based-access-control-configure.md) te scheiden van de behoeften van uw administratieve.
>
>

## <a name="design"></a>Ontwerpen
Zodra u weet dat de antwoorden op de vragen in de [plannen](#Plan) sectie, controleert u het volgende voordat u uw vnet's definieert.

### <a name="number-of-subscriptions-and-vnets"></a>Het aantal abonnementen en VNets
U moet rekening houden met het maken van meerdere VNets in de volgende scenario's:

* **Virtuele machines die moeten worden geplaatst in verschillende Azure locaties**. VNets in Azure zijn regionaal. Ze niet meerdere locaties kunnen omvatten. Daarom moet u ten minste één VNet voor elke gewenste host virtuele machines in Azure-locatie.
* **Workloads die moeten worden volledig geïsoleerd van elkaar**. U kunt afzonderlijke vnet's die gebruikmaken van de dezelfde IP-adresruimten, zelfs voor verschillende werkbelastingen van elkaar isoleren.

Houd er rekening mee dat de bovenstaande grenzen per regio per abonnement zijn. Dit betekent dat u meerdere abonnementen kunt gebruiken om de limiet van resources die u in Azure onderhouden kunt te verhogen. U kunt een site-naar-site-VPN en een ExpressRoute-circuit VNets verbinden met verschillende abonnementen.

### <a name="subscription-and-vnet-design-patterns"></a>Abonnement en de ontwerppatronen VNet
De onderstaande tabel ziet enkele algemene ontwerppatronen voor abonnementen en VNets.

| Scenario | Diagram | Professionals | Nadelen |
| --- | --- | --- | --- |
| Abonnement van één, twee VNets per app |![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure1.png) |Slechts één abonnement om te beheren. |Maximum aantal VNets per Azure-regio. Daarna moet u meer abonnementen. Controleer de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel voor meer informatie. |
| Met één abonnement per app, twee VNets per app |![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure2.png) |Maakt gebruik van slechts twee VNets per abonnement. |Moeilijker te beheren wanneer er te veel apps zijn. |
| Met één abonnement per bedrijfseenheid, twee VNets per app. |![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure3.png) |Verhouding tussen aantal abonnementen en VNets. |Maximum aantal VNets per bedrijfseenheid (abonnement). Controleer de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel voor meer informatie. |
| Met één abonnement per bedrijfseenheid, twee VNets per groep van toepassingen. |![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure4.png) |Verhouding tussen aantal abonnementen en VNets. |Apps moeten worden geïsoleerd met behulp van subnetten en nsg's. |

### <a name="number-of-subnets"></a>Aantal subnetten
U moet rekening houden met meerdere subnetten in een VNet in de volgende scenario's:

* **Er is onvoldoende privé IP-adressen voor alle NIC's in een subnet**. Als uw subnetadresruimte niet voldoende IP-adressen voor het aantal NIC's in het subnet bevat, moet u meerdere subnetten maken. Houd er rekening mee dat Azure reserveert 5 privé IP-adressen van elk subnet dat kan niet worden gebruikt: de eerste en laatste adressen van de adresruimte (voor de subnetadres en multicast) en 3 adressen intern (voor DHCP en DNS-doeleinden) moet worden gebruikt.
* **Beveiliging**. U kunt subnetten gebruiken om groepen van virtuele machines van elkaar te scheiden voor werkbelastingen met een gelaagde structuur, en van toepassing op andere [netwerkbeveiligingsgroepen (nsg's)](virtual-networks-nsg.md#subnets) voor deze subnetten.
* **Hybride verbindingen**. U kunt VPN-gateways en een ExpressRoute-circuits aan [verbinding](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) uw VNets met elkaar en aan uw lokale gegevens center(s). VPN-gateways en een ExpressRoute-circuits vereist een subnet van hun eigen worden gemaakt.
* **Virtuele apparaten**. U kunt een virtueel apparaat, zoals een firewall, WAN-accelerator of VPN-gateway gebruiken in een Azure VNet. Wanneer u doet dit, moet u [verkeer routeren](virtual-networks-udr-overview.md) op deze apparaten en ze te isoleren in hun eigen subnet.

### <a name="subnet-and-nsg-design-patterns"></a>Subnet en ontwerppatronen voor NSG
De onderstaande tabel ziet enkele algemene ontwerppatronen voor het gebruik van subnetten.

| Scenario | Diagram | Professionals | Nadelen |
| --- | --- | --- | --- |
| Nsg's per toepassingslaag per app met één subnet |![Één subnet](./media/virtual-network-vnet-plan-design-arm/figure5.png) |Slechts één subnet om te beheren. |Meerdere nsg's nodig voor het isoleren van elke toepassing. |
| Eén subnet per nsg's per toepassingslaag-app |![Subnet per app](./media/virtual-network-vnet-plan-design-arm/figure6.png) |Minder nsg's om te beheren. |Meerdere subnetten om te beheren. |
| Eén subnet per toepassingslaag, nsg's per app. |![Subnet per laag](./media/virtual-network-vnet-plan-design-arm/figure7.png) |Verhouding tussen aantal subnetten en nsg's. |Maximum aantal nsg's per abonnement. Controleer de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel voor meer informatie. |
| Eén subnet per toepassingslaag per app, nsg's per subnet |![Subnet per laag per app](./media/virtual-network-vnet-plan-design-arm/figure8.png) |Mogelijk kleiner aantal nsg's. |Meerdere subnetten om te beheren. |

## <a name="sample-design"></a>Voorbeeld-ontwerp
Overweeg het volgende scenario ter illustratie van de toepassing van de informatie in dit artikel.

U werkt voor een bedrijf met 2 datacenters in Noord-Amerika en twee datacenters Europa. U hebt vastgesteld 6 verschillende gerichte toepassingen van de klant beheerd door 2 verschillende business units die u wilt migreren naar Azure als een test. De basisarchitectuur voor de toepassingen zijn als volgt:

* App1, App2 App3 en App4 zijn op Linux-servers Ubuntu gehoste webtoepassingen. Elke toepassing verbinding maakt met een afzonderlijke toepassing-server die als host fungeert van de RESTful-services op Linux-servers. De RESTful-services verbinding met een back-end MySQL-database.
* App5 en App6 zijn webtoepassingen die worden gehost op Windows-servers met Windows Server 2012 R2. Elke toepassing verbinding maakt met een back-end van SQL Server-database.
* Alle apps worden momenteel gehost in een van het bedrijf datacenters in Noord-Amerika.
* De lokale datacenters gebruiken de adresruimte 10.0.0.0/8.

U moet een virtueel netwerkoplossing ontwerpen die voldoet aan de volgende vereisten:

* Elk bedrijfsonderdeel moet niet worden beïnvloed door het brongebruik van andere bedrijfseenheden.
* U moet de hoeveelheid vnet's en subnetten om beheer te vereenvoudigen minimaliseren.
* Elk bedrijfsonderdeel moet een enkel test-/ ontwikkelingsfase gebruikt voor alle toepassingen VNet hebben.
* Elke toepassing wordt gehost in 2 verschillende Azure-datacenters per continent (Noord-Amerika en Europa).
* Elke toepassing is volledig geïsoleerd van elkaar.
* Elke toepassing toegankelijk is voor klanten via Internet met behulp van HTTP.
* Elke toepassing toegankelijk zijn voor gebruikers via een gecodeerde tunnel verbonden met de lokale datacenters.
* Verbinding met lokale datacenters moet bestaande VPN-apparaten gebruiken.
* Het bedrijf netwerken groep moet volledig beheer over de VNet-configuratie hebben.
* Ontwikkelaars in elk bedrijfsonderdeel moeten alleen mogelijk voor het implementeren van virtuele machines met bestaande subnetten.
* Alle toepassingen worden gemigreerd omdat deze naar Azure (lift-en-shift).
* De databases in elke locatie moeten worden gerepliceerd naar andere locaties Azure eenmaal per dag.
* Elke toepassing gebruik 5 front-end-webservers, 2 toepassingsservers (indien nodig) en 2-databaseservers.

### <a name="plan"></a>Plannen
U moet beginnen met uw ontwerp plannen door het beantwoorden van de vraag in de [vereisten definiëren](#Define-requirements) sectie zoals hieronder wordt weergegeven.

1. Welke Azure locaties gebruikt u host VNets?

    2 locaties in Noord-Amerika en Europa 2 locaties. U moet kiezen die zijn gebaseerd op de fysieke locatie van uw bestaande lokale datacenters. Op die manier heeft de verbinding van uw fysieke locaties in Azure een betere latentie.
2. Moet u communicatie tussen deze Azure locaties?

    Ja. Aangezien de databases moeten worden gerepliceerd naar alle locaties.
3. Moet u communicatie tussen uw Azure-VNet(s) en uw on-premises gegevens center(s) bieden?

    Ja. Omdat gebruikers met verbonden moet de lokale datacenters toegang tot de toepassingen via een gecodeerde tunnel zijn.
4. Hoeveel IaaS VM's moet u voor uw oplossing?

    200 IaaS VM's. App1, App2 App3 en App4 vereisen 5 webservers elke 2 toepassingsservers elke en 2-databaseservers elke. Dit is een totaal van 9 IaaS VM's per toepassing of 36 IaaS VM's. App5 en App6 moet 5 webservers en 2-databaseservers. Dit is een totaal van 7 IaaS VM's per toepassing of 14 IaaS VM's. Daarom moet u 50 IaaS VM's voor alle toepassingen in elke Azure-regio. Omdat we 4 regio's gebruiken moeten, zullen er 200 IaaS VM's.

    U moet ook DNS-servers in elke VNet of in uw lokale datacenters naam tussen uw Azure IaaS VM's en uw on-premises netwerk om te zetten bieden.
5. Moet u voor het isoleren van verkeer op basis van groepen met VM's (dat wil zeggen front-end-webservers en back-end-databaseservers)?

    Ja. Elke toepassing moet volledig geïsoleerd van elkaar en elke toepassingslaag moet ook worden geïsoleerd.
6. Moet u het verkeer met behulp van virtuele apparaten beheren?

    Nee. Virtuele apparaten kunnen worden gebruikt om te bieden meer controle over het netwerkverkeer, met inbegrip van meer gedetailleerde logboekregistratie voor het vlak van gegevens.
7. Moeten gebruikers verschillende sets van machtigingen voor andere Azure-resources

    Ja. Het team van de netwerken moet volledig beheer over de instellingen voor virtuele netwerken terwijl ontwikkelaars moeten alleen hun virtuele machines implementeren met vooraf bestaande subnetten.

### <a name="design"></a>Ontwerpen
U moet het ontwerp van de abonnementen, VNets, subnetten en nsg's geven volgen. Nsg's hier worden besproken, maar u moet weten over [nsg's](virtual-networks-nsg.md) voordat u uw ontwerp voltooit.

**Het aantal abonnementen en VNets**

De volgende vereisten zijn gerelateerd aan abonnementen en VNets:

* Elk bedrijfsonderdeel moet niet worden beïnvloed door het brongebruik van andere bedrijfseenheden.
* De hoeveelheid vnet's en subnetten te beperken.
* Elk bedrijfsonderdeel moet een enkel test-/ ontwikkelingsfase gebruikt voor alle toepassingen VNet hebben.
* Elke toepassing wordt gehost in 2 verschillende Azure-datacenters per continent (Noord-Amerika en Europa).

Op basis van deze vereisten, moet u een abonnement voor elk bedrijfsonderdeel. Op die manier verbruik van bronnen van een zakelijke eenheid niet meetelt limieten voor andere bedrijfseenheden. En omdat u beperken het aantal vnet's wilt, moet u overwegen de **met één abonnement per bedrijfseenheid, twee VNets per groep van toepassingen** patroon zoals hieronder wordt weergegeven.

![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure9.png)

U moet ook de adresruimte opgeven voor elk VNet. Omdat u moet connectiviteit tussen de on-premises gegevens wordt gecentreerd en de Azure-regio's, de adresruimte die wordt gebruikt voor Azure VNets kan niet conflicteren met de on-premises netwerk en de adresruimte die wordt gebruikt door elk VNet moet niet conflicteren met andere bestaande vnet's. U kunt de adresruimten in de onderstaande tabel gebruiken om te voldoen aan deze vereisten.  

| **Abonnement** | **VNet** | **Azure-regio** | **Adresruimte** |
| --- | --- | --- | --- |
| BU1 |ProdBU1US1 |VS - west |172.16.0.0/16 |
| BU1 |ProdBU1US2 |VS - oost |172.17.0.0/16 |
| BU1 |ProdBU1EU1 |Noord-Europa |172.18.0.0/16 |
| BU1 |ProdBU1EU2 |West-Europa |172.19.0.0/16 |
| BU1 |TestDevBU1 |VS - west |172.20.0.0/16 |
| BU2 |TestDevBU2 |VS - west |172.21.0.0/16 |
| BU2 |ProdBU2US1 |VS - west |172.22.0.0/16 |
| BU2 |ProdBU2US2 |VS - oost |172.23.0.0/16 |
| BU2 |ProdBU2EU1 |Noord-Europa |172.24.0.0/16 |
| BU2 |ProdBU2EU2 |West-Europa |172.25.0.0/16 |

**Aantal subnetten en nsg 's**

De volgende vereisten zijn gerelateerd aan subnetten en nsg's:

* De hoeveelheid vnet's en subnetten te beperken.
* Elke toepassing is volledig geïsoleerd van elkaar.
* Elke toepassing toegankelijk is voor klanten via Internet met behulp van HTTP.
* Elke toepassing toegankelijk zijn voor gebruikers via een gecodeerde tunnel verbonden met de lokale datacenters.
* Verbinding met lokale datacenters moet bestaande VPN-apparaten gebruiken.
* De databases in elke locatie moeten worden gerepliceerd naar andere locaties Azure eenmaal per dag.

Op basis van deze vereisten, kan u één subnet per toepassingslaag gebruiken, en nsg's gebruiken voor verkeer van het filter per toepassing. Op die manier hoeft u slechts 3 subnetten in elke VNet (front-end, toepassingslaag en gegevenslaag) en één NSG per toepassing per subnet. In dit geval moet u overwegen de **één subnet per toepassingslaag, nsg's per app** ontwerppatroon. De onderstaande afbeelding ziet u het gebruik van het ontwerp patroon voor de **ProdBU1US1** VNet.

![Eén subnet per laag, één NSG per toepassing per laag](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Echter, u moet ook een extra subnet maken voor de VPN-verbinding tussen de VNets en uw lokale datacenters. En u moet de adresruimte voor elk subnet opgeven. De afbeelding hieronder ziet u een Voorbeeldoplossing voor **ProdBU1US1** VNet. U kunt dit scenario wilt repliceren voor elk VNet. Elke kleur vertegenwoordigt een andere toepassing.

![Voorbeeld VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Toegangsbeheer**

De volgende vereisten zijn gerelateerd aan het toegangsbeheer:

* Het bedrijf netwerken groep moet volledig beheer over de VNet-configuratie hebben.
* Ontwikkelaars in elk bedrijfsonderdeel moeten alleen mogelijk voor het implementeren van virtuele machines met bestaande subnetten.

Op basis van deze vereisten, kan u gebruikers van het VPN-team toevoegen aan de ingebouwde **Network Contributor** rol in elk abonnement; en een aangepaste beveiligingsrol maken voor de toepassingsontwikkelaars in elk abonnement door ze te machtigen virtuele machines toevoegen aan bestaande subnetten.

## <a name="next-steps"></a>Volgende stappen
* [Implementeren van een virtueel netwerk](virtual-networks-create-vnet-arm-template-click.md) op basis van een scenario.
* Begrijpen hoe [verdelen](../load-balancer/load-balancer-overview.md) IaaS VM's en [beheren van routering via meerdere Azure-regio's](../traffic-manager/traffic-manager-overview.md).
* Meer informatie over [nsg's en het plannen en ontwerpen](virtual-networks-nsg.md) een NSG-oplossing.
* Meer informatie over uw [cross-premises en VNet-connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti).
