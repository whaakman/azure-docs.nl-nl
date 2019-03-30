---
title: Beveiligde communicatie van de service voor externe toegang met behulp van Java in Azure Service Fabric | Microsoft Docs
description: Meer informatie over het beveiligen van communicatie met service voor externe toegang op basis van betrouwbare Java-services die worden uitgevoerd in een Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: chackdan
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: b465ab602a14285f8cf40b24ce1dfa9c763fecb8
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666990"
---
# <a name="secure-service-remoting-communications-in-a-java-service"></a>Beveiligde communicatie van de service voor externe toegang in een Java-service
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java op Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

Security is een van de belangrijkste aspecten van de communicatie. Het framework van de toepassing betrouwbare Services biedt een aantal vooraf gedefinieerde communicatie-stacks en hulpprogramma's die u gebruiken kunt om beveiliging te verbeteren. In dit artikel wordt beschreven hoe u de beveiliging verbeteren wanneer u service voor externe toegang in een Java-service. Dit is gebaseerd op een bestaande [voorbeeld](service-fabric-reliable-services-communication-remoting-java.md) waarin wordt uitgelegd hoe u voor het instellen van externe toegang tot betrouwbare services die zijn geschreven in Java. 

Als u wilt beveiligen een-service wanneer u externe toegang tot service met Java-services, de volgende stappen uit:

1. Maken van een interface `HelloWorldStateless`, die de methoden die beschikbaar zijn voor een externe procedureaanroep op uw service definieert. Uw service maakt gebruik van `FabricTransportServiceRemotingListener`, die is gedeclareerd in de `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` pakket. Dit is een `CommunicationListener` mogelijkheden voor externe toegang biedt-implementatie.

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
2. -Listener-instellingen en beveiligingsreferenties toevoegen.

    Zorg ervoor dat het certificaat dat u wilt gebruiken voor het beveiligen van uw servicecommunicatie is geïnstalleerd op alle knooppunten in het cluster. Voor services die worden uitgevoerd op Linux, moet het certificaat beschikbaar zijn als een PEM-formmatted-bestand. een van beide een `.pem` -bestand dat het certificaat en de persoonlijke sleutel bevat of een `.crt` -bestand dat het certificaat bevat en een `.key` -bestand dat de persoonlijke sleutel bevat. Zie voor meer informatie, [locatie en de opmaak van X.509-certificaten op Linux-knooppunten](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes).
    
    Er zijn twee manieren u listenerinstellingen en beveiligingsreferenties kunt opgeven:

   1. Ze bieden met behulp van een [configuratiepakket](service-fabric-application-and-service-manifests.md):

       Voeg een benoemd exemplaar `TransportSettings` sectie in het bestand settings.xml.

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

       In dit geval de `createServiceInstanceListeners` methode er als volgt:

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
        In dit geval de `CreateServiceInstanceListeners` methode er als volgt:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Wanneer u methoden aanroepen op een beveiligde service met behulp van de stack voor externe toegang, in plaats van de `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klasse voor het maken van een serviceproxy, gebruikt u `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    Als de clientcode wordt uitgevoerd als onderdeel van een service, kunt u laden `FabricTransportSettings` uit het bestand settings.xml. Maak een sectie TransportSettings die vergelijkbaar is met de servicecode, zoals eerder besproken. De volgende wijzigingen aanbrengen in de clientcode:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
