---
title: Maken van uw eerste betrouwbare Azure Service Fabric-service in Java | Microsoft Docs
description: Inleiding tot het maken van een Microsoft Azure Service Fabric-toepassing met staatloze en stateful services.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: timlt
editor: ''
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 0b044b15b41e2d74f08c4bc989e22b6a19949445
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56170921"
---
# <a name="get-started-with-reliable-services"></a>Aan de slag met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-quick-start.md)
> * [Java op Linux](service-fabric-reliable-services-quick-start-java.md)
>
>

In dit artikel vindt u de basisbeginselen van Azure Service Fabric Reliable Services en leidt u door het maken en implementeren van een eenvoudige betrouwbare servicetoepassing die zijn geschreven in Java. 

## <a name="installation-and-setup"></a>Installeren en instellen
Voordat u begint, zorg ervoor dat u de Service Fabric-ontwikkelomgeving instellen op uw computer hebt.
Als u instellen wilt, gaat u naar [aan de slag op Mac](service-fabric-get-started-mac.md) of [aan de slag met Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Basisbegrippen
Als u wilt aan de slag met Reliable Services, moet u slechts enkele fundamentele concepten begrijpen:

* **Servicetype**: Dit is uw service-implementatie. Deze is gedefinieerd door de klasse u schrijven die een uitbreiding `StatelessService` en een andere code of afhankelijkheden gebruikt, samen met een naam en een uniek versienummer.
* **Service-exemplaar met de naam**: Voor het uitvoeren van uw service, u benoemde exemplaren van het servicetype, veel, zoals het maken van instanties van objecten van een klassentype. Service-exemplaren zijn in feite objectinstanties van uw serviceklasse die u schrijft.
* **ServiceHost**: De benoemde service-instanties dat u maakt moeten uitvoeren in een host. De ServiceHost is alleen een proces waarbij instanties van uw service kunnen worden uitgevoerd.
* **Service-registratie**: Inschrijving brengt alles bij elkaar. Type van de service moet worden geregistreerd bij de Service Fabric-runtime in een ServiceHost om toe te staan van Service Fabric om instanties van deze te maken om uit te voeren.  

## <a name="create-a-stateless-service"></a>Een stateless service maken
Beginnen met het maken van een Service Fabric-toepassing. De Service Fabric-SDK voor Linux bevat een Yeoman generator voor de opbouw van een Service Fabric-toepassing met een stateless service. Start door het uitvoeren van de volgende Yeoman opdracht:

```bash
$ yo azuresfjava
```

Volg de instructies voor het maken van een **betrouwbare Stateless Service**. Voor deze zelfstudie, noem de toepassing "HelloWorldApplication" en "Hallowereld" van de service. Het resultaat bevat mappen voor de `HelloWorldApplication` en `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```
### <a name="service-registration"></a>De service is geregistreerd
Servicetypen moeten worden geregistreerd bij de Service Fabric-runtime. Type van de service is gedefinieerd in de `ServiceManifest.xml` en uw serviceklasse die `StatelessService`. Registratie van de service wordt uitgevoerd in het proces belangrijkste toegangspunt. In dit voorbeeld wordt het proces belangrijkste toegangspunt is `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="implement-the-service"></a>De service implementeren

Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Deze klasse definieert het servicetype en code kunt uitvoeren. De service-API biedt twee toegangspunten voor uw code:

* Een methode met een vermelding punt, met de naam `runAsync()`, waar u kunt beginnen alle workloads, met inbegrip van langlopende compute-workloads uitvoeren.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Een communicatie-toegangspunt waar u in uw communicatiestack naar keuze kunt aansluiten. Dit is waar u ontvangen van aanvragen van gebruikers en andere services kan starten.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Deze zelfstudie richt zich op de `runAsync()` vermelding punt methode. Dit is waar u kunt direct beginnen waarop uw code wordt uitgevoerd.

### <a name="runasync"></a>RunAsync
Het platform wordt deze methode wanneer een exemplaar van een service is geplaatst en gereed is om uit te voeren. Voor een stateless service betekent dat wanneer de service-exemplaar wordt geopend. Een token annulering is opgegeven voor de coördinatie van wanneer uw service-exemplaar moet worden gesloten. Deze cyclus openen en sluiten van een service-exemplaar kan vaak gedurende de levensduur van de service in Service Fabric optreden als geheel. Dit kan gebeuren om verschillende redenen, waaronder:

* Het systeem wordt uw service-instanties voor bron-balancing verplaatst.
* Fouten optreden in uw code.
* De toepassing of het systeem is bijgewerkt.
* De onderliggende hardware er een storing optreedt.

Deze indeling wordt beheerd door Service Fabric houdt u uw service maximaal beschikbare en goed met gelijke taakverdeling.

`runAsync()` moet niet synchroon blokkeren. De implementatie van runAsync moet retourneren een CompletableFuture zodat de runtime om door te gaan. Als uw werkbelasting moet een langlopende taak die moet worden uitgevoerd binnen de CompletableFuture wilt implementeren.

#### <a name="cancellation"></a>Annulering
De annulering van uw werkbelasting is een gezamenlijke inspanning ingedeeld door de opgegeven annulering-token. Het systeem wacht tot de taak te beëindigen (met succes is voltooid, annulering of fout) voordat deze op. Het is belangrijk om te voldoen aan het token annulering, werk voltooien en sluit `runAsync()` zo snel mogelijk wanneer het systeem annulering aanvraagt. Het volgende voorbeeld ziet u hoe u een annulering-gebeurtenis verwerken:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

    // TODO: Replace the following sample code with your own logic
    // or remove this runAsync override if it's not needed in your service.

    return CompletableFuture.runAsync(() -> {
        long iterations = 0;
        while(true)
        {
        cancellationToken.throwIfCancellationRequested();
        logger.log(Level.INFO, "Working-{0}", ++iterations);

        try {
            Thread.sleep(1000);
        } catch (InterruptedException ex){}
        }
    });
}
```

In dit voorbeeld stateless service, wordt het aantal opgeslagen in een lokale variabele. Maar omdat dit een staatloze service is, de waarde die opgeslagen bestaat alleen voor de huidige levenscyclus van de service-exemplaar. Wanneer de service worden verplaatst of opnieuw wordt opgestart, wordt de waarde is verloren gegaan.

## <a name="create-a-stateful-service"></a>Een stateful service maken
Service Fabric introduceert een nieuw soort stateful service. Een stateful service kunt onderhouden van de status op betrouwbare wijze binnen de service zelf, CO-locatie met de code die wordt gebruikt. De status is maximaal beschikbaar gemaakt door de Service Fabric zonder de noodzaak om vast te leggen de status van een externe opslag.

Als u wilt omzetten in een waarde van het prestatiemeteritem van staatloze hoge beschikbaarheid en permanente, zelfs wanneer de service worden verplaatst of opnieuw wordt opgestart, moet u een stateful service.

In dezelfde map als de HelloWorld-toepassing, kunt u een nieuwe service toevoegen door het uitvoeren van de `yo azuresfjava:AddService` opdracht. Kies de 'betrouwbare Stateful Service' voor uw framework en noem de service 'HelloWorldStateful'. 

Uw toepassing heeft nu twee services: de stateless service HelloWorld en de stateful service HelloWorldStateful.

Een stateful service heeft de dezelfde toegangspunten als een stateless service. Het belangrijkste verschil is de beschikbaarheid van een state-provider die de status op betrouwbare wijze kunt opslaan. Service Fabric wordt geleverd door een implementatie voor een status-provider met de naam betrouwbare verzamelingen, waarmee u structuren van gerepliceerde gegevens via de betrouwbare status Manager maken. Een stateful betrouwbare Service maakt gebruik van deze provider staat standaard.

Open HelloWorldStateful.java in **HelloWorldStateful src ->**, die de volgende RunAsync-methode bevat:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    Transaction tx = stateManager.createTransaction();
    return this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap").thenCompose((map) -> {
        return map.computeAsync(tx, "counter", (k, v) -> {
            if (v == null)
                return 1L;
            else
                return ++v;
            }, Duration.ofSeconds(4), cancellationToken)
                .thenCompose((r) -> tx.commitAsync())
                .whenComplete((r, e) -> {
            try {
                tx.close();
            } catch (Exception e) {
                logger.log(Level.SEVERE, e.getMessage());
            }
        });
    });
}
```

### <a name="runasync"></a>RunAsync
`RunAsync()` werkt op dezelfde manier in stateful en stateless services. Echter, in een stateful service het platform extra werk uitvoert namens voordat deze wordt uitgevoerd `RunAsync()`. Deze taak kunt opnemen ervoor zorgen dat de Manager van betrouwbare status en de betrouwbare verzamelingen klaar zijn voor gebruik.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Betrouwbare verzamelingen en de betrouwbare status Manager
```java
ReliableHashMap<String,Long> map = this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap")
```

[ReliableHashMap](https://docs.microsoft.com/java/api/microsoft.servicefabric.data.collections.reliablehashmap) is een dictionary-implementatie die u gebruiken kunt om op te slaan op een betrouwbare manier staat in de service. Met Service Fabric en betrouwbare HashMaps, kunt u gegevens rechtstreeks in uw service zonder de noodzaak van een externe permanente archief opslaan. Betrouwbare HashMaps uw gegevens maximaal beschikbaar maken. Service Fabric doet dit door het maken en beheren van meerdere *replica's* van uw service voor u. Het biedt ook een API die de complexiteit van het beheer van deze replica's en hun statusovergangen volledig weggewerkt.

Betrouwbare verzamelingen kunnen elk type zijn Java, met inbegrip van uw aangepaste typen, met enkele aanvullende opmerkingen opslaan:

* Service Fabric zorgt de status voor hoge beschikbaarheid *repliceren* status over knooppunten en betrouwbare HashMap slaat uw gegevens naar de lokale schijf op elke replica. Dit betekent dat alles die zijn opgeslagen in betrouwbare HashMaps moet worden *serialiseerbare*. 
* Wanneer u transacties op betrouwbare HashMaps objecten gerepliceerd voor hoge beschikbaarheid. Objecten die zijn opgeslagen in betrouwbare HashMaps worden opgeslagen in lokaal geheugen in uw service. Dit betekent dat u een lokale verwijzing naar het object hebt.
  
   Het is belangrijk dat u geen lokale exemplaren van deze objecten muteren zonder dat er een updatebewerking op de betrouwbare verzameling in een transactie. Dit is omdat wijzigingen aan lokale exemplaren van objecten wordt niet automatisch worden gerepliceerd. U moet het object opnieuw terug in de woordenlijst of gebruik een van de *bijwerken* methoden in de woordenlijst.

De betrouwbare status Manager beheert betrouwbare HashMaps voor u. U kunt vragen de betrouwbare status Manager voor een betrouwbare verzameling met de naam op elk gewenst moment en op een willekeurige plaats in uw service. De betrouwbare status Manager zorgt ervoor dat u een verwijzing terug worden opgehaald. Wordt niet aanbevolen dat u verwijzingen naar instanties van betrouwbare verzameling in Klassenlidvariabelen of eigenschappen opslaat. Speciale aandacht moet worden uitgevoerd om ervoor te zorgen dat de verwijzing is ingesteld op een exemplaar te allen tijde in de levenscyclus van de service. De betrouwbare status Manager verwerkt dit werk voor u en geoptimaliseerd voor herhalingen bezoeken.


### <a name="transactional-and-asynchronous-operations"></a>Transactionele en asynchrone bewerkingen
```java
return map.computeAsync(tx, "counter", (k, v) -> {
    if (v == null)
        return 1L;
    else
        return ++v;
    }, Duration.ofSeconds(4), cancellationToken)
        .thenCompose((r) -> tx.commitAsync())
        .whenComplete((r, e) -> {
    try {
        tx.close();
    } catch (Exception e) {
        logger.log(Level.SEVERE, e.getMessage());
    }
});
```

Bewerkingen op betrouwbare HashMaps zijn asynchroon. Dit is omdat schrijfbewerkingen met betrouwbare verzamelingen i/o-bewerkingen om te repliceren en behoud van gegevens naar schijf uitvoeren.

Betrouwbare HashMap bewerkingen zijn *transactionele*, zodat u status consistent voor meerdere betrouwbare HashMaps en bewerkingen houden kunt. Bijvoorbeeld, kan u een werkitem ophalen uit een betrouwbare Dictionary, het uitvoeren van een bewerking op en het resultaat opslaan in een andere betrouwbare HashMap, allemaal binnen één transactie. Dit wordt behandeld als een atomische bewerking, en dit zorgt ervoor dat de gehele bewerking slaagt of de gehele bewerking wordt teruggedraaid. Als een fout optreedt nadat u het item in wachtrij, maar voordat u het resultaat opslaat, wordt de hele transactie wordt teruggedraaid en blijft het item in de wachtrij voor verwerking.


## <a name="build-the-application"></a>De toepassing bouwen

De Yeoman opbouw bevat een gradle-script voor het maken van de toepassing en bash-scripts om te implementeren en verwijderen van de toepassing. Als u wilt de toepassing uitvoert, moet u eerst de toepassing met gradle bouwen:

```bash
$ gradle
```

Dit resulteert in een Service Fabric-toepassingspakket dat kan worden geïmplementeerd met behulp van Service Fabric-CLI.

## <a name="deploy-the-application"></a>De toepassing implementeren

Nadat de toepassing is gemaakt, kunt u deze implementeren in het lokale cluster.

1. Maak verbinding met het lokale cluster van Service Fabric.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Voer het installatiescript uit dat is opgegeven in de sjabloon om het toepassingspakket te kopiëren naar de installatiekopieopslag van het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

    ```bash
    ./install.sh
    ```

De implementatie van de gemaakte toepassing werkt hetzelfde als van andere Service Fabric-toepassingen. Zie de documentatie over het [beheren van een Service Fabric-toepassing met de Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md) voor gedetailleerde instructies.

Parameters voor deze opdrachten vindt u in de gegenereerde manifesten binnen het toepassingspakket.

Nadat de toepassing is geïmplementeerd, opent u een browser en gaat u naar [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) op [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Vouw vervolgens het knooppunt **Toepassingen** uit. U ziet dat er nu een vermelding is voor uw toepassingstype en nog een voor het eerste exemplaar van dat type.

> [!IMPORTANT]
> Voor het implementeren van de toepassing met een beveiligd Linux-cluster in Azure, moet u een certificaat voor het valideren van uw toepassing met de Service Fabric-runtime configureren. In dat geval kunt uw Reliable Services-services om te communiceren met de onderliggende Service Fabric-runtime-API's. Zie voor meer informatie, [een Reliable Services-app uit te voeren op Linux-clusters configureren](./service-fabric-configure-certificates-linux.md#configure-a-reliable-services-app-to-run-on-linux-clusters).  
>

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met de Service Fabric-CLI](service-fabric-cli.md)
