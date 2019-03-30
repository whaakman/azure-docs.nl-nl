---
title: Periodieke back-up en herstel in Azure Service Fabric | Microsoft Docs
description: Gebruik van Service Fabric periodieke back-up en herstellen van de functie voor het inschakelen van periodieke gegevens back-up van uw toepassingsgegevens.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: chackdan
editor: hrushib
ms.assetid: FAADBCAB-F0CF-4CBC-B663-4A6DCCB4DEE1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: hrushib
ms.openlocfilehash: 1a1c1bafd0a575b01e9774e79a98515d34646f7c
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670659"
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric"></a>Periodieke back-up en herstel in Azure Service Fabric
> [!div class="op_single_selector"]
> * [Clusters op Azure](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Zelfstandige Clusters](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric is een platform voor gedistribueerde systemen waarmee u eenvoudig ontwikkelen en beheren van betrouwbare, gedistribueerde, microservices, op basis van cloudtoepassingen. Hierdoor kan het uitvoeren van staatloze en stateful microservices. Stateful services kunnen veranderlijke, gezaghebbende status, buiten de aanvraag en antwoord of een volledige transactie behouden. Als een Stateful service gedurende een lange periode uitvalt of gegevens vanwege een noodgeval verliest, moet deze mogelijk worden hersteld naar een recente back-up van de status om te kunnen blijven bieden service nadat deze is hersteld.

Service Fabric worden gerepliceerd van de status over meerdere knooppunten om ervoor te zorgen dat de service maximaal beschikbaar is. Zelfs als één knooppunt in het cluster is mislukt, wordt de service blijft beschikbaar. In bepaalde gevallen, het is echter nog steeds wenselijk dat de servicegegevens tegen fouten in de bredere betrouwbaar.
 
Een service wilt bijvoorbeeld een back-up van de gegevens om u te beschermen tegen de volgende scenario's:
- Permanent verlies van een hele Service Fabric-cluster.
- Permanent verlies van een meerderheid van de replica's van de servicepartitie van een
- Administratieve fouten waarbij de status per ongeluk wordt verwijderd of beschadigd. Een beheerder met voldoende bevoegdheden verwijdert bijvoorbeeld per ongeluk de service.
- Fouten in de service die leiden gegevensbeschadiging tot. Dit kan bijvoorbeeld gebeuren wanneer een upgrade van de code service begint beschadigde gegevens schrijven naar een betrouwbare verzameling. In dat geval, zowel de code en de gegevens mogelijk teruggezet naar een eerdere toestand.
- Offline gegevens verwerken. Het is mogelijk handig zijn als offline verwerking van gegevens voor business intelligence die plaatsvindt afzonderlijk van de service die de gegevens worden gegenereerd.

Service Fabric biedt een ingebouwde API voor het naar een bepaald tijdstip [back-up en herstellen](service-fabric-reliable-services-backup-restore.md). Ontwikkelaars van toepassingen kunnen deze API's gebruiken om regelmatig een back-up van de status van de service. Bovendien, als servicebeheerders wilt voor het activeren van een back-up van buiten de service op een bepaald tijdstip, moeten zoals vóór de upgrade van de toepassing ontwikkelaars beschikbaar maken van back-up (en terugzetten) als een API van de service. Onderhoud van de back-ups is er gelden aanvullende kosten boven dit. U wilt bijvoorbeeld vijf incrementele back-ups elk half uur duren, gevolgd door een volledige back-up. Nadat de volledige back-up, kunt u de eerdere incrementele back-ups verwijderen. Deze aanpak vereist aanvullende code leidt tot extra kosten tijdens de ontwikkeling van toepassingen.

Back-up van de toepassingsgegevens op periodieke basis is een eenvoudige nodig voor het beheren van een gedistribueerde toepassing en te beveiligen tegen verlies van gegevens of langdurige verlies van beschikbaarheid van de service. Service Fabric biedt een optionele back-up en herstellen-service, zodat u kunt periodieke back-up van stateful Reliable Services (inclusief Actorservices) configureren zonder extra code te schrijven. Het vereenvoudigt ook eerder gemaakt back-ups herstellen. 

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
* Service Fabric-cluster met Fabric versie 6.2 en hoger. Het cluster moet worden ingesteld op Windows Server. Verwijzen naar dit [artikel](service-fabric-cluster-creation-for-windows-server.md) voor stappen om vereiste pakket te downloaden.
* X.509-certificaat voor het versleutelen van geheimen die nodig zijn voor het verbinding maken met opslag voor het opslaan van back-ups. Raadpleeg [artikel](service-fabric-windows-cluster-x509-security.md) te weten hoe u aan te schaffen of te maken van een zelfondertekend X.509-certificaat.
* Betrouwbare Stateful van service Fabric-toepassing die is gebouwd met behulp van Service Fabric SDK versie 3.0 of hoger. Voor toepassingen die gericht is op .NET Core 2.0, toepassing moet worden gemaakt met behulp van Service Fabric SDK versie 3.1 of hoger.

## <a name="enabling-backup-and-restore-service"></a>Back-up en herstel-service inschakelen
U moet eerst om in te schakelen de _back-up en herstellen van service_ in uw cluster. De sjabloon ophalen voor het cluster dat u wilt implementeren. U kunt de [-voorbeeldsjablonen](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples). Schakel de _back-up en herstellen van service_ met de volgende stappen uit:

1. Controleer of de `apiversion` is ingesteld op `10-2017` in de configuratie van het bestand, en als dat niet het geval is, werken zoals wordt weergegeven in het volgende codefragment:

    ```json
    {
        "apiVersion": "10-2017",
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        ...
    }
    ```

2. Schakel nu in de _back-up en herstellen van service_ door toe te voegen van de volgende `addonFeatures` sectie onder `properties` sectie zoals wordt weergegeven in het volgende codefragment: 

    ```json
        "properties": {
            ...
            "addonFeatures": ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```

3. X.509-certificaat voor versleuteling van referenties configureren. Dit is belangrijk om ervoor te zorgen dat de referenties die zijn opgegeven, als een verbinding maken met opslag worden versleuteld voordat het persistent maken. Versleutelingscertificaat configureren door toe te voegen van de volgende `BackupRestoreService` sectie onder `fabricSettings` sectie zoals wordt weergegeven in het volgende codefragment: 

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

4. Nadat u uw cluster-configuratiebestand hebt bijgewerkt met de bovenstaande wijzigingen, past deze toe en laat wordt de implementatie/upgrade voltooien. Wanneer u klaar bent, de _back-up en herstellen van service_ wordt uitgevoerd in uw cluster. De Uri van deze service is `fabric:/System/BackupRestoreService` en de service kan zich bevinden onder system service-sectie in de Service Fabric explorer. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Periodieke back-up voor betrouwbare Stateful service en Reliable Actors inschakelen
We nemen stappen voor het inschakelen van periodieke back-up voor betrouwbare Stateful service en Reliable Actors. Deze stappen wordt ervan uitgegaan
- Het cluster is ingesteld met _back-up en herstellen van service_.
- Een betrouwbare Stateful service wordt geïmplementeerd op het cluster. Ten behoeve van deze snelstartgids toepassings-Uri is `fabric:/SampleApp` en de Uri voor betrouwbare Stateful service die behoren tot deze toepassing is `fabric:/SampleApp/MyStatefulService`. Deze service is geïmplementeerd met één partitie en de partitie-ID is `23aebc1e-e9ea-4e16-9d5c-e91a614fefa7`.  

### <a name="create-backup-policy"></a>Back-upbeleid maken

Eerste stap is het maken van back-upbeleid met een beschrijving van back-upschema, gericht op opslag voor back-upgegevens, de naam van beleid, maximale incrementele back-ups worden toegestaan voordat het activeren van de volledige back-up- en bewaarbeleid voor back-upopslag. 

Voor back-upopslag-bestandsshare maken en ReadWrite toegang geven tot deze bestandsshare voor alle machines in Service Fabric-knooppunt. In dit voorbeeld wordt ervan uitgegaan dat de share met de naam `BackupStore` aanwezig is op `StorageServer`.

Voer de volgende PowerShell-script voor het aanroepen van vereiste REST-API voor het maken van nieuw beleid.

```powershell
$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}   

$StorageInfo = @{
    Path = '\\StorageServer\BackupStore'
    StorageKind = 'FileShare'
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
$url = "http://localhost:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
```

> [!IMPORTANT]
> Zorg ervoor dat de bewaartermijn in het bewaarbeleid is geconfigureerd voor minder dan 24 dagen anders resulteert dit in de service gaan naar het quorum verloren na replica-failover back-up herstellen vanwege een probleem in de runtime.

### <a name="enable-periodic-backup"></a>Periodieke back-up inschakelen
Na het definiëren van beleid om te voldoen aan de beveiligingsvereisten voor gegevens van de toepassing, moet het back-upbeleid gekoppeld aan de toepassing. Afhankelijk van vereiste, kan het back-upbeleid zijn gekoppeld aan een toepassing, service of een partitie.

Voer de volgende PowerShell-script voor het aanroepen van vereiste REST-API om te koppelen van back-upbeleid met de naam `BackupPolicy1` gemaakt in de hierboven genoemde stap met toepassing `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "http://localhost:19080/Applications/SampleApp/$/EnableBackup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Controleren of de periodieke back-ups zijn werkt

Nadat de back-up voor de toepassing is ingeschakeld, wordt alle partities die behoren tot betrouwbare Stateful services en Reliable Actors onder de toepassing ophalen van een back-up periodiek aan de hand van de gekoppelde back-upbeleid gestart. 

![Partitie BackedUp statusgebeurtenis][0]

### <a name="list-backups"></a>Lijst met back-ups

Back-ups die zijn gekoppeld aan alle partities die behoren tot een betrouwbare Stateful services en Reliable Actors van de toepassing kunnen worden geïnventariseerd met behulp van _GetBackups_ API. Afhankelijk van vereiste, kunnen de back-ups voor de toepassing, service of een partitie worden opgesomd.

Voer de volgende PowerShell-script voor het aanroepen van de HTTP-API voor het inventariseren van de back-ups gemaakt voor alle partities in de `SampleApp` toepassing.

```powershell
$url = "http://localhost:19080/Applications/SampleApp/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Voorbeeld van uitvoer voor de bovenstaande uitvoeren:

```
BackupId                : d7e4038e-2c46-47c6-9549-10698766e714
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.39.40.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2058
CreationTimeUtc         : 2018-04-01T19:39:40Z
FailureError            : 

BackupId                : 8c21398a-2141-4133-b4d7-e1a35f0d7aac
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.54.38.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2237
CreationTimeUtc         : 2018-04-01T19:54:38Z
FailureError            : 

BackupId                : fc75bd4c-798c-4c9a-beee-e725321f73b2
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 20.09.44.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2437
CreationTimeUtc         : 2018-04-01T20:09:44Z
FailureError            : 
```

## <a name="known-issues"></a>Bekende problemen
- Zorg ervoor dat de bewaartermijn is geconfigureerd voor minder dan 24 dagen. 
- Back-up herstellen-service zijn niet beschikbaar op Landinstellingen waar het decimale scheidingsteken anders dan is '.'
- Back-up herstellen-service kan niet zijn beschikbaar op het cluster is beveiligd met gMSA op basis van beveiliging.

## <a name="limitation-caveats"></a>Beperking / voorbehouden
- Er is geen Service-Fabric gebouwd in PowerShell-cmdlets.
- Er is geen ondersteuning voor Service Fabric-clusters op Linux.

## <a name="next-steps"></a>Volgende stappen
- [Understanding periodieke back-upconfiguratie](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent.png

