---
title: Betrouwbare WCF Services communicatiestack | Microsoft Docs
description: De ingebouwde WCF communicatie-stack in Service Fabric biedt WCF-communicatie van client-service voor Reliable Services.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 75516e1e-ee57-4bc7-95fe-71ec42d452b2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: 7037620ebdc26a9f18531064bf45d058f5060e39
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-communicatiestack voor Reliable Services
Het framework Reliable Services kan auteurs in service kiezen van de communicatiestack die ze willen gebruiken voor hun service. Ze kunnen plug-in de communicatiestack van hun keuze via de **ICommunicationListener** geretourneerd door de [CreateServiceReplicaListeners of CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) methoden. Het framework biedt een implementatie van de communicatiestack gebaseerd op Windows Communication Foundation (WCF) voor service-auteurs die u wilt gebruiken, WCF-communicatie.

## <a name="wcf-communication-listener"></a>Listener voor WCF-communicatie
De WCF-specifieke implementatie van **ICommunicationListener** wordt geleverd door de **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** klasse.

Lest spreek we een servicecontract van het type hebben`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

We kunnen de listener voor een WCF-communicatie maken in de service de volgende manier.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Schrijven van clients voor de WCF-stack-communicatie
Voor het schrijven van clients te communiceren met services met behulp van WCF het framework biedt **WcfClientCommunicationFactory**, dit is de WCF-specifieke implementatie van [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Het communicatiekanaal WCF toegankelijk is vanaf de **WcfCommunicationClient** gemaakt door de **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Clientcode kunt gebruiken de **WcfCommunicationClientFactory** samen met de **WcfCommunicationClient** welke implements **ServicePartitionClient** om te bepalen van het service-eindpunt en communiceren met de service.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
> [!NOTE]
> De standaardwaarde ServicePartitionResolver wordt ervan uitgegaan dat de client in hetzelfde cluster als de service wordt uitgevoerd. Als dat niet het geval is, een ServicePartitionResolver-object maken en in de eindpunten van het cluster-verbinding.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Externe procedureaanroep met Reliable Services voor externe toegang](service-fabric-reliable-services-communication-remoting.md)
* [Web-API met OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Beveiligen van communicatie voor Reliable Services](service-fabric-reliable-services-secure-communication.md)

