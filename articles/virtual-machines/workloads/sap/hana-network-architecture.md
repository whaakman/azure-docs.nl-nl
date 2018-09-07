---
title: Het netwerk van de architectuur van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Architectuur van SAP HANA op Azure (grote instanties) implementeren op het netwerk.
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
ms.date: 09/04/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c2ab917f701ebcb78ae01a4ed97915858e5b95db
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028168"
---
# <a name="sap-hana-large-instances-network-architecture"></a>Netwerkarchitectuur van SAP HANA (grote instanties)

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

 

## <a name="additional-virtual-network-information"></a>Informatie over aanvullende virtuele netwerken

Voor een virtueel netwerk verbinding met ExpressRoute, moet een Azure-gateway worden gemaakt. Zie voor meer informatie, [virtuele netwerkgateways voor ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Een Azure-gateway kan worden gebruikt met ExpressRoute naar een infrastructuur buiten Azure of naar een Azure Large Instance-stempel. Een Azure-gateway kan ook worden gebruikt verbinding maken tussen virtuele netwerken. Zie voor meer informatie, [een netwerk-netwerkverbinding voor Resource Manager configureren met behulp van PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). U kunt de Azure-gateway tot een maximum van vier andere ExpressRoute-verbindingen kunt verbinden, zolang deze verbindingen afkomstig van andere Microsoft enterprise-randrouters zijn. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> De doorvoer vindt u een Azure-gateway verschilt voor beide. Zie voor meer informatie, [over VPN-Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). De maximale doorvoer die u met een virtuele netwerkgateway bereiken kunt is 10 Gbps met behulp van een ExpressRoute-verbinding. Kopiëren van bestanden tussen een virtuele machine die zich in een virtueel netwerk bevinden en een on-premises (als een stroom één exemplaar) de volledige doorvoer van de andere gateway-SKU's niet bereiken. Als u wilt gebruikmaken van de volledige bandbreedte van de virtuele netwerkgateway, gebruik van meerdere streams. Of u moet verschillende bestanden in parallelle stromen van één enkel bestand kopiëren.


## <a name="networking-architecture-for-hana-large-instance"></a>Architectuur van de netwerken voor HANA grote instantie
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



## <a name="single-sap-system"></a>Één SAP-systeem

De on-premises infrastructuur die eerder is getoond is verbonden via ExpressRoute in Azure. Het ExpressRoute-circuit verbinding maakt in een enterprise edge router. Zie voor meer informatie, [technisch overzicht van ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nadat de route tot stand is gebracht, verbinding wordt gemaakt in de Azure-backbone en alle Azure-regio's toegankelijk zijn.

> [!NOTE] 
> Als u wilt uitvoeren van SAP-landschappen in Azure, verbinding maken met de die het dichtst bij de Azure-regio in de SAP-landschap enterprise edge router. Azure Large Instance stempels zijn verbonden via een speciale enterprise edge router-apparaten voor minimale netwerklatentie tussen virtuele machines in Azure IaaS- en grote instantie stempels.

De virtuele netwerkgateway voor de virtuele machines die als exemplaren van SAP-toepassing host is verbonden met het ExpressRoute-circuit. Hetzelfde virtuele netwerk is verbonden met een speciaal voor de verbinding te maken met een grote instantie stempels afzonderlijke enterprise edge router.

Dit systeem is een eenvoudig voorbeeld van een enkele SAP-systeem. Het niveau van de SAP-toepassing wordt gehost in Azure. De SAP HANA-database wordt uitgevoerd op SAP HANA op Azure (grote instanties). De veronderstelling is dat een knelpunt niet staan voor de bandbreedte van het virtuele netwerk-gateway van 2 Gbps of 10 Gbps-doorvoer.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Meerdere SAP-systemen of grote SAP-systemen

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


## <a name="routing-in-azure"></a>Routering in Azure

Er zijn drie netwerk routering overwegingen belangrijk voor SAP HANA op Azure (grote instanties):

* SAP HANA op Azure (grote instanties) kan alleen worden verkregen via virtuele machines en het toegewezen ExpressRoute-verbinding wordt niet rechtstreeks van on-premises. Directe toegang vanaf on-premises naar de eenheden HANA grote instantie, die door Microsoft worden geleverd, kunt u niet mogelijk is onmiddellijk. De transitieve routering beperkingen zijn vanwege de huidige Azure-netwerk-architectuur die worden gebruikt voor SAP HANA grote instantie. Bepaalde beheerclients en alle toepassingen die toegang, zoals SAP oplossing Manager directe moeten on-premises uitgevoerd kunnen geen verbinding maken met de SAP HANA-database.

* Als u HANA grote instantie eenheden die zijn geïmplementeerd in twee verschillende Azure-regio's voor herstel na noodgevallen hebt, wordt de dezelfde tijdelijke routering beperkingen gelden. Met andere woorden, IP-adressen van een eenheid HANA grote instantie in één regio (bijvoorbeeld VS West) niet doorgestuurd naar een eenheid HANA grote instantie is geïmplementeerd in een andere regio (bijvoorbeeld VS Oost). Deze beperking is afhankelijk van het gebruik van Azure-netwerk peering met andere regio's of het ExpressRoute-circuits die verbinding maken met HANA grote instantie eenheden naar virtuele netwerken cross-verbinding. Zie voor een grafische weergave in de afbeelding in de sectie 'Gebruik HANA grote instantie eenheden in meerdere regio's '. Deze beperking, die wordt geleverd met de geïmplementeerde architectuur, verbiedt het direct gebruik van HANA-Systeemreplicatie als herstel na noodgevallen.

* SAP HANA op Azure (grote instanties)-eenheden hebben een toegewezen IP-adres uit het adresbereik van server IP-adresgroep die u hebt ingediend. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dit IP-adres is toegankelijk via de Azure-abonnementen en ExpressRoute dat virtuele netwerken met HANA op Azure (grote instanties verbindt). Het IP-adres toegewezen van die server IP-adresgroepbereik dat rechtstreeks is toegewezen aan de hardware-eenheid. Er *niet* toegewezen via NAT zich voordoet, net als in de eerste implementatie van deze oplossing. 

> [!NOTE] 
> De beperking in tijdelijke routering manieren, zoals wordt beschreven in de eerste twee items, om de extra onderdelen voor het routering te gebruiken. Onderdelen die kunnen worden gebruikt om te strijden tegen de beperking kunnen zijn:

> * Een omgekeerde proxy voor het routeren van gegevens naar en uit. Bijvoorbeeld, F5 BIG-IP, NGINX met Traffic Manager, geïmplementeerd in Azure als een virtuele firewall/verkeer routeren oplossing.
> * Met behulp van [regels van IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) in een Linux-VM om in te schakelen routering tussen on-premises locaties en HANA grote instantie eenheden, of tussen HANA grote instantie eenheden in verschillende regio's.

> Let erop dat de implementatie en ondersteuning voor aangepaste oplossingen van derden met betrekking tot netwerkapparaten of IPTables wordt niet geleverd door Microsoft. Ondersteuning moet worden opgegeven door de leverancier van het onderdeel dat wordt gebruikt of de gegevensintegrator worden verstrekt. 

## <a name="internet-connectivity-of-hana-large-instance"></a>Verbinding met Internet van HANA grote instantie
HANA grote instantie heeft *niet* directe verbinding met internet hebben. Als u bijvoorbeeld mogelijk deze beperking kunt gebruiken om het registreren van de installatiekopie van het besturingssysteem rechtstreeks met de leverancier van de OS beperken. Mogelijk moet u werken met uw lokale abonnement beheerprogramma voor SUSE Linux Enterprise Server-server of de Doelabonnementbeheerder van Red Hat Enterprise Linux.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>Versleuteling van gegevens tussen VM's en HANA grote instantie
Gegevens die worden overgedragen tussen HANA grote instantie en virtuele machines is niet versleuteld. U kunt echter alleen voor de uitwisseling tussen de HANA DBMS- en JDBC/ODBC-toepassingen, versleutelen van verkeer inschakelen. Zie voor meer informatie, [deze documentatie door SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>HANA grote instantie eenheden gebruiken in meerdere regio 's

Mogelijk hebt u redenen voor het implementeren van SAP HANA op Azure (grote instanties) in meerdere Azure-regio's dan voor herstel na noodgevallen. Misschien wilt u toegang tot HANA grote instantie van elk van de virtuele machines geïmplementeerd in de verschillende virtuele netwerken in de regio's. De IP-adressen die zijn toegewezen aan de verschillende HANA grote instantie-eenheden worden niet doorgegeven buiten de virtuele netwerken die rechtstreeks zijn verbonden via de gateway naar de exemplaren. Als gevolg hiervan is een kleine wijziging geïntroduceerd in het ontwerp van het virtuele netwerk. Vier verschillende ExpressRoute-circuits uit verschillende enterprise-randrouters kan worden verwerkt door een virtuele netwerkgateway. Elk virtueel netwerk die is verbonden met een van de grote instantie stempels kan worden verbonden met de stempel grote instantie in een andere Azure-regio.

![Virtueel netwerk is verbonden met Azure Large Instance stempels in verschillende Azure-regio 's](./media/hana-overview-architecture/image8-multiple-regions.png)

De afbeelding ziet u hoe de verschillende virtuele netwerken in beide regio's zijn verbonden met twee verschillende ExpressRoute-circuits verbinding maken met SAP HANA op Azure (grote instanties) worden gebruikt in zowel Azure-regio's. De onlangs geïntroduceerde verbindingen zijn het rechthoekige rode lijnen. Met deze verbindingen buiten de virtuele netwerken, de virtuele machines die worden uitgevoerd in een van deze virtuele netwerken kunnen toegang krijgen tot elk van de verschillende HANA grote instantie eenheden geïmplementeerd in de twee regio's. Zoals in de afbeelding wordt weergegeven, wordt ervan uitgegaan dat u twee ExpressRoute-verbindingen van on-premises naar de twee Azure-regio's hebt. Deze benadering wordt aanbevolen voor disaster recovery redenen.

> [!IMPORTANT] 
> Als u meerdere ExpressRoute-circuits hebt gebruikt, mogen AS-padtoevoeging en lokale voorkeur BGP-instellingen worden gebruikt om ervoor te zorgen goede routering van verkeer.

**Volgende stappen**
- Raadpleeg [opslagarchitectuur van SAP HANA (grote instanties)](hana-storage-architecture.md)