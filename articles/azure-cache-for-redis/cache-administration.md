---
title: Azure Cache beheren voor Redis | Microsoft Docs
description: Meer informatie over het uitvoeren van beheertaken, zoals opnieuw opstarten en schema-updates voor Azure Cache voor Redis
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 8c915ae6-5322-4046-9938-8f7832403000
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 07/05/2017
ms.author: yegu
ms.openlocfilehash: b75a2faa2030fc343cbabb17eb37b63c9ea34f70
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56232378"
---
# <a name="how-to-administer-azure-cache-for-redis"></a>Azure Cache beheren voor Redis
In dit onderwerp wordt beschreven hoe u uitvoeren van beheertaken zoals [opnieuw wordt opgestart](#reboot) en [updates plannen](#schedule-updates) voor uw Azure-Cache voor instanties van Redis.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reboot"></a>Opnieuw opstarten
De **opnieuw opstarten** blade kunt u een of meer knooppunten van uw cache opnieuw opstarten. Deze mogelijkheid opnieuw opstarten, kunt u uw testtoepassing voor tolerantie als er een storing van een cacheknooppunt.

![Opnieuw opstarten](./media/cache-administration/redis-cache-administration-reboot.png)

Selecteer de knooppunten op te starten en klik op **opnieuw opstarten**.

![Opnieuw opstarten](./media/cache-administration/redis-cache-reboot.png)

Als u een premium-cache hebt met clustering is ingeschakeld, kunt u selecteren welke shards van de cache op te starten.

![Opnieuw opstarten](./media/cache-administration/redis-cache-reboot-cluster.png)

Een of meer knooppunten van uw cache opnieuw opstarten, selecteer de gewenste knooppunten en klikt u op **opnieuw opstarten**. Als u een premium-cache hebt met clustering is ingeschakeld, selecteert u de gewenste shards op te starten en klik vervolgens op **opnieuw opstarten**. Na een paar minuten weer de geselecteerde knooppunten opnieuw opstarten, en zijn online een paar minuten later opnieuw.

De impact op clienttoepassingen varieert, afhankelijk van welke knooppunten dat u opnieuw op.

* **Master** : wanneer het hoofdknooppunt opnieuw wordt opgestart, Azure voor Redis-Cache wordt overgenomen door de replica-knooppunt en het schrijven van het model. Tijdens deze failover, is er mogelijk een korte periode waarin verbindingen met de cache kunnen mislukken.
* **Ondergeschikte** : wanneer de slave-knooppunt opnieuw wordt opgestart, is doorgaans niet van invloed op de cache-clients.
* **Hoofd- en slave** -wanneer zowel cache-knooppunten opnieuw worden opgestart, alle gegevens gaan verloren in de cache en verbindingen met de cache mislukken totdat het primaire knooppunt weer online komt. Als u hebt geconfigureerd [gegevenspersistentie](cache-how-to-premium-persistence.md), de meest recente back-up wordt hersteld als de cache weer online komt, maar een cache-schrijfbewerkingen die zijn opgetreden na de meest recente back-up gaan verloren.
* **Knooppunten van een premium-cache met clustering is ingeschakeld** : wanneer u een of meer knooppunten van een premium-cache opnieuw opstarten met clustering is ingeschakeld, het gedrag voor de geselecteerde knooppunten is hetzelfde als wanneer u de bijbehorende knooppunt of de knooppunten van een niet-geclusterde cache opnieuw.

> [!IMPORTANT]
> Opnieuw opstarten is nu beschikbaar voor alle Prijscategorieën.
> 
> 

## <a name="reboot-faq"></a>Veelgestelde vragen over het opnieuw opstarten
* [Welk knooppunt moet ik opnieuw opstarten om mijn toepassing te testen?](#which-node-should-i-reboot-to-test-my-application)
* [Kan ik de cache om te wissen van clientverbindingen opnieuw opstarten?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Ik gaan gegevens verloren uit de cache als ik een opnieuw opstarten?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [Kan ik mijn cache met behulp van PowerShell, CLI of andere beheerprogramma's opnieuw opstarten?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [De prijzen van lagen, kan de functionaliteit voor opnieuw opstarten gebruiken?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-to-test-my-application"></a>Welk knooppunt moet ik opnieuw opstarten om mijn toepassing te testen?
Als u wilt testen van de tolerantie van uw toepassing tegen fouten van het primaire knooppunt van uw cache, opnieuw opstarten de **Master** knooppunt. Als u wilt de tolerantie van uw toepassing tegen fouten van het secundaire knooppunt hebt getest, start opnieuw op de **Slave** knooppunt. Als u wilt testen van de flexibiliteit van uw toepassing meer veiligheid tegen beschadiging van de cache, opnieuw opstarten **beide** knooppunten.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Kan ik de cache om te wissen van clientverbindingen opnieuw opstarten?
Ja, als u opnieuw opstarten om de cache die alle clientverbindingen zijn uitgeschakeld. Opnieuw opstarten kan handig zijn in het geval waarbij alle clientverbindingen vanwege een logische fout of een bug in de clienttoepassing worden gebruikt. Elke prijscategorie heeft verschillende [client limieten](cache-configure.md#default-redis-server-configuration) voor de verschillende grootten en eenmaal deze limieten zijn bereikt, niet meer clientverbindingen worden geaccepteerd. De cache opnieuw wordt opgestart, biedt een manier om te wissen van alle clientverbindingen.

> [!IMPORTANT]
> Als u uw cache om te wissen van clientverbindingen opnieuw opstarten, wordt StackExchange.Redis automatisch hersteld wanneer de Redis-knooppunt weer online is. Als het onderliggende probleem niet is opgelost, blijven de verbindingen van clients worden verbruikt.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Ik gaan gegevens verloren uit de cache als ik een opnieuw opstarten?
Als u opnieuw opstarten om zowel de **Master** en **Slave** knooppunten, alle gegevens in de cache (of shard als u een premium-cache met clustering is ingeschakeld) is verbroken. Als u hebt geconfigureerd [gegevenspersistentie](cache-how-to-premium-persistence.md), de meest recente back-up wordt hersteld wanneer de cache weer online komt, maar een cache-schrijfbewerkingen die hebben plaatsgevonden na de back-up is gemaakt, gaan verloren.

Als u slechts een van de knooppunten opnieuw opstarten, gegevens zijn niet doorgaans verloren, maar nog steeds kan zijn. Bijvoorbeeld als het hoofdknooppunt opnieuw wordt opgestart en een cache-Schrijf wordt uitgevoerd, de gegevens uit de cache schrijven is verloren gegaan. Een ander scenario voor preventie van gegevensverlies zou zijn als u een knooppunt opnieuw opstarten en het andere knooppunt gebeurt vanwege een fout op hetzelfde moment gaan. Zie voor meer informatie over mogelijke oorzaken voor verlies van gegevens, [wat is er gebeurd met mijn gegevens in Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Kan ik mijn cache met behulp van PowerShell, CLI of andere beheerprogramma's opnieuw opstarten?
Ja, voor PowerShell instructies Zie [opnieuw op te starten van een Azure-Cache voor Redis](cache-howto-manage-redis-cache-powershell.md#to-reboot-an-azure-cache-for-redis).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>De prijzen van lagen, kan de functionaliteit voor opnieuw opstarten gebruiken?
Opnieuw opstarten is beschikbaar voor alle Prijscategorieën.

## <a name="schedule-updates"></a>Updates plannen
De **updates plannen** blade kunt u om een onderhoudsvenster voor uw Premium-laag-cache toe te wijzen. Wanneer het onderhoudsvenster is opgegeven, worden eventuele updates van Redis-server gemaakt tijdens dit venster. 

> [!NOTE] 
> Het onderhoudsvenster is alleen van toepassing op server-updates, Redis en niet voor alle Azure-updates of updates voor het besturingssysteem van de virtuele machines die als host de cache fungeren.
> 
> 

![Updates plannen](./media/cache-administration/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleert u de gewenste dagen en geef de begintijd van onderhoud-venster voor elke dag en op **OK**. Houd er rekening mee dat de duur van het onderhoudsvenster is ingesteld op UTC. 

De standaard- en ten minste onderhoudsvenster voor updates is vijf uur. Deze waarde kan niet worden geconfigureerd via de Azure-portal, maar u kunt deze configureren in PowerShell met behulp van de `MaintenanceWindow` parameter van de [New-AzRedisCacheScheduleEntry](/powershell/module/az.rediscache/new-azrediscachescheduleentry) cmdlet. Zie voor meer informatie, [kan ik de geplande updates met behulp van PowerShell, CLI of andere beheerprogramma's beheren?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)


## <a name="schedule-updates-faq"></a>Veelgestelde vragen over updates plannen
* [Wanneer treden er updates op als ik de functie voor schema-updates niet gebruiken?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Welk type updates zijn gemaakt tijdens het geplande onderhoudsvenster?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [Kan ik geplande updates met behulp van PowerShell, CLI of andere beheerprogramma's beheren?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [De prijzen van lagen, kan de schema-updates-functionaliteit gebruiken?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Wanneer treden er updates op als ik de functie voor schema-updates niet gebruiken?
Als u een onderhoudsvenster niet opgeeft, kunnen updates op elk gewenst moment worden gemaakt.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Welk type updates zijn gemaakt tijdens het geplande onderhoudsvenster?
Alleen Redis-server updates worden uitgevoerd tijdens het geplande onderhoudsvenster. Het onderhoudsvenster is niet van toepassing op Azure-updates of updates van het besturingssysteem van de virtuele machine.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Kan ik beheerde geplande updates met behulp van PowerShell, CLI of andere beheerprogramma's?
Ja, kunt u uw geplande updates met de volgende PowerShell-cmdlets beheren:

* [Get-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/get-azrediscachepatchschedule)
* [New-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/new-azrediscachepatchschedule)
* [New-AzRedisCacheScheduleEntry](/powershell/module/az.rediscache/new-azrediscachescheduleentry)
* [Remove-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/remove-azrediscachepatchschedule)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>De prijzen van lagen, kan de schema-updates-functionaliteit gebruiken?
De **updates plannen** functie is alleen beschikbaar in de prijscategorie premium.

## <a name="next-steps"></a>Volgende stappen
* Meer verkennen [Azure Cache voor de laag premium Redis](cache-premium-tier-intro.md) functies.

