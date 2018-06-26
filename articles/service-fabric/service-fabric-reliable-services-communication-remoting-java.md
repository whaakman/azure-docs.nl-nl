---
title: Service voor externe toegang met behulp van Java in Azure Service Fabric | Microsoft Docs
description: Service Fabric remoting kan clients en -services te communiceren met Java-services met behulp van een remote procedure call.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 3215ee4adf907524626b4919b637ce23b9e0e782
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750177"
---
# <a name="service-remoting-in-java-with-reliable-services"></a>Service voor externe toegang in Java met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java op Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Voor services die niet zijn gekoppeld aan een bepaalde communicatieprotocol of stack, zoals WebAPI, Windows Communication Foundation (WCF) of anderen, biedt het framework Reliable Services een mechanisme voor externe toegang voor het snel en eenvoudig instellen van externe procedureaanroepen weer dat voor Services.  Dit artikel wordt beschreven hoe u externe procedureaanroepen weer dat voor services die zijn geschreven met behulp van Java instelt.

## <a name="set-up-remoting-on-a-service"></a>Instellen van externe toegang voor een service
Instellen van externe toegang tot een service wordt uitgevoerd in de twee eenvoudige stappen:

1. Maak een interface voor uw service te implementeren. Deze interface definieert de methoden die beschikbaar voor een externe procedureaanroep voor uw service zijn. De methoden moeten geretourneerd asynchrone methoden. De interface moet implementeren `microsoft.serviceFabric.services.remoting.Service` om aan te geven dat de service een externe communicatie-interface heeft.
2. Gebruik een listener voor externe toegang in uw service. Dit is een `CommunicationListener` -implementatie mogelijkheden voor externe toegang biedt. `FabricTransportServiceRemotingListener` kan worden gebruikt voor het maken van de listener voor een externe toegang met behulp van het standaardprotocol voor het transport van externe toegang.

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
Het maken van ServiceProxy is een lichtgewicht bewerking, zodat u zoveel naar wens kunt maken. Proxy-service-exemplaren kunnen opnieuw worden gebruikt zolang ze nodig zijn. U kunt een remote procedure call genereert een uitzondering, nog steeds hetzelfde exemplaar van de proxy te hergebruiken. Elke ServiceProxy bevat een communicatie-client gebruikt om berichten te verzenden via de kabel. Tijdens het aanroepen van externe oproepen, worden interne controles uitgevoerd om te bepalen of de client communicatie geldig is. Op basis van de resultaten van deze controles, de client communicatie wordt opnieuw gemaakt indien nodig. Dus als er een uitzondering optreedt, u niet wilt opnieuw `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory Lifetime
[FabricServiceProxyFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) is een factory die de proxy voor externe communicatie van andere interfaces maakt. Als u API gebruikt `ServiceProxyBase.create` voor het maken van proxy framework maakt vervolgens een `FabricServiceProxyFactory`.
Is het nuttig om een handmatig maken als u wilt onderdrukken [ServiceRemotingClientFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) eigenschappen.
Factory is een dure bewerking. `FabricServiceProxyFactory` houdt de cache van clients voor communicatie.
Aanbevolen procedure is aan het cachegeheugen `FabricServiceProxyFactory` voor zo lang mogelijk.

## <a name="remoting-exception-handling"></a>Afhandeling van uitzonderingen voor externe toegang
De externe uitzondering veroorzaakt door de API-service, worden als RuntimeException of FabricException terug naar de client verzonden.

ServiceProxy wordt verwerkt alle failover-uitzondering voor de service-partitie dat deze wordt gemaakt. Deze opnieuw de eindpunten opgelost als er Failover Exceptions(Non-Transient Exceptions) en probeert om opnieuw de aanroep met het juiste eindpunt. Aantal nieuwe pogingen voor failover-uitzondering is oneindig.
In geval van een TransientExceptions, het alleen opnieuw probeert de aanroep.

Standaard opnieuw parameters worden opgegeven door [OperationRetrySettings]. (https://docs.microsoft.com/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Kunt u deze waarden configureren door OperationRetrySettings object doorgeven aan de constructor ServiceProxyFactory.

## <a name="next-steps"></a>Volgende stappen
* [Beveiligen van communicatie voor Reliable Services](service-fabric-reliable-services-secure-communication-java.md)
