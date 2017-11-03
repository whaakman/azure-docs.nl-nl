---
title: Wijzig de instellingen in Azure microservices ReliableDictionaryActorStateProvider | Microsoft Docs
description: Meer informatie over het configureren van stateful actoren van het type ReliableDictionaryActorStateProvider Azure Service Fabric.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: 79b48ffa-2474-4f1c-a857-3471f9590ded
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: 5dcd1b4f5a070e9a09b6f8338928d93d10227d38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Reliable Actors--ReliableDictionaryActorStateProvider configureren
U kunt de standaardconfiguratie van ReliableDictionaryActorStateProvider wijzigen door het settings.xml-bestand voor de opgegeven actor gegenereerd in de hoofdmap van het Visual Studio-pakket onder de map Config wordt gewijzigd.

De Azure Service Fabric-runtime gezocht naar vooraf gedefinieerde sectienamen in het bestand settings.xml en de configuratiewaarden verbruikt tijdens het maken van de onderliggende runtimeonderdelen.

> [!NOTE]
> Voer **niet** verwijderen of wijzigen van de namen van de volgende configuraties in het settings.xml-bestand dat wordt gegenereerd in de Visual Studio-oplossing.
> 
> 

Er zijn ook algemene instellingen voor de configuratie van ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Globale configuratie
De globale configuratie is opgegeven in het clustermanifest voor het cluster onder de sectie KtlLogger. Hierdoor kan de configuratie van de gedeelde locatie en grootte plus de globale geheugenlimieten gebruikt door het logboek. Houd er rekening mee dat wijzigingen in het clustermanifest invloed zijn op alle services die gebruikmaken van ReliableDictionaryActorStateProvider en betrouwbare stateful services.

Het clustermanifest is een XML-bestand met instellingen en configuraties die van toepassing op alle knooppunten en services in het cluster. Het bestand wordt doorgaans ClusterManifest.xml genoemd. U kunt zien dat het cluster voor uw cluster met behulp van de powershell-opdracht Get-ServiceFabricClusterManifest manifest.

### <a name="configuration-names"></a>Configuratienamen
| Naam | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |KB |8388608 |Minimum aantal KB in de kernelmodus voor het logboek schrijven buffergroep geheugen toewijzen. Deze geheugengroep wordt gebruikt voor het opslaan van informatie over de status voor het schrijven naar de schijf. |
| WriteBufferMemoryPoolMaximumInKB |KB |Geen limiet |Maximale grootte waarnaar het logboek schrijven buffergroep geheugen kan worden uitgebreid. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke GUID moet worden gebruikt voor het identificeren van de gedeelde standaardlogboekbestand gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogId in hun specifieke configuratie van de service geen opgeven. Als SharedLogId is opgegeven, moet klikt u vervolgens SharedLogPath ook worden opgegeven. |
| SharedLogPath |Naam van het volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand wordt gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogPath in hun specifieke configuratie van de service geen opgeven. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moet ook worden opgegeven. |
| SharedLogSizeInMB |Megabytes |8192 |Hiermee geeft u het aantal MB aan schijfruimte voor het statisch toewijzen voor de gedeelde logboek. De waarde moet 2048 aan of groter zijn. |

### <a name="sample-cluster-manifest-section"></a>Voorbeeld cluster manifest-sectie
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Opmerkingen
Het logboek heeft een algemene groep geheugen toegewezen uit het voor niet-wisselbaar kernelgeheugen die beschikbaar is voor alle betrouwbare services op een knooppunt voor het opslaan van gegevens van de gebruikersstatus voordat het toegewezen logboek wordt geschreven die is gekoppeld aan de replica betrouwbare service. Grootte van de groep wordt bepaald door de instellingen voor WriteBufferMemoryPoolMinimumInKB en WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB geeft de begingrootte van deze geheugengroep en de grootte van de laagste waarnaar de geheugengroep kan verkleinen. WriteBufferMemoryPoolMaximumInKB is de hoogste grootte waarnaar de geheugengroep kan groeien. Elke replica betrouwbare service die wordt geopend, kan de grootte van de geheugengroep verhogen door een systeem bepaald bedrag tot WriteBufferMemoryPoolMaximumInKB. Als er meer aanvraag voor het geheugen van de geheugengroep dan beschikbaar is, worden aanvragen voor het geheugen wordt uitgesteld tot geheugen beschikbaar is. Daarom als de geheugengroep voor write-buffer te klein voor een specifieke configuratie is vervolgens tot slechtere prestaties.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt voor het definiëren van de GUID en de locatie voor het standaard gedeelde logboek voor alle knooppunten in het cluster. De standaard gedeelde logboek wordt gebruikt voor alle betrouwbare services die de instellingen in de settings.xml voor de specifieke service geen opgeven. Voor de beste prestaties worden gedeelde logboekbestanden geplaatst op schijven die uitsluitend voor de gedeelde logboekbestand gebruikt om te conflicten te verminderen.

SharedLogSizeInMB geeft de hoeveelheid schijfruimte toe te wijzen voor het standaard gedeelde logboek op alle knooppunten.  SharedLogId en SharedLogPath hoeft niet te worden opgegeven in de volgorde voor SharedLogSizeInMB worden opgegeven.

## <a name="replicator-security-configuration"></a>Replicator Beveiligingsconfiguratie
Beveilig het communicatiekanaal dat wordt gebruikt tijdens de replicatie worden beveiligingsconfiguraties Replicator gebruikt. Dit betekent dat services elkaars replicatieverkeer, zodat de gegevens die maximaal beschikbaar is, is ook veilig kunnen niet zien.
Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.

### <a name="section-name"></a>De sectienaam van de
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Configuratie van de Replicator
Replicator configuraties gebruikt voor het configureren van de replicatie die verantwoordelijk is voor de Actor State-Provider staat maximaal betrouwbare door repliceren en de status van de lokaal persistent maken.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. Deze sectie wordt gesproken over aanvullende configuraties die beschikbaar zijn voor het afstemmen van de replicator.

### <a name="section-name"></a>De sectienaam van de
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Configuratienamen
| Naam | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Seconden |0.015 |Periode waarvoor de replicatie op de secundaire Wacht na de ontvangst van een bewerking voor het verzenden van back-een bevestiging naar de primaire. Alle andere bevestigingen voor bewerkingen binnen dit interval verwerkt worden verzonden, worden verzonden als een reactie. |
| ReplicatorEndpoint |N.v.t. |Er is geen standaard--vereiste parameter |IP-adres en poort op die de primaire en secundaire replicatie wordt gebruikt om te communiceren met andere replicaties in de replica is ingesteld. Dit moet verwijzen naar een resource TCP-eindpunt in het servicemanifest. Raadpleeg [Service manifest resources](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van endpoint-resources in servicemanifest. |
| MaxReplicationMessageSize |Bytes |50 MB |Maximale grootte van de replicatiegegevens die kunnen worden overgebracht in één bericht. |
| MaxPrimaryReplicationQueueSize |Aantal bewerkingen |8192 |Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicatie een bevestiging van de secundaire replicaties ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2 zijn. |
| MaxSecondaryReplicationQueueSize |Aantal bewerkingen |16384 |Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking wordt vrijgemaakt nadat u de status maximaal beschikbaar is via persistentie. Deze waarde moet groter zijn dan 64 en een macht van 2 zijn. |
| CheckpointThresholdInMB |MB |200 |De hoeveelheid ruimte in logboekbestand waarna de status gecontroleerd wordt. |
| MaxRecordSizeInKB |KB |1024 |Grootste recordgrootte dat de replicatie mogelijk in het logboek schrijven. Deze waarde moet een meervoud van 4 en groter zijn dan 16. |
| OptimizeLogForLowerDiskUsage |Booleaanse waarde |De waarde True |Indien true, wordt het logboek geconfigureerd zodat toegewezen logboekbestand van de replica wordt gemaakt met behulp van een NTFS-sparse bestand. Hierdoor wordt het werkelijke gebruik van schijfruimte voor het bestand verlaagd. Als deze eigenschap ONWAAR is, wordt het bestand gemaakt met vaste toewijzingen, wat mogelijk is dat de beste schrijfprestaties. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke guid moet worden gebruikt voor het identificeren van de gedeelde logboekbestand gebruikt in combinatie met deze replica. Services moeten normaal gesproken niet gebruiken voor deze instelling. Echter, als SharedLogId is opgegeven, klikt u vervolgens SharedLogPath moet ook worden opgegeven. |
| SharedLogPath |Naam van het volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand voor deze replica wordt gemaakt. Services moeten normaal gesproken niet gebruiken voor deze instelling. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moet ook worden opgegeven. |

## <a name="sample-configuration-file"></a>Voorbeeld van configuratiebestand
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## <a name="remarks"></a>Opmerkingen
De parameter BatchAcknowledgementInterval bepaalt replicatielatentie. Een waarde van '0' resulteert in de laagst mogelijke latentie koste doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Hoe hoger de waarde voor BatchAcknowledgementInterval, hoe hoger de algehele replicatie doorvoer, koste hogere latentie van de bewerking. Hierdoor kan rechtstreeks aan de latentie van transactie doorvoeracties.

De parameter CheckpointThresholdInMB bepaalt de hoeveelheid schijfruimte die de replicator gebruiken kunt voor het opslaan van informatie over de status in de toegewezen logboekbestand van de replica. Dit op een hogere waarde dan de standaardwaarde verhogen kan leiden tot herconfiguratie sneller worden wanneer een nieuwe replica wordt toegevoegd aan de set. Dit komt door de van gedeeltelijke statusoverdracht die uitgevoerd als gevolg van de beschikbaarheid van meer geschiedenis van bewerkingen in het logboek wordt. Hierdoor kan de hersteltijd van een replica mogelijk na een crash toenemen.

Als u OptimizeForLowerDiskUsage ingesteld op true, ruimte in logboekbestand worden te veel ingerichte zodat actieve replica's kunnen meer informatie over de status opslaan in de logboekbestanden, terwijl minder schijfruimte wordt gebruikt door inactieve replica's. Dit maakt het mogelijk voor het hosten van meer replica's op een knooppunt. Als u OptimizeForLowerDiskUsage ingesteld op false, wordt de informatie over de sneller geschreven naar de logboekbestanden.

De instelling MaxRecordSizeInKB definieert de maximale grootte van een record die kan worden geschreven met de replicatie in het logboekbestand. In de meeste gevallen is de standaardgrootte van 1024 KB record optimaal. Echter, als de service groter gegevensitems deel uitmaken van de informatie over de status veroorzaakt, deze waarde moet mogelijk worden verhoogd. Er is weinig voordeel bij het maken van MaxRecordSizeInKB kleiner zijn dan 1024, zoals kleinere records alleen de benodigde ruimte voor de record kleiner gebruiken. We verwachten dat deze waarde moet alleen worden gewijzigd in zeldzame gevallen.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om een service een apart gedeelde logboek uit het standaard gedeelde logboek gebruiken voor het knooppunt. Voor een optimale efficiency moeten zoveel services mogelijk hetzelfde gedeelde logboek opgeven. Gedeelde logboekbestanden moeten worden geplaatst op schijven die uitsluitend voor de gedeelde logboekbestand worden gebruikt om te conflicten head verkeer beperken. We verwachten dat deze waarden moet alleen worden gewijzigd in zeldzame gevallen.

