---
title: Geo-replicatie configureren voor Azure Redis-Cache | Microsoft Docs
description: Informatie over het repliceren van uw Azure Redis-Cache-exemplaren in geografische regio's.
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: wesmc
ms.openlocfilehash: 883683f6af7943fa4da49095c9a15aefd5cfa719
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-geo-replication-for-azure-redis-cache"></a>Geo-replicatie configureren voor Azure Redis-Cache

Geo-replicatie biedt een mechanisme voor het koppelen van twee exemplaren van Premium-laag Azure Redis-Cache. Een cache is aangewezen als het primaire gekoppelde cachegeheugen en de andere als de secundaire gekoppelde cache. De secundaire gekoppelde cache wordt alleen-lezen en gegevens naar de primaire cache geschreven worden gerepliceerd naar de secundaire gekoppelde cache. Deze functionaliteit kan worden gebruikt voor het repliceren van een cache in Azure-regio's. Dit artikel bevat een handleiding voor het configureren van Geo-replicatie voor de Premium-laag Azure Redis-Cache-exemplaren.

## <a name="geo-replication-prerequisites"></a>Geo-replicatie-vereisten

Geo-replicatie tussen de twee caches configureren, moeten de volgende vereisten worden voldaan:

- Beide caches moet [Premium-laag](cache-premium-tier-intro.md) in de cache opslaat.
- Beide caches moeten zich in dezelfde Azure-abonnement.
- De secundaire gekoppelde cache moet op de prijscategorie van de dezelfde of een grotere prijscategorie dan de primaire gekoppelde cache.
- Als de primaire gekoppelde cache clusteren is ingeschakeld heeft, moet de secundaire gekoppelde cache hebben clustering met hetzelfde aantal shards als de primaire gekoppelde cache ingeschakeld.
- Beide caches moeten worden gemaakt en actief is.
- Persistentie moet niet worden ingeschakeld in de cache.
- Geo-replicatie tussen caches in hetzelfde VNET wordt ondersteund. Geo-replicatie tussen caches in verschillende VNETs wordt ook ondersteund als de twee VNETs die zijn geconfigureerd in zodanig dat de resources in de VNETs kunnen bereiken elkaar via TCP-verbindingen zijn.

Nadat Geo-replicatie is geconfigureerd, gelden de volgende beperkingen naar uw paar gekoppelde cache:

- De secundaire gekoppelde cache is alleen-lezen. kan in lezen, maar u kunt geen gegevens kan niet schrijven naar het. 
- De gegevens in de cache van de gekoppelde secundaire voordat de koppeling is toegevoegd, wordt verwijderd. Als de Geo-replicatie wordt vervolgens echter verwijderd, blijft de gerepliceerde gegevens in de secundaire gekoppelde cache.
- U kan niet starten een [bewerking schalen](cache-how-to-scale.md) in de cache of [wijzigen van het aantal shards](cache-how-to-premium-clustering.md) als het cachegeheugen clusteren is ingeschakeld.
- U kunt de persistentie van de cache niet inschakelen.
- U kunt [exporteren](cache-how-to-import-export-data.md#export) met de cache, maar u kunt alleen [importeren](cache-how-to-import-export-data.md#import) in de primaire gekoppelde cache.
- U kunt gekoppelde cache of de resourcegroep waarin ze, totdat u de koppeling Geo-replicatie niet verwijderen. Zie voor meer informatie [waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Als de twee caches zich in verschillende regio's, zijn de kosten voor uitgaand netwerkverkeer van toepassing op de gegevens gerepliceerd tussen regio's naar de secundaire gekoppelde cache. Zie voor meer informatie [hoeveel kost het repliceren van mijn gegevens op Azure-regio's?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Er is geen automatische failover naar de secundaire gekoppelde cache als de primaire-cache (en de replica) uitvallen. In de volgorde voor failover-clienttoepassingen moet u handmatig verwijderen van de koppeling Geo-replicatie en wijst u de clienttoepassingen aan het cachegeheugen die de secundaire gekoppelde cache voorheen. Zie voor meer informatie [hoe werkt worden niet via de gekoppelde secundaire-cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Een Geo-replicatie-koppeling toevoegen

1. Als u wilt twee premium caches aan elkaar koppelt voor geo-replicatie, klikt u op **Geo-replicatie** in het menu Resource van de cache is bedoeld als de primaire gekoppeld in de cache en klik vervolgens op **toevoegen cache replicatiekoppeling** van de **Geo-replicatie** blade.

    ![Koppeling toevoegen](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Klik op de naam van de gewenste secundaire cache van de **compatibel caches** lijst. Als de gewenste cache niet wordt weergegeven in de lijst, Controleer de [Geo-replicatie vereisten](#geo-replication-prerequisites) voor de gewenste secundaire cache wordt voldaan. Als u wilt filteren van de caches per regio, klikt u op de gewenste regio in de kaart om weer te geven alleen deze caches in de **compatibel caches** lijst.

    ![Geo-replicatie compatibel caches](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    U kunt ook het koppelingsproces initiëren of details weergeven over de secundaire cache met behulp van het contextmenu.

    ![Contextmenu geo-replicatie](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Klik op **koppeling** de twee caches aan elkaar koppelt en beginnen met de replicatie.

    ![Koppeling caches](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. U kunt de voortgang van het replicatieproces weergeven op de **Geo-replicatie** blade.

    ![Status koppelen](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    U kunt ook de status van de gekoppelde weergeven op de **overzicht** blade voor de primaire en secundaire caches.

    ![Status van de cache](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Zodra de replicatie voltooid is, de **status koppelen** wijzigingen in **geslaagd**.

    ![Status van de cache](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    De primaire gekoppelde cache blijft beschikbaar voor gebruik tijdens het koppelingsproces maar de secundaire gekoppelde cache is niet beschikbaar totdat het koppelingsproces is voltooid.

## <a name="remove-a-geo-replication-link"></a>Een koppeling Geo-replicatie verwijderen

1. Als u wilt verwijderen van de koppeling tussen twee caches en Geo-replicatie te stoppen, klikt u op **ontkoppelen caches** van de **Geo-replicatie** blade.
    
    ![Caches ontkoppelen](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Wanneer het ontkoppelen proces is voltooid, is de secundaire cache is beschikbaar voor zowel lees- en schrijfbewerkingen.

>[!NOTE]
>Wanneer de koppeling Geo-replicatie wordt verwijderd, blijft de gerepliceerde gegevens van de primaire gekoppelde cache in het cachegeheugen van de secundaire.
>
>

## <a name="geo-replication-faq"></a>Veelgestelde vragen over geo-replicatie

- [Kan ik met een cache van de laag Standard of Basic Geo-replicatie gebruiken?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Mijn cache is beschikbaar voor gebruik tijdens het koppelen of ontkoppelen?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [Kan ik meer dan twee caches aan elkaar koppelen?](#can-i-link-more-than-two-caches-together)
- [Kan ik twee caches van verschillende Azure-abonnementen koppelen](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Kan ik twee caches van verschillende grootten koppelen](#can-i-link-two-caches-with-different-sizes)
- [Kan ik met clustering ingeschakeld Geo-replicatie gebruiken?](#can-i-use-geo-replication-with-clustering-enabled)
- [Kan ik Geo-replicatie met mijn caches in een VNET gebruiken?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Wat is het replicatieschema voor Redis geo-replicatie?](#what-is-the-replication-schedule-for-redis-geo-replication)
- [Hoe lang duurt geo-replicatie replicatie voordat?](#how-long-does-geo-replication-replication-take)
- [Kan het herstelpunt replicatie gegarandeerd?](#is-the-replication-recovery-point-guaranteed)
- [Kan ik PowerShell of Azure CLI voor het beheren van Geo-replicatie gebruiken?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Hoeveel kost het repliceren van mijn gegevens over Azure-regio's?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [Welke regio moet ik voor mijn secundaire gekoppelde cache gebruiken?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [Hoe werkt worden niet via de gekoppelde secundaire-cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Kan ik met een cache van de laag Standard of Basic Geo-replicatie gebruiken?

Nee, Geo-replicatie is alleen beschikbaar voor Premium-laag caches.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>Mijn cache is beschikbaar voor gebruik tijdens het koppelen of ontkoppelen?

- Wanneer twee caches aan elkaar koppelen voor Geo-replicatie, de primaire gekoppelde cache blijft beschikbaar voor gebruik, maar de secundaire gekoppelde cache is niet beschikbaar totdat het koppelingsproces is voltooid.
- Wanneer u de koppeling Geo-replicatie tussen de twee caches verwijdert, blijven beide caches beschikbaar voor gebruik.

### <a name="can-i-link-more-than-two-caches-together"></a>Kan ik meer dan twee caches aan elkaar koppelen?

Nee, bij gebruik van Geo-replicatie kunt u alleen koppelen twee caches samen.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Kan ik twee caches van verschillende Azure-abonnementen koppelen

Nee, beide caches moeten zich in dezelfde Azure-abonnement.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Kan ik twee caches van verschillende grootten koppelen

Ja, als de secundaire gekoppelde cache groter dan de primaire gekoppelde cache is.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Kan ik met clustering ingeschakeld Geo-replicatie gebruiken?

Ja, mits beide caches hetzelfde aantal shards hebben.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Kan ik Geo-replicatie met mijn caches in een VNET gebruiken?

Ja, Geo-replicatie caches in vnet's worden ondersteund. 

- Geo-replicatie tussen caches in hetzelfde VNET wordt ondersteund.
- Geo-replicatie tussen caches in verschillende VNETs wordt ook ondersteund als de twee VNETs die zijn geconfigureerd in zodanig dat de resources in de VNETs kunnen bereiken elkaar via TCP-verbindingen zijn.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>Wat is het replicatieschema voor Redis geo-replicatie?

Replicatie wordt niet gevraagd op een specifiek schema, is continue en asynchrone eenledige alle schrijfbewerkingen naar de primaire gedaan worden onmiddellijk asynchroon gerepliceerd op de secundaire server.

### <a name="how-long-does-geo-replication-replication-take"></a>Hoe lang duurt geo-replicatie replicatie voordat?

Replicatie is incrementele, asynchrone en continue en is de tijd is doorgaans niet veel verschilt van de latentie tussen regio's. Onder bepaalde omstandigheden op bepaalde tijden de secundaire mogelijk moet een volledige synchronisatie van de gegevens van de primaire doen. De replicatie-tijd in dit geval is afhankelijk van factoren zoals: belasting op de primaire-cache, beschikbare bandbreedte op de machine cache inter regio latentie, enzovoort. Als u bijvoorbeeld op basis van bepaalde testen we hebben ontdekt dat tijdstip replicatie voor een volledige 53 GB geogerepliceerde koppelen in Oost ons en regio's VS-West, kunnen worden ingesteld tussen 5 tot 10 minuten.

### <a name="is-the-replication-recovery-point-guaranteed"></a>Kan het herstelpunt replicatie gegarandeerd?

Op dit moment voor caches in een modus geogerepliceerde is persistentie en importeren/exporteren uitgeschakeld. Dus voor het geval van een klant geïnitieerd failover of in gevallen waarin een replicatiekoppeling verbroken tussen het paar geo-replicatie is, de in het geheugen die de secundaire behouden, gegevens die deze heeft gesynchroniseerd vanaf de primaire tot dat moment. Er is geen garantie recovery point in dergelijke situaties.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Kan ik PowerShell of Azure CLI voor het beheren van Geo-replicatie gebruiken?

Op dit moment kunt u alleen Geo-replicatie met de Azure portal beheren.

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Hoeveel kost het repliceren van mijn gegevens over Azure-regio's?

Wanneer u Geo-replicatie gebruikt, worden gegevens uit de primaire gekoppelde cache wordt gerepliceerd naar de secundaire gekoppelde cache. Als de twee gekoppelde caches zich in dezelfde Azure-regio, zijn er geen kosten voor de gegevensoverdracht. Als de twee gekoppelde caches zich in verschillende Azure-regio's, is de kosten van Geo-replicatie gegevensoverdracht de bandbreedte kosten voor het repliceren van die gegevens naar de andere Azure-regio. Zie voor meer informatie [bandbreedte prijsinformatie](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>Waarom is de bewerking mislukt bij het verwijderen van mijn gekoppelde cache?

Wanneer twee caches zijn gekoppeld, kunt u de cache of de resourcegroep waarin ze totdat u de koppeling Geo-replicatie niet verwijderen. Als u probeert te verwijderen van de resourcegroep die een of beide van de gekoppelde caches bevat, de andere bronnen in de resourcegroep zijn verwijderd, maar de resourcegroep blijft de `deleting` systeemstatus- en alle gekoppelde caches in de resourcegroep blijven in de `running` staat. Verbreek de Geo-replicatie-koppeling voor het voltooien van de verwijdering van de resourcegroep en de gekoppelde caches binnen het zoals beschreven in [verwijderen van een Geo-replicatiekoppeling](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>Welke regio moet ik voor mijn secundaire gekoppelde cache gebruiken?

In het algemeen is het aanbevolen voor uw cache in dezelfde Azure-regio als de toepassing die toegang heeft tot deze voor te komen. Als uw toepassing een primaire en terugval regio heeft, moeten uw primaire en secundaire caches bestaan in dezelfde regio's. Zie voor meer informatie over de gekoppelde regio [Best Practices – gekoppelde Azure-regio's](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>Hoe werkt worden niet via de gekoppelde secundaire-cache?

In de eerste release van Geo-replicatie ondersteunt Azure Redis-Cache geen automatische failover tussen Azure-regio's. Geo-replicatie wordt voornamelijk gebruikt in een noodherstelscenario. In een herstelscenario distater klanten moeten online zetten van de gehele toepassing stack in een back-regio in gecoördineerde wijze in plaats van afzonderlijke toepassingsonderdelen bepalen wanneer overschakelen naar de back-ups op hun eigen laten. Dit is vooral relevant zijn voor Redis. Een van de belangrijkste voordelen van Redis is dat het is een zeer lage latentie-archief. Als Redis gebruikt door een toepassing wordt overgenomen door een andere Azure-regio, maar niet voor de compute-laag, zou toegevoegde round trip time een merkbare invloed hebben op prestaties. Om deze reden graag willen we Redis mislukt via automatisch voorkomen vanwege problemen met de tijdelijke beschikbaarheid.

Op dit moment voor het initiëren van de failover moet u de koppeling Geo-replicatie verwijderen in de Azure portal en wijzig vervolgens het eindpunt van de verbinding in de Redis-client uit de primaire gekoppelde cache in de cache voor (voorheen gekoppelde) secundaire. Wanneer de twee caches zijn ontkoppeld, wordt de replica weer een reguliere lezen / schrijven-cache en aanvragen rechtstreeks vanuit de Redis-clients accepteert.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md).

