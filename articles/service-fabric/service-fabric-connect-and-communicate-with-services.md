---
title: Verbinding maken en te communiceren met services in Azure Service Fabric | Microsoft Docs
description: Informatie over het oplossen, verbinding maken en communiceren met Service Fabric-services.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: d0b4ff1959465ade5f57c045d2a005e828638eb2
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Verbinding maken en te communiceren met Service Fabric-services
In Service Fabric, een service wordt uitgevoerd ergens in een Service Fabric-cluster, doorgaans verdeeld over meerdere virtuele machines. Deze kan worden verplaatst vanaf één locatie naar een andere, door de eigenaar van de service of automatisch door de Service Fabric. Services zijn niet statisch gekoppeld aan een bepaalde machine of een adres.

Een Service Fabric-toepassing in het algemeen bestaat uit veel verschillende services, waarbij elke service een speciale taak uitvoert. Deze services kunnen communiceren met elkaar om een volledige functie, zoals de rendering van verschillende onderdelen van een webtoepassing. Er zijn ook clienttoepassingen die verbinding maken met en communiceren met de services. Dit document wordt beschreven hoe u voor het instellen van de communicatie met en tussen uw services in Service Fabric.

Servicecommunicatie wordt ook beschreven in deze video van Microsoft Virtual Academy:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965">  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a>Breng uw eigen protocol
Service Fabric helpt de levenscyclus van uw services beheren, maar maakt geen nemen van beslissingen over wat uw services doen. Dit omvat communicatie. Wanneer uw service wordt geopend door Service Fabric, is dat uw service mogelijkheid voor het instellen van een eindpunt voor binnenkomende aanvragen, ongeacht protocol of communicatie stack die u wilt gebruiken. Uw service luistert op een normale **IP: poort** adres met behulp van een adresschema gebruiken, zoals een URI. Meerdere service-exemplaren of replica's kunnen delen een hostproces in dat geval ze moet verschillende poorten gebruiken of een mechanisme met delen van de poort, zoals het stuurprogramma van de kernel http.sys in Windows. In beide gevallen moet elke service-exemplaar of de replica in een hostproces uniek adresseerbaar zijn.

![Service-eindpunten][1]

## <a name="service-discovery-and-resolution"></a>Servicedetectie en oplossing
In een gedistribueerde systeem kunnen services verplaatsen van de ene machine naar een andere gedurende een bepaalde periode. Dit kan gebeuren om verschillende redenen, waaronder resourceverdeling, upgrades, failovers of scale-out. Dit betekent dat adressen van de service-eindpunten wijzigen, omdat de service wordt verplaatst naar de knooppunten met verschillende IP-adressen en op verschillende poorten openen kan als de service gebruikmaakt van een dynamisch geselecteerde poort.

![Distributie van services][7]

Service Fabric biedt een service voor het opsporen en oplossen met de naam van de Naming Service. De Naming Service onderhoudt een tabel die is toegewezen met de service-exemplaren en de endpoint-adressen die ze luisteren op naam. Alle benoemde service-exemplaren in Service Fabric unieke namen weergegeven als URI's, bijvoorbeeld hebben `"fabric:/MyApplication/MyService"`. De naam van de service niet verandert gedurende de levensduur van de service, is de endpoint-adressen die wijzigen kunnen wanneer services verplaatst. Dit is vergelijkbaar met websites die constante URL's, maar waarbij het IP-adres kan worden gewijzigd. En vergelijkbare naar DNS op het web en website-URL's worden omgezet in IP-adressen, Service Fabric heeft een registrar die servicenamen wordt toegewezen aan hun eindpuntadres.

![Service-eindpunten][2]

Het omzetten van en verbinding maken met services omvat de volgende stappen uitvoeren in een lus:

* **Los**: ophalen van het eindpunt dat een service die beschikbaar heeft gesteld van de Service namen.
* **Verbinding maken met**: verbinding maken met de service via het protocol ongeacht gebruikt op dat eindpunt.
* **Probeer**: een verbindingspoging kan mislukken voor een aantal redenen, voor als de service is verplaatst sinds de laatste keer dat het eindpuntadres bijvoorbeeld opgelost is. In dat geval de voorgaande oplossen en stappen moet opnieuw worden geprobeerd verbinding maken en deze cyclus wordt herhaald totdat de verbinding is geslaagd.

## <a name="connecting-to-other-services"></a>Verbinding met andere services
Services met elkaar verbinden in een cluster in het algemeen kunnen rechtstreeks toegang tot de eindpunten van andere services, omdat de knooppunten in een cluster zich op hetzelfde lokale netwerk. Als u wilt maken is eenvoudiger verbinding maken tussen services, Service Fabric bevat aanvullende services die gebruikmaken van de Naming Service. Een DNS-service en een omgekeerde proxy-service.


### <a name="dns-service"></a>DNS-service
Omdat veel services vooral beperkte services, een bestaande URL-naam kunnen hebben, kunnen deze oplossen met behulp van de standaard DNS-protocol (in plaats van het protocol Naming Service) het is zeer handig, met name in scenario's voor toepassing 'lift- en verschuiven'. Dit is precies wat de DNS-service bestaat. Hiermee kunt u DNS-namen toewijzen aan een servicenaam en daarom eindpunt IP-adressen omzetten. 

Zoals u in het volgende diagram, wordt de DNS-service, in het Service Fabric-cluster, DNS-namen toegewezen aan Servicenamen die vervolgens worden opgelost door de Naming Service om te retourneren van de endpoint-adressen verbinding maken met. De DNS-naam voor de service is opgegeven op het moment van maken. 

![Service-eindpunten][9]

Voor meer informatie over het gebruik van de DNS-service Zie [DNS-service in Azure Service Fabric](service-fabric-dnsservice.md) artikel.

### <a name="reverse-proxy-service"></a>Reverse proxy-service
De omgekeerde proxy adressen services in het cluster dat toegang biedt tot HTTP-eindpunten, met inbegrip van HTTPS. De omgekeerde proxy aanzienlijk vereenvoudigt het aanroepen van andere services en de methoden wanneer er een specifieke URI-indeling en verwerkt de resolve verbinding, stappen die nodig zijn voor een service te communiceren met andere met het Naming Service opnieuw. Het verborgen met andere woorden, de Naming Service van u bij het aanroepen van andere services door het maken van dit net zo eenvoudig als het aanroepen van een URL.

![Service-eindpunten][10]

Zie voor meer informatie over het gebruik van de reverse proxy-service [omgekeerde proxy in Azure Service Fabric](service-fabric-reverseproxy.md) artikel.

## <a name="connections-from-external-clients"></a>Verbindingen met externe clients
Services met elkaar verbinden in een cluster in het algemeen kunnen rechtstreeks toegang tot de eindpunten van andere services, omdat de knooppunten in een cluster zich op hetzelfde lokale netwerk. In sommige omgevingen echter een cluster mogelijk achter een load balancer waarmee externe inkomend verkeer via een beperkte set van poorten. In dergelijke gevallen services kunnen nog steeds met elkaar communiceren en adressen met behulp van de Naming Service omzetten, maar extra stappen moeten worden genomen om externe clients verbinding maken met services toestaan.

## <a name="service-fabric-in-azure"></a>Service Fabric in Azure
Een Service Fabric-cluster in Azure wordt geplaatst achter een Load Balancer van Azure. Alle externe verkeer naar het cluster moet doorgeven aan de load balancer. De load balancer wordt automatisch doorsturen van verkeer inkomend verkeer op een opgegeven poort in een willekeurige *knooppunt* die dezelfde poort geopend is. De Azure Load Balancer alleen bewust van poorten geopend op de *knooppunten*, niet weet over poorten openen door afzonderlijke *services*.

![Azure Load Balancer en Service Fabric-topologie][3]

Bijvoorbeeld, om te kunnen accepteren van externe verkeer op poort **80**, de volgende zaken moeten worden geconfigureerd:

1. Schrijven van een service die op poort 80 luistert. Poort 80 in de service ServiceManifest.xml configureren en open een listener in de service, bijvoorbeeld een host zichzelf webserver.

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
2. Een Service Fabric-Cluster in Azure maken en Geef poort **80** als een aangepaste endpoint-poort voor het knooppunttype die als host voor de service fungeert. Als u meer dan één knooppunttype hebt, kunt u instellen een *plaatsing beperking* op de service zodat deze kan alleen worden uitgevoerd op het knooppunttype met de aangepaste eindpuntpoort geopend.

    ![Een poort op een knooppunttype openen][4]
3. Zodra het cluster is gemaakt, moet u de Azure Load Balancer configureren in de clusterbrongroep voor het doorsturen van verkeer op poort 80. Wanneer u een cluster via de Azure portal, is deze ingesteld automatisch voor elke aangepaste endpoint-poort die is geconfigureerd.

    ![Doorsturen van verkeer in de Azure Load Balancer][5]
4. De Azure Load Balancer gebruikmaakt van een test om te bepalen of verkeer te verzenden naar een bepaald knooppunt. De test controleert regelmatig of er een eindpunt op elk knooppunt om te bepalen of het knooppunt reageert. Als de test geen reactie ontvangen na een geconfigureerde aantal keren, stopt de load balancer verkeer kunnen verzenden naar dat knooppunt. Bij het maken van een cluster via de Azure portal is een test automatisch ingesteld voor elke aangepaste endpoint-poort die is geconfigureerd.

    ![Doorsturen van verkeer in de Azure Load Balancer][8]

Het is belangrijk te weten dat de Load Balancer van Azure en de test alleen weten over de *knooppunten*, niet de *services* uitgevoerd op de knooppunten. De Azure Load Balancer wordt altijd verkeer verzenden naar knooppunten die op de test, reageren dus wees voorzichtig om ervoor te zorgen services beschikbaar zijn op de knooppunten die kunnen reageren op de test.

## <a name="reliable-services-built-in-communication-api-options"></a>Betrouwbare Services: Opties van de ingebouwde communicatie API
Het framework Reliable Services geleverd met verschillende vooraf samengestelde communicatieopties. De beslissing over welke een voor u geschikt is afhankelijk van de keuze van het programmeermodel, de communicatie-framework en de programmeertaal die uw services zijn geschreven in.

* **Er is geen specifieke protocol:** als u niet een bepaalde keuze van communicatie framework hebt, maar u iets up snel gebruiksklaar wilt maken, is de ideale oplossing voor u [remoting service](service-fabric-reliable-services-communication-remoting.md), waardoor externe procedure sterk getypeerde roept voor Reliable Services en Reliable Actors. Dit is het snelst en eenvoudigst manier aan de slag met servicecommunicatie. Service voor externe toegang zorgt omzetting van de service-adressen, verbinding, probeer het opnieuw en foutafhandeling. Dit is beschikbaar voor zowel C# en Java-toepassingen.
* **HTTP**: voor taal networkdirect communicatie, HTTP biedt een keuze industriestandaard met hulpprogramma's en HTTP-servers die beschikbaar zijn in verschillende talen, geheel ondersteund door Service Fabric. Services kunnen elke beschikbaar is, met inbegrip van HTTP-stack gebruiken [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) voor C#-toepassingen. Clients die zijn geschreven in C# kunnen gebruikmaken van de `ICommunicationClient` en `ServicePartitionClient` klassen, terwijl voor Java, gebruikt u de `CommunicationClient` en `FabricServicePartitionClient` klassen, [voor omzetting van de service, HTTP-verbindingen en probeer het opnieuw lussen](service-fabric-reliable-services-communication.md).
* **WCF**: als u bestaande code die gebruikmaakt van WCF als uw communicatie-framework hebt, dan kunt u de `WcfCommunicationListener` voor de serverzijde en `WcfCommunicationClient` en `ServicePartitionClient` klassen voor de client. Dit echter is alleen beschikbaar voor C#-toepassingen op Windows gebaseerde clusters. Zie voor meer informatie in dit artikel over [WCF-gebaseerde implementatie van de communicatiestack](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Met behulp van aangepaste protocollen en andere frameworks communicatie
Services kunnen gebruiken elke protocol of elk framework voor communicatie gebruikt, ongeacht of dit een aangepast binaire protocol via TCP-sockets of streaming-gebeurtenissen via [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) of [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/). Service Fabric bevat communicatie API's die u uw communicatiestack in, sluit kunt terwijl het werk om te detecteren en verbinding maken is gescheiden van u. Raadpleeg dit artikel over de [betrouwbare Service communicatiemodel](service-fabric-reliable-services-communication.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de concepten en de beschikbare API's in de [Reliable Services communicatiemodel](service-fabric-reliable-services-communication.md), vervolgens snel aan de slag met [remoting service](service-fabric-reliable-services-communication-remoting.md) of Ga voor informatie over het schrijven van een mededeling diepgaande listener met [Web-API met OWIN zelf host](service-fabric-reliable-services-communication-webapi.md).

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
