---
title: Overzicht en architectuur van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Overzicht van de architectuur van SAP HANA op Azure (grote instanties) implementeren.
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
ms.date: 08/27/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d28b6ea5612a3db539c51d2603c3f12282ca519
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090413"
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Overzicht van de SAP HANA (grote instanties) en architectuur op Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Wat is de SAP HANA op Azure (grote instanties)?

SAP HANA op Azure (grote instanties) is een unieke oplossing naar Azure. Naast het leveren van virtuele machines voor het implementeren en uitvoeren van SAP HANA, biedt Azure u de mogelijkheid voor het uitvoeren en implementeren van SAP HANA op bare-metal-servers die aan u zijn toegewezen. De SAP HANA op Azure (grote instanties)-oplossing is gebaseerd op niet-gedeelde/hostserver bare-metal hardware die aan u is toegewezen. De serverhardware is ingesloten in grotere stempels met compute/server-, netwerk- en opslaginfrastructuur. Als een combinatie is HANA die zijn afgestemd center gegevensintegratie (TDI) gecertificeerd. SAP HANA op Azure (grote instanties) biedt een andere server SKU's of grootten. Eenheden kunnen hebben 36 Intel CPU-kernen en biedt 768 GB van geheugen en gaat u naar maximaal eenheden die maximaal 480 Intel CPU-kernen hebben en maximaal 24 uur per dag TB aan geheugen.

De klantisolatie binnen de stempel infrastructuur wordt uitgevoerd in tenants, die lijkt op:

- **Netwerken**: isolatie van klanten in de infrastructuur-stack via virtuele netwerken per klanttenant toegewezen. Een tenant is toegewezen aan één klant. Een klant kan meerdere tenants hebben. De netwerkisolatie van tenants verbiedt netwerkcommunicatie tussen tenants op het niveau van de stempel infrastructuur, zelfs als de tenants deel uitmaken van dezelfde klant.
- **Opslagonderdelen**: isolatie door middel van opslag virtuele machines met opslagvolumes die zijn toegewezen. Opslagvolumes die kunnen worden toegewezen aan slechts één opslag virtuele machine. Een opslag virtuele machine wordt toegewezen aan één enkele tenant in de SAP HANA TDI-gecertificeerde infrastructuur-stack. Als gevolg hiervan kunnen opslagvolumes die zijn toegewezen aan een virtuele machine voor opslag worden geopend in een specifieke en gerelateerde tenant alleen. Ze zijn niet zichtbaar zijn tussen de verschillende geïmplementeerde tenants.
- **Server of de host**: een server of de host-eenheid niet worden gedeeld tussen klanten of tenants. Een server of de host is geïmplementeerd voor een klant, is een atomische bare metal-compute-eenheid die is toegewezen aan één enkele tenant. *Geen* hardware partitioneren of zachte partitioneren wordt gebruikt dat kan leiden tot u delen van een host of een server met een andere klant. Opslagvolumes die zijn toegewezen aan de opslag virtuele machine van de specifieke tenant zijn gekoppeld aan een dergelijke server. Een tenant kunt beschikken over een aantal eenheden van de server van verschillende SKU's exclusief toegewezen.
- Binnen een SAP HANA op Azure (grote instanties)-infrastructuur stempel, zijn veel verschillende tenants geïmplementeerd en ten opzichte van elkaar geïsoleerd door middel van de tenant-concepten in netwerken, opslag en Computing-niveau. 


Deze eenheden bare metal-server worden ondersteund voor het uitvoeren van SAP HANA alleen. De SAP-toepassingslaag of workload middleware-laag wordt uitgevoerd in virtuele machines. De stempels infrastructuur met SAP HANA op Azure (grote instanties) eenheden zijn verbonden met de Azure-netwerk services wervels. Op deze manier krijgt u connectiviteit met lage latentie tussen virtuele machines en SAP HANA op Azure (grote instanties)-eenheden.

Dit document is een van verschillende documenten die betrekking hebben op SAP HANA op Azure (grote instanties). Dit document worden de basisarchitectuur, verantwoordelijkheden en services die worden geleverd door de oplossing. Op hoog niveau van de mogelijkheden van de oplossing worden ook besproken. Vier andere documenten dekking voor de meeste andere gebieden, zoals netwerk- en -connectiviteit, details en inzoomen op gegevens. De documentatie van SAP HANA op Azure (grote instanties) dekt niet aspecten van de SAP NetWeaver-installatie of implementatie van SAP NetWeaver in virtuele machines. SAP NetWeaver op Azure wordt behandeld in afzonderlijke documenten gevonden in dezelfde Azure-documentatie-container. 


De verschillende documenten van HANA grote instantie richtlijnen omvatten de volgende gebieden:

- [Overzicht van de SAP HANA (grote instanties) en architectuur op Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Installeren en configureren van SAP HANA (grote instanties) op Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Het oplossen van SAP HANA (grote instanties) en de controle op Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hoge beschikbaarheid in SUSE is ingesteld met behulp van de stonith instellen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [OS-back-up en herstel voor SKU's Type II](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Definities

Verschillende gemeenschappelijke definities worden veel gebruikt in de architectuur en technische Implementatiehandleiding. Houd rekening met de volgende voorwaarden en hun betekenis:

- **IaaS**: infrastructuur als een service.
- **PaaS**: Platform als een service.
- **SaaS**: Software als een service.
- **SAP-onderdeel**: een afzonderlijke SAP-toepassing, zoals ERP centraal onderdeel (ECC), Business Warehouse (BW), Manager van de oplossing of Enterprise-Portal (EP). SAP-onderdelen kunnen worden gebaseerd op de traditionele ABAP- of Java-technologieën of een toepassing NetWeaver op basis van zoals zakelijke objecten.
- **SAP-omgeving**: een of meer onderdelen van SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke-functie, zoals ontwikkeling, kwaliteit te waarborgen, training, herstel na noodgevallen of productie.
- **SAP-landschap**: verwijst naar de gehele SAP-elementen in uw IT-landschap. De SAP-landschap bevat alle productie- en niet-productieomgevingen.
- **SAP-systeem**: de combinatie van DBMS-laag en niveau van de toepassing van bijvoorbeeld een systeem voor de ontwikkeling SAP ERP, een testsysteem SAP BW en een SAP CRM productiesysteem. Azure-implementaties bieden geen ondersteuning voor het delen van deze twee lagen tussen on-premises en Azure. Een SAP-systeem is on-premises geïmplementeerd of de geïmplementeerd in Azure. U kunt de verschillende systemen van SAP-landschap dat in Azure of on-premises implementeren. U kunt bijvoorbeeld implementeren van de SAP CRM-ontwikkeling en testen van systemen in Azure, hoewel u de SAP CRM productie system on-premises implementeren. Voor SAP HANA op Azure (grote instanties), dit bedoeld dat u de SAP-toepassingslaag van SAP-systemen in virtuele machines en de bijbehorende SAP HANA-exemplaar op een eenheid in de SAP HANA op Azure (grote instanties) stempel hosten.
- **Grote instantiestempel**: een hardware-infrastructuur-stack die SAP HANA TDI-gecertificeerde en toegewezen aan het uitvoeren van SAP HANA-instanties in Azure.
- **SAP HANA op Azure (grote instanties):** officiële naam voor de aanbieding in Azure om uit te voeren van HANA-exemplaren in op SAP HANA TDI-gecertificeerde hardware die geïmplementeerd in grote instantie stempels in verschillende Azure-regio's. De gerelateerde term *HANA grote instantie* is te kort voor *SAP HANA op Azure (grote instanties)* en wordt veel gebruikt in deze handleiding technische implementatie.
- **Cross-premises**: Beschrijving van een scenario waarbij virtuele machines worden geïmplementeerd naar een Azure-abonnement dat site-naar-site, meerdere sites of Azure ExpressRoute-connectiviteit tussen on-premises datacenters en Azure. In algemene Azure-documentatie en dit soort implementaties worden ook beschreven als cross-premises scenario's. De reden voor de verbinding is om de on-premises domeinen, on-premises Azure Active Directory/OpenLDAP en on-premises DNS uitbreiden naar Azure. Het on-premises-landschap is uitgebreid naar de Azure-assets van de Azure-abonnementen. Met deze uitbreiding, kunnen de virtuele machines deel uitmaken van de on-premises domein. 

   Gebruikers van een domein van de on-premises domein kunnen toegang tot de servers en services op deze VM's (zoals DBMS-services) uitvoeren. On-premises communicatie en naamomzetting tussen VM's geïmplementeerd en virtuele machines die met Azure is mogelijk. In dit scenario is normaal dat van de manier waarop de meeste SAP-activa zijn geïmplementeerd. Zie voor meer informatie, [plannen en ontwerpen voor Azure VPN-Gateway](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [een virtueel netwerk met een site-naar-site-verbinding maken met behulp van de Azure-portal](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Tenant**: een klant die is geïmplementeerd in HANA grote instantie stempel wordt geïsoleerd in een *tenant.* Een tenant wordt geïsoleerd in de netwerken, opslag en compute-laag van andere tenants. Opslag- en rekencapaciteit eenheden toegewezen aan de verschillende tenants kunnen Zie elkaar of met elkaar communiceren op het niveau van de stempel HANA grote instantie. Een klant kan ervoor kiezen implementaties in verschillende tenants. Zelfs dan is er geen communicatie tussen tenants op het niveau van de stempel HANA grote instantie.
- **SKU-categorie**: voor HANA grote instantie, de volgende twee categorieën van SKU's worden aangeboden:
    - **Type ik klasse**: S72, S72m, S144, S144m, S192, S192m en S192xm
    - **Typ II klasse**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm en S960m


Een aantal aanvullende resources zijn beschikbaar voor het implementeren van een SAP-workload in de cloud. Als u van plan bent om uit te voeren van een implementatie van SAP HANA in Azure, moet u ervaring hebt met en op de hoogte van de beginselen van Azure IaaS en de implementatie van SAP-workloads op Azure IaaS. Voordat u doorgaat, Zie [gebruik SAP-oplossingen op Azure virtual machines](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie. 

## <a name="certification"></a>Certificering

SAP vereist naast de NetWeaver-certificering, een speciale certificering voor SAP HANA voor de ondersteuning van SAP HANA op bepaalde infrastructuur, zoals Azure IaaS.

De core SAP-notitie op NetWeaver en een certificaat van de SAP HANA mate is [SAP Opmerking #1928533-SAP-toepassingen op Azure: ondersteunde producten en typen Azure VM's](https://launchpad.support.sap.com/#/notes/1928533).

De CA-records voor SAP HANA op Azure (grote instanties)-eenheden kunnen u vinden in de [SAP HANA-gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) site. 

De SAP HANA op Azure (grote instanties)-typen, waarnaar in de SAP HANA-gecertificeerde site IaaS-platformen, biedt Microsoft en SAP-klanten de mogelijkheid tot het implementeren van grote SAP Business Suite, SAP BW, BW/4HANA, s/4 HANA of andere SAP HANA-workloads in Azure. De oplossing is gebaseerd op het stempel SAP HANA-gecertificeerde toegewezen hardware ([SAP HANA op maat center gegevensintegratie – TDI](https://scn.sap.com/docs/DOC-63140)). Als u een oplossing voor SAP HANA TDI-geconfigureerde uitvoert, worden alle SAP HANA-toepassingen (zoals SAP Business Suite op SAP HANA, SAP BW op HANA SAP S4/HANA en BW4/HANA) werkt op de hardware-infrastructuur.

Vergeleken met het uitvoeren van SAP HANA in virtuele machines, is deze oplossing een voordeel. Het biedt voor veel grotere volumes voor geheugen. Als u wilt deze oplossing inschakelt, moet u inzicht in de volgende belangrijke aspecten:

- De SAP-toepassing-laag en niet-SAP-toepassingen uitvoeren in virtuele machines die worden gehost in de gebruikelijke Azure hardware stempels.
- Klanten on-premises datacenters, infrastructuur en implementaties van toepassingen zijn verbonden met het cloud-platform via ExpressRoute (aanbevolen) of een virtueel particulier netwerk (VPN). Active Directory en DNS worden ook uitgebreid naar Azure.
- De SAP HANA-database-instantie voor HANA-werkbelasting wordt uitgevoerd op SAP HANA op Azure (grote instanties). De stempel grote instantie is verbonden met het Azure-netwerken, zodat de software die wordt uitgevoerd in virtuele machines kan communiceren met de HANA-instantie die wordt uitgevoerd in HANA grote instantie.
- De hardware van SAP HANA op Azure (grote instanties) is specifieke hardware die zijn opgegeven in een IaaS met SUSE Linux Enterprise Server of Red Hat Enterprise Linux vooraf is geïnstalleerd. Net als bij virtuele machines, verdere is updates en onderhoud van het besturingssysteem uw verantwoordelijkheid.
- Installatie van HANA of eventuele extra onderdelen die nodig zijn voor het uitvoeren van SAP HANA op eenheden van HANA grote instantie is uw verantwoordelijkheid. Alle betreffende lopende bewerkingen en het beheer van SAP HANA op Azure zijn ook uw eigen verantwoordelijkheid.
- Naast de oplossingen die hier worden beschreven, kunt u andere onderdelen installeren op uw Azure-abonnement dat verbinding met SAP HANA op Azure (grote instanties maakt). Voorbeelden zijn onderdelen waarmee communicatie met of rechtstreeks naar de SAP HANA-database, zoals Vliegende servers, RDP-servers, SAP HANA Studio, SAP-gegevensservices voor SAP BI-scenario's, of een bewakingsoplossingen.
- Als in Azure biedt HANA grote instantie ondersteuning voor hoge beschikbaarheid en herstel na noodgevallen.

## <a name="architecture"></a>Architectuur

Op hoog niveau heeft de SAP HANA op Azure (grote instanties)-oplossing het niveau van de SAP-toepassing die in virtuele machines zich bevinden. De databaselaag bevindt zich op SAP TDI-geconfigureerde hardware die zich in een grote instantie stempel in dezelfde Azure-regio die is verbonden met Azure IaaS.

> [!NOTE]
> Implementeer de SAP-toepassingslaag in dezelfde Azure-regio als de SAP DBMS-laag. Deze regel wordt goed gedocumenteerd in gepubliceerde informatie over SAP-workloads op Azure. 

De algehele architectuur van SAP HANA op Azure (grote instanties) biedt een SAP TDI-gecertificeerde hardwareconfiguratie, namelijk een niet-gevirtualiseerde, bare metal, high performance-server voor de SAP HANA-database. Het biedt ook de mogelijkheid om en flexibiliteit van Azure schalen van resources voor de SAP-toepassingslaag om te voldoen aan uw behoeften.

![Overzicht van de architectuur van SAP HANA op Azure (grote instanties)](./media/hana-overview-architecture/image1-architecture.png)

De architectuur die wordt weergegeven is onderverdeeld in drie secties:

- **Rechts**: geeft een on-premises infrastructuur met verschillende toepassingen in de gegevens zodat eindgebruikers toegang hebben tot LOB-toepassingen, zoals SAP draait. In het ideale geval deze on-premises infrastructuur naar Azure met is verbonden [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Center**: Azure IaaS bevat en in dit geval, gebruik van virtuele machines voor het hosten van SAP of andere toepassingen die gebruikmaken van SAP HANA als een DBMS-systeem. Kleinere HANA-instanties die werken met het geheugen waarmee u virtuele machines worden geïmplementeerd in virtuele machines samen met het niveau van hun toepassing. Zie voor meer informatie over virtuele machines, [virtuele machines](https://azure.microsoft.com/services/virtual-machines/).

   Netwerk van Azure-services worden gebruikt voor het groeperen van SAP-systemen, samen met andere toepassingen in virtuele netwerken. Deze virtuele netwerken verbinden met on-premises systemen evenals garantie voor SAP HANA op Azure (grote instanties).

   Zie voor SAP NetWeaver-toepassingen en databases die worden ondersteund om uit te voeren in Azure, [SAP Support Opmerking #1928533-SAP-toepassingen op Azure: ondersteunde producten en typen Azure VM's](https://launchpad.support.sap.com/#/notes/1928533). Zie voor documentatie over het implementeren van SAP-oplossingen op Azure:

  -  [SAP op virtuele machines van Windows gebruiken](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [SAP-oplossingen op Azure virtual machines gebruiken](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Links**: toont de SAP HANA TDI-gecertificeerde hardware in de Azure Large Instance-stempel. De eenheden HANA grote instantie zijn verbonden met de virtuele netwerken van uw abonnement met behulp van dezelfde technologie als de connectiviteit van on-premises naar Azure.

De Azure Large Instance-stempel zelf combineert de volgende onderdelen:

- **Computing**: Servers die zijn gebaseerd op Intel Xeon E7-8890v3 of Intel Xeon E7-8890v4 processors die de benodigde computing mogelijkheid bieden en SAP HANA is gecertificeerd.
- **Netwerk**: A unified zeer snelle, netwerk-fabric die de computing, opslag en LAN-onderdelen met elkaar verbindt.
- **Opslag**: een opslaginfrastructuur die toegankelijk is via een uniform netwerk-fabric. De specifieke opslagcapaciteit die wordt geleverd, is afhankelijk van de specifieke SAP HANA op Azure (grote instanties)-configuratie die is geïmplementeerd. Meer opslagcapaciteit is beschikbaar op een extra maandelijkse kosten.

Klanten worden binnen de multitenant-infrastructuur van de stempel grote instantie wordt geïmplementeerd als geïsoleerde tenants. Tijdens de implementatie van de tenant, kunt u een Azure-abonnement naam binnen uw Azure-inschrijving. Dit Azure-abonnement is degene die de HANA grote instantie wordt gefactureerd onder. Deze tenants hebben een 1:1-relatie met de Azure-abonnement. Voor een netwerk is het mogelijk voor toegang tot een eenheid HANA grote instantie is geïmplementeerd in een tenant in een Azure-regio van verschillende virtuele netwerken die deel uitmaken van verschillende Azure-abonnementen. Deze Azure-abonnementen moeten behoren tot dezelfde Azure-inschrijving. 

SAP HANA op Azure (grote instanties) is net als bij virtuele machines, beschikbaar in meerdere Azure-regio's. Als u wilt bieden disaster recovery-mogelijkheden, kunt u opt-in. Andere grote instantie stempels binnen één geografisch politieke regio zijn aan elkaar verbonden. Bijvoorbeeld, zijn HANA grote instantie stempels in VS West en VS-Oost verbonden via een speciaal netwerk-koppeling voor disaster recovery-replicatie. 

Net zoals u tussen verschillende VM-typen met Azure Virtual Machines kiezen kunt, kunt u kiezen uit verschillende SKU's van HANA grote instantie die speciaal voor de werkbelasting van de verschillende typen van SAP HANA geschikt zijn. SAP is van toepassing geheugen processor socket-ratio's voor verschillende workloads op basis van de generaties Intel-processor. De volgende tabel ziet u de SKU-typen die worden aangeboden.

SAP HANA op Azure (grote instanties)-service is beschikbaar in verschillende configuraties in de Azure-regio's VS West en VS-Oost, Australië-Oost, Australië-Zuidoost, West-Europa, Noord-Europa, Japan-Oost en Japan-West.

[SKU's van HANA grote instanties van SAP HANA-gecertificeerde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) lijst, zoals:

| SAP-oplossing | CPU | Geheugen | Storage | Beschikbaarheid |
| --- | --- | --- | --- | --- |
| Geoptimaliseerd voor OLAP: SAP BW, BW/4HANA<br /> of SAP HANA voor werkbelastingen van algemene OLAP-systemen | SAP HANA op Azure S72<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU-kernen en 72 CPU-threads |  768 GB |  3 TB | Beschikbaar |
| --- | SAP HANA op Azure S144<br /> -4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU-kernen en threads 144 CPU |  1,5 TB |  6 TB OPSLAGRUIMTE | Niet beschikbaar meer |
| --- | SAP HANA op Azure S192<br /> -4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en threads 192 CPU |  2.0 TB |  8 TB | Beschikbaar |
| --- | SAP HANA op Azure S384<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  4.0 TB |  16 TB | Beschikbaar |
| Geoptimaliseerd voor OLTP: SAP Business Suite<br /> op SAP HANA of S/4HANA (OLTP)<br /> algemene OLTP-systemen | SAP HANA op Azure S72m<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU-kernen en 72 CPU-threads |  1,5 TB |  6 TB OPSLAGRUIMTE | Beschikbaar |
|---| SAP HANA op Azure S144m<br /> -4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU-kernen en threads 144 CPU |  3.0 TB |  12 TB | Niet beschikbaar meer |
|---| SAP HANA op Azure S192m<br /> -4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en threads 192 CPU  |  4.0 TB |  16 TB | Beschikbaar |
|---| SAP HANA op Azure S384m<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  6.0 TB |  18 TB | Beschikbaar |
|---| SAP HANA op Azure S384xm<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  8.0 TB |  22 TB |  Beschikbaar |
|---| SAP HANA op Azure S576m<br /> tot en met 12 x Intel® Xeon® Processor E7-8890 v4<br /> 288 CPU-kernen en 576 CPU-threads |  12.0 TB |  28 TB | Beschikbaar |
|---| SAP HANA op Azure S768m<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU-kernen en threads 768 CPU |  16,0 TB |  36 TB | Beschikbaar |
|---| SAP HANA op Azure S960m<br /> -20 x Intel® Xeon® Processor E7-8890 v4<br /> 480 CPU-kernen en threads 960 CPU |  20.0 TB |  46 TB | Beschikbaar |


Onder TDIv5 voor SAP HANA kunt SAP klantspecifieke grootte en klantspecifieke projecten die tot de server-configuraties die niet zijn opgenomen leiden mogelijk gecertificeerde:

- [SAP HANA-gecertificeerde apparaten](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [SAP HANA-gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

In veel gevallen voert u deze serverconfiguraties klantspecifieke meer geheugen dan de server-eenheden met SAP gecertificeerde. Klanten hoeven bij het werken met SAP, de mogelijkheid voor het ophalen van SAP-ondersteuning en certificeren voor hun klant-specifieke grootte server-configuraties. In Azure worden de volgende HANA grote instantie standaard SKU's zijn beschikbaar en in de Microsoft prijslijst voor dergelijke TDIv5 klantspecifieke sizing-projecten.


| Oorspronkelijke SKU die kan worden <br /> uitgebreid in het geheugen | CPU | Geheugen | Storage | Beschikbaarheid |
| --- | --- | --- | --- | --- |
| S192m kan worden uitgebreid naar | SAP HANA op Azure S192xm<br /> -4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en threads 192 CPU |  6.0 TB |  16 TB | Beschikbaar |
| S384xm kan worden uitgebreid naar | SAP HANA op Azure S384xxm<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  12.0 TB |  28 TB | Beschikbaar |
| S576m kan worden uitgebreid naar | SAP HANA op Azure S576xm<br /> tot en met 12 x Intel® Xeon® Processor E7-8890 v4<br /> 288 CPU-kernen en 576 CPU-threads |  18.0 TB |  41 TB | Beschikbaar |
| S768m kan worden uitgebreid naar | SAP HANA op Azure S768xm<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU-kernen en threads 768 CPU |  24.0 TB |  56 TB | Beschikbaar |

- CPU-kernen = de som van niet-hyper-threaded CPU-kernen van de som van de processors van de server-eenheid.
- CPU-threads = de som van de compute-threads geleverd door hyper-threaded CPU-kernen van de som van de processors van de server-eenheid. De meeste eenheden zijn standaard geconfigureerd voor Hyper-Threading-technologie gebruiken.
- Op basis van de leverancier aanbevelingen S768m, zijn S768xm en S960m niet geconfigureerd voor het gebruik van Hyper-Threading voor het uitvoeren van SAP HANA.


De specifieke configuraties die is gekozen, zijn afhankelijk van de werkbelasting, CPU-resources en gewenste geheugen. Het is mogelijk dat de OLTP-werkbelasting met de SKU's die zijn geoptimaliseerd voor de OLAP-werkbelasting. 

De basis voor de aanbiedingen, met uitzondering van eenheden voor grootte van de klant-specifieke projecten, hardware zijn SAP HANA TDI-gecertificeerde. Twee verschillende soorten hardware deelt de SKU's in:

- S72, S72m S144, S144m, S192, S192m en S192xm, die worden aangeduid als "Type ik klasse" van SKU's.
- S384, S384m, S384xm, S384xxm, S576m, S576xm S768m, S768xm en S960m, die worden aangeduid als de "Type II-klasse" van SKU's.

Een volledige HANA grote instantie stempel is niet uitsluitend voor één klant toegewezen&#39;s gebruiken. Dit geldt voor de rekken van reken- en resources die zijn verbonden via een netwerk-fabric ook geïmplementeerd in Azure. HANA grote instantie infrastructuur, zoals Azure, verschillende klanten implementeert &quot;tenants&quot; die zijn geïsoleerd van elkaar in de volgende drie niveaus:

- **Netwerk**: isolatie door middel van virtuele netwerken in de stempel HANA grote instantie.
- **Opslag**: isolatie door middel van opslag virtuele machines met opslagvolumes die zijn toegewezen en isoleren opslagvolumes tussen tenants.
- **COMPUTE**: toewijzing van server-eenheden toegewezen aan één tenant. Geen vaste of zachte partitioneren van eenheden van de server. Er is geen delen van één server of de host eenheid tussen tenants. 

De implementaties van HANA grote instantie eenheden tussen verschillende tenants zijn niet zichtbaar zijn voor elkaar. HANA grote instantie eenheden die zijn geïmplementeerd in verschillende tenants kunnen niet communiceren rechtstreeks met elkaar op het niveau van de stempel HANA grote instantie. Alleen HANA grote instantie eenheden binnen één tenant kunnen met elkaar communiceren op het niveau van de stempel HANA grote instantie.

Een geïmplementeerde tenant in de stempel grote instantie wordt toegewezen aan één Azure-abonnement voor factureringsdoeleinden. Voor een netwerk, kan deze worden geopend van virtuele netwerken van andere Azure-abonnementen binnen dezelfde Azure-inschrijving. Als u met een andere Azure-abonnement in dezelfde Azure-regio implementeert, kunt ook u vragen om een gescheiden HANA grote instantie-tenant.

Er zijn belangrijke verschillen tussen het uitvoeren van SAP HANA op HANA grote instantie en SAP HANA uitvoeren op virtuele machines die zijn geïmplementeerd in Azure:

- Er is geen virtualisatielaag voor SAP HANA op Azure (grote instanties). Krijgt u de prestaties van de onderliggende hardware van bare-metal.
- In tegenstelling tot Azure, is de SAP HANA op Azure (grote instanties)-server worden toegewezen aan een specifieke klant. Er is geen mogelijkheid is dat een eenheid van de server of de host is hard of zachte gepartitioneerd. Als gevolg hiervan wordt een eenheid HANA grote instantie gebruikt als geheel naar een tenant en die aan u toegewezen. Opnieuw opstarten of afsluiten van de server leiden niet automatisch tot het besturingssysteem en de SAP HANA wordt geïmplementeerd op een andere server. (Voor het Type ik klasse SKU's, de enige uitzondering hierop is als een server problemen detecteert en opnieuw implementeren moet worden uitgevoerd op een andere server.)
- In tegenstelling tot Azure, waarbij host processortypen zijn geselecteerd voor de beste prijs-prestatieverhouding-verhouding, zijn de processortypen voor SAP HANA op Azure (grote instanties) hebt gekozen, de hoogste prestaties van de Intel E7v3 en E7v4 processor-regel.


### <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Meerdere exemplaren van de SAP HANA uitvoeren op één HANA grote instantie-eenheid
Het is mogelijk om meer dan één actieve SAP HANA-instantie op HANA grote instantie eenheden te hosten. Een dergelijke configuratie vereist om de mogelijkheden van opslagmomentopnamen en herstel na noodgevallen, een volume per exemplaar instellen. Op dit moment kunnen HANA grote instantie eenheden als volgt worden onderverdeeld:

- **S72, S72m, S144, S192**: In stappen van 256 GB, 256 GB wordt de kleinste vanaf eenheid. Verschillende stappen zoals 256 GB en 512 GB kunnen worden gecombineerd met het maximum van het geheugen van de eenheid.
- **S144m en S192m**: In stappen van 256 GB, 512 GB wordt de kleinste eenheid. Verschillende stappen zoals 512 GB en biedt 768 GB kunnen worden gecombineerd met het maximum van het geheugen van de eenheid.
- **Typ II klasse**: In stappen van 512 GB, waarbij de kleinste vanaf eenheid van 2 TB. Verschillende stappen zoals 512 GB, 1 TB en 1,5 TB kunnen worden gecombineerd met het maximum van het geheugen van de eenheid.

Enkele voorbeelden van het uitvoeren van meerdere SAP HANA-instanties ziet er als volgt uit.

| SKU | Geheugengrootte | Opslaggrootte | Grootten met meerdere databases |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA-instantie<br /> of 1 x 512 GB exemplaar + 1 x 256 GB-exemplaar<br /> of exemplaren van 3 x 256 GB | 
| S72m | 1,5 TB | 6 TB OPSLAGRUIMTE | 3x512GB HANA-instanties<br />of 1 x 512 GB exemplaar + 1 x 1 TB exemplaar<br />of 6 x 256 GB-exemplaren<br />of het exemplaar 1x1.5 TB | 
| S192m | 4 TB | 16 TB | 8 x 512 GB-instanties<br />of exemplaren van 4 x 1 TB<br />of exemplaren van 4 x 512 GB + 2 x 1 TB exemplaren<br />of exemplaren van 4 x 768 GB + 2 x 512 GB-instanties<br />of het exemplaar van 1 x 4 TB |
| S384xm | 8 TB | 22 TB | exemplaren van 4 x 2 TB<br />of 2 x 4 TB-exemplaren<br />of exemplaren van 2 x 3 TB + 1 x 2 TB exemplaren<br />of instanties 2x2.5 TB + 1 x 3 TB exemplaren<br />of het exemplaar van 1 x 8 TB |


Er zijn ook andere verschillen. 

### <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA-gegevens in lagen en extensie-knooppunten gebruiken
SAP biedt ondersteuning voor een gegevensmodel warmtemeting voor SAP BW van verschillende versies van SAP NetWeaver en SAP BW/4HANA. Raadpleeg het document SAP voor meer informatie over het gegevensmodel voor cloudlagen [SAP BW/4HANA en SAP BW op HANA met SAP HANA-extensie knooppunten](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Met HANA grote instantie kunt u de optie-1-configuratie van SAP HANA-extensie knooppunten, zoals wordt beschreven in de veelgestelde vragen en SAP blog-documenten. Optie 2-configuraties kunnen worden ingesteld met de volgende HANA grote instantie SKU's: S72m, S192, S192m, S384 en S384m. 

Wanneer u de documentatie bekijkt, het voordeel mogelijk niet meer zichtbaar onmiddellijk. Maar wanneer u de richtlijnen van de grootte SAP bekijkt, ziet u een voordeel met behulp van de optie-1 en de optie-2 SAP HANA-extensie-knooppunten. Hier volgen enkele voorbeelden:

- Richtlijnen voor SAP HANA-grootte is meestal de dubbele hoeveelheid gegevensvolume als geheugen vereist. Wanneer u uw SAP HANA-instantie met de dynamische gegevens uitvoert, hebt u slechts 50% of minder van het geheugen gevuld met gegevens. De rest van het geheugen worden in het ideale geval bewaard voor SAP HANA zijn werk doet.
- Dat betekent dat in een HANA grote instantie S192-eenheid met 2 TB aan geheugen, met een SAP BW-database, hoeft u slechts 1 TB als gegevensvolume.
- Als u een extra knooppunt van SAP HANA-extensie van de optie-1, ook een S192 HANA grote instantie SKU, dit biedt u een extra capaciteit van 2 TB voor gegevensvolume. In de optie-2-configuratie krijgt u een extra 4 TB voor warme gegevensvolume. Vergeleken met het knooppunt ' hot ', kan de volledige geheugencapaciteit van het knooppunt 'warme' extensie worden gebruikt voor het opslaan van gegevens voor de optie-1. Dubbele het geheugen kan worden gebruikt voor gegevensvolume in optie 2 SAP HANA-extensieconfiguratie in het knooppunt.
- U uiteindelijk een capaciteit van 3 TB voor uw gegevens en een ' hot ' naar warm-verhouding van 1:2 voor optie 1. U hebt 5 TB aan gegevens en een 1:4-verhouding met de configuratie voor de uitbreiding van de optie-2-knooppunt.

Hoe hoger het gegevensvolume in vergelijking met het geheugen, hoe hoger de kans op zijn dat de warme gegevens die u daarvoor is opgeslagen op de schijfopslag.


## <a name="operations-model-and-responsibilities"></a>Operationeel model en -verantwoordelijkheden

De service die wordt geleverd met SAP HANA op Azure (grote instanties) is afgestemd op Azure IaaS-services. U krijgt een exemplaar van een HANA grote instantie met een geïnstalleerd besturingssysteem dat is geoptimaliseerd voor SAP HANA. Met Azure IaaS VM's, de meeste van de taken van de beperking van het besturingssysteem, extra software te installeren, HANA installeren, die het besturingssysteem en HANA en bijwerken van het besturingssysteem en HANA is uw verantwoordelijkheid. Microsoft biedt geen verplichten om updates van het besturingssysteem of updates voor HANA op u.

![De verantwoordelijkheden van SAP HANA op Azure (grote instanties)](./media/hana-overview-architecture/image2-responsibilities.png)

In het diagram wordt weergegeven, is SAP HANA op Azure (grote instanties) een multitenant IaaS bieden. Voor het overgrote deel is de verdeling van de verantwoordelijkheid van de grens van de OS-infrastructuur. Microsoft is verantwoordelijk voor alle aspecten van de service onder de lijn van het besturingssysteem. U bent verantwoordelijk voor alle aspecten van de service boven de regel. Het besturingssysteem is uw verantwoordelijkheid. U kunt echter ook doorgaan met de meest recente on-premises-methoden die u gebruikmaken van mogelijk voor naleving, beveiliging, beheer van toepassingen, basis en OS-beheer. De systemen worden weergegeven alsof ze zich in uw netwerk in alle opzichten.

Deze service is geoptimaliseerd voor SAP HANA, zodat er gebieden waar u nodig hebt om te werken met Microsoft voor het gebruik van de mogelijkheden van de onderliggende infrastructuur voor de beste resultaten.

De volgende lijst vindt u meer details over op elk van de lagen en uw verantwoordelijkheden:

**Netwerken**: de interne netwerken voor het uitvoeren van SAP HANA grote instantie-stempel. Uw eigen verantwoordelijkheid omvat toegang tot opslag, verbinding tussen de exemplaren (voor scale-out en andere functies), verbinding met het landschap van en verbinding met Azure waar de SAP-toepassingslaag wordt gehost in VM's. Dit omvat ook WAN-verbinding tussen Azure-datacenters voor disaster recovery doeleinden-replicatie. Alle netwerken worden gepartitioneerd op basis van de tenant en kwaliteit van de service toegepast hebben.

**Opslag**: de gevirtualiseerde gepartitioneerd opslag voor alle volumes die nodig zijn voor de SAP HANA-servers, evenals voor momentopnamen. 

**Servers**: de toegewezen fysieke servers om uit te voeren van de SAP HANA-databases die zijn toegewezen aan tenants. De servers van het Type ik klasse van SKU's zijn hardware geabstraheerd. Configuratie van de server is met de volgende typen servers, die worden verzameld en onderhouden in profielen die kunnen worden verplaatst van een fysieke hardware voor een andere fysieke hardware. Dergelijke een (handmatig) verplaatsen van een profiel door bewerkingen kan een en ander Azure-service herstel worden vergeleken. De servers van de SKU's Type II-klasse bieden niet een dergelijke mogelijkheid.

**SDDC**: de software die wordt gebruikt om gegevens te beheren als entiteiten software gedefinieerde datacenters. Hierdoor kan Microsoft tot resources van schaal, beschikbaarheid en prestaties van toepassingen.

**Besturingssysteem**: het besturingssysteem die u kiest (SUSE Linux of Red Hat Linux) die wordt uitgevoerd op de servers. De installatiekopieën van het besturingssysteem die u worden geleverd met zijn opgegeven door de leverancier van de afzonderlijke Linux naar Microsoft voor het uitvoeren van SAP HANA. U moet een abonnement met de Linux-leverancier voor de specifieke geoptimaliseerd voor SAP HANA-installatiekopie hebben. U bent verantwoordelijk voor het registreren van de afbeeldingen met de leverancier van het besturingssysteem. 

Vanaf het moment van oproepdienstoverdracht door Microsoft bent u verantwoordelijk voor alle verdere patchen van de Linux-besturingssysteem. Deze patches bevat aanvullende pakketten die mogelijk noodzakelijk voor een geslaagde installatie van de SAP HANA en die niet zijn opgenomen door de specifieke Linux-leverancier in hun SAP HANA geoptimaliseerd voor installatiekopieën van het besturingssysteem. (Zie de documentatie voor installatie van HANA en SAP-opmerkingen van SAP voor meer informatie.) 

U bent verantwoordelijk voor het patchen van het besturingssysteem door de storing of optimalisatie van het besturingssysteem en de stuurprogramma's relatief ten opzichte van de specifieke serverhardware. Ook bent u verantwoordelijk voor het beveiligings- of functioneel patchen van het besturingssysteem. 

Uw eigen verantwoordelijkheid bevat ook bewaken en plannen van capaciteit van:

- Gebruik van CPU-resources.
- Het geheugengebruik.
- Volumes op schijven die betrekking hebben op vrije ruimte, IOPS en latentie.
- Volume netwerkverkeer tussen HANA grote instantie en de SAP-toepassingslaag.

De onderliggende infrastructuur van HANA grote instantie biedt functionaliteit voor back-up en herstel van het volume met het besturingssysteem. Met deze functionaliteit is ook uw eigen verantwoordelijkheid.

**Middleware**: SAP HANA-exemplaar, voornamelijk. Bent u verantwoordelijk voor beheer, bewerkingen en bewaking. De opgegeven functionaliteit kunt u storage-momentopnamen gebruiken voor back-up en herstel en disaster recovery-doeleinden. Deze mogelijkheden worden geleverd door de infrastructuur. Uw verantwoordelijkheden ook het ontwerpen van hoge beschikbaarheid of herstel na noodgeval met deze mogelijkheden gebruik te maken van deze en te bewaken om te bepalen of storage-momentopnamen is uitgevoerd.

**Gegevens**: uw gegevens die worden beheerd door SAP HANA en andere gegevens zoals back-ups van bestanden die zijn opgeslagen op volumes of het bestand deelt. Uw verantwoordelijkheden zijn onder andere vrije schijfruimte controleren en beheren van de inhoud op de volumes. Ook bent u verantwoordelijk voor het bewaken van de voltooiing van uitvoering van back-ups van volumes op schijven en storage-momentopnamen. Geslaagde uitvoering van de replicatie van gegevens naar disaster recovery sites is de verantwoordelijkheid van Microsoft.

**Toepassingen:** de toepassingsexemplaren SAP of, in het geval van niet-SAP-toepassingen, het niveau van de toepassing van deze toepassingen. Uw verantwoordelijkheden zijn onder andere implementatie, beheer, bewerkingen en bewaking van deze toepassingen. U bent verantwoordelijk voor het plannen van capaciteit van CPU-verbruik van resources, geheugenverbruik, gebruik van Azure-opslag en netwerkbandbreedte in virtuele netwerken. Ook bent u verantwoordelijk voor de capaciteitsplanning voor bronverbruik van virtuele netwerken met SAP HANA op Azure (grote instanties).

**WAN's**: de verbindingen die u tot stand van on-premises naar Azure-implementaties voor werkbelastingen brengen. Alle klanten met HANA grote instantie Azure ExpressRoute gebruiken voor verbindingen. Deze verbinding maakt geen deel uit van de SAP HANA op Azure (grote instanties)-oplossing. U bent verantwoordelijk voor het instellen van deze verbinding.

**Archief**: U misschien liever archiveren kopieën van gegevens met behulp van uw eigen methoden in de storage-accounts. Archiveren is vereist, beheer, naleving, kosten en bewerkingen. U bent verantwoordelijk voor het genereren archive-exemplaren en back-ups op Azure en opslaat in een compatibele manier.

Zie de [SLA voor SAP HANA op Azure (grote instanties)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Grootte aanpassen

Groottes voor HANA grote instantie is niet anders dan in het algemeen voor HANA formaat. Voor bestaande en systemen die u verplaatsen van andere RDBMS met HANA wilt, SAP biedt een aantal rapporten die worden uitgevoerd op uw bestaande SAP-systemen geïmplementeerd. Als de database is verplaatst naar HANA, worden deze rapporten Controleer de gegevens en berekenen geheugenvereisten voor de HANA-instantie. Lees de volgende SAP-opmerkingen voor meer informatie over het uitvoeren van deze rapporten en hun meest recente patches of versies te verkrijgen:

- [SAP-notitie #1793345 - grootte voor SAP-Suite op HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP-notitie #1872170 - Suite op HANA en s/4 HANA sizing-rapport](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-notitie #2121330 - Veelgestelde vragen over: SAP BW op HANA formaat van rapport](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP-notitie #1736976 - rapport van de grootte voor BW op HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP-notitie #2296290 - rapport nieuwe grootte voor BW op HANA](https://launchpad.support.sap.com/#/notes/2296290)

Snel SAP-Sizer is voor implementaties van de voorafgaande beperkingen, beschikbaar voor het berekenen van geheugenvereisten van de implementatie van SAP-software boven op HANA.

Geheugenvereisten voor HANA verhogen naarmate het gegevensvolume toeneemt. Houd rekening met uw huidige geheugengebruik om te voorspellen wat er gebeurt in de toekomst zijn. Op basis van geheugenvereisten, kunt klikt u vervolgens u toewijzen uw vraag in een van de HANA grote instantie-SKU's.

## <a name="requirements"></a>Vereisten

Deze lijst ophaalprotocol vereisten voor het uitvoeren van SAP HANA op Azure (grotere instanties).

**Microsoft Azure**

- Een Azure-abonnement dat kan worden gekoppeld aan SAP HANA op Azure (grote instanties).
- Microsoft Premier support-contract. Zie voor specifieke informatie met betrekking tot het uitvoeren van SAP in Azure, [SAP Support Opmerking #2015553 – SAP op Microsoft Azure: vereisten voor ondersteuning](https://launchpad.support.sap.com/#/notes/2015553). Als u HANA grote instantie eenheden met 384 en meer CPU's gebruikt, moet u ook om uit te breiden de Premier-ondersteuningscontract om op te nemen van Azure Rapid Response.
- Status van de HANA grote instantie SKU's u moet na het uitvoeren van een oefening schaling met SAP.

**Verbinding met het netwerk**

- ExpressRoute tussen on-premises naar Azure: als u wilt verbinding maken met uw on-premises Datacenter naar Azure, zorg ervoor dat u de volgorde van ten minste 1 Gbps-verbinding via uw Internetprovider. 

**Besturingssysteem**

- Licenties voor SUSE Linux Enterprise Server 12 voor SAP-toepassingen.

   > [!NOTE] 
   > Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd bij SUSE. Het is niet verbonden met een exemplaar van het beheerprogramma voor abonnement.

- SUSE Linux abonnement Management Tool geïmplementeerd in Azure op een virtuele machine. Dit hulpprogramma biedt de mogelijkheid voor SAP HANA op Azure (grote instanties) worden geregistreerd en respectievelijk bijgewerkt door SUSE. (Er is geen toegang tot internet binnen het datacenter HANA grote instantie.) 
- Licenties voor Red Hat Enterprise Linux 6.7 of 7.x voor SAP HANA.

   > [!NOTE]
   > Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd met Red Hat. Het is niet verbonden met een Doelabonnementbeheerder van Red Hat-exemplaar.

- Red Hat abonnement Manager is geïmplementeerd in Azure op een virtuele machine. De Red Hat abonnement Manager biedt de mogelijkheid voor SAP HANA op Azure (grote instanties) worden geregistreerd en respectievelijk bijgewerkt door Red Hat. (Er is geen directe toegang tot internet vanaf binnen de tenant die is geïmplementeerd op de Azure Large Instance-stempel.)
- SAP vereist dat u hebt een ondersteuning contract bij uw Linux-provider. Deze vereiste wordt niet verwijderd door de oplossing van HANA grote instantie of het feit dat u Linux in Azure worden uitgevoerd. In tegenstelling tot met enkele van de Linux Azure-galerie met installatiekopieën, de kosten van de service is *niet* deel uitmaakt van de aanbieding van de oplossing van HANA grote instantie. Het is uw verantwoordelijkheid om te voldoen aan de vereisten van SAP met betrekking tot support-contract met de distributor Linux. 
   - SUSE Linux, zoekt u de vereisten van support-contract in [SAP Opmerking #1984787 - SUSE Linux Enterprise Server 12: opmerkingen bij de installatie](https://launchpad.support.sap.com/#/notes/1984787) en [SAP Opmerking #1056161 - SUSE prioriteitsondersteuning voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/1056161).
   - Voor Red Hat Linux moet u het juiste abonnement niveaus die zijn ondersteuning en service-updates voor de besturingssystemen van HANA grote instantie. Red Hat beveelt aan het Red Hat Enterprise Linux voor [SAP-oplossingen] (https://access.redhat.com/solutions/3082481 abonnement. 

Zie voor de ondersteuningsmatrix van de verschillende versies van de SAP HANA met de verschillende versies van Linux, [SAP Opmerking #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Raadpleeg voor de compatibiliteitsmatrix van het besturingssysteem en firmware-/ stuurprogrammaversies HLI [bijwerken van het besturingssysteem voor HLI](os-upgrade-hana-large-instance.md).


> [!IMPORTANT] 
> Voor Type II eenheden alleen de SLES 12 SP2 OS-versie op dit moment ondersteund. 


**Database**

- De licenties en software-installatie van onderdelen voor SAP HANA (platform- of enterprise edition).

**Toepassingen**

- Licenties en software-installatie van onderdelen voor SAP-toepassingen die verbinding met SAP HANA en gerelateerde SAP maken ondersteuning voor opdrachten.
- Licenties en software-installatie van onderdelen voor niet-SAP-toepassingen gebruikt met betrekking tot SAP HANA op Azure (grote instanties)-omgevingen en ondersteuningscontracten die betrekking hebben.

**Vaardigheden**

- Ervaring met en kennis van Azure IaaS en de bijbehorende onderdelen.
- Ervaring met en kennis van hoe u een SAP-workloads in Azure implementeert.
- Installatie van SAP HANA-gecertificeerde personeel.
- SAP-architect vaardigheden voor het ontwerpen van hoge beschikbaarheid en herstel na noodgevallen om SAP HANA.

**SAP**

- Wordt ervan uitgegaan dat u een SAP-klant bent en een hebben contract met SAP.
- Met name voor implementaties van het Type II-klasse van HANA grote instantie SKU's, neem contact op met SAP op versies van SAP HANA en de uiteindelijke configuraties op grote schaal omhoog hardware.


## <a name="storage"></a>Storage

De opslagindeling voor SAP HANA op Azure (grote instanties) is geconfigureerd door SAP HANA op het klassieke implementatiemodel via SAP aanbevolen richtlijnen. De richtlijnen worden beschreven in de [opslagvereisten voor SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) technisch document.

De HANA grote instantie van het Type ik klasse wordt geleverd met vier keer het geheugen volume als opslagvolume. De opslag is niet voor de klasse Type II van HANA grote instantie eenheden, vier keer meer. De eenheden worden geleverd met een volume dat is bedoeld voor het opslaan van HANA transactielogboekback-ups. Zie voor meer informatie, [installeren en configureren van SAP HANA (grote instanties) op Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie de volgende tabel in termen van Opslagtoewijzing. De tabel bevat de ruwe capaciteit voor de verschillende volumes die zijn opgegeven met de verschillende HANA grote instantie-eenheden.

| HANA grote instantie SKU | Hana/gegevens | Hana/log | Hana/gedeeld | Hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S192 | 4.608 GB | 1.024 GB | 1536 GB | 1.024 GB |
| S192m | 11,520 GB | 1536 GB | 1.792 GB | 1536 GB |
| S192xm |  11,520 GB |  1536 GB |  1.792 GB |  1536 GB |
| S384 | 11,520 GB | 1536 GB | 1.792 GB | 1536 GB |
| S384m | 12.000 GB | 2.050 GB | 2.050 GB | 2040 GB |
| S384xm | 16.000 GB | 2.050 GB | 2.050 GB | 2040 GB |
| S384xxm |  20.000 GB | 3100 GB | 2.050 GB | 3100 GB |
| S576m | 20.000 GB | 3100 GB | 2.050 GB | 3100 GB |
| S576xm | 31.744 GB | 4096 GB | 2.048 GB | 4096 GB |
| S768m | 28.000 GB | 3100 GB | 2.050 GB | 3100 GB |
| S768xm | 40.960 GB | 6.144 GB | 4096 GB | 6.144 GB |
| S960m | 36.000 GB | 4,100 GB | 2.050 GB | 4,100 GB |


Werkelijke geïmplementeerde volumes kunnen verschillen op basis van de implementatie en het hulpprogramma dat wordt gebruikt om de volumes groter weer te geven.

Als u een HANA grote instantie SKU onderverdelen, enkele voorbeelden van mogelijke deling onderdelen als volgt uitzien:

| Geheugenpartitie in GB | Hana/gegevens | Hana/log | Hana/gedeeld | Hana/log/back-up |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1.792 GB | 640 GB | 1.024 GB | 640 GB |
| 1536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Deze grootten zijn ruwe volume getallen die kan verschillen enigszins op basis van de implementatie en de hulpprogramma's gebruikt om te kijken naar de volumes. Er zijn ook de grootte van andere partities, zoals 2,5 TB. Deze opslagruimten worden berekend met een formule die vergelijkbaar is met de gebruikt voor de vorige partities. De term 'partities' betekent niet dat het besturingssysteem, geheugen of CPU-resources worden op geen enkele manier gepartitioneerd. Opslag-partities voor de verschillende HANA-instanties die u mogelijk wilt implementeren op één HANA grote instantie eenheid geeft u aan. 

U kunt meer opslag nodig hebt. U kunt opslag toevoegen door het aanschaffen van extra opslagruimte in eenheden van 1 TB. Deze extra opslag kan worden toegevoegd als een extra volume. Er kan ook worden gebruikt om uit te breiden van een of meer van de bestaande volumes. Het is niet mogelijk om te verkleinen van de grootte van de volumes zoals oorspronkelijk geïmplementeerd en voornamelijk door de vorige tabellen worden beschreven. Is het ook niet mogelijk om te wijzigen van de namen van de volumes of koppelen van namen. De opslagvolumes die eerder is beschreven, zijn gekoppeld aan de eenheden HANA grote instantie als NFS4 volumes.

U kunt storage-momentopnamen gebruiken voor back-up en herstel en disaster recovery-doeleinden. Zie voor meer informatie, [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Raadpleeg [HLI scenario's ondersteund](hana-supported-scenario.md) voor lay-out opslaggegevens voor uw scenario.

### <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest
De opslag die wordt gebruikt voor HANA grote instantie kunt een transparante versleuteling van de gegevens zoals deze wordt opgeslagen op de schijven. Als een eenheid HANA grote instantie is geïmplementeerd, kunt u dit type versleuteling inschakelen. U kunt ook wijzigen met versleutelde volumes na de implementatie plaatsvindt. De overstap van niet-versleutelde versleutelde volumes is transparant en vereist geen downtime. 

Met het Type ik klasse van SKU's, het volume met de LUN wordt opgeslagen op opstarten is versleuteld. Voor het Type II-klasse van SKU's van HANA grote instantie moet u voor het versleutelen van het bestand Boot.ini LUN met besturingssysteem-methoden. Voor meer informatie contact op met het Microsoft-Service Management-team.


## <a name="networking"></a>Netwerken

De architectuur van Azure network services is een belangrijk onderdeel van de succesvolle implementatie van SAP-toepassingen op HANA grote instantie. SAP HANA op Azure (grote instanties) implementaties hebben meestal een grotere SAP-landschap met diverse verschillende SAP-oplossingen met verschillende grootten van databases, resourceverbruik CPU en geheugengebruik. Is het waarschijnlijk dat niet al deze SAP-systemen zijn gebaseerd op SAP HANA. Uw SAP-landschap dat is waarschijnlijk een hybride die gebruikmaakt van:

- SAP-systemen on-premises geïmplementeerd. Vanwege de grootte, niet kunnen deze systemen op dit moment worden gehost in Azure. Een voorbeeld is een productie SAP ERP-systeem dat wordt uitgevoerd op SQL Server (als de database) en meer CPU of geheugen resources dan virtuele machines nodig.
- SAP HANA op basis van SAP-systemen on-premises geïmplementeerd.
- Geïmplementeerde SAP-systemen in virtuele machines. Deze systemen kunnen ontwikkelen, testen, sandbox of productie-exemplaren voor het gebruik van de SAP NetWeaver gebaseerde toepassingen die in Azure (op VM's), op basis van verbruik en het geheugen vraag naar resources kunnen implementeren. Deze systemen kunnen ook worden gebaseerd op, zoals SQL Server-databases. Zie voor meer informatie, [SAP Support Opmerking #1928533-SAP-toepassingen op Azure: ondersteunde producten en typen Azure VM's](https://launchpad.support.sap.com/#/notes/1928533/E). En deze systemen kunnen worden gebaseerd op, zoals SAP HANA-databases. Zie voor meer informatie, [SAP HANA-gecertificeerde IaaS-platformen](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
- SAP-toepassingsservers in Azure (op VM's) die gebruikmaken van SAP HANA op Azure (grote instanties) in Azure Large Instance stempels geïmplementeerd.

Een hybride SAP-landschap met vier of meer verschillende implementatiescenario's is normaal. Er zijn ook veel klantcases van complete SAP-landschappen die worden uitgevoerd in Azure. Als VM's nog krachtigere worden, verhoogt het aantal klanten die alle hun SAP-oplossingen op Azure worden verplaatst.

Azure-netwerken in de context van SAP-systemen die zijn geïmplementeerd in Azure is eenvoudig. Deze gebaseerd op de volgende principes:

- Virtuele netwerken van Azure moeten worden verbonden met het ExpressRoute-circuit dat is verbonden met een on-premises netwerk.
- Een ExpressRoute-circuit die on-premises doorgaans verbinding met Azure, moet een bandbreedte van 1 Gbps of hoger hebben. Deze minimale bandbreedte kunt voldoende bandbreedte voor de overdracht van gegevens tussen on-premises systemen en systemen die worden uitgevoerd op virtuele machines. Daarnaast kunt u voldoende bandbreedte voor de verbinding met Azure-systemen van on-premises gebruikers.
- Alle SAP-systemen in Azure moeten worden ingesteld in virtuele netwerken met elkaar communiceren.
- Active Directory en DNS-on-premises gehost worden uitgebreid naar Azure via ExpressRoute van on-premises.


> [!NOTE] 
> Uit een oogpunt van de facturering kan slechts één Azure-abonnement zijn gekoppeld aan slechts één tenant in een grote instantie stempel in een specifieke Azure-regio. Één grote instantie stempel tenant kan daarentegen worden gekoppeld aan slechts één Azure-abonnement. Deze vereiste is consistent met andere factureerbare objecten in Azure.

Als SAP HANA op Azure (grote instanties) is geïmplementeerd in meerdere verschillende Azure-regio's, wordt een afzonderlijke tenant wordt geïmplementeerd in de stempel grote instantie. U kunt beide onder hetzelfde Azure-abonnement uitvoeren, zolang deze exemplaren onderdeel van de SAP-landschap dat dezelfde zijn. 

> [!IMPORTANT] 
> Alleen de Azure Resource Manager-implementatie wordt ondersteund met SAP HANA op Azure (grote instanties).

 

### <a name="additional-virtual-network-information"></a>Informatie over aanvullende virtuele netwerken

Voor een virtueel netwerk verbinding met ExpressRoute, moet een Azure-gateway worden gemaakt. Zie voor meer informatie, [virtuele netwerkgateways voor ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 

Een Azure-gateway kan worden gebruikt met ExpressRoute naar een infrastructuur buiten Azure of naar een Azure Large Instance-stempel. Een Azure-gateway kan ook worden gebruikt verbinding maken tussen virtuele netwerken. Zie voor meer informatie, [een netwerk-netwerkverbinding voor Resource Manager configureren met behulp van PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). U kunt de Azure-gateway tot een maximum van vier andere ExpressRoute-verbindingen kunt verbinden, zolang deze verbindingen afkomstig van andere Microsoft enterprise-randrouters zijn. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> De doorvoer vindt u een Azure-gateway verschilt voor beide. Zie voor meer informatie, [over VPN-Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). De maximale doorvoer die u met een virtuele netwerkgateway bereiken kunt is 10 Gbps met behulp van een ExpressRoute-verbinding. Kopiëren van bestanden tussen een virtuele machine die zich in een virtueel netwerk bevinden en een on-premises (als een stroom één exemplaar) de volledige doorvoer van de andere gateway-SKU's niet bereiken. Als u wilt gebruikmaken van de volledige bandbreedte van de virtuele netwerkgateway, gebruik van meerdere streams. Of u moet verschillende bestanden in parallelle stromen van één enkel bestand kopiëren.


### <a name="networking-architecture-for-hana-large-instance"></a>Architectuur van de netwerken voor HANA grote instantie
De VPN-architectuur voor HANA grote instantie kan worden onderverdeeld in vier verschillende onderdelen:

- On-premises netwerken en ExpressRoute-verbinding naar Azure. Dit onderdeel is van de klant domein en is verbonden met Azure via ExpressRoute. Zie de rechtsonder in de volgende afbeelding.
- Netwerk van Azure-services, zoals eerder besproken, met virtuele netwerken, die opnieuw gateways zijn. Dit onderdeel is een gebied waar u nodig hebt om te vinden van de juiste ontwerpen voor uw toepassingsvereisten-, beveiligings- en nalevingsvereisten. Of u HANA grote instantie is een ander punt rekening houden met betrekking tot het aantal virtuele netwerken en Azure gateway-SKU's waaruit u kunt kiezen. Zie de rechtsboven in de afbeelding.
- Connectiviteit van HANA grote instantie via ExpressRoute-technologie in Azure. Dit onderdeel is geïmplementeerd en uitgevoerd door Microsoft. U hoeft alleen is bieden bepaalde IP-adresbereiken na de implementatie van uw activa in HANA grote instantie verbinding maken met het ExpressRoute-circuit met de virtuele netwerken. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Netwerken in HANA grote instantie, die voornamelijk transparant is voor u.

![Virtuele netwerken die zijn verbonden met SAP HANA op Azure (grote instanties) en on-premises](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

De vereiste dat uw on-premises assets verbinding via ExpressRoute naar Azure maken moeten verandert niet omdat u gebruikmaakt van HANA grote instantie. De vereiste voor een of meerdere virtuele netwerken die worden uitgevoerd van de virtuele machines, waarop het niveau van de toepassing die verbinding met de HANA-instanties die worden gehost in eenheden van HANA grote instantie maakt wordt gehost, hebben ook wijzigen niet. 

De verschillen naar SAP-oplossingen in Azure zijn:

- De eenheden HANA grote instantie van de tenant van uw klant zijn via een andere ExpressRoute-circuit in uw virtuele netwerken verbonden. Naar een afzonderlijke load voorwaarden, delen niet de on-premises naar virtuele netwerken ExpressRoute en de koppelingen tussen virtuele netwerken en HANA grote instantie de routers dezelfde.
- De werkbelastingprofiel tussen het niveau van de SAP-toepassing en de HANA grote instantie is van een andere aard, met een groot aantal kleine aanvragen en pieken zoals (resultatensets) van SAP HANA in de toepassingslaag gegevensoverdracht.
- De architectuur van SAP-toepassing is gevoeliger voor netwerklatentie dan typische scenario's waarbij gegevens worden uitgewisseld tussen on-premises en Azure.
- De virtuele netwerkgateway heeft ten minste twee ExpressRoute-verbindingen. Beide verbindingen delen de maximale bandbreedte voor binnenkomende gegevens van de virtuele netwerkgateway.

De ervaren netwerklatentie tussen virtuele machines en HANA grote instantie eenheden kunnen hoger zijn dan een normaal traject latentie van de VM-VM-netwerk. Afhankelijk van de Azure-regio, de meetwaarden kunnen groter zijn dan de 0,7 ms traject latentie geclassificeerd als een waarde onder het gemiddelde in [SAP Opmerking #1100926 - Veelgestelde vragen over: prestaties van het netwerk](https://launchpad.support.sap.com/#/notes/1100926/E). Niettemin implementeren klanten voor productie op basis van SAP HANA SAP-toepassingen is op SAP HANA grote instantie. De klanten die geweldige verbeteringen voor rapporten geïmplementeerd door het uitvoeren van hun SAP-toepassingen op SAP HANA met behulp van HANA grote instantie eenheden. Zorg ervoor dat u uw bedrijfsprocessen grondig testen in Azure HANA grote instantie.
 
Voor deterministische netwerklatentie tussen virtuele machines en HANA grote instantie, de keuze van de virtuele netwerkgateway SKU is essentieel. In tegenstelling tot de patronen in het netwerkverkeer tussen on-premises en virtuele machines, kunt het patroon voor verkeer tussen virtuele machines en HANA grote instantie ontwikkelen met kleine maar hoge pieken van aanvragen en gegevens volumes moet worden verzonden. Voor het afhandelen van dergelijke bursts goed, raden we het gebruik van de gateway-SKU UltraPerformance. Voor het Type II-klasse van HANA grote instantie SKU's is het gebruik van de gateway-SKU UltraPerformance als een virtuele netwerkgateway verplicht.

> [!IMPORTANT] 
> Gezien de algehele netwerkverkeer tussen de SAP-toepassing en de database-lagen, worden alleen de HighPerformance of UltraPerformance gateway-SKU's voor virtuele netwerken ondersteund voor het verbinden met SAP HANA op Azure (grote instanties). Voor HANA grote instantie SKU's Type II, wordt alleen de gateway-SKU UltraPerformance ondersteund als een virtuele netwerkgateway.



### <a name="single-sap-system"></a>Één SAP-systeem

De on-premises infrastructuur die eerder is getoond is verbonden via ExpressRoute in Azure. Het ExpressRoute-circuit verbinding maakt in een enterprise edge router. Zie voor meer informatie, [technisch overzicht van ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nadat de route tot stand is gebracht, verbinding wordt gemaakt in de Azure-backbone en alle Azure-regio's toegankelijk zijn.

> [!NOTE] 
> Als u wilt uitvoeren van SAP-landschappen in Azure, verbinding maken met de die het dichtst bij de Azure-regio in de SAP-landschap enterprise edge router. Azure Large Instance stempels zijn verbonden via een speciale enterprise edge router-apparaten voor minimale netwerklatentie tussen virtuele machines in Azure IaaS- en grote instantie stempels.

De virtuele netwerkgateway voor de virtuele machines die als exemplaren van SAP-toepassing host is verbonden met het ExpressRoute-circuit. Hetzelfde virtuele netwerk is verbonden met een speciaal voor de verbinding te maken met een grote instantie stempels afzonderlijke enterprise edge router.

Dit systeem is een eenvoudig voorbeeld van een enkele SAP-systeem. Het niveau van de SAP-toepassing wordt gehost in Azure. De SAP HANA-database wordt uitgevoerd op SAP HANA op Azure (grote instanties). De veronderstelling is dat een knelpunt niet staan voor de bandbreedte van het virtuele netwerk-gateway van 2 Gbps of 10 Gbps-doorvoer.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Meerdere SAP-systemen of grote SAP-systemen

Als er meerdere SAP-systemen of grote SAP-systemen worden geïmplementeerd om te verbinden met SAP HANA op Azure (grote instanties), kan een knelpunt worden in de doorvoer van de virtuele netwerkgateway. In dat geval de toepassingslagen te splitsen in meerdere virtuele netwerken. U kunt ook een speciale virtuele netwerk dat verbinding met HANA grote instantie voor het geval, zoals maakt maken:

- Het uitvoeren van back-ups rechtstreeks vanuit de HANA-exemplaren in HANA grote instantie aan een virtuele machine in Azure die als host fungeert voor NFS-shares.
- Grote back-ups of andere bestanden kopiëren van HANA grote instantie eenheden schijfruimte beheerd in Azure.

Gebruik een afzonderlijke virtuele netwerk naar host-VM's die opslag beheren. In deze rangschikking vermijdt u de gevolgen van grote bestanden of overdracht van gegevens van HANA grote instantie naar Azure op de virtuele netwerkgateway die de VM's waarop de SAP-toepassingslaag fungeert. 

Voor een beter schaalbaar netwerkarchitectuur:

- Maak gebruik van meerdere virtuele netwerken voor een enkele, grotere SAP-toepassingslaag.
- Implementeer een afzonderlijke virtuele netwerk voor elk SAP-systeem dat is geïmplementeerd, vergeleken met het combineren van deze SAP-systemen in afzonderlijke subnetten in hetzelfde virtuele netwerk.

 Een beter schaalbaar netwerkarchitectuur voor SAP HANA op Azure (grote instanties):

![SAP-toepassingslaag via meerdere virtuele netwerken implementeren](./media/hana-overview-architecture/image4-networking-architecture.png)

De afbeelding ziet u de SAP-toepassingslaag of -onderdelen, geïmplementeerd in meerdere virtuele netwerken. Deze configuratie is onvermijdelijk latentie overhead die is opgetreden tijdens de communicatie tussen de toepassingen die worden gehost in de virtuele netwerken geïntroduceerd. Standaard wordt het netwerkverkeer tussen virtuele machines zich in verschillende virtuele netwerken doorsturen via het enterprise-randrouters in deze configuratie. De manier om te optimaliseren en beperken van de latentie in de communicatie tussen twee virtuele netwerken is door de peering van virtuele netwerken binnen dezelfde regio. Deze methode werkt, zelfs als deze virtuele netwerken zich in verschillende abonnementen. Met virtueel-netwerkpeering, kunt de communicatie tussen virtuele machines in twee verschillende virtuele netwerken de Azure-netwerk-backbone gebruiken om rechtstreeks met elkaar communiceren. Latentie bevat als de virtuele machines zich in hetzelfde virtuele netwerk. Verkeer dat adressen IP-adresbereiken die zijn verbonden via de gateway van virtueel Azure-netwerk wordt doorgestuurd via de afzonderlijke virtuele netwerkgateway van het virtuele netwerk. 

Zie voor meer informatie over virtueel-netwerkpeering [peering van virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Routering in Azure

Er zijn drie netwerk routering overwegingen belangrijk voor SAP HANA op Azure (grote instanties):

* SAP HANA op Azure (grote instanties) kan alleen worden verkregen via virtuele machines en het toegewezen ExpressRoute-verbinding wordt niet rechtstreeks van on-premises. Directe toegang vanaf on-premises naar de eenheden HANA grote instantie, die door Microsoft worden geleverd, kunt u niet mogelijk is onmiddellijk. De transitieve routering beperkingen zijn vanwege de huidige Azure-netwerk-architectuur die worden gebruikt voor SAP HANA grote instantie. Bepaalde beheerclients en alle toepassingen die toegang, zoals SAP oplossing Manager directe moeten on-premises uitgevoerd kunnen geen verbinding maken met de SAP HANA-database.

* Als u HANA grote instantie eenheden die zijn geïmplementeerd in twee verschillende Azure-regio's voor herstel na noodgevallen hebt, wordt de dezelfde tijdelijke routering beperkingen gelden. Met andere woorden, IP-adressen van een eenheid HANA grote instantie in één regio (bijvoorbeeld VS West) niet doorgestuurd naar een eenheid HANA grote instantie is geïmplementeerd in een andere regio (bijvoorbeeld VS Oost). Deze beperking is afhankelijk van het gebruik van Azure-netwerk peering met andere regio's of het ExpressRoute-circuits die verbinding maken met HANA grote instantie eenheden naar virtuele netwerken cross-verbinding. Zie voor een grafische weergave in de afbeelding in de sectie 'Gebruik HANA grote instantie eenheden in meerdere regio's '. Deze beperking, die wordt geleverd met de geïmplementeerde architectuur, verbiedt het direct gebruik van HANA-Systeemreplicatie als herstel na noodgevallen.

* SAP HANA op Azure (grote instanties)-eenheden hebben een toegewezen IP-adres uit het adresbereik van server IP-adresgroep die u hebt ingediend. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dit IP-adres is toegankelijk via de Azure-abonnementen en ExpressRoute dat virtuele netwerken met HANA op Azure (grote instanties verbindt). Het IP-adres toegewezen van die server IP-adresgroepbereik dat rechtstreeks is toegewezen aan de hardware-eenheid. Er *niet* toegewezen via NAT zich voordoet, net als in de eerste implementatie van deze oplossing. 

> [!NOTE] 
> De beperking in tijdelijke routering manieren, zoals wordt beschreven in de eerste twee items, om de extra onderdelen voor het routering te gebruiken. Onderdelen die kunnen worden gebruikt om te strijden tegen de beperking kunnen zijn:

> * Een omgekeerde proxy voor het routeren van gegevens naar en uit. Bijvoorbeeld, F5 BIG-IP, NGINX met Traffic Manager, geïmplementeerd in Azure als een virtuele firewall/verkeer routeren oplossing.
> * Met behulp van [regels van IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) in een Linux-VM om in te schakelen routering tussen on-premises locaties en HANA grote instantie eenheden, of tussen HANA grote instantie eenheden in verschillende regio's.

> Let erop dat de implementatie en ondersteuning voor aangepaste oplossingen van derden met betrekking tot netwerkapparaten of IPTables wordt niet geleverd door Microsoft. Ondersteuning moet worden opgegeven door de leverancier van het onderdeel dat wordt gebruikt of de gegevensintegrator worden verstrekt. 

### <a name="internet-connectivity-of-hana-large-instance"></a>Verbinding met Internet van HANA grote instantie
HANA grote instantie heeft *niet* directe verbinding met internet hebben. Als u bijvoorbeeld mogelijk deze beperking kunt gebruiken om het registreren van de installatiekopie van het besturingssysteem rechtstreeks met de leverancier van de OS beperken. Mogelijk moet u werken met uw lokale abonnement beheerprogramma voor SUSE Linux Enterprise Server-server of de Doelabonnementbeheerder van Red Hat Enterprise Linux.

### <a name="data-encryption-between-vms-and-hana-large-instance"></a>Versleuteling van gegevens tussen VM's en HANA grote instantie
Gegevens die worden overgedragen tussen HANA grote instantie en virtuele machines is niet versleuteld. U kunt echter alleen voor de uitwisseling tussen de HANA DBMS- en JDBC/ODBC-toepassingen, versleutelen van verkeer inschakelen. Zie voor meer informatie, [deze documentatie door SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

### <a name="use-hana-large-instance-units-in-multiple-regions"></a>HANA grote instantie eenheden gebruiken in meerdere regio 's

Mogelijk hebt u redenen voor het implementeren van SAP HANA op Azure (grote instanties) in meerdere Azure-regio's dan voor herstel na noodgevallen. Misschien wilt u toegang tot HANA grote instantie van elk van de virtuele machines geïmplementeerd in de verschillende virtuele netwerken in de regio's. De IP-adressen die zijn toegewezen aan de verschillende HANA grote instantie-eenheden worden niet doorgegeven buiten de virtuele netwerken die rechtstreeks zijn verbonden via de gateway naar de exemplaren. Als gevolg hiervan is een kleine wijziging geïntroduceerd in het ontwerp van het virtuele netwerk. Vier verschillende ExpressRoute-circuits uit verschillende enterprise-randrouters kan worden verwerkt door een virtuele netwerkgateway. Elk virtueel netwerk die is verbonden met een van de grote instantie stempels kan worden verbonden met de stempel grote instantie in een andere Azure-regio.

![Virtueel netwerk is verbonden met Azure Large Instance stempels in verschillende Azure-regio 's](./media/hana-overview-architecture/image8-multiple-regions.png)

De afbeelding ziet u hoe de verschillende virtuele netwerken in beide regio's zijn verbonden met twee verschillende ExpressRoute-circuits verbinding maken met SAP HANA op Azure (grote instanties) worden gebruikt in zowel Azure-regio's. De onlangs geïntroduceerde verbindingen zijn het rechthoekige rode lijnen. Met deze verbindingen buiten de virtuele netwerken, de virtuele machines die worden uitgevoerd in een van deze virtuele netwerken kunnen toegang krijgen tot elk van de verschillende HANA grote instantie eenheden geïmplementeerd in de twee regio's. Zoals in de afbeelding wordt weergegeven, wordt ervan uitgegaan dat u twee ExpressRoute-verbindingen van on-premises naar de twee Azure-regio's hebt. Deze benadering wordt aanbevolen voor disaster recovery redenen.

> [!IMPORTANT] 
> Als u meerdere ExpressRoute-circuits hebt gebruikt, mogen AS-padtoevoeging en lokale voorkeur BGP-instellingen worden gebruikt om ervoor te zorgen goede routering van verkeer.


