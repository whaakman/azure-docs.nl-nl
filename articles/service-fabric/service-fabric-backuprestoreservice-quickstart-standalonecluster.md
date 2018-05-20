---
title: Periodieke back-up en herstel in Azure Service Fabric (Preview) | Microsoft Docs
description: Gebruik van Service Fabric periodieke back-up en herstellen van de functie voor het beveiligen van uw toepassingen tegen gegevensverlies.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: timlt
editor: hrushib
ms.assetid: FAADBCAB-F0CF-4CBC-B663-4A6DCCB4DEE1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: hrushib
ms.openlocfilehash: cfbc0e6ca255bd005bb6e4cc381a9121347fe227
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric-preview"></a>Periodieke back-up en herstel in Azure Service Fabric (Preview)
> [!div class="op_single_selector"]
> * [Clusters op Azure](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Zelfstandige Clusters](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric is een platform voor gedistribueerde systemen waarmee u gemakkelijk te ontwikkelen en beheren van betrouwbare, gedistribueerde, microservices op basis van cloud-toepassingen. Hiermee kunt de uitvoering van staatloze en stateful micro services. Stateful services kunnen veranderlijke, gezaghebbende status afgezien van de aanvraag en antwoord of een volledige transactie behouden. Als een Stateful service gedurende een lange periode uitvalt of informatie als gevolg van een noodgeval verliest, moet deze mogelijk worden hersteld naar een recente back-up van de status om te kunnen blijven zonder serviceonderbrekingen nadat deze terug voordoet.

De status van de repliceert service Fabric op meerdere knooppunten om ervoor te zorgen dat de service maximaal beschikbaar is. Zelfs als een knooppunt in het cluster is mislukt, blijft de service beschikbaar. In bepaalde gevallen is het echter nog steeds wenselijk is voor de servicegegevens tegen fouten in de breder betrouwbaar.
 
Service wil bijvoorbeeld back-up van de gegevens om te voorkomen dat de volgende scenario's:
- In geval van een permanent verlies van een volledige Service Fabric-cluster.
- Permanent verlies van een meerderheid van de replica's van de partitie van een service
- Administratieve fouten waarbij de status per ongeluk wordt verwijderd of beschadigd. Bijvoorbeeld: een beheerder met voldoende bevoegdheden ten onrechte Hiermee verwijdert u de service.
- Fouten in de service die leiden gegevensbeschadiging van tot. Dit kan bijvoorbeeld gebeuren wanneer een code-upgrade van service wordt gestart met het schrijven van beschadigde gegevens naar een betrouwbare verzameling. In dat geval zowel de code en de gegevens mogelijk teruggezet naar een eerdere status.
- De gegevensverwerking is offline. Kan het zijn handig zijn als offline verwerking van gegevens voor business intelligence die plaatsvindt afzonderlijk van de service die de gegevens genereert.

Service Fabric bevat een ingebouwde API kunnen tijdstip [back-up en herstel](service-fabric-reliable-services-backup-restore.md). Ontwikkelaars van toepassingen kunnen deze API's gebruiken om regelmatig een back-up van de status van de service. Daarnaast als servicebeheerders activeren van een back-up van buiten de service op een bepaalde tijd wilt, moeten zoals vóór de upgrade van de toepassing ontwikkelaars zichtbaar back-up (en terugzetten) als een API van de service. Onderhoud van de back-ups is een extra kosten boven dit. U wilt bijvoorbeeld 5 incrementele back-ups elk half uur duren, gevolgd door een volledige back-up. U kunt de eerdere incrementele back-ups verwijderen na het volledige back-up. Deze aanpak vereist aanvullende code, wat leidt tot extra kosten tijdens de ontwikkeling van toepassingen.

Back-up van de toepassingsgegevens op periodieke basis is een eenvoudige nodig voor het beheren van een gedistribueerde toepassing en te beveiligen tegen verlies van gegevens of langdurige verlies van beschikbaarheid van de service. Service Fabric biedt een optionele back-up en herstel service, zodat u kunt de periodieke back-up van stateful Reliable Services (inclusief Actorservices) configureren zonder dat aanvullende code te schrijven. Vereenvoudigt ook eerder uitgevoerde back-ups herstellen. 

> [!NOTE]
> Periodieke back-up en herstel functie is momenteel in **Preview** en niet wordt ondersteund voor productieworkloads. 
>

Service Fabric bevat een reeks API's voor het bereiken van de volgende functionaliteit betrekking hebben op periodieke back-up en herstellen van de functie:

- Periodieke reservekopie van betrouwbare Stateful services en Reliable Actors met ondersteuning voor het uploaden van de back-up naar (extern) opslaglocaties plannen. Ondersteunde opslaglocaties
    - Azure Storage
    - Bestandsshare (on-premises)
- Back-ups opsommen
- Activeert een ad-hoc back-up van een partitie
- Een partitie met de vorige back-up herstellen
- Back-ups tijdelijk onderbreken
- Bewaren van beheer van back-ups (toekomstige)

## <a name="prerequisites"></a>Vereisten
* Service Fabric-cluster met Fabric versie 6.2 en hoger. Het cluster moet worden ingesteld op Windows Server. Raadpleeg [artikel](service-fabric-cluster-creation-for-windows-server.md) voor stappen om vereiste pakket te downloaden.
* X.509-certificaat voor het versleutelen van geheimen die verbinding maken met opslag nodig is voor het opslaan van back-ups. Raadpleeg [artikel](service-fabric-windows-cluster-x509-security.md) te weten hoe u aan te schaffen of een zelf-ondertekend X.509-certificaat maken.
* Service Fabric betrouwbare Stateful toepassingen die zijn gebouwd met behulp van Service Fabric SDK versie 3.0 of hoger. Voor toepassingen die gericht is op .net Core 2.0, de toepassing moet worden gemaakt via Service Fabric SDK versie 3.1 of hoger.

## <a name="enabling-backup-and-restore-service"></a>Back-up en herstel service inschakelen
U moet eerst om in te schakelen de _back-up en herstellen van de service_ in uw cluster. Haal de sjabloon voor het cluster dat u wilt implementeren. U kunt de [voorbeeldsjablonen](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples). Schakel de _back-up en herstellen van de service_ met de volgende stappen:

1. Controleer of de `apiversion` is ingesteld op `10-2017` in de configuratie van het bestand, en als dat niet bijwerken zoals weergegeven in het volgende fragment:

    ```json
    {
        "apiVersion": "10-2017",
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        ...
    }
    ```

2. Nu de _back-up en herstellen van de service_ door het volgende toe te voegen `addonFeatures` onder sectie `properties` sectie zoals weergegeven in het volgende fragment: 

    ```json
        "properties": {
            ...
            "addonFeatures": ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```

3. X.509-certificaat voor versleuteling van referenties configureren. Dit is belangrijk om ervoor te zorgen dat de referenties die zijn opgegeven, als een verbinding maken met opslag worden versleuteld voordat het behouden blijven. Versleutelingscertificaat configureren door het volgende toe te voegen `BackupRestoreService` onder sectie `fabricSettings` sectie zoals weergegeven in het volgende fragment: 

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

4. Nadat u uw cluster-configuratiebestand hebt bijgewerkt met de voorgaande wijzigingen, ze toepassen en kunt u bijwerken van de implementatie uitvoeren. Wanneer u klaar bent, de _back-up en herstellen van de service_ wordt gestart in het cluster. De Uri van deze service is `fabric:/System/BackupRestoreService` en de service kan zich bevinden onder sectie van de systeem-service in de Service Fabric explorer. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Inschakelen van periodieke back-up voor betrouwbare Stateful service en Reliable Actors
Laten we helpt u stapsgewijs door de stappen voor het inschakelen van periodieke back-up voor betrouwbare Stateful service en Reliable Actors. Deze stappen wordt ervan uitgegaan dat
- Dat het cluster ingesteld met is _back-up en herstellen van de service_.
- Een betrouwbare Stateful service wordt geïmplementeerd op het cluster. Deze snelstartgids omwille van de toepassings-Uri is `fabric:/SampleApp` en de Uri voor betrouwbare Stateful service die horen bij deze toepassing is `fabric:/SampleApp/MyStatefulService`. Deze service wordt geïmplementeerd met één partitie en de partitie-ID is `23aebc1e-e9ea-4e16-9d5c-e91a614fefa7`.  

### <a name="create-backup-policy"></a>Back-upbeleid maken

Eerste stap is het maken van back-upbeleid met een beschrijving van back-upschema, doelopslag voor back-upgegevens, de naam van beleid en maximale incrementele back-ups moet worden toegestaan voordat volledige back-up. 

Voor back-up maken van bestandsshare en ReadWrite toegang geven tot deze bestandsshare voor alle computers die Service Fabric-knooppunt. In dit voorbeeld wordt ervan uitgegaan dat de share met de naam `BackupStore` aanwezig is op `StorageServer`.

Uitvoeren van volgende PowerShell-script om vereiste REST-API voor het maken van nieuw beleid aan te roepen.

```powershell
$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}   

$StorageInfo = @{
    Path = '\\StorageServer\BackupStore'
    StorageKind = 'FileShare'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "http://localhost:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
```

### <a name="enable-periodic-backup"></a>Periodieke back-up inschakelen
Na het definiëren van beleid om te voldoen aan de beveiligingsvereisten voor gegevens van de toepassing, moet het back-upbeleid gekoppeld aan de toepassing. Afhankelijk van vereiste, kan het back-upbeleid worden gekoppeld aan een toepassing, service of een partitie.

Uitvoeren van volgende PowerShell-script voor het aanroepen van vereiste REST-API, koppelt u de naam van back-upbeleid `BackupPolicy1` gemaakt in de bovenstaande stap met toepassing `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "http://localhost:19080/Applications/SampleApp/$/EnableBackup?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Controleren of periodiek back-ups werken

Nadat de back-up voor de toepassing is ingeschakeld, gaat alle partities die behoren tot betrouwbare Stateful services en Reliable Actors onder de toepassing ophalen van een back-up periodiek volgens het bijbehorende back-upbeleid. 

![Partitie BackedUp statusgebeurtenis][0]

### <a name="list-backups"></a>Lijst met back-ups

Back-ups die zijn gekoppeld aan alle partities van betrouwbare Stateful services en Reliable Actors van de toepassing kunnen worden opgesomd met _GetBackups_ API. Afhankelijk van vereiste, kunnen de back-ups voor de toepassing, service of een partitie worden opgesomd.

Uitvoeren van volgende PowerShell-script voor het aanroepen van de HTTP-API voor het inventariseren van de back-ups gemaakt voor alle partities in de `SampleApp` toepassing.

```powershell
$url = "http://localhost:19080/Applications/SampleApp/$/GetBackups?api-version=6.2-preview"

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

## <a name="preview-limitation-caveats"></a>Preview-beperking / voorbehoud
- Er is geen Fabric-Service is gebouwd in PowerShell-cmdlets.
- Er is geen ondersteuning voor Service Fabric CLI.
- Geen ondersteuning voor het opschonen van automatische back-up. Vereist handmatige opschoning van back-ups.
- Er is geen ondersteuning voor Service Fabric-clusters op Linux.

## <a name="next-steps"></a>Volgende stappen
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent.png

