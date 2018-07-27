---
title: Azure Service Fabric-cluster wijzigen | Microsoft Docs
description: Dit artikel beschrijft de fabric-instellingen en het Upgradebeleid voor infrastructuur die u kunt aanpassen.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/25/2018
ms.author: aljo
ms.openlocfilehash: 56c904c0da87c3b0023fe5c9a125a359e23678dc
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263807"
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Instellingen voor Service Fabric-cluster en Fabric-Upgradebeleid aanpassen
Dit document leest u hoe de verschillende fabric-instellingen aanpassen en Upgradebeleid in de infrastructuur voor uw Service Fabric-cluster. U kunt aanpassen via de [Azure-portal](https://portal.azure.com) of met behulp van een Azure Resource Manager-sjabloon.

> [!NOTE]
> Niet alle instellingen zijn beschikbaar in de portal. Als een instelling hieronder vermelde niet beschikbaar is via de portal aanpassen met behulp van een Azure Resource Manager-sjabloon.
> 

## <a name="description-of-the-different-upgrade-policies"></a>Beschrijving van de verschillende beleidsregels voor upgrade

- **Dynamische** – wijzigingen naar een dynamische configuratie zorgen niet voor elk proces opnieuw opstarten van Service Fabric-processen of de host-service wordt verwerkt. 
- **Statische** : wijzigingen in de configuratie van een statisch, zullen de Service Fabric-knooppunt opnieuw op te starten om de wijziging in beslag nemen. Services op de knooppunten wordt opnieuw gestart.
- **NotAllowed** : deze instellingen kunnen niet worden gewijzigd. Wijzigen van deze instellingen vereist dat het cluster worden vernietigd en een nieuw cluster gemaakt. 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Instellingen van het cluster met behulp van Resource Manager-sjablonen aanpassen
De volgende stappen laten zien hoe u een nieuwe instelling toevoegen *MaxDiskQuotaInMB* naar de *Diagnostics* sectie.

1. Ga naar https://resources.azure.com
2. Navigeer naar uw abonnement door uit te vouwen **abonnementen** -> **\<uw abonnement >** -> **resourceGroups**  ->   **\<Uw resourcegroep >** -> **providers** -> **Microsoft.ServiceFabric**  ->  **clusters** -> **\<de naam van uw Cluster >**
3. Selecteer in de rechterbovenhoek, **lezen/schrijven.**
4. Selecteer **bewerken** en werk de `fabricSettings` JSON-element en nieuw-element toe te voegen:

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

Hierna volgt een lijst van Fabric-instellingen die u kunt aanpassen, ingedeeld per sectie.

## <a name="applicationgatewayhttp"></a>Application Gateway/Http
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ApplicationCertificateValidationPolicy|reeks, standaard is ingesteld op 'None'|Statisch| Dit valideert niet certificaat van de server; de aanvraag mislukt. Raadpleeg config ServiceCertificateThumbprints voor de door komma's gescheiden lijst met vingerafdrukken van de externe certificaten die de reverse proxy kunt vertrouwen. Raadpleeg config ServiceCommonNameAndIssuer voor de vingerafdruk van het onderwerp en -verlenersleutel van de externe certificaten die de reverse proxy kunt vertrouwen. |
|BodyChunkSize |Uint, de standaardwaarde is 16384 tekens |Dynamisch| Geeft de grootte van voor het segment in bytes die worden gebruikt om te lezen van de hoofdtekst. |
|CrlCheckingFlag|uint, standaard is 0x40000000 |Dynamisch| Vlaggen voor de toepassing/service validatie van certificaatketen; bijvoorbeeld CRL-controle 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY instellen op 0 Hiermee schakelt CRL controleren of volledige lijst met ondersteunde waarden wordt beschreven door dwFlags van CertGetCertificateChain: http://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx  |
|DefaultHttpRequestTimeout |Tijd in seconden. de standaardwaarde is 120 |Dynamisch|Interval in seconden opgeven.  Geeft de standaardtime-out-aanvraag naar de http-aanvragen worden verwerkt in de http-gateway-app. |
|ForwardClientCertificate|BOOL, standaard is ingesteld op FALSE|Dynamisch|Wanneer wordt ingesteld op false, omgekeerde proxy geen aanvragen voor het clientcertificaat. Wanneer is ingesteld op true, omgekeerde proxy wordt voor het clientcertificaat tijdens de SSL-handshake aanvragen en doorsturen van de met base64 gecodeerde kan tekenreeks voor PEM-indeling naar de service in een header X-Client-Certificate.The service genaamd de aanvraag met de juiste statuscode mislukken na het inspecteren van gegevens van het certificaat. Als dit correct is en de client heeft een certificaat niet aanwezig, wordt reverse proxy-doorsturen van een lege-header en kan de service die de aanvraag te verwerken. Omgekeerde proxy fungeert als een transparante laag.|
|GatewayAuthCredentialType |reeks, standaard is ingesteld op 'None' |Statisch| Hiermee geeft u het type van de beveiligingsreferenties voor het gebruik van op de HTTP-app gateway-eindpunt geldige waarden zijn ' geen / X 509. |
|GatewayX509CertificateFindType |tekenreeks, standaard is "FindByThumbprint" |Dynamisch| Geeft aan hoe om te zoeken naar certificaat in het archief dat is opgegeven door de waarde GatewayX509CertificateStoreName ondersteund: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | tekenreeks, standaardwaarde is "" |Dynamisch| Filter zoekwaarde gebruikt voor het HTTP-app gateway-certificaat te zoeken. Dit certificaat is geconfigureerd op het https-eindpunt en kan ook worden gebruikt om te controleren of de identiteit van de app zo nodig door de services. FindValue wordt eerst; opgezocht en als dat niet bestaat; FindValueSecondary wordt opgezocht. |
|GatewayX509CertificateFindValueSecondary | tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het HTTP-app gateway-certificaat te zoeken. Dit certificaat is geconfigureerd op het https-eindpunt en kan ook worden gebruikt om te controleren of de identiteit van de app zo nodig door de services. FindValue wordt eerst; opgezocht en als dat niet bestaat; FindValueSecondary wordt opgezocht.|
|GatewayX509CertificateStoreName |tekenreeks, standaardwaarde is 'My' |Dynamisch| De naam van X.509-certificaatarchief dat certificaat voor http-app-gateway bevat. |
|HttpRequestConnectTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(5)|Dynamisch|Interval in seconden opgeven.  Geeft de time-out voor de verbinding voor de http-aanvragen worden verzonden vanaf de http-gateway-app.  |
|IgnoreCrlOfflineError|BOOL, standaard is ingesteld op TRUE|Dynamisch|Hiermee geeft u op of u offline fout van de CRL voor certificaatverificatie van de toepassing/service-negeert. |
|IsEnabled |BOOL, de standaardinstelling is false |Statisch| / Schakelt de HttpApplicationGateway. HttpApplicationGateway is standaard uitgeschakeld en deze configuratie moet worden ingesteld om te schakelen. |
|NumberOfParallelOperations | Uint, de standaardwaarde is 5000 |Statisch|Het aantal leesbewerkingen en op de server HTTP-wachtrij plaatsen. Hiermee bepaalt u het aantal gelijktijdige aanvragen op dat door de HttpGateway kan worden voldaan. |
|RemoveServiceResponseHeaders|tekenreeks, standaard is 'datum; Server'|Statisch|Een puntkomma / door komma's gescheiden lijst met reactieheaders die wordt verwijderd uit het antwoord van de service; voordat deze worden doorgestuurd naar de client. Als deze is ingesteld op een lege tekenreeks; doorgeven van de headers die zijn geretourneerd door de service-is. Internet Explorer de datum en de Server niet overschrijven |
|ResolveServiceBackoffInterval |Tijd in seconden, de standaardwaarde is 5 |Dynamisch|Interval in seconden opgeven.  Geeft het oplossen van het standaard back-off interval voordat opnieuw wordt geprobeerd een mislukte service bewerking. |
|SecureOnlyMode|BOOL, standaard is ingesteld op FALSE|Dynamisch| SecureOnlyMode: true: omgekeerde Proxy alleen wordt doorgestuurd naar services die beveiligde eindpunten publiceren. ONWAAR: omgekeerde Proxy kan aanvragen voor beveiligde/niet-beveiligde eindpunten worden doorgestuurd.  |
|ServiceCertificateThumbprints|tekenreeks, standaardwaarde is ""|Dynamisch|De door komma's gescheiden lijst met vingerafdrukken van de externe certificaten die de reverse proxy kunt vertrouwen.  |

## <a name="applicationgatewayhttpservicecommonnameandissuer"></a>Application Gateway/Http/ServiceCommonNameAndIssuer
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, is standaard ingesteld op geen|Dynamisch| Onderwerp en -verlenersleutel vingerafdruk van het externe certificaten die de reverse proxy kunt vertrouwen.|

## <a name="backuprestoreservice"></a>BackupRestoreService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|MinReplicaSetSize|int, standaard is 0|Statisch|De MinReplicaSetSize voor BackupRestoreService |
|PlacementConstraints|tekenreeks, standaardwaarde is ""|Statisch|  De PlacementConstraints voor BackupRestore-service |
|SecretEncryptionCertThumbprint|tekenreeks, standaardwaarde is ""|Dynamisch|Vingerafdruk van het geheim versleutelingscertificaat X509 |
|SecretEncryptionCertX509StoreName|tekenreeks, standaardwaarde is 'My'|   Dynamisch|    Dit geeft aan dat het certificaat moet worden gebruikt voor versleuteling en ontsleuteling van referenties voor de naam van de X.509-certificaatarchief die wordt gebruikt voor het versleutelen van ontsleutelen store referenties die worden gebruikt door de service back-up terugzetten |
|TargetReplicaSetSize|int, standaard is 0|Statisch| De TargetReplicaSetSize voor BackupRestoreService |

## <a name="clustermanager"></a>ClusterManager
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|EnableDefaultServicesUpgrade | BOOL, de standaardinstelling is false |Dynamisch|Upgrade standaardservices inschakelen tijdens de upgrade van de toepassing. Beschrijvingen van de standaardservice wordt na de upgrade overschreven. |
|FabricUpgradeHealthCheckInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van de health-status controleren tijdens een bewaakte Fabric-upgrade |
|FabricUpgradeStatusPollInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van polling voor de status van de Fabric-upgrade. Deze waarde bepaalt de frequentie van de update voor elke aanroep GetFabricUpgradeProgress |
|ImageBuilderTimeoutBuffer |Tijd in seconden, de standaardwaarde is 3 |Dynamisch|Interval in seconden opgeven. De hoeveelheid tijd om toe te staan voor Image Builder specifieke time-outfouten optreden om terug te keren naar de client. Als deze buffer te klein is. vervolgens de client een time-out optreedt voordat de server en een algemene time-outfout opgehaald. |
|InfrastructureTaskHealthCheckRetryTimeout | Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Interval in seconden opgeven. De hoeveelheid tijd te besteden aan het opnieuw proberen is statuscontroles tijdens het verwerken van een taak infrastructuur na mislukt. Naleving van de statuscontrole doorgegeven, wordt deze timer opnieuw ingesteld. |
|InfrastructureTaskHealthCheckStableDuration | Tijd in seconden, de standaardwaarde is 0|Dynamisch| Interval in seconden opgeven. De hoeveelheid tijd om te observeren opeenvolgende doorgegeven statuscontroles voorafgaand aan de na verwerking van een infrastructuur-taak is voltooid. Naleving van de statuscontrole is mislukt, worden deze timer opnieuw ingesteld. |
|InfrastructureTaskHealthCheckWaitDuration |Tijd in seconden, de standaardwaarde is 0|Dynamisch| Interval in seconden opgeven. De hoeveelheid tijd moet worden gewacht voordat u begint met statuscontroles na naverwerking van de taak van een infrastructuur. |
|InfrastructureTaskProcessingInterval | Tijd in seconden, de standaardwaarde is 10 |Dynamisch|Interval in seconden opgeven. Het verwerkingsinterval die worden gebruikt door de infrastructuur-taak toestandsmachine verwerken. |
|MaxCommunicationTimeout |Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Interval in seconden opgeven. De maximale time-out voor interne communicatie tussen ClusterManager en andere services (dat wil zeggen; Naming-Service. "Failover Manager en enz.). Deze time-outwaarde moet kleiner zijn dan global MaxOperationTimeout (als er mogelijk meerdere communicatie tussen onderdelen van het systeem voor elke clientbewerking). |
|MaxDataMigrationTimeout |Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Interval in seconden opgeven. De maximale time-out voor data migration herstelbewerkingen nadat de upgrade van een Fabric heeft plaatsgevonden. |
|MaxOperationRetryDelay |Tijd in seconden, de standaardwaarde is 5|Dynamisch| Interval in seconden opgeven. De maximale vertraging voor interne nieuwe pogingen als er fouten zijn aangetroffen. |
|MaxOperationTimeout |Tijd in seconden, de standaardwaarde is MaxValue |Dynamisch| Interval in seconden opgeven. De maximale algemene time-out voor het verwerken van intern bewerkingen op ClusterManager. |
|MaxTimeoutRetryBuffer | Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Interval in seconden opgeven. Maximale time-out voor de bewerking als de intern vanwege time-outs voor opnieuw proberen is <Original Time out>  +  <MaxTimeoutRetryBuffer>. Aanvullende time-out wordt in stappen van MinOperationTimeout toegevoegd. |
|MinOperationTimeout | Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Interval in seconden opgeven. De minimale algemene time-out voor het verwerken van intern bewerkingen op ClusterManager. |
|MinReplicaSetSize |Int, de standaardwaarde is 3 |Niet toegestaan|De MinReplicaSetSize voor ClusterManager. |
|PlacementConstraints | tekenreeks, standaardwaarde is "" |Niet toegestaan|De PlacementConstraints voor ClusterManager. |
|QuorumLossWaitDuration |Tijd in seconden, de standaardwaarde is MaxValue |Niet toegestaan| Interval in seconden opgeven. De QuorumLossWaitDuration voor ClusterManager. |
|ReplicaRestartWaitDuration |Tijd in seconden, wordt standaard (60,0 * 30)|Niet toegestaan|Interval in seconden opgeven. De ReplicaRestartWaitDuration voor ClusterManager. |
|ReplicaSetCheckTimeoutRollbackOverride |Tijd in seconden, is standaard 1200 |Dynamisch| Interval in seconden opgeven. Als ReplicaSetCheckTimeout is ingesteld op de maximale waarde van DWORD; en vervolgens deze overschreven door de waarde van deze configuratie voor de toepassing van terugdraaien. De waarde die wordt gebruikt voor doorvoeren wordt nooit overschreven. |
|SkipRollbackUpdateDefaultService | BOOL, de standaardinstelling is false |Dynamisch|De CM wordt herstellen bijgewerkte standaardservices overgeslagen tijdens de upgrade terugdraaien van de toepassing. |
|StandByReplicaKeepDuration | Tijd in seconden, wordt standaard (3600.0 * 2)|Niet toegestaan|Interval in seconden opgeven. De StandByReplicaKeepDuration voor ClusterManager. |
|TargetReplicaSetSize |Int, de standaardwaarde is 7 |Niet toegestaan|De TargetReplicaSetSize voor ClusterManager. |
|UpgradeHealthCheckInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van de status van controleert tijdens upgrades van een bewaakte toepassingen |
|UpgradeStatusPollInterval |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|De frequentie van polling voor de upgradestatus van toepassing. Deze waarde bepaalt de frequentie van de update voor elke aanroep GetApplicationUpgradeProgress |

## <a name="common"></a>Algemeen
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PerfMonitorInterval |Tijd in seconden, de standaardwaarde is 1 |Dynamisch|Interval in seconden opgeven. Prestaties van controle-interval. Instelling op 0 of negatieve waarde wordt uitgeschakeld bewaking. |

## <a name="defragmentationemptynodedistributionpolicy"></a>DefragmentationEmptyNodeDistributionPolicy
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, is standaard ingesteld op geen|Dynamisch|Hiermee geeft u dat het beleid defragmentatie volgt bij het leegmaken van knooppunten. Voor een bepaalde waarde betekent 0 dat SF proberen moet te defragmenteren knooppunten gelijkmatig op ud's en FD's; 1 geeft alleen aan dat de knooppunten moeten worden gedefragmenteerd |

## <a name="defragmentationmetrics"></a>DefragmentationMetrics
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyBoolValueMap, is standaard ingesteld op geen|Dynamisch|Bepaalt de set met metrische gegevens die moeten worden gebruikt voor defragmentatie en niet voor taakverdeling. |

## <a name="defragmentationmetricspercentornumberofemptynodestriggeringthreshold"></a>DefragmentationMetricsPercentOrNumberOfEmptyNodesTriggeringThreshold
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, is standaard ingesteld op geen|Dynamisch|Bepaalt het aantal gratis knooppunten die nodig zijn om te overwegen cluster gedefragmenteerd door een van beide procent op te geven in bereik [0.0-1.0) of het nummer van lege knooppunten als getal > = 1,0 |

## <a name="diagnostics"></a>Diagnostiek
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|AppDiagnosticStoreAccessRequiresImpersonation |BOOL, de standaardinstelling is true | Dynamisch |Imitatie is wel of niet vereist wanneer toegang tot diagnostische namens de toepassing opslaat. |
|AppEtwTraceDeletionAgeInDays |Int, de standaardwaarde is 3 | Dynamisch |Aantal dagen waarna we oude ETL-bestanden met toepassing ETW-traceringen wilt verwijderen. |
|ApplicationLogsFormatVersion |Int, de standaardwaarde is 0 | Dynamisch |Versie voor de toepassing registreert indeling. Ondersteunde waarden zijn 0 en 1. Versie 1 bevat meer velden uit de record van de ETW-gebeurtenis dan de versie van 0. |
|ClusterId |Reeks | Dynamisch |De unieke id van het cluster. Dit wordt gegenereerd wanneer het cluster is gemaakt. |
|ConsumerInstances |Reeks | Dynamisch |De lijst van DCA consumentexemplaren. |
|DiskFullSafetySpaceInMB |Int, standaard is 1024 | Dynamisch |Resterende schijfruimte in MB te beschermen tegen door DCA worden gebruikt. |
|EnableCircularTraceSession |BOOL, de standaardinstelling is false | Statisch |Vlag geeft aan of cirkelvormige trace-sessies moeten worden gebruikt. |
|EnableTelemetry |BOOL, de standaardinstelling is true | Dynamisch |Dit gaat in- of uitschakelen telemetrie. |
|MaxDiskQuotaInMB |Int, de standaardwaarde is 65536 | Dynamisch |Schijfquotum in MB voor Windows Fabric-logboekbestanden. |
|ProducerInstances |Reeks | Dynamisch |De lijst met instanties van DCA-producent. |

## <a name="dnsservice"></a>De DNS-service
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|InstanceCount|int, standaard is 1|Statisch|standaardwaarde is 1, wat betekent dat de DNS-service wordt uitgevoerd op elk knooppunt. OneBox moet dit moet worden ingesteld op 1, omdat de DNS-service maakt gebruik van bekende poort 53, zodat deze kan niet meerdere exemplaren op dezelfde computer hebt.|
|IsEnabled|BOOL, standaard is ingesteld op FALSE|Statisch|Schakelt/DNS-service. De DNS-service is standaard uitgeschakeld en deze configuratie moet worden ingesteld zodat het. |
|PartitionPrefix|tekenreeks, standaardwaarde is "-"|Statisch|Hiermee bepaalt u de partitie voorvoegsel string-waarde in de DNS-query's voor gepartitioneerde services. De waarde: <ul><li>Moet zijn RFC-compatibele omdat deze deel van een DNS-query uitmaken.</li><li>Mag niet een punt, '.', zoals stip gedrag van DNS-achtervoegsel verstoort.</li><li>Mag niet langer zijn dan 5 tekens.</li><li>Mag geen lege tekenreeks zijn.</li><li>Als de instelling PartitionPrefix wordt overschreven, wordt de PartitionSuffix moet worden vervangen, en vice versa.</li></ul>Zie voor meer informatie, [DNS aan Service Fabric.](service-fabric-dnsservice.md).|
|PartitionSuffix|tekenreeks, standaardwaarde is ""|Statisch|Hiermee bepaalt u de partitie achtervoegsel string-waarde in de DNS-query's voor gepartitioneerde services. De waarde: <ul><li>Moet zijn RFC-compatibele omdat deze deel van een DNS-query uitmaken.</li><li>Mag niet een punt, '.', zoals stip gedrag van DNS-achtervoegsel verstoort.</li><li>Mag niet langer zijn dan 5 tekens.</li><li>Als de instelling PartitionPrefix wordt overschreven, wordt de PartitionSuffix moet worden vervangen, en vice versa.</li></ul>Zie voor meer informatie, [DNS aan Service Fabric.](service-fabric-dnsservice.md). |

## <a name="fabricclient"></a>FabricClient
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ConnectionInitializationTimeout |Tijd in seconden, de standaardwaarde is 2 |Dynamisch|Interval in seconden opgeven. Time-outinterval voor elke keer-client verbinding probeert te openen van een verbinding met de gateway.|
|HealthOperationTimeout |Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Interval in seconden opgeven. De time-out voor een rapportbericht verzonden naar de status Manager. |
|HealthReportRetrySendInterval |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Interval in seconden opgeven. Het interval waarmee reporting onderdeel opnieuw verzendt samengevoegde statusrapporten naar Health Manager. |
|HealthReportSendInterval |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Interval in seconden opgeven. Het interval waarmee reporting onderdeel stuurt samengevoegde statusrapporten naar Health Manager. |
|KeepAliveIntervalInSeconds |Int, de standaardwaarde is 20 |Statisch|Het interval waarmee het transport FabricClient keepalive-berichten naar de gateway verzendt. Voor 0; keepAlive is uitgeschakeld. Moet een positieve waarde zijn. |
|MaxFileSenderThreads |Uint, de standaardwaarde is 10 |Statisch|Het maximum aantal bestanden die parallel worden overgedragen. |
|NodeAddresses |tekenreeks, standaardwaarde is "" |Statisch|Een verzameling van adressen (verbindingsreeksen) op verschillende knooppunten die kunnen worden gebruikt om te communiceren met de Naming-Service. In eerste instantie verbindt de Client een van de adressen willekeurig selecteren. Als er meer dan één verbindingsreeks wordt geleverd en een verbinding is mislukt vanwege een communicatie- of time-outfout; de Client-switches te gebruiken het volgende adres sequentieel worden verwerkt. Zie de naamgeving van het adres van de sectie voor meer informatie over de semantiek voor nieuwe pogingen opnieuw. |
|PartitionLocationCacheLimit |Int, standaard is 100.000 |Statisch|Het aantal partities in de cache opgeslagen voor service-oplossing (ingesteld op 0 om geen limiet). |
|RetryBackoffInterval |Tijd in seconden, de standaardwaarde is 3 |Dynamisch|Interval in seconden opgeven. De uitstelinterval voordat u de bewerking opnieuw uitvoert. |
|ServiceChangePollInterval |Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Interval in seconden opgeven. Het interval tussen opeenvolgende polls voor de service wordt gewijzigd van de client naar de gateway voor meldingen callbacks van geregistreerde service wijzigen. |

## <a name="fabrichost"></a>Fabrichost wordt bij
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Int, de standaardwaarde is 10 |Dynamisch|Dit is het maximale aantal die systeem wordt opnieuw geprobeerd mislukte activering voordat geeft. |
|ActivationMaxRetryInterval |Tijd in seconden, de standaardwaarde is 300 |Dynamisch|Interval in seconden opgeven. Interval voor maximum aantal nieuwe pogingen voor activering. Bij elke continue fout wordt het interval voor opnieuw proberen berekend als Min (ActivationMaxRetryInterval; Continue foutenteller * ActivationRetryBackoffInterval). |
|ActivationRetryBackoffInterval |Tijd in seconden, de standaardwaarde is 5 |Dynamisch|Interval in seconden opgeven. Uitstel interval op om de activering is mislukt; op om het systeem wordt opnieuw geprobeerd de activering van de maximaal de MaxActivationFailureCount continue activering is mislukt. Het interval voor opnieuw proberen bij elke poging is een product van continue de activering is mislukt en de uitstelinterval activering. |
|EnableRestartManagement |BOOL, de standaardinstelling is false |Dynamisch|Dit is om in te schakelen van de server opnieuw is gestart. |
|EnableServiceFabricAutomaticUpdates |BOOL, de standaardinstelling is false |Dynamisch|Dit is een automatische update van de fabric via Windows Update wilt inschakelen. |
|EnableServiceFabricBaseUpgrade |BOOL, de standaardinstelling is false |Dynamisch|Dit is bijgewerkt voor server inschakelen. |
|StartTimeout |Tijd in seconden, de standaardwaarde is 300 |Dynamisch|Interval in seconden opgeven. Time-out voor het opstarten van fabricactivationmanager. |
|StopTimeout |Tijd in seconden, de standaardwaarde is 300 |Dynamisch|Interval in seconden opgeven. De time-out voor activering van de gehoste service. deactiveren en upgrade. |

## <a name="fabricnode"></a>FabricNode
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ClientAuthX509FindType |tekenreeks, standaard is "FindByThumbprint" |Dynamisch|Geeft aan hoe om te zoeken naar certificaat in het archief dat is opgegeven door de waarde ClientAuthX509StoreName ondersteund: FindByThumbprint; FindBySubjectName. |
|ClientAuthX509FindValue |tekenreeks, standaardwaarde is "" | Dynamisch|Filter zoekwaarde gebruikt voor het certificaat voor standaard-beheerdersrol FabricClient te zoeken. |
|ClientAuthX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het certificaat voor standaard-beheerdersrol FabricClient te zoeken. |
|ClientAuthX509StoreName |tekenreeks, standaardwaarde is 'My' |Dynamisch|De naam van het x.509-certificaat-archief met certificaten voor standaard-beheerdersrol FabricClient. |
|ClusterX509FindType |tekenreeks, standaard is "FindByThumbprint" |Dynamisch|Geeft aan hoe om te zoeken naar clustercertificaat in het archief van opgegeven waarden ClusterX509StoreName ondersteund: "FindByThumbprint"; "FindBySubjectName" met "FindBySubjectName"; Wanneer er meerdere overeenkomsten; de categorie met de vervaldatum van het verst wordt gebruikt. |
|ClusterX509FindValue |tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het clustercertificaat te zoeken. |
|ClusterX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het clustercertificaat te zoeken. |
|ClusterX509StoreName |tekenreeks, standaardwaarde is 'My' |Dynamisch|De naam van X.509-certificaatarchief dat clustercertificaat voor het beveiligen van communicatie tussen intra-cluster bevat. |
|EndApplicationPortRange |Int, de standaardwaarde is 0 |Statisch|Einde (niet inclusief) van de toepassingspoorten die worden beheerd door die als host fungeert subsysteem. Vereist als EndpointFilteringEnabled ingesteld op true in Hosting is. |
|ServerAuthX509FindType |tekenreeks, standaard is "FindByThumbprint" |Dynamisch|Geeft aan hoe om te zoeken naar servercertificaat in het archief dat is opgegeven door de waarde ServerAuthX509StoreName ondersteund: FindByThumbprint; FindBySubjectName. |
|ServerAuthX509FindValue |tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het servercertificaat te zoeken. |
|ServerAuthX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het servercertificaat te zoeken. |
|ServerAuthX509StoreName |tekenreeks, standaardwaarde is 'My' |Dynamisch|De naam van X.509-certificaatarchief die het servercertificaat voor soepele Kennismaking service bevat. |
|StartApplicationPortRange |Int, de standaardwaarde is 0 |Statisch|Begin van de toepassingspoorten die worden beheerd door die als host fungeert subsysteem. Vereist als EndpointFilteringEnabled ingesteld op true in Hosting is. |
|StateTraceInterval |Tijd in seconden, de standaardwaarde is 300 |Statisch|Interval in seconden opgeven. Het interval voor het traceren van knooppuntstatus op elk knooppunt en van knooppunten op FM/FMM. |
|UserRoleClientX509FindType |tekenreeks, standaard is "FindByThumbprint" |Dynamisch|Geeft aan hoe om te zoeken naar certificaat in het archief dat is opgegeven door de waarde UserRoleClientX509StoreName ondersteund: FindByThumbprint; FindBySubjectName. |
|UserRoleClientX509FindValue |tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het certificaat voor de rol van de gebruiker standaard FabricClient te zoeken. |
|UserRoleClientX509FindValueSecondary |tekenreeks, standaardwaarde is "" |Dynamisch|Filter zoekwaarde gebruikt voor het certificaat voor de rol van de gebruiker standaard FabricClient te zoeken. |
|UserRoleClientX509StoreName |tekenreeks, standaardwaarde is 'My' |Dynamisch|De naam van het X.509-certificaatarchief dat certificaat voor de rol van de gebruiker standaard FabricClient bevat. |

## <a name="failovermanager"></a>FailoverManager
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|BuildReplicaTimeLimit|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(3600)|Dynamisch|Interval in seconden opgeven. De tijdslimiet voor het bouwen van een stateful replica. na een statusrapport van de waarschuwing wordt gestart |
|ClusterPauseThreshold|int, standaard is 1|Dynamisch|Als het aantal knooppunten in het systeem onder deze waarde wordt de plaatsing; taken verdelen; en de failover is gestopt. |
|CreateInstanceTimeLimit|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(300)|Dynamisch|Interval in seconden opgeven. De tijdslimiet voor het maken van een staatloze exemplaar; na een statusrapport van de waarschuwing wordt gestart |
|ExpectedClusterSize|int, standaard is 1|Dynamisch|Wanneer het cluster in eerste instantie wordt gestart de FM wacht tot dit veel knooppunten om te rapporteren zelf omhoog voordat deze wordt gestart met het plaatsen van andere services; met inbegrip van het system-services, zoals namen. Deze waarde verhoogt, worden de tijd die nodig is een cluster om opnieuw te starten; maar voorkomt u dat de vroege knooppunten uit overbelast en ook de aanvullende verplaatst die niet nodig als u meer knooppunten online is gekomen. Deze waarde moet in het algemeen worden ingesteld op een klein deel van de eerste clustergrootte. |
|ExpectedNodeDeactivationDuration|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Interval in seconden opgeven. Dit is de verwachte duur voor een knooppunt om te deactiveren in voltooien. |
|ExpectedNodeFabricUpgradeDuration|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Interval in seconden opgeven. Dit is de verwachte duur voor een knooppunt moet worden bijgewerkt tijdens de upgrade van Windows Fabric. |
|ExpectedReplicaUpgradeDuration|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Interval in seconden opgeven. Dit is de verwachte duur voor alle replica's moeten worden bijgewerkt op een knooppunt tijdens de upgrade van de toepassing. |
|IsSingletonReplicaMoveAllowedDuringUpgrade|BOOL, standaard is ingesteld op TRUE|Dynamisch|Als is ingesteld op onwaar. replica's met een grootte van de doel-1 worden toegestaan om te verplaatsen tijdens de upgrade. |
|MinReplicaSetSize|Int, de standaardwaarde is 3|Niet toegestaan|Dit is de grootte van de minimale replica voor de FM. Als het aantal actieve FM-replica's onder deze waarde komt; de FM weigert wijzigingen aan het cluster tot ten minste het minimum aantal replica's kunnen worden hersteld |
|PlacementConstraints|tekenreeks, standaardwaarde is ""|Niet toegestaan|Alle plaatsingsbeperkingen voor de failover manager replica 's |
|PlacementTimeLimit|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(600)|Dynamisch|Interval in seconden opgeven. De tijdslimiet voor het bereiken van doel replica count; na een statusrapport van de waarschuwing wordt gestart |
|QuorumLossWaitDuration |Tijd in seconden, de standaardwaarde is MaxValue |Dynamisch|Interval in seconden opgeven. Dit is de maximale duur waarvoor we een partitie in een status van quorumverlies toestaan. Als de partitie nog steeds sprake van quorumverlies na deze duur is; de partitie is hersteld van quorumverlies door rekening te houden de omlaag replica's als verloren. Houd er rekening mee dat dit mogelijk verlies van gegevens kan dit tot. |
|ReconfigurationTimeLimit|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(300)|Dynamisch|Interval in seconden opgeven. De tijdslimiet voor herconfiguratie; na een statusrapport van de waarschuwing wordt gestart |
|ReplicaRestartWaitDuration|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(60.0 * 30)|Niet toegestaan|Interval in seconden opgeven. Dit is de ReplicaRestartWaitDuration voor de FMService |
|StandByReplicaKeepDuration|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(3600.0 * 24 * 7)|Niet toegestaan|Interval in seconden opgeven. Dit is de StandByReplicaKeepDuration voor de FMService |
|TargetReplicaSetSize|Int, de standaardwaarde is 7|Niet toegestaan|Dit is het doelaantal FM-replica's die wordt onderhouden van Windows Fabric. Hoe hoger de waarde resulteert in meer betrouwbaarheid van de gegevens FM; met een negatieve gevolgen voor de prestaties van kleine. |
|UserMaxStandByReplicaCount |Int, de standaardwaarde is 1 |Dynamisch|De standaard maximale aantal StandBy-replica's die het systeem voor de services gebruiken blijft. |
|UserReplicaRestartWaitDuration |Tijd in seconden, is standaard 60,0 * 30 |Dynamisch|Interval in seconden opgeven. Wanneer een persistente replica wordt uitgeschakeld; Windows Fabric wacht voor deze duur voor de replica op terugkomen om vóór het maken van nieuwe vervanging replica's (waarvoor een kopie van de status). |
|UserStandByReplicaKeepDuration |Tijd in seconden, de standaardwaarde is 3600.0 * 24 * 7 |Dynamisch|Interval in seconden opgeven. Wanneer een persistente replica terugkeren van een status down; mogelijk is deze al vervangen. Deze timer bepaalt hoe lang de FM de standby-replica blijft bewaard voordat deze worden verwijderd. |

## <a name="faultanalysisservice"></a>FaultAnalysisService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|CompletedActionKeepDurationInSeconds | Int, de standaardwaarde is 604800 |Statisch| Dit is ongeveer hoelang de acties die zich in een definitieve status behouden. Dit is ook afhankelijk StoredActionCleanupIntervalInSeconds; omdat het werk worden opgeschoond wordt alleen uitgevoerd op dat het interval. 604800 is 7 dagen. |
|DataLossCheckPollIntervalInSeconds|int, standaard is 5|Statisch|Dit is de tijd tussen de controles die het systeem wordt uitgevoerd tijdens het wachten tot verlies van gegevens worden uitgevoerd. Het aantal keren dat die het nummer van de gegevens verloren gaan, wordt gecontroleerd per interne iteratie is DataLossCheckWaitDurationInSeconds/dit. |
|DataLossCheckWaitDurationInSeconds|int, de standaardwaarde is 25|Statisch|De totale hoeveelheid tijd; in seconden. dat het systeem wacht tot verlies van gegevens worden uitgevoerd. Dit wordt intern gebruikt wanneer de StartPartitionDataLossAsync()-api wordt aangeroepen. |
|MinReplicaSetSize |Int, de standaardwaarde is 0 |Statisch|De MinReplicaSetSize voor FaultAnalysisService. |
|PlacementConstraints | tekenreeks, standaardwaarde is ""|Statisch| De PlacementConstraints voor FaultAnalysisService. |
|QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Statisch|Interval in seconden opgeven. De QuorumLossWaitDuration voor FaultAnalysisService. |
|ReplicaDropWaitDurationInSeconds|int, de standaardwaarde is 600|Statisch|Deze parameter wordt gebruikt wanneer het verlies van gegevens-api wordt aangeroepen. Het bepaalt hoe lang het systeem moet wachten voor een replica moet worden verwijderd na het verwijderen replica intern erop wordt aangeroepen. |
|ReplicaRestartWaitDuration |Tijd in seconden, de standaardwaarde is 60 minuten|Statisch|Interval in seconden opgeven. De ReplicaRestartWaitDuration voor FaultAnalysisService. |
|StandByReplicaKeepDuration| Tijd in seconden, de standaardwaarde is (60*24*7) minuten |Statisch|Interval in seconden opgeven. De StandByReplicaKeepDuration voor FaultAnalysisService. |
|StoredActionCleanupIntervalInSeconds | Int, de standaardwaarde is 3600 |Statisch|Dit is hoe vaak de store worden opgeschoond. Alleen acties in een definitieve status; en dat ten minste voltooid CompletedActionKeepDurationInSeconds geleden is verwijderd. |
|StoredChaosEventCleanupIntervalInSeconds | Int, de standaardwaarde is 3600 |Statisch|Dit is hoe vaak de store wordt gecontroleerd om op te schonen; Als het aantal gebeurtenissen meer dan 30000 is; het opruimen van de wordt gestart. |
|TargetReplicaSetSize |Int, de standaardwaarde is 0 |Statisch|De TargetReplicaSetSize voor FaultAnalysisService NOT_PLATFORM_UNIX_START. |

## <a name="federation"></a>Federatie
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|GlobalTicketLeaseDuration|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(300)|Statisch|Interval in seconden opgeven. Knooppunten in het cluster moeten een globale lease onderhouden met de kiezers. Kiezers indienen hun algemene leases worden doorgegeven in het cluster voor deze duur. Als de duur is verlopen; de lease is verbroken. Quorumverlies van de lease zorgt ervoor dat een knooppunt aan het afbreken van het cluster. door gebrek aan communicatie met een quorum van knooppunten in deze periode ontvangen.  Deze waarde moet worden aangepast op basis van de grootte van het cluster. |
|LeaseDuration |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|De duur die een lease tussen een knooppunt en de aangrenzende routers duurt. |
|LeaseDurationAcrossFaultDomain |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|De duur die een lease tussen een knooppunt en de aangrenzende routers over foutdomeinen duurt. |

## <a name="filestoreservice"></a>FileStoreService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|AcceptChunkUpload|BOOL, standaard is ingesteld op TRUE|Dynamisch|De configuratie te bepalen of het bestand store-service uploaden van chunk op basis van het bestand accepteert of niet tijdens het toepassingspakket kopiëren. |
|AnonymousAccessEnabled | BOOL, de standaardinstelling is true |Statisch|Anonieme toegang tot de shares FileStoreService inschakelen/uitschakelen. |
|CommonName1Ntlmx509CommonName|tekenreeks, standaardwaarde is ""|Statisch| De algemene naam van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de CommonName1NtlmPasswordSecret bij het gebruik van NTLM-verificatie |
|CommonName1Ntlmx509StoreLocation|tekenreeks, standaard is "LocalMachine /"|Statisch|De opslaglocatie van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de CommonName1NtlmPasswordSecret bij het gebruik van NTLM-verificatie |
|CommonName1Ntlmx509StoreName|tekenreeks, standaard is 'MY'| Statisch|De naam van de store van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de CommonName1NtlmPasswordSecret bij het gebruik van NTLM-verificatie |
|CommonName2Ntlmx509CommonName|tekenreeks, standaardwaarde is ""|Statisch|De algemene naam van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de CommonName2NtlmPasswordSecret bij het gebruik van NTLM-verificatie |
|CommonName2Ntlmx509StoreLocation|tekenreeks, standaard is "LocalMachine /"| Statisch|De opslaglocatie van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de CommonName2NtlmPasswordSecret bij het gebruik van NTLM-verificatie |
|CommonName2Ntlmx509StoreName|tekenreeks, standaard is 'MY'|Statisch| De naam van de store van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de CommonName2NtlmPasswordSecret bij het gebruik van NTLM-verificatie |
|CommonNameNtlmPasswordSecret|SecureString, de standaardwaarde is Common::SecureString("")| Statisch|Het geheim wachtwoord dat gebruikt als de seed aan hetzelfde wachtwoord gegenereerd bij het gebruik van NTLM-verificatie |
|GenerateV1CommonNameAccount| BOOL, standaard is ingesteld op TRUE|Statisch|Hiermee bepaalt u of voor het genereren van een account met de gebruiker de naam van V1 generatie algoritme. Beginnen met Service Fabric versie 6.1; een account met v2 generatie wordt altijd gemaakt. De V1-account is nodig voor upgrades van/naar versies die geen ondersteuning voor het genereren van de V2 (vóór 6.1).|
|MaxCopyOperationThreads | Uint, de standaardwaarde is 0 |Dynamisch| Het maximale aantal parallelle bestanden dat secundaire kunt van primaire kopiëren. '0' == aantal kernen. |
|MaxFileOperationThreads | Uint, de standaardwaarde is 100 |Statisch| Het maximale aantal parallelle threads om uit te voeren FileOperations (kopiëren/verplaatsen) in de primaire toegestaan. '0' == aantal kernen. |
|MaxRequestProcessingThreads | Uint, standaard is 200 |Statisch|Het maximale aantal parallelle threads voor het verwerken van aanvragen in de primaire toegestaan. '0' == aantal kernen. |
|MaxSecondaryFileCopyFailureThreshold | Uint, de standaardwaarde is 25|Dynamisch|Het maximale aantal bestand kopiëren nieuwe pogingen op de secundaire voordat geeft. |
|MaxStoreOperations | Uint, de standaardwaarde is 4096 |Statisch|Het maximale aantal parallelle store transactie bewerkingen op primaire toegestaan. '0' == aantal kernen. |
|NamingOperationTimeout |Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Interval in seconden opgeven. De time-out voor het uitvoeren van naamgeving bewerking. |
|PrimaryAccountNTLMPasswordSecret | SecureString, de standaardwaarde is leeg |Statisch| Het geheim van het wachtwoord die als de seed aan gegenereerde hetzelfde wachtwoord wordt gebruikt bij het gebruik van NTLM-verificatie. |
|PrimaryAccountNTLMX509StoreLocation | tekenreeks, standaard is "LocalMachine /"|Statisch| De opslaglocatie van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de PrimaryAccountNTLMPasswordSecret bij het gebruik van NTLM-verificatie. |
|PrimaryAccountNTLMX509StoreName | tekenreeks, standaard is 'MY'|Statisch| De naam van de store van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de PrimaryAccountNTLMPasswordSecret bij het gebruik van NTLM-verificatie. |
|PrimaryAccountNTLMX509Thumbprint | tekenreeks, standaardwaarde is ""|Statisch|De vingerafdruk van het X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de PrimaryAccountNTLMPasswordSecret bij het gebruik van NTLM-verificatie. |
|PrimaryAccountType | tekenreeks, standaardwaarde is "" |Statisch|De primaire AccountType van de principal-ACL de FileStoreService deelt. |
|PrimaryAccountUserName | tekenreeks, standaardwaarde is "" |Statisch|De primaire gebruikersnaam van de principal-ACL-account de FileStoreService deelt. |
|PrimaryAccountUserPassword | SecureString, de standaardwaarde is leeg |Statisch|Het wachtwoord van de primaire account van de principal-ACL de FileStoreService deelt. |
|QueryOperationTimeout | Tijd in seconden, de standaardwaarde is 60 |Dynamisch|Interval in seconden opgeven. De time-out voor het uitvoeren van querybewerking. |
|SecondaryAccountNTLMPasswordSecret | SecureString, de standaardwaarde is leeg |Statisch| Het geheim van het wachtwoord die als de seed aan gegenereerde hetzelfde wachtwoord wordt gebruikt bij het gebruik van NTLM-verificatie. |
|SecondaryAccountNTLMX509StoreLocation | tekenreeks, standaard is "LocalMachine /" |Statisch|De opslaglocatie van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de SecondaryAccountNTLMPasswordSecret bij het gebruik van NTLM-verificatie. |
|SecondaryAccountNTLMX509StoreName | tekenreeks, standaard is 'MY' |Statisch|De naam van de store van de X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de SecondaryAccountNTLMPasswordSecret bij het gebruik van NTLM-verificatie. |
|SecondaryAccountNTLMX509Thumbprint | tekenreeks, standaardwaarde is ""| Statisch|De vingerafdruk van het X509 certificaat dat wordt gebruikt voor het genereren van HMAC op de SecondaryAccountNTLMPasswordSecret bij het gebruik van NTLM-verificatie. |
|SecondaryAccountType | tekenreeks, standaardwaarde is ""|Statisch| De secundaire AccountType van de principal-ACL de FileStoreService deelt. |
|SecondaryAccountUserName | tekenreeks, standaardwaarde is ""| Statisch|De secundaire gebruikersnaam van de principal-ACL-account de FileStoreService deelt. |
|SecondaryAccountUserPassword | SecureString, de standaardwaarde is leeg |Statisch|Het wachtwoord van de secundaire account van de principal-ACL de FileStoreService deelt. |

## <a name="healthmanager"></a>HealthManager
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |BOOL, de standaardinstelling is false |Statisch|Beleid voor evaluatie van status van cluster: inschakelen per toepassing type evalueren. |

## <a name="healthmanagerclusterhealthpolicy"></a>HealthManager/ClusterHealthPolicy
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ConsiderWarningAsError |BOOL, de standaardinstelling is false |Statisch|Beleid voor evaluatie van status van cluster: waarschuwingen worden behandeld als fouten. |
|MaxPercentUnhealthyApplications | Int, de standaardwaarde is 0 |Statisch|Beleid voor evaluatie van status van cluster: maximale percentage van de beschadigde toepassingen toegestaan voor het cluster in orde zijn. |
|MaxPercentUnhealthyNodes | Int, de standaardwaarde is 0 |Statisch|Beleid voor evaluatie van status van cluster: maximale percentage van knooppunten met slechte toegestaan voor het cluster in orde zijn. |

## <a name="healthmanagerclusterupgradehealthpolicy"></a>HealthManager/ClusterUpgradeHealthPolicy
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|MaxPercentDeltaUnhealthyNodes|Int, de standaardwaarde is 10|Statisch|Upgrade statusbeleid voor evaluatie van cluster: maximale percentage van knooppunten met slechte deltastatus toegestaan voor het cluster in orde zijn |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes|int, de standaardwaarde is 15|Statisch|Upgrade statusbeleid voor evaluatie van cluster: maximale percentage van de delta van beschadigde knooppunten in een upgradedomein is toegestaan voor het cluster in orde zijn |

## <a name="hosting"></a>Die als host fungeert
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Geheel getal, standaardinstelling is 10 |Dynamisch|Aantal keren dat het systeem nieuwe pogingen activeren voordat geeft is mislukt |
|ActivationMaxRetryInterval |Tijd in seconden, de standaardwaarde is 300 |Dynamisch|Op elke continue de activering is mislukt, probeert de activering van de maximaal ActivationMaxFailureCount opnieuw door het systeem. ActivationMaxRetryInterval Hiermee geeft u de tijdsinterval wachten voordat u opnieuw proberen na elke activering is mislukt |
|ActivationRetryBackoffInterval |Tijd in seconden, de standaardwaarde is 5 |Dynamisch|Interval voor uitstel op om de activering is mislukt; Op elke continue de activering is mislukt, probeert de activering van de maximaal de MaxActivationFailureCount opnieuw door het systeem. Het interval voor opnieuw proberen bij elke poging is een product van continue de activering is mislukt en de uitstelinterval activering. |
|ActivationTimeout| Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(180)|Dynamisch| Interval in seconden opgeven. De time-out voor activering van de toepassing. deactiveren en upgrade. |
|ApplicationHostCloseTimeout| Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch| Interval in seconden opgeven. Als Fabric afsluiten is gedetecteerd in een zelf geactiveerde processen; FabricRuntime alle replica's in het hostproces (applicationhost) van de gebruiker gesloten. Dit is de time-out voor de bewerking sluiten. |
|ApplicationUpgradeTimeout| Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(360)|Dynamisch| Interval in seconden opgeven. De time-out voor upgrade van de toepassing. Als de time-out minder is dan de deployer "ActivationTimeout" mislukken. |
|ContainerServiceArguments|tekenreeks, standaardwaarde is "-H localhost: 2375 -H npipe: / /"|Statisch|Service Fabric (SF) beheert docker-daemon (behalve op windows-clientcomputers, zoals Win10). Deze configuratie kan de gebruiker om op te geven van de aangepaste argumenten die moeten worden doorgegeven aan docker-daemon bij het starten van het. Wanneer er aangepaste argumenten zijn opgegeven, Service Fabric niet een van de andere argumenten doorgegeven aan Docker-engine, behalve '--pidfile' argument. Daarom mag geen gebruikers opgegeven '--pidfile' argument als onderdeel van de argumenten van de klant. De aangepaste argumenten moeten zorg er ook voor dat docker daemon luistert op de standaard benoemde pipe voor Windows (of Unix-domeinsocket voor Linux) voor Service Fabric om te communiceren met het.|
|CreateFabricRuntimeTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch| Interval in seconden opgeven. De time-outwaarde voor de synchronisatie FabricCreateRuntime aanroepen |
|DefaultContainerRepositoryAccountName|tekenreeks, standaardwaarde is ""|Statisch|Standaardreferenties gebruikt in plaats van de referenties die zijn opgegeven in ApplicationManifest.xml |
|DefaultContainerRepositoryPassword|tekenreeks, standaardwaarde is ""|Statisch|Standaard-wachtwoordreferenties die is gebruikt in plaats van de referenties die zijn opgegeven in ApplicationManifest.xml|
|DeploymentMaxFailureCount|Int, de standaardwaarde is 20| Dynamisch|Implementatie van de toepassing wordt opnieuw uitgevoerd voor DeploymentMaxFailureCount tijden voor het mislukken van de implementatie van toepassing op het knooppunt.| 
|DeploymentMaxRetryInterval| Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(3600)|Dynamisch| Interval in seconden opgeven. Interval voor maximum aantal nieuwe pogingen voor de implementatie. Bij elke continue fout wordt het interval voor opnieuw proberen berekend als Min (DeploymentMaxRetryInterval; Continue foutenteller * DeploymentRetryBackoffInterval) |
|DeploymentRetryBackoffInterval| Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(10)|Dynamisch|Interval in seconden opgeven. Uitstelinterval voor de implementatie mislukt. Het systeem wordt opnieuw geprobeerd de implementatie voor maximaal de MaxDeploymentFailureCount op elke continue implementatie is mislukt. Het interval voor opnieuw proberen is een product van continue implementatie is mislukt en het uitstel implementatie interval. |
|EnableActivateNoWindow| BOOL, standaard is ingesteld op FALSE|Dynamisch| De geactiveerde procedure is gemaakt op de achtergrond zonder een console. |
|EnableDockerHealthCheckIntegration|BOOL, standaard is ingesteld op TRUE|Statisch|Integratie van docker-statuscontrole gebeurtenissen met Service Fabric system health-rapport |
|EnableProcessDebugging|BOOL, standaard is ingesteld op FALSE|Dynamisch| Hiermee toepassingshosts onder foutopsporing starten |
|EndpointProviderEnabled| BOOL, standaard is ingesteld op FALSE|Statisch| Hiermee kunt beheer van resources door Fabric eindpunt. Vereist specificatie van de begin- en poortbereik voor toepassingen in FabricNode. |
|FabricContainerAppsEnabled| BOOL, standaard is ingesteld op FALSE|Statisch| |
|FirewallPolicyEnabled|BOOL, standaard is ingesteld op FALSE|Statisch| Hiermee firewall-poorten voor eindpunt resources te openen met expliciete poorten die zijn opgegeven in het ServiceManifest |
|GetCodePackageActivationContextTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch|Interval in seconden opgeven. De time-outwaarde voor de CodePackageActivationContext-aanroepen. Dit is niet van toepassing op ad-hoc-services. |
|IPProviderEnabled|BOOL, standaard is ingesteld op FALSE|Statisch|Hiermee is het beheer van IP-adressen. |
|IsDefaultContainerRepositoryPasswordEncrypted|BOOL, standaard is ingesteld op FALSE|Statisch|Of de DefaultContainerRepositoryPassword is versleuteld of niet.|
|LinuxExternalExecutablePath|tekenreeks, standaardwaarde is ' / usr/bin / " |Statisch|De primaire-map van externe uitvoerbare opdrachten op het knooppunt.|
|NTLMAuthenticationEnabled|BOOL, standaard is ingesteld op FALSE|Statisch| Hiermee schakelt u ondersteuning voor het gebruik van NTLM door de codepakketten die uitgevoerd als andere gebruikers worden zodat de processen tussen computers veilig kunnen communiceren. |
|NTLMAuthenticationPasswordSecret|SecureString, de standaardwaarde is Common::SecureString("")|Statisch|Is dat een versleutelde heeft die wordt gebruikt voor het genereren van het wachtwoord voor NTLM-gebruikers. Moet worden ingesteld als NTLMAuthenticationEnabled ingesteld op true is. Gevalideerd door de implementatie. |
|NTLMSecurityUsersByX509CommonNamesRefreshInterval|Interval, de standaardwaarde is Common::TimeSpan::FromMinutes(3)|Dynamisch|Interval in seconden opgeven. Omgeving-specifieke instellingen voor het periodiek interval op welke Hosting scant op nieuwe certificaten worden gebruikt voor de configuratie van FileStoreService NTLM. |
|NTLMSecurityUsersByX509CommonNamesRefreshTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromMinutes(4)|Dynamisch| Interval in seconden opgeven. De time-out voor het configureren van NTLM-gebruikers met behulp van de algemene naam van het certificaat. De NTLM-gebruikers nodig zijn voor FileStoreService shares. |
|RegisterCodePackageHostTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch| Interval in seconden opgeven. De time-outwaarde voor de FabricRegisterCodePackageHost sync-aanroep. Dit is van toepassing voor alleen meerdere code pakket application hosts, zoals FWP |
|RequestTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(30)|Dynamisch| Interval in seconden opgeven. Hiermee wordt de time-out voor de communicatie tussen de toepassingshost en Fabric-proces voor verschillende hosting gerelateerde bewerkingen, zoals factory registratie; van de gebruiker Runtime-registratie. |
|RunAsPolicyEnabled| BOOL, standaard is ingesteld op FALSE|Statisch| Hiermee schakelt u de codepakketten uitvoeren als lokale gebruiker dan de gebruiker onder welke infrastructuur proces wordt uitgevoerd. Om in te schakelen van dit beleid de Fabric moet worden uitgevoerd als systeem of als de gebruiker die SeAssignPrimaryTokenPrivilege heeft. |
|ServiceFactoryRegistrationTimeout| Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch|Interval in seconden opgeven. De time-outwaarde voor de synchronisatie registreren (Stateless/Stateful) ServiceFactory aanroep |
|ServiceTypeDisableFailureThreshold |Geheel getal, standaard is 1 |Dynamisch|Dit is de drempelwaarde voor het aantal storingen waarna FailoverManager (FM), wordt een melding voor het type van de service op het knooppunt uitschakelen en probeer een ander knooppunt voor plaatsing. |
|ServiceTypeDisableGraceInterval|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(30)|Dynamisch|Interval in seconden opgeven. Tijdsinterval waarna type van de service kan worden uitgeschakeld |
|ServiceTypeRegistrationTimeout |Tijd in seconden, de standaardwaarde is 300 |Dynamisch|Maximale tijd voor het ServiceType worden geregistreerd bij fabric toegestaan |
|UseContainerServiceArguments|BOOL, standaard is ingesteld op TRUE|Statisch|Deze configuratie wordt aangegeven die als host fungeert voor de argumenten doorgeven (opgegeven in configuratie ContainerServiceArguments) gaat u verder met docker-daemon.|

## <a name="httpgateway"></a>HttpGateway
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ActiveListeners |Uint, de standaardwaarde is 50 |Statisch| Het aantal leesbewerkingen en op de server HTTP-wachtrij plaatsen. Hiermee bepaalt u het aantal gelijktijdige aanvragen op dat door de HttpGateway kan worden voldaan. |
|HttpGatewayHealthReportSendInterval |Tijd in seconden, de standaardwaarde is 30 |Statisch|Interval in seconden opgeven. Het interval waarmee de Http-Gateway samengevoegde status verzendt rapporteert de status Manager. |
|IsEnabled|BOOL, de standaardinstelling is false |Statisch| / Schakelt de HttpGateway. HttpGateway is standaard uitgeschakeld. |
|MaxEntityBodySize |Uint, de standaardwaarde is 4194304 |Dynamisch|Geeft de maximale grootte van de instantie die een http-aanvraag kan worden verwacht. Standaardwaarde is 4MB. Httpgateway een aanvraag mislukt als er een instantie van de grootte van > deze waarde. Minimale lezen chunkgrootte is 4096 bytes. Zodat de sleutel moet > = 4096. |

## <a name="imagestoreclient"></a>ImageStoreClient
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ClientCopyTimeout | Tijd in seconden, de standaardwaarde is 1800 |Dynamisch| Interval in seconden opgeven. Time-outwaarde voor de aanvraag op het hoogste niveau kopiëren naar afbeelding Store-Service. |
|ClientDefaultTimeout | Tijd in seconden, wordt standaard 180 |Dynamisch| Interval in seconden opgeven. Time-outwaarde voor alle aanvragen voor niet-uploaden/niet-downloaden (bijvoorbeeld bestaat, verwijderen) naar afbeelding Store-Service. |
|ClientDownloadTimeout | Tijd in seconden, de standaardwaarde is 1800 |Dynamisch| Interval in seconden opgeven. Time-outwaarde voor het van het hoogste niveau downloadaanvraag Image Store-Service. |
|ClientListTimeout | Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Interval in seconden opgeven. Time-outwaarde voor de aanvraag op het hoogste niveau lijst met afbeelding Store-Service. |
|ClientUploadTimeout |Tijd in seconden, de standaardwaarde is 1800 |Dynamisch|Interval in seconden opgeven. Time-outwaarde voor de uploadaanvraag op het hoogste niveau naar afbeelding Store-Service. |

## <a name="imagestoreservice"></a>ImageStoreService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|Ingeschakeld |BOOL, de standaardinstelling is false |Statisch|De vlag ingeschakeld voor ImageStoreService. Standaard: false |
|MinReplicaSetSize | Int, de standaardwaarde is 3 |Statisch|De MinReplicaSetSize voor ImageStoreService. |
|PlacementConstraints | tekenreeks, standaardwaarde is "" |Statisch| De PlacementConstraints voor ImageStoreService. |
|QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Statisch| Interval in seconden opgeven. De QuorumLossWaitDuration voor ImageStoreService. |
|ReplicaRestartWaitDuration | Tijd in seconden, is standaard 60,0 * 30 |Statisch|Interval in seconden opgeven. De ReplicaRestartWaitDuration voor ImageStoreService. |
|StandByReplicaKeepDuration | Tijd in seconden, de standaardwaarde is 3600.0 * 2 |Statisch| Interval in seconden opgeven. De StandByReplicaKeepDuration voor ImageStoreService. |
|TargetReplicaSetSize | Int, de standaardwaarde is 7 |Statisch|De TargetReplicaSetSize voor ImageStoreService. |

## <a name="ktllogger"></a>KtlLogger
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|AutomaticMemoryConfiguration |Int, de standaardwaarde is 1 |Dynamisch|Markering waarmee wordt aangegeven als de instellingen voor geheugen moeten worden automatisch en dynamisch geconfigureerd. Als nul wordt de configuratie-instellingen van het geheugen rechtstreeks worden gebruikt en niet te wijzigen op basis van systeem voorwaarden. Als een worden de instellingen voor geheugen worden automatisch geconfigureerd en kunnen worden gewijzigd op basis van systeem voorwaarden. |
|MaximumDestagingWriteOutstandingInKB | Int, de standaardwaarde is 0 |Dynamisch|Het aantal KB tot de gedeelde aanmelden om door te gaan voor het toegewezen logboek toestaan. Gebruik 0 om aan te geven geen limiet.
|SharedLogId |tekenreeks, standaardwaarde is "" |Statisch|De unieke guid voor gedeelde log-container. Gebruik ' ' als standaardpad onder fabric-gegevenshoofdmap. |
|SharedLogPath |tekenreeks, standaardwaarde is "" |Statisch|Pad en de naam naar de locatie voor gedeelde log-container. Gebruik ' ' voor het gebruik van standaardpad onder fabric-gegevenshoofdmap. |
|SharedLogSizeInMB |Int, de standaardwaarde is 8192 liggen |Statisch|Het aantal MB om toe te wijzen in de gedeelde log-container. |
|WriteBufferMemoryPoolMaximumInKB | Int, de standaardwaarde is 0 |Dynamisch|Het aantal KB om toe te staan de schrijven geheugen buffergroep tot groeien. Gebruik 0 om aan te geven geen limiet. |
|WriteBufferMemoryPoolMinimumInKB |Int, de standaardwaarde is 8388608 |Dynamisch|Het aantal KB in eerste instantie voor het schrijven voor buffergroep in het geheugen toewijzen. Gebruik 0 om aan te geven geen limiet standaard moet consistent zijn met SharedLogSizeInMB hieronder. |

## <a name="management"></a>Beheer
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|AzureStorageMaxConnections | Int, de standaardwaarde is 5000 |Dynamisch|Het maximale aantal gelijktijdige verbindingen naar azure storage. |
|AzureStorageMaxWorkerThreads | int, de standaardwaarde is 25 |Dynamisch|Het maximum aantal werkthreads parallel. |
|AzureStorageOperationTimeout | Tijd in seconden, de standaardwaarde is 6000 |Dynamisch|Interval in seconden opgeven. Time-out voor xstore bewerking is voltooid. |
|CleanupApplicationPackageOnProvisionSuccess|BOOL, standaard is ingesteld op FALSE |Dynamisch|Deze configuratie of het automatisch opschonen van het toepassingspakket op geslaagde inrichten uitgeschakeld. |
|DisableChecksumValidation | BOOL, de standaardinstelling is false |Statisch| Deze configuratie kan we controlesomvalidatie tijdens het inrichten van de toepassing uit te schakelen. |
|DisableServerSideCopy | BOOL, de standaardinstelling is false |Statisch|Deze configuratie- of serverzijde kopie van het toepassingspakket op de ImageStore tijdens het inrichten van de toepassing uitgeschakeld. |
|ImageCachingEnabled | BOOL, de standaardinstelling is true |Statisch|Deze configuratie kan we in- of uitschakelen in cache opslaan. |
|ImageStoreConnectionString |SecureString |Statisch|De verbindingsreeks naar de hoofdmap voor ImageStore. |
|ImageStoreMinimumTransferBPS | Int, standaard is 1024 |Dynamisch|De minimale overdrachtssnelheid tussen het cluster en de ImageStore. Deze waarde wordt gebruikt om te bepalen van de time-out bij het openen van de externe ImageStore. Deze waarde alleen wijzigen als de latentie tussen het cluster en de ImageStore waarmee meer tijd voor het cluster te downloaden van de externe ImageStore hoog is. |

## <a name="metricactivitythresholds"></a>MetricActivityThresholds
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, is standaard ingesteld op geen|Dynamisch|Bepaalt de set MetricActivityThresholds voor de metrische gegevens in het cluster. Taakverdeling werkt als maxNodeLoad groter is dan MetricActivityThresholds. Defragmentatie metrische gegevens voor deze bepaalt de hoeveelheid load dat gelijk is aan of onder die Service Fabric worden beschouwd als het knooppunt leeg zijn |

## <a name="metricbalancingthresholds"></a>MetricBalancingThresholds
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, is standaard ingesteld op geen|Dynamisch|Bepaalt de set MetricBalancingThresholds voor de metrische gegevens in het cluster. Taakverdeling werkt als maxNodeLoad/minNodeLoad groter is dan MetricBalancingThresholds. Defragmentatie werken als maxNodeLoad/minNodeLoad in ten minste één FD of UD kleiner dan MetricBalancingThresholds is. |

## <a name="namingservice"></a>NamingService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|GatewayServiceDescriptionCacheLimit |Int, de standaardwaarde is 0 |Statisch|Het maximale aantal vermeldingen behouden in de cache LRU service beschrijving bij de naamgeving van Gateway (ingesteld op 0 om geen limiet). |
|MaxClientConnections |Int, de standaardwaarde is 1000 |Dynamisch|De maximaal toegestane aantal clientverbindingen per gateway. |
|MaxFileOperationTimeout |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Interval in seconden opgeven. De maximale time-out die is toegestaan voor bestandsbewerking store-service. Aanvragen op te geven een hogere time-out worden geweigerd. |
|MaxIndexedEmptyPartitions |Int, de standaardwaarde is 1000 |Dynamisch|Het maximum aantal lege partities die blijft geïndexeerd in de cache van de melding voor het herstellen van verbindingen clients synchroniseren. Alle lege partities boven dit nummer wordt verwijderd uit de index in oplopende volgorde van lookup-versie. Nog steeds kunt opnieuw verbinden van clients synchroniseren en ontvangen van updates gemist lege partitie; maar het synchronisatieprotocol dat wordt duurder. |
|MaxMessageSize |Int, de standaardwaarde is 4\*1024\*1024 |Statisch|De maximale berichtgrootte voor clientcommunicatie van knooppunt bij het gebruik van namen. DOS-aanval verlichting; standaardwaarde is 4MB. |
|MaxNamingServiceHealthReports | Int, de standaardwaarde is 10 |Dynamisch|Het maximale aantal trage bewerkingen waarin Naming-service in één keer rapporten die niet in orde. Als u 0; alle trage bewerkingen worden verzonden. |
|MaxOperationTimeout |Tijd in seconden, de standaardwaarde is 600 |Dynamisch|Interval in seconden opgeven. De maximale time-out die is toegestaan voor de clientbewerkingen. Aanvragen op te geven een hogere time-out worden geweigerd. |
|MaxOutstandingNotificationsPerClient |Int, de standaardwaarde is 1000 |Dynamisch|Het maximum aantal openstaande meldingen voordat de clientregistratie van een is geforceerd gesloten door de gateway. |
|MinReplicaSetSize | Int, de standaardwaarde is 3 |Niet toegestaan| Het minimum aantal Naming-Service-replica's die nodig zijn om te schrijven in om een update te voltooien. Als er minder replica's dan deze actief zijn in het systeem het systeem betrouwbaarheid weigert updates naar de Naming-Service-Store totdat de replica's zijn hersteld. Deze waarde moet groter dan de TargetReplicaSetSize nooit. |
|PartitionCount |Int, de standaardwaarde is 3 |Niet toegestaan|Het aantal partities van de Naming-Service worden opgeslagen om te worden gemaakt. Elke partitie is eigenaar van een enkele partitie-sleutel die overeenkomt met de index; dit het geval is partitiesleutels [0; PartitionCount) bestaat. De schaal die de Naming-Service op uitvoeren kunt door te verminderen van de gemiddelde hoeveelheid gegevens die zijn opgeslagen door de replica van een back-ups maken van het aantal Naming-Service partities toeneemt verhoogt ingesteld. terwijl de kosten van de toename in het gebruik van resources (sinds PartitionCount * ReplicaSetSize service-replica's moeten worden onderhouden).|
|PlacementConstraints | tekenreeks, standaardwaarde is "" |Niet toegestaan| Plaatsing van de beperking voor de Naming-Service. |
|QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Niet toegestaan| Interval in seconden opgeven. Wanneer een Naming-Service krijgt de quorumverlies; Deze timer wordt gestart. Wanneer het verloopt de FM Houd rekening met de omlaag replica's als verloren. en proberen om het quorum te herstellen. Niet op dat dit kan leiden tot verlies van gegevens. |
|RepairInterval | Tijd in seconden, de standaardwaarde is 5 |Statisch| Interval in seconden opgeven. Interval waarin de naamgevingsconventie inconsistentie tussen de eigenaar van de instantie en de van de naameigenaar herstellen wordt gestart. |
|ReplicaRestartWaitDuration | Tijd in seconden, wordt standaard (60,0 * 30)|Niet toegestaan| Interval in seconden opgeven. Wanneer een replica Naming-Service wordt uitgeschakeld; Deze timer wordt gestart. Wanneer het verloopt de FM beginnen met het vervangen van de replica's die niet beschikbaar zijn (dit wordt niet nog rekening gehouden met deze verloren). |
|ServiceDescriptionCacheLimit | Int, de standaardwaarde is 0 |Statisch| Het maximale aantal vermeldingen behouden in de cache LRU service beschrijving van de Naming Store-Service (ingesteld op 0 om geen limiet). |
|ServiceNotificationTimeout |Tijd in seconden, de standaardwaarde is 30 |Dynamisch|Interval in seconden opgeven. De time-out bij het leveren van servicemeldingen naar de client gebruikt. |
|StandByReplicaKeepDuration | Tijd in seconden, de standaardwaarde is 3600.0 * 2 |Niet toegestaan| Interval in seconden opgeven. Wanneer een replica Naming-Service terugkeren van een status down; mogelijk is deze al vervangen. Deze timer bepaalt hoe lang de FM de standby-replica blijft bewaard voordat deze worden verwijderd. |
|TargetReplicaSetSize |Int, de standaardwaarde is 7 |Niet toegestaan|Hiermee stelt u het aantal replica's voor elke partitie van de store Naming-Service. Het aantal replicasets verhoogt, worden het niveau van betrouwbaarheid van de informatie in de Store van de Naming-Service; de wijziging dat als gevolg van knooppuntfouten; de gegevens gaan verloren verlagen het duurt om uit te voeren van updates voor de naamgeving gegevens terwijl de kosten van toegenomen belasting op de Windows Fabric en de hoeveelheid tijd.|

## <a name="nodebufferpercentage"></a>NodeBufferPercentage
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, is standaard ingesteld op geen|Dynamisch|Percentage van knooppunt-capaciteit per metrische naam; Als een buffer gebruikt om te voorkomen dat een gratis plaats op een knooppunt voor het geval van failover. |

## <a name="nodecapacities"></a>NodeCapacities
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup |NodeCapacityCollectionMap |Statisch|Een verzameling van knooppuntcapaciteiten voor verschillende metrische gegevens. |

## <a name="nodedomainids"></a>NodeDomainIds
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup |NodeFaultDomainIdCollection |Statisch|Beschrijving van de domeinen met fouten die een knooppunt behoort. Het foutdomein wordt gedefinieerd via een URI die de locatie van het knooppunt in het datacenter beschrijft.  Fout met betrekking tot domein URI's zijn van de indeling fd: / fd/gevolgd door een URI-pad-segment.|
|UpgradeDomainId |tekenreeks, standaardwaarde is "" |Statisch|Hierin wordt beschreven in het upgradedomein dat een knooppunt behoort. |

## <a name="nodeproperties"></a>NodeProperties
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup |NodePropertyCollectionMap |Statisch|Een verzameling van reeks sleutel / waarde-paren voor eigenschappen van het knooppunt. |

## <a name="paas"></a>Paas
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ClusterId |tekenreeks, standaardwaarde is "" |Niet toegestaan|X509 certificaatarchief door fabric gebruikt voor de beveiliging van de configuratie. |

## <a name="performancecounterlocalstore"></a>PerformanceCounterLocalStore
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|Prestatiemeteritems |Reeks | Dynamisch |Door komma's gescheiden lijst met te verzamelen prestatiemeteritems. |
|IsEnabled |BOOL, de standaardinstelling is true | Dynamisch |Vlag geeft aan of het verzamelen van prestatiemeteritems op het lokale knooppunt is ingeschakeld. |
|MaxCounterBinaryFileSizeInMB |Int, de standaardwaarde is 1 | Dynamisch |Maximale grootte (in MB) voor elk prestaties teller binaire bestand. |
|NewCounterBinaryFileCreationIntervalInMinutes |Int, de standaardwaarde is 10 | Dynamisch |Maximaal interval (in seconden) waarna een nieuw prestaties teller binaire bestand is gemaakt. |
|SamplingIntervalInSeconds |Int, de standaardwaarde is 60 | Dynamisch |Steekproefinterval voor prestatiemeteritems die worden verzameld. |

## <a name="placementandloadbalancing"></a>PlacementAndLoadBalancing
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|AffinityConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Hiermee bepaalt u de prioriteit van de affiniteit beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|ApplicationCapacityConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Hiermee bepaalt u de prioriteit van capaciteit beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|AutoDetectAvailableResources|BOOL, standaard is ingesteld op TRUE|Statisch|Deze configuratie automatische detectie van beschikbare resources op knooppunt (CPU en geheugen) wordt geactiveerd wanneer deze configuratie is ingesteld op true: we lezen echte capaciteiten en corrigeer deze indien de gebruiker opgegeven beschadigde knooppuntcapaciteit of hebt gedefinieerd ze helemaal als deze configuratie is ingesteld op false - zullen we  een waarschuwing dat die door de gebruiker opgegeven beschadigde knooppuntcapaciteit; traceren maar we zullen niet corrigeren. wat betekent dat de gebruiker wil dat de capaciteit die is opgegeven als > dan op het knooppunt is echt of als capaciteitswaarden niet-gedefinieerde zijn; wordt ervan uitgegaan dat onbeperkte capaciteit |
|BalancingDelayAfterNewNode | Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Interval in seconden opgeven. Start niet balancing activiteiten binnen deze periode na het toevoegen van een nieuw knooppunt. |
|BalancingDelayAfterNodeDown | Tijd in seconden, de standaardwaarde is 120 |Dynamisch|Interval in seconden opgeven. Start niet balancing activiteiten binnen deze periode na een knooppunt is uitgevallen gebeurtenis. |
|CapacityConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Hiermee bepaalt u de prioriteit van capaciteit beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, de standaardwaarde is 20 | Dynamisch|Hiermee definieert u het aantal opeenvolgende keren dat ResourceBalancer uitgegeven verplaatsingen van het type zijn verwijderd voordat u diagnostische gegevens worden uitgevoerd en de van gezondheidswaarschuwingen worden verzonden. Negatieve: Er zijn geen waarschuwingen verzonden onder deze voorwaarde. |
|ConstraintFixPartialDelayAfterNewNode | Tijd in seconden, de standaardwaarde is 120 |Dynamisch| Interval in seconden opgeven. DDo FaultDomain niet oplossen en schendingen van plaatsingsbeperkingen UpgradeDomain binnen deze periode na het toevoegen van een nieuw knooppunt. |
|ConstraintFixPartialDelayAfterNodeDown | Tijd in seconden, de standaardwaarde is 120 |Dynamisch| Interval in seconden opgeven. Niet repareren FaultDomain en UpgradeDomain schendingen van plaatsingsbeperkingen doen binnen deze periode na een knooppunt is uitgevallen gebeurtenis. |
|ConstraintViolationHealthReportLimit | Int, de standaardwaarde is 50 |Dynamisch| Hiermee definieert u het aantal keren beperking schenden replica is permanent niet-opgeloste moet voor de diagnostische gegevens worden uitgevoerd en systeemstatusrapporten worden uitgezonden. |
|DetailedConstraintViolationHealthReportLimit | Int, standaard is 200 |Dynamisch| Hiermee definieert u het aantal keren beperking schenden replica moet permanent niet-opgeloste voor diagnostische gegevens worden uitgevoerd en gedetailleerde rapporten worden uitgezonden status heeft. |
|DetailedDiagnosticsInfoListLimit | int, de standaardwaarde is 15 |Dynamisch| Definieert het aantal diagnose-items (met gedetailleerde informatie) per beperking om op te nemen voor afkapping in diagnostische gegevens.|
|DetailedNodeListLimit | int, de standaardwaarde is 15 |Dynamisch| Hiermee definieert u het aantal knooppunten per beperking om op te nemen voor afkapping in de rapporten met niet-geplaatste Replica. |
|DetailedPartitionListLimit | int, de standaardwaarde is 15 |Dynamisch| Hiermee definieert u het aantal partities per diagnostische vermelding voor een beperking voor om op te nemen voor afkapping in diagnostische gegevens. |
|DetailedVerboseHealthReportLimit | Int, standaard is 200 | Dynamisch|Hiermee definieert u het aantal keren dat die een niet-geplaatste replica niet permanent-geplaatste is voordat gedetailleerde statusrapporten worden uitgezonden. |
|FaultDomainConstraintPriority | Int, de standaardwaarde is 0 |Dynamisch| Hiermee bepaalt u de prioriteit van fout met betrekking tot domeinbeperking: 0: vaste; 1: zachte; negatieve: negeren. |
|GlobalMovementThrottleCountingInterval | Tijd in seconden, de standaardwaarde is 600 |Statisch| Interval in seconden opgeven. De lengte van het afgelopen interval waarvoor om bij te houden per domein replica verplaatsingen (gebruikt samen met GlobalMovementThrottleThreshold) geven. Kan worden ingesteld op 0 om te negeren globale beperking kan worden overgeslagen. |
|GlobalMovementThrottleThreshold | Uint, de standaardwaarde is 1000 |Dynamisch| Maximum aantal verplaatsingen van het type toegestaan in de fase taakverdeling in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval. |
|GlobalMovementThrottleThresholdForBalancing | Uint, de standaardwaarde is 0 | Dynamisch|Maximum aantal verplaatsingen van het type toegestaan in de fase taakverdeling in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval. 0 geeft aan dat er geen limiet. |
|GlobalMovementThrottleThresholdForPlacement | Uint, de standaardwaarde is 0 |Dynamisch| Maximum aantal verplaatsingen van het type toegestaan in de fase voor plaatsing in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval.0 geeft aan dat er geen limiet.|
|GlobalMovementThrottleThresholdPercentage|dubbel, de standaardwaarde is 0|Dynamisch|Maximum aantal verplaatsingen van het totale aantal type toegestaan in taakverdeling en plaatsing fasen (uitgedrukt als percentage van totale aantal replica's in het cluster) in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval. 0 geeft aan dat er geen limiet. Als dit en GlobalMovementThrottleThreshold zijn opgegeven. vervolgens wordt meer conservatieve limiet gebruikt.|
|GlobalMovementThrottleThresholdPercentageForBalancing|dubbel, de standaardwaarde is 0|Dynamisch|Maximum aantal verplaatsingen van het type toegestaan in Balancing fase (uitgedrukt als percentage van totaal aantal replica's in de functieaanroep PLB) in het afgelopen interval aangegeven door GlobalMovementThrottleCountingInterval. 0 geeft aan dat er geen limiet. Als dit en GlobalMovementThrottleThresholdForBalancing zijn opgegeven. vervolgens wordt meer conservatieve limiet gebruikt.|
|InBuildThrottlingAssociatedMetric | tekenreeks, standaardwaarde is "" |Statisch| De bijbehorende metrische naam op voor deze beperking. |
|InBuildThrottlingEnabled | BOOL, de standaardinstelling is false |Dynamisch| Bepalen of de in-build-beperking is ingeschakeld. |
|InBuildThrottlingGlobalMaxValue | Int, de standaardwaarde is 0 |Dynamisch|Het maximale aantal-build-replica's wereldwijd toegestaan. |
|InterruptBalancingForAllFailoverUnitUpdates | BOOL, de standaardinstelling is false | Dynamisch|Bepaalt of elk type failover-eenheid update moet snel interrupt of traag taakverdeling worden uitgevoerd. Met de opgegeven 'false' taakverdeling worden uitgevoerd, worden onderbroken als FailoverUnit: is gemaakt/verwijderd. ontbrekende replica's; locatie van de primaire replica of gewijzigde aantal replica's gewijzigd. Taakverdeling worden uitgevoerd, worden niet onderbroken in andere gevallen - als FailoverUnit: heeft extra replica's; een replica-vlag; gewijzigd alleen de versie van de partitie of alle andere gevallen gewijzigd. |
|MinConstraintCheckInterval | Tijd in seconden, de standaardwaarde is 1 |Dynamisch| Interval in seconden opgeven. Hiermee definieert u de minimale hoeveelheid tijd die moet worden gewacht voordat er twee opeenvolgende beperking controleren afgerond. |
|MinLoadBalancingInterval | Tijd in seconden, de standaardwaarde is 5 |Dynamisch| Interval in seconden opgeven. Hiermee definieert u de minimale hoeveelheid tijd dat moet worden gewacht voordat u twee opeenvolgende taakverdeling afgerond. |
|MinPlacementInterval | Tijd in seconden, de standaardwaarde is 1 |Dynamisch| Interval in seconden opgeven. Hiermee definieert u de minimale hoeveelheid tijd dat moet worden gewacht voordat u twee opeenvolgende plaatsing afgerond. |
|MoveExistingReplicaForPlacement | BOOL, de standaardinstelling is true |Dynamisch|Instellen dat bepaalt wanneer bestaande replica verplaatsen tijdens de plaatsing. |
|MovementPerPartitionThrottleCountingInterval | Tijd in seconden, de standaardwaarde is 600 |Statisch| Interval in seconden opgeven. De lengte van het afgelopen interval voor die voor het bijhouden van bewegingen van de replica voor elke partitie (gebruikt samen met MovementPerPartitionThrottleThreshold) geven. |
|MovementPerPartitionThrottleThreshold | Uint, de standaardwaarde is 50 |Dynamisch| Er is geen verkeer netwerktaakverdeling met betrekking tot wordt uitgevoerd voor een partitie als het aantal gerelateerde verplaatsingen van het type voor replica's van de betreffende partitie balancing heeft bereikt of overschreden MovementPerFailoverUnitThrottleThreshold in het afgelopen interval aangegeven door MovementPerPartitionThrottleCountingInterval. |
|MoveParentToFixAffinityViolation | BOOL, de standaardinstelling is false |Dynamisch| Instelling waarmee wordt bepaald of bovenliggende replica's op te lossen affiniteit beperkingen kunnen worden verwijderd.|
|PartiallyPlaceServices | BOOL, de standaardinstelling is true |Dynamisch| Bepaalt als alle service-replica's in het cluster worden geplaatst 'Alles of niets' opgegeven beperkt geschikt knooppunten voor hen.|
|PlaceChildWithoutParent | BOOL, de standaardinstelling is true | Dynamisch|Instellen dat bepaalt als onderliggende replica-service kan worden geplaatst als er geen bovenliggende replica actief is. |
|PlacementConstraintPriority | Int, de standaardwaarde is 0 | Dynamisch|Hiermee bepaalt u de prioriteit van plaatsing beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|PlacementConstraintValidationCacheSize | Int, standaard is 10000 |Dynamisch| Beperkt de grootte van de tabel die wordt gebruikt voor de snelle validatie en van plaatsing beperking expressies in de cache. |
|PlacementSearchTimeout | Tijd in seconden, de standaardwaarde is 0,5 |Dynamisch| Interval in seconden opgeven. Bij het plaatsen van services; zoeken naar maximaal deze lange alvorens een resultaat. |
|PLBRefreshGap | Tijd in seconden, de standaardwaarde is 1 |Dynamisch| Interval in seconden opgeven. Hiermee definieert u de minimale hoeveelheid tijd die moet worden gewacht voordat de functieaanroep PLB wordt vernieuwd status opnieuw. |
|PreferredLocationConstraintPriority | Int, de standaardwaarde is 2| Dynamisch|Hiermee bepaalt u de prioriteit van de beperking van de gewenste locatie: 0: vaste; 1: zachte; 2: optimalisatie; negatieve: negeren |
|PreventTransientOvercommit | BOOL, de standaardinstelling is false | Dynamisch|Hiermee bepaalt u moet PLB onmiddellijk worden geteld voor de resources die zullen worden vrijgemaakt door de gestarte verplaatst. Standaard; Basis voor PLB verplaatsen uit tot stand kan brengen en verplaatsen in op hetzelfde knooppunt wat tot tijdelijke leiden kan overcommit. Deze parameter instellen op ' True ' wordt voorkomen dat deze soorten van overcommits en defragmentatie op aanvraag (ook wel placementWithMove) die worden uitgeschakeld. |
|ScaleoutCountConstraintPriority | Int, de standaardwaarde is 0 |Dynamisch| Hiermee bepaalt u de prioriteit van scaleout aantal beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|SwapPrimaryThrottlingAssociatedMetric | tekenreeks, standaardwaarde is ""|Statisch| De bijbehorende metrische naam op voor deze beperking. |
|SwapPrimaryThrottlingEnabled | BOOL, de standaardinstelling is false|Dynamisch| Bepalen of het swap-primaire beperking is ingeschakeld. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, de standaardwaarde is 0 |Dynamisch| Het maximale aantal swap-primaire-replica's wereldwijd toegestaan. |
|TraceCRMReasons |BOOL, de standaardinstelling is true |Dynamisch|Hiermee bepaalt u of u wilt traceren redenen voor CRM verplaatsingen van het type verleend aan het kanaal operationele gebeurtenissen. |
|UpgradeDomainConstraintPriority | Int, de standaardwaarde is 1| Dynamisch|Hiermee bepaalt u de prioriteit van het upgradedomein beperking: 0: vaste; 1: zachte; negatieve: negeren. |
|UseMoveCostReports | BOOL, de standaardinstelling is false | Dynamisch|Hiermee geeft u de LB kostenelementen van het scoringfunctie; negeren resulterende potentieel groter aantal verplaatsingen voor beter met gelijke taakverdeling plaatsing. |
|UseSeparateSecondaryLoad | BOOL, de standaardinstelling is true | Dynamisch|Instellen dat bepaalt als andere secundaire load gebruiken. |
|ValidatePlacementConstraint | BOOL, de standaardinstelling is true |Dynamisch| Hiermee geeft u op of de expressie PlacementConstraint voor een service wordt gevalideerd als een service ServiceDescription wordt bijgewerkt. |
|VerboseHealthReportLimit | Int, de standaardwaarde is 20 | Dynamisch|Hiermee definieert u het aantal keren dat die een replica heeft niet-geplaatste gaan voordat een status-Waarschuwing voor deze wordt gerapporteerd (als de gezondheid van uitgebreide rapportage is ingeschakeld). |

## <a name="reconfigurationagent"></a>ReconfigurationAgent
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ApplicationUpgradeMaxReplicaCloseDuration | Tijd in seconden, de standaardwaarde is 900 |Dynamisch|Interval in seconden opgeven. De tijdsduur waarvoor het systeem wacht voordat het beëindigen van servicehosts die replica's hebben die zitten vast in sluit tijdens de Upgrade van de toepassing.|
|FabricUpgradeMaxReplicaCloseDuration | Tijd in seconden, de standaardwaarde is 900 |Dynamisch| Interval in seconden opgeven. De tijdsduur waarvoor het systeem wacht voordat het beëindigen van servicehosts die replica's hebben die zitten vast in sluit tijdens de fabric-upgrade. |
|GracefulReplicaShutdownMaxDuration|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(120)|Dynamisch|Interval in seconden opgeven. De tijdsduur waarvoor het systeem wacht voordat het beëindigen van servicehosts die replica's hebben die zitten vast in sluiten. Als deze waarde is ingesteld op 0, worden replica's niet gevraagd om te sluiten.|
|NodeDeactivationMaxReplicaCloseDuration | Tijd in seconden, de standaardwaarde is 900 |Dynamisch|Interval in seconden opgeven. De tijdsduur waarvoor het systeem wacht voordat het beëindigen van servicehosts die replica's hebben die zitten vast in sluiten tijdens knooppunt deactiveren. |
|PeriodicApiSlowTraceInterval | Tijd in seconden, de standaardwaarde is 5 minuten |Dynamisch| Interval in seconden opgeven. PeriodicApiSlowTraceInterval definieert het interval waarover wordt traag API-aanroepen worden ververst door de API-monitor. |
|ReplicaChangeRoleFailureRestartThreshold|Int, de standaardwaarde is 10|Dynamisch| Geheel getal. Geef het aantal API-fouten tijdens de primaire promotie waarna automatisch risicobeperking actie (replica opnieuw is opgestart) wordt toegepast. |
|ReplicaChangeRoleFailureWarningReportThreshold|int, de standaardwaarde is 2147483647|Dynamisch| Geheel getal. Geef het aantal API-fouten tijdens primaire promotie waarna statusrapport waarschuwing wordt weergegeven.|
|ServiceApiHealthDuration | Tijd in seconden, de standaardwaarde is 30 minuten |Dynamisch| Interval in seconden opgeven. ServiceApiHealthDuration wordt gedefinieerd hoe lang moet we wachten voor een service-API om uit te voeren voordat we deze niet in orde. |
|ServiceReconfigurationApiHealthDuration | Tijd in seconden, de standaardwaarde is 30 |Dynamisch| Interval in seconden opgeven. ServiceReconfigurationApiHealthDuration wordt gedefinieerd hoe lang moet we wachten voor een service-API om uit te voeren voordat we niet in orde. Dit geldt voor API-aanroepen die invloed hebben op beschikbaarheid.|

## <a name="replication"></a>Replicatie
| **Parameter** | **Toegestane waarden** | **Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval|Interval, de standaardwaarde is Common::TimeSpan::FromMilliseconds(15)|Statisch|Interval in seconden opgeven. Bepaalt de hoeveelheid tijd dat de replicatie moet wachten na de ontvangst van een bewerking voordat u een bevestiging terug te sturen. Andere bewerkingen die tijdens deze periode is ontvangen, wordt hun bevestigingen verzonden in een enkel bericht te reduceren netwerkverkeer, maar mogelijk vermindering van de doorvoer van de replicatie -> hebben.|
|MaxCopyQueueSize|uint, standaard is 1024|Statisch|Dit is de maximale waarde definieert de aanvankelijke grootte van de wachtrij waarmee replicatiebewerkingen worden bewaard. Houd er rekening mee dat het moet een macht van 2. Als tijdens runtime de wachtrij voor deze bewerking grootte toeneemt worden beperkt tussen de primaire en secundaire distributeurs.|
|MaxPrimaryReplicationQueueMemorySize|Uint, de standaardwaarde is 0|Statisch|Dit is de maximale waarde van de wachtrij primaire replicatie in bytes.|
|MaxPrimaryReplicationQueueSize|uint, standaard is 1024|Statisch|Dit is het maximum aantal bewerkingen die kunnen bestaan in de wachtrij primaire replicatie. Houd er rekening mee dat het moet een macht van 2.|
|MaxReplicationMessageSize|uint, standaard is 52428800|Statisch|Maximale berichtgrootte van replicatiebewerkingen. Standaardwaarde is 50MB.|
|MaxSecondaryReplicationQueueMemorySize|Uint, de standaardwaarde is 0|Statisch|Dit is de maximale waarde van de secundaire replicatiewachtrij in bytes.|
|MaxSecondaryReplicationQueueSize|uint, standaard is 2048|Statisch|Dit is het maximum aantal bewerkingen die kunnen bestaan in de replicatiewachtrij secundaire. Houd er rekening mee dat het moet een macht van 2.|
|QueueHealthMonitoringInterval|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(30)|Statisch|Interval in seconden opgeven. Deze waarde bepaalt de periode die is gebruikt door de replicatie voor het bewaken van de waarschuwing/fout health-gebeurtenissen in de bewerking replicatiewachtrijen. Een waarde van de statuscontrole van '0' uitgeschakeld |
|QueueHealthWarningAtUsagePercent|uint, standaard is 80|Statisch|Deze waarde bepaalt de replicatie wachtrij gebruik (als percentage) waarna we waarschuwing rapporteren over het gebruik van hoge wachtrij. Dit doen we na een interval van de respijtperiode van QueueHealthMonitoringInterval. Als de wachtrij-gebruik lager is dan dit percentage in de respijtperiode-interval|
|ReplicatorAddress|tekenreeks, standaard is "localhost:0"|Statisch|Het eindpunt in de vorm van een tekenreeks-'IP: poort' die wordt gebruikt door de Windows Fabric-replicatie verbindingen tot stand gebracht met andere replica's om te kunnen verzenden en ontvangen van bewerkingen.|
|ReplicatorListenAddress|tekenreeks, standaard is "localhost:0"|Statisch|Het eindpunt in de vorm van een tekenreeks-'IP: poort' die met de Windows Fabric-replicatie wordt gebruikt voor het ontvangen van bewerkingen van andere replica's.|
|ReplicatorPublishAddress|tekenreeks, standaard is "localhost:0"|Statisch|Het eindpunt in de vorm van een tekenreeks-'IP: poort' die met de Windows Fabric-replicatie wordt gebruikt voor het verzenden van bewerkingen op andere replica's.|
|retryInterval|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(5)|Statisch|Interval in seconden opgeven. Wanneer een bewerking verloren is gegaan of deze timer geweigerd bepaalt hoe vaak de replicatie opnieuw verzenden van de bewerking wordt uit.|

## <a name="resourcemonitorservice"></a>ResourceMonitorService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|IsEnabled|BOOL, standaard is ingesteld op FALSE |Statisch|Bepaalt of de service is ingeschakeld in het cluster of niet. |

## <a name="runas"></a>Uitvoeren als
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat de RunAs-accountnaam. Dit is alleen nodig voor account "Domeingebruiker" of "ManagedServiceAccount" type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het Run as-accounttype. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' Domeingebruiker/NetworkService/ManagedServiceAccount/LocalSystem' zijn.|
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het wachtwoord voor het uitvoeren als-account. Dit is alleen nodig voor type 'Domeingebruiker'. |

## <a name="runasdca"></a>RunAs_DCA
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat de RunAs-accountnaam. Dit is alleen nodig voor account "Domeingebruiker" of "ManagedServiceAccount" type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het Run as-accounttype. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' Lokalegebruiker/domeingebruiker/NetworkService/ManagedServiceAccount/LocalSystem' zijn. |
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het wachtwoord voor het uitvoeren als-account. Dit is alleen nodig voor type 'Domeingebruiker'. |

## <a name="runasfabric"></a>RunAs_Fabric
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat de RunAs-accountnaam. Dit is alleen nodig voor account "Domeingebruiker" of "ManagedServiceAccount" type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het Run as-accounttype. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' Lokalegebruiker/domeingebruiker/NetworkService/ManagedServiceAccount/LocalSystem' zijn. |
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het wachtwoord voor het uitvoeren als-account. Dit is alleen nodig voor type 'Domeingebruiker'. |

## <a name="runashttpgateway"></a>RunAs_HttpGateway
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|RunAsAccountName |tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat de RunAs-accountnaam. Dit is alleen nodig voor account "Domeingebruiker" of "ManagedServiceAccount" type. Geldige waarden zijn 'domein\gebruiker' of 'user@domain'. |
|RunAsAccountType|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het Run as-accounttype. Dit is nodig voor alle geldige waarden voor RunAs-sectie ' Lokalegebruiker/domeingebruiker/NetworkService/ManagedServiceAccount/LocalSystem' zijn. |
|Wachtwoord|tekenreeks, standaardwaarde is "" |Dynamisch|Geeft aan dat het wachtwoord voor het uitvoeren als-account. Dit is alleen nodig voor type 'Domeingebruiker'. |

## <a name="security"></a>Beveiliging
| **Parameter** | **Toegestane waarden** |**Upgradebeleid**| **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|AADClientApplication|tekenreeks, standaardwaarde is ""|Statisch|Naam van de toepassing Native Client of voor Clients van de Fabric-ID |
|AADClusterApplication|tekenreeks, standaardwaarde is ""|Statisch|Naam van de web-API-toepassing of voor het cluster-ID |
|AADTenantId|tekenreeks, standaardwaarde is ""|Statisch|Tenant-ID (GUID) |
|AdminClientCertThumbprints|tekenreeks, standaardwaarde is ""|Dynamisch|Vingerafdrukken van de certificaten die worden gebruikt door clients in de rol van beheerder. Het is een naam door komma's gescheiden lijst. |
|AdminClientClaims|tekenreeks, standaardwaarde is ""|Dynamisch|Alle mogelijke claims verwacht van beheerder clients; dezelfde indeling als ClientClaims; Deze lijst wordt intern toegevoegd aan ClientClaims; dus hoeft u ook de dezelfde items toevoegen aan ClientClaims. |
|AdminClientIdentities|tekenreeks, standaardwaarde is ""|Dynamisch|Windows-identiteit van de fabric-clients in de rol admin; gebruikt voor de autorisatie van fabric bevoorrechte bewerkingen. Het is een door komma's gescheiden lijst. elk item is een domeinaccountnaam of groepsnaam. Voor het gemak; het account dat wordt uitgevoerd fabric.exe is beheerdersrol; automatisch toegewezen wordt gegroepeerd zodat ServiceFabricAdministrators. |
|CertificateExpirySafetyMargin|Interval, de standaardwaarde is Common::TimeSpan::FromMinutes(43200)|Statisch|Interval in seconden opgeven. Veiligheidsmarge voor certificaat verloopt; certificaat health rapportstatus verandert van OK in waarschuwing wanneer de vervaldatum is minder dan dit. Standaard is 30 dagen. |
|CertificateHealthReportingInterval|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(3600 * 8)|Statisch|Interval in seconden opgeven. Interval voor certificaat rapportage voor health; opgeven standaard ingesteld op 8 uur; instellen op 0 schakelt certificaat status rapporteren |
|ClientCertThumbprints|tekenreeks, standaardwaarde is ""|Dynamisch|Vingerafdrukken van de certificaten die door clients worden gebruikt om te communiceren met het cluster. cluster maakt gebruik van dit autoriseren van binnenkomende verbinding. Het is een naam door komma's gescheiden lijst. |
|ClientClaimAuthEnabled|BOOL, standaard is ingesteld op FALSE|Statisch|Geeft aan of claims gebaseerde verificatie is ingeschakeld op clients; Hiermee stelt u deze instelling de waarde true impliciet ClientRoleEnabled. |
|ClientClaims|tekenreeks, standaardwaarde is ""|Dynamisch|Alle mogelijke claims verwacht van clients voor het verbinden met de gateway. Dit is een lijst 'OR': ClaimsEntry \| \| ClaimsEntry \| \| ClaimsEntry... elke ClaimsEntry is een lijst met 'En': ClaimType ClaimValue = & & ClaimType ClaimValue = & & ClaimType = ClaimValue... |
|ClientIdentities|tekenreeks, standaardwaarde is ""|Dynamisch|Windows-identiteit van FabricClient; naamgeving gateway gebruikt dit om te autoriseren van binnenkomende verbindingen. Het is een door komma's gescheiden lijst. elk item is een domeinaccountnaam of groepsnaam. Voor het gemak; het account dat wordt uitgevoerd fabric.exe is automatisch toegestaan. groep ServiceFabricAllowedUsers en ServiceFabricAdministrators zijn. |
|ClientRoleEnabled|BOOL, standaard is ingesteld op FALSE|Statisch|Geeft aan of de functie is ingeschakeld; Wanneer ingesteld op ' True '; clients toegewezen rollen op basis van hun identiteiten. Voor V2; inschakelen van dit betekent dat de client niet in AdminClientCommonNames/AdminClientIdentities alleen-lezen bewerkingen kan alleen worden uitgevoerd. |
|ClusterCertThumbprints|tekenreeks, standaardwaarde is ""|Dynamisch|Vingerafdrukken van certificaten mogen worden toegevoegd aan het cluster. een naam door komma's gescheiden lijst. |
|ClusterCredentialType|reeks, standaard is ingesteld op 'None'|Niet toegestaan|Geeft het type van de beveiligingsreferenties gebruiken om u te beveiligen van het cluster. Geldige waarden zijn "Geen/X509/Windows" |
|ClusterIdentities|tekenreeks, standaardwaarde is ""|Dynamisch|Windows-identiteit van de clusterknooppunten; gebruikt voor het cluster lidmaatschap autorisatie. Het is een door komma's gescheiden lijst. elk item is een domeinaccountnaam of groepsnaam |
|ClusterSpn|tekenreeks, standaardwaarde is ""|Niet toegestaan|Service-principal-naam van het cluster. Wanneer de infrastructuur wordt uitgevoerd als een gebruiker één domein (gMSA-domein-gebruikersaccount). Het is de SPN-naam van de lease-listeners en listeners in fabric.exe: federation listeners; interne replicatieprocedure listeners; Runtime-service-listener en naming gateway-listener. Dit mag leeg zijn wanneer de infrastructuur wordt uitgevoerd als computeraccounts; compute-listener SPN van listener transport-adres in dat geval verbinding te maken aan clientzijde. |
|CrlCheckingFlag|uint, standaard is 0x40000000|Dynamisch|Standaard certificaatketen validatie vlag. kan worden genegeerd door component-specifieke vlag. bijvoorbeeld Federation/X509CertChainFlags 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ ALLEEN instellen op 0, wordt het CRL controleren of volledige lijst met ondersteunde waarden wordt beschreven door dwFlags van CertGetCertificateChain: http://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx |
|CrlDisablePeriod|Interval, de standaardwaarde is Common::TimeSpan::FromMinutes(15)|Dynamisch|Interval in seconden opgeven. Hoe lang CRL-controle is uitgeschakeld voor een bepaald certificaat na het vaststellen van offline-fout. als CRL offline fout kan worden genegeerd. |
|CrlOfflineHealthReportTtl|Interval, de standaardwaarde is Common::TimeSpan::FromMinutes(1440)|Dynamisch|Interval in seconden opgeven. |
|DisableFirewallRuleForDomainProfile| BOOL, standaard is ingesteld op TRUE |Statisch| Hiermee wordt aangegeven als firewallregel mag niet worden ingeschakeld voor het domeinprofiel |
|DisableFirewallRuleForPrivateProfile| BOOL, standaard is ingesteld op TRUE |Statisch| Hiermee wordt aangegeven als firewallregel mag niet worden ingeschakeld voor persoonlijk profiel | 
|DisableFirewallRuleForPublicProfile| BOOL, standaard is ingesteld op TRUE | Statisch|Hiermee wordt aangegeven als firewallregel mag niet worden ingeschakeld voor openbare profiel |
|FabricHostSpn| tekenreeks, standaardwaarde is "" |Statisch| Service principal name FabricHost; Wanneer de infrastructuur wordt uitgevoerd als een gebruiker één domein (gMSA-domein-gebruikersaccount) en FabricHost wordt uitgevoerd onder het computeraccount. Het is de SPN van IPC-listener voor FabricHost; Hiervoor is standaard leeg omdat FabricHost wordt uitgevoerd onder het computeraccount |
|IgnoreCrlOfflineError|BOOL, standaard is ingesteld op FALSE|Dynamisch|Of u wilt de CRL offline fout negeren wanneer serverzijde controleert of inkomende clientcertificaten |
|IgnoreSvrCrlOfflineError|BOOL, standaard is ingesteld op TRUE|Dynamisch|Of u wilt de CRL offline fout negeren wanneer clientzijde controleert of servercertificaten voor binnenkomende; standaard op ' True '. Aanvallen met ingetrokken certificaten vereisen dat DNS; moeilijker dan met ingetrokken clientcertificaten. |
|ServerAuthCredentialType|reeks, standaard is ingesteld op 'None'|Statisch|Geeft het type van de beveiligingsreferenties gebruiken om u te beveiligen de communicatie tussen FabricClient en het Cluster. Geldige waarden zijn "Geen/X509/Windows" |
|ServerCertThumbprints|tekenreeks, standaardwaarde is ""|Dynamisch|Vingerafdrukken van servercertificaten door cluster gebruikt om te communiceren met clients; clients gebruiken deze om te verifiëren van het cluster. Het is een naam door komma's gescheiden lijst. |
|SettingsX509StoreName| tekenreeks, standaard is 'MY'| Dynamisch|X509 certificaatarchief door fabric gebruikt voor de beveiliging van de configuratie |
|UseClusterCertForIpcServerTlsSecurity|BOOL, standaard is ingesteld op FALSE|Statisch|Of u wilt het clustercertificaat dat is gebruikt voor het beveiligen van IPC Server TLS eenheid transport |
|X509Folder|tekenreeks, standaard is /var/lib/waagent|Statisch|Map waar X509 certificaten en persoonlijke sleutels zich bevinden |

## <a name="securityadminclientx509names"></a>Beveiliging/AdminClientX509Names
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, is standaard ingesteld op geen|Dynamisch|Dit is een lijst van een sleutelpaar met "Naam" en "Waarde". Elke 'naam' is van het algemene naam van het onderwerp of DNS-naam van X509 certificaten die zijn gemachtigd voor beheerder clientbewerkingen. Voor een bepaalde "naam", "Waarde" is een met door komma's gescheiden lijst met vingerafdrukken van het certificaat voor de verlener vast te maken, als dat niet leeg is, wordt de directe verlener van beheerder-clientcertificaten moet zich in de lijst. |

## <a name="securityclientaccess"></a>Beveiliging/ClientAccess
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ActivateNode |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor activering van een knooppunt. |
|CancelTestCommand |tekenreeks, standaard is 'Admin' |Dynamisch| Als er tijdens de overdracht is, wordt een specifieke TestCommand - geannuleerd. |
|CodePackageControl |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie codepakketten opnieuw wordt opgestart. |
|SubmitMetadata |tekenreeks, standaard is 'Admin' | Dynamisch|De beveiligingsconfiguratie voor het maken van de toepassing. |
|CreateComposeDeployment|tekenreeks, standaard is 'Admin'| Dynamisch|Maakt een opstellen implementatie beschreven door compositiebestanden |
|CreateName |tekenreeks, standaard is 'Admin' |Dynamisch|De beveiligingsconfiguratie voor het maken van naamgevings-URI. |
|CreateService |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie om services te maken. |
|CreateServiceFromTemplate |tekenreeks, standaard is 'Admin' |Dynamisch|Beveiligingsconfiguratie om services te maken van de sjabloon. |
|CreateVolume|tekenreeks, standaard is 'Admin'|Dynamisch|Hiermee maakt u een volume |
|DeactivateNode |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het deactiveren van een knooppunt. |
|DeactivateNodesBatch |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het deactiveren van meerdere knooppunten. |
|Verwijderen |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguraties voor afbeelding opslaan clientbewerking verwijderen. |
|DeleteApplication |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor verwijdering van de toepassing. |
|DeleteComposeDeployment|tekenreeks, standaard is 'Admin'| Dynamisch|Hiermee verwijdert u de implementatie voor opstellen |
|DeleteName |tekenreeks, standaard is 'Admin' |Dynamisch|De beveiligingsconfiguratie voor verwijdering naamgevings-URI. |
|DeleteService |tekenreeks, standaard is 'Admin' |Dynamisch|De beveiligingsconfiguratie voor verwijdering van de service. |
|DeleteVolume|tekenreeks, standaard is 'Admin'|Dynamisch|Hiermee verwijdert u een volume.| 
|EnumerateProperties |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|De beveiligingsconfiguratie voor de naamgeving van inventarisatie van eigenschappen. |
|EnumerateSubnames |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor naamgevings-URI-opsomming. |
|FileContent |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de installatiekopie van opslaan bestandsoverdracht client (extern aan cluster). |
|FileDownload |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de installatiekopie van store-client-bestand downloaden Inleiding (externe aan cluster). |
|FinishInfrastructureTask |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor voltooien infrastructuurtaken. |
|GetChaosReport | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De status van Chaos binnen het bereik van een bepaald moment kan worden opgehaald. |
|GetClusterConfiguration | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|Induceert GetClusterConfiguration op een partitie. |
|GetClusterConfigurationUpgradeStatus | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| Induceert GetClusterConfigurationUpgradeStatus op een partitie. |
|GetFabricUpgradeStatus |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor de polling-status van de cluster-upgrade. |
|GetNodeDeactivationStatus |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor deactiveringsstatus controleren. |
|GetNodeTransitionProgress | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor het ophalen van wordt uitgevoerd op een knooppunt overgang-opdracht. |
|GetPartitionDataLossProgress | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|Haalt de voortgang van een api-aanroep van invoke gegevens verloren gaan. |
|GetPartitionQuorumLossProgress | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| Haalt de voortgang van een api-aanroep van invoke quorum verloren gaan. |
|GetPartitionRestartProgress | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| Haalt de voortgang van de api-aanroep voor een opnieuw opstarten. |
|GetServiceDescription |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor servicemeldingen long-polling en servicebeschrijvingen te lezen. |
|GetStagingLocation |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de installatiekopie van opslaan client staging-locatie voor het ophalen. |
|GetStoreLocation |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor de installatiekopie van client voor het ophalen van store-locatie opslaan. |
|GetUpgradeOrchestrationServiceState|tekenreeks, standaard is 'Admin'| Dynamisch|GetUpgradeOrchestrationServiceState induceert op een partitie |
|GetUpgradesPendingApproval |tekenreeks, standaard is 'Admin' |Dynamisch| Induceert GetUpgradesPendingApproval op een partitie. |
|GetUpgradeStatus |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor polling upgradestatus van toepassing. |
|InternalList |tekenreeks, standaard is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor de installatiekopie van bestand lijst clientbewerking (intern) worden opgeslagen. |
|InvokeContainerApi|tekenreeks, standaard is 'Admin'|Dynamisch|Container-API aanroepen |
|InvokeInfrastructureCommand |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor infrastructuur-opdrachten voor het beheer van taak. |
|InvokeInfrastructureQuery |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|Beveiligingsconfiguratie voor het uitvoeren van query's infrastructuurtaken. |
|Lijst |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|Beveiligingsconfiguratie voor de installatiekopie van clientbewerking bestand lijst opslaan. |
|MoveNextFabricUpgradeDomain |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het upgraden van clusters met een expliciet is het upgraden van domein wordt hervat. |
|MoveNextUpgradeDomain |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor upgrades van toepassingen met een expliciete Upgrade domein wordt hervat. |
|MoveReplicaControl |tekenreeks, standaard is 'Admin' | Dynamisch|De replica is verplaatst. |
|NameExists |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|Beveiligingsconfiguratie naamgevings-URI aanwezigheid controleert. |
|NodeControl |tekenreeks, standaard is 'Admin' |Dynamisch| Configuratie van de beveiliging voor het starten van; moet worden gestopt; en opnieuw starten van de knooppunten. |
|NodeStateRemoved |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het melden van status van het knooppunt verwijderd. |
|Ping |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor client-pings. |
|PredeployPackageToNode |tekenreeks, standaard is 'Admin' |Dynamisch| Voorafgaand aan implementatie-api. |
|PrefixResolveService |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor de omzetting van de service op basis van een klacht voorvoegsel. |
|PropertyReadBatch |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| Beveiligingsconfiguratie voor de eigenschap naamgeving voor leesbewerkingen. |
|PropertyWriteBatch |tekenreeks, standaard is 'Admin' |Dynamisch|Beveiligingsconfiguraties voor naamgeving voor eigenschap schrijfbewerkingen. |
|ProvisionApplicationType |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het inrichten van de toepassing-type. |
|ProvisionFabric |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het Manifest inrichten MSI en/of -Cluster. |
|Query’s uitvoeren |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor query's. |
|RecoverPartition |tekenreeks, standaard is 'Admin' | Dynamisch|De beveiligingsconfiguratie voor het herstellen van een partitie. |
|RecoverPartitions |tekenreeks, standaard is 'Admin' | Dynamisch|De beveiligingsconfiguratie voor het herstellen van partities. |
|RecoverServicePartitions |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het herstellen van servicepartities. |
|RecoverSystemPartitions |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het herstellen van systeem-servicepartities. |
|RemoveNodeDeactivations |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor herstellen deactiveren op meerdere knooppunten. |
|ReportFabricUpgradeHealth |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het upgraden van clusters met de voortgang van de huidige upgrade wordt hervat. |
|ReportFault |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor rapportage van fouten. |
|ReportHealth |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het melden van status. |
|ReportUpgradeHealth |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor upgrades van toepassingen met de voortgang van de huidige upgrade wordt hervat. |
|ResetPartitionLoad |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor opnieuw instellen van de belasting voor een failoverUnit. |
|ResolveNameOwner |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|De beveiligingsconfiguratie voor het oplossen van de eigenaar van naamgevings-URI. |
|ResolvePartition |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " | Dynamisch|De beveiligingsconfiguratie voor het oplossen van systeemservices. |
|ResolveService |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor de omzetting van de service op basis van een klacht. |
|ResolveSystemService|tekenreeks, standaardwaarde is ' Admin\|\|gebruiker "|Dynamisch| Configuratie van de beveiliging voor het oplossen van systeemservices |
|RollbackApplicationUpgrade |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor upgrades van toepassingen worden teruggedraaid. |
|RollbackFabricUpgrade |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het terugdraaien van een cluster-upgrades. |
|ServiceNotifications |tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor servicemeldingen op basis van gebeurtenissen. |
|SetUpgradeOrchestrationServiceState|tekenreeks, standaard is 'Admin'| Dynamisch|SetUpgradeOrchestrationServiceState induceert op een partitie |
|StartApprovedUpgrades |tekenreeks, standaard is 'Admin' |Dynamisch| Induceert StartApprovedUpgrades op een partitie. |
|StartChaos |tekenreeks, standaard is 'Admin' |Dynamisch| Chaos - gestart als deze nog niet is gestart. |
|StartClusterConfigurationUpgrade |tekenreeks, standaard is 'Admin' |Dynamisch| Induceert StartClusterConfigurationUpgrade op een partitie. |
|StartInfrastructureTask |tekenreeks, standaard is 'Admin' | Dynamisch|De beveiligingsconfiguratie voor het starten van infrastructuurtaken. |
|StartNodeTransition |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het starten van de knooppuntovergang van een. |
|StartPartitionDataLoss |tekenreeks, standaard is 'Admin' |Dynamisch| Induceert verlies van gegevens op een partitie. |
|StartPartitionQuorumLoss |tekenreeks, standaard is 'Admin' |Dynamisch| Induceert quorumverlies op een partitie. |
|StartPartitionRestart |tekenreeks, standaard is 'Admin' |Dynamisch| Tegelijkertijd wordt opnieuw opgestart sommige of alle de replica's van een partitie. |
|StopChaos |tekenreeks, standaard is 'Admin' |Dynamisch| Chaos - stopt als deze is gestart. |
|ToggleVerboseServicePlacementHealthReporting | tekenreeks, standaardwaarde is ' Admin\|\|gebruiker " |Dynamisch| De beveiligingsconfiguratie voor uitgebreide ServicePlacement HealthReporting bij het omschakelen van. |
|UnprovisionApplicationType |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor de toepassing type hierbij. |
|UnprovisionFabric |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het Manifest hierbij MSI en/of -Cluster. |
|UnreliableTransportControl |tekenreeks, standaard is 'Admin' |Dynamisch| Onbetrouwbare Transport voor het toevoegen en verwijderen van gedrag. |
|UpdateService |tekenreeks, standaard is 'Admin' |Dynamisch|De beveiligingsconfiguratie voor service-updates. |
|UpgradeApplication |tekenreeks, standaard is 'Admin' |Dynamisch| Beveiligingsconfiguratie voor het starten of dat upgrades van toepassingen wordt onderbroken. |
|UpgradeComposeDeployment|tekenreeks, standaard is 'Admin'| Dynamisch|Upgrades van de implementatie voor opstellen |
|UpgradeFabric |tekenreeks, standaard is 'Admin' |Dynamisch| De beveiligingsconfiguratie voor het upgraden van clusters te starten. |
|Uploaden |tekenreeks, standaard is 'Admin' | Dynamisch|Beveiligingsconfiguratie voor de installatiekopie uploaden clientbewerking opslaan. |

## <a name="securityclientcertificateissuerstores"></a>Beveiliging/ClientCertificateIssuerStores
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, is standaard ingesteld op geen |Dynamisch|X509 verlener certificaatarchieven voor clientcertificaten; Naam = clientIssuerCN; Waarde = door komma's gescheiden lijst met winkels |

## <a name="securityclientx509names"></a>Beveiliging/ClientX509Names
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, is standaard ingesteld op geen|Dynamisch|Dit is een lijst van een sleutelpaar met "Naam" en "Waarde". Elke 'naam' is van het algemene naam van het onderwerp of DNS-naam van X509 certificaten die zijn gemachtigd voor de clientbewerkingen. Voor een bepaalde "naam", "Waarde" is een met door komma's gescheiden lijst met vingerafdrukken van het certificaat voor de verlener vast te maken, als dat niet leeg is, wordt de directe verlener van clientcertificaten moet zich in de lijst.|

## <a name="securityclustercertificateissuerstores"></a>Beveiliging/ClusterCertificateIssuerStores
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, is standaard ingesteld op geen |Dynamisch|X509 verlener certificaatarchieven voor clustercertificaten; Naam = clusterIssuerCN; Waarde = door komma's gescheiden lijst met winkels |

## <a name="securityclusterx509names"></a>Beveiliging/ClusterX509Names
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, is standaard ingesteld op geen|Dynamisch|Dit is een lijst van een sleutelpaar met "Naam" en "Waarde". Elke 'naam' is van het algemene naam van het onderwerp of DNS-naam van X509 certificaten die zijn gemachtigd voor bewerkingen voor een cluster. Voor een bepaalde "naam", "Waarde" is een met door komma's gescheiden lijst met vingerafdrukken van het certificaat voor de verlener vast te maken, als dat niet leeg is, wordt de directe verlener van clustercertificaten moet zich in de lijst.|

## <a name="securityservercertificateissuerstores"></a>Beveiliging/ServerCertificateIssuerStores
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, is standaard ingesteld op geen |Dynamisch|X509 verlener certificaatarchieven voor servercertificaten; Naam = serverIssuerCN; Waarde = door komma's gescheiden lijst met winkels |

## <a name="securityserverx509names"></a>Beveiliging/ServerX509Names
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, is standaard ingesteld op geen|Dynamisch|Dit is een lijst van een sleutelpaar met "Naam" en "Waarde". Elke 'naam' is van het algemene naam van het onderwerp of DNS-naam van X509 certificaten die zijn gemachtigd voor siteserverbewerkingen. Voor een bepaalde "naam", "Waarde" is een met door komma's gescheiden lijst met vingerafdrukken van het certificaat voor de verlener vast te maken, als dat niet leeg is, wordt de directe verlener van certificaten moet zich in de lijst.|

## <a name="setup"></a>Instellen
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ContainerNetworkName|tekenreeks, standaardwaarde is ""| Statisch |De naam van het netwerk te gebruiken bij het instellen van een netwerk container.|
|ContainerNetworkSetup|BOOL, standaard is ingesteld op FALSE| Statisch |Hiermee geeft u op of u voor het instellen van een netwerk container.|
|FabricDataRoot |Reeks | Niet toegestaan |Hoofdmap voor service Fabric-gegevens. Standaard voor Azure d:\svcfab is |
|FabricLogRoot |Reeks | Niet toegestaan |Service fabric log-hoofdmap. Dit is waar de SF-logboeken en traceringen worden geplaatst. |
|NodesToBeRemoved|tekenreeks, standaardwaarde is ""| Dynamisch |De knooppunten die moeten worden verwijderd als onderdeel van de upgrade van de configuratie. (Alleen voor implementaties van de zelfstandige versie)|
|ServiceRunAsAccountName |Reeks | Niet toegestaan |De accountnaam waaronder fabric host-service uit te voeren. |
|SkipContainerNetworkResetOnReboot|BOOL, standaard is ingesteld op FALSE|NotAllowed|Hiermee geeft u op of u moet worden overgeslagen bij het opnieuw instellen container netwerk bij het opnieuw opstarten.|
|SkipFirewallConfiguration |BOOL, de standaardinstelling is false | Niet toegestaan |Hiermee geeft u als de firewall-instellingen moeten worden ingesteld door het systeem of niet. Dit geldt alleen als u windows firewall. Als u firewalls van derden gebruikt, moet klikt u vervolgens u de poorten openen voor het systeem en de toepassingen te gebruiken |

## <a name="tokenvalidationservice"></a>TokenValidationService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|Providers |tekenreeks, standaard is "DSTS" |Statisch|Door komma's gescheiden lijst met providers van validatie van tokens om in te schakelen (geldige providers zijn: DSTS; AAD). Op dit moment slechts één provider kan worden ingeschakeld op elk gewenst moment. |

## <a name="traceetw"></a>Tracering/Etw
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|Niveau |Int, de standaardwaarde is 4 | Dynamisch |Traceerniveau etw kunt nemen de waarden 1, 2, 3, 4. Ondersteund moet u het traceringsniveau op 4 |

## <a name="transactionalreplicator"></a>TransactionalReplicator
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval | Tijd in seconden, de standaardwaarde is 0,015 | Statisch | Interval in seconden opgeven. Bepaalt de hoeveelheid tijd dat de replicatie moet wachten na de ontvangst van een bewerking voordat u een bevestiging terug te sturen. Andere bewerkingen die tijdens deze periode is ontvangen, wordt hun bevestigingen verzonden in een enkel bericht te reduceren netwerkverkeer, maar mogelijk vermindering van de doorvoer van de replicatie -> hebben. |
|MaxCopyQueueSize |Uint, de standaardwaarde is 16384 tekens | Statisch |Dit is de maximale waarde definieert de aanvankelijke grootte van de wachtrij waarmee replicatiebewerkingen worden bewaard. Houd er rekening mee dat het moet een macht van 2. Als tijdens runtime de wachtrij voor deze bewerking grootte toeneemt worden beperkt tussen de primaire en secundaire distributeurs. |
|MaxPrimaryReplicationQueueMemorySize |Uint, de standaardwaarde is 0 | Statisch |Dit is de maximale waarde van de wachtrij primaire replicatie in bytes. |
|MaxPrimaryReplicationQueueSize |Uint, de standaardwaarde is 8192 liggen | Statisch |Dit is het maximum aantal bewerkingen die kunnen bestaan in de wachtrij primaire replicatie. Houd er rekening mee dat het moet een macht van 2. |
|MaxReplicationMessageSize |uint, standaard is 52428800 | Statisch | Maximale berichtgrootte van replicatiebewerkingen. Standaardwaarde is 50MB. |
|MaxSecondaryReplicationQueueMemorySize |Uint, de standaardwaarde is 0 | Statisch |Dit is de maximale waarde van de secundaire replicatiewachtrij in bytes. |
|MaxSecondaryReplicationQueueSize |Uint, de standaardwaarde is 16384 tekens | Statisch |Dit is het maximum aantal bewerkingen die kunnen bestaan in de replicatiewachtrij secundaire. Houd er rekening mee dat het moet een macht van 2. |
|ReplicatorAddress |tekenreeks, standaard is "localhost:0" | Statisch | Het eindpunt in de vorm van een tekenreeks-'IP: poort' die wordt gebruikt door de Windows Fabric-replicatie verbindingen tot stand gebracht met andere replica's om te kunnen verzenden en ontvangen van bewerkingen. |

## <a name="transport"></a>Transport
| **Parameter** | **Toegestane waarden** |**Upgradebeleid** |**Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|ConnectionOpenTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(60)|Statisch|Interval in seconden opgeven. Time-out voor de installatie van de verbinding aan zowel inkomende als accepteren (inclusief onderhandelen over de beveiliging in veilige modus) |
|FrameHeaderErrorCheckingEnabled|BOOL, standaard is ingesteld op TRUE|Statisch|Standaardinstelling voor een fout bij het controleren op frameheader in niet-beveiligde modus. onderdeelinstelling overschrijft dit. |
|MessageErrorCheckingEnabled|BOOL, standaard is ingesteld op FALSE|Statisch|Standaardinstelling voor een fout bij het controleren op de koptekst van bericht en de hoofdtekst in niet-beveiligde modus; onderdeelinstelling overschrijft dit. |
|ResolveOption|tekenreeks, de standaardwaarde is 'Onbekende'|Statisch|Bepaalt hoe de FQDN-naam is opgelost.  Geldige waarden zijn 'niet opgegeven/ipv4/ipv6'. |
|SendTimeout|Interval, de standaardwaarde is Common::TimeSpan::FromSeconds(300)|Dynamisch|Interval in seconden opgeven. Time-out voor het detecteren van vastgelopen verbinding verzenden. TCP-fout rapporten zijn niet betrouwbaar zijn in een omgeving. Dit moet mogelijk worden aangepast op basis van de beschikbare netwerkbandbreedte en de grootte van uitgaande gegevens (\*MaxMessageSize\/\*SendQueueSizeLimit). |

## <a name="upgradeorchestrationservice"></a>UpgradeOrchestrationService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|AutoupgradeEnabled | BOOL, de standaardinstelling is true |Statisch| Automatische polling en upgrade-actie op basis van een doel-status-bestand. |
|MinReplicaSetSize |Int, de standaardwaarde is 0 |Statisch |De MinReplicaSetSize voor UpgradeOrchestrationService.
|PlacementConstraints | tekenreeks, standaardwaarde is "" |Statisch| De PlacementConstraints voor UpgradeOrchestrationService. |
|QuorumLossWaitDuration | Tijd in seconden, de standaardwaarde is MaxValue |Statisch| Interval in seconden opgeven. De QuorumLossWaitDuration voor UpgradeOrchestrationService. |
|ReplicaRestartWaitDuration | Tijd in seconden, de standaardwaarde is 60 minuten|Statisch| Interval in seconden opgeven. De ReplicaRestartWaitDuration voor UpgradeOrchestrationService. |
|StandByReplicaKeepDuration | Tijd in seconden, de standaardwaarde is 60*24*7 minuten |Statisch| Interval in seconden opgeven. De StandByReplicaKeepDuration voor UpgradeOrchestrationService. |
|TargetReplicaSetSize |Int, de standaardwaarde is 0 |Statisch |De TargetReplicaSetSize voor UpgradeOrchestrationService. |
|UpgradeApprovalRequired | BOOL, de standaardinstelling is false | Statisch|Instellen om te maken van code-upgrade vereisen van goedkeuring door beheerder voordat u doorgaat. |

## <a name="upgradeservice"></a>UpgradeService
| **Parameter** | **Toegestane waarden** | **Upgradebeleid** | **Richtlijnen of korte beschrijving** |
| --- | --- | --- | --- |
|BaseUrl | tekenreeks, standaardwaarde is "" |Statisch|BaseUrl voor UpgradeService. |
|ClusterId | tekenreeks, standaardwaarde is "" |Statisch|ClusterId voor UpgradeService. |
|CoordinatorType | tekenreeks, standaard is "WUTest"|Niet toegestaan|De CoordinatorType voor UpgradeService. |
|MinReplicaSetSize | Int, de standaardwaarde is 2 |Niet toegestaan| De MinReplicaSetSize voor UpgradeService. |
|OnlyBaseUpgrade | BOOL, de standaardinstelling is false |Dynamisch|OnlyBaseUpgrade voor UpgradeService. |
|PlacementConstraints |tekenreeks, standaardwaarde is "" |Niet toegestaan|De PlacementConstraints voor Upgrade-service. |
|TargetReplicaSetSize | Int, de standaardwaarde is 3 |Niet toegestaan| De TargetReplicaSetSize voor UpgradeService. |
|TestCabFolder | tekenreeks, standaardwaarde is "" |Statisch| TestCabFolder voor UpgradeService. |
|X509FindType | tekenreeks, standaardwaarde is ""|Dynamisch| X509FindType voor UpgradeService. |
|X509FindValue | tekenreeks, standaardwaarde is "" |Dynamisch| X509FindValue voor UpgradeService. |
|X509SecondaryFindValue | tekenreeks, standaardwaarde is "" |Dynamisch| X509SecondaryFindValue voor UpgradeService. |
|X509StoreLocation | tekenreeks, standaardwaarde is "" |Dynamisch| X509StoreLocation voor UpgradeService. |
|X509StoreName | tekenreeks, standaardwaarde is 'My'|Dynamisch|X509StoreName voor UpgradeService. |

## <a name="next-steps"></a>Volgende stappen
Lees deze artikelen voor meer informatie over het Clusterbeheer:

[Toevoegen, meegenomen, certificaten uit uw Azure-cluster verwijderen ](service-fabric-cluster-security-update-certs-azure.md) 

