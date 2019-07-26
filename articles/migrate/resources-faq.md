---
title: Veelgestelde vragen over Azure Migrate | Microsoft Docs
description: Hierin worden veelgestelde vragen over Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: snehaa
ms.openlocfilehash: 0708502087ae6880d9559cf17f0ba9982b2ba040
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372484"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate-Veelgestelde vragen (FAQ)

In dit artikel vindt u veelgestelde vragen over Azure Migrate. Als u na het lezen van dit artikel nog meer query's hebt uitgevoerd, plaatst u deze op het [Azure migrate forum](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Algemeen

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Welke Azure-geografies worden ondersteund door Azure Migrate?

U vindt hier de lijst voor [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) en voor [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Wat is het verschil tussen Azure Migrate en Azure Site Recovery?

Azure Migrate biedt een gecentraliseerde hub om de detectie, evaluatie en migratie van machines en workloads naar Azure te starten, uit te voeren en te volgen. [Azure site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) is een oplossing voor nood herstel. Azure Migrate server migratie maakt gebruik van Azure Site Recovery in de back-end om migratie scenario's mogelijk te maken voor de migratie van on-premises machines in de lift-en-Shift-computers.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Azure Migrate apparaat (VMware/fysieke servers)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Hoe maakt het Azure Migrate apparaat verbinding met Azure?

De verbinding kan via internet zijn of u kunt ExpressRoute gebruiken met open bare/micro soft-peering.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate-server-assessment-and-migration"></a>Welke vereisten voor netwerk connectiviteit zijn nodig voor de evaluatie en migratie van Azure Migrate server

Raadpleeg de [VMware](migrate-support-matrix-vmware.md) -en [Hyper-V-](migrate-support-matrix-hyper-v.md) ondersteunings matrices voor de url's en poorten die nodig zijn om Azure migrate te communiceren met Azure.

### <a name="can-i-harden-the-appliance-vm-i-set-up-with-the-template"></a>Kan ik de apparaat-VM die ik heb ingesteld met de sjabloon, een harder maken?

Aanvullende onderdelen (bijvoorbeeld anti-virus) kunnen worden toegevoegd aan de sjabloon, zolang de communicatie-en firewall regels die vereist zijn voor het Azure Migrate apparaat blijven staan.   

### <a name="what-data-is-collected-by-azure-migrate-appliance"></a>Welke gegevens worden door Azure Migrate apparaat verzameld?

[Hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#collected-performance-data-vmware)vindt u meer informatie over de gegevens die worden verzameld door Azure migrate apparaat.

### <a name="is-there-any-performance-impact-on-the-analyzed-vmware-or-hyper-v-environment"></a>Is er sprake van invloed op de prestaties van de geanalyseerde VMware-of Hyper-V-omgeving?

Met een doorlopend profileren van prestatie gegevens, de Azure Migrate apparaat profielen on-premises machines om de prestatie gegevens van de virtuele machine te meten. Dit heeft bijna geen invloed op de prestaties op de Hyper-V-ESXi-hosts, en op de vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Waar worden de verzamelde gegevens opgeslagen en hoelang?

De gegevens die door het Azure Migrate apparaat worden verzameld, worden opgeslagen op de Azure-locatie die u opgeeft bij het maken van het migratie project. De gegevens worden veilig opgeslagen in een micro soft-abonnement en worden verwijderd wanneer u het Azure Migrate project verwijdert.

Als u agents op de virtuele machines installeert, worden de gegevens die worden verzameld door de afhankelijkheids agents opgeslagen in de Verenigde Staten, in een Log Analytics werk ruimte die in het Azure-abonnement is gemaakt. Deze gegevens worden verwijderd wanneer u de Log Analytics-werk ruimte in uw abonnement verwijdert. [Meer informatie](concepts-dependency-visualization.md).

### <a name="what-is-the-volume-of-data-uploaded-by-azure-migrate-appliance-during-continuous-profiling"></a>Wat is het volume van de gegevens die door Azure Migrate apparaat worden geüpload tijdens een doorlopende profile ring?

Het volume van de gegevens die naar Azure Migrate worden verzonden, varieert op basis van verschillende para meters. Om een indicatief nummer te geven, stuurt een Azure Migrate project met 10 machines (elk met één schijf en één NIC) ongeveer 50 MB per dag. Dit is een geschatte waarde, die wordt gewijzigd op basis van het aantal gegevens punten voor de Nic's en schijven (de verzonden gegevens zijn niet-lineair als het aantal machines, Nic's of schijven toenemen).

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Zijn de gegevens op rest en in transit versleuteld?

Ja voor beide. De meta gegevens worden via internet veilig verzonden naar de Azure Migrate-service via https. De meta gegevens worden opgeslagen in een [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest)en in [Azure Blob-opslag](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in een micro soft-abonnement en worden op rest versleuteld.

De gegevens die door de afhankelijkheids agenten worden verzameld, worden ook versleuteld in door Voer (beveiligde HTTPS) en worden opgeslagen in een Log Analytics-werk ruimte in het gebruikers abonnement. Het is ook versleuteld op rest.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Hoe communiceert het Azure Migrate apparaat met de vCenter Server en de Azure Migrate service?

Het apparaat maakt verbinding met de vCenter Server (poort 443) met behulp van de referenties die zijn opgegeven bij het instellen van het apparaat. Er wordt een query uitgevoerd op de vCenter Server met behulp van VMware PowerCLI om meta gegevens te verzamelen over de Vm's die worden beheerd door vCenter Server. Er worden zowel configuratie gegevens verzameld over Vm's (kernen, geheugen, schijven, NIC enz.), als de prestatie geschiedenis van elke virtuele machine voor de afgelopen maand. De verzamelde meta gegevens worden vervolgens verzonden naar de evaluatie van de Azure Migrate-server (via internet via HTTPS) voor evaluatie.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-servers"></a>Kan ik hetzelfde apparaat verbinden met meerdere vCenter-servers?

Ja, Eén Azure Migrate apparaat kan worden gebruikt om meerdere vCenter-servers te detecteren, maar niet gelijktijdig. U moet detecties één na de andere uitvoeren.

### <a name="i-changed-my-machine-size-can-i-rerun-an-assessment"></a>Ik heb mijn computer grootte gewijzigd. Kan ik een evaluatie opnieuw uitvoeren?

Het Azure Migrate apparaat verzamelt voortdurend informatie over de on-premises omgeving. Een evaluatie is echter een tijdgebonden moment opname van on-premises Vm's. Als u de instellingen van een virtuele machine die u wilt beoordelen, wijzigt, gebruikt u de optie opnieuw berekenen om de evaluatie bij te werken met de meest recente wijzigingen.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate-server-assessment"></a>Hoe kan ik een omgeving met meerdere tenants detecteren in Azure Migrate server-evaluatie?

Als u voor VMware een omgeving hebt die wordt gedeeld tussen tenants en u niet wilt dat de virtuele machines van één Tenant in het abonnement van een andere Tenant worden gedetecteerd, maakt u vCenter Server referenties met alleen toegang tot de virtuele machines die u wilt detecteren. Gebruik vervolgens de referenties voor het starten van detectie in het Azure Migrate apparaat.

Voor Hyper-V gebruikt de detectie referenties voor Hyper-V-hosts, als de virtuele machines dezelfde Hyper-V-host delen, is er op dit moment geen manier om de detectie te scheiden.  

### <a name="how-many-vms-can-be-discovered-using-a-single-migration-appliance"></a>Hoeveel Vm's kunnen worden gedetecteerd met één migratie apparaat?

U kunt Maxi maal 10.000 VMware-Vm's en Maxi maal 5.000 virtuele Hyper-V-machines detecteren met één migratie apparaat.  Als u meer computers in uw on-premises omgeving hebt, leert u hoe u de [Hyper-V-](scale-hyper-v-assessment.md) en [VMware](scale-vmware-assessment.md) -evaluatie kunt schalen.

## <a name="azure-migrate-server-assessment"></a>Azure Migrate: Server Assessment

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Azure Migrate: Evaluatie van server evaluatie ondersteuning van fysieke servers?

Nee, Azure Migrate biedt momenteel geen ondersteuning voor de evaluatie van fysieke servers.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Heeft Azure Migrate vCenter Server nodig voor het detecteren van een VMware-omgeving?

Ja, Azure Migrate moet vCenter Server om een VMware-omgeving te detecteren. Het biedt geen ondersteuning voor detectie van ESXi-hosts die niet worden beheerd door vCenter Server.

### <a name="whats-the-difference-between-using-azure-migrate-server-assessment-and-the-map-toolkit"></a>Wat is het verschil tussen het gebruik van Azure Migrate: Server evaluatie en de kaart Toolkit?

Azure Migrate: Server evaluatie biedt migratie-evaluatie om te helpen bij de voor bereiding op de migratie en de evaluatie van werk belastingen voor migratie naar Azure. [Micro soft Assessment and planning (kaart) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) heeft andere functies, zoals migratie planning voor nieuwere versies van Windows-besturings systemen voor clients en servers, en het bijhouden van software gebruik. Voor deze scenario's kunt u de kaart Toolkit blijven gebruiken.

### <a name="how-is-azure-migrate-server-assessment-different-from-azure-site-recovery-deployment-planner"></a>Hoe wordt Azure Migrate: De server evaluatie wijkt af van Azure Site Recovery Deployment Planner?

Azure Migrate: Server evaluatie is een hulp programma voor migratie planning. Azure Site Recovery Deployment Planner is een hulp programma voor het plannen van nood herstel.

- **Migratie van VMware/Hyper-V naar Azure**: Als u van plan bent om uw on-premises servers naar Azure te migreren, gebruikt u de Azure Migrate: Hulp programma voor Server evaluatie voor migratie planning. Het hulp programma evalueert on-premises workloads en biedt richt lijnen, inzichten en mechanismen om u te helpen bij het migreren naar Azure. Wanneer u klaar bent met uw migratie plan, kunt u hulpprogram ma's zoals Azure Migrate gebruiken: Server migratie, om de machines te migreren naar Azure.
- **Herstel na nood geval van VMware/Hyper-V naar Azure**: Gebruik voor herstel na nood gevallen naar Azure met Site Recovery de Site Recovery Deployment Planner voor de planning voor nood herstel. Site Recovery Deployment Planner een diepe, Site Recovery-specifieke evaluatie van uw on-premises omgeving. Het biedt aanbevelingen die nodig zijn voor Site Recovery voor geslaagde rampen, zoals replicatie, en failover van Vm's.

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Ondersteunt Azure Migrate een raming op basis van Enterprise Agreement (EA)?

Azure Migrate biedt momenteel geen ondersteuning voor de kosten raming voor [Enterprise Agreement aanbieding](https://azure.microsoft.com/offers/enterprise-agreement-support/). De tijdelijke oplossing is het opgeven van een betalen per gebruik-als-aanbieding en het kortings percentage (van toepassing op het abonnement) hand matig op te geven in het veld Discount van de eigenschappen van de beoordeling.

  ![Korting](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Wat is het verschil tussen de on-premises grootte en de grootte van de prestaties?

- Bij een on-premises grootte Azure Migrate geen rekening gehouden met de prestatie gegevens van de virtuele machine. Het formaat van de virtuele machines op basis van de on-premises configuratie. -In grootte op basis van prestaties is het formaat gebaseerd op gebruiks gegevens.
- Als een on-premises VM bijvoorbeeld 4 kernen en 8 GB geheugen heeft met een CPU-gebruik van 50% en het geheugen gebruik van 50%, wordt een Azure VM-SKU aanbevolen met vier kernen en 8 GB geheugen. Op basis van de prestaties wordt echter een VM-SKU van twee kernen en 4 GB aanbevolen, omdat het gebruiks percentage wordt overwogen.
- De schijf grootte is ook afhankelijk van twee evaluatie-eigenschappen: grootte criterium en opslag type.
= Als het criterium voor het wijzigen van de prestaties en het opslag type automatisch is, worden de IOPS-en doorvoer waarden van de schijf overwogen bij het identificeren van het doel schijf type (Standard of Premium).
- Als het criterium voor het bepalen van de grootte op basis van prestaties is en het opslag type Premium is, wordt een Premium-schijf aanbevolen. De Premium-schijf-SKU wordt geselecteerd op basis van de grootte van de on-premises schijf. Dezelfde logica wordt gebruikt voor het uitvoeren van schijf grootte wanneer het criterium voor het aanpassen van de grootte de grootte van de on-premises heeft en het opslag type Standard of Premium is.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Welke invloed heeft de prestaties van de prestatie geschiedenis en het percentiel?

Deze eigenschappen zijn alleen van toepassing wanneer de grootte wordt bepaald op basis van prestaties.

- Azure Migrate verzamelt de prestatie geschiedenis van on-premises machines en gebruikt deze om de VM-grootte en het schijf type in azure aan te bevelen.
- Het apparaat maakt continu gebruik van de on-premises omgeving voor het verzamelen van gegevens in realtime om de 20 seconden. De collector verzamelt de gegevens die iedere 20 seconden worden opgehaald en maakt een enkel gegevenspunt voor elke 15 minuten. Om dat gegevenspunt te bepalen, selecteert de collector de hoogste waarde uit alle opgehaalde waarden voor die 15 minuten en verzendt de collector deze naar Azure.
- Wanneer u een evaluatie in azure maakt (op basis van de waarde voor prestatie duur en prestatie geschiedenis percentiel), wordt Azure Migrate de juiste gebruiks waarde berekend en wordt deze gebruikt voor het aanpassen van de grootte.
- Als u bijvoorbeeld de prestatie duur instelt op één dag en de percentiel waarde op 95 percentiel, Azure Migrate gebruikt de voorbeeld punten van 15 minuten die worden verzonden door de collector voor de laatste dag, worden deze in oplopende volg orde gesorteerd en wordt de waarde van het 95e percentiel opgehaald als de effectief gebruik.
- De 95e percentiel waarde zorgt ervoor dat u alle uitbijters negeert die kunnen optreden als u het 99e percentiel gebruikt. Als u het piekgebruik voor de periode wilt ophalen en uitbijters niet wilt uitsluiten, selecteert u het 99e percentiel.

### <a name="what-is-dependency-visualization"></a>Wat is de visualisatie van afhankelijkheden?

Met behulp van afhankelijkheids visualisatie kunt u groepen Vm's beoordelen voor migratie met een grotere betrouw baarheid. Er wordt gekeken naar computer afhankelijkheden voordat u een evaluatie uitvoert. Met een afhankelijkheids visualisatie kunt u ervoor zorgen dat er niets achter komt en onverwachte storingen voor komen wanneer u naar Azure migreert. Azure Migrate maakt gebruik van de Servicetoewijzing oplossing in Azure Monitor Logboeken om de visualisatie van afhankelijkheden in te scha kelen.

> [!NOTE]
> De functie voor visualisatie van afhankelijkheden is niet beschikbaar in Azure Government.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Moet ik betalen voor het gebruik van de functie voor afhankelijkheids visualisatie?

Nee. Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Moet ik iets installeren voor de visualisatie van afhankelijkheden?

Als u afhankelijkheids visualisatie wilt gebruiken, moet u agents downloaden en installeren op elke on-premises computer die u wilt evalueren.

- [Micro soft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) moet op elke computer worden geïnstalleerd.
- De [afhankelijkheids agent](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) moet op elke computer worden geïnstalleerd.
- Als u bovendien computers hebt zonder Internet verbinding, moet u de Log Analytics-gateway hierop downloaden en installeren.

U hebt deze agents niet nodig tenzij u gebruikmaakt van afhankelijkheids visualisatie.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kan ik een bestaande werk ruimte gebruiken voor de visualisatie van afhankelijkheden?

Ja, u kunt een bestaande werk ruimte koppelen aan het migratie project en deze gebruiken voor de visualisatie van afhankelijkheden. [Meer informatie](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Kan ik het visualisatie rapport van de afhankelijkheid exporteren?

Nee, de visualisatie van de afhankelijkheid kan niet worden geëxporteerd. Omdat Azure Migrate echter Servicetoewijzing gebruikt voor de visualisatie van afhankelijkheden, kunt u de [SERVICETOEWIJZING rest-api's](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) gebruiken om de afhankelijkheden in een JSON-indeling op te halen.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Hoe kan ik de installatie van micro soft Monitoring Agent (MMA) en de afhankelijkheids agent automatiseren?

[Gebruik dit script](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) voor de installatie van de agents. [Volg deze instructies](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) voor het installeren van MMA met behulp van de opdracht regel of de automatisering. Gebruik [Dit script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)voor MMA.

Naast scripts kunt u implementatie hulpprogramma's als System Center Configuration Manager, [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) enzovoort, gebruiken om de agents te implementeren.

### <a name="what-operating-systems-are-supported-by-mma"></a>Welke besturings systemen worden ondersteund door MMA?

- [Bekijk](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) de lijst met Windows-besturings systemen die worden ondersteund door MMA.
- [Bekijk](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) de lijst met Linux-besturings systemen die worden ondersteund door MMA.

### <a name="what-are-the-operating-systems-supported-by-the-dependency-agent"></a>Wat zijn de besturings systemen die worden ondersteund door de afhankelijkheids agent?

[Bekijk](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) de Windows-besturings systemen die worden ondersteund door de afhankelijkheids agent.
[Bekijk](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems) de lijst met Linux-besturings systemen die worden ondersteund door de afhankelijkheids agent.

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Kan ik afhankelijkheden in Azure Migrate meer dan een uur visualiseren?
Nee, u kunt afhankelijkheden tot een uur visualiseren. U kunt teruggaan naar een bepaalde datum in de geschiedenis, tot de laatste maand, maar de maximale duur voor visualisatie is een uur. U kunt bijvoorbeeld de tijds duur in de afhankelijkheids toewijzing gebruiken om afhankelijkheden voor gisteren weer te geven, maar deze kunnen alleen voor een venster van één uur worden weer gegeven. U kunt echter Azure Monitor-Logboeken gebruiken om een langere duur van [afhankelijkheids gegevens](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) op te vragen.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>Wordt visualisatie van afhankelijkheden ondersteund voor groepen met meer dan 10 virtuele machines?
U kunt [afhankelijkheden voor groepen](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) met Maxi maal tien vm's visualiseren. Als u een groep hebt met meer dan tien Vm's, raden we u aan de groep te splitsen in kleinere groepen en vervolgens de afhankelijkheden te visualiseren.

## <a name="azure-migrate-server-migration"></a>Azure Migrate: Server Migration

### <a name="how-is-azure-migrate-server-migration-different-from-azure-site-recovery"></a>Hoe wordt Azure Migrate: Server migratie verschilt van Azure Site Recovery?

Azure Migrate: Server migratie maakt gebruik van de replicatie-engine van Site Recovery voor de migratie van virtuele VMware-machines op basis van een agent, migratie van Hyper-V-Vm's en migratie van fysieke servers naar Azure. De optie voor het migreren van virtuele VMware-machines is standaard ingebouwd in server migratie.

## <a name="next-steps"></a>Volgende stappen
Lees het [Azure migrate overzicht](migrate-services-overview.md)
