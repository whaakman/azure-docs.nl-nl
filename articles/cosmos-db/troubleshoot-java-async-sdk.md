---
title: Problemen vaststellen en oplossen van Azure Cosmos DB Java asynchrone SDK
description: Gebruik functies zoals client-side-logboekregistratie en andere hulpprogramma's van derden om te identificeren, onderzoeken en oplossen van problemen met Azure Cosmos DB.
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 0a2bbb33182fcdef3cc6ed7ff213557f90be4544
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57880039"
---
# <a name="troubleshoot-issues-when-you-use-the-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Oplossen van problemen wanneer u de Async Java SDK met Azure Cosmos DB SQL API-accounts gebruiken
In dit artikel bevat informatie over veelvoorkomende problemen, oplossingen, diagnostische stappen en hulpprogramma's wanneer u de [Async Java-SDK](sql-api-sdk-async-java.md) met Azure Cosmos DB SQL API-accounts.
De Async Java SDK biedt logische representatie van de client-side voor toegang tot de Azure Cosmos DB SQL API. Dit artikel beschrijft de hulpmiddelen en benaderingen om u te helpen als u problemen ondervindt.

Beginnen met deze lijst:

* Bekijk de [Veelvoorkomende problemen en oplossingen] sectie in dit artikel.
* Kijken naar de SDK, die beschikbaar is [open source op GitHub](https://github.com/Azure/azure-cosmosdb-java). Er is een [sectie problemen](https://github.com/Azure/azure-cosmosdb-java/issues) die actief worden bewaakt. Controleer als er al een soortgelijk probleem met een tijdelijke oplossing is ingediend.
* Controleer de [tips voor betere prestaties](performance-tips-async-java.md), en volg de aanbevolen procedures.
* Lees de rest van dit artikel als u geen oplossing gevonden. Vervolgens het bestand een [GitHub-probleem](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-workarounds"></a>Veelvoorkomende problemen en oplossingen

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Problemen met het netwerk Netty lezen time-out fout, lage doorvoer, hoge latentie

#### <a name="general-suggestions"></a>Algemene suggesties
* Zorg ervoor dat de app wordt uitgevoerd op dezelfde regio als uw Azure Cosmos DB-account. 
* Controleer het CPU-gebruik op de host waarop de app wordt uitgevoerd. Als het CPU-gebruik is 90 procent of meer, kunt u uw app uitvoeren op een host met een hogere configuratie. Of u de belasting van meer virtuele machines kunt distribueren.

#### <a name="connection-throttling"></a>Bandbreedtebeperking van verbinding
Vanwege een beperking van de verbinding vindt een [Limiet voor verbindingen op een hostcomputer] of [Azure SNAT (PAT) poortuitputting].

##### <a name="connection-limit-on-host"></a>Limiet voor verbindingen op een hostcomputer
Sommige Linux-systemen, zoals Red Hat, hebben een bovengrens voor het totale aantal geopende bestanden. Sockets in Linux worden geïmplementeerd als bestanden, zodat dit nummer het totale aantal verbindingen te beperkt.
Voer de volgende opdracht uit.

```bash
ulimit -a
```
Het nummer van de maximale toegestane geopende bestanden, die worden aangegeven als 'nofile', moet ten minste tweemaal de grootte van de verbindingsgroep. Zie voor meer informatie, [tips voor betere prestaties](performance-tips-async-java.md).

##### <a name="snat"></a>Azure SNAT (PAT) poortuitputting

Als uw app op Azure Virtual Machines geïmplementeerd zonder een openbaar IP-adres, standaard [Azure SNAT poorten](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) verbindingen instellen met een willekeurig eindpunt buiten uw virtuele machine. Het aantal verbindingen van de virtuele machine met de Azure Cosmos DB-eindpunt wordt beperkt door de [Azure SNAT configuratie](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Azure SNAT poorten worden gebruikt alleen wanneer uw virtuele machine een privé IP-adres heeft en een proces van de virtuele machine probeert verbinding maken met een openbaar IP-adres. Er zijn twee oplossingen om te voorkomen dat Azure SNAT beperking:

* Uw Azure Cosmos DB-service-eindpunt toevoegen aan het subnet van het virtuele netwerk van Azure Virtual Machines. Zie voor meer informatie, [Azure Virtual Network service-eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    Wanneer het service-eindpunt is ingeschakeld, worden de aanvragen niet meer vanaf een openbaar IP-adres verzonden met Azure Cosmos DB. In plaats daarvan worden het virtuele netwerk en subnet-id verzonden. Deze wijziging kan leiden tot firewall val als alleen openbare IP-adressen zijn toegestaan. Als u een firewall gebruikt wanneer u het service-eindpunt inschakelt, voegt u een subnet toe aan de firewall met behulp van [virtueel netwerk-ACL's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Een openbaar IP-adres toewijzen aan uw Azure-VM.

#### <a name="http-proxy"></a>HTTP-proxy

Als u een HTTP-proxy gebruikt, zorg ervoor dat het aantal verbindingen die zijn geconfigureerd in de SDK kan ondersteunen `ConnectionPolicy`.
U wordt geconfronteerd anders verbindingsproblemen.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Ongeldige codering patroon: Netty i/o-thread blokkeren

De SDK gebruikt de [Netty](https://netty.io/) i/o-bibliotheek om te communiceren met Azure Cosmos DB. De SDK heeft asynchrone APIs en maakt gebruik van niet-blokkerende i/o-API's van Netty. De SDK i/o-werk wordt uitgevoerd in Netty i/o-threads. Het aantal threads voor i/o-Netty is geconfigureerd om hetzelfde als het aantal CPU-kernen van de app-machine. 

De Netty i/o-threads zijn bedoeld om te worden gebruikt alleen voor niet-blokkerende Netty i/o-werk. De SDK retourneert het resultaat van de aanroep API op een van de Netty i/o-threads van de app-code. Als de app wordt een langdurige bewerking uitgevoerd nadat deze resultaten op de Netty thread ontvangt, is de SDK mogelijk niet voldoende i/o-threads om de interne i/o-werk te doen. Het coderen van deze app kan leiden tot lage doorvoer, hoge latentie en `io.netty.handler.timeout.ReadTimeoutException` fouten. De tijdelijke oplossing is om over te schakelen van de thread als u weet dat de bewerking duurt.

Bijvoorbeeld: Bekijk het volgende codefragment. U kunt langdurige werk die het duurt maximaal enkele milliseconden op de Netty thread uitvoeren. Als dit het geval is, krijgt uiteindelijk u in een status waarin geen Netty i/o-thread is aanwezig om i/o-werk te verwerken. Als gevolg hiervan, treedt er een fout ReadTimeoutException.
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
                        // Time-consuming work is, for example,
                        // writing to a file, computationally heavy work, or just sleep.
                        // Basically, it's anything that takes more than a few milliseconds.
                        // Doing such operations on the IO Netty thread
                        // without a proper scheduler will cause problems.
                        // The subscriber will get a ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //It will be io.netty.handler.timeout.ReadTimeoutException.
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
   De tijdelijke oplossing is om te wijzigen van de thread waarop u werk die het kost tijd uitvoert. Definieer een singleton-instantie van de planner voor uw app.
   ```java
// Have a singleton instance of an executor and a scheduler.
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Mogelijk moet u werken die duurt time, bijvoorbeeld rekenintensief zware werk- of blokkerende i/o. In dit geval de thread overschakelen naar een werknemer die is geleverd door uw `customScheduler` met behulp van de `.observeOn(customScheduler)` API.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // Switches the thread.
        .subscribe(
            // ...
        );
```
Met behulp van `observeOn(customScheduler)`, u de thread Netty i/o-release en schakel over naar uw eigen aangepaste thread die is opgegeven door de aangepaste scheduler. Deze wijziging is het probleem opgelost. Ontvangen geen een `io.netty.handler.timeout.ReadTimeoutException` meer fout.

### <a name="connection-pool-exhausted-issue"></a>Verbinding opgebruikt probleem

`PoolExhaustedException` is een fout aan de clientzijde. Deze fout geeft aan dat de werkbelasting van uw app hoger is dan wat de verbindingsgroep SDK kan fungeren. De grootte van de verbindingsgroep vergroten of de verdelen over meerdere apps.

### <a name="request-rate-too-large"></a>Snelheid van aanvragen te groot
Hiermee wordt een server-side '-fout. Hiermee wordt aangegeven dat u uw ingerichte doorvoer verbruikt. Probeer het later opnieuw. Als u deze fout vaak krijgt, kunt u overwegen een toename in de verzamelingsdoorvoer.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Fout bij het verbinding maken met Azure Cosmos DB-emulator

Het Azure Cosmos DB-emulator HTTPS-certificaat is zelfondertekend. Voor de SDK om te werken met de emulator, door de emulator-certificaat te importeren naar een Java-TrustStore. Zie voor meer informatie, [exporteren van Azure Cosmos DB-emulatorcertificaten](local-emulator-export-ssl-certificates.md).

### <a name="dependency-conflict-issues"></a>Problemen met het Conflict van afhankelijkheid

```console
Exception in thread "main" java.lang.NoSuchMethodError: rx.Observable.toSingle()Lrx/Single;
```

De bovenstaande uitzondering stelt dat u een afhankelijkheid op een oudere versie van RxJava lib (bijvoorbeeld 1.2.2) hebt. Onze SDK is gebaseerd op RxJava 1.3.8 met API's die niet beschikbaar in eerdere versie van RxJava. 

De tijdelijke oplossing voor dergelijke issuses is om te bepalen welke andere afhankelijkheden brengt in RxJava 1.2.2 en uitsluiten van de transitieve afhankelijkheid van RxJava 1.2.2 en toestaan CosmosDB SDK doen om de nieuwere versie.

Om te identificeren welke bibliotheek waarmee u de volgende opdracht uitvoeren naast uw pom.xml-bestand van het project RxJava-1.2.2:
```bash
mvn dependency:tree
```
Zie voor meer informatie de [maven-afhankelijkheid structuur handleiding](https://maven.apache.org/plugins/maven-dependency-plugin/examples/resolving-conflicts-using-the-dependency-tree.html).

Wanneer u RxJava 1.2.2 geïdentificeerd is transitieve afhankelijkheid van welke andere afhankelijkheden van uw project, kunt u de afhankelijkheid wijzigen op die lib in het pom-bestand en uitsluiten RxJava transitieve afhankelijkheid het:

```xml
<dependency>
  <groupId>${groupid-of-lib-which-brings-in-rxjava1.2.2}</groupId>
  <artifactId>${artifactId-of-lib-which-brings-in-rxjava1.2.2}</artifactId>
  <version>${version-of-lib-which-brings-in-rxjava1.2.2}</version>
  <exclusions>
    <exclusion>
      <groupId>io.reactivex</groupId>
      <artifactId>rxjava</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

Zie voor meer informatie de [uitsluiten transitieve afhankelijkheid handleiding](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html).


## <a name="enable-client-sice-logging"></a>Client SDK logboekregistratie inschakelen

De SDK voor Java-asynchrone gebruikt SLF4j als de gevel logboekregistratie die ondersteunt aanmelden bij de logboekregistratie van populaire frameworks zoals log4j en logback.

Bijvoorbeeld, als u log4j gebruiken als het framework voor logboekregistratie wilt, toevoegen de volgende bibliotheken in het klassepad van uw Java.

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

Ook een log4j-configuratie toevoegen.
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

Zie voor meer informatie de [sfl4j logboekregistratie handmatig](https://www.slf4j.org/manual.html).

## <a name="netstats"></a>OS-netwerkstatistieken
Voer de opdracht ' netstat uit naar een idee van het aantal verbindingen in Staten zoals zijn `ESTABLISHED` en `CLOSE_WAIT`.

U kunt de volgende opdracht uitvoeren op Linux.
```bash
netstat -nap
```
Het resultaat om alleen verbindingen met de Azure Cosmos DB-eindpunt te filteren.

Het aantal verbindingen met het Azure Cosmos DB-eindpunt in de `ESTABLISHED` status kan niet groter zijn dan de geconfigureerde grootte van de verbindingsgroep.

Aantal verbindingen met het Azure Cosmos DB-eindpunt wordt mogelijk de `CLOSE_WAIT` staat. Er zijn meer dan 1000. Een getal dat hoge geeft aan dat verbindingen tot stand gebracht en snel gegevenskanaal. Deze situatie wordt mogelijk veroorzaakt problemen. Zie voor meer informatie de [Veelvoorkomende problemen en oplossingen] sectie.

 <!--Anchors-->
[Veelvoorkomende problemen en oplossingen]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Limiet voor verbindingen op een hostcomputer]: #connection-limit-on-host
[Azure SNAT (PAT) poortuitputting]: #snat


