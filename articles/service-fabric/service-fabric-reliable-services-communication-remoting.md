---
title: Service voor externe toegang in Service Fabric | Microsoft Docs
description: Service Fabric remoting kan clients en -services te communiceren met services met behulp van een remote procedure call.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: df4a86e3de87daad22646672f278c7f3226660c6
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="service-remoting-with-reliable-services"></a>Service voor externe toegang met Reliable Services
Voor services die niet zijn gekoppeld aan een bepaalde communicatieprotocol of stack, zoals WebAPI, Windows Communication Foundation (WCF) of anderen, biedt het framework Reliable Services een mechanisme voor externe toegang voor het snel en eenvoudig instellen van externe procedureaanroep voor services.

## <a name="set-up-remoting-on-a-service"></a>Instellen van externe toegang voor een Service
Instellen van externe toegang tot een service wordt uitgevoerd in de twee eenvoudige stappen:

1. Maak een interface voor uw service te implementeren. Deze interface definieert de methoden die beschikbaar voor een externe procedureaanroep voor uw service zijn. De methoden moeten geretourneerd asynchrone methoden. De interface moet implementeren `Microsoft.ServiceFabric.Services.Remoting.IService` om aan te geven dat de service een externe communicatie-interface heeft.
2. Gebruik een listener voor externe toegang in uw service. RemotingListener is een `ICommunicationListener` -implementatie mogelijkheden voor externe toegang biedt. De `Microsoft.ServiceFabric.Services.Remoting.Runtime` -naamruimte bevat een uitbreidingsmethode`CreateServiceRemotingListener` voor zowel stateless als stateful services die kunnen worden gebruikt voor het maken van de listener voor een externe toegang met behulp van het standaardprotocol voor het transport van externe toegang.

>[!NOTE]
>De `Remoting` naamruimte is beschikbaar als een afzonderlijke NuGet-pakket aangeroepen`Microsoft.ServiceFabric.Services.Remoting`

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

    public Task HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context =>            this.CreateServiceRemotingListener(context)) };
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
Het maken van ServiceProxy is een lichtgewicht bewerking, zodat gebruikers zo veel als ze nodig hebben kunnen maken. Proxy-service-exemplaren kunnen opnieuw worden gebruikt als gebruikers deze moeten. Als een remote procedure call er een uitzondering gegenereerd, kunnen gebruikers nog steeds opnieuw gebruiken hetzelfde exemplaar van de proxy. Elke ServiceProxy bevat een communicatie-client gebruikt om berichten te verzenden via de kabel. Tijdens het aanroepen van externe oproepen Controleer we intern of de client communicatie geldig is. Op basis van die resulteren, maken we opnieuw de client communicatie indien nodig. Dus als er een uitzondering optreedt, gebruikers hoeven niet opnieuw maken `ServiceProxy` omdat deze is gedaan transparant.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory levensduur
[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) is een factory die de proxy worden exemplaren voor externe communicatie van andere interfaces gemaakt. Als u de api gebruiken `ServiceProxy.Create` voor het maken van proxy het framework maakt u een singleton-ServiceProxy.
Is het nuttig om een handmatig maken als u wilt onderdrukken [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) eigenschappen.
Maken van de factory is een dure bewerking. ServiceProxyFactory onderhoudt een interne cache van de client communicatie.
Aanbevolen procedure is het in de cache ServiceProxyFactory zo lang mogelijk.

## <a name="remoting-exception-handling"></a>Afhandeling van uitzonderingen voor externe toegang
Alle externe uitzonderingen die door de API-service worden verzonden naar de client als AggregateException. RemoteExceptions moet DataContract serialiseerbaar; Als dat niet het geval is, de proxy API genereert [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) met de serialisatiefout in het.

ServiceProxy verwerkt alle failover-uitzonderingen voor de service-partitie dat deze wordt gemaakt. Het opnieuw de eindpunten wordt omgezet als er failover-uitzonderingen (tijdelijke uitzonderingen) en de aanroep met het juiste eindpunt opnieuw probeert. Het aantal nieuwe pogingen voor failover-uitzonderingen zijn onbepaalde.
Als tijdelijke uitzonderingen optreden, probeert de proxy opnieuw de aanroep.

Standaard opnieuw parameters worden opgegeven door [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).
Gebruiker kan deze waarden door OperationRetrySettings object doorgeven aan de constructor ServiceProxyFactory configureren.
## <a name="how-to-use-remoting-v2-stack"></a>Het gebruik van externe toegang-V2-stack
Met 2,8 Remoting NuGet-pakket hebt u de optie voor het gebruiken van externe toegang-V2-stack. Externe communicatie V2-stack is meer zodat en biedt functies, zoals aangepaste serialiseerbaar en meer pluggable Api.
Als u geen wijzigingen, standaard blijft deze Remoting V1-Stack gebruiken.
Externe communicatie V2 is niet compatibel met V1 (vorige Remoting stack), volg hiervoor onderstaande artikel over het upgraden van V1 tot V2 zonder enige impact op de beschikbaarheid van de service.

### <a name="using-assembly-attribute-to-use-v2-stack"></a>Met behulp van Assembly-kenmerk V2-stack gebruiken.

Hier volgen de stappen volgen om te wijzigen in V2-Stack.

1. De bron van een eindpunt met naam toevoegen als 'ServiceEndpointV2' in het servicemanifest.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />  
    </Endpoints>
  </Resources>
  ```

2.  Externe communicatie uitbreidingsmethode gebruik maken van externe toegang-Listener.

  ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
  ```

3.  Assembly-kenmerk toevoegen op Interfaces voor externe toegang.

  ```csharp
  [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
  ```
Er zijn geen wijzigingen zijn vereist in Client-Project.
De Client-assembly met interface assembly bouwen, naar zorgt ervoor dat hierboven assembly kenmerk wordt gebruikt.

### <a name="using-explicit-v2-classes-to-create-listener-clientfactory"></a>Listener maken via een expliciete V2 klassen / ClientFactory
Hier volgen de stappen te volgen.
1.  De bron van een eindpunt met naam toevoegen als 'ServiceEndpointV2' in het servicemanifest.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />  
    </Endpoints>
  </Resources>
  ```

2. Gebruik [Remoting V2Listener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingistener?view=azure-dotnet). Resource van de Service-eindpunt standaardnaam die wordt gebruikt is 'ServiceEndpointV2' en moet worden gedefinieerd in Service Manifest.

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

3. Gebruik van V2 [Clientfabriek](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet).
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
