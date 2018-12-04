---
title: Azure Traffic Manager met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u met Azure Traffic Manager met Azure Site Recovery voor migratie en herstel na noodgevallen
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: ca898f36b09d09cb7e0d67d373c54c46e15e0264
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842557"
---
# <a name="azure-traffic-manager-with-azure-site-recovery"></a>Azure Traffic Manager met Azure Site Recovery

Met Azure Traffic Manager kunt u voor het beheren van de distributie van verkeer tussen de toepassingseindpunten van uw. Een eindpunt is een internetgerichte service die binnen of buiten Azure wordt gehost.

Traffic Manager maakt gebruik van de Domain Name System (DNS) om te leiden aanvragen van clients op het meest geschikte eindpunt, op basis van een methode routering van verkeer en de status van de eindpunten. Traffic Manager biedt een scala aan [routeringsmethoden voor verkeer](../traffic-manager/traffic-manager-routing-methods.md) en [opties voor eindpuntcontrole](../traffic-manager/traffic-manager-monitoring.md) om verschillende toepassingsbehoeften en modellen voor automatische failover mogelijk te kunnen maken. Clients rechtstreeks verbinding maken met het geselecteerde eindpunt. Traffic Manager is niet een proxy of een gateway en wordt het verkeer te geven tussen de client en de service niet te zien.

Dit artikel wordt beschreven hoe u intelligente routering van Azure verkeer controleren met de krachtige noodherstel van Azure Site Recovery en migratiemogelijkheden voor kunt combineren.

## <a name="on-premises-to-azure-failover"></a>On-premises naar Azure failover

Voor het eerste scenario kunt u overwegen **bedrijf A** waarvoor alle van de toepassingsinfrastructuur die wordt uitgevoerd in de on-premises omgeving. Voor zakelijke continuïteit en naleving redenen **bedrijf A** wil Azure Site Recovery gebruiken de toepassingen te beschermen.

**Bedrijf A** toepassingen wordt uitgevoerd met openbare eindpunten en wil de mogelijkheid naadloos verkeer omleiden naar Azure in een noodgebeurtenis. De [prioriteit](../traffic-manager/traffic-manager-configure-priority-routing-method.md) verkeersrouteringsmethode in Azure Traffic Manager kan een bedrijf een eenvoudig dit patroon wilt implementeren failover.

De installatie is als volgt:
- **Bedrijf A** maakt een [Traffic Manager-profiel](../traffic-manager/traffic-manager-create-profile.md).
- Met behulp van de **prioriteit** routeringsmethode, **bedrijf A** maakt u twee eindpunten: **primaire** voor on-premises en **Failover** voor Azure. **Primaire** prioriteit 1 is toegewezen en **Failover** prioriteit 2 is toegewezen.
- Omdat de **primaire** -eindpunt wordt gehost buiten Azure, het eindpunt is gemaakt als een [externe](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) eindpunt.
- Met Azure Site Recovery heeft de Azure-site geen elke virtuele machine of toepassingen die worden uitgevoerd voordat u een failover. Dus de **Failover** eindpunt wordt ook gemaakt als een **externe** eindpunt.
- Standaard gebruikersverkeer doorgestuurd naar de on-premises toepassing omdat dit eindpunt de hoogste prioriteit die ermee verbonden zijn heeft. Geen verkeer wordt omgeleid naar Azure als de **primaire** eindpunt in orde is.

![On-premises-to-Azure voordat de failover](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-before.png)

In een noodgebeurtenis bedrijf A kunt activeren een [failover](site-recovery-failover.md) naar Azure en de toepassingen op Azure te herstellen. Wanneer Azure Traffic Manager detecteert dat de **primaire** eindpunt niet meer in orde is, wordt automatisch de **Failover** eindpunt in de DNS-antwoord en gebruikers verbinding maken met de toepassing die wordt hersteld op Azure.

![On-premises-to-Azure na een failover](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-after.png)

Afhankelijk van de zakelijke vereisten, **bedrijf A** kunt ervoor kiezen een hogere of lagere [probing frequentie](../traffic-manager/traffic-manager-monitoring.md) schakelen tussen on-premises naar Azure in een noodgebeurtenis, en ervoor zorgen dat minimale downtime voor gebruikers.

Wanneer het noodgeval is opgenomen, **bedrijf A** kunt failback van Azure naar de on-premises-omgeving ([VMware](vmware-azure-failback.md) of [Hyper-V](hyper-v-azure-failback.md)) met behulp van Azure Site Recovery. Nu als Traffic Manager vaststelt dat de **primaire** eindpunt weer in orde is, worden automatisch wordt toegepast. de **primaire** eindpunt in de DNS-antwoorden.

## <a name="on-premises-to-azure-migration"></a>On-premises naar Azure-migratie

Naast het herstel na noodgevallen kunt Azure Site Recovery ook [migraties naar Azure](migrate-overview.md). Met behulp van Azure Site Recovery krachtige test failover wordt toegepast, beoordelen klanten prestaties van toepassingen op Azure zonder dat hun on-premises omgeving. En wanneer klanten klaar om te migreren zijn, ze kunnen kiezen samen gehele workloads migreren of ervoor kiezen om te migreren en geleidelijk schalen.

Met Azure Traffic Manager van [gewogen](../traffic-manager/traffic-manager-configure-weighted-routing-method.md) routeringsmethode kan worden gebruikt om een deel van het binnenkomende verkeer naar Azure rechtstreeks bij het doorsturen van de meeste naar de on-premises omgeving. Deze aanpak kan helpen met het beoordelen van prestaties, zoals u kunt doorgaan met het verhogen van het gewicht dat is toegewezen aan Azure bij het migreren van meer en meer van uw workloads naar Azure.

Bijvoorbeeld, **bedrijf B** kiest voor het migreren van in fasen, verplaatst enkele van de toepassingsomgeving behoudt de rest on-premises. Tijdens de eerste fasen van dat bij het grootste deel van de omgeving is on-premises een grotere gewicht is toegewezen aan de on-premises omgeving. Traffic manager retourneert een eindpunt op basis van gewicht is toegewezen aan de beschikbare eindpunten.

![On-premises-to-Azure-migratie](./media/concepts-traffic-manager-with-site-recovery/on-premises-migration.png)

Tijdens de migratie, beide eindpunten zijn actief en het meeste verkeer wordt omgeleid naar de on-premises omgeving. Als de migratie wordt voortgezet, een grotere gewicht kan worden toegewezen aan het eindpunt op Azure en ten slotte het eindpunt van de on-premises gedeactiveerde nadat de migratie kan zijn.

## <a name="azure-to-azure-failover"></a>Failover van Azure naar Azure

Voor dit voorbeeld kunt u overwegen **bedrijf C** waarvoor alle van de toepassingsinfrastructuur met Azure. Voor zakelijke continuïteit en naleving redenen **bedrijf C** wil Azure Site Recovery gebruiken de toepassingen te beschermen.

**Bedrijf C** toepassingen wordt uitgevoerd met openbare eindpunten en wil de mogelijkheid naadloos verkeer omleiden naar een andere Azure-regio in een noodgebeurtenis. De [prioriteit](../traffic-manager/traffic-manager-configure-priority-routing-method.md) verkeersrouteringsmethode kunt **bedrijf C** dit patroon failover eenvoudig kunt implementeren.

De installatie is als volgt:
- **Bedrijf C** maakt een [Traffic Manager-profiel](../traffic-manager/traffic-manager-create-profile.md).
- Met behulp van de **prioriteit** routeringsmethode, **bedrijf C** maakt u twee eindpunten: **primaire** voor de regio van de gegevensbron (Azure Oost-Azië) en **Failover** voor de recovery-regio (Azure Zuidoost-Azië). **Primaire** prioriteit 1 is toegewezen en **Failover** prioriteit 2 is toegewezen.
- Omdat de **primaire** eindpunt wordt gehost in Azure, het eindpunt kan worden als een [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) eindpunt.
- Met Azure Site Recovery, heeft het herstel van Azure site geen elke virtuele machine of toepassingen die worden uitgevoerd voordat u een failover. Dus de **Failover** eindpunt kan worden gemaakt als een [externe](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) eindpunt.
- Standaard gebruikersverkeer doorgestuurd naar de toepassing van de bron-regio's (Oost-Azië) als dit eindpunt de hoogste prioriteit die ermee verbonden zijn heeft. Geen verkeer wordt omgeleid naar de recovery-regio als de **primaire** eindpunt in orde is.

![Azure-naar-Azure voordat de failover](./media/concepts-traffic-manager-with-site-recovery/azure-failover-before.png)

In een noodgebeurtenis **bedrijf C** kunt activeren een [failover](azure-to-azure-tutorial-failover-failback.md) en herstellen van de toepassingen op de Azure-regio van het herstel. Wanneer Azure Traffic Manager detecteert dat het primaire eindpunt niet meer in orde is, gebruikt deze automatisch de **Failover** eindpunt in de DNS-antwoord en gebruikers verbinding maken met de toepassing die wordt hersteld op de herstelserver (Azure-regio Zuidoost-Azië).

![Azure naar Azure na een failover](./media/concepts-traffic-manager-with-site-recovery/azure-failover-after.png)

Afhankelijk van de zakelijke vereisten, **bedrijf C** kunt ervoor kiezen een hogere of lagere [probing frequentie](../traffic-manager/traffic-manager-monitoring.md) schakelen tussen de bron- en herstelinstellingen regio's, en ervoor zorgen dat minimale downtime voor gebruikers.

Wanneer het noodgeval is opgenomen, **bedrijf C** kunt failback van Azure-regio van het herstel naar de bron-Azure-regio met Azure Site Recovery. Nu als Traffic Manager vaststelt dat de **primaire** eindpunt weer in orde is, worden automatisch wordt toegepast. de **primaire** eindpunt in de DNS-antwoorden.

## <a name="protecting-multi-region-enterprise-applications"></a>Beveiliging van bedrijfstoepassingen voor meerdere regio 's

Wereldwijd opererende ondernemingen de klantervaring vaak verbeteren door het afstemmen van hun toepassingen om te voldoen aan regionale behoeften. Lokalisatie en vermindering van de latentie kunnen leiden tot toepassingsinfrastructuur verdeeld over regio's. Ondernemingen zijn ook gebonden bent aan de wetten van de regionale gegevens in bepaalde gebieden en kiest u voor het isoleren van een onderdeel de toepassingsinfrastructuur van hun binnen de grenzen van regionale.  

Een voorbeeld waarin **bedrijf D** is gesplitst door de toepassingseindpunten afzonderlijk voor Duitsland en de rest van de hele wereld. **Bedrijf D** maakt gebruik van Azure Traffic Manager [geografisch](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) routeringsmethode dit instellen. Al het verkeer dat afkomstig is uit Duitsland wordt omgeleid naar **Eindpunt1** en al het verkeer dat afkomstig is buiten Duitsland wordt omgeleid naar **eindpunt 2**.

Het probleem met deze instelling is dat als **Eindpunt1** niet meer werkt voor een bepaalde reden, er is geen omleiden van verkeer naar **eindpunt 2**. Verkeer dat afkomstig is uit Duitsland blijft worden omgeleid naar **Eindpunt1** verlaten, ongeacht de status van het eindpunt, Duitse gebruikers geen toegang tot **bedrijf D**van toepassing. Op dezelfde manier als **eindpunt 2** offline is, is er geen omleiding van het verkeer naar **Eindpunt1**.

![Toepassing voor meerdere regio 's](./media/concepts-traffic-manager-with-site-recovery/geographic-application-before.png)

Kunt u voorkomen dat dit probleem zich voordoet en ervoor zorgen dat tolerantie voor toepassing **bedrijf D** maakt gebruik van [geneste Traffic Manager-profielen](../traffic-manager/traffic-manager-nested-profiles.md) met Azure Site Recovery. In een geneste profielinstellingen is verkeer niet gericht op afzonderlijke eindpunten, maar in plaats daarvan naar andere Traffic Manager-profielen. Dit is de werking van deze instellingen:
- In plaats van het gebruik van de geografische routering met afzonderlijke eindpunten **bedrijf D** maakt gebruik van de geografische routering met Traffic Manager-profielen.
- Maakt gebruik van elke onderliggende Traffic Manager-profiel **prioriteit** routering met een primaire en een eindpunt herstel, dus het nesten van **prioriteit** routering binnen **geografisch** routering.
- Om in te schakelen tolerantie voor toepassing, de verdeling van elke werkbelasting gebruikmaakt van Azure Site Recovery voor failover naar een recovery regio op basis van in het geval van een noodgebeurtenis.
- Wanneer de bovenliggende Traffic Manager een DNS-query ontvangt, wordt deze omgeleid naar de relevante onderliggende Traffic Manager die met de query met een eindpunt beschikbaar overeenkomt.

![Toepassing na meerdere regio 's](./media/concepts-traffic-manager-with-site-recovery/geographic-application-after.png)

Bijvoorbeeld, als het eindpunt in Duitsland-centraal mislukt, kan de toepassing snel worden hersteld naar Duitsland-Noordoost. Het nieuwe eindpunt verwerkt verkeer dat afkomstig is uit Duitsland met minimale downtime voor gebruikers. Een eindpunt storing in West-Europa kan op dezelfde manier worden verwerkt door het herstellen van de workload van de toepassing naar Noord-Europa, met Azure Traffic Manager-verwerking die DNS wordt omgeleid naar het eindpunt beschikbaar.

De bovenstaande instellingen kan worden uitgebreid naar zo veel combinaties van de regio en -eindpunt vereist. Traffic Manager kunt maximaal 10 niveaus van geneste profielen en lussen in de geneste-configuratie niet is toegestaan.

## <a name="recovery-time-objective-rto-considerations"></a>Overwegingen voor herstel tijd beoogde hersteltijd (RTO)

In de meeste organisaties, wordt toevoegen of wijzigen van de DNS-records verwerkt door een afzonderlijk team of door iemand buiten de organisatie. Hiermee wordt de taak van het DNS-records die zeer moeilijk te wijzigen. De gebruikte tijd voor het bijwerken van DNS-records door andere teams of organisaties beheren van DNS-infrastructuur is afhankelijk van de organisatie, en heeft gevolgen voor de RTO bepaalt van de toepassing.

Door het gebruik van Traffic Manager, kunt u frontload het werk dat nodig is voor DNS-updates. Er is geen actie handmatig of gepland op het moment van de werkelijke failover vereist. Deze aanpak helpt snel overschakelen (en dus breek RTO) en het voorkomen van kostbare tijd DNS-wijziging fouten in een noodgebeurtenis. Met Traffic Manager, zelfs de stap van de failback is geautomatiseerd, die anders zelf zou moeten afzonderlijk worden beheerd.

Instellen van de juiste [testinterval](../traffic-manager/traffic-manager-monitoring.md) via basic of snelle interval health controles kunnen aanzienlijk de RTO uitvallen tijdens de failover en minder uitvaltijd voor gebruikers.

Daarnaast kunt u de DNS Time to Live (TTL) waarde voor het Traffic Manager-profiel optimaliseren. TTL-waarde is de waarde waarvoor een DNS-vermelding worden in de cache door een client opgeslagen zou. Voor een record zou DNS niet twee keer binnen het bereik van TTL worden opgevraagd. Elke DNS-record heeft een TTL-waarde die is gekoppeld aan deze. Deze waarde verlaagt resulteert in meer DNS-query's op Traffic Manager, maar kunt RTO verkorten door het detecteren van storingen sneller.

De TTL-waarde door de client is ook verhogen niet als het aantal DNS-resolvers tussen de client en de gezaghebbende DNS-server wordt verhoogd. DNS-resolvers 'aftellen' de TTL-waarde en geef alleen op een TTL-waarde die overeenkomt met de verstreken tijd sinds de record in de cache is opgeslagen. Dit zorgt ervoor dat de DNS-record wordt vernieuwd op de client na de TTL-waarde, ongeacht het aantal DNS-Resolvers in de keten.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over Traffic Manager [routeringsmethoden](../traffic-manager/traffic-manager-routing-methods.md).
- Meer informatie over [geneste Traffic Manager-profielen](../traffic-manager/traffic-manager-nested-profiles.md).
- Meer informatie over [eindpuntbewaking](../traffic-manager/traffic-manager-monitoring.md).
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover wilt automatiseren.
