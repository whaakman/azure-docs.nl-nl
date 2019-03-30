---
title: Service voor externe toegang met behulp van Java in Azure Service Fabric | Microsoft Docs
description: Externe communicatie met Service Fabric kan clients en services om te communiceren met Java-services met behulp van een externe procedureaanroep.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: chackdan
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 51c8c689bd3fe3e8967bab77e776ad02f9cb59f1
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667963"
---
# <a name="service-remoting-in-java-with-reliable-services"></a>Service voor externe toegang in Java met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java op Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Voor services die niet zijn gekoppeld aan een bepaalde communicatieprotocol of stack, zoals WebAPI, Windows Communication Foundation (WCF) of andere resources, biedt het framework van Reliable Services een mechanisme voor externe toegang snel en eenvoudig instellen van externe procedureaanroepen voor Services.  In dit artikel wordt beschreven hoe het instellen van externe procedureaanroepen voor services die zijn geschreven met behulp van Java.

## <a name="set-up-remoting-on-a-service"></a>Instellen van een service voor externe toegang
Instellen van externe toegang tot een service wordt uitgevoerd in twee eenvoudige stappen:

1. Maak een interface voor uw service te implementeren. Deze interface definieert de methoden die beschikbaar voor een externe procedureaanroep op uw service zijn. De methoden retourneren van de taak moeten zijn asynchrone methoden. De interface moet implementeren `microsoft.serviceFabric.services.remoting.Service` om aan te geven dat de service een externe communicatie-interface heeft.
2. Gebruik een listener voor externe toegang in uw service. Dit is een `CommunicationListener` mogelijkheden voor externe toegang biedt-implementatie. `FabricTransportServiceRemotingListener` kan worden gebruikt om te maken van een externe listener met behulp van het standaardprotocol voor het transport van externe toegang.

Bijvoorbeeld, een één methode voor het "Hallo wereld" ophalen via een externe procedureaanroep wordt aangegeven dat de volgende stateless service.

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> De argumenten en de typen retourneren die in de service-interface kunnen een eenvoudige, complexe of aangepaste typen zijn, maar ze moeten worden geserialiseerd.
>
>

## <a name="call-remote-service-methods"></a>Externe servicemethoden aanroepen
Methoden voor het aanroepen van een service met behulp van de stack voor externe toegang wordt uitgevoerd met behulp van een lokale proxyserver naar de service via de `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klasse. De `ServiceProxyBase` methode maakt u een lokale proxyserver met behulp van dezelfde interface die de service implementeert. Met deze proxy, kunt u gewoon methoden aanroepen op de interface op afstand.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Het framework voor externe toegang geeft uitzonderingen op de service naar de client. Dus uitzonderingsverwerking logica op de client met behulp van `ServiceProxyBase` uitzonderingen die de service genereert kunnen rechtstreeks worden verwerkt.

## <a name="service-proxy-lifetime"></a>De levensduur van de service-Proxy
Het maken van ServiceProxy is een lichte bewerking, zodat u zoveel behoefte kunt maken. Proxy-service-exemplaren kunnen worden hergebruikt, zolang ze wel nodig zijn. Als een externe procedureaanroep is een uitzondering genereert, kunt u nog steeds hetzelfde exemplaar van de proxy opnieuw gebruiken. Elke ServiceProxy bevat een communicatie-client gebruikt voor het verzenden van berichten via de kabel. Tijdens het aanroepen van externe oproepen, interne controles uitgevoerd om te bepalen of de client communicatie geldig is. Op basis van de resultaten van deze controles, de client communicatie wordt opnieuw gemaakt indien nodig. Dus als er een uitzondering optreedt, u hoeft niet opnieuw maken `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory Lifetime
[FabricServiceProxyFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client.fabricserviceproxyfactory) is een factory die proxy voor externe communicatie van andere interfaces worden gemaakt. Als u API `ServiceProxyBase.create` voor het maken van proxy, framework maakt vervolgens een `FabricServiceProxyFactory`.
Het is handig om een handmatig maken wanneer u nodig hebt voor de onderdrukking [ServiceRemotingClientFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client.serviceremotingclientfactory) eigenschappen.
Factory is een dure bewerking. `FabricServiceProxyFactory` behoudt de cache van clients voor communicatie.
Best practice is om de cache `FabricServiceProxyFactory` zo lang mogelijk.

## <a name="remoting-exception-handling"></a>Afhandeling van uitzonderingen voor externe toegang
De externe uitzondering gegenereerd door de service-API, worden als RuntimeException of FabricException terug naar de client verzonden.

ServiceProxy kan overweg met alle failover-uitzondering voor de servicepartitie deze wordt gemaakt. Dit opnieuw de eindpunten oplost als er Failover Exceptions(Non-Transient Exceptions) en probeert om opnieuw de aanroep met het juiste eindpunt. Aantal nieuwe pogingen voor failover-uitzondering is onbepaalde tijd.
In het geval van TransientExceptions, het alleen opnieuw probeert de aanroep.

Standaardparameters voor nieuwe pogingen worden opgegeven door [OperationRetrySettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.communication.client.operationretrysettings).
U kunt deze waarden configureren doordat OperationRetrySettings object doorgegeven aan de constructor ServiceProxyFactory.

## <a name="next-steps"></a>Volgende stappen
* [Beveiliging van de communicatie voor Reliable Services](service-fabric-reliable-services-secure-communication-java.md)
