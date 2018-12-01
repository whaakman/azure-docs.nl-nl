---
title: On-Demand back-ups in Azure Service Fabric | Microsoft Docs
description: Gebruik van Service Fabric-back-up en herstellen van de functie voor back-up van uw toepassingsgegevens op basis van behoefte.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 02DA262A-EEF6-4F90-842E-FFC4A09003E5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 2e83dcb4ce42a0c08ce482e9ffbbf1ac18634fd5
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730164"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>On-demand back-ups in Azure Service Fabric

De gegevens van betrouwbare Stateful services en Reliable Actors kan een back-worden gemaakt voor noodherstel of gegevens verloren gaan.

Service Fabric is uitgerust met functies voor [periodieke back-up van gegevens](service-fabric-backuprestoreservice-quickstart-azurecluster.md) en back-up van gegevens op basis van behoefte. Back-up op aanvraag is nuttig omdat deze biedt bescherming tegen de _gegevensverlies_/_gegevensbeschadiging_ veroorzaakt door geplande wijzigingen in de onderliggende service of de omgeving.

De back-up op aanvraag-functies kan handig zijn in het vastleggen van de status van de services, voordat u een handmatig geactiveerde bewerking met betrekking tot de service of de service-omgeving. Zoals het wijzigen van de service binaire bestanden, dat wil zeggen, upgraden of downgraden van de service; omdat het de gegevens die zijn beveiligd tegen beschadiging van gegevens door fouten in de code van toepassing hebben zal.

## <a name="triggering-on-demand-backup"></a>Back-up op aanvraag activeren

De on-demand back-up is opslaggegevens vereist voor het uploaden van back-upbestanden. De on-demand back-up wordt in de opslag die is opgegeven in de periodieke back-upbeleid of in de opgegeven opslaggroep in de back-upaanvraag op aanvraag.

### <a name="on-demand-backup-to-storage-specified-by-periodic-backup-policy"></a>On-demand back-up naar de opslag die zijn opgegeven door periodieke back-upbeleid

De partitie van een betrouwbare Stateful service of een Reliable Actor kan worden aangevraagd voor een extra op hoeft uit te voeren back-up naar de opslag die zijn opgegeven in de periodieke back-upbeleid. 

Het volgende voorbeeld is de voortzetting van voorbeeld zoals vermeld in [inschakelen periodieke back-up voor betrouwbare Stateful service en Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), waarbij de partitie een back-upbeleid ingeschakeld heeft en back-up met een gewenste frequentie duurt in Azure Storage.

De on-demand back-up voor partitie-ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22` kan worden geactiveerd door [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API. 

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

De [back-up wordt uitgevoerd op aanvraag](service-fabric-backuprestoreservice-ondemand-backup.md#tracking-on-demand-backup-progress) kan worden gevolgd door [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API.

### <a name="on-demand-backup-to-specified-storage"></a>On-demand back-up naar de opgegeven opslag

De on-demand back-up kan worden aangevraagd voor een partitie van een betrouwbare Stateful service of een Reliable Actor samen met de opslaginformatie. De opslaginformatie moet worden opgegeven als onderdeel van de back-upaanvraag op aanvraag.

De on-demand back-up voor partitie-ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22` kan worden geactiveerd door [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API met Azure storage-gegevens, zoals hieronder weergegeven.

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

De [back-up wordt uitgevoerd op aanvraag](service-fabric-backuprestoreservice-ondemand-backup.md#tracking-on-demand-backup-progress) kan worden gevolgd door [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API.


## <a name="tracking-on-demand-backup-progress"></a>Back-up op aanvraag de voortgang volgen

Een partitie van een betrouwbare Stateful service of een Reliable Actor accepteert slechts één on-demand back-upaanvraag tegelijk. Een nieuwe aanvraag kan worden geaccepteerd, alleen wanneer de huidige back-up op aanvraag-aanvraag is voltooid. 

Meerdere on-demand back-aanvragen kunnen worden geactiveerd in verschillende partities op een moment.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

Voortgang van de on-demand back-up van de aanvraag kan een van de volgende statussen zijn

* **Geaccepteerd** -de back-up op de partitie is gestart en wordt uitgevoerd.
    ```
    BackupState             : Accepted
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          : 
    EpochOfLastBackupRecord : 
    LsnOfLastBackupRecord   : 0
    FailureError            : 
    ```
    
* **Geslaagd / mislukt / Timeout** -een aangevraagde back-up op aanvraag kan worden uitgevoerd in een van de volgende statussen. Elke Amerikaanse staat zijn de gegevens van de volgende betekenis en reactie.

    * **Succes** -de status van de back-up als _succes_ geeft aan dat de status van de partitie een back-up is. Het antwoord krijgt __BackupEpoch__ en __BackupLSN__ voor de partitie, samen met de tijd in UTC.
        ```
        BackupState             : Success
        TimeStampUtc            : 2018-11-21T20:00:01Z
        BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
        BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
        EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
        LsnOfLastBackupRecord   : 36
        FailureError            : 
        ```
        
    * **Fout** -de status van de back-up als _fout_ geeft aan dat de fout is opgetreden tijdens de back-up van de status van de partitie. De oorzaak van het probleem zal worden vermeld in de reactie.
        ```
        BackupState             : Failure
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
        ```
       
    * **Time-out** -de status van de back-up als _time-out_ geeft aan dat de back-up van de partitie kan niet worden gemaakt de opgegeven tijdsbestek; standaard time-outwaarde 10 minuten is. Een nieuwe back-aanvraag met groter [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) in on-demand back-upaanvraag in dit scenario wordt aanbevolen.

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
- [Understanding periodieke back-upconfiguratie](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

