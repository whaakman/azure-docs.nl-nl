---
title: Met Azure Traffic Manager met Azure Site Recovery | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure Traffic Manager gebruiken met Azure Site Recovery voor de migratie en herstel na noodgevallen
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: manayar
ms.openlocfilehash: 8305a354588875926cab52a55d99d3a29bcfb509
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="azure-traffic-manager-with-azure-site-recovery"></a>Azure Traffic Manager met Azure Site Recovery

Azure Traffic Manager kunt u het beheer van de distributie van verkeer tussen de eindpunten van uw toepassing. Een eindpunt is een internetgerichte service die wordt gehost binnen of buiten Azure.

Traffic Manager maakt gebruik van de Domain Name System (DNS) om te leiden aanvragen van clients naar de meest geschikte eindpunt, op basis van een methode voor verkeersroutering en de status van de eindpunten. Traffic Manager biedt een reeks [verkeersroutering methoden](../traffic-manager/traffic-manager-routing-methods.md) en [eindpunt controle-opties](../traffic-manager/traffic-manager-monitoring.md) aanpassen aan de behoeften van verschillende groepen van toepassingen en automatische failover-modellen. Clients rechtstreeks verbinding gemaakt met het geselecteerde eindpunt. Traffic Manager is niet een proxy of gateway en het ondersteunt niet raadpleegt u het verkeer tussen de client en de service wordt doorgegeven.

Dit artikel wordt beschreven hoe u kunt combineren intelligent routering van Azure verkeer bewaken met Azure Site Recovery krachtige noodherstel en migratiemogelijkheden.

## <a name="on-premises-to-azure-failover"></a>On-premises naar Azure failover

Voor het eerste scenario kunt u overwegen **bedrijf A** die alle van de toepassing-infrastructuur uitgevoerd in de on-premises omgeving is. Voor zakelijke continuïteit en naleving redenen **bedrijf A** beslist haar toepassingen beveiligen met Azure Site Recovery.

**Bedrijf A** toepassingen worden uitgevoerd met de openbare eindpunten en wil de mogelijkheid verkeer naadloos omleiden naar Azure in een gebeurtenis na noodgevallen. De [prioriteit](../traffic-manager/traffic-manager-configure-priority-routing-method.md) verkeersroutering methode in Azure Traffic Manager kan een bedrijf een eenvoudig implementeren dit patroon van een failover.

De installatie is als volgt:
- **Bedrijf A** maakt een [Traffic Manager-profiel](../traffic-manager/traffic-manager-create-profile.md).
- Gebruik de **prioriteit** routeringsmethode, **bedrijf A** maakt twee eindpunten – **primaire** voor on-premises en **Failover** voor Azure. **Primaire** prioriteit 1 is toegewezen en **Failover** prioriteit 2 is toegewezen.
- Aangezien de **primaire** eindpunt buiten Azure wordt gehost, het eindpunt is gemaakt als een [externe](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) eindpunt.
- Met Azure Site Recovery heeft de Azure site geen alle virtuele machines of toepassingen die worden uitgevoerd voordat er failover. Ja, de **Failover** eindpunt wordt ook gemaakt als een **externe** eindpunt.
- Standaard wordt gebruikersverkeer omgeleid naar de on-premises toepassing omdat dat eindpunt de hoogste prioriteit gekoppeld heeft. Er is geen verkeer wordt omgeleid naar Azure als de **primaire** eindpunt is in orde.

![Op lokale-die naar Azure voordat failover wordt uitgevoerd](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-before.png)

In een gebeurtenis na noodgevallen bedrijf A kunt activeren een [failover](site-recovery-failover.md) naar Azure en herstellen van de toepassingen in Azure. Wanneer Azure Traffic Manager gedetecteerd dat de **primaire** eindpunt is niet langer in orde, gebruikt automatisch de **Failover** eindpunt in de DNS-antwoord en gebruikers verbinding maken met de toepassing die wordt hersteld op Azure.

![Op lokale-die naar Azure na een failover](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-after.png)

Afhankelijk van de zakelijke vereisten, **bedrijf A** kunt kiezen een hogere of lagere [probing frequentie](../traffic-manager/traffic-manager-monitoring.md) overschakelen tussen on-premises naar Azure in een gebeurtenis na noodgevallen en zorg ervoor dat de minimale downtime voor gebruikers.

Wanneer de sitedatabase is opgenomen, **bedrijf A** kunt failback vanuit Azure naar de on-premises-omgeving ([VMware](vmware-azure-failback.md) of [Hyper-V](hyper-v-azure-failback.md)) met Azure Site Recovery. Nu wanneer Traffic Manager gedetecteerd dat de **primaire** eindpunt opnieuw in orde is, wordt automatisch gebruikt de **primaire** eindpunt in de DNS-antwoorden.

## <a name="on-premises-to-azure-migration"></a>On-premises naar Azure migreren

Naast het herstel na noodgevallen, kunnen Azure Site Recovery ook [migraties naar Azure](migrate-overview.md). Met behulp van Azure Site Recovery krachtige test failoverfuncties beoordelen klanten toepassingsprestaties op Azure zonder hun on-premises omgeving. En wanneer klanten gereed om te migreren zijn, volledige werkbelastingen samen migreren of ervoor kiezen om te migreren en geleidelijk schalen kunt kiezen.

Azure Traffic Manager van [gewogen](../traffic-manager/traffic-manager-configure-weighted-routing-method.md) routeringsmethode kan worden gebruikt om een deel van het binnenkomende verkeer naar Azure leiden bij het doorsturen van de meeste naar de on-premises omgeving. Deze aanpak kunt beoordelen schaal, prestaties, zoals u kunt doorgaan met het verhogen van het gewicht toegewezen naar Azure als u meer en meer van de werkbelasting van uw naar Azure migreren.

Bijvoorbeeld: **bedrijf B** kiest voor het migreren van in fasen, enkele van de omgeving van toepassing verplaatsen terwijl de rest on-premises behouden. Tijdens de eerste fasen dat wanneer het merendeel van de omgeving is on-premises een grotere gewicht is toegewezen aan de on-premises omgeving. Het Traffic manager retourneert een eindpunt op basis van gewicht is toegewezen aan het aantal beschikbare eindpunten.

![Migratie van lokale-die naar Azure](./media/concepts-traffic-manager-with-site-recovery/on-premises-migration.png)

Beide eindpunten actief zijn tijdens de migratie en het meeste verkeer wordt omgeleid naar de on-premises omgeving. Als de migratie wordt voortgezet, kan een grotere gewicht kan worden toegewezen aan het eindpunt op Azure en ten slotte het lokale eindpunt gedeactiveerde na migratie.

## <a name="azure-to-azure-failover"></a>Failover van Azure naar Azure

Voor dit voorbeeld kunt u overwegen **bedrijf C** is met alle van de toepassing-infrastructuur met Azure. Voor zakelijke continuïteit en naleving redenen **bedrijf C** beslist haar toepassingen beveiligen met Azure Site Recovery.

**Bedrijf C** toepassingen worden uitgevoerd met de openbare eindpunten en wil de mogelijkheid verkeer naadloos omleiden naar een ander Azure-regio in een gebeurtenis na noodgevallen. De [prioriteit](../traffic-manager/traffic-manager-configure-priority-routing-method.md) verkeersroutering methode kunt **bedrijf C** dit patroon failover eenvoudig te implementeren.

De installatie is als volgt:
- **Bedrijf C** maakt een [Traffic Manager-profiel](../traffic-manager/traffic-manager-create-profile.md).
- Gebruik de **prioriteit** routeringsmethode, **bedrijf C** maakt twee eindpunten – **primaire** voor de bron-regio (Azure Oost-Azië) en **Failover** voor het herstel de regio (Azure Zuidoost-Azië). **Primaire** prioriteit 1 is toegewezen en **Failover** prioriteit 2 is toegewezen.
- Aangezien de **primaire** eindpunt wordt gehost in Azure, het eindpunt kan als een [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) eindpunt.
- Met Azure Site Recovery heeft de Azure site-recovery geen alle virtuele machines of toepassingen die worden uitgevoerd voordat er failover. Ja, de **Failover** eindpunt kan worden gemaakt als een [externe](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) eindpunt.
- Standaard wordt gebruikersverkeer omgeleid naar de bron regio (Oost-Azië)-toepassing als dat eindpunt de hoogste prioriteit gekoppeld heeft. Er is geen verkeer wordt omgeleid naar de herstel-regio als de **primaire** eindpunt is in orde.

![Azure naar Azure voordat failover wordt uitgevoerd](./media/concepts-traffic-manager-with-site-recovery/azure-failover-before.png)

In een gebeurtenis na noodgevallen **bedrijf C** kunt activeren een [failover](azure-to-azure-tutorial-failover-failback.md) en herstellen van de toepassingen op de herstelserver Azure-regio. Wanneer Azure Traffic Manager detecteert dat het primaire eindpunt niet meer in orde is, gebruikt deze automatisch de **Failover** eindpunt in de DNS-antwoord en gebruikers verbinding maken met de toepassing die wordt hersteld op de herstelserver (Azure-regio Zuidoost-Azië).

![Azure naar Azure na een failover](./media/concepts-traffic-manager-with-site-recovery/azure-failover-after.png)

Afhankelijk van de zakelijke vereisten, **bedrijf C** kunt kiezen een hogere of lagere [probing frequentie](../traffic-manager/traffic-manager-monitoring.md) overschakelen tussen bron- en herstelinstellingen regio's en zorg ervoor dat de minimale downtime voor gebruikers.

Wanneer de sitedatabase is opgenomen, **bedrijf C** kunt failback vanuit Azure-regio van het herstel naar de bron van Azure-regio met Azure Site Recovery. Nu wanneer Traffic Manager gedetecteerd dat de **primaire** eindpunt opnieuw in orde is, wordt automatisch gebruikt de **primaire** eindpunt in de DNS-antwoorden.

## <a name="protecting-multi-region-enterprise-applications"></a>Meerdere landen/regio enterprise toepassingen beveiligen

Globale ondernemingen de klantervaring vaak verbeteren door het afstemmen van hun toepassingen voor het uitvoeren van regionale behoeften. Lokalisatie en de latentie te beperken, kunnen leiden tot infrastructuur verdelen over regio's. Ondernemingen zijn ook afhankelijk is van regionale wetten in bepaalde gebieden en kies een onderdeel hun infrastructuur toepassing binnen de grenzen van regionale isoleren.  

Laten we eens een voorbeeld waarin **bedrijf D** is opgesplitst in de toepassing eindpunten afzonderlijk voor Duitsland en de rest van de hele wereld. **Bedrijf D** maakt gebruik van Azure Traffic Manager [geografisch](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) routeringsmethode voor dit. Die afkomstig zijn van Duitsland verkeer wordt omgeleid naar **Eindpunt1** en alle verkeer dat afkomstig is buiten Duitsland wordt omgeleid voor **eindpunt 2**.

Het probleem met deze installatie is dat als **Eindpunt1** vastloopt om welke reden er is geen omleiding van verkeer naar **eindpunt 2**. Verkeer van Duitsland blijft worden omgeleid naar **Eindpunt1** ongeacht de status van het eindpunt verlaten Duitse gebruikers geen toegang hebben tot **bedrijf D**van toepassing. Op dezelfde manier als **eindpunt 2** gaat offline, er is geen omleiding van verkeer naar **Eindpunt1**.

![Meerdere landen/regio toepassing voordat](./media/concepts-traffic-manager-with-site-recovery/geographic-application-before.png)

Kunt u voorkomen dat dit probleem en ervoor zorgen dat toepassingen tolerantie, **bedrijf D** gebruikt [Traffic Manager-profielen genest](../traffic-manager/traffic-manager-nested-profiles.md) met Azure Site Recovery. In een geneste profielinstellingen is verkeer niet gericht op afzonderlijke eindpunten, maar in plaats daarvan aan andere Traffic Manager-profielen. Dit is de werking van deze installatie:
- In plaats van met behulp van geografische routering met afzonderlijke eindpunten **bedrijf D** geografische routering met Traffic Manager-profielen gebruikt.
- Maakt gebruik van elke onderliggende Traffic Manager-profiel **prioriteit** routering met een primaire en een eindpunt voor herstel, daarom nesten **prioriteit** routering binnen **geografisch** routering.
- De verdeling van elke werkbelasting maakt gebruik van Azure Site Recovery voor failover naar een recovery regio op basis van in het geval van een gebeurtenis na noodgevallen zodat tolerantie van toepassing.
- Wanneer de bovenliggende Traffic Manager ontvangt een DNS-query, is het omgeleid naar de relevante onderliggende Traffic Manager dat met de query met een beschikbare eindpunt overeenkomt.

![Meerdere landen/regio toepassing na](./media/concepts-traffic-manager-with-site-recovery/geographic-application-after.png)

Bijvoorbeeld, als het eindpunt in Duitsland centraal mislukt, kan de toepassing snel worden hersteld naar Duitsland noordoosten. Het nieuwe eindpunt verwerkt verkeer die afkomstig zijn van Duitsland met minimale downtime voor gebruikers. Een eindpunt storing in West-Europa kan op dezelfde manier worden verwerkt door het herstellen van de werkbelasting van de toepassing naar Noord-Europa, met Azure Traffic Manager-verwerking die DNS wordt omgeleid naar de beschikbare eindpunt.

De bovenstaande instellingen kan worden uitgebreid met zoveel regio- en eindpunt combinaties vereist. Traffic Manager kunt maximaal 10 niveaus van geneste profielen en staat niet toe dat lussen binnen de geneste configuratie.

## <a name="recovery-time-objective-rto-considerations"></a>Overwegingen voor herstel tijd Objective (RTO)

In de meeste organisaties wordt toevoegen of wijzigen van DNS-records verwerkt door een afzonderlijk team of door iemand buiten de organisatie. Hierdoor kunt u de taak van het DNS-records erg lastig te wijzigen. De tijd om bij te werken van DNS-records die andere teams of organisaties beheren DNS-infrastructuur verschilt van de organisatie en heeft impact op de RTO van de toepassing.

Door het gebruik van Traffic Manager, kunt u frontload het werk dat nodig is voor DNS-updates. Er is geen actie handmatige of een script op het moment van de werkelijke failover vereist. Deze aanpak helpt bij het snel overschakelen (en dus breek RTO) evenals vermijden kostbare tijdrovend DNS-wijziging fouten in een gebeurtenis na noodgevallen. Met Traffic Manager zelfs de stap failback wordt geautomatiseerd, die anders zelf zou moeten afzonderlijk worden beheerd.

Instellen van de juiste [probing interval](../traffic-manager/traffic-manager-monitoring.md) via basis of snelle interval health controles kunnen aanzienlijk brengt u de RTO tijdens de failover en de downtime voor gebruikers.

Bovendien kunt u de DNS Time to Live (TTL)-waarde voor het Traffic Manager-profiel optimaliseren. TTL is de waarde waarvoor een DNS-vermelding worden in de cache door een client opgeslagen zou. Voor een record zou DNS niet twee keer binnen het bereik van TTL worden opgevraagd. Elke DNS-record heeft TTL gekoppeld. Deze waarde verlaagt, resulteert in meer DNS-query's aan Traffic Manager, maar RTO kunt beperken door de detectie van storingen sneller.

De TTL is door de client ook verhogen niet als het aantal DNS-resolvers tussen de client en de gezaghebbende DNS-server verhoogt. DNS-resolvers 'aftellen' de TTL-waarde op en alleen op een TTL-waarde die overeenkomt met de verstreken tijd sinds de record in de cache is opgeslagen. Dit zorgt ervoor dat de DNS-record wordt vernieuwd op de client na de TTL, ongeacht het aantal DNS-Resolvers in de keten.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het Traffic Manager [methoden voor het doorsturen](../traffic-manager/traffic-manager-routing-methods.md).
- Meer informatie over [Traffic Manager-profielen genest](../traffic-manager/traffic-manager-nested-profiles.md).
- Meer informatie over [eindpuntcontrole](../traffic-manager/traffic-manager-monitoring.md).
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover te automatiseren.
