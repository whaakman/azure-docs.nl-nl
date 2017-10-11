---
title: Uw eerste Service Fabric-toepassing maken in C# | Microsoft Docs
description: Inleiding tot het maken van een Microsoft Azure Service Fabric-toepassing met staatloze en stateful services.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: vturecek
ms.openlocfilehash: 813021d6239ae3cf79bb84b78f77e39c9e0783f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-reliable-services"></a>Aan de slag met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-quick-start.md)
> * [Java op Linux](service-fabric-reliable-services-quick-start-java.md)
> 
> 

Een Azure Service Fabric-toepassing bevat een of meer services waarop uw code wordt uitgevoerd. Deze handleiding wordt beschreven hoe u maakt zowel stateless als stateful Service Fabric-toepassingen met [Reliable Services](service-fabric-reliable-services-introduction.md).  Deze video Microsoft Virtual Academy leest u hoe u een stateless betrouwbare service maken:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965">  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a>Basisconcepten
Om aan de slag met Reliable Services, moet u slechts enkele basisbeginselen begrijpen:

* **Servicetype**: dit is uw service-implementatie. Dit is gedefinieerd door de klasse u schrijven die uitgebreider is dan `StatelessService` en andere code of afhankelijkheden gebruikt, samen met een naam en een uniek versienummer.
* **Service-exemplaar met de naam**: voor het uitvoeren van uw service, u maakt benoemde exemplaren van het servicetype veel zoals u instanties van objecten van het type van een klasse maken. Een service-exemplaar heeft een naam in de vorm van een URI met de "fabric: / ' schema, zoals" fabric: / MyApp/MijnService '.
* **ServiceHost**: de benoemde service-exemplaren die u maakt moeten uitvoeren in een hostproces. De ServiceHost is alleen een proces waarbij exemplaren van de service kunnen worden uitgevoerd.
* **Service-registratie**: inschrijving brengt alles bij elkaar. Type van de service moet zijn geregistreerd met de Service Fabric-runtime in een ServiceHost om toe te staan van Service Fabric om exemplaren van deze te maken om uit te voeren.  

## <a name="create-a-stateless-service"></a>Een stateless service maken
Een stateless service is een type service dat momenteel is de norm in de cloud-toepassingen. Dit wordt beschouwd als staatloze omdat de service zelf bevat geen gegevens die moet worden opgeslagen op betrouwbare wijze of maximaal beschikbaar is gemaakt. Als u een exemplaar van een staatloze service wordt afgesloten, is alle van de interne status verloren. In dit type van de service moet status worden opgeslagen op een externe winkel, zoals Azure-tabellen of een SQL-database voor de maximaal beschikbare en betrouwbare worden gemaakt.

Start Visual Studio 2015 of Visual Studio 2017 als beheerder en maak een nieuwe Service Fabric-toepassingsproject met de naam *HelloWorld*:

![Gebruik het dialoogvenster Nieuw Project voor het maken van een nieuwe Service Fabric-toepassing](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Maak vervolgens een stateless service-project met de naam *HelloWorldStateless*:

![Klik in het dialoogvenster tweede een stateless serviceproject maken](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Uw oplossing bevat nu twee projecten:

* *HelloWorld*. Dit is de *toepassing* project met uw *services*. Het bevat ook het toepassingsmanifest die worden beschreven van de toepassing, evenals een aantal PowerShell-scripts die u helpen bij het implementeren van uw toepassing.
* *HelloWorldStateless*. Dit is het serviceproject. Het bevat de staatloze service-implementatie.

## <a name="implement-the-service"></a>De service implementeren
Open de **HelloWorldStateless.cs** bestand in de serviceproject. Een service kunt eventuele bedrijfslogica uitvoeren in Service Fabric. De service-API biedt twee toegangspunten voor uw code:

* Een methode met een vermelding point, aangeroepen *RunAsync*, waar u kunt beginnen alle werkbelastingen, met inbegrip van langlopende compute-workloads uitvoeren.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* Een communicatie-ingangspunt waar u uw communicatiestack keuze, zoals ASP.NET Core kunt aansluiten. Dit is waar u ontvangen van aanvragen van gebruikers en andere services kan starten.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

In deze zelfstudie gaan we in op de `RunAsync()` entry point-methode. Dit is waar u kunt onmiddellijk starten uitvoeren van uw code.
De projectsjabloon bevat een Voorbeeldimplementatie van `RunAsync()` die een aantal rolling verhoogd.

> [!NOTE]
> Zie voor meer informatie over het werken met een communicatiestack [Service Fabric-Web-API-services met OWIN zelf die als host fungeert](service-fabric-reliable-services-communication-webapi.md)
> 
> 

### <a name="runasync"></a>RunAsync
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

Het platform roept deze methode wanneer een exemplaar van een service is geplaatst en kan worden uitgevoerd. Voor een stateless service betekent dat gewoon wanneer het service-exemplaar wordt geopend. Een token annulering is opgegeven voor het coördineren van wanneer uw service-exemplaar moet worden gesloten. Deze cyclus openen en sluiten van een service-exemplaar kan vaak gedurende de levensduur van de service in Service Fabric optreden als geheel. Dit kan gebeuren om verschillende redenen, waaronder:

* Het systeem wordt uw service-exemplaren voor resourceverdeling verplaatst.
* Fouten die voorkomen in uw code.
* De toepassing of het systeem is bijgewerkt.
* De onderliggende hardware optreedt een storing.

Deze indeling wordt beheerd door het systeem te houden van uw service maximaal beschikbaar is en goed taakverdeling.

`RunAsync()`moet niet synchroon blokkeren. De implementatie van RunAsync moet een taak retourneren of await op langlopende of blokkeringen bewerkingen waarmee de runtime om door te gaan. Houd er rekening mee de `while(true)` lus in het vorige voorbeeld wordt een taak retourneren `await Task.Delay()` wordt gebruikt. Als uw werkbelasting synchroon blokkeert moet, moet u een nieuwe taak plannen `Task.Run()` in uw `RunAsync` implementatie.

Annulering van uw werkbelasting is een gezamenlijke inspanning gedirigeerd door het token opgegeven annulering. Het systeem wacht tot de taak te beëindigen (met succes is voltooid, annulering of fault) voordat het wordt verplaatst op. Het is belangrijk om te voldoen aan het token annulering, werk voltooien en af te sluiten `RunAsync()` zo snel mogelijk wanneer het systeem annulering aanvraagt.

In dit voorbeeld staatloze service wordt het aantal opgeslagen in een lokale variabele. Maar omdat dit een stateless service, de waarde die opgeslagen bestaat alleen voor de huidige levenscyclus van de service-exemplaar. Wanneer de service wordt verplaatst of opnieuw wordt opgestart, wordt de waarde is verloren gegaan.

## <a name="create-a-stateful-service"></a>Een stateful service maken
Service Fabric introduceert een nieuw soort stateful service. Een stateful service te onderhouden op betrouwbare wijze binnen de service zelf, dezelfde locatie bevindt als de code die wordt gebruikt. Status is maximaal beschikbaar is gemaakt door de Service Fabric zonder de noodzaak om vast te leggen van de status van een externe winkel.

Een waarde van het prestatiemeteritem van staatloze conversie naar maximaal beschikbare en permanente, zelfs wanneer de service wordt verplaatst of opnieuw wordt opgestart, moet u een stateful service.

In dezelfde *HelloWorld* toepassing, kunt u een nieuwe service toevoegen door met de verwijzingen in het project voor een toepassing met de rechtermuisknop op de Services en **toevoegen -> nieuwe Service Fabric-Service**.

![Een service aan uw Service Fabric-toepassing toevoegen](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Selecteer **Stateful Service** en noem deze *HelloWorldStateful*. Klik op **OK**.

![Gebruik het dialoogvenster Nieuw Project voor het maken van een nieuwe stateful Service Fabric-service](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Uw toepassing hebt nu twee services: de staatloze service *HelloWorldStateless* en de stateful service *HelloWorldStateful*.

Een stateful service heeft de dezelfde toegangspunten als een stateless service. Het belangrijkste verschil is de beschikbaarheid van een *state-provider* die de status op betrouwbare wijze kunt opslaan. Service Fabric wordt geleverd met een implementatie van de persistentieprovider status aangeroepen [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md), dit kunt u gegevensstructuren via statusbeheer voor het betrouwbare gerepliceerd. Een stateful betrouwbare Service maakt standaard gebruik van deze state-provider.

Open **HelloWorldStateful.cs** in *HelloWorldStateful*, die de volgende RunAsync-methode bevat:

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, the transaction aborts, all changes are
            // discarded, and nothing is saved to the secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a>RunAsync
`RunAsync()`draait op dezelfde manier in stateful en staatloze services. Echter, in een stateful service het platform extra werk uitvoert namens jou voordat deze wordt uitgevoerd `RunAsync()`. Deze taak kunt opnemen ervoor te zorgen dat de betrouwbare statusbeheer en betrouwbare verzamelingen klaar zijn voor gebruik.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Betrouwbare verzamelingen en de betrouwbare status Manager
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) is een dictionary-implementatie die u gebruiken kunt om op te slaan op betrouwbare wijze staat in de service. Met Service Fabric en betrouwbare verzamelingen, kunt u gegevens opslaan rechtstreeks in uw service zonder de noodzaak van een externe permanente archief. Betrouwbare verzamelingen uw gegevens maximaal beschikbaar maken. Service Fabric bewerkstelligt dit door het maken en beheren van meerdere *replica's* van uw service voor u. Het bevat ook een API die weg de complexiteit isoleert van het beheren van deze replica's en hun statusovergangen.

Betrouwbare verzamelingen kunnen elk type .NET, met inbegrip van uw aangepaste typen, met een aantal aanvullende opmerkingen worden opgeslagen.

* Service Fabric worden uw status maximaal beschikbaar door *repliceren* staat tussen knooppunten en betrouwbare verzamelingen opslaan van gegevens naar de lokale schijf op elke replica. Dit betekent dat alles dat is opgeslagen in betrouwbare verzamelingen moet *serialiseerbaar*. Standaard gebruik van betrouwbare verzamelingen [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) voor serialisatie, dus is het belangrijk om ervoor te zorgen dat uw typen zijn [ondersteund door de serialisatiefunctie van het gegevenscontract](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) wanneer u de serialisatiefunctie standaard gebruikt.
* Objecten worden gerepliceerd voor hoge beschikbaarheid, wanneer u transacties op betrouwbare verzamelingen doorvoert. Objecten die zijn opgeslagen in betrouwbare verzamelingen worden opgeslagen in het lokale geheugen van uw service. Dit betekent dat u een lokale verwijzing naar het object hebt.
  
   Het is belangrijk dat u geen lokale exemplaren van deze objecten muteren zonder dat u een updatebewerking op de verzameling betrouwbare uitvoert in een transactie. Dit is omdat wijzigingen in de lokale exemplaren van objecten worden niet automatisch worden gerepliceerd. U moet het object opnieuw weer in de woordenlijst invoegen of gebruik een van de *bijwerken* methoden in de woordenlijst.

Statusbeheer voor het betrouwbare beheert betrouwbare verzamelingen voor u. U kunt gewoon vragen statusbeheer voor het betrouwbare voor een betrouwbare verzameling met de naam op elk gewenst moment en op een willekeurige plaats in uw service. Statusbeheer voor het betrouwbare zorgt ervoor dat u een verwijzing terug. Wordt niet aanbevolen verwijzingen naar betrouwbare verzameling exemplaren te slaan in klasselid variabelen of eigenschappen. Speciale aandacht moet worden uitgevoerd om ervoor te zorgen dat de verwijzing naar een exemplaar te allen tijde in de levenscyclus van de service is ingesteld. Statusbeheer voor het betrouwbare dit werk voor u verwerkt en geoptimaliseerd voor herhaaldelijk bezoeken.

### <a name="transactional-and-asynchronous-operations"></a>Transactionele en asynchrone bewerkingen
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Betrouwbare verzamelingen hebben veel van dezelfde bewerkingen die hun `System.Collections.Generic` en `System.Collections.Concurrent` collega's doen, met uitzondering van LINQ. Bewerkingen op betrouwbare verzamelingen zijn asynchroon. Dit is omdat schrijfbewerkingen met betrouwbare verzamelingen i/o-bewerkingen als u wilt repliceren en de gegevens naar schijf te behouden.

Betrouwbare verzameling bewerkingen zijn *transactionele*, zodat u status consistent voor meerdere betrouwbare verzamelingen en bewerkingen houden kunt. U kunt bijvoorbeeld een werkitem van een betrouwbare wachtrij in wachtrij, een bewerking uitvoeren op deze en het resultaat opslaan in een betrouwbare woordenlijst alle binnen een transactie. Dit wordt beschouwd als een atomic-bewerking en dit zorgt ervoor dat de hele bewerking slaagt of de hele bewerking wordt teruggedraaid. Als een fout optreedt nadat u het item in wachtrij, maar voordat u het resultaat opslaat, wordt de hele transactie wordt teruggedraaid en blijft het item in de wachtrij voor verwerking.

## <a name="run-the-application"></a>De toepassing uitvoeren
We nu terug naar de *HelloWorld* toepassing. U kunt nu bouwen en implementeren van uw services. Wanneer u drukt op **F5**, uw toepassing worden gemaakt en geïmplementeerd op uw lokale cluster.

Nadat de services zijn gestart, kunt u weergeven met de gegenereerde Event Tracing voor Windows (ETW)-gebeurtenissen in een **diagnostische gebeurtenissen** venster. Denk eraan dat de weergegeven gebeurtenissen afkomstig van zowel de staatloze service en de stateful service in de toepassing zijn. U kunt de stroom onderbreken door te klikken op de **onderbreken** knop. U kunt vervolgens de details van een bericht controleren door het uitbreiden van dat bericht.

> [!NOTE]
> Voordat u de toepassing uitvoert, zorg ervoor dat er een lokaal ontwikkelcluster uitgevoerd. Bekijk de [instructiehandleiding](service-fabric-get-started.md) voor informatie over het instellen van uw lokale omgeving.
> 
> 

![Diagnostische gebeurtenissen weergeven in Visual Studio](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Volgende stappen
[Fouten opsporen in uw Service Fabric-toepassing in Visual Studio](service-fabric-debugging-your-application.md)

[Aan de slag: Service Fabric-Web-API-services met OWIN zelf die als host fungeert](service-fabric-reliable-services-communication-webapi.md)

[Meer informatie over betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)

[Een toepassing implementeren](service-fabric-deploy-remove-applications.md)

[Upgrade van de toepassing](service-fabric-application-upgrade.md)

[Referentie voor ontwikkelaars voor Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

