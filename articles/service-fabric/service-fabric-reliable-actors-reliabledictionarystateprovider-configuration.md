---
title: ReliableDictionaryActorStateProvider wijzigen in Azure Service Fabric-actoren | Microsoft Docs
description: Meer informatie over het configureren van stateful Azure Service Fabric-actoren van het type ReliableDictionaryActorStateProvider.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: ''
ms.assetid: 79b48ffa-2474-4f1c-a857-3471f9590ded
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: 186305083d3e952d87a67b890a88e52eacc2b361
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734537"
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Reliable Actors--ReliableDictionaryActorStateProvider configureren
U kunt de standaardconfiguratie van ReliableDictionaryActorStateProvider wijzigen door het veranderen van de settings.xml-bestand gegenereerd in de hoofdmap van het Visual Studio-pakket in de Config-map voor de opgegeven actor.

De Azure Service Fabric-runtime vooraf gedefinieerde namen in het bestand settings.xml zoekt en neemt de configuratiewaarden tijdens het maken van de onderliggende runtimeonderdelen.

> [!NOTE]
> Voer **niet** verwijderen of wijzigen van de namen van de volgende configuraties in het settings.xml-bestand dat wordt gegenereerd in Visual Studio-oplossing.
> 
> 

Er zijn ook algemene instellingen die invloed hebben op de configuratie van ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Algemene configuratie
De globale configuratie is opgegeven in het clustermanifest voor het cluster onder de sectie KtlLogger. Hierdoor kan de configuratie van de gedeelde locatie en grootte plus de globale geheugenlimieten die worden gebruikt door het logboek. Houd er rekening mee dat wijzigingen in het clustermanifest invloed hebben op alle services die gebruikmaken van ReliableDictionaryActorStateProvider en betrouwbare stateful services.

Het clustermanifest is een XML-bestand waarin de instellingen en configuraties die van toepassing op alle knooppunten en services in het cluster zijn. Het bestand wordt doorgaans ClusterManifest.xml genoemd. U kunt zien dat het cluster het manifest van uw cluster met behulp van de powershell-opdracht Get-ServiceFabricClusterManifest.

### <a name="configuration-names"></a>Configuratienamen
| Name | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobytes |8388608 |Minimum aantal KB in de kernelmodus voor de logger schrijven bufferpool geheugen toewijzen. Deze geheugengroep wordt gebruikt voor het opslaan van informatie over de status voor schrijven naar schijf. |
| WriteBufferMemoryPoolMaximumInKB |Kilobytes |Geen limiet |Maximale grootte waarvoor het logboek schrijven buffergroep geheugen te vergroten. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke GUID moet worden gebruikt voor het identificeren van de gedeelde standaardlogboekbestand gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogId in hun specifieke configuratie van de service geen opgeven. Als SharedLogId is opgegeven, moeten klikt u vervolgens SharedLogPath ook worden opgegeven. |
| SharedLogPath |De naam van de volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand wordt gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogPath in hun specifieke configuratie van de service geen opgeven. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moeten ook worden opgegeven. |
| SharedLogSizeInMB |Megabytes |8192 |Hiermee geeft u het aantal MB aan schijfruimte vrij statisch toewijzen voor het gedeelde logboek. De waarde moet 2048 of groter. |

### <a name="sample-cluster-manifest-section"></a>Voorbeeld van cluster manifest-sectie
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
Het logboek heeft een globale groep van het geheugen toegewezen vanuit de van niet-wisselbaar kernelgeheugen die beschikbaar is voor alle betrouwbare services op een knooppunt voor het opslaan van gegevens van de gebruikersstatus voordat wordt geschreven naar het logboek voor toegewezen die zijn gekoppeld aan de betrouwbare service-replica. Grootte van de groep wordt bepaald door de instellingen voor WriteBufferMemoryPoolMinimumInKB en WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB Hiermee geeft u zowel de aanvankelijke grootte van deze geheugengroep van en de grootte van de laagste waaraan de geheugengroep kan verkleinen. WriteBufferMemoryPoolMaximumInKB is de grootte van de hoogste waaraan de geheugengroep kan groeien. Elke replica betrouwbare service die is geopend verhogen met een bepaald systeem bedrag maximaal WriteBufferMemoryPoolMaximumInKB de grootte van de geheugengroep. Als er meer vraag naar geheugen van de geheugengroep dan beschikbaar is, wordt er aanvragen voor het geheugen worden uitgesteld totdat geheugen beschikbaar is. Dus als de geheugengroep schrijven-buffer te klein voor een specifieke configuratie is vervolgens tot slechtere prestaties.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt voor het definiëren van de GUID en de locatie voor het standaard gedeelde logboek voor alle knooppunten in het cluster. Het standaard gedeelde logboek wordt gebruikt voor alle betrouwbare services die de instellingen in de settings.xml voor de specifieke service geen opgeven. Voor de beste prestaties moeten de gedeelde bestanden worden geplaatst op schijven die uitsluitend voor de gedeelde logboekbestand worden gebruikt voor het aantal conflicten verminderen.

SharedLogSizeInMB geeft de hoeveelheid schijfruimte toe te wijzen voor het standaard gedeelde logboek op alle knooppunten.  SharedLogId en SharedLogPath hoeft te worden opgegeven in de volgorde voor SharedLogSizeInMB worden opgegeven.

## <a name="replicator-security-configuration"></a>Configuratie van de replicatie-beveiliging
Beveiligingsconfiguraties replicatie worden gebruikt om Beveilig het communicatiekanaal dat wordt gebruikt tijdens de replicatie. Dit betekent dat services elkaars replicatieverkeer, ervoor te zorgen dat de gegevens die maximaal beschikbaar is, is ook veilig kunnen niet zien.
Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.

> [!IMPORTANT]
> Op Linux-knooppunten moeten certificaten een PEM-indeling. Zie voor meer informatie over het zoeken naar en configureren van certificaten voor Linux [certificaten configureren op Linux](./service-fabric-configure-certificates-linux.md). 
> 

### <a name="section-name"></a>Sectienaam
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Configuratie van de replicatie
Replicatie-configuraties worden gebruikt voor het configureren van de replicatie dat verantwoordelijk is voor de status van de Actor State-Provider zeer betrouwbare maken door deze te repliceren en het behouden van de status lokaal.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. In deze sectie wordt besproken aanvullende configuraties die beschikbaar zijn voor het afstemmen van de replicatie.

### <a name="section-name"></a>Sectienaam
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Configuratienamen
| Name | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Seconden |0.015 |De periode waarvoor de replicatie op de secundaire Wacht na de ontvangst van een bewerking voor het verzenden van een bevestiging terug naar de primaire. Alle andere bevestigingen voor bewerkingen die worden verwerkt binnen dit interval worden verzonden als één antwoord verzonden. |
| ReplicatorEndpoint |N/A |Er is geen standaard--de vereiste parameter |IP-adres en poort op die de primaire/secundaire replicatie wordt gebruikt om te communiceren met andere distributeurs in de replica instellen. Dit moet verwijzen naar een TCP-resource-eindpunt in het servicemanifest. Raadpleeg [servicemanifest-resources](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van eindpunt resources in het servicemanifest. |
| MaxReplicationMessageSize |Bytes |50 MB |Maximale grootte van de replicatiegegevens die kunnen worden overgebracht in één bericht. |
| MaxPrimaryReplicationQueueSize |Aantal bewerkingen |8192 |Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicatie ontvangt een bevestiging van de secundaire distributeurs. Deze waarde moet groter zijn dan 64 en een macht van 2. |
| MaxSecondaryReplicationQueueSize |Aantal bewerkingen |16384 |Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking wordt vrijgemaakt nadat u de status maximaal beschikbaar is via persistentie. Deze waarde moet groter zijn dan 64 en een macht van 2. |
| CheckpointThresholdInMB |MB |200 |De hoeveelheid ruimte in logboekbestand waarna de status gecontroleerd wordt. |
| MaxRecordSizeInKB |KB |1024 |Grootste recordgrootte die de replicatie in het logboek kan schrijven. Deze waarde moet een meervoud van 4 en hoger zijn dan 16. |
| OptimizeLogForLowerDiskUsage |Booleaans |true |Indien waar, wordt het logboek geconfigureerd zodat toegewezen logboekbestand van de replica wordt gemaakt met behulp van een NTFS-sparse-bestand. Hiermee wordt het werkelijke gebruik van schijfruimte voor het bestand verlaagd. Als deze eigenschap ONWAAR is, wordt het bestand wordt gemaakt met vaste toewijzingen, waardoor dat de beste prestaties schrijven. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke guid moet worden gebruikt voor het identificeren van de gedeelde logboekbestand gebruikt in combinatie met deze replica. Normaal gesproken moeten deze instelling niet gebruikt door services. Echter, als SharedLogId is opgegeven, klikt u vervolgens SharedLogPath moeten ook worden opgegeven. |
| SharedLogPath |De naam van de volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand voor deze replica wordt gemaakt. Normaal gesproken moeten deze instelling niet gebruikt door services. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moeten ook worden opgegeven. |

## <a name="sample-configuration-file"></a>Voorbeeldconfiguratiebestand
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
De parameter BatchAcknowledgementInterval bepaalt replicatielatentie. Een waarde van '0' resulteert in de laagst mogelijke latentie leiden doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Hoe hoger de waarde voor BatchAcknowledgementInterval, hoe hoger de algemene replicatie doorvoer, leiden van hogere latentie van de bewerking. Dit wordt rechtstreeks omgezet in de latentie van transacties doorgevoerd.

De parameter CheckpointThresholdInMB bepaalt de hoeveelheid schijfruimte die de replicatie gebruiken kunt voor het opslaan van informatie over de status in de toegewezen logboekbestand van de replica. Dit op een hogere waarde dan de standaardwaarde verhogen kan leiden tot snellere herconfiguratie keer wanneer een nieuwe replica wordt toegevoegd aan de set. Dit komt door de gedeeltelijke statusoverdracht die uitgevoerd vanwege de beschikbaarheid van meer geschiedenis van activiteiten in het logboek wordt. Hierdoor kan de hersteltijd van een replica mogelijk na een crash toenemen.

Als u OptimizeForLowerDiskUsage ingesteld op true, bestand logboekruimte worden drukke zodat actieve replica's kunnen meer informatie over de status opslaan in de logboekbestanden, terwijl inactieve replica's minder schijfruimte wordt gebruikt. Dit maakt het mogelijk voor het hosten van meer replica's op een knooppunt. Als u OptimizeForLowerDiskUsage ingesteld op false, wordt de informatie over de status sneller geschreven naar de logboekbestanden.

De instelling MaxRecordSizeInKB bepaalt de maximale grootte van een record die kan worden geschreven met de replicatie in het logboekbestand. In de meeste gevallen is de standaardgrootte van 1024 KB record optimaal. Echter, als de service wordt veroorzaakt door grotere gegevensitems moet deel uitmaken van de informatie over de status, deze waarde moet mogelijk worden verhoogd. Er is weinig voordeel bij het maken van MaxRecordSizeInKB kleiner zijn dan 1024, kleinere records gebruiken alleen de benodigde ruimte voor de kleinere record. We verwachten dat deze waarde moet alleen worden gewijzigd in zeldzame gevallen.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om een service waarmee een afzonderlijke gedeelde logboek van het standaard gedeelde logboek voor het knooppunt. Voor maximale efficiency moeten zo veel services mogelijk hetzelfde gedeelde logboek opgeven. Gedeelde logboekbestanden moeten worden geplaatst op schijven die uitsluitend voor de gedeelde logboekbestand worden gebruikt voor het aantal bewegingen conflicten verminderen. We verwachten dat deze waarden moet alleen worden gewijzigd in zeldzame gevallen.

