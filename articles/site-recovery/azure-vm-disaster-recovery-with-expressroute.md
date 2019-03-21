---
title: Integratie van Azure ExpressRoute met herstel na noodgevallen voor Azure VM's met behulp van de Azure Site Recovery-service | Microsoft Docs
description: Hierin wordt beschreven hoe u herstel na noodgevallen instelt voor virtuele Azure-machines met behulp van Azure Site Recovery en Azure ExpressRoute
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 4622809f0e261236d6753daf5bb2e00ff814c849
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087870"
---
# <a name="integrate-azure-expressroute-with-disaster-recovery-for-azure-vms"></a>Integratie van Azure ExpressRoute met herstel na noodgevallen voor Azure VM 's


Dit artikel wordt beschreven hoe u Azure ExpressRoute met integreert [Azure Site Recovery](site-recovery-overview.md), bij het instellen van herstel na noodgevallen voor Azure VM's naar een secundaire Azure-regio.

Site Recovery maakt herstel na noodgeval voor virtuele Azure-machines door Azure VM-gegevens te repliceren naar Azure.

- Als Azure-VM's met [Azure beheerde schijven](../virtual-machines/windows/managed-disks-overview.md), VM-gegevens worden gerepliceerd naar een gerepliceerde beheerde schijf in de secundaire regio.
- Als Azure virtuele machines geen beheerde schijven gebruiken, wordt de VM-gegevens gerepliceerd naar Azure storage-account.
- Replicatie-eindpunten openbaar zijn, maar niet via het internet gaat replicatieverkeer voor Azure VM's.

ExpressRoute kunt u on-premises netwerken in de Microsoft Azure-cloud uitbreiden via een persoonlijke verbinding, die wordt gefaciliteerd door een connectiviteitsprovider. Als u ExpressRoute geconfigureerd hebt, deze kan worden geïntegreerd met Site Recovery als volgt:

- **Tijdens de replicatie tussen Azure-regio's**: Replicatieverkeer voor noodherstel van Azure-VM is alleen in Azure, en ExpressRoute niet nodig is of wordt gebruikt voor replicatie. Als u verbinding vanaf een on-premises site naar de Azure VM's in de primaire site voor Azure maakt, zijn er echter een aantal problemen rekening mee moet houden wanneer u van herstel na noodgevallen voor deze virtuele machines van Azure instellen bent.
- **Failover tussen Azure-regio's**: Als er uitval optreedt, schakelt u over Azure-VM's van de primaire naar secundaire Azure-regio. Na de failover wordt uitgevoerd naar een secundaire regio zijn er een aantal stappen doen om toegang te krijgen tot de Azure VM's in de secundaire regio met behulp van ExpressRoute.


## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint, zorg ervoor dat de volgende concepten:

- ExpressRoute [circuits](../expressroute/expressroute-circuit-peerings.md)
- ExpressRoute [Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#routingdomains)
- ExpressRoute [locaties](../expressroute/expressroute-locations.md).
- Azure-VM [-replicatiearchitectuur](azure-to-azure-architecture.md)
- Hoe u [instellen van replicatie](azure-to-azure-tutorial-enable-replication.md) voor virtuele Azure-machines.
- Hoe u [failover](azure-to-azure-tutorial-failover-failback.md) virtuele Azure-machines.


## <a name="general-recommendations"></a>Algemene aanbevelingen

Voor de beste praktijken, en om ervoor te zorgen efficiënte Recovery Time Objectives (RTO's) voor herstel na noodgevallen, raden we dat u het volgende doen bij het instellen van Site Recovery om te integreren met ExpressRoute:

- Netwerkonderdelen voordat de failover naar een secundaire regio inrichten:
    - Wanneer u replicatie voor virtuele Azure-machines inschakelt, kunnen netwerkresources zoals netwerken, subnetten en gateways in de Azure-doelregio, afhankelijk van instellingen voor gegevensbron netwerk automatisch door Site Recovery implementeren.
    - Site Recovery instellen niet automatisch van netwerkresources, zoals de VNet-gateways.
    - U wordt aangeraden dat u deze extra netwerkresources voordat de failover wordt inrichten. Een kleine uitvaltijd is gekoppeld aan deze implementatie en het kan gevolgen hebben voor de algehele hersteltijd, als u niet hebt voor het account tijdens het plannen van de implementatie.
- Reguliere noodhersteloefeningen uitvoeren:
    - Een analyse valideert uw replicatiestrategie zonder gegevensverlies of uitvaltijd, en is niet van invloed op uw productieomgeving. Het helpt te voorkomen dat de laatste minuut configuratieproblemen die nadelige gevolgen van RTO hebben kunnen.
    - Wanneer u een testfailover uitvoeren voor de analyse uitgevoerd, raden wij aan dat u een afzonderlijke Azure-VM-netwerk, in plaats van het standaardnetwerk dat ingesteld wanneer u replicatie inschakelt.
- Gebruik verschillende IP-adresruimten als er een enkel ExpressRoute-circuit.
    - U wordt aangeraden dat u een ander IP-adresruimte voor het virtuele netwerk gebruiken. Hiermee voorkomt u problemen bij het maken van verbindingen tijdens een regionale onderbreking.
    - Als u niet een afzonderlijke adresruimte gebruiken, zorg er dan voor dat het uitvoeren van de testfailover disaster recovery Inzoomen op een afzonderlijke testnetwerk met verschillende IP-adressen. U kunt geen twee VNets verbinden met overlappende IP-adresruimte aan hetzelfde ExpressRoute-circuit.

## <a name="replicate-azure-vms-when-using-expressroute"></a>Virtuele Azure-machines repliceren bij het gebruik van ExpressRoute


Als u wilt instellen van replicatie voor virtuele Azure-machines in een primaire site en u verbinding met deze VM's van uw on-premises site via ExpressRoute maakt, is dit wat u moet doen:

1. [Replicatie inschakelen](azure-to-azure-tutorial-enable-replication.md) voor elke Azure-VM.
2. Eventueel kunt instellen Site Recovery netwerken:
    - Wanneer u configureren en inschakelen van replicatie, stelt Site Recovery u netwerken, subnetten en gateway-subnetten in de doel-Azure-regio, zodat deze overeenkomen met die in de bronregio. Site Recovery wordt ook aangegeven tussen de bron- en virtuele netwerken.
    - Als u niet dat Site Recovery om dit te doen automatisch wilt, de doelzijde netwerkresources maken voordat u replicatie inschakelt.
3. Andere netwerkelementen maken:
    - Site Recovery maakt geen routetabellen, VNet-gateways, verbindingen voor VNet-gateway, VNet-peering, of andere netwerken resources en -verbindingen in de secundaire regio.
    - U moet maken van deze extra Netwerkelementen in de secundaire regio, elk gewenst moment voordat u een failover uitvoert van de primaire regio.
    - U kunt [herstelplannen](site-recovery-create-recovery-plans.md) en netwerkresources automatiseringsscripts instellen en deze verbinding te maken.
1. Als u een virtueel netwerkapparaat (NVA hebt) geïmplementeerd voor het beheren van de stroom van het netwerkverkeer, houd er rekening mee dat:
    - Azure standaardroute systeem voor Azure VM-replicatie is 0.0.0.0/0.
    - Normaal gesproken definiëren NVA implementaties ook een standaardroute (0.0.0.0/0) die ervoor zorgt uitgaand internetverkeer dat naar flow via het NVA. De standaardroute wordt gebruikt wanneer er geen andere specifieke route-configuratie kan worden gevonden.
    - Als dit het geval is, is de NVA mogelijk overbelast als alle replicatieverkeer wordt doorgegeven via de NVA.
    - Deze beperking geldt ook wanneer standaardroutes voor de routering van verkeer van alle virtuele machine van Azure naar on-premises implementaties.
    - In dit scenario raden wij aan dat u [maken van een service-eindpunt van het netwerk](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in uw virtuele netwerk voor de Microsoft.Storage-service, zodat het replicatieverkeer niet Azure grens achterlaten.

## <a name="replication-example"></a>Voorbeeld van de replicatie

Enterprise-implementaties hebben meestal verdeeld over meerdere Azure VNets, met een hub centrale connectiviteit voor externe verbinding met internet en sites die on-premises workloads. Een hub en spoke-topologie wordt meestal gebruikt samen met ExpressRoute.

![On-premises-to-Azure met ExpressRoute voordat de failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

- **Regio**. Apps zijn geïmplementeerd in de regio Oost-Azië van Azure.
- **Knooppunt vNets**. Apps worden geïmplementeerd in twee spoke-vNets:
    - **Bron vNet1**: 10.1.0.0/24.
    - **Bron vNet2**: 10.2.0.0/24.
    - Elk knooppunt virtueel netwerk is verbonden met **Hub vNet**.
- **Hub vNet**. Er is een hub vNet **bron Hub vNet**: 10.10.10.0/24.
  - Dit vNet hub fungeert als de gatekeeper.
  - Alle communicatie tussen subnetten zijn doorlopen van deze hub.
    - Hub vNet subnetten **. De hub vNet heeft twee subnetten:
    - **NVA-subnet**: 10.10.10.0/25. Dit subnet bevat een NVA (10.10.10.10).
    - **Gatewaysubnet**: 10.10.10.128/25. Dit subnet bevat een ExpressRoute-gateway verbonden met een ExpressRoute-verbinding die u routes naar de on-premises site via een persoonlijke peering routeringsdomein.
- Het on-premises datacenter heeft een ExpressRoute-circuit-verbinding via een partner edge in Hong Kong SAR.
- Alle routering wordt geregeld via de Azure-routetabellen (UDR).
- Al het uitgaande verkeer tussen vNets of het on-premises datacenter wordt doorgestuurd via de NVA.

### <a name="hub-and-spoke-peering-settings"></a>Hub en spoke-instellingen voor peering

#### <a name="spoke-to-hub"></a>Spoke naar hub

**Richting** | **Instelling** | **status**
--- | --- | ---
Spoke naar hub | Adres van het virtuele netwerk toestaan | Ingeschakeld
Spoke naar hub | Doorgestuurd verkeer toestaan | Ingeschakeld
Spoke naar hub | Gatewayoverdracht toestaan | Uitgeschakeld
Spoke naar hub | Remove-gateways gebruiken | Ingeschakeld

 ![Knooppunt naar hub peering-configuratie](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

#### <a name="hub-to-spoke"></a>Hub naar spoke

**Richting** | **Instelling** | **status**
--- | --- | ---
Hub naar spoke | Adres van het virtuele netwerk toestaan | Ingeschakeld
Hub naar spoke | Doorgestuurd verkeer toestaan | Ingeschakeld
Hub naar spoke | Gatewayoverdracht toestaan | Ingeschakeld
Hub naar spoke | Remove-gateways gebruiken | Uitgeschakeld

 ![Hub en een peeringconfiguratie knooppunt](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="example-steps"></a>Voorbeelden van stappen

In ons voorbeeld moet het volgende gebeuren wanneer het inschakelen van replicatie voor virtuele Azure-machines in het Bronnetwerk:

1. U [inschakelen replicatie](azure-to-azure-tutorial-enable-replication.md) voor een virtuele machine.
2. Site Recovery maakt replica vnet's, subnetten en gateway-subnetten in de doelregio.
3. Toewijzingen tussen de bronnetwerken en de replica-doelnetwerken die wordt gemaakt, maakt site Recovery.
4. U maken handmatig voor virtuele netwerkgateways, virtuele netwerkgatewayverbindingen, peering op virtueel netwerk, of een andere netwerken bronnen of verbindingen.


## <a name="fail-over-azure-vms-when-using-expressroute"></a>Failover van virtuele machines van Azure bij het gebruik van ExpressRoute

Nadat u een failover Azure-VM's met de doel-Azure-regio met behulp van Site Recovery, kunt u ze openen met behulp van ExpressRoute [privépeering](../expressroute/expressroute-circuit-peerings.md#privatepeering).

- U moet verbinding maken met ExpressRoute met de doel-vNet met een nieuwe verbinding. De bestaande ExpressRoute-verbinding niet automatisch doorgegeven.
- De manier waarop u uw ExpressRoute-verbinding met het doel-vNet hebt ingesteld, is afhankelijk van de topologie van uw ExpressRoute.


### <a name="access-with-two-circuits"></a>Toegang met twee circuits

#### <a name="two-circuits-with-two-peering-locations"></a>Twee circuits met twee peeringlocaties

Deze configuratie beveiligt ExpressRoute-circuits tegen regionale na noodgevallen. Als uw primaire locatie uitvalt, worden verbindingen kunnen blijven van de andere locatie.

- Het circuit dat is verbonden met de productie-omgeving is meestal de primaire. Het secundaire circuit heeft meestal een lagere bandbreedte, die kan worden verhoogd als zich een noodgeval voordoet.
- U kunt verbindingen van de secundaire ExpressRoute-circuit met de doel-vNet maken na een failover. U kunt ook verbindingen instellen en in geval van nood, klaar om te beperken van de algehele hersteltijd hebben.
- Zorg dat uw on-premises routering alleen gebruikmaakt van de secundaire circuit en de verbinding na een failover met gelijktijdige verbindingen met zowel de primaire en doel-vnet's.
- De bron en doel-vNets kunnen worden ontvangen van nieuwe IP-adressen of behouden dezelfde die, na een failover. In beide gevallen kunnen de secundaire verbindingen worden gemaakt voordat u een failover.


#### <a name="two-circuits-with-single-peering-location"></a>Twee circuits met één locatie

Deze configuratie helpt te beschermen tegen fouten in het primaire ExpressRoute-circuit, maar niet als de één locatie van de ExpressRoute-peering uitgeschakeld wordt, van invloed op beide circuits.

- U kunt gelijktijdige verbindingen van het on-premises datacenter met bron vNEt met het primaire circuit, en met de doel-vNet met het secundaire circuit hebben.
- Zorg ervoor dat on-premises alleen routering maakt gebruik van de secundaire circuit en de verbinding na een failover met gelijktijdige verbindingen met primaire en doel.
-   U kunt beide circuits geen verbinding met hetzelfde vNet wanneer circuits zijn gemaakt op dezelfde locatie peering.

### <a name="access-with-a-single-circuit"></a>Toegang met een enkel-circuit

In deze configuratie wordt slechts één Expressroute-circuit. Hoewel het circuit een redundante verbinding heeft in het geval een uitvalt, wordt een enkele route-circuit niet flexibiliteit bieden als uw peering regio uitvalt. Houd rekening met het volgende:

- U kunt virtuele Azure-machines repliceren naar een Azure-regio in de [dezelfde geografische locatie](azure-to-azure-support-matrix.md#region-support). Als het doel-Azure-regio is niet op dezelfde locatie als de bron, moet u ExpressRoute Premium inschakelen als u een enkel ExpressRoute-circuit. Meer informatie over [ExpressRoute-locaties](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) en [prijzen voor ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).
- U kan geen verbinding maken bron en doel-vnet's tegelijkertijd aan het circuit als dezelfde IP-adresruimte wordt gebruikt voor de doelregio. In dit scenario geldt het volgende:    
    -  Verbreken van verbinding met de gegevensbron aan clientzijde, en vervolgens de doel-kant-verbinding tot stand te brengen. Deze wijziging verbinding kan scripts worden gebruikt als onderdeel van een plan voor herstel van Site Recovery. Houd rekening met het volgende:
        - In een regionale storing optreedt, als de primaire regio niet toegankelijk is, is kan de verbinding verbreken-bewerking mislukken. Dit kan gevolgen voor het maken van verbinding naar de doelregio.
        - Als u de verbinding hebt gemaakt in de doelregio en primaire regio later herstelt, ervaren kunt u pakket val als twee gelijktijdige verbindingen probeert verbinding maken met dezelfde-adresruimte.
        - Om dit te voorkomen, moet u de primaire verbinding onmiddellijk beëindigd.
        - Nadat de virtuele machine failback naar de primaire regio, de primaire verbinding kan opnieuw worden gemaakt, nadat u de secundaire verbinding verbreken.
-   Als een andere adresruimten wordt gebruikt op de doel-vNet, kunt u tegelijkertijd naar de bron en doel-vnet's verbinden vanaf hetzelfde ExpressRoute-circuit.


## <a name="failover-example"></a>Voorbeeld van de failover

In ons voorbeeld gebruiken we de volgende topologie:

- Twee verschillende ExpressRoute-circuits in twee verschillende peeringlocaties.
- Privé-IP-adressen behouden voor de Azure VM's na een failover.
- De doelregio voor herstel is Azure Zuidoost-Azië.
- Een secundaire ExpressRoute-circuit-verbinding tot stand is gebracht via een partner edge in Singapore.

Voor een eenvoudige topologie die gebruikmaakt van een enkel ExpressRoute-circuit met hetzelfde IP-adres na een failover, [raadpleegt u dit artikel](site-recovery-retain-ip-azure-vm-failover.md#hybrid-resources-full-failover).

### <a name="example-steps"></a>Voorbeelden van stappen
Voor het automatiseren van herstel in deze hier volgt een voorbeeld van wat u moet doen:

1. Volg de stappen voor het instellen van replicatie.
2. [Failover van de Azure VM's](azure-to-azure-tutorial-failover-failback.md), met deze extra stappen tijdens of na de failover.

    a. De Azure ExpressRoute-Gateway maken in de doel-regio hub VNet. U moet dit het doel hub vNet verbinden met het ExpressRoute-circuit.

    b. De verbinding van het doel hub vNet maken met de doel-ExpressRoute-circuit.

    c. Instellen van de VNet-peerings tussen de doelregio hub en spoke-netwerken. De eigenschappen van de peering in de doelregio is hetzelfde als die in de bronregio.

    d. Stel de udr's in de hub VNet en het knooppunt twee VNets.

    - De eigenschappen van de doel-kant udr's zijn hetzelfde als die aan de bron wanneer met de dezelfde IP-adressen.
    - Met een ander doel-IP-adressen, moeten de udr's dienovereenkomstig worden gewijzigd.


De bovenstaande stappen kunnen scripts worden gebruikt als onderdeel van een [herstelplan](site-recovery-create-recovery-plans.md). Afhankelijk van de connectiviteit van toepassingen en de vereisten voor het moment van herstel, kunnen ook de bovenstaande stappen worden uitgevoerd voordat de failover wordt gestart.

#### <a name="after-recovery"></a>Na het herstel

Na het herstellen van de virtuele machines en voltooien van de connectiviteit, is betrouwbare omgeving voor het herstel als volgt uit.

![On-premises-to-Azure met ExpressRoute na een Failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)



## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van [herstelplannen](site-recovery-create-recovery-plans.md) app failover wilt automatiseren.
