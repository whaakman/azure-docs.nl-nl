---
title: Veelgestelde vragen - VMware-oplossing door CloudSimple
description: Veelgestelde vragen over Azure VMware-oplossing door CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e187a4a75a27e206a632388f1e20a94da032eb08
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595348"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>Veelgestelde vragen over de oplossing VMware door CloudSimple

Veelgestelde vragen en antwoorden over Azure VMware-oplossing door CloudSimple waarmee u inzicht in de service en het gebruik ervan. De vragen en antwoorden zijn gerangschikt in de volgende categorieën:

* CloudSimple service
* Connectiviteit
* Netwerken
* Beveiliging
* Compute
* Storage
* VMware
* Integratie van Azure
 
## <a name="cloudsimple-service"></a>CloudSimple service

**Wat is Azure VMware oplossing door CloudSimple?**

Azure VMware-oplossing door CloudSimple transformeert en breidt de VMware-workloads naar persoonlijke, exclusieve clouds op Azure in enkele minuten. De oplossing inricht, beheert de infrastructuur en werkbelasting tussen on-premises en Azure regelt. Omdat uw apps worden uitgevoerd exact de dezelfde on-premises en in Azure, wordt u profiteren van de elasticiteit en services van de cloud zonder de complexiteit van uw apps opnieuw modelleren. CloudSimple wordt verlaagd van uw totale eigendomskosten met een cloud-verbruik model waarmee u op aanvraag inrichten, betaal-als-u-groeit en capaciteitsoptimalisatie van. Zie voor functies, voordelen en scenario's, [wat is Azure VMware oplossing door CloudSimple?](cloudsimple-vmware-solutions-overview.md).

**Wat is een privécloud CloudSimple?**

U richt in een persoonlijke, exclusieve cloud die uit een hoogwaardige computer-, opslag- en netwerkomgeving zijn geïmplementeerd op Microsoft Azure-infrastructuur (hardware- en datacenter-ruimte) in Azure-locaties bestaat. De privécloud biedt een systeemeigen VMware-platform als een service. In termen van VMware bevat elke privécloud precies één exemplaar van de vCenter-Server. De vCenter Server beheert meerdere ESXi-knooppunten die zijn opgenomen in een of meer vSphere-hostclusters, samen met de bijbehorende virtuele SAN-opslag. Een service CloudSimple kan bevatten meerdere persoonlijke clouds in uw Azure-abonnement. Zie voor meer informatie over privéclouds [privécloud overzicht](cloudsimple-private-cloud.md).

**Waar is de service CloudSimple beschikbaar?**

CloudSimple is beschikbaar in regio's VS-Oost en VS-West.

**Hoe kan ik mijn abonnement voor CloudSimple inschakelen?**

Neem contact op met uw Microsoft-accountvertegenwoordiger op [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com) om in te schakelen van uw abonnement voor de service CloudSimple. Geef uw abonnements-ID in het e-mailadres waarvoor u wilt dat CloudSimple-service is ingeschakeld. 

**Hoe krijg ik toegang tot de portal CloudSimple?**

U toegang tot de portal CloudSimple vanuit Azure portal. Zie voor meer informatie over hoe u toegang tot de portal CloudSimple [toegang krijgen tot de VMware-oplossing door CloudSimple portal vanuit de Azure-portal](https://docs.azure.cloudsimple.com/access-cloudsimple-portal).

**Hoe ik de capaciteit voor een privécloud vergroten?**

U knooppunten van de Azure-portal in te richten en uitbreiden van uw privécloud vanuit de portal CloudSimple. U kunt uw persoonlijke cloud uitbreiden door toe te voegen knooppunten aan een bestaand vSphere-cluster of een nieuwe vSphere-cluster maken. Zie voor informatie over de procedure [uitbreiden van een privécloud CloudSimple](https://docs.azure.cloudsimple.com/expand-private-cloud).

**Wat gebeurt er met mijn persoonlijke cloud tijdens onderhoud?**

CloudSimple biedt dagen van de periodieke meldingen voor gepland onderhoud. Onderhoud wordt uitgevoerd op een ongestoord manier om te controleren of de beschikbaarheid van uw privécloud. Onderhoud kan van de volgende typen zijn:

- **CloudSimple infrastructuur**: De infrastructuur CloudSimple is zo ontworpen dat maximaal beschikbaar. Tijdens het onderhoud, wordt connectiviteit en de beschikbaarheid van uw privécloud gegarandeerd door bij te werken redundante onderdelen één bewerking tegelijk zonder enige gevolgen. U moet toegang tot uw persoonlijke cloud vCenter, alle virtuele machines, internetverbinding van uw privécloud en verbindingen met on-premises of Azure.
- **CloudSimple portal**: Tijdens onderhoud bepaalde functies op de portal CloudSimple mogelijk niet toegankelijk of is mogelijk uitgeschakeld. De onderhoudsmelding bevat informatie over wat kan worden gedaan in de portal.

## <a name="connectivity"></a>Connectiviteit

**Wat zijn mijn verbindingsopties met mijn regio CloudSimple netwerk?**

CloudSimple biedt drie verschillende connectiviteitsopties verbinding maken met uw netwerk CloudSimple-regio. Alle drie de opties kunnen samen worden gebruikt:

- Azure ExpressRoute-verbinding van uw on-premises datacenter met CloudSimple regio netwerk: Een privékoppeling met hoge snelheid met lage latentie beveiligde persoonlijke verbinding die voor de verbinding van uw on-premises ExpressRoute-circuit met uw CloudSimple ExpressRoute-circuit met behulp van globaal bereik. Als u de verbinding instelt, Zie [verbinding maken met on-premises CloudSimple met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection).
- ExpressRoute-verbinding van uw Azure-netwerk met het netwerk van de regio CloudSimple: Een hoge en lage latentie beveiligde persoonlijke verbinding die voor de verbinding van uw virtuele netwerk in Azure met uw CloudSimple ExpressRoute-circuit met behulp van de virtuele netwerkgateways. Als u de verbinding instelt, Zie [uw CloudSimple privécloud-omgeving met de Azure-netwerk verbinden met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/azure-expressroute-connection).
- Site-naar-site VPN-verbinding vanaf uw on-premises datacenter met het netwerk van de regio CloudSimple: Een beveiligd virtueel particulier netwerk van uw on-premises VPN-apparaat bij uw persoonlijke cloud CloudSimple regio. Als u de verbinding instelt, Zie [instellen van een VPN-verbinding tussen uw on-premises netwerk en de privécloud CloudSimple](https://docs.azure.cloudsimple.com/set-up-vpn).

**Hoe maak ik verbinding naar een privécloud?**

U kunt details van uw privécloud weergeven in de portal CloudSimple. Als u wilt verbinding maken met de vCenter-die overeenkomt met uw privécloud, zorg dat een netwerkverbinding tot stand is gebracht via site-naar-site, punt-naar-site of ExpressRoute. Vervolgens start u de portal CloudSimple vanuit Azure portal. Selecteer **vSphere-Client starten** op de startpagina of op de pagina met details van de privécloud.

**Wat is het voordeel van een ExpressRoute-circuit?**

Een Azure ExpressRoute-circuit biedt een beveiligde verbinding privékoppeling met hoge snelheid en lage latentie. CloudSimple biedt een speciaal ExpressRoute-circuit per regio per klant. Dit circuit gebruikt, kunt u beveiligde verbinding tussen on-premises en uw Azure-abonnement maken.

**Wat zijn de kosten voor netwerkverkeer naar en van CloudSimple verbinding? Zijn er kosten voor uitgaand verkeer naar en van CloudSimple naar Azure? Zijn er kosten voor uitgaand verkeer tussen regio's?**

Er zijn geen kosten voor uitgaand netwerkverkeer. Azure standard-tarieven van toepassing op al het uitgaande verkeer vanuit uw virtuele netwerk of vanuit een on-premises ExpressRoute-circuit.

## <a name="networking"></a>Netwerken

**Welke netwerkfuncties zijn beschikbaar voor mijn persoonlijke cloud?**

U kunt VLAN's en hun subnetten en firewall-tabellen inrichten. U kunt openbare IP-adressen toewijzen en toewijzen aan een virtuele machine die wordt uitgevoerd in uw privécloud. Zie voor meer informatie, [VLAN's en subnetten overzicht](cloudsimple-vlans-subnets.md), [tabellen firewalloverzicht](cloudsimple-firewall-tables.md), en [openbare IP-adres overzicht](cloudsimple-public-ip-address.md).

**Hoe kan ik Stel verschillende subnetten voor mijn toepassingen in mijn privécloud?**

U kunt VLAN's in uw persoonlijke cloud maken vanuit de portal van uw CloudSimple. Nadat u het VLAN maakt, kunt u een poortgroep gedistribueerde op de vCenter-privécloud maken met behulp van het VLAN en virtuele machines die zijn verbonden met de gedistribueerde poortgroep maken. U kunt een tabel firewall inschakelen voor de VLAN of het subnet en firewall-regels voor het beveiligen van netwerkverkeer definiëren.

**Welke firewall-instellingen zijn beschikbaar voor mijn persoonlijke clouds?**

U kunt regels voor Noord-Zuid en Oost-west-verkeer configureren. De regels zijn gedefinieerd in een tabel met firewall. De tabel firewall kan worden gekoppeld aan de VLAN's op uw privécloud. Zie voor de installatieprocedure [firewall tabellen en regels voor persoonlijke clouds instellen](https://docs.azure.cloudsimple.com/firewall).

**Kan ik openbare IP-adressen toewijzen voor virtuele machines in Mijn persoonlijke cloudomgeving?**

U kunt eenvoudig een nieuwe openbare IP-adres toewijzen en deze koppelen aan een privé IP-adres van uw virtuele machine of een apparaat in de portal CloudSimple. U kunt ook nieuwe firewallregels maken of bestaande firewallregels zodat verkeer van bepaalde poorten en specifieke sets met IP-adressen in de portal van toepassing. Zie voor de installatieprocedure [openbare IP-adressen toewijzen voor een persoonlijke cloudomgeving](https://docs.azure.cloudsimple.com/public-ips).

## <a name="security"></a>Beveiliging

**Wat zijn mijn beveiligingsopties op CloudSimple?**

Een privécloud CloudSimple biedt de volgende beveiligingsfuncties voor het beveiligen van uw persoonlijke cloudomgeving:

- **Data-at-rest versleuteling:** U kunt data-at-rest. Deze bevindt zich op een virtueel SAN-opslag in uw privécloud versleutelen. virtueel SAN biedt ondersteuning voor een externe sleutel beheerserver, die kan worden geïmplementeerd in uw Azure virtual network of on-premises-omgeving. Zie voor meer informatie, [configureren virtueel SAN-versleuteling voor uw privécloud CloudSimple](https://docs.azure.cloudsimple.com/vsan-encryption).
- **Netwerkbeveiliging:** Besturingselement voor netwerkverkeer van en naar uw privécloud van het internet, on-premises en in de subnetten van uw persoonlijke cloud met behulp van firewallregels.
- **Veilige, particuliere verbinding:** Veilige, particuliere verbinding tussen uw on-premises netwerk en uw Azure-abonnement.

## <a name="compute"></a>Compute

**Wat voor soort hosts zijn beschikbaar?**

CloudSimple biedt twee hosttypen:

* **CS28 knooppunt**: CPU:2 x 2.2 GHz, totale 28 kernen, 48 HT. RAM: 256 GB. Storage: 1600 GB NVMe-Cache, 5760 GB aan gegevens (All-Flash). Netwerk: 2x25Gbe NIC.
* **CS36 knooppunt**: CPU 2 x 2,3 GHz, totale 36 kernen, 72 HT. RAM: 512 GB. Storage: 3200 GB NVMe Cache 11,520 GB aan gegevens (All-Flash). Netwerk: 2x25Gbe NIC.

**Hoe kan ik problemen met hardware verwerkt?**

Alle CloudSimple infrastructuur wordt continu bewaakt door het platform CloudSimple en de service operations-teams. Als een hardwarefout wordt gedetecteerd, wordt een nieuw knooppunt toegevoegd aan uw privécloud. Het knooppunt wordt verwijderd om ervoor te zorgen de hoge beschikbaarheid van uw privécloud.

## <a name="storage"></a>Storage

**Wat voor soort opslag wordt ondersteund op een privécloud?**

CloudSimple biedt **All-flash VMware virtuele SAN-opslag** met elke privécloud. Elke vSphere wordt gemaakt met een eigen gegevensarchief virtueel SAN. Zie voor meer informatie, [onderdelen in de privécloud-VMware - virtuele SAN-opslag](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage).

**Is de versleuteling van gegevens die worden ondersteund?**
Ja. U kunt instellen van de virtueel SAN-opslag in uw privécloud een key management server (KMS), die on-premises geïmplementeerd gebruiken of op Azure voor het versleutelen van gegevens die zijn opgeslagen op een virtueel SAN.

**Hoe worden de schijven verwerkt?**

Alle hardware-onderdelen van de privécloud CloudSimple bewaking continu worden bewaakt. Als schijf storing wordt gedetecteerd of een schijf als mislukken op basis van heuristiek wordt geïdentificeerd, wordt een nieuw knooppunt automatisch toegevoegd aan de privécloud. Het knooppunt met een beschadigde schijf wordt verwijderd uit de privécloud.

## <a name="vmware"></a>VMware

**Hoe voer ik uploaden van grote schaal en de migratie van toepassingen en gegevens van on-premises?**

CloudSimple biedt een systeemeigen VMware vSphere-oplossing. Een hulpprogramma voor migratie van grote hoeveelheden gegevens kan worden gebruikt met een privécloud CloudSimple. Enkele van de beschikbare opties zijn:

- VMware HCX voor bulksgewijs migratie van gegevens.
- Koude migratie van gegevens met behulp van Storage vMotion van on-premises naar CloudSimple.

**Kan ik een VMware-hulpprogramma's installeren?**

CloudSimple biedt een systeemeigen VMware vSphere-oplossing. Een hulpprogramma dat wordt gebruikt voor het beheren van een on-premises kunnen worden gebruikt op CloudSimple vSphere-omgeving. CloudSimple ondersteunt een bring-your-own-license (BYOL)-model voor het installeren van VMware-hulpprogramma's.

**Hoe worden updates en upgrades beheerd?**

CloudSimple beheert en alle onderdelen van de infrastructuur van uw privécloud-updates in een naadloze ongestoord manier. Een update of de beveiligingspatch vrijgegeven door leveranciers van VMware of infrastructure is gepland voor update zodra deze gekwalificeerd door CloudSimple.

CloudSimple uitvoeren niet voor upgrades of updates van toepassingen worden geïnstalleerd op de privécloud.

## <a name="azure-integration"></a>Integratie van Azure

**Welke Azure-services worden ondersteund?**

CloudSimple biedt Azure ExpressRoute-verbinding met uw abonnement op Azure. Alle services die worden uitgevoerd in uw abonnement op het netwerk verbonden zijn met uw persoonlijke cloud en verbinding kunnen maken met uw persoonlijke cloud. Voorbeelden:

- **Azure Active Directory**: Gebruik Azure Active Directory als de identiteitsbron van een voor uw vCenter CloudSimple.
- **Azure Storage**: Storage gebruiken voor het opslaan van back-ups, afbeeldingen en andere gegevens van uw privécloud.
- **Hybride toepassingen**: Toepassingsarchitectuur die openbare en privéclouds omvat, kunt u maken. Bijvoorbeeld, kunt u webservers in Azure die toegang tot toepassingen en de database-servers in een privécloud CloudSimple.
- **Azure Monitor** en **Azure Security Center**: Werkbelasting die wordt uitgevoerd op VMware kunt bewaken en Security Center gebruiken voor logboekregistratie, metrische gegevens voor prestaties en beveiligingsbeheer.

**Hoe ik mijn tenants voor VMware naar Azure toewijzen?**

CloudSimple biedt de unieke mogelijkheid voor het beheren van uw virtuele VMware-machines in een privécloud van Azure portal. Een vCenter-resourcegroep geconfigureerd met de resourcebeperkingen die u wilt dat door de globale beheerder kunnen worden toegewezen aan uw abonnement. 

**Welke licentievoordelen krijg ik met Azure?**

Met CloudSimple, kunt u profiteren van Azure Hybrid Benefit en tot 90 procent besparen op licenties voor het behouden van uw investering in Microsoft-licenties en verlaag uw totale eigendomskosten in vergelijking met andere clouds. U kunt beveiligingsupdates ook ophalen uitgebreid voor Windows Server 2008 en Microsoft SQL Server 2008. Houd de kosten voor de laag met BYOL naar de cloud voor veelgebruikte apps, zoals Veeam, Zerto en anderen. 
