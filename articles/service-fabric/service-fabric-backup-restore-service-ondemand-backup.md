---
title: On-demand back-ups in Azure Service Fabric | Microsoft Docs
description: Gebruik van de back-up en herstellen van de functie in Service Fabric back-up van uw toepassingsgegevens op basis van behoefte.
services: service-fabric
documentationcenter: .net
author: aagup
manager: chackdan
editor: aagup
ms.assetid: 02DA262A-EEF6-4F90-842E-FFC4A09003E5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: c80a9ac30e79607d2a255debf73f6542df7c6498
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666573"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>On-demand back-ups in Azure Service Fabric

U kunt back-ups van betrouwbare Stateful services en Reliable Actors voor noodherstel of gegevens verloren gaan.

Azure Service Fabric bevat functies voor de [periodieke back-up van gegevens](service-fabric-backuprestoreservice-quickstart-azurecluster.md) en de back-up van gegevens op basis van behoefte. Back-up op aanvraag is handig, omdat deze biedt bescherming tegen de _gegevensverlies_/_gegevensbeschadiging_ vanwege geplande wijzigingen in de onderliggende service of de omgeving.

De back-up op aanvraag-functies zijn nuttig voor het vastleggen van de status van de services voordat u een service of de bewerking voor service-omgeving voor het handmatig activeren. Bijvoorbeeld, als u een wijziging aanbrengt in de binaire bestanden bij het upgraden of downgraden van de service. In dat geval kunt u back-up op aanvraag beter beschermen van de gegevens tegen beschadiging door toepassing code bugs.

## <a name="triggering-on-demand-backup"></a>Back-up op aanvraag activeren

Back-up op aanvraag is opslaggegevens vereist voor het uploaden van back-upbestanden. U opgeven de back-uplocatie op aanvraag in de periodieke back-upbeleid of in een on-demand back-upaanvraag.

### <a name="on-demand-backup-to-storage-specified-by-a-periodic-backup-policy"></a>On-demand back-up naar de opslag die zijn opgegeven door een periodieke back-upbeleid

U kunt de periodieke back-upbeleid voor het gebruik van een partitie van een betrouwbare Stateful service of een Reliable Actor voor extra on-demand back-up naar de opslag configureren.

Het volgende voorbeeld is de voortzetting van het scenario in [inschakelen periodieke back-up voor betrouwbare Stateful service en Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). In dit geval u een back-upbeleid te gebruiken van een partitie inschakelen en een back-up wordt uitgevoerd op een frequentie instellen in Azure Storage.

Gebruik de [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API om in te stellen voor de on-demand back-up activeren voor partitie-ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Gebruik de [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API voor het inschakelen van bijhouden voor de [back-up wordt uitgevoerd op aanvraag](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress).

### <a name="on-demand-backup-to-specified-storage"></a>On-demand back-up naar de opgegeven opslag

U kunt on-demand back-up voor een partitie van een betrouwbare Stateful service of een Reliable Actor aanvragen. Geef de opslaggegevens als onderdeel van de back-upaanvraag op aanvraag.

Gebruik de [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API om in te stellen voor de on-demand back-up activeren voor partitie-ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22`. Voeg de volgende Azure Storage-informatie:

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

U kunt de [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API voor het instellen van bijhouden voor de [back-up wordt uitgevoerd op aanvraag](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress).

## <a name="tracking-on-demand-backup-progress"></a>Back-up op aanvraag de voortgang volgen

Een partitie van een betrouwbare Stateful service of een Reliable Actor accepteert slechts één on-demand back-upaanvraag tegelijk. Een nieuwe aanvraag kan worden geaccepteerd nadat de huidige back-up op aanvraag-aanvraag is voltooid.

Verschillende partities kunnen on-demand back-aanvragen op een moment activeren.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

On-demand back-aanvragen kunnen zich in de volgende statussen:

- **Geaccepteerd**: De back-up op de partitie is gestart en wordt uitgevoerd.
  ```
  BackupState             : Accepted
  TimeStampUtc            : 0001-01-01T00:00:00Z
  BackupId                : 00000000-0000-0000-0000-000000000000
  BackupLocation          :
  EpochOfLastBackupRecord :
  LsnOfLastBackupRecord   : 0
  FailureError            :
  ```
- **Succes**, **fout**, of **time-out**: Een aangevraagde back-up op aanvraag kan worden uitgevoerd in een van de volgende statussen:
  - **Geslaagd**: Een _succes_ back-up heeft, betekent dit dat de status van de partitie heeft back-ups van is. Het antwoord bevat _BackupEpoch_ en _BackupLSN_ voor de partitie, samen met de tijd in UTC.
    ```
    BackupState             : Success
    TimeStampUtc            : 2018-11-21T20:00:01Z
    BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
    BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
    EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
    LsnOfLastBackupRecord   : 36
    FailureError            :
    ```
  - **Fout**: Een _fout_ back-up heeft, betekent dit dat is een fout tijdens de back-up van de status van de partitie opgetreden. De oorzaak van de fout wordt vermeld in de reactie.
    ```
    BackupState             : Failure
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
    ```
  - **Time-out**: Een _time-out_ back-status geeft aan dat de back-up van de partitie kan niet worden gemaakt in een bepaalde periode. De standaard time-outwaarde is 10 minuten. Start een nieuwe back-up op aanvraag-aanvraag met groter [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) in dit scenario.
    ```
    BackupState             : Timeout
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
    ```

## <a name="next-steps"></a>Volgende stappen

- [Periodieke back-upconfiguratie begrijpen](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [BackupRestore REST API-naslaginformatie](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
