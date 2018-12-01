---
title: Herstellen van back-ups in Azure Service Fabric | Microsoft Docs
description: Gebruik van Service Fabric periodieke back-up en herstellen van de functie voor het herstellen van gegevens van back-up van uw toepassingsgegevens.
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
ms.openlocfilehash: d0b385e5856e85ad0d7d9baeaddbac0fb4230e3b
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730155"
---
#  <a name="restoring-backup-in-azure-service-fabric"></a>Herstellen van back-ups in Azure Service Fabric


Betrouwbare Stateful services en Reliable Actors in Service Fabric kan veranderlijke, gezaghebbende status, buiten de aanvraag en antwoord of een volledige transactie onderhouden. Als een stateful service gedurende een lange periode uitvalt of gegevens vanwege een noodgeval verliest, moet deze mogelijk worden hersteld naar de meest recente acceptabele back-up van de status om door te gaan met het leveren van de service nadat deze terugkomen.

Service wilt bijvoorbeeld back-up van de gegevens om u te beschermen tegen de volgende scenario's:

- In het geval van het permanente verlies van gegevens van een hele Service Fabric-cluster. **(De aanvraag van herstel na noodgevallen - DR)**
- Permanente verlies van een meerderheid van de replica's van een servicepartitie. **(In geval van verlies van gegevens)**
- Administratieve fouten waarbij de status per ongeluk wordt verwijderd of beschadigd. Een beheerder met voldoende bevoegdheden verwijdert bijvoorbeeld per ongeluk de service. **(Geval van verlies van gegevens)**
- Fouten in de service die leiden gegevensbeschadiging tot. Beschadiging van gegevens kan bijvoorbeeld gebeuren wanneer een upgrade van de code service begint beschadigde gegevens schrijven naar een betrouwbare verzameling. In dat geval, zowel de code en de gegevens mogelijk teruggezet naar een eerdere toestand. **(In geval van beschadiging van gegevens)**


## <a name="prerequisites"></a>Vereisten
* Herstellen als u wilt activeren, de _Fault Analysis Service (FAS)_ moet worden ingeschakeld voor het cluster
* De back-up moet terugzetten moet worden genomen door _back-up herstellen Service (BRS)_
* De herstelbewerking kan alleen worden geactiveerd op een partitie.

## <a name="triggering-restore"></a>Activeren van de herstelbewerking

De terugzetbewerking kan worden voor het gebruik van de volgende scenario 's 
* Gegevens herstellen in geval van _herstel na noodgevallen_ (DR)
* Gegevens herstellen in geval van _gegevensbeschadiging / verlies van gegevens_



### <a name="data-restore-in-the-event-of-disaster-recovery-dr"></a>Gegevens herstellen in geval van _herstel na noodgevallen_ (DR)
In het geval van een hele Service Fabric-cluster wordt verbroken, kunnen de gegevens voor de partities van de betrouwbare Stateful service en Reliable Actors worden hersteld naar een ander cluster. De gewenste back-up kan worden geselecteerd in de inventarisatie van [GetBackupAPI met Details van de back-upopslag](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation). De back-up-opsomming kan zijn voor een toepassing, service of partitie.

Stel het verloren cluster is het cluster dat wordt vermeld in [inschakelen periodieke back-up voor betrouwbare Stateful service en Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), die waren `SampleApp` geïmplementeerd, waarbij de partitie is dat back-upbeleid ingeschakeld en back-ups plaatsvinden in Azure Storage. 


Voer de volgende PowerShell-script voor het aanroepen van de REST-API voor het inventariseren van de back-ups gemaakt voor alle partities in de `SampleApp` toepassing in Service Fabric-cluster gaan verloren. De opsomming API vereist opslaggegevens, waarin de back-ups van een toepassing worden opgeslagen, voor het inventariseren van beschikbare back-ups. 

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



Voor het activeren van het herstellen, moeten we Kies de gewenste back-up. Laat de gewenste back-up voor de huidige noodherstel (DR) zijn de volgende back-up

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

Voor de API terugzetten, moet u aan de __BackupId__ en __back-uplocatie__ details. De partitie in de alternatieve cluster moet gekozen volgens de [partitieschema](service-fabric-concepts-partitioning.md#get-started-with-partitioning). Het is de verantwoordelijkheid van de gebruiker op de doelpartitie om terug te zetten van de back-up van het andere cluster aan de hand van het partitieschema in oorspronkelijke verloren cluster hebt gekozen.

Wordt ervan uitgegaan dat de partitie-ID op alternatieve cluster `1c42c47f-439e-4e09-98b9-88b8f60800c6`, die verwijst naar de partitie-ID van het oorspronkelijke cluster `974bd92a-b395-4631-8a7f-53bd4ae9cf22` door het vergelijken van de hoge en lage sleutel voor _(UniformInt64Partition) variabele partitioneren_.

Voor _met de naam partitioneren_, de naamwaarde voor het identificeren van de doelpartitie in andere cluster wordt vergeleken.

Het herstel op basis van de partitie van de back-cluster is aangevraagd door de volgende [API herstellen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)

```powershell 
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 
De voortgang van de terugzetbewerking kan worden [TrackRestoreProgress](service-fabric-backuprestoreservice-trigger-restore.md#tracking-restore-progress)

### <a name="data-restore-in-the-event-of-data-corruption--data-loss"></a>Gegevens herstellen in geval van _gegevensbeschadiging / verlies van gegevens_

Voor het geval van _gegevensverlies_ of _gegevensbeschadiging_ de gegevens voor de partities van de betrouwbare Stateful service en Reliable Actors kunnen worden hersteld naar een van de gekozen back-ups. Het volgende voorbeeld is de voortzetting van voorbeeld zoals vermeld in [inschakelen periodieke back-up voor betrouwbare Stateful service en Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), waarbij de partitie een back-upbeleid ingeschakeld heeft en back-up met een gewenste frequentie duurt in een Azure-opslag. 

De gewenste back-up is geselecteerd in de uitvoer van [GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups). In dit scenario wordt is de back-up gegenereerd op basis van hetzelfde cluster in het verleden.
Voor het activeren van het herstellen, moeten we de gewenste back-up in de lijst kiezen. Laat ons gewenste back-up voor de huidige _gegevensverlies_ / _gegevensbeschadiging_ worden de volgende back-up

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

Voor de API terugzetten, moet u aan de __BackupId__ en __back-uplocatie__ details. Nadat het cluster heeft back-up ingeschakeld voor de Service Fabric _back-up herstellen Service (BRS)_ identificeert de juiste opslaglocatie van de gekoppelde back-upbeleid.

```powershell
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4', 
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

De voortgang van de terugzetbewerking kan worden [TrackRestoreProgress](service-fabric-backuprestoreservice-trigger-restore.md#tracking-restore-progress)


## <a name="tracking-restore-progress"></a>Bijhouden van de voortgang herstellen

Een partitie van een betrouwbare Stateful service of een Reliable Actor accepteert slechts één aanvraag voor herstellen op een tijdstip. Een nieuwe aanvraag kan worden geaccepteerd, alleen wanneer de huidige restore-aanvraag is voltooid. Meerdere aanvragen van de herstelbewerking kunnen worden geactiveerd in verschillende partities op een moment.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
 
$restoreResponse = (ConvertFrom-Json $response.Content) 
$restoreResponse | Format-List
```

de voortgang van de aanvraag voor herstellen in de volgende volgorde

1. __Geaccepteerde__ -de status herstellen als _geaccepteerde_ geeft aan dat de aanvraag is geactiveerd met de juiste aanvraagparameters.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
2. __InProgress__ -de status herstellen als _InProgress_ geeft aan dat de partitie een terugzetbewerking met de back-up die worden vermeld in de aanvraag ietwat. De partitie ontvangt een melding _dataloss_ staat.
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. __Succes__/ __fout__/ __time-out__ -een aangevraagde terugzetbewerking kan worden uitgevoerd in een van de volgende statussen. Elke Amerikaanse staat zijn de gegevens van de volgende betekenis en reactie.
       
    * __Succes__ -de status herstellen als _succes_ geeft aan dat de status van de partitie is hersteld. Het antwoord biedt RestoreEpoch en RestordLSN voor de partitie, samen met de tijd in UTC. 
    
        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```
        
    *. __Fout__ -de status herstellen als _fout_ geeft aan dat de uitval van de restore-aanvraag. De oorzaak van het probleem zal worden vermeld in de aanvraag.
        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    *. __Time-out__ -de status herstellen als _time-out_ geeft aan dat de aanvraag heeft de timeout. Nieuwe restore-aanvraag met groter [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) wordt aanbevolen; standaard time-out is 10 minuten. Het wordt aanbevolen om ervoor te zorgen dat de partitie uit gegevens verloren gaan status, is voordat u opnieuw herstel aanvraagt.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="auto-restore"></a>Automatisch herstel

De partities voor de betrouwbare Stateful service en de betrouwbare actoren in het Service Fabric-cluster kunnen worden geconfigureerd voor _automatisch herstel_. Tijdens het maken van het back-upbeleid, het beleid kunt hebben `AutoRestore` ingesteld op _waar_.  Inschakelen van _automatisch herstel_ voor een partitie, herstelt u de gegevens van de meest recente back-up als verlies van gegevens wordt gerapporteerd.
 
 [Automatisch herstel activering in de back-upbeleid](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)


[RestorePartition API-verwijzing](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
[GetPartitionRestoreProgress API-verwijzing](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Volgende stappen
- [Understanding periodieke back-upconfiguratie](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
