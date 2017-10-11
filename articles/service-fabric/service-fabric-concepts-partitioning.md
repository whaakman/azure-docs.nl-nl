---
title: Service Fabric-services partitioneren | Microsoft Docs
description: Beschrijft hoe Service Fabric stateful services partitie. Partities kunnen de opslag van gegevens op de lokale computers zodat u gegevens en rekencapaciteit samen kunnen worden geschaald.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 3c1e80305cb65f41a6981b99f69e8b87f89599ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="partition-service-fabric-reliable-services"></a>Betrouwbare partitie Service Fabric-services
Dit artikel bevat een inleiding tot de basisconcepten van Azure Service Fabric betrouwbare services partitioneren. De broncode gebruikt in het artikel is ook beschikbaar op [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="partitioning"></a>Partitionering
Partitioneren is niet uniek is voor Service Fabric. Het is in feite een patroon core van het bouwen van schaalbare services. We kunnen in een ruimere zin nadenken over partitioneren als een concept van het delen van status (gegevens) en compute in kleinere toegankelijk eenheden schaalbaarheid en prestaties te verbeteren. Is een bekende formulier van het partitioneren van [partitioneren van gegevens][wikipartition], ook wel aangeduid als sharding.

### <a name="partition-service-fabric-stateless-services"></a>Partitie Service Fabric stateless services
U kunt een partitie wordt een logische eenheid met een of meer exemplaren van een service bedenken voor stateless services. Afbeelding 1 toont een stateless service met vijf exemplaren verdeeld over een cluster met één partitie.

![Staatloze service](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Er zijn in feite twee soorten stateless Services-oplossingen. De eerste is een service die de status extern, bijvoorbeeld zich blijft voordoen in een Azure SQL database (zoals een website die de sessie-informatie en gegevens worden opgeslagen). Het tweede is alleen-berekeningen services (zoals een miniatuur Rekenmachine of afbeelding) die geen permanente status niet beheren.

Ofwel in geval een stateless service partitioneren is een zeldzame scenario--schaalbaarheid en beschikbaarheid normaal worden bereikt door meer exemplaren toe te voegen. De enige keer dat u wilt meerdere partities voor stateless service-exemplaren is als u nodig hebt om te voldoen aan speciale routering van aanvragen.

Een voorbeeld kunt u een aanvraag waarin gebruikers met de id's in een bepaald bereik moeten alleen worden geleverd door een bepaalde service-exemplaar. Een ander voorbeeld van wanneer u een stateless service kan partitioneren is wanneer u een echt gepartitioneerde back-end (bijvoorbeeld een shard SQL-database) en u wilt bepalen welk service-exemplaar moet schrijven naar de database shard-- of andere taken voorbereiding binnen de staatloze service waarvoor partitionering dezelfde informatie als wordt gebruikt in de back-end. Dergelijke scenario's kunnen ook op verschillende manieren worden opgelost en niet noodzakelijk partitioneren van de service.

De rest van dit scenario ligt de nadruk op stateful services.

### <a name="partition-service-fabric-stateful-services"></a>Partitie Service Fabric stateful services
Service Fabric kunt eenvoudig ontwikkelen van schaalbare stateful services door het aanbieden van een uitstekende manier om de partitie-status (gegevens). Conceptueel gezien u over een partitie van een stateful service kunt beschouwen als een schaaleenheid die zeer betrouwbaar via [replica's](service-fabric-availability-services.md) die zijn gedistribueerd en verdeeld zijn over de knooppunten in een cluster.

In de context van de Service Fabric stateful services partitioneren verwijst naar het proces om te bepalen dat een bepaalde service partitie verantwoordelijk voor een deel van de volledige status van de service is. (Zoals al eerder vermeld, een partitie is een reeks [replica's](service-fabric-availability-services.md)). Een aardige van Service Fabric is dat de partities wordt geplaatst op verschillende knooppunten. Hierdoor kunnen ze toe aan een knooppunt resource limiet. Wanneer de gegevens groeien behoeften, groeien partities en Service Fabric rebalances partities over knooppunten. Hierdoor worden de blijvende efficiënt gebruik van de hardwarebronnen.

Als u een voorbeeld geven, dat u begint met een 5-node cluster en een service die is geconfigureerd met 10 partities en een doel van drie replica's. In dit geval Service Fabric zou worden verdeeld en de replica's verdelen over het cluster-- en zou u uiteindelijk eindigen met twee primaire [replica's](service-fabric-availability-services.md) per knooppunt.
Als u nu uitbreiden van het cluster aan 10 knooppunten wilt, de primaire zou opnieuw verdelen Service Fabric [replica's](service-fabric-availability-services.md) op alle knooppunten van 10. Op dezelfde manier als u terug naar 5 knooppunten geschaald, Service Fabric zou opnieuw verdelen alle replica's op de 5-knooppunten.  

Afbeelding 2 toont de distributie van 10 partities voor en na het schalen van het cluster.

![Stateful service](./media/service-fabric-concepts-partitioning/partitions.png)

De scale-out wordt hierdoor bereikt omdat aanvragen van clients worden verdeeld over computers, algehele prestaties van de toepassing is verbeterd en conflicten op toegang tot gegevenssegmenten van wordt verminderd.

## <a name="plan-for-partitioning"></a>Plan voor het partitioneren
Voordat u een service implementeert, moet u altijd de partitionering strategie die vereist is voor het uitbreiden van overwegen. Er zijn verschillende manieren, maar ze allemaal zich richten op wat de toepassing moet bereiken. De context van dit artikel, laten we eens enkele van de belangrijkste aspecten.

Er is een goede aanpak om na te denken over de structuur van de status die moet worden gepartitioneerd als de eerste stap.

U gaat nu een eenvoudig voorbeeld. Als u een service voor een countywide poll bouwen, kunt u een partitie voor elke stad kunt maken in de regio. Vervolgens kunt u de stemmen voor elke persoon opslaan in de plaats in de partitie die overeenkomt met die stad. Afbeelding 3 ziet u een set gebruikers en de plaats waar ze zich bevinden.

![Eenvoudige partitie](./media/service-fabric-concepts-partitioning/cities.png)

Als de populatie van steden varieert, zou u uiteindelijk met een aantal partities met een grote hoeveelheid gegevens (bijvoorbeeld Haarlem) en andere partities met weinig status (bijvoorbeeld Kirkland). Wat is de invloed van de partities met ongelijke hoeveelheden status hebben?

Als u opnieuw over het voorbeeld denkt, kunt u eenvoudig de partitie die de stemmen voor Seattle bevat krijgt meer verkeer dan de Kirkland een bekijken. Service Fabric maakt standaard ervoor dat er over hetzelfde aantal primaire en secundaire replica's op elk knooppunt. Zo zou u uiteindelijk met knooppunten die fungeren als replica's die dienen meer verkeer en anderen die minder verkeer dienen houdt. U wilt bij voorkeur warme en koude plaatsen als volgt in een cluster voorkomen.

Om te voorkomen dat dit, moet u twee dingen doen vanuit het partitionering oogpunt:

* Probeer voor het partitioneren van de status, zodat deze evenredig verdeeld over alle partities.
* Rapporteren over de belasting van elk van de replica's voor de service. (Voor meer informatie over het controleren van dit artikel [metrische gegevens en de belasting](service-fabric-cluster-resource-manager-metrics.md)). Service Fabric biedt de mogelijkheid om te rapporteren over de belasting die door services, zoals de hoeveelheid geheugen of het aantal records worden gebruikt. Op basis van de metrische gegevens gerapporteerd, detecteert Service Fabric dat een aantal partities fungeren hogere belasting dan andere en het cluster rebalances door te verplaatsen van replica's naar geschikte knooppunten, zodat de algemene geen knooppunt is overbelast.

Soms weet u niet kunt hoeveel gegevens worden weergegeven in een bepaalde partitie. Een algemene aanbeveling wordt beide--eerst door partities die selecteert, de gegevens gelijkmatig over de partities en de tweede, met reporting load.  De eerste methode voorkomt situaties beschreven in het voorbeeld stemmende tijdens de tweede helpt vloeiend maken tijdelijke verschillen in access of load gedurende een bepaalde periode.

Een ander aspect van de planning van de partitie is het juiste aantal partities beginnen te kiezen.
Vanuit het perspectief van een Service Fabric is er niets dat verhindert dat u begint met een hoger aantal partities dan verwacht voor uw scenario.
Ervan uitgaande dat het maximum aantal partities is in feite een geldige benadering.

In zeldzame gevallen, zou u uiteindelijk hoeven meer partities dan u oorspronkelijk hebt gekozen. Als u het aantal partities niet nadat het feit wijzigen, zou u moet sommige geavanceerde partitie benaderingen, zoals het maken van een nieuwe service-exemplaar van hetzelfde servicetype toepassen. U moet ook bepaalde client-side '-logica waarmee de aanvragen worden doorgestuurd naar de juiste service-exemplaar, gebaseerd op kennis van clientzijde die u ervoor dat uw clientcode zorgen moet implementeren.

Andere overweging voor het partitioneren van de planning is de beschikbare computerbronnen. Als de status moet worden geopend en opgeslagen, wordt u gebonden te volgen:

* Netwerk bandbreedtelimieten
* Systeem geheugenlimieten
* Schijf opslaglimieten

Wat gebeurt zodat er als u in resourcebeperkingen in een actief cluster uitvoert? Het antwoord is dat u gewoon kunt schalen uit het cluster om de nieuwe vereisten mogelijk te maken.

[De handleiding voor capaciteitsplanning](service-fabric-capacity-planning.md) biedt richtlijnen voor het bepalen van het aantal knooppunten uw cluster moet.

## <a name="get-started-with-partitioning"></a>Aan de slag met partitioneren
Deze sectie beschrijft hoe u aan de slag met het partitioneren van uw service.

Service Fabric biedt een keuze uit drie partitieschema's:

* Varieerden partitioneren (anders UniformInt64Partition genoemd).
* Met de naam partitioneren. Toepassingen die gebruikmaken van dit model meestal beschikken over gegevens die kunnen worden bucketed binnen een begrensde set. Enkele algemene voorbeelden van gegevensvelden gebruikt als benoemde partitiesleutels zou worden regio's, postcode codes, klantengroepen of andere zakelijke grenzen.
* Singleton partitioneren. Singleton-partities worden meestal gebruikt wanneer de service geen aanvullende routering vereist. Bijvoorbeeld, stateless services deze partitieschema standaard gebruikt.

Met de naam en partitionering Singleton-schema's zijn speciale soorten varieerde partities. Standaard de Visual Studio-sjablonen voor Service Fabric gebruik varieerden partitioneren, zoals dit de meest voorkomende en nuttige is. De rest van dit artikel is gericht op een ranged partitionering.

### <a name="ranged-partitioning-scheme"></a>Partitieschema varieerde
Dit wordt gebruikt om op te geven van een geheel getal-bereik (aangeduid met een lage en hoge sleutel) en een aantal partities (n). N partities, elke verantwoordelijk is voor een niet-overlappende subbereik van de algehele partitiesleutelbereik wordt gemaakt. Bijvoorbeeld, een ranged partitieschema met een lage sleutel 0, een hoge sleutel 99 en een telling van 4 maakt vier partities zoals hieronder wordt weergegeven.

![Bereik partitioneren](./media/service-fabric-concepts-partitioning/range-partitioning.png)

Een gemeenschappelijke aanpak is het maken van een hash op basis van een unieke sleutel in de gegevensset. Enkele algemene voorbeelden van sleutels is een vehicle id-nummer (VIN), een werknemer-ID of een unieke tekenreeks zijn. Met behulp van deze unieke sleutel, zou u vervolgens een hash-code, modulus de belangrijkste bereik, om te gebruiken als uw sleutel te genereren. U kunt de hogere en lagere grenzen van het toegestane bereik van de sleutel opgeven.

### <a name="select-a-hash-algorithm"></a>Selecteer een hash-algoritme
Een belangrijk onderdeel van het hash-selecteert hash-algoritme. Overweging is of het doel is om soortgelijke sleutels in de buurt van elkaar (plaats gevoelige hashing)--groep of als activiteit moet op grote schaal worden gedistribueerd voor alle partities (hash-distributie), waarmee veelvoorkomende is.

De kenmerken van een goede distributie hash-algoritme zijn dat het is gemakkelijk om te berekenen, enkele conflicten heeft en wordt de sleutels gelijkmatig gedistribueerd. Een goed voorbeeld van een efficiënte hash-algoritme is de [FNV 1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash-algoritme.

Een goede resource voor algemene hash-code algoritme opties is de [pagina Wikipedia (Engelstalig) op de hash-functies](http://en.wikipedia.org/wiki/Hash_function).

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Een stateful service met meerdere partities bouwen
Stel uw eerste betrouwbare stateful service maken met meerdere partities. In dit voorbeeld maakt u een zeer eenvoudige toepassing waarin u wilt opslaan van alle laatste namen die met dezelfde letter in dezelfde partitie beginnen.

Voordat u een code te schrijven, moet u nadenken over de partities en partitiesleutels. U 26 partities (één voor elke letter van het alfabet), maar wat over de lage en hoge sleutels nodig?
Als we letterlijk één partitie per letter hebben willen, kunt we gebruiken 0 als de lage sleutel en 25 als de hoge sleutel elke letter is zijn eigen sleutel.

> [!NOTE]
> Dit is een vereenvoudigde scenario in feite de distributie ongelijke zou zijn. Laatste namen die beginnen met de letters "S" of ''M' komen vaker dan degene die beginnen met 'X' of 'Y'.
> 
> 

1. Open **Visual Studio** > **bestand** > **nieuwe** > **Project**.
2. In de **nieuw Project** dialoogvenster Kies de Service Fabric-toepassing.
3. Het project 'AlphabetPartitions' aanroepen.
4. In de **maken van een Service** dialoogvenster Kies **Stateful** service en deze aanroepen 'Alphabet.Processing' zoals weergegeven in de onderstaande afbeelding.
       ![Dialoogvenster voor nieuwe service in Visual Studio][1]

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. Stel het aantal partities. Open het bestand Applicationmanifest.xml in de map ApplicationPackageRoot van het project AlphabetPartitions en bijwerken van de parameter Processing_PartitionCount 26, zoals hieronder wordt weergegeven.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    U moet ook bijwerken van de eigenschappen LowKey en HighKey van het element StatefulService in de ApplicationManifest.xml zoals hieronder wordt weergegeven.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. Voor de service toegankelijk is, opent u een eindpunt op een poort door het eindpuntelement van ServiceManifest.xml (te vinden in de map PackageRoot) toe te voegen voor de service Alphabet.Processing zoals hieronder wordt weergegeven:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    De service is nu geconfigureerd om te luisteren naar een interne eindpunt met 26 partities.
7. Vervolgens moet u voor het onderdrukken van de `CreateServiceReplicaListeners()` methode van de klasse verwerking.
   
   > [!NOTE]
   > Voor dit voorbeeld nemen we aan dat u van een eenvoudige HttpCommunicationListener gebruikmaakt. Zie voor meer informatie over betrouwbare servicecommunicatie [communicatiemodel de betrouwbare Service](service-fabric-reliable-services-communication.md).
   > 
   > 
8. Een aanbevolen patroon voor de URL die een replica luistert op de volgende indeling is: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Zo wilt u de listener communicatie om te luisteren op de juiste eindpunten en met dit patroon configureren.
   
    Meerdere replica's van deze service kunnen worden gehost op dezelfde computer, zodat dit adres moet uniek zijn voor de replica. Daarom partitie-ID + replica-ID in de URL zijn. HttpListener kan luisteren op meerdere adressen op dezelfde poort, zolang het URL-voorvoegsel uniek is.
   
    De extra GUID is er voor een geavanceerde aanvraag waarop secundaire replica's ook voor alleen-lezen-aanvragen luisteren. Wanneer dit het geval is, u om ervoor te zorgen dat een nieuw uniek adres tijdens het veranderen van primaire naar secundaire wordt gebruikt voor het afdwingen voor clients opnieuw omzetten van het adres. '+' wordt gebruikt als het adres hier zodat de replica luistert op alle beschikbare hosts (IP-, FQDM, ' localhost ', enz.) De volgende code toont een voorbeeld.
   
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
   
    Het is ook opgemerkt dat de gepubliceerde URL enigszins afwijken van de luisterende URL-voorvoegsel is.
    De URL van de luisterende aan HttpListener gegeven. De gepubliceerde URL is de URL die is gepubliceerd op de Service Fabric Naming Service, die wordt gebruikt voor servicedetectie. Clients vragen voor dit adres via die discovery-service. Het adres dat clients ophalen moet de werkelijke IP of FQDN van het knooppunt om verbinding te kunnen hebben. Daarom moet u vervangen '+' met het knooppunt IP of FQDN-naam zoals hierboven.
9. De laatste stap is het toevoegen van de logica voor verwerking naar de service, zoals hieronder wordt weergegeven.
   
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
   
    `ProcessInternalRequest`de waarden van de querytekenreeksparameter gebruikt voor het aanroepen van de partitie en aanroepen leest `AddUserAsync` achternaam toevoegen aan de woordenlijst voor betrouwbare `dictionary`.
10. We gaan een stateless service toevoegen aan het project om te zien hoe u een bepaalde partitie kunt aanroepen.
    
    Deze service fungeert als een eenvoudige webinterface die de achternaam als een queryreeksparameter accepteert, bepaalt de partitiesleutel en verzendt het naar de service Alphabet.Processing voor verwerking.
11. In de **maken van een Service** dialoogvenster Kies **Stateless** service en deze aanroepen 'Alphabet.Web' zoals hieronder wordt weergegeven.
    
    ![Schermopname van staatloze service](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. Werk de eindpuntinformatie in de ServiceManifest.xml van de service Alphabet.WebApi openen van een poort zoals hieronder wordt weergegeven.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. U moet een verzameling ServiceInstanceListeners retourneren in de Web-klasse. U kunt opnieuw een eenvoudige HttpCommunicationListener implementeren.
    
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
14. U moet nu de verwerking van logica implementeren. Het aanroepen van de HttpCommunicationListener `ProcessInputRequest` wanneer een aanvraag binnenkomt. Dus we gaan nu en voeg de volgende code.
    
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
    
    We doorlopen die deze stap voor stap. De code leest de eerste letter van de querytekenreeksparameter `lastname` in een tekenset. Vervolgens wordt de partitiesleutel voor deze brief bepaald door af te trekken van de hexadecimale waarde van `A` van de hexadecimale waarde van de eerste letter van de laatste namen.
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Vergeet niet dat voor dit voorbeeld gebruiken we 26 partities met één partitiesleutel per partitie.
    Vervolgens verkrijgen van de partitie service `partition` voor deze sleutel met behulp van de `ResolveAsync` methode op de `servicePartitionResolver` object. `servicePartitionResolver`is gedefinieerd als
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    De `ResolveAsync` methode neemt de URI van de service, de partitiesleutel en een annulering als parameters token. De service-URI voor de verwerkingsservice is `fabric:/AlphabetPartitions/Processing`. Vervolgens wordt het eindpunt van de partitie ophalen.
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Ten slotte we de eindpunt-URL plus de querytekenreeks bouwen en de van verwerkingsservice aanroepen.
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    Als de verwerking is voltooid, wordt de uitvoer terug te schrijven.
15. De laatste stap is het testen van de service. Visual Studio maakt gebruik van parameters voor de toepassing voor lokale en cloudimplementatie. Als u wilt de service met 26 partities lokaal testen, moet u bijwerken de `Local.xml` bestand in de map ApplicationParameters van het project AlphabetPartitions zoals hieronder wordt weergegeven:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Wanneer u klaar bent voor implementatie, kunt u de service en alle van de partities in de Service Fabric Explorer controleren.
    
    ![Schermafbeelding van de service Fabric Explorer](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. In een browser, kunt u de partitionering logica testen door te voeren `http://localhost:8081/?lastname=somename`. U ziet dat elke achternaam op die met dezelfde letter begint worden opgeslagen in dezelfde partitie.
    
    ![Schermafbeelding van de browser](./media/service-fabric-concepts-partitioning/samplerunning.png)

De volledige broncode van het voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor informatie over Service Fabric-concepten:

* [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
* [Schaalbaarheid van Service Fabric-services](service-fabric-concepts-scalability.md)
* [Capaciteitsplanning voor Service Fabric-toepassingen](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png