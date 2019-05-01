---
title: Beveiligen van Azure Computing-architectuur
description: Dit is een referentiearchitectuur voor een architectuur op ondernemingsniveau DMZ, met behulp van Network Virtual Appliances en andere hulpprogramma's. Deze architectuur is ontworpen om te voldoen aan het Ministerie van defensie van Secure Cloud Computing-architectuur functionele vereisten. Het kan echter worden gebruikt voor elke organisatie. Deze referentie bevat twee geautomatiseerde opties met apparatuur uit de Citrix of F5.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: f2e3d72db3f29dbc6d03b3259acb18daf684fb12
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917589"
---
# <a name="secure-azure-computing-architecture"></a>Beveiligen van Azure Computing-architectuur

Richtlijnen voor het instellen van beveiligde virtuele netwerken en het configureren van de beveiligingsprogramma's en services die worden bepaald door het Amerikaanse ministerie van defensie standaarden en procedures die een snel groeiende aantal DoD-klanten workloads implementeren in Azure is aangevraagd. DISA gepubliceerd de [Secure Cloud Computing-architectuur (SCCA) functionele vereisten Document](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) in 2017. SCCA beschrijft de functionele doelstellingen voor het beveiligen van Defense Information System van het netwerk (DISN) en commerciële Cloudprovider verbinding verwijst, en hoe essentiële veilige cloudtoepassingen eigenaren aan de grens van de verbinding. Het is verplicht dat elke DoD-entiteit die verbinding met de commerciële cloud maakt de richtlijnen die zijn uiteengezet in de FRD SCCA volgt.
 
Er zijn vier onderdelen van de SCCA. De grens Cloud Access Point (BCAP), virtueel Datacenter Security-Stack (VDSS), virtueel Datacenter beheerde Services (VDM's) en vertrouwde Cloud Referentiebeheer (TCCM). Er is een oplossing die voldoet aan de vereisten SCCA voor zowel IL4 en IL5 werkbelasting die wordt uitgevoerd in Azure ontwikkeld. Deze specifieke Azure-oplossing wordt beveiligd Azure Computing-architectuur (SACA) genoemd. Klanten die SACA implementeren in overeenstemming met de FRD SCCA zijn en kunnen klanten DoD-werkbelastingen verplaatsen naar Azure eenmaal verbonden. 

SCCA richtlijnen en architecturen zijn specifiek voor klanten van de Amerikaanse ministerie van defensie, helpt de meest recente wijzigingen aan SACA ook bij civiele klanten te voldoen aan de richtlijnen voor vertrouwde internet verbinding (TIC), evenals commerciële klanten die aansluiten op een veilige DMZ te implementeren beveiligen van hun azure-omgevingen.


## <a name="secure-cloud-computing-architecture-components"></a>Onderdelen van beveiligde Cloud Computing-architectuur

**BCAP**

Het doel van de BCAP is het beschermen van de DISN tegen aanvallen die afkomstig zijn van de cloudomgeving. BCAP zal uitvoeren inbraakdetectie en preventie, evenals ongeautoriseerd verkeer filteren. Dit onderdeel kan worden geplaatst met andere onderdelen van de SCCA. Het is raadzaam dat dit onderdeel wordt geïmplementeerd met behulp van de fysieke hardware. Hieronder vindt u de lijst met BCAP beveiligingsvereisten.

***BCAP voor beveiliging voldoen***

![BCAP vereisten matrix](media/bcapreqs.png)


**VDSS**

Het doel van de VDSS is eigenaar van de Amerikaanse ministerie van defensie essentiële toepassingen die worden gehost in Azure te beschermen. Het grootste deel van de beveiligingsbewerkingen voert VDSS in de SCCA. Het voert inspectie van verkeer van de toepassingen die worden uitgevoerd in Azure beveiligen. Dit onderdeel kan worden opgegeven in uw Azure-omgeving.

***VDSS voor beveiliging voldoen***

![VDSS vereisten matrix](media/vdssreqs.png)

**VDMS**

Het doel van VDM's voor de hostbeveiliging is, evenals gedeelde data center-services. De functies van VDM's kunnen uitvoeren in de hub van uw SCCA of de eigenaar van de essentiële onderdelen van het in hun eigen specifieke Azure-abonnement kunt implementeren. Dit onderdeel kan worden opgegeven in uw Azure-omgeving.

***VDM's voor beveiliging voldoen***

![Matrix van VDM's-vereisten](media/vdmsreqs.png)


**TCCM**

TCCM is een zakelijke rol. Deze persoon is verantwoordelijk voor het beheren van de SCCA. Hun taken omvatten tot stand brengen van abonnementen en beleidsregels voor toegang tot de cloudomgeving, ervoor te zorgen dat de identiteit en toegangsbeheer is goed functioneren, en de plannen voor het beheer van Cloud-referentie onderhouden. Deze persoon is aangewezen door officiële autoriseren. De BCAP, VDSS en VDM's biedt de mogelijkheden die nodig zijn voor de TCCM om uit te voeren hun functie.

***TCCM voor beveiliging voldoen***

![TCCM vereisten matrix](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>SACA onderdelen en overwegingen bij de planning 

De referentiearchitectuur SACA is ontworpen voor het implementeren van de onderdelen VDSS en VDM's in azure, evenals de TCCM inschakelen. Deze architectuur is modulaire, wat betekent dat alle benodigde onderdelen van VDSS en VDM's zich in een gecentraliseerde hub bevinden kunt of een aantal van de besturingselementen in de missie eigenaar ruimte of zelfs on-premises kan worden voldaan. De aanbeveling van ons team van Microsoft is in een centrale virtuele Net dat alle essentiële eigenaren verbinding via maken kunnen de onderdelen VDSS en VDM's plaatsen. Het onderstaande diagram bevat onze aanbevolen architectuur. 


![Diagram van SACA referentie-architectuur](media/sacav2generic.png)

Er zijn veel aandachtspunten bij het plannen van uw strategie voor SCCA nalevingsbeleid en de technische architectuur. Het is belangrijk dat de volgende onderwerpen vanaf het begin rekening wordt gehouden wanneer elke klant moet betrekking hebben op deze. De volgende onderwerpen zijn problemen die afkomstig zijn van met echte DoD-klanten en vaak te vertragen om de planning en uitvoering. 

- Welke BCAP gaat gebruiken in uw organisatie?
    - DISA BCAP
        - DISA heeft twee operationele BCAPs op vijfhoek en Camp Dorner CA, met een derde online binnenkort beschikbaar. 
        - De DISA BCAPs alle hebt ExpressRoute-circuits aan Azure, die kan worden gebruikt door het Amerikaanse ministerie van defensie klanten voor connectiviteit. 
        - DISA heeft al een Microsoft-Peering-sessie op ondernemingsniveau voor DoD-klanten die willen abonneren op Microsoft SaaS-hulpprogramma's, zoals Office 365. Met behulp van DISA BCAP, kunt u de connectiviteit en peering met uw exemplaar SACA inschakelen. 
    - Bouw uw eigen BCAP
        - Hiervoor moeten u ruimte in een CO-datacenter van de lease en instellen van een ExpressRoute-circuit naar Azure. 
        - Deze optie is aanvullende goedkeuring vereist 
        - Deze optie wordt vanwege de aanvullende goedkeuring en een fysieke build van de meeste tijd. 
    - De aanbeveling van Microsoft is de BCAP DISA gebruiken. Deze optie direct beschikbaar, met ingebouwde redundantie en al op klanten die op deze vandaag nog in de productieomgeving.
- Amerikaanse ministerie van defensie routeerbare IP-adresruimte
    - U moet gebruiken DoD routeerbare IP-adresruimte aan de rand. De optie voor NAT die met particuliere IP-adresruimte in Azure is beschikbaar.  
    - Neem contact op met DoD-NIC voor IP-adresruimte van wel vereist als onderdeel van uw inzending voor de module met DISA. 
    - Als u van plan NAT met privé-adresruimte in Azure bent, moet u minimaal een/24 subnet van de adresruimte van de NIC voor elke regio die u wilt implementeren SACA toegewezen. 
- Redundantie 
    - Microsoft raadt u aan een SACA-exemplaar te implementeren op ten minste twee regio's. In de cloud voor Amerikaanse ministerie van defensie, kan dit betekenen dat u deze implementeren op beide beschikbaar DoD-regio's. 
    - Ook is het nodig dat u verbinding met ten minste twee BCAPs via afzonderlijke ExpressRoute-circuits maken. Beide Express-Routes kunnen vervolgens worden gekoppeld aan elke regio waarin SACA exemplaar. 
- Onderdeel-specifieke vereisten voor Amerikaanse ministerie van defensie
    - Heeft uw organisatie specifieke vereisten buiten de SCCA-vereisten? (Sommige organisaties hebben vereisten voor specifieke IP-Adressen)
- SACA is een modulaire architectuur  
    - Gebruik alleen de onderdelen die u nodig hebt voor uw omgeving. 
        - NVA's implementeren in een laag van één of meerdere lagen
        - Geïntegreerd IP-Adressen of Voeg uw eigen IP-Adressen
- Niveau van de DoD impact van uw toepassingen en gegevens
    - Als er mogelijkheid om toepassingen die in onze IL5 regio's worden uitgevoerd, is het nodig dat u uw exemplaar SACA in IL5 maken. Het exemplaar kan worden gebruikt voor zowel IL4 toepassingen als IL5. Maar een IL4 SACA-exemplaar voor een toepassing IL5 accreditatie waarschijnlijk niet ontvangen. 
- Welke leverancier Network Virtual Appliance zult u gebruiken voor VDSS?
    - Zoals eerder vermeld, kan deze verwijzing SACA worden gebouwd met behulp van verschillende apparaten en Azure-services. We hebben echter geautomatiseerde oplossingssjablonen om de architectuur SACA met F5 en Citrix te implementeren. Deze oplossingen wordt hieronder in detail worden besproken. 
- Welke Azure-services wilt u gebruiken?
    - Er zijn Azure-services die kunnen voldoen aan eisen op aan log analytics, host gebaseerde beveiliging en functionaliteit van de id's. Het is echter mogelijk dat bepaalde services algemeen beschikbaar in onze IL5 regio's zijn. Dit kan leiden tot hoeft te worden sommige 3e partij-hulpprogramma's gebruikt als deze Azure-services kunnen niet voldoen aan uw behoeften. U moet om te kijken welke u hulpmiddelen vertrouwd bent met zijn en de haalbaarheid van het gebruik van systeemeigen hulpprogramma Azure. 
    - Het is van Microsoft-aanbeveling dat u net zoveel Azure systeemeigen-hulpprogramma's mogelijk zoals ze zijn gebouwd met cloudbeveiliging in gedachten en naadloos met de rest van het Azure-platform integreren. Hieronder volgt een lijst met Azure systeemeigen hulpprogramma's die kunnen worden gebruikt om te voldoen aan verschillende eisen van SCCA. 
        - [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview )
        - [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) 
        - [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
        - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
        - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
        - [Application Gateway](https://docs.microsoft.com/azure/application-gateway/overview)
        - [Azure Firewall](https://docs.microsoft.com/azure/firewall/overview) 
        - [Azure Front Door](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
        - [Beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview)
        - [Azure DDoS Protection](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
        - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Grootte aanpassen
    - Een oefening grootte moet worden voltooid. U moet om te kijken naar het aantal gelijktijdige verbindingen u via het exemplaar SACA, evenals de netwerkvereisten voor de doorvoer hebt mogelijk. 
    - Dit is een belangrijke stap als deze wordt helpen om de grootte van de virtuele machines, evenals helpen bij het bepalen van de licenties die vereist is door de verschillende leveranciers die u in uw exemplaar SACA gebruiken wilt. 
    - Een goede kostenanalyse kan niet worden uitgevoerd zonder deze oefening grootte, het is ook belangrijk om te controleren of alles juist is aangepast om toe te staan voor de beste prestaties. 


## <a name="most-common-deployment-scenario"></a>Meest voorkomende implementatiescenario

Microsoft heeft verschillende klanten die al hebben doorlopen aan de volledige implementatie of van plan zijn ten minste fasen van hun omgeving SACA. Dit heeft hadden we inzicht krijgen in de meest voorkomende implementatiescenario. In het onderstaande diagram ziet u de meest voorkomende architectuur. 


![Diagram van SACA referentie-architectuur](media/sacav2commonscenario.png) 


Zoals u in het diagram zien kunt, abonneren DoD klanten normaal gesproken op twee van de BCAPs DISA, een van deze leven op de westkust en het andere leven op de oostkust. Een particuliere ExpressRoute-peer is ingeschakeld voor Azure op elke locatie DISA BCAP. Deze ExpressRoute-Peers worden vervolgens gekoppeld aan de Gateway van het virtuele netwerk in de DoD-Oost en DoD centraal Azure-regio's. Een exemplaar van SACA wordt geïmplementeerd in de DoD-Oost en DoD centraal Azure-regio en alle inkomende en uitgaande verkeer stroomt via deze naar en van de Express Route-verbinding met de BCAP DISA. 

Missiekritieke toepassingen en kies vervolgens welke Azure-regio('s) implementeert ze hun toepassingen in de eigenaar en gebruik Peering in virtuele netwerken aan hun toepassing verbinden met's Virtueelnetwerk met het Virtueelnetwerk van SACA. Geforceerde Tunneling van hun verkeer via het VDSS-exemplaar. 

Deze architectuur wordt sterk aanbevolen door Microsoft, als deze voldoet aan vereisten voor SCCA, het maximaal beschikbare, makkelijk schaalbaar is en het vereenvoudigt de implementatie en beheer.

## <a name="automated-saca-deployment-options"></a>Automatische SACA implementatie-opties

 Eerder gezegd dat Microsoft is een partnerschap aangegaan met twee leveranciers te maken van een automatische SACA sjabloon. Beide sjablonen implementeren de volgende Azure-onderdelen. 

- SACA Virtual Network
    - Beheersubnet
        - Wanneer de beheer-VM's en services zijn geïmplementeerd (jumpboxes)
        - VDMS Subnet
            - Welke virtuele machines en services die worden gebruikt voor VDM's zijn geïmplementeerd
        - Niet-vertrouwde en betrouwbare subnetten 
            - Welke virtuele apparaten zijn geïmplementeerd
        - Gatewaysubnet
            - Waar de ExpressRoute-Gateway wordt geïmplementeerd
- Korte inleiding in het virtuele Beheermachines
    - Gebruikt voor Out of Band Management van de omgeving.
- Virtuele netwerkapparaten
    - Citrix of druk op F5 afhankelijk van de sjabloon die u implementeert.
- Openbare IP-adressen
    - Gebruikt voor front-end totdat ExpressRoute online wordt gebracht. Deze IP-adressen wordt vertaald naar de back-end Azure privé-adresruimte
- Routetabellen 
    - Toegepast tijdens de automatisering, deze route tabellen geforceerde tunneling al het verkeer via het virtuele apparaat
- Azure Load Balancers - Standard SKU
    - Deze worden gebruikt om verkeer te verdelen over de apparaten
- Netwerkbeveiligingsgroepen
    - Gebruikt om te bepalen welke typen verkeer kunnen bladeren naar bepaalde eindpunten


**Implementatie van Citrix SACA**

Citrix heeft gemaakt van een sjabloon voor de implementatie die twee lagen van de maximaal beschikbare Citrix ADC-apparaten implementeert. Deze architectuur voldoet aan de vereisten van VDSS. 

![Citrix SACA Diagram](media/citrixsaca.png)


Citrix-documentatie en implementatie-script kan worden gevonden [hier.](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca)


 **F5 SACA implementatie**

F5 heeft twee afzonderlijke implementatiesjablonen die betrekking hebben op twee verschillende architecturen worden gemaakt. Het eerste item heeft slechts één laag van F5-apparaten in een maximaal beschikbare actief-actief-configuratie. Deze architectuur voldoet aan de vereisten voor VDSS. De tweede wordt een tweede beveiligingslaag van maximaal beschikbare actief-actief-F5s toegevoegd. Het doel van deze tweede laag is waarmee klanten hun eigen IP-Adressen gescheiden van F5 tussen de lagen F5 toevoegen. Niet alle onderdelen van het Amerikaanse ministerie van defensie hebben specifieke IP-Adressen voorgeschreven voor gebruik. Als dit het geval is, werken de één laag van F5 apparaten voor meest sinds die architectuur IP-Adressen op de F5-apparaten bevat.  

![Citrix SACA Diagram](media/f5saca.png)

F5-documentatie en implementatie-script kan worden gevonden [hier.](https://github.com/f5devcentral/f5-azure-saca) 












