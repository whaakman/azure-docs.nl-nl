---
title: Azure Migrate - Frequently Asked Questions (FAQ) | Microsoft Docs
description: Veelgestelde vragen over Azure Migrate adressen
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: snehaa
ms.openlocfilehash: 08a0312f12b3daab8b7f5e88da118b5bcbeb2f4c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807329"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate - Asked Frequently Questions (FAQ)

Dit artikel bevat veelgestelde vragen over Azure Migrate. Hebt u geen verdere query's na het lezen van dit artikel, plaatst u deze op de [Azure Migrate forum](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Algemeen

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Welke Azure-regio's worden ondersteund door Azure Migrate?
Azure Migrate ondersteunt momenteel een aantal verschillende geografische gebieden waarin een Azure Migrate-project kan worden gemaakt. Hoewel u alleen projecten in deze regio's maken kunt, kunt u nog steeds uw machines voor andere doellocaties beoordelen. De geografische locatie van project wordt alleen gebruikt voor het opslaan van de gedetecteerde metagegevens.

**Geografie** | **metagegevens opslaglocatie** Azure Government | VS (overheid) Virginia Azië | Zuidoost-Azië of Europa Oost-Azië | Zuid-Europa of West-Europa Verenigd Koninkrijk | UK-Zuid of Verenigd Koninkrijk West Verenigde Staten | -Centraal VS of VS-West 2

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Wat is Azure Migrate verschil met Azure Site Recovery?

Azure Migrate biedt hulpprogramma's die u helpen te detecteren, beoordelen en migreren van computers en werkbelastingen naar Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) is een oplossing voor noodherstel. Beide services deelt sommige onderdelen.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Azure Migrate-apparaat (VMware/fysieke servers)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Hoe het apparaat Azure Migrate verbinding met Azure?

De verbinding kan via internet, of kunt u ExpressRoute met openbare peering.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate-server-assessment-and-migration"></a>Welke netwerkverbindingsvereisten nodig zijn voor Azure Migrate-Server-evaluatie en migratie

Voor de URL's en de poorten die nodig zijn voor Azure Migrate om te communiceren met Azure, raadpleegt u de [VMWare](migrate-support-matrix-vmware.md) en [Hyper-V](migrate-support-matrix-hyper-v.md) matrices ondersteunen.

### <a name="can-i-harden-the-appliance-vmware-vm-i-set-up-with-the-ova-template"></a>Kan ik beveiliging van het apparaat kan ik met het OVA-sjabloon instellen voor VMware-VM?

Extra onderdelen (zoals antivirusprogramma's) kunnen worden toegevoegd in het OVA-sjabloon, zolang de communicatie en firewall-regels voor het apparaat Azure Migrate re links als vereist is.   

### <a name="to-harden-the-azure-migrate-appliance-what-are-the-recommended-antivirus-av-exclusions"></a>Voor het beperken van het apparaat Azure Migrate, wat zijn de aanbevolen uitsluitingen van Antivirus (AV)?

U moet de volgende mappen uitsluiten van scannen op het apparaat:

- Map met binaire bestanden voor de Azure Migrate-Service. Sluit alle submappen.
- %ProgramFiles%\ProfilerService  
- Azure Migrate-webtoepassing. Sluit alle submappen.
- %SystemDrive%\inetpub\wwwroot
- De lokale cache voor de database en logboekbestanden. De service Azure Migrate moet lezen/schrijven toegang tot deze map.
  - %SystemDrive%\Profiler

### <a name="what-data-is-collected-by-azure-migrate"></a>Welke gegevens worden verzameld door Azure Migrate?

Het apparaat Azure Migrate verzamelt metagegevens voor on-premises machines, met inbegrip van:

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

TN bovendien als u de afhankelijkheidstoewijzing, implementeert de afhankelijkheid toewijzing van agents verzamelen van informatie, waaronder machine FQDN-naam, besturingssysteem, IP-adres, MAC-adres, processen die de virtuele machine en de inkomende/uitgaande TCP-verbindingen voor de virtuele machine worden uitgevoerd. Deze detectie is optioneel, alleen gebruikt als u de afhankelijkheidstoewijzing van de voor detectie inschakelen.

### <a name="is-there-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>Is er prestatie-invloed op de geanalyseerde ESXi-host-omgeving?

Met continue profilering van prestatiegegevens, het apparaat Azure Migrate profielen op lokale machines voor het meten van prestatiegegevens van de virtuele machine. Dit is bijna nul invloed op de prestaties op de ESXi-hosts, maar ook op de vCenter-Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Waar worden de verzamelde gegevens opgeslagen en hoe lang?

De gegevens die zijn verzameld door het apparaat Azure Migrate is opgeslagen in de Azure-locatie die u opgeeft bij het maken van het migratieproject. De gegevens worden veilig opgeslagen in een Microsoft-abonnement, en wordt verwijderd wanneer u de Azure Migrate-project verwijdert.

Als u agents op de virtuele machines installeren, wordt de gegevens die zijn verzameld door de agents van de afhankelijkheid voor visualisatie van afhankelijkheden opgeslagen in de Verenigde Staten, in een Log Analytics-werkruimte hebt gemaakt in het Azure-abonnement. Deze gegevens worden verwijderd wanneer u de Log Analytics-werkruimte in uw abonnement verwijdert. [Meer informatie](concepts-dependency-visualization.md).

### <a name="what-is-the-volume-of-data-uploaded-by-azure-migrate-during-continuous-profiling"></a>Wat is de hoeveelheid gegevens die door Azure Migrate wordt geüpload tijdens de continue profilering?

De hoeveelheid gegevens die worden verzonden naar de Azure Migrate is afhankelijk van verschillende parameters. Als u wilt geven een indicatieve getal, verzendt een Azure Migrate-project met 10 machines (elk met één schijf en één NIC), ongeveer 50 MB per dag. Dit is een geschatte waarde, die op basis van het aantal gegevenspunten voor de NIC's en schijven (de verzonden gegevens is niet-lineaire als het aantal machines, NIC's of schijven verhogen).

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Is de versleutelde gegevens in rust en in-transit?

Ja voor beide. De metagegevens is veilig worden verzonden naar de Azure Migrate-service via internet, via https. De metagegevens worden opgeslagen in een [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest), en in [Azure blob-opslag](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in een Microsoft-abonnement en is versleuteld.

De gegevens die zijn verzameld door de agents afhankelijkheid is ook versleuteld in doorvoer (beveiligde HTTPS), en wordt opgeslagen in een Log Analytics-werkruimte in het gebruikersabonnement. Het is ook versleuteld.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Hoe het apparaat Azure Migrate communiceren met de vCenter-Server en de service Azure Migrate?

Het apparaat verbinding maakt met de vCenter-Server (poort 443) met behulp van de referenties die zijn opgegeven bij het instellen van het apparaat. Deze query uitgevoerd voor de vCenter-Server met behulp van VMware PowerCLI, voor het verzamelen van metagegevens over de VM's beheerd door vCenter Server. Beide configuratiegegevens over VM's (kernen, geheugen, schijven, NIC enzovoort), worden verzameld en de prestatiegeschiedenis van elke virtuele machine voor de afgelopen maand. De verzamelde metagegevens wordt verzonden naar de Azure Migrate-Server-evaluatie (via het internet via HTTPS) voor de beoordeling. 

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-servers"></a>Kan ik het hetzelfde apparaat verbinden met meerdere vCenter-servers?

Ja, een enkel Azure Migrate-apparaat kan worden gebruikt voor het detecteren van meerdere vCenter-Servers, maar niet gelijktijdig. U moet uitvoeren detecties elkaar.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Is het OVA-sjabloon die wordt gebruikt door Site Recovery geïntegreerd met het ova-bestand gebruikt door Azure Migrate?

Er is momenteel geen-integratie. De. OVA-sjabloon in Site Recovery wordt gebruikt voor het instellen van een Site Recovery-configuratieserver voor replicatie van virtuele VMware-machine/fysieke server. De. OVA die worden gebruikt door Azure Migrate wordt gebruikt voor het detecteren van VMware-VM's beheerd door een vCenter-server voor de toepassing van de evaluatie en migratie.

### <a name="i-changed-my-machine-size-can-i-rerun-an-assessment"></a>Ik heb mijn machinegrootte gewijzigd. Kan ik een evaluatie opnieuw uitvoeren?
Het apparaat Azure Migrate wordt continu verzamelt informatie over de on-premises omgeving. Een evaluatie is echter een momentopname van een point-in-time on-premises VM's. Als u de instellingen op een virtuele machine wijzigen die u wilt evalueren, gebruik de optie 'Opnieuw berekenen' voor de evaluatie van updates met de meest recente wijzigingen.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Hoe kan ik een multitenant-omgeving in Azure Migrate detecteren?

Voor VMware, als u een omgeving die wordt gedeeld door tenants hebt en u niet wilt dat voor het detecteren van de virtuele machines van één tenant in een andere tenant-abonnement, maak vCenter-serverreferenties met alleen toegang tot deze virtuele machines die u wilt detecteren. Gebruik vervolgens de referenties wanneer detectie in het apparaat Azure Migrate begon.

Voor Hyper-V, de detectie maakt gebruik van referenties van de Hyper-V-host als de virtuele machines de dezelfde Hyper-V-host delen, is er momenteel geen manier voor het scheiden van de detectie.  

### <a name="how-many-vms-can-be-discovered-using-a-single-migration-appliance"></a>Hoeveel virtuele machines kunnen worden gedetecteerd met behulp van een migratie van één apparaat?

U kunt maximaal 10.000 VMware-VM's en maximaal 5000 Hyper-V-machines met behulp van een apparaat voor eenmalige migratie detecteren.  Als u meer computers in uw on-premises-omgeving hebt, leert u hoe u schaalt [Hyper-V](scale-hyper-v-assessment.md) en [VMware](scale-vmware-assessment.md) evaluatie.


## <a name="azure-migrate-server-assessment"></a>Azure Migrate: Server Assessment

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Azure Migrate: Server-evaluatie ondersteuning biedt voor evaluatie van fysieke servers?

Nee, Azure Migrate op dit moment geen ondersteuning voor evaluatie van fysieke servers. 

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Heeft Azure Migrate vCenter-Server voor het detecteren van een VMware-omgeving nodig?

Ja, Azure Migrate moet vCenter-Server voor het detecteren van een VMware-omgeving. Het biedt geen ondersteuning voor detectie van ESXi-hosts die niet worden beheerd door vCenter-Server.

### <a name="whats-the-difference-between-using-azure-migrate-server-assessment-and-the-map-toolkit"></a>Wat is het verschil tussen het gebruik van Azure Migrate: Server-evaluatie en de Map Toolkit?

Azure Migrate: Evaluatie van de server biedt migratie-analyse om te helpen bij de voorbereiding op de migratie en evaluatie van de workloads voor migratie naar Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) heeft andere functies, zoals migratieplanning voor nieuwere versies van Windows-client en server-besturingssystemen en het bijhouden van softwaregebruik. Voor deze scenario's, echter ook doorgaan met de MAP Toolkit.

### <a name="how-is-azure-migrate-server-assessment-different-from-azure-site-recovery-deployment-planner"></a>Hoe wordt Azure Migrate: Server-evaluatie verschilt van de Azure Site Recovery Deployment Planner?

Azure Migrate: Evaluatie van de server is een hulpprogramma voor migratieplanning. Azure Site Recovery Deployment Planner is een herstel na noodgevallen hulpprogramma voor het plannen.

- **Migratie van VMware/Hyper-V naar Azure**: Als u van plan bent uw on-premises servers naar Azure migreert, gebruikt u de Azure Migrate: Evaluatieprogramma voor beheerders voor migratieplanning server. Het hulpprogramma beoordeelt on-premises werkbelastingen en biedt begeleiding, inzichten en mechanismen om u te helpen bij het migreren naar Azure. Wanneer u klaar met uw migratieplan bent, kunt u hulpprogramma's, zoals Azure Migrate gebruiken: Servermigratie, de machines migreren naar Azure.
- **Herstel na noodgevallen van VMware/Hyper-V naar Azure**: Voor herstel na noodgevallen naar Azure met Site Recovery, gebruikt u de Site Recovery Deployment Planner voor de planning voor noodherstel gaat. Site Recovery Deployment Planner biedt een grondige, Site Recovery-specifieke evaluatie van uw on-premises omgeving. Het biedt aanbevelingen die nodig zijn door Site Recovery voor succesvol noodherstel bewerkingen zoals replicatie en failover van virtuele machines. 

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Biedt Azure Migrate ondersteuning voor schatting van de kosten op basis van een Enterprise Agreement EA?

Azure Migrate op dit moment biedt geen ondersteuning voor schatting van de kosten voor [Enterprise overeenkomst-aanbieding](https://azure.microsoft.com/offers/enterprise-agreement-support/). De oplossing is om te betalen per gebruik opgeven als de aanbieding en het kortingspercentage (van toepassing op het abonnement) handmatig opgeven in het veld 'Korting' van de evaluatie-eigenschappen.

  ![Korting](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Wat is het verschil tussen de grootte van de as-on-premises en de prestaties gebaseerde groottebepaling?

- In als on-premises formaat wijzigen, Azure Migrate geen rekening gehouden met prestatiegegevens van de virtuele machine. Het formaat van de virtuele machines op basis van de on-premises configuratie. -Grootte is in prestaties gebaseerde groottebepaling gebaseerd op gegevens over het gebruik.
- Bijvoorbeeld, als een on-premises VM heeft 4 kernen en het geheugen van 8 GB met 50% CPU-gebruik en het geheugengebruik van 50%, zoals on-premises sizing raadt aan om een Azure VM-SKU met 4 kernen en 8GB geheugen. Grootte op basis van prestaties, raadt echter aan een VM-SKU van 2 kernen en 4 GB, omdat het gebruikspercentage wordt beschouwd als.
- Grootte van de schijf is op dezelfde manier, afhankelijk van twee evaluatie-eigenschappen - formaat type criterium en opslag.
= If het criterium voor het instellen op basis van prestaties is en opslagtype automatisch is, de waarden voor IOPS en doorvoer van de schijf worden beschouwd bij het identificeren van de doel-schijftype (Standard of Premium).
- Als het criterium voor het instellen op basis van prestaties en het opslagtype premium is, wordt een premium-schijf wordt aanbevolen. De premium-schijf die SKU is geselecteerd, is afhankelijk van de grootte van de on-premises schijf. Dezelfde logica wordt gebruikt om u te sizing schijf wanneer het criterium voor het instellen als on-premises formaat is en het opslagtype standard of premium is.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Wat zijn de gevolgen heeft de gebruik van de geschiedenis en percentiel van de prestaties van de aanbevelingen voor de grootte?

Deze eigenschappen zijn alleen van toepassing wanneer de grootte wordt bepaald op basis van prestaties.

- Azure Migrate prestatiegeschiedenis van on-premises machines verzameld en gebruikt om aan te bevelen de VM-grootte en de schijf type in Azure.
- Het toestel profielen continu de on-premises omgeving voor het verzamelen van gegevens over het gebruik van realtime elke 20 seconden. De collector verzamelt de gegevens die iedere 20 seconden worden opgehaald en maakt een enkel gegevenspunt voor elke 15 minuten. Om dat gegevenspunt te bepalen, selecteert de collector de hoogste waarde uit alle opgehaalde waarden voor die 15 minuten en verzendt de collector deze naar Azure.
- Wanneer u een evaluatie maken in Azure (gebaseerd op de duur van de prestaties en de prestaties van geschiedenis-percentielwaarde), Azure Migrate berekent de waarde van het daadwerkelijke gebruik en gebruikt u de grootte instelt.
- Bijvoorbeeld, als u de duur van de prestaties op één dag worden ingesteld en de percentielwaarde op 95 percentiel, Azure Migrate maakt gebruik van de 15 minuten voorbeeldpunten die door de collector voor de laatste dag worden verzonden, gesorteerd in oplopende volgorde en kiest u de 95e-percentielwaarde als de effectieve gebruik.
- De 95e-percentielwaarde zorgt ervoor dat u uitbijters die optreden kunnen als u het 99e percentiel wordt genegeerd. Als u het piekgebruik voor de periode wilt ophalen en uitbijters niet wilt uitsluiten, selecteert u het 99e percentiel.

### <a name="what-is-dependency-visualization"></a>Wat is visualisatie van afhankelijkheden?

Visualisatie van afhankelijkheden, kunt u groepen met VM's voor migratie evalueren met meer vertrouwen. Deze controles machineafhankelijkheden voordat u een evaluatie uitvoert. Visualisatie van afhankelijkheden helpt ervoor te zorgen dat er niets is overgebleven en voorkomen van onverwachte storingen wanneer u naar Azure migreert. Azure Migrate maakt gebruik van de oplossing Serviceoverzicht in Azure Monitor-Logboeken, zodat de visualisatie van afhankelijkheden.

> [!NOTE]
> De functie voor visualisatie van afhankelijkheden is niet beschikbaar in Azure Government.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Moet ik betalen voor het gebruik van de functie voor visualisatie van afhankelijkheden?

Nee. Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Moet ik niets voor visualisatie van afhankelijkheden te installeren?

Voor het gebruik van visualisatie van afhankelijkheden, die u wilt downloaden en installeren van agents op elke on-premises computer die u wilt evalueren.

- [Microsoft Monitoring agent(MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) moet worden geïnstalleerd op elke computer.
- De [agent voor afhankelijkheden](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) moet worden geïnstalleerd op elke computer.
- Bovendien, als u computers geen verbinding met internet hebt, moet u om te downloaden en Log Analytics-gateway installeren op deze.

U hoeft niet op deze agents, tenzij u visualisatie van afhankelijkheden.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kan ik een bestaande werkruimte gebruiken voor visualisatie van afhankelijkheden?

Ja, kunt u een bestaande werkruimte koppelen aan het migratieproject en gebruikmaken van deze voor visualisatie van afhankelijkheden. [Meer informatie](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Kan ik de visualisatie Afhankelijkheidsrapport exporteren?

Nee, de visualisatie van afhankelijkheden kan niet worden geëxporteerd. Echter, omdat Azure Migrate maakt gebruik van servicetoewijzing voor visualisatie van afhankelijkheden, kunt u de [Service kaart REST-API's](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) om de afhankelijkheden in een json-indeling.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Hoe kan ik de installatie van Microsoft Monitoring Agent (MMA) en de agent voor afhankelijkheden automatiseren?

[Gebruik dit script](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) voor de installatie van de agents. [Volg deze instructies](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) met behulp van de opdrachtregel of automatisering van MMA installeren. Voor de MMA, gebruikmaken van [met dit script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Naast scripts, kunt u hulpprogramma's voor implementatie, zoals System Center Configuration Manager [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) enzovoort naar de agents te implementeren.

### <a name="what-operating-systems-are-supported-by-mma"></a>Welke besturingssystemen worden ondersteund door de MMA?

- [Beoordeling](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) de lijst met Windows-besturingssystemen wordt ondersteund door de MMA.
- [Revisie] https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) de lijst met Linux-besturingssystemen wordt ondersteund door de MMA.

### <a name="what-are-the-operating-systems-supported-by-the-dependency-agent"></a>Wat zijn de besturingssystemen die door de agent voor afhankelijkheden?

[Beoordeling](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) de Windows-besturingssystemen die door de agent voor afhankelijkheden.
[Beoordeling](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems) de lijst met Linux-besturingssystemen wordt ondersteund door de agent voor afhankelijkheden.

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Kan ik meer dan een uur afhankelijkheden in Azure Migrate visualiseren?
Nee, kunt u visualiseren afhankelijkheden voor maximaal één uur. U kunt teruggaan naar een bepaalde datum in de geschiedenis, de laatste maand, maar de maximale duur voor visualisatie is een uur. Bijvoorbeeld, u de tijdsduur in de afhankelijkheidskaart kunt gebruiken om afhankelijkheden voor gisteren, maar kan alleen weergeven voor een venster van één uur. Echter, kunt u logboeken met Azure Monitor [afhankelijkheidsgegevens op te vragen](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) gedurende een langere periode.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-ten-vms"></a>Visualisatie van afhankelijkheden is wordt ondersteund voor groepen met meer dan tien virtuele machines?
U kunt [visualiseren afhankelijkheden voor groepen](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) met maximaal tien virtuele machines. Als u een groep met meer dan tien virtuele machines hebt, we raden u aan het splitsen van de groep in kleinere groepen en vervolgens de afhankelijkheden te visualiseren.

## <a name="azure-migrate-server-migration"></a>Azure Migrate: Server Migration

### <a name="how-is-azure-migrate-server-migration-different-from-azure-site-recovery"></a>Hoe wordt Azure Migrate: De migratie van de server is anders dan Azure Site Recovery?

Azure Migrate: Migratie-server maakt gebruik van de replicatie-engine van Site Recovery voor migratie van servers naar Azure op basis van een agent.
## <a name="next-steps"></a>Volgende stappen
Lees de [Azure Migrate-overzicht](migrate-services-overview.md)
