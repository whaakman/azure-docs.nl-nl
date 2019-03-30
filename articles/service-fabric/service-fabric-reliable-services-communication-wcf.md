---
title: Reliable Services WCF-communicatiestack | Microsoft Docs
description: De ingebouwde WCF-communicatiestack in Service Fabric biedt WCF-communicatie van client-service voor Reliable Services.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: chackdan
editor: vturecek
ms.assetid: 75516e1e-ee57-4bc7-95fe-71ec42d452b2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: ae8a0ab0382083ebfca0834d2238403668efa71d
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670573"
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a>Op basis van WCF communicatiestack voor Reliable Services
Het framework van Reliable Services kan auteurs van de service om te kiezen van de communicatiestack die ze willen gebruiken voor hun service. Ze kunnen plug-in de communicatiestack van hun keuze via de **ICommunicationListener** geretourneerd door de [CreateServiceReplicaListeners of CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) methoden. Het framework biedt een implementatie van de communicatiestack op basis van de Windows Communication Foundation (WCF) voor service-auteurs die u wilt gebruiken op basis van een WCF-communicatie.

## <a name="wcf-communication-listener"></a>WCF Communication Listener
De WCF-specifieke implementatie van **ICommunicationListener** wordt geleverd door de **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** klasse.

Lest zeggen we een servicecontract van het type hebben `ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

In de service de volgende manier kunnen we een WCF-communicatielistener maken.

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

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Clients voor de WCF-communicatiestack schrijven
Voor het schrijven van clients kunnen communiceren met services met behulp van WCF, het framework biedt **WcfClientCommunicationFactory**, dit is het WCF-specifieke implementatie van [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Het communicatiekanaal WCF is toegankelijk vanuit de **WcfCommunicationClient** die zijn gemaakt door de **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Clientcode kunt gebruiken de **WcfCommunicationClientFactory** samen met de **WcfCommunicationClient** welke implementeert **ServicePartitionClient** om te bepalen de service-eindpunt en communiceren met de service.

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
> De standaardwaarde ServicePartitionResolver wordt ervan uitgegaan dat de client wordt uitgevoerd in hetzelfde cluster als de service. Als dat niet het geval is, maakt u een ServicePartitionResolver-object en doorgeven in de eindpunten van het cluster-verbinding.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Externe procedureaanroep met externe communicatie Reliable Services](service-fabric-reliable-services-communication-remoting.md)
* [Web-API met OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Beveiliging van de communicatie voor Reliable Services](service-fabric-reliable-services-secure-communication-wcf.md)

