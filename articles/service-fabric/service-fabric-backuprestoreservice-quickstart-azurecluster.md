---
title: Periodieke back-up en herstel in Azure Service Fabric (Preview) | Microsoft Docs
description: Gebruik van Service Fabric periodieke back-up en herstellen van de functie voor het beveiligen van uw toepassingen tegen gegevensverlies.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: timlt
editor: hrushib
ms.assetid: FAA58600-897E-4CEE-9D1C-93FACF98AD1C
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: hrushib
ms.openlocfilehash: f2ab583ecc77122f2c76f905a1cd7b936278bd41
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
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
* Service Fabric-cluster met Fabric versie 6.2 en hoger. Het cluster moet worden ingesteld op Windows Server. Raadpleeg [artikel](service-fabric-cluster-creation-via-arm.md) voor stappen voor het maken van Service Fabric-cluster met behulp van Azure-resource-sjabloon.
* X.509-certificaat voor het versleutelen van geheimen die verbinding maken met opslag nodig is voor het opslaan van back-ups. Raadpleeg [artikel](service-fabric-cluster-creation-via-arm.md) weten hoe de op te halen of maken van een X.509-certificaat.
* Service Fabric betrouwbare Stateful toepassingen die zijn gebouwd met behulp van Service Fabric SDK versie 3.0 of hoger. Voor toepassingen die gericht is op .net Core 2.0, de toepassing moet worden gemaakt via Service Fabric SDK versie 3.1 of hoger.
* Azure Storage-account voor het opslaan van back-ups van de toepassing maken.

## <a name="enabling-backup-and-restore-service"></a>Back-up en herstel service inschakelen
U moet eerst om in te schakelen de _back-up en herstellen van de service_ in uw cluster. Haal de sjabloon voor het cluster dat u wilt implementeren. Kunt u ofwel de [voorbeeldsjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) of een Resource Manager-sjabloon maken. Schakel de _back-up en herstellen van de service_ met de volgende stappen:

1. Controleer of de `apiversion` is ingesteld op **`2018-02-01`** voor de `Microsoft.ServiceFabric/clusters` resource, en als dat niet bijwerken zoals weergegeven in het volgende fragment:

    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Nu de _back-up en herstellen van de service_ door het volgende toe te voegen `addonFeatures` onder sectie `properties` sectie zoals weergegeven in het volgende fragment: 

    ```json
        "properties": {
            ...
            "addonFeatures":  ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```
3. X.509-certificaat voor versleuteling van referenties configureren. Dit is belangrijk om ervoor te zorgen dat de opgegeven verbinding maken met opslag referenties zijn versleuteld voordat het behouden blijven. Versleutelingscertificaat configureren door het volgende toe te voegen `BackupRestoreService` onder sectie `fabricSettings` sectie zoals weergegeven in het volgende fragment: 

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

4. Nadat u de sjabloon voor het cluster hebt bijgewerkt met de voorgaande wijzigingen, ze toepassen en kunt wordt de implementatie/upgrade voltooien. Wanneer u klaar bent, de _back-up en herstellen van de service_ wordt gestart in het cluster. De Uri van deze service is `fabric:/System/BackupRestoreService` en de service kan zich bevinden onder sectie van de systeem-service in de Service Fabric explorer. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Inschakelen van periodieke back-up voor betrouwbare Stateful service en Reliable Actors
Laten we helpt u stapsgewijs door de stappen voor het inschakelen van periodieke back-up voor betrouwbare Stateful service en Reliable Actors. Deze stappen wordt ervan uitgegaan dat
- Het cluster, is geïnstalleerd met X.509-beveiliging met _back-up en herstellen van de service_.
- Een betrouwbare Stateful service wordt geïmplementeerd op het cluster. Deze snelstartgids omwille van de toepassings-Uri is `fabric:/SampleApp` en de Uri voor betrouwbare Stateful service die horen bij deze toepassing is `fabric:/SampleApp/MyStatefulService`. Deze service wordt geïmplementeerd met één partitie en de partitie-ID is `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.
- Het clientcertificaat met administrator-rol is geïnstalleerd in _mijn_ (_persoonlijke_) naam van opslaan _CurrentUser_ opslaglocatie op de computer waar hieronder van het certificaat scripts wordt aangeroepen. In dit voorbeeld wordt `1b7ebe2174649c45474a4819dafae956712c31d3` als de vingerafdruk van dit certificaat. Zie voor meer informatie over clientcertificaten [toegangsbeheer op basis van rollen voor Service Fabric-clients](service-fabric-cluster-security-roles.md).

### <a name="create-backup-policy"></a>Back-upbeleid maken

Eerste stap is het maken van back-upbeleid met een beschrijving van back-upschema, doelopslag voor back-upgegevens, de naam van beleid en maximale incrementele back-ups moet worden toegestaan voordat volledige back-up. 

Gebruik de Azure Storage-account hierboven gemaakt voor back-up. In dit voorbeeld wordt ervan uitgegaan dat de Azure Storage-account met de naam `sfbackupstore`. Container `backup-container` is geconfigureerd voor het opslaan van back-ups van de container met deze naam is gemaakt, als deze niet al aanwezig, tijdens het uploaden van back-up. Vullen `ConnectionString` met geldige verbindingsreeks voor de Azure Storage-account.

Uitvoeren van volgende PowerShell-script om vereiste REST-API voor het maken van nieuw beleid aan te roepen.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=sfbackupstore;AccountKey=64S+3ykBgOuKhd2DK1qHJJtDml3NtRzgaZUa+8iwwBAH4EzuGt95JmOm7mp/HOe8V3l645iv5l8oBfnhhc7dJA==;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

### <a name="enable-periodic-backup"></a>Periodieke back-up inschakelen
Na het definiëren van back-upbeleid om te voldoen aan de beveiligingsvereisten voor gegevens van de toepassing, moet het back-upbeleid gekoppeld aan de toepassing. Afhankelijk van vereiste, kan het back-upbeleid worden gekoppeld aan een toepassing, service of een partitie.

Uitvoeren van volgende PowerShell-script voor het aanroepen van vereiste REST-API, koppelt u de naam van back-upbeleid `BackupPolicy1` gemaakt in de bovenstaande stap met toepassing `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/EnableBackup?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Controleren of periodiek back-ups werken

Nadat de back-up op toepassingsniveau is ingeschakeld, gaat alle partities die behoren tot betrouwbare Stateful services en Reliable Actors onder de toepassing ophalen van een back-up periodiek volgens het bijbehorende back-upbeleid. 

![Partitie BackedUp statusgebeurtenis][0]

### <a name="list-backups"></a>Lijst met back-ups

Back-ups die zijn gekoppeld aan alle partities van betrouwbare Stateful services en Reliable Actors van de toepassing kunnen worden opgesomd met _GetBackups_ API. Back-ups kunnen worden opgesomd voor een toepassing, service of een partitie.

Uitvoeren van volgende PowerShell-script voor het aanroepen van de HTTP-API voor het inventariseren van de back-ups gemaakt voor alle partities in de `SampleApp` toepassing.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/GetBackups?api-version=6.2-preview"

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

## <a name="preview-limitation-caveats"></a>Preview-beperking / voorbehoud
- Er is geen Fabric-Service is gebouwd in PowerShell-cmdlets.
- Er is geen ondersteuning voor Service Fabric CLI.
- Geen ondersteuning voor het opschonen van automatische back-up. Vereist handmatige opschoning van back-ups.
- Er is geen ondersteuning voor Service Fabric-clusters op Linux.

## <a name="next-steps"></a>Volgende stappen
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent_Azure.png

