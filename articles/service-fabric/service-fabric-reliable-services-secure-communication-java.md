---
title: Veilige communicatie van de service voor externe toegang met behulp van Java in Azure Service Fabric | Microsoft Docs
description: Informatie over het beveiligen van de service voor externe toegang op basis van communicatie voor betrouwbare Java-services die worden uitgevoerd in een Azure Service Fabric-cluster.
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
ms.openlocfilehash: 1843720b9700e66af8ee84766cf7d63ac62e6283
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749909"
---
# <a name="secure-service-remoting-communications-in-a-java-service"></a>Veilige communicatie van de service voor externe toegang in een Java-service
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java op Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

Beveiliging is een van de belangrijkste aspecten van de communicatie. Het framework Reliable Services biedt enkele vooraf gedefinieerde communicatie stacks en hulpprogramma's die u gebruiken kunt om beveiliging te verbeteren. In dit artikel wordt beschreven hoe de beveiliging verbeteren wanneer u externe toegang de service in een Java-service. Dit is gebaseerd op een bestaande [voorbeeld](service-fabric-reliable-services-communication-remoting-java.md) waarin wordt uitgelegd hoe instellen van externe toegang voor betrouwbare services die zijn geschreven in Java. 

Om te helpen beveiligen van een service wanneer u service voor externe toegang met Java-services, als volgt:

1. Maken van een interface `HelloWorldStateless`, die definieert de methoden die beschikbaar zijn voor een externe procedureaanroep voor uw service. Uw service maakt gebruik van `FabricTransportServiceRemotingListener`, die is gedeclareerd in de `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` pakket. Dit is een `CommunicationListener` -implementatie mogelijkheden voor externe toegang biedt.

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. Instellingen voor listener en beveiligingsreferenties toevoegen.

    Zorg ervoor dat het certificaat dat u wilt gebruiken om te helpen beveiligen van uw servicecommunicatie is geïnstalleerd op alle knooppunten in het cluster. Er zijn twee manieren waarop u instellingen voor listener en beveiligingsreferenties kunt opgeven:

   1. Ze bieden met behulp van een [configuratiepakket](service-fabric-application-and-service-manifests.md):

       Toevoegen van een benoemde `TransportSettings` sectie in het bestand settings.xml.

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       In dit geval de `createServiceInstanceListeners` methode ziet er als volgt:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Als u een `TransportSettings` sectie in het bestand settings.xml zonder voorvoegsel, `FabricTransportListenerSettings` alle instellingen in deze sectie standaard wordt geladen.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        In dit geval de `CreateServiceInstanceListeners` methode ziet er als volgt:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Wanneer u methoden aanroepen voor een beveiligde service met behulp van de stack voor externe toegang, in plaats van de `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klasse te maken van een serviceproxy, gebruikt u `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    Als de clientcode wordt uitgevoerd als onderdeel van een service, kunt u laden `FabricTransportSettings` uit het bestand settings.xml. Maak een sectie TransportSettings die vergelijkbaar is met de servicecode, zoals eerder besproken. De volgende wijzigingen aanbrengen in de clientcode:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
