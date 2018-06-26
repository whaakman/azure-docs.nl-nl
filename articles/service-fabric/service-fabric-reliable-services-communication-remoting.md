---
title: Service voor externe toegang met C# in Service Fabric | Microsoft Docs
description: Service Fabric remoting kan clients en -services te communiceren met C#-services met behulp van een remote procedure call.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: ad56580e73c06acff95b3146f6dc2d83ab2ba3ae
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945969"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>Service voor externe toegang in C# met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java op Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Voor services die niet zijn gekoppeld aan een bepaalde communicatieprotocol of stack, zoals WebAPI, Windows Communication Foundation (WCF) of anderen, biedt het framework Reliable Services een mechanisme voor externe toegang voor het snel en eenvoudig instellen van externe procedureaanroepen weer dat voor Services. Dit artikel wordt beschreven hoe u externe procedureaanroepen weer dat voor services die zijn geschreven met C# instelt.

## <a name="set-up-remoting-on-a-service"></a>Instellen van externe toegang voor een Service
Instellen van externe toegang tot een service wordt uitgevoerd in de twee eenvoudige stappen:

1. Maak een interface voor uw service te implementeren. Deze interface definieert de methoden die beschikbaar voor een externe procedureaanroep voor uw service zijn. De methoden moeten geretourneerd asynchrone methoden. De interface moet implementeren `Microsoft.ServiceFabric.Services.Remoting.IService` om aan te geven dat de service een externe communicatie-interface heeft.
2. Gebruik een listener voor externe toegang in uw service. RemotingListener is een `ICommunicationListener` -implementatie mogelijkheden voor externe toegang biedt. De `Microsoft.ServiceFabric.Services.Remoting.Runtime` -naamruimte bevat een uitbreidingsmethode`CreateServiceRemotingListener` voor zowel stateless als stateful services die kunnen worden gebruikt voor het maken van de listener voor een externe toegang met behulp van het standaardprotocol voor het transport van externe toegang.

>[!NOTE]
>De `Remoting` naamruimte is beschikbaar als een afzonderlijke NuGet-pakket aangeroepen `Microsoft.ServiceFabric.Services.Remoting`

De volgende staatloze service wordt bijvoorbeeld één methode voor 'Hallo wereld' via een externe procedureaanroep.

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task<string> HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context => this.CreateServiceRemotingListener(context)) };
    }
}
```
> [!NOTE]
> De argumenten en de retourtypen in de interface van de service mag geen enkelvoudig, complexe of aangepaste typen, maar ze moeten serialiseerbaar zijn door de .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Externe servicemethoden aanroepen
Bij het aanroepen van methoden met een service met behulp van de stack voor externe toegang wordt uitgevoerd met behulp van een lokale proxy voor de service via de `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klasse. De `ServiceProxy` methode maakt u een lokale proxyserver met behulp van dezelfde interface die de service implementeert. Met deze proxy, kunt u methoden extern aanroepen op de interface.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Het framework remoting uitzonderingen worden veroorzaakt door de service naar de client wordt doorgegeven. Als gevolg hiervan, wanneer u `ServiceProxy`, de client is verantwoordelijk voor het verwerken van de uitzonderingen die door de service.

## <a name="service-proxy-lifetime"></a>Levensduur voor service-Proxy
Het maken van ServiceProxy is een lichtgewicht bewerking, zodat u zoveel naar wens kunt maken. Proxy-service-exemplaren kunnen opnieuw worden gebruikt zolang ze nodig zijn. U kunt een remote procedure call genereert een uitzondering, nog steeds hetzelfde exemplaar van de proxy te hergebruiken. Elke ServiceProxy bevat een communicatie-client gebruikt om berichten te verzenden via de kabel. Tijdens het aanroepen van externe oproepen, worden interne controles uitgevoerd om te bepalen of de client communicatie geldig is. Op basis van de resultaten van deze controles, de client communicatie wordt opnieuw gemaakt indien nodig. Dus als er een uitzondering optreedt, u niet wilt opnieuw `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory Lifetime
[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) is een factory die de proxy worden exemplaren voor externe communicatie van andere interfaces gemaakt. Als u de api gebruiken `ServiceProxy.Create` voor het maken van proxy het framework maakt u een singleton-ServiceProxy.
Is het nuttig om een handmatig maken als u wilt onderdrukken [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) eigenschappen.
Maken van de factory is een dure bewerking. ServiceProxyFactory onderhoudt een interne cache van de client communicatie.
Aanbevolen procedure is het in de cache ServiceProxyFactory zo lang mogelijk.

## <a name="remoting-exception-handling"></a>Afhandeling van uitzonderingen voor externe toegang
Alle externe uitzonderingen die door de API-service worden verzonden naar de client als AggregateException. RemoteExceptions moet DataContract serialiseerbaar; Als dat niet het geval is, de proxy API genereert [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) met de serialisatiefout in het.

ServiceProxy verwerkt alle failover-uitzonderingen voor de service-partitie dat deze wordt gemaakt. Het opnieuw de eindpunten wordt omgezet als er failover-uitzonderingen (tijdelijke uitzonderingen) en de aanroep met het juiste eindpunt opnieuw probeert. Het aantal nieuwe pogingen voor failover-uitzonderingen zijn onbepaalde.
Als tijdelijke uitzonderingen optreden, probeert de proxy opnieuw de aanroep.

Standaard opnieuw parameters worden opgegeven door [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).

U kunt deze waarden configureren door OperationRetrySettings object doorgeven aan de constructor ServiceProxyFactory.

## <a name="how-to-use-the-remoting-v2-stack"></a>Het gebruik van de stack Remoting V2

U hebt de optie voor het gebruik van de stack Remoting V2 vanaf versie 2.8 van de Remoting NuGet-pakket. De externe toegang-V2-stack is van meer zodat en functies zoals aangepaste serialisatie en meer pluggable API biedt.
Sjablooncode blijft de Remoting V1-Stack gebruiken.
Externe communicatie V2 is niet compatibel met V1 (de vorige Remoting stack), volg daarom onderstaande instructies op [van V1 upgraden naar V2](#how-to-upgrade-from-remoting-v1-to-remoting-v2) zonder enige impact op de beschikbaarheid van de service.

De volgende methoden zijn beschikbaar voor het inschakelen van de V2-stack.

### <a name="using-an-assembly-attribute-to-use-the-v2-stack"></a>Via een assembly-kenmerk voor het gebruik van de stack V2

Deze stappen sjablooncode voor het gebruik van de V2-Stack met behulp van een assembly-kenmerk te wijzigen.

1. Wijzigen van de Endpoint-bron van `"ServiceEndpoint"` naar `"ServiceEndpointV2"` in het servicemanifest.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Gebruik de `Microsoft.ServiceFabric.Services.Remoting.Runtime.CreateServiceRemotingInstanceListeners` uitbreidingsmethode remoting-listeners (equal voor V1- en V2) maken.

  ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
  ```

3. Markeer de assembly met de interfaces voor externe toegang met een `FabricTransportServiceRemotingProvider` kenmerk.

  ```csharp
  [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
  ```

Er zijn geen codewijzigingen zijn vereist in het client-project.
Bouw de client-assembly met de interface-assembly om ervoor te zorgen dat de assembly-kenmerk dat wordt weergegeven boven wordt gebruikt.

### <a name="using-explicit-v2-classes-to-use-the-v2-stack"></a>Expliciete V2 klassen via het gebruik van de stack V2

Als alternatief voor het gebruik van een assembly-kenmerk worden de V2-stack ook ingeschakeld met behulp van expliciete V2-klassen.

Deze stappen sjablooncode voor het gebruik van de V2-Stack met expliciete V2-klassen te wijzigen.

1. Wijzigen van de Endpoint-bron van `"ServiceEndpoint"` naar `"ServiceEndpointV2"` in het servicemanifest.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Gebruik de [FabricTransportServiceRemotingListener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingListener?view=azure-dotnet) van de `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Runtime` naamruimte.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                return new FabricTransportServiceRemotingListener(c, this);

            })
        };
    }
  ```

3. Gebruik de [FabricTransportServiceRemotingClientFactory ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet) van de `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client` naamruimte, maken clients.

  ```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
  ```

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2"></a>Het upgraden van externe toegang V1 naar V2 voor externe toegang.
Om te kunnen upgraden V1 naar V2, zijn 2-stap upgrades vereist. Volgende stappen om te worden uitgevoerd in de reeks die worden vermeld.

1. Upgrade van de V1-Service naar Service V2 via CompactListener kenmerk.
Deze wijziging zorgt ervoor dat service op V1- en V2-Listener luistert.

    een) de bron van een eindpunt met naam toevoegen als 'ServiceEndpointV2' in het servicemanifest.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    b) gebruiken na uitbreidingsmethode Remoting Listener te maken.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c) Assembly-kenmerk toevoegen op Interfaces voor externe toegang om CompatListener en V2-Client te gebruiken.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.CompatListener, RemotingClient = RemotingClient.V2Client)]

      ```
2. Upgrade van de V1-Client naar V2-Client via een kenmerk V2-Client.
Deze stap zorgt ervoor dat client V2-stack wordt gebruikt.
Er is geen wijziging in Client-Project/Service vereist. Client-projecten bouwen met bijgewerkte interface assembly is voldoende.

3. Deze stap is optioneel. Het kenmerk V2Listener gebruiken en werk vervolgens de V2-Service.
Deze stap zorgt ervoor dat service alleen op V2-Listener luistert.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
```

## <a name="how-to-use-custom-serialization-with-remoting-v2"></a>Het gebruik van aangepaste serialisatie met V2 voor externe toegang.
In het volgende voorbeeld wordt Json-serialisatie met Remoting V2 gebruikt.
1. Implementeer IServiceRemotingMessageSerializationProvider interface voor implementatie van aangepaste serialisatie.
    Hier volgt het codefragment op hoe implementatie ziet.

 ```csharp
    public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
    {
        public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType,
            IEnumerable<Type> requestBodyTypes)
        {
            return new ServiceRemotingRequestJsonMessageBodySerializer(serviceInterfaceType, requestBodyTypes);
        }

        public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType,
            IEnumerable<Type> responseBodyTypes)
        {
            return new ServiceRemotingResponseJsonMessageBodySerializer(serviceInterfaceType, responseBodyTypes);
        }

        public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
        {
            return new JsonMessageFactory();
        }
    }

    class JsonMessageFactory : IServiceRemotingMessageBodyFactory
    {
        public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName,
            int numberOfParameters)
        {
            return new JsonRemotingRequestBody();
        }

        public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName)
        {
            return new JsonRemotingResponseBody();
        }
    }

    class ServiceRemotingRequestJsonMessageBodySerializer : IServiceRemotingRequestMessageBodySerializer
    {
        public ServiceRemotingRequestJsonMessageBodySerializer(Type serviceInterfaceType,
            IEnumerable<Type> parameterInfo)
        {
        }

        public OutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
        {
            if (serviceRemotingRequestMessageBody == null)
            {
                return null;
            }

            var writeStream = new MemoryStream();
            var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream));

            var serializer = JsonSerializer.Create(new JsonSerializerSettings()
            {
                TypeNameHandling = TypeNameHandling.All
            });
            serializer.Serialize(jsonWriter, serviceRemotingRequestMessageBody);

            jsonWriter.Flush();
            var segment = new ArraySegment<byte>(writeStream.ToArray());
            var segments = new List<ArraySegment<byte>> { segment };
            return new OutgoingMessageBody(segments);
        }

        public IServiceRemotingRequestMessageBody Deserialize(IncomingMessageBody messageBody)
        {
            using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

            using (JsonReader reader = new JsonTextReader(sr))
            {
                var serializer = JsonSerializer.Create(new JsonSerializerSettings()
                {
                    TypeNameHandling = TypeNameHandling.All
                });

                return serializer.Deserialize<JsonRemotingRequestBody>(reader);
            }
        }
    }

    class ServiceRemotingResponseJsonMessageBodySerializer : IServiceRemotingResponseMessageBodySerializer
    {
        public ServiceRemotingResponseJsonMessageBodySerializer(Type serviceInterfaceType,
            IEnumerable<Type> parameterInfo)
        {
        }

        public OutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
        {
            var json = JsonConvert.SerializeObject(responseMessageBody, new JsonSerializerSettings()
            {
                TypeNameHandling = TypeNameHandling.All
            });
            var bytes = Encoding.UTF8.GetBytes(json);
            var segment = new ArraySegment<byte>(bytes);
            var list = new List<ArraySegment<byte>> { segment };
            return new OutgoingMessageBody(list);
        }

        public IServiceRemotingResponseMessageBody Deserialize(IncomingMessageBody messageBody)
        {
            using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

            using (var reader = new JsonTextReader(sr))
            {
                var serializer = JsonSerializer.Create(new JsonSerializerSettings()
                {
                    TypeNameHandling = TypeNameHandling.All
                });

                return serializer.Deserialize<JsonRemotingResponseBody>(reader);
            }
        }
    }

    internal class JsonRemotingResponseBody : IServiceRemotingResponseMessageBody
    {
        public object Value;

        public void Set(object response)
        {
            this.Value = response;
        }

        public object Get(Type paramType)
        {
            return this.Value;
        }
    }

    class JsonRemotingRequestBody : IServiceRemotingRequestMessageBody
    {
        public readonly Dictionary<string, object> parameters = new Dictionary<string, object>();        

        public void SetParameter(int position, string parameName, object parameter)
        {
            this.parameters[parameName] = parameter;
        }

        public object GetParameter(int position, string parameName, Type paramType)
        {
            return this.parameters[parameName];
        }
    }
 ```

2.    Serialisatie van standaardprovider met JsonSerializationProvider voor externe toegang-Listener overschrijven.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
   {
       return new[]
       {
           new ServiceInstanceListener((c) =>
           {
               return new FabricTransportServiceRemotingListener(c, this,
                   new ServiceRemotingJsonSerializationProvider());
           })
       };
   }
  ```

3.    Onderdrukking van de standaardprovider voor serialisatie met JsonSerializationProvider voor Clientfabriek van externe toegang.

```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
            {
                return new FabricTransportServiceRemotingClientFactory(
                    serializationProvider: new ServiceRemotingJsonSerializationProvider());
            });
  ```

## <a name="next-steps"></a>Volgende stappen
* [Web-API met OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-communicatie met Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Beveiligen van communicatie voor Reliable Services](service-fabric-reliable-services-secure-communication.md)

