---
title: Azure Service Fabric Reliable Services configureren | Microsoft Docs
description: Meer informatie over het configureren van stateful Reliable Services in Azure Service Fabric.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: chackdan
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: sumukhs
ms.openlocfilehash: 97cf4fafb53156eec654bcc67cd8dccf3d973c32
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660694"
---
# <a name="configure-stateful-reliable-services"></a>Stateful reliable services configureren
Er zijn twee sets met configuratie-instellingen voor betrouwbare services. Één set is globale voor alle betrouwbare services in het cluster, terwijl de andere set specifiek voor een bepaalde betrouwbare service is.

## <a name="global-configuration"></a>Algemene configuratie
Configuratie van de algemene betrouwbare service is opgegeven in het clustermanifest voor het cluster onder de sectie KtlLogger. Hierdoor kan de configuratie van de gedeelde locatie en grootte plus de globale geheugenlimieten die worden gebruikt door het logboek. Het clustermanifest is een XML-bestand waarin de instellingen en configuraties die van toepassing op alle knooppunten en services in het cluster zijn. Het bestand wordt doorgaans ClusterManifest.xml genoemd. U kunt zien dat het cluster het manifest van uw cluster met behulp van de powershell-opdracht Get-ServiceFabricClusterManifest.

### <a name="configuration-names"></a>Configuratienamen
| Name | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobytes |8388608 |Minimum aantal KB in de kernelmodus voor de logger schrijven bufferpool geheugen toewijzen. Deze geheugengroep wordt gebruikt voor het opslaan van informatie over de status voor schrijven naar schijf. |
| WriteBufferMemoryPoolMaximumInKB |Kilobytes |Geen limiet |Maximale grootte waarvoor het logboek schrijven buffergroep geheugen te vergroten. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke GUID moet worden gebruikt voor het identificeren van de gedeelde standaardlogboekbestand gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogId in hun specifieke configuratie van de service geen opgeven. Als SharedLogId is opgegeven, moeten klikt u vervolgens SharedLogPath ook worden opgegeven. |
| SharedLogPath |De naam van de volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand wordt gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogPath in hun specifieke configuratie van de service geen opgeven. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moeten ook worden opgegeven. |
| SharedLogSizeInMB |Megabytes |8192 |Hiermee geeft u het aantal MB aan schijfruimte vrij statisch toewijzen voor het gedeelde logboek. De waarde moet 2048 of groter. |

In Azure ARM- of on-premises JSON-sjabloon ziet in het volgende voorbeeld u het wijzigen van de gedeelde transactielogboek die back-ups maken geen betrouwbare verzamelingen voor stateful services wordt gemaakt.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>Voorbeeld van lokale developer cluster manifest-sectie
Als u dit wijzigen op uw lokale ontwikkelomgeving wilt, moet u het bestand lokaal clustermanifest.xml bewerken.

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
Het logboek heeft een globale groep van het geheugen toegewezen vanuit de van niet-wisselbaar kernelgeheugen die beschikbaar is voor alle betrouwbare services op een knooppunt voor het opslaan van gegevens van de gebruikersstatus voordat wordt geschreven naar het logboek voor toegewezen die zijn gekoppeld aan de betrouwbare service-replica. Grootte van de groep wordt bepaald door de instellingen voor WriteBufferMemoryPoolMinimumInKB en WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB Hiermee geeft u zowel de aanvankelijke grootte van deze geheugengroep van en de grootte van de laagste waaraan de geheugengroep kan verkleinen. WriteBufferMemoryPoolMaximumInKB is de grootte van de hoogste waaraan de geheugengroep kan groeien. Elke replica betrouwbare service die is geopend verhogen met een bepaald systeem bedrag maximaal WriteBufferMemoryPoolMaximumInKB de grootte van de geheugengroep. Als er meer vraag naar geheugen van de geheugengroep dan beschikbaar is, wordt er aanvragen voor het geheugen worden uitgesteld totdat geheugen beschikbaar is. Dus als de geheugengroep schrijven-buffer te klein voor een specifieke configuratie is vervolgens tot slechtere prestaties.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt voor het definiëren van de GUID en de locatie voor het standaard gedeelde logboek voor alle knooppunten in het cluster. Het standaard gedeelde logboek wordt gebruikt voor alle betrouwbare services die de instellingen in de settings.xml voor de specifieke service geen opgeven. Voor de beste prestaties moeten de gedeelde bestanden worden geplaatst op schijven die uitsluitend voor de gedeelde logboekbestand worden gebruikt voor het aantal conflicten verminderen.

SharedLogSizeInMB geeft de hoeveelheid schijfruimte toe te wijzen voor het standaard gedeelde logboek op alle knooppunten.  SharedLogId en SharedLogPath hoeft te worden opgegeven in de volgorde voor SharedLogSizeInMB worden opgegeven.

## <a name="service-specific-configuration"></a>Configuratie-specifieke service
U kunt de standaardconfiguraties stateful Reliable Services wijzigen met behulp van het configuratiepakket (configuratie) of de service-implementatie (code).

* **Config** -configuratie via het configuratiepakket wordt bereikt door het wijzigen van het bestand Settings.xml die wordt gegenereerd in de hoofdmap van het pakket Microsoft Visual Studio onder de map configuratie voor elke service in de toepassing.
* **Code** -configuratie via code wordt bereikt door het maken van een ReliableStateManager met behulp van een ReliableStateManagerConfiguration-object met de juiste opties ingesteld.

Standaard worden de Azure Service Fabric-runtime vooraf gedefinieerde namen in het bestand Settings.xml zoekt en de configuratiewaarden verbruikt tijdens het maken van de onderliggende runtimeonderdelen.

> [!NOTE]
> Voer **niet** verwijderen van de namen van de volgende configuraties in het Settings.xml-bestand dat wordt gegenereerd in Visual Studio-oplossing, tenzij u van plan bent om uw service via code te configureren.
> Naam van de namen van pakket of de sectie configuratie is een codewijziging vereist bij het configureren van de ReliableStateManager.
> 
> 

### <a name="replicator-security-configuration"></a>Configuratie van de replicatie-beveiliging
Beveiligingsconfiguraties replicatie worden gebruikt om Beveilig het communicatiekanaal dat wordt gebruikt tijdens de replicatie. Dit betekent dat services is niet mogelijk om te zien van elkaars replicatieverkeer, ervoor te zorgen dat de gegevens die maximaal beschikbaar worden gesteld ook beveiligd is. Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.

> [!IMPORTANT]
> Op Linux-knooppunten moeten certificaten een PEM-indeling. Zie voor meer informatie over het zoeken naar en configureren van certificaten voor Linux [certificaten configureren op Linux](./service-fabric-configure-certificates-linux.md). 
> 
> 

### <a name="default-section-name"></a>De naam van de standaard-sectie
ReplicatorSecurityConfig

> [!NOTE]
> Als u wilt wijzigen de naam van deze sectie, vervangt u de parameter replicatorSecuritySectionName aan de constructor ReliableStateManagerConfiguration bij het maken van de ReliableStateManager voor deze service.
> 
> 

### <a name="replicator-configuration"></a>Configuratie van de replicatie
Configuraties voor replicatie configureren de replicatie dat verantwoordelijk is voor de status van de stateful betrouwbare Service zeer betrouwbare maken door deze te repliceren en het behouden van de status lokaal.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. In deze sectie wordt besproken aanvullende configuraties die beschikbaar zijn voor het afstemmen van de replicatie.

### <a name="default-section-name"></a>De naam van de standaard-sectie
ReplicatorConfig

> [!NOTE]
> Als u wilt wijzigen de naam van deze sectie, vervangt u de parameter replicatorSettingsSectionName aan de constructor ReliableStateManagerConfiguration bij het maken van de ReliableStateManager voor deze service.
> 
> 

### <a name="configuration-names"></a>Configuratienamen
| Name | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Seconden |0.015 |De periode waarvoor de replicatie op de secundaire Wacht na de ontvangst van een bewerking voor het verzenden van een bevestiging terug naar de primaire. Alle andere bevestigingen voor bewerkingen die worden verwerkt binnen dit interval worden verzonden als één antwoord verzonden. |
| ReplicatorEndpoint |N/A |Er is geen standaard--de vereiste parameter |IP-adres en poort op die de primaire/secundaire replicatie wordt gebruikt om te communiceren met andere distributeurs in de replica instellen. Dit moet verwijzen naar een TCP-resource-eindpunt in het servicemanifest. Raadpleeg [servicemanifest-resources](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van eindpunt resources in een servicemanifest. |
| MaxPrimaryReplicationQueueSize |Aantal bewerkingen |8192 |Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicatie ontvangt een bevestiging van de secundaire distributeurs. Deze waarde moet groter zijn dan 64 en een macht van 2. |
| MaxSecondaryReplicationQueueSize |Aantal bewerkingen |16384 |Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking wordt vrijgemaakt nadat u de status maximaal beschikbaar is via persistentie. Deze waarde moet groter zijn dan 64 en een macht van 2. |
| CheckpointThresholdInMB |MB |50 |De hoeveelheid ruimte in logboekbestand waarna de status gecontroleerd wordt. |
| MaxRecordSizeInKB |KB |1024 |Grootste recordgrootte die de replicatie in het logboek kan schrijven. Deze waarde moet een meervoud van 4 en hoger zijn dan 16. |
| MinLogSizeInMB |MB |0 (systeem bepaald) |Minimale grootte van het logboek voor transactionele. Het logboek wordt niet toegestaan in een grootte onder deze instelling worden afgekapt. 0 geeft aan dat de minimale grootte wordt bepaald door de replicatie. Deze waarde verhoogt, wordt de mogelijkheid om het gedeeltelijke kopie maken en incrementele back-ups sinds de kans op relevante logboekrecords worden afgekapt worden verlaagd. |
| TruncationThresholdFactor |Multi-factor Authentication |2 |Bepaalt op welke grootte van het logboek moet worden afgekapt wordt geactiveerd. Drempelwaarde voor afkapping wordt bepaald door MinLogSizeInMB TruncationThresholdFactor vermenigvuldigd. TruncationThresholdFactor moet groter zijn dan 1 zijn. MinLogSizeInMB * TruncationThresholdFactor moet kleiner dan MaxStreamSizeInMB. |
| ThrottlingThresholdFactor |Multi-factor Authentication |4 |Hiermee bepaalt u welke grootte van het logboek, de replica wordt gestart, wordt beperkt. Bandbreedtebeperking drempelwaarde (in MB) wordt bepaald door de maximale ((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)). Bandbreedtebeperking drempelwaarde (in MB) moet groter zijn dan de drempelwaarde (in MB) moet worden afgekapt. Afkapping van drempelwaarde (in MB) moet kleiner zijn dan MaxStreamSizeInMB. |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |Maximale grootte (in MB) van de back-Logboeken in een bepaalde back-uplogboeken bij elkaar opgeteld. Een incrementele back-aanvragen mislukken als de incrementele back-up een back-uplogboek die kan leiden de totale back-uplogboeken sinds de relevante volledige back-up dat ertoe moet groter zijn dan deze grootte genereert. In dergelijke gevallen is de gebruiker een volledige back-up vereist. |
| SharedLogId |GUID |"" |Hiermee geeft u een unieke GUID moet worden gebruikt voor het identificeren van de gedeelde logboekbestand gebruikt in combinatie met deze replica. Normaal gesproken moeten deze instelling niet gebruikt door services. Echter, als SharedLogId is opgegeven, klikt u vervolgens SharedLogPath moeten ook worden opgegeven. |
| SharedLogPath |De naam van de volledig gekwalificeerde pad |"" |Hiermee geeft u de volledig gekwalificeerde pad waar het gedeelde logboekbestand voor deze replica wordt gemaakt. Normaal gesproken moeten deze instelling niet gebruikt door services. Echter, als SharedLogPath is opgegeven, klikt u vervolgens SharedLogId moeten ook worden opgegeven. |
| SlowApiMonitoringDuration |Seconden |300 |Hiermee stelt u de controle-interval voor beheerde API-aanroepen. Voorbeeld: de gebruiker opgegeven back-callback-functie. Nadat het interval is verstreken, wordt een statusrapport waarschuwing verzonden naar de Health Manager. |
| LogTruncationIntervalSeconds |Seconden |0 |Configureerbare interval op waarin de logboekbestanden moet worden afgekapt op elke replica wordt gestart. Het wordt gebruikt om ervoor te zorgen logboek ook is afgekapt op basis van tijd in plaats van alleen de grootte van het logboekbestand. Deze instelling dwingt ook opschonen van verwijderde items in een betrouwbare dictionary. Daarom kan deze worden gebruikt om te controleren of verwijderde items tijdig worden opgeschoond. |

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


### <a name="sample-configuration-file"></a>Voorbeeldconfiguratiebestand
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
Replicatievertraging van BatchAcknowledgementInterval besturingselementen. Een waarde van '0' resulteert in de laagst mogelijke latentie leiden doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Hoe hoger de waarde voor BatchAcknowledgementInterval, hoe hoger de algemene replicatie doorvoer, leiden van hogere latentie van de bewerking. Dit wordt rechtstreeks omgezet in de latentie van transacties doorgevoerd.

De waarde voor CheckpointThresholdInMB bepaalt de hoeveelheid schijfruimte die de replicatie gebruiken kunt voor het opslaan van informatie over de status in de toegewezen logboekbestand van de replica. Dit op een hogere waarde dan de standaardwaarde verhogen kan leiden tot snellere herconfiguratie keer wanneer een nieuwe replica wordt toegevoegd aan de set. Dit komt door de gedeeltelijke statusoverdracht die uitgevoerd vanwege de beschikbaarheid van meer geschiedenis van activiteiten in het logboek wordt. Hierdoor kan de hersteltijd van een replica mogelijk na een crash toenemen.

De instelling MaxRecordSizeInKB bepaalt de maximale grootte van een record die kan worden geschreven met de replicatie in het logboekbestand. In de meeste gevallen is de standaardgrootte van 1024 KB record optimaal. Echter, als de service wordt veroorzaakt door grotere gegevensitems moet deel uitmaken van de informatie over de status, deze waarde moet mogelijk worden verhoogd. Er is weinig voordeel bij het maken van MaxRecordSizeInKB kleiner zijn dan 1024, kleinere records gebruiken alleen de benodigde ruimte voor de kleinere record. We verwachten dat deze waarde moet worden gewijzigd in alleen zeldzame gevallen.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om een service waarmee een afzonderlijke gedeelde logboek van het standaard gedeelde logboek voor het knooppunt. Voor maximale efficiency moeten zo veel services mogelijk hetzelfde gedeelde logboek opgeven. Gedeelde logboekbestanden moeten worden geplaatst op schijven die uitsluitend voor de gedeelde logboekbestand worden gebruikt voor het aantal bewegingen conflicten verminderen. We verwachten dat deze waarde moet worden gewijzigd in alleen zeldzame gevallen.

## <a name="next-steps"></a>Volgende stappen
* [Fouten opsporen in uw Service Fabric-toepassing in Visual Studio](service-fabric-debugging-your-application.md)
* [Referentie voor ontwikkelaars voor Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

