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
ms.openlocfilehash: 59b58e9d9bdb044c81261fd19338c3f95bd409b3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
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

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
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
