---
title: Overzicht en architectuur van SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Overzicht van de architectuur van het SAP HANA in Azure (grote exemplaren) implementeren.
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
ms.date: 10/31/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4ef5ec3d8f4b96d4a318e01b449d3baad8a6324a
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Overzicht van de SAP HANA (grote exemplaren) en architectuur op Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Wat is een SAP HANA in Azure (grote exemplaren)?

SAP HANA in Azure (grote exemplaar) is een unieke oplossing naar Azure. Naast het bieden van Azure Virtual Machines omwille van de implementeren en uitvoeren van SAP HANA biedt Azure u de mogelijkheid om te worden uitgevoerd en SAP HANA implementeren op bare metal-servers die zijn toegewezen aan u als klant. De SAP HANA in Azure (grote exemplaren)-oplossing is gebaseerd op niet-gedeelde/hostserver bare-metal hardware die aan u is toegewezen als een klant. De serverhardware is ingesloten in grotere stempels die compute/server-, netwerk- en opslaginfrastructuur bevatten. Dit, als een combinatie HANA TDI gecertificeerd is. De service-aanbieding voor SAP HANA in Azure (grote exemplaren) biedt verschillende SKU's van andere server of grootten die beginnen met eenheden die 72 CPU's en 768 GB geheugen aan eenheden waarvoor 960 CPU's en 20 TB geheugen hebben.

De klantisolatie binnen de infrastructuur stempel moet worden uitgevoerd in tenants, die in details ziet eruit als:

- Netwerken: Isolatie van klanten in de infrastructuur-stack via virtuele netwerken per klant toegewezen tenant. Een tenant wordt toegewezen aan één klant. Een klant kan meerdere tenants hebben. De netwerkisolatie van tenants verbiedt netwerkcommunicatie tussen de tenants op het niveau van de stempel infrastructuur. Zelfs als tenants deel uitmaken van dezelfde klant.
- Opslagonderdelen: isolatie door middel van opslag virtuele machines met opslagvolumes die zijn toegewezen. Opslagvolumes kunnen worden toegewezen aan slechts één opslag virtuele machine. Een opslag virtuele machine is toegewezen aan één één tenant in de SAP HANA TDI gecertificeerde infrastructuur-stack. Als gevolg hiervan zijn opslagvolumes die zijn toegewezen aan een virtuele machine voor opslag in een specifieke en verwante-tenant alleen toegankelijk. En zijn niet zichtbaar zijn tussen de verschillende geïmplementeerde tenants.
- Server- of host: een eenheid server- of host wordt niet gedeeld tussen klanten of tenants. Een server of de host is geïmplementeerd voor een klant, is een atomic bare-metal compute-eenheid die is toegewezen aan één voor één tenant. **Geen** hardware partitioneren of soft partitioneren wordt gebruikt die kunnen leiden tot u, als een klant, delen van een host of een server met een andere klant. Opslagvolumes die zijn toegewezen aan de opslag virtuele machine van de specifieke tenant zijn gekoppeld aan deze server. Een tenant kan een naar veel server eenheden van verschillende SKU's exclusief toegewezen hebben.
- Binnen een SAP HANA op Azure (grote exemplaar) infrastructuur stempel, zijn veel verschillende tenants geïmplementeerd en ten opzichte van elkaar geïsoleerd door middel van de tenant-concepten van netwerken, opslag en compute-niveau. 


Deze eenheden bare metal-server worden ondersteund voor SAP HANA alleen worden uitgevoerd. De toepassingslaag SAP of werkbelasting middleware-laag wordt uitgevoerd in Microsoft Azure Virtual Machines. De infrastructuur stempels de SAP HANA uitgevoerd op Azure (grote exemplaar) units zijn verbonden met de Azure-netwerk wervels, dus die lage latentie connectiviteit tussen SAP HANA op Azure (grote exemplaar) eenheden en virtuele Machines van Azure wordt geleverd.

Dit document is een van vijf documenten die betrekking hebben op het onderwerp van het SAP HANA in Azure (grote exemplaar). In dit document gaan we de basisarchitectuur, verantwoordelijkheden, de services die worden geleverd en op een hoog niveau met behulp van de functionaliteit van de oplossing. Voor het merendeel van de gebieden, zoals netwerken en verbindingen, worden de vier documenten zijn die betrekking hebben op meer informatie en keuzelijsten inzoomen. De documentatie voor SAP HANA in Azure (grote exemplaar) wordt niet uitgelegd aspecten van SAP NetWeaver-installatie of implementaties van SAP NetWeaver in Azure VM's. In dit onderwerp wordt beschreven in afzonderlijke documentatie gevonden in dezelfde container documentatie. 


De vijf onderdelen van deze handleiding komen de volgende onderwerpen:

- [Overzicht van de SAP HANA (grote exemplaar) en architectuur op Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastructuur voor SAP HANA (grote exemplaren) en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Het installeren en configureren van SAP HANA (grote exemplaren) in Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Probleemoplossing voor SAP HANA (grote exemplaren) en de controle op Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="definitions"></a>Definities

Verschillende gemeenschappelijke definities worden veel gebruikt in de architectuur en technische Deployment Guide. Houd rekening met de volgende termen en hun betekenis:

- **IaaS:** infrastructuur als een Service
- **PaaS:** Platform as a Service
- **SaaS:** Software als een Service
- **SAP-onderdeel:** een afzonderlijke SAP-toepassing, zoals ECC, BW, oplossing Manager of EP is geplaatst. SAP-onderdelen kunnen worden gebaseerd op traditionele ABAP of Java-technologieën of een NetWeaver op basis van toepassing zoals zakelijke objecten.
- **SAP-omgeving:** een of meer onderdelen voor SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke functie, zoals ontwikkeling, QAS, Training, DR of productie.
- **SAP liggend:** verwijst naar de gehele SAP bedrijfsmiddelen in uw IT-Liggend. De SAP-liggend bevat alle productie en niet-productieve omgevingen.
- **SAP-systeem:** de combinatie van laag DBMS en toepassingslaag van een SAP ERP-ontwikkelsysteem, SAP BW testsysteem, productiesysteem SAP CRM, enzovoort. Implementaties van Azure bieden geen ondersteuning voor het delen van deze twee lagen tussen on-premises en Azure. Betekent een SAP-systeem is geïmplementeerde on-premises of deze is geïmplementeerd in Azure. U kunt echter de verschillende systemen van een liggend SAP in Azure of on-premises implementeren. U kunt bijvoorbeeld de CRM SAP-ontwikkeling en tests systemen in Azure, implementeren tijdens de implementatie van de SAP CRM productie system on-premises. Het is bedoeld om u de toepassingslaag SAP van SAP-systemen in Azure VM's en het bijbehorende SAP HANA-exemplaar op een eenheid in de stempel HANA grote exemplaar host te voor SAP HANA in Azure (grote exemplaren).
- **Grote exemplaar stempel:** een hardware-infrastructuur stack die SAP HANA TDI gecertificeerd en toegewezen voor SAP HANA-exemplaren in Azure uitvoeren.
- **SAP HANA in Azure (grote exemplaren):** officiële taal voor de aanbieding in Azure uitvoeren HANA-exemplaren op SAP HANA TDI gecertificeerde hardware die wordt geïmplementeerd in grote exemplaar stempels in verschillende Azure-regio's. De verwante term **HANA grote exemplaar** kort voor SAP HANA in Azure (grote exemplaren) en veel is deze technische handleiding gebruikt.
- **Cross-Premises:** beschrijft een scenario waarin virtuele machines zijn geïmplementeerd met een Azure-abonnement met site-naar-site meerdere locaties of ExpressRoute-verbinding tussen de lokale clientresources en Azure. Gemeenschappelijk Azure-documentatie, dit soort implementaties worden ook beschreven als Cross-Premises scenario's. De reden voor de verbinding is om lokale domeinen, lokale Active Directory/OpenLDAP en lokale DNS-uitbreiden naar Azure. De lokale Liggend wordt uitgebreid naar de Azure activa van de Azure-abonnementen. Met deze uitbreiding, kunnen de virtuele machines deel uitmaken van het lokale domein. Domeingebruikers van het lokale domein hebben toegang tot de servers en services kunnen worden uitgevoerd op deze virtuele machines (zoals DBMS services). Communicatie en naamomzetting tussen de geïmplementeerde virtuele machines on-premises en geïmplementeerde virtuele machines in Azure is mogelijk. Dergelijke is het typische scenario in waarop de meeste SAP activa zijn geïmplementeerd. Zie de handleidingen van [Planning en ontwerp voor VPN-Gateway](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [maken van een VNet met een Site-naar-Site-verbinding met de Azure portal](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie gedetailleerde.
- **Tenant:** een klant die is geïmplementeerd in grote exemplaren HANA stempel opgehaald geïsoleerd in een 'tenant'. Een tenant is geïsoleerd van netwerken, opslag en berekeningslaag van andere tenants. Ja, dat opslagruimte en rekencapaciteit eenheden die zijn toegewezen aan de andere tenants niet elkaar zien of op het niveau van de stempel HANA grote exemplaar met elkaar communiceren. Een klant kunt implementaties in verschillende tenants hebben. Er is zelfs dan geen communicatie tussen de tenants op het niveau van de stempel HANA grote exemplaar.
- **SKU-categorie:** voor de grote exemplaren HANA de volgende twee categorieën van SKU's worden aangeboden.
    - **Typ ik klasse:** S72, S72m S144, S144m, S192 en S192m
    - **Klasse van het type II:** S384, S384m S384xm, S576, S768 en S960


Er zijn tal van aanvullende bronnen die zijn gepubliceerd in het onderwerp van het implementeren van SAP-werkbelasting op Microsoft Azure openbare cloud. Het is raadzaam dat iedereen plannen en uitvoeren van een SAP HANA-implementatie in Azure ervaren en op de hoogte van de principals van Azure IaaS en de implementatie van SAP werkbelastingen op Azure IaaS is. De volgende bronnen geven informatie en moeten worden verwezen voordat u verdergaat:


- [SAP-oplossingen gebruiken op Microsoft Azure virtuele machines](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Certificering

Naast de certificering NetWeaver vereist SAP een speciale voor SAP HANA ondersteuning bieden voor SAP HANA op bepaalde infrastructuren, zoals Azure IaaS-certificering.

De core SAP-notitie op NetWeaver en een zekere mate SAP HANA-certificaat is [SAP-notitie #1928533 – SAP-toepassingen in Azure: typen ondersteunde producten en de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533).

Dit [SAP Opmerking #2316233 - SAP HANA op Microsoft Azure (grote exemplaren)](https://launchpad.support.sap.com/#/notes/2316233/E) is ook belangrijk. Deze heeft de oplossing in deze handleiding beschreven. Bovendien worden ondersteund voor SAP HANA uitvoering in het type GS5 VM van Azure. [Informatie voor deze aanvraag is gepubliceerd op de website SAP](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

De SAP HANA in Azure (grote exemplaren)-oplossing in SAP-notitie #2316233 bedoelde biedt Microsoft en SAP-klanten de mogelijkheid voor het implementeren van grote SAP Business Suite, SAP Business Warehouse (BW), S/4 HANA, BW/4HANA of andere werkbelastingen SAP HANA in Azure. De oplossing is gebaseerd op de SAP HANA gecertificeerde specifieke hardware stempel ([SAP HANA aangepast Datacenter integratie – TDI](https://scn.sap.com/docs/DOC-63140)). Wordt uitgevoerd als een SAP HANA TDI biedt geconfigureerde oplossing u het vertrouwen van weten dat alle SAP HANA-toepassingen (inclusief SAP Business Suite van SAP HANA en SAP Business Warehouse (BW) voor SAP HANA, S4/HANA en BW4/HANA) wilt werken op de hardware infrastructuur.

SAP HANA uitgevoerd in Azure Virtual Machines die deze oplossing is een voordeel ten opzichte van, het zorgt voor veel grotere volumes van geheugen. Als u inschakelen van deze oplossing wilt, zijn er enkele belangrijke aspecten om te begrijpen:

- De SAP-toepassing-laag en niet-SAP-toepassingen worden uitgevoerd in Azure Virtual Machines (VM's) die worden gehost in de gebruikelijke Azure hardware stempels.
- Klant on-premises infrastructuur, datacenters en implementaties van toepassingen zijn verbonden met de Microsoft Azure cloudplatform, via Azure ExpressRoute (aanbevolen) of het virtuele particuliere netwerk (VPN). Active Directory (AD) en DNS ook worden uitgebreid naar Azure.
- Het SAP HANA-database-exemplaar voor de werkbelasting van HANA SAP HANA in Azure (grote exemplaren) wordt uitgevoerd. De stempel grote exemplaar is verbonden in Azure-netwerken, zodat de software die wordt uitgevoerd in Azure VM's kan communiceren met de HANA-exemplaar in grote HANA-exemplaren.
- Hardware van de SAP HANA in Azure (grote exemplaren) is specifieke hardware die zijn opgegeven in een infrastructuur als een Service (IaaS) met SUSE Linux Enterprise Server of Red Hat Enterprise Linux, vooraf is geïnstalleerd. Als is met Azure Virtual Machines verdere updates en onderhoud van het besturingssysteem uw verantwoordelijkheid.
- Installatie van HANA of eventuele extra onderdelen die nodig zijn voor SAP HANA uitvoeren op eenheden HANA grote exemplaren van is uw verantwoordelijkheid, evenals alle respectieve conflicterende bewerkingen worden uitgevoerd en administraties van SAP HANA op Azure.
- Naast de oplossingen die hier wordt beschreven, kunt u andere onderdelen in uw Azure-abonnement maakt verbinding met SAP HANA in Azure (grote exemplaren) installeren.  Bijvoorbeeld, de onderdelen die communicatie met en/of rechtstreeks naar de SAP HANA-database inschakelen (korte inleiding-servers, RDP-servers, SAP HANA Studio SAP Data Services voor SAP BI-scenario's of bewakingsoplossingen netwerk).
- Als in Azure bieden HANA grote exemplaren ondersteunende functionaliteit voor hoge beschikbaarheid en herstel na noodgevallen.

## <a name="architecture"></a>Architectuur

Op een hoog niveau heeft de SAP HANA in Azure (grote exemplaren)-oplossing de SAP-toepassingslaag die zich in Azure VM's en de databaselaag op SAP TDI geconfigureerd hardware zich in een grote exemplaar stempel in de Azure-regio die is verbonden met Azure IaaS .

> [!NOTE]
> U moet de toepassingslaag SAP in dezelfde Azure-regio als de laag SAP DBMS implementeren. Deze regel is goed gedocumenteerd in gepubliceerde informatie over SAP werkbelasting op Azure. 

De algehele architectuur van SAP HANA in Azure (grote exemplaren) biedt een SAP TDI gecertificeerde hardware-configuratie (niet-gevirtualiseerde, bare metal, hoge prestaties server voor SAP HANA-database) en de mogelijkheid en flexibiliteit van Azure op schaal resources voor de SAP-toepassingslaag om te voldoen aan uw behoeften.

![Overzicht van de architectuur van SAP HANA in Azure (grote exemplaren)](./media/hana-overview-architecture/image1-architecture.png)

De architectuur die wordt weergegeven, is onderverdeeld in drie secties:

- **Rechts:** een on-premises infrastructuur met verschillende toepassingen in datacenters met eindgebruikers die LOB-toepassingen (zoals SAP) openen. In het ideale geval dit lokale infrastructuur vervolgens is verbonden met Azure met Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Center:** toont Azure IaaS en in dit geval gebruik van Azure Virtual machines als host voor SAP of andere toepassingen die gebruikmaken van SAP HANA als een DBMS-systeem. Kleinere HANA-exemplaren die functie met de Azure VM's van het geheugen bieden zijn samen met hun toepassingslaag in virtuele Azure-machines geïmplementeerd. Meer informatie over [virtuele Machines](https://azure.microsoft.com/services/virtual-machines/).
<br />Azure-netwerken wordt gebruikt voor het groeperen van SAP-systemen samen met andere toepassingen in Azure virtuele netwerken (vnet's). Deze VNets verbinden met on-premises systemen ook garantie voor SAP HANA in Azure (grote exemplaren).
<br />Zie voor SAP NetWeaver toepassingen en databases die worden ondersteund voor het uitvoeren in Microsoft Azure [SAP ondersteuning Opmerking #1928533 – SAP-toepassingen in Azure: typen ondersteunde producten en de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533). Controleer voor documentatie over het implementeren van SAP-oplossingen in Azure:

  -  [SAP gebruiken op Windows virtuele machines (VM's)](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [SAP-oplossingen gebruiken op Microsoft Azure virtuele machines](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Links:** toont de SAP HANA TDI gecertificeerde hardware in de stempel grote exemplaar van Azure. De eenheden HANA grote exemplaar zijn verbonden met de Azure VNets van uw abonnement met dezelfde technologie als de connectiviteit van on-premises in Azure.

De stempel Azure grote exemplaar zelf combineert de volgende onderdelen:

- **Berekeningen:** Servers die zijn gebaseerd op Intel Xeon E7-8890v3 of Intel Xeon E7-8890v4 processors die bieden de mogelijkheid voor nodig computergebruik en SAP HANA gecertificeerd.
- **Netwerk:** A unified snel netwerk-fabric die de computer-, opslag en LAN-onderdelen met elkaar verbindt.
- **Opslag:** een opslaginfrastructuur die toegankelijk is via een netwerk-infrastructuur. Specifieke opslagcapaciteit is opgegeven, afhankelijk van de specifieke SAP HANA van Azure (grote exemplaren)-configuratie wordt geïmplementeerd (meer opslagcapaciteit is beschikbaar op een extra maandelijkse kosten).

Binnen de infrastructuur van de stempel grote exemplaar van meerdere tenants, klanten geïmplementeerd als geïsoleerde tenants. Bij de implementatie van de tenant moet u de naam van een Azure-abonnement binnen uw Azure-inschrijving. Dit gaat worden van het Azure-abonnement, de grote HANA-exemplaren moet worden gefactureerd tegen gaat. Deze tenants een 1:1 relatie hebben met het Azure-abonnement. Netwerk verstandig is het mogelijk voor toegang tot een grote exemplaar-eenheid van HANA, geïmplementeerd in een tenant in een Azure-regio in verschillende Azure VNets die tot verschillende Azure-abonnementen behoren. Hoewel deze Azure-abonnementen behoren tot de dezelfde Azure-inschrijving moeten. 

Net als bij Azure Virtual machines, wordt SAP HANA in Azure (grote exemplaren) aangeboden in meerdere Azure-regio's. Om te kunnen bieden mogelijkheden voor herstel na noodgevallen, kunt u kiezen voor deelname. Andere grote exemplaar stempels binnen één geografisch politieke regio zijn aan elkaar verbonden. Bijvoorbeeld, zijn HANA grote exemplaar stempels in VS-West en VS-Oost verbonden via een speciaal netwerkkoppeling omwille van de DR-replicatie. 

Net zoals u tussen verschillende VM typen met Azure Virtual Machines kiezen kunt, kunt u kiezen uit verschillende SKU's van HANA grote exemplaren die speciaal voor de werkbelasting van de verschillende soorten SAP HANA geschikt zijn. SAP geldt geheugen voor socket ratio voor verschillende werkbelastingen op basis van de Intel processor generaties. De volgende tabel toont de SKU-typen die worden aangeboden.

SAP HANA in Azure (grote exemplaren) is vanaf juli 2017 beschikbaar in verschillende configuraties in de Azure-regio's van VS-West, VS-Oost, Australië-Oost, Australië-Zuidoost, West-Europa en Noord-Europa:

| SAP-oplossing | CPU | Geheugen | Storage | Beschikbaarheid |
| --- | --- | --- | --- | --- |
| Geoptimaliseerd voor OLAP-: SAP BW, BW/4HANA<br /> of SAP HANA voor algemene OLAP-werkbelasting | SAP HANA op Azure S72<br /> – 2 x Intel Xeon®-Processor E7 8890 v3<br /> 36 CPU-kernen en 72 CPU threads |  768 GB |  3 TB | Beschikbaar |
| --- | SAP HANA op Azure S144<br /> – 4 x Intel Xeon®-Processor E7 8890 v3<br /> 72 CPU-kernen en 144 CPU threads |  1,5 TB |  6 TB | Niet beschikbaar meer |
| --- | SAP HANA op Azure S192<br /> – 4 x Intel Xeon®-Processor E7 8890 v4<br /> 96 CPU-kernen en 192 CPU threads |  2.0 TB |  8 TB | Beschikbaar |
| --- | SAP HANA op Azure S384<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kernen en 384 CPU threads |  4.0 TB |  16 TB | Gereed om te sorteren |
| Geoptimaliseerd voor OLTP: SAP Business Suite<br /> voor SAP HANA of S/4HANA (OLTP)<br /> algemene OLTP | SAP HANA op Azure S72m<br /> – 2 x Intel Xeon®-Processor E7 8890 v3<br /> 36 CPU-kernen en 72 CPU threads |  1,5 TB |  6 TB | Beschikbaar |
|---| SAP HANA op Azure S144m<br /> – 4 x Intel Xeon®-Processor E7 8890 v3<br /> 72 CPU-kernen en 144 CPU threads |  3.0 TB |  12 TB | Niet beschikbaar meer |
|---| SAP HANA op Azure S192m<br /> – 4 x Intel Xeon®-Processor E7 8890 v4<br /> 96 CPU-kernen en 192 CPU threads  |  4.0 TB |  16 TB | Beschikbaar |
|---| SAP HANA op Azure S384m<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kernen en 384 CPU threads |  6.0 TB |  18 TB | Gereed om te sorteren |
|---| SAP HANA op Azure S384xm<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kernen en 384 CPU threads |  8.0 TB |  22 TB |  Gereed om te sorteren |
|---| SAP HANA op Azure S576<br /> – 12 x Intel Xeon®-Processor E7 8890 v4<br /> 288 CPU-kernen en 576 CPU threads |  12.0 TB |  28 TB | Gereed om te sorteren |
|---| SAP HANA op Azure S768<br /> – 16 x Intel Xeon®-Processor E7 8890 v4<br /> 384 CPU-kernen en 768 CPU threads |  16,0 TB |  36 TB | Gereed om te sorteren |
|---| SAP HANA op Azure S960<br /> – 20 x Intel Xeon®-Processor E7 8890 v4<br /> 480 CPU-kernen en 960 CPU threads |  20,0 TB |  46 TB | Gereed om te sorteren |

- CPU-kernen = de som van de CPU-kernen niet-hyper-threaded van de som van de processors van de server-eenheid.
- CPU-threads = de som van de compute-threads geleverd door hyper-threaded CPU-kernen van de som van de processors van de server-eenheid. Alle eenheden zijn standaard geconfigureerd voor gebruik van Hyper-Threading.


De verschillende bovenstaande configuraties die beschikbaar zijn of zijn 'niet beschikbaar voor meer' wordt verwezen in [SAP ondersteuning Opmerking #2316233 – SAP HANA op Microsoft Azure (grote exemplaren)](https://launchpad.support.sap.com/#/notes/2316233/E). De configuraties die zijn gemarkeerd als 'gereed om te sorteren, vindt hun vermelding in de SAP-notitie snel. Echter kan deze exemplaar SKU's worden besteld al voor de zes verschillende Azure-regio's dat de grote exemplaar HANA-service beschikbaar is.

De specifieke configuraties gekozen zijn afhankelijk van de werkbelasting, CPU-bronnen en gewenste geheugen. Het is mogelijk voor OLTP-werkbelasting met de SKU's die zijn geoptimaliseerd voor OLAP-werkbelasting. 

De base hardware voor de aanbiedingen zijn SAP HANA TDI gecertificeerd. Maar we onderscheid maken tussen twee verschillende soorten hardware, waarbij verdeelt de SKU's in:

- S72, S72m S144, S144m, S192 en S192m die naar verwezen als 'Type ik klasse' van SKU's.
- S384, S384m S384xm, S576, S768 en S960 die naar als de 'Type II class-instructie van SKU's verwezen.

Het is belangrijk te weten dat een stempel met volledige HANA grote exemplaar niet uitsluitend is toegewezen voor één klant &#39; s gebruik. Dit geldt voor rekken van berekenings- en bronnen die verbonden zijn via een netwerk fabric ook geïmplementeerd in Azure. Grote exemplaren HANA-infrastructuur, zoals Azure, implementeert u verschillende klanten &quot;tenants&quot; die zijn geïsoleerd van elkaar in de volgende drie niveaus:

- : Netwerkisolatie via virtuele netwerken in het stempel HANA grote exemplaar.
- Opslag: Isolatie door middel van opslag virtuele machines die opslagvolumes hebt toegewezen en isoleren opslagvolumes tussen de tenants.
- COMPUTE: Toewijzing van server eenheden toegewezen aan een enkele tenant. Geen vaste of zachte partitionering van eenheden van de server. Er is geen delen van een enkele server of de hostnaam eenheid tussen de tenants. 

De implementaties van eenheden tussen verschillende tenants HANA grote exemplaren zijn als zodanig niet zichtbaar zijn met elkaar. Noch HANA grote exemplaar eenheden is geïmplementeerd in verschillende tenants kan communiceren direct met elkaar op het niveau van de stempel HANA grote exemplaar. Alleen HANA grote exemplaar eenheden binnen één tenant kunnen met elkaar communiceren op het niveau van de stempel HANA grote exemplaar.
Een geïmplementeerde tenant in de grote exemplaar stempel is toegewezen aan de facturering verstandig aan één Azure-abonnement. Netwerk echter verstandig dat deze kan worden geopend op Azure VNets van andere Azure-abonnementen binnen de dezelfde Azure-inschrijving. Als u met een andere Azure-abonnement in dezelfde Azure-regio implementeert, kunt ook u om een gescheiden HANA grote exemplaar tenant te vragen.

Er zijn belangrijke verschillen tussen actieve SAP HANA op grote HANA-exemplaren en SAP HANA uitgevoerd op Azure Virtual machines die zijn geïmplementeerd in Azure:

- Er is geen virtualisatielaag voor SAP HANA in Azure (grote exemplaren). Krijgt u de prestaties van de onderliggende bare-metal hardware.
- In tegenstelling tot Azure, worden de SAP HANA op Azure (grote exemplaren)-server is toegewezen aan een specifieke klant. Er is geen mogelijkheid om een server-eenheid of de host moeilijk of soft gepartitioneerd is. Als gevolg hiervan wordt een grote HANA-exemplaar gebruikt als in zijn geheel in een tenant en met die aan u toegewezen als een klant. Een opnieuw opstarten of afsluiten van de server leidt niet automatisch tot het besturingssysteem en SAP HANA wordt geïmplementeerd op een andere server. (Voor het Type ik klasse SKU's, de enige uitzondering hierop is als een server problemen optreden kan en opnieuw implementeren gaande moet worden uitgevoerd op een andere server.)
- In tegenstelling tot Azure, waarbij host processortypen zijn geselecteerd voor de beste prijs/prestatie-verhouding, zijn de processortypen gekozen voor SAP HANA in Azure (grote exemplaren) de hoogste prestaties van de regel Intel E7v3 en E7v4 processor.


### <a name="running-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Meerdere exemplaren van de SAP HANA uitgevoerd op één grote exemplaar HANA-eenheid
Het is mogelijk voor het hosten van meer dan één actieve SAP HANA-exemplaar op de eenheden HANA grote exemplaar. Een dergelijke configuratie vereist om te kunnen bieden voor de mogelijkheden van opslag-momentopnamen en herstel na noodgevallen, een volume per exemplaar instellen. Vanaf nu worden de eenheden HANA grote exemplaar als volgt ingedeeld:

- S72, S72m, S144, S192: In stappen van 256 GB met de kleinste begin eenheid van 256 GB. Verschillende stappen zoals 256 GB, 512 GB, enzovoort, kunnen worden gecombineerd tot het maximum van het geheugen van de eenheid.
- S144m en S192m: In stappen van 256 GB met de kleinste eenheid van 512 GB. Verschillende stappen zoals 512 GB, 768 GB, enzovoort, kunnen worden gecombineerd tot het maximum van het geheugen van de eenheid.
- Typ de klasse II: In stappen van 512 GB met de kleinste begin eenheid van 2 TB. Verschillende stappen zoals 512 GB, 1 TB, 1,5 TB, enzovoort, kunnen worden gecombineerd tot het maximum van het geheugen van de eenheid.

Enkele voorbeelden van meerdere instanties voor SAP HANA kunnen eruitzien als:

| SKU | Geheugengrootte | Opslaggrootte | Grootten met meerdere databases |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA exemplaar<br /> of 1 x 512 GB exemplaar + 1 x 256 GB-exemplaar<br /> of 3 x 256 GB exemplaren | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA exemplaren<br />of 1 x 512 GB exemplaar + 1 x 1 TB exemplaar<br />of 6 x 256 GB exemplaren<br />of 1x1.5 TB exemplaar | 
| S192m | 4 TB | 16 TB | 8 x 512 GB exemplaren<br />of 4 x 1 TB exemplaren<br />of exemplaren van 4 x 512 GB + 2 x 1 TB exemplaren<br />of exemplaren van 4 x 768 GB + 2 x 512 GB exemplaren<br />of 1 x 4 TB-exemplaar |
| S384xm | 8 TB | 22 TB | 4 x 2 TB exemplaren<br />of 2 x 4 TB exemplaren<br />of 2 x 3 TB exemplaren + 1 x 2 TB exemplaren<br />of 2x2.5 TB exemplaren + 1 x 3 TB exemplaren<br />of 1 x 8 TB exemplaar |


U het idee te krijgen. Er zijn veel andere variaties ook. 

### <a name="using-sap-hana-data-tiering-and-extension-nodes"></a>Met behulp van lagen van SAP HANA gegevens en de extensie knooppunten
SAP biedt ondersteuning voor een model gegevens lagen voor SAP BW van verschillende SAP NetWeaver releases en SAP BW/4HANA. Details met betrekking tot aan de gegevens in lagen model u in dit document en waarnaar wordt verwezen in dit document door SAP-blog vindt: [SAP BW/4HANA SAP BW ON HANA met SAP HANA-EXTENSIE knooppunten en](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Met grote exemplaren HANA, kunt u optie 1-configuratie van SAP HANA-extensie knooppunten zoals beschreven in deze Veelgestelde vragen over en SAP-blog-documenten. Optie 2-configuraties kunnen worden ingesteld met de volgende HANA grote exemplaar SKU's: S72m, S192 S192m, S384 en S384m.  
Kijken naar de documentatie van het voordeel mogelijk niet direct zichtbaar. Maar op zoek naar de SAP sizing richtlijnen, ziet u een voordeel met behulp van de optie-1 en de optie 2 SAP HANA-extensie-knooppunten. Hier een voorbeeld:

- Richtlijnen voor SAP HANA-formaat is meestal de dubbele hoeveelheid gegevensvolume als geheugen vereist. Wanneer u uw SAP HANA-exemplaar met de hot gegevens uitvoert, dus u hoeft alleen 50% of minder van het geheugen gevuld met gegevens. De rest van het geheugen wordt in het ideale geval bewaard voor SAP HANA tijdens het doorzoeken van het werk.
- Dat betekent in een eenheid HANA grote exemplaar S192 met 2 TB aan geheugen, met een SAP BW-database, hoeft u slechts 1 TB als gegevensvolume.
- Als u een extra SAP HANA-extensie-knooppunt van optie 1, ook een S192 HANA grote exemplaar SKU, deze krijgt u een extra capaciteit 2 TB voor gegevensvolume. In de configuratie van de optie 2 zelfs en aanvullende 4 TB voor warme gegevensvolume. Vergeleken met het knooppunt hot, worden de volledige geheugencapaciteit van de 'warme' extensieknooppunt gebruikt voor gegevens opslaan voor optie 1 en dubbele die het geheugen kan worden gebruikt voor het gegevensvolume in knooppuntconfiguratie voor optie 2 SAP HANA-extensie.
- Als gevolg hiervan u uiteindelijk eindigen met een capaciteit van 3 TB voor uw gegevens en een ratio van hot naar warme 1:2 voor optie 1 en 5 TB aan gegevens en een ratio van 1:4 in knooppuntconfiguratie optie 2-extensie.

Echter, hoe groter het gegevensvolume vergeleken met het geheugen, hoe groter de kans op die zijn dat de warme gegevens u vragen voor wordt opgeslagen op de schijfopslag.


## <a name="operations-model-and-responsibilities"></a>Operations-model en verantwoordelijkheden

De service die wordt geleverd met SAP HANA in Azure (grote exemplaren) wordt uitgelijnd met Azure IaaS-services. Krijgt u een grote exemplaren HANA-exemplaar met een geïnstalleerde besturingssysteem die is geoptimaliseerd voor SAP HANA. Net als bij Azure IaaS VM's, de meeste taken beperking van het besturingssysteem van aanvullende software geïnstalleerd moet u, HANA installeren, besturingssystemen en HANA, besturingssysteem en bijwerken van het besturingssysteem en HANA is uw verantwoordelijkheid. Microsoft biedt geen OS-updates of updates HANA afdwingen op u.

![De verantwoordelijkheden van SAP HANA in Azure (grote exemplaren)](./media/hana-overview-architecture/image2-responsibilities.png)

Zoals u in het bovenstaande diagram zien kunt, is SAP HANA in Azure (grote exemplaren) een multitenant infrastructuur als een Service-aanbieding. En als gevolg hiervan de deling van de verantwoordelijkheid van de grens OS-infrastructuur voor het grootste deel. Microsoft is verantwoordelijk voor alle aspecten van de service onder de regel van het besturingssysteem en u bent zelf verantwoordelijk boven de regel, met inbegrip van het besturingssysteem. Meest recente lokale methoden u kan worden om voor naleving, beveiliging, toepassingsbeheer, basis en OS-management kunnen dus blijven worden gebruikt. De systemen worden weergegeven alsof ze zich in uw netwerk ten opzichte van alle.

Deze service is echter wel geoptimaliseerd voor SAP HANA zodat er gebieden waar u en Microsoft werken samen moeten om de onderliggende infrastructuurmogelijkheden gebruiken voor de beste resultaten.

De volgende lijst geeft meer details over elk van de lagen en uw verantwoordelijkheden:

**Netwerken:** alle interne netwerken voor SAP HANA, de toegang tot de opslag, connectiviteit tussen de exemplaren (voor scale-out en andere functies), verbinding met de liggend en verbindingen naar Azure met grote exemplaar stempel waar de toepassingslaag SAP wordt gehost in Azure Virtual Machines. Dit omvat ook WAN-verbinding tussen Azure-datacenters voor herstel na noodgevallen doeleinden replicatie. Alle netwerken zijn gepartitioneerd door de tenant en QOS hebt toegepast.

**Opslag:** de gevirtualiseerde opslag voor alle volumes die door de SAP HANA-servers nodig, evenals voor momentopnamen is gepartitioneerd. 

**Servers:** de toegewezen fysieke servers naar de toegewezen aan tenants SAP HANA-databases worden uitgevoerd. De servers van het Type ik klasse van SKU's zijn gescheiden hardware. Met deze typen servers, de serverconfiguratie worden verzameld en onderhouden in profielen die kunnen worden verplaatst van één fysieke hardware voor een andere fysieke hardware. (Dergelijke een handmatig) verplaatsen van een profiel door bewerkingen kan enigszins naar Azure Service healing worden vergeleken. De servers van het Type II klasse SKU's zijn niet zoals een mogelijkheid bieden.

**SDDC:** de beheersoftware die wordt gebruikt voor het beheren van gegevens wordt gecentreerd als software-gedefinieerde entiteiten. Dit kan Microsoft naar bronnen voor schaal, beschikbaarheid en prestaties van toepassingen.

**Besturingssysteem:** het besturingssysteem kiest u (SUSE Linux of Red Hat Linux) die wordt uitgevoerd op de servers. U vindt u installatiekopieën van het besturingssysteem zijn de afbeeldingen geleverd door de leverancier van de afzonderlijke Linux naar Microsoft omwille van de SAP HANA uitgevoerd. U zijn hebt een abonnement met de leverancier Linux voor de specifieke geoptimaliseerd voor SAP HANA-installatiekopie vereist. Uw verantwoordelijkheden zijn installatiekopieën van het registreren met de leverancier van het besturingssysteem. Vanaf het moment van overdracht door Microsoft bent u ook verantwoordelijk voor eventuele verdere patchen van het Linux-besturingssysteem. Deze patchen ook extra pakketten die mogelijk nodig is voor een geslaagde installatie van de SAP HANA bevat (Zie de SAP HANA installatie documentatie en SAP opmerkingen) en die niet zijn opgenomen door de leverancier van de specifieke Linux in hun SAP HANA geoptimaliseerd OS afbeeldingen. De verantwoordelijkheid van de klant tevens patchen van het besturingssysteem die is gerelateerd aan storing/optimalisatie van het besturingssysteem en de stuurprogramma's die betrekking hebben op de specifieke serverhardware. Of een beveiligings- of functioneel patchen van het besturingssysteem. De verantwoordelijkheid van klant is ook bewakings- en capaciteitsplanning van:

- Resource-CPU-verbruik
- Geheugengebruik
- Volumes op schijven die zijn gerelateerd aan vrije ruimte, IOPS en latentie
- Volume van netwerkverkeer tussen HANA grote exemplaar en de toepassingslaag SAP

De onderliggende infrastructuur van grote exemplaren HANA biedt functionaliteit voor back-up en herstel van het volume met het besturingssysteem. Deze functionaliteit is ook uw eigen verantwoordelijkheid.

**Middleware:** de SAP HANA-exemplaar, voornamelijk. Bent u verantwoordelijk voor beheer, bewerkingen en controle. Er is functionaliteit opgegeven waarmee u opslag-momentopnamen gebruiken voor back-up/herstel en herstel na noodgevallen doeleinden. Deze mogelijkheden worden geleverd door de infrastructuur. Uw verantwoordelijkheden zijn echter ook hoge beschikbaarheid of herstel na noodgevallen ontwerpen met deze mogelijkheden, gebruik van deze, en de bewaking dat opslag-momentopnamen met succes uitgevoerd.

**Gegevens:** uw gegevens worden beheerd door een SAP HANA en andere gegevens zoals back-ups van bestanden op volumes of bestand deelt. Uw verantwoordelijkheden zijn vrije schijfruimte controleren en het beheren van de inhoud op de volumes en het controleren van de voltooiing van uitvoering van de back-ups van volumes op schijven en opslag-momentopnamen. Geslaagde uitvoering van de gegevensreplicatie naar DR-sites is echter de verantwoordelijkheid van Microsoft.

**Toepassingen:** de SAP-toepassingsexemplaren of, in geval van een niet-SAP-toepassingen, de toepassingslaag van deze toepassingen. Uw verantwoordelijkheden zijn implementatie, beheer, bewerkingen en controle van deze toepassingen die betrekking hebben op het plannen van capaciteit van het verbruik van CPU, geheugengebruik, Azure Storage-verbruik en netwerkbandbreedte in Azure VNets en naar Azure VNets aan SAP HANA in Azure (grote exemplaren).

**WAN:** de verbindingen die u tot stand van on-premises naar Azure implementaties voor werkbelastingen brengen. Onze klanten met grote exemplaren HANA gebruik Azure ExpressRoute voor connectiviteit. Deze verbinding maakt geen deel uit van de SAP HANA in Azure (grote exemplaren)-oplossing, zodat u zelf verantwoordelijk voor het instellen van deze verbinding bent.

**Archief:** u mogelijk liever archiveren kopieën van gegevens met uw eigen methoden in de storage-accounts. Archiveren vereist management, naleving, kosten en bewerkingen. U bent zelf verantwoordelijk voor het archiveren kopieert en back-ups op Azure genereren en opslaan in een compatibele manier.

Zie de [SLA voor SAP HANA in Azure (grote exemplaren)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Schaling

Sizing voor HANA grote exemplaren gaat niet anders dan in het algemeen voor HANA formaat. Voor bestaande en geïmplementeerd systemen, u wilt verplaatsen van andere RDBMS naar HANA, SAP biedt een aantal rapporten die worden uitgevoerd op uw bestaande SAP-systemen. Als de database is verplaatst naar HANA, worden deze rapporten Controleer de gegevens en geheugenvereisten voor het exemplaar HANA berekenen. Lees de volgende SAP-opmerkingen voor meer informatie over het uitvoeren van deze rapporten en het verkrijgen van hun meest recente patches/versies:

- [SAP-notitie #1793345 - formaat voor SAP-Suite op HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP-notitie #1872170 - Suite op HANA en S/4 HANA sizing rapport](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-notitie #2121330 - Veelgestelde vragen over: SAP BW op HANA Sizing rapport](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP-notitie #1736976 - schaling rapport voor BW op HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP-notitie #2296290 - nieuw rapport voor schaling voor BW op HANA](https://launchpad.support.sap.com/#/notes/2296290)

Groen veld implementaties is snelle Sizer SAP voor geheugenvereisten van de implementatie van software SAP bovenop HANA berekenen.

Geheugenvereisten voor HANA meer naarmate gegevensvolume toeneemt, dus u moet rekening houden met het geheugenverbruik nu en kunnen voorspellen wat er gebeurt in de toekomst. Op basis van de geheugenvereisten, u kunt vervolgens toe te wijzen uw vraag een van de HANA grote exemplaar SKU's.

## <a name="requirements"></a>Vereisten

Deze lijst samengevoegd naast de vereisten voor SAP HANA uitgevoerd op Azure (grotere exemplaren).

**Microsoft Azure:**

- Een Azure-abonnement dat kan worden gekoppeld aan een SAP HANA in Azure (grote exemplaren).
- Microsoft Premier Support-Contract. Zie [SAP ondersteuning Opmerking #2015553 – SAP op Microsoft Azure: ondersteuning vereisten](https://launchpad.support.sap.com/#/notes/2015553) voor specifieke informatie met betrekking tot SAP in Azure wordt uitgevoerd. HANA grote exemplaar eenheden met 384 en meer CPU's gebruikt, moet u ook uitbreiden Premier Support-contract Azure snelle reactie (ARR) opnemen.
- Kennis van de HANA grote exemplaren SKU's u moet na het uitvoeren van een sizing oefenen met SAP.

**Netwerkverbinding:**

- Azure ExpressRoute tussen on-premises naar Azure: voor uw on-premises datacentrum verbinding met Azure, zorg ervoor dat u ten minste 1 Gbps-verbindingen rangschikken van uw Internetprovider. 

**Besturingssysteem:**

- Licenties voor SUSE Linux Enterprise Server 12 voor SAP-toepassingen.

> [!NOTE] 
> Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd bij SUSE, noch het met een SMT-exemplaar is verbonden.

- SUSE Linux abonnement Management Tool (SMT) geïmplementeerd in Azure op een Azure VM. Dit biedt de mogelijkheid voor SAP HANA in Azure (grote exemplaren) worden geregistreerd en respectievelijk door SUSE bijgewerkt (zoals er geen internettoegang binnen grote exemplaren HANA datacenter is). 
- Licenties voor Red Hat Enterprise Linux 6.7 of 7.2 voor SAP HANA.

> [!NOTE]
> Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd bij Red Hat, noch is het verbonden met een exemplaar van Red Hat abonnement Manager.

- Red Hat abonnement Manager is geïmplementeerd in Azure op een Azure VM. De Red Hat abonnement Manager biedt de mogelijkheid voor SAP HANA in Azure (grote exemplaren) worden geregistreerd en respectievelijk op Red Hat bijgewerkt (zoals er geen directe toegang tot internet vanaf binnen de tenant die wordt geïmplementeerd op de stempel grote exemplaar van Azure is).
- SAP moet u een ondersteuningsaanvraag hebben contract met uw Linux-provider. Deze vereiste is niet gewist door de oplossing van grote HANA-exemplaren of het feit dat uw Linux uitvoeren in Azure. In tegenstelling tot met enkele van de installatiekopieën van het galerie Linux Azure is de servicekosten niet opgenomen in de aanbieding oplossing voor grote HANA-exemplaren. Het is voor u als klant om te voldoen aan de vereisten van SAP met betrekking tot support-contract met de distributor Linux.   
   - SUSE Linux, zoekt u de vereisten van het ondersteuningscontract in [SAP Opmerking #1984787 - SUSE LINUX Enterprise Server 12: opmerkingen bij de installatie](https://launchpad.support.sap.com/#/notes/1984787) en [SAP Opmerking #1056161 - SUSE prioriteit ondersteuning voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux moet u het juiste abonnement toegangsniveaus die zijn ondersteuning en service (updates voor de besturingssystemen van grote HANA-exemplaren. Red Hat raadt aan om een 'RHEL voor SAP Business-toepassingen' abonnement ophalen. Controleren met betrekking tot ondersteuning en services [SAP Opmerking #2002167 - Red Hat Enterprise Linux 7.x: installatie en Upgrade](https://launchpad.support.sap.com/#/notes/2002167) en [SAP Opmerking #1496410 - Red Hat Enterprise Linux 6.x: installatie en Upgrade](https://launchpad.support.sap.com/#/notes/1496410) voor meer informatie.

**Database:**

- Licenties en software-installatie van onderdelen voor SAP HANA (platform of enterprise edition).

**Toepassingen:**

- Licenties en software-installatie-onderdelen voor alle SAP-toepassingen verbinding met SAP HANA en gerelateerde SAP ondersteuning voor contracten.
- Licenties en software-installatie-onderdelen voor alle toepassingen die niet SAP gebruikt in relatie tot SAP HANA op Azure (grote exemplaren)-omgeving en gerelateerde support-contract.

**Vaardigheden:**

- Ervaring en kennis op Azure IaaS en de bijbehorende onderdelen.
- Ervaring en kennis over het implementeren van de werkbelasting SAP in Azure.
- Gecertificeerd persoonlijke SAP HANA-installatie.
- SAP architect vaardigheden voor ontwerp hoge beschikbaarheid en herstel na noodgevallen rond SAP HANA.

**SAP:**

- Verwachting is dat u een SAP-klant bent en een hebben contract met SAP
- Met name voor implementaties op de klasse Type II van HANA grote exemplaar SKU's, is het raadzaam overleg met SAP op versies van SAP HANA en uiteindelijke-configuraties van groot formaat omhoog schalen-hardware.


## <a name="storage"></a>Storage

De opslagindeling voor SAP HANA in Azure (grote exemplaren) wordt geconfigureerd via de SAP HANA op Azure Service Management via SAP aanbevolen richtlijnen beschreven in de [opslagvereisten voor SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) witboek.

De HANA grote exemplaren van het Type ik klasse worden geleverd met vier keer het geheugen volume als opslagvolume. Voor het type klasse II van grote exemplaar HANA eenheden zal de opslag niet worden vier keer meer. De eenheden worden geleverd met een volume dat bestemd is voor het opslaan van HANA transactielogboekback-ups. Meer informatie in [installeren en configureren van SAP HANA (grote exemplaren) in Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Zie de volgende tabel in termen van de opslagtoewijzing van. De tabel bevat grofweg de capaciteit voor de verschillende volumes die zijn opgegeven met de verschillende HANA grote exemplaar eenheden.

| HANA grote exemplaar SKU | Hana/gegevens | Hana/logboek | Hana/gedeeld | Hana/log/back-up |
| --- | --- | --- | --- | --- |
| S72 | 1280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3328 GB | 768 GB |1280 GB | 768 GB |
| S192 | 4608 GB | 1.024 GB | 1536 GB | 1.024 GB |
| S192m | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384 | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384m | 12.000 GB | 2050 GB | 2050 GB | 2040 GB |
| S384xm | 16.000 GB | 2050 GB | 2050 GB | 2040 GB |
| S576 | 20.000 GB | 3100 GB | 2050 GB | 3100 GB |
| S768 | 28.000 GB | 3100 GB | 2050 GB | 3100 GB |
| S960 | 36.000 GB | 4100 GB | 2050 GB | 4100 GB |


Werkelijke geïmplementeerde volumes kunnen verschillen enigszins op basis van de implementatie en het hulpprogramma dat wordt gebruikt om de volumegrootten weer te geven.

Als u een exemplaar van HANA groot SKU onderverdelen, wordt een paar voorbeelden van mogelijke deling stukken eruit als:

| Geheugenpartitie in GB | Hana/gegevens | Hana/logboek | Hana/gedeeld | Hana/log/back-up |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1792 GB | 640 GB | 1.024 GB | 640 GB |
| 1536 | 3328 GB | 768 GB | 1280 GB | 768 GB |


Deze grootten zijn ruwe volume cijfers die kan verschillen enigszins verschillen op basis van de implementatie en hulpprogramma's gebruikt om te kijken naar de volumes. Er zijn ook andere partitiegrootten thinkable zoals 2,5 TB. Deze maximale grootten zou worden berekend met een vergelijkbare formule als gebruikt voor de bovenstaande partities. De term 'partitions' geeft niet aan het besturingssysteem, geheugen of CPU-bronnen een manier gepartitioneerd zijn. NET geeft u aan opslag partities voor de verschillende HANA-exemplaren die u mogelijk wilt implementeren op een enkele grote exemplaar HANA-eenheid. 

Als een klant wellicht moet u voor meer opslagruimte, hebt u de mogelijkheid om opslag aanschaffen van extra opslagruimte in eenheden van 1 TB toevoegen. Deze extra opslagruimte kan worden toegevoegd als extra volume of kan worden gebruikt voor het uitbreiden van een of meer van de bestaande volumes. Het is niet mogelijk te verlagen de grootte van de volumes oorspronkelijk geïmplementeerd en voornamelijk gedocumenteerd door de bovenstaande tabellen. Het is ook niet mogelijk om te wijzigen van de namen van de volumes of namen koppelen. De opslagvolumes zoals hierboven beschreven zijn gekoppeld aan de eenheden HANA grote exemplaar als NFS4 volumes.

Als een klant kunt u opslag-momentopnamen gebruiken voor back-up/herstel en noodherstel hersteldoeleinden. Meer informatie over dit onderwerp worden beschreven in [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest
Opslagruimte wordt gebruikt voor grote HANA-exemplaren kan een transparante codering van de gegevens, zoals deze wordt opgeslagen op de schijven. U hebt de mogelijkheid voor dit soort versleuteling ingeschakeld tijdens de implementatie van een exemplaar van HANA grote eenheid. U kunt ook wijzigen in versleutelde volumes na de implementatie al. De overgang van niet-versleutelde naar versleutelde volumes is transparant en vereist geen een uitvaltijd. 

Met het Type dat ik klasse SKU's, het volume het opstarten van LUN is opgeslagen, worden versleuteld. In geval van het type klasse II van SKU's van HANA grote exemplaren moet u voor het versleutelen van het bestand Boot.ini LUN met OS-methoden. Voor meer informatie contact op met het beheer van de Service Microsoft-team.


## <a name="networking"></a>Netwerken

De architectuur van de Azure-netwerken is een belangrijk onderdeel voor een geslaagde implementatie van SAP-toepassingen op grote HANA-exemplaren. SAP HANA in implementaties van Azure (grote exemplaren) hebben doorgaans een groter SAP Liggend met verschillende andere SAP-oplossingen met verschillende grootten van databases, verbruik van CPU en geheugengebruik. Waarschijnlijk zijn niet alle die SAP-systemen gebaseerd op SAP HANA, zodat uw SAP-liggend is waarschijnlijk een hybride die gebruikmaakt van:

- SAP systemen lokale geïmplementeerd. Vanwege de grootte kunnen niet deze systemen die momenteel worden gehost in Azure. een klassieke voorbeeld hiervan is een productie SAP ERP-systeem waarop Microsoft SQL Server (als de database) waarvoor meer CPU of geheugenbronnen die virtuele Azure-machines kunnen bieden.
- SAP SAP HANA gebaseerde systemen lokale geïmplementeerd.
- Geïmplementeerde SAP-systemen in Azure VM's. Deze systemen kunnen ontwikkeling, testen, sandbox, of productie instanties voor een van de SAP NetWeaver-toepassingen in Azure (op VM's) implementeren kunnen, op basis van gebruiks- en behoefte aan bronnen. Deze systemen ook kunnen worden gebaseerd op zoals SQL Server-databases (Zie [SAP ondersteuning Opmerking #1928533 – SAP-toepassingen in Azure: typen ondersteunde producten en de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533/E)) of SAP HANA (Zie [SAP HANA gecertificeerd IaaS Platforms](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- SAP toepassingsservers in Azure (op VM's) die gebruikmaken van de SAP HANA in Azure (grote exemplaar) in Azure grote exemplaar stempels geïmplementeerd.

Een hybride SAP liggend (met vier of meer verschillende implementatiescenario's) is typerend, maar er zijn veel aanvragen van de klant van volledige SAP Liggend in Azure wordt uitgevoerd. Als u Microsoft Azure VM's worden steeds krachtiger, wordt het aantal klanten verplaatsen van alle hun SAP-oplossingen in Azure verhogen.

Er is geen Azure-netwerken in de context van SAP-systemen die zijn geïmplementeerd in Azure ingewikkeld. Deze is gebaseerd op de volgende principes:

- Virtuele netwerken van Azure (vnet's) moet worden verbonden met het Azure ExpressRoute-circuit dat verbinding met on-premises netwerk maakt.
- Een ExpressRoute-circuit verbinding maken met on-premises naar Azure meestal moet een bandbreedte van 1 Gbps of hoger hebben. Deze minimale bandbreedte kunt voldoende bandbreedte voor de gegevensoverdracht tussen on-premises systemen en systemen die worden uitgevoerd op Azure Virtual machines (evenals verbinding met Azure systemen van eindgebruikers on-premises).
- Alle SAP-systemen in Azure moeten worden ingesteld in Azure VNets met elkaar communiceren.
- Active Directory en DNS gehost lokale van on-premises worden uitgebreid naar Azure via ExpressRoute.


> [!NOTE] 
> Vanuit het facturering oogpunt alleen Eén Azure-abonnement kan alleen aan één één tenant in een grote exemplaar stempel in een specifieke Azure-regio worden gekoppeld en als u daarentegen een enkele grote exemplaar stempel tenant alleen aan één Azure-abonnement kan worden gekoppeld. Dit feit is niet anders dan bij andere factureerbare objecten in Azure

U implementeert SAP HANA in Azure (grote exemplaren) in meerdere verschillende Azure-regio's, resulteert in een afzonderlijke tenant om te worden geïmplementeerd in de stempel grote exemplaar. U kunt beide onder dezelfde Azure-abonnement echter uitvoeren, zolang deze instanties deel van de dezelfde mate van SAP uitmaken. 

> [!IMPORTANT] 
> Alleen Azure Resource Management-implementatie wordt ondersteund met SAP HANA in Azure (grote exemplaren).

 

### <a name="additional-azure-vnet-information"></a>Aanvullende informatie voor Azure VNet

Om een Azure-VNet verbinding met ExpressRoute, een Azure-gateway moet worden gemaakt (Zie [over virtuele netwerkgateways voor ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Een Azure-gateway kan worden gebruikt met ExpressRoute naar een infrastructuur buiten Azure (of een grote Azure exemplaar stempel), of verbinding maken tussen Azure VNets (Zie [voor Resource Manager, met behulp van PowerShellvaneenVNet-naar-VNet-verbindingconfigureren](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). U kunt verbinding maken met de Azure gateway tot maximaal vier verschillende ExpressRoute-verbindingen, zolang deze verbindingen afkomstig zijn uit verschillende MS Enterprise randen (MSEE)-routers.  Zie voor meer informatie [SAP HANA (grote exemplaren)-infrastructuur en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> De doorvoer die een Azure-gateway biedt verschilt voor beide gevallen gebruikt (Zie [over VPN-Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). De maximale doorvoer die we met een VNet-gateway bereiken kunt is 10 Gbps, met behulp van een ExpressRoute-verbinding. Houd er rekening mee dat het kopiëren van bestanden tussen een Azure-virtuele machine die zich in een Azure VNet en een systeem lokale (als een stream met één exemplaar) biedt de volledige doorvoer van de verschillende gateway-SKU's niet bereiken. Als u wilt gebruikmaken van de volledige bandbreedte van de VNet-gateway, moet u meerdere streams gebruiken of andere bestanden in de parallelle stromen van één enkel bestand kopiëren.


### <a name="networking-architecture-for-hana-large-instances"></a>Architectuur van de netwerken voor grote HANA-exemplaren
De netwerkarchitectuur voor grote exemplaren HANA kan zoals hieronder aangegeven, worden onderverdeeld in vier verschillende onderdelen:

- On-premises netwerken en een ExpressRoute-verbinding met Azure. Dit onderdeel is het domein van klanten en verbonden met Azure via ExpressRoute. Dit is het gedeelte in de rechterbenedenhoek van de onderstaande afbeeldingen.
- Azure Networking zo kort hierboven besproken met Azure VNets waarvoor opnieuw gateways. Dit is een gebied waarin u wilt zoeken naar de juiste ontwerpen voor uw vereisten voor toepassingen, beveiligings- en nalevingsvereisten. Met grote HANA-instanties is een ander punt van overweging in termen van het aantal vnet's en Azure gateway-SKU's waaruit u kunt kiezen. Dit is het gedeelte in de rechterbovenhoek van de afbeeldingen.
- Connectiviteit van grote exemplaren HANA via ExpressRoute technologie in Azure. Dit onderdeel is geïmplementeerd en uitgevoerd door Microsoft. U hoeft te doen als een klant te bieden een aantal IP-adresbereiken en na de implementatie van uw assets in HANA grote exemplaren verbinding maken met de ExpressRoute-circuit aan de Azure-VNet(s) (Zie [SAP HANA (grote exemplaren)-infrastructuur en connectiviteit op Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
- Netwerken in grote exemplaren HANA, die voornamelijk transparant is voor u als klant.

![Azure VNet verbinding met SAP HANA op Azure (grote exemplaren) en on-premises](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Het feit dat u grote HANA exemplaren verandert niet de vereiste om uw on-premises activa verbonden via ExpressRoute naar Azure. Deze wordt ook niet de vereiste voor een of meerdere VNets met de Azure VM's op welke host de toepassingslaag die verbinding met de HANA-exemplaren maakt die worden gehost in grote exemplaar HANA eenheden gewijzigd. 

Het verschil op SAP-implementatie in pure Azure wordt geleverd met de feiten die:

- De eenheden HANA grote exemplaar van de tenant van uw klant zijn verbonden via een ander ExpressRoute-circuit in uw Azure-VNet(s). Om te kunnen verschillende belastingsvoorwaarden, worden de routers dezelfde in de on-premises naar Azure VNets ExpressRoute en koppelingen tussen Azure VNets en grote HANA exemplaren niet delen.
- Het profiel van de werkbelasting tussen de toepassingslaag SAP en het exemplaar HANA is een andere aard van veel kleine aanvragen en burst zoals gegevens verzendt (resultaatsets) van SAP HANA in de toepassingslaag.
- De architectuur van de toepassing SAP is meer gevoelig voor netwerklatentie dan typische scenario's waarbij gegevens opgehaald uitgewisseld tussen on-premises en Azure.
- De VNet-gateway is ten minste twee ExpressRoute-verbindingen en beide verbindingen delen de maximale bandbreedte voor binnenkomende gegevens van de VNet-gateway.

De netwerklatentie tussen Azure VM's en grote HANA exemplaar eenheden ervaren kan hoger zijn dan een typische round trip latentie van de VM-VM-netwerk. Afhankelijk van de Azure-regio, de meetwaarden kunnen overschrijden de 0,7 ms round trip latentie geclassificeerd zoals hieronder gemiddelde in [SAP Opmerking #1100926 - Veelgestelde vragen over: prestaties van het netwerk](https://launchpad.support.sap.com/#/notes/1100926/E). Niettemin klanten geïmplementeerd op basis van een SAP HANA SAP productietoepassingen zeer uitgevoerd op grote SAP HANA-exemplaren. De klanten die zijn geïmplementeerd gerapporteerd verbeterd door het uitvoeren van hun SAP-toepassingen op SAP HANA HANA grote exemplaar eenheden wilt gebruiken. Niettemin moet u uw bedrijfsprocessen grondig testen in Azure HANA grote exemplaren.
 
Om te voorzien van deterministische netwerklatentie tussen Azure VM's en grote HANA-exemplaar, is de keuze van de Azure-VNet-Gateway-SKU essentieel. In tegenstelling tot de verkeerspatronen tussen on-premises en Azure VM's kunt het patroon verkeer tussen virtuele Azure-machines en grote exemplaren HANA ontwikkelen kleine maar groot lichtflitsen aanvragen en gegevensvolumes worden verzonden. Om dergelijke bursts goed verwerkt, raden we het gebruik van de Gateway-SKU UltraPerformance. Voor de klasse Type II HANA grote exemplaar SKU is het gebruik van de gateway UltraPerformance SKU als Azure VNet Gateway verplicht.  

> [!IMPORTANT] 
> Gezien de algehele netwerkverkeer tussen de SAP-toepassing en de lagen van de database, wordt alleen de HighPerformance of UltraPerformance gateway-SKU's voor VNets voor de verbinding met SAP HANA in Azure (grote exemplaren) ondersteund. Voor grote HANA exemplaar Type II SKU's, wordt alleen de UltraPerformance Gateway-SKU als Azure VNet Gateway ondersteund.



### <a name="single-sap-system"></a>Eenmalige SAP-systeem

De on-premises infrastructuur hierboven is verbonden via ExpressRoute in Azure en het ExpressRoute-circuit in een Microsoft Enterprise Edge Router (MSEE) verbinding maakt (Zie [technisch overzicht van ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Zodra die route verbinding tot stand gebracht, in de Microsoft Azure-backbone en alle Azure-regio's toegankelijk zijn.

> [!NOTE] 
> SAP landschappen in Azure wordt uitgevoerd, verbinding maken met het dichtst bij de Azure-regio in de SAP-liggend MSEE. Azure grote exemplaar stempels zijn verbonden via speciale MSEE apparaten netwerklatentie tussen virtuele machines in Azure IaaS en grote exemplaar stempels Azure minimaliseren.

De gateway VNet voor de Azure VM's, die als host fungeert voor SAP toepassingsexemplaren, is verbonden met dat ExpressRoute-circuit en hetzelfde VNet is verbonden met een aparte MSEE Router die specifiek zijn voor het verbinden met grote exemplaar stempels.

Dit is een eenvoudig voorbeeld van een enkel SAP-systeem, waarbij de toepassingslaag SAP wordt gehost in Azure en de SAP HANA-database wordt uitgevoerd op SAP HANA in Azure (grote exemplaren). De veronderstelling is de VNet-gateway-bandbreedte van 2 Gbps of 10 Gbps doorvoer vertegenwoordigt geen een knelpunt.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Meerdere SAP-systemen of grote SAP-systemen

Als er meerdere SAP-systemen of grote SAP-systemen worden geïmplementeerd verbinding met SAP HANA op Azure (grote exemplaren), &#39; vormen s redelijkerwijs wordt ervan uitgegaan dat de doorvoer van de VNet-gateway een knelpunt. In dat geval moet u de toepassingslagen splitsen in meerdere Azure VNets. Het ook mogelijk recommendable speciale VNets die verbinding met de grote exemplaren HANA voor zulke gevallen maken maken:

- Uitvoeren van back-ups rechtstreeks vanuit de HANA-exemplaren in grote HANA-exemplaren op een virtuele machine in Azure die als host fungeert voor NFS-shares
- Back-ups van grote of andere bestanden kopiëren van grote exemplaar HANA eenheden schijfruimte beheerd in Azure.

Met behulp van afzonderlijke VNets dat host virtuele machines die de opslag te beheren op het milieu op grote bestanden voorkomt of overdracht van gegevens van grote HANA-exemplaren aan Azure op de VNet-Gateway die voor de virtuele machines met de toepassingslaag SAP fungeert. 

Voor een meer schaalbare netwerkarchitectuur:

- Maak gebruik van meerdere Azure VNets voor een enkele, grotere SAP toepassingslaag.
- Implementeer een afzonderlijke Azure VNet voor elk SAP-systeem dat is geïmplementeerd, vergeleken met de SAP-systemen in afzonderlijke subnetten in hetzelfde VNet combineren.

 Een meer schaalbare netwerkarchitectuur voor SAP HANA in Azure (grote exemplaren):

![SAP toepassingslaag over meerdere Azure VNets wordt geïmplementeerd](./media/hana-overview-architecture/image4-networking-architecture.png)

De toepassingslaag SAP of onderdelen, over meerdere Azure VNets zoals hierboven, wordt geïmplementeerd onvermijdelijke latentie overhead die is opgetreden tijdens de communicatie tussen de toepassingen die worden gehost in de Azure VNets geïntroduceerd. Standaard wordt het netwerkverkeer tussen virtuele Azure-machines zich in verschillende VNets doorsturen via de MSEE-routers in deze configuratie. Echter sinds September 2016 kan deze routering worden geoptimaliseerd. De manier te optimaliseren en de latentie in de communicatie tussen de twee VNets beperken is door de peering Azure VNets binnen dezelfde regio. Zelfs als de VNets die tot verschillende abonnementen behoren. Met Azure VNet-peering, via de communicatie tussen VM's in twee verschillende Azure VNets het netwerk van Azure-backbone rechtstreeks met elkaar communiceren. Gelijksoortige latentie weergegeven waardoor als de virtuele machines in hetzelfde VNet zou worden. Terwijl het verkeer, IP-adresbereiken die zijn verbonden via de gateway Azure VNet adressering doorgestuurd via de afzonderlijke VNet-gateway van het VNet. U krijgt meer informatie over Azure-VNet-peering in het artikel [VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Routering in Azure

Er zijn twee belangrijke netwerk aandachtspunten voor routering voor SAP HANA in Azure (grote exemplaren):

1. SAP HANA in Azure (grote exemplaren) zijn alleen toegankelijk voor virtuele Azure-machines in de toegewezen ExpressRoute-verbinding; niet rechtstreeks vanuit een on-premises. Bepaalde beheerclients en alle toepassingen die directe toegang, zoals SAP oplossing Manager on-premises uitgevoerd nodig kunnen geen verbinding maken met de SAP HANA-database.

2. SAP HANA op Azure (grote exemplaren) eenheden hebben een toegewezen IP-adres van de groep met IP-adres u liggen als de klant ingediende (Zie [SAP HANA (grote exemplaren)-infrastructuur en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie).  Dit IP-adres is toegankelijk via de Azure-abonnementen en ExpressRoute die verbinding Azure VNets met HANA in Azure (grote exemplaren maakt). Het IP-adres toegewezen buiten die servergroep IP-adresbereik rechtstreeks aan de hardware-eenheid is toegewezen en is niet NAT'ed meer als dit het geval is in de eerste implementatie van deze oplossing is. 

> [!NOTE] 
> Als u verbinding maken met de SAP HANA in Azure (grote exemplaren wilt) een _datawarehouse_ scenario, waarin toepassingen en/of eindgebruikers verbinding moeten maken met de SAP HANA-database (rechtstreeks wordt uitgevoerd), een ander onderdeel toegang moet worden gebruikt: een omgekeerde proxy voor het routeren van gegevens naar en uit. Zoals F5 BIG-IP, NGINX met Traffic Manager, geïmplementeerd in Azure als virtuele firewall/verkeer routering oplossing.

### <a name="internet-connectivity-of-hana-large-instances"></a>Verbinding met Internet van grote HANA-exemplaren
Grote exemplaren HANA hoeft geen directe verbinding met internet. Dit is uw mogelijkheden voor het registreren van de installatiekopie van het besturingssysteem bijvoorbeeld rechtstreeks met de leverancier van het besturingssysteem te beperken. Daarom moet u mogelijk werken met lokale SLES SMT-server of RHEL abonnement Manager

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Versleuteling van gegevens tussen Azure VM's en grote HANA-exemplaren
Gegevens die overgedragen tussen HANA grote exemplaren en Azure VM's is niet versleuteld. U kunt echter uitsluitend voor de uitwisseling tussen de HANA DBMS en op basis van ODBC-en JDBC toepassingen codering van het verkeer inschakelen. Verwijzing [deze documentatie door SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false)

### <a name="using-hana-large-instance-units-in-multiple-regions"></a>Gebruik HANA grote exemplaar eenheden in meerdere regio 's

U hebt mogelijk andere redenen voor het implementeren van SAP HANA in Azure (grote exemplaren) in meerdere Azure-regio's, naast het herstel na noodgevallen. Misschien wilt u toegang tot grote HANA-exemplaren van elk van de VM is geïmplementeerd in de verschillende VNets in de regio's. Omdat de IP-adressen toegewezen aan de andere grote exemplaren HANA eenheden worden niet doorgegeven naar buiten de Azure VNets (die rechtstreeks zijn verbonden via de gateway van de exemplaren), er is een kleine wijziging in het ontwerp van de VNet die hierboven zijn geïntroduceerd: een Azure-VNet gateway kan verwerken vier verschillende ExpressRoute-circuits buiten verschillende msee's en elk VNet dat is verbonden met een van de stempels grote exemplaar kan worden verbonden met de stempel grote exemplaar in een andere Azure-regio.

![Azure VNets die zijn verbonden met Azure grote exemplaar stempels in verschillende Azure-regio 's](./media/hana-overview-architecture/image8-multiple-regions.png)

De bovenstaande afbeelding ziet u hoe de andere Azure VNets in beide regio's zijn verbonden met twee verschillende ExpressRoute-circuits zijn gebruikt voor verbinding met SAP HANA in Azure (grote exemplaren) in beide Azure-regio's. De nieuwe verbindingen zijn de rechthoekige rode lijnen. Met deze verbindingen, buiten de VNets Azure de virtuele machines in een van deze VNets met toegang tot elk van de verschillende HANA grote exemplaren eenheden dat is geïmplementeerd in de twee regio's. Zoals u in de bovenstaande afbeeldingen ziet, wordt ervan uitgegaan dat u twee ExpressRoute-verbindingen van on-premises twee Azure-regio's hebt; aanbevolen voor herstel na noodgevallen redenen.

> [!IMPORTANT] 
> Als u meerdere ExpressRoute-circuits gebruikt, moeten lokale voorkeur BGP-instellingen en AS-padtoevoeging correcte routering van verkeer worden gebruikt.


