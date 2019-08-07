---
title: Veelgestelde vragen-VMware-oplossing per CloudSimple
description: Veelgestelde vragen over de Azure VMware-oplossing per CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 80380a1c33927029e000e59a5834f297340f5be3
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816239"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>Veelgestelde vragen over VMware-oplossing per CloudSimple

Veelgestelde vragen en antwoorden over de Azure VMware-oplossing door CloudSimple, waarmee u de service kunt begrijpen en hoe u deze kunt gebruiken. De vragen en antwoorden zijn ingedeeld in de volgende categorieën:

* CloudSimple-service
* Connectiviteit
* Netwerken
* Beveiliging
* Compute
* Storage
* VMware
* Azure-integratie
 
## <a name="cloudsimple-service"></a>CloudSimple-service

**Wat is de Azure VMware-oplossing op CloudSimple?**

Met de Azure VMware-oplossing van CloudSimple worden VMware-workloads in enkele minuten getransformeerd en uitgebreid naar persoonlijke, specifieke Clouds op Azure. De oplossings voorzieningen, beheert de-infra structuur en organiseert de werk belastingen tussen on-premises en Azure. Omdat uw apps precies dezelfde on-premises en in Azure worden uitgevoerd, kunt u profiteren van de elasticiteit en de services van de Cloud, zonder de complexiteit van het opnieuw ontwerpen van uw apps. CloudSimple verlaagt uw total cost of ownership met een model voor Cloud verbruik dat op aanvraag voorziet van inrichtingen, betalen per gebruik en capaciteits optimalisatie. Zie [Wat is Azure VMware-oplossing door CloudSimple?](cloudsimple-vmware-solutions-overview.md)voor functies, voor delen en scenario's.

**Wat is een CloudSimple-privécloud?**

U richt een persoonlijke, specifieke Cloud in die bestaat uit een hoogwaardige compute-, opslag-en netwerk omgeving die is geïmplementeerd op Microsoft Azure infrastructuur (hardware-en datacenter ruimte) in azure-locaties. De privécloud biedt een systeem eigen VMware-platform als een service. In VMware-voor waarden bevat elke privécloud precies één exemplaar van de vCenter Server. De vCenter Server beheert meerdere ESXi-knoop punten in een of meer vSphere-clusters, samen met de bijbehorende vSAN-opslag. Een CloudSimple-service kan meerdere persoonlijke Clouds in uw Azure-abonnement bevatten. Zie [overzicht van privécloud](cloudsimple-private-cloud.md)voor meer informatie over persoonlijke Clouds.

**Waar is de CloudSimple-service beschikbaar?**

CloudSimple is beschikbaar in de regio's VS-Oost en VS-West.

**Hoe kan ik mijn abonnement voor CloudSimple inschakelen?**

Neem contact op met uw [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) Microsoft-account-vertegenwoordiger om uw abonnement voor de CloudSimple-service in te scha kelen. Geef uw abonnements-ID op in het e-mail adres waarvoor u de CloudSimple-service wilt inschakelen. 

**Hoe kan ik toegang tot de CloudSimple-Portal?**

U hebt toegang tot de CloudSimple-portal via de Azure Portal. Zie voor meer informatie over het openen van de CloudSimple-Portal [toegang tot de VMware-oplossing door CloudSimple portal van de Azure Portal](https://docs.azure.cloudsimple.com/access-cloudsimple-portal).

**Hoe kan ik capaciteit voor een privécloud verg Roten?**

U richt knooppunten in vanuit de Azure Portal en breidt uw privécloud uit vanuit de CloudSimple-Portal. U kunt uw privécloud uitbreiden door knoop punten toe te voegen aan een bestaand vSphere-cluster of door een nieuw vSphere-cluster te maken. Zie [een CloudSimple-Privécloud uitbreiden](https://docs.azure.cloudsimple.com/expand-private-cloud)voor meer informatie over de procedure.

**Wat gebeurt er met mijn privécloud tijdens het onderhoud?**

CloudSimple biedt periodieke meldingen dagen voor gepland onderhoud. Onderhoud vindt plaats op een ononderbroken manier om de beschik baarheid van uw privécloud te garanderen. Onderhoud kan van de volgende typen zijn:

- **CloudSimple-infra structuur**: De CloudSimple-infra structuur is ontworpen om Maxi maal beschikbaar te zijn. Tijdens het onderhoud wordt de connectiviteit en beschik baarheid van uw privécloud gegarandeerd door het bijwerken van redundante onderdelen één op een tijdstip zonder enige impact. U hebt toegang tot uw persoonlijke Cloud-vCenter, alle virtuele machines, Internet verbinding vanuit uw privécloud en verbindingen met on-premises of Azure.
- **CloudSimple-Portal**: Tijdens het onderhoud zijn sommige functies van de CloudSimple-Portal mogelijk niet toegankelijk of worden deze mogelijk uitgeschakeld. De onderhouds melding bevat informatie over wat er op de portal kan worden uitgevoerd.

## <a name="connectivity"></a>Connectiviteit

**Wat zijn mijn connectiviteits opties voor mijn CloudSimple Region-netwerk?**

CloudSimple biedt drie verschillende connectiviteits opties om verbinding te maken met uw CloudSimple-regio netwerk. Alle drie de opties kunnen samen worden gebruikt:

- Azure ExpressRoute-verbinding van uw on-premises Data Center naar CloudSimple Region Network: Een beveiligde persoonlijke particuliere verbinding met hoge snelheid met lage latentie die uw on-premises ExpressRoute-circuit met uw CloudSimple ExpressRoute-circuit vastbrugt met behulp van Global Reach. Zie [verbinding maken vanaf on-premises naar CloudSimple met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)om de verbinding in te stellen.
- ExpressRoute-verbinding van uw virtuele Azure-netwerk naar uw CloudSimple regio netwerk: Een veilige persoonlijke particuliere verbinding met hoge snelheid en lage latentie waarmee uw virtuele netwerk op Azure wordt gebrugd met uw CloudSimple ExpressRoute-circuit met behulp van virtuele netwerk gateways. Zie [uw CloudSimple Private Cloud Environment verbinden met het virtuele Azure-netwerk met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/azure-expressroute-connection)om de verbinding in te stellen.
- Site-naar-site-VPN-verbinding van uw on-premises Data Center naar uw CloudSimple Region-netwerk: Een beveiligd virtueel particulier netwerk van uw on-premises VPN-apparaat naar uw CloudSimple Private Cloud-regio. Zie [een VPN-verbinding instellen tussen uw on-premises netwerk en de privécloud van CloudSimple](https://docs.azure.cloudsimple.com/set-up-vpn)voor meer informatie over het instellen van de verbinding.

**Hoe kan ik verbinding maken met een privécloud?**

U kunt de details van uw privécloud weer geven in de CloudSimple-Portal. Als u verbinding wilt maken met de vCenter die overeenkomt met uw privécloud, controleert u of er een netwerk verbinding tot stand is gebracht met behulp van site-naar-site-, punt-naar-site-of ExpressRoute. Start vervolgens de CloudSimple-Portal vanuit de Azure Portal. Selecteer **vSphere-client starten** op de start pagina of op de pagina met details van de privécloud.

**Wat is het voor deel van een ExpressRoute-circuit?**

Een Azure ExpressRoute-circuit voorziet in een snelle en lage latentie veilige verbinding. CloudSimple biedt een speciaal ExpressRoute circuit per regio per klant. Met dit circuit kunt u een veilige verbinding tot stand brengen vanuit een on-premises en uw Azure-abonnement.

**Wat zijn de netwerk kosten om verbinding te maken met en van CloudSimple? Zijn er kosten voor uitgaand verkeer naar en van CloudSimple naar Azure? Zijn er uitstaande kosten in rekening gebracht voor regio's?**

Er worden geen kosten in rekening gebracht voor netwerk uitgaand verkeer. De standaard tarieven van Azure gelden voor elk uitgaand verkeer van uw virtuele netwerk of van een on-premises ExpressRoute-circuit.

## <a name="networking"></a>Netwerken

**Welke netwerk functies zijn beschikbaar voor mijn privécloud?**

U kunt VLAN'S en hun subnetten en firewall tabellen inrichten. U kunt open bare IP-adressen toewijzen en toewijzen aan een virtuele machine die wordt uitgevoerd in uw privécloud. Zie voor meer informatie [vlan's en subnetten Overview](cloudsimple-vlans-subnets.md), overzicht van [firewall tabellen](cloudsimple-firewall-tables.md)en overzicht van [open bare IP-adressen](cloudsimple-public-ip-address.md).

**Hoe kan ik verschillende subnetten instellen voor mijn toepassingen in mijn privécloud?**

U kunt VLAN'S maken op uw privécloud vanuit uw CloudSimple-Portal. Nadat u het VLAN hebt gemaakt, kunt u een gedistribueerde poort groep maken op uw privécloud met behulp van het VLAN en virtuele machines maken die zijn verbonden met de gedistribueerde poort groep. U kunt een firewall tabel inschakelen voor het VLAN of subnet en firewall regels definiëren om netwerk verkeer te beveiligen.

**Welke firewall instellingen zijn beschikbaar voor mijn persoonlijke Clouds?**

U kunt regels voor Noord-Zuid-en Oost-West-verkeer configureren. De regels worden gedefinieerd in een firewall tabel. De firewall tabel kan worden gekoppeld aan VLAN'S in uw privécloud. Zie voor de installatie procedure [firewall tabellen en regels voor persoonlijke Clouds instellen](https://docs.azure.cloudsimple.com/firewall).

**Kan ik open bare IP-adressen voor Vm's toewijzen in mijn privécloud?**

In de CloudSimple-Portal kunt u eenvoudig een nieuw openbaar IP-adres toewijzen en dit koppelen aan een privé-IP-adres van uw virtuele machine of een apparaat. U kunt ook nieuwe firewall regels maken of bestaande firewall regels Toep assen om verkeer toe te staan van specifieke poorten en specifieke sets IP-adressen in de portal. Zie voor de installatie procedure [open bare IP-adressen toewijzen voor een privécloud](https://docs.azure.cloudsimple.com/public-ips).

## <a name="security"></a>Beveiliging

**Wat zijn mijn beveiligings opties voor CloudSimple?**

Een CloudSimple-privécloud biedt de volgende beveiligings functies voor het beveiligen van uw privécloud:

- **Data-at-rest versleuteling:** U kunt gegevens in rust versleutelen die zich in vSAN-opslag in uw privécloud bevinden. vSAN biedt ondersteuning voor een externe sleutel beheer server, die kan worden geïmplementeerd in uw virtuele Azure-netwerk of in een on-premises omgeving. Zie [vSAN-versleuteling voor uw CloudSimple-Privécloud configureren](https://docs.azure.cloudsimple.com/vsan-encryption)voor meer informatie.
- **Netwerk beveiliging:** Beheer netwerk verkeer van en naar uw privécloud vanuit het Internet, on-premises en binnen de subnetten van uw privécloud door gebruik te maken van firewall regels.
- **Beveiligde, particuliere verbinding:** Veilige, particuliere verbinding tussen uw on-premises netwerk en uw Azure-abonnement.

## <a name="compute"></a>Compute

**Wat voor soort hosts zijn er beschikbaar?**

CloudSimple biedt twee typen host:

* **CS28-knoop punt**: CPU: 2x 2,2 GHz, totaal 28 kern geheugens, 48 HT. RAM: 256 GB. Storage: 1600-GB NVMe-cache, 5760-GB gegevens (alle-Flash). Netwerk 2x25Gbe NIC.
* **CS36-knoop punt**: CPU 2x 2,3 GHz, totale 36 kern geheugens 72 HT. RAM: 512 GB. Storage: 3200-GB NVMe cache 11.520-GB gegevens (alle-Flash). Netwerk 2x25Gbe NIC.

**Hoe worden hardwarestoringen afgehandeld?**

Alle CloudSimple-infra structuur wordt voortdurend bewaakt door het CloudSimple-platform en de bijbehorende Service Operations-teams. Als er een hardwarefout wordt gedetecteerd, wordt een nieuw knoop punt toegevoegd aan uw privécloud. Het knoop punt is niet verwijderd om te zorgen voor hoge Beschik baarheid van uw privécloud.

## <a name="storage"></a>Storage

**Welk type opslag wordt ondersteund op een privécloud?**

CloudSimple biedt **alle-Flash VMware vSAN-opslag** met elke privécloud. Elke vSphere wordt gemaakt met een eigen vSAN-gegevens opslag. Zie voor meer informatie [Private Cloud VMware-onderdelen-vSAN-opslag](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage).

**Wordt versleuteling van gegevens ondersteund?**
Ja. U kunt de vSAN-opslag in uw privécloud instellen om een KMS (Key Management Server) te gebruiken, die on-premises of op Azure wordt geïmplementeerd voor het versleutelen van gegevens die zijn opgeslagen op vSAN.

**Hoe worden defecte schijven afgehandeld?**

CloudSimple-bewaking bewaakt voortdurend alle hardwareonderdelen van de privécloud. Als er een schijf fout wordt gedetecteerd of een schijf wordt geïdentificeerd als mislukt op basis van heuristiek, wordt er automatisch een nieuw knoop punt toegevoegd aan de privécloud. Het knoop punt met een mislukte of niet-werkende schijf wordt verwijderd uit de privécloud.

## <a name="vmware"></a>VMware

**Hoe kan ik een grootschalige upload en migratie van toepassingen en gegevens van on-premises uitvoeren?**

CloudSimple biedt een systeem eigen VMware vSphere oplossing. Elk hulp programma dat wordt gebruikt voor bulk gegevens migratie kan worden gebruikt met een CloudSimple-privécloud. Enkele van de beschik bare opties zijn:

- VMware HCX voor het bulksgewijs migreren van gegevens.
- Een koude migratie van gegevens met behulp van Storage vMotion van on-premises naar CloudSimple.

**Kan ik VMware-hulpprogram ma's installeren?**

CloudSimple biedt een systeem eigen VMware vSphere oplossing. Elk hulp programma dat wordt gebruikt om on-premises vSphere-omgeving te beheren, kan worden gebruikt op CloudSimple. CloudSimple biedt ondersteuning voor een BYOL-model (inbreng uw eigen licentie) voor het installeren van VMware-hulpprogram ma's.

**Hoe worden updates en upgrades beheerd?**

CloudSimple beheert en werkt alle infrastructuur onderdelen van uw privécloud op een naadloze, ononderbroken manier toe. Alle updates of beveiligings patches die zijn uitgegeven door VMware of infrastructuur leveranciers, worden gepland voor een update zodra deze is gekwalificeerd door CloudSimple.

CloudSimple voert geen upgrades of updates van toepassingen uit die zijn geïnstalleerd in de privécloud.

## <a name="azure-integration"></a>Azure-integratie

**Welke Azure-Services worden ondersteund?**

CloudSimple biedt Azure ExpressRoute-verbinding met uw abonnement op Azure. Alle services die worden uitgevoerd in uw abonnement, hebben een netwerk verbinding met uw privécloud en kunnen verbinding maken met uw privécloud. Voorbeelden:

- **Azure Active Directory**: Gebruik Azure Active Directory als een id-bron voor uw CloudSimple vCenter.
- **Azure Storage**: Gebruik opslag om back-ups, afbeeldingen en andere gegevens op te slaan vanuit uw privécloud.
- **Hybride toepassingen**: U kunt een toepassings architectuur maken die open bare en persoonlijke Clouds omspant. U kunt bijvoorbeeld webservers maken in azure die toegang hebben tot toepassings-en database servers in een CloudSimple-privécloud.
- **Azure monitor** en **Azure Security Center**: De werk belasting die wordt uitgevoerd op VMware kan monitor en Security Center gebruiken voor logboek registratie, metrische gegevens over prestaties en beveiligings beheer.

**Hoe kan ik mijn VMware-tenants aan Azure toe te wijzen?**

CloudSimple biedt de unieke mogelijkheid om uw VMware-Vm's te beheren op een privécloud vanuit het Azure Portal. Een vCenter-resource groep die is geconfigureerd met de resource beperkingen die u wilt, kunnen worden toegewezen aan uw abonnement door de globale beheerder. 

**Welke licentie voordelen krijg ik bij Azure?**

Met CloudSimple kunt u profiteren van de Azure Hybrid Benefit en bespaart u tot 90 procent op licenties om uw investering in micro soft-licenties te bewaren en uw total cost of ownership te verlagen ten opzichte van andere Clouds. U krijgt ook uitgebreide beveiligings updates voor Windows Server 2008 en Microsoft SQL Server 2008. Blijf uw kosten laag met BYOL naar de Cloud voor algemene apps zoals Veeam, Zerto en anderen. 
