---
title: Secure Azure Computing Architecture
description: Deze referentiearchitectuur voor een architectuur op ondernemingsniveau-DMZ maakt gebruik van network virtual appliances en andere hulpprogramma's. Deze architectuur is ontworpen om te voldoen aan het Ministerie van defensie van Secure Cloud Computing-architectuur functionele vereisten. Deze kan ook worden gebruikt voor elke organisatie. Deze referentie bevat twee geautomatiseerde opties die gebruikmaken van Citrix of F5 apparaten.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: 017a26d5672f666d4d8eaf629a0f53fe0cfe517f
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963232"
---
# <a name="secure-azure-computing-architecture"></a>Secure Azure Computing Architecture

V.S. Ministerie van defensie (DoD)-klanten die workloads in Azure implementeren hebben gevraagd om informatie voor de beveiligde virtuele netwerken configureren en instellen van de beveiligingsprogramma's en services die worden bepaald door het Amerikaanse ministerie van defensie standaarden en procedures. 

Defense Information System Agency (DISA) gepubliceerd de [Secure Cloud Computing-architectuur (SCCA) functionele vereisten Document (FRD)](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) in 2017. SCCA beschrijft de functionele doelstellingen voor het beveiligen van de Defense Information System van netwerk (DISN) en de commerciële cloud connection points van de provider. SCCA beschrijft ook hoe missie eigenaren van beveiligde toepassingen aan de grens van de verbinding in de cloud. Elke DoD-entiteit die verbinding met de commerciële cloud maakt moet de richtlijnen die zijn uiteengezet in de FRD SCCA volgen.
 
De SCCA bestaat uit vier onderdelen:
 
- Grens Cloud Access Point (BCAP)
- Virtueel Datacenter Security-Stack (VDSS)
- Virtueel Datacenter beheerde Service (VDM's)
- Referentiebeheer vertrouwde Cloud (TCCM) 

Er is een oplossing die voldoet aan de SCCA voor zowel IL4 en IL5 workloads die worden uitgevoerd in Azure ontwikkeld. Deze Azure-specifieke oplossing wordt de beveiligde Azure Computing-architectuur (SACA) genoemd. Klanten die SACA implementeren zijn in overeenstemming met de FRD SCCA. Klanten DoD-werkbelastingen verplaatsen naar Azure, nadat ze zijn verbonden kan worden ingeschakeld.

SCCA richtlijnen en architecturen zijn specifiek voor het Amerikaanse ministerie van defensie klanten, maar de meest recente wijzigingen aan SACA help civiele klanten te voldoen aan de richtlijnen voor vertrouwde internet verbinding (TIC). De meest recente wijzigingen kunnen ook commerciële klanten die voor het implementeren van een beveiligd DMZ ter bescherming van hun Azure-omgevingen.


## <a name="secure-cloud-computing-architecture-components"></a>Onderdelen van beveiligde Cloud Computing-architectuur

### <a name="bcap"></a>BCAP

Het doel van de BCAP is de DISN beschermen tegen aanvallen die afkomstig uit de cloudomgeving zijn. BCAP voert inbraakdetectie en preventie. het is ook gefilterd van niet-geautoriseerde verkeer. Dit onderdeel kan worden geplaatst met andere onderdelen van de SCCA. Het is raadzaam dat u dit onderdeel implementeren met behulp van de fysieke hardware. BCAP beveiligingsvereisten worden in de volgende tabel weergegeven.

#### <a name="bcap-security-requirements"></a>BCAP voor beveiliging voldoen

![BCAP vereisten matrix](media/bcapreqs.png)


### <a name="vdss"></a>VDSS

Het doel van de VDSS is Amerikaanse ministerie van defensie eigenaar van essentiële toepassingen die worden gehost in Azure te beschermen. Het grootste deel van de beveiligingsbewerkingen voert VDSS in de SCCA. Het voert inspectie van verkeer voor het beveiligen van de toepassingen die worden uitgevoerd in Azure. Dit onderdeel kan worden opgegeven in uw Azure-omgeving.

#### <a name="vdss-security-requirements"></a>VDSS voor beveiliging voldoen

![VDSS vereisten matrix](media/vdssreqs.png)

### <a name="vdms"></a>VDMS

Het doel van VDM's is voor de hostbeveiliging en gedeelde data center-services. De functies van VDM's kunnen uitvoeren in de hub van uw SCCA of de eigenaar van de essentiële onderdelen van het in hun eigen specifieke Azure-abonnement kunt implementeren. Dit onderdeel kan worden opgegeven in uw Azure-omgeving.

#### <a name="vdms-security-requirements"></a>VDM's voor beveiliging voldoen

![Matrix van VDM's-vereisten](media/vdmsreqs.png)


### <a name="tccm"></a>TCCM

TCCM is een zakelijke rol. Deze persoon is verantwoordelijk voor het beheren van de SCCA. Hun taken zijn: 

- Tot stand brengen plannen en beleidsregels voor toegang tot de cloudomgeving. 
- Zorg ervoor dat identiteits-en goed functioneren. 
- Plan voor de Cloud referentie onderhouden. 

Deze persoon is aangewezen door de autorisatie Official-kwalificatie. De BCAP, VDSS en VDM's bieden de mogelijkheden die de TCCM nodig heeft om uit te voeren hun taak.

#### <a name="tccm-security-requirements"></a>TCCM voor beveiliging voldoen

![TCCM vereisten matrix](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>SACA onderdelen en overwegingen bij de planning 

De referentiearchitectuur SACA is ontworpen voor het implementeren van de onderdelen VDSS en VDM's in Azure en de TCCM inschakelen. Deze architectuur is modulaire. Alle benodigde onderdelen van VDSS en VDM's kunt bevinden zich in een centrale hub. Enkele van de besturingselementen aan kan worden voldaan in de ruimte van de eigenaar van de essentiële of zelfs on-premises. Microsoft raadt aan dat u de onderdelen VDSS en VDM's dezelfde aan een centrale virtuele netwerk dat alle essentiële eigenaren verbinding via maken kunnen plaatsen. Het volgende diagram ziet u deze architectuur: 


![Diagram van SACA referentie-architectuur](media/sacav2generic.png)

Wanneer u uw strategie voor SCCA nalevingsbeleid en de technische architectuur plant, houd rekening met de volgende onderwerpen vanaf het begin omdat ze invloed hebben op elke klant. De volgende problemen zijn met DoD-klanten en meestal vertragen planning en uitvoering. 

#### <a name="which-bcap-will-your-organization-use"></a>Welke BCAP gaat gebruiken in uw organisatie?
   - DISA BCAP:
        - DISA heeft twee operationele BCAPs op de vijfhoek en op Camp Dorner, CA. Een derde is aankomen online gepland. 
        - De DISA BCAPs alle hebt Azure ExpressRoute-circuits naar Azure, die door het Amerikaanse ministerie van defensie klanten kan worden gebruikt voor connectiviteit. 
        - DISA heeft een Microsoft-peeringsessie op ondernemingsniveau voor het Amerikaanse ministerie van defensie-klanten die willen abonneren op Microsoft-software als een service (SaaS)-hulpprogramma's, zoals Office 365. U kunt met behulp van de BCAP DISA connectiviteit en peering met uw exemplaar SACA inschakelen. 
    - Bouw uw eigen BCAP:
        - Deze optie moet u ruimte in een CO-datacenter van de lease en het instellen van een ExpressRoute-circuit naar Azure. 
        - Deze optie is aanvullende goedkeuring vereist. 
        - Vanwege de extra goedkeuring en een fysieke build-out wordt deze optie de meeste tijd. 
    - U wordt aangeraden dat u de BCAP DISA. Deze optie direct beschikbaar is, is redundantie ingebouwd en klanten die worden uitgevoerd op het vandaag nog in de productieomgeving heeft.
- Amerikaanse ministerie van defensie routeerbare IP-adresruimte:
    - U moet DoD routeerbare IP-adresruimte aan de rand. De optie voor het gebruik van NAT verbinding maken die ruimten met particuliere IP-adresruimte in Azure is beschikbaar.
    - Neem contact op met het Amerikaanse ministerie van defensie netwerk informatie Center (NIC) om op te halen van IP-adresruimte. U hebt deze nodig als onderdeel van uw inzending System/Network goedkeuring proces (module) met DISA. 
    - Als u van plan bent te NAT gebruikt om privé-adresruimte in Azure verbinding te maken, moet u minimaal een/24 subnet van de adresruimte van de NIC voor elke regio waarin u van plan bent om te implementeren SACA toegewezen.
- Redundantie:
    - Implementeer een exemplaar van SACA in ten minste twee regio's. In de cloud voor Amerikaanse ministerie van defensie implementeert u deze in beide beschikbaar DoD-regio's.
    - Verbinding maken met ten minste twee BCAPs via afzonderlijke ExpressRoute-circuits. Beide ExpressRoute-verbindingen kunnen vervolgens worden gekoppeld aan elke regio waarin SACA exemplaar. 
- DoD-onderdeel-specifieke vereisten:
    - Heeft uw organisatie specifieke vereisten buiten de SCCA-vereisten? Sommige organisaties hebben vereisten voor specifieke IP-Adressen.
- SACA is een modulaire architectuur:
    - Gebruik alleen de onderdelen die u nodig hebt voor uw omgeving. 
        - Virtuele netwerkapparaten in één laag of meerdere lagen implementeren.
        - Geïntegreerde IP-Adressen of bring-your-own IP-Adressen gebruiken.
- Niveau van de DoD impact van uw toepassingen en gegevens:
    - Als er mogelijkheid om toepassingen die worden uitgevoerd in Microsoft IL5 regio's, bouw uw exemplaar SACA in IL5. Het exemplaar kan worden gebruikt in het zicht van IL4 toepassingen en IL5. Een exemplaar IL4 SACA voor een toepassing IL5 waarschijnlijk ontvangt geen erkenning.

#### <a name="which-network-virtual-appliance-vendor-will-you-use-for-vdss"></a>Welke leverancier van het virtuele apparaat network zult u gebruiken voor VDSS?
Zoals eerder vermeld, kunt u deze verwijzing SACA bouwen met behulp van verschillende apparaten en Azure-services. Microsoft heeft de sjablonen voor oplossingen voor het implementeren van de architectuur SACA met F5 en Citrix geautomatiseerd. Deze oplossingen worden behandeld in de volgende sectie.

#### <a name="which-azure-services-will-you-use"></a>Welke Azure-services wilt u gebruiken?
- Er zijn Azure-services die voldoen aan de vereisten voor log analytics, host gebaseerde beveiliging en functionaliteit van de id's. Het is mogelijk dat bepaalde services algemeen beschikbaar in Microsoft IL5 regio's zijn. In dit geval moet u mogelijk gebruik van hulpprogramma's van derden als deze Azure-services kunnen niet voldoen aan uw vereisten. Bekijk de hulpprogramma's die u vertrouwd met bent en de haalbaarheid van het gebruik van Azure systeemeigen hulpprogramma.
- U wordt aangeraden dat u net zoveel Azure systeemeigen-hulpprogramma's mogelijk. Ze zijn gebouwd met cloudbeveiliging in gedachten en naadloos integreren met de rest van het Azure-platform. Gebruik de Azure-systeemeigen hulpprogramma's in de volgende lijst om te voldoen aan verschillende eisen van SCCA:

    - [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview )
    - [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) 
    - [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
    - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
    - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
    - [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/overview)
    - [Azure Firewall](https://docs.microsoft.com/azure/firewall/overview) 
    - [Azure Front Door](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
    - [Azure-beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview)
    - [Azure DDoS Protection](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
    - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Grootte aanpassen
    - Een oefening grootte moet worden voltooid. Het aantal gelijktijdige verbindingen u via het SACA-exemplaar en de doorvoer netwerkvereisten hebt mogelijk bekijken. 
    - Deze stap is cruciaal. Zo kunt u het formaat van de virtuele machines en de licenties die zijn vereist door de verschillende leveranciers die u in uw exemplaar SACA te identificeren. 
    - Een goede kostenanalyse kan niet worden uitgevoerd zonder deze oefening grootte. Juiste formaat wordt ook kan voor de beste prestaties. 


## <a name="most-common-deployment-scenario"></a>Meest voorkomende implementatiescenario

 Verschillende klanten van Microsoft zijn gebleven door de volledige implementatie of op minimaal de planningfase van hun omgevingen SACA. Inzicht in de meest voorkomende scenario voor implementaties van hun ervaringen aan het licht komt. Het volgende diagram toont de architectuur van de meest voorkomende: 


![Diagram van SACA referentie-architectuur](media/sacav2commonscenario.png) 


Zoals u in het diagram zien kunt, abonneren DoD klanten normaal gesproken op twee van de BCAPs DISA. Een van deze wordt toegevoegd aan de westkust en het andere leven op de oostkust. Een particuliere ExpressRoute-peer is ingeschakeld voor Azure op elke locatie DISA BCAP. Deze ExpressRoute-peers worden vervolgens gekoppeld aan de virtuele netwerkgateway in de DoD-Oost en DoD centraal Azure-regio's. Een exemplaar van SACA wordt geïmplementeerd in de DoD-Oost en DoD centraal Azure-regio's. Alle inkomende en uitgaande verkeer stroomt via deze naar en van de ExpressRoute-verbinding met de BCAP DISA.

Eigenaar van essentiële toepassingen kiest u de Azure-regio's waarin ze van plan bent om hun toepassingen te implementeren. Ze gebruik virtual network-peering voor het virtuele netwerk van hun toepassing verbinden met het virtuele netwerk van SACA. En ze force tunneling van hun verkeer via het VDSS-exemplaar.

We raden deze architectuur omdat deze voldoet aan de vereisten SCCA. Het is maximaal beschikbaar en eenvoudig worden geschaald. Het vereenvoudigt ook implementatie en beheer.

## <a name="automated-saca-deployment-options"></a>Automatische SACA implementatie-opties

 Zoals eerder vermeld, is Microsoft een partnerschap aangegaan met twee leveranciers te maken van een automatische SACA sjabloon. Beide sjablonen implementeren de volgende Azure-onderdelen: 

- Virtueel netwerk van SACA
    - Beheersubnet
        - Dit subnet is waar de beheer-VM's en services zijn geïmplementeerd, ook wel bekend als een sprong vakken.
        - VDMS subnet
            - Dit subnet is waar de virtuele machines en services die worden gebruikt voor VDM's worden geïmplementeerd.
        - Niet-vertrouwde en betrouwbare subnetten
            - Deze subnetten zijn waarop virtuele apparaten zijn geïmplementeerd.
        - Gatewaysubnet
            - Dit subnet is waarop de ExpressRoute-Gateway wordt geïmplementeerd.
- Korte inleiding in het virtuele beheermachines
    - Ze worden gebruikt voor out-of-band-beheer van de omgeving.
- Virtuele netwerkapparaten
    - Gebruik van een van beide Citrix of F5 op basis van welke sjabloon u implementeert.
- Openbare IP-adressen
    - Ze worden gebruikt voor de front-end totdat ExpressRoute online wordt gebracht. Deze IP-adressen vertalen naar de back-end Azure privé-adresruimte.
- Routetabellen 
    - Toegepast tijdens automation, deze tabellen geforceerde tunnel alle verkeer routeren via het virtuele apparaat.
- Azure-load balancers - standaard-SKU
    - Ze worden gebruikt om verkeer te verdelen over de apparaten.
- Netwerkbeveiligingsgroepen
    - Ze worden gebruikt om te bepalen welke typen verkeer naar bepaalde eindpunten kunnen passeren.


### <a name="citrix-saca-deployment"></a>Implementatie van Citrix SACA

Een sjabloon voor de implementatie Citrix implementeert u twee lagen van de maximaal beschikbare Citrix ADC-apparaten. Deze architectuur voldoet aan de vereisten van VDSS. 

![Diagram van Citrix SACA](media/citrixsaca.png)


Zie voor de Citrix-documentatie en een script voor implementatie, [deze link naar GitHub](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca).


 ### <a name="f5-saca-deployment"></a>F5 SACA implementatie

Twee afzonderlijke F5 implementatiesjablonen betrekking hebben op twee verschillende architecturen. De eerste sjabloon heeft slechts één laag van F5-apparaten in een maximaal beschikbare actief-actief-configuratie. Deze architectuur voldoet aan de vereisten voor VDSS. De tweede sjabloon wordt een tweede beveiligingslaag van maximaal beschikbare actief-actief-F5s toegevoegd. Deze tweede laag kan klanten hun eigen IP-Adressen gescheiden van F5 tussen de lagen F5 toevoegen. Niet alle onderdelen van het Amerikaanse ministerie van defensie hebben specifieke IP-Adressen voorgeschreven voor gebruik. Als dit het geval is, werkt de één laag van F5 apparaten voor de meeste, omdat deze architectuur IP-Adressen op de F5-apparaten bevat.

![Diagram van F5 SACA](media/f5saca.png)

Zie voor de F5-documentatie en een script voor implementatie, [deze link naar GitHub](https://github.com/f5devcentral/f5-azure-saca).












