---
title: Veilige communicatie van de service voor externe toegang met C# in Azure Service Fabric | Microsoft Docs
description: Informatie over het beveiligen van de service voor externe toegang op basis van communicatie voor C# betrouwbare services die worden uitgevoerd in een Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: be5dab7b9714f13a4bd30e6ab33a5a0e2016212d
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37020016"
---
# <a name="secure-service-remoting-communications-in-a-c-service"></a>Veilige communicatie van de service voor externe toegang in een C#-service
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java op Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

Beveiliging is een van de belangrijkste aspecten van de communicatie. Het framework Reliable Services biedt enkele vooraf gedefinieerde communicatie stacks en hulpprogramma's die u gebruiken kunt om beveiliging te verbeteren. In dit artikel wordt beschreven hoe de beveiliging verbeteren wanneer u externe toegang de service in een C#-service. Dit is gebaseerd op een bestaande [voorbeeld](service-fabric-reliable-services-communication-remoting.md) waarin wordt uitgelegd hoe instellen van externe toegang voor betrouwbare services die zijn geschreven in C#. 

Om te helpen beveiligen van een service wanneer u service voor externe toegang met C#-services, als volgt:

1. Maken van een interface `IHelloWorldStateful`, die definieert de methoden die beschikbaar zijn voor een externe procedureaanroep voor uw service. Uw service maakt gebruik van `FabricTransportServiceRemotingListener`, die is gedeclareerd in de `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` naamruimte. Dit is een `ICommunicationListener` -implementatie mogelijkheden voor externe toegang biedt.

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
2. Instellingen voor listener en beveiligingsreferenties toevoegen.

    Zorg ervoor dat het certificaat dat u wilt gebruiken om te helpen beveiligen van uw servicecommunicatie is geïnstalleerd op alle knooppunten in het cluster. 
    
    > [!NOTE]
    > Op Linux-knooppunten het certificaat moet aanwezig zijn als bestanden PEM-indeling in de */var/lib/sfcerts* directory. Zie voor meer informatie, [locatie en indeling van het X.509-certificaten op Linux-knooppunten](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes). 

    Er zijn twee manieren waarop u instellingen voor listener en beveiligingsreferenties kunt opgeven:

   1. Ze bieden rechtstreeks in de servicecode:

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

       Toevoegen van een benoemde `TransportSettings` sectie in het bestand settings.xml.

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

       In dit geval de `CreateServiceReplicaListeners` methode ziet er als volgt:

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
        In dit geval de `CreateServiceReplicaListeners` methode ziet er als volgt:

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
3. Wanneer u methoden aanroepen voor een beveiligde service met behulp van de stack voor externe toegang, in plaats van de `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klasse te maken van een serviceproxy, gebruikt u `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Doorgeven `FabricTransportRemotingSettings`, die bevat `SecurityCredentials`.

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

    Als de client wordt niet uitgevoerd als onderdeel van een service, kunt u een bestand client_name.settings.xml op dezelfde locatie waar de client_name.exe is. Maak vervolgens een TransportSettings-sectie in dat bestand.

    Net als bij de service, als u een `TransportSettings` sectie in client-settings.xml/client_name.settings.xml `FabricTransportRemotingSettings` alle instellingen in deze sectie standaard laadt.

    In dat geval wordt de eerdere code nog verder vereenvoudigd:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```


Een volgende stap lezen [Web-API met OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md).
