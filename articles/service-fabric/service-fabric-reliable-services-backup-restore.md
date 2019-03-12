---
title: Service Fabric-back-up en herstellen | Microsoft Docs
description: Algemene documentatie voor Service Fabric-back-up en herstel
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: subramar,zhol
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: mcoskun
ms.openlocfilehash: d01d2f18ed35d1752f97f405ae7f7bfb4708ca0d
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570042"
---
# <a name="backup-and-restore-reliable-services-and-reliable-actors"></a>Back-up en herstellen van Reliable Services en Reliable Actors
Azure Service Fabric is een hoge beschikbaarheid-platform die de status over meerdere knooppunten voor deze hoge beschikbaarheid zijn gerepliceerd.  Dus zelfs als één knooppunt in het cluster is mislukt, blijven de services beschikbaar. Hoewel deze ingebouwde redundantie die wordt geleverd door het platform mogelijk voldoende zijn voor sommige, in bepaalde gevallen is het wenselijk zijn voor de service naar de back-ups (naar een externe opslag).

> [!NOTE]
> Het is essentieel dat de back-up maken en uw gegevens te herstellen (en test deze naar verwachting werkt), zodat u vanaf gegevensverlies herstellen kunt.
> 

> [!NOTE]
> Microsoft raadt aan om te gebruiken [periodieke back-up en herstel](service-fabric-backuprestoreservice-quickstart-azurecluster.md) voor het configureren van gegevensback-up van betrouwbare Stateful services en Reliable Actors. 
> 


Een service wilt bijvoorbeeld een back-up van gegevens om u te beschermen tegen de volgende scenario's:

- In het geval van het permanente verlies van gegevens van een hele Service Fabric-cluster.
- Permanent verlies van een meerderheid van de replica's van de servicepartitie van een
- Administratieve fouten waarbij de status per ongeluk wordt verwijderd of beschadigd. Dit kan bijvoorbeeld gebeuren als een beheerder met voldoende bevoegdheden per ongeluk worden verwijderd van de service.
- Fouten in de service die leiden gegevensbeschadiging tot. Dit kan bijvoorbeeld gebeuren wanneer een upgrade van de code service begint beschadigde gegevens schrijven naar een betrouwbare verzameling. In dat geval, zowel de code en de gegevens mogelijk teruggezet naar een eerdere toestand.
- Offline gegevens verwerken. Het is mogelijk handig zijn als offline verwerking van gegevens voor business intelligence die plaatsvindt afzonderlijk van de service die de gegevens worden gegenereerd.

De functie back-up/herstel kunnen services die zijn gebouwd op de betrouwbare Services-API maken en herstellen van back-ups. De back-API die is geleverd door het platform kunt back-upbestand(en) van de status van de servicepartitie van een, zonder blokkering lees- of schrijfbewerkingen. Het terugzetten van API's maken van de servicepartitie van een staat worden hersteld vanuit een gekozen back-up.

## <a name="types-of-backup"></a>Typen back-up
Er zijn twee opties voor back-up: Volledige en incrementele.
Een volledige back-up is een back-up waarin alle gegevens die nodig zijn om de status van de replica opnieuw te maken: controlepunten en alle records in logboek registreren.
Omdat dit de controlepunten en het logboek heeft, kan een volledige back-up kan worden hersteld op zichzelf.

Het probleem met volledige back-ups daartoe zich voordoet wanneer de controlepunten groot zijn.
Bijvoorbeeld, heeft een replica waarvoor de status van 16 GB controlepunten die toevoegen tot ongeveer 16 GB.
Als we een beoogd herstelpunt van vijf minuten hebt, moet de replica een back-up om de vijf minuten.
Telkens wanneer het back-ups van nodig is om te kopiëren van controlepunten naast 50 MB van 16 GB (met behulp van configureerbare `CheckpointThresholdInMB`) aan logboeken.

![Voorbeeld van de volledige back-up.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

De oplossing voor dit probleem is incrementele back-ups, waarbij back-up bevat alleen de gewijzigde logboekrecords sinds de laatste back-up.

![Voorbeeld van incrementele back-up.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Omdat de incrementele back-ups worden alleen wijzigingen sinds de laatste back-up (bevat geen controlepunten), ze vaak sneller zijn, maar ze op hun eigen kunnen niet worden hersteld.
Als u een incrementele back-up herstellen, zijn de volledige back-keten is vereist.
Een back-keten is van een keten van back-ups die beginnen met een volledige back-up en gevolgd door een aantal aaneengesloten incrementele back-ups.

## <a name="backup-reliable-services"></a>Back-up Reliable Services
De auteur van de service heeft volledige controle over wanneer u het beste back-ups en waar de back-ups worden opgeslagen.

Voor het starten van een back-up, de service moet aanroepen van de lidfunctie overgenomen `BackupAsync`.  
Back-ups kunnen alleen vanaf de primaire replica's worden gemaakt en ze nodig hebben schrijven status om te worden toegekend.

Zoals hieronder aangegeven, `BackupAsync` wordt in een `BackupDescription` -object, een waar een volledige of incrementele back-up, evenals een retouraanroepfunctie opgeven kunt, `Func<< BackupInfo, CancellationToken, Task<bool>>>` die wordt aangeroepen wanneer de back-upmap lokaal is gemaakt en gereed om te worden verplaatst is uit tot een aantal externe opslag.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Aanvraag voor een incrementele back-up kan mislukken met `FabricMissingFullBackupException`. Deze uitzondering geeft aan dat er een van de volgende dingen gebeurt:

- de replica is een volledige back-up nooit genomen, omdat deze is geworden primaire,
- enkele van de records in logboek registreren omdat de laatste back-up is afgekapt of
- replica doorgegeven de `MaxAccumulatedBackupLogSizeInMB` limiet.

Gebruikers kunnen vergroot de kans dat kunnen incrementele back-ups doen door het configureren van `MinLogSizeInMB` of `TruncationThresholdFactor`.
Verhoging van deze waarden verhoogt de per gebruik van de replica.
Zie voor meer informatie, [Reliable Services-configuratie](service-fabric-reliable-services-configuration.md)

`BackupInfo` bevat informatie met betrekking tot de back-up, met inbegrip van de locatie van de map waarin de runtime opgeslagen voor de back-up (`BackupInfo.Directory`). De callback-functie kunt verplaatsen de `BackupInfo.Directory` naar een externe opslag of een andere locatie.  Deze functie retourneert ook een Boole-waarde die aangeeft of het is de back-upmap overstappen naar de doellocatie.

De volgende code laat zien hoe de `BackupCallbackAsync` methode kan worden gebruikt voor het uploaden van de back-up naar Azure Storage:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

In het voorgaande voorbeeld `ExternalBackupStore` is de voorbeeldklasse die wordt gebruikt voor interactie met Azure Blob-opslag en `UploadBackupFolderAsync` is de methode die de map gecomprimeerd en plaatst het in de Azure Blob-archief.

Houd rekening met het volgende:

  - Er mag slechts één back-upbewerking die onderweg zijn per replica op een bepaald moment. Meer dan één `BackupAsync` aanroep tegelijk genereert `FabricBackupInProgressException` actieve back-ups op een beperken.
  - Als een replica failover optreedt terwijl een back-up uitgevoerd wordt, kan de back-up niet zijn voltooid. Nadat de failover is voltooid, het is dus de verantwoordelijkheid van de service opnieuw starten van de back-up door aan te roepen `BackupAsync` indien nodig.

## <a name="restore-reliable-services"></a>Herstellen van Reliable Services
In het algemeen de gevallen wanneer u nodig hebt mogelijk om uit te voeren van een herstelbewerking worden onderverdeeld in een van deze categorieën:

  - De service verloren gegevens partitioneren. Bijvoorbeeld, wordt de schijf voor twee van de drie replica's voor een partitie (met inbegrip van de primaire replica) beschadigd of wordt gewist. De nieuwe primaire mogelijk gegevens uit een back-up te herstellen.
  - De hele service gaat verloren. Bijvoorbeeld, een beheerder verwijdert de hele service, en dus de service en de gegevens moeten worden teruggezet.
  - De service gerepliceerd beschadigd toepassingsgegevens (bijvoorbeeld vanwege de fout van een toepassing). In dit geval de service worden bijgewerkt of als u wilt verwijderen van de oorzaak van de beschadiging hersteld, en niet beschadigd gegevens kunnen worden hersteld.

Hoewel veel benaderingen mogelijk, bieden we enkele voorbeelden over het gebruik van `RestoreAsync` om te herstellen van de bovenstaande scenario's.

## <a name="partition-data-loss-in-reliable-services"></a>Partitie gegevensverlies in Reliable Services
In dit geval de runtime automatisch wilt detecteren van het verlies van gegevens en aanroepen de `OnDataLossAsync` API.

De auteur van de service moet uitvoeren van het volgende herstellen:

  - De virtuele basisklasse-methode overschrijven `OnDataLossAsync`.
  - De meest recente back-up niet vinden in de externe locatie met back-ups van de service.
  - Download de meest recente back-up (en decomprimeren van de back-up in de back-upmap als deze is gecomprimeerd).
  - De `OnDataLossAsync` methode biedt een `RestoreContext`. Roep de `RestoreAsync` API op de opgegeven `RestoreContext`.
  - Retourneert waar als de herstelbewerking gelukt is.

Hieronder volgt een voorbeeld van de implementatie van de `OnDataLossAsync` methode:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

`RestoreDescription` doorgegeven aan de `RestoreContext.RestoreAsync` aanroep van een lid met de naam bevat `BackupFolderPath`.
Bij het herstellen van een eenmalige volledige back-up, dit `BackupFolderPath` moet worden ingesteld op het lokale pad van de map waarin uw volledige back-up.
Bij het herstellen van een volledige back-up en een aantal incrementele back-ups, `BackupFolderPath` moet worden ingesteld op het lokale pad naar de map die niet alleen de volledige back-up, maar ook alle de incrementele back-ups bevat.
`RestoreAsync` gesprek kunt throw `FabricMissingFullBackupException` als de `BackupFolderPath` opgegeven bevat een volledige back-up.
Het kan ook throw `ArgumentException` als `BackupFolderPath` bevat een verbroken keten van incrementele back-ups.
Bijvoorbeeld, als deze de volledige back-up, bevat de eerste incrementele en de derde incrementele back-up, maar niet de tweede incrementele back-up.

> [!NOTE]
> De RestorePolicy is standaard ingesteld op Safe.  Dit betekent dat de `RestoreAsync` API mislukt en ArgumentException als wordt gedetecteerd dat de back-upmap een status die ouder zijn dan of gelijk is aan de status die deel uitmaken van deze replica bevat.  `RestorePolicy.Force` kan worden gebruikt om deze veiligheidscontrole overslaan. Dit is opgegeven als onderdeel van `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>Verwijderde of verloren-service
Als een service wordt verwijderd, moet u eerst opnieuw maken de service voordat de gegevens kunnen worden hersteld.  Het is belangrijk om te maken van de service met dezelfde configuratie, bijvoorbeeld partitieschema, zodat de gegevens kunnen naadloos worden hersteld.  Zodra de service is ingeschakeld, de API om gegevens te herstellen (`OnDataLossAsync` hierboven) moet worden aangeroepen in elke partitie van deze service. Een manier om dit is met behulp van [FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx) op elke partitie.  

Vanaf dit punt is implementatie hetzelfde als het bovenstaande scenario. Elke partitie moet de nieuwste relevante back-up herstellen vanuit de externe opslag. Een nadeel is dat de partitie-ID mogelijk is nu gewijzigd, omdat de runtime partitie-id dynamisch gemaakt. Daarom moet de service voor het opslaan van de gewenste partitie informatie en service-naam voor het identificeren van de juiste meest recente back-up te herstellen uit voor elke partitie.

> [!NOTE]
> Het wordt afgeraden om te gebruiken `FabricClient.ServiceManager.InvokeDataLossAsync` op elke partitie om terug te zetten van de hele service, omdat dat de clusterstatus van uw mogelijk beschadigd.
> 

## <a name="replication-of-corrupt-application-data"></a>Replicatie van gegevens beschadigd van toepassing
Als de upgrade van de geïmplementeerde toepassing een fout, die leiden tot beschadiging van gegevens heeft kan. Een upgrade van de toepassing kan bijvoorbeeld starten record bij te werken elke telefoon nummer in een betrouwbare Dictionary met een ongeldige netnummer.  In dit geval worden de ongeldige telefoonnummers gerepliceerd sinds de Service Fabric is niet op de hoogte van de aard van de gegevens die worden opgeslagen.

Allereerst moet doen nadat u deze een egregious bug die ervoor zorgt beschadigde gegevens dat detecteren is blokkeren van de service op het toepassingsniveau van de en, indien mogelijk een upgrade uitvoert naar de versie van de toepassingscode die beschikt niet over de fout.  Echter, zelfs nadat de servicecode is opgelost, de gegevens nog steeds mogelijk beschadigd, en dus gegevens moeten mogelijk worden hersteld.  In dergelijke gevallen kan deze mogelijk niet voldoende om terug te zetten van de meest recente back-up, omdat de meest recente back-ups ook beschadigd zijn kunnen.  U moet dus vinden van de laatste back-up die is gemaakt voordat de gegevens is beschadigd.

Als u niet zeker weet welke back-ups zijn beschadigd, kan u een nieuw Service Fabric-cluster implementeren en de back-ups van de betrokken partities net als de bovenstaande 'Verwijderd of verloren service' scenario.  Voor elke partitie, start de back-up van de meest recente terugzetten naar minst. Als u een back-up waarop de beschadiging niet is gevonden, verplaatsen en verwijderen alle back-ups van deze partitie die (dan deze back-up waren). Herhaal dit proces voor elke partitie. Nu als `OnDataLossAsync` wordt aangeroepen op de partitie in de productiecluster, de laatste back-up gevonden in de externe opslag zal worden verzameld door de bovenstaande proces.

Nu de stappen in 'verwijderd of verloren service' sectie kan worden gebruikt om te herstellen van de status van de service naar de status voordat de status van de buggy code beschadigd.

Houd rekening met het volgende:

  - Wanneer u herstelt, wordt er een kans dat de back-up wordt hersteld ouder is dan de status van de partitie is voordat de gegevens verbroken is. Als gevolg hiervan moet u herstellen alleen als laatste redmiddel om zo veel gegevens mogelijk te herstellen.
  - De tekenreeks die de back-upmap pad en de paden van bestanden in de back-upmap vertegenwoordigt kan niet groter zijn dan 255 tekens, afhankelijk van het pad FabricDataRoot en de lengte van de naam van het Type toepassing. Hierdoor kunnen sommige .NET-methoden, zoals `Directory.Move`, om de `PathTooLongException` uitzondering. Een tijdelijke oplossing is om aan te roepen rechtstreeks kan kernel32-API's, zoals `CopyFile`.

## <a name="back-up-and-restore-reliable-actors"></a>Back-up en herstellen van Reliable Actors


Reliable Actors-Framework is gebaseerd op Reliable Services. De ActorService, die als host fungeert voor de actor(s) is een stateful betrouwbare service. Alle back-up en herstel functionaliteit beschikbaar zijn in Reliable Services is daarom ook beschikbaar voor Reliable Actors (met uitzondering van gedrag die specifieke state-provider). Sinds de back-ups moeten worden ondernomen op basis van per partitie, statussen voor alle actoren in deze partitie worden back-ups (en herstel is vergelijkbaar en wordt uitgevoerd op basis van per partitie). Om uit te voeren back-up/herstellen, moet de eigenaar van de service maakt u een aangepaste actor-service-klasse die is afgeleid van klasse ActorService en voer back-up/herstel die vergelijkbaar is met Reliable Services zoals hierboven is beschreven in de vorige secties.

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo)
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

Wanneer u een aangepaste actor-service-klasse maakt, moet u die ook registreren bij het registreren van de actor.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
    (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

De status standaardprovider voor betrouwbare actoren `KvsActorStateProvider`. Incrementele back-up is niet standaard ingeschakeld voor `KvsActorStateProvider`. U kunt de incrementele back-up inschakelen door het maken van `KvsActorStateProvider` met de juiste instelling in de constructor en vervolgens doorgegeven aan de constructor ActorService zoals wordt weergegeven in het volgende codefragment:

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo, null, null, new KvsActorStateProvider(true)) // Enable incremental backup
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

Nadat incrementele back-up is ingeschakeld, een incrementele back-up maken voor een van de volgende redenen mislukken met FabricMissingFullBackupException en moet u een volledige back-up uitvoeren voordat u incrementeel back-upbestand(en):

  - De replica heeft nooit genomen voor een volledige back-up omdat deze primair zijn geworden.
  - Enkele van de records in logboek registreren zijn afgekapt sinds de laatste back-up is gemaakt.

Als incrementele back-up is ingeschakeld, `KvsActorStateProvider` circulaire buffer niet gebruikt voor het beheren van de records in logboek registreren en periodiek worden afgekapt. Als er geen back-up is gemaakt door gebruiker gedurende een periode van 45 minuten, wordt de records in logboek registreren in het systeem automatisch afgekapt. Dit interval kan worden geconfigureerd door op te geven `logTruncationIntervalInMinutes` in `KvsActorStateProvider` constructor (net als bij het inschakelen van incrementele back-up). De records in logboek registreren kunnen ook ophalen afgekapt als primaire replica moet een andere replica kunt maken met alle bijbehorende gegevens te verzenden.

Bij het uitvoeren van terugzetten vanuit een back-keten, vergelijkbaar met Reliable Services de BackupFolderPath moet bevatten submappen met een submap met de volledige back-up en anderen met incrementele back-upbestand(en) submappen. De API terugzetten genereert FabricException met foutbericht weergegeven als de validatie van de back-certificaatketen mislukt. 

> [!NOTE]
> `KvsActorStateProvider` op dit moment wordt de optie RestorePolicy.Safe genegeerd. Ondersteuning voor deze functie is gepland in een toekomstige release.
> 

## <a name="testing-back-up-and-restore"></a>Testen van een Back-up en herstel
Het is belangrijk om ervoor te zorgen dat essentiële gegevens back-up en van kan worden hersteld. Dit kan worden gedaan door het aanroepen van de `Start-ServiceFabricPartitionDataLoss` cmdlet in PowerShell die een bepaalde partitie om te testen of de gegevens back-up en herstellen van functionaliteit voor uw service werkt zoals verwacht verlies van gegevens kan veroorzaken.  Het is ook mogelijk om via een programma gegevensverlies aanroepen en herstellen van die gebeurtenis ook.

> [!NOTE]
> U vindt een Voorbeeldimplementatie van back-up en herstel functionaliteit in de Web-App voor verwijzing op GitHub. Bekijk de `Inventory.Service` service voor meer informatie.
> 
> 

## <a name="under-the-hood-more-details-on-backup-and-restore"></a>Achter de schermen: meer informatie over back-up en herstel
Hier volgt wat meer informatie over back-up en herstel.

### <a name="backup"></a>Backup
De betrouwbare status Manager biedt de mogelijkheid toepassingsconsistente back-ups maken zonder dat alle lezen wordt geblokkeerd of schrijfbewerkingen. Om dit te doen, maakt het gebruik van een persistentie controlepunt- en logboekbestanden.  De betrouwbare status Manager duurt fuzzy (lichtgewicht) controlepunten op bepaalde tijdstippen om te ontlasten van het logboek voor transactionele zwaar wordt belast en hersteltijden verbeteren.  Wanneer `BackupAsync` wordt aangeroepen, de betrouwbare status Manager geeft alle betrouwbare objecten hun meest recente om controlepuntbestanden te kopiëren naar een lokale back-upmap.  Vervolgens kopieert de betrouwbare status Manager alle records in logboek registreren, vanaf de wijzer' start' naar de meest recente record voor de logboekbestanden in de back-upmap.  Aangezien alle logboekrecords tot de meest recente logboekrecord zijn opgenomen in de back-up en de betrouwbare status Manager write-ahead logboekregistratie behoudt, de betrouwbare status Manager zorgt ervoor dat alle transacties die toegewezen zijn (`CommitAsync` met succes heeft geretourneerd ) zijn opgenomen in de back-up.

De transactie die doorvoeringen na `BackupAsync` mei is aangeroepen of mogelijk niet in de back-up.  Zodra de lokale back-upmap is ingevuld door het platform (dat wil zeggen, lokale back-up is voltooid door de runtime), back-callback van de service wordt aangeroepen.  Deze aanroep is verantwoordelijk voor het verplaatsen van de back-upmap op een externe locatie, zoals Azure Storage.

### <a name="restore"></a>Herstellen
De betrouwbare status Manager biedt de mogelijkheid om terug te zetten vanuit een back-up met behulp van de `RestoreAsync` API.  
De `RestoreAsync` methode voor `RestoreContext` kan worden aangeroepen alleen binnen het `OnDataLossAsync` methode.
De bool geretourneerd door `OnDataLossAsync` geeft aan of de service de status hersteld vanuit een externe bron.
Als de `OnDataLossAsync` resulteert in waar, Service Fabric wordt opnieuw opgebouwd alle andere replica's van deze primaire. Service Fabric zorgt ervoor dat replica's die worden ontvangen `OnDataLossAsync` aanroepen van de eerste overgang naar de primaire rol, maar zijn niet de status verleend lezen of schrijven van de status.
Dit houdt in dat voor StatefulService implementers, `RunAsync` zal niet worden aangeroepen totdat `OnDataLossAsync` is voltooid.
Vervolgens `OnDataLossAsync` wordt aangeroepen op de nieuwe primaire.
Totdat een service sluitstuk van deze API is (waar of onwaar retourneren) en de relevante nieuwe configuratie is voltooid, de API wordt behouden die wordt aangeroepen één voor één.

`RestoreAsync` eerst komt alle bestaande statussen van de primaire replica die er is met de naam op. De betrouwbare status Manager maakt vervolgens de betrouwbare objecten die zijn opgenomen in de back-upmap. Vervolgens wordt de betrouwbare objecten terugzetten vanuit hun controlepunten in de back-upmap geïnstrueerd. Ten slotte de betrouwbare status Manager Hiermee herstelt u een eigen status van de records in logboek registreren in de back-upmap en herstel wordt uitgevoerd. Als onderdeel van het herstelproces opnieuw vanaf het "beginpunt" bewerkingen die records in logboek registreren in de back-upmap hebt doorgevoerd afgespeeld op de betrouwbare objecten. Deze stap zorgt ervoor dat de status van de herstelde consistent is.

## <a name="next-steps"></a>Volgende stappen
  - [Betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
  - [Snelstartgids voor betrouwbare Services](service-fabric-reliable-services-quick-start.md)
  - [Meldingen van betrouwbare Services](service-fabric-reliable-services-notifications.md)
  - [Configuratie van betrouwbare Services](service-fabric-reliable-services-configuration.md)
  - [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  - [Periodic backup and restore in Azure Service Fabric](service-fabric-backuprestoreservice-quickstart-azurecluster.md) (Periodieke back-up en herstel in Azure Service Fabric)

