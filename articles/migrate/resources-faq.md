---
title: Azure Migrate - Frequently Asked Questions (FAQ) | Microsoft Docs
description: Veelgestelde vragen over Azure Migrate adressen
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/03/2018
ms.author: snehaa
ms.openlocfilehash: f4ce2130b18b183f633c649f98fc1add30753a27
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296004"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate - Asked Frequently Questions (FAQ)

Dit artikel bevat veelgestelde vragen over Azure Migrate. Hebt u geen verdere query's na het lezen van dit artikel, plaatst u deze op de [Azure Migrate forum](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Algemeen

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>Biedt Azure Migrate ondersteuning voor beoordeling van de VMware-workloads?

Ja, Azure Migrate ondersteunt momenteel alleen beoordeling van de VMware-workloads. Ondersteuning voor Hyper-V en fysieke servers wordt in de toekomst worden ingeschakeld.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Heeft Azure Migrate vCenter-Server voor het detecteren van een VMware-omgeving nodig?

Ja, vereist Azure Migrate vCenter-Server voor het detecteren van een VMware-omgeving. Het biedt geen ondersteuning voor detectie van ESXi-hosts die niet worden beheerd door een vCenter-Server.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Wat is Azure Migrate verschil met Azure Site Recovery?

Azure Migrate is een service voor beveiligingsbeoordeling die helpt u bij het detecteren van uw on-premises workloads en plan uw migratie naar Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), samen met enkel een oplossing voor noodherstel, helpt u bij het migreren van on-premises werkbelastingen naar IaaS-VM's in Azure.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>Wat is het verschil tussen het gebruik van Azure Migrate voor beoordelingen en de Map Toolkit?

[Azure Migrate](migrate-overview.md) biedt migratie-analyse met name om te helpen bij de voorbereiding op de migratie en evaluatie van de on-premises werkbelastingen in Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=7826) heeft andere functies. Voorbeeld: de migratie plannen voor nieuwere versies van Windows-client en server-besturingssystemen, software gebruik bijhouden enzovoort. Voor deze scenario's, echter ook doorgaan met de MAP Toolkit.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Wat is Azure Migrate verschil met Azure Site Recovery Deployment Planner?

Azure Migrate is een hulpprogramma voor migratieplanning en Azure Site Recovery Deployment Planner is een hulpprogramma voor het plannen noodherstel (DR).

**Migratie van VMware naar Azure**: als u van plan bent uw on-premises workloads migreren naar Azure, Azure Migrate gebruiken voor het plannen van de migratie. Azure Migrate beoordeelt on-premises werkbelastingen en biedt richtlijnen, inzichten en mechanismen om u te helpen bij het migreren naar Azure. Wanneer u klaar met uw migratieplan bent, kunt u services zoals Azure Site Recovery en Azure Database Migration Service de machines migreren naar Azure.
 
**Migratie van Hyper-V naar Azure**: Azure Migrate ondersteunt momenteel alleen evaluatie van virtuele VMware-machines voor migratie naar Azure. Ondersteuning voor Hyper-V is op de roadmap voor Azure Migrate. In de tussentijd kunt u Site Recovery Deployment Planner. Zodra de Hyper-V-ondersteuning is ingeschakeld in Azure Migrate, kunt u Azure Migrate kunt gebruiken voor het plannen van de migratie van Hyper-V-werkbelastingen.

**Herstel na noodgevallen van VMware/Hyper-V naar Azure**: als u van plan bent om te doen, herstel na noodgeval (DR) op Azure met behulp van Azure Site Recovery (Site Recovery), gebruikt u Site Recovery Deployment Planner voor herstel na Noodgevallen plannen. Site Recovery Deployment Planner biedt een diepgaande, ASR-specifieke beoordeling van uw on-premises omgeving. Het biedt aanbevelingen die door Site Recovery voor geslaagde DR-bewerkingen, zoals replicatie, failover van uw virtuele machines vereist zijn.  

### <a name="which-azure-regions-are-supported-by-azure-migrate"></a>Welke Azure-regio's worden ondersteund door Azure Migrate?

Azure Migrate ondersteunt momenteel VS-Oost en West-Centraal VS als de locaties van migratie. Houd er rekening mee dat hoewel in West-Centraal VS en VS-Oost, u alleen migration-projecten maken kunt, kunt u nog steeds uw machines beoordelen voor [doellocaties voor meerdere](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). De projectlocatie wordt alleen gebruikt voor het opslaan van de gedetecteerde gegevens.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>Hoe worden de on-premises site, maakt verbinding met Azure Migrate?

De verbinding kan worden via internet of het gebruik van ExpressRoute met openbare peering.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>Kan ik beveiliging van de virtuele machine instellen met de. OVA-sjabloon?

Extra onderdelen (zoals antivirusprogramma's) kunnen worden toegevoegd aan de. OVA-sjabloon is, zolang de communicatie en firewall-regels die zijn vereist voor het apparaat Azure Migrate werken worden gehandhaafd.   

## <a name="discovery-and-assessment"></a>Detectie en evaluatie

### <a name="what-data-is-collected-by-azure-migrate"></a>Welke gegevens worden verzameld door Azure Migrate?

Azure Migrate ondersteunt twee soorten detectie, detectie op basis van een apparaat en op basis van een agent.
De detectie op basis van een apparaat verzamelt metagegevens over de on-premises VM's, de volledige lijst met metagegevens die zijn verzameld door het apparaat vindt u hieronder:

**Configuratiegegevens van de virtuele machine**
- Naam van de virtuele machine weergegeven (op vCenter)
- VM-inventarisatie-pad (host/cluster/map in vCenter)
- IP-adres
- MAC-adres
- Besturingssysteem
- Aantal kernen, schijven, NIC 's
- Grootte van geheugen, schijf-grootten

**Prestatiegegevens van de virtuele machine**
- CPU-gebruik
- Geheugengebruik
- Voor elke schijf die is gekoppeld aan de virtuele machine:
  - Schijf lezen doorvoer
  - Doorvoer van schrijfbewerkingen per schijf
  - Schijf lezen-bewerkingen per seconde
  - Schijf schrijft-bewerkingen per seconde
- Voor elke netwerkadapter die is gekoppeld aan de virtuele machine:
  - Het netwerk
  - Netwerk-uitgaand

De detectie op basis van een agent is een optie die beschikbaar zijn op de detectie op basis van een apparaat en helpt klanten [afhankelijkheden visualiseren](how-to-create-group-machine-dependencies.md) van de on-premises virtuele machines. De agents afhankelijkheid verzamelen van gegevens, zoals besturingssysteem, FQDN-naam-IP-adres, MAC-adres, processen die op de virtuele machine en de inkomende/uitgaande TCP-verbindingen van de virtuele machine. De detectie op basis van een agent is optioneel en u kunt de agents niet te installeren als u niet wilt visualiseren van de afhankelijkheden van de virtuele machines.

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>Zou er prestatie-invloed op de geanalyseerde ESXi-host-omgeving?

Omdat we de gegevens worden verzameld via de vCenter-server, moet u er geen invloed op de prestaties is op de ESXi-hosts. Zelfs op de vCenter-server is er bijna geen invloed op de prestaties.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Waar worden de verzamelde gegevens opgeslagen en hoe lang?

De gegevens die zijn verzameld door de collector-apparaat is opgeslagen in de Azure-locatie die u opgeeft tijdens het maken van het migratieproject. De gegevens worden veilig opgeslagen in een Microsoft-abonnement en wordt verwijderd wanneer de gebruiker wordt verwijderd van de Azure Migrate-project.

Als u agents op de virtuele machines installeren, wordt de gegevens die zijn verzameld door de agents van de afhankelijkheid voor visualisatie van afhankelijkheden opgeslagen in de Verenigde Staten in een OMS-werkruimte hebt gemaakt in het abonnement van de gebruiker. Deze gegevens worden verwijderd wanneer u de OMS-werkruimte in uw abonnement verwijdert. [Meer informatie](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>De gegevens worden versleuteld in rust en onderweg zijn?

Ja, de verzamelde gegevens worden versleuteld in rust en onderweg zijn. De metagegevens die zijn verzameld door het apparaat veilig verzonden naar de Azure Migrate-service via internet via https. De verzamelde metagegevens worden opgeslagen in [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) en [Azure blob-opslag](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in een Microsoft-abonnement en in rust worden versleuteld.

De gegevens die zijn verzameld door de agents afhankelijkheid is ook versleuteld in doorvoer (beveiligde https-kanaal) en wordt opgeslagen in een Log Analytics-werkruimte in het abonnement van de gebruiker. Het is ook versleuteld in rust.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Hoe de collector communiceren met de vCenter-Server en de service Azure Migrate?

Het collector-apparaat verbinding maakt met de vCenter-Server (poort 443) met behulp van de referenties die zijn opgegeven door de gebruiker in het apparaat. Deze query uitgevoerd voor de vCenter-Server met behulp van VMware PowerCLI voor het verzamelen van metagegevens over de VM's beheerd door vCenter Server. Deze verzamelt beide configuratiegegevens over VM's (kernen, geheugen, schijven, NIC enzovoort) en de prestatiegeschiedenis van elke virtuele machine voor de laatste maand van vCenter-Server. De verzamelde metagegevens wordt verzonden naar de service Azure Migrate (via internet via https) voor de beoordeling. [Meer informatie](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>Kan ik het dezelfde collector-apparaat verbinden met meerdere vCenter-servers?

Ja, een enkel collector-apparaat kan worden gebruikt voor het detecteren van meerdere vCenter-Servers, maar niet gelijktijdig. U moet de detecties na elkaar uitgevoerd.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Is de. OVA-sjabloon die wordt gebruikt door Site Recovery worden geïntegreerd met de. OVA die worden gebruikt door Azure Migrate?

Er is momenteel geen-integratie. De. OVA-sjabloon in Site Recovery wordt gebruikt voor het instellen van een Site Recovery-configuratieserver voor replicatie van virtuele VMware-machine/fysieke server. De. OVA die worden gebruikt door Azure Migrate wordt gebruikt voor het detecteren van VMware-VM's beheerd door een vCenter-server voor de doeleinden van migratie-analyse.

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>Ik heb mijn machinegrootte gewijzigd. Kan ik de evaluatie opnieuw uitvoeren?

Als u wijzigt de instellingen op een virtuele machine die u wilt beoordelen, Ontdek trigger opnieuw met behulp van het collector-apparaat. Gebruik in het toestel, de **verzamelen opnieuw starten** optie om dit te doen. Nadat de verzameling gereed is, selecteert u in de portal de optie **Opnieuw berekenen** voor de evaluatie om de evaluatieresultaten op te halen.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Hoe kan ik een multitenant-omgeving in Azure Migrate detecteren?

Als u een omgeving die wordt gedeeld door tenants hebt en u niet wilt detecteren van de virtuele machines van één tenant in een andere tenant-abonnement, kunt u het veld bereik in het collector-apparaat als bereik voor de detectie. Als de tenants hosts delen, een referentie met alleen-lezen toegang tot alleen de virtuele machines die horen bij de specifieke tenant maken en deze referentie gebruiken in het collector-apparaat en het bereik opgeven als de host te doen de detectie. U kunt ook u kunt ook mappen maken in vCenter Server (Stel Map1 voor tenant1 en folder2 voor tenant2), onder de gedeelde host, de virtuele machines voor tenant1 in Map1 en tenant2 verplaatsen naar folder2 en vervolgens het bereik van de detecties in de collector dienovereenkomstig door op te geven op de juiste map.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Het aantal virtuele machines kunnen worden gevonden in een enkele migration-project?

U kunt 1500 virtuele machines in een enkele migratieproject detecteren. Als u meer computers in uw on-premises-omgeving hebt [meer](how-to-scale-assessment.md) over hoe u een grote omgeving in Azure Migrate kunt detecteren.

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Ondersteuning voor Azure Migrate Enterprise Agreement (EA) op basis van kostenraming?

Azure Migrate ondersteunt momenteel geen schatting van de kosten voor [Enterprise overeenkomst-aanbieding](https://azure.microsoft.com/offers/enterprise-agreement-support/). De oplossing is om te betalen per gebruik als de aanbieding en handmatig opgeven van het kortingspercentage (van toepassing op het abonnement) in het veld 'Korting' van de evaluatie-eigenschappen opgeven.

  ![Korting](./media/resources-faq/discount.png)

## <a name="dependency-visualization"></a>Visualisatie van afhankelijkheden

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Moet ik betalen voor het gebruik van de functie voor visualisatie van afhankelijkheden?

Azure Migrate is zonder extra kosten beschikbaar. Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kan ik een bestaande werkruimte gebruiken voor visualisatie van afhankelijkheden?

Azure Migrate biedt geen ondersteuning voor het gebruik van een bestaande werkruimte voor visualisatie van afhankelijkheden, echter Microsoft Monitoring Agent (MMA) biedt ondersteuning voor multihoming en kunt u gegevens verzenden naar meerdere werkruimten. Dus als u al de agents geïmplementeerd en geconfigureerd met een werkruimte hebt, kunt u gebruikmaken van multihoming in de MMA-agent en configureert in de werkruimte Azure Migrate (naast de bestaande werkruimte) en daarvan. [Hier](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) is een blog over hoe u multihoming in een MMA-agent kunt inschakelen.

## <a name="next-steps"></a>Volgende stappen

- Lees de [Azure Migrate-overzicht](migrate-overview.md)
- Leer hoe u kunt [detecteren en evalueren](tutorial-assessment-vmware.md) een VMware-omgeving
