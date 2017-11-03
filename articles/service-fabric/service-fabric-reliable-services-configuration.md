---
title: Betrouwbare microservices voor Azure configureren | Microsoft Docs
description: Meer informatie over het configureren van stateful Reliable Services in Azure Service Fabric.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: sumukhs
ms.openlocfilehash: 84111b37f5cdecf377442bca0b15af2092d57414
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-stateful-reliable-services"></a>Stateful betrouwbare services configureren
Er zijn twee sets van configuratie-instellingen voor betrouwbare services. Er is één set globale voor alle betrouwbare services in het cluster terwijl de andere set specifiek voor een bepaalde betrouwbare service is.

## <a name="global-configuration"></a>Globale configuratie
Configuratie van de globale betrouwbare service is opgegeven in het clustermanifest voor het cluster onder de sectie KtlLogger. Hierdoor kan de configuratie van de gedeelde locatie en grootte plus de globale geheugenlimieten gebruikt door het logboek. Het clustermanifest is een XML-bestand met instellingen en configuraties die van toepassing op alle knooppunten en services in het cluster. Het bestand wordt doorgaans ClusterManifest.xml genoemd. U kunt zien dat het cluster voor uw cluster met behulp van de powershell-opdracht Get-ServiceFabricClusterManifest manifest.

### <a name="configuration-names"></a>Configuratienamen
| Naam | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |KB |8388608 |Minimum aantal KB in de kernelmodus voor het logboek schrijven buffergroep geheugen toewijzen. Deze geheugengroep wordt gebruikt voor het opslaan van informatie over de status voor het schrijven naar de schijf. |
| WriteBufferMemoryPoolMaximumInKB |KB |Geen limiet |Maximale grootte waarnaar het logboek schrijven buffergroep geheugen kan worden uitgebreid. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke GUID moet worden gebruikt voor het identificeren van de gedeelde standaardlogboekbestand gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogId in hun specifieke configuratie van de service geen opgeven. Als SharedLogId is opgegeven, moet klikt u vervolgens SharedLogPath ook worden opgegeven. |
| SharedLogPath |Naam van het volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand wordt gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogPath in hun specifieke configuratie van de service geen opgeven. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moet ook worden opgegeven. |
| SharedLogSizeInMB |Megabytes |8192 |Hiermee geeft u het aantal MB aan schijfruimte voor het statisch toewijzen voor de gedeelde logboek. De waarde moet 2048 aan of groter zijn. |

In Azure ARM of lokale JSON-sjabloon, het volgende voorbeeld ziet het wijzigen van de het gedeelde transactielogboek die wilt maken van een betrouwbare verzamelingen voor stateful services wordt gemaakt.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>Voorbeeld lokale developer cluster manifest-sectie
Als u dit wijzigen op uw lokale ontwikkelingsomgeving wilt, moet u het bestand lokale clustermanifest.xml bewerken.

```xml
   <Section Name="KtlLogger">
     <Parameter Name="SharedLogSizeInMB" Value="4096"/>
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
   </Section>
```

### <a name="remarks"></a>Opmerkingen
Het logboek heeft een algemene groep geheugen toegewezen uit het voor niet-wisselbaar kernelgeheugen die beschikbaar is voor alle betrouwbare services op een knooppunt voor het opslaan van gegevens van de gebruikersstatus voordat het toegewezen logboek wordt geschreven die is gekoppeld aan de replica betrouwbare service. Grootte van de groep wordt bepaald door de instellingen voor WriteBufferMemoryPoolMinimumInKB en WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB geeft de begingrootte van deze geheugengroep en de grootte van de laagste waarnaar de geheugengroep kan verkleinen. WriteBufferMemoryPoolMaximumInKB is de hoogste grootte waarnaar de geheugengroep kan groeien. Elke replica betrouwbare service die wordt geopend, kan de grootte van de geheugengroep verhogen door een systeem bepaald bedrag tot WriteBufferMemoryPoolMaximumInKB. Als er meer aanvraag voor het geheugen van de geheugengroep dan beschikbaar is, worden aanvragen voor het geheugen wordt uitgesteld tot geheugen beschikbaar is. Daarom als de geheugengroep voor write-buffer te klein voor een specifieke configuratie is vervolgens tot slechtere prestaties.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt voor het definiëren van de GUID en de locatie voor het standaard gedeelde logboek voor alle knooppunten in het cluster. De standaard gedeelde logboek wordt gebruikt voor alle betrouwbare services die de instellingen in de settings.xml voor de specifieke service geen opgeven. Voor de beste prestaties worden gedeelde logboekbestanden geplaatst op schijven die uitsluitend voor de gedeelde logboekbestand gebruikt om te conflicten te verminderen.

SharedLogSizeInMB geeft de hoeveelheid schijfruimte toe te wijzen voor het standaard gedeelde logboek op alle knooppunten.  SharedLogId en SharedLogPath hoeft niet te worden opgegeven in de volgorde voor SharedLogSizeInMB worden opgegeven.

## <a name="service-specific-configuration"></a>Specifieke configuratie van service
U kunt de standaardconfiguraties stateful Reliable Services wijzigen met behulp van het configuratiepakket (configuratie) of de service-implementatie (code).

* **Config** -configuratie via het configuratiepakket wordt bereikt door het wijzigen van het bestand Settings.xml die wordt gegenereerd in de hoofdmap van het pakket Microsoft Visual Studio onder de map configuratie voor elke service in de toepassing.
* **Code** -configuratie via code wordt bereikt door het maken van een ReliableStateManager met behulp van een object ReliableStateManagerConfiguration met de juiste opties ingesteld.

Standaard wordt met de Azure Service Fabric-runtime gezocht naar vooraf gedefinieerde sectienamen in het bestand Settings.xml en de configuratiewaarden verbruikt tijdens het maken van de onderliggende runtimeonderdelen.

> [!NOTE]
> Voer **niet** verwijderen van de namen van de volgende configuraties in het Settings.xml-bestand dat wordt gegenereerd in de Visual Studio-oplossing, tenzij u van plan bent om uw service via programmacode te configureren.
> Naam van de namen van pakket of de sectie configuratie is een codewijziging vereist bij het configureren van de ReliableStateManager.
> 
> 

### <a name="replicator-security-configuration"></a>Replicator Beveiligingsconfiguratie
Beveilig het communicatiekanaal dat wordt gebruikt tijdens de replicatie worden beveiligingsconfiguraties Replicator gebruikt. Dit betekent dat services niet mogelijk om te zien van elkaars replicatieverkeer, waarbij u ervoor zorgt dat de gegevens die maximaal beschikbaar is ook beveiligd is. Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.

### <a name="default-section-name"></a>De naam van de standaard-sectie
ReplicatorSecurityConfig

> [!NOTE]
> Deze als sectienaam wilt wijzigen, vervangt u de parameter replicatorSecuritySectionName aan de constructor ReliableStateManagerConfiguration bij het maken van de ReliableStateManager voor deze service.
> 
> 

### <a name="replicator-configuration"></a>Configuratie van de Replicator
Replicator configuraties configureren de replicator die verantwoordelijk is voor het maken van de status van de stateful betrouwbare Service uiterst betrouwbare door repliceren en de status van de lokaal persistent maken.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. Deze sectie wordt gesproken over aanvullende configuraties die beschikbaar zijn voor het afstemmen van de replicator.

### <a name="default-section-name"></a>De naam van de standaard-sectie
ReplicatorConfig

> [!NOTE]
> Deze als sectienaam wilt wijzigen, vervangt u de parameter replicatorSettingsSectionName aan de constructor ReliableStateManagerConfiguration bij het maken van de ReliableStateManager voor deze service.
> 
> 

### <a name="configuration-names"></a>Configuratienamen
| Naam | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Seconden |0.015 |Periode waarvoor de replicatie op de secundaire Wacht na de ontvangst van een bewerking voor het verzenden van back-een bevestiging naar de primaire. Alle andere bevestigingen voor bewerkingen binnen dit interval verwerkt worden verzonden, worden verzonden als een reactie. |
| ReplicatorEndpoint |N.v.t. |Er is geen standaard--vereiste parameter |IP-adres en poort op die de primaire en secundaire replicatie wordt gebruikt om te communiceren met andere replicaties in de replica is ingesteld. Dit moet verwijzen naar een resource TCP-eindpunt in het servicemanifest. Raadpleeg [Service manifest resources](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van endpoint-resources in een servicemanifest van de. |
| MaxPrimaryReplicationQueueSize |Aantal bewerkingen |8192 |Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicatie een bevestiging van de secundaire replicaties ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2 zijn. |
| MaxSecondaryReplicationQueueSize |Aantal bewerkingen |16384 |Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking wordt vrijgemaakt nadat u de status maximaal beschikbaar is via persistentie. Deze waarde moet groter zijn dan 64 en een macht van 2 zijn. |
| CheckpointThresholdInMB |MB |50 |De hoeveelheid ruimte in logboekbestand waarna de status gecontroleerd wordt. |
| MaxRecordSizeInKB |KB |1024 |Grootste recordgrootte dat de replicatie mogelijk in het logboek schrijven. Deze waarde moet een meervoud van 4 en groter zijn dan 16. |
| MinLogSizeInMB |MB |0 (systeem bepaald) |De minimumgrootte van de transactionele logboek. Het logboek niet mag worden afgekapt tot een grootte dan deze instelling. 0 geeft aan dat de replicator de minimale grootte bepaalt. Deze waarde verhoogt, wordt de mogelijkheid van de handeling gedeeltelijke kopie en incrementele back-ups sinds de kans op relevante logboekrecords worden afgekapt wordt verlaagd. |
| TruncationThresholdFactor |Factor |2 |Bepaalt op welke grootte van het logboek moet worden afgekapt wordt geactiveerd. Afkappen drempelwaarde wordt bepaald door MinLogSizeInMB TruncationThresholdFactor vermenigvuldigd. TruncationThresholdFactor moet groter dan 1 zijn. MinLogSizeInMB * TruncationThresholdFactor moet kleiner dan MaxStreamSizeInMB. |
| ThrottlingThresholdFactor |Factor |4 |Hiermee wordt bepaald welke grootte van het logboek, de replica wordt gestart, wordt beperkt. Bandbreedtebeperking drempelwaarde (in MB) wordt bepaald door de maximale ((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)). Bandbreedtebeperking drempelwaarde (in MB) moet groter zijn dan de drempelwaarde (in MB) moet worden afgekapt. Afkappen drempelwaarde (in MB) moet kleiner zijn dan MaxStreamSizeInMB. |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |Maximale verzameld grootte (in MB) van de back-uplogboeken in een keten van de opgegeven back-uplogboek. Een incrementele back-aanvragen mislukken als de incrementele back-up een back-uplogboek waardoor de totale back-uplogboeken sinds de relevante volledige back-up niet groter zijn dan de grootte van deze genereert. Gebruiker is in dergelijke gevallen vereist voor een volledige back-up te maken. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke GUID moet worden gebruikt voor het identificeren van de gedeelde logboekbestand gebruikt in combinatie met deze replica. Services moeten normaal gesproken niet gebruiken voor deze instelling. Echter, als SharedLogId is opgegeven, klikt u vervolgens SharedLogPath moet ook worden opgegeven. |
| SharedLogPath |Naam van het volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand voor deze replica wordt gemaakt. Services moeten normaal gesproken niet gebruiken voor deze instelling. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moet ook worden opgegeven. |
| SlowApiMonitoringDuration |Seconden |300 |Hiermee stelt u de controle-interval voor beheerde API-aanroepen. Voorbeeld: de gebruiker opgegeven back-callback-functie. Nadat het interval is verstreken, wordt een statusrapport waarschuwing verzonden naar de Health Manager. |

### <a name="sample-configuration-via-code"></a>Voorbeeldconfiguratie via code
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Voorbeeld van configuratiebestand
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Opmerkingen
BatchAcknowledgementInterval bepaalt replicatielatentie. Een waarde van '0' resulteert in de laagst mogelijke latentie koste doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Hoe hoger de waarde voor BatchAcknowledgementInterval, hoe hoger de algehele replicatie doorvoer, koste hogere latentie van de bewerking. Hierdoor kan rechtstreeks aan de latentie van transactie doorvoeracties.

De waarde voor CheckpointThresholdInMB bepaalt de hoeveelheid schijfruimte die de replicator gebruiken kunt voor het opslaan van informatie over de status in de toegewezen logboekbestand van de replica. Dit op een hogere waarde dan de standaardwaarde verhogen kan leiden tot herconfiguratie sneller worden wanneer een nieuwe replica wordt toegevoegd aan de set. Dit komt door de van gedeeltelijke statusoverdracht die uitgevoerd als gevolg van de beschikbaarheid van meer geschiedenis van bewerkingen in het logboek wordt. Hierdoor kan de hersteltijd van een replica mogelijk na een crash toenemen.

De instelling MaxRecordSizeInKB definieert de maximale grootte van een record die kan worden geschreven met de replicatie in het logboekbestand. In de meeste gevallen is de standaardgrootte van 1024 KB record optimaal. Echter, als de service groter gegevensitems deel uitmaken van de informatie over de status veroorzaakt, deze waarde moet mogelijk worden verhoogd. Er is weinig voordeel bij het maken van MaxRecordSizeInKB kleiner zijn dan 1024, zoals kleinere records alleen de benodigde ruimte voor de record kleiner gebruiken. We verwachten dat deze waarde moet worden gewijzigd in alleen zeldzame gevallen.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om een service een apart gedeelde logboek uit het standaard gedeelde logboek gebruiken voor het knooppunt. Voor een optimale efficiency moeten zoveel services mogelijk hetzelfde gedeelde logboek opgeven. Gedeelde logboekbestanden moeten worden geplaatst op schijven die worden gebruikt uitsluitend voor de gedeelde logboekbestand om te conflicten head verkeer beperken. We verwachten dat deze waarde moet worden gewijzigd in alleen zeldzame gevallen.

## <a name="next-steps"></a>Volgende stappen
* [Fouten opsporen in uw Service Fabric-toepassing in Visual Studio](service-fabric-debugging-your-application.md)
* [Referentie voor ontwikkelaars voor Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

