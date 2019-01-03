---
title: Periodieke back-upconfiguratie inzicht in Azure Service Fabric | Microsoft Docs
description: Gebruik van Service Fabric periodieke back-up en herstellen van de functie voor het inschakelen van periodieke gegevens back-up van uw toepassingsgegevens.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: timlt
editor: hrushib
ms.assetid: FAA45B4A-0258-4CB3-A825-7E8F70F28401
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2018
ms.author: hrushib
ms.openlocfilehash: f2a1cd79a99e16460c96d28ebeb0a2bd68975361
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794240"
---
# <a name="understanding-periodic-backup-configuration-in-azure-service-fabric"></a>Informatie over periodieke back-upconfiguratie in Azure Service Fabric

Configuratie periodieke back-up van uw Reliable stateful services of Reliable Actors bestaat uit de volgende stappen uit:

1. **Het maken van back-upbeleid**: Een of meer back-upbeleid worden in deze stap is gemaakt, afhankelijk van de vereisten.

2. **Om back-up**: In deze stap koppelt u de back-upbeleid is gemaakt in **stap 1** aan de vereiste entiteiten _toepassing_, _Service_, of een _partitie_.

## <a name="create-backup-policy"></a>Back-upbeleid maken

Een back-upbeleid bestaat uit de volgende configuraties:

* **Terugzetten op het verlies van gegevens automatisch**: Geeft aan of de herstelbewerking wordt automatisch met behulp van de meest recente beschikbare back-up in het geval de partitie een gegevensverlies optreedt.

* **Maximum aantal incrementele back-ups**: Hiermee definieert u het maximum aantal incrementele back-ups moeten worden uitgevoerd tussen twee volledige back-ups. Maximum aantal incrementele back-ups opgeven de bovenste limiet. Een volledige back-up kan worden uitgevoerd voordat het opgegeven aantal incrementele back-ups wordt uitgevoerd in een van de volgende voorwaarden

    1. De replica heeft nooit genomen voor een volledige back-up omdat het primaire is geworden.

    2. Sommige van de records in logboek registreren omdat de laatste back-up is afgebroken.

    3. Replica doorgegeven de limiet MaxAccumulatedBackupLogSizeInMB.

* **Back-upschema**: De tijd of de frequentie waarmee periodieke back-ups maken. Een kunt back-ups om te worden herhaald op basis van opgegeven interval of op een vaste tijd dagelijks / wekelijks plannen.

    1. **Op basis van een frequentie van back-upschema**: Dit schematype moet worden gebruikt als de noodzaak is het back-up van gegevens met vaste intervallen. Gewenste tijdsinterval tussen twee opeenvolgende back-ups is gedefinieerd met behulp van de ISO8601-notatie. Op basis van een frequentie van back-upschema ondersteunt interval resolutie van maximaal minuut.
        ```json
        {
            "ScheduleKind": "FrequencyBased",
            "Interval": "PT10M"
        }
        ```

    2. **Back-upschema op basis van tijd**: Dit schematype moet worden gebruikt als de noodzaak te nemen van de back-up op specifieke tijdstippen van de dag of week. Frequentie schematype zijn dagelijks of wekelijks.
        1. **_Dagelijkse_ back-upschema op basis van tijd**: Dit schematype moet worden gebruikt als de id hoeft te nemen van de back-up op specifieke tijdstippen van de dag. Als dit opgeven, stelt u `ScheduleFrequencyType` naar _dagelijkse_; en stel `RunTimes` aan de lijst met de gewenste tijd gedurende de dag ISO8601-indeling hebben, datum en tijd opgegeven wordt genegeerd. Bijvoorbeeld, `0001-01-01T18:00:00` vertegenwoordigt _18:00:00 uur_ elke dag, datum wordt genegeerd _0001-01-01_. Onderstaande voorbeeld ziet u de configuratie van trigger dagelijkse back-up op _9:00 uur_ en _18:00:00 uur_ dagelijks.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Daily",
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

        2. **_Wekelijkse_ back-upschema op basis van tijd**: Dit schematype moet worden gebruikt als de id hoeft te nemen van de back-up op specifieke tijdstippen van de dag. Als dit opgeven, stelt u `ScheduleFrequencyType` naar _wekelijkse_; set `RunDays` aan lijst van dagen in een week wanneer back-up moet worden geactiveerd en stel `RunTimes` aan de lijst met de gewenste tijd gedurende de dag ISO8601-indeling hebben, datum en tijd opgegeven worden genegeerd. Lijst van dagen van een week bij het activeren van de periodieke back-up. Onderstaande voorbeeld ziet u de configuratie van trigger dagelijkse back-up op _9:00 uur_ en _18:00:00 uur_ tijdens maandag tot en met vrijdag.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Weekly",
                "RunDays": [
                   "Monday",
                   "Tuesday",
                   "Wednesday",
                   "Thursday",
                   "Friday"
                ],
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

* **Back-upopslag**: Hiermee geeft u de locatie op voor het uploaden van de back-ups. Opslag kan een Azure blob-archief of bestandsshare.
    1. **Azure blob-archief**: Dit opslagtype moet worden geselecteerd als de noodzaak voor store gegenereerde back-ups in Azure is. Beide _zelfstandige_ en _op basis van Azure_ clusters kunnen dit opslagtype gebruiken. Beschrijving voor dit opslagtype is vereist voor de verbindingstekenreeks en de naam van de container waar de back-ups moeten worden geüpload. Als de container met de opgegeven naam niet beschikbaar is, wordt tijdens het uploaden van een back-up wordt gemaakt.
        ```json
        {
            "StorageKind": "AzureBlobStore",
            "FriendlyName": "Azure_storagesample",
            "ConnectionString": "<Put your Azure blob store connection string here>",
            "ContainerName": "BackupContainer"
        }
        ```

    2. **Bestandsshare**: Dit opslagtype moet worden geselecteerd voor _zelfstandige_ clusters als de noodzaak is voor het opslaan van gegevens back-up maken van de on-premises. Beschrijving voor dit opslagtype vereist pad naar de bestandsshare waar de back-ups moeten worden geüpload. Toegang tot de bestandsshare kan worden geconfigureerd met behulp van een van de volgende opties
        1. _Geïntegreerde Windows-authenticatie_, waar de toegang tot de bestandsshare is opgegeven voor alle computers die behoren tot de Service Fabric-cluster. In dit geval, stelt de volgende velden configureren _bestandsshare_ op basis van de back-upopslag.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore"
            }
            ```

        2. _Beveiliging-bestandsshare met behulp van de gebruikersnaam en wachtwoord_, waar de toegang tot de bestandsshare is opgegeven voor specifieke gebruikers. File share storage specificatie biedt tevens de mogelijkheid om op te geven van de secundaire gebruikersnaam en het secundaire wachtwoord voor fall-back-referenties als verificatie is mislukt met de primaire gebruikersnaam en het primaire wachtwoord. In dit geval, stelt de volgende velden configureren _bestandsshare_ op basis van de back-upopslag.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore",
                "PrimaryUserName": "backupaccount",
                "PrimaryPassword": "<Password for backupaccount>",
                "SecondaryUserName": "backupaccount2",
                "SecondaryPassword": "<Password for backupaccount2>"
            }
            ```

> [!NOTE]
> Zorg ervoor dat de betrouwbaarheid van de opslag voldoet aan of vereisten van de betrouwbaarheid van back-upgegevens overschrijdt.
>

* **Bewaarbeleid**: Hiermee geeft u het beleid voor het bewaren van back-ups in de geconfigureerde opslag. Alleen eenvoudige bewaarbeleid wordt ondersteund.
    1. **Basic bewaarbeleid**: Dit beleid voor Gegevensretentie kunt om te controleren of gebruik van de optimale opslag door het verwijderen van back-upbestanden die u niet meer nodig. `RetentionDuration` om in te stellen de tijdsspanne waarvoor back-ups zijn vereist om te worden bewaard in de opslag kan worden opgegeven. `MinimumNumberOfBackups` is een optionele parameter die kan worden opgegeven om ervoor te zorgen dat het opgegeven aantal back-ups blijft altijd ongeacht behouden de `RetentionDuration`. Onderstaande voorbeeld ziet u de configuratie voor het bewaren van back-ups voor _10_ dagen en staat niet toe dat het aantal back-ups onder _20_.

        ```json
        {
            "RetentionPolicyType": "Basic",
            "RetentionDuration" : "P10D",
            "MinimumNumberOfBackups": 20
        }
        ```

> [!IMPORTANT]
> Zorg ervoor dat de bewaartermijn in het bewaarbeleid is geconfigureerd voor minder dan 24 dagen anders resulteert dit in de service gaan naar het quorum verloren na replica-failover back-up herstellen vanwege een probleem in de runtime.

## <a name="enable-periodic-backup"></a>Periodieke back-up inschakelen
Na het definiëren van back-upbeleid om te voldoen aan vereisten voor back-up van gegevens, het back-upbeleid moet worden op de juiste wijze gekoppeld ofwel met een _toepassing_, of _service_, of een _partitie_.

### <a name="hierarchical-propagation-of-backup-policy"></a>Hiërarchische doorgifte van back-upbeleid
In Service Fabric kan de relatie tussen de toepassing, service en partities is hiërarchisch zoals uitgelegd in [toepassingsmodel](./service-fabric-application-model.md). Back-beleid kan worden gekoppeld aan een _toepassing_, _service_, of een _partitie_ in de hiërarchie. Back-upbeleid is hiërarchisch worden doorgevoerd naar het volgende niveau. Ervan uitgaande dat er is slechts één back-upbeleid is gemaakt en die zijn gekoppeld aan een _toepassing_, alle stateful partities die behoren tot alle _betrouwbare stateful services_ en _Reliable Actors_ van de _toepassing_ zal worden back-up met behulp van de back-upbeleid. Of als de back-upbeleid is gekoppeld aan een _betrouwbare stateful service_, alle partities ervan zal worden back-up met behulp van de back-upbeleid.

### <a name="overriding-backup-policy"></a>Back-upbeleid te overschrijven
Er is mogelijk een scenario waar back-up van gegevens met dezelfde back-upschema vereist voor alle services van de toepassing, met uitzondering van bepaalde services waarbij de noodzaak is gegevensback-up met behulp van hogere frequentie planning of back-up maken naar een ander opslagaccount hebben of bestandsshare. Om dergelijke scenario's op te lossen, biedt back-up herstellen service onderdrukking doorgegeven beleid in het service- en partitiefilters bereik-mogelijkheid. Wanneer de back-upbeleid is gekoppeld aan _service_ of _partitie_, dit back-upbeleid is doorgegeven, vervangt, indien van toepassing.

### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt setup met twee toepassingen _MyApp_A_ en _MyApp_B_. Toepassing _MyApp_A_ bevat twee betrouwbare Stateful services _SvcA1_ & _SvcA3_, en een Reliable Actor-service, _ActorA2_. _SvcA1_ bevat drie partities tijdens _ActorA2_ en _SvcA3_ twee partities bevatten.  Toepassing _MyApp_B_ bevat drie betrouwbare Stateful services _SvcB1_, _SvcB2_, en _SvcB3_. _SvcB1_ en _SvcB2_ bevat twee partities tijdens _SvcB3_ drie partities bevat.

Wordt ervan uitgegaan dat deze toepassingen gegevens back-vereisten als volgt zijn

1. MyApp_A
    1. Dagelijkse back-up van gegevens voor alle partities van alle _betrouwbare Stateful services_ en _Reliable Actors_ die behoren tot de toepassing. Back-gegevens uploaden naar de locatie _BackupStore1_.

    2. Een van de services, _SvcA3_, gegevensback-up vereist om het uur.

    3. Gegevensgrootte in partitie _SvcA1_P2_ is hoger dan verwacht en de back-upgegevens moet worden opgeslagen naar andere opslaglocatie _BackupStore2_.

2. MyApp_B
    1. Back-up van gegevens maken elke zondag om 8:00 uur voor alle partities van _SvcB1_ service. Back-gegevens uploaden naar de locatie _BackupStore1_.

    2. Back-up van gegevens maken elke dag om 8:00 uur voor de partitie _SvcB2_P1_. Back-gegevens uploaden naar de locatie _BackupStore1_.

Om deze vereisten voor het back-up van gegevens op te lossen, back-upbeleid BP_1 naar BP_5 worden gemaakt en back-up als volgt is ingeschakeld.
1. MyApp_A
    1. Back-upbeleid maken _BP_1_, met op basis van een frequentie van back-upschema waarvan de frequentie is ingesteld op 24 uur. en back-upopslag geconfigureerd voor het gebruik van de opslaglocatie _BackupStore1_. Dit beleid inschakelt voor toepassing _MyApp_A_ met behulp van [toepassing Backup inschakelen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableapplicationbackup) API. Deze actie kunt u gegevensback-up met behulp van back-upbeleid _BP_1_ voor alle partities van _betrouwbare Stateful services_ en _Reliable Actors_ die behoren tot de toepassing  _MyApp_A_.

    2. Back-upbeleid maken _BP_2_, met back-upschema frequentie op basis van waar de frequentie is ingesteld op 1 uur. en back-upopslag geconfigureerd voor het gebruik van de opslaglocatie _BackupStore1_. Dit beleid voor de service inschakelen _SvcA3_ met behulp van [Service Backup inschakelen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableservicebackup) API. Deze actie is doorgegeven beleid overschreven _BP_1_ door back-upbeleid expliciet ingeschakeld _BP_2_ voor alle partities van de service _SvcA3_ leidt tot de back-up met behulp van back-up beleid _BP_2_ voor deze partities.

    3. Back-upbeleid maken _BP_3_, met op basis van een frequentie van back-upschema waarvan de frequentie is ingesteld op 24 uur. en back-upopslag geconfigureerd voor het gebruik van de opslaglocatie _BackupStore2_. Inschakelen van dit beleid voor de partitie _SvcA1_P2_ met behulp van [partitie Backup inschakelen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enablepartitionbackup) API. Deze actie is doorgegeven beleid overschreven _BP_1_ door back-upbeleid expliciet ingeschakeld _BP_3_ voor partitie _SvcA1_P2_.

2. MyApp_B
    1. Back-upbeleid maken _BP_4_, met op basis van tijd back-upschema waarvan schematype frequentie is ingesteld op wekelijks, voer dagen is ingesteld op zondag en uitvoeringstijden is ingesteld op 8:00 uur. Back-upopslag geconfigureerd voor het gebruik van de opslaglocatie _BackupStore1_. Dit beleid voor de service inschakelen _SvcB1_ met behulp van [Service Backup inschakelen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableservicebackup) API. Deze actie kunt u gegevensback-up met behulp van back-upbeleid _BP_4_ voor alle partities van de service _SvcB1_.

    2. Back-upbeleid maken _BP_5_, met back-upschema op basis van tijd waar schematype frequentie is ingesteld op dagelijks en uitvoeringstijden is ingesteld op 8:00 uur. Back-upopslag geconfigureerd voor het gebruik van de opslaglocatie _BackupStore1_. Inschakelen van dit beleid voor de partitie _SvcB2_P1_ met behulp van [partitie Backup inschakelen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enablepartitionbackup) API. Deze actie kunt u gegevensback-up met behulp van back-upbeleid _BP_5_ voor partitie _SvcB2_P1_.

Volgende diagram ziet u expliciet ingeschakelde back-upbeleid en back-upbeleid doorgegeven.

![Service Fabric-toepassingshiërarchie][0]

## <a name="disable-backup"></a>Back-up uitschakelen
Back-upbeleid kunnen worden uitgeschakeld als er geen moeten back-upgegevens. Back-upbeleid ingeschakeld op een _toepassing_ kan alleen worden uitgeschakeld op hetzelfde _toepassing_ met behulp van [uitschakelen toepassing Backup](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disableapplicationbackup) API, back-up-beleid is ingeschakeld op een _service_ kan worden uitgeschakeld op hetzelfde _service_ met behulp van [uitschakelen Service Backup](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disableservicebackup) API, en back-upbeleid ingeschakeld op een _partitie_ kan worden uitgeschakeld op hetzelfde _partitie_ met behulp van [uitschakelen partitie Backup](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disablepartitionbackup) API.

* Uitschakelen van back-upbeleid voor een _toepassing_ stopt alle gegevens op periodieke back-ups gebeurt als gevolg van het doorgeven van het back-upbeleid betrouwbare Stateful servicepartities of Reliable Actor-partities.

* Uitschakelen van back-upbeleid voor een _service_ stopt alle gegevens op periodieke back-ups gebeurt als gevolg van dit back-upbeleid worden doorgegeven aan de partities van de _service_.

* Uitschakelen van back-upbeleid voor een _partitie_ stopt alle gegevens op periodieke back-up gebeurt vanwege het back-upbeleid op de partitie.

* Bij het uitschakelen van back-up voor een entity(application/service/partition) `CleanBackup` kan worden ingesteld op _waar_ verwijderen van alle de back-ups in de geconfigureerde opslag.
    ```json
    {
        "CleanBackup": true 
    }
    ```

## <a name="suspend--resume-backup"></a>Onderbreken en hervatten van back-up
Bepaalde situatie kan tijdelijke onderbreking van periodieke back-up van gegevens van de vraag. In deze situatie, afhankelijk van het vereiste onderbreken back-up API kan worden gebruikt bij een _toepassing_, _Service_, of _partitie_. Periodieke back-up onderbreking kan worden overgedragen via een substructuur van de hiërarchie van de toepassing vanaf het punt dat deze wordt toegepast. 

* Wanneer de onderbreking is toegepast op een _toepassing_ met behulp van [toepassing back-onderbreken](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendapplicationbackup) API en vervolgens alle services en partities in deze toepassing zijn onderbroken voor periodieke back-up van gegevens.

* Wanneer de onderbreking is toegepast op een _Service_ met behulp van [Service back-onderbreken](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendservicebackup) API en vervolgens alle partities in deze service wordt onderbroken voor periodieke back-up van gegevens.

* Wanneer de onderbreking is toegepast op een _partitie_ met behulp van [partitie back-onderbreken](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendpartitionbackup) API en vervolgens onderbreekt u partities in deze service wordt onderbroken voor periodieke back-up van gegevens.

Zodra de noodzaak van opschorting uitgevoerd is, kan klikt u vervolgens de gegevens op periodieke back-up worden hersteld met behulp van de back-up API respectieve hervatten. Periodieke back-up moet worden hervat tegelijkertijd _toepassing_, _service_, of _partitie_ waar deze werd onderbroken.

* Als de onderbreking is toegepast op een _toepassing_, en vervolgens moet worden hervat met behulp van [toepassing back-up hervatten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumeapplicationbackup) API. 

* Als de onderbreking is toegepast op een _Service_, en vervolgens moet worden hervat met behulp van [Service back-up hervatten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumeservicebackup) API.

* Als de onderbreking is toegepast op een _partitie_, en vervolgens moet worden hervat met behulp van [partitie back-up hervatten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumepartitionbackup) API.

### <a name="difference-between-suspend-and-disable-backups"></a>Verschil tussen de back-ups van de stand-by- en uitschakelen
Schakel back-up moet worden gebruikt als back-ups niet langer vereist voor een bepaalde toepassing, service of partitie zijn. Één kan infact aanroepen uitschakelen back-upaanvraag samen met de parameter schone back-ups te zijn ' True ' wat betekent dat alle bestaande back-ups worden ook verwijderd. Echter, onderbreken moet worden gebruikt in scenario's waarbij een wil uitschakelen back-ups tijdelijk zoals wanneer de lokale schijf vol raakt of uploaden van back-up is mislukt vanwege bekende netwerkprobleem enzovoort. 

Hoewel uitschakelen alleen op een niveau kan worden aangeroepen die eerder is ingeschakeld voor back-up explicilty echter onderbreking kan worden toegepast op elk niveau die momenteel is ingeschakeld voor back-up ofwel rechtstreeks of via overname / hiërarchie. Als de back-up is ingeschakeld op het toepassingsniveau van een, kan een bijvoorbeeld uitschakelen aanroepen alleen op het toepassingsniveau van de echter onderbreken kan worden aangeroepen op de toepassing, een service of de partitie in die toepassing. 

## <a name="auto-restore-on-data-loss"></a>Automatisch herstel op verlies van gegevens
De servicepartitie kan gegevens vanwege onverwachte fouten worden verbroken. Bijvoorbeeld, wordt de schijf voor twee van de drie replica's voor een partitie (met inbegrip van de primaire replica) beschadigd of wordt gewist.

Bij het Service Fabric detecteert dat de partitie bevindt zich in het verlies van gegevens, roept het `OnDataLossAsync` interface-methode voor de partitie en wordt verwacht dat de partitie om de vereiste actie afkomstig zijn uit het verlies van gegevens te ondernemen. In dit geval, als de effectieve back-upbeleid op de partitie heeft `AutoRestoreOnDataLoss` vlag ingesteld op `true` en vervolgens de herstelbewerking automatisch met behulp van meest recente beschikbare back-up voor deze partitie wordt geactiveerd.

## <a name="get-backup-configuration"></a>Back-upconfiguratie ophalen
Afzonderlijke API's beschikbaar worden gesteld aan back-upconfiguratie informatie op een _toepassing_, _service_, en _partitie_ bereik. [Informatie over de configuratie van de toepassing back-up](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getapplicationbackupconfigurationinfo), [Service back-up-configuratie-informatie ophalen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getservicebackupconfigurationinfo), en [partitie back-up-configuratie-informatie ophalen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupconfigurationinfo) zijn deze respectievelijk API's. Deze API's retourneren voornamelijk van toepassing back-upbeleid, bereik waarop de back-upbeleid toegepast en back-up onderbreking details is. Hieronder volgt een korte beschrijving over geretourneerde resultaten van deze API's.

- Back-upconfiguratie toepassingsgegevens: bevat de details van back-upbeleid toegepast op de toepassing en alle te veel ridden beleidsregels op services en partities die behoren tot de toepassing. Dit omvat ook de onderbreking-informatie voor de toepassing en -services, en partities.

- Service-back-upconfiguratie info: bevat de details van de effectieve back-upbeleid op de service en het bereik op waarop dit beleid is toegepast en alle te veel ridden beleidsregels op de partities. Dit omvat ook de onderbreking-informatie voor de service en de partities.

- Partitie-informatie over de back-configuratie: bevat de details van een doeltreffende back-upbeleid op de partitie en het bereik waarop dit beleid is toegepast. Dit omvat ook de onderbreking-informatie voor de partities.

## <a name="list-available-backups"></a>Lijst met beschikbare back-ups

Beschikbare back-ups kunnen worden weergegeven met behulp van back-up lijst API ophalen. Resultaat van API-aanroep bevat back-upinformatie items in verband met alle de back-ups beschikbaar op de back-upopslag, dat wordt geconfigureerd in de desbetreffende back-upbeleid. Verschillende varianten van deze API worden aan de lijst met beschikbare back-ups die behoren tot een toepassing, service of partitie verstrekt. Deze API's bieden ondersteuning aan de _nieuwste_ beschikbare back-up van alle toepasselijke partities of het filteren van back-ups op basis van _begindatum_ en _einddatum_.

Deze API's bieden ook ondersteuning voor paginering van de resultaten als _MaxResults_ parameter is ingesteld op nul positief geheel getal zijn en de API retourneert maximale _MaxResults_ back-up van info-items. In het geval, er zijn meer back-upinformatie items beschikbaar dan de _MaxResults_ waarde, wordt een vervolgtoken geretourneerd. Geldige voortzetting van token parameter kan worden gebruikt om op te halen volgende set resultaten. Wanneer het token geldig voortzetting-waarde wordt doorgegeven aan de volgende aanroep van de API, retourneert de API volgende set resultaten. Er is geen vervolgtoken is opgenomen in het antwoord wanneer alle beschikbare resultaten worden geretourneerd.

Hieronder volgt de beknopte informatie over de ondersteunde varianten.

- [Lijst met back-up van toepassingen ophalen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getapplicationbackuplist): Retourneert een lijst met back-ups beschikbaar voor elke partitie die behoren tot de opgegeven Service Fabric-toepassing.

- [Service-back-lijst ophalen](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getservicebackuplist): Retourneert een lijst met back-ups beschikbaar voor elke partitie die behoren tot de opgegeven Service Fabric-service.
 
- [Ophalen van de back-up Partitielijst](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackuplist): Retourneert een lijst met back-ups beschikbaar voor de opgegeven partitie.

## <a name="next-steps"></a>Volgende stappen
- [Naslaginformatie over REST API voor back-up terugzetten](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/BackupPolicyAssociationExample.png
