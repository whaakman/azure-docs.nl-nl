---
title: Met behulp van ExpressRoute met herstel van virtuele Azure-machine na noodgevallen | Microsoft Docs
description: Beschrijft hoe u Azure ExpressRoute gebruiken met Azure Site Recovery voor noodherstel van de virtuele machine van Azure
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 73514b524f554affb9730ba63ccd608491497af2
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920467"
---
# <a name="using-expressroute-with-azure-virtual-machine-disaster-recovery"></a>Met behulp van ExpressRoute met herstel van virtuele Azure-machine na noodgevallen

Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Dit artikel wordt beschreven hoe u ExpressRoute met Site Recovery kunt gebruiken voor herstel na noodgevallen van virtuele machines van Azure.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u begrijpt:
-   ExpressRoute [circuits](../expressroute/expressroute-circuit-peerings.md)
-   ExpressRoute [Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
-   Virtuele Azure-machine [-replicatiearchitectuur](azure-to-azure-architecture.md)
-   [Instellen van replicatie](azure-to-azure-tutorial-enable-replication.md) voor virtuele machines van Azure
-   [Failover-overschakeling uitvoeren](azure-to-azure-tutorial-failover-failback.md) virtuele machines van Azure

## <a name="expressroute-and-azure-virtual-machine-replication"></a>ExpressRoute- en Azure VM-replicatie

Bij het beveiligen van virtuele machines met Site Recovery in Azure, gegevens van replicatie wordt verzonden naar een Azure Storage-account of replica beheerde schijf op de doel-Azure-regio, afhankelijk van of uw virtuele machines van Azure gebruik [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md). Hoewel de replicatie-eindpunten openbaar zijn, door replicatieverkeer voor Azure VM-replicatie, standaard, het Internet, ongeacht welke Azure-regio de bron van virtueel netwerk bestaat in niet bladeren.

Voor noodherstel van de virtuele machine van Azure, zoals gegevens van replicatie de grens van de Azure laat is ExpressRoute niet vereist voor replicatie. Nadat de virtuele machines, failover naar de doel-Azure-regio, kunt u ze openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

## <a name="replicating-azure-deployments"></a>Azure-implementaties te repliceren

Een eerdere [artikel](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity), die een eenvoudige configuratie met een virtueel Azure-netwerk is verbonden met on-premises datacenter klant via ExpressRoute worden beschreven. Standaard enterprise-implementaties hebben werklasten verdeeld over meerdere virtuele Azure-netwerken en een hub centrale verbinding wordt tot stand gebracht van de externe verbinding, met Internet en on-premises implementaties.

Dit voorbeeld wordt beschreven in een hub en spoke-topologie, dit is gebruikelijk in enterprise-implementaties:
-   De implementatie is in de **Azure Oost-Azië** heeft een ExpressRoute-circuit-verbinding via een partner edge in Hong Kong regio en het on-premises datacenter.
-   Toepassingen worden geïmplementeerd op verschillende twee spoke-netwerken: **bron VNet1** met adresruimte 10.1.0.0/24 en **bron VNet2** met adresruimte 10.2.0.0/24.
-   Het virtuele netwerk van de hub, **bron Hub VNet**, met de adresruimte 10.10.10.0/24 fungeert als de gatekeeper. Alle communicatie tussen subnetten verloopt via de hub.
-   De virtuele hub-netwerk heeft twee subnetten: **NVA-Subnet** met adresruimte 10.10.10.0/25 en **Gatewaysubnet** met adresruimte 10.10.10.128/25.
-   De **NVA-subnet** heeft een virtueel netwerkapparaat met IP-adres 10.10.10.10.
-   De **Gatewaysubnet** is een ExpressRoute-gateway is verbonden met een ExpressRoute-verbinding die u routes naar klanten on-premises datacenter via een persoonlijke Peering routeringsdomein.
-   Elk knooppunt virtueel netwerk is verbonden met het virtuele hub-netwerk en alle routering in deze topologie van netwerk wordt beheerd via Azure Route tabellen (UDR). Al het uitgaande verkeer van het ene VNet naar het andere VNet of naar het on-premises datacenter wordt doorgestuurd via de NVA.

![On-premises-to-Azure met ExpressRoute voordat de Failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

### <a name="hub-and-spoke-peering"></a>Hub en spoke-peering

Het knooppunt naar hub-peering heeft de volgende configuratie:
-   Virtueel netwerkadres toestaan: ingeschakeld
-   Doorgestuurd verkeer toestaan: ingeschakeld
-   Gatewayoverdracht toestaan: uitgeschakeld
-   Gebruik verwijderen gateways: ingeschakeld

 ![Knooppunt naar hub peering-configuratie](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

De hub en knooppunt-peering heeft de volgende configuratie:
-   Virtueel netwerkadres toestaan: ingeschakeld
-   Doorgestuurd verkeer toestaan: ingeschakeld
-   Gatewayoverdracht toestaan: ingeschakeld
-   Gebruik verwijderen gateways: uitgeschakeld

 ![Hub en een peeringconfiguratie knooppunt](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="enabling-replication-for-the-deployment"></a>Inschakelen van replicatie voor de implementatie

Voor de installatie van de bovenstaande eerste [herstel na noodgevallen instellen](azure-to-azure-tutorial-enable-replication.md) voor elke virtuele machine met behulp van Site Recovery. Site Recovery kunt maken van de replica virtuele netwerken (met inbegrip van subnetten en gateway-subnetten) op de doelregio en de vereiste toewijzingen tussen de bron- en virtuele netwerken maken. U kunt ook vooraf maken de doel-kant netwerken en subnetten en gebruiken dezelfde tijdens het inschakelen van replicatie.

Site Recovery repliceert niet routetabellen, gateways voor virtueel netwerk, virtuele netwerkgatewayverbindingen, peering op virtueel netwerk, of een andere netwerken resources of verbindingen. Deze en andere resources die geen deel uit van de [replicatieproces](azure-to-azure-architecture.md#replication-process) moeten worden gemaakt tijdens of voordat de failover en verbinding maakt met de relevante bronnen. U kunt Azure Site Recovery krachtige [herstelplannen](site-recovery-create-recovery-plans.md) om het maken van en verbinding te maken van aanvullende bronnen met behulp van automatiseringsscripts te automatiseren.

Standaard verlaat het replicatieverkeer niet de Azure-grens. Normaal gesproken definiëren NVA implementaties ook een standaardroute (0.0.0.0/0) die ervoor zorgt uitgaand internetverkeer dat naar flow via het NVA. In dit geval kan het apparaat beperkingen als het replicatieverkeer wordt doorgegeven via de NVA. Hetzelfde geldt ook wanneer standaardroutes voor de routering van verkeer van alle virtuele machine van Azure naar on-premises implementaties. Het is raadzaam [het maken van een service-eindpunt voor virtueel netwerk](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in uw virtuele netwerk voor 'Opslag' zodat het replicatieverkeer niet Azure grens verlaat.

## <a name="failover-models-with-expressroute"></a>Failover-modellen met ExpressRoute

Wanneer virtuele machines van Azure zijn een failover naar een andere regio, is de bestaande ExpressRoute-verbinding met de bron van virtueel netwerk niet automatisch doorgegeven aan het virtuele doelnetwerk van de herstelregio. Een nieuwe verbinding is vereist voor ExpressRoute verbinden met het virtuele netwerk.

U kunt virtuele Azure-machines repliceren naar een Azure-regio binnen hetzelfde geografische cluster als gedetailleerde [hier](azure-to-azure-support-matrix.md#region-support). Als de gekozen Azure-doelregio niet binnen dezelfde geopolitieke regio als de bron valt, moet u ExpressRoute Premium inschakelen als u een enkel ExpressRoute-circuit voor bron- en regio-connectiviteit. Raadpleeg voor meer informatie het [ExpressRoute-locaties](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) en [prijzen voor ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="two-expressroute-circuits-in-two-different-expressroute-peering-locations"></a>Twee ExpressRoute-circuits in twee verschillende ExpressRoute-peeringlocaties
-   Dit is handig als u wilt zorgen dat tegen uitval van het primaire ExpressRoute-circuit en grootschalige regionale rampen, die ook kunnen van invloed zijn op ExpressRoute-peeringlocaties en uw primaire ExpressRoute-circuit worden onderbroken.
-   Het circuit dat is verbonden met de productie-omgeving wordt normaal gesproken gebruikt als het primaire circuit en het secundaire circuit is een failsafe en doorgaans met lagere bandbreedte. De bandbreedte van de secundaire server worden in een noodgebeurtenis, wanneer de secundaire als de primaire overnemen moet verhoogd.
-   Met deze configuratie kunt u maken verbindingen van uw secundaire ExpressRoute-circuit met het virtuele netwerk na een failover of de verbindingen tot stand gebrachte en gereed is voor een verklaring na noodgevallen hebben uw algehele hersteltijd te verminderen. Met gelijktijdige verbindingen met zowel de primaire en doelregio virtuele netwerken, zorg ervoor dat uw on-premises routering gebruikmaakt van de secundaire circuit en de verbinding alleen na een failover.
-   De bron- en virtuele netwerken voor virtuele machines die zijn beveiligd met Site Recovery kunnen de hetzelfde of een ander IP-adressen na een failover per uw nodig hebben. In beide gevallen kunnen de secundaire verbindingen worden gemaakt voordat u een failover.

### <a name="two-expressroute-circuits-in-the-same-expressroute-peering-location"></a>Twee ExpressRoute-circuits in de dezelfde locatie van de ExpressRoute-peering
-   Met deze configuratie kunt u ervoor kunt zorgen tegen fouten in het primaire ExpressRoute-circuit, maar niet beschermd tegen grootschalige regionale rampen, dit kan gevolgen hebben voor ExpressRoute-peeringlocaties. Met de laatste, kunnen de primaire en secundaire circuits beïnvloed.
-   De overige voorwaarden voor IP-adressen en verbindingen blijven hetzelfde als die in het vorige geval. U kunt gelijktijdige verbindingen van on-premises datacenter naar de bron van virtueel netwerk met het primaire circuit en met het virtuele netwerk met het secundaire circuit hebben. Met gelijktijdige verbindingen met zowel de primaire en doelregio virtuele netwerken, zorg ervoor dat uw on-premises routering gebruikmaakt van de secundaire circuit en de verbinding alleen na een failover.
-   U kunt beide circuits geen verbinding met hetzelfde virtuele netwerk wanneer circuits zijn gemaakt op dezelfde locatie peering.

### <a name="single-expressroute-circuit"></a>Één ExpressRoute-circuit
-   Deze configuratie wordt niet ervoor gezorgd op basis van een grootschalige regionaal noodgeval, die invloed kan hebben op de locatie van ExpressRoute-peering.
-   U kunt geen met een enkel ExpressRoute-circuit, verbinding maken met de bron en doel van virtuele netwerken tegelijk aan circuit als dezelfde IP-adresruimte wordt gebruikt voor de doelregio.
-   Wanneer de dezelfde IP-adresruimte wordt gebruikt in de doelregio, verbinding met de gegevensbron aan clientzijde moet worden verbroken en het doel side-verbinding tot stand gebracht daarna. Deze wijziging verbinding kan scripts worden gebruikt als onderdeel van een plan voor herstel.
-   In een regionale storing optreedt, als de primaire regio niet toegankelijk is, is kan de verbinding verbreken-bewerking mislukken. Dergelijke een storing kan van invloed zijn op het maken van verbinding naar de doelregio wanneer dezelfde IP-adresruimte wordt gebruikt op het virtuele doelnetwerk.
-   Als het maken van verbinding is geslaagd op het doel en de primaire regio later herstelt, kunt u pakket val geconfronteerd als twee gelijktijdige verbindingen probeert verbinding maken met dezelfde-adresruimte. Om te voorkomen dat pakket val, moet de primaire verbinding onmiddellijk worden beëindigd. Failback van virtuele machines naar de primaire regio, de primaire verbinding kan opnieuw bericht na het verbreken van de secundaire verbinding tot stand worden gebracht.
-   Als andere adresruimte op het virtuele netwerk wordt gebruikt, kunt klikt u vervolgens u tegelijk verbinding maken met de bron en doel van de virtuele netwerken van hetzelfde ExpressRoute-circuit.

## <a name="recovering-azure-deployments"></a>Herstellen van Azure-implementaties
Houd rekening met het failover-model met twee verschillende ExpressRoute-circuits in twee verschillende peeringlocaties, en de retentie van privé IP-adressen voor de beveiligde virtuele machines van Azure. De doelregio voor herstel is Azure Zuidoost-Azië en een secundaire ExpressRoute-circuit-verbinding tot stand is gebracht via een partner edge in Singapore.

Voor het automatiseren van herstel van de volledige implementatie, naast het repliceren van virtuele machines en virtuele netwerken, moeten andere relevante bronnen voor netwerken en verbindingen ook worden gemaakt. Voor de eerdere hub en spoke topologie van de volgende aanvullende stappen dienen te worden genomen tijdens of na de [failover](azure-to-azure-tutorial-failover-failback.md) bewerking:
1.  De Azure ExpressRoute-Gateway maken in de doel-regio hub-netwerk. De ExpressRoute-Gateway is vereist voor de doel-virtuele hub-netwerk verbinden met het ExpressRoute-circuit.
2.  Verbinding van het virtuele netwerk van de doel-virtuele hub-netwerk maken met de doel-ExpressRoute-circuit.
3.  Instellen van de VNet-peerings tussen de doelregio hub en spoke-netwerken. De eigenschappen van de peering in de doelregio is hetzelfde als die in de bronregio.
4.  Stel de udr's in de hub VNet en het knooppunt twee VNets. De eigenschappen van de doel-kant udr's zijn hetzelfde als die aan de bron wanneer met de dezelfde IP-adressen. Met een ander doel-IP-adressen, moeten de udr's dienovereenkomstig worden gewijzigd.

De bovenstaande stappen kunnen scripts worden gebruikt als onderdeel van een [herstelplan](site-recovery-create-recovery-plans.md). Afhankelijk van de connectiviteit van toepassingen en de vereisten voor het moment van herstel, kunnen ook de bovenstaande stappen worden uitgevoerd voordat de failover wordt gestart.

Boeken van het herstel van de virtuele machines en de voltooiing van de andere stappen van de connectiviteit, de Herstelomgeving ziet er als volgt uit: ![On-premises-to-Azure met ExpressRoute na een Failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)

Een voorbeeld van een eenvoudige topologie voor noodherstel van Azure-VM met één ExpressRoute-circuit met hetzelfde IP-adres op de virtuele doelmachines is gedetailleerde [hier](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity).

## <a name="recovery-time-objective-rto-considerations"></a>Overwegingen voor herstel tijd beoogde hersteltijd (RTO)
Om te beperken de algehele hersteltijd voor uw implementatie, wordt aangeraden inrichten en implementeren van de aanvullende doelregio [netwerkonderdelen](azure-vm-disaster-recovery-with-expressroute.md#enabling-replication-for-the-deployment) zoals vooraf virtuele netwerkgateways. Een kleine uitvaltijd is gekoppeld aan het implementeren van extra resources en deze uitvaltijd kan invloed hebben op de algehele hersteltijd, als dat niet verwerkt tijdens de planning.

Het beste uitvoeren regular [noodhersteloefeningen](azure-to-azure-tutorial-dr-drill.md) voor beveiligde implementaties. Een analyse valideert uw replicatiestrategie zonder gegevensverlies of uitvaltijd en heeft geen invloed op uw productieomgeving. Een detailanalyse uitvoeren de laatste minuut configuratiekwesties waarmee u negatieve invloed kunnen hebben op de beoogde hersteltijd ook voorkomt. Het is raadzaam om met behulp van een afzonderlijke Azure-VM-netwerk voor de testfailover, in plaats van het standaardnetwerk dat is opgezet toen u replicatie inschakelde.

Als u een enkel ExpressRoute-circuit, wordt u aangeraden een ander IP-adresruimte voor het virtuele netwerk verbinding tot stand brengen om problemen te vermijden tijdens regionale rampen. Als u met behulp van verschillende IP-adressen is niet haalbaar voor de herstelde productie-omgeving, moet de disaster recovery inzoomen test-failover worden uitgevoerd op een afzonderlijke testnetwerk met verschillende IP-adressen als u twee virtuele netwerken kan niet verbinden met overlappende IP adresruimte aan hetzelfde ExpressRoute-circuit.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [ExpressRoute-circuits](../expressroute/expressroute-circuit-peerings.md).
- Meer informatie over [ExpressRoute-Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Meer informatie over [ExpressRoute-locaties](../expressroute/expressroute-locations.md).
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover wilt automatiseren.
