---
title: Geo-replicatie configureren voor Azure Cache voor Redis | Microsoft Docs
description: Leer hoe u uw Azure-Cache voor instanties van Redis gerepliceerd in geografische regio's.
services: azure-cache-for-redis
documentationcenter: ''
author: yegu-ms
manager: cfowler
editor: ''
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: azure-cache-for-redis
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: yegu
ms.openlocfilehash: d2f01c700875a33ca63a1c30d5aca4934d412d18
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113665"
---
# <a name="how-to-configure-geo-replication-for-azure-cache-for-redis"></a>Geo-replicatie configureren voor Azure Cache voor Redis

Geo-replicatie biedt een mechanisme voor het koppelen van twee Premium-laag Azure Cache voor instanties van Redis. Een cache is ingesteld als de primaire gekoppelde cache, en de andere als de gekoppelde secundaire cache. De gekoppelde secundaire cache wordt alleen-lezen en gegevens geschreven naar de primaire cache gerepliceerd naar de secundaire gekoppelde cache. Deze functionaliteit kan worden gebruikt voor het repliceren van een cache Azure-regio's. Dit artikel bevat een handleiding voor het configureren van Geo-replicatie voor de Premium-laag Azure Cache voor instanties van Redis.

## <a name="geo-replication-prerequisites"></a>Vereisten voor geo-replicatie

Voor het configureren van Geo-replicatie tussen twee caches, moeten de volgende vereisten worden voldaan:

- Beide caches moet [Premium-laag](cache-premium-tier-intro.md) in de cache opslaat.
- Beide caches moeten zich in hetzelfde Azure-abonnement.
- De gekoppelde secundaire cache moet dezelfde prijscategorie of een groter is dan de primaire gekoppelde cache-laag.
- Als de primaire gekoppelde cache clustering is ingeschakeld heeft, moet de gekoppelde secundaire cache clustering is ingeschakeld met hetzelfde aantal shards als de primaire gekoppelde cache.
- Beide caches moeten worden gemaakt en in een status running doorbrengt.
- Persistentie moet niet worden ingeschakeld op de cache.
- Geo-replicatie tussen caches in hetzelfde VNET wordt ondersteund. 
- Geo-replicatie tussen caches in gekoppelde VNETs binnen dezelfde regio is momenteel een preview-functie. De twee VNETs moeten worden geconfigureerd in zodanig dat de resources in de VNETs kunnen bereiken elkaar worden verbonden via TCP-verbindingen zijn.
- Geo-replicatie tussen caches in gekoppelde VNETs in verschillende regio's wordt nog niet ondersteund, maar wordt binnenkort in preview.

Nadat de Geo-replicatie is geconfigureerd, gelden de volgende beperkingen op de twee gekoppelde cache:

- De gekoppelde secundaire cache is alleen-lezen. u kunt lezen uit, maar u kunt geen gegevens kan niet schrijven naar het. 
- Alle gegevens die in de gekoppelde secundaire cache was voordat de koppeling is toegevoegd wordt verwijderd. Als de Geo-replicatie later echter wordt verwijderd, blijft de gerepliceerde gegevens in de gekoppelde secundaire cache.
- U kunt geen starten een [bewerking schalen](cache-how-to-scale.md) op een van beide cache of [wijzigen van het aantal shards](cache-how-to-premium-clustering.md) als de cache clustering is ingeschakeld heeft.
- U kunt de persistentie van de cache niet inschakelen.
- Kunt u [exporteren](cache-how-to-import-export-data.md#export) met een cache, maar u kunt alleen [importeren](cache-how-to-import-export-data.md#import) in de primaire gekoppelde cache.
- U kunt gekoppelde cache of de resourcegroep waarin ze, totdat u de koppeling voor Geo-replicatie niet verwijderen. Zie voor meer informatie, [waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Als de twee caches in verschillende regio's, zijn de kosten voor uitgaand netwerkverkeer van toepassing op de gegevens gerepliceerd in regio's naar de secundaire gekoppelde cache. Zie voor meer informatie, [hoeveel kost het repliceren van mijn gegevens op Azure-regio's?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Er is geen automatische failover naar de secundaire gekoppelde cache als de primaire cache (en de bijbehorende replica) gaat u naar beneden. In de volgorde voor failover-clienttoepassingen moet u handmatig verwijderen van de koppeling voor Geo-replicatie en wijst u de clienttoepassingen die tot de cache die voorheen de gekoppelde secundaire cache was. Zie voor meer informatie, [hoe werkt failover wordt uitgevoerd naar de secundaire gekoppelde cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Een Geo-replicatie-koppeling toevoegen

1. Als u wilt koppelen twee premium-caches samen voor geo-replicatie, klikt u op **Geo-replicatie** in het menu Resource van de cache is echter bedoeld als de primaire gekoppeld in de cache en klik vervolgens op **koppeling voor cachereplicatie toevoegen** uit de **Geo-replicatie** blade.

    ![Koppeling toevoegen](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Klik op de naam van de gewenste secundaire cache van de **compatibele caches** lijst. Als de gewenste cache niet wordt weergegeven in de lijst, Controleer de [Geo-replicatie vereisten](#geo-replication-prerequisites) voor de gewenste secundaire cache wordt voldaan. Als u wilt filteren de caches per regio, klikt u op de gewenste regio op de kaart om weer te geven alleen deze caches in de **compatibele caches** lijst.

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

    De primaire gekoppelde cache blijft beschikbaar voor gebruik tijdens het koppelen, maar de gekoppelde secundaire cache is niet beschikbaar totdat het koppelingsproces is voltooid.

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

- Wanneer twee caches koppelen voor Geo-replicatie, de primaire gekoppelde cache blijft beschikbaar voor gebruik, maar de gekoppelde secundaire cache is niet beschikbaar totdat het koppelingsproces is voltooid.
- Wanneer u de koppeling voor Geo-replicatie tussen twee caches verwijdert, blijven zowel caches beschikbaar voor gebruik.

### <a name="can-i-link-more-than-two-caches-together"></a>Kan ik meer dan twee caches koppelen?

Nee, bij het gebruik van Geo-replicatie kunt u alleen koppelen twee caches samen.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Kan ik twee caches uit verschillende Azure-abonnementen koppelen?

Nee, beide caches moeten zich in hetzelfde Azure-abonnement.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Kan ik twee caches met verschillende formaten koppelen?

Ja, zolang de gekoppelde secundaire cache groter dan de primaire gekoppelde cache is.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Kan ik Geo-replicatie gebruiken met clustering is ingeschakeld?

Ja, zolang beide caches hetzelfde aantal shards hebben.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Kan ik Geo-replicatie met mijn caches in een VNET gebruiken?

Ja, Geo-replicatie van caches in VNETs worden ondersteund. 

- Geo-replicatie tussen caches in hetzelfde VNET wordt ondersteund.
- Geo-replicatie tussen caches in verschillende VNETs wordt ook ondersteund als de twee VNETs zijn geconfigureerd in zodanig dat de resources in de VNETs kunnen bereiken elkaar worden verbonden via TCP-verbindingen zijn.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>Wat is het replicatieschema voor Redis geo-replicatie?

Replicatie wordt niet uitgevoerd op een specifiek schema is continue en asynchrone Internet Explorer alle schrijfbewerkingen naar de primaire gedaan worden onmiddellijk asynchroon gerepliceerd op de secundaire server.

### <a name="how-long-does-geo-replication-replication-take"></a>Hoe lang duurt geo-replicatie-replicatie?

Replicatie is incrementele, asynchrone en continue en de tijd die nodig is doorgaans niet veel af van de latentie tussen regio's. Onder bepaalde omstandigheden op bepaalde tijden de secundaire mogelijk moet een volledige synchronisatie van de gegevens van de primaire doen. De tijd van de replicatie in dit geval is afhankelijk van factoren zoals aantal: belasting van de primaire cache, de beschikbare bandbreedte op de machine cache inter regio latentie, enzovoort. Een voorbeeld: op basis van enkele tests die we hebben ontdekt dat moment replicatie voor een volledige 53 GB geo-replicatie Koppel in Oost VS en VS-West-regio's waar dan ook kunnen liggen tussen 5 tot 10 minuten.

### <a name="is-the-replication-recovery-point-guaranteed"></a>Kan het herstelpunt van de replicatie worden gegarandeerd?

Voor caches in een modus voor geo-replicatie, persistentie en import/export-functionaliteit op dit moment uitgeschakeld. Dus in het geval van een klant geïnitieerd failover of in gevallen waarin een replicatiekoppeling verbroken tussen de twee geo-replicatie is, de in het geheugen die de secundaire behouden, gegevens die deze vanuit de primaire tot dat moment is gesynchroniseerd. Er is geen garantie recovery point in dergelijke situaties.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Kan ik PowerShell of Azure CLI gebruiken voor het beheren van Geo-replicatie?

U kunt op dit moment alleen Geo-replicatie met behulp van de Azure-portal beheren.

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Hoeveel kost het repliceren van mijn gegevens in Azure-regio's?

Bij het gebruik van Geo-replicatie, worden gegevens uit de primaire gekoppelde cache wordt gerepliceerd naar de secundaire gekoppelde cache. Als de twee gekoppelde caches zich in dezelfde Azure-regio, zijn er geen kosten voor de overdracht van gegevens. Als de twee gekoppelde caches zich in verschillende Azure-regio's, is de kosten voor gegevensoverdracht met Geo-replicatie de bandbreedtekosten van het repliceren van gegevens naar de andere Azure-regio. Zie voor meer informatie, [Bandwidth Pricing Details](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>Waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?

Wanneer twee caches zijn gekoppeld, kunt u de cache of de resourcegroep waarin ze totdat u de koppeling voor Geo-replicatie niet verwijderen. Als u probeert te verwijderen van de resourcegroep met een of beide van de gekoppelde caches, worden de andere resources in de resourcegroep verwijderd, maar de resourcegroep blijft de `deleting` systeemstatus- en alle gekoppelde caches in de resourcegroep in de blijven`running`staat. Voor het voltooien van de verwijdering van de resourcegroep en de gekoppelde caches daarin de Geo-replicatie koppeling verbroken zoals beschreven in [verwijderen van een Geo-replicatie koppeling](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>Welke regio moet ik voor mijn secundaire gekoppelde cache gebruiken?

In het algemeen is het aanbevolen voor uw cache in dezelfde Azure-regio als de toepassing die toegang heeft tot deze voor te komen. Als uw toepassing een primaire en alternatieve regio heeft, moeten uw primaire en secundaire caches bestaan in dezelfde regio's. Zie voor meer informatie over gekoppelde regio's, [aanbevolen procedures: Azure gekoppelde regio's](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>Hoe werkt de failover wordt uitgevoerd naar de secundaire gekoppelde cache?

In de eerste release van Geo-replicatie ondersteunt Azure Cache voor Redis geen automatische failover Azure-regio's. Geo-replicatie wordt voornamelijk gebruikt in een noodherstelscenario. In een scenario voor het herstel van distater moeten klanten de volledige toepassingsstack in een back-up regio tevoorschijn in een gecoördineerde manier samenwerken in plaats van afzonderlijke toepassingsonderdelen bepalen wanneer u overschakelen naar de back-ups op hun eigen laten. Dit is vooral relevant zijn voor Redis. Een van de belangrijkste voordelen van Redis is een zeer lage latentie-store. Als Redis wordt gebruikt door een toepassing wordt overgenomen door een andere Azure-regio, maar niet voor de compute-laag, zouden toegevoegd round trip time een merkbare invloed hebben op prestaties. Om deze reden graag willen we Redis mislukt via automatisch voorkomen vanwege problemen met de tijdelijke beschikbaarheid.

Op dit moment voor het starten van de failover, moet u de koppeling Geo-replicatie verwijderen in Azure portal, en wijzig vervolgens het eindpunt van de verbinding in de Redis-client uit de primaire gekoppelde cache in de (voorheen gekoppelde) secundaire cache. Wanneer de twee caches zijn losgekoppeld, wordt de replica een reguliere lezen / schrijven-cache opnieuw wordt en accepteert aanvragen rechtstreeks vanuit de Redis-clients.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Azure Cache voor Premium-laag Redis](cache-premium-tier-intro.md).

