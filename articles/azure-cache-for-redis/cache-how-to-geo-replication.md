---
title: Geo-replicatie configureren voor Azure Cache voor Redis | Microsoft Docs
description: Leer hoe u uw Azure-Cache voor instanties van Redis gerepliceerd in geografische regio's.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/06/2019
ms.author: yegu
ms.openlocfilehash: 4254175955c3560c7bd0fdd08c6b60c318238b76
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57991576"
---
# <a name="how-to-configure-geo-replication-for-azure-cache-for-redis"></a>Geo-replicatie configureren voor Azure Cache voor Redis

Geo-replicatie biedt een mechanisme voor het koppelen van twee Premium-laag Azure Cache voor instanties van Redis. Een cache is gekozen als de primaire gekoppelde cache, en de andere als de gekoppelde secundaire cache. De gekoppelde secundaire cache wordt alleen-lezen en gegevens geschreven naar de primaire cache gerepliceerd naar de secundaire gekoppelde cache. Deze functionaliteit kan worden gebruikt voor het repliceren van een cache Azure-regio's. Dit artikel bevat een handleiding voor het configureren van Geo-replicatie voor de Premium-laag Azure Cache voor instanties van Redis.

## <a name="geo-replication-prerequisites"></a>Vereisten voor geo-replicatie

Voor het configureren van Geo-replicatie tussen twee caches, moeten de volgende vereisten worden voldaan:

- Beide caches zijn [Premium-laag](cache-premium-tier-intro.md) in de cache opslaat.
- Beide caches zijn in hetzelfde Azure-abonnement.
- De gekoppelde secundaire cache is dezelfde grootte van de cache of een cache groter dan de primaire gekoppelde cache.
- Beide caches worden gemaakt en in een status running doorbrengt.

Sommige functies worden niet ondersteund met geo-replicatie:

- Geo-replicatie wordt niet ondersteund voor persistentie.
- Clustering wordt ondersteund als beide caches clustering is ingeschakeld hebben en hetzelfde aantal shards.
- Caches in hetzelfde VNET worden ondersteund.
- Caches in verschillende VNETs worden ondersteund met waarschuwingen. Zie [kan ik Geo-replicatie gebruiken met mijn caches in een VNET?](#can-i-use-geo-replication-with-my-caches-in-a-vnet) voor meer informatie.

Nadat de Geo-replicatie is geconfigureerd, gelden de volgende beperkingen op de twee gekoppelde cache:

- De gekoppelde secundaire cache is alleen-lezen. u kunt lezen uit, maar u kunt geen gegevens kan niet schrijven naar het. 
- Alle gegevens die in de gekoppelde secundaire cache was voordat de koppeling is toegevoegd wordt verwijderd. Als de Geo-replicatie later is verwijderd echter de gerepliceerde gegevens blijven in de gekoppelde secundaire cache.
- U kunt geen [schaal](cache-how-to-scale.md) beide cache terwijl de caches zijn gekoppeld.
- U kunt geen [wijzigen van het aantal shards](cache-how-to-premium-clustering.md) als de cache clustering is ingeschakeld heeft.
- U kunt de persistentie van de cache niet inschakelen.
- U kunt [exporteren](cache-how-to-import-export-data.md#export) uit de cache.
- U kunt geen [importeren](cache-how-to-import-export-data.md#import) in de gekoppelde secundaire cache.
- U kunt gekoppelde cache of de resourcegroep waarin ze, totdat u de caches ontkoppelen niet verwijderen. Zie voor meer informatie, [waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Als de cache zich in verschillende regio's, worden kosten voor uitgaand netwerkverkeer toepassen op de gegevens verplaatst tussen regio's. Zie voor meer informatie, [hoeveel kost het repliceren van mijn gegevens op Azure-regio's?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Automatische failover optreden niet tussen de primaire en secundaire gekoppelde cache. Zie voor meer informatie en informatie over het failover een clienttoepassing [hoe werkt failover wordt uitgevoerd naar de secundaire gekoppelde cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Een Geo-replicatie-koppeling toevoegen

1. Als u wilt koppelen twee caches samen voor geo-replicatie, klikt u op voor het eerst **Geo-replicatie** in het menu Resource van de cache die u van plan bent om te worden van de primaire cache gekoppeld. Klik vervolgens op **koppeling voor cachereplicatie toevoegen** uit de **Geo-replicatie** blade.

    ![Koppeling toevoegen](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Klik op de naam van uw beoogde secundaire cache van de **compatibele caches** lijst. Als uw secundaire cache niet wordt weergegeven in de lijst, Controleer de [Geo-replicatie vereisten](#geo-replication-prerequisites) voor de secundaire cache wordt voldaan. Als u wilt filteren de caches per regio, klikt u op de regio in de kaart om weer te geven alleen deze caches in de **compatibele caches** lijst.

    ![Compatibele caches geo-replicatie](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    U kunt ook het koppelingsproces starten of Geef details weer over de secundaire cache met behulp van het contextmenu.

    ![Contextmenu voor geo-replicatie](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Klik op **koppeling** voor de twee caches aan elkaar koppelen en beginnen met het replicatieproces.

    ![Caches koppelen](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. U kunt de voortgang van het replicatieproces weergeven op de **Geo-replicatie** blade.

    ![Status koppelen](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    U ziet ook de status van de koppeling op de **overzicht** blade voor de primaire en secundaire cache.

    ![Status van de cache](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Zodra het replicatieproces voltooid is, de **status koppelen** wordt gewijzigd in **geslaagd**.

    ![Status van de cache](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    De primaire gekoppelde cache blijft beschikbaar voor gebruik tijdens het koppelen. De gekoppelde secundaire cache is niet beschikbaar totdat het koppelingsproces is voltooid.

## <a name="remove-a-geo-replication-link"></a>Een Geo-replicatie-koppeling verwijderen

1. Als u wilt verwijderen van de koppeling tussen twee caches en Geo-replicatie beëindigen, klikt u op **caches ontkoppelen** uit de **Geo-replicatie** blade.
    
    ![Caches ontkoppelen](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Wanneer het proces voor de koppeling is voltooid, is de secundaire cache is beschikbaar voor zowel lees- en schrijfbewerkingen.

>[!NOTE]
>Wanneer de koppeling voor Geo-replicatie wordt verwijderd, blijft de gerepliceerde gegevens uit de primaire gekoppelde cache in de secundaire cache.
>
>

## <a name="geo-replication-faq"></a>Veelgestelde vragen over geo-replicatie

- [Kan ik Geo-replicatie gebruiken met een Standard- of Basic-laag-cache?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Mijn cache is beschikbaar voor gebruik tijdens het koppelen of ontkoppelen?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [Kan ik meer dan twee caches koppelen?](#can-i-link-more-than-two-caches-together)
- [Kan ik twee caches uit verschillende Azure-abonnementen koppelen?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Kan ik twee caches met verschillende formaten koppelen?](#can-i-link-two-caches-with-different-sizes)
- [Kan ik Geo-replicatie gebruiken met clustering is ingeschakeld?](#can-i-use-geo-replication-with-clustering-enabled)
- [Kan ik Geo-replicatie met mijn caches in een VNET gebruiken?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Wat is het replicatieschema voor Redis geo-replicatie?](#what-is-the-replication-schedule-for-redis-geo-replication)
- [Hoe lang duurt geo-replicatie-replicatie?](#how-long-does-geo-replication-replication-take)
- [Kan het herstelpunt van de replicatie worden gegarandeerd?](#is-the-replication-recovery-point-guaranteed)
- [Kan ik PowerShell of Azure CLI gebruiken voor het beheren van Geo-replicatie?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Hoeveel kost het repliceren van mijn gegevens in Azure-regio's?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [Welke regio moet ik voor mijn secundaire gekoppelde cache gebruiken?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [Hoe werkt de failover wordt uitgevoerd naar de secundaire gekoppelde cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Kan ik Geo-replicatie gebruiken met een Standard- of Basic-laag-cache?

Nee, Geo-replicatie is alleen beschikbaar voor Premium-laag-caches.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>Mijn cache is beschikbaar voor gebruik tijdens het koppelen of ontkoppelen?

- Wanneer u een koppeling, blijft de primaire gekoppelde cache beschikbaar terwijl het koppelingsproces is voltooid.
- Wanneer u een koppeling, is de secundaire gekoppelde cache niet beschikbaar totdat het koppelingsproces is voltooid.
- Bij het ontkoppelen, wordt met beide caches beschikbaar blijven tijdens het ontkoppelen proces is voltooid.

### <a name="can-i-link-more-than-two-caches-together"></a>Kan ik meer dan twee caches koppelen?

Nee, u kunt alleen twee caches aan elkaar koppelen.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Kan ik twee caches uit verschillende Azure-abonnementen koppelen?

Nee, beide caches moeten zich in hetzelfde Azure-abonnement.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Kan ik twee caches met verschillende formaten koppelen?

Ja, zolang de gekoppelde secundaire cache groter dan de primaire gekoppelde cache is.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Kan ik Geo-replicatie gebruiken met clustering is ingeschakeld?

Ja, zolang beide caches hetzelfde aantal shards hebben.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Kan ik Geo-replicatie met mijn caches in een VNET gebruiken?

Ja, Geo-replicatie van caches in vnet's wordt ondersteund met aanvullende opmerkingen:

- Geo-replicatie tussen caches in hetzelfde VNET wordt ondersteund.
- Geo-replicatie tussen caches in verschillende VNETs wordt ook ondersteund.
  - Als de VNETs zich in dezelfde regio bevinden, kunt u deze verbinden met behulp van [VNET-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) of een [VPN-Gateway VNET-naar-VNET-verbinding](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#V2V).
  - Als de VNETs zich in verschillende regio's, geo-replicatie met behulp van VNET-peering wordt niet ondersteund vanwege een beperking met Basic interne load balancers. Zie voor meer informatie over VNET-peering beperkingen [Virtual Network - Peering - vereisten en beperkingen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#requirements-and-constraints). De aanbevolen oplossing is het gebruik van een VPN-Gateway VNET-naar-VNET-verbinding.

Met behulp van [deze Azure-sjabloon](https://azure.microsoft.com/resources/templates/201-redis-vnet-geo-replication/), u kunt snel implementeren met twee caches met geo-replicatie in een VNET dat is verbonden met een VPN-Gateway VNET-naar-VNET-verbinding.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>Wat is het replicatieschema voor Redis geo-replicatie?

Replicatie is continue en asynchrone en gebeurt niet op een specifiek schema. Alle schrijfbewerkingen naar de primaire gedaan worden onmiddellijk en asynchroon gerepliceerd op de secundaire server.

### <a name="how-long-does-geo-replication-replication-take"></a>Hoe lang duurt geo-replicatie-replicatie?

Replicatie is incrementele, asynchrone en continue en de tijd die niet veel af van de latentie tussen regio's. Onder bepaalde omstandigheden de secundaire cache mogelijk moet een volledige synchronisatie van de gegevens van de primaire doen. De tijd van de replicatie in dit geval is afhankelijk van factoren zoals aantal: belasting van de primaire cache, de beschikbare netwerkbandbreedte en de latentie tussen regio's. We hebben gevonden tijd van de replicatie voor een volledige 53 GB geo-replicatie paar overal kan liggen tussen 5 tot 10 minuten.

### <a name="is-the-replication-recovery-point-guaranteed"></a>Kan het herstelpunt van de replicatie worden gegarandeerd?

Voor caches in een modus voor geo-replicatie, persistentie uitgeschakeld. Als de combinatie van een geo-replicatie ontkoppeld, zoals een failover gestart door de klant is, blijft de secundaire cache van de gekoppelde de gesynchroniseerde gegevens op dat moment. Er is geen herstelpunt kan worden gegarandeerd in dergelijke situaties.

Verkrijgen van een herstelpunt [exporteren](cache-how-to-import-export-data.md#export) uit de cache. U kunt later [importeren](cache-how-to-import-export-data.md#import) in de primaire gekoppelde cache.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Kan ik PowerShell of Azure CLI gebruiken voor het beheren van Geo-replicatie?

Geo-replicatie kan Ja, worden beheerd met behulp van de Azure portal, PowerShell of Azure CLI. Zie voor meer informatie de [PowerShell-documenten](https://docs.microsoft.com/powershell/module/az.rediscache/?view=azps-1.4.0#redis_cache) of [Azure CLI-documenten](https://docs.microsoft.com/cli/azure/redis/server-link?view=azure-cli-latest).

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Hoeveel kost het repliceren van mijn gegevens in Azure-regio's?

Bij het gebruik van Geo-replicatie, worden gegevens uit de primaire gekoppelde cache wordt gerepliceerd naar de secundaire gekoppelde cache. Er zijn geen kosten voor de overdracht van gegevens als de twee gekoppelde caches in dezelfde regio zijn. Als de twee gekoppelde caches in verschillende regio's, is de kosten voor gegevensoverdracht met de netwerkkosten voor uitgaand verkeer van de gegevens verplaatst in beide regio. Zie voor meer informatie, [Bandwidth Pricing Details](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>Waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?

Caches geo-replicatie en hun resourcegroepen kunnen niet worden verwijderd terwijl de gekoppelde totdat u de koppeling voor geo-replicatie. Als u probeert te verwijderen van de resourcegroep met een of beide van de gekoppelde caches, worden de andere resources in de resourcegroep verwijderd, maar de resourcegroep blijft de `deleting` systeemstatus- en alle gekoppelde caches in de resourcegroep in de blijven`running`staat. Als u wilt de resourcegroep en de gekoppelde caches binnen het volledig verwijdert, de caches ontkoppelen zoals beschreven in [verwijderen van een Geo-replicatie koppeling](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>Welke regio moet ik voor mijn secundaire gekoppelde cache gebruiken?

Het verdient in het algemeen voor uw cache in dezelfde Azure-regio als de toepassing die toegang heeft tot deze voor te komen. Het raadzaam dat uw primaire en secundaire caches aanwezig zijn in dezelfde regio's voor toepassingen met afzonderlijke primaire en alternatieve regio's. Zie voor meer informatie over gekoppelde regio's, [aanbevolen procedures: Azure gekoppelde regio's](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>Hoe werkt de failover wordt uitgevoerd naar de secundaire gekoppelde cache?

Automatische failover Azure-regio's wordt niet ondersteund voor caches geo-replicatie. In een scenario voor herstel na noodgevallen weer klanten van de gehele toepassing-stack op een gecoördineerde manier in hun back-regio. Afzonderlijke toepassing componenten bepalen laten bij het overschakelen naar de back-ups op hun eigen kan de prestaties nadelig beïnvloeden. Een van de belangrijkste voordelen van Redis is een zeer lage latentie-store. Als de hoofdtoepassing van de klant zich in een andere regio dan de cache, zou de toegevoegde retourtijd een merkbare invloed hebben op prestaties. Om deze reden wordt voorkomen dat automatisch Failover-overschakeling uitvoeren vanwege problemen met de tijdelijke beschikbaarheid.

Voor het starten van een failover gestart door de klant, moet u eerst de caches ontkoppelen. Wijzig uw Redis-client voor het gebruik van het verbindingseindpunt van het (voorheen gekoppelde) secundaire cache. Wanneer de twee caches losgekoppeld zijn, wordt de secundaire cache een reguliere lezen / schrijven-cache opnieuw wordt en accepteert aanvragen rechtstreeks vanuit de Redis-clients.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Azure Cache voor Premium-laag Redis](cache-premium-tier-intro.md).
