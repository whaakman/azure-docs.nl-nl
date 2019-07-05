---
title: Aanbevolen procedures voor Azure voor Redis-Cache
description: Leer hoe u uw Azure-Cache voor Redis effectief gebruiken door deze aanbevolen procedures te volgen.
services: cache
documentationcenter: na
author: joncole
manager: jhubbard
editor: tysonn
ms.assetid: 3e4905e3-89e3-47f7-8cfb-12caf1c6e50e
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 06/21/2019
ms.author: joncole
ms.openlocfilehash: bdc75033e0aa2e401a511789728feef3248d46ad
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452460"
---
# <a name="best-practices-for-azure-cache-for-redis"></a>Aanbevolen procedures voor Azure voor Redis-Cache 
Door deze aanbevolen procedures te volgen, kunt u helpen de prestaties en het rendabele gebruik van uw Azure-Cache voor Redis-exemplaar te maximaliseren.

## <a name="configuration-and-concepts"></a>Configuratie en concepten
 * **Standard of Premium-laag gebruiken voor productiesystemen.**  De laag Basic is een systeem met één knooppunt met geen replicatie van gegevens en er wordt geen SLA. Ook ten minste een C1-cache gebruiken.  C0 caches zijn bedoeld voor scenario's eenvoudig ontwikkelen en testen omdat ze een gedeelde CPU-kern, weinig geheugen hebben en gevoelig zijn voor ' luidruchtige buren zijn '.

 * **Houd er rekening mee dat Redis een in-memory-gegevens-opslag is.**  [In dit artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) geeft een overzicht van enkele scenario's waarbij gegevens verloren gaan kunnen.

 * **Ontwikkelen van uw systeem, zodat ze verbinding blips kan verwerken** [vanwege het toepassen van patches en failover](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

 * **Configureer uw [maxmemory-gereserveerde instelling](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor het verbeteren van de reactiesnelheid van het systeem** onder druk te verlichten geheugenruimte.  Deze instelling is vooral belangrijk voor schrijfintensief workloads of als u hogere waarden (100 KB of meer) in Redis opslaat.  Ik zou kunt het beste beginnen met 10% van de grootte van uw cache en vervolgens hebt u schrijfintensief belastingen verhogen. Zie [enkele overwegingen](cache-how-to-troubleshoot.md#considerations-for-memory-reservations) bij het selecteren van een waarde.

 * **Werkt beste redis met lagere waarden**, dus houd rekening met hakken grotere gegevens in meerdere sleutels.  In [deze Redis-discussie](https://stackoverflow.com/questions/55517224/what-is-the-ideal-value-size-range-for-redis-is-100kb-too-large/), enkele overwegingen die u moet zorgvuldig worden vermeld.  Lezen [in dit artikel](cache-how-to-troubleshoot.md#large-requestresponse-size) voor een voorbeeld-probleem dat kan worden veroorzaakt door grote waarden.

 * **Ga naar uw cache-exemplaar en uw toepassing in dezelfde regio.**  Verbinding maken met een cache in een andere regio kan aanzienlijk hogere latentie en betrouwbaarheid te verminderen.  Hoewel u verbinding van buiten Azure maken kunt, dit niet aanbevolen *met name bij gebruik van Redis als een cache*.  Als u Redis als een sleutel/waarde-archief gebruikt, is latentie mogelijk niet het belangrijkste aandachtspunt. 

 * **Verbindingen opnieuw** -het maken van nieuwe verbindingen is duur en verhoogt de latentie, dus zo veel mogelijk verbindingen opnieuw te gebruiken. Als u ervoor kiest om nieuwe verbindingen te maken, moet u de oude om verbindingen te sluiten voordat u deze release (zelfs in beheerde geheugen talen zoals .NET of Java).

 * **Configureren van uw clientbibliotheek te gebruiken een *time-out voor verbinding* van ten minste 15 seconden**, de systeemtijd om verbinding te maken zodat zelfs onder hogere CPU-voorwaarden.  Een kleine verbinding time-outwaarde is geen garantie dat de verbinding tot stand is gebracht in die periode wordt voltooid.  Als er iets verkeerd uitvalt (hoog client CPU, hoge server CPU, enzovoort) en vervolgens een korte verbinding time-outwaarde zorgt ervoor dat de verbindingspoging is mislukt. Hierdoor is vaak een onjuiste situatie nog erger.  In plaats van helpen, verergeren kortere time-outs voor het probleem door af te dwingen van het systeem opnieuw opstarten van het proces van probeert opnieuw verbinding maken, wat tot leiden kan een *verbinding mislukt -> Probeer het opnieuw ->* lus. Het algemeen wordt aangeraden dat u de verbinding time-out op 15 seconden of hoger laat. Het is beter om uw poging om verbinding te voltooien na 15 of 20 seconden dan om dit snel mislukken, kunnen alleen om opnieuw te proberen. Dergelijke een lus voor opnieuw proberen kan leiden tot de storing tot meer dan als u het systeem slechts laten nemen meer in eerste instantie laatste.  
     > [!NOTE]
     > Dit artikel is specifiek voor de *verbindingspoging* en niet gerelateerd zijn aan het moment dat u bereid bent te wachten op een *bewerking* , zoals GET of instellen om te voltooien.
 

 * **Voorkomen van uitgebreide opdrachten** -sommige redis-bewerkingen, zoals de [opdracht sleutels](https://redis.io/commands/keys), zijn *zeer* duur en moeten worden vermeden.  Zie voor meer informatie, [enkele overwegingen om uitgebreide opdrachten](cache-how-to-troubleshoot.md#expensive-commands)


 
## <a name="memory-management"></a>Geheugenbeheer van
Er zijn verschillende dingen met betrekking tot geheugengebruik in uw Redis-server-exemplaar dat u misschien wilt gebruiken.  Hier volgen enkele:

 * **Kies een [verwijderingsbeleid](https://redis.io/topics/lru-cache) dat werkt voor uw toepassing.**  Het standaardbeleid voor Azure Redis is *vluchtige lru*, wat betekent dat alleen de sleutels die een TTL-waarde hebt ingesteld waarde komen in aanmerking voor verwijdering.  Als er geen sleutels een TTL-waarde hebt, wordt niet elke sleutel verwijderen door het systeem.  Als u wilt dat het systeem om toe te staan een willekeurige toets om te worden verwijderd als onder geheugendruk, dan kunt u overwegen de *allkeys lru* beleid.

 * **De waarde voor een verlopen instellen op uw sleutels.**  Hiermee verwijdert u sleutels proactief in plaats van te wachten totdat de geheugenbelasting.  Wanneer verwijdering geactiveerd vanwege geheugendruk, kan dit leiden tot extra belasting op uw server.  Zie voor meer informatie de documentatie voor de [verloopt](https://redis.io/commands/expire) en [ExpireAt](https://redis.io/commands/expireat) opdrachten.
 
## <a name="client-library-specific-guidance"></a>Servicespecifieke richtlijnen voor client-bibliotheek
 * [StackExchange.Redis (.NET)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-stackexchange-redis-md)
 * [Java - client die moet ik gebruiken?](https://gist.github.com/warrenzhu25/1beb02a09b6afd41dff2c27c53918ce7#file-azure-redis-java-best-practices-md)
 * [Sla (Java)](https://gist.github.com/warrenzhu25/181ccac7fa70411f7eb72aff23aa8a6a#file-azure-redis-lettuce-best-practices-md)
 * [Jedis (Java)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-java-jedis-md)
 * [Node.js](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-node-js-md)
 * [PHP](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-php-md)
 * [Asp.Net Session State Provider](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-session-state-provider-md)


## <a name="when-is-it-safe-to-retry"></a>Wanneer is het veilig om opnieuw te proberen?
Er is geen eenvoudig antwoord.  Elke toepassing nodig heeft om te bepalen welke bewerkingen kunnen opnieuw worden uitgevoerd en die niet kan.  Elke bewerking heeft andere vereisten en afhankelijkheden tussen sleutel.  Hier volgen enkele dingen die u kunt overwegen:

 * Hoewel Redis is uitgevoerd de opdracht die wordt u gevraagd deze wordt uitgevoerd, kunt u client-side '-fouten krijgen.  Bijvoorbeeld:
     - Time-outs zijn een client-side-concept.  Als de bewerking heeft de server bereikt, kan de server de opdracht wordt uitgevoerd, zelfs als de client wachten geeft.  
     - Wanneer er een fout optreedt op de socketverbinding, is het niet mogelijk om te weten als de bewerking daadwerkelijk is uitgevoerd op de server.  De verbindingsfout kan bijvoorbeeld gebeuren nadat de aanvraag wordt verwerkt door de server, maar voordat het antwoord wordt ontvangen door de client.
 *  Hoe mijn toepassing reageren als ik per ongeluk twee keer dezelfde bewerking uitvoeren?  Bijvoorbeeld, wat gebeurt er als ik een geheel getal tweemaal in plaats van slechts één keer verhogen?  Mijn toepassing schrijft naar dezelfde sleutel vanaf meerdere locaties?  Wat gebeurt er als mijn logica voor opnieuw proberen overschrijft een waarde ingesteld op sommige andere deel van mijn app?

Als u wilt testen hoe uw code werkt onder foutvoorwaarden, kunt u overwegen de [functie opnieuw opstarten](cache-administration.md#reboot). Hiermee kunt u zien hoe verbinding blips van invloed zijn op uw toepassing.

## <a name="performance-testing"></a>Prestaties testen
 * **Start met behulp van `redis-benchmark.exe`**  gecontroleerd op mogelijke doorvoer/latentie een idee krijgen voordat u uw eigen perf schrijft.  Documentatie voor redis-benchmark [hier](http://redis.io/topics/benchmarks).  Houd er rekening mee dat redis-benchmark geen ondersteuning voor SSL, zodat u [de niet-SSL-poort via de Portal in te schakelen](cache-configure.md#access-ports) voordat u de test uitvoert.  [Een compatibel windows-versie van redis-benchmark.exe vindt u hier](https://github.com/MSOpenTech/redis/releases)
 * De VM die wordt gebruikt voor het testen van de client moet **in dezelfde regio** als uw Redis-cache-exemplaar.
 * **We raden aan met behulp van de Dv2-serie VM's** voor de client als ze beschikken over betere hardware en de beste resultaten krijgt.
 * Zorg ervoor dat de client die u gebruikt de virtuele machine is **ten minste net zoveel reken- en bandbreedte* als de cache worden getest. 
 * **Inschakelen van VRSS** op de clientcomputer als u van Windows gebruikmaakt.  [Hier ziet voor meer informatie](https://technet.microsoft.com/library/dn383582(v=ws.11).aspx).  Voorbeeld powershell-script:
     >PowerShell - ExecutionPolicy Unrestricted inschakelen-NetAdapterRSS-naam (Get-NetAdapter). De naam 
     
 * **Overweeg het gebruik van instanties van Redis voor Premium-laag**.  Deze cache-grootten hebben betere netwerklatentie en doorvoer omdat deze nu worden uitgevoerd op betere hardware voor zowel CPU en het netwerk.
 
     > [!NOTE]
     > De prestatieresultaten van de waargenomen zijn [hier gepubliceerd](cache-faq.md#azure-cache-for-redis-performance) ter referentie.   Let op dat SSL/TLS enige overhead toegevoegd zodat u mogelijk verschillende latenties en/of doorvoer als u transportversleuteling.
 
### <a name="redis-benchmark-examples"></a>Voorbeelden van redis-Benchmark
**Pretest setup**: Hiermee wordt het cache-exemplaar voorbereiden met gegevens die nodig zijn voor de latentie en doorvoer testen van de onderstaande opdrachten.
> redis-benchmark.exe - h yourcache.redis.cache.windows.net - yourAccesskey -t instellen -n 10 - d 1024 

**Voor het testen van latentie**: Deze opdracht test de GET-aanvragen met behulp van een 1 k-nettolading.
> redis-benchmark.exe - h yourcache.redis.cache.windows.net - yourAccesskey -t ophalen -d 1024 - P 50 -c 4

**Doorvoer testen:** Dit maakt gebruik van Pipelined GET-aanvragen met de nettolading van 1 kB.
> redis-benchmark.exe - h yourcache.redis.cache.windows.net - yourAccesskey -t ophalen -n 1000000 - d 1024 - P 50 - c 50
