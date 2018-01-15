---
title: Maken van uw eerste betrouwbare Azure microservice in Java | Microsoft Docs
description: Inleiding tot het maken van een Microsoft Azure Service Fabric-toepassing met staatloze en stateful services.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: e885a482edcba48c18e425c54f4acc28ee650ddd
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="get-started-with-reliable-services"></a>Aan de slag met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-quick-start.md)
> * [Java op Linux](service-fabric-reliable-services-quick-start-java.md)
>
>

Dit artikel vindt u de basisbeginselen van Azure Service Fabric Reliable Services en leidt u door het maken en implementeren van een eenvoudige betrouwbare servicetoepassing geschreven in Java. Deze video Microsoft Virtual Academy leest u hoe u een stateless betrouwbare service maken:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Installatie en configuratie
Voordat u begint, moet u de Service Fabric-ontwikkelomgeving instellen op uw computer.
Als u instellen wilt, gaat u naar [aan de slag op Mac](service-fabric-get-started-mac.md) of [aan de slag op Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Basisbegrippen
Om aan de slag met Reliable Services, moet u slechts enkele basisbeginselen begrijpen:

* **Servicetype**: dit is uw service-implementatie. Dit is gedefinieerd door de klasse u schrijven die uitgebreider is dan `StatelessService` en andere code of afhankelijkheden gebruikt, samen met een naam en een uniek versienummer.
* **Service-exemplaar met de naam**: voor het uitvoeren van uw service, u maakt benoemde exemplaren van het servicetype veel zoals u instanties van objecten van het type van een klasse maken. Service-exemplaren zijn in feite objectinstanties van uw serviceklasse die u schrijft.
* **ServiceHost**: de benoemde service-exemplaren die u maakt moeten uitvoeren in een host. De ServiceHost is alleen een proces waarbij exemplaren van de service kunnen worden uitgevoerd.
* **Service-registratie**: inschrijving brengt alles bij elkaar. Type van de service moet zijn geregistreerd met de Service Fabric-runtime in een ServiceHost om toe te staan van Service Fabric om exemplaren van deze te maken om uit te voeren.  

## <a name="create-a-stateless-service"></a>Een stateless service maken
Begint met het maken van een Service Fabric-toepassing. De Service Fabric SDK voor Linux bevat een Yeoman generator in op de steiger voor een Service Fabric-toepassing met een stateless service leveren. Start door het uitvoeren van de volgende Yeoman opdracht:

```bash
$ yo azuresfjava
```

Volg de instructies voor het maken van een **betrouwbare staatloze Service**. Voor deze zelfstudie Noem de toepassing 'HelloWorldApplication' en de service "Hallo wereld". Het resultaat bevat mappen voor de `HelloWorldApplication` en `HelloWorld`.

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
### <a name="service-registration"></a>Registratie van service
Servicetypen moeten zijn geregistreerd met de Service Fabric-runtime. Type van de service is gedefinieerd in de `ServiceManifest.xml` en uw serviceklasse die implementeert `StatelessService`. Registratie van de service wordt uitgevoerd in de Hoofdingangspunt proces. In dit voorbeeld wordt het proces Hoofdingangspunt is `HelloWorldServiceHost.java`:

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

Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Deze klasse definieert het servicetype en code kan worden uitgevoerd. De service-API biedt twee toegangspunten voor uw code:

* Een methode met een vermelding point, aangeroepen `runAsync()`, waar u kunt beginnen alle werkbelastingen, met inbegrip van langlopende compute-workloads uitvoeren.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Een communicatie-ingangspunt waar u uw communicatiestack van keuze kunt aansluiten. Dit is waar u ontvangen van aanvragen van gebruikers en andere services kan starten.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

In deze zelfstudie we richten op de `runAsync()` entry point-methode. Dit is waar u kunt onmiddellijk starten uitvoeren van uw code.

### <a name="runasync"></a>RunAsync
Het platform roept deze methode wanneer een exemplaar van een service is geplaatst en kan worden uitgevoerd. Voor een stateless service betekent dat gewoon wanneer het service-exemplaar wordt geopend. Een token annulering is opgegeven voor het coördineren van wanneer uw service-exemplaar moet worden gesloten. Deze cyclus openen en sluiten van een service-exemplaar kan vaak gedurende de levensduur van de service in Service Fabric optreden als geheel. Dit kan gebeuren om verschillende redenen, waaronder:

* Het systeem wordt uw service-exemplaren voor resourceverdeling verplaatst.
* Fouten die voorkomen in uw code.
* De toepassing of het systeem is bijgewerkt.
* De onderliggende hardware optreedt een storing.

Deze indeling wordt beheerd door Service Fabric ervoor zorgen dat uw service maximaal beschikbare en goed taakverdeling.

`runAsync()`moet niet synchroon blokkeren. De implementatie van runAsync als resultaat moet een CompletableFuture zodat de runtime om door te gaan. Als uw werkbelasting moet een langlopende taak die moet worden uitgevoerd binnen de CompletableFuture implementeren.

#### <a name="cancellation"></a>Annulering
Annulering van uw werkbelasting is een gezamenlijke inspanning gedirigeerd door het token opgegeven annulering. Het systeem wacht tot de taak te beëindigen (met succes is voltooid, annulering of fault) voordat het wordt verplaatst op. Het is belangrijk om te voldoen aan het token annulering, werk voltooien en af te sluiten `runAsync()` zo snel mogelijk wanneer het systeem annulering aanvraagt. Het volgende voorbeeld laat zien hoe een annulering gebeurtenis te verwerken:

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

In dit voorbeeld staatloze service wordt het aantal opgeslagen in een lokale variabele. Maar omdat dit een stateless service, de waarde die opgeslagen bestaat alleen voor de huidige levenscyclus van de service-exemplaar. Wanneer de service wordt verplaatst of opnieuw wordt opgestart, wordt de waarde is verloren gegaan.

## <a name="create-a-stateful-service"></a>Een stateful service maken
Service Fabric introduceert een nieuw soort stateful service. Een stateful service te onderhouden op betrouwbare wijze binnen de service zelf, dezelfde locatie bevindt als de code die wordt gebruikt. Status is maximaal beschikbaar is gemaakt door de Service Fabric zonder de noodzaak om vast te leggen van de status van een externe winkel.

Een waarde van het prestatiemeteritem van staatloze conversie naar maximaal beschikbare en permanente, zelfs wanneer de service wordt verplaatst of opnieuw wordt opgestart, moet u een stateful service.

In dezelfde map als de toepassing HelloWorld, kunt u een nieuwe service toevoegen door het uitvoeren van de `yo azuresfjava:AddService` opdracht. Kies de 'betrouwbare Stateful Service' voor uw framework en de naam van de service 'HelloWorldStateful'. 

Uw toepassing hebt nu twee services: de staatloze HelloWorld-service en de stateful service HelloWorldStateful.

Een stateful service heeft de dezelfde toegangspunten als een stateless service. Het belangrijkste verschil is de beschikbaarheid van een state-provider die de status op betrouwbare wijze kunt opslaan. Service Fabric wordt geleverd met een implementatie van de persistentieprovider status betrouwbare collecties genoemd, waarmee u de structuren gerepliceerde gegevens via een betrouwbare statusbeheer voor het maken. Een stateful betrouwbare Service maakt standaard gebruik van deze state-provider.

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
`RunAsync()`draait op dezelfde manier in stateful en staatloze services. Echter, in een stateful service het platform extra werk uitvoert namens jou voordat deze wordt uitgevoerd `RunAsync()`. Deze taak kunt opnemen ervoor te zorgen dat de betrouwbare statusbeheer en betrouwbare verzamelingen klaar zijn voor gebruik.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Betrouwbare verzamelingen en de betrouwbare status Manager
```java
ReliableHashMap<String,Long> map = this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap")
```

[ReliableHashMap](https://docs.microsoft.com/java/api/microsoft.servicefabric.data.collections._reliable_hash_map) is een dictionary-implementatie die u gebruiken kunt om op te slaan op betrouwbare wijze staat in de service. Met Service Fabric en betrouwbare Hashmaps, kunt u gegevens opslaan rechtstreeks in uw service zonder de noodzaak van een externe permanente archief. Betrouwbare Hashmaps uw gegevens maximaal beschikbaar maken. Service Fabric bewerkstelligt dit door het maken en beheren van meerdere *replica's* van uw service voor u. Het bevat ook een API die weg de complexiteit isoleert van het beheren van deze replica's en hun statusovergangen.

Betrouwbare verzamelingen kunnen elk type Java, met inbegrip van uw aangepaste typen, met een aantal aanvullende opmerkingen worden opgeslagen.

* Service Fabric worden uw status maximaal beschikbaar door *repliceren* staat tussen knooppunten en betrouwbare Hashmap worden de gegevens naar de lokale schijf opgeslagen op elke replica. Dit betekent dat alles dat is opgeslagen in betrouwbare Hashmaps moet *serialiseerbaar*. 
* Objecten worden gerepliceerd voor hoge beschikbaarheid, wanneer u transacties op betrouwbare Hashmaps doorvoert. Objecten die zijn opgeslagen in betrouwbare Hashmaps worden opgeslagen in het lokale geheugen van uw service. Dit betekent dat u een lokale verwijzing naar het object hebt.
  
   Het is belangrijk dat u geen lokale exemplaren van deze objecten muteren zonder dat u een updatebewerking op de verzameling betrouwbare uitvoert in een transactie. Dit is omdat wijzigingen in de lokale exemplaren van objecten worden niet automatisch worden gerepliceerd. U moet het object opnieuw weer in de woordenlijst invoegen of gebruik een van de *bijwerken* methoden in de woordenlijst.

Statusbeheer voor het betrouwbare beheert betrouwbare Hashmaps voor u. U kunt gewoon vragen statusbeheer voor het betrouwbare voor een betrouwbare verzameling met de naam op elk gewenst moment en op een willekeurige plaats in uw service. Statusbeheer voor het betrouwbare zorgt ervoor dat u een verwijzing terug. Wordt niet aanbevolen verwijzingen naar betrouwbare verzameling exemplaren te slaan in klasselid variabelen of eigenschappen. Speciale aandacht moet worden uitgevoerd om ervoor te zorgen dat de verwijzing naar een exemplaar te allen tijde in de levenscyclus van de service is ingesteld. Statusbeheer voor het betrouwbare dit werk voor u verwerkt en geoptimaliseerd voor herhaaldelijk bezoeken.


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

Bewerkingen op betrouwbare Hashmaps zijn asynchroon. Dit is omdat schrijfbewerkingen met betrouwbare verzamelingen i/o-bewerkingen als u wilt repliceren en de gegevens naar schijf te behouden.

Bewerkingen voor betrouwbare Hashmap zijn *transactionele*, zodat u status consistent voor meerdere betrouwbare Hashmaps en bewerkingen houden kunt. U kunt bijvoorbeeld ophalen van een werkitem van een betrouwbare woordenboek, een bewerking uitvoeren op deze en opslaan van het resultaat in anoter betrouwbare Hashmap, alle binnen een transactie. Dit wordt beschouwd als een atomic-bewerking en dit zorgt ervoor dat de hele bewerking slaagt of de hele bewerking wordt teruggedraaid. Als een fout optreedt nadat u het item in wachtrij, maar voordat u het resultaat opslaat, wordt de hele transactie wordt teruggedraaid en blijft het item in de wachtrij voor verwerking.


## <a name="run-the-application"></a>De toepassing uitvoeren

De Yeoman steigers bevat een script gradle voor het bouwen van de toepassing en bash-scripts voor het implementeren en de toepassing verwijderen. Om de toepassing uitvoert, moet u eerst de toepassing met gradle bouwen:

```bash
$ gradle
```

Dit resulteert in een Service Fabric-toepassingspakket dat kan worden geïmplementeerd met behulp van Service Fabric CLI.

### <a name="deploy-with-service-fabric-cli"></a>Met Service Fabric CLI implementeren

Het script install.sh bevat de benodigde Service Fabric CLI-opdrachten voor het implementeren van het toepassingspakket. Voer het script install.sh voor het implementeren van de toepassing.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met de Service Fabric-CLI](service-fabric-cli.md)
