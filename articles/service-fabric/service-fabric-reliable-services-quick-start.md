---
title: Maak uw eerste Service Fabric-toepassing in C# | Microsoft Docs
description: Inleiding tot het maken van een Microsoft Azure Service Fabric-toepassing met staatloze en stateful services.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/16/2018
ms.author: vturecek
ms.openlocfilehash: 26520e3ff8f5bb97866015d3148b4c8b8813b319
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58168567"
---
# <a name="get-started-with-reliable-services"></a>Aan de slag met Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-quick-start.md)
> * [Java op Linux](service-fabric-reliable-services-quick-start-java.md)
> 
> 

Een Azure Service Fabric-toepassing bevat een of meer services waarop uw code wordt uitgevoerd. Deze handleiding wordt beschreven hoe u het maken van staatloze en stateful Service Fabric-toepassingen met [Reliable Services](service-fabric-reliable-services-introduction.md).  

## <a name="basic-concepts"></a>Basisbegrippen
Als u wilt aan de slag met Reliable Services, moet u slechts enkele fundamentele concepten begrijpen:

* **Servicetype**: Dit is uw service-implementatie. Deze is gedefinieerd door de klasse u schrijven die een uitbreiding `StatelessService` en een andere code of afhankelijkheden gebruikt, samen met een naam en een uniek versienummer.
* **Service-exemplaar met de naam**: Voor het uitvoeren van uw service, u benoemde exemplaren van het servicetype, veel, zoals het maken van instanties van objecten van een klassentype. Een service-exemplaar heeft een naam in de vorm van een URI met de ' fabric: / "schema, zoals ' fabric: / Mijntoep/MijnService '.
* **ServiceHost**: De benoemde service-instanties dat u maakt moeten binnen een proces uitgevoerd. De ServiceHost is alleen een proces waarbij instanties van uw service kunnen worden uitgevoerd.
* **Service-registratie**: Inschrijving brengt alles bij elkaar. Type van de service moet worden geregistreerd bij de Service Fabric-runtime in een ServiceHost om toe te staan van Service Fabric om instanties van deze te maken om uit te voeren.  

## <a name="create-a-stateless-service"></a>Een stateless service maken
Een stateless service is een type van de service die momenteel zijn de norm in cloudtoepassingen. Het wordt beschouwd als stateless omdat de service zelf bevat geen gegevens die moet worden opgeslagen op een betrouwbare manier of maximaal beschikbaar gemaakt. Als een exemplaar van een staatloze service wordt uitgeschakeld, wordt alle van de interne status verloren gaan. In dit type service kan moet status worden opgeslagen op een externe opslag, zoals Azure Tables of een SQL-database om te worden aangebracht maximaal beschikbare en betrouwbare.

Start Visual Studio 2015 of Visual Studio 2017 als beheerder en maak een nieuw Service Fabric-toepassing-project met de naam *HelloWorld*:

![Gebruik van het dialoogvenster Nieuw Project te maken van een nieuwe Service Fabric-toepassing](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Maak vervolgens een stateless service-project met **.NET Core 2.0** met de naam *HelloWorldStateless*:

![Klik in het dialoogvenster tweede een stateless serviceproject maken](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Uw oplossing bevat nu twee projecten:

* *HelloWorld*. Dit is de *toepassing* project met daarin uw *services*. Het bevat ook het toepassingsmanifest die worden beschreven van de toepassing, evenals een aantal PowerShell-scripts die u helpen bij het implementeren van uw toepassing.
* *HelloWorldStateless*. Dit is de serviceproject. Het bevat de stateless service-implementatie.

## <a name="implement-the-service"></a>De service implementeren
Open de **HelloWorldStateless.cs** bestand in het service-project. Een service kan in Service Fabric, eventuele bedrijfslogica worden uitgevoerd. De service-API biedt twee toegangspunten voor uw code:

* Een methode met een vermelding punt, met de naam *RunAsync*, waar u kunt beginnen alle workloads, met inbegrip van langlopende compute-workloads uitvoeren.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* Een communicatie-toegangspunt waar u in uw communicatiestack van keuze, bijvoorbeeld ASP.NET Core aansluiten kunt. Dit is waar u ontvangen van aanvragen van gebruikers en andere services kan starten.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

In deze zelfstudie gaan we in op de `RunAsync()` vermelding punt methode. Dit is waar u kunt direct beginnen waarop uw code wordt uitgevoerd.
De projectsjabloon, maken omvat een Voorbeeldimplementatie van `RunAsync()` die een rolling telling wordt verhoogd.

> [!NOTE]
> Zie voor meer informatie over het werken met een communicatiestack [Web-API voor Service Fabric-services met OWIN zelf die als host fungeert](service-fabric-reliable-services-communication-webapi.md)
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

        ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

Het platform wordt deze methode wanneer een exemplaar van een service is geplaatst en gereed is om uit te voeren. Voor een stateless service betekent dat alleen wanneer het service-exemplaar wordt geopend. Een token annulering is opgegeven voor de coördinatie van wanneer uw service-exemplaar moet worden gesloten. Deze cyclus openen en sluiten van een service-exemplaar kan vaak gedurende de levensduur van de service in Service Fabric optreden als geheel. Dit kan gebeuren om verschillende redenen, waaronder:

* Het systeem wordt uw service-instanties voor bron-balancing verplaatst.
* Fouten optreden in uw code.
* De toepassing of het systeem is bijgewerkt.
* De onderliggende hardware er een storing optreedt.

Deze indeling wordt beheerd door het systeem te houden van uw service hoge mate beschikbare en goed met gelijke taakverdeling.

`RunAsync()` moet niet synchroon blokkeren. De implementatie van RunAsync moet een taak retourneren of await op langlopende of blokkerende bewerkingen om toe te staan van de runtime om door te gaan. Houd er rekening mee de `while(true)` lus in het vorige voorbeeld wordt een taak retourneren `await Task.Delay()` wordt gebruikt. Als uw werkbelasting synchroon blokkeert moet, moet u een nieuwe taak met plannen `Task.Run()` in uw `RunAsync` implementatie.

De annulering van uw werkbelasting is een gezamenlijke inspanning ingedeeld door de opgegeven annulering-token. Het systeem wacht tot de taak te beëindigen (met succes is voltooid, annulering of fout) voordat deze op. Het is belangrijk om te voldoen aan het token annulering, werk voltooien en sluit `RunAsync()` zo snel mogelijk wanneer het systeem annulering aanvraagt.

In dit voorbeeld stateless service, wordt het aantal opgeslagen in een lokale variabele. Maar omdat dit een staatloze service is, de waarde die opgeslagen bestaat alleen voor de huidige levenscyclus van de service-exemplaar. Wanneer de service worden verplaatst of opnieuw wordt opgestart, wordt de waarde is verloren gegaan.

## <a name="create-a-stateful-service"></a>Een stateful service maken
Service Fabric introduceert een nieuw soort stateful service. Een stateful service kunt onderhouden van de status op betrouwbare wijze binnen de service zelf, CO-locatie met de code die wordt gebruikt. De status is maximaal beschikbaar gemaakt door de Service Fabric zonder de noodzaak om vast te leggen de status van een externe opslag.

Als u wilt omzetten in een waarde van het prestatiemeteritem van staatloze hoge beschikbaarheid en permanente, zelfs wanneer de service worden verplaatst of opnieuw wordt opgestart, moet u een stateful service.

In dezelfde *HelloWorld* toepassing, kunt u een nieuwe service toevoegen met de rechtermuisknop op de Services verwijzingen in het toepassingsproject en selecteer **Add -> New Service Fabric Service**.

![Een service toevoegen aan uw Service Fabric-toepassing](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Selecteer **.NET Core 2.0 -> Stateful Service** en noem het *HelloWorldStateful*. Klik op **OK**.

![Gebruik van het dialoogvenster Nieuw Project te maken van een nieuwe Service Fabric stateful service](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Uw toepassing heeft nu twee services: de stateless service *HelloWorldStateless* en de stateful service *HelloWorldStateful*.

Een stateful service heeft de dezelfde toegangspunten als een stateless service. Het belangrijkste verschil is de beschikbaarheid van een *state-provider* die status op betrouwbare wijze kunt opslaan. Service Fabric wordt geleverd met een status-provider-implementatie met de naam [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md), welke kunt u maken gerepliceerd gegevensstructuren via de betrouwbare status Manager. Een stateful betrouwbare Service maakt gebruik van deze provider staat standaard.

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

            ServiceEventSource.Current.ServiceMessage(this.Context, "Current Counter Value: {0}",
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
`RunAsync()` werkt op dezelfde manier in stateful en stateless services. Echter, in een stateful service het platform extra werk uitvoert namens voordat deze wordt uitgevoerd `RunAsync()`. Deze taak kunt opnemen ervoor zorgen dat de Manager van betrouwbare status en de betrouwbare verzamelingen klaar zijn voor gebruik.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Betrouwbare verzamelingen en de betrouwbare status Manager
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) is een dictionary-implementatie die u gebruiken kunt om op te slaan op een betrouwbare manier staat in de service. Met Service Fabric en betrouwbare verzamelingen, kunt u gegevens rechtstreeks in uw service zonder de noodzaak van een externe permanente archief opslaan. Betrouwbare verzamelingen uw gegevens maximaal beschikbaar maken. Service Fabric doet dit door het maken en beheren van meerdere *replica's* van uw service voor u. Het biedt ook een API die de complexiteit van het beheer van deze replica's en hun statusovergangen volledig weggewerkt.

Betrouwbare verzamelingen kunnen elk type .NET, met inbegrip van uw aangepaste typen, met enkele aanvullende opmerkingen opslaan:

* Service Fabric zorgt de status voor hoge beschikbaarheid *repliceren* status over knooppunten en betrouwbare verzamelingen gegevens naar de lokale schijf opslaan op elke replica. Dit betekent dat alles die zijn opgeslagen in Reliable Collections moet worden *serialiseerbare*. Betrouwbare verzamelingen gebruiken standaard [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) voor serialisatie, dus is het belangrijk om ervoor te zorgen dat uw typen zijn [ondersteund door de Serializer van de opdracht gegevens](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) bij het gebruik van de serializer van de standaard.
* Objecten worden gerepliceerd voor hoge beschikbaarheid bij het doorvoeren van transacties voor betrouwbare verzamelingen. Objecten die zijn opgeslagen in de betrouwbare verzamelingen zijn opgeslagen in het lokale geheugen van uw service. Dit betekent dat u een lokale verwijzing naar het object hebt.
  
   Het is belangrijk dat u geen lokale exemplaren van deze objecten muteren zonder dat er een updatebewerking op de betrouwbare verzameling in een transactie. Dit is omdat wijzigingen aan lokale exemplaren van objecten wordt niet automatisch worden gerepliceerd. U moet het object opnieuw terug in de woordenlijst invoegen of gebruik een van de *bijwerken* methoden in de woordenlijst.

De betrouwbare status Manager beheert betrouwbare verzamelingen voor u. U kunt gewoon vragen de betrouwbare status Manager voor een betrouwbare verzameling met de naam op elk gewenst moment en op een willekeurige plaats in uw service. De betrouwbare status Manager zorgt ervoor dat u een verwijzing terug worden opgehaald. Niet wordt aangeraden dat u verwijzingen naar instanties van betrouwbare verzameling opslaat in klasse variabelen of eigenschappen. Speciale aandacht moet worden uitgevoerd om ervoor te zorgen dat de verwijzing is ingesteld op een exemplaar te allen tijde in de levenscyclus van de service. De betrouwbare status Manager verwerkt dit werk voor u en geoptimaliseerd voor herhalingen bezoeken.

### <a name="transactional-and-asynchronous-operations"></a>Transactionele en asynchrone bewerkingen
```csharp
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Betrouwbare verzamelingen zijn veel van dezelfde bewerkingen die hun `System.Collections.Generic` en `System.Collections.Concurrent` tegenhangers doen, met uitzondering van LINQ. Bewerkingen voor betrouwbare verzamelingen zijn asynchroon. Dit is omdat schrijfbewerkingen met betrouwbare verzamelingen i/o-bewerkingen om te repliceren en behoud van gegevens naar schijf uitvoeren.

Betrouwbare verzameling bewerkingen zijn *transactionele*, zodat u status consistent voor meerdere betrouwbare verzamelingen en bewerkingen houden kunt. Bijvoorbeeld, kan u uit de wachtrij verwijderen een werkitem van een betrouwbare wachtrij, het uitvoeren van een bewerking op en het resultaat opslaan in een betrouwbare Dictionary, allemaal binnen één transactie. Dit wordt behandeld als een atomische bewerking, en dit zorgt ervoor dat de gehele bewerking slaagt of de gehele bewerking wordt teruggedraaid. Als een fout optreedt nadat u het item in wachtrij, maar voordat u het resultaat opslaat, wordt de hele transactie wordt teruggedraaid en blijft het item in de wachtrij voor verwerking.

## <a name="run-the-application"></a>De toepassing uitvoeren
We nu terug naar de *HelloWorld* toepassing. U kunt nu bouwen en implementeren van uw services. Wanneer u drukt u op **F5**, uw toepassing wordt gebouwd en geïmplementeerd op uw lokale cluster.

Nadat de services zijn gestart, kunt u weergeven met de gegenereerde gebeurtenissen van Event Tracing voor Windows (ETW) in een **diagnostische gebeurtenissen** venster. Merk op dat de weergegeven gebeurtenissen afkomstig van zowel de stateless service en de stateful service in de toepassing zijn. U kunt de stroom onderbreken door te klikken op de **onderbreken** knop. U kunt vervolgens de details van een bericht controleren door het uitbreiden van dat bericht.

> [!NOTE]
> Voordat u de toepassing uitvoert, zorg ervoor dat u hebt een lokaal ontwikkelcluster uitgevoerd. Bekijk de [introductiehandleiding](service-fabric-get-started.md) voor informatie over het instellen van uw lokale omgeving.
> 
> 

![Diagnostische gebeurtenissen in Visual Studio weergeven](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Volgende stappen
[Fouten opsporen in uw Service Fabric-toepassing in Visual Studio](service-fabric-debugging-your-application.md)

[Aan de slag: Web-API voor service Fabric-services met OWIN zelf die als host fungeert](service-fabric-reliable-services-communication-webapi.md)

[Meer informatie over betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)

[Een app implementeren](service-fabric-deploy-remove-applications.md)

[Toepassingsupgrade](service-fabric-application-upgrade.md)

[Referentie voor ontwikkelaars voor Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

