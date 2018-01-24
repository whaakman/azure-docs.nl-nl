---
title: Maken van een webfront-end voor uw Azure Service Fabric-app met behulp van ASP.NET Core | Microsoft Docs
description: Uw Service Fabric-toepassing op het web weergeven met behulp van een ASP.NET Core project en service-interne communicatie via de Service voor externe toegang.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: d4f78c63117e5c54eb855178c75d6c294957f2a1
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Een service webfront-end voor uw toepassing met behulp van ASP.NET Core bouwen
Azure Service Fabric-services bieden standaard geen een openbare interface op het web. Om de functionaliteit van uw toepassing HTTP-clients weergeven, moet u een webproject maken om te fungeren als een toegangspunt en vervolgens communiceren van daaruit naar uw afzonderlijke services.

In deze zelfstudie gaan we waar we werd afgebroken de [maken van uw eerste toepassing in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) zelfstudie en een ASP.NET Core webservice voor de teller stateful service toevoegen. Als u dit nog niet hebt gedaan, moet u teruggaan en deze zelfstudie eerst doorlopen.

## <a name="set-up-your-environment-for-aspnet-core"></a>Instellen van uw omgeving voor ASP.NET Core

Moet u Visual Studio 2017 volgen samen met deze zelfstudie. Elke editie wordt gedaan. 

 - [Install Visual Studio 2017](https://www.visualstudio.com/)

Om ASP.NET Core Service Fabric-toepassingen te ontwikkelen, hebt u de volgende werkbelastingen geïnstalleerd:
 - **Ontwikkelen van Azure** (onder *Web- en Cloud*)
 - **ASP.NET- en web ontwikkeling** (onder *Web- en Cloud*)
 - **.NET core platformoverschrijdende ontwikkeling** (onder *andere Toolsets*)

>[!Note] 
>De .NET Core hulpprogramma's voor Visual Studio 2015 zijn niet langer wordt bijgewerkt, maar als u nog steeds Visual Studio 2015 gebruikt, moet u [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) geïnstalleerd.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Een ASP.NET Core-service aan uw toepassing toevoegen
ASP.NET Core is een lichtgewicht, platformoverschrijdende ontwikkeling webframework die u gebruiken kunt voor het maken van moderne webgebruikersinterface en web-API's. 

We gaan een ASP.NET Web API-project toevoegen aan onze bestaande toepassing.

1. Klik in Solution Explorer met de rechtermuisknop op **Services** in de toepassing project en kies **toevoegen > nieuwe Service Fabric-Service**.
   
    ![Een nieuwe service toe te voegen aan een bestaande toepassing][vs-add-new-service]
2. Op de **maken van een Service** pagina **ASP.NET Core** en een naam geven.
   
    ![ASP.NET-webservice te kiezen in het dialoogvenster voor nieuwe service][vs-new-service-dialog]

3. De volgende pagina bevat een set van ASP.NET Core projectsjablonen. Houd er rekening mee dat ze dezelfde opties die u zou zien als u een project ASP.NET Core buiten een Service Fabric-toepassing hebt gemaakt met een kleine hoeveelheid aanvullende code voor het registreren van de service met de Service Fabric-runtime zijn. Voor deze zelfstudie kiest **Web API**. U kunt echter dezelfde concepten toepassen voor het ontwikkelen van een volledige webtoepassing.
   
    ![ASP.NET-projecttype kiezen][vs-new-aspnet-project-dialog]
   
    Zodra uw Web API-project is gemaakt, hebt u twee services in uw toepassing. Als u doorgaat met uw toepassing bouwen, kunt u meer services toevoegen op exact dezelfde manier. Elk zijn onafhankelijk samengestelde en bijgewerkte.

## <a name="run-the-application"></a>De toepassing uitvoeren
Om een beeld krijgt van wat we hebt gedaan, gaan we de nieuwe toepassing implementeren en bekijk het standaardgedrag dat de sjabloon ASP.NET Core Web-API biedt.

1. Druk op F5 in Visual Studio om de app voor foutopsporing.
2. Wanneer de implementatie is voltooid, Start Visual Studio een browser naar de hoofdmap van de ASP.NET Web API-service. De sjabloon ASP.NET Core Web-API biedt niet standaardgedrag voor de basis-, zodat u een 404-fout in de browser ziet.
3. Voeg `/api/values` naar de locatie in de browser. Hiermee wordt de `Get` methode op de ValuesController in de Web-API-sjabloon. Deze retourneert de standaardreactie die wordt geleverd door de sjabloon--een JSON-matrix die twee tekenreeksen bevat:
   
    ![Standaardwaarden geretourneerd van ASP.NET Core Web API-sjabloon][browser-aspnet-template-values]
   
    Deze pagina wordt de meest recente itemwaarde van onze stateful service in plaats van de standaardtekenreeksen weergegeven aan het einde van de zelfstudie.

## <a name="connect-the-services"></a>Verbinding maken met de services
Service Fabric biedt volledige flexibiliteit in hoe u met reliable services communiceren. Binnen één toepassing hebt u mogelijk services die toegankelijk zijn via TCP, andere services die toegankelijk via een HTTP REST-API zijn en nog andere services die toegankelijk via het websockets zijn. Zie voor achtergrondinformatie over de beschikbare opties en de wisselwerking betrokken [services communiceert](service-fabric-connect-and-communicate-with-services.md). In deze zelfstudie gebruiken we [Service Fabric-Service voor externe toegang](service-fabric-reliable-services-communication-remoting.md), opgegeven in de SDK.

In de Service voor externe toegang-benadering (gemodelleerd op externe procedureaanroepen of RPC's), definieert u een interface om te fungeren als de openbare contract voor de service. Vervolgens gebruikt u deze interface voor het genereren van een proxy-klasse voor interactie met de service.

### <a name="create-the-remoting-interface"></a>De interface voor externe toegang maken
Begint met het maken van de interface om te fungeren als de overeenkomst tussen de stateful service en andere services, in dit geval de kern van het ASP.NET web project. Deze interface moet worden gedeeld door alle services die gebruikmaken van deze RPC-aanroepen, maken, zodat deze in een eigen Class Library-project maakt.

1. Klik in Solution Explorer met de rechtermuisknop op uw oplossing en kies **toevoegen** > **nieuw Project**.

2. Kies de **Visual C#** vermelding in het navigatiedeelvenster links en selecteer vervolgens de **Class Library** sjabloon. Zorg ervoor dat de .NET Framework-versie is ingesteld op **4.5.2**.
   
    ![Een project interface voor uw stateful service maken][vs-add-class-library-project]

3. Installeer de **Microsoft.ServiceFabric.Services.Remoting** NuGet-pakket. Zoeken naar **Microsoft.ServiceFabric.Services.Remoting** manager in het NuGet-pakket en installeer het voor alle projecten in de oplossing die gebruikmaken van de Service voor externe toegang, waaronder:
   - Het Class Library-project met de service-interface
   - De Stateful Service-project
   - Het ASP.NET Core web service-project
   
    ![Toevoegen van de Services NuGet-pakket][vs-services-nuget-package]

4. Maak in de class-bibliotheek een interface met een enkele methode `GetCountAsync`, en uitbreiden van de interface van `Microsoft.ServiceFabric.Services.Remoting.IService`. De externe communicatie-interface moet worden afgeleid van deze interface om aan te geven dat het een Service voor externe toegang-interface is.
   
    ```csharp
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-the-interface-in-your-stateful-service"></a>Implementeer de interface in uw stateful service
Nu we de interface hebt gedefinieerd, moet we het in de stateful service implementeren.

1. Voeg een verwijzing naar de klasse library-project met de interface in uw stateful service.
   
    ![Een verwijzing naar de klasse library-project toevoegen in de stateful service][vs-add-class-library-reference]
2. Zoek de klasse die eigenschappen van overneemt `StatefulService`, zoals `MyStatefulService`, en deze uitbreiden voor het implementeren van de `ICounter` interface.
   
    ```csharp
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. Nu geïmplementeerd de één methode die is gedefinieerd in de `ICounter` interface, `GetCountAsync`.
   
    ```csharp
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>De stateful service met behulp van een service voor externe toegang-listener blootstelt
Met de `ICounter` -interface geïmplementeerd, de laatste stap is het communicatiekanaal Remoting Service openen. Voor stateful services, Service Fabric bevat een overschrijfbare methode aangeroepen `CreateServiceReplicaListeners`. Met deze methode kunt u een of meer communicatielisteners, op basis van het type communicatie die u wilt inschakelen voor uw service.

> [!NOTE]
> De equivalente methode voor het openen van een communicatiekanaal naar stateless services wordt aangeroepen `CreateServiceInstanceListeners`.

In dit geval wordt de bestaande vervangen `CreateServiceReplicaListeners` methode en geef een exemplaar van `ServiceRemotingListener`, die een RPC-eindpunt maakt die kan worden aangeroepen van clients via `ServiceProxy`.  

De `CreateServiceRemotingListener` extensiemethode op de `IService` interface kunt u gemakkelijk maken een `ServiceRemotingListener` met alle standaardinstellingen. Deze extensiemethode gebruikt, zorg ervoor dat u hebt de `Microsoft.ServiceFabric.Services.Remoting.Runtime` naamruimte geïmporteerd. 

```csharp
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>De klasse ServiceProxy gebruiken om te communiceren met de service
Onze stateful service is nu gereed voor het ontvangen verkeer van andere services via RPC. Blijft toegevoegd zodat de code van de ASP.NET-webservice te communiceren.

1. Voeg in uw ASP.NET-project een verwijzing naar de class-bibliotheek bevat de `ICounter` interface.

2. Eerder u toegevoegd de **Microsoft.ServiceFabric.Services.Remoting** NuGet-pakket aan de ASP.NET-project. Dit pakket biedt de `ServiceProxy` klasse die u met RPC aanroepen naar de stateful service. Zorg ervoor dat deze NuGet-pakket is geïnstalleerd in het project ASP.NET Core web service.

4. In de **domeincontrollers** map, open de `ValuesController` klasse. Houd er rekening mee dat de `Get` methode wordt momenteel alleen een vastgelegde tekenreeksmatrix van 'waarde1' en 'waarde2'--die overeenkomt met wat we eerder in de browser gezien. Vervang deze implementatie met de volgende code:
   
    ```csharp
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    De eerste coderegel is de sleutel. Voor het maken van de proxy ICounter naar de stateful service, moet u twee soorten informatie opgeven: een partitie-ID en de naam van de service.
   
    U kunt partitioneren op schaal stateful services door te verdelen van hun status in verschillende buckets, op basis van een sleutel die u hebt gedefinieerd, zoals een klant-ID of postcode. In onze trivial toepassing heeft de stateful service slechts één partitie, zodat de sleutel is niet van belang. Een sleutel die u opgeeft, zal leiden tot dezelfde partitie. Zie voor meer informatie over services partitioneren, [hoe partitie Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).
   
    De servicenaam van de is een URI van het formulier fabric: /&lt;$application_name&gt;/&lt;service_name&gt;.
   
    Service Fabric kunt met deze twee soorten informatie, de computer die aanvragen moeten worden verzonden naar een unieke identificatie. De `ServiceProxy` klasse ook naadloos verwerkt wanneer de computer die als host fungeert voor de partitie stateful service mislukt en een andere computer moet worden gepromoveerd tot de plaatsvinden. Deze abstractie maakt de clientcode omgaan met andere services die aanzienlijk eenvoudiger te schrijven.
   
    Wanneer we de proxy hebt, roepen we gewoon de `GetCountAsync` methode en het resultaat te retourneren.

5. Druk op F5 om opnieuw uit te voeren van de gewijzigde toepassing. Als u vóór, Start Visual Studio automatisch de browser naar de hoofdmap van het webproject. Voeg het pad 'api en-waarden' en ziet u de huidige waarde geretourneerd.
   
    ![De stateful waarde weergegeven in de browser][browser-aspnet-counter-value]
   
    Vernieuw de browser regelmatig om te zien van de itemwaarde bijwerken.

## <a name="connecting-to-a-reliable-actor-service"></a>Verbinding maken met een betrouwbare Actor-service
Deze zelfstudie is gericht op het toevoegen van een webfront-end die gecommuniceerd met een stateful service. U kunt echter een vergelijkbaar model contact opnemen met actoren volgen. Wanneer u een betrouwbare Actor-project maakt, wordt in Visual Studio automatisch een interface-project voor u gegenereerd. U kunt die interface gebruiken voor het genereren van een actor-proxy in het webproject om te communiceren met de actor. Het communicatiekanaal is automatisch beschikbaar. Dus u niet hoeft te ondernemen die gelijk is aan het tot stand brengen van een `ServiceRemotingListener` net zoals voor de stateful service in deze zelfstudie.

## <a name="how-web-services-work-on-your-local-cluster"></a>De werking van web-services op uw lokale cluster
In het algemeen kunt u exact dezelfde Service Fabric toepassing naar een cluster met meerdere machines die u hebt geïmplementeerd op uw lokale cluster implementeren en maximaal vertrouwen werkt zoals verwacht. Dit is omdat uw lokale cluster is gewoon een configuratie vijf knooppunten die tot een enkele computer is samengevouwen.

Als het gaat om webservices, maar is er een belangrijke aspect. Als uw cluster zich achter een load balancer, zoals dat in Azure, moet u ervoor zorgen dat uw webservices worden geïmplementeerd op elke computer omdat de load balancer gewoon round robins verkeer op de computers. U kunt dit doen door in te stellen de `InstanceCount` voor de service op de speciale waarde '1'.

Wanneer u een webservice lokaal uitvoert, moet u ervoor zorgen dat slechts één exemplaar van wordt daarentegen is de service uitgevoerd. Anders uitvoeren u in conflicten uit meerdere processen die op het pad en de poort luisteren. Als gevolg hiervan moet het aantal exemplaren van web-service worden ingesteld op '1' voor lokale implementaties.

Zie voor meer informatie over het configureren van verschillende waarden voor verschillende omgeving, [beheren voor omgevingen met meerdere parameters voor de toepassing](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Volgende stappen
Nu dat u een web-front end set hebt voor uw toepassing met ASP.NET Core, meer informatie over [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) voor een diepgaand inzicht in hoe ASP.NET Core met Service Fabric werkt.

Vervolgens [meer informatie over de communicatie met services](service-fabric-connect-and-communicate-with-services.md) afbeelding in het algemeen een complete ophalen van hoe service communicatie werkt in Service Fabric.

Zodra u een goed inzicht hebt in hoe servicecommunicatie werkt, [een cluster in Azure maken en implementeren van uw toepassing naar de cloud](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
