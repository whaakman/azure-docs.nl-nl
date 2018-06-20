---
title: Migreren van de Azure - Veelgestelde vragen (FAQ) | Microsoft Docs
description: Adressen Veelgestelde vragen over Azure migreren
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/06/2018
ms.author: snehaa
ms.openlocfilehash: a18cab73a019039bf5e5829ad1faa4b8f1a70391
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209942"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Migreren van de Azure - Veelgestelde vragen (FAQ)

Dit artikel bevat veelgestelde vragen over Azure migreren. Als u aanvullende vragen na het lezen van dit artikel hebt, plaatst u deze op de [forum Azure migreren](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Algemeen

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Verschilt Azure migreren vanuit Azure Site Recovery?

Azure migreren is een beoordeling service waarmee u de werkbelasting van uw lokale detecteren en plan de migratie naar Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), samen met alleen een noodherstel, helpt u bij het migreren van lokale werkbelastingen voor IaaS VM's in Azure. 

### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Hoe wordt Azure migreren vanuit Azure Site Recovery implementatie Planner verschillende?

Azure migreren is een hulpprogramma voor migratieplanning en Azure Site Recovery implementatie Planner is een hulpprogramma voor het plannen noodherstel (DR).

**Migratie van VMware naar Azure**: als u van plan bent uw lokale werkbelastingen naar Azure migreren, gebruikt u Azure migreren voor beheerders voor migratieplanning. Azure migreren evalueert voor lokale werkbelastingen en biedt richtlijnen en inzichten mechanismen om u te helpen migreren naar Azure. Wanneer u klaar met u van plan bent bent, kunt u services, zoals Azure Site Recovery en Azure Database migratie-Service voor het migreren van de machines naar Azure.

**Migratie van Hyper-V naar Azure**: Azure migreren ondersteunt momenteel alleen beoordeling van de virtuele VMware-machines op de migratie naar Azure. Ondersteuning voor Hyper-V is op het overzicht voor het migreren van Azure. In de tussentijd kunt u ASR implementatie Planner. Wanneer Hyper-V-ondersteuning is ingeschakeld in Azure migreren, kunt u Azure migreren kunt gebruiken voor het plannen van de migratie van Hyper-V-werkbelastingen.

**Herstel na noodgevallen van VMware/Hyper-V naar Azure**: als u van plan bent te doen noodherstel (DR) op Azure met Azure Site Recovery (ASR), gebruikt u ASR implementatie Planner voor herstel na Noodgevallen plannen. ASR implementatie Planner biedt een diepgaande, ASR-specifieke beoordeling van uw on-premises omgeving. Deze aanbevelingen die door ASR voor geslaagde DR-bewerkingen zoals replicatie, failover van uw virtuele machines vereist zijn.  

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Heeft Azure migreren vCenter-Server voor het detecteren van een VMware-omgeving nodig?

Ja, Azure migreren vereist vCenter-Server voor het detecteren van een VMware-omgeving. Geen biedt ondersteuning voor detectie van ESXi-hosts die niet worden beheerd door een vCenter-Server.

## <a name="discovery"></a>Detectie

### <a name="what-data-is-collected-by-azure-migrate"></a>Welke gegevens worden verzameld door Azure migreren?

Azure migreren ondersteunt twee soorten detectie, detectie op basis van het toestel en detectie op basis van de agent.
De detectie op basis van het toestel verzamelt metagegevens over de lokale virtuele machines, de volledige lijst met metagegevens dat is verzameld door het toestel hieronder wordt:

**Configuratiegegevens van de virtuele machine**
- Weergavenaam van de virtuele machine (op vCenter)
- VM-inventarisatie-pad (host of cluster/de map in vCenter)
- IP-adres
- MAC-adres
- Besturingssysteem
- Het aantal kernen, schijven, NIC 's
- Grootte van het systeemgeheugen, schijfgrootten

**Prestatiegegevens van de virtuele machine**
- CPU-gebruik
- Geheugengebruik
- Voor elke schijf die is gekoppeld aan de virtuele machine:
  - Schijf lezen doorvoer
  - Schijf schrijven doorvoer
  - Schijf leesbewerkingen per seconde
  - Schijf schrijven per seconde
- Voor elke netwerkadapter die is gekoppeld aan de virtuele machine:
  - Het netwerk
  - Uit het netwerk

De detectie op basis van een agent is een optie beschikbaar boven op de detectie op basis van het toestel en helpt klanten [visualiseren afhankelijkheden](how-to-create-group-machine-dependencies.md) van de on-premises virtuele machines. De afhankelijkheid agents verzamelen van gegevens, zoals de FQDN-naam, besturingssysteem, IP-adres, MAC-adres, de processen die worden uitgevoerd in de virtuele machine en de binnenkomende/uitgaande TCP-verbindingen van de virtuele machine. De detectie op basis van een agent is optioneel en u kunt de agents niet te installeren als u niet wilt dat de afhankelijkheden van de virtuele machines te visualiseren.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Waar bevindt zich de verzamelde gegevens opgeslagen en hoe lang?

De gegevens die door het toestel collector zijn verzameld, wordt opgeslagen in de Azure-locatie die u opgeeft tijdens het maken van het migratieproject. De gegevens zijn veilig opgeslagen in een Microsoft-abonnement en wordt verwijderd wanneer de gebruiker wordt verwijderd van het project Azure migreren.

Als u agenten op de virtuele machines installeert, wordt de gegevens die worden verzameld door de agents afhankelijkheid voor afhankelijkheid visualisatie opgeslagen in de Verenigde Staten in een OMS-werkruimte gemaakt in het abonnement van de gebruiker. Deze gegevens worden verwijderd wanneer de gebruiker de OMS-werkruimte in haar of zijn abonnement verwijdert. [Meer informatie](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Hoe de collector communiceren met de vCenter-Server en de service Azure migreren?

Het toestel collector verbinding maakt met de vCenter-Server (poort 443) met de referenties die zijn opgegeven door de gebruiker in het toestel. De vCenter-Server met VMware PowerCLI voor het verzamelen van metagegevens over de virtuele machines die worden beheerd door de vCenter-Server opgevraagd. Worden verzameld beide configuratiegegevens over virtuele machines (kernen, geheugen, schijven, NIC enz.) en de geschiedenis van elke virtuele machine van de laatste één maand van de vCenter-Server. De verzamelde metagegevens wordt dan gezonden naar de service Azure migreren (via internet via https) voor evaluatie. [Meer informatie](concepts-collector.md)

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>De gegevens worden versleuteld in rust en onderweg?

Ja, de verzamelde gegevens worden versleuteld zowel in rust en onderweg. De metagegevens die zijn verzameld door het toestel veilig verzonden naar de service Azure migreren via internet via https. De verzamelde metagegevens worden opgeslagen in [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) en in [Azure blob-opslag](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in een Microsoft-abonnement en in rust versleuteld.

De gegevens die worden verzameld door de agents afhankelijkheid is ook versleuteld in doorvoer (beveiligde https-kanaal) en wordt opgeslagen in een werkruimte voor logboekanalyse in het abonnement van de gebruiker. Het is ook in rust versleuteld.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Hoe kan ik een omgeving met meerdere tenants in Azure migreren detecteren?

Als u een omgeving die worden gedeeld door tenants hebt en u niet wilt detecteren van de virtuele machines van een tenant in een andere tenant abonnement, kunt u het veld bereik in het toestel collector als bereik voor de detectie. Als de tenants hosts deelt, een referentie die alleen-lezen toegang heeft tot alleen de virtuele machines die horen bij de specifieke tenant maken en deze referentie gebruiken in het toestel collector en het bereik opgeven als host voor de detectie uitvoeren. Als alternatief kunt u ook mappen maken in vCenter Server (Stel Map1 voor tenant1 en folder2 voor tenant2), onder de gedeelde host, de virtuele machines voor tenant1 in Map1 en tenant2 verplaatsen naar folder2 en vervolgens de detecties in de collector dienovereenkomstig bereik door op te geven op de juiste map.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Het aantal virtuele machines kunnen worden gevonden in een project voor afzonderlijke migratie?

U kunt 1500 virtuele machines in een project voor afzonderlijke migratie detecteren. Als u meer computers in uw on-premises-omgeving hebt [meer](how-to-scale-assessment.md) over hoe u een grote omgeving in Azure migreren kunt detecteren.

## <a name="dependency-visualization"></a>Visualisatie van afhankelijkheden

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Heb ik nodig om te betalen de afhankelijkheid-visualisatie gebruiken?

Azure Migrate is zonder extra kosten beschikbaar. Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kan ik een bestaande werkruimte voor afhankelijkheid visualisatie gebruiken?

Azure migreren biedt geen ondersteuning voor het gebruik van een bestaande werkruimte voor afhankelijkheid visualisatie, echter Microsoft Monitoring Agent (MMA) biedt ondersteuning voor multihoming en kunt u gegevens verzenden naar meerdere werkruimten. Dus als u de agents geïmplementeerd en geconfigureerd met een werkruimte al hebt, kunt u gebruikmaken van multihoming in de agent MMA en deze configureren in de werkruimte Azure migreren (in aanvulling op de bestaande werkruimte) en werken. [Hier](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) is een blog over hoe u multihoming in een agent MMA kunt inschakelen.

## <a name="next-steps"></a>Volgende stappen

- Lees de [overzicht Azure migreren](migrate-overview.md)
- Meer informatie over hoe u kunt [detecteren en te beoordelen](tutorial-assessment-vmware.md) een omgeving met VMware
