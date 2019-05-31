---
title: Veelgestelde vragen - VMware-oplossing door CloudSimple
description: Veelgestelde vragen over Azure VMware-oplossing door CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e727398e1b7bfa406166574ab40320c68dac5709
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358526"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>Veelgestelde vragen over de oplossing VMware door CloudSimple

Veelgestelde vragen en antwoorden over CloudSimple VMware-oplossing waarmee u inzicht in de service en het gebruik ervan.  De vragen en antwoorden zijn gerangschikt in de volgende categorieën.

* CloudSimple Service
* Connectiviteit
* Netwerken
* Beveiliging
* Compute
* Opslag
* VMware
* Integratie van Azure
  
## <a name="cloudsimple-service"></a>CloudSimple Service

**Wat is de oplossing VMware door CloudSimple?**

**VMware-oplossing door CloudSimple** transformeert en breidt de VMware-workloads naar persoonlijke, exclusieve clouds op Azure in enkele minuten. Wij zorgen voor inrichting, het beheren van de infrastructuur en het organiseren van werkbelasting tussen on-premises en Azure. Omdat uw apps worden uitgevoerd exact de dezelfde on-premises en in Azure, wordt u profiteren van de elasticiteit en services van de cloud zonder de complexiteit van uw apps opnieuw modelleren. CloudSimple wordt verlaagd van uw totale eigendomskosten met een cloud-verbruik model waarmee u op aanvraag inrichten, betaal-als-u-groeit en capaciteitsoptimalisatie van.  Zie [wat is de oplossing VMware op Azure door CloudSimple](cloudsimple-vmware-solutions-overview.md) voor functies, voordelen en scenario's.

**Wat is een CloudSimple Private Cloud?**

U richt in een persoonlijke, exclusieve cloud die bestaan uit high performance computing, opslag en netwerken omgeving geïmplementeerd op Microsoft Azure-infrastructuur (hardware- en datacenter-ruimte) in Azure-locaties.  Private Cloud biedt een systeemeigen VMware-platform-as-a-service'. In termen van VMware bevat elke Privécloud precies één exemplaar van de vCenter-Server. De vCenter Server beheert meerdere ESXi-knooppunten die zijn opgenomen in een of meer vSphere-Hostclusters, samen met de bijbehorende virtuele SAN (vSAN)-opslag. Een CloudSimple Service kan bevatten meerdere persoonlijke clouds in uw Azure-abonnement.  Zie voor meer informatie over privéclouds [Private Cloud overzicht](cloudsimple-private-cloud.md).

**Waar CloudSimple service beschikbaar is?**

CloudSimple is beschikbaar in regio's VS-Oost en VS-West.

**Hoe kan ik mijn abonnement voor CloudSimple inschakelen?**

U kunt contact opnemen met uw Microsoft-accountvertegenwoordiger op [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com) om in te schakelen van uw abonnement voor CloudSimple-service. Geef uw abonnements-ID in het e-mailadres waarvoor u wilt dat CloudSimple-service is ingeschakeld.  

**Hoe krijg ik toegang tot de portal CloudSimple?**

U openen CloudSimple portal vanuit Azure portal.  Zie [VMware-oplossing door CloudSimple portal van Azure portal toegang tot](https://docs.azure.cloudsimple.com/access-cloudsimple-portal) artikel voor meer informatie over toegang tot CloudSimple-portal. 

**Hoe ik de capaciteit voor een Privécloud vergroten?**

U koopt knooppunten uit Azure portal en vouw uw Privécloud vanuit de portal CloudSimple.  U kunt uw Private Cloud uitbreiden door extra knooppunten toevoegen aan bestaande vSphere-cluster of door het maken van nieuwe vSphere-cluster.  Zie [Vouw een CloudSimple Private Cloud](https://docs.azure.cloudsimple.com/expand-private-cloud) artikel voor de procedure.

**Wat gebeurt er met mijn persoonlijke Cloud tijdens onderhoud?**

CloudSimple biedt periodieke meldingen van dagen voorafgaand aan de gepland onderhoud.  Onderhoud wordt uitgevoerd op een niet-storende manier om te controleren of de beschikbaarheid van uw Privécloud.  Onderhoud kan van de volgende typen zijn:

1. **CloudSimple infrastructuur:**  CloudSimple infrastructuur is zo ontworpen dat maximaal beschikbaar.  Tijdens het onderhoud, wordt connectiviteit en de beschikbaarheid van uw Privécloud gegarandeerd door bij te werken redundante onderdelen één bewerking tegelijk zonder enige gevolgen.  U hebt toegang tot uw vCenter Private Cloud, alle virtuele machines, internetverbinding van uw Privécloud en verbindingen met on-premises of Azure.
2. **CloudSimple-Portal:** Tijdens onderhoud bepaalde functies op CloudSimple portal mogelijk niet toegankelijk of is uitgeschakeld.  Onderhoudsmelding bevat de details van wat in de portal kan worden gedaan.

## <a name="connectivity"></a>Connectiviteit

**Wat zijn mijn connectiviteitsopties voor met CloudSimple regio netwerk?**

CloudSimple biedt drie verschillende connectiviteitsopties verbinding maken met uw netwerk CloudSimple-regio.  Alle drie kunnen samen worden gebruikt.

1. ExpressRoute-verbinding van uw on-premises datacenter met CloudSimple regio netwerk - privékoppeling met hoge snelheid met lage latentie beveiligde privéverbinding bridging van uw on-premises ExpressRoute-circuit met uw CloudSimple ExpressRoute-circuit met behulp van globaal bereik. Zie [verbinding maken met on-premises CloudSimple met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection) artikel voor het instellen van de verbinding.
2. ExpressRoute-verbinding van uw Azure-netwerk met het netwerk van de regio CloudSimple - privékoppeling met hoge snelheid met lage latentie beveiligde privéverbinding bridging van het virtuele netwerk in Azure met uw CloudSimple ExpressRoute-circuit met behulp van de virtuele netwerkgateways.  Zie [uw omgeving CloudSimple Private Cloud verbinden met de Azure-netwerk met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/azure-expressroute-connection) artikel voor het instellen van de verbinding.
3. Site-naar-Site VPN-verbinding vanaf uw on-premises datacenter met het netwerk van de regio CloudSimple - beveiligde virtueel particulier netwerk van uw on-premises VPN-apparaat bij uw CloudSimple Private Cloud-regio.  Zie [Set up VPN-verbinding tussen uw on-premises netwerk en de Privécloud CloudSimple]-artikel voor het instellen van VPN-verbinding.

**Hoe maak ik verbinding in een Privécloud?**

U kunt details van uw Privécloud bekijken in de CloudSimple Portal. Als u wilt verbinding maken met de vCenter-overeenkomt met uw Privécloud, zorg ervoor dat netwerkverbinding tot stand gebrachte met behulp van Site-naar-Site, punt-naar-Site of ExpressRoute. Start vervolgens, de portal CloudSimple via Azure portal en klik op *vSphere-Client starten* op de startpagina of op de pagina met details van de Privécloud.

**Wat is het voordeel van het ExpressRoute-circuit?**

Azure ExpressRoute-circuit biedt beveiligde verbinding privékoppeling met hoge snelheid met lage latentie.  CloudSimple biedt een speciaal ExpressRoute-Circuit per regio per klant.  Dit circuit gebruikt, kunt u veilig verbinding van on-premises en/of uw Azure-abonnement maken.

**Wat zijn de netwerkkosten voor verbinding maken vanaf CloudSimple. Alle kosten voor uitgaand verkeer naar/van CloudSimple naar Azure? In regio's?**

Er zijn geen kosten voor een uitgaand netwerkverkeer.  Azure standard-tarieven van toepassing op al het uitgaande verkeer van het virtuele netwerk of van on-premises ExpressRoute-circuit.

## <a name="networking"></a>Netwerken

**Welke netwerkfuncties zijn beschikbaar voor mijn Private Cloud?**

U kunt VLAN's (en hun subnetten) inrichten, firewall-tabellen, en het openbare IP-adressen toewijzen, en worden toegewezen aan een virtuele machine die wordt uitgevoerd in uw Privécloud.  Zie voor meer informatie, [VLAN's / subnetten overzicht](cloudsimple-vlans-subnets.md), [Firewall tabellen overzicht](cloudsimple-firewall-tables.md), en [openbare IP-adres overzicht](cloudsimple-public-ip-address.md) artikelen.

**Hoe kan ik Stel verschillende subnetten voor mijn toepassingen in mijn Private Cloud?**

U kunt VLAN's op uw Privécloud maken vanuit de Portal van uw CloudSimple.  Als u het VLAN maakt, kunt u een gedistribueerde poortgroep maken op uw Privécloud vCenter met behulp van het VLAN en maken van virtuele machines die zijn verbonden met de gedistribueerde poortgroep.  U kunt de tabel van de firewall inschakelen voor het VLAN/Subnet en firewallregels voor het beveiligen van netwerkverkeer definiëren.

**Welke firewall-instellingen zijn beschikbaar voor mijn Privéclouds?**

U kunt regels voor Noord-Zuid en Oost-west-verkeer configureren.  De regels zijn gedefinieerd in een tabel met firewall.  De tabel firewall kan worden gekoppeld aan VLAN(s) op uw Privécloud.  Zie [firewall tabellen en regels instellen voor Privéclouds](https://docs.azure.cloudsimple.com/firewall) artikel voor de procedure.

**Kan ik openbare IP-adressen toewijzen voor virtuele machines in mijn Private Cloud-omgeving?**

U kunt eenvoudig een nieuwe openbare IP-adres toewijzen en deze koppelen aan een privé IP-adres van uw virtuele machine of een apparaat in de portal CloudSimple.  U kunt ook nieuwe firewallregels maken of bestaande firewallregels zodat verkeer van bepaalde poorten en/of specifieke set IP-adressen in de Portal van toepassing. Zie [openbare IP-adressen toewijzen voor Private Cloud omgeving](https://docs.azure.cloudsimple.com/public-ips) voor de procedure.

## <a name="security"></a>Beveiliging

**Wat zijn mijn beveiligingsopties op CloudSimple?**

CloudSimple Private Cloud biedt de volgende beveiligingsfuncties voor het beveiligen van uw omgeving Private Cloud:

1. **Data-at-rest versleuteling**: U kunt gegevens in rust die zich bevinden op de virtuele SAN-opslag in uw Privécloud versleutelen. virtueel SAN biedt ondersteuning voor externe key management server, die kan worden geïmplementeerd in uw Azure vNet of on-premises-omgeving.  Zie [configureren virtueel SAN-versleuteling voor uw Privécloud CloudSimple](https://docs.azure.cloudsimple.com/vsan-encryption) voor meer informatie.
2. **Netwerkbeveiliging**: Besturingselement voor netwerkverkeer van/naar uw Privécloud van Internet, on-premises en in de subnetten van uw persoonlijke Cloud met behulp van firewallregels.
3. **Veilig, particuliere verbinding**: Beveiligde particuliere verbinding tussen uw on-premises netwerk en uw Azure-abonnement.

## <a name="compute"></a>Compute

**Wat voor soort hosts zijn beschikbaar?**

CloudSimple biedt twee hosttypen:

* **CS28 knooppunt:** CPU:2 x 2.2 GHz, totale 28 kernen, 48 HT.  RAM: 256 GB.  Storage: 1600 GB NVMe Cache, 5760 GB aan gegevens (All-Flash). Netwerk: 2x25Gbe NIC
* **CS36 knooppunt:** CPU 2 x 2,3 GHz, totale 36 kernen, 72 HT.  RAM: 512 GB.  Storage: 3200 GB NVMe Cache 11,520 GB aan gegevens (All-Flash).  Netwerk: 2x25Gbe NIC

**Hoe worden de mogelijke hardwarefouten verwerkt?**

Alle CloudSimple infrastructuur wordt continu bewaakt door het platform CloudSimple en onze service operations-teams.  Als er een hardware-uitval is gedetecteerd, wordt een nieuw knooppunt wordt toegevoegd aan de Privécloud en knooppunt verwijderd ervoor te zorgen dat de hoge beschikbaarheid van uw Privécloud.

## <a name="storage"></a>Opslag

**Wat voor soort opslag wordt ondersteund op een Privécloud?**

CloudSimple biedt **All-flash VMware virtuele SAN-opslag** met elke Privécloud.  Elke vSphere wordt gemaakt met een eigen gegevensarchief virtueel SAN.  Zie [onderdelen Private Cloud VMware - virtuele SAN-opslag](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage) artikel voor meer informatie.

**Is de versleuteling van gegevens die worden ondersteund?**
Ja.  U kunt de vSAN opslag instellen op uw Privécloud een key management server (KMS) die on-premises geïmplementeerd gebruiken of op Azure voor het versleutelen van gegevens die zijn opgeslagen op een virtueel SAN

**Hoe worden de schijven verwerkt?**

Alle hardware-onderdelen van de Privécloud CloudSimple bewaking continu worden bewaakt.  Als een schijf-storing wordt gedetecteerd of een schijf wordt geïdentificeerd als mislukt (op basis van heuristiek), wordt een nieuw knooppunt automatisch toegevoegd aan de Privécloud.  Het knooppunt met beschadigde schijf is verwijderd uit de Privécloud.

## <a name="vmware"></a>VMware

**Hoe voer ik grootschalige uploaden/migreren van toepassingen en gegevens van on-premises?**

CloudSimple biedt systeemeigen VMware vSphere-oplossing.  Een hulpprogramma voor migratie van grote hoeveelheden gegevens kan worden gebruikt met CloudSimple Private Cloud.  Enkele van de beschikbare opties zijn:

1. VMware HCX voor bulksgewijs migratie van gegevens.
2. Koude migratie van gegevens met behulp van Storage vMotion van on-premises naar CloudSimple.

**Kan ik een VMware-hulpprogramma's installeren?**

CloudSimple biedt systeemeigen VMware vSphere-oplossing.  Een hulpprogramma dat wordt gebruikt voor het beheren van vSphere-omgeving op de locatie kan worden gebruikt op CloudSimple.  CloudSimple biedt ondersteuning voor Bring-Your-Own-License (BYOL)-model voor het installeren van VMware-hulpprogramma's.

**Hoe worden updates en upgrades beheerd?**

CloudSimple beheert en alle onderdelen van de infrastructuur van uw Privécloud-updates in een naadloze niet-verstorende manier.  Een update of de beveiligingspatch vrijgegeven door leveranciers van VMware of infrastructure wordt gepland voor een update zodra deze is gekwalificeerd door CloudSimple.

CloudSimple voert geen upgrades of updates van toepassingen worden geïnstalleerd op de Privécloud.

## <a name="azure-integration"></a>Integratie van Azure

**Welke Azure-services worden ondersteund?**

CloudSimple biedt Azure ExpressRoute-verbinding met uw abonnement op Azure.  Alle services die worden uitgevoerd in uw abonnement heeft een netwerkverbinding in uw Privécloud en verbinding kan maken met uw Privécloud.  Voorbeelden:

1. **Azure Active Directory** als een identiteitsbron van de van uw vCenter CloudSimple
2. **Azure-opslag** voor het opslaan van back-ups, afbeeldingen en andere gegevens van uw Privécloud
3. **Hybride toepassingen** -kunt u toepassingsarchitectuur die zich openbare en privéclouds uitstrekt.  U kunt bijvoorbeeld webservers maken in Azure die toegang hebben tot de toepassing en database-servers op CloudSimple Private Cloud.
4. **Met Azure monitor** en **Azure security center** -werkbelasting wordt uitgevoerd op VMware kunt gebruiken voor logboekregistratie, metrische gegevens voor prestaties en beveiligingsbeheer.

**Hoe ik mijn tenants voor VMware naar Azure toewijzen?**

CloudSimple biedt de unieke mogelijkheid voor het beheren van uw virtuele VMware-machines op de Privécloud van Azure-portal.  Een vCenter-resourcegroep (geconfigureerd met de gewenste resourcebeperkingen) kan worden toegewezen aan uw abonnement door de globale beheerder.  

**Welke licentievoordelen krijg ik met Azure?**

Met CloudSimple, kunt u profiteren van de Azure-voordeel gebruik en maximaal 90% besparen op licenties voor uw investering in Microsoft-Licenses verlagen van uw totale Eigendomskosten in vergelijking met andere clouds te behouden. Bovendien krijgen u beveiligingsupdates voor Windows Server 2008 en Microsoft SQL Server 2008 uitgebreid.  Houd de kosten voor de laag met Bring Your Own licenties (BYOL) naar de cloud voor veelgebruikte apps, zoals Veeam, Zerto en anderen.  