---
title: Service Fabric Backup and Restore | Microsoft Docs
description: Conceptuele documentatie voor Service Fabric-back-up en herstel
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: subramar,jessebenson
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: mcoskun
ms.openlocfilehash: 8d81abec1c879ac6edbd4610dafdfd43ec7cf903
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-and-restore-reliable-services-and-reliable-actors"></a>Back-up en herstel Reliable Services en Reliable Actors
Azure Service Fabric is een hoge beschikbaarheid-platform die de status repliceert op meerdere knooppunten om deze hoge beschikbaarheid te houden.  Dus zelfs als een knooppunt in het cluster is mislukt, blijven de services beschikbaar. Bij deze ingebouwde redundantie geleverd door het platform is mogelijk niet voldoende is voor sommige, in bepaalde gevallen is het wenselijk voor de service voor back-ups (naar een externe winkel).

> [!NOTE]
> Het is essentieel voor back-up en herstellen van uw gegevens (en test werkt zoals verwacht) zodat u vanaf gegevensverlies herstellen kunt.
> 
> 

Een service wilt bijvoorbeeld een back-up van gegevens om te voorkomen dat de volgende scenario's:

- In geval van een permanent verlies van een volledige Service Fabric-cluster.
- Permanent verlies van een meerderheid van de replica's van de partitie van een service
- Administratieve fouten waarbij de status per ongeluk wordt verwijderd of beschadigd. Dit kan bijvoorbeeld gebeuren als een beheerder met voldoende bevoegdheden per ongeluk worden verwijderd van de service.
- Fouten in de service die leiden gegevensbeschadiging van tot. Dit kan bijvoorbeeld gebeuren wanneer een code-upgrade van service wordt gestart met het schrijven van beschadigde gegevens naar een betrouwbare verzameling. In dat geval zowel de code en de gegevens mogelijk teruggezet naar een eerdere status.
- De gegevensverwerking is offline. Kan het zijn handig zijn als offline verwerking van gegevens voor business intelligence die plaatsvindt afzonderlijk van de service die de gegevens genereert.

De functie back-up/herstel kan services die zijn gebaseerd op de API voor betrouwbare Services maken en herstellen van back-ups. De back-API's die wordt geleverd door het platform toestaan backup(s) van status van een service-partitie, zonder blokkerende lees- of schrijfbewerkingen. Het terugzetten van API's maken van de partitie van een service-status moet worden teruggezet vanuit een gekozen back-up.

## <a name="types-of-backup"></a>Typen back-ups
Er zijn twee opties voor back-up: volledige en incrementele.
Een volledige back-up is een back-up waarin alle gegevens die nodig zijn om de status van de replica opnieuw te maken: controlepunten en alle records in het logboek.
Aangezien deze de controlepunten en het logboekbestand heeft, kan een volledige back-up kan worden hersteld door zichzelf.

Het probleem met volledige back-ups treedt op wanneer de controlepunten groot zijn.
Een replica met 16 GB status heeft bijvoorbeeld controlepunten die toevoegen tot ongeveer 16 GB.
Als we een beoogd herstelpunt van vijf minuten hebt, moet de replica een back-up om de vijf minuten.
Telkens wanneer er een back-up moet worden gekopieerd van 16 GB van controlepunten naast 50 MB (met behulp van configureerbare `CheckpointThresholdInMB`) logboeken aan.

![Voorbeeld van de volledige back-up.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

De oplossing voor dit probleem is incrementele back-ups wanneer back-up bevat alleen de gewijzigde logboekrecords sinds de laatste back-up.

![Incrementele back-voorbeeld.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Aangezien incrementele back-ups alleen wijzigingen sinds de laatste back-up (omvat geen controlepunten), ze vaak sneller zijn, maar ze kunnen niet worden teruggezet op hun eigen.
Als u een incrementele back-up herstellen, is de volledige back-keten vereist.
Een back-keten is een keten van back-ups die beginnen met een volledige back-up en gevolgd door een aantal aaneengesloten incrementele back-ups.

## <a name="backup-reliable-services"></a>Back-Reliable Services
De auteur van de service heeft volledig beheer van bij het maken van back-ups en waar back-ups worden opgeslagen.

Als u een back-up wilt, moet de service aanroepen van de functie overgenomen lid `BackupAsync`.  
Back-ups kunnen alleen vanaf de primaire replica's worden gemaakt, en ze vereisen schrijven status om te worden toegekend.

Zoals hieronder aangegeven, `BackupAsync` wordt in een `BackupDescription` object, een waar een volledige of incrementele back-up, evenals een callback-functie opgeven kunt, `Func<< BackupInfo, CancellationToken, Task<bool>>>` die wordt geactiveerd wanneer de back-upmap lokaal is gemaakt en gereed om te worden verplaatst is uit op sommige externe opslag.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Aanvraag voor het uitvoeren van een incrementele back-up mislukken met `FabricMissingFullBackupException`. Deze uitzondering geeft aan dat er een van de volgende dingen gebeurt:

- de replica is nooit gemaakt van een volledige back-up nadat primaire geworden,
- Sommige van de logboekrecords sinds de laatste back-up is afgekapt of
- replica doorgegeven de `MaxAccumulatedBackupLogSizeInMB` limiet.

Gebruikers kunnen vergroot de kans kunt geen incrementele back-ups configureren `MinLogSizeInMB` of `TruncationThresholdFactor`.
Opmerking dat uitbreiding van deze verhoogt waarden de per replica schijfgebruik.
Zie voor meer informatie [betrouwbare configuratie van Services](service-fabric-reliable-services-configuration.md)

`BackupInfo`bevat informatie over de back-up, waaronder de locatie van de map waarin de back-up in de runtime worden opgeslagen (`BackupInfo.Directory`). De callback-functie kunt verplaatsen de `BackupInfo.Directory` naar een externe winkel of een andere locatie.  Deze functie wordt ook een Boole-waarde die aangeeft of het kunnen de back-upmap is verplaatsen naar de doellocatie.

De volgende code laat zien hoe de `BackupCallbackAsync` methode kan worden gebruikt voor het uploaden van de back-up naar Azure Storage:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

In het voorgaande voorbeeld `ExternalBackupStore` is de voorbeeldklasse die wordt gebruikt voor de interface met Azure Blob storage en `UploadBackupFolderAsync` is de methode die de map gecomprimeerd en plaatst het in de Azure Blob-opslag.

Opmerking:

  - Er mag slechts één back-upbewerking onderweg per replica op elk moment. Meer dan één `BackupAsync` aanroep tegelijk genereert `FabricBackupInProgressException` inflight back-ups op een beperken.
  - Als een replica mislukt via tijdens een back-up uitgevoerd wordt, kan de back-up niet zijn voltooid. Zodra de failover is voltooid, het is dus de verantwoordelijkheid van de service opnieuw starten van de back-up door aan te roepen `BackupAsync` indien nodig.

## <a name="restore-reliable-services"></a>Reliable Services herstellen
In het algemeen vallen de gevallen wanneer moet u mogelijk een herstelbewerking uitvoeren in een van de volgende categorieën:

  - De service-partitie gegevens verloren gegaan. De schijf voor twee van de drie replica's voor een partitie (met inbegrip van de primaire replica) opgehaald bijvoorbeeld beschadigd of gewist. De nieuwe primaire moet mogelijk gegevens terugzetten vanuit een back-up.
  - De hele service wordt verbroken. Bijvoorbeeld: een beheerder verwijdert de hele service, en dus de service en de gegevens moeten worden teruggezet.
  - De service gerepliceerd beschadigd toepassingsgegevens (bijv, vanwege een fout van toepassing). In dit geval wordt de service worden bijgewerkt of hersteld voor het verwijderen van de oorzaak van de beschadiging is, en niet beschadigd gegevens moet worden hersteld.

Hoewel veel manieren mogelijk, bieden we enkele voorbeelden voor het gebruik van `RestoreAsync` van de bovenstaande scenario's wilt herstellen.

## <a name="partition-data-loss-in-reliable-services"></a>Partitie gegevensverlies in Reliable Services
In dit geval de runtime automatisch wilt detecteren het verlies van gegevens en aanroepen de `OnDataLossAsync` API.

De auteur van de service moet uitvoeren van de volgende herstellen:

  - Negeer de methode virtuele basisklasse `OnDataLossAsync`.
  - De meest recente back-up niet vinden in de externe locatie waarin de back-ups van de service.
  - Download de meest recente back-up (en de back-up in de back-upmap decomprimeren als het is gecomprimeerd).
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

`RestoreDescription`de doorgegeven aan de `RestoreContext.RestoreAsync` aanroep van een lid genaamd bevat `BackupFolderPath`.
Bij het herstellen van een eenmalige volledige back-up, dit `BackupFolderPath` moet worden ingesteld op het lokale pad naar de map waarin uw volledige back-up.
Bij het herstellen van een volledige back-up en een aantal incrementele back-ups `BackupFolderPath` moet worden ingesteld op het lokale pad naar de map waarin niet alleen de volledige back-up, maar ook alle incrementele back-ups.
`RestoreAsync`aanroep kunt throw `FabricMissingFullBackupException` als de `BackupFolderPath` opgegeven bevat geen een volledige back-up.
Het kan ook de throw `ArgumentException` als `BackupFolderPath` heeft een verbroken keten van incrementele back-ups.
Bijvoorbeeld, als deze de volledige back-up bevat de eerste incrementele en de derde incrementele back-up, maar geen de tweede incrementele back-up.

> [!NOTE]
> De RestorePolicy is standaard ingesteld op de kluis.  Dit betekent dat de `RestoreAsync` API mislukt met ArgumentException als wordt gedetecteerd dat de back-upmap een status die ouder is dan of gelijk zijn aan de status van deze replica bevat.  `RestorePolicy.Force`kan worden gebruikt om deze veiligheidscontrole overslaan. Dit is opgegeven als onderdeel van `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>Verwijderde of verloren-service
Als een service wordt verwijderd, moet u eerst opnieuw maken de service voordat u de gegevens kunnen worden hersteld.  Het is belangrijk voor het maken van de service met dezelfde configuratie, bijvoorbeeld partitieschema, zodat de gegevens naadloos kan worden hersteld.  Zodra de service is, de API om gegevens te herstellen (`OnDataLossAsync` hierboven) moet worden aangeroepen voor elke partitie van deze service. Een manier om dat te bereiken dit is met behulp van `[FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx)` op elke partitie.  

Vanaf dit punt is implementatie hetzelfde als het bovenstaande scenario. Elke partitie moet de meest relevante back-up terugzetten vanaf de externe winkel. Een voorbehoud is dat de partitie-ID mogelijk is nu gewijzigd, omdat de runtime partitie-id dynamisch maakt. Daarom moet de service voor het opslaan van de juiste informatie en service partitienaam voor het identificeren van de juiste meest recente back-up wilt herstellen uit voor elke partitie.

> [!NOTE]
> Het wordt niet aangeraden om te gebruiken `FabricClient.ServiceManager.InvokeDataLossAsync` op elke partitie te herstellen van de hele service, omdat dat de clusterstatus mogelijk beschadigd.
> 

## <a name="replication-of-corrupt-application-data"></a>Replicatie van gegevens beschadigd toepassing
Als de upgrade van de geïmplementeerde toepassing een fout heeft, die mogelijk leiden tot beschadiging van gegevens. Een upgrade van de toepassing kan bijvoorbeeld starten record bij te werken elke telefoon nummer in een betrouwbare woordenlijst met een ongeldige netnummer.  In dit geval wordt de ongeldige telefoonnummers gerepliceerd sinds de Service Fabric is niet op de hoogte van de aard van de gegevens die worden opgeslagen.

Het eerste wat te doen nadat u deze een egregious fout die ervoor zorgt gegevensbeschadiging dat detecteren is het blokkeren van de service op toepassingsniveau en, indien mogelijk een upgrade uitvoert naar de versie van de toepassingscode die beschikt niet over de fout.  Zelfs nadat de servicecode is opgelost, de gegevens mogelijk nog steeds beschadigd en dus gegevens moeten mogelijk worden hersteld.  In dergelijke gevallen kan het niet toereikend zijn de meest recente back-up herstellen sinds de meest recente back-ups ook mogelijk beschadigd.  U moet dus vinden van de laatste back-up die is gemaakt voordat de gegevens zijn beschadigd.

Als u niet zeker welke back-ups zijn beschadigd weet, kan u een nieuwe Service Fabric-cluster implementeren en herstel van de back-ups van betrokken partities net als de bovenstaande 'Deleted of verloren service' scenario.  Voor elke partitie terug te zetten van de back-ups van de meest recente minst. Als u een back-up die geen beschadiging heeft gevonden, verplaatsen en verwijderen alle back-ups van deze partitie die recenter (dan back-up waren). Herhaal dit proces voor elke partitie. Nu als `OnDataLossAsync` wordt aangeroepen op de partitie in de productiecluster, de laatste back-up zijn gevonden in de externe opslag zal worden verzameld door de bovenstaande procedure.

Nu de stappen in 'Deleted of verloren service' sectie kan worden gebruikt voor het herstellen van de status van de service op de status voordat de status van de buggy code beschadigd.

Opmerking:

  - Wanneer u herstelt, is er een kans is dat de back-up wordt hersteld ouder is dan de status van de partitie is voordat de gegevens verloren gegaan is. Daarom moet u alleen als een laatste toevlucht herstellen zoveel mogelijk gegevens weer.
  - De tekenreeks met het pad van de back-up en de paden van bestanden in de back-upmap kan niet groter zijn dan 255 tekens, afhankelijk van het pad FabricDataRoot en toepassingstype naamlengte. Hierdoor kunnen sommige .NET-methoden zoals `Directory.Move`, om de `PathTooLongException` uitzondering. Een tijdelijke oplossing is om aan te roepen rechtstreeks kan kernel32-API's, zoals `CopyFile`.

## <a name="backup-and-restore-reliable-actors"></a>Back-up en herstel Reliable Actors


Betrouwbare actoren Framework is ingebouwd in Reliable Services. De ActorService die als host fungeert voor de actor(s) is een stateful betrouwbare service. Alle back-up en herstel functionaliteit beschikbaar zijn in Reliable Services is daarom ook beschikbaar voor Reliable Actors (met uitzondering van het gedrag dat specifieke state-provider zijn). Sinds de back-ups zullen worden uitgevoerd op basis van de per-partitie, statussen voor alle actoren in de betreffende partitie, worden back-up (en herstel lijkt en gebeurt op basis van per partitie). Als u wilt uitvoeren van back-up/herstel eigenaar van de service moet een aangepaste actor serviceklasse maken die is afgeleid van klasse ActorService en voer back-up/herstel vergelijkbaar met Reliable Services zoals hierboven is beschreven in de vorige secties.

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

Wanneer u een aangepaste actor serviceklasse maakt, moet u die ook registreren bij het registreren van de actor.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
   (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

De standaardprovider voor de status voor Reliable Actors `KvsActorStateProvider`. Incrementele back-up is niet standaard ingeschakeld voor `KvsActorStateProvider`. U kunt de incrementele back-up inschakelen door het maken van `KvsActorStateProvider` met de juiste instelling in de constructor en deze vervolgens doorgegeven aan de constructor ActorService zoals weergegeven in het volgende codefragment:

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

Nadat de incrementele back-up is ingeschakeld, duurt een incrementele back-up voor een van de volgende redenen mislukken met FabricMissingFullBackupException en moet u een volledige back-up uitvoeren voordat u incrementele backup(s):

  - De replica is nooit een volledige back-up genomen nadat primaire werd.
  - Sommige van de records in het logboek zijn afgebroken sinds laatste back-up is gemaakt.

Wanneer incrementele back-up is ingeschakeld, `KvsActorStateProvider` gebruikt geen circulaire buffer voor het beheren van de records in het logboek en regelmatig worden afgekapt. Als geen back-up wordt gemaakt door gebruiker gedurende een periode van 45 minuten, wordt de logboekrecords automatisch afgekapt door het systeem. Dit interval kan worden geconfigureerd door op te geven `logTrunctationIntervalInMinutes` in `KvsActorStateProvider` constructor (net als bij het inschakelen van incrementele back-up). De logboekrecords kunnen ook ophalen afgekapt als primaire replica maken van een andere replica wilt door alle bijbehorende gegevens te verzenden.

Bij het uitvoeren van herstel van een back-keten, vergelijkbaar met Reliable Services de BackupFolderPath moet bevatten submappen met een submap met volledige back-up en anderen die incrementele backup(s) submappen. De API terugzetten genereert FabricException met het bijbehorende bericht als de back-up van de certificaatketen is mislukt. 

> [!NOTE]
> `KvsActorStateProvider`op dit moment wordt de optie RestorePolicy.Safe genegeerd. Ondersteuning voor deze functie is in een toekomstige release gepland.
> 

## <a name="testing-backup-and-restore"></a>Testen van back-up en herstel
Het is belangrijk om ervoor te zorgen dat essentiële gegevens wordt back-up en kan worden hersteld vanuit. Dit kan worden gedaan door het aanroepen van de `Start-ServiceFabricPartitionDataLoss` cmdlet in PowerShell die verlies van gegevens in een bepaalde partitie om te controleren of de gegevens back-up en herstellen van de functionaliteit voor uw service werkt zoals verwacht kan veroorzaken.  Het is ook mogelijk om programmatisch aanroepen verlies van gegevens en herstellen van die gebeurtenis ook.

> [!NOTE]
> U vindt een Voorbeeldimplementatie van back-up en herstel functionaliteit in de Web-App voor verwijzing op GitHub. Bekijk de `Inventory.Service` service voor meer informatie.
> 
> 

## <a name="under-the-hood-more-details-on-backup-and-restore"></a>Achter de schermen: meer informatie over back-up en herstel
Hier volgt een aantal meer informatie over back-up en herstel.

### <a name="backup"></a>Back-up
Statusbeheer voor het betrouwbare biedt de mogelijkheid te consistente back-ups maken zonder blokkering van alle lees- of schrijfbewerkingen. Om dit te doen, maakt het gebruik van een mechanisme controlepunt- en logboekbestanden.  Statusbeheer voor het betrouwbare duurt fuzzy (lightweight) controlepunten op bepaalde tijdstippen te ontlasten zware belasting van het logboek voor transactionele en hersteltijden te verbeteren.  Wanneer `BackupAsync` wordt aangeroepen, statusbeheer voor het betrouwbare Hiermee geeft u alle objecten van betrouwbare hun meest recente om controlepuntbestanden te kopiëren naar een lokale map voor back-up.  Statusbeheer voor het betrouwbare kopieert vervolgens alle logboekrecords vanaf de aanwijzer' start' naar de meest recente logboekrecord in de back-upmap.  Aangezien de logboekrecords tot de meest recente logboekrecord zijn opgenomen in de back-up en betrouwbare statusbeheer voor het vooraf geschreven logboekregistratie behoudt, betrouwbare statusbeheer voor het zorgt ervoor dat alle transacties die toegewezen zijn (`CommitAsync` met succes heeft geretourneerd ) zijn opgenomen in de back-up.

De transactie die na doorvoeren `BackupAsync` mei is aangeroepen of mogelijk niet in de back-up.  Zodra de lokale back-upmap is gevuld door het platform (dat wil zeggen, lokale back-up is voltooid door de runtime), back-up van de service-callback is aangeroepen.  Deze retouraanroep is verantwoordelijk voor de back-map verplaatst naar een externe locatie zoals Azure Storage.

### <a name="restore"></a>Herstellen
Statusbeheer voor het betrouwbare biedt de mogelijkheid om terug te zetten vanuit een back-up met behulp van de `RestoreAsync` API.  
De `RestoreAsync` methode op `RestoreContext` kan worden aangeroepen alleen in de `OnDataLossAsync` methode.
De bool geretourneerd door `OnDataLossAsync` geeft aan of de service hersteld van de status van een externe bron.
Als de `OnDataLossAsync` true retourneert, Service Fabric alle andere replica's van deze primaire opnieuw opgebouwd. Service Fabric zorgt ervoor dat replica's die worden ontvangen `OnDataLossAsync` aanroepen van eerste overgang naar de primaire rol, maar zijn niet verleend status lezen of schrijven status.
Dit houdt in dat voor StatefulService implementeerders, `RunAsync` wordt niet aangeroepen tot `OnDataLossAsync` met succes wordt voltooid.
Vervolgens `OnDataLossAsync` wordt aangeroepen op de nieuwe primaire.
Totdat een service deze API is (met retourneert true of false voltooit) en de relevante herconfiguratie is voltooid, wordt de API behouden wordt aangeroepen één tegelijk.

`RestoreAsync`alle bestaande statussen van de primaire replica die is aangeroepen op eerst verwijderd.  
Statusbeheer voor het betrouwbare maakt vervolgens de betrouwbare objecten die zijn opgenomen in de back-upmap.  
Vervolgens wordt de betrouwbare objecten terugzetten vanaf hun controlepunten in de back-upmap geïnstrueerd.  
Ten slotte betrouwbare statusbeheer voor het eigen status vanuit de logboekrecords in de back-upmap herstelt en wordt een herstelbewerking uitgevoerd.  
Als onderdeel van het herstelproces worden vanaf het 'beginpunt' bewerkingen waarvoor logboekrecords doorvoeren in de back-upmap cookies op betrouwbare objecten.  
Deze stap zorgt ervoor dat de status van de herstelde consistent is.

## <a name="next-steps"></a>Volgende stappen
  - [Betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
  - [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start.md)
  - [Betrouwbare Services meldingen](service-fabric-reliable-services-notifications.md)
  - [Configuratie van betrouwbare Services](service-fabric-reliable-services-configuration.md)
  - [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

