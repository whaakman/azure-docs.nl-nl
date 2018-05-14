---
title: Hiervoor wordt ExpressRoute gebruikt met virtuele machine van Azure-noodherstel | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure ExpressRoute gebruiken met Azure Site Recovery voor noodherstel van de virtuele machine van Azure
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 44ecbcc51cb53f4d7b68f5c5e24e7d81c5a4208c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="using-expressroute-with-azure-virtual-machine-disaster-recovery"></a>Hiervoor wordt ExpressRoute gebruikt met virtuele machine van Azure-noodherstel

Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Dit artikel wordt beschreven hoe u ExpressRoute met Site Recovery kunt gebruiken voor herstel na noodgevallen van virtuele machines in Azure.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u begrijpt voordat u begint:
-   ExpressRoute [circuits](../expressroute/expressroute-circuit-peerings.md)
-   ExpressRoute [Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
-   Virtuele machine van Azure [replicatie-architectuur](azure-to-azure-architecture.md)
-   [Replicatie in te stellen](azure-to-azure-tutorial-enable-replication.md) voor Azure virtual machines
-   [Failover](azure-to-azure-tutorial-failover-failback.md) virtuele machines in Azure

## <a name="expressroute-and-azure-virtual-machine-replication"></a>ExpressRoute- en Azure replicatie voor virtuele machines

Bij de beveiliging met Site Recovery virtuele Azure-machines, gegevens van replicatie wordt verzonden naar een Azure Storage-account of replica beheerd schijf op de doel-Azure-regio, afhankelijk van of uw Azure virtual machines gebruik [Azure beheerd schijven](../virtual-machines/windows/managed-disks-overview.md). Hoewel de replicatie-eindpunten openbaar zijn, wordt in replicatieverkeer voor replicatie van de virtuele machine in Azure, standaard Internet, ongeacht welke Azure-regio het virtuele netwerk van de bron bestaat in niet passeren.

Voor noodherstel van de virtuele machine in Azure, zoals gegevens van replicatie de grens Azure laat is ExpressRoute niet vereist voor replicatie. Nadat de virtuele machines, failover met de doel-Azure-regio, kunt u deze kunt openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

## <a name="replicating-azure-deployments"></a>Implementaties van Azure repliceren

Een eerdere [artikel](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity), beschreven eenvoudige configuratie met één virtueel Azure-netwerk is verbonden met de klant on-premises datacentrum via ExpressRoute. Typische bedrijfsimplementaties workloads verdelen over meerdere virtuele Azure-netwerken hebben en een centrale connectiviteit hub stelt de externe verbinding, met het Internet en on-premises implementaties.

In dit voorbeeld een hub en spoke-topologie algemene zakelijke implementaties wordt beschreven:
-   De implementatie is in de **Azure Oost-Azië** regio en de on-premises datacentrum heeft een ExpressRoute-circuit-verbinding via een partner-edge in Hongkong.
-   Toepassingen zijn geïmplementeerd op twee spoke virtuele netwerken – **bron VNet1** met adresruimte 10.1.0.0/24 en **bron VNet2** met adresruimte 10.2.0.0/24.
-   Het virtuele netwerk hub **bron Hub VNet**, met de adresruimte 10.10.10.0/24 fungeert als de poortwachter. Alle communicatie naar verschillende subnetten gaat via de hub.
-   Het virtuele netwerk hub heeft twee subnetten: **NVA Subnet** met adresruimte 10.10.10.0/25 en **Gatewaysubnet** met adresruimte 10.10.10.128/25.
-   De **NVA subnet** heeft een virtueel netwerkapparaat met IP-adres 10.10.10.10.
-   De **Gatewaysubnet** heeft een ExpressRoute-gateway is verbonden met een ExpressRoute-verbinding naar de klant on-premises datacentrum via een persoonlijke Peering routeringsdomein worden doorgestuurd.
-   Elke spoke virtueel netwerk is verbonden met het virtuele netwerk hub en alle routering binnen deze netwerktopologie wordt beheerd via Azure Route tabellen (UDR). Alle uitgaande verkeer van een VNet naar het andere VNet of naar de on-premises datacentrum doorgestuurd via de NVA.

![Op lokale-die naar Azure met ExpressRoute voor Failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

### <a name="hub-and-spoke-peering"></a>Hub en spoke-peering

De spoke op hub van de peering heeft de volgende configuratie:
-   Virtueel netwerkadres toestaan: ingeschakeld
-   Toestaan van doorgestuurd verkeer: ingeschakeld
-   Gateway-doorvoer toestaan: uitgeschakeld
-   Gebruik gateways verwijderen: ingeschakeld

 ![Spaak aan peeringconfiguratie hub](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

De hub en spaak peering heeft de volgende configuratie:
-   Virtueel netwerkadres toestaan: ingeschakeld
-   Toestaan van doorgestuurd verkeer: ingeschakeld
-   Gateway-doorvoer toestaan: ingeschakeld
-   Gebruik gateways verwijderen: uitgeschakeld

 ![Hub en spaak peeringconfiguratie](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="enabling-replication-for-the-deployment"></a>Inschakelen van replicatie voor de implementatie

Voor de installatie van de bovenstaande eerste [instellen van herstel na noodgevallen](azure-to-azure-tutorial-enable-replication.md) voor elke virtuele machine met Site Recovery. Site Recovery kan de replica op de doelregio virtuele netwerken (inclusief subnetten en gatewaysubnetten) maken en de vereiste toewijzingen maken tussen de bron- en virtuele netwerken. U kunt ook vooraf maken de doelnetwerken aan clientzijde en subnetten en gebruik van dezelfde tijdens het inschakelen van replicatie.

Site Recovery wordt niet gerepliceerd routetabellen, gateways voor virtueel netwerk, gatewayverbindingen virtueel netwerk, virtueel netwerk peering, of geen andere netwerken resources of verbindingen. Deze en andere resources die geen deel uit van de [replicatieproces](azure-to-azure-architecture.md#replication-process) moeten worden gemaakt tijdens of vóór de failover en verbonden met de relevante bronnen. U kunt Azure Site Recovery krachtige [herstelplannen](site-recovery-create-recovery-plans.md) automatiseren maken en aanvullende bronnen met behulp van automatiseringsscripts te verbinden.

Standaard laat het replicatieverkeer niet de grens van Azure. Normaal gesproken definiëren NVA implementaties ook een standaardroute (0.0.0.0/0) dat ervoor zorgt uitgaand internetverkeer dat doorstromen de NVA. In dit geval kan het toestel ophalen beperkt als het replicatieverkeer de NVA passeert. Hetzelfde geldt ook wanneer standaardroutes voor de routering van alle verkeer van de virtuele machine van Azure naar on-premises implementaties. Het is raadzaam [maken van een service-eindpunt van het virtuele netwerk](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in uw virtuele netwerk voor 'Opslag' zodat het replicatieverkeer niet Azure grens laat.

## <a name="failover-models-with-expressroute"></a>Failover-modellen met ExpressRoute

Wanneer virtuele machines in Azure wordt een failover uitgevoerd in een andere regio, is de bestaande ExpressRoute-verbinding met het virtuele netwerk van de bron niet automatisch overgebracht naar het virtuele doelnetwerk van de regio van het herstel. Een nieuwe verbinding is vereist voor ExpressRoute verbinding met het virtuele netwerk.

U kunt Azure virtuele machines repliceren naar een Azure-regio in hetzelfde geografische cluster zoals beschreven [hier](azure-to-azure-support-matrix.md#region-support). Als het gekozen doel-Azure-regio niet binnen dezelfde geopolitieke regio als de bron is, moet u ExpressRoute Premium inschakelen als u één ExpressRoute-circuit regio connectiviteit van de bron en doel. Raadpleeg voor meer details [ExpressRoute-locaties](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) en [ExpressRoute prijzen](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="two-expressroute-circuits-in-two-different-expressroute-peering-locations"></a>Twee ExpressRoute-circuits in twee verschillende ExpressRoute-peeringlocaties
-   Dit is handig als u wilt zorgen dat tegen uitval van het primaire ExpressRoute-circuit en grootschalige regionale noodsituaties, die ook kunnen ExpressRoute-peeringlocaties invloed en uw primaire ExpressRoute-circuit verstoren.
-   Het circuit dat is verbonden met de productie-omgeving wordt normaal gesproken gebruikt als het primaire circuit en de secundaire circuit is een failsafe en meestal met lagere bandbreedte. De bandbreedte van de secundaire kan in een gebeurtenis na noodgevallen, wanneer de secundaire als de primaire overnemen moet worden verhoogd.
-   Met deze configuratie kunt u instellen verbindingen van uw secundaire ExpressRoute-circuit met het virtuele netwerk na de failover of de verbindingen tot stand gebrachte en gereed is voor een declaratie na noodgevallen voor de algehele hersteltijd verminderen. Zorg ervoor dat uw on-premises routering gebruikmaakt van de secundaire circuit en de verbinding na een failover met gelijktijdige verbindingen met zowel de primaire en doel regio virtuele netwerken.
-   De bron- en virtuele netwerken voor virtuele machines die zijn beveiligd met Site Recovery kunnen de dezelfde of verschillende IP-adressen hebben bij een failover per uw behoeften. In beide gevallen kunnen de secundaire verbindingen worden gemaakt voordat er failover.

### <a name="two-expressroute-circuits-in-the-same-expressroute-peering-location"></a>Twee ExpressRoute-circuits in de dezelfde locatie van de ExpressRoute-peering
-   Met deze configuratie kunt u ervoor kunt zorgen tegen fouten in het primaire ExpressRoute-circuit, maar niet tegen grootschalige regionale noodsituaties, dit kan invloed hebben op ExpressRoute-peeringlocaties. Met deze laatste kunnen de primaire en secundaire circuits ophalen beïnvloed.
-   De overige voorwaarden voor IP-adressen en verbindingen blijven hetzelfde als die in de eerdere aanvraag. U kunt gelijktijdige verbindingen van on-premises datacentrum bron virtueel netwerk met het primaire circuit en met het virtuele netwerk met de secundaire circuit hebben. Zorg ervoor dat uw on-premises routering gebruikmaakt van de secundaire circuit en de verbinding na een failover met gelijktijdige verbindingen met zowel de primaire en doel regio virtuele netwerken.
-   U kunt beide circuits kan geen verbinding met hetzelfde virtuele netwerk wanneer circuits zijn gemaakt op dezelfde locatie peering.

### <a name="single-expressroute-circuit"></a>Één ExpressRoute-circuit
-   Deze configuratie biedt geen verzekering tegen een grootschalige regionale ramp, dit kan invloed hebben op de locatie van ExpressRoute-peering.
-   Met een enkel ExpressRoute-circuit kan u verbinding maken met de bron en doel van virtuele netwerken tegelijk circuit als dezelfde IP-adresruimte wordt gebruikt voor de doelregio.
-   Wanneer u dezelfde IP-adresruimte wordt gebruikt op de doelregio, side verbinding met de gegevensbron moet worden beëindigd en de doel-side-verbinding tot stand gebracht daarna. Deze wijziging verbinding kan scripts worden gebruikt als onderdeel van een herstelplan.
-   In een regionale fout als de primaire regio niet toegankelijk is, is kan de verbinding verbreken mislukken. Dergelijke een storing aanzienlijke invloed kunnen hebben de doelregio verbinding maken wanneer dezelfde IP-adresruimte op doel virtueel netwerk wordt gebruikt.
-   Als het maken van verbinding is geslaagd op het doel en de primaire regio later herstelt, kunt u pakket verwijdert geconfronteerd als twee gelijktijdige verbindingen proberen te verbinden met dezelfde adresruimte. Om te voorkomen dat een pakket verwijdert, moet de primaire verbinding is onmiddellijk worden beëindigd. Post failback van virtuele machines naar de primaire regio, de primaire verbinding kan opnieuw na het verbreken van de secundaire verbinding tot stand worden gebracht.
-   Als andere adresruimte wordt gebruikt voor het virtuele netwerk, vervolgens tegelijk verbinding maken met de bron en doel van de virtuele netwerken van hetzelfde ExpressRoute-circuit.

## <a name="recovering-azure-deployments"></a>Herstellen van de Azure-implementaties
Houd rekening met het failover-model met twee verschillende ExpressRoute-circuits in twee verschillende peeringlocaties en retentie van privé IP-adressen voor de beveiligde virtuele machines in Azure. De doelregio voor herstel is Azure Zuidoost-Azië, en een secundaire ExpressRoute-circuit-verbinding tot stand is gebracht via een partner-edge in Singapore.

Voor het automatiseren van herstel van de volledige implementatie, naast het repliceren van virtuele machines en virtuele netwerken, moeten andere relevante bronnen voor netwerken en verbindingen ook worden gemaakt. Voor de eerdere hub en spoke netwerktopologie de volgende aanvullende stappen moeten worden ondernomen tijdens of na de [failover](azure-to-azure-tutorial-failover-failback.md) bewerking:
1.  De Azure ExpressRoute-Gateway maken in het virtuele netwerk voor doel regio hub. De ExpressRoute-Gateway is vereist voor de doel-hub virtueel netwerk verbinden met het ExpressRoute-circuit.
2.  Verbinding van het virtuele netwerk van het virtuele netwerk hub maken met de doel-ExpressRoute-circuit.
3.  Instellen van de VNet-peerings tussen de doelregio hub en spoke virtuele netwerken. De eigenschappen van de peering op de doelregio is hetzelfde als die op de bron-regio.
4.  Stel de udr's in de hub VNet en de twee spoke VNets. De eigenschappen van de doel-zijde udr's zijn hetzelfde als die aan de bronkant wanneer met de dezelfde IP-adressen. Met andere doel-IP-adressen, moeten de udr's dienovereenkomstig worden gewijzigd.

De bovenstaande stappen kunnen scripts worden gebruikt als onderdeel van een [herstelplan](site-recovery-create-recovery-plans.md). Afhankelijk van de connectiviteit van toepassingen en herstel Tijdvereisten kunnen ook de bovenstaande stappen worden uitgevoerd voordat u start de failover.

Post het herstel van de virtuele machines en de voltooiing van de andere stappen voor de connectiviteit, de Herstelomgeving er als volgt uitziet: ![op lokale-die naar Azure met ExpressRoute na een Failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)

Er is een voorbeeld van een eenvoudige topologie voor noodherstel van de virtuele machine van Azure met één ExpressRoute-circuit met dezelfde IP-adres op de virtuele doelmachines, gedetailleerde [hier](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity).

## <a name="recovery-time-objective-rto-considerations"></a>Overwegingen voor herstel tijd Objective (RTO)
Als u de algehele hersteltijd voor uw implementatie, het is raadzaam inrichten en implementeren van de aanvullende doelregio [netwerkonderdelen](azure-vm-disaster-recovery-with-expressroute.md#enabling-replication-for-the-deployment) zoals virtuele netwerkgateways tevoren. Een kleine uitvaltijd is gekoppeld aan de implementatie van aanvullende bronnen en deze uitvaltijd kan invloed hebben op de algehele hersteltijd als ze niet worden verwerkt tijdens de planning.

Het is raadzaam regular uitgevoerd [noodhersteloefeningen](azure-to-azure-tutorial-dr-drill.md) voor beveiligde implementaties. Een detailanalyse valideert van uw replicatiestrategie voor zonder verlies van gegevens of uitvaltijd en niet van invloed op uw productieomgeving. Een detailanalyse uitgevoerd voorkomt laatste configuratiekwesties waarmee u een negatieve invloed beoogde hersteltijd hebben kunnen ook. U wordt aangeraden met behulp van een afzonderlijke Azure VM-netwerk voor de testfailover, in plaats van het standaardnetwerk dat is ingesteld wanneer u replicatie is ingeschakeld.

Als u één ExpressRoute-circuit, wordt u aangeraden een ander IP-adresruimte voor het virtuele doelnetwerk gebruiken om te voorkomen dat de verbinding tot stand brengen van problemen tijdens de regionale noodsituaties. Als u met behulp van verschillende IP-adressen is niet haalbaar voor de herstelde productieomgeving, moet de testfailover disaster recovery Inzoomen op een afzonderlijke testnetwerk met verschillende IP-adressen worden gedaan als u twee virtuele netwerken kan niet verbinden met overlappende IP adresruimte aan hetzelfde ExpressRoute-circuit.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [ExpressRoute-circuits](../expressroute/expressroute-circuit-peerings.md).
- Meer informatie over [ExpressRoute-Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Meer informatie over [ExpressRoute-locaties](../expressroute/expressroute-locations.md).
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover te automatiseren.
