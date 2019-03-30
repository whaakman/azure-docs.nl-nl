---
title: Service voor externe toegang met behulp van C# in Service Fabric | Microsoft Docs
description: Externe communicatie met Service Fabric kan clients en services om te communiceren met C#-services met behulp van een externe procedureaanroep.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: f9cd6e2fee738d2d42c790b4eb7b9a876a44b01d
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670792"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>Service voor externe toegang in C# met Reliable Services

> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java op Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Voor services die niet zijn gekoppeld aan een bepaalde communicatieprotocol of stack, zoals een web-API, Windows Communication Foundation of andere resources, biedt het framework van Reliable Services een mechanisme voor externe toegang snel en eenvoudig instellen van externe procedureaanroepen voor Services. In dit artikel wordt beschreven hoe het instellen van externe procedureaanroepen voor services die zijn geschreven met C#.

## <a name="set-up-remoting-on-a-service"></a>Instellen van een service voor externe toegang

U kunt instellen van externe toegang tot een service in twee eenvoudige stappen:

1. Maak een interface voor uw service te implementeren. Deze interface definieert de methoden die beschikbaar voor een externe procedureaanroep op uw service zijn. De methoden retourneren van de taak moeten zijn asynchrone methoden. De interface moet implementeren `Microsoft.ServiceFabric.Services.Remoting.IService` om aan te geven dat de service een externe communicatie-interface heeft.
2. Gebruik een listener voor externe toegang in uw service. Een listener voor externe toegang is een `ICommunicationListener` mogelijkheden voor externe toegang biedt-implementatie. De `Microsoft.ServiceFabric.Services.Remoting.Runtime` naamruimte bevat de uitbreidingsmethode `CreateServiceRemotingListener` voor staatloze en stateful services die kunnen worden gebruikt voor het maken van een listener voor externe toegang met behulp van het standaardprotocol voor het transport van externe toegang.

>[!NOTE]
>De `Remoting` naamruimte is beschikbaar als een afzonderlijke NuGet-pakket met de naam `Microsoft.ServiceFabric.Services.Remoting`.

Bijvoorbeeld, een één methode voor het "Hallo wereld" ophalen via een externe procedureaanroep wordt aangegeven dat de volgende stateless service.

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
     return this.CreateServiceRemotingInstanceListeners();
    }
}
```

> [!NOTE]
> De argumenten en de typen retourneren die in de service-interface kunnen een eenvoudige, complexe of aangepaste typen zijn, maar moeten ze kunnen worden geserialiseerd met de .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Externe servicemethoden aanroepen

Methoden voor het aanroepen van een service met behulp van de stack voor externe toegang wordt uitgevoerd met behulp van een lokale proxyserver naar de service via de `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klasse. De `ServiceProxy` methode maakt u een lokale proxyserver met behulp van dezelfde interface die de service implementeert. Met deze proxy, kunt u methoden op afstand op de interface aanroepen.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Het framework voor externe toegang geeft uitzonderingen die door de service naar de client. Als gevolg hiervan, wanneer `ServiceProxy`wordt gebruikt, de client is verantwoordelijk voor het verwerken van de uitzonderingen die door de service.

## <a name="service-proxy-lifetime"></a>De levensduur van de Service-proxy

Maken van de Service-proxy is een lichte bewerking, zodat u zoveel behoefte kunt maken. Proxy-service-exemplaren kunnen worden hergebruikt voor, zolang ze wel nodig zijn. Als een externe procedureaanroep is een uitzondering genereert, kunt u nog steeds hetzelfde exemplaar van de proxy opnieuw gebruiken. De proxy van elke service bevat een communicatie-client gebruikt voor het verzenden van berichten via de kabel. Tijdens het aanroepen van externe oproepen, interne controles uitgevoerd om te bepalen of de client communicatie geldig is. Op basis van de resultaten van deze controles, wordt de client communicatie opnieuw gemaakt als nodig is. Dus als er een uitzondering optreedt, u hoeft niet opnieuw maken `ServiceProxy`.

### <a name="service-proxy-factory-lifetime"></a>Service-proxy factory levensduur

[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) is een factory die de proxy worden exemplaren voor externe communicatie van andere interfaces gemaakt. Als u de API `ServiceProxyFactory.CreateServiceProxy` voor het maken van een proxy, het framework maakt u een singleton-serviceproxy.
Het is handig om een handmatig maken wanneer u nodig hebt voor de onderdrukking [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v1.client.iserviceremotingclientfactory) eigenschappen.
Maken van de factory is een dure bewerking. Een service proxy factory onderhoudt een interne cache van de client communicatie.
Er is een best practice om de service proxy factory zo lang mogelijk in cache.

## <a name="remoting-exception-handling"></a>Afhandeling van uitzonderingen voor externe toegang

Alle externe uitzonderingen die door de API-service worden verzonden naar de client als AggregateException. Externe uitzonderingen moet kunnen worden geserialiseerd met DataContract. Als dat niet het geval is, de API-proxy genereert [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) met de serialisatiefout erin.

Proxy voor de service handelt alle failover-uitzonderingen voor de servicepartitie die deze wordt gemaakt. Deze oplossing voor de eindpunten weer als er failover-uitzonderingen (niet-tijdelijke uitzonderingen) zijn en probeert om opnieuw de aanroep met het juiste eindpunt. Het aantal nieuwe pogingen voor failover-uitzonderingen is onbepaalde tijd.
Als er tijdelijke uitzonderingen optreden, probeert de proxy opnieuw de aanroep.

Standaardparameters voor nieuwe pogingen worden geleverd door [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).

Een gebruiker kan deze waarden configureren door het OperationRetrySettings-object doorgeven aan de constructor ServiceProxyFactory.

## <a name="use-the-remoting-v2-stack"></a>De remoting V2-stack gebruiken

Vanaf versie 2.8 van het NuGet-pakket voor externe toegang hebt u de optie voor het gebruik van de remoting V2-stack. De remoting V2-stack presteert beter. Het biedt ook functies zoals meer pluggable API's en aangepaste serialisatie.
Sjablooncode blijft de V1-stack van externe toegang gebruiken.
Remoting V2 is niet compatibel met V1 (de vorige voor externe toegang-stack). Volg de instructies in het artikel [upgraden van V1 in V2](#upgrade-from-remoting-v1-to-remoting-v2) om te voorkomen dat gevolgen voor de beschikbaarheid van de service.

De volgende methoden zijn beschikbaar voor het inschakelen van de V2-stack.

### <a name="use-an-assembly-attribute-to-use-the-v2-stack"></a>Een assembly-kenmerk gebruiken om te gebruiken van de stack V2

Deze stappen wijzigen de sjablooncode voor het gebruik van de V2-stack met behulp van een assembly-kenmerk.

1. Wijzigen van de endpoint-resource in `"ServiceEndpoint"` naar `"ServiceEndpointV2"` in het servicemanifest.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
   </Resources>
   ```

2. Gebruik de `Microsoft.ServiceFabric.Services.Remoting.Runtime.CreateServiceRemotingInstanceListeners` uitbreidingsmethode maken voor externe toegang-listeners (equal voor zowel V1 als V2).

   ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
   ```

3. Markeren van de assembly die de interfaces voor externe toegang met bevat een `FabricTransportServiceRemotingProvider` kenmerk.

   ```csharp
   [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
   ```

Er zijn geen codewijzigingen vereist zijn in het clientproject.
Bouw de client-assembly met de interface-assembly om ervoor te zorgen dat de assembly-kenmerk dat eerder is getoond wordt gebruikt.

### <a name="use-explicit-v2-classes-to-use-the-v2-stack"></a>Expliciete V2-klassen te gebruiken van de V2-stack gebruiken

Als alternatief voor het gebruik van een kenmerk van de assembly, kan het V2-stack ook worden ingeschakeld met behulp van expliciete V2-klassen.

Deze stappen wijzigen de sjablooncode voor het gebruik van de V2-stack met behulp van expliciete V2-klassen.

1. Wijzigen van de endpoint-resource in `"ServiceEndpoint"` naar `"ServiceEndpointV2"` in het servicemanifest.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
   </Resources>
   ```

2. Gebruik [FabricTransportServiceRemotingListener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingListener?view=azure-dotnet) uit de `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Runtime` naamruimte.

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

3. Gebruik [FabricTransportServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet) uit de `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client` naamruimte voor het maken van clients.

   ```csharp
   var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
   ```

## <a name="upgrade-from-remoting-v1-to-remoting-v2"></a>Upgrade uitvoeren van externe toegang V1 naar remoting V2

Als u wilt upgraden van V1 in V2, zijn upgrades voor verificatie in twee stappen vereist. Volg de stappen in deze reeks.

1. De V1-service upgraden naar V2-service met behulp van dit kenmerk.
Deze wijziging zorgt ervoor dat de service de V1 en V2-listener luistert.

    a. Voeg een eindpuntresource met de naam 'ServiceEndpointV2' in het servicemanifest.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    b. Gebruik de volgende uitbreidingsmethode te maken van een listener voor externe toegang.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c. Een assembly-kenmerk toevoegen op interfaces voor externe toegang om de V1 en V2-listener en de V2-client te gebruiken.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2|RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2)]

      ```
2. Upgrade van de V1-client naar een V2-client met behulp van het kenmerk van de client V2.
Deze stap zorgt ervoor dat de client maakt gebruik van de V2-stack.
Er is geen wijziging in de client-project/service vereist. Client-projecten bouwen met bijgewerkte interface assembly is voldoende.

3. Deze stap is optioneel. Gebruik het V2-listener-kenmerk en werk vervolgens de V2-service.
Deze stap zorgt ervoor dat de service alleen op de V2-listener luistert.

    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
    ```


## <a name="use-the-remoting-v2-interface-compatible-stack"></a>De remoting V2 (compatibel interface)-stack gebruiken

 De remoting V2 (interface compatibel zijn, ook wel V2_1) stack heeft alle functies van de externe toegang-stack V2. De interface-stack is compatibel met de externe communicatie van V1-stack, maar het is niet compatibel met V2 en V1. Om te upgraden van V1 naar V2_1 zonder die betrekking hebben op beschikbaarheid van de service, volgt u de stappen in het artikel Upgrade van V1 in V2 (compatibel-interface).


### <a name="use-an-assembly-attribute-to-use-the-remoting-v2-interface-compatible-stack"></a>Een assembly-kenmerk gebruiken om te gebruiken van de remoting V2-stack (interface-compatibel)

Volg deze stappen om te wijzigen in een V2_1-stack.

1. Voeg een eindpuntresource met de naam 'ServiceEndpointV2_1' in het servicemanifest.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
   </Resources>
   ```

2. Gebruik de methode van de extensie voor externe toegang om te maken van een listener voor externe toegang.

   ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
   ```

3. Voeg een [kenmerk assembly](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.fabrictransport.fabrictransportserviceremotingproviderattribute?view=azure-dotnet) op interfaces voor externe toegang.

   ```csharp
    [assembly:  FabricTransportServiceRemotingProvider(RemotingListenerVersion=  RemotingListenerVersion.V2_1, RemotingClientVersion= RemotingClientVersion.V2_1)]

   ```

Er zijn geen wijzigingen vereist zijn in het clientproject.
Bouw de client-assembly met de interface-assembly om ervoor te zorgen dat het vorige kenmerk van de assembly wordt gebruikt.

### <a name="use-explicit-remoting-classes-to-create-a-listenerclient-factory-for-the-v2-interface-compatible-version"></a>Externe communicatie met expliciete klassen gebruiken om een gegevensfactory-listener /-client voor de V2 (compatibel interface) versie te maken

Volg deze stappen:

1. Voeg een eindpuntresource met de naam 'ServiceEndpointV2_1' in het servicemanifest.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
   </Resources>
   ```

2. Gebruik de [remoting V2-listener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotinglistener?view=azure-dotnet). De service-eindpunt resource standaardnaam gebruikt is "ServiceEndpointV2_1." Het moet worden gedefinieerd in het servicemanifest.

   ```csharp
   protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                var settings = new FabricTransportRemotingListenerSettings();
                settings.UseWrappedMessage = true;
                return new FabricTransportServiceRemotingListener(c, this,settings);

            })
        };
    }
   ```

3. Gebruik de V2 [clientfabriek](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet).
   ```csharp
   var proxyFactory = new ServiceProxyFactory((c) =>
          {
            var settings = new FabricTransportRemotingSettings();
            settings.UseWrappedMessage = true;
            return new FabricTransportServiceRemotingClientFactory(settings);
          });
   ```

## <a name="upgrade-from-remoting-v1-to-remoting-v2-interface-compatible"></a>Upgrade van externe toegang V1 naar remoting V2 (compatibel-interface)

Upgrade uitvoeren van V1 in V2 (interface compatibel zijn, V2_1 genoemd), upgrades voor verificatie in twee stappen zijn vereist. Volg de stappen in deze reeks.

1. De V1-service upgraden naar V2_1 service met behulp van het volgende kenmerk.
Deze wijziging zorgt ervoor dat de service op de V1- en de V2_1-listener luistert.

    a. Voeg een eindpuntresource met de naam 'ServiceEndpointV2_1' in het servicemanifest.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2_1" />  
        </Endpoints>
      </Resources>
      ```

    b. Gebruik de volgende uitbreidingsmethode te maken van een listener voor externe toegang.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c. Een assembly-kenmerk op interfaces voor externe toegang toevoegen aan de V1, V2_1 listener en u V2_1 client.
    ```csharp
   [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1 | RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2_1)]

      ```
2. De V1-client naar de client V2_1 upgraden met behulp van het kenmerk van de client V2_1.
Deze stap zorgt ervoor dat de client gebruikt de V2_1-stack.
Er is geen wijziging in de client-project/service vereist. Client-projecten bouwen met bijgewerkte interface assembly is voldoende.

3. Deze stap is optioneel. De versie van de listener V1 verwijderen uit het kenmerk en werk vervolgens de V2-service.
Deze stap zorgt ervoor dat de service alleen op de V2-listener luistert.

    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1, RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```
  
### <a name="use-custom-serialization-with-a-remoting-wrapped-message"></a>Gebruik aangepaste serialisatie met een externe toegang verpakt bericht

Voor een ingepakte bericht van externe toegang maken we een enkel object verpakte met alle parameters als een veld in het.
Volg deze stappen:

1. Implementeer de `IServiceRemotingMessageSerializationProvider` interface voor de implementatie voor aangepaste serialisatie.
    Dit codefragment laat zien hoe de implementatie eruit ziet.

      ```csharp
      public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
      {
        public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
        {
          return new JsonMessageFactory();
        }

        public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> requestWrappedType, IEnumerable<Type> requestBodyTypes = null)
        {
          return new ServiceRemotingRequestJsonMessageBodySerializer();
        }

        public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> responseWrappedType, IEnumerable<Type> responseBodyTypes = null)
        {
          return new ServiceRemotingResponseJsonMessageBodySerializer();
        }
      }
      ```
      ```csharp
        class JsonMessageFactory : IServiceRemotingMessageBodyFactory
            {

              public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName, int numberOfParameters, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }

              public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }
            }
      ```
      ```csharp
      class ServiceRemotingRequestJsonMessageBodySerializer : IServiceRemotingRequestMessageBodySerializer
        {
            private JsonSerializer serializer;

            public ServiceRemotingRequestJsonMessageBodySerializer()
            {
              serializer = JsonSerializer.Create(new JsonSerializerSettings()
              {
                TypeNameHandling = TypeNameHandling.All
                });
              }

              public IOutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
             {
               if (serviceRemotingRequestMessageBody == null)
               {
                 return null;
               }          
               using (var writeStream = new MemoryStream())
               {
                 using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
                 {
                   serializer.Serialize(jsonWriter, serviceRemotingRequestMessageBody);
                   jsonWriter.Flush();
                   var bytes = writeStream.ToArray();
                   var segment = new ArraySegment<byte>(bytes);
                   var segments = new List<ArraySegment<byte>> { segment };
                   return new OutgoingMessageBody(segments);
                 }
               }
              }

              public IServiceRemotingRequestMessageBody Deserialize(IIncomingMessageBody messageBody)
             {
               using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
               {
                 using (JsonReader reader = new JsonTextReader(sr))
                 {
                   var ob = serializer.Deserialize<JsonBody>(reader);
                   return ob;
                 }
               }
             }
            }
      ```
      ```csharp
      class ServiceRemotingResponseJsonMessageBodySerializer : IServiceRemotingResponseMessageBodySerializer
       {
         private JsonSerializer serializer;

        public ServiceRemotingResponseJsonMessageBodySerializer()
        {
          serializer = JsonSerializer.Create(new JsonSerializerSettings()
          {
              TypeNameHandling = TypeNameHandling.All
            });
          }

          public IOutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
          {
            if (responseMessageBody == null)
            {
              return null;
            }

            using (var writeStream = new MemoryStream())
            {
              using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
              {
                serializer.Serialize(jsonWriter, responseMessageBody);
                jsonWriter.Flush();
                var bytes = writeStream.ToArray();
                var segment = new ArraySegment<byte>(bytes);
                var segments = new List<ArraySegment<byte>> { segment };
                return new OutgoingMessageBody(segments);
              }
            }
          }

          public IServiceRemotingResponseMessageBody Deserialize(IIncomingMessageBody messageBody)
          {

             using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
             {
               using (var reader = new JsonTextReader(sr))
               {
                 var obj = serializer.Deserialize<JsonBody>(reader);
                 return obj;
               }
             }
           }
       }
    ```
    ```csharp
    class JsonBody : WrappedMessage, IServiceRemotingRequestMessageBody, IServiceRemotingResponseMessageBody
    {
          public JsonBody(object wrapped)
          {
            this.Value = wrapped;
          }

          public void SetParameter(int position, string parameName, object parameter)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object GetParameter(int position, string parameName, Type paramType)
          {
            //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public void Set(object response)
          { //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object Get(Type paramType)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }
    }
    ```

2. Onderdrukking van de standaardprovider voor serialisatie met `JsonSerializationProvider` voor een listener voor externe toegang.

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

3. Onderdrukking van de standaardprovider voor serialisatie met `JsonSerializationProvider` voor de clientfabriek van een externe communicatie.

    ```csharp
    var proxyFactory = new ServiceProxyFactory((c) =>
    {
        return new FabricTransportServiceRemotingClientFactory(
        serializationProvider: new ServiceRemotingJsonSerializationProvider());
      });
      ```

## <a name="next-steps"></a>Volgende stappen

* [Web-API met OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Windows Communication Foundation communicatie via Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Beveiligde communicatie voor Reliable Services](service-fabric-reliable-services-secure-communication.md)
