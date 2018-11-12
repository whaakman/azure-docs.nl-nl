---
title: Problemen vaststellen en oplossen van Azure Cosmos DB Java asynchrone SDK | Microsoft Docs
description: Gebruik functies zoals logboekregistratie op de client en andere hulpprogramma's van derden te identificeren, problemen vaststellen en oplossen van problemen met Azure Cosmos DB.
services: cosmos-db
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.component: cosmosdb-sql
ms.openlocfilehash: 74813634aad95f163b06717521bb2c746ac3df6b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51238826"
---
# <a name="troubleshooting-issues-when-using-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Het oplossen van problemen bij het gebruik van Java-SDK voor asynchrone met Azure Cosmos DB SQL API-accounts
In dit artikel bevat informatie over veelvoorkomende problemen, oplossingen, diagnostische stappen en hulpprogramma's bij het gebruik van [Java asynchrone ADK](sql-api-sdk-async-java.md) met Azure Cosmos DB SQL API-accounts.
Java-SDK voor asynchrone biedt logische representatie van de client-side voor toegang tot Azure Cosmos DB SQL API. Dit artikel beschrijft de hulpmiddelen en benaderingen om u te helpen als u problemen ondervindt.

Beginnen met deze lijst:
    * Bekijk de [Veelvoorkomende problemen en oplossingen] sectie in dit artikel.
    * Onze SDK is [open source op github](https://github.com/Azure/azure-cosmosdb-java) en we hebben [sectie problemen](https://github.com/Azure/azure-cosmosdb-java/issues) die we controleren. Controleer of u een soortgelijk probleem met een tijdelijke oplossing al gearchiveerd vinden.
    * Beoordeling [tips voor betere prestaties](performance-tips-async-java.md) en volg de aanbevolen procedures.
    * Volg de rest van dit artikel als u een oplossing niet hebt gevonden, het bestand een [GitHub-probleem](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-workarounds"></a>Veelvoorkomende problemen en oplossingen

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Problemen met het netwerk Netty lezen time-out fout, lage doorvoer, hoge latentie

#### <a name="general-suggestions"></a>Algemene suggesties
* Zorg ervoor dat de app wordt uitgevoerd op dezelfde regio als uw Cosmos DB-account. 
* Controleer het CPU-gebruik op de host waarop de app wordt uitgevoerd. Als het CPU-gebruik is 90% of meer, houd rekening met het uitvoeren van uw app op een host met een hogere configuratie of de verdelen op meer virtuele machines.

#### <a name="connection-throttling"></a>Bandbreedtebeperking van verbinding
Beperking van de verbinding kan gebeuren vanwege een [Limiet voor verbindingen op de hostcomputer], of [Azure SNAT (PAT) poortuitputting]:

##### <a name="connection-limit-on-host"></a>Limiet voor verbindingen op de hostcomputer
Sommige Linux-systemen (zoals 'Red Hat') hebben een bovengrens voor het totale aantal geopende bestanden. Sockets in Linux worden geïmplementeerd als bestanden, zodat dit nummer het totale aantal verbindingen te beperkt.
Voer de volgende opdracht uit:

```bash
ulimit -a
```
Het aantal geopende bestanden ("nofile") moet groot genoeg is (op minste als dubbel zijn als de grootte van de verbindingsgroep). Lees meer details in [tips voor betere prestaties](performance-tips-async-java.md).

##### <a name="snat"></a>Azure SNAT (PAT) poortuitputting

Als uw app wordt geïmplementeerd op Azure VM zonder een openbaar IP-adres, standaard [Azure SNAT poorten](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) tot stand brengen van verbindingen met een willekeurig eindpunt buiten uw virtuele machine worden gebruikt. Het aantal verbindingen van de virtuele machine met de Cosmos DB-eindpunt wordt beperkt door de [Azure SNAT configuratie](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

De Azure-SNAT-poorten worden gebruikt wanneer uw Azure-VM een privé IP-adres heeft en een proces van de virtuele machine probeert te maken van een verbinding met een openbaar IP-adres. Er zijn dus twee oplossingen om te voorkomen dat Azure SNAT beperking:
    * Uw Azure Cosmos DB-service-eindpunt toevoegen aan het subnet van uw Azure VM-VNET, zoals uitgelegd in [VNET Service-eindpunt inschakelen](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). Wanneer service-eindpunt is ingeschakeld, de aanvragen niet meer worden verzonden vanuit een openbare IP-adres naar cosmos DB in plaats daarvan het VNET en subnet-id is verzonden. Deze wijziging kan leiden tot firewall val als alleen openbare IP-adressen zijn toegestaan. Als u firewall gebruikt bij het inschakelen van service-eindpunt, voegt u firewallregels met behulp van het subnet [VNET ACL's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
    * Een openbaar IP-adres toewijzen aan uw Azure-VM.

#### <a name="http-proxy"></a>HTTP-proxy

Als u een HttpProxy gebruikt, controleert u of uw HttpProxy kan het aantal verbindingen die zijn geconfigureerd in de SDK ondersteunen `ConnectionPolicy`.
U wordt geconfronteerd anders verbindingsproblemen.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Ongeldig patroon coderen: Netty i/o-thread blokkeren

De SDK gebruikt [Netty](https://netty.io/) i/o-bibliotheek voor de communicatie met Azure Cosmos DB-Service. We hebben Async-API en gebruiken we de niet-blokkerende i/o-API's van netty. De SDK i/o-werk wordt uitgevoerd in de i/o-netty threads. Het aantal i/o-netty threads is geconfigureerd om hetzelfde als het nummer van de CPU-kernen van de app-machine. De netty i/o-threads zijn alleen bedoeld om te worden gebruikt voor niet-blokkerende netty i/o-werk. De SDK retourneert het resultaat van de aanroep API op een van de netty i/o-threads van de apps-code. Als de app na de ontvangst van de resultaten in de netty thread een duurzame bewerking op de netty thread wordt uitgevoerd, kan dat resulteren in SDK hebben niet voldoende aantal i/o-threads voor het uitvoeren van de interne i/o-werk. Het coderen van deze app kan leiden tot lage doorvoer, hoge latentie en `io.netty.handler.timeout.ReadTimeoutException` fouten. De tijdelijke oplossing is om over te schakelen van de thread als u weet dat de bewerking duurt lang.

   Bijvoorbeeld, het volgende codefragment laat zien dat als u op de netty thread duurzame werk, waarbij meer dan enkele milliseconden verrichten, uiteindelijk u in een status waarbij geen netty i/o-thread aanwezig om i/o-werk te verwerken krijgt, en Hierdoor krijgt u ReadTimeou tException:
```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // time consuming work. For example:
                        // writing to a file, computationally heavy work, or just sleep
                        // basically anything which takes more than a few milliseconds
                        // doing such operation on the IO netty thread
                        // without a proper scheduler, will cause problems.
                        // The subscriber will get ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //will be io.netty.handler.timeout.ReadTimeoutException
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
   De tijdelijke oplossing is om te wijzigen van de thread waarop u de tijd nemen werk verrichten. Definieer een singleton-instantie van de planner voor uw app:
   ```java
// have a singleton instance of executor and scheduler
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Wanneer u moet om werk tijd (bijvoorbeeld rekenintensief zware werk, blokkeren van i/o), de thread overschakelen naar een werknemer die is geleverd door uw `customScheduler` met behulp van `.observeOn(customScheduler)` API.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // switches the thread.
        .subscribe(
            // ...
        );
```
Met behulp van `observeOn(customScheduler)`, u de netty i/o-thread vrij en schakel over naar uw eigen aangepaste thread geleverd door customScheduler. Deze wijziging wordt het probleem is opgelost en krijgen geen `io.netty.handler.timeout.ReadTimeoutException` meer fout.

### <a name="connection-pool-exhausted-issue"></a>Verbinding opgebruikt probleem

`PoolExhaustedException` is een fout aan de clientzijde. Als u deze fout vaak, dat is indicatie dat de werkbelasting van uw app is hoger dan wat de SDK-verbindingsgroep kan fungeren. Grootte van de verbindingsgroep verhogen of distributie van de belasting van meerdere apps kan helpen.

### <a name="request-rate-too-large"></a>Snelheid van aanvragen te groot
Hiermee wordt een server-side '-fout die aangeeft dat u de ingerichte doorvoer gebruikt en moet later opnieuw proberen. Als u deze fout vaak krijgt, kunt u overwegen om de verzamelingsdoorvoer.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Fout bij het verbinding maken met Azure Cosmos DB-emulator

Cosmos DB-emulator HTTPS-certificaat is zelfondertekend. Voor SDK, om te werken met de emulator die u moet de emulator certificaat importeren op Java TrustStore. Zoals uitgelegd [hier](local-emulator-export-ssl-certificates.md).


## <a name="enable-client-sice-logging"></a>Client SDK logboekregistratie inschakelen

Async Java-SDK gebruikt SLF4j als de gevel logboekregistratie die ondersteunt aanmelden bij de logboekregistratie van populaire frameworks zoals log4j en logback.

Bijvoorbeeld, als u log4j gebruiken als het framework voor logboekregistratie wilt, toevoegen de volgende bibliotheken in het klassepad van uw Java:

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Ook een log4j-configuratie toevoegen:
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Beoordeling [sfl4j logboekregistratie handmatig](https://www.slf4j.org/manual.html) voor meer informatie.

## <a name="netstats"></a>OS-netwerkstatistieken
Netstat opdracht uitvoeren om een idee van het aantal verbindingen zijn `Established` staat `CLOSE_WAIT` status, enzovoort.

U kunt de volgende opdracht uitvoeren op Linux:
```bash
netstat -nap
```
Het resultaat om alleen verbindingen met Cosmos DB-eindpunt te filteren.

Blijkbaar, het aantal verbindingen met Cosmos DB-eindpunt in `Established` status kan niet groter zijn dan de geconfigureerde grootte van de verbindingsgroep.

Als er zijn veel verbindingen met Cosmos DB-eindpunt in `CLOSE_WAIT` staat, voor voorbeeld van meer dan 1000 verbindingen, dat een indicatie van verbindingen tot stand gebracht en gegevenskanaal snel, die mogelijk problemen kunnen veroorzaken. Beoordeling [Veelvoorkomende problemen en oplossingen] sectie voor meer informatie.

 <!--Anchors-->
[Veelvoorkomende problemen en oplossingen]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Limiet voor verbindingen op de hostcomputer]: #connection-limit-on-host
[Azure SNAT (PAT) poortuitputting]: #snat


