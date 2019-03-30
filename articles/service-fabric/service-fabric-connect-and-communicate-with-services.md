---
title: Verbinding maken en te communiceren met services in Azure Service Fabric | Microsoft Docs
description: Informatie over het oplossen, verbinden en communiceren met services in Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 55a0a1a8097ea46c7a3407b5f42824973edcf1a2
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666117"
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Verbinding maken en te communiceren met services in Service Fabric
In Service Fabric, een service wordt uitgevoerd ergens in een Service Fabric-cluster, meestal verdeeld over meerdere virtuele machines. Deze kan worden verplaatst vanaf één locatie naar een andere, door de eigenaar van de service of automatisch door Service Fabric. Services zijn niet statisch gekoppeld aan een bepaalde machine of een adres.

Een Service Fabric-toepassing bestaat meestal uit veel verschillende services uit, waarbij elke service een speciale taak uitvoert. Deze services kunnen communiceren met elkaar om een volledige functie, zoals rendering verschillende onderdelen van een web-App. Er zijn ook clienttoepassingen die verbinding maken met en communiceren met services. Dit document wordt beschreven hoe u communicatie met en tussen uw services in Service Fabric instelt.

## <a name="bring-your-own-protocol"></a>Breng uw eigen protocol
Service Fabric kunt u de levenscyclus van uw services beheren, maar deze heeft geen beslissingen over wat uw services doen. Dit geldt ook voor communicatie. Wanneer de service wordt geopend door Service Fabric, dat is de mogelijkheid van uw service voor het instellen van een eindpunt voor binnenkomende aanvragen en welke protocol of communicatie-stack die u wilt gebruiken. Uw service controleert op een normale **IP: poort** adres met behulp van een adresschema gebruiken, zoals een URI. Meerdere service-exemplaren of replica's kunnen delen een hostproces in dat geval wordt een moeten verschillende poorten gebruiken of een mechanisme delen van de poort, zoals het http.sys-stuurprogramma voor kernel in Windows. In beide gevallen moet elke service-exemplaar of de replica in een hostproces uniek adresseerbaar zijn.

![Service-eindpunten][1]

## <a name="service-discovery-and-resolution"></a>Servicedetectie en oplossing
In een gedistribueerd systeem kunnen services verplaatsen van een machine naar een andere na verloop van tijd. Dit kan gebeuren om verschillende redenen, waaronder resourceverdeling, upgrades, failover of scale-out. Dit betekent dat service-eindpuntadressen wijzigen als de service wordt verplaatst naar knooppunten met verschillende IP-adressen en kan op verschillende poorten geopend als de service gebruikmaakt van een dynamisch geselecteerde poort.

![Distributie van services][7]

Service Fabric biedt een oplossing voor het opsporen en service met de naam van de Naming-Service. De Naming-Service onderhoudt een tabel die is toegewezen met de naam van de service-exemplaren naar de eindpuntadressen die ze luisteren. Alle benoemde service-exemplaren in Service Fabric hebben unieke namen weergegeven als de URI's, bijvoorbeeld `"fabric:/MyApplication/MyService"`. De naam van de service wordt niet gewijzigd tijdens de levensduur van de service, is de eindpuntadressen die veranderen kunnen wanneer services verplaatsen. Dit is vergelijkbaar met websites met constante URL's, maar waar het IP-adres kan wijzigen. En is vergelijkbaar met DNS op het web, waarmee website-URL's worden omgezet in IP-adressen, Service Fabric een registrar die servicenamen wordt toegewezen aan hun eindpuntadres.

![Service-eindpunten][2]

Omvat de volgende stappen uitvoeren in een lus op te lossen en verbinding maken met services:

* **Los**: Het eindpunt dat een service die beschikbaar heeft gesteld ophalen uit de Naming-Service.
* **Verbinding maken met**: Verbinding maken met de service via welke protocol op dat eindpunt wordt gebruikt.
* **Probeer**: Een poging om verbinding te kan mislukken bij een aantal redenen, voor bijvoorbeeld als de service is verplaatst, omdat de laatste keer dat adres van het eindpunt opgelost is. In dat geval de voorgaande oplossen en verbinding maken met stappen moeten opnieuw worden uitgevoerd en deze cyclus wordt herhaald totdat de verbinding is geslaagd.

## <a name="connecting-to-other-services"></a>Verbinding maken met andere services
Services met elkaar verbinden in een cluster in het algemeen kunnen rechtstreeks toegang tot de eindpunten van andere services, omdat de knooppunten in een cluster zich op hetzelfde lokale netwerk. Als u wilt maken is eenvoudiger verbinding maken tussen services, Service Fabric biedt aanvullende services die gebruikmaken van de Naming-Service. Een DNS-service en een omgekeerde proxy-service.


### <a name="dns-service"></a>DNS-service
Aangezien veel services, services, met name in containers, kan de naam van een bestaande URL hebt, kunt deze oplossen met behulp van de standaard DNS-protocol (in plaats van het protocol Naming-Service) is kunnen zeer handig, met name in toepassingsscenario's 'lift and shift'. Dit is precies wat de DNS-service heeft. Hiermee kunt u het DNS-namen worden toegewezen aan een servicenaam en kan daarom oplossen eindpunt IP-adressen. 

Zoals u in het volgende diagram, wordt de DNS-service, die worden uitgevoerd in de Service Fabric-cluster DNS-namen toegewezen aan service-naam, die vervolgens worden opgelost door de Naming-Service om terug te keren de eindpuntadressen verbinding maken met. De DNS-naam voor de service wordt geleverd op het moment dat wordt gemaakt. 

![Service-eindpunten][9]

Voor meer informatie over het gebruik van de DNS-service Zie [DNS-service in Azure Service Fabric](service-fabric-dnsservice.md) artikel.

### <a name="reverse-proxy-service"></a>Omgekeerde proxy-service
De omgekeerde proxy adressen services in het cluster dat wordt aangegeven dat HTTP-eindpunten, met inbegrip van HTTPS. De omgekeerde proxy aanzienlijk eenvoudiger met een specifieke URI-indeling voor het aanroepen van andere services en de methoden en verwerkt de oplossen, verbinding maken en stappen die nodig zijn voor een service om te communiceren met een andere met behulp van de Naming-Service opnieuw. Met andere woorden, deze wordt ingeschakeld, wordt de Naming-Service van u bij het aanroepen van andere services doordat dit net zo eenvoudig als het aanroepen van een URL.

![Service-eindpunten][10]

Zie voor meer informatie over het gebruik van de reverse proxy-service [omgekeerde proxy in Azure Service Fabric](service-fabric-reverseproxy.md) artikel.

## <a name="connections-from-external-clients"></a>Verbindingen van externe clients
Services met elkaar verbinden in een cluster in het algemeen kunnen rechtstreeks toegang tot de eindpunten van andere services, omdat de knooppunten in een cluster zich op hetzelfde lokale netwerk. In sommige omgevingen, maar een cluster mogelijk achter een load balancer waarmee externe inkomend verkeer via een beperkte set van poorten. In dergelijke gevallen services kunnen nog steeds communiceren met elkaar en omzetten van adressen met behulp van de Naming-Service, maar extra stappen moeten worden genomen om toe te staan externe clients verbinding maken met services.

## <a name="service-fabric-in-azure"></a>Service Fabric in Azure
Een Service Fabric-cluster in Azure bevindt zich achter een Load Balancer van Azure. Alle externe verkeer naar het cluster moet doorgeven aan de load balancer. De load balancer wordt automatisch doorsturen van verkeer inkomend verkeer op een bepaalde poort op een willekeurige *knooppunt* dat dezelfde poort geopend is. De Azure Load Balancer weet alleen over de poorten geopend op de *knooppunten*, weet het niet over het openen van poorten door afzonderlijke *services*.

![Azure Load Balancer en Service Fabric-topologie][3]

Bijvoorbeeld, om te kunnen accepteren extern verkeer op poort **80**, de volgende zaken moeten worden geconfigureerd:

1. Schrijven van een service die op poort 80 luistert. Poort 80 in ServiceManifest.xml van de service configureren en open een listener in de service, bijvoorbeeld een zelf-hostend webserver.

    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...

            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint =
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string publishUri = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
                return Task.FromResult(publishUri);
            }

            ...
        }

        class WebService : StatelessService
        {
            ...

            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] { new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }

            ...
        }
    ```
    ```java
        class HttpCommunicationlistener implements CommunicationListener {
            ...

            @Override
            public CompletableFuture<String> openAsync(CancellationToken arg0) {
                EndpointResourceDescription endpoint =
                    this.serviceContext.getCodePackageActivationContext().getEndpoint("WebEndpoint");
                try {
                    HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(endpoint.getPort()), 0);
                    server.start();

                    String publishUri = String.format("http://%s:%d/",
                        this.serviceContext.getNodeContext().getIpAddressOrFQDN(), endpoint.getPort());
                    return CompletableFuture.completedFuture(publishUri);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            ...
        }

        class WebService extends StatelessService {
            ...

            @Override
            protected List<ServiceInstanceListener> createServiceInstanceListeners() {
                <ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
                listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationlistener(context)));
                return listeners;       
            }

            ...
        }
    ```
2. Een Service Fabric-Cluster maken in Azure en geeft u poort **80** als een aangepast eindpunt-poort voor het knooppunttype die als voor de service host. Als u meer dan één knooppunttype hebt, kunt u instellen een *plaatsing beperking* op de service te controleren of deze alleen kan worden uitgevoerd op het knooppunttype waarvoor de aangepast eindpuntpoort geopend.

    ![Open een poort op een knooppunttype][4]
3. Zodra het cluster is gemaakt, configureert u de Azure Load Balancer in de resourcegroep van het cluster voor het doorsturen van verkeer op poort 80. Bij het maken van een cluster via Azure portal, wordt dit automatisch ingesteld voor elk aangepast eindpunt-poort die is geconfigureerd.

    ![Doorsturen van verkeer in de Azure Load Balancer][5]
4. De Azure Load Balancer maakt gebruik van een test om te bepalen of u niet om verkeer te verzenden naar een bepaald knooppunt. De test controleert regelmatig of er een eindpunt op elk knooppunt om te bepalen of het knooppunt reageert. Als de test is mislukt voor het ontvangen van een reactie na een geconfigureerde aantal keren, stopt de load balancer verkeer verzenden naar dat knooppunt. Bij het maken van een cluster via Azure portal, is een test automatisch ingesteld voor elk aangepast eindpunt-poort die is geconfigureerd.

    ![Doorsturen van verkeer in de Azure Load Balancer][8]

Het is belangrijk te onthouden dat de Azure Load Balancer en de test alleen weet over de *knooppunten*, niet de *services* die worden uitgevoerd op de knooppunten. De Azure Load Balancer wordt altijd verkeer verzenden naar knooppunten die op de test, reageren zodat het van belang om ervoor te zorgen services zijn beschikbaar op de knooppunten die kunnen reageren op de test.

## <a name="reliable-services-built-in-communication-api-options"></a>Reliable Services: Opties voor communicatie van ingebouwde API
Het framework van Reliable Services wordt geleverd met verschillende opties voor vooraf gemaakte communicatie. De beslissing over welke een methode het beste voor u werkt is afhankelijk van de keuze van het programmeermodel, de communicatie-framework en de programmeertaal die uw services zijn geschreven in.

* **Er zijn geen specifieke protocol:**  Als u een bepaalde keuze van communicatie-framework niet hebt, maar u iets van snel gebruiksklaar wilt maken, is de ideale optie voor u [service remoting](service-fabric-reliable-services-communication-remoting.md), waarmee externe procedure sterk getypeerde wordt aangeroepen Reliable Services en Reliable Actors. Dit is de eenvoudigste en snelste manier aan de slag met servicecommunicatie. Service voor externe toegang zorgt voor omzetting van de service-adressen, verbinding, opnieuw proberen en foutafhandeling. Dit is beschikbaar voor zowel C# en Java-toepassingen.
* **HTTP**: Voor communicatie met de taal-neutraal biedt HTTP een industriestandaard-keuze met hulpprogramma's en HTTP-servers die beschikbaar zijn in verschillende talen, geheel ondersteund door Service Fabric. Services kunnen elke beschikbaar, waaronder HTTP-stack gebruiken [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) voor C# toepassingen. Clients die zijn geschreven in C# kunnen gebruikmaken van de `ICommunicationClient` en `ServicePartitionClient` klassen, terwijl voor Java, gebruikt u de `CommunicationClient` en `FabricServicePartitionClient` klassen, [voor service-oplossing, HTTP-verbindingen en probeer het opnieuw lussen](service-fabric-reliable-services-communication.md).
* **WCF**: Als u bestaande code die gebruikmaakt van WCF als uw communicatie-framework hebt, dan kunt u de `WcfCommunicationListener` voor de serverkant en `WcfCommunicationClient` en `ServicePartitionClient` klassen voor de client. Dit echter is alleen beschikbaar voor C# toepassingen op Windows gebaseerde clusters. Zie dit artikel voor meer informatie over [WCF-gebaseerde implementatie van de communicatiestack](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Met behulp van aangepaste protocollen en andere frameworks voor communicatie
Services kunnen gebruiken elk protocol of framework voor communicatie, ongeacht of dit een aangepaste binaire protocol via TCP-sockets of streaming van gebeurtenissen via [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) of [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/). Service Fabric biedt een communicatie-API's die u uw communicatiestack in, sluit kunt terwijl al het werk om te detecteren en verbinding maken abstract van u gemaakt is. Raadpleeg dit artikel over de [betrouwbare Service berichtenmodel](service-fabric-reliable-services-communication.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de concepten en de beschikbare API's in de [Reliable Services-berichtenmodel](service-fabric-reliable-services-communication.md), klikt u vervolgens snel aan de slag met [service voor externe toegang](service-fabric-reliable-services-communication-remoting.md) of gaat u uitgebreide voor meer informatie over het schrijven van een bericht listener met behulp van [Web-API met OWIN zelf-hostende](service-fabric-reliable-services-communication-webapi.md).

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
