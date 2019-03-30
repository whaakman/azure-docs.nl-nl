---
title: Periodieke back-up en herstel in Azure Service Fabric | Microsoft Docs
description: Gebruik van Service Fabric periodieke back-up en herstellen van de functie voor het inschakelen van periodieke gegevens back-up van uw toepassingsgegevens.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: chackdan
editor: hrushib
ms.assetid: FAA58600-897E-4CEE-9D1C-93FACF98AD1C
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: hrushib
ms.openlocfilehash: 4d4bc69f00f86bc81c353ef0cc40f37f000ba6c4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670435"
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric"></a>Periodieke back-up en herstel in Azure Service Fabric 
> [!div class="op_single_selector"]
> * [Clusters op Azure](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Zelfstandige Clusters](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric is een platform voor gedistribueerde systemen waarmee u eenvoudig ontwikkelen en beheren van betrouwbare, gedistribueerde, microservices, op basis van cloudtoepassingen. Hierdoor kan het uitvoeren van staatloze en stateful microservices. Stateful services kunnen veranderlijke, gezaghebbende status, buiten de aanvraag en antwoord of een volledige transactie behouden. Als een Stateful service gedurende een lange periode uitvalt of gegevens vanwege een noodgeval verliest, moet deze mogelijk worden hersteld naar een recente back-up van de status om te kunnen blijven bieden service nadat deze is hersteld.

Service Fabric worden gerepliceerd van de status over meerdere knooppunten om ervoor te zorgen dat de service maximaal beschikbaar is. Zelfs als één knooppunt in het cluster is mislukt, wordt de service blijft beschikbaar. In bepaalde gevallen, het is echter nog steeds wenselijk dat de servicegegevens tegen fouten in de bredere betrouwbaar.
 
Service wilt bijvoorbeeld een back-up van de gegevens om u te beschermen tegen de volgende scenario's:
- In het geval van het permanente verlies van gegevens van een hele Service Fabric-cluster.
- Permanent verlies van een meerderheid van de replica's van de servicepartitie van een
- Administratieve fouten waarbij de status per ongeluk wordt verwijderd of beschadigd. Een beheerder met voldoende bevoegdheden verwijdert bijvoorbeeld per ongeluk de service.
- Fouten in de service die leiden gegevensbeschadiging tot. Dit kan bijvoorbeeld gebeuren wanneer een upgrade van de code service begint beschadigde gegevens schrijven naar een betrouwbare verzameling. In dat geval, zowel de code en de gegevens mogelijk teruggezet naar een eerdere toestand.
- Offline gegevens verwerken. Het is mogelijk handig zijn als offline verwerking van gegevens voor business intelligence die plaatsvindt afzonderlijk van de service die de gegevens worden gegenereerd.

Service Fabric biedt een ingebouwde API voor het naar een bepaald tijdstip [back-up en herstellen](service-fabric-reliable-services-backup-restore.md). Ontwikkelaars van toepassingen kunnen deze API's gebruiken om regelmatig een back-up van de status van de service. Bovendien, als servicebeheerders wilt voor het activeren van een back-up van buiten de service op een bepaald tijdstip, moeten zoals vóór de upgrade van de toepassing ontwikkelaars beschikbaar maken van back-up (en terugzetten) als een API van de service. Onderhoud van de back-ups is er gelden aanvullende kosten boven dit. U wilt bijvoorbeeld vijf incrementele back-ups elk half uur duren, gevolgd door een volledige back-up. Nadat de volledige back-up, kunt u de eerdere incrementele back-ups verwijderen. Deze aanpak vereist aanvullende code leidt tot extra kosten tijdens de ontwikkeling van toepassingen.

De back-up en herstel-service in Service Fabric kunt eenvoudig en automatisch back-up van gegevens die zijn opgeslagen in een stateful services. Back-ups van toepassingsgegevens op periodieke basis is van cruciaal belang voor het beveiligen tegen gegevens beschermd tegen verlies en -service niet beschikbaar zijn. Service Fabric biedt een optionele back-up en herstellen-service, zodat u kunt periodieke back-up van stateful Reliable Services (inclusief Actorservices) configureren zonder extra code te schrijven. Het vereenvoudigt ook eerder gemaakt back-ups herstellen. 


Service Fabric biedt een set API's voor het bereiken van de volgende functionaliteit betrekking hebben op periodieke back-up en herstellen van de functie:

- Periodieke back-up van betrouwbare Stateful services en Reliable Actors met ondersteuning voor back-up naar (extern) opslaglocaties uploaden plannen. Ondersteunde opslaglocaties
    - Azure Storage
    - Bestandsshare (on-premises)
- Back-ups opsommen
- Activeren van een ad-hoc back-up van een partitie
- Een partitie met behulp van de vorige back-up herstellen
- Back-ups tijdelijk onderbreken
- Retentie-beheer van back-ups (toekomstige)

## <a name="prerequisites"></a>Vereisten
* Service Fabric-cluster met Fabric versie 6.2 en hoger. Het cluster moet worden ingesteld op Windows Server. Verwijzen naar dit [artikel](service-fabric-cluster-creation-via-arm.md) voor stappen voor het maken van Service Fabric-cluster met behulp van Azure-resource-sjabloon.
* X.509-certificaat voor het versleutelen van geheimen die nodig zijn voor het verbinding maken met opslag voor het opslaan van back-ups. Raadpleeg [artikel](service-fabric-cluster-creation-via-arm.md) wilt weten hoe ophalen of maken van een X.509-certificaat.
* Betrouwbare Stateful van service Fabric-toepassing die is gebouwd met behulp van Service Fabric SDK versie 3.0 of hoger. Voor toepassingen die gericht is op .NET Core 2.0, toepassing moet worden gemaakt met behulp van Service Fabric SDK versie 3.1 of hoger.
* Azure Storage-account voor het opslaan van back-ups van de toepassing maken.

## <a name="enabling-backup-and-restore-service"></a>Back-up en herstel-service inschakelen
U moet eerst om in te schakelen de _back-up en herstellen van service_ in uw cluster. De sjabloon ophalen voor het cluster dat u wilt implementeren. Kunt u ofwel de [-voorbeeldsjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) of een Resource Manager-sjabloon maken. Schakel de _back-up en herstellen van service_ met de volgende stappen uit:

1. Controleer of de `apiversion` is ingesteld op **`2018-02-01`** voor de `Microsoft.ServiceFabric/clusters` resource, en als dat niet het geval is, werken zoals wordt weergegeven in het volgende codefragment:

    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Schakel nu in de _back-up en herstellen van service_ door toe te voegen van de volgende `addonFeatures` sectie onder `properties` sectie zoals wordt weergegeven in het volgende codefragment: 

    ```json
        "properties": {
            ...
            "addonFeatures":  ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```
3. X.509-certificaat voor versleuteling van referenties configureren. Dit is belangrijk om ervoor te zorgen dat de opgegeven verbinding maken met opslag referenties worden versleuteld voordat het persistent maken. Versleutelingscertificaat configureren door toe te voegen van de volgende `BackupRestoreService` sectie onder `fabricSettings` sectie zoals wordt weergegeven in het volgende codefragment: 

    ```json
    "properties": {
        ...
        "addonFeatures": ["BackupRestoreService"],
        "fabricSettings": [{
            "name": "BackupRestoreService",
            "parameters":  [{
                "name": "SecretEncryptionCertThumbprint",
                "value": "[Thumbprint]"
            }]
        }
        ...
    }
    ```

4. Nadat u de clustersjabloon voor het hebt bijgewerkt met de bovenstaande wijzigingen, past deze toe en laat wordt de implementatie/upgrade voltooien. Wanneer u klaar bent, de _back-up en herstellen van service_ wordt uitgevoerd in uw cluster. De Uri van deze service is `fabric:/System/BackupRestoreService` en de service kan zich bevinden onder system service-sectie in de Service Fabric explorer. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Periodieke back-up voor betrouwbare Stateful service en Reliable Actors inschakelen
We nemen stappen voor het inschakelen van periodieke back-up voor betrouwbare Stateful service en Reliable Actors. Deze stappen wordt ervan uitgegaan
- Dat het cluster is installatie met behulp van x.509-beveiliging met _back-up en herstellen van service_.
- Een betrouwbare Stateful service wordt geïmplementeerd op het cluster. Ten behoeve van deze snelstartgids toepassings-Uri is `fabric:/SampleApp` en de Uri voor betrouwbare Stateful service die behoren tot deze toepassing is `fabric:/SampleApp/MyStatefulService`. Deze service is geïmplementeerd met één partitie en de partitie-ID is `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.
- Het clientcertificaat met administrator-rol is geïnstalleerd in _mijn_ (_persoonlijke_) naam van opslaan _CurrentUser_ opslaglocatie op de virtuele machine van waar u hieronder het certificaat scripts wordt aangeroepen. In dit voorbeeld wordt `1b7ebe2174649c45474a4819dafae956712c31d3` als de vingerafdruk van dit certificaat. Zie voor meer informatie over clientcertificaten [rollen gebaseerd toegangsbeheer voor Service Fabric-clients](service-fabric-cluster-security-roles.md).

### <a name="create-backup-policy"></a>Back-upbeleid maken

Eerste stap is het maken van back-upbeleid met een beschrijving van back-upschema, gericht op opslag voor back-upgegevens, de naam van beleid, maximale incrementele back-ups worden toegestaan voordat het activeren van de volledige back-up- en bewaarbeleid voor back-upopslag. 

Gebruik de Azure Storage-account hierboven gemaakt voor back-upopslag. Container `backup-container` is geconfigureerd voor het opslaan van back-ups. Een container met deze naam wordt gemaakt, als deze nog niet, tijdens het back-up uploaden bestaat. Vullen `ConnectionString` met een geldige verbindingsreeks voor de Azure Storage-account, vervangen `account-name` met de naam van uw opslagaccount, en `account-key` met sleutel van uw opslagaccount.

Voer de volgende PowerShell-script voor het aanroepen van vereiste REST-API voor het maken van nieuw beleid. Vervang `account-name` met de naam van uw opslagaccount, en `account-key` met sleutel van uw opslagaccount.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}

$RetentionPolicy = @{ 
    RetentionPolicyType = 'Basic'
    RetentionDuration =  'P10D'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
    RetentionPolicy = $RetentionPolicy
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

> [!IMPORTANT]
> Zorg ervoor dat de bewaartermijn in het bewaarbeleid is geconfigureerd voor minder dan 24 dagen anders resulteert dit in de service gaan naar het quorum verloren na replica-failover back-up herstellen vanwege een probleem in de runtime.

### <a name="enable-periodic-backup"></a>Periodieke back-up inschakelen
Na het definiëren van back-upbeleid om te voldoen aan de beveiligingsvereisten voor gegevens van de toepassing, moet het back-upbeleid gekoppeld aan de toepassing. Afhankelijk van vereiste, kan het back-upbeleid zijn gekoppeld aan een toepassing, service of een partitie.

Voer de volgende PowerShell-script voor het aanroepen van vereiste REST-API om te koppelen van back-upbeleid met de naam `BackupPolicy1` gemaakt in de hierboven genoemde stap met toepassing `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/EnableBackup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Controleren of de periodieke back-ups zijn werkt

Nadat de back-up op het toepassingsniveau van de is ingeschakeld, wordt alle partities die behoren tot betrouwbare Stateful services en Reliable Actors onder de toepassing ophalen van een back-up periodiek aan de hand van de gekoppelde back-upbeleid gestart. 

![Partitie BackedUp statusgebeurtenis][0]

### <a name="list-backups"></a>Lijst met back-ups

Back-ups die zijn gekoppeld aan alle partities die behoren tot een betrouwbare Stateful services en Reliable Actors van de toepassing kunnen worden geïnventariseerd met behulp van _GetBackups_ API. Back-ups kunnen worden opgesomd voor een toepassing, service of een partitie.

Voer de volgende PowerShell-script voor het aanroepen van de HTTP-API voor het inventariseren van de back-ups gemaakt voor alle partities in de `SampleApp` toepassing.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

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

## <a name="limitation-caveats"></a>Beperking / voorbehouden
- Er is geen Service-Fabric gebouwd in PowerShell-cmdlets.
- Er is geen ondersteuning voor Service Fabric-clusters op Linux.

## <a name="known-issues"></a>Bekende problemen
- Zorg ervoor dat de bewaartermijn is geconfigureerd voor minder dan 24 dagen. 

## <a name="next-steps"></a>Volgende stappen
- [Understanding periodieke back-upconfiguratie](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent_Azure.png

