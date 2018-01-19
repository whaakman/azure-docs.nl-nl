---
title: Het beheren van Azure Redis-Cache | Microsoft Docs
description: Meer informatie over het uitvoeren van beheertaken, zoals het opnieuw opstarten en schema-updates voor Azure Redis-Cache
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 8c915ae6-5322-4046-9938-8f7832403000
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 07/05/2017
ms.author: wesmc
ms.openlocfilehash: 37e7395a26ead737009ad9e285e9f88372b25d26
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-administer-azure-redis-cache"></a>Het beheren van Azure Redis-Cache
Dit onderwerp beschrijft hoe uitvoeren zoals beheertaken [opnieuw wordt opgestart nadat](#reboot) en [updates plannen](#schedule-updates) voor uw Azure Redis-Cache-exemplaren.

## <a name="reboot"></a>Opnieuw opstarten
De **opnieuw opstarten** blade kunt u een of meer knooppunten van de cache opnieuw opstarten. Deze mogelijkheid opnieuw opstarten, kunt u de toepassing te testen voor tolerantie als er een storing van een cacheknooppunt.

![Opnieuw opstarten](./media/cache-administration/redis-cache-administration-reboot.png)

Selecteer de knooppunten op te starten en klik op **opnieuw opstarten**.

![Opnieuw opstarten](./media/cache-administration/redis-cache-reboot.png)

Als u een premium-cache hebt met clusteren is ingeschakeld, kunt u selecteren welke shards van de cache op te starten.

![Opnieuw opstarten](./media/cache-administration/redis-cache-reboot-cluster.png)

Opnieuw opstarten van een of meer knooppunten van de cache, selecteer de gewenste knooppunten en klikt u op **opnieuw opstarten**. Als u een premium-cache hebt met clusteren is ingeschakeld, selecteert u de gewenste shards op te starten en klik vervolgens op **opnieuw opstarten**. Na een paar minuten weer de geselecteerde knooppunten opnieuw worden opgestart en er wordt on line later een paar minuten.

De gevolgen voor de clienttoepassingen varieert, afhankelijk van welke knooppunten opnieuw te starten.

* **Master** : wanneer het hoofdknooppunt opnieuw opgestart, Azure Redis-Cache mislukt via het knooppunt van de replica en bijdraagt aan het model. Tijdens deze failover mogelijk zijn er een korte periode waarin de verbindingen naar de cache kunnen mislukken.
* **Slave** : wanneer het knooppunt slave opnieuw wordt opgestart, er zijn doorgaans geen gevolgen voor cache-clients.
* **Hoofd- en slave** -wanneer zowel cache-knooppunten worden opgestart, alle gegevens gaan verloren in de cache en verbindingen met de cache mislukken totdat het primaire knooppunt weer online wordt gezet. Als u hebt geconfigureerd [gegevenspersistentie](cache-how-to-premium-persistence.md), de meest recente back-up wordt hersteld als de cache weer online wordt gezet, maar er cache-schrijfbewerkingen die na de meest recente back-up gaan verloren.
* **Knooppunten van een premium in de cache met clustering ingeschakeld** : wanneer u een of meer knooppunten van een premium-cache opnieuw opstarten met clustering is ingeschakeld, het gedrag voor de geselecteerde knooppunten is hetzelfde als wanneer u het bijbehorende knooppunt of knooppunten van een niet-geclusterde cache opnieuw.

> [!IMPORTANT]
> Opnieuw opstarten is nu beschikbaar voor alle Prijscategorieën.
> 
> 

## <a name="reboot-faq"></a>Veelgestelde vragen over het opnieuw opstarten
* [Welk knooppunt moet ik opnieuw worden opgestart voor de toepassing testen?](#which-node-should-i-reboot-to-test-my-application)
* [Kan ik de cache om te wissen clientverbindingen opnieuw opstarten?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Ik verliest gegevens uit de cache als een herstart moet ik doen?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [Kan ik mijn-cache met behulp van PowerShell, CLI of andere beheerhulpprogramma's opnieuw opstarten?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [Welke prijzen lagen, kunt de functionaliteit voor opnieuw opstarten?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-to-test-my-application"></a>Welk knooppunt moet ik opnieuw worden opgestart voor de toepassing testen?
Als u wilt de tolerantie van uw toepassing tegen fouten van het primaire knooppunt van de cache testen, start opnieuw op de **Master** knooppunt. Als u wilt testen de tolerantie van uw toepassing tegen fouten van het secundaire knooppunt, opnieuw opstarten de **Slave** knooppunt. Als u wilt de tolerantie van uw toepassing tegen beschadiging van de cache testen, opnieuw opstarten **beide** knooppunten.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Kan ik de cache om te wissen clientverbindingen opnieuw opstarten?
Ja, als u opnieuw opstarten van de cache die alle clientverbindingen zijn uitgeschakeld. Opnieuw opstarten is handig in gevallen waar alle clientverbindingen vanwege een fout of een fout in de clienttoepassing worden gebruikt. Elke prijscategorie heeft verschillende [client verbindingslimieten](cache-configure.md#default-redis-server-configuration) voor de verschillende grootten en eens deze limieten zijn bereikt, niet meer clientverbindingen worden geaccepteerd. Opnieuw opstarten van de cache biedt een manier om te wissen van alle clientverbindingen.

> [!IMPORTANT]
> Als u uw cache om te wissen clientverbindingen opnieuw opstart, wordt StackExchange.Redis automatisch hersteld zodra de Redis-knooppunt weer online is. Als het onderliggende probleem niet is opgelost, blijven de verbindingen van clients worden gebruikt.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Ik verliest gegevens uit de cache als een herstart moet ik doen?
Als u beide opnieuw opstarten de **Master** en **Slave** knooppunten, de gegevens in de cache (of in die shard als u een premium-cache met clusteren is ingeschakeld) worden verwijderd. Als u hebt geconfigureerd [gegevenspersistentie](cache-how-to-premium-persistence.md), de meest recente back-up wordt hersteld als de cache weer online wordt gezet, maar er cache-schrijfbewerkingen die hebben plaatsgevonden na het uitvoeren van de back-up gaan verloren.

Als u opnieuw slechts een van de knooppunten opstarten, gegevens zijn niet doorgaans verloren, maar nog steeds mogelijk. Bijvoorbeeld als het hoofdknooppunt opnieuw wordt opgestart en een schrijfbewerking cache wordt uitgevoerd, de gegevens uit de cache voor schrijven is verloren gegaan. Een ander scenario voor gegevensverlies zou zijn als u één knooppunt opnieuw opstarten en het andere knooppunt omlaag gaan vanwege een fout op hetzelfde moment gebeurt. Zie voor meer informatie over mogelijke oorzaken van gegevensverlies [waar vind ik mijn gegevens in Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Kan ik mijn-cache met behulp van PowerShell, CLI of andere beheerhulpprogramma's opnieuw opstarten?
Ja, voor PowerShell instructies raadpleegt u [opnieuw opstarten van een Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Welke prijzen lagen, kunt de functionaliteit voor opnieuw opstarten?
Opnieuw opstarten is beschikbaar voor alle Prijscategorieën.

## <a name="schedule-updates"></a>Updates plannen
De **updates plannen** blade kunt u een onderhoudsvenster voor uw cache van de laag Premium aanwijzen. Wanneer het onderhoudsvenster wordt opgegeven, wordt een Redis-server bijgewerkt tijdens dit venster. 

> [!NOTE] 
> Het onderhoudsvenster is alleen van toepassing op Redis-serverupdates, en niet naar een Azure-updates of updates voor het besturingssysteem van de virtuele machines die als host fungeren van de cache.
> 
> 

![Updates plannen](./media/cache-administration/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleren de gewenste dagen het Beginuur van het venster Onderhoud voor elke dag opgeeft, en klik **OK**. Houd er rekening mee dat de duur van het onderhoudsvenster is ingesteld op UTC. 

> [!NOTE]
> Het standaardonderhoudsvenster voor updates is vijf uur. Deze waarde kan niet worden geconfigureerd via de Azure portal, maar u kunt deze configureren in met behulp van PowerShell de `MaintenanceWindow` parameter van de [nieuw AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry) cmdlet. Zie voor meer informatie [kan ik geplande updates met behulp van PowerShell, CLI of andere beheerhulpprogramma's beheren?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)
> 
> 

## <a name="schedule-updates-faq"></a>Veelgestelde vragen over updates plannen
* [Wanneer updates worden uitgevoerd als ik de functie voor schema-updates niet gebruiken?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Welk type updates zijn aangebracht tijdens het geplande onderhoudsvenster?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [Kan ik geplande updates met behulp van PowerShell, CLI of andere beheerhulpprogramma's beheren?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Welke prijzen lagen, kan de functie voor schema-updates gebruiken?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Wanneer updates worden uitgevoerd als ik de functie voor schema-updates niet gebruiken?
Als u een onderhoudsvenster niet opgeeft, kunnen updates op elk gewenst moment worden gemaakt.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Welk type updates zijn aangebracht tijdens het geplande onderhoudsvenster?
Alleen Redis-server updates zijn aangebracht tijdens het geplande onderhoudsvenster. Het onderhoudsvenster is niet van toepassing op Azure-updates of updates voor het besturingssysteem van de VM.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Kan ik beheerde geplande updates met behulp van PowerShell, CLI of andere beheerhulpprogramma's?
Ja, kunt u uw geplande updates met de volgende PowerShell-cmdlets beheren:

* [Get-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/get-azurermrediscachepatchschedule)
* [New-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/new-azurermrediscachepatchschedule)
* [New-AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry)
* [Remove-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/remove-azurermrediscachepatchschedule)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Welke prijzen lagen, kan de functie voor schema-updates gebruiken?
De **updates plannen** functie is alleen beschikbaar in de prijscategorie premium.

## <a name="next-steps"></a>Volgende stappen
* Ontdek meer [premium-laag van Azure Redis-Cache](cache-premium-tier-intro.md) functies.

