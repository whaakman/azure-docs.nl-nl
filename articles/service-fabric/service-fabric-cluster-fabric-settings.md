---
title: Wijzig de instellingen van Azure Service Fabric-cluster | Microsoft Docs
description: In dit artikel beschrijft de fabric-instellingen en de fabric-upgrade beleidsregels die u kunt aanpassen.
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: chackdan
ms.openlocfilehash: c6289df50d17de1ef542abbf617eb1cdb2f32311
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Instellingen voor Service Fabric-cluster en het beleid voor Fabric-Upgrade aanpassen
Dit document wordt uitgelegd hoe de verschillende fabric-instellingen aanpassen en de fabric-upgrade beleid voor uw Service Fabric-cluster. U kunt aanpassen via de [Azure-portal](https://portal.azure.com) of met een Azure Resource Manager-sjabloon.

> [!NOTE]
> Niet alle instellingen zijn beschikbaar in de portal. Als een instelling hieronder vermelde niet beschikbaar via de portal aanpassen met behulp van een Azure Resource Manager-sjabloon.
> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Clusterinstellingen van het met behulp van Resource Manager-sjablonen aanpassen
De volgende stappen uit te laten zien hoe u een nieuwe instelling toevoegen *MaxDiskQuotaInMB* naar de *Diagnostics* sectie.

1. Ga naar https://resources.azure.com
2. Navigeer naar uw abonnement door het uitbreiden van **abonnementen** -> **resourcegroepen** -> **Microsoft.ServiceFabric**  ->   **\<Uw Clusternaam >**
3. Selecteer in de rechterbovenhoek, **lezen/schrijven.**
4. Selecteer **bewerken** en werk de `fabricSettings` JSON-element en een nieuw element toevoegen:

```
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Hieronder volgt een lijst van Fabric instellingen die u kunt aanpassen, geordend door de sectie.

### <a name="section-name-diagnostics"></a>Sectienaam: diagnostische gegevens
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| ConsumerInstances |Tekenreeks | Dynamisch |De lijst van DCA consumer exemplaren. |
| ProducerInstances |Tekenreeks | Dynamisch |De lijst van DCA producent exemplaren. |
| AppEtwTraceDeletionAgeInDays |Int, de standaardwaarde is 3 | Dynamisch |Aantal dagen waarna we oude ETL-bestanden met ETW toepassingstraceringen verwijderen. |
| AppDiagnosticStoreAccessRequiresImpersonation |BOOL, de standaardwaarde is true | Dynamisch |Imitatie wel of niet is vereist wanneer toegang tot diagnostische voor de toepassing is opslaat. |
| MaxDiskQuotaInMB |Int, de standaardwaarde is 65536 | Dynamisch |Schijfquota in MB voor Windows Fabric-logboekbestanden. |
| DiskFullSafetySpaceInMB |Int, standaard is 1024 | Dynamisch |Resterende schijfruimte in MB te beschermen tegen door DCA worden gebruikt. |
| ApplicationLogsFormatVersion |Int, de standaardwaarde is 0 | Dynamisch |Versie voor de toepassing Logboeken indeling. Ondersteunde waarden zijn 0 en 1. Versie 1 bevat meer velden uit de record ETW-gebeurtenis dan versie 0. |
| ClusterId |Tekenreeks | Dynamisch |De unieke id van het cluster. Dit wordt gegenereerd wanneer het cluster is gemaakt. |
| EnableTelemetry |BOOL, de standaardwaarde is true | Dynamisch |Dit zal in- of uitschakelen van telemetrie. |
| EnableCircularTraceSession |BOOL, de standaardwaarde is ONWAAR | Statisch |Vlag geeft aan of cirkelvormige traceersessies moeten worden gebruikt. |

### <a name="section-name-traceetw"></a>De naam van de sectie: Trace/Etw
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| Niveau |Int, de standaardwaarde is 4 | Dynamisch |Traceerniveau etw kan duren voordat de waarden 1, 2, 3, 4. Wordt alleen ondersteund u moet ervoor zorgen dat het traceerniveau op 4 |

### <a name="section-name-performancecounterlocalstore"></a>Sectienaam: PerformanceCounterLocalStore
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| IsEnabled |BOOL, de standaardwaarde is true | Dynamisch |Vlag wordt aangegeven of het verzamelen van prestatiemeteritems op het lokale knooppunt is ingeschakeld. |
| SamplingIntervalInSeconds |Int, de standaardwaarde is 60 | Dynamisch |Steekproefinterval voor prestatiemeteritems worden verzameld. |
| Prestatiemeteritems |Tekenreeks | Dynamisch |Door komma's gescheiden lijst met te verzamelen prestatiemeteritems. |
| MaxCounterBinaryFileSizeInMB |Int, de standaardwaarde is 1 | Dynamisch |Maximale grootte (in MB) voor elk prestaties prestatiemeteritems binaire bestand. |
| NewCounterBinaryFileCreationIntervalInMinutes |Int, de standaardwaarde is 10 | Dynamisch |Maximale interval (in seconden) waarna een nieuw prestaties prestatiemeteritems binaire bestand is gemaakt. |

### <a name="section-name-setup"></a>Sectienaam: Setup
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| FabricDataRoot |Tekenreeks | Niet toegestaan |Hoofdmap voor service Fabric-gegevens. Standaard voor Azure d:\svcfab is |
| FabricLogRoot |Tekenreeks | Niet toegestaan |Hoofdmap voor service fabric-logboek. Dit is waar SF-logboeken en traceringen worden geplaatst. |
| ServiceRunAsAccountName |Tekenreeks | Niet toegestaan |De accountnaam waaronder fabric host-service uit te voeren. |
| SkipFirewallConfiguration |BOOL, de standaardwaarde is ONWAAR | Niet toegestaan |Hiermee geeft u aan als firewall-instellingen worden ingesteld door het systeem moeten of niet. Dit geldt alleen als u windows firewall gebruikt. Als u firewalls van derden gebruikt, moet vervolgens u de poorten openen voor het systeem en -toepassingen gebruiken |
|NodesToBeRemoved|tekenreeks, standaardwaarde is ""| Dynamisch |De knooppunten die moeten worden verwijderd als onderdeel van de upgrade van de configuratie. (Alleen voor zelfstandige implementaties)|
|ContainerNetworkSetup|BOOL, standaard is ONWAAR| Statisch |Hiermee geeft u op of u voor het instellen van een container-netwerk.|
|ContainerNetworkName|tekenreeks, default is L""| Statisch |De netwerknaam moet worden gebruikt bij het instellen van een container-netwerk.|

### <a name="section-name-transactionalreplicator"></a>Sectienaam: TransactionalReplicator
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| MaxCopyQueueSize |Uint, de standaardwaarde is 16384 | Statisch |Dit is de maximale waarde definieert de aanvankelijke grootte van de wachtrij die replicatiebewerkingen onderhoudt. Houd er rekening mee dat deze een macht van 2 moet. Als tijdens runtime de wachtrij neemt toe aan de grootte van deze bewerking wordt de snelheid van tussen de primaire en secundaire replicaties. |
| BatchAcknowledgementInterval | Tijd in seconden, de standaardwaarde is 0,015 | Statisch | Geef de interval in seconden. Bepaalt de hoeveelheid tijd dat de replicator moet wachten na de ontvangst van een bewerking voor het verzenden van een bevestiging. Andere bewerkingen zijn ontvangen tijdens deze periode wordt hun bevestigingen terug in een enkel bericht verzonden verminderen netwerkverkeer maar mogelijk verminderen de doorvoer van de replicatie -> hebben. |
| MaxReplicationMessageSize |Uint, de standaardwaarde is 52428800 | Statisch | Maximale berichtgrootte van replicatiebewerkingen. Standaardwaarde is 50MB. |
| ReplicatorAddress |tekenreeks, default is 'localhost:0' | Statisch | Het eindpunt in de vorm van een tekenreeks-'IP: poort' die met de Windows Fabric-replicatie wordt gebruikt voor het tot stand brengen van verbindingen met andere replica's kunnen bewerkingen verzenden/ontvangen. |
| InitialPrimaryReplicationQueueSize |Uint, de standaardwaarde is 64 | Statisch |Deze waarde bepaalt de aanvankelijke grootte van de wachtrij die de replicatiebewerkingen op de primaire onderhoudt. Houd er rekening mee dat deze een macht van 2 moet.|
| MaxPrimaryReplicationQueueSize |Uint, de standaardwaarde is 8192 | Statisch |Dit is het maximum aantal bewerkingen die kunnen bestaan in de wachtrij primaire replicatie. Houd er rekening mee dat deze een macht van 2 moet. |
| MaxPrimaryReplicationQueueMemorySize |Uint, de standaardwaarde is 0 | Statisch |Dit is de maximale waarde van de wachtrij voor de primaire replicatie in bytes. |
| InitialSecondaryReplicationQueueSize |Uint, de standaardwaarde is 64 | Statisch |Deze waarde bepaalt de aanvankelijke grootte van de wachtrij die de replicatiebewerkingen op de secundaire onderhoudt. Houd er rekening mee dat deze een macht van 2 moet. |
| MaxSecondaryReplicationQueueSize |Uint, de standaardwaarde is 16384 | Statisch |Dit is het maximum aantal bewerkingen die kunnen bestaan in de secundaire replicatiewachtrij. Houd er rekening mee dat deze een macht van 2 moet. |
| MaxSecondaryReplicationQueueMemorySize |Uint, de standaardwaarde is 0 | Statisch |Dit is de maximale waarde van de secundaire replicatiewachtrij in bytes. |
| SecondaryClearAcknowledgedOperations |BOOL, de standaardwaarde is ONWAAR | Statisch |BOOL die bepaalt of de bewerkingen op de secundaire replicator gewist zodra ze zijn bevestigd naar de primaire (leeggemaakt op de schijf). Instellingen voor dit in op TRUE, kan leiden tot extra leesbewerkingen op de nieuwe primaire tijdens het afvangen van replica's na een failover. |
| MaxMetadataSizeInKB |Int, de standaardwaarde is 4 |Niet toegestaan|Maximale grootte van de metagegevens van het synchronisatielogboek stroom. |
| MaxRecordSizeInKB |Uint, standaard is 1024 |Niet toegestaan| Maximale grootte van een stroom logboekrecord. |
| CheckpointThresholdInMB |Int, de standaardwaarde is 50 |Statisch|Een controlepunt wordt gestart wanneer het logboek-Gebruik deze waarde overschrijdt. |
| MaxAccumulatedBackupLogSizeInMB |Int, de standaardwaarde is 800 |Statisch|Maximale verzameld grootte (in MB) van de back-uplogboeken in een keten van de opgegeven back-uplogboek. Een incrementele back-aanvragen mislukken als de incrementele back-up een back-uplogboek waardoor de totale back-uplogboeken sinds de relevante volledige back-up niet groter zijn dan de grootte van deze genereert. Gebruiker is in dergelijke gevallen vereist voor een volledige back-up te maken. |
| MaxWriteQueueDepthInKB |Int, de standaardwaarde is 0 |Niet toegestaan| Int voor maximum schrijven wachtrijdiepte die het logboek core zoals opgegeven in kilobytes kunt gebruiken voor het logboek dat is gekoppeld aan deze replica. Deze waarde is het maximum aantal bytes dat in behandeling tijdens het bijwerken van de core berichtenlogboek zijn kan. 0 zijn voor het logboek core berekenen een geschikte waarde of een meervoud van 4 kan zijn. |
| SharedLogId |Tekenreeks |Niet toegestaan|Gedeelde logboek-id. Dit is een guid en moet uniek zijn voor elke gedeelde logboek. |
| SharedLogPath |Tekenreeks |Niet toegestaan|Pad naar het gedeelde logboek. Het standaard gedeelde logboek wordt gebruikt als deze waarde leeg is. |
| SlowApiMonitoringDuration |Tijd in seconden, is standaard 300 |Statisch| Duur voor api opgeven voordat de waarschuwing health gebeurtenis wordt geactiveerd.|
| MinLogSizeInMB |Int, de standaardwaarde is 0 |Statisch|De minimumgrootte van de transactionele logboek. Het logboek niet mag worden afgekapt tot een grootte dan deze instelling. 0 geeft aan dat de replicator de minimale logboekgrootte volgens andere instellingen bepaalt. Deze waarde verhoogt, wordt de mogelijkheid van de handeling gedeeltelijke kopie en incrementele back-ups sinds de kans op relevante logboekrecords worden afgekapt wordt verlaagd. |

### <a name="section-name-fabricclient"></a>Sectienaam: FabricClient
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| NodeAddresses |tekenreeks, standaardwaarde is "" |Statisch|Een verzameling van adressen (verbindingsreeksen) op verschillende knooppunten die kunnen worden gebruikt om te communiceren met de de Naming Service. In eerste instantie verbindt de Client een van de adressen willekeurig selecteren. Als meer dan een verbindingsreeks wordt verstrekt en een verbinding is mislukt vanwege een communicatie- of time-outfout; de Client-switches sequentieel het volgende adres gebruiken. Zie de naamgeving van het adres van de sectie voor meer informatie over de semantiek voor nieuwe pogingen opnieuw. |
| ConnectionInitializationTimeout |Tijd in seconden, de standaardwaarde is 2 |Dynamisch|Geef de interval in seconden. Time-outinterval voor elke keer client verbinding probeert te openen van een verbinding met de gateway. |
| Partitionlocationcachelimit op |Int, de standaardwaarde is 100000 |Statisch|Het aantal partities in de cache opgeslagen voor de omzetting van de service (ingesteld op 0 in voor onbeperkt). |
| ServiceChangePollInterval |Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Geef de interval in seconden. Het interval tussen opeenvolgende polls voor service van de client in de gateway voor geregistreerde service wijziging meldingen retouraanroepen verandert. |
| KeepAliveIntervalInSeconds |Int, de standaardwaarde is 20 |Statisch|Het interval waarmee het transport FabricClient keepalive-berichten naar de gateway verzendt. Voor 0; keepAlive is uitgeschakeld. Moet een positieve waarde zijn. |
| HealthOperationTimeout |Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Geef de interval in seconden. De time-out voor een rapportbericht verzonden naar de Health Manager. |
| HealthReportSendInterval |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Geef de interval in seconden. Het interval waarmee rapportage onderdeel stuurt een samengevoegde statusrapporten voor Health Manager. |
| HealthReportRetrySendInterval |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Geef de interval in seconden. Het interval waarmee rapportage onderdeel opnieuw verzendt samengevoegde statusrapporten voor Health Manager. |
| RetryBackoffInterval |Tijd in seconden, de standaardwaarde is 3 |Dynamisch|Geef de interval in seconden. Het interval weer uitschakelen voordat u de bewerking. |
| MaxFileSenderThreads |Uint, de standaardwaarde is 10 |Statisch|Het maximum aantal bestanden die parallel worden overgedragen. |

### <a name="section-name-common"></a>Sectienaam: algemene
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| PerfMonitorInterval |Tijd in seconden, de standaardwaarde is 1 |Dynamisch|Geef de interval in seconden. Prestaties controle-interval. Instellen op 0 of een negatieve waarde wordt de bewaking uitgeschakeld. |

### <a name="section-name-healthmanager"></a>Sectienaam: HealthManager
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |BOOL, de standaardwaarde is ONWAAR |Statisch|Het statusbeleid voor de evaluatie van het cluster: per toepassing type health evaluatie inschakelen. |

### <a name="section-name-nodedomainids"></a>Sectienaam: NodeDomainIds
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| UpgradeDomainId |tekenreeks, standaardwaarde is "" |Statisch|Beschrijft het een knooppunt tot een behoort upgradedomein. |
| PropertyGroup |NodeFaultDomainIdCollection |Statisch|Hierin wordt beschreven in de domeinen met fouten die een knooppunt tot een behoort. Het foutdomein wordt gedefinieerd via een URI die de locatie van het knooppunt in het datacenter beschrijft.  Fout met betrekking tot domein URI's zijn in de notatie fd: / fd/gevolgd door een padsegment URI.|

### <a name="section-name-nodeproperties"></a>Sectienaam: NodeProperties
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| PropertyGroup |NodePropertyCollectionMap |Statisch|Een verzameling van tekenreeks sleutel-waardeparen voor eigenschappen van het knooppunt. |

### <a name="section-name-nodecapacities"></a>Sectienaam: NodeCapacities
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| PropertyGroup |NodeCapacityCollectionMap |Statisch|Een verzameling van knooppunt capaciteitswaarden voor andere metrische gegevens. |

### <a name="section-name-fabricnode"></a>Sectienaam: FabricNode
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| StateTraceInterval |Tijd in seconden, is standaard 300 |Statisch|Geef de interval in seconden. Het interval voor het knooppunt status op elk knooppunt en van knooppunten op FM/FMM traceren. |
| StartApplicationPortRange |Int, de standaardwaarde is 0 |Statisch|Starten van de toepassing-poorten die worden beheerd door de hosting-subsysteem. Vereist als EndpointFilteringEnabled ingesteld op true in Hosting is. |
| EndApplicationPortRange |Int, de standaardwaarde is 0 |Statisch|Einde (geen inclusief) van de toepassing-poorten die worden beheerd door de hosting-subsysteem. Vereist als EndpointFilteringEnabled ingesteld op true in Hosting is. |
| ClusterX509StoreName |tekenreeks, standaardwaarde is "Mijn" |Dynamisch|Naam van het X.509-certificaatarchief dat cluster certificaat voor het beveiligen van communicatie binnen het cluster bevat. |
| ClusterX509FindType |tekenreeks, default is 'FindByThumbprint' |Dynamisch|Geeft aan hoe om te zoeken naar cluster-certificaat in het archief van opgegeven waarden ClusterX509StoreName ondersteund: 'FindByThumbprint'; 'Findbysubjectname zijn' met 'Findbysubjectname zijn'; Wanneer er meerdere overeenkomsten; de categorie met de verst vervaldatum wordt gebruikt. |
| ClusterX509FindValue |tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het cluster certificaat vinden. |
| ClusterX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het cluster certificaat vinden. |
| ServerAuthX509StoreName |tekenreeks, standaardwaarde is "Mijn" |Dynamisch|Naam van het X.509-certificaatarchief die het servercertificaat voor entrée service bevat. |
| ServerAuthX509FindType |tekenreeks, default is 'FindByThumbprint' |Dynamisch|Geeft aan hoe om te zoeken naar servercertificaat in het archief dat is opgegeven door de waarde ServerAuthX509StoreName ondersteund: FindByThumbprint; Findbysubjectname zijn. |
| ServerAuthX509FindValue |tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het servercertificaat te vinden. |
| ServerAuthX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het servercertificaat te vinden. |
| ClientAuthX509StoreName |tekenreeks, standaardwaarde is "Mijn" |Dynamisch|Naam van het X.509-certificaatarchief dat certificaat voor de rol van standaard admin FabricClient bevat. |
| ClientAuthX509FindType |tekenreeks, default is 'FindByThumbprint' |Dynamisch|Geeft aan hoe om te zoeken naar certificaat in het archief dat is opgegeven door de waarde ClientAuthX509StoreName ondersteund: FindByThumbprint; Findbysubjectname zijn. |
| ClientAuthX509FindValue |tekenreeks, standaardwaarde is "" | Dynamisch|Zoeken filterwaarde voor het certificaat voor de rol van standaard admin FabricClient vinden. |
| ClientAuthX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het certificaat voor de rol van standaard admin FabricClient vinden. |
| UserRoleClientX509StoreName |tekenreeks, standaardwaarde is "Mijn" |Dynamisch|Naam van het X.509-certificaatarchief dat certificaat voor de standaardgebruikersrol FabricClient bevat. |
| UserRoleClientX509FindType |tekenreeks, default is 'FindByThumbprint' |Dynamisch|Geeft aan hoe om te zoeken naar certificaat in het archief dat is opgegeven door de waarde UserRoleClientX509StoreName ondersteund: FindByThumbprint; Findbysubjectname zijn. |
| UserRoleClientX509FindValue |tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het certificaat voor de standaardgebruikersrol FabricClient vinden. |
| UserRoleClientX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het certificaat voor de standaardgebruikersrol FabricClient vinden. |

### <a name="section-name-paas"></a>Sectienaam: Paas
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| ClusterId |tekenreeks, standaardwaarde is "" |Niet toegestaan|X509 archief wordt gebruikt voor de infrastructuur voor de beveiliging van de configuratie-certificaat. |

### <a name="section-name-fabrichost"></a>Sectienaam: FabricHost
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| StopTimeout |Tijd in seconden, is standaard 300 |Dynamisch|Geef de interval in seconden. De time-out voor activering van de gehoste service. deactivering en upgrade. |
| StartTimeout |Tijd in seconden, is standaard 300 |Dynamisch|Geef de interval in seconden. Time-out voor fabricactivationmanager starten. |
| ActivationRetryBackoffInterval |Tijd in seconden, de standaardwaarde is 5 |Dynamisch|Geef de interval in seconden. Backoff interval op om de activering is mislukt; op om het systeem opnieuw wordt geprobeerd de activering van de tot de MaxActivationFailureCount continue activering is mislukt. Het interval op elke probeer is een product van continue activering is mislukt en de back-off-activeringsinterval. |
| ActivationMaxRetryInterval |Tijd in seconden, is standaard 300 |Dynamisch|Geef de interval in seconden. Interval voor maximum aantal nieuwe pogingen voor activering. Op elke continue fout wordt het interval berekend als Min (ActivationMaxRetryInterval; Continue foutentelling * ActivationRetryBackoffInterval). |
| ActivationMaxFailureCount |Int, de standaardwaarde is 10 |Dynamisch|Dit is het maximum aantal dat systeem wordt opnieuw geprobeerd mislukte activering van wordt gegeven. |
| EnableServiceFabricAutomaticUpdates |BOOL, de standaardwaarde is ONWAAR |Dynamisch|Dit is het inschakelen van automatische updates via Windows Update-infrastructuur. |
| EnableServiceFabricBaseUpgrade |BOOL, de standaardwaarde is ONWAAR |Dynamisch|Dit is bijgewerkt voor server inschakelen. |
| EnableRestartManagement |BOOL, de standaardwaarde is ONWAAR |Dynamisch|Dit is het inschakelen van de server opnieuw worden opgestart. |


### <a name="section-name-failovermanager"></a>Sectienaam: FailoverManager
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| UserReplicaRestartWaitDuration |Tijd in seconden, de standaardwaarde is 60,0 * 30 |Dynamisch|Geef de interval in seconden. Wanneer een persistente replica afneemt; Windows Fabric wacht tot deze duur voor de replica weer actief voordat u nieuwe vervanging replica's (die een kopie van de status vereist). |
| QuorumLossWaitDuration |Tijd in seconden, de standaardwaarde is MaxValue |Dynamisch|Geef de interval in seconden. Dit is de maximumduur waarvoor we een partitie in een status van quorumverlies toestaan. Als de partitie nog steeds in quorumverlies na deze duur is; de partitie is hersteld van quorumverlies op basis van de omlaag replica's als verloren. Houd er rekening mee dat dit mogelijk gegevensverlies kan tot. |
| UserStandByReplicaKeepDuration |Tijd in seconden, de standaardwaarde is 3600.0 * 24 * 7 |Dynamisch|Geef de interval in seconden. Wanneer een persistente replica keert u terug uit een inactief; mogelijk is deze al vervangen. Deze timer bepaalt hoe lang de FM behoudt de stand-by-replica eerst. |
| UserMaxStandByReplicaCount |Int, de standaardwaarde is 1 |Dynamisch|Het standaard maximum aantal StandBy-replica's die het systeem voor services van de gebruiker blijft. |
| ExpectedClusterSize|Int, de standaardwaarde is 1|Dynamisch|Als het cluster in eerste instantie wordt gestart de FM wacht tot dit veel knooppunten uit om te rapporteren zelf voordat deze begint met het plaatsen van andere services; met inbegrip van de systeemservices zoals naming.  Deze waarde verhoogt, worden de tijd die nodig is een cluster te starten; maar wordt voorkomen dat de vroege knooppunten van steeds overbelast en ook het extra verplaatsingen nodig naarmate meer knooppunten online komen.  Deze waarde moet in het algemeen worden ingesteld op een klein deel van de eerste clustergrootte. |
|ClusterPauseThreshold|Int, de standaardwaarde is 1|Dynamisch|Als het aantal knooppunten in het systeem onder deze waarde wordt de plaatsing gaat; taakverdeling; en failover is gestopt. |
|TargetReplicaSetSize|Int, de standaardwaarde is 7|Niet toegestaan|Dit is het doelaantal replica's FM die Windows Fabric blijft.  Een hogere waarde resulteert in hogere betrouwbaarheid van de gegevens FM; met een afweging kleine prestaties. |
|MinReplicaSetSize|Int, de standaardwaarde is 3|Niet toegestaan|Dit is de grootte van de minimale replica voor de FM.  Als het aantal actieve FM replica's onder deze waarde; de FM weigert wijzigingen aan het cluster tot ten minste het minimum aantal replica's kunnen worden hersteld |
|ReplicaRestartWaitDuration|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Niet toegestaan|Geef de interval in seconden. Dit is de ReplicaRestartWaitDuration voor de FMService |
|StandByReplicaKeepDuration|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(3600.0 * 24 * 7)|Niet toegestaan|Geef de interval in seconden. Dit is de StandByReplicaKeepDuration voor de FMService |
|PlacementConstraints|tekenreeks, default is L""|Niet toegestaan|Alle plaatsingsbeperkingen voor de failover manager replica 's |
|ExpectedNodeFabricUpgradeDuration|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Geef de interval in seconden. Dit is de verwachte duur van een knooppunt moet worden bijgewerkt tijdens de upgrade van Windows Fabric. |
|ExpectedReplicaUpgradeDuration|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Geef de interval in seconden. Dit is de verwachte duur voor alle replica's op een knooppunt wordt bijgewerkt tijdens de upgrade van de toepassing. |
|ExpectedNodeDeactivationDuration|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Geef de interval in seconden. Dit is de verwachte duur van een knooppunt deactivering in voltooid. |
|IsSingletonReplicaMoveAllowedDuringUpgrade|BOOL, standaard is ingesteld op TRUE|Dynamisch|Als is ingesteld op waar is; replica's met een doelgrootte van replicaset 1 mogen verplaatsen tijdens de upgrade. |
|ReconfigurationTimeLimit|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(300)|Dynamisch|Geef de interval in seconden. De tijdslimiet voor herconfiguratie; waarna een statusrapport van de waarschuwing wordt gestart |
|BuildReplicaTimeLimit|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(3600)|Dynamisch|Geef de interval in seconden. De tijdslimiet voor het bouwen van een stateful replica; waarna een statusrapport van de waarschuwing wordt gestart |
|CreateInstanceTimeLimit|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(300)|Dynamisch|Geef de interval in seconden. De tijdslimiet voor het maken van een staatloze exemplaar; waarna een statusrapport van de waarschuwing wordt gestart |
|PlacementTimeLimit|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(600)|Dynamisch|Geef de interval in seconden. De tijdslimiet voor het bereiken van doel replica count; waarna een statusrapport van de waarschuwing wordt gestart |

### <a name="section-name-namingservice"></a>Sectienaam: NamingService
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| TargetReplicaSetSize |Int, de standaardwaarde is 7 |Niet toegestaan|Hiermee stelt u het aantal replica's voor elke partitie van het archief Naming Service. Het aantal replicasets verhoogt, worden het niveau van betrouwbaarheid voor de informatie in het archief met Naming Service; de wijziging dat de gegevens niet verloren als gevolg van knooppuntfouten; verlagen met een waarde van de toegenomen belasting op Windows Fabric en de hoeveelheid tijd nodig die is om uit te voeren van updates aan de naamgeving gegevens.|
|MinReplicaSetSize | Int, de standaardwaarde is 3 |Niet toegestaan| Het minimum aantal Naming Service replica's die zijn vereist om te schrijven in om een update te voltooien. Als er minder replica's dan deze actief is in het systeem het systeem betrouwbaarheid weigert updates naar de Naming Service Store totdat de replica's zijn hersteld. Deze waarde moet nooit meer dan de TargetReplicaSetSize. |
|ReplicaRestartWaitDuration | Tijd in seconden, de standaardwaarde is (60,0 * 30)|Niet toegestaan| Geef de interval in seconden. Wanneer een replica Naming Service uitvalt; Deze timer wordt gestart.  Wanneer het verloopt de FM begint ter vervanging van de replica's die zijn niet beschikbaar (deze niet nog beschouwt ze verloren). |
|QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Niet toegestaan| Geef de interval in seconden. Wanneer een Naming Service opgehaald in quorumverlies; Deze timer wordt gestart.  Als het verloopt worden de FM beschouwd als de omlaag replica's als verloren. en proberen te herstellen quorum. Niet of dit tot verlies van gegevens leiden kan. |
|StandByReplicaKeepDuration | Tijd in seconden, de standaardwaarde is 3600.0 * 2 |Niet toegestaan| Geef de interval in seconden. Wanneer een replica Naming Service keert u terug uit een inactief; mogelijk is deze al vervangen.  Deze timer bepaalt hoe lang de FM behoudt de stand-by-replica eerst. |
|PlacementConstraints | tekenreeks, standaardwaarde is "" |Niet toegestaan| Plaatsing van de beperking voor de Naming Service. |
|ServiceDescriptionCacheLimit | Int, de standaardwaarde is 0 |Statisch| Het maximale aantal vermeldingen bewaard in de cache LRU service beschrijving aan de naamgeving van Store-Service (ingesteld op 0 in voor onbeperkt). |
|RepairInterval | Tijd in seconden, de standaardwaarde is 5 |Statisch| Geef de interval in seconden. Interval waarin de naamgevingsconventie inconsistentie tussen de instantie van eigenaar en naameigenaar herstellen wordt gestart. |
|MaxNamingServiceHealthReports | Int, de standaardwaarde is 10 |Dynamisch|Het maximum aantal trage operations waarin Naming service rapporten niet in orde in één keer. Als u 0; alle trage bewerkingen worden verzonden. |
| MaxMessageSize |Int, de standaardwaarde is 4\*1024\*1024 |Statisch|De maximale berichtgrootte voor clientcommunicatie van knooppunt bij gebruik van naamgeving. DOS-aanval verlichting; standaardwaarde is 4MB. |
| MaxFileOperationTimeout |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Geef de interval in seconden. De maximale time-out toegestaan voor bestandsbewerking store-service. Het opgeven van een hogere time-out aanvragen worden geweigerd. |
| MaxOperationTimeout |Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Geef de interval in seconden. De maximale time-out toegestaan voor de clientbewerkingen. Het opgeven van een hogere time-out aanvragen worden geweigerd. |
| MaxClientConnections |Int, de standaardwaarde is 1000 |Dynamisch|Het maximaal toegestane aantal clientverbindingen per gateway. |
| ServiceNotificationTimeout |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Geef de interval in seconden. De time-out voor het leveren van servicemeldingen naar de client gebruikt. |
| MaxOutstandingNotificationsPerClient |Int, de standaardwaarde is 1000 |Dynamisch|Het maximum aantal openstaande meldingen voordat de clientregistratie van een is geforceerd gesloten door de gateway. |
| MaxIndexedEmptyPartitions |Int, de standaardwaarde is 1000 |Dynamisch|Het maximum aantal lege partities die nog moeten worden geïndexeerd in de cache van de melding voor het herstellen van verbindingen clients synchroniseren. Alle lege partities boven dit nummer wordt verwijderd uit de index in oplopende volgorde van lookup-versie. Opnieuw verbinding te maken van clients kunt synchroniseren en ontvangen van updates voor gemiste lege partitie; maar het synchronisatieprotocol wordt meer kostbaar. |
| GatewayServiceDescriptionCacheLimit |Int, de standaardwaarde is 0 |Statisch|Het maximale aantal vermeldingen bewaard in de cache LRU service beschrijving aan de naamgeving van Gateway (ingesteld op 0 in voor onbeperkt). |
| PartitionCount |Int, de standaardwaarde is 3 |Niet toegestaan|Het aantal partities van de Naming Service opslaan om te worden gemaakt. Elke partitie eigenaar is van een enkele partitie-sleutel die overeenkomt met de index; in dat geval partitiesleutels [0; PartitionCount) bestaat. De schaal die op de Naming Service doen kunt door te verlagen van de gemiddelde hoeveelheid gegevens die zijn opgeslagen door de replica van een back-ups maken van het aantal Naming Service partities toeneemt verhogen ingesteld. met een waarde van de toename in het gebruik van bronnen (sinds het PartitionCount * ReplicaSetSize service replica's moeten worden onderhouden).|

### <a name="section-name-runas"></a>Sectienaam: RunAs
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft de naam van Run as-account. Dit is alleen nodig voor account 'DomainUser' of 'ManagedServiceAccount' type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft het type Run as-account. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' DomainUser/NetworkService/ManagedServiceAccount/LocalSystem' zijn.|
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Hiermee geeft u het wachtwoord voor RunAs-account. Dit is alleen nodig voor accounttype 'DomainUser'. |

### <a name="section-name-runasfabric"></a>Sectienaam: RunAs_Fabric
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft de naam van Run as-account. Dit is alleen nodig voor account 'DomainUser' of 'ManagedServiceAccount' type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft het type Run as-account. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' Lokalegebruiker/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem' zijn. |
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Hiermee geeft u het wachtwoord voor RunAs-account. Dit is alleen nodig voor accounttype 'DomainUser'. |

### <a name="section-name-runashttpgateway"></a>Sectienaam: RunAs_HttpGateway
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft de naam van Run as-account. Dit is alleen nodig voor account 'DomainUser' of 'ManagedServiceAccount' type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft het type Run as-account. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' Lokalegebruiker/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem' zijn. |
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Hiermee geeft u het wachtwoord voor RunAs-account. Dit is alleen nodig voor accounttype 'DomainUser'. |

### <a name="section-name-runasdca"></a>Sectienaam: RunAs_DCA
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft de naam van Run as-account. Dit is alleen nodig voor account 'DomainUser' of 'ManagedServiceAccount' type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft het type Run as-account. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' Lokalegebruiker/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem' zijn. |
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Hiermee geeft u het wachtwoord voor RunAs-account. Dit is alleen nodig voor accounttype 'DomainUser'. |

### <a name="section-name-httpgateway"></a>Sectienaam: HttpGateway
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|IsEnabled|BOOL, de standaardwaarde is ONWAAR |Statisch| Hiermee wordt en uitgeschakeld de HttpGateway. HttpGateway is standaard uitgeschakeld. |
|ActiveListeners |Uint, de standaardwaarde is 50 |Statisch| Het aantal leesbewerkingen te posten naar de wachtrij van HTTP-server. Hiermee bepaalt u het aantal gelijktijdige aanvragen die door de HttpGateway kunnen worden verwerkt. |
|MaxEntityBodySize |Uint, de standaardwaarde is 4194304 |Dynamisch|Geeft de maximale grootte van de instantie die een http-aanvraag kan worden verwacht. Standaardwaarde is 4MB. Httpgateway een aanvraag mislukt als er een instantie van de grootte van > deze waarde. Minimale lezen chunkgrootte is 4096 bytes. Dus dit moet worden > = 4096. |
|HttpGatewayHealthReportSendInterval |Tijd in seconden, de standaardwaarde is 30 |Statisch|Geef de interval in seconden. Het interval waarmee de Http-Gateway samengevoegde health verzendt rapporteert naar de Health Manager. |

### <a name="section-name-ktllogger"></a>Sectienaam: KtlLogger
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|AutomaticMemoryConfiguration |Int, de standaardwaarde is 1 |Dynamisch|Vlag waarmee wordt aangegeven als de instellingen voor geheugen moeten worden automatisch en dynamisch geconfigureerd. Als nul en vervolgens de configuratie-instellingen van het geheugen rechtstreeks worden gebruikt en niet op basis van het systeem voorwaarden wijzigen. Als een vervolgens de geheugeninstellingen worden automatisch geconfigureerd en kunnen worden gewijzigd op basis van de voorwaarden van de system. |
|WriteBufferMemoryPoolMinimumInKB |Int, de standaardwaarde is 8388608 |Dynamisch|Het aantal KB in eerste instantie toewijzen voor de geheugengroep voor schrijven buffer. Gebruik 0 om aan te geven geen limiet standaard moet consistent zijn met SharedLogSizeInMB hieronder. |
|WriteBufferMemoryPoolMaximumInKB | Int, de standaardwaarde is 0 |Dynamisch|Het aantal KB om toe te staan de schrijven geheugen buffergroep maximaal groeien. Gebruik 0 in om aan te geven geen limiet. |
|MaximumDestagingWriteOutstandingInKB | Int, de standaardwaarde is 0 |Dynamisch|Het aantal KB tot de gedeelde aanmelden om door te gaan voor het specifieke logboek toestaan. Gebruik 0 in om aan te geven geen limiet.
|SharedLogPath |tekenreeks, standaardwaarde is "" |Statisch|Naam en pad en de bestandsnaam op locatie gedeelde logboek container te plaatsen. Gebruik ' ' voor het gebruik van standaardpad onder fabric-gegevenshoofdmap. |
|SharedLogId |tekenreeks, standaardwaarde is "" |Statisch|De unieke guid voor gedeelde logboek-container. Gebruik ' ' als standaardpad onder fabric-gegevenshoofdmap gebruikt. |
|SharedLogSizeInMB |Int, de standaardwaarde is 8192 |Statisch|Het aantal MB toewijzen in de gedeelde logboek-container. |

### <a name="section-name-applicationgatewayhttp"></a>De naam van de sectie: ApplicationGateway/Http
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|IsEnabled |BOOL, de standaardwaarde is ONWAAR |Statisch| Hiermee wordt en uitgeschakeld de HttpApplicationGateway. HttpApplicationGateway is standaard uitgeschakeld en deze configuratie moet worden ingesteld in te schakelen. |
|NumberOfParallelOperations | Uint, de standaardwaarde is 5000 |Statisch|Het aantal leesbewerkingen te posten naar de wachtrij van HTTP-server. Hiermee bepaalt u het aantal gelijktijdige aanvragen die door de HttpGateway kunnen worden verwerkt. |
|DefaultHttpRequestTimeout |Tijd in seconden. de standaardwaarde is 120 |Dynamisch|Geef de interval in seconden.  Geeft de time-out van de standaard-aanvraag voor de http-aanvragen worden verwerkt in de HTTP-app-gateway. |
|ResolveServiceBackoffInterval |Tijd in seconden, de standaardwaarde is 5 |Dynamisch|Geef de interval in seconden.  Servicebewerking biedt back-off het standaardinterval voordat u probeert een mislukte oplossen. |
|BodyChunkSize |Uint, de standaardwaarde is 16384 |Dynamisch| Geeft de grootte van voor de chunk in bytes dat wordt gebruikt om te lezen van de hoofdtekst. |
|GatewayAuthCredentialType |tekenreeks, default is 'None' |Statisch| Hiermee geeft u het type van beveiligingsreferenties gebruik op de HTTP-app gateway eindpunt geldige waarden zijn ' geen / X 509. |
|GatewayX509CertificateStoreName |tekenreeks, standaardwaarde is "Mijn" |Dynamisch| Naam van het X.509-certificaatarchief dat certificaat voor HTTP-app gateway bevat. |
|GatewayX509CertificateFindType |tekenreeks, default is 'FindByThumbprint' |Dynamisch| Geeft aan hoe om te zoeken naar certificaat in het archief dat is opgegeven door de waarde GatewayX509CertificateStoreName ondersteund: FindByThumbprint; Findbysubjectname zijn. |
|GatewayX509CertificateFindValue | tekenreeks, standaardwaarde is "" |Dynamisch| Zoeken filterwaarde voor het vinden van het certificaat voor HTTP-app-gateway. Dit certificaat is geconfigureerd op het https-eindpunt en kan ook worden gebruikt om te controleren of de identiteit van de app als nodig is voor de services. FindValue wordt opgezocht eerste; en als dat niet bestaat; FindValueSecondary wordt opgezocht. |
|GatewayX509CertificateFindValueSecondary | tekenreeks, standaardwaarde is "" |Dynamisch|Zoeken filterwaarde voor het vinden van het certificaat voor HTTP-app-gateway. Dit certificaat is geconfigureerd op het https-eindpunt en kan ook worden gebruikt om te controleren of de identiteit van de app als nodig is voor de services. FindValue wordt opgezocht eerste; en als dat niet bestaat; FindValueSecondary wordt opgezocht.|
|HttpRequestConnectTimeout|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(5)|Dynamisch|Geef de interval in seconden.  Geeft de time-out voor verbinding maken voor de http-aanvragen worden verzonden vanaf de HTTP-app-gateway.  |
|RemoveServiceResponseHeaders|tekenreeks, default is L "datum. Server'|Statisch|Puntkomma / door komma's gescheiden lijst van antwoordheaders die wordt verwijderd uit het antwoord van de service; voordat deze worden doorgestuurd naar de client. Als deze optie is ingesteld op een lege tekenreeks; Geeft de headers die zijn geretourneerd door de service-is. Internet Explorer de datum en de Server niet overschrijven |
|ApplicationCertificateValidationPolicy|tekenreeks, default is L 'None'|Statisch| ApplicationCertificateValidationPolicy: Geen: servercertificaat; wordt niet gevalideerd de aanvraag mislukt. ServiceCertificateThumbprints: Raadpleeg config ServiceCertificateThumbprints voor de door komma's gescheiden lijst met vingerafdrukken van externe certificaten die de omgekeerde proxy kan vertrouwen. ServiceCommonNameAndIssuer: Raadpleeg config ServiceCommonNameAndIssuer voor de vingerafdruk van het onderwerp en de verlener van externe certificaten die de omgekeerde proxy kan vertrouwen. |
|ServiceCertificateThumbprints|tekenreeks, default is L""|Dynamisch| |
|CrlCheckingFlag|uint, standaard is 0x40000000 |Dynamisch| De vlaggen voor de toepassing/service validatie van certificaatketen; bijvoorbeeld CRL-controle 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY wijzigt in 0 schakelt CRL controleren of volledige lijst met ondersteunde waarden wordt beschreven door dwFlags van CertGetCertificateChain: http://msdn.microsoft.com/library/windows/desktop/aa376078 (v=vs.85).aspx  |
|IgnoreCrlOfflineError|BOOL, standaard is ingesteld op TRUE|Dynamisch|Hiermee geeft u op of u offline fout van de CRL voor verificatie van de toepassing/service-certificaat te negeren. |
|SecureOnlyMode|BOOL, standaard is ONWAAR|Dynamisch| SecureOnlyMode: true: Reverse Proxy alleen doorstuurt naar de services die beveiligde eindpunten publiceren. ONWAAR: Reverse Proxy kan eindpunten beveiligen/niet-beveiligde aanvragen worden doorgestuurd.  |
|ForwardClientCertificate|BOOL, standaard is ONWAAR|Dynamisch| |

### <a name="section-name-applicationgatewayhttpservicecommonnameandissuer"></a>De naam van de sectie: Http-ApplicationGateway/ServiceCommonNameAndIssuer
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, standaardwaarde is geen|Dynamisch|  |

### <a name="section-name-management"></a>Sectienaam: Management
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| ImageStoreConnectionString |SecureString |Statisch|Verbindingsreeks naar de hoofdmap voor de Installatiekopieopslag. |
| ImageStoreMinimumTransferBPS | Int, standaard is 1024 |Dynamisch|De minimale overdrachtssnelheid tussen het cluster en de Installatiekopieopslag. Deze waarde wordt gebruikt om te bepalen van de time-out bij het openen van externe imagestore opgetreden. Deze waarde alleen wijzigen als de latentie tussen het cluster en de Installatiekopieopslag hoge om meer tijd voor het cluster te downloaden vanaf de externe Installatiekopieopslag te bieden. |
|AzureStorageMaxWorkerThreads | Int, de standaardwaarde is 25 |Dynamisch|Het maximum aantal werkthreads parallel. |
|AzureStorageMaxConnections | Int, de standaardwaarde is 5000 |Dynamisch|Het maximum aantal gelijktijdige verbindingen naar azure storage. |
|AzureStorageOperationTimeout | Tijd in seconden, de standaardwaarde is 6000 |Dynamisch|Geef de interval in seconden. Time-out voor xstore bewerking is voltooid. |
|ImageCachingEnabled | BOOL, de standaardwaarde is true |Statisch|Deze configuratie kan wij in- of uitschakelen in cache opslaan. |
|DisableChecksumValidation | BOOL, de standaardwaarde is ONWAAR |Statisch| Deze configuratie kan wij of controlesomvalidatie tijdens het inrichten van de toepassing uit te schakelen. |
|DisableServerSideCopy | BOOL, de standaardwaarde is ONWAAR |Statisch|Deze configuratie- of uitgeschakeld serverzijde-kopie van het toepassingspakket op imagestore opgetreden tijdens het inrichten van de toepassing. |

### <a name="section-name-healthmanagerclusterhealthpolicy"></a>De naam van de sectie: HealthManager/ClusterHealthPolicy
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| ConsiderWarningAsError |BOOL, de standaardwaarde is ONWAAR |Statisch|Het statusbeleid voor de evaluatie van het cluster: waarschuwingen als fouten worden behandeld. |
| MaxPercentUnhealthyNodes | Int, de standaardwaarde is 0 |Statisch|Het statusbeleid voor de evaluatie van het cluster: maximumpercentage van slecht knooppunten is toegestaan voor het cluster worden in orde. |
| MaxPercentUnhealthyApplications | Int, de standaardwaarde is 0 |Statisch|Het statusbeleid voor de evaluatie van het cluster: maximale percentage van de beschadigde toepassingen toegestaan voor het cluster worden in orde. |

### <a name="section-name-healthmanagerclusterupgradehealthpolicy"></a>De naam van de sectie: HealthManager/ClusterUpgradeHealthPolicy
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|MaxPercentDeltaUnhealthyNodes|Int, de standaardwaarde is 10|Statisch|Upgrade van het statusbeleid voor de evaluatie van het cluster: maximale percentage van de delta slecht knooppunten is toegestaan voor het cluster worden in orde |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes|Int, de standaardwaarde is 15|Statisch|Upgrade van het statusbeleid voor de evaluatie van het cluster: maximale percentage van de delta van beschadigde knooppunten in een upgradedomein toegestaan voor het cluster worden in orde |

### <a name="section-name-faultanalysisservice"></a>Sectienaam: FaultAnalysisService
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| TargetReplicaSetSize |Int, de standaardwaarde is 0 |Niet toegestaan|De TargetReplicaSetSize voor FaultAnalysisService NOT_PLATFORM_UNIX_START. |
| MinReplicaSetSize |Int, de standaardwaarde is 0 |Niet toegestaan|De MinReplicaSetSize voor FaultAnalysisService. |
| ReplicaRestartWaitDuration |Tijd in seconden, de standaardwaarde is 60 minuten|Statisch|Geef de interval in seconden. De ReplicaRestartWaitDuration voor FaultAnalysisService. |
| QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Statisch|Geef de interval in seconden. De QuorumLossWaitDuration voor FaultAnalysisService. |
| StandByReplicaKeepDuration| Tijd in seconden, de standaardwaarde is (60*24*7) minuten |Statisch|Geef de interval in seconden. De StandByReplicaKeepDuration voor FaultAnalysisService. |
| PlacementConstraints | tekenreeks, standaardwaarde is ""|Statisch| De PlacementConstraints voor FaultAnalysisService. |
| StoredActionCleanupIntervalInSeconds | Int, de standaardwaarde is 3600 |Statisch|Dit is hoe vaak de store worden opgeschoond.  Alleen acties in een definitieve status; en die ten minste voltooid CompletedActionKeepDurationInSeconds geleden worden verwijderd. |
| CompletedActionKeepDurationInSeconds | Int, de standaardwaarde is 604800 |Statisch| Dit is ongeveer hoe lang zodat acties die zich in een definitieve status.  Dit is ook afhankelijk StoredActionCleanupIntervalInSeconds; Aangezien het werk op te schonen wordt alleen uitgevoerd op dat interval. 604800 is 7 dagen. |
| StoredChaosEventCleanupIntervalInSeconds | Int, de standaardwaarde is 3600 |Statisch|Dit is hoe vaak de store worden gecontroleerd voor opschoning; Als het aantal gebeurtenissen meer dan 30000 is; het opruimen van de wordt kick in. |
|DataLossCheckWaitDurationInSeconds|Int, de standaardwaarde is 25|Statisch|De totale hoeveelheid tijd; (in seconden); dat het systeem tot verlies van gegevens wacht gebeurt.  Dit wordt intern gebruikt wanneer de StartPartitionDataLossAsync()-api wordt aangeroepen. |
|DataLossCheckPollIntervalInSeconds|int, standaard is 5|Statisch|Dit is de tijd tussen de controles van die het systeem wordt uitgevoerd tijdens het wachten tot verlies van gegevens gebeurt.  Het aantal keren dat het nummer van de gegevens verloren gaan, wordt gecontroleerd per interne herhaling is DataLossCheckWaitDurationInSeconds/deze. |
|ReplicaDropWaitDurationInSeconds|int, standaard is 600|Statisch|Deze parameter wordt gebruikt wanneer het gegevensverlies api wordt aangeroepen.  Dit bepaalt hoe lang het systeem wordt gewacht op een replica ophalen verwijderd na het verwijderen replica intern erop wordt aangeroepen. |

### <a name="section-name-filestoreservice"></a>Sectienaam: FileStoreService
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| NamingOperationTimeout |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Geef de interval in seconden. De time-out voor het uitvoeren van naamgeving bewerking. |
| QueryOperationTimeout | Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Geef de interval in seconden. De time-out voor het uitvoeren van querybewerking. |
| MaxCopyOperationThreads | Uint, de standaardwaarde is 0 |Dynamisch| Het maximale aantal parallelle bestanden dat secundaire kunt van primaire kopiëren. "0" == aantal kernen. |
| MaxFileOperationThreads | Uint, de standaardwaarde is 100 |Statisch| Het maximumaantal toegestane FileOperations (kopiëren of verplaatsen) uitvoeren in de primaire parallelle threads. "0" == aantal kernen. |
| MaxStoreOperations | Uint, de standaardwaarde is 4096 |Statisch|Het maximale aantal parallelle store transactie bewerkingen toegestaan op de primaire. "0" == aantal kernen. |
| MaxRequestProcessingThreads | Uint, de standaardwaarde is 200 |Statisch|Het maximale aantal parallelle threads toegestaan voor het verwerken van aanvragen in de primaire. "0" == aantal kernen. |
| MaxSecondaryFileCopyFailureThreshold | Uint, de standaardwaarde is 25|Dynamisch|Het maximum aantal bestand kopiëren nieuwe pogingen bereikt op de secundaire voordat geeft. |
| AnonymousAccessEnabled | BOOL, de standaardwaarde is true |Statisch|Anonieme toegang tot de shares FileStoreService inschakelen/uitschakelen. |
| PrimaryAccountType | tekenreeks, standaardwaarde is "" |Statisch|De primaire AccountType van de principal voor de Toegangsbeheerlijst van de FileStoreService deelt. |
| PrimaryAccountUserName | tekenreeks, standaardwaarde is "" |Statisch|Het hoofdaccount gebruikersnaam van de principal voor de Toegangsbeheerlijst van de FileStoreService deelt. |
| PrimaryAccountUserPassword | SecureString, de standaardwaarde is leeg |Statisch|Het primaire accountwachtwoord van de principal voor de Toegangsbeheerlijst van de FileStoreService deelt. |
| PrimaryAccountNTLMPasswordSecret | SecureString, de standaardwaarde is leeg |Statisch| Het geheim wachtwoord als seed aan waarmee gegenereerd hetzelfde wachtwoord als NTLM-verificatie. |
| PrimaryAccountNTLMX509StoreLocation | tekenreeks, default is 'LocalMachine'|Statisch| De archieflocatie van de X509 certificaat gebruikt voor het genereren van HMAC op de PrimaryAccountNTLMPasswordSecret bij gebruik van NTLM-verificatie. |
| PrimaryAccountNTLMX509StoreName | tekenreeks is de standaardwaarde is "Mijn"|Statisch| De naam van het archief van de X509 certificaat gebruikt voor het genereren van HMAC op de PrimaryAccountNTLMPasswordSecret bij gebruik van NTLM-verificatie. |
| PrimaryAccountNTLMX509Thumbprint | tekenreeks, standaardwaarde is ""|Statisch|De vingerafdruk van het X509 certificaat gebruikt voor het genereren van HMAC op de PrimaryAccountNTLMPasswordSecret bij gebruik van NTLM-verificatie. |
| SecondaryAccountType | tekenreeks, standaardwaarde is ""|Statisch| De secundaire AccountType van de principal voor de Toegangsbeheerlijst van de FileStoreService deelt. |
| SecondaryAccountUserName | tekenreeks, standaardwaarde is ""| Statisch|De secundaire account gebruikersnaam van de principal voor de Toegangsbeheerlijst van de FileStoreService deelt. |
| SecondaryAccountUserPassword | SecureString, de standaardwaarde is leeg |Statisch|Het wachtwoord van de secundaire account van de principal voor de Toegangsbeheerlijst van de FileStoreService deelt.  |
| SecondaryAccountNTLMPasswordSecret | SecureString, de standaardwaarde is leeg |Statisch| Het geheim wachtwoord als seed aan waarmee gegenereerd hetzelfde wachtwoord als NTLM-verificatie. |
| SecondaryAccountNTLMX509StoreLocation | tekenreeks, default is 'LocalMachine' |Statisch|De archieflocatie van de X509 certificaat gebruikt voor het genereren van HMAC op de SecondaryAccountNTLMPasswordSecret bij gebruik van NTLM-verificatie. |
| SecondaryAccountNTLMX509StoreName | tekenreeks is de standaardwaarde is "Mijn" |Statisch|De naam van het archief van de X509 certificaat gebruikt voor het genereren van HMAC op de SecondaryAccountNTLMPasswordSecret bij gebruik van NTLM-verificatie. |
| SecondaryAccountNTLMX509Thumbprint | tekenreeks, standaardwaarde is ""| Statisch|De vingerafdruk van het X509 certificaat gebruikt voor het genereren van HMAC op de SecondaryAccountNTLMPasswordSecret bij gebruik van NTLM-verificatie. |
|CommonNameNtlmPasswordSecret|SecureString, de standaardwaarde is Common::SecureString(L"")| Statisch|Het geheim wachtwoord als seed aan waarmee gegenereerd hetzelfde wachtwoord bij gebruik van NTLM-verificatie |
|CommonName1Ntlmx509StoreLocation|tekenreeks, default is L 'LocalMachine'|Statisch|De archieflocatie van de X509 certificaat gebruikt voor het genereren van HMAC op de CommonName1NtlmPasswordSecret bij gebruik van NTLM-verificatie |
|CommonName1Ntlmx509StoreName|tekenreeks, default is L "Mijn"| Statisch|De naam van het archief van de X509 certificaat gebruikt voor het genereren van HMAC op de CommonName1NtlmPasswordSecret bij gebruik van NTLM-verificatie |
|CommonName1Ntlmx509CommonName|tekenreeks, default is L""|Statisch| De algemene naam van de X509 certificaat gebruikt voor het genereren van HMAC op de CommonName1NtlmPasswordSecret bij gebruik van NTLM-verificatie |
|CommonName2Ntlmx509StoreLocation|tekenreeks, default is L 'LocalMachine'| Statisch|De archieflocatie van de X509 certificaat gebruikt voor het genereren van HMAC op de CommonName2NtlmPasswordSecret bij gebruik van NTLM-verificatie |
|CommonName2Ntlmx509StoreName|tekenreeks, default is L "Mijn"|Statisch| De naam van het archief van de X509 certificaat gebruikt voor het genereren van HMAC op de CommonName2NtlmPasswordSecret bij gebruik van NTLM-verificatie |
|CommonName2Ntlmx509CommonName|tekenreeks, default is L""|Statisch|De algemene naam van de X509 certificaat gebruikt voor het genereren van HMAC op de CommonName2NtlmPasswordSecret bij gebruik van NTLM-verificatie |

### <a name="section-name-imagestoreservice"></a>Sectienaam: ImageStoreService
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| Ingeschakeld |BOOL, de standaardwaarde is ONWAAR |Statisch|De vlag ingeschakeld voor ImageStoreService. Standaard: false |
| TargetReplicaSetSize | Int, de standaardwaarde is 7 |Niet toegestaan|De TargetReplicaSetSize voor ImageStoreService. |
| MinReplicaSetSize | Int, de standaardwaarde is 3 |Niet toegestaan|De MinReplicaSetSize voor ImageStoreService. |
| ReplicaRestartWaitDuration | Tijd in seconden, de standaardwaarde is 60,0 * 30 |Statisch|Geef de interval in seconden. De ReplicaRestartWaitDuration voor ImageStoreService. |
| QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Statisch| Geef de interval in seconden. De QuorumLossWaitDuration voor ImageStoreService. |
| StandByReplicaKeepDuration | Tijd in seconden, de standaardwaarde is 3600.0 * 2 |Statisch| Geef de interval in seconden. De StandByReplicaKeepDuration voor ImageStoreService. |
| PlacementConstraints | tekenreeks, standaardwaarde is "" |Statisch| De PlacementConstraints voor ImageStoreService. |

### <a name="section-name-imagestoreclient"></a>Sectienaam: ImageStoreClient
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| ClientUploadTimeout |Tijd in seconden, de standaardwaarde is 1800 |Dynamisch|Geef de interval in seconden. Time-outwaarde voor de uploadaanvraag op het hoogste niveau naar de Image Store-Service. |
| ClientCopyTimeout | Tijd in seconden, de standaardwaarde is 1800 |Dynamisch| Geef de interval in seconden. De waarde van de time-out voor Image Store-Service op het hoogste niveau kopie-aanvraag. |
|ClientDownloadTimeout | Tijd in seconden, de standaardwaarde is 1800 |Dynamisch| Geef de interval in seconden. Time-outwaarde voor het van het hoogste niveau downloadaanvraag Image Store-Service. |
|ClientListTimeout | Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Geef de interval in seconden. De waarde van de time-out voor Image Store-Service op het hoogste niveau lijst-aanvraag. |
|ClientDefaultTimeout | Tijd in seconden, is standaard 180 |Dynamisch| Geef de interval in seconden. Time-outwaarde voor alle aanvragen voor niet-uploaden/niet-download (bijvoorbeeld bestaat; verwijderen) op de Image Store-Service. |

### <a name="section-name-tokenvalidationservice"></a>Sectienaam: TokenValidationService
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| Providers |tekenreeks, default is 'DSTS' |Statisch|Door komma's gescheiden lijst met providers van de validatie van tokens om in te schakelen (geldig providers zijn: DSTS; AAD). Op dit moment slechts één provider kan worden ingeschakeld op elk gewenst moment. |

### <a name="section-name-upgradeorchestrationservice"></a>Sectienaam: UpgradeOrchestrationService
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| TargetReplicaSetSize |Int, de standaardwaarde is 0 |Niet toegestaan|De TargetReplicaSetSize voor UpgradeOrchestrationService. |
| MinReplicaSetSize |Int, de standaardwaarde is 0 |Niet toegestaan|De MinReplicaSetSize voor UpgradeOrchestrationService.
| ReplicaRestartWaitDuration | Tijd in seconden, de standaardwaarde is 60 minuten|Statisch| Geef de interval in seconden. De ReplicaRestartWaitDuration voor UpgradeOrchestrationService. |
| QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Statisch| Geef de interval in seconden. De QuorumLossWaitDuration voor UpgradeOrchestrationService. |
| StandByReplicaKeepDuration | Tijd in seconden, de standaardwaarde is 60*24*7 minuten |Statisch| Geef de interval in seconden. De StandByReplicaKeepDuration voor UpgradeOrchestrationService. |
| PlacementConstraints | tekenreeks, standaardwaarde is "" |Statisch| De PlacementConstraints voor UpgradeOrchestrationService. |
| AutoupgradeEnabled | BOOL, de standaardwaarde is true |Statisch| Automatische polling en upgrade-actie op basis van een doel-toestandbestand. |
| UpgradeApprovalRequired | BOOL, de standaardwaarde is ONWAAR | Statisch|Als u op maken voordat u doorgaat goedkeuring door beheerder vereisen code-upgrade. |

### <a name="section-name-upgradeservice"></a>Sectienaam: UpgradeService
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| PlacementConstraints |tekenreeks, standaardwaarde is "" |Niet toegestaan|De PlacementConstraints voor de Upgrade-service. |
| TargetReplicaSetSize | Int, de standaardwaarde is 3 |Niet toegestaan| De TargetReplicaSetSize voor UpgradeService. |
| MinReplicaSetSize | Int, de standaardwaarde is 2 |Niet toegestaan| De MinReplicaSetSize voor UpgradeService. |
| CoordinatorType | tekenreeks, default is 'WUTest'|Niet toegestaan|De CoordinatorType voor UpgradeService. |
| BaseUrl | tekenreeks, standaardwaarde is "" |Statisch|BaseUrl voor UpgradeService. |
| ClusterId | tekenreeks, standaardwaarde is "" |Statisch|ClusterId voor UpgradeService. |
| X509StoreName | tekenreeks, standaardwaarde is "Mijn"|Dynamisch|X509StoreName voor UpgradeService. |
| X509StoreLocation | tekenreeks, standaardwaarde is "" |Dynamisch| X509StoreLocation voor UpgradeService. |
| X509FindType | tekenreeks, standaardwaarde is ""|Dynamisch| X509FindType voor UpgradeService. |
| X509FindValue | tekenreeks, standaardwaarde is "" |Dynamisch| X509FindValue voor UpgradeService. |
| X509SecondaryFindValue | tekenreeks, standaardwaarde is "" |Dynamisch| X509SecondaryFindValue voor UpgradeService. |
| OnlyBaseUpgrade | BOOL, de standaardwaarde is ONWAAR |Dynamisch|OnlyBaseUpgrade voor UpgradeService. |
| TestCabFolder | tekenreeks, standaardwaarde is "" |Statisch| TestCabFolder voor UpgradeService. |

### <a name="section-name-security"></a>Sectienaam: beveiliging
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|ClusterCredentialType|tekenreeks, default is L 'None'|Niet toegestaan|Geeft het type van beveiligingsreferenties moet gebruiken om te beveiligen van het cluster. Geldige waarden zijn ' Geen/X509/Windows' |
|ServerAuthCredentialType|tekenreeks, default is L 'None'|Statisch|Geeft het type van beveiligingsreferenties moet gebruiken om te beveiligen de communicatie tussen FabricClient en het Cluster. Geldige waarden zijn ' Geen/X509/Windows' |
|ClientRoleEnabled|BOOL, standaard is ONWAAR|Statisch|Hiermee wordt aangegeven als client-rol is ingeschakeld; Wanneer ingesteld op true; clients toegewezen rollen op basis van hun identiteit. Voor V2; inschakelen van dit betekent dat de client niet in AdminClientCommonNames/AdminClientIdentities alleen-lezen bewerkingen kan alleen worden uitgevoerd. |
|ClusterCertThumbprints|tekenreeks, default is L""|Dynamisch|Vingerafdrukken van certificaten die kunnen deelnemen aan het cluster. een naam door komma's gescheiden lijst. |
|ServerCertThumbprints|tekenreeks, default is L""|Dynamisch|Vingerafdrukken van servercertificaten door cluster gebruikt voor communicatie met clients; clients gebruiken deze om te verifiëren van het cluster. Het is een naam door komma's gescheiden lijst. |
|ClientCertThumbprints|tekenreeks, default is L""|Dynamisch|Vingerafdrukken van certificaten die door clients wordt gebruikt voor communicatie met het cluster. cluster gebruikt deze binnenkomende verbinding te autoriseren. Het is een naam door komma's gescheiden lijst. |
|AdminClientCertThumbprints|tekenreeks, default is L""|Dynamisch|Vingerafdrukken van certificaten die worden gebruikt door clients in de beheerdersrol. Het is een naam door komma's gescheiden lijst. |
|CrlCheckingFlag|uint, standaard is 0x40000000|Dynamisch|Standaard certificaatketen validatie vlag; kan worden genegeerd door de onderdeel-specifieke vlag; bijvoorbeeld Federation/X509CertChainFlags 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ ALLEEN de instelling op 0, schakelt CRL-controle volledige lijst met ondersteunde waarden wordt beschreven door dwFlags van CertGetCertificateChain: http://msdn.microsoft.com/library/windows/desktop/aa376078 (v=vs.85).aspx |
|IgnoreCrlOfflineError|BOOL, standaard is ONWAAR|Dynamisch|Of de fout te negeren CRL offline wanneer serverzijde binnenkomende clientcertificaten controleert |
|IgnoreSvrCrlOfflineError|BOOL, standaard is ingesteld op TRUE|Dynamisch|Of de fout te negeren CRL offline wanneer de client-side controleert of servercertificaten voor binnenkomende; de standaardwaarde op true. Aanvallen met ingetrokken certificaten vereisen inbreuk op DNS; moeilijker dan met ingetrokken certificaten. |
|CrlDisablePeriod|TimeSpan, de standaardwaarde is Common::TimeSpan::FromMinutes(15)|Dynamisch|Geef de interval in seconden. Hoe lang CRL-controle is uitgeschakeld voor een bepaald certificaat na het vaststellen van offline-fout. Als u offline CRL-fout kan worden genegeerd. |
|CrlOfflineHealthReportTtl|TimeSpan, de standaardwaarde is Common::TimeSpan::FromMinutes(1440)|Dynamisch|Geef de interval in seconden. |
|CertificateHealthReportingInterval|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(3600 * 8)|Statisch|Geef de interval in seconden. Geef het interval voor het certificaat health melden; standaard 8 uur; de instelling op 0 schakelt certificaat health rapportage |
|CertificateExpirySafetyMargin|TimeSpan, de standaardwaarde is Common::TimeSpan::FromMinutes(43200)|Statisch|Geef de interval in seconden. Veiligheidsmarge voor certificaten die verlopen; certificaat health rapportstatus verandert van OK waarschuwing wanneer de vervaldatum is beter dan deze. Standaardwaarde is 30 dagen. |
|ClientClaimAuthEnabled|BOOL, standaard is ONWAAR|Statisch|Hiermee wordt aangegeven of claims gebaseerde verificatie is ingeschakeld op clients; Als dit waar impliciet instelt, ClientRoleEnabled. |
|ClientClaims|tekenreeks, default is L""|Dynamisch|Alle mogelijke claims verwacht van clients voor het verbinden met de gateway. Dit is een lijst 'Of': ClaimsEntry || ClaimsEntry || ClaimsEntry... elke ClaimsEntry is een lijst 'En': ClaimType ClaimValue = & & ClaimType ClaimValue = & & ClaimType ClaimValue =... |
|AdminClientClaims|tekenreeks, default is L""|Dynamisch|Alle mogelijke claims verwacht van beheerclients; dezelfde indeling als ClientClaims; Deze lijst wordt intern toegevoegd aan ClientClaims; hoeft dus niet ook dezelfde items toevoegen aan ClientClaims. |
|ClusterSpn|tekenreeks, default is L""|Niet toegestaan|Service-principal-naam van het cluster. Wanneer de fabric wordt uitgevoerd als een gebruiker één domein (gMSA-domein-gebruikersaccount). Het is de SPN van de lease listeners en luisteraars in fabric.exe: federation-listeners; interne replicatieprocedure listeners; Runtime-service-listener en naming gateway-listener. Dit mag leeg zijn wanneer fabric wordt uitgevoerd als computeraccounts; compute listener SPN van listener transportadres in dat geval verbinding te maken aan clientzijde. |
|ClusterIdentities|tekenreeks, default is L""|Dynamisch|Windows-identiteit van de clusterknooppunten; gebruikt voor autorisatie van cluster lidmaatschap. Het is een door komma's gescheiden lijst. elk item is een naam domeinaccount op of de groepsnaam |
|ClientIdentities|tekenreeks, default is L""|Dynamisch|Windows-identiteit van FabricClient; naamgeving gateway gebruikt dit voor het autoriseren van binnenkomende verbindingen. Het is een door komma's gescheiden lijst. elk item is een domeinaccountnaam of groepsnaam op. Voor uw gemak. het account dat wordt uitgevoerd fabric.exe is automatisch toegestaan. groep ServiceFabricAllowedUsers en ServiceFabricAdministrators zijn. |
|AdminClientIdentities|tekenreeks, default is L""|Dynamisch|Windows-identiteit van de fabric-clients in de beheerdersrol; gebruikt voor het autoriseren van bevoegde fabric-bewerkingen. Het is een door komma's gescheiden lijst. elk item is een domeinaccountnaam of groepsnaam op. Voor uw gemak. het account dat wordt uitgevoerd fabric.exe automatisch toegewezen rol admin; dus is ServiceFabricAdministrators gegroepeerd. |
|AADTenantId|tekenreeks, default is L""|Statisch|Tenant-ID (GUID) |
|AADClusterApplication|tekenreeks, default is L""|Statisch|De naam van de web-API-toepassing of -ID voor het cluster |
|AADClientApplication|tekenreeks, default is L""|Statisch|Native Client toepassingsnaam of ID voor de Fabric-Clients |
|X509Folder|tekenreeks, default is /var/lib/waagent|Statisch|Map waar X509 certificaten en persoonlijke sleutels zich bevinden |
|FabricHostSpn| tekenreeks, default is L"" |Statisch| Service principal name FabricHost; Wanneer fabric wordt uitgevoerd als een gebruiker één domein (gebruikersaccount gMSA-domein) en FabricHost wordt uitgevoerd onder de computeraccount. Het is de SPN van IPC-listener voor FabricHost; die standaard moet leeg worden gelaten omdat FabricHost wordt uitgevoerd onder de computeraccount |
|DisableFirewallRuleForPublicProfile| BOOL, standaard is ingesteld op TRUE | Statisch|Hiermee wordt aangegeven als firewallregel mag niet worden ingeschakeld voor het openbare profiel |
|DisableFirewallRuleForPrivateProfile| BOOL, standaard is ingesteld op TRUE |Statisch| Hiermee wordt aangegeven als firewallregel mag niet worden ingeschakeld voor persoonlijk profiel | 
|DisableFirewallRuleForDomainProfile| BOOL, standaard is ingesteld op TRUE |Statisch| Hiermee wordt aangegeven als firewallregel mag niet worden ingeschakeld voor het domeinprofiel |
|SettingsX509StoreName| tekenreeks, default is L "Mijn"| Dynamisch|X509 archief wordt gebruikt voor de infrastructuur voor de beveiliging van de configuratie-certificaat |

### <a name="section-name-securityadminclientx509names"></a>De naam van de sectie: Beveiliging/AdminClientX509Names
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, standaardwaarde is geen|Dynamisch| |

### <a name="section-name-securityclientx509names"></a>De naam van de sectie: Beveiliging/ClientX509Names
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
PropertyGroup|X509NameMap, standaardwaarde is geen|Dynamisch| |

### <a name="section-name-securityclusterx509names"></a>De naam van de sectie: Beveiliging/ClusterX509Names
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
PropertyGroup|X509NameMap, standaardwaarde is geen|Dynamisch| |

### <a name="section-name-securityserverx509names"></a>De naam van de sectie: Beveiliging/ServerX509Names
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
PropertyGroup|X509NameMap, standaardwaarde is geen|Dynamisch| |

### <a name="section-name-securityclientaccess"></a>De naam van de sectie: Beveiliging/ClientAccess
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| CreateName |tekenreeks, default is 'Admin' |Dynamisch|Beveiligingsconfiguratie voor het maken van naamgevings-URI. |
| DeleteName |tekenreeks, default is 'Admin' |Dynamisch|Beveiligingsconfiguratie voor verwijdering naamgevings-URI. |
| PropertyWriteBatch |tekenreeks, default is 'Admin' |Dynamisch|Beveiligingsconfiguraties voor Naming eigenschap schrijfbewerkingen. |
| CreateService |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het maken van de service. |
| CreateServiceFromTemplate |tekenreeks, default is 'Admin' |Dynamisch|Beveiligingsconfiguratie voor het maken van een service van sjabloon. |
| UpdateService |tekenreeks, default is 'Admin' |Dynamisch|Beveiligingsconfiguratie voor service-updates. |
| DeleteService  |tekenreeks, default is 'Admin' |Dynamisch|Beveiligingsconfiguratie voor verwijdering van de service. |
| ProvisionApplicationType |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het inrichten van de toepassing-type. |
| SubmitMetadata |tekenreeks, default is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor het maken van de toepassing. |
| DeleteApplication |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor verwijdering van de toepassing. |
| UpgradeApplication |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het starten of toepassingsupgrades onderbreken. |
| RollbackApplicationUpgrade |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor toepassingsupgrades worden teruggedraaid. |
| UnprovisionApplicationType |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de toepassing type hierbij. |
| MoveNextUpgradeDomain |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor toepassingsupgrades met een expliciete Upgrade domein wordt hervat. |
| ReportUpgradeHealth |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor toepassingsupgrades met de voortgang van de huidige upgrade hervatten. |
| ReportHealth |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het melden van health. |
| ProvisionFabric |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor MSI en de clusternaambron Manifest inrichten. |
| UpgradeFabric |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het starten van de cluster-upgrades. |
| RollbackFabricUpgrade |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de cluster-upgrades worden teruggedraaid. |
| UnprovisionFabric |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor MSI en de clusternaambron Manifest ongedaan maken inrichting. |
| MoveNextFabricUpgradeDomain |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het hervatten van de cluster-upgrades met een expliciet is het upgraden van domein. |
| ReportFabricUpgradeHealth |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de cluster-upgrades met de voortgang van de huidige upgrade hervatten. |
| StartInfrastructureTask |tekenreeks, default is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor infrastructuurtaken starten. |
| FinishInfrastructureTask |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor bijna afgelopen infrastructuurtaken. |
| ActivateNode |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor activering van een knooppunt. |
| DeactivateNode |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het deactiveren van een knooppunt. |
| DeactivateNodesBatch |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het deactiveren van meerdere knooppunten. |
| RemoveNodeDeactivations |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor omzetten deactivering op meerdere knooppunten. |
| GetNodeDeactivationStatus |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor deactivering van de status controleren. |
| NodeStateRemoved |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het melden van status van het knooppunt verwijderd. |
| RecoverPartition |tekenreeks, default is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor het herstellen van een partitie. |
| RecoverPartitions |tekenreeks, default is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor herstel van partities. |
| RecoverServicePartitions |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor herstel van servicepartities. |
| RecoverSystemPartitions |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor herstel van partities voor systeem-service. |
| ReportFault |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor rapportage van fouten. |
| InvokeInfrastructureCommand |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor infrastructuur-opdrachten voor het beheer van taak. |
| FileContent |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de installatiekopie opslaan bestandsoverdracht client (extern naar cluster). |
| FileDownload |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de image store client bestand downloaden Inleiding (externe aan cluster). |
| InternalList |tekenreeks, default is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor de installatiekopie van bestand lijst clientbewerking (intern) opslaan. |
| Verwijderen |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguraties voor installatiekopie opslaan client delete-bewerking. |
| Uploaden |tekenreeks, default is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor de installatiekopie van het uploaden van clientbewerking opslaan. |
| GetStagingLocation |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de installatiekopie opslaan client tijdelijke locatie ophalen. |
| GetStoreLocation |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de installatiekopie opslaan client store locatie ophalen. |
| NodeControl |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor gestart; wordt gestopt... en opnieuw starten van de knooppunten. |
| CodePackageControl |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor pakketten van de code opnieuw te starten. |
| UnreliableTransportControl |tekenreeks, default is 'Admin' |Dynamisch| Onbetrouwbaar Transport voor het toevoegen en verwijderen van gedrag. |
| MoveReplicaControl |tekenreeks, default is 'Admin' | Dynamisch|Verplaats de replica. |
| PredeployPackageToNode |tekenreeks, default is 'Admin' |Dynamisch| Deze api. |
| StartPartitionDataLoss |tekenreeks, default is 'Admin' |Dynamisch| Induceert verlies van gegevens op een partitie. |
| StartPartitionQuorumLoss |tekenreeks, default is 'Admin' |Dynamisch| Induceert quorumverlies op een partitie. |
| StartPartitionRestart |tekenreeks, default is 'Admin' |Dynamisch| Sommige of alle de replica's van een partitie tegelijk opnieuw is opgestart. |
| CancelTestCommand |tekenreeks, default is 'Admin' |Dynamisch| Hiermee annuleert u een specifieke TestCommand - als tijdens de vlucht. |
| StartChaos |tekenreeks, default is 'Admin' |Dynamisch| Start Chaos - als deze nog niet is gestart. |
| StopChaos |tekenreeks, default is 'Admin' |Dynamisch| Stopt Chaos - als deze is gestart. |
| StartNodeTransition |tekenreeks, default is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de overgang van een knooppunt wordt gestart. |
| StartClusterConfigurationUpgrade |tekenreeks, default is 'Admin' |Dynamisch| Induceert StartClusterConfigurationUpgrade op een partitie. |
| GetUpgradesPendingApproval |tekenreeks, default is 'Admin' |Dynamisch| Induceert GetUpgradesPendingApproval op een partitie. |
| StartApprovedUpgrades |tekenreeks, default is 'Admin' |Dynamisch| Induceert StartApprovedUpgrades op een partitie. |
| Ping |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor client-pings. |
| Query’s uitvoeren |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor query's. |
| NameExists |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Beveiligingsconfiguratie voor naamgevings-URI bestaan controleert. |
| EnumerateSubnames |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor naamgevings-URI-opsomming. |
| EnumerateProperties |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Beveiligingsconfiguratie voor de naamgeving van de eigenschap opsomming. |
| PropertyReadBatch |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor de eigenschap Naming leesbewerkingen. |
| GetServiceDescription |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor de lange polling servicemeldingen en servicebeschrijvingen te lezen. |
| ResolveService |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor de omzetting van de service op basis van een compatibel is. |
| ResolveNameOwner |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Beveiligingsconfiguratie voor het oplossen van de eigenaar van de naamgevings-URI. |
| ResolvePartition |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Beveiligingsconfiguratie voor het oplossen van systeemservices. |
| ServiceNotifications |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor servicemeldingen op basis van gebeurtenissen. |
| PrefixResolveService |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor de omzetting van de service op basis van een compatibele voorvoegsel. |
| GetUpgradeStatus |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor polling upgradestatus van toepassing. |
| GetFabricUpgradeStatus |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor polling upgradestatus van het cluster. |
| InvokeInfrastructureQuery |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Beveiligingsconfiguratie voor het uitvoeren van query's infrastructuurtaken. |
| Lijst |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Beveiligingsconfiguratie voor de installatiekopie opslaan clientbewerking bestand lijst. |
| ResetPartitionLoad |tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor de werklast opnieuw instellen voor een failoverUnit. |
| ToggleVerboseServicePlacementHealthReporting | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor uitgebreide ServicePlacement HealthReporting schakelen. |
| GetPartitionDataLossProgress | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Haalt de voortgang van een api-aanroep van invoke gegevens verloren gaan. |
| GetPartitionQuorumLossProgress | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Haalt de voortgang van een api-aanroep van invoke quorum verloren gaan. |
| GetPartitionRestartProgress | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Haalt de voortgang voor de api-aanroep voor een opnieuw opstarten. |
| GetChaosReport | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Haalt de status dank binnen een bepaald tijdsbereik. |
| GetNodeTransitionProgress | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Beveiligingsconfiguratie voor het ophalen van de voortgang van een knooppunt overgang-opdracht. |
| GetClusterConfigurationUpgradeStatus | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' |Dynamisch| Induceert GetClusterConfigurationUpgradeStatus op een partitie. |
| GetClusterConfiguration | tekenreeks, de standaardwaarde is "Admin\|\|Gebruiker' | Dynamisch|Induceert GetClusterConfiguration op een partitie. |
|CreateComposeDeployment|tekenreeks, default is 'Admin' L| Dynamisch|Maakt een implementatie compose is beschreven door opstellen bestanden |
|DeleteComposeDeployment|tekenreeks, default is 'Admin' L| Dynamisch|Hiermee verwijdert u de implementatie opstellen |
|UpgradeComposeDeployment|tekenreeks, default is 'Admin' L| Dynamisch|Upgrades van de implementatie opstellen |
|ResolveSystemService|tekenreeks, default is L "Admin\|\|Gebruiker'|Dynamisch| Beveiligingsconfiguratie voor het oplossen van systeemservices |
|GetUpgradeOrchestrationServiceState|tekenreeks, default is 'Admin' L| Dynamisch|GetUpgradeOrchestrationServiceState induceert op een andere partitie |
|SetUpgradeOrchestrationServiceState|tekenreeks, default is 'Admin' L| Dynamisch|SetUpgradeOrchestrationServiceState induceert op een andere partitie |

### <a name="section-name-reconfigurationagent"></a>Sectienaam: ReconfigurationAgent
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| ApplicationUpgradeMaxReplicaCloseDuration | Tijd in seconden, de standaardwaarde is 900 |Dynamisch|Geef de interval in seconden. De duur waarvoor het systeem wachten moet voordat het wordt beëindigd servicehosts die replica's hebben die zijn achtergebleven in de wordt afgesloten tijdens het upgraden van de toepassing.|
| ServiceApiHealthDuration | Tijd in seconden, de standaardwaarde is 30 minuten |Dynamisch| Geef de interval in seconden. ServiceApiHealthDuration definieert hoe lang Wij wachten op een API service uit te voeren voordat we het slecht rapporteren. |
| ServiceReconfigurationApiHealthDuration | Tijd in seconden, de standaardwaarde is 30 |Dynamisch| Geef de interval in seconden. ServiceReconfigurationApiHealthDuration definieert hoe lang Wij wachten op een API service uit te voeren voordat we slecht rapporteren. Dit geldt voor API-aanroepen die van invloed zijn op beschikbaarheid.|
| PeriodicApiSlowTraceInterval | Tijd in seconden, de standaardwaarde is 5 minuten |Dynamisch| Geef de interval in seconden. PeriodicApiSlowTraceInterval definieert het interval waarover wordt traag API-aanroepen worden ververst door de API-monitor. |
| NodeDeactivationMaxReplicaCloseDuration | Tijd in seconden, de standaardwaarde is 900 |Dynamisch|Geef de interval in seconden. De duur waarvoor het systeem wachten moet voordat het wordt beëindigd servicehosts die replica's hebben die zijn achtergebleven in de wordt afgesloten tijdens de deactivering van het knooppunt. |
| FabricUpgradeMaxReplicaCloseDuration | Tijd in seconden, de standaardwaarde is 900 |Dynamisch| Geef de interval in seconden. De duur waarvoor het systeem wachten moet voordat het wordt beëindigd servicehosts die replica's hebben die zijn achtergebleven in de wordt afgesloten tijdens de fabric-upgrade. |
|GracefulReplicaShutdownMaxDuration|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch|Geef de interval in seconden. De duur waarvoor het systeem wachten moet voordat het wordt beëindigd servicehosts die replica's hebben die zijn vastgelopen in te sluiten. Als deze waarde is ingesteld op 0, replica's niet gevraagd om te sluiten.|
|ReplicaChangeRoleFailureRestartThreshold|Int, de standaardwaarde is 10|Dynamisch| Geheel getal. Geef het aantal API-fouten tijdens de promotie van primaire waarna automatisch risicobeperking actie (replica opnieuw is opgestart) wordt toegepast. |
|ReplicaChangeRoleFailureWarningReportThreshold|int, de standaardwaarde is 2147483647|Dynamisch| Geheel getal. Geef het aantal API-fouten tijdens de promotie van primaire waarna statusrapport waarschuwing verschijnt.|

### <a name="section-name-placementandloadbalancing"></a>Sectienaam: PlacementAndLoadBalancing
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| TraceCRMReasons |BOOL, de standaardwaarde is true |Dynamisch|Geeft aan of redenen voor CRM verplaatsingen verleend aan het kanaal operationele gebeurtenissen traceren. |
| ValidatePlacementConstraint | BOOL, de standaardwaarde is true |Dynamisch| Hiermee geeft u op of de PlacementConstraint-expressie voor een service wordt gevalideerd wanneer een service ServiceDescription wordt bijgewerkt. |
| PlacementConstraintValidationCacheSize | Int, de standaardwaarde is 10000 |Dynamisch| Beperkt de grootte van de tabel voor de snelle validatie en in cache plaatsen van plaatsing beperking expressies. |
|VerboseHealthReportLimit | Int, de standaardwaarde is 20 | Dynamisch|Definieert het aantal keren dat een replica heeft niet-geplaatste gaan voordat een waarschuwing wordt gemeld voor (als uitgebreide health reporting is ingeschakeld). |
|ConstraintViolationHealthReportLimit | Int, de standaardwaarde is 50 |Dynamisch| Definieert het aantal keren beperking schenden replica blijft niet-opgeloste moet voordat diagnostische gegevens worden uitgevoerd en systeemstatusrapporten worden verzonden. |
|DetailedConstraintViolationHealthReportLimit | Int, de standaardwaarde is 200 |Dynamisch| Definieert het aantal keren beperking schenden replica heeft blijft niet-opgeloste moet voor de diagnostische gegevens worden uitgevoerd en gedetailleerde health rapporten worden verzonden. |
|DetailedVerboseHealthReportLimit | Int, de standaardwaarde is 200 | Dynamisch|Definieert het aantal keren dat een niet-geplaatste replica zijn niet permanent-geplaatste voordat gedetailleerde statusrapporten worden verzonden. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, de standaardwaarde is 20 | Dynamisch|Definieert het aantal opeenvolgende keren dat ResourceBalancer uitgegeven verplaatsingen zijn verwijderd voordat diagnostische gegevens worden uitgevoerd en waarschuwingen worden gegenereerd. Negatief: Er zijn geen waarschuwingen verzonden onder deze voorwaarde. |
|DetailedNodeListLimit | Int, de standaardwaarde is 15 |Dynamisch| Definieert het aantal knooppunten per beperking om op te nemen voordat in de rapporten voor niet-geplaatste Replica moet worden afgekapt. |
|DetailedPartitionListLimit | Int, de standaardwaarde is 15 |Dynamisch| Definieert het aantal partities per diagnostische vermelding voor een beperking om op te nemen voordat moet worden afgekapt in diagnostische gegevens. |
|DetailedDiagnosticsInfoListLimit | Int, de standaardwaarde is 15 |Dynamisch| Definieert het aantal diagnose-items (met gedetailleerde informatie) per beperking om op te nemen voordat moet worden afgekapt in diagnostische gegevens.|
|PLBRefreshGap | Tijd in seconden, de standaardwaarde is 1 |Dynamisch| Geef de interval in seconden. Hiermee definieert u de minimale hoeveelheid tijd die moet worden gewacht voordat de PLB wordt vernieuwd status opnieuw. |
|MinPlacementInterval | Tijd in seconden, de standaardwaarde is 1 |Dynamisch| Geef de interval in seconden. Hiermee definieert u de minimale hoeveelheid tijd die voordat u twee opeenvolgende plaatsing patronen verstrijken moet. |
|MinConstraintCheckInterval | Tijd in seconden, de standaardwaarde is 1 |Dynamisch| Geef de interval in seconden. Hiermee definieert u de minimale hoeveelheid tijd die moet worden gewacht voordat er twee opeenvolgende beperking Rondt controleren. |
|MinLoadBalancingInterval | Tijd in seconden, de standaardwaarde is 5 |Dynamisch| Geef de interval in seconden. Hiermee definieert u de minimale hoeveelheid tijd die voordat u twee opeenvolgende taakverdeling patronen verstrijken moet. |
|BalancingDelayAfterNodeDown | Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Geef de interval in seconden. Start niet balancing activiteiten binnen deze periode na een knooppunt omlaag gebeurtenis. |
|BalancingDelayAfterNewNode | Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Geef de interval in seconden. Start niet balancing activiteiten binnen deze periode na het toevoegen van een nieuw knooppunt. |
|ConstraintFixPartialDelayAfterNodeDown | Tijd in seconden, de standaardwaarde is 120 |Dynamisch| Geef de interval in seconden. Geen herstel FaultDomain en UpgradeDomain schendingen van plaatsingsbeperkingen binnen deze periode na een knooppunt omlaag gebeurtenis doen. |
|ConstraintFixPartialDelayAfterNewNode | Tijd in seconden, de standaardwaarde is 120 |Dynamisch| Geef de interval in seconden. DDo FaultDomain niet oplossen en schendingen van plaatsingsbeperkingen UpgradeDomain binnen deze periode na het toevoegen van een nieuw knooppunt. |
|GlobalMovementThrottleThreshold | Uint, de standaardwaarde is 1000 |Dynamisch| Maximum aantal verplaatsingen van het type in de fase Netwerktaakverdeling in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval toegestaan. |
|GlobalMovementThrottleThresholdForPlacement | Uint, de standaardwaarde is 0 |Dynamisch| Maximum aantal verplaatsingen van het type toegestaan in plaatsing fase in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval.0 geeft geen limiet.|
|GlobalMovementThrottleThresholdForBalancing | Uint, de standaardwaarde is 0 | Dynamisch|Maximum aantal verplaatsingen van het type in de fase Netwerktaakverdeling in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval toegestaan. 0 geeft geen limiet. |
|GlobalMovementThrottleCountingInterval | Tijd in seconden, de standaardwaarde is 600 |Statisch| Geef de interval in seconden. De lengte van het afgelopen interval waarvoor om bij te houden per domein replica verplaatsingen (gebruikt samen met GlobalMovementThrottleThreshold) geven. Kan voor het negeren van algemene bandbreedtebeperking helemaal niet worden ingesteld op 0. |
|MovementPerPartitionThrottleThreshold | Uint, de standaardwaarde is 50 |Dynamisch| Er zijn geen verkeer balancing-gerelateerde wordt uitgevoerd voor een partitie als het aantal gerelateerde verplaatsingen voor replica's van de betreffende partitie balancing heeft bereikt of overschreden MovementPerFailoverUnitThrottleThreshold in het afgelopen interval aangegeven door MovementPerPartitionThrottleCountingInterval. |
|MovementPerPartitionThrottleCountingInterval | Tijd in seconden, de standaardwaarde is 600 |Statisch| Geef de interval in seconden. De lengte van het afgelopen interval waarvoor u wilt bijhouden replica verplaatsingen voor elke partitie (gebruikt samen met MovementPerPartitionThrottleThreshold) geven. |
|PlacementSearchTimeout | Tijd in seconden, de standaardwaarde is 0,5 |Dynamisch| Geef de interval in seconden. Bij het plaatsen van services; zoeken naar maximaal deze lang voordat een resultaat te retourneren. |
|UseMoveCostReports | BOOL, de standaardwaarde is ONWAAR | Dynamisch|Hiermee geeft u de Load Balancer voor het negeren van het kostenelement van de functie scoreprofiel. resulterende potentieel grote aantal om betere taakverdeling plaatsing verplaatsen. |
|PreventTransientOvercommit | BOOL, de standaardwaarde is ONWAAR | Dynamisch|Hiermee wordt bepaald moet PLB onmiddellijk worden geteld voor resources die worden vrijgemaakt door de geïnitieerd verplaatst. Standaard; PLB verplaatsen uit kan initiëren en verplaatsing in op hetzelfde knooppunt wat tot tijdelijke leiden kan overcommit. Als deze parameter in op true, wordt deze soorten voorkomen van overcommits op aanvraag defragmentatie (aka placementWithMove) wordt uitgeschakeld. |
|InBuildThrottlingEnabled | BOOL, de standaardwaarde is ONWAAR |Dynamisch| Bepalen of de in-build-beperking is ingeschakeld. |
|InBuildThrottlingAssociatedMetric | tekenreeks, standaardwaarde is "" |Statisch| De gekoppelde metrische naam voor deze beperking. |
|InBuildThrottlingGlobalMaxValue | Int, de standaardwaarde is 0 |Dynamisch|Het maximale aantal van-build-replica's globaal toegestaan. |
|SwapPrimaryThrottlingEnabled | BOOL, de standaardwaarde is ONWAAR|Dynamisch| Bepalen of het swap-primaire beperking is ingeschakeld. |
|SwapPrimaryThrottlingAssociatedMetric | tekenreeks, standaardwaarde is ""|Statisch| De gekoppelde metrische naam voor deze beperking. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, de standaardwaarde is 0 |Dynamisch| Het maximale aantal van de wisseling primaire replica's globaal toegestaan. |
|PlacementConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Bepaalt de prioriteit van plaatsing beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|PreferredLocationConstraintPriority | Int, de standaardwaarde is 2| Dynamisch|Bepaalt de prioriteit van voorkeur locatiebeperking: 0: vaste; 1: zachte; 2: optimalisatie; negatieve: negeren |
|CapacityConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Bepaalt de prioriteit van capaciteit beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|AffinityConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Bepaalt de prioriteit van affiniteit beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|FaultDomainConstraintPriority | Int, de standaardwaarde is 0 |Dynamisch| Bepaalt de prioriteit van fouttolerantie domeinbeperking: 0: vaste; 1: zachte; negatieve: negeren. |
|UpgradeDomainConstraintPriority | Int, de standaardwaarde is 1| Dynamisch|Bepaalt de prioriteit van het upgradedomein beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|ScaleoutCountConstraintPriority | Int, de standaardwaarde is 0 |Dynamisch| Bepaalt de prioriteit van scaleout aantal beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|ApplicationCapacityConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Bepaalt de prioriteit van capaciteit beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|MoveParentToFixAffinityViolation | BOOL, de standaardwaarde is ONWAAR |Dynamisch| Instelling die bepaalt als bovenliggende replica's kunnen worden verplaatst om op te lossen affiniteit beperkingen.|
|MoveExistingReplicaForPlacement | BOOL, de standaardwaarde is true |Dynamisch|Instelling die bepaalt wanneer bestaande replica verplaatsen tijdens de plaatsing. |
|UseSeparateSecondaryLoad | BOOL, de standaardwaarde is true | Dynamisch|Instelling die bepaalt of andere secundaire load gebruiken. |
|PlaceChildWithoutParent | BOOL, de standaardwaarde is true | Dynamisch|Instelling die bepaalt of de onderliggende service replica kan worden geplaatst als er geen bovenliggende replica actief is. |
|PartiallyPlaceServices | BOOL, de standaardwaarde is true |Dynamisch| Bepaalt of alle service-replica's in het cluster wordt geplaatst als 'Alles of niets' gegeven beperkt geschikte knooppunten voor hen.|
|InterruptBalancingForAllFailoverUnitUpdates | BOOL, de standaardwaarde is ONWAAR | Dynamisch|Bepaalt of elk type failover-eenheid update moet snel interrupt of traag Netwerktaakverdeling uitvoeren. Met de opgegeven 'false' Netwerktaakverdeling uitvoeren wordt onderbroken als FailoverUnit: is gemaakt/verwijderd; ontbrekende replica's; locatie van de primaire replica of gewijzigde aantal replica's worden gewijzigd. Netwerktaakverdeling uitvoeren wordt niet onderbroken in andere gevallen - als FailoverUnit: heeft extra replica's; een replica-vlag; gewijzigd alleen de versie van de partitie of ander gewijzigd. |
|GlobalMovementThrottleThresholdPercentage|Double, de standaardwaarde is 0|Dynamisch|Maximum aantal totale verplaatsingen toegestaan in fasen voor taakverdeling en plaatsing (uitgedrukt als percentage van totaal aantal replica's in het cluster) in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval. 0 geeft geen limiet. Als dit en GlobalMovementThrottleThreshold zijn opgegeven. vervolgens wordt meer conservatief limiet gebruikt.|
|GlobalMovementThrottleThresholdPercentageForBalancing|Double, de standaardwaarde is 0|Dynamisch|Maximum aantal verplaatsingen van het type in Balancing fase (uitgedrukt als percentage van totaal aantal replica's in de functieaanroep PLB) in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval toegestaan. 0 geeft geen limiet. Als dit en GlobalMovementThrottleThresholdForBalancing zijn opgegeven. vervolgens wordt meer conservatief limiet gebruikt.|
|AutoDetectAvailableResources|BOOL, standaard is ingesteld op TRUE|Statisch|Deze configuratie automatische detectie van beschikbare resources op knooppunt (CPU en geheugen) wordt geactiveerd wanneer deze configuratie is ingesteld op true - wordt gelezen echte capaciteiten en corrigeer deze als gebruiker onjuiste knooppunt capaciteiten opgegeven of niet helemaal definieert als deze configuratie is ingesteld op onwaar - wordt  een waarschuwing dat die gebruiker opgegeven onjuiste knooppunt capaciteiten; traceren maar we niet worden hersteld door deze; Dit betekent dat de gebruiker wil dat de capaciteit die is opgegeven als > dan het knooppunt echt heeft of als capaciteitswaarden niet-gedefinieerde zijn; wordt ervan uitgegaan dat onbeperkte capaciteit |

### <a name="section-name-hosting"></a>Sectienaam: die als host fungeert
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| ServiceTypeRegistrationTimeout |Tijd in seconden, is standaard 300 |Dynamisch|Maximale tijd waarbinnen ServiceType worden geregistreerd met fabric |
| ServiceTypeDisableFailureThreshold |Geheel getal van de standaardwaarde is 1 |Dynamisch|Dit is de drempel voor het aantal mislukte waarna FailoverManager (FM) is gemeld dat het type van de service op het knooppunt uitschakelen en probeer een ander knooppunt voor plaatsing. |
| ActivationRetryBackoffInterval |Tijd in seconden, de standaardwaarde is 5 |Dynamisch|Backoff interval op om de activering is mislukt; Op elke continue activering is mislukt, probeert de activering van de tot de MaxActivationFailureCount opnieuw door het systeem. Het interval op elke probeer is een product van continue activering is mislukt en de back-off-activeringsinterval. |
| ActivationMaxRetryInterval |Tijd in seconden, is standaard 300 |Dynamisch|Op elke continue activering is mislukt, probeert de activering van de maximaal ActivationMaxFailureCount opnieuw door het systeem. ActivationMaxRetryInterval bevat de tijdsinterval wacht voordat u opnieuw proberen na elke activering is mislukt |
| ActivationMaxFailureCount |Geheel getal van de standaardwaarde is 10 |Dynamisch|Hoe vaak system pogingen activering voordat geeft mislukt |
|ActivationTimeout| TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(180)|Dynamisch| Geef de interval in seconden. De time-out voor toepassingsactivering. deactivering en upgrade. |
|ApplicationHostCloseTimeout| TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch| Geef de interval in seconden. Als Fabric afsluiten is gedetecteerd in een self geactiveerde processen; FabricRuntime alle de replica's in het hostproces (applicationhost) van de gebruiker gesloten. Dit is de time-out voor de bewerking sluiten. |
|ApplicationUpgradeTimeout| TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(360)|Dynamisch| Geef de interval in seconden. De time-out voor de upgrade van de toepassing. Als de time-out minder is dan de deployer 'ActivationTimeout' mislukt. |
|CreateFabricRuntimeTimeout|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch| Geef de interval in seconden. De time-outwaarde voor de synchronisatie FabricCreateRuntime aanroepen |
|DeploymentMaxFailureCount|Int, de standaardwaarde is 20| Dynamisch|Implementatie van de toepassing wordt opnieuw geprobeerd voor DeploymentMaxFailureCount tijden voordat het mislukken van de implementatie van toepassing op het knooppunt.| 
|DeploymentMaxRetryInterval| TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(3600)|Dynamisch| Geef de interval in seconden. Interval voor maximum aantal nieuwe pogingen voor de implementatie. Op elke continue fout wordt het interval berekend als Min (DeploymentMaxRetryInterval; Continue foutentelling * DeploymentRetryBackoffInterval) |
|DeploymentRetryBackoffInterval| TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(10)|Dynamisch|Geef de interval in seconden. Back-off de interval voor de implementatie is mislukt. Het systeem wordt opnieuw geprobeerd de implementatie voor maximaal de MaxDeploymentFailureCount op elke doorlopende implementatie is mislukt. Het interval is een product van doorlopende implementatie is mislukt en de implementatie backoff-interval. |
|EnableActivateNoWindow| BOOL, standaard is ONWAAR|Dynamisch| Het geactiveerde proces is op de achtergrond zonder een console gemaakt. |
|EnableProcessDebugging|BOOL, standaard is ONWAAR|Dynamisch| Hiermee toepassingshosts onder foutopsporing starten |
|EndpointProviderEnabled| BOOL, standaard is ONWAAR|Statisch| Maakt beheer van bronnen door Fabric eindpunt. Specificatie van het begin- en toepassingspoortbereik in FabricNode vereist. |
|FabricContainerAppsEnabled| BOOL, standaard is ONWAAR|Statisch| |
|FirewallPolicyEnabled|BOOL, standaard is ONWAAR|Statisch| Hiermee firewallpoorten voor eindpunt resources met expliciete poorten die zijn opgegeven in ServiceManifest openen |
|GetCodePackageActivationContextTimeout|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch|Geef de interval in seconden. De time-outwaarde voor de CodePackageActivationContext aanroepen. Dit geldt niet voor ad-hoc-services. |
|IPProviderEnabled|BOOL, standaard is ONWAAR|Statisch|Maakt beheer van IP-adressen. |
|NTLMAuthenticationEnabled|BOOL, standaard is ONWAAR|Statisch| Hiermee wordt ondersteuning geboden voor het gebruik van NTLM door de code-pakketten die uitgevoerd als andere gebruikers worden zodat de processen over machines veilig kunnen communiceren. |
|NTLMAuthenticationPasswordSecret|SecureString, de standaardwaarde is Common::SecureString(L"")|Statisch|Is dat een versleutelde heeft die wordt gebruikt voor het genereren van het wachtwoord voor NTLM-gebruikers. Moet worden ingesteld als NTLMAuthenticationEnabled ingesteld op true is. Gevalideerd door de deployer. |
|NTLMSecurityUsersByX509CommonNamesRefreshInterval|TimeSpan, de standaardwaarde is Common::TimeSpan::FromMinutes(3)|Dynamisch|Geef de interval in seconden. Omgeving-specifieke instellingen voor de periodieke interval op welke Hosting scant op nieuwe certificaten worden gebruikt voor FileStoreService NTLM-configuratie. |
|NTLMSecurityUsersByX509CommonNamesRefreshTimeout|TimeSpan, de standaardwaarde is Common::TimeSpan::FromMinutes(4)|Dynamisch| Geef de interval in seconden. De time-out voor het configureren van NTLM-gebruikers met behulp van de algemene naam van het certificaat. De NTLM-gebruikers zijn nodig voor FileStoreService shares. |
|RegisterCodePackageHostTimeout|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch| Geef de interval in seconden. De time-outwaarde voor de aanroep van de synchronisatie FabricRegisterCodePackageHost. Dit geldt voor alleen meerdere code pakket toepassingshosts zoals FWP |
|RequestTimeout|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(30)|Dynamisch| Geef de interval in seconden. Hiermee wordt de time-out voor de communicatie tussen de toepassingshost en Fabric-proces voor verschillende hosting gerelateerde bewerkingen zoals factory inschrijving; van de gebruiker registratie van de runtime. |
|RunAsPolicyEnabled| BOOL, standaard is ONWAAR|Statisch| Hiermee kunt u code pakketten uitgevoerd als lokale gebruiker dan de gebruiker onder welke infrastructuur proces wordt uitgevoerd. Om in te schakelen van dit beleid moet als SYSTEEMACCOUNT of als de gebruiker die SeAssignPrimaryTokenPrivilege heeft Fabric worden uitgevoerd. |
|ServiceFactoryRegistrationTimeout| TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch|Geef de interval in seconden. De time-outwaarde voor de synchronisatie registreren (Stateless/Stateful) ServiceFactory aanroep |
|ServiceTypeDisableGraceInterval|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(30)|Dynamisch|Geef de interval in seconden. Tijdsinterval waarna type van de service kan worden uitgeschakeld |

### <a name="section-name-federation"></a>Sectienaam: Federation
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| LeaseDuration |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|De duur die een lease tussen een knooppunt en de bijbehorende neighbors duurt. |
| LeaseDurationAcrossFaultDomain |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|De duur die een lease tussen een knooppunt en de bijbehorende neighbors tussen domeinen met fouten duurt. |

### <a name="section-name-clustermanager"></a>Sectienaam: ClusterManager
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade** | **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
| UpgradeStatusPollInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van polling voor de upgradestatus van toepassing. Deze waarde bepaalt de frequentie van de update voor elke aanroep GetApplicationUpgradeProgress |
| UpgradeHealthCheckInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van de status controleert tijdens een upgrades van de bewaakte toepassingen |
| FabricUpgradeStatusPollInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van polling voor Fabric-upgrade-status. Deze waarde bepaalt de frequentie van de update voor elke aanroep GetFabricUpgradeProgress |
| FabricUpgradeHealthCheckInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van de status controleren tijdens een bewaakte Fabric-upgrade |
|InfrastructureTaskProcessingInterval | Tijd in seconden, de standaardwaarde is 10 |Dynamisch|Geef de interval in seconden. Het verwerkingsinterval die worden gebruikt door de infrastructuur-taak statusmachine verwerken. |
|TargetReplicaSetSize |Int, de standaardwaarde is 7 |Niet toegestaan|De TargetReplicaSetSize voor ClusterManager. |
|MinReplicaSetSize |Int, de standaardwaarde is 3 |Niet toegestaan|De MinReplicaSetSize voor ClusterManager. |
|ReplicaRestartWaitDuration |Tijd in seconden, de standaardwaarde is (60,0 * 30)|Niet toegestaan|Geef de interval in seconden. De ReplicaRestartWaitDuration voor ClusterManager. |
|QuorumLossWaitDuration |Tijd in seconden, de standaardwaarde is MaxValue |Niet toegestaan| Geef de interval in seconden. De QuorumLossWaitDuration voor ClusterManager. |
|StandByReplicaKeepDuration | Tijd in seconden, de standaardwaarde is (3600.0 * 2)|Niet toegestaan|Geef de interval in seconden. De StandByReplicaKeepDuration voor ClusterManager. |
|PlacementConstraints | tekenreeks, standaardwaarde is "" |Niet toegestaan|De PlacementConstraints voor ClusterManager. |
|SkipRollbackUpdateDefaultService | BOOL, de standaardwaarde is ONWAAR |Dynamisch|De CM wordt omzetten bijgewerkte standaardservices overslaan tijdens de upgrade terugdraaien van de toepassing. |
|EnableDefaultServicesUpgrade | BOOL, de standaardwaarde is ONWAAR |Dynamisch|Upgraden standaardservices inschakelen tijdens de upgrade van de toepassing. Beschrijvingen van de standaardservice zou na de upgrade worden overschreven. |
|InfrastructureTaskHealthCheckWaitDuration |Tijd in seconden, de standaardwaarde is 0|Dynamisch| Geef de interval in seconden. De hoeveelheid tijd moet worden gewacht voordat u begint statuscontroles na het verwerken van een taak infrastructuur na. |
|InfrastructureTaskHealthCheckStableDuration | Tijd in seconden, de standaardwaarde is 0|Dynamisch| Geef de interval in seconden. De hoeveelheid tijd om te observeren opeenvolgende doorgegeven statuscontroles voorafgaand aan de na verwerking van de taak van een infrastructuur met succes wordt voltooid. Een mislukte statuscontrole geobserveerd, wordt deze timer opnieuw ingesteld. |
|InfrastructureTaskHealthCheckRetryTimeout | Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Geef de interval in seconden. De hoeveelheid tijd te besteden opnieuw uit te voeren is mislukt statuscontroles tijdens het verwerken van een taak infrastructuur na. Doorgegeven statuscontrole geobserveerd, wordt deze timer opnieuw ingesteld. |
|ImageBuilderTimeoutBuffer |Tijd in seconden, de standaardwaarde is 3 |Dynamisch|Geef de interval in seconden. De hoeveelheid tijd om toe te staan voor Image Builder specifieke time-outfouten om terug te keren naar de client. Als deze buffer te klein is. vervolgens wordt de client een time-out optreedt voordat de server en een algemene time-outfout opgehaald. |
|MinOperationTimeout | Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Geef de interval in seconden. De minimale algemene time-out voor intern verwerkingen op ClusterManager. |
|MaxOperationTimeout |Tijd in seconden, de standaardwaarde is MaxValue |Dynamisch| Geef de interval in seconden. De maximale algemene time-out voor intern verwerkingen op ClusterManager. |
|MaxTimeoutRetryBuffer | Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Geef de interval in seconden. Maximale time-out voor de bewerking als intern opnieuw wordt uitgevoerd als gevolg van time-outs is <Original Time out>  +  <MaxTimeoutRetryBuffer>. Aanvullende time-out wordt in stappen van MinOperationTimeout toegevoegd. |
|MaxCommunicationTimeout |Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Geef de interval in seconden. De maximale time-out voor interne communicatie tussen ClusterManager en andere services (dat wil zeggen; Naamgevingsservice; "Failover Manager en enz.). Deze time-out moet kleiner zijn dan globale MaxOperationTimeout (zoals het is mogelijk meerdere communicatie tussen onderdelen van het systeem voor elke clientbewerking). |
|MaxDataMigrationTimeout |Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Geef de interval in seconden. De maximale time-out voor data recovery migratiebewerkingen nadat een Fabric-upgrade heeft plaatsgevonden. |
|MaxOperationRetryDelay |Tijd in seconden, de standaardwaarde is 5|Dynamisch| Geef de interval in seconden. De maximale wachttijd voor interne pogingen wanneer er fouten zijn opgetreden. |
|ReplicaSetCheckTimeoutRollbackOverride |Tijd in seconden, is standaard 1200 |Dynamisch| Geef de interval in seconden. Als ReplicaSetCheckTimeout is ingesteld op de maximumwaarde van DWORD; Deze wordt vervolgens overschreven met de waarde van deze configuratie voor de doeleinden van terugdraaien. De waarde gebruikt voor doorvoeren wordt nooit overschreven. |
|ImageBuilderJobQueueThrottle |Int, de standaardwaarde is 10 |Dynamisch|Aantal versnelling voor Image Builder proxy taakwachtrij van toepassingsaanvragen thread. |
|MaxExponentialOperationRetryDelay|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(30)|Dynamisch|Geef de interval in seconden. De maximale exponentiële vertraging voor interne pogingen wanneer er fouten zijn opgetreden, herhaaldelijk |

### <a name="section-name-defragmentationemptynodedistributionpolicy"></a>Sectienaam: DefragmentationEmptyNodeDistributionPolicy
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, standaardwaarde is geen|Dynamisch|Hiermee geeft u dat de defragmentatie policy volgt bij het leegmaken van knooppunten. Voor een bepaalde waarde 0 geeft aan dat SF proberen moet te defragmenteren knooppunten gelijkmatig over UDs en FDs; 1 geeft alleen aan dat de knooppunten moeten worden gedefragmenteerd |

### <a name="section-name-defragmentationmetrics"></a>Sectienaam: DefragmentationMetrics
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyBoolValueMap, standaardwaarde is geen|Dynamisch|Bepaalt de set met metrische gegevens die moeten worden gebruikt voor defragmentatie en niet voor taakverdeling. |

### <a name="section-name-defragmentationmetricspercentornumberofemptynodestriggeringthreshold"></a>Sectienaam: DefragmentationMetricsPercentOrNumberOfEmptyNodesTriggeringThreshold
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, standaardwaarde is geen|Dynamisch|Bepaalt het aantal beschikbare knooppunten die nodig zijn om het cluster gedefragmenteerd door te geven van beide procent in bereik Overweeg [0.0-1.0) of het aantal lege knooppunten as-nummer > = 1.0 |

### <a name="section-name-dnsservice"></a>Sectienaam: DnsService
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|IsEnabled|BOOL, standaard is ONWAAR|Statisch| |
|InstanceCount|int, standaard-is -1|Statisch|  |

### <a name="section-name-metricactivitythresholds"></a>Sectienaam: MetricActivityThresholds
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, standaardwaarde is geen|Dynamisch|Bepaalt de reeks MetricActivityThresholds van de metrische gegevens in het cluster. Taakverdeling werkt als maxNodeLoad groter dan MetricActivityThresholds is. Defragmentatie metrische gegevens voor de hoeveelheid load gelijk wordt gedefinieerd op of onder die Service Fabric worden beschouwd als het knooppunt leeg |

### <a name="section-name-metricbalancingthresholds"></a>Sectienaam: MetricBalancingThresholds
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, standaardwaarde is geen|Dynamisch|Bepaalt de reeks MetricBalancingThresholds van de metrische gegevens in het cluster. Taakverdeling werkt als maxNodeLoad/minNodeLoad groter dan MetricBalancingThresholds is. Defragmentatie werkt als maxNodeLoad/minNodeLoad in ten minste één FD of UD kleiner dan MetricBalancingThresholds is. |

### <a name="section-name-nodebufferpercentage"></a>Sectienaam: NodeBufferPercentage
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, standaardwaarde is geen|Dynamisch|Percentage van knooppunt capaciteit per metrische naam; Als een buffer gebruikt om te voorkomen dat een gratis plaats op een knooppunt voor de failover-aanvraag. |

### <a name="section-name-replication"></a>Sectienaam: replicatie
| **Parameter** | **Toegestane waarden** | **Beleid voor upgrade**| **Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|MaxCopyQueueSize|Uint, standaard is 1024|Statisch|Dit is de maximale waarde definieert de aanvankelijke grootte van de wachtrij die replicatiebewerkingen onderhoudt.  Houd er rekening mee dat deze een macht van 2 moet.  Als tijdens runtime de wachtrij neemt toe aan de grootte van deze bewerking wordt de snelheid van tussen de primaire en secundaire replicaties.|
|BatchAcknowledgementInterval|TimeSpan, de standaardwaarde is Common::TimeSpan::FromMilliseconds(15)|Statisch|Geef de interval in seconden. Bepaalt de hoeveelheid tijd dat de replicator moet wachten na de ontvangst van een bewerking voor het verzenden van een bevestiging. Andere bewerkingen zijn ontvangen tijdens deze periode wordt hun bevestigingen terug in een enkel bericht verzonden verminderen netwerkverkeer maar mogelijk verminderen de doorvoer van de replicatie -> hebben.|
|MaxReplicationMessageSize|Uint, de standaardwaarde is 52428800|Statisch|Maximale berichtgrootte van replicatiebewerkingen. Standaardwaarde is 50MB.|
|ReplicatorAddress|tekenreeks, default is L "localhost:0"|Statisch|Het eindpunt in de vorm van een tekenreeks-'IP: poort' die met de Windows Fabric-replicatie wordt gebruikt voor het tot stand brengen van verbindingen met andere replica's kunnen bewerkingen verzenden/ontvangen.|
|ReplicatorListenAddress|tekenreeks, default is L "localhost:0"|Statisch|Het eindpunt in de vorm van een tekenreeks-'IP: poort' die met de Windows Fabric-replicatie wordt gebruikt voor bewerkingen van andere replica's ontvangen.|
|ReplicatorPublishAddress|tekenreeks, default is L "localhost:0"|Statisch|Het eindpunt in de vorm van een tekenreeks-'IP: poort' die met de Windows Fabric-replicatie wordt gebruikt voor bewerkingen verzenden naar andere replica's.|
|MaxPrimaryReplicationQueueSize|Uint, standaard is 1024|Statisch|Dit is het maximum aantal bewerkingen die kunnen bestaan in de wachtrij primaire replicatie. Houd er rekening mee dat deze een macht van 2 moet.|
|MaxPrimaryReplicationQueueMemorySize|Uint, de standaardwaarde is 0|Statisch|Dit is de maximale waarde van de wachtrij voor de primaire replicatie in bytes.|
|MaxSecondaryReplicationQueueSize|uint, standaard is 2048|Statisch|Dit is het maximum aantal bewerkingen die kunnen bestaan in de secundaire replicatiewachtrij. Houd er rekening mee dat deze een macht van 2 moet.|
|MaxSecondaryReplicationQueueMemorySize|Uint, de standaardwaarde is 0|Statisch|Dit is de maximale waarde van de secundaire replicatiewachtrij in bytes.|
|QueueHealthMonitoringInterval|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(30)|Statisch|Geef de interval in seconden. Deze waarde bepaalt de periode die wordt gebruikt door de replicatie voor het bewaken van de waarschuwing/fout health-gebeurtenissen in de bewerking replicatiewachtrijen. De waarde '0' wordt uitgeschakeld statuscontrole |
|QueueHealthWarningAtUsagePercent|uint, standaard is 80|Statisch|Deze waarde bepaalt de replicatie wachtrij gebruik (in percentage) waarna er waarschuwing rapporteren over het gebruik van hoge wachtrij. We doen dit na een interval van de respijtperiode van QueueHealthMonitoringInterval. Als de wachtrij-gebruik lager is dan dit percentage in het interval respijtperiode|
|retryInterval|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(5)|Statisch|Geef de interval in seconden. Wanneer een bewerking verloren is gegaan of afgewezen deze timer bepaalt hoe vaak de replicatie wordt opnieuw geprobeerd de bewerking uit te verzenden.|

### <a name="section-name-transport"></a>Sectienaam: Transport
| **Parameter** | **Toegestane waarden** |**Beleid voor upgrade** |**Hulp of korte beschrijving** |
| --- | --- | --- | --- |
|ResolveOption|tekenreeks is de standaardwaarde is L "niet-opgegeven"|Statisch|Hiermee wordt bepaald hoe de FQDN-naam is opgelost.  Geldige waarden zijn 'niet opgegeven/ipv4/ipv6'. |
|ConnectionOpenTimeout|TimeSpan, de standaardwaarde is Common::TimeSpan::FromSeconds(60)|Statisch|Geef de interval in seconden. Time-out voor verbinding instellen voor zowel inkomende als overnemende zijde (inclusief beveiligingsonderhandeling in veilige modus) |

## <a name="next-steps"></a>Volgende stappen
Lees deze artikelen voor meer informatie over het Clusterbeheer:

[Toevoegen, overschakelen, certificaten uit uw Azure-cluster verwijderen](service-fabric-cluster-security-update-certs-azure.md) 

