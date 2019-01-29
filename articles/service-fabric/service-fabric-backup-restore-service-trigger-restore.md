---
title: Herstellen van back-ups in Azure Service Fabric | Microsoft Docs
description: Gebruik de periodieke back-up en herstellen van de functie in Service Fabric voor het herstellen van gegevens vanuit een back-up van uw toepassingsgegevens.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 802F55B6-6575-4AE1-8A8E-C9B03512FF88
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 1482497f3767e7533d1d56e6eb63e55cdb5c9ebb
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104640"
---
# <a name="restoring-backup-in-azure-service-fabric"></a>Herstellen van back-ups in Azure Service Fabric

Azure Service Fabric kunt betrouwbare Stateful services en Reliable Actors handhaven een veranderlijke, gezaghebbende status nadat een aanvraag en respons transactie is voltooid. Een stateful service mogelijk gedurende een lange periode uitvallen of gegevens gaan verloren vanwege een noodgeval. Als dit gebeurt, wordt de service moet worden hersteld vanuit de meest recente acceptabele back-up, zodat deze kan blijven werken.

U kunt bijvoorbeeld configureren dat een service voor het back-up van de gegevens voor bescherming tegen de volgende scenario's:

- **Geval van herstel na noodgevallen**: Permanent verlies van een hele Service Fabric-cluster.
- **Geval van verlies van gegevens**: Permanente verlies van een meerderheid van de replica's van een servicepartitie.
- **Geval van verlies van gegevens**: Per ongeluk verwijderen of beschadiging van de service. Een beheerder verwijdert bijvoorbeeld per ongeluk de service.
- **Geval van beschadiging van gegevens**: Fouten in de service kunnen gegevens beschadigd raken. Beschadiging van gegevens kan bijvoorbeeld gebeuren wanneer de upgrade van een service-code beschadigde gegevens naar een betrouwbare verzameling schrijft. In dat geval moet wellicht u zowel de code en de gegevens te herstellen naar een eerdere toestand.

## <a name="prerequisites"></a>Vereisten

- Voor het activeren van een herstelbewerking de _Fault Analysis Service (FAS)_ moet zijn ingeschakeld voor het cluster.
- De _back-up herstellen Service (BRS)_ de back-up gemaakt.
- De herstelbewerking kan alleen worden geactiveerd op een partitie.

## <a name="triggered-restore"></a>Geactiveerde herstellen

Een terugzetbewerking kan worden geactiveerd voor het gebruik van de volgende scenario's:

- Gegevens herstellen voor _herstel na noodgevallen_.
- Gegevens herstellen voor _gegevens beschadigd/gegevensverlies_.

### <a name="data-restore-in-the-case-of-disaster-recovery"></a>Het herstellen van gegevens in het geval van herstel na noodgevallen

Als een hele Service Fabric-cluster verloren gegaan is, kunt u de gegevens voor de partities van de betrouwbare Stateful service en betrouwbare actoren herstellen. De gewenste back-up kan worden geselecteerd in de lijst wanneer u [GetBackupAPI met details van de back-upopslag](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation). De back-opsomming kan zijn voor een toepassing, service of partitie.

Voor het volgende voorbeeld wordt ervan uitgegaan dat het verloren cluster hetzelfde cluster waarnaar wordt verwezen in [inschakelen periodieke back-up voor betrouwbare Stateful service en Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). In dit geval `SampleApp` wordt geïmplementeerd met back-upbeleid ingeschakeld, en de back-ups zijn geconfigureerd voor Azure Storage.

Voer een PowerShell-script voor het gebruik van de REST-API om terug te keren een lijst van de back-ups gemaakt voor alle partities in de `SampleApp` toepassing. De API vereist dat de back-upopslag-gegevens om de beschikbare back-ups weer te geven.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$BackupEntity = @{
    EntityKind = 'Application'
    ApplicationName='fabric:/SampleApp'
}

$BackupLocationAndEntityInfo = @{
    Storage = $StorageInfo
    BackupEntity = $BackupEntity
}

$body = (ConvertTo-Json $BackupLocationAndEntityInfo)
$url = "https://myalternatesfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```

Voorbeeld van uitvoer voor de bovenstaande uitvoeren:

```
BackupId                : b9577400-1131-4f88-b309-2bb1e943322c
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 20.55.16.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3334
CreationTimeUtc         : 2018-04-06T20:55:16Z
FailureError            : 
*
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
*
BackupId                : 69436834-c810-4163-9386-a7a800f78359
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.25.36.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3764
CreationTimeUtc         : 2018-04-06T21:25:36Z
FailureError            :
```

Als u wilt de terugzetbewerking kan worden geactiveerd, kiest u een van de back-ups. De huidige back-up voor herstel na noodgevallen kan bijvoorbeeld de volgende back-up:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
```

Voor de API terugzetten, moet u opgeven de _BackupId_ en _back-uplocatie_ details.

U moet ook een doelpartitie kiezen in het andere cluster zoals beschreven in de [partitieschema](service-fabric-concepts-partitioning.md#get-started-with-partitioning). De alternatieve cluster back-up wordt hersteld naar de partitie die is opgegeven in het partitieschema van het oorspronkelijke cluster gaan verloren.

Als de partitie-ID op alternatieve cluster `1c42c47f-439e-4e09-98b9-88b8f60800c6`, kunt u deze toewijzen aan de partitie-ID van het oorspronkelijke cluster `974bd92a-b395-4631-8a7f-53bd4ae9cf22` door het vergelijken van de hoge en lage sleutel voor _(UniformInt64Partition) variabele partitioneren_.

Voor _met de naam partitioneren_, de naamwaarde voor het identificeren van de doelpartitie in andere cluster wordt vergeleken.

U het herstel op basis van de partitie van de back-cluster met behulp van de volgende aanvragen [herstellen API](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition):

```powershell
$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
}

$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

U kunt de voortgang van een herstelpunt met volgen [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress).

### <a name="data-restore-for-data-corruptiondata-loss"></a>Gegevens herstellen voor _gegevensbeschadiging_/_verlies van gegevens_

Voor _gegevensverlies_ of _gegevensbeschadiging_, partities op de back-up voor betrouwbare Stateful service en Reliable Actors-partities kunnen worden hersteld naar een van de gekozen back-ups.

Het volgende voorbeeld is een vervolg van [inschakelen periodieke back-up voor betrouwbare Stateful service en Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). In dit voorbeeld wordt een back-upbeleid is ingeschakeld voor de partitie en de service is back-ups maken met een gewenste frequentie in Azure Storage.

Selecteer een back-up uit de uitvoer van [GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups). In dit scenario wordt de back-up van hetzelfde cluster als voordat gegenereerd.

Als u wilt de terugzetbewerking kan worden geactiveerd, kiest u een back-up in de lijst. Voor de huidige _gegevensverlies_/_gegevensbeschadiging_, selecteert u de volgende back-up:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
```

Voor de restore-API, geeft u de _BackupId_ en _back-uplocatie_ details. Het cluster heeft back-up waardoor de Service Fabric _back-up herstellen Service (BRS)_ identificeert de juiste opslaglocatie van de gekoppelde back-upbeleid.

```powershell
$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4',
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
}

$body = (ConvertTo-Json $RestorePartitionReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

U kunt de voortgang terugzetten volgen met behulp van [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress).

## <a name="track-restore-progress"></a>Restore-voortgang bijhouden

Een partitie van een betrouwbare Stateful service of een Reliable Actor accepteert slechts één aanvraag voor herstellen op een tijdstip. Een partitie accepteert alleen een andere aanvraag na de huidige restore-aanvraag is voltooid. Meerdere aanvragen van de terugzetbewerking kunnen worden geactiveerd in verschillende partities op hetzelfde moment.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

$restoreResponse = (ConvertFrom-Json $response.Content)
$restoreResponse | Format-List
```

De voortgang van de aanvraag voor herstellen in de volgende volgorde:

1. **Geaccepteerd**: Een _geaccepteerde_ terugzetten heeft, betekent dit dat de gevraagde partitie met de parameters van de juiste aanvraag is geactiveerd.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
2. **InProgress**: Een _InProgress_ terugzetten heeft, betekent dit dat een herstelbewerking in de partitie met de back-up die worden vermeld in de aanvraag plaatsvindt. De partitie-rapporten de _dataloss_ staat.
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. **Succes**, **fout**, of **time-out**: Een aangevraagde terugzetbewerking kan worden uitgevoerd in een van de volgende statussen. Elke Amerikaanse staat zijn de gegevens van de volgende betekenis en antwoord:
    - **Succes**: Een _succes_ terugzetten status geeft de status van een partitie heeft gekregen. De partitie-rapporten _RestoredEpoch_ en _RestoredLSN_ Staten samen met de tijd in UTC.

        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```        
    - **Fout**: Een _fout_ status herstellen geeft aan dat de uitval van de restore-aanvraag. De oorzaak van de fout wordt gerapporteerd.

        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    - **Time-out**: Een _time-out_ terugzetten heeft, betekent dit dat de aanvraag heeft een time-out. Maak een nieuwe aanvraag voor herstellen met groter [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout). De standaardtime-out is 10 minuten. Zorg ervoor dat de partitie niet in een status van de verlies van gegevens voordat u opnieuw herstel aanvraagt.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="automatic-restore"></a>Automatisch herstel

U kunt betrouwbare Stateful service configureren en Reliable Actors-partities in de Service Fabric-cluster voor _automatisch herstel_. Stel in het back-upbeleid `AutoRestore` naar _waar_. Inschakelen van _automatisch herstel_ herstelt automatisch gegevens van de meest recente partitie back-up wanneer verlies van gegevens wordt gemeld. Zie voor meer informatie:

- [Automatisch herstel activering in de back-upbeleid](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)
- [RestorePartition API-verwijzing](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
- [GetPartitionRestoreProgress API-verwijzing](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Volgende stappen
- [Understanding periodieke back-upconfiguratie](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
