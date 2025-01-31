---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: singhkays
ms.service: virtual-machines
ms.topic: include
ms.date: 05/18/2018
ms.author: kasing
ms.custom: include file
ms.openlocfilehash: c4f9334a91e99c92e74af060d84308a8aaccc6e7
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671254"
---
# <a name="frequently-asked-questions-about-classic-to-azure-resource-manager-migration"></a>Veelgestelde vragen over de migratie van klassiek naar Azure Resource Manager

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Is dit migratieplan van invloed op mijn bestaande services en toepassingen die worden uitgevoerd op virtuele Azure-machines? 

Nee. De VM's (klassiek) zijn volledig ondersteunde services met een algemene beschikbaarheid. U kunt deze resources blijven gebruiken om uw footprint in Microsoft Azure te vergroten.

## <a name="what-happens-to-my-vms-if-i-dont-plan-on-migrating-in-the-near-future"></a>Wat gebeurt er met mijn virtuele machines als ik niet van plan ben om in de nabije toekomst te migreren? 

De bestaande klassieke API's en het bestaande resourcemodel worden niet buiten gebruik gesteld. Het is de bedoeling om migreren eenvoudig te maken omdat er veel geavanceerde functies beschikbaar zijn in het Resource Manager-implementatiemodel. Het wordt aanbevolen om [enkele van de ontwikkelingen](../articles/azure-resource-manager/resource-manager-deployment-model.md) te bekijken die deel uitmaken van IaaS via Resource Manager.

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Wat betekent dit migratieplan voor mijn bestaande tooling? 

Het bijwerken van uw tooling voor het Resource Manager-implementatiemodel is een van de belangrijkste veranderingen die moet worden doorgevoerd voor uw migratieplannen.

## <a name="how-long-will-the-management-plane-downtime-be"></a>Hoe lang duurt de downtime voor het management? 

Dit is afhankelijk van het aantal resources dat wordt gemigreerd. Bij kleinere implementaties (enkele tientallen virtuele machines) zou de volledige migratie minder dan een uur moeten duren. Bij grootschalige implementaties (honderden virtuele machines) kan de migratie enkele uren duren.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Kan ik het migreren van mijn resources ongedaan maken nadat de resources zijn doorgevoerd in Resource Manager? 

U kunt de migratie afbreken wanneer de resources zich nog in de staat Voorbereid bevinden. U kunt het migreren niet meer ongedaan maken wanneer de resources eenmaal zijn doorgevoerd.

## <a name="can-i-roll-back-my-migration-if-the-commit-operation-fails"></a>Kan ik de migratie ongedaan maken wanneer de doorvoerbewerking is mislukt? 

U kunt de migratie niet afbreken als wanneer de doorvoerbewerking is mislukt. Alle migratiebewerkingen, met inbegrip van de doorvoerbewerking, zijn idempotent. Daarom is het raadzaam om de bewerking na een korte tijd opnieuw uit te voeren. Als de bewerking blijft mislukken, maakt u een ondersteuningsticket of een bericht met de tag ClassicIaaSMigration op ons [VM-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).

## <a name="do-i-have-to-buy-another-express-route-circuit-if-i-have-to-use-iaas-under-resource-manager"></a>Moet ik nog een ExpressRoute-circuit kopen als ik IaaS via Resource Manager wil gebruiken? 

Nee. Recent is het [verplaatsen van ExpressRoute-circuits van het klassieke naar het Resource Manager-implementatiemodel](../articles/expressroute/expressroute-move.md) ingeschakeld. U hoeft geen nieuw ExpressRoute-circuit te kopen als u er al een hebt.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Wat gebeurt er als ik op rollen gebaseerd toegangsbeheerbeleid heb geconfigureerd voor mijn klassieke IaaS-resources? 

Tijdens de migratie worden de klassieke resources Resource Manager-resources. Daarom is het raadzaam dat u van plan de RBAC-beleid-updates die moeten bent worden uitgevoerd na de migratie.

## <a name="i-backed-up-my-classic-vms-in-a-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Kan ik back-ups van mijn klassieke virtuele machines in een kluis. Kan ik mijn virtuele machines migreren van de klassieke modus naar de Resource Manager-modus en ze beschermen in een Recovery Services-kluis?

<a name="vault">Wanneer</a> u een virtuele machine verplaatsen van klassiek naar Resource Manager-modus, back-ups die vóór de migratie worden niet gemigreerd naar de onlangs gemigreerde Resource Manager-VM. Als u houden van uw back-ups van klassieke virtuele machines wilt, volgt u deze stappen vóór de migratie. 

1. In de Recovery Services-kluis, gaat u naar de **beveiligde Items** tabblad en selecteer de virtuele machine. 
2. Klik op Stop de beveiliging. Laat de optie *Gekoppelde back-upgegevens verwijderen* **uitgeschakeld**.

> [!NOTE]
> U wordt gefactureerd kosten van back-instantie totdat u de gegevens bewaren. Back-ups wordt aan de hand van bewaartermijn worden verwijderd. Laatste back-up is echter altijd bewaard totdat u expliciet back-upgegevens verwijdert. Het wordt aanbevolen om te controleren of de bewaartermijn van de virtuele machine en het activeren 'Back-upgegevens verwijderen' op het beveiligde item in de kluis zodra de bewaartermijn uitgevoerd is. 
>
>

De virtuele machine migreren naar Resource Manager-modus 

1. Verwijder de back-up-/momentopname-extensie uit de VM.
2. Migreer de virtuele machines van de klassieke modus naar de Resource Manager-modus. Zorg ervoor dat de opslagruimte en de netwerkgegevens die corresponderen met de virtuele machine, ook naar de Resource Manager-modus worden gemigreerd.

Bovendien, als u back-up van de gemigreerde virtuele machine wilt, gaat u naar de VM-beheerblade op [back-up inschakelen](../articles/backup/quick-backup-vm-portal.md#enable-backup-on-a-vm).

## <a name="can-i-validate-my-subscription-or-resources-to-see-if-theyre-capable-of-migration"></a>Kan ik mijn abonnement of resources valideren om te ontdekken of ze geschikt zijn voor migratie? 

Ja. Bij de door het platform ondersteunde migratieoptie is de eerste stap van het voorbereiden op de migratie het controleren of de resources geschikt zijn voor migratie. Als de validatiebewerking mislukt, ontvangt u berichten over alle redenen waarom de migratie niet kan worden voltooid.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-the-iaas-resources-for-migration"></a>Wat gebeurt er als er een quotumfout optreedt bij het voorbereiden van de IaaS-resources op migratie? 

Het wordt aanbevolen om de migratie af te breken en daarna een ondersteuningsaanvraag in te dienen voor het verhogen van de quota in de regio waarin u de VM's wilt migreren. Wanneer het quotaverzoek is goedgekeurd, kunt u de migratiestappen opnieuw uitvoeren.

## <a name="how-do-i-report-an-issue"></a>Hoe meld ik een probleem? 

Plaats berichten over uw problemen met migratie en vragen over migratie op ons [VM-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows), met het trefwoord ClassicIaaSMigration. Het wordt aanbevolen om al uw vragen op dit forum te plaatsen. Als u een ondersteuningscontract hebt, kunt u ook een ondersteuningsticket aanmaken.

## <a name="what-if-i-dont-like-the-names-of-the-resources-that-the-platform-chose-during-migration"></a>Wat kan ik doen als ik de resourcenamen niet leuk vind die het platform heeft gekozen tijdens de migratie? 

Alle resources waarvoor u expliciet namen opgeeft in het klassieke implementatiemodel, behouden die naam tijdens de migratie. In sommige gevallen worden er nieuwe resources gemaakt. Er wordt dan bijvoorbeeld een netwerkinterface gemaakt voor elke VM. Momenteel wordt de mogelijkheid niet ondersteund om invloed uit te oefenen op de namen van de nieuwe resources die tijdens de migratie worden gemaakt. Geef het via het [Azure-feedbackforum](https://feedback.azure.com) aan als u deze mogelijkheid graag zou willen laten toevoegen.

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Kan ik ExpressRoute-circuits migreren die voor verschillende abonnementen met autorisatielinks worden gebruikt? 

ExpressRoute-circuits met abonnementsoverstijgende autorisatielinks kunnen niet automatisch worden gemigreerd zonder downtime. Er is informatie beschikbaar over het uitvoeren van handmatige migratie. Zie [ExpressRoute-circuits en de bijbehorende virtuele netwerken van het klassieke naar het Resource Manager-implementatiemodel migreren](../articles/expressroute/expressroute-migration-classic-resource-manager.md) voor stappen en meer informatie.

## <a name="i-got-the-message-vm-is-reporting-the-overall-agent-status-as-not-ready-hence-the-vm-cannot-be-migrated-ensure-that-the-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-the-vm-hence-this-vm-cannot-be-migrated"></a>Ik krijg het bericht *' virtuele machine rapporteert de algehele agentstatus niet gereed. Hence, the VM cannot be migrated. Zorg ervoor dat de VM-Agent is reporting overall agentstatus as Ready'* of *'virtuele machine bevat de extensie waarvan u de Status van de virtuele machine niet wordt gerapporteerd. Daarom kan kan niet deze virtuele machine worden gemigreerd."*

Dit bericht wordt weergegeven wanneer de VM geen uitgaande verbinding heeft met internet. De VM-agent maakt gebruik van een uitgaande verbinding om het Azure-opslagaccount te bereiken. Zo kan de agentstatus elke vijf minuten worden bijgewerkt.
