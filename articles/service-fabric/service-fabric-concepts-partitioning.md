---
title: Partitioneren van Service Fabric-services | Microsoft Docs
description: Beschrijft hoe u voor het partitioneren van stateful Service Fabric-services. Partities kunnen de opslag van gegevens op de lokale machines, zodat gegevens en rekenprocessen samen kunnen worden geschaald.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 9072a25b55bf461ad7dcc8393b98a66d87866d48
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054870"
---
# <a name="partition-service-fabric-reliable-services"></a>Betrouwbare Service Fabric-services partitioneren
In dit artikel bevat een inleiding tot de basisconcepten van Azure Service Fabric reliable services partitioneren. De broncode die wordt gebruikt in het artikel is ook beschikbaar op [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="partitioning"></a>Partitionering
Partitioneren is niet uniek is voor Service Fabric. Het is in feite een patroon core van het bouwen van schaalbare services. In een breder zin, kunnen we denken over het partitioneren van als een concept van het delen van status (gegevens) en compute in kleinere toegankelijk eenheden schaalbaarheid en prestaties te verbeteren. Een bekende vorm van partitioneren is [gegevenspartitionering][wikipartition], ook wel sharding.

### <a name="partition-service-fabric-stateless-services"></a>Stateless Service Fabric-services partitioneren
Voor stateless services, kunt u denken over een partitie wordt een logische eenheid met een of meer exemplaren van een service. Afbeelding 1 ziet een stateless service met vijf exemplaren die zijn verdeeld over een cluster met één partitie.

![Stateless service](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Er zijn in feite twee typen oplossingen stateless service. De eerste is een service die de status extern, bijvoorbeeld in een Azure SQL-database (zoals een website die de sessie-informatie en gegevens worden opgeslagen). De tweede waarde is alleen-berekening services (zoals een miniatuur calculator of afbeelding) die een permanente status niet beheren.

Ofwel in geval een stateless service partitioneren is een heel zeldzaam scenario--schaalbaarheid en beschikbaarheid normaal gesproken worden bereikt door meer instanties toe te voegen. De enige keer dat u wilt uitvoeren van meerdere partities voor stateless service-exemplaren is wanneer u nodig hebt om te voldoen aan de speciale routeren van aanvragen.

Een voorbeeld kunt u een aanvraag waarin gebruikers met de id's in een bepaald bereik moeten alleen worden geleverd door een bepaalde service-exemplaar. Een ander voorbeeld van wanneer u een stateless service kan partitioneren is wanneer u een echt gepartitioneerde back-end (bijvoorbeeld een shard SQL-database) en u wilt bepalen welke service-exemplaar moet schrijven naar de database-shard - of andere taken voorbereiding binnen de stateless service waarvoor u partitionering dezelfde informatie als in de back-end wordt gebruikt. Dergelijke scenario's kunnen ook op verschillende manieren worden opgelost en niet noodzakelijk service partitioneren.

De rest van dit scenario is gericht op stateful services.

### <a name="partition-service-fabric-stateful-services"></a>Stateful Service Fabric-services partitioneren
Service Fabric kunt eenvoudig schaalbare stateful services ontwikkelen door het aanbieden van een uitstekende manier om partitie status (gegevens). Conceptueel gezien, kunt u zien over een partitie van een stateful service als een schaaleenheid die zeer betrouwbare via [replica's](service-fabric-availability-services.md) die worden gedistribueerd en verdeeld over de knooppunten in een cluster.

In de context van stateful Service Fabric-services partitioneren verwijst naar het proces om te bepalen of de partitie van een bepaalde service verantwoordelijk voor een gedeelte van de volledige status van de service is. (Zoals al eerder vermeld, een partitie is een set [replica's](service-fabric-availability-services.md)). Een mooie over Service Fabric is dat de partities op verschillende knooppunten geplaatst. Hierdoor kunnen ze uitbreiden tot een knooppuntlimiet resource. Als de gegevens groeien behoeften, partities groeien en Service Fabric rebalances partities over knooppunten. Dit zorgt ervoor blijven efficiënt gebruik van hardwarebronnen.

Als u wilt geven u een voorbeeld, dat u begint met een cluster met 5-knooppunten en een service die is geconfigureerd met 10 partities en een doel van drie replica's. In dit geval, Service Fabric in balans brengen en de replica's verdelen over de cluster, en u zou uiteindelijk met twee primaire [replica's](service-fabric-availability-services.md) per knooppunt.
Als u nu moet uit het cluster met 10 knooppunten te schalen, Service Fabric de primaire zou herverdelen [replica's](service-fabric-availability-services.md) voor alle 10 knooppunten. Op dezelfde manier als u geschaald naar 5 knooppunten, Service Fabric zou opnieuw verdelen alle replica's op de 5 knooppunten.  

Afbeelding 2 toont de distributie van 10 partities voor en na het schalen van het cluster.

![Stateful service](./media/service-fabric-concepts-partitioning/partitions.png)

De scale-out wordt als gevolg hiervan bereikt omdat aanvragen van clients op computers worden gedistribueerd, algehele prestaties van de toepassing is verbeterd en conflicten op toegang tot delen van gegevens wordt verminderd.

## <a name="plan-for-partitioning"></a>Plan voor het partitioneren van
Voordat u een service implementeert, moet u altijd rekening houden met de strategie voor partitioneren die nodig is om uit te schalen. Er zijn verschillende manieren, maar ze allemaal zich richten op wat de toepassing nodig heeft om te realiseren. Voor de context van dit artikel laten we eens enkele van de belangrijkste aspecten.

Er is een goede aanpak om na te denken over de structuur van de status die moet worden gepartitioneerd als de eerste stap.

We nemen een eenvoudig voorbeeld. Als u een service voor een countywide poll bouwt, kunt u een partitie voor elke plaats kunt maken in de regio. Vervolgens kunt u de stemmen voor elke persoon opslaan in de plaats in de partitie die overeenkomt met die plaats. Afbeelding 3 ziet u een set personen en de plaats waarin ze zich bevinden.

![Eenvoudige partitie](./media/service-fabric-concepts-partitioning/cities.png)

Als de bevolking van steden aanzienlijk varieert, mag u uiteindelijk met het aantal partities die een grote hoeveelheid gegevens (bijvoorbeeld Amsterdam) bevatten en andere partities met weinig status (bijvoorbeeld Kirkland). Wat is de impact van het hebben van partities met ongelijke hoeveelheden status?

Als u het voorbeeld opnieuw vindt, kunt u eenvoudig zien dat de partitie die de stemmen voor Seattle bevat meer verkeer dan de Kirkland een krijgt. Standaard zorgt Service Fabric ervoor dat er over hetzelfde aantal primaire en secundaire replica's op elk knooppunt. Zo zou u uiteindelijk met knooppunten die replica's die u beschikbaar maakt meer verkeer en anderen die minder verkeer dienen bevatten. U moet bij voorkeur om te voorkomen dat hot en cold punten als volgt in een cluster.

Om te voorkomen dat dit, moet u twee dingen doen vanuit een partitioneren oogpunt:

* Probeer voor het partitioneren van de status, zodat deze wordt evenredig verdeeld over alle partities.
* Rapporteren over de belasting van elk van de replica's voor de service. (Voor meer informatie over Lees dit artikel op [metrische gegevens en belasting](service-fabric-cluster-resource-manager-metrics.md)). Service Fabric biedt de mogelijkheid om te rapporteren over de belasting die worden gebruikt door services, zoals de hoeveelheid geheugen of het aantal records. Op basis van de metrische gegevens gerapporteerd, detecteert Service Fabric dat aantal partities intensiever dan andere fungeren en het cluster rebalances door te verplaatsen van replica's naar meer geschikt knooppunten, zodat algemene kan geen knooppunt is overbelast.

Soms weten niet u hoeveel gegevens worden weergegeven in een bepaalde partitie. Daarom een algemene aanbeveling is om beide--eerst verspreidt door een strategie voor partitionering die de gegevens gelijkmatig over de partities en de seconde, door reporting laden.  De eerste methode voorkomt situaties die worden beschreven in het voorbeeld stemmende terwijl de tweede helpt om tijdelijke verschillen in access of laden na verloop van tijd.

Een ander aspect van de planning van de partitie is begint met het juiste aantal partities kiezen.
Vanuit het oogpunt van het Service Fabric is er niets waarmee wordt voorkomen u begint met een hoger aantal partities dat dan verwacht voor uw scenario.
Ervan uitgaande dat het maximum aantal partities is in feite een geldige benadering.

In zeldzame gevallen, zou u uiteindelijk meer partities dan u in eerste instantie hebt hoeven. Zoals u kunt het aantal partities niet nadat het feit wijzigen, moet u enkele geavanceerde partitie benaderingen, zoals het maken van een nieuwe service-exemplaar van hetzelfde type van de service van toepassing. U moet ook om bepaalde client-side-logica die de aanvragen naar de juiste service-exemplaar, op basis van de client-side-kennis die u ervoor dat uw clientcode zorgen moet te implementeren.

Een andere overweging voor het partitioneren van de planning is de beschikbare computerbronnen. Als de status moet worden geopend of opgeslagen, wordt u afhankelijk te volgen:

* Netwerk-bandbreedtelimieten
* Geheugenlimieten System
* Maximale schijf-opslag

Wat gebeurt dus er als u resourcebeperkingen in een actief cluster? Het antwoord is dat u kunt gewoon uit het cluster om aan de nieuwe vereisten te schalen.

[De handleiding voor capaciteitsplanning](service-fabric-capacity-planning.md) biedt richtlijnen voor hoe u bepaalt hoeveel knooppunten nodig voor het cluster.

## <a name="get-started-with-partitioning"></a>Aan de slag met partitioneren
In deze sectie wordt beschreven hoe u aan de slag met het partitioneren van uw service.

Service Fabric biedt een keuze uit drie partitieschema:

* Variabele partitioneren (ook wel UniformInt64Partition genoemd).
* Met de naam partitioneren. Toepassingen die gebruikmaken van dit model meestal zijn gegevens die kunnen worden bucketed, binnen een begrensde set. Enkele algemene voorbeelden van velden gebruikt als benoemde partitiesleutels zou zijn regio's, postcodes, klantgroepen of andere zakelijke grenzen.
* Singleton-partities. Singleton-partities worden meestal gebruikt wanneer de service geen eventuele aanvullende routering vereist. Bijvoorbeeld, stateless services dit partitieschema standaard gebruikt.

Met de naam en de partitioneringsschema's Singleton zijn speciale soorten variabele partities. Standaard de Visual Studio-sjablonen voor gebruik van Service Fabric variabele partitionering, zoals dit de meestgebruikte en nuttig is. De rest van dit artikel richt zich op het ranged partitieschema.

### <a name="ranged-partitioning-scheme"></a>Variabele partitieschema
Dit wordt gebruikt om op te geven van een bereik gehele getallen (aangeduid met een lage en hoge sleutel) en een aantal partities (n). N partities, elk die verantwoordelijk is voor een niet-overlappende subbereik van de algehele partitiesleutelbereik wordt gemaakt. Bijvoorbeeld, een ranged partitieschema met een lage sleutel van 0, een hoge sleutel 99 en een telling van 4 maakt vier partities, zoals hieronder wordt weergegeven.

![Bereik partitioneren](./media/service-fabric-concepts-partitioning/range-partitioning.png)

Er is een veelgebruikte manier om een hash op basis van een unieke sleutel in de gegevensset te maken. Enkele algemene voorbeelden van sleutels is een vehicle id-nummer (VIN), een werknemer-ID of een unieke tekenreeks. Met behulp van deze unieke sleutel, zou u vervolgens een hash-code, modulus de sleutelbereik, om te gebruiken als uw sleutel te genereren. U kunt de boven- en ondergrenzen van het toegestane bereik van de sleutel opgeven.

### <a name="select-a-hash-algorithm"></a>Selecteer een hash-algoritme
Een belangrijk onderdeel van het hash-wordt de hash-algoritme selecteren. Overweging is of het doel is om vergelijkbare sleutels dicht bij elkaar (plaats gevoelige hashing), groep of als activiteit moet globaal worden verdeeld over alle partities (hash-distributie), dat is het gebruikelijker.

De kenmerken van een goede distributie hash-algoritme zijn dat het is gemakkelijk om te berekenen, het aantal conflicten is en deze de sleutels gelijkmatig verdeelt. Een goed voorbeeld van een doeltreffende hash-algoritme is de [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash-algoritme.

Een goede bron voor algemene hash-code algoritme opties is het [Wikipedia-pagina op hash-functies](http://en.wikipedia.org/wiki/Hash_function).

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Een stateful service met meerdere partities bouwen
Uw eerste betrouwbare stateful service maken met meerdere partities. In dit voorbeeld maakt u een heel eenvoudige toepassing waar u voor het opslaan van alle laatste namen die met de dezelfde letter in dezelfde partitie beginnen.

Voordat u code schrijft, moet u nadenken over de partities en de partitiesleutels. U moet 26 partities (één voor elke letter in het alfabet), maar wat over de lage en hoge sleutels?
Als we letterlijk één partitie per letter hebben willen, kunnen we gebruiken 0 als de lage sleutelwaarde en 25 als hoge sleutel, omdat elke letter een eigen sleutel is.

> [!NOTE]
> Dit is een vereenvoudigde scenario, zoals in feite de distributie ongelijke zou zijn. Laatste namen die beginnen met de letter "S" of "M-zijn vaker voor dan degene die beginnen met 'X' of 'Y'.
> 
> 

1. Open **Visual Studio** > **bestand** > **nieuwe** > **Project**.
2. In de **nieuw Project** dialoogvenster vak, kiest u de Service Fabric-toepassing.
3. Het project "AlphabetPartitions" aanroepen.
4. In de **maken van een Service** dialoogvenster vak, kiest u **Stateful** service en noem deze 'Alphabet.Processing' zoals wordt weergegeven in de onderstaande afbeelding.
       ![Dialoogvenster voor nieuwe service in Visual Studio][1]

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. Stel het aantal partities. Open het bestand Applicationmanifest.xml zich in de map ApplicationPackageRoot van het project AlphabetPartitions en bijwerken van de parameter Processing_PartitionCount tot en met 26, zoals hieronder wordt weergegeven.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    U moet ook de LowKey en HighKey eigenschappen van het element StatefulService in de ApplicationManifest.xml zoals hieronder wordt weergegeven bij te werken.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. Voor de service toegankelijk is, opent u een eindpunt op een andere poort door toe te voegen van de eindpuntelement van ServiceManifest.xml (te vinden in de map PackageRoot) voor de service Alphabet.Processing zoals hieronder wordt weergegeven:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    De service is nu geconfigureerd om te luisteren naar een intern eindpunt met 26 partities.
7. Vervolgens moet u voor de onderdrukking de `CreateServiceReplicaListeners()` methode van de klasse verwerking.
   
   > [!NOTE]
   > Voor dit voorbeeld veronderstellen we dat u van een eenvoudige HttpCommunicationListener gebruikmaakt. Zie voor meer informatie over betrouwbare servicecommunicatie [de betrouwbare Service berichtenmodel](service-fabric-reliable-services-communication.md).
   > 
   > 
8. Een aanbevolen patroon voor de URL die een replica luistert op de volgende indeling is: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Zo wilt u uw communicatielistener om te luisteren op de juiste eindpunten en met dit patroon configureren.
   
    Meerdere replica's van deze service kunnen worden gehost op dezelfde computer, dus dit adres moet uniek zijn voor de replica. Dit is de reden waarom de partitie-ID en replica-ID in de URL zijn. HttpListener kan op meerdere adressen op dezelfde poort luistert, zolang het URL-voorvoegsel uniek is.
   
    De extra GUID is er voor een geavanceerde geval waarbij secundaire replica's ook naar aanvragen voor alleen-lezen luisteren. Als dit het geval is, wilt u om ervoor te zorgen dat een nieuw uniek adres wordt gebruikt bij het overstappen van primaire naar secundaire om af te dwingen van clients op het adres opnieuw omzetten. '+' wordt gebruikt als het adres hier zodat de replica luistert op alle beschikbare hosts (IP, FQDM, ' localhost ', enz.) De volgende code toont een voorbeeld.
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    Het is ook het vermelden waard dat de gepubliceerde URL enigszins af van het luisteren naar URL-voorvoegsel wijkt.
    De URL van de luisterende is besteed aan HttpListener. De gepubliceerde URL is de URL die is gepubliceerd naar de Service Fabric Naming-Service, die wordt gebruikt voor de servicedetectie. Clients vragen voor dit adres via deze service voor de detectie. Het adres dat clients ophalen moet de werkelijke IP of FQDN-naam van het knooppunt om verbinding te kunnen hebben. Daarom moet u vervangen '+' van het knooppunt IP of FQDN-naam zoals hierboven.
9. De laatste stap is het toevoegen van de verwerkingslogica voor naar de service, zoals hieronder weergegeven.
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    `ProcessInternalRequest` leest de waarden van de queryreeks-parameter gebruikt voor het aanroepen van de partitie en aanroepen `AddUserAsync` achternaam toevoegen aan de betrouwbare woordenlijst `dictionary`.
10. Laten we een stateless service toevoegen aan het project om te zien hoe u een bepaalde partitie kunt aanroepen.
    
    Deze service fungeert als een eenvoudige web-interface die de achternaam als een queryreeks-parameter accepteert, bepaalt de partitiesleutel en verzendt dit naar de service Alphabet.Processing voor verwerking.
11. In de **maken van een Service** dialoogvenster vak, kiest u **Stateless** service en noem deze 'Alphabet.Web', zoals hieronder weergegeven.
    
    ![Schermafbeelding van de stateless service](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. Werk de eindpunt-informatie in de ServiceManifest.xml van de service Alphabet.WebApi openen van een poort, zoals hieronder weergegeven.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. U moet een verzameling van ServiceInstanceListeners retourneren in de klasse Web. U kunt opnieuw implementeren van een eenvoudige HttpCommunicationListener.
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is the node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. U moet nu de verwerkingslogica implementeren. Het aanroepen van de HttpCommunicationListener `ProcessInputRequest` wanneer een aanvraag binnenkomt. Dus gaan we en voeg de volgende code toe.
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from the first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added to Partition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    Laten we eens via deze stap voor stap. De code leest de eerste letter van de queryreeks-parameter `lastname` in een char. Vervolgens de partitiesleutel voor deze letter wordt bepaald door af te trekken van de hexadecimale waarde van `A` van de hexadecimale waarde van de eerste letter van de laatste namen.
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Let op: in dit voorbeeld gebruiken we 26 partities met één partitiesleutel per partitie.
    Vervolgens wordt de servicepartitie verkrijgen `partition` voor deze sleutel met behulp van de `ResolveAsync` methode voor het `servicePartitionResolver` object. `servicePartitionResolver` is gedefinieerd als
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    De `ResolveAsync` methode wordt de service-URI, de partitiesleutel en een annulering als parameters token. De service-URI voor de service voor het verwerken is `fabric:/AlphabetPartitions/Processing`. Vervolgens wordt het eindpunt van de partitie ophalen.
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Ten slotte we de eindpunt-URL plus de querytekenreeks ontwikkelen en aanroepen van de service voor het verwerken.
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    Zodra de verwerking is voltooid, wordt de uitvoer terug te schrijven.
15. De laatste stap is om de service te testen. Visual Studio maakt gebruik van parameters voor de toepassing voor lokale en cloudimplementatie. Als u wilt de service met 26 partities lokaal testen, moet u bijwerken de `Local.xml` bestand in de map ApplicationParameters van het project AlphabetPartitions zoals hieronder wordt weergegeven:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Wanneer u klaar bent met implementatie, kunt u de service en alle van de partities in de Service Fabric Explorer controleren.
    
    ![Schermafbeelding van de service Fabric Explorer](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. In een browser, kunt u de logica voor partitionering testen door in te voeren `http://localhost:8081/?lastname=somename`. U ziet dat elke achternaam op die met de dezelfde letter begint worden opgeslagen in dezelfde partitie.
    
    ![Schermafbeelding van browser](./media/service-fabric-concepts-partitioning/samplerunning.png)

De volledige broncode van het voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="reliable-services-and-actor-forking-subprocesses"></a>Betrouwbare Services en Actor subprocessen vertakking
Service Fabric biedt geen ondersteuning voor betrouwbare services en later reliable actors subprocessen vertakken. Een voorbeeld van waarom niet wordt ondersteund is [CodePackageActivationContext](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.codepackageactivationcontext?view=azure-dotnet) kan niet worden gebruikt voor het registreren van een niet-ondersteunde subproces en annulering tokens worden alleen verzonden voor geregistreerde processen, wat resulteert in allerlei problemen, zoals upgrade mislukt, wanneer subprocessen niet sluit nadat het bovenliggende proces-token van een annulering heeft ontvangen. 

## <a name="next-steps"></a>Volgende stappen
Voor informatie over Service Fabric-concepten, Zie de volgende:

* [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
* [Schaalbaarheid van Service Fabric-services](service-fabric-concepts-scalability.md)
* [Capaciteitsplanning voor Service Fabric-toepassingen](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
