---
title: Overzicht en architectuur van SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Overzicht van de architectuur van het SAP HANA in Azure (grote exemplaren) implementeren.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/02/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ceeec6991aaac64211301313c1bb8dc5f5faa1c0
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Overzicht van de SAP HANA (grote exemplaren) en architectuur op Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Wat is een SAP HANA in Azure (grote exemplaren)?

SAP HANA in Azure (grote exemplaren) is een unieke oplossing naar Azure. Naast het leveren van virtuele machines voor het implementeren en uitvoeren van SAP HANA, biedt Azure u de mogelijkheid om te worden uitgevoerd en SAP HANA implementeren op bare metal-servers die aan u zijn toegewezen. De SAP HANA in Azure (grote exemplaren)-oplossing is gebaseerd op niet-gedeelde/hostserver bare-metal hardware die aan u is toegewezen. De serverhardware is ingesloten in grotere stempels die compute/server-, netwerk- en opslaginfrastructuur bevatten. Als een combinatie is afgestemd HANA center gegevensintegratie (TDI) gecertificeerd. SAP HANA in Azure (grote exemplaren) biedt een andere server SKU's of grootten. Eenheden kunnen 72 CPU's en 768 GB geheugen en Ga naar eenheden waarvoor 960 CPU's en 20 TB aan geheugen.

De klantisolatie binnen de infrastructuur stempel moet worden uitgevoerd in tenants, ziet dat:

- **Networking**: isolatie van klanten in de infrastructuur-stack via virtuele netwerken per klant toegewezen tenant. Een tenant wordt toegewezen aan één klant. Een klant kan meerdere tenants hebben. De netwerkisolatie van tenants verbiedt netwerkcommunicatie tussen de tenants op het niveau van de stempel infrastructuur, zelfs als de tenants deel uitmaken van dezelfde klant.
- **Opslagonderdelen**: isolatie door middel van opslag virtuele machines met opslagvolumes die zijn toegewezen. Opslagvolumes kunnen worden toegewezen aan slechts één opslag virtuele machine. Een opslag virtuele machine is toegewezen aan één één tenant in de SAP HANA TDI gecertificeerde infrastructuur-stack. Als gevolg hiervan zijn opslagvolumes die zijn toegewezen aan een virtuele machine voor opslag in een specifieke en verwante-tenant alleen toegankelijk. Ze zijn niet zichtbaar zijn tussen de verschillende geïmplementeerde tenants.
- **Server- of host**: een eenheid server- of host wordt niet gedeeld tussen klanten of tenants. Een server of de host is geïmplementeerd voor een klant, is een atomic bare-metal compute-eenheid die is toegewezen aan één voor één tenant. *Geen* hardware partitioneren of zachte partitioneren wordt gebruikt die ertoe kunnen leiden dat u het delen van een host of een server met een andere klant. Opslagvolumes die zijn toegewezen aan de opslag virtuele machine van de specifieke tenant zijn gekoppeld aan deze server. Een tenant kan een naar veel server eenheden van verschillende SKU's exclusief toegewezen hebben.
- Binnen een SAP HANA op Azure (grote exemplaren) infrastructuur stempel, zijn veel verschillende tenants geïmplementeerd en ten opzichte van elkaar geïsoleerd door middel van de tenant-concepten van netwerken, opslag en compute-niveau. 


Deze eenheden bare metal-server worden ondersteund voor SAP HANA alleen worden uitgevoerd. De toepassingslaag SAP of werkbelasting middleware-laag wordt uitgevoerd op virtuele machines. De stempels infrastructuur met de SAP HANA op Azure (grote exemplaren) eenheden zijn verbonden met het netwerk van Azure services wervels. Op deze manier, is lage latentie connectiviteit tussen virtuele machines en SAP HANA op Azure (grote exemplaren) eenheden opgegeven.

Dit document is een van de verschillende documenten die betrekking hebben op SAP HANA in Azure (grote exemplaren). Dit document worden de basisarchitectuur, verantwoordelijkheden en services die worden geleverd door de oplossing. Op hoog niveau worden van de oplossing ook besproken. Vier andere documenten uitgelegd voor de meeste andere gebieden, zoals netwerken en verbindingen, details en inzoomen informatie. De documentatie voor SAP HANA in Azure (grote exemplaren) dekt niet aspecten van de installatie van de SAP NetWeaver of implementaties van SAP NetWeaver in virtuele machines. SAP NetWeaver in Azure wordt beschreven in afzonderlijke documenten gevonden in de container met dezelfde Azure-documentatie. 


De verschillende documenten van een grote exemplaar HANA richtlijn ingegaan op de volgende gebieden:

- [Overzicht van de SAP HANA (grote exemplaren) en architectuur op Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastructuur voor SAP HANA (grote exemplaren) en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Installeren en configureren van SAP HANA (grote exemplaren) op Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen in Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Probleemoplossing voor SAP HANA (grote exemplaren) en de controle op Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hoge beschikbaarheid in SUSE met behulp van de STONITH instellen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [OS-back-up en herstel voor Type II SKU 's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Definities

Verschillende gemeenschappelijke definities worden veel gebruikt in de architectuur en technische Deployment Guide. Houd rekening met de volgende termen en hun betekenis:

- **IaaS**: infrastructuur als een service.
- **PaaS**: Platform als een service.
- **SaaS**: Software als een service.
- **SAP-onderdeel**: een afzonderlijke SAP-toepassing, zoals ERP centrale onderdeel (ECC), Business Warehouse (BW), oplossing Manager of EP (Enterprise Portal). SAP-onderdelen kunnen worden gebaseerd op traditionele ABAP of Java-technologieën of een NetWeaver op basis van toepassing zoals zakelijke objecten.
- **SAP-omgeving**: een of meer onderdelen voor SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke functie, zoals ontwikkeling, kwaliteit zekerheid, training, herstel na noodgevallen of productie.
- **SAP liggend**: verwijst naar de gehele SAP bedrijfsmiddelen in uw IT-Liggend. De SAP-liggend bevat alle productie en niet-productieve omgevingen.
- **SAP-systeem**: de combinatie van laag DBMS en toepassingslaag van bijvoorbeeld een SAP ERP-ontwikkelsysteem, een testsysteem SAP BW en een productiesysteem SAP CRM. Implementaties van Azure bieden geen ondersteuning voor het delen van deze twee lagen tussen on-premises en Azure. Een SAP-systeem is geïmplementeerde on-premises of de geïmplementeerd in Azure. U kunt de verschillende systemen van een liggend SAP in Azure of on-premises implementeren. U kunt bijvoorbeeld de CRM SAP-ontwikkeling implementeren en testen van systemen in Azure, terwijl u de SAP CRM productie system on-premises implementeert. Het bedoeld om u de toepassingslaag SAP van SAP-systemen in virtuele machines en de bijbehorende SAP HANA-exemplaar op een eenheid in de SAP HANA op Azure (grote exemplaren) stempel host te voor SAP HANA in Azure (grote exemplaren).
- **Grote exemplaar stempel**: een hardware-infrastructuur stack SAP HANA TDI gecertificeerd en toegewezen aan een SAP HANA-exemplaren in Azure uitvoeren.
- **SAP HANA in Azure (grote exemplaren):** officiële taal voor de aanbieding in Azure uitvoeren HANA-exemplaren op SAP HANA TDI-gecertificeerde hardware die wordt geïmplementeerd in grote exemplaar stempels in verschillende Azure-regio's. De verwante term *HANA grote exemplaar* is te kort voor *SAP HANA in Azure (grote exemplaren)* en wordt veel gebruikt in deze handleiding technische implementatie.
- **Cross-premises**: Beschrijving van een scenario waarin virtuele machines zijn geïmplementeerd met een Azure-abonnement met site-naar-site meerdere locaties of Azure ExpressRoute-verbinding tussen lokale datacenters en Azure. Gemeenschappelijk Azure-documentatie, dit soort implementaties worden ook beschreven als cross-premises scenario's. De reden voor de verbinding is om lokale domeinen, lokale Azure Active Directory/OpenLDAP en lokale DNS-uitbreiden naar Azure. De lokale Liggend wordt uitgebreid naar de Azure activa van de Azure-abonnementen. Met deze uitbreiding, kunnen de virtuele machines deel uitmaken van het lokale domein. 

   Gebruikers van een domein van het lokale domein kunnen toegang tot de servers en services uitvoeren op deze VMs (zoals DBMS services). Communicatie en naamomzetting tussen VM's geïmplementeerd lokale en virtuele machines van Azure is geïmplementeerd, is mogelijk. Dit scenario is Typerend voor de manier waarop de meeste SAP-elementen zijn geïmplementeerd. Zie voor meer informatie [plannen en ontwerpen voor Azure VPN-Gateway](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [een virtueel netwerk met een site-naar-site-verbinding maken met behulp van de Azure-portal](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Tenant**: een klant die is geïmplementeerd in grote exemplaar HANA stempel opgehaald geïsoleerd in een *tenant.* Een tenant is geïsoleerd van netwerken, opslag en berekeningslaag van andere tenants. Opslag en berekeningen eenheden die zijn toegewezen aan de verschillende tenants kunnen elkaar zien of op het niveau van de stempel HANA grote exemplaar met elkaar communiceren. Een klant kunt implementaties in verschillende tenants hebben. Er is zelfs dan geen communicatie tussen de tenants op het niveau van de stempel HANA grote exemplaar.
- **SKU-categorie**: voor HANA grote exemplaar, de volgende twee categorieën van SKU's zijn beschikbaar:
    - **Type ik klasse**: S72, S72m S144, S144m, S192 en S192m
    - **Typ II klasse**: S384, S384m S384xm, S576, S768 en S960


Een aantal extra bronnen zijn beschikbaar op het implementeren van een SAP-workload in de cloud. Als u van plan bent om een SAP HANA-implementatie worden uitgevoerd in Azure, moet u ervaring hebt met en op de hoogte van de beginselen van Azure IaaS en de implementatie van SAP werkbelastingen op Azure IaaS. Voordat u doorgaat, Zie [gebruik SAP-oplossingen voor virtuele machines in Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie. 

## <a name="certification"></a>Certificering

Naast de certificering NetWeaver vereist SAP een speciale voor SAP HANA ondersteuning bieden voor SAP HANA op bepaalde infrastructuren, zoals Azure IaaS-certificering.

De core SAP-notitie op NetWeaver en een zekere mate SAP HANA-certificaat is [SAP-notitie #1928533 – SAP-toepassingen in Azure: ondersteunde producten en typen van de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533).

De [SAP Opmerking #2316233 - SAP HANA op Microsoft Azure (grote exemplaren)](https://launchpad.support.sap.com/#/notes/2316233/E) is ook belangrijk. Deze heeft de oplossing in deze handleiding beschreven. Bovendien worden ondersteund voor SAP HANA uitvoering in het type GS5 VM van Azure. Informatie voor deze aanvraag is gepubliceerd op [de SAP-website](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

De SAP HANA in Azure (grote exemplaren)-oplossing in SAP-notitie #2316233 bedoelde biedt Microsoft en SAP-klanten de mogelijkheid voor het implementeren van grote SAP Business Suite, SAP BW, S/4 HANA, BW/4HANA of andere werkbelastingen SAP HANA in Azure. De oplossing is gebaseerd op de SAP HANA gecertificeerde specifieke hardware stempel ([SAP HANA aangepast center gegevensintegratie – TDI](https://scn.sap.com/docs/DOC-63140)). Als u een oplossing voor SAP HANA TDI geconfigureerd uitvoert, worden alle SAP HANA-toepassingen (zoals SAP Business Suite van SAP HANA, SAP BW voor SAP HANA, S4/HANA en BW4/HANA) werkt op de hardware-infrastructuur.

Vergeleken met de SAP HANA uitgevoerd in de virtuele machines, is deze oplossing een voordeel. Het biedt voor een veel grotere volumes van geheugen. Om deze oplossing, moet u inzicht in de volgende belangrijke aspecten:

- De SAP-toepassing-laag en niet-SAP-toepassingen uitvoeren in virtuele machines die worden gehost in de gebruikelijke Azure hardware stempels.
- Klant on-premises infrastructuur, datacenters en implementaties van toepassingen zijn verbonden met de cloudplatform, via ExpressRoute (aanbevolen) of een virtueel particulier netwerk (VPN). Active Directory en DNS worden ook uitgebreid naar Azure.
- Het SAP HANA-database-exemplaar voor de werkbelasting van HANA SAP HANA in Azure (grote exemplaren) wordt uitgevoerd. De stempel grote exemplaar is verbonden in Azure-netwerken, zodat de software die wordt uitgevoerd in virtuele machines kan communiceren met de HANA-exemplaar in grote HANA-exemplaar.
- Hardware van de SAP HANA in Azure (grote exemplaren) is specifieke hardware die zijn opgegeven in een IaaS met SUSE Linux Enterprise Server of Red Hat Enterprise Linux vooraf geïnstalleerd. Net als bij virtuele machines, verdere is updates en onderhoud van het besturingssysteem uw verantwoordelijkheid.
- Installatie van HANA of eventuele extra onderdelen die nodig zijn voor SAP HANA uitvoeren op eenheden van grote HANA-exemplaar is uw verantwoordelijkheid. Alle respectieve conflicterende bewerkingen worden uitgevoerd en het beheer van SAP HANA in Azure zijn ook uw eigen verantwoordelijkheid.
- Naast de oplossingen die hier wordt beschreven, kunt u andere onderdelen in uw Azure-abonnement maakt verbinding met SAP HANA in Azure (grote exemplaren) installeren. Voorbeelden zijn onderdelen waarmee de communicatie met of rechtstreeks in de SAP HANA databaseservers, zoals jump servers, RDP, SAP HANA Studio SAP Data Services voor SAP BI-scenario's of bewakingsoplossingen netwerk.
- Als in Azure biedt grote exemplaar HANA ondersteuning voor hoge beschikbaarheid en functionaliteit voor herstel na noodgevallen.

## <a name="architecture"></a>Architectuur

De SAP HANA in Azure (grote exemplaren)-oplossing heeft op een hoog niveau de SAP-toepassingslaag die zich in de virtuele machines. De databaselaag bevindt zich op SAP TDI geconfigureerde hardware die zich in een grote exemplaar stempel in dezelfde Azure-regio die is verbonden met Azure IaaS.

> [!NOTE]
> Implementeer de toepassingslaag SAP in dezelfde Azure-regio als de SAP DBMS-laag. Deze regel goed wordt gedocumenteerd in gepubliceerde informatie over SAP werkbelastingen op Azure. 

De algehele architectuur van SAP HANA in Azure (grote exemplaren) biedt een SAP TDI gecertificeerd hardwareconfiguratie, namelijk een niet-gevirtualiseerde, bare metal, hoge prestaties server voor SAP HANA-database is. Het biedt tevens de mogelijkheid en flexibiliteit van Azure tot bronnen van de schaal voor de toepassingslaag SAP om te voldoen aan uw behoeften.

![Overzicht van de architectuur van SAP HANA in Azure (grote exemplaren)](./media/hana-overview-architecture/image1-architecture.png)

De architectuur die wordt weergegeven, is onderverdeeld in drie secties:

- **Rechts**: bevat een on-premises infrastructuur met verschillende toepassingen in de gegevens zodat eindgebruikers toegang LOB-toepassingen, zoals SAP tot draait. In het ideale geval dit lokale infrastructuur vervolgens is verbonden met Azure met [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Center**: Azure IaaS toont en in dit geval gebruik van virtuele machines als host voor SAP of andere toepassingen die gebruikmaken van SAP HANA als een DBMS-systeem. Kleinere HANA-exemplaren die werken met het geheugen dat virtuele machines bieden zijn samen met hun toepassingslaag in virtuele machines geïmplementeerd. Zie voor meer informatie over virtuele machines [virtuele machines](https://azure.microsoft.com/services/virtual-machines/).

   Netwerk van Azure-services worden gebruikt voor de groep SAP-systemen samen met andere toepassingen in virtuele netwerken. Deze virtuele netwerken verbinding met on-premises systemen ook garantie voor SAP HANA in Azure (grote exemplaren).

   Zie voor SAP NetWeaver toepassingen en databases die worden ondersteund voor het uitvoeren in Azure, [SAP ondersteuning Opmerking #1928533 – SAP-toepassingen in Azure: ondersteunde producten en typen van de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533). Zie voor documentatie over het implementeren van SAP-oplossingen in Azure:

  -  [SAP op virtuele machines van Windows gebruiken](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [SAP-oplossingen gebruiken op virtuele machines in Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Links**: toont de SAP HANA TDI-gecertificeerde hardware in de stempel grote exemplaar van Azure. De eenheden HANA grote exemplaar zijn verbonden met de virtuele netwerken van uw abonnement met behulp van dezelfde technologie als de connectiviteit van on-premises in Azure.

De stempel Azure grote exemplaar zelf combineert de volgende onderdelen:

- **Computing**: Servers die zijn gebaseerd op Intel Xeon E7-8890v3 of Intel Xeon E7-8890v4 processors die bieden de mogelijkheid voor nodig computergebruik en SAP HANA gecertificeerd.
- **Netwerk**: A unified snel netwerk-fabric die de computer-, opslag en LAN-onderdelen met elkaar verbindt.
- **Opslag**: een opslaginfrastructuur die toegankelijk is via een netwerk-infrastructuur. De specifieke opslagcapaciteit die is opgegeven, is afhankelijk van de specifieke SAP HANA van Azure (grote exemplaren)-configuratie die is geïmplementeerd. Meer opslagcapaciteit is beschikbaar op een extra maandelijkse kosten.

Binnen de infrastructuur van de stempel grote exemplaar van meerdere tenants, klanten geïmplementeerd als geïsoleerde tenants. Bij de implementatie van de tenant, kunt u een Azure-abonnement naam binnen uw Azure-inschrijving. Deze Azure-abonnement is degene die het grote HANA-exemplaar wordt in rekening gebracht tegen. Deze tenants een 1:1 relatie hebben met het Azure-abonnement. Voor een netwerk is het mogelijk toegang tot een grote exemplaar HANA eenheid geïmplementeerd in een tenant in een Azure-regio uit verschillende virtuele netwerken die deel uitmaken van verschillende Azure-abonnementen. Deze Azure-abonnementen moeten behoren tot de dezelfde Azure-inschrijving. 

Net als bij virtuele machines, wordt SAP HANA in Azure (grote exemplaren) aangeboden in meerdere Azure-regio's. Wilt disaster recovery mogelijkheden bieden, kunt u kiezen voor deelname. Andere grote exemplaar stempels binnen één geografisch politieke regio zijn aan elkaar verbonden. Bijvoorbeeld, zijn HANA grote exemplaar stempels in VS-West en VS-Oost verbonden via een speciaal netwerk-koppeling voor disaster recovery replicatie. 

Net zoals u tussen verschillende VM typen met Azure Virtual Machines kiezen kunt, kunt u kiezen uit verschillende SKU's van HANA grote instantie die speciaal voor de werkbelasting van de verschillende soorten SAP HANA geschikt zijn. SAP geldt geheugen processor socket-ratio's voor verschillende werkbelastingen op basis van de Intel processor generaties. De volgende tabel toont de SKU-typen die worden aangeboden.

SAP HANA in Azure (grote exemplaren) is vanaf juli 2017 beschikbaar in verschillende configuraties in de Azure-regio's VS-West, VS-Oost, Australië-Oost, Australië-Zuidoost, West-Europa en van Noord-Europa.

| SAP-oplossing | CPU | Geheugen | Storage | Beschikbaarheid |
| --- | --- | --- | --- | --- |
| Geoptimaliseerd voor OLAP-: SAP BW, BW/4HANA<br /> of SAP HANA voor algemene OLAP-werkbelasting | SAP HANA op Azure S72<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU-kernen en 72 CPU threads |  768 GB |  3 TB | Beschikbaar |
| --- | SAP HANA op Azure S144<br /> – 4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU-kernen en 144 CPU threads |  1,5 TB |  6 TB | Niet beschikbaar meer |
| --- | SAP HANA op Azure S192<br /> – 4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en 192 CPU threads |  2.0 TB |  8 TB | Beschikbaar |
| --- | SAP HANA op Azure S384<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kernen en 384 CPU threads |  4.0 TB |  16 TB | Beschikbaar |
| Geoptimaliseerd voor OLTP: SAP Business Suite<br /> voor SAP HANA of S/4HANA (OLTP)<br /> algemene OLTP | SAP HANA op Azure S72m<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU-kernen en 72 CPU threads |  1,5 TB |  6 TB | Beschikbaar |
|---| SAP HANA op Azure S144m<br /> – 4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU-kernen en 144 CPU threads |  3.0 TB |  12 TB | Niet beschikbaar meer |
|---| SAP HANA op Azure S192m<br /> – 4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en 192 CPU threads  |  4.0 TB |  16 TB | Beschikbaar |
|---| SAP HANA op Azure S384m<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kernen en 384 CPU threads |  6.0 TB |  18 TB | Beschikbaar |
|---| SAP HANA op Azure S384xm<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kernen en 384 CPU threads |  8.0 TB |  22 TB |  Beschikbaar |
|---| SAP HANA op Azure S576<br /> – 12 x Intel Xeon®-Processor E7 8890 v4<br /> 288 CPU-kernen en 576 CPU threads |  12.0 TB |  28 TB | Beschikbaar |
|---| SAP HANA op Azure S768<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU-kernen en 768 CPU threads |  16,0 TB |  36 TB | Beschikbaar |
|---| SAP HANA op Azure S960<br /> – 20 x Intel® Xeon® Processor E7-8890 v4<br /> 480 CPU-kernen en 960 CPU threads |  20,0 TB |  46 TB | Beschikbaar |

- CPU-kernen = de som van de CPU-kernen niet-hyper-threaded van de som van de processors van de server-eenheid.
- CPU-threads = de som van de compute-threads geleverd door hyper-threaded CPU-kernen van de som van de processors van de server-eenheid. Alle eenheden zijn standaard geconfigureerd voor gebruik van Hyper-Threading-technologie.


De specifieke configuraties gekozen zijn afhankelijk van de werkbelasting, CPU-bronnen en gewenste geheugen. Het is mogelijk voor OLTP-werkbelasting met de SKU's die zijn geoptimaliseerd voor de OLAP-werkbelasting. 

De hardware voor de aanbiedingen zijn SAP HANA TDI gecertificeerd. Twee verschillende soorten hardware deelt de SKU's in:

- S72, S72m S144, S144m, S192 en S192m, die zijn aangeduid als 'Type ik klasse' van SKU's.
- S384, S384m S384xm, S576, S768 en S960 die zijn aangeduid als de 'Typ II klasse' van SKU's.

Een volledige HANA grote exemplaar stempel uitsluitend voor één klant is niet toegewezen&#39;s gebruik. Dit geldt voor rekken van berekenings- en bronnen die verbonden zijn via een netwerk fabric ook geïmplementeerd in Azure. Grote exemplaar HANA-infrastructuur, zoals Azure, implementeert u verschillende klanten &quot;tenants&quot; die zijn geïsoleerd van elkaar in de volgende drie niveaus:

- **Netwerk**: isolatie door middel van virtuele netwerken in het stempel HANA grote exemplaar.
- **Opslag**: isolatie door middel van opslag virtuele machines die zijn toegewezen opslagvolumes en isoleren opslagvolumes tussen de tenants.
- **COMPUTE**: toewijzing van eenheden van de server toegewezen aan een enkele tenant. Nee hard of soft partitionering van eenheden van de server. Er is geen delen van een enkele server of de hostnaam eenheid tussen de tenants. 

De implementaties van eenheden tussen verschillende tenants HANA grote exemplaar zijn niet zichtbaar zijn voor elkaar. Grote exemplaar HANA eenheden die zijn geïmplementeerd in verschillende tenants kunnen niet communiceren direct met elkaar op het niveau van de stempel HANA grote exemplaar. Alleen groot exemplaar HANA eenheden binnen een tenant kunnen communiceren met elkaar op het niveau van de stempel HANA grote exemplaar.

Een geïmplementeerde tenant in de grote exemplaar stempel is toegewezen aan één Azure-abonnement voor facturatie. Voor een netwerk, kan deze worden geopend op virtuele netwerken van andere Azure-abonnementen binnen de dezelfde Azure-inschrijving. Als u met een andere Azure-abonnement in dezelfde Azure-regio implementeert, kunt ook u om een gescheiden HANA grote exemplaar tenant te vragen.

Er zijn belangrijke verschillen tussen actieve SAP HANA op grote HANA-exemplaar en SAP HANA uitgevoerd op virtuele machines die zijn geïmplementeerd in Azure:

- Er is geen virtualisatielaag voor SAP HANA in Azure (grote exemplaren). Krijgt u de prestaties van de onderliggende bare-metal hardware.
- In tegenstelling tot Azure, worden de SAP HANA op Azure (grote exemplaren)-server is toegewezen aan een specifieke klant. Er is geen mogelijkheid waarop een eenheid van de server of de host is hard of soft gepartitioneerd. Als gevolg hiervan wordt een grote HANA-exemplaar gebruikt als in zijn geheel in een tenant en met die aan u toegewezen. Een opnieuw opstarten of afsluiten van de server leiden niet automatisch tot het besturingssysteem en SAP HANA wordt geïmplementeerd op een andere server. (Voor het Type ik klasse SKU's, de enige uitzondering hierop is als een server problemen optreedt en opnieuw implementeren gaande moet worden uitgevoerd op een andere server.)
- In tegenstelling tot Azure, waarbij host processortypen zijn geselecteerd voor de beste prijs/prestatie-verhouding, zijn de processortypen gekozen voor SAP HANA in Azure (grote exemplaren) de hoogste prestaties van de regel Intel E7v3 en E7v4 processor.


### <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Meerdere exemplaren van de SAP HANA uitvoeren op een grote exemplaar HANA-eenheid
Het is mogelijk voor het hosten van meer dan één actieve SAP HANA-exemplaar op grote exemplaar HANA eenheden. Een dergelijke configuratie vereist om te bieden de mogelijkheden van opslag-momentopnamen en herstel na noodgevallen, een volume per exemplaar instellen. Grote exemplaar HANA eenheden kunnen op dit moment is als volgt worden onderverdeeld:

- **S72, S72m, S144, S192**: In stappen van 256 GB, met de kleinste begin eenheid van 256 GB. Verschillende stappen zoals 256 en 512 GB kunnen worden gecombineerd tot het maximum van het geheugen van de eenheid.
- **S144m en S192m**: In stappen van 256 GB, met de kleinste eenheid van 512 GB. Verschillende stappen zoals 512 en 768 GB kunnen worden gecombineerd tot het maximum van het geheugen van de eenheid.
- **Typ II klasse**: In stappen van 512 GB met de kleinste begin eenheid van 2 TB. Verschillende stappen zoals 512 GB en 1 TB 1,5 TB kunnen worden gecombineerd tot het maximum van het geheugen van de eenheid.

Enkele voorbeelden van meerdere instanties voor SAP HANA kunnen er als volgt uitzien.

| SKU | Geheugengrootte | Opslaggrootte | Grootten met meerdere databases |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA exemplaar<br /> of 1 x 512 GB exemplaar + 1 x 256 GB-exemplaar<br /> of 3 x 256 GB exemplaren | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA exemplaren<br />of 1 x 512 GB exemplaar + 1 x 1 TB exemplaar<br />of 6 x 256 GB exemplaren<br />of 1x1.5 TB exemplaar | 
| S192m | 4 TB | 16 TB | 8 x 512 GB exemplaren<br />of 4 x 1 TB exemplaren<br />of exemplaren van 4 x 512 GB + 2 x 1 TB exemplaren<br />of exemplaren van 4 x 768 GB + 2 x 512 GB exemplaren<br />of 1 x 4 TB-exemplaar |
| S384xm | 8 TB | 22 TB | 4 x 2 TB exemplaren<br />of 2 x 4 TB exemplaren<br />of 2 x 3 TB exemplaren + 1 x 2 TB exemplaren<br />of 2x2.5 TB exemplaren + 1 x 3 TB exemplaren<br />of 1 x 8 TB exemplaar |


Er zijn ook andere variaties. 

### <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA in lagen en extensie gegevensknooppunten gebruiken
SAP ondersteunt een gegevensmodel lagen voor SAP BW van verschillende SAP NetWeaver releases en SAP BW/4HANA. Zie voor meer informatie over de lagen van het gegevensmodel, het document SAP [SAP BW/4HANA en SAP BW op HANA met knooppunten voor SAP HANA-extensie](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Met grote HANA-exemplaar kunt u de optie 1-configuratie van SAP HANA-extensie knooppunten zoals uitgelegd in de blog Veelgestelde vragen over en SAP documenten. Optie 2-configuraties kunnen worden ingesteld met de volgende HANA grote exemplaar SKU's: S72m, S192 S192m, S384 en S384m. 

Wanneer u de documentatie bekijkt, het voordeel mogelijk niet direct zichtbaar. Maar wanneer u de richtlijnen van SAP sizing bekijkt, ziet u een voordeel met behulp van de optie-1 en de optie 2 SAP HANA-extensie-knooppunten. Hier volgen voorbeelden:

- Richtlijnen voor SAP HANA-formaat is meestal de dubbele hoeveelheid gegevensvolume als geheugen vereist. Wanneer u uw SAP HANA-exemplaar met de hot gegevens uitvoert, hebt u slechts 50% of minder van het geheugen gevuld met gegevens. De rest van het geheugen wordt in het ideale geval bewaard voor SAP HANA tijdens het doorzoeken van het werk.
- Dat betekent in een eenheid HANA grote exemplaar S192 met 2 TB aan geheugen, met een SAP BW-database, hoeft u slechts 1 TB als gegevensvolume.
- Als u een extra knooppunt van de extensie SAP HANA van optie 1, ook een S192 HANA grote exemplaar SKU, dit biedt u een extra capaciteit 2 TB voor gegevensvolume. In de optie 2-configuratie, kunt u een extra 4 TB voor warme gegevensvolume ophalen. Vergeleken met het knooppunt hot, worden de volledige geheugencapaciteit van de 'warme' extensieknooppunt gebruikt voor het opslaan van gegevens voor de optie-1. Dubbele het geheugen kan worden gebruikt voor gegevensvolume in knooppuntconfiguratie voor optie 2 SAP HANA-extensie.
- U uiteindelijk eindigen met een capaciteit van 3 TB voor uw gegevens en een ratio van hot naar warme 1:2 voor optie 1. U hebt 5 TB aan gegevens en een ratio van 1:4 met de configuratie voor de uitbreiding van de optie 2-knooppunt.

Hoe hoger het gegevensvolume vergeleken met het geheugen, hoe groter de kans op zijn dat de warme gegevens dat u daarvoor is opgeslagen op de schijfopslag.


## <a name="operations-model-and-responsibilities"></a>Operations-model en verantwoordelijkheden

De service die wordt geleverd met SAP HANA in Azure (grote exemplaren) wordt uitgelijnd met Azure IaaS-services. Krijgt u een exemplaar van een grote HANA-exemplaar met een geïnstalleerd besturingssysteem die is geoptimaliseerd voor SAP HANA. Met Azure IaaS VM's, de meeste van de taken voor het beperken van het besturingssysteem, extra software te installeren, HANA installeren, besturingssystemen en HANA besturingssysteem en bijwerken van het besturingssysteem en HANA is uw verantwoordelijkheid. Microsoft biedt geen OS-updates of updates HANA afdwingen op u.

![De verantwoordelijkheden van SAP HANA in Azure (grote exemplaren)](./media/hana-overview-architecture/image2-responsibilities.png)

Zoals u in het diagram, is de SAP HANA in Azure (grote exemplaren) een multitenant IaaS bieden. De deling van verantwoordelijk is voor het grootste deel op de grens OS-infrastructuur. Microsoft is verantwoordelijk voor alle aspecten van de service onder de regel van het besturingssysteem. U bent zelf verantwoordelijk voor alle aspecten van de service boven de regel. Het besturingssysteem is uw verantwoordelijkheid. U kunt blijven gebruiken van meest recente lokale methoden die u gebruikmaken van mogelijk voor naleving, beveiliging, toepassingsbeheer, basis en OS-beheer. De systemen worden weergegeven alsof ze zich in uw netwerk ten opzichte van alle.

Deze service is geoptimaliseerd voor SAP HANA, dus er gebieden waarin u werken met Microsoft zijn wilt om de onderliggende infrastructuurmogelijkheden gebruiken voor de beste resultaten.

De volgende lijst geeft meer details over elk van de lagen en uw verantwoordelijkheden:

**Networking**: de interne netwerken voor grote exemplaar van stempel met SAP HANA. Uw verantwoordelijkheid bevat toegang tot opslag, connectiviteit tussen de exemplaren (voor scale-out en andere functies), verbinding met de liggend en verbindingen naar Azure waar de toepassingslaag SAP wordt gehost in virtuele machines. Dit omvat ook WAN-verbinding tussen Azure-datacenters voor disaster recovery doeleinden replicatie. Alle netwerken zijn gepartitioneerd door de tenant en kwaliteit van de service toegepast hebben.

**Opslag**: de gevirtualiseerde opslag voor alle volumes die door de SAP HANA-servers nodig, evenals voor momentopnamen is gepartitioneerd. 

**Servers**: de speciale fysieke servers naar de SAP HANA-databases die zijn toegewezen aan tenants worden uitgevoerd. De servers van het Type ik klasse van SKU's zijn gescheiden hardware. Met deze typen servers, de serverconfiguratie worden verzameld en onderhouden in profielen die kunnen worden verplaatst van één fysieke hardware voor een andere fysieke hardware. (Dergelijke een handmatig) verplaatsen van een profiel door bewerkingen kan enigszins voor herstel van de Azure-service worden vergeleken. De servers van het Type II klasse SKU's bieden niet dergelijke functionaliteit.

**SDDC**: de software die wordt gebruikt om gegevens te beheren als door software gedefinieerde entiteiten draait. Dit kan Microsoft naar bronnen voor schaal, beschikbaarheid en prestaties van toepassingen.

**Besturingssysteem**: het besturingssysteem die u kiest (SUSE Linux of Red Hat Linux) die wordt uitgevoerd op de servers. De installatiekopieën van die u worden geleverd met het besturingssysteem zijn opgegeven door de leverancier van de afzonderlijke Linux naar Microsoft voor het uitvoeren van SAP HANA. U moet een abonnement met de leverancier van de Linux voor de specifieke geoptimaliseerd voor SAP HANA-installatiekopie hebben. U bent zelf verantwoordelijk voor het registreren van de afbeeldingen met de leverancier van het besturingssysteem. 

Vanaf het moment van overdracht door Microsoft bent u verantwoordelijk voor eventuele verdere patchen van het Linux-besturingssysteem. Deze patchen extra pakketten die mogelijk noodzakelijk voor een geslaagde installatie van de SAP HANA en die niet zijn opgenomen door de leverancier van de specifieke Linux in hun SAP HANA bevat geoptimaliseerde installatiekopieën van het besturingssysteem. (Zie de SAP HANA installatie documentatie en SAP-opmerkingen voor meer informatie.) 

U bent zelf verantwoordelijk voor het besturingssysteem patchen als gevolg van storing of optimalisatie van het besturingssysteem en de stuurprogramma's ten opzichte van de specifieke serverhardware. Ook bent u verantwoordelijk voor beveiliging of functionele patchen van het besturingssysteem. 

Uw verantwoordelijkheid bevat ook bewaken en plannen van capaciteit van:

- CPU-resourceverbruik.
- Geheugengebruik.
- Schijfvolumes gerelateerd aan vrije ruimte, IOPS en latentie.
- Volume netwerkverkeer tussen HANA grote exemplaar en de toepassingslaag SAP.

De onderliggende infrastructuur van grote exemplaar HANA biedt functionaliteit voor back-up en herstel van het volume met het besturingssysteem. Deze functionaliteit is ook uw eigen verantwoordelijkheid.

**Middleware**: de SAP HANA-exemplaar, voornamelijk. Bent u verantwoordelijk voor beheer, bewerkingen en controle. De opgegeven functionaliteit kunt u opslag-momentopnamen gebruiken voor back-up en herstel en noodherstel hersteldoeleinden. Deze mogelijkheden worden geleverd door de infrastructuur. Uw verantwoordelijkheden ook ontwerpen van hoge beschikbaarheid of herstel na noodgevallen met deze mogelijkheden gebruik te maken van deze en controle kunt bepalen of opslag-momentopnamen met succes uitgevoerd.

**Gegevens**: uw gegevens worden beheerd door een SAP HANA en andere gegevens zoals back-ups van bestanden op volumes of bestand deelt. Uw verantwoordelijkheden zijn vrije schijfruimte controleren en beheren van de inhoud op de volumes. Ook bent u verantwoordelijk voor het bewaken van de voltooiing van uitvoering van de back-ups van volumes op schijven en opslag-momentopnamen. Voltooiing van uitvoering van de gegevensreplicatie naar noodherstelsites is de verantwoordelijkheid van Microsoft.

**Toepassingen:** de SAP-toepassingsexemplaren of, in het geval van niet-SAP-toepassingen, de toepassingslaag van deze toepassingen. Uw verantwoordelijkheden zijn implementatie, beheer, bewerkingen en controle van deze toepassingen. U bent zelf verantwoordelijk voor de capaciteitsplanning van verbruik van CPU, geheugengebruik, Azure Storage-verbruik en netwerkbandbreedte in virtuele netwerken. Ook bent u verantwoordelijk voor capaciteitsplanning voor het verbruik van virtuele netwerken voor SAP HANA in Azure (grote exemplaren).

**WAN**: de verbindingen die u tot stand van on-premises naar Azure implementaties voor werkbelastingen brengen. Alle klanten met HANA grote exemplaar gebruiken Azure ExpressRoute voor connectiviteit. Deze verbinding maakt geen deel uit van de SAP HANA in Azure (grote exemplaren)-oplossing. U bent zelf verantwoordelijk voor het instellen van deze verbinding.

**Archief**: U mogelijk liever archiveren kopieën van gegevens met behulp van uw eigen methoden in opslagaccounts. Archiveren vereist management, naleving, kosten en bewerkingen. U bent verantwoordelijk voor het genereren archiveren kopieert en back-ups op Azure en deze opslaan op een compatibele manier.

Zie de [SLA voor SAP HANA in Azure (grote exemplaren)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Schaling

Sizing voor HANA grote exemplaar gaat niet anders dan in het algemeen voor HANA formaat. Voor bestaande en systemen die u verplaatsen van andere RDBMS naar HANA wilt, SAP biedt een aantal rapporten die worden uitgevoerd op uw bestaande SAP-systemen geïmplementeerd. Als de database is verplaatst naar HANA, worden deze rapporten Controleer de gegevens en geheugenvereisten voor het exemplaar HANA berekenen. Lees de volgende SAP-opmerkingen voor meer informatie over het uitvoeren van deze rapporten en krijgen hun meest recente patches of versies:

- [SAP-notitie #1793345 - formaat voor SAP-Suite op HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP-notitie #1872170 - Suite op HANA en S/4 HANA sizing rapport](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-notitie #2121330 - Veelgestelde vragen over: SAP BW op HANA sizing rapport](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP-notitie #1736976 - schaling rapport voor BW op HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP-notitie #2296290 - formaat nieuw rapport voor BW op HANA](https://launchpad.support.sap.com/#/notes/2296290)

Groen veld implementaties is snelle Sizer SAP voor geheugenvereisten van de implementatie van software SAP bovenop HANA berekenen.

Geheugenvereisten voor HANA verhogen naarmate gegevensvolume toeneemt. Let op het huidige geheugengebruik om te voorspellen wat er gebeurt in de toekomst. Op basis van geheugenvereisten, kunt klikt u vervolgens u toewijzen uw vraag in een van de HANA grote exemplaar SKU's.

## <a name="requirements"></a>Vereisten

Deze lijst samengevoegd naast de vereisten voor SAP HANA uitgevoerd op Azure (grotere exemplaren).

**Microsoft Azure**

- Een Azure-abonnement dat kan worden gekoppeld aan een SAP HANA in Azure (grote exemplaren).
- Microsoft Premier support-contract. Zie voor specifieke informatie die betrekking hebben op SAP worden uitgevoerd in Azure, [SAP ondersteuning Opmerking #2015553 – SAP op Microsoft Azure: vereisten ondersteunen](https://launchpad.support.sap.com/#/notes/2015553). Als u grote exemplaar HANA eenheden met 384 en meer CPU's gebruikt, moet u ook uitbreiden van de Premier support-contract snelle Azure-antwoord opnemen.
- Kennis van de HANA grote exemplaar SKU's u moet na het uitvoeren van een oefening sizing met SAP.

**Netwerkverbinding**

- ExpressRoute tussen on-premises naar Azure: als u wilt verbinding maken met uw on-premises Datacenter naar Azure, zorg ervoor dat u ten minste een 1-Gbps verbinding rangschikken van uw Internetprovider. 

**Besturingssysteem**

- Licenties voor SUSE Linux Enterprise Server 12 voor SAP-toepassingen.

   > [!NOTE] 
   > Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd bij SUSE. Dit is niet gekoppeld aan een exemplaar van het beheerprogramma voor abonnement.

- SUSE Linux abonnement beheerprogramma geïmplementeerd in Azure op een virtuele machine. Dit hulpprogramma biedt de mogelijkheid voor SAP HANA in Azure (grote exemplaren) worden geregistreerd en respectievelijk bijgewerkt door SUSE. (Er is geen internettoegang binnen het datacenter grote exemplaar HANA.) 
- Licenties voor Red Hat Enterprise Linux 6.7 of 7.2 voor SAP HANA.

   > [!NOTE]
   > Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd met Red Hat. Dit is niet gekoppeld aan een exemplaar van het Red Hat abonnement Manager.

- Red Hat abonnement Manager is geïmplementeerd in Azure op een virtuele machine. De Red Hat abonnement Manager biedt de mogelijkheid voor SAP HANA in Azure (grote exemplaren) worden geregistreerd en respectievelijk bijgewerkt door Red Hat. (Er is geen directe toegang tot internet vanaf binnen de tenant die wordt geïmplementeerd op de stempel grote exemplaar van Azure.)
- SAP moet u een ondersteuningsaanvraag hebben contract met uw Linux-provider. Deze vereiste is niet verwijderd door de oplossing van grote HANA-exemplaar of het feit Linux in Azure uit te voeren. In tegenstelling tot met enkele van de installatiekopieën van het galerie Linux Azure de servicekosten is *niet* opgenomen in de aanbieding oplossing voor grote HANA-exemplaar. Het is uw verantwoordelijkheid om te voldoen aan de vereisten van SAP met betrekking tot support-contract met de distributor Linux. 
   - SUSE Linux, zoekt u de vereisten van support-contract in [SAP Opmerking #1984787 - SUSE Linux Enterprise Server 12: opmerkingen bij de installatie](https://launchpad.support.sap.com/#/notes/1984787) en [SAP Opmerking #1056161 - SUSE prioriteitsondersteuning voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/1056161).
   - Voor Red Hat Linux moet u het juiste abonnement toegangsniveaus die zijn ondersteuning en service-updates voor de besturingssystemen van grote HANA-exemplaar. Red Hat Red Hat Enterprise Linux aanbeveelt voor [SAP-oplossingen] (https://access.redhat.com/solutions/3082481 abonnement. 

Zie voor de ondersteuningsmatrix van de verschillende SAP HANA-versies met de verschillende versies van Linux [SAP-notitie #2235581](https://launchpad.support.sap.com/#/notes/2235581).


**Database**

- Licenties en software-installatie van onderdelen voor SAP HANA (platform of enterprise edition).

**Toepassingen**

- Licenties en software-installatie-onderdelen voor alle SAP-toepassingen die verbinding met SAP HANA en verwante SAP maken ondersteuning voor contracten.
- Licenties en software-installatie-onderdelen voor alle toepassingen die niet SAP gebruikt in relatie tot SAP HANA op Azure (grote exemplaren)-omgevingen en gerelateerde support-contract.

**Vaardigheden**

- Ervaring met en kennis van Azure IaaS en de bijbehorende onderdelen.
- Ervaring met en kennis over het implementeren van een werkbelasting SAP in Azure.
- SAP HANA-installatie gecertificeerd personeel.
- SAP architect vaardigheden voor het ontwerpen van hoge beschikbaarheid en herstel na noodgevallen rond SAP HANA.

**SAP**

- Verwachting is dat u een SAP-klant bent en ondersteuning voor een contract met SAP.
- Raadpleeg SAP op versies van SAP HANA en de uiteindelijke configuraties op grote schaal omhoog hardware met name voor implementaties van de klasse Type II van HANA grote exemplaar SKU's.


## <a name="storage"></a>Storage

De opslagindeling voor SAP HANA in Azure (grote exemplaren) is geconfigureerd door SAP HANA op het klassieke implementatiemodel via SAP aanbevolen richtlijnen. De richtlijnen zijn gedocumenteerd in de [opslagvereisten voor SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) witboek.

Het HANA grote exemplaar van het Type ik klasse wordt geleverd met vier keer het geheugen volume opslagvolume. Voor de klasse Type II van grote exemplaar HANA eenheden, de opslag bevindt zich niet meer vier keer. De eenheden worden geleverd met een volume dat is bedoeld voor het opslaan van HANA transactielogboekback-ups. Zie voor meer informatie [installeren en configureren van SAP HANA (grote exemplaren) op Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie de volgende tabel in termen van de opslagtoewijzing van. De tabel bevat de ruwe capaciteit voor de verschillende volumes die zijn opgegeven met de verschillende HANA grote exemplaar eenheden.

| HANA grote exemplaar SKU | hana/data | Hana/logboek | Hana/gedeeld | Hana/log/back-up |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S192 | 4.608 GB | 1.024 GB | 1.536 GB | 1.024 GB |
| S192m | 11,520 GB | 1.536 GB | 1.792 GB | 1.536 GB |
| S384 | 11,520 GB | 1.536 GB | 1.792 GB | 1.536 GB |
| S384m | 12.000 GB | 2.050 GB | 2.050 GB | 2,040 GB |
| S384xm | 16.000 GB | 2.050 GB | 2.050 GB | 2,040 GB |
| S576 | 20.000 GB | 3,100 GB | 2.050 GB | 3,100 GB |
| S768 | 28.000 GB | 3,100 GB | 2.050 GB | 3,100 GB |
| S960 | 36.000 GB | 4,100 GB | 2.050 GB | 4,100 GB |


Werkelijke geïmplementeerde volumes kunnen variëren, afhankelijk van de implementatie en het hulpprogramma dat wordt gebruikt voor het weergeven van de grootte van het volume.

Als u een exemplaar van HANA groot SKU onderverdelen, een paar voorbeelden van mogelijke deling stukken als volgt uitzien:

| Geheugenpartitie in GB | hana/data | Hana/logboek | Hana/gedeeld | Hana/log/back-up |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1.792 GB | 640 GB | 1.024 GB | 640 GB |
| 1536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Deze grootten zijn ruwe volume cijfers die kan verschillen enigszins verschillen op basis van de implementatie en de hulpprogramma's gebruikt om te kijken naar de volumes. Er zijn ook andere partitiegrootten zoals 2,5 TB. Deze maximale grootten zijn berekend met een formule vergelijkbaar met die van de vorige partities. De term 'partities' betekent niet dat het besturingssysteem, geheugen of CPU-bronnen worden op elke manier gepartitioneerd. Hiermee wordt aangegeven partities van de opslag voor de verschillende HANA-exemplaren die u mogelijk wilt implementeren op een enkele grote exemplaar HANA-eenheid. 

Mogelijk moet u meer opslagruimte. U kunt opslag toevoegen door het aanschaffen van extra opslagruimte in eenheden van 1 TB. Deze extra opslagruimte kan worden toegevoegd als extra volume. Het kan ook worden gebruikt voor het uitbreiden van een of meer van de bestaande volumes. Het niet mogelijk te verlagen de grootte van de volumes oorspronkelijk geïmplementeerd en voornamelijk door de vorige tabellen worden beschreven. Is het ook niet kunt wijzigen van de namen van de volumes of namen koppelen. De eerder beschreven opslagvolumes zijn gekoppeld aan de eenheden HANA grote exemplaar als NFS4 volumes.

Voor hersteldoeleinden back-up en herstel en noodherstel kunt u opslag-momentopnamen. Zie voor meer informatie [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest
Opslagruimte wordt gebruikt voor grote exemplaar HANA kan een transparante codering van de gegevens, zoals deze wordt opgeslagen op de schijven. Wanneer een grote exemplaar HANA-eenheid wordt geïmplementeerd, kunt u dit soort versleuteling inschakelen. U kunt ook wijzigen met versleutelde volumes nadat de implementatie plaatsgevonden heeft. De overgang van niet-versleutelde naar versleutelde volumes is transparant en geen downtime nodig. 

Met het Type klasse ik van SKU's, de LUN wordt opgeslagen op opstarten van het volume is versleuteld. Voor de klasse Type II van SKU's van HANA grote exemplaar moet u voor het versleutelen van het bestand Boot.ini LUN met OS-methoden. Voor meer informatie contact op met het beheer van de Service Microsoft-team.


## <a name="networking"></a>Netwerken

De architectuur van het netwerk van Azure services is een belangrijk onderdeel van de geslaagde implementatie van SAP-toepassingen op grote HANA-exemplaar. SAP HANA in implementaties van Azure (grote exemplaren) hebben doorgaans een groter SAP Liggend met verschillende andere SAP-oplossingen met verschillende grootten van databases, verbruik van CPU en geheugengebruik. Is het waarschijnlijk dat niet alle die SAP-systemen zijn gebaseerd op SAP HANA. Uw SAP-liggend is waarschijnlijk een hybride die gebruikmaakt van:

- SAP systemen lokale geïmplementeerd. Vanwege de grootte niet kunnen deze systemen op dit moment worden gehost in Azure. Een voorbeeld is een productie SAP ERP-systeem die wordt uitgevoerd op SQL Server (als de database) en vereist meer CPU of geheugen bronnen dan virtuele machines kunnen bieden.
- SAP SAP HANA gebaseerde systemen lokale geïmplementeerd.
- Geïmplementeerde SAP-systemen in virtuele machines. Deze systemen kunnen ontwikkeling, testen, sandbox, of productie instanties voor een van de SAP NetWeaver-toepassingen in Azure (op VM's) implementeren kunnen, op basis van gebruiks- en behoefte aan bronnen. Deze systemen kunnen ook worden gebaseerd op zoals SQL Server-databases. Zie voor meer informatie [SAP ondersteuning Opmerking #1928533 – SAP-toepassingen in Azure: ondersteunde producten en typen van de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533/E). En deze systemen kunnen worden gebaseerd op databases zoals SAP HANA. Zie voor meer informatie [SAP HANA gecertificeerd IaaS platforms](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
- SAP toepassingsservers in Azure (op VM's) die gebruikmaken van de SAP HANA in Azure (grote exemplaren) in Azure grote exemplaar stempels geïmplementeerd.

Een hybride SAP Liggend met vier of meer verschillende scenario's is normaal. Er zijn ook veel aanvragen van de klant van volledige SAP landschappen die worden uitgevoerd in Azure. Wanneer er virtuele machines komen krachtiger, verhoogt het aantal klanten die overschakelen van alle hun SAP-oplossingen in Azure.

Azure-netwerken in de context van SAP-systemen die zijn geïmplementeerd in Azure is eenvoudig. Deze gebaseerd op de volgende principes:

- Virtuele netwerken van Azure moeten worden verbonden met het ExpressRoute-circuit dat verbinding met een on-premises netwerk maakt.
- Een ExpressRoute-circuit die meestal verbindt van lokaal naar Azure moet een bandbreedte van 1 Gbps of hoger hebben. Deze minimale bandbreedte kunt voldoende bandbreedte voor de overdracht van gegevens tussen on-premises systemen en systemen die worden uitgevoerd op virtuele machines. Kunt u ook voldoende bandbreedte voor de verbinding met Azure systemen van on-premises gebruikers.
- Alle SAP-systemen in Azure moeten worden ingesteld in virtuele netwerken met elkaar communiceren.
- Active Directory en DNS gehost lokale on-premises worden uitgebreid naar Azure via ExpressRoute.


> [!NOTE] 
> Vanuit het facturering oogpunt kan slechts één Azure-abonnement worden gekoppeld aan slechts één tenant in een grote exemplaar stempel in een specifieke Azure-regio. Als u daarentegen kan een enkele grote exemplaar stempel tenant zijn gekoppeld aan slechts één Azure-abonnement. Deze vereiste is consistent met andere factureerbare objecten in Azure.

Als SAP HANA in Azure (grote exemplaren) is geïmplementeerd in meerdere verschillende Azure-regio's, wordt een afzonderlijke tenant geïmplementeerd in de stempel grote exemplaar. U kunt beide onder dezelfde Azure-abonnement uitvoeren, zolang deze instanties deel van de dezelfde mate van SAP uitmaken. 

> [!IMPORTANT] 
> De implementatie van de Azure Resource Manager wordt ondersteund met SAP HANA in Azure (grote exemplaren).

 

### <a name="additional-virtual-network-information"></a>Informatie over aanvullende virtuele netwerk

Voor een virtueel netwerk verbinding met ExpressRoute, moet een Azure-gateway worden gemaakt. Zie voor meer informatie [over virtuele netwerkgateways voor ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 

Een Azure-gateway kan worden gebruikt met ExpressRoute naar een infrastructuur buiten Azure of een stempel grote exemplaar van Azure. Een Azure-gateway kan ook verbinding maken tussen virtuele netwerken worden gebruikt. Zie voor meer informatie [een netwerk-netwerkverbinding voor Resource Manager configureren met behulp van PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). U kunt verbinding maken met de Azure gateway tot maximaal vier verschillende ExpressRoute-verbindingen, zolang deze verbindingen afkomstig van andere Microsoft enterprise-randrouters zijn. Zie voor meer informatie [SAP HANA (grote exemplaren)-infrastructuur en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> De doorvoer die een Azure-gateway biedt verschilt voor beide gevallen gebruikt. Zie voor meer informatie [over VPN-Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). De maximale doorvoer die u met een virtuele netwerkgateway bereiken kunt is 10 Gbps met behulp van een ExpressRoute-verbinding. Kopiëren van bestanden tussen een virtuele machine die zich in een virtueel netwerk bevindt en een systeem lokale (als een stream met één exemplaar) de volledige doorvoer van de verschillende gateway-SKU's niet bereiken. Als u wilt gebruikmaken van de volledige bandbreedte van de virtuele netwerkgateway gebruik meerdere streams. Of moet u verschillende bestanden parallelle stromen van één enkel bestand kopiëren.


### <a name="networking-architecture-for-hana-large-instance"></a>Architectuur van de netwerken voor grote HANA-exemplaar
De netwerkarchitectuur voor grote HANA-exemplaar kan worden onderverdeeld in vier verschillende onderdelen:

- On-premises netwerken en ExpressRoute-verbinding met Azure. Dit onderdeel is van de klant domein en is verbonden met Azure via ExpressRoute. Zie de rechtsonder in de volgende afbeelding.
- Netwerk van Azure-services, zoals eerder besproken, met virtuele netwerken, die opnieuw gateways zijn. Dit onderdeel is een gebied waarin u moet de juiste ontwerpen voor uw toepassingsvereisten-, beveiligings- en nalevingsvereisten niet vinden. Of u grote HANA-exemplaar is een ander punt rekening houden met betrekking tot het aantal virtuele netwerken en Azure gateway-SKU's kiezen. Zie de rechtsboven in de afbeelding.
- Connectiviteit van grote exemplaar HANA via ExpressRoute technologie in Azure. Dit onderdeel is geïmplementeerd en uitgevoerd door Microsoft. U hoeft te doen is bieden sommige IP-adresbereiken na de implementatie van uw assets in grote exemplaar HANA het ExpressRoute-circuit verbinding met de virtuele netwerken. Zie voor meer informatie [SAP HANA (grote exemplaren)-infrastructuur en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Netwerken in grote HANA-instantie, die voornamelijk transparant is voor u.

![Virtueel netwerk is verbonden met SAP HANA op Azure (grote exemplaren) en on-premises](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

De vereisten die uw assets lokale verbinding via ExpressRoute naar Azure maken moeten wijzigen niet omdat u HANA grote exemplaar gebruiken. De vereiste om een of meerdere virtuele netwerken die worden uitgevoerd van de virtuele machines, die de toepassingslaag die verbinding met de HANA-exemplaren die worden gehost in eenheden HANA grote exemplaar maakt host, ook wijzigen niet. 

De verschillen op SAP-implementatie in Azure zijn:

- De eenheden HANA grote exemplaar van de tenant van uw klant zijn verbonden via een ander ExpressRoute-circuit in uw virtuele netwerken. Belastingsvoorwaarden afzonderlijke, delen niet de on-premises virtuele netwerken ExpressRoute koppelingen en de koppelingen tussen virtuele netwerken en grote HANA-exemplaar de routers dezelfde.
- Het profiel van de werkbelasting tussen de toepassingslaag SAP en het grote HANA-exemplaar is van een andere aard, met veel kleine aanvragen en barst zoals (resultaatsets) uit de SAP HANA in de toepassingslaag gegevensoverdracht.
- De architectuur van de toepassing SAP is meer gevoelig voor netwerklatentie dan typische scenario's waarbij gegevens worden uitgewisseld tussen on-premises en Azure.
- De virtuele netwerkgateway heeft ten minste twee ExpressRoute-verbindingen. Beide verbindingen delen de maximale bandbreedte voor binnenkomende gegevens van de virtuele netwerkgateway.

De netwerklatentie opgetreden tussen VM's en grote exemplaar HANA eenheden kunnen hoger zijn dan een typische round trip latentie van de VM-VM-netwerk. Afhankelijk van de Azure-regio, de meetwaarden kunnen overschrijden de 0,7 ms round trip latentie geclassificeerd zoals hieronder gemiddelde in [SAP Opmerking #1100926 - Veelgestelde vragen over: prestaties van het netwerk](https://launchpad.support.sap.com/#/notes/1100926/E). Niettemin implementeren klanten op basis van een SAP HANA SAP productietoepassingen uitgevoerd op grote SAP HANA-exemplaar. De klanten die zijn geïmplementeerd rapport verbeterd door het uitvoeren van hun SAP-toepassingen op SAP HANA met behulp van de grote exemplaar HANA eenheden. Zorg ervoor dat u uw bedrijfsprocessen grondig te testen in Azure HANA grote exemplaar.
 
Om te bieden deterministische netwerklatentie tussen VM's en grote HANA-exemplaar, de keuze van de virtuele netwerkgateway SKU is essentieel. In tegenstelling tot de verkeerspatronen tussen on-premises en virtuele machines, kunt het patroon verkeer tussen VM's en grote HANA-exemplaar ontwikkelen van kleine maar groot lichtflitsen aanvragen en gegevensvolumes worden verzonden. Voor het afhandelen van dergelijke bursts goed, raden we het gebruik van de gateway UltraPerformance SKU. Voor de klasse Type II HANA grote exemplaar SKU is het gebruik van de gateway UltraPerformance SKU als een virtuele netwerkgateway verplicht.

> [!IMPORTANT] 
> Gezien de algehele netwerkverkeer tussen de SAP-toepassing en de lagen van de database, worden alleen de HighPerformance of UltraPerformance gateway-SKU's voor virtuele netwerken voor de verbinding met SAP HANA in Azure (grote exemplaren) ondersteund. Voor HANA grote exemplaar Type II SKU's, worden alleen de UltraPerformance-gateway SKU wordt ondersteund als een virtuele netwerkgateway.



### <a name="single-sap-system"></a>Eenmalige SAP-systeem

De on-premises infrastructuur eerder weergegeven is verbonden via ExpressRoute in Azure. Het ExpressRoute-circuit op een enterprise-rand router worden aangesloten. Zie voor meer informatie [technisch overzicht van ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nadat de route is gemaakt, wordt deze verbinding maakt in de Azure-backbone en alle Azure-regio's zijn toegankelijk.

> [!NOTE] 
> Als u wilt SAP landschappen in Azure uitvoert, verbinding maken met de enterprise edge-router die het dichtst bij de Azure-regio in de SAP-Liggend. Azure grote exemplaar stempels zijn verbonden via speciale enterprise edge router apparaten netwerklatentie tussen virtuele machines in Azure IaaS en grote exemplaar stempels minimaliseren.

De virtuele netwerkgateway voor de virtuele machines die als host SAP toepassingsexemplaren fungeren is verbonden met het ExpressRoute-circuit. Hetzelfde virtuele netwerk is verbonden met een afzonderlijke enterprise edge-router toegewezen aan de verbinding te maken met grote exemplaar stempels.

Dit systeem is een eenvoudig voorbeeld van een enkel SAP-systeem. De toepassingslaag SAP wordt gehost in Azure. De SAP HANA-database wordt uitgevoerd op een SAP HANA in Azure (grote exemplaren). De veronderstelling is dat de virtuele gateway bandbreedte van 2 Gbps of 10 Gbps doorvoer een knelpunt niet vertegenwoordigen.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Meerdere SAP-systemen of grote SAP-systemen

Als meerdere SAP-systemen of grote SAP-systemen worden geïmplementeerd voor verbinding met SAP HANA in Azure (grote exemplaren), kan de doorvoer van de virtuele netwerkgateway een knelpunt geworden. In dat geval de toepassingslagen in meerdere virtuele netwerken te splitsen. U kunt ook een speciale virtuele netwerk die verbinding met de grote exemplaar HANA gevallen, zoals maakt maken:

- Het uitvoeren van back-ups rechtstreeks vanuit de HANA-exemplaren in grote HANA-exemplaar voor een virtuele machine in Azure die als host fungeert voor NFS-shares.
- Back-ups van grote of andere bestanden kopiëren van grote exemplaar HANA eenheden schijfruimte beheerd in Azure.

Gebruik een afzonderlijke virtueel netwerk naar host virtuele machines die opslag te beheren. Deze regeling voorkomt de gevolgen van grote bestanden of overdracht van gegevens uit de grote exemplaar HANA naar Azure op de virtuele netwerkgateway die de virtuele machines die worden uitgevoerd van de toepassingslaag SAP fungeert. 

Voor een meer schaalbare netwerkarchitectuur:

- Maak gebruik van meerdere virtuele netwerken voor een enkele, grotere SAP toepassingslaag.
- Implementeer één afzonderlijke virtueel netwerk voor elk SAP-systeem dat is geïmplementeerd, vergeleken met het combineren van deze systemen SAP in afzonderlijke subnetten in hetzelfde virtuele netwerk.

 Een meer schaalbare netwerkarchitectuur voor SAP HANA in Azure (grote exemplaren):

![SAP toepassingslaag via meerdere virtuele netwerken implementeren](./media/hana-overview-architecture/image4-networking-architecture.png)

De afbeelding toont de toepassingslaag SAP of onderdelen, geïmplementeerd in meerdere virtuele netwerken. Deze configuratie geïntroduceerd onvermijdelijke latentie overhead die is opgetreden tijdens de communicatie tussen de toepassingen die worden gehost in deze virtuele netwerken. Standaard wordt het netwerkverkeer tussen virtuele machines zich in verschillende virtuele netwerken doorsturen via de enterprise-randrouters in deze configuratie. Sinds September 2016 kan deze routering worden geoptimaliseerd. 

De manier te optimaliseren en de latentie in de communicatie tussen twee virtuele netwerken beperken is door de peering virtuele netwerken in dezelfde regio. Deze methode werkt ook als deze virtuele netwerken zich op verschillende abonnementen behoren. Met het virtuele netwerk peering, kunt de communicatie tussen VM's in twee verschillende virtuele netwerken de Azure-netwerk-backbone gebruiken om rechtstreeks met elkaar communiceren. Latentie toont alsof de virtuele machines zich in hetzelfde virtuele netwerk. Het verkeer die zijn gericht op IP-adresbereiken die zijn verbonden via de gateway virtuele Azure-netwerk doorgestuurd via de afzonderlijke virtuele netwerkgateway van het virtuele netwerk. 

Zie voor meer informatie over het virtuele netwerk peering [virtuele netwerk peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Routering in Azure

Drie netwerk routering overwegingen zijn belangrijk voor SAP HANA in Azure (grote exemplaren):

* SAP HANA in Azure (grote exemplaren) kan alleen worden benaderd via VM's en de speciale ExpressRoute-verbinding niet rechtstreeks van on-premises. Directe toegang krijgen tot on-premises de HANA grote exemplaar eenheden, die door Microsoft worden geleverd, is niet mogelijk onmiddellijk. De transitieve routering beperkingen zijn vanwege de huidige Azure-netwerk-architectuur voor SAP HANA grote exemplaar gebruikt. Bepaalde beheerclients en alle toepassingen die u nodig hebt voor rechtstreekse toegang, zoals SAP oplossing Manager on-premises uitgevoerd kunnen geen verbinding maken met de SAP HANA-database.

* Als er grote exemplaar HANA eenheden die zijn geïmplementeerd in twee verschillende Azure-regio's voor herstel na noodgevallen, wordt de dezelfde tijdelijke routering beperkingen toegepast. Met andere woorden, IP-adressen van een grote exemplaar HANA-eenheid in één regio (bijvoorbeeld VS-West) niet doorgestuurd naar een grote exemplaar HANA eenheid geïmplementeerd in een andere regio (bijvoorbeeld VS-Oost). Deze beperking is onafhankelijk van het gebruik van Azure-netwerk peering tussen regio's of de ExpressRoute-circuits die grote exemplaar HANA eenheden verbinding met virtuele netwerken cross-verbinding. Zie voor een grafische weergave in de afbeelding in de sectie "Gebruik HANA grote exemplaar eenheden in meerdere regio's." Deze beperking wordt geleverd met de geïmplementeerde architectuur, verbiedt het direct gebruik van HANA System Replication als de herstelfunctionaliteit na noodgevallen.

* SAP HANA op Azure (grote exemplaren) eenheden hebben een toegewezen IP-adres van de server IP-adresgroepbereik dat u hebt ingediend. Zie voor meer informatie [SAP HANA (grote exemplaren)-infrastructuur en de verbindingen van Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dit IP-adres is toegankelijk via de Azure-abonnementen en ExpressRoute die is verbonden virtuele netwerken met HANA in Azure (grote exemplaren). Het IP-adres toegewezen uit dat de server IP-adresbereik van adresgroep rechtstreeks aan de hardware-eenheid is toegewezen. Deze heeft *niet* toegewezen via NAT voordoet, net zoals in de eerste implementatie van deze oplossing. 

> [!NOTE] 
> Gebruik voor de beperking in de tijdelijke routering overwinnen zoals wordt beschreven in de eerste twee lijstitems, extra onderdelen voor routering. Onderdelen die kunnen worden gebruikt voor de beperking overwinnen kunnen zijn:

> * Een omgekeerde proxy voor het routeren van gegevens naar en uit. Zoals F5 BIG-IP, NGINX met Traffic Manager, geïmplementeerd in Azure als virtuele firewall/verkeer routering oplossing.
> * Met behulp van [IPTables regels](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) in een Linux-VM routering tussen lokale locaties en grote exemplaar HANA eenheden of tussen HANA grote exemplaar eenheden in verschillende regio's inschakelen.

> Let erop dat de implementatie en ondersteuning voor aangepaste oplossingen van derden met betrekking tot het netwerk toestellen of IPTables wordt niet geleverd door Microsoft. Worden moet ondersteund door de leverancier van het onderdeel dat wordt gebruikt of de integrator. 

### <a name="internet-connectivity-of-hana-large-instance"></a>Verbinding met Internet van grote HANA-exemplaar
HANA grote exemplaar komt *niet* beschikt over een rechtstreekse internetverbinding. Als u bijvoorbeeld kan deze beperking beperken uw registreren van de installatiekopie van het besturingssysteem rechtstreeks met de leverancier van het besturingssysteem. Mogelijk moet u samenwerken met uw lokale SUSE Linux Enterprise Server abonnement Management Tool-server of Red Hat Enterprise Linux abonnement Manager.

### <a name="data-encryption-between-vms-and-hana-large-instance"></a>Versleuteling van gegevens tussen VM's en grote HANA-exemplaar
Gegevens die overgedragen tussen HANA grote exemplaar en virtuele machines is niet versleuteld. U kunt echter uitsluitend voor de uitwisseling tussen de HANA DBMS en JDBC/ODBC-toepassingen, versleuteling van verkeer inschakelen. Zie voor meer informatie [deze documentatie door SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

### <a name="use-hana-large-instance-units-in-multiple-regions"></a>Grote exemplaar HANA eenheden gebruiken in meerdere regio 's

Mogelijk hebt u redenen voor het implementeren van SAP HANA in Azure (grote exemplaren) in meerdere Azure-regio's anders dan voor herstel na noodgevallen. Misschien wilt u toegang tot grote HANA-exemplaar van elk van de VM is geïmplementeerd in de verschillende virtuele netwerken in de regio's. De IP-adressen toegewezen aan de andere grote exemplaar HANA eenheden worden niet doorgegeven afgezien van de virtuele netwerken die rechtstreeks zijn verbonden via de gateway van de exemplaren. Als gevolg hiervan is een kleine wijziging werd geïntroduceerd in het ontwerp van het virtuele netwerk. Een virtuele netwerkgateway kan vier verschillende ExpressRoute-circuits buiten de andere enterprise-randrouters verwerken. Elk virtueel netwerk dat is verbonden met een van de stempels grote exemplaar kan worden verbonden met de stempel grote exemplaar in een andere Azure-regio.

![Virtueel netwerk is verbonden met Azure grote exemplaar stempels in verschillende Azure-regio 's](./media/hana-overview-architecture/image8-multiple-regions.png)

De afbeelding toont hoe de andere virtuele netwerken in beide regio's zijn verbonden met twee verschillende ExpressRoute-circuits zijn gebruikt voor verbinding met SAP HANA in Azure (grote exemplaren) in beide Azure-regio's. De nieuwe verbindingen zijn de rechthoekige rode lijnen. Met deze verbindingen, buiten de virtuele netwerken, de virtuele machines uitvoeren in een van deze virtuele netwerken toegang tot elk van de verschillende HANA grote exemplaar eenheden dat is geïmplementeerd in de twee regio's. Zoals u in de afbeelding ziet, wordt ervan uitgegaan dat u twee ExpressRoute-verbindingen van on-premises twee Azure-regio's hebt. Deze regeling wordt aanbevolen voor disaster recovery redenen.

> [!IMPORTANT] 
> Als u meerdere ExpressRoute-circuits gebruikt, moeten lokale voorkeur BGP-instellingen en AS-padtoevoeging correcte routering van verkeer worden gebruikt.


