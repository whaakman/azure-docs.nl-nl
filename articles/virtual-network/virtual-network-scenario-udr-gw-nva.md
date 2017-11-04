---
title: Hybride verbinding met 2-tier-toepassing | Microsoft Docs
description: Informatie over het implementeren van virtuele apparaten en UDR de omgeving van een toepassing met meerdere lagen maken in Azure
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: jdial
ms.openlocfilehash: 544ba6484b23da425d53594622122b1e18b92359
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="virtual-appliance-scenario"></a>Virtueel apparaat scenario
Een veelvoorkomend scenario tussen groter Azure klant is de noodzaak om een toepassing twee lagen blootgesteld aan Internet, terwijl toegang tot de back-laag van een on-premises datacenter. Dit document helpt u bij een scenario waarbij de gebruiker gedefinieerde Routes (UDR), een VPN-Gateway en virtuele netwerkapparaten voor het implementeren van een omgeving met twee lagen die voldoet aan de volgende vereisten:

* Webtoepassing moet toegankelijk zijn via openbaar Internet.
* Webserver die als host fungeert voor de toepassing moet toegang hebben tot de server van een back-end-toepassing.
* Al het verkeer van Internet naar de webtoepassing moet via een virtueel firewallapparaat gaan. Deze virtueel apparaat wordt gebruikt voor Internet-verkeer.
* Alle verkeer wordt doorgestuurd naar de toepassingsserver moet gaan via een firewall virtueel apparaat. Deze virtueel apparaat wordt gebruikt voor toegang tot de back-endserver, en afkomstig uit de on-premises netwerk via een VPN-Gateway.
* Beheerders moeten kunnen de firewall virtuele apparaten van hun lokale computers beheren, met behulp van een derde firewall virtueel apparaat gebruikt uitsluitend voor beheerdoeleinden.

Dit is een standaard DMZ scenario met een Perimeternetwerk en een beveiligd netwerk. Dergelijke scenario kan worden gemaakt in Azure met nsg's, firewall virtuele apparaten of een combinatie van beide. De onderstaande tabel bevat enkele van de voor- en nadelen tussen nsg's en virtuele firewallapparaten.

|  | Professionals | Nadelen |
| --- | --- | --- |
| NSG |Er zijn geen kosten. <br/>Geïntegreerd in Azure RBAC. <br/>Regels kunnen worden gemaakt in ARM-sjablonen. |Complexiteit kan variëren in grotere omgevingen. |
| Firewall |Volledige controle over vlak van gegevens. <br/>Centraal beheer via de firewall-console. |Kosten van de firewall-apparaat. <br/>Niet geïntegreerd met Azure RBAC. |

De onderstaande oplossing maakt gebruik van virtuele firewallapparaten voor het implementeren van een netwerk DMZ/beveiligde scenario.

## <a name="considerations"></a>Overwegingen
U kunt de omgeving die hierboven worden uitgelegd in Azure met behulp van verschillende functies die beschikbaar zijn vandaag, als volgt implementeren.

* **Virtueel netwerk (VNet)**. Een Azure VNet fungeert in dezelfde manier als een on-premises netwerk en kunnen worden gesegmenteerd in een of meer subnetten voor isolatie van verkeer en scheiding van problemen.
* **Virtueel apparaat**. Diverse partners bieden virtuele apparaten in Azure Marketplace die kunnen worden gebruikt voor de drie firewalls die hierboven worden beschreven. 
* **De gebruiker gedefinieerde Routes (UDR)**. Routetabellen kunnen udr's die worden gebruikt door Azure netwerken voor het beheren van de stroom van pakketten binnen een VNet bevatten. Deze routetabellen kunnen worden toegepast op subnetten. Een van de nieuwste functies in Azure is de mogelijkheid een routetabel toepassen op het GatewaySubnet, bieden de mogelijkheid voor het doorsturen van alle verkeer dat afkomstig van een hybride verbinding is in de Azure-VNet naar een virtueel apparaat.
* **Doorsturen via IP**. Standaard de Azure VPN-engine doorsturen van pakketten naar virtuele netwerkinterfacekaarten (NIC's) alleen als het pakket doel-IP-adres overeenkomt met de NIC IP-adres. Als een UDR gedefinieerd dat een pakket naar een bepaald virtueel apparaat moet worden verzonden, zou de Azure VPN-engine daarom dat pakket verwijderen. Om te controleren of dat het pakket aan een virtuele machine (in dit geval een virtueel apparaat) dat niet het werkelijke doel voor het pakket wordt geleverd, moet u doorsturen via IP inschakelen voor het virtuele apparaat.
* **Netwerkbeveiligingsgroepen (nsg's)**. Het volgende voorbeeld maakt geen gebruik van het nsg's, maar u kunt nsg's die worden toegepast op de subnetten en/of NIC's in deze oplossing gebruiken als het verkeer van en naar deze subnetten en NIC's verder te filteren.

![IPv6-connectiviteit](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

In dit voorbeeld is er een abonnement dat het volgende bevat:

* 2 resourcegroepen, niet weergegeven in het diagram. 
  * **ONPREMRG**. Bevat alle resources die nodig zijn om te simuleren van een on-premises netwerk.
  * **AZURERG**. Bevat alle resources die nodig zijn voor de Azure virtual network-omgeving. 
* Een VNet met de naam **onpremvnet** gebruikt om na te bootsen van een on-premises datacentrum gesegmenteerde zoals hieronder weergegeven.
  * **onpremsn1**. Subnet met een virtuele machine (VM) Ubuntu na te bootsen van een on-premises server wordt uitgevoerd.
  * **onpremsn2**. Subnet met een virtuele machine Ubuntu na te bootsen van een on-premises-computer die wordt gebruikt door een beheerder uitgevoerd.
* Er is een firewall virtueel apparaat met de naam **OPFW** op **onpremvnet** gebruikt voor het onderhouden van een tunnel naar **azurevnet**.
* Een VNet met de naam **azurevnet** gesegmenteerde zoals hieronder weergegeven.
  * **azsn1**. Externe firewall subnet uitsluitend gebruikt voor de externe firewall. Al het internetverkeer wordt geleverd in via dit subnet. Dit subnet bevat alleen een NIC die is gekoppeld aan de externe firewall.
  * **azsn2**. Front-end-subnet die als host fungeert voor een virtuele machine wordt uitgevoerd als een webserver die kan worden bereikt vanaf het Internet.
  * **azsn3**. Back-end subnet die als host fungeert voor een virtuele machine een back-end-toepassingsserver die wordt geopend door de front-end-webserver uitgevoerd.
  * **azsn4**. Management-subnet worden uitsluitend gebruikt voor beheer van toegang tot alle firewall virtuele apparaten. Dit subnet bevat alleen een NIC voor elke firewall virtueel apparaat gebruikt voor de oplossing.
  * **GatewaySubnet**. Azure hybride verbinding subnet vereist voor de ExpressRoute- en VPN-Gateway voor een verbinding tussen Azure VNets en andere netwerken. 
* Er zijn 3 firewall virtuele apparaten in de **azurevnet** netwerk. 
  * **AZF1**. Externe firewall blootgesteld aan het openbare Internet met behulp van een openbare IP-adres resource in Azure. U moet zorg ervoor dat u hebt een sjabloon uit de Marketplace of rechtstreeks uit de leverancier van uw apparaat, dat voorziet in een virtueel apparaat 3-NIC.
  * **AZF2**. Interne firewall gebruikt voor het beheren van verkeer tussen **azsn2** en **azsn3**. Dit is ook een 3-NIC virtueel apparaat.
  * **AZF3**. Beheer van firewall toegankelijk voor beheerders van de on-premises datacentrum en is verbonden met een management subnet dat wordt gebruikt voor het beheren van alle firewallapparaten. U kunt 2-NIC sjablonen voor virtueel apparaat vinden in de Marketplace of vraag een rechtstreeks van de leverancier van uw apparaat.

## <a name="user-defined-routing-udr"></a>Door de gebruiker gedefinieerde Routering
Elk subnet in Azure kan worden gekoppeld aan een tabel UDR is gebruikt om te definiëren hoe verkeer wordt gestart in dat subnet wordt doorgestuurd. Als er geen udr's zijn gedefinieerd, Azure standaardroutes gebruikt om verkeer van één subnet naar andere stromen te staan. Voor een beter begrip udr's, gaat u naar [wat de gebruiker gedefinieerde Routes en doorsturen via IP zijn](virtual-networks-udr-overview.md).

Om ervoor te zorgen communicatie vindt plaats via de juiste firewallapparaat op basis van de laatste vereiste hierboven, moet u de volgende routetabel met udr's in maken **azurevnet**.

### <a name="azgwudr"></a>azgwudr
In dit scenario wordt alleen verkeer stroomt van on-premises naar Azure wordt gebruikt voor het beheren van de firewalls door verbinding te maken **AZF3**, en dat verkeer via de interne firewall moet gaan **AZF2**. Slechts één route is daarom nodig in de **GatewaySubnet** zoals hieronder wordt weergegeven.

| Doel | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Lokale gegevensverkeer te bereiken management firewall **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Doel | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |Staat toe dat verkeer het back-end-subnet voor het hosten van de toepassingsserver via **AZF2** |
| 0.0.0.0/0 |10.0.2.10 |Hiermee kunt u al het andere verkeer te routeren via **AZF1** |

### <a name="azsn3udr"></a>azsn3udr
| Doel | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |Staat toe dat verkeer **azsn2** naar de stroom van appserver naar de webserver via **AZF2** |

U moet ook routetabellen van de subnetten in maken **onpremvnet** na te bootsen van de on-premises datacentrum.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Doel | Volgende hop | Uitleg |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |Staat toe dat verkeer **onpremsn2** via **OPFW** |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Doel | Volgende hop | Uitleg |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |Hiermee kunt u verkeer naar de back-subnet in Azure maakt via **OPFW** |
| 192.168.1.0/24 |192.168.2.4 |Staat toe dat verkeer **onpremsn1** via **OPFW** |

## <a name="ip-forwarding"></a>Doorsturen via IP
UDR en doorsturen via IP zijn functies die u kunt gebruiken in combinatie waarmee virtuele apparaten moeten worden gebruikt voor het beheren van netwerkverkeer in een Azure VNet.  Een virtueel apparaat is niets meer dan een virtuele machine waarop een toepassing wordt uitgevoerd om netwerkverkeer op een bepaalde manier af te handelen, zoals een firewall of een NAT-apparaat.

Deze VM op het virtuele apparaat moet in staat zijn om binnenkomend verkeer te ontvangen dat niet is geadresseerd aan zichzelf. Om met een VM verkeer te kunnen ontvangen dat is geadresseerd aan andere bestemmingen, moet u Doorsturen via IP inschakelen voor die VM. Dit is een Azure-instelling, niet een instelling in het gastbesturingssysteem. Uw virtuele apparaat moet nog steeds worden uitgevoerd van een type van de toepassing de binnenkomend verkeer verwerken en deze op de juiste wijze te routeren.

Voor meer informatie over het doorsturen via IP, gaat u naar [wat de gebruiker gedefinieerde Routes en doorsturen via IP zijn](virtual-networks-udr-overview.md).

Stel, dat u hebt de volgende instellingen in een Azure vnet bijvoorbeeld:

* Subnet **onpremsn1** bevat een virtuele machine met de naam **onpremvm1**.
* Subnet **onpremsn2** bevat een virtuele machine met de naam **onpremvm2**.
* Een virtueel apparaat met de naam **OPFW** is verbonden met **onpremsn1** en **onpremsn2**.
* Een door de gebruiker gedefinieerde route gekoppeld aan **onpremsn1** geeft aan dat alle verkeer naar **onpremsn2** moet worden verzonden naar **OPFW**.

Op dit punt, als **onpremvm1** probeert een verbinding maken met **onpremvm2**, de UDR wordt gebruikt en dat verkeer wordt verzonden naar **OPFW** als de volgende hop. Houd er rekening mee dat het doel van de daadwerkelijke pakket niet wordt gewijzigd, wordt nog steeds aangegeven **onpremvm2** is de bestemming. 

Zonder de IP-doorsturen is ingeschakeld voor **OPFW**, de Azure virtuele netwerken logica wordt de pakketten verwijderen omdat deze alleen kan pakketten worden verzonden naar een virtuele machine als de VM IP-adres het doel voor het pakket is.

Met het doorsturen via IP stuurt de logica van de virtuele Azure-netwerk de pakketten naar OPFW, zonder dat u de oorspronkelijke doeladres wijzigt. **OPFW** moet verwerken van de pakketten en bepalen wat te doen met deze.

Voor het bovenstaande scenario om te werken, moet u inschakelen op de NIC's voor doorsturen via IP **OPFW**, **AZF1**, **AZF2**, en **AZF3** die worden gebruikt voor het doorsturen van (alle NIC's, behalve die is gekoppeld aan het subnet management). 

## <a name="firewall-rules"></a>Firewallregels
Zoals hierboven wordt beschreven, doorsturen via IP alleen zorgt ervoor dat pakketten worden verzonden naar de virtuele apparaten. Uw apparaat moet nog steeds bepalen wat te doen met de pakketten. In het scenario moet u de volgende regels maken in uw apparaten:

### <a name="opfw"></a>OPFW
OPFW vertegenwoordigt een on-premises-apparaat met de volgende regels:

* **Route**: alle verkeer naar 10.0.0.0/16 (**azurevnet**) moet worden verzonden via de tunnel **ONPREMAZURE**.
* **Beleid**: alle bidirectionele verkeer tussen **port2** en **ONPREMAZURE**.

### <a name="azf1"></a>AZF1
AZF1 vertegenwoordigt een Azure virtueel apparaat met de volgende regels:

* **Beleid**: alle bidirectionele verkeer tussen **port1** en **port2**.

### <a name="azf2"></a>AZF2
AZF2 vertegenwoordigt een Azure virtueel apparaat met de volgende regels:

* **Route**: alle verkeer naar 10.0.0.0/16 (**onpremvnet**) moet worden verzonden naar de Azure-gateway IP-adres (dat wil zeggen 10.0.0.1) via **port1**.
* **Beleid**: alle bidirectionele verkeer tussen **port1** en **port2**.

## <a name="network-security-groups-nsgs"></a>Netwerkbeveiligingsgroepen (nsg's)
Nsg's worden in dit scenario wordt niet gebruikt. U kunt echter nsg's toepassen aan elk subnet moet het inkomende en uitgaande verkeer te beperken. U kan bijvoorbeeld de volgende NSG-regels toepassen op het externe FW subnet.

**Binnenkomende**

* Alle TCP-verkeer op poort 80 op elke virtuele machine in het subnet vanaf het Internet toestaan.
* Al het andere verkeer vanaf Internet weigeren.

**Uitgaande**

* Al het verkeer naar Internet weigeren.

## <a name="high-level-steps"></a>Hoog niveau stappen
Volg de onderstaande stappen hoog niveau voor het implementeren van dit scenario.

1. Meld u aan met uw Azure-abonnement.
2. Als u implementeren van een VNet om na te bootsen van de on-premises netwerk wilt, de resources die deel van uitmaken inrichten **ONPREMRG**.
3. De resources die deel van uitmaken inrichten **AZURERG**.
4. Inrichten van de tunnel van **onpremvnet** naar **azurevnet**.
5. Zodra alle resources zijn ingericht, meld u aan bij **onpremvm2** en ping 10.0.3.101 connectiviteit tussen testen **onpremsn2** en **azsn3**.

