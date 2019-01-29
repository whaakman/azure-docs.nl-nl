---
title: Azure Service Fabric reverse proxy | Microsoft Docs
description: Omgekeerde proxy van Service Fabric gebruikt voor communicatie met microservices van binnen en buiten het cluster.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/03/2017
ms.author: bharatn
ms.openlocfilehash: 4b6ef4823fc78c15dda31e96d8bd6c4f798c0e99
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097744"
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Omgekeerde proxy in Azure Service Fabric
Omgekeerde proxy die is ingebouwd in Azure Service Fabric kunt u microservices die worden uitgevoerd in een Service Fabric-cluster detecteren en te communiceren met andere services die http-eindpunten hebben.

## <a name="microservices-communication-model"></a>Communicatie-Microservices-model
Microservices in Service Fabric worden uitgevoerd op een subset van de knooppunten in het cluster en kunt migreren tussen de knooppunten om verschillende redenen. Als gevolg hiervan kunnen de eindpunten voor microservices dynamisch wijzigen. Als u wilt detecteren en communiceren met andere services in het cluster, moet microservice gaan via de volgende stappen uit:

1. Los de servicelocatie via de naming-service.
2. Verbinding maken met de service.
3. De voorgaande stappen verpakken in een lus die service-oplossing implementeert en beleidsregels toepassen op fouten bij het verbinden voor opnieuw proberen

Zie voor meer informatie, [Connect en communiceren met services](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-the-reverse-proxy"></a>Communiceren met behulp van de omgekeerde proxy
Omgekeerde proxy is een service die wordt uitgevoerd op elk knooppunt en verwerkt eindpunt resolutie, automatische nieuwe pogingen en andere verbindingsfouten namens de client-services. Omgekeerde proxy kan worden geconfigureerd voor verschillende beleidsregels van toepassing als het aanvragen van client-services worden verwerkt. Met behulp van een omgekeerde proxy kan de clientservice op elke client-side HTTP-communicatie-bibliotheken en geen speciale oplossing vereisen en Pogingslogica in de service. 

Omgekeerde proxy wordt aangegeven dat een of meer eindpunten op het lokale knooppunt voor clientservices moet worden gebruikt voor het verzenden van aanvragen naar andere services.

![Interne communicatie][1]

> [!NOTE]
> **Ondersteunde Platforms**
>
> Omgekeerde proxy in Service Fabric ondersteunt momenteel de volgende platforms
> * *Windows-Cluster*: Windows 8 en hoger of WindowsServer 2012 en hoger
> * *Linux-Cluster*: Omgekeerde Proxy is momenteel niet beschikbaar voor Linux-clusters
>

## <a name="reaching-microservices-from-outside-the-cluster"></a>Microservices van buiten het cluster is bereikt
De standaard externe communicatiemodel voor microservices, is een opt-in voor waar elke service kan niet worden geopend rechtstreeks vanuit externe clients. [Azure Load Balancer](../load-balancer/load-balancer-overview.md), dit is een netwerkgrens tussen microservices en externe clients, voert u netwerkadresomzetting en externe stuurt aanvragen door naar interne IP: poort-eindpunten. Als u wilt maken van een microservice-eindpunt rechtstreeks toegankelijk is voor externe clients, moet u eerst de Load Balancer voor het doorsturen van verkeer naar elke poort die de service in het cluster gebruikt configureren. De meeste microservices, met name stateful microservices, live geen bovendien op alle knooppunten van het cluster. De microservices kunt verplaatsen tussen knooppunten bij failover. In dergelijke gevallen kan geen Load Balancer effectief te bepalen de locatie van het doelknooppunt van de replica's waaraan moet het doorsturen van verkeer.

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a>Microservices via de omgekeerde proxy van buiten het cluster is bereikt
In plaats van het configureren van de poort van een afzonderlijke service in de Load Balancer, kunt u alleen de poort van de omgekeerde proxy configureren in de Load Balancer. Deze configuratie kiest, kunnen clients buiten het cluster services binnen het cluster bereiken met behulp van de omgekeerde proxy zonder aanvullende configuratie.

![Externe communicatie][0]

> [!WARNING]
> Wanneer u de omgekeerde proxy poort in de Load Balancer configureert, kunnen alle microservices in het cluster die beschikbaar maken van een HTTP-eindpunt worden opgevraagd van buiten het cluster. Dit betekent dat de microservices is bedoeld voor intern mogelijk kunnen worden gedetecteerd door een bepaald kwaadwillende gebruiker. Dit geeft kunnen ernstige problemen die kunnen worden misbruikt; bijvoorbeeld:
>
> * Een kwaadwillende gebruiker mogelijk een denial of service-aanval starten door een interne service beschikt niet over een voldoende beperkte kwetsbaarheid voor aanvallen herhaaldelijk aan te roepen.
> * Een kwaadwillende gebruiker mogelijk onjuist ingedeelde pakketten leveren aan een interne service, wat resulteert in onbedoeld gedrag.
> * Een service die is bedoeld voor intern opleveren persoonlijke of gevoelige gegevens niet zijn bedoeld om te worden blootgesteld aan services buiten het cluster, dus wanneer deze gevoelige gegevens naar een kwaadwillende gebruiker beschikbaar komen. 
>
> Zorg ervoor dat u volledig begrijpen en oplossen van de mogelijke gevolgen voor de beveiliging voor uw cluster en de apps die zijn uitgevoerd, voordat u de omgekeerde-proxypoort openbaar maken. 
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a>URI-indeling voor het adresseren van services met behulp van de omgekeerde proxy
De omgekeerde proxy maakt gebruik van een specifieke uniform resource identifier (URI)-indeling voor het identificeren van de servicepartitie waarnaar de binnenkomende aanvraag moet worden doorgestuurd:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **http(s):** De omgekeerde proxy kan worden geconfigureerd voor het accepteren van HTTP of HTTPS-verkeer. Raadpleeg voor het doorsturen van HTTPS, [verbinding maken met een beveiligd service met de omgekeerde proxy](service-fabric-reverseproxy-configure-secure-communication.md) beschikt u over omgekeerde proxy-instellingen om te luisteren op HTTPS.
* **Cluster volledig gekwalificeerde domeinnaam (FQDN) | intern IP:** U kunt de omgekeerde proxy voor externe clients configureren zodat deze bereikbaar is via de clusterdomein, zoals mycluster.eastus.cloudapp.azure.com. De omgekeerde proxy wordt standaard uitgevoerd op elk knooppunt. Voor interne verkeer, kan de omgekeerde proxy worden bereikt op localhost of op elk knooppunt van interne IP-, bijvoorbeeld 10.0.0.1.
* **Poort:** Dit is de poort, zoals 19081, die is opgegeven voor de omgekeerde proxy.
* **ServiceInstanceName:** Dit is de volledig gekwalificeerde naam van de geïmplementeerde service-exemplaar dat u probeert te bereiken zonder de ' fabric: / "schema. Bijvoorbeeld, om te bereiken het *fabric: / Mijntoep/MijnService/* -service, gebruikt u *Mijntoep/MijnService*.

    De naam van de service-exemplaar is hoofdlettergevoelig. Gebruik een ander hoofdlettergebruik voor de naam van de service-exemplaar in de URL zorgt ervoor dat de aanvragen mislukt met 404 (niet gevonden).
* **Pad naar het achtervoegsel:** Dit is het werkelijke URL-pad, zoals *myapi/waarden/toevoegen/3*, voor de service waarmee u verbinding wilt maken.
* **PartitionKey:** Dit is de berekende partitiesleutel van de partitie die u wilt bereiken voor een gepartitioneerde service. Houd er rekening mee dat dit *niet* de partitie-ID GUID. Deze parameter is niet vereist voor services die gebruikmaken van het partitieschema singleton.
* **PartitionKind:** Dit is het partitieschema van de service. Dit kan zijn 'Int64Range' of 'Met de naam'. Deze parameter is niet vereist voor services die gebruikmaken van het partitieschema singleton.
* **ListenerName** de eindpunten van de service van het formulier zijn {"Eindpunten": {"Listener1": "1", "Listener2": "Endpoint2"...}}. Wanneer de service wordt aangegeven dat meerdere eindpunten, maar Hiermee wordt aangegeven het eindpunt dat de aanvraag van de client moet worden doorgestuurd. Dit kan worden genegeerd als de service slechts één listener heeft.
* **TargetReplicaSelector** Hiermee wordt aangegeven hoe de doelreplica of het exemplaar moet worden geselecteerd.
  * Wanneer de doelservice stateful is, kan de TargetReplicaSelector een van de volgende zijn:  'PrimaryReplica', 'RandomSecondaryReplica' of 'RandomReplica'. Als deze parameter niet is opgegeven, is de standaardwaarde 'PrimaryReplica'.
  * Wanneer de doelservice stateless is, kiest de omgekeerde proxy een willekeurig exemplaar van de servicepartitie voor het doorsturen van de aanvraag voor het.
* **Time-out voor:**  Hiermee geeft u de time-out voor de HTTP-aanvraag die zijn gemaakt door de omgekeerde proxy voor de service namens de clientaanvraag. De standaardwaarde is 60 seconden. Dit is een optionele parameter.

### <a name="example-usage"></a>Voorbeeld van gebruik
Als u bijvoorbeeld eerst even de *fabric: / Mijntoep/MijnService* service waarmee een HTTP-listener op de volgende URL:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Hieronder vindt u de resources voor de service:

* `/index.html`
* `/api/users/<userId>`

Als de service gebruikt de singleton partitieschema, de *PartitionKey* en *PartitionKind* queryreeksparameters zijn niet vereist en de service kan worden bereikt met behulp van de gateway als:

* Extern: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Intern: `http://localhost:19081/MyApp/MyService`

Als de service het partitieschema Uniform Int64 gebruikt de *PartitionKey* en *PartitionKind* queryreeksparameters moeten worden gebruikt voor het bereiken van een partitie van de service:

* Extern: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Intern: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Als u wilt de resources die wordt aangegeven dat de service is bereikt, plaatst u het resourcepad nadat de naam van de service in de URL:

* Extern: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Intern: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

De gateway wordt deze aanvragen vervolgens doorsturen naar de URL van de service:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Speciale handelingen voor het delen van de poort services
De Service Fabric reverse proxy probeert opnieuw omzetten van een serviceadres en de aanvraag opnieuw uitvoeren wanneer een service kan niet worden bereikt. Over het algemeen als een service kan niet worden bereikt, is de service-exemplaar of de replica verplaatst naar een ander knooppunt als onderdeel van de normale levenscyclus. Als dit gebeurt, kan de omgekeerde proxy een netwerkfout verbinding waarmee wordt aangegeven dat een eindpunt niet meer geopend op het oorspronkelijk omgezette adres is ontvangt.

Echter, replica's of service-exemplaren kunnen delen met een hostproces en mogelijk ook een poort wanneer deze wordt gehost door een http.sys gebaseerde webserver, delen met inbegrip van:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In dit geval is het waarschijnlijk dat de webserver is beschikbaar in het hostproces en reageren op aanvragen, maar de opgelost service-exemplaar of de replica niet meer beschikbaar op de host is. In dit geval wordt de gateway een HTTP 404-antwoord ontvangen van de webserver. Een HTTP 404-respons kan dus twee verschillende betekenis hebben:

- Geval #1: Het serviceadres juist is, maar de resource die de gebruiker heeft aangegeven bestaat niet.
- Geval #2: Het serviceadres is onjuist en de resource die de gebruiker heeft aangegeven bestaat mogelijk op een ander knooppunt.

Het eerste geval is een normale HTTP 404-fout, die wordt beschouwd als een gebruikersfout opgetreden. De gebruiker heeft echter in het tweede geval verzocht een resource die bestaat. De omgekeerde proxy is niet vinden omdat de service zelf is verplaatst. De omgekeerde proxy moet het adres opnieuw omzetten en de aanvraag opnieuw.

De omgekeerde proxy moet daarom een methode onderscheid maken tussen deze twee gevallen. Als u dit onderscheid, is een hint van de server vereist.

* Standaard wordt de omgekeerde proxy wordt ervan uitgegaan dat het geval #2 en wil oplossen en de aanvraag opnieuw.
* Om aan te geven geval #1 naar de omgekeerde proxy, moet de service de volgende HTTP-antwoordheader geretourneerd:

  `X-ServiceFabric : ResourceNotFound`

Deze HTTP-antwoordheader geeft aan dat een normale HTTP 404-situatie waarin de aangevraagde resource bestaat niet en de omgekeerde proxy niet voor het omzetten van de serviceadres opnieuw probeert.

## <a name="special-handling-for-services-running-in-containers"></a>Speciale handelingen Services in containers wordt uitgevoerd

Voor services die in containers worden uitgevoerd, kunt u de omgevingsvariabele `Fabric_NodeIPOrFQDN` om samen te stellen de [reverse proxy-URL](#uri-format-for-addressing-services-by-using-the-reverse-proxy) zoals in de volgende code:

```csharp
    var fqdn = Environment.GetEnvironmentVariable("Fabric_NodeIPOrFQDN");
    var serviceUrl = $"http://{fqdn}:19081/DockerSFApp/UserApiContainer";
```
Voor het lokale cluster `Fabric_NodeIPOrFQDN` is standaard ingesteld op 'localhost'. Start het lokale cluster met de `-UseMachineName` parameter om ervoor te zorgen dat containers omgekeerde proxy's die worden uitgevoerd op het knooppunt kunnen bereiken. Zie voor meer informatie, [configureren van uw ontwikkelomgeving instellen om op te sporen containers](service-fabric-how-to-debug-windows-containers.md#configure-your-developer-environment-to-debug-containers).

Service Fabric-services die worden uitgevoerd binnen containers met Docker Compose vereisen een speciale docker-compose.yml *poorten sectie* http: of https: configuratie. Zie voor meer informatie, [ondersteuning voor implementatie van Docker Compose in Azure Service Fabric](service-fabric-docker-compose.md).

## <a name="next-steps"></a>Volgende stappen
* [Instellen en configureren van omgekeerde proxy op een cluster](service-fabric-reverseproxy-setup.md).
* [Doorsturen naar beveiligde HTTP-service met de omgekeerde proxy instellen](service-fabric-reverseproxy-configure-secure-communication.md)
* [Omgekeerde proxy gebeurtenissen onderzoeken](service-fabric-reverse-proxy-diagnostics.md)
* Bekijk een voorbeeld van HTTP-communicatie tussen services in een [voorbeeldproject op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Externe procedureaanroepen met externe communicatie Reliable Services](service-fabric-reliable-services-communication-remoting.md)
* [Web-API die gebruikmaakt van OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-communicatie via Reliable Services](service-fabric-reliable-services-communication-wcf.md)

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
