---
title: Service voor externe toegang in Azure Service Fabric | Microsoft Docs
description: Service Fabric remoting kan clients en -services te communiceren met services met behulp van een remote procedure call.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 51a9c8bd628ef9e65d04a3a4ddbdc127d84d4b54
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="service-remoting-with-reliable-services"></a>Service voor externe toegang met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java op Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Het framework Reliable Services biedt een mechanisme voor externe toegang voor het snel en eenvoudig instellen van externe procedureaanroep voor services.

## <a name="set-up-remoting-on-a-service"></a>Instellen van externe toegang voor een service
Instellen van externe toegang tot een service wordt uitgevoerd in de twee eenvoudige stappen:

1. Maak een interface voor uw service te implementeren. Deze interface definieert de methoden die beschikbaar voor een externe procedureaanroep voor uw service zijn. De methoden moeten geretourneerd asynchrone methoden. De interface moet implementeren `microsoft.serviceFabric.services.remoting.Service` om aan te geven dat de service een externe communicatie-interface heeft.
2. Gebruik een listener voor externe toegang in uw service. Dit is een `CommunicationListener` -implementatie mogelijkheden voor externe toegang biedt. `FabricTransportServiceRemotingListener`kan worden gebruikt voor het maken van de listener voor een externe toegang met behulp van het standaardprotocol voor het transport van externe toegang.

De volgende staatloze service wordt bijvoorbeeld één methode voor 'Hallo wereld' via een externe procedureaanroep.

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
> De argumenten en de retourtypen in de interface van de service mag geen enkelvoudig, complexe of aangepaste typen, maar ze moeten worden geserialiseerd.
>
>

## <a name="call-remote-service-methods"></a>Externe servicemethoden aanroepen
Bij het aanroepen van methoden met een service met behulp van de stack voor externe toegang wordt uitgevoerd met behulp van een lokale proxy voor de service via de `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klasse. De `ServiceProxyBase` methode maakt u een lokale proxyserver met behulp van dezelfde interface die de service implementeert. Met de proxyserver, kunt u gewoon methoden aanroepen op de interface op afstand.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Het framework remoting uitzonderingen op de service naar de client wordt doorgegeven. Dus uitzonderingsverwerking logica op de client met behulp van `ServiceProxyBase` uitzonderingen die de service genereert rechtstreeks kan verwerken.

## <a name="service-proxy-lifetime"></a>Levensduur voor service-Proxy
Het maken van ServiceProxy is een lichtgewicht bewerking, zodat gebruikers zo veel naar behoefte kunt maken. Proxy-service kan opnieuw worden gebruikt zolang de gebruiker nodig. Gebruiker kan dezelfde proxy in geval van een uitzondering opnieuw gebruiken. Elke ServiceProxy bevat communicatie-client gebruikt om berichten te verzenden via de kabel. Tijdens het aanroepen van API, hebben we interne controleert u of de client wordt communicatie gebruikt geldig is. Op basis van die resulteren, maken wordt de client communicatie opnieuw. Gebruiker hoeft dus niet opnieuw maken van serviceproxy in geval van een uitzondering.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory levensduur
[FabricServiceProxyFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) is een factory die de proxy voor externe communicatie van andere interfaces maakt. Als u API gebruikt `ServiceProxyBase.create` voor het maken van proxy framework maakt vervolgens een `FabricServiceProxyFactory`.
Is het nuttig om een handmatig maken als u wilt onderdrukken [ServiceRemotingClientFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) eigenschappen.
Factory is een dure bewerking. `FabricServiceProxyFactory`houdt de cache van clients voor communicatie.
Aanbevolen procedure is aan het cachegeheugen `FabricServiceProxyFactory` voor zo lang mogelijk.

## <a name="remoting-exception-handling"></a>Afhandeling van uitzonderingen voor externe toegang
De externe uitzondering veroorzaakt door de API-service, worden als RuntimeException of FabricException terug naar de client verzonden.

ServiceProxy wordt verwerkt alle failover-uitzondering voor de service-partitie dat deze wordt gemaakt. Deze opnieuw de eindpunten opgelost als er Failover Exceptions(Non-Transient Exceptions) en probeert om opnieuw de aanroep met het juiste eindpunt. Aantal nieuwe pogingen voor failover-uitzondering is oneindig.
In geval van een TransientExceptions, het alleen opnieuw probeert de aanroep.

Standaard opnieuw parameters worden opgegeven door [OperationRetrySettings]. (https://docs.microsoft.com/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Gebruiker kan deze waarden door OperationRetrySettings object doorgeven aan de constructor ServiceProxyFactory configureren.

## <a name="next-steps"></a>Volgende stappen
* [Beveiligen van communicatie voor Reliable Services](service-fabric-reliable-services-secure-communication.md)
