---
title: Hybride verbinding met 2-tier-toepassing | Microsoft Docs
description: Meer informatie over het implementeren van virtuele apparaten en UDR, zodat deze de omgeving van een toepassing met meerdere lagen maken in Azure
services: virtual-network
documentationcenter: na
author: KumudD
manager: carmonm
editor: tysonn
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: kumud
ms.openlocfilehash: 1bdc485dfb352144e8a8d0fb75965cbb78288e2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64575577"
---
# <a name="virtual-appliance-scenario"></a>Virtueel apparaat scenario
Een veelvoorkomend scenario tussen grotere Azure-klant is de hoeft op te geven van een toepassing met twee lagen die toegang heeft tot het Internet, terwijl toegang tot de back-laag van een on-premises datacenter. Dit document begeleidt u stapsgewijs door van een scenario voor het gebruik van de gebruiker gedefinieerde Routes (UDR), een VPN-Gateway en virtuele netwerkapparaten voor het implementeren van een omgeving met twee lagen die voldoet aan de volgende vereisten:

* Web-App moet toegankelijk zijn via het openbare Internet alleen.
* Webserver die als host fungeert voor de toepassing moet toegang hebben tot een back-end-toepassingsserver.
* Al het verkeer van Internet naar de webtoepassing moet via een virtueel apparaat van de firewall gaan. Deze virtueel apparaat wordt gebruikt voor Internet-verkeer.
* Al het verkeer naar de toepassingsserver moet gaan via een firewall virtueel apparaat. Deze virtueel apparaat wordt gebruikt voor toegang tot de back-end-server, en die afkomstig zijn van de on-premises netwerk via een VPN-Gateway.
* Beheerders moeten kunnen de firewall virtuele apparaten van hun on-premises computers te beheren, met behulp van een derde firewall virtueel apparaat gebruikt uitsluitend bedoeld voor beheerdoeleinden.

Dit is een standaard perimeter network (ook knowns als DMZ)-scenario met een DMZ en een beveiligd netwerk. Dit scenario kan worden gemaakt in Azure met behulp van nsg's, virtuele firewallapparaten of een combinatie van beide. De onderstaande tabel ziet u enkele van de voor- en nadelen tussen nsg's en firewall virtuele apparaten.

|  | Professionals | Nadelen |
| --- | --- | --- |
| NSG |Er zijn geen kosten. <br/>Geïntegreerd in Azure RBAC. <br/>Regels kunnen worden gemaakt in Azure Resource Manager-sjablonen. |Complexiteit kan variëren in grotere omgevingen. |
| Firewall |Volledige controle over de gegevenslaag. <br/>Centraal beheer via de firewall-console. |Kosten van firewall-apparaat. <br/>Niet geïntegreerd met Azure RBAC. |

De onderstaande oplossing maakt gebruik van virtuele firewallapparaten voor het implementeren van een perimeternetwerk (DMZ) / scenario voor het netwerk beveiligd.

## <a name="considerations"></a>Overwegingen
U kunt de omgeving die hierboven is uitgelegd in Azure met behulp van verschillende functies die beschikbaar vandaag, als volgt implementeren.

* **Virtueel netwerk (VNet)** . Een Azure VNet fungeert in soortgelijke wijze als bij een on-premises netwerk en kan worden gesegmenteerd in een of meer subnetten om netwerkverkeer te isoleren en scheiding van taken te bieden.
* **Virtueel apparaat**. Diverse partners bieden virtuele apparaten in de Azure Marketplace die kunnen worden gebruikt voor de drie firewalls die hierboven worden beschreven. 
* **Gebruiker gedefinieerde Routes (UDR)** . Routetabellen mag udr's die door Azure-netwerken worden gebruikt voor het beheren van de stroom van pakketten binnen een VNet. Deze routetabellen kunnen worden toegepast op subnetten. Een van de nieuwste functies van Azure is de mogelijkheid een routetabel toepassen op het GatewaySubnet, biedt de mogelijkheid om alle verkeer die in het Azure-VNet afkomstig zijn van een hybride verbinding naar een virtueel apparaat te sturen.
* **Doorsturen via IP**. Standaard de Azure VPN-engine doorsturen van pakketten naar virtuele netwerkinterfacekaarten (NIC's) alleen als het pakket doel-IP-adres overeenkomt met het NIC-IP-adres. Als in een UDR gedefinieerd dat een pakket moet worden verzonden naar een bepaald virtueel apparaat, wordt de Azure VPN-engine daarom dat pakket verwijderen. Om te controleren of dat het pakket aan een virtuele machine (in dit geval een virtueel apparaat) dat niet het werkelijke doel van het pakket wordt geleverd, moet u doorsturen via IP inschakelen voor het virtuele apparaat.
* **Netwerkbeveiligingsgroepen (nsg's)** . In het volgende voorbeeld maakt geen gebruik van nsg's, maar u kunt nsg's die worden toegepast op de subnetten en/of NIC's in deze oplossing gebruiken om het verkeer naar en uit deze subnetten en NIC's, verder te filteren.

![IPv6-connectiviteit](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

In dit voorbeeld is er een abonnement dat u het volgende bevat:

* 2 resourcegroepen, niet weergegeven in het diagram. 
  * **ONPREMRG**. Bevat alle resources die nodig zijn voor het simuleren van een on-premises netwerk.
  * **AZURERG**. Bevat alle resources die nodig zijn voor de Azure virtual network-omgeving. 
* Een VNet met de naam **onpremvnet** gebruikt om na te bootsen van een on-premises datacentrum gesegmenteerd zoals hieronder vermeld.
  * **onpremsn1**. Subnet met een virtuele machine (VM) met Ubuntu om na te bootsen van een on-premises server.
  * **onpremsn2**. Subnet met een virtuele machine met Ubuntu om na te bootsen van een on-premises computer die wordt gebruikt door een beheerder.
* Er is een firewall-virtueel apparaat, met de naam **OPFW** op **onpremvnet** gebruikt voor het onderhouden van een tunnel naar **azurevnet**.
* Een VNet met de naam **azurevnet** gesegmenteerd zoals hieronder vermeld.
  * **azsn1**. Externe firewall subnet uitsluitend voor de externe firewall gebruikt. Al het internetverkeer wordt geleverd via dit subnet. Dit subnet bevat alleen een NIC die is gekoppeld aan de externe firewall.
  * **azsn2**. Front-end-subnet die als host fungeert voor een virtuele machine die wordt uitgevoerd als een webserver die kan worden bereikt vanaf het Internet.
  * **azsn3**. Back-end-subnet die als host fungeert voor een virtuele machine met een back-end-toepassingsserver die wordt gebruikt door de front-end-webserver.
  * **azsn4**. Beheersubnet gebruikt uitsluitend voor van beheertoegang tot alle firewall virtuele apparaten. Dit subnet bevat alleen een NIC voor elke firewall virtueel apparaat in de oplossing gebruikt.
  * **GatewaySubnet**. Azure hybrid connection-subnet is vereist voor ExpressRoute en VPN-Gateway voor connectiviteit tussen Azure vnet's en andere netwerken. 
* Er zijn 3 firewall virtuele apparaten in de **azurevnet** netwerk. 
  * **AZF1**. Externe firewall toegang heeft tot het openbare Internet met behulp van een openbare IP-adresresource in Azure. U wilt controleren of dat u hebt een sjabloon uit de Marketplace, of rechtstreeks vanuit de leverancier van uw apparaat, die bepaalde een 3-NIC virtueel apparaat.
  * **AZF2**. Interne firewall gebruikt voor het beheren van verkeer tussen **azsn2** en **azsn3**. Dit is ook een 3-NIC virtueel apparaat.
  * **AZF3**. Management firewall toegankelijk zijn voor beheerders van de on-premises datacenter en is verbonden met een beheersubnet gebruikt voor het beheren van alle firewallapparaten. U kunt 2 NIC sjablonen voor virtueel apparaat vinden in de Marketplace, of vraag een rechtstreeks vanuit de leverancier van uw apparaat.

## <a name="user-defined-routing-udr"></a>Gebruiker gedefinieerde Routering
Elk subnet in Azure kan worden gekoppeld aan een UDR-tabel die wordt gebruikt om te definiëren hoe verkeer wordt gestart in dat subnet wordt doorgestuurd. Als er geen udr's zijn gedefinieerd, gebruikt Azure standaardroutes waarmee verkeer van het ene subnet naar een andere. Voor meer informatie over udr's, gaat u naar [wat zijn de gebruiker gedefinieerde Routes en doorsturen via IP](virtual-networks-udr-overview.md).

Om ervoor te zorgen communicatie vindt plaats via de juiste firewall-apparaat, op basis van de laatste bovenstaande vereiste dat u wilt maken van de volgende routetabel udr's in met **azurevnet**.

### <a name="azgwudr"></a>azgwudr
In dit scenario wordt het alleen verkeer van on-premises naar Azure worden gebruikt voor het beheren van de firewalls door verbinding te maken met **AZF3**, en dat verkeer via de interne firewall moet gaan **AZF2**. Slechts één route is daarom noodzakelijk zijn in de **GatewaySubnet** zoals hieronder wordt weergegeven.

| Bestemming | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Hiermee kunt u on-premises verkeer management firewall bereiken **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Bestemming | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |Hiermee kunt u verkeer naar de back endsubnet die als host fungeert voor de toepassingsserver via **AZF2** |
| 0.0.0.0/0 |10.0.2.10 |Hiermee kunt u al het andere verkeer worden gerouteerd via **AZF1** |

### <a name="azsn3udr"></a>azsn3udr
| Bestemming | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |Hiermee kunt u verkeer naar **azsn2** aan stroom vanaf de appserver naar de webserver via **AZF2** |

U moet ook routetabellen voor de subnetten in maken **onpremvnet** om na te bootsen van het on-premises datacenter.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Bestemming | Volgende hop | Uitleg |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |Hiermee kunt u verkeer naar **onpremsn2** via **OPFW** |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Bestemming | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |Hiermee kunt u verkeer naar het subnet van de back-ups in Azure via **OPFW** |
| 192.168.1.0/24 |192.168.2.4 |Hiermee kunt u verkeer naar **onpremsn1** via **OPFW** |

## <a name="ip-forwarding"></a>Doorsturen via IP
UDR en doorsturen via IP zijn functies die u kunt gebruiken in combinatie om toe te staan van virtuele apparaten moet worden gebruikt voor het beheren van netwerkverkeer in een Azure VNet.  Een virtueel apparaat is niets meer dan een virtuele machine waarop een toepassing wordt uitgevoerd om netwerkverkeer op een bepaalde manier af te handelen, zoals een firewall of een NAT-apparaat.

Deze VM op het virtuele apparaat moet in staat zijn om binnenkomend verkeer te ontvangen dat niet is geadresseerd aan zichzelf. Om met een VM verkeer te kunnen ontvangen dat is geadresseerd aan andere bestemmingen, moet u Doorsturen via IP inschakelen voor die VM. Dit is een Azure-instelling, niet een instelling in het gastbesturingssysteem. Er moet nog steeds uw virtueel apparaat om uit te voeren van een soort van toepassing op het binnenkomende verkeer verwerken en op de juiste wijze te routeren.

Voor meer informatie over het doorsturen via IP, gaat u naar [wat zijn de gebruiker gedefinieerde Routes en doorsturen via IP](virtual-networks-udr-overview.md).

Stel, dat u hebt de volgende instellingen in een Azure vnet als een voorbeeld:

* Subnet **onpremsn1** bevat een virtuele machine met de naam **onpremvm1**.
* Subnet **onpremsn2** bevat een virtuele machine met de naam **onpremvm2**.
* Een virtueel apparaat met de naam **OPFW** is verbonden met **onpremsn1** en **onpremsn2**.
* Een gebruiker gedefinieerde route is gekoppeld aan **onpremsn1** geeft aan dat alle verkeer naar **onpremsn2** moeten worden verzonden naar **OPFW**.

Op dit punt, als **onpremvm1** probeert een verbinding met **onpremvm2**, wordt de UDR gebruikt en verkeer wordt verzonden naar **OPFW** als de volgende hop. Houd er rekening mee dat de bestemming van het daadwerkelijke pakket niet wordt gewijzigd, wordt nog steeds aangegeven **onpremvm2** is de bestemming. 

Zonder de IP-doorsturen is ingeschakeld voor **OPFW**, de Azure virtuele netwerken logica doorlaat de pakketten, omdat alleen pakketten worden verzonden naar een virtuele machine als IP-adres van de VM de bestemming voor het pakket is.

Met doorsturen via IP stuurt de logica van de Azure-netwerk de pakketten naar OPFW, zonder dat u de oorspronkelijke doeladres wijzigt. **OPFW** moet de pakketten verwerken en te bepalen wat u ermee kunt doen.

Voor het bovenstaande scenario wilt werken, moet u inschakelen op de NIC's voor doorsturen via IP **OPFW**, **AZF1**, **AZF2**, en **AZF3** die worden gebruikt voor routering (alle NIC's, behalve die gekoppeld aan het beheersubnet). 

## <a name="firewall-rules"></a>Firewallregels
Zoals hierboven beschreven, doorsturen via IP alleen zorgt ervoor dat pakketten worden verzonden naar de virtuele apparaten. Uw apparaat is nog steeds nodig heeft om te bepalen wat te doen met deze pakketten. In het bovenstaande scenario moet u de volgende regels in uw apparaten te maken:

### <a name="opfw"></a>OPFW
OPFW staat voor een on-premises apparaat dat u de volgende regels bevat:

* **route**: Al het verkeer naar 10.0.0.0/16 (**azurevnet**) moeten worden verzonden via de tunnel **ONPREMAZURE**.
* **Beleid**: Toestaan dat al het verkeer in twee richtingen tussen **port2** en **ONPREMAZURE**.

### <a name="azf1"></a>AZF1
AZF1 vertegenwoordigt een Azure-virtuele apparaat met de volgende regels:

* **Beleid**: Toestaan dat al het verkeer in twee richtingen tussen **port1** en **port2**.

### <a name="azf2"></a>AZF2
AZF2 vertegenwoordigt een Azure-virtuele apparaat met de volgende regels:

* **route**: Al het verkeer naar 10.0.0.0/16 (**onpremvnet**) moeten worden verzonden naar de Azure-gateway IP-adres (dat wil zeggen 10.0.0.1) via **port1**.
* **Beleid**: Toestaan dat al het verkeer in twee richtingen tussen **port1** en **port2**.

## <a name="network-security-groups-nsgs"></a>Netwerkbeveiligingsgroepen (nsg's)
In dit scenario, worden nsg's niet gebruikt. U kunt echter nsg's toepassen op elk subnet om te beperken van binnenkomende en uitgaande verkeer. U kunt bijvoorbeeld de volgende NSG-regels toepassen op het externe FW-subnet.

**Incoming**

* Alle TCP-verkeer vanaf het Internet toestaan op poort 80 op alle virtuele machines in het subnet.
* Al het andere verkeer vanaf Internet weigeren.

**Uitgaande**

* Al het verkeer naar Internet weigeren.

## <a name="high-level-steps"></a>Hoog niveau stappen
Volg de onderstaande stappen op hoog niveau voor het implementeren van dit scenario.

1. Meld u aan met uw Azure-abonnement.
2. Als u een VNet wilt volgens de on-premises netwerk implementeert, richt u de resources die deel van uitmaken **ONPREMRG**.
3. Richt u de resources die deel van uitmaken **AZURERG**.
4. Inrichten van de tunnel van **onpremvnet** naar **azurevnet**.
5. Nadat alle resources zijn ingericht, zich aanmelden bij **onpremvm2** en 10.0.3.101 als u wilt testen van verbinding tussen een ping **onpremsn2** en **azsn3**.

