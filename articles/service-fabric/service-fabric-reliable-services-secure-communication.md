---
title: Veilige communicatie van de service voor externe toegang via C# in Azure Service Fabric | Microsoft Docs
description: Meer informatie over het beveiligen van externe toegang op basis van servicecommunicatie voor C# betrouwbare services die worden uitgevoerd in een Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: chackdan
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: b6d4a44a53ba553ab4fd514c81867156192b69f5
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662530"
---
# <a name="secure-service-remoting-communications-in-a-c-service"></a>Beveiligde communicatie van de service voor externe toegang in een C# service
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java op Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

Security is een van de belangrijkste aspecten van de communicatie. Het framework van de toepassing betrouwbare Services biedt een aantal vooraf gedefinieerde communicatie-stacks en hulpprogramma's die u gebruiken kunt om beveiliging te verbeteren. In dit artikel wordt beschreven hoe u de beveiliging te verbeteren wanneer u externe communicatie in een C# service. Dit is gebaseerd op een bestaande [voorbeeld](service-fabric-reliable-services-communication-remoting.md) waarin wordt uitgelegd hoe u voor het instellen van externe toegang tot betrouwbare services die zijn geschreven in C#. 

Om te helpen beveiligen van een service als u service voor externe toegang met C# services, als volgt te werk:

1. Maken van een interface `IHelloWorldStateful`, die de methoden die beschikbaar zijn voor een externe procedureaanroep op uw service definieert. Uw service maakt gebruik van `FabricTransportServiceRemotingListener`, die is gedeclareerd in de `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` naamruimte. Dit is een `ICommunicationListener` mogelijkheden voor externe toegang biedt-implementatie.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```
2. -Listener-instellingen en beveiligingsreferenties toevoegen.

    Zorg ervoor dat het certificaat dat u wilt gebruiken voor het beveiligen van uw servicecommunicatie is geïnstalleerd op alle knooppunten in het cluster. 
    
    > [!NOTE]
    > Op Linux-knooppunten, het certificaat moet aanwezig zijn als PEM-indeling bestanden in de */var/lib/sfcerts* directory. Zie voor meer informatie, [locatie en de opmaak van X.509-certificaten op Linux-knooppunten](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes). 

    Er zijn twee manieren u listenerinstellingen en beveiligingsreferenties kunt opgeven:

   1. Geef ze rechtstreeks in de servicecode:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           FabricTransportRemotingListenerSettings  listenerSettings = new FabricTransportRemotingListenerSettings
           {
               MaxMessageSize = 10000000,
               SecurityCredentials = GetSecurityCredentials()
           };
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
           };
       }

       private static SecurityCredentials GetSecurityCredentials()
       {
           // Provide certificate details.
           var x509Credentials = new X509Credentials
           {
               FindType = X509FindType.FindByThumbprint,
               FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
               StoreLocation = StoreLocation.LocalMachine,
               StoreName = "My",
               ProtectionLevel = ProtectionLevel.EncryptAndSign
           };
           x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
           x509Credentials.RemoteCertThumbprints.Add("9FEF3950642138446CC364A396E1E881DB76B483");
           return x509Credentials;
       }
       ```
   2. Ze bieden met behulp van een [configuratiepakket](service-fabric-application-and-service-manifests.md):

       Voeg een benoemd exemplaar `TransportSettings` sectie in het bestand settings.xml.

       ```xml
       <Section Name="HelloWorldStatefulTransportSettings">
           <Parameter Name="MaxMessageSize" Value="10000000" />
           <Parameter Name="SecurityCredentialsType" Value="X509" />
           <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
           <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
           <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
           <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
           <Parameter Name="CertificateStoreName" Value="My" />
           <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
           <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
       </Section>
       ```

       In dit geval de `CreateServiceReplicaListeners` methode er als volgt:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(
                       context,this,FabricTransportRemotingListenerSettings .LoadFrom("HelloWorldStatefulTransportSettings")))
           };
       }
       ```

        Als u een `TransportSettings` sectie in het bestand settings.xml `FabricTransportRemotingListenerSettings ` alle instellingen in deze sectie standaard wordt geladen.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        In dit geval de `CreateServiceReplicaListeners` methode er als volgt:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                return new[]{
                        new ServiceReplicaListener(
                            (context) => new FabricTransportServiceRemotingListener(context,this))};
            };
        }
        ```
3. Wanneer u methoden aanroepen op een beveiligde service met behulp van de stack voor externe toegang, in plaats van de `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klasse voor het maken van een serviceproxy, gebruikt u `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Doorgeven `FabricTransportRemotingSettings`, die bevat `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "9FEF3950642138446CC364A396E1E881DB76B483",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
    x509Credentials.RemoteCertThumbprints.Add("4FEF3950642138446CC364A396E1E881DB76B48C");

    FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Als de clientcode wordt uitgevoerd als onderdeel van een service, kunt u laden `FabricTransportRemotingSettings` uit het bestand settings.xml. Maak een sectie HelloWorldClientTransportSettings die vergelijkbaar is met de servicecode, zoals eerder besproken. De volgende wijzigingen aanbrengen in de clientcode:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Als de client wordt niet uitgevoerd als onderdeel van een service, kunt u een bestand client_name.settings.xml maken op dezelfde locatie waar de client_name.exe is. Vervolgens maakt u een sectie TransportSettings in dat bestand.

    Vergelijkbaar met de service, als u een `TransportSettings` sectie in client-settings.xml/client_name.settings.xml `FabricTransportRemotingSettings` alle instellingen in deze sectie standaard geladen.

    In dat geval wordt de eerdere code nog verder vereenvoudigd:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```


Lees de volgende stap, [Web-API met OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md).
