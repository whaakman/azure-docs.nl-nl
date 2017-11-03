---
title: Wijzig de instellingen in Azure microservices KVSActorStateProvider | Microsoft Docs
description: Meer informatie over het configureren van stateful actoren van het type KVSActorStateProvider Azure Service Fabric.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: d3424aa7a8e0f6011bbef4aa61274c1f598f5c86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Reliable Actors--KVSActorStateProvider configureren
U kunt de standaardconfiguratie van KVSActorStateProvider wijzigen door het bestand settings.xml die wordt gegenereerd in de hoofdmap van het pakket Microsoft Visual Studio onder de map configuratie voor de opgegeven actor wordt gewijzigd.

De Azure Service Fabric-runtime gezocht naar vooraf gedefinieerde sectienamen in het bestand settings.xml en de configuratiewaarden verbruikt tijdens het maken van de onderliggende runtimeonderdelen.

> [!NOTE]
> Voer **niet** verwijderen of wijzigen van de namen van de volgende configuraties in het settings.xml-bestand dat wordt gegenereerd in de Visual Studio-oplossing.
> 
> 

## <a name="replicator-security-configuration"></a>Replicator Beveiligingsconfiguratie
Beveilig het communicatiekanaal dat wordt gebruikt tijdens de replicatie worden beveiligingsconfiguraties Replicator gebruikt. Dit betekent dat services elkaars replicatieverkeer, om te garanderen dat de gegevens die maximaal beschikbaar is ook beveiligde kunnen niet zien.
Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.

### <a name="section-name"></a>De sectienaam van de
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Configuratie van de Replicator
Replicator configuraties configureren de replicator die verantwoordelijk is voor het maken van de status van de Actor State-Provider uiterst betrouwbaar.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. Deze sectie wordt gesproken over aanvullende configuraties die beschikbaar zijn voor het afstemmen van de replicator.

### <a name="section-name"></a>De sectienaam van de
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Configuratienamen
| Naam | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Seconden |0.015 |Periode waarvoor de replicatie op de secundaire Wacht na de ontvangst van een bewerking voor het verzenden van back-een bevestiging naar de primaire. Alle andere bevestigingen voor bewerkingen binnen dit interval verwerkt worden verzonden, worden verzonden als een reactie. |
| ReplicatorEndpoint |N.v.t. |Er is geen standaard--vereiste parameter |IP-adres en poort op die de primaire en secundaire replicatie wordt gebruikt om te communiceren met andere replicaties in de replica is ingesteld. Dit moet verwijzen naar een resource TCP-eindpunt in het servicemanifest. Raadpleeg [Service manifest resources](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van endpoint-resources in het servicemanifest. |
| retryInterval |Seconden |5 |De periode waarna de replicator opnieuw een bericht verzendt als deze niet een bevestiging voor een bewerking ontvangt. |
| MaxReplicationMessageSize |Bytes |50 MB |Maximale grootte van de replicatiegegevens die kunnen worden overgebracht in één bericht. |
| MaxPrimaryReplicationQueueSize |Aantal bewerkingen |1024 |Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicatie een bevestiging van de secundaire replicaties ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2 zijn. |
| MaxSecondaryReplicationQueueSize |Aantal bewerkingen |2048 |Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking wordt vrijgemaakt nadat u de status maximaal beschikbaar is via persistentie. Deze waarde moet groter zijn dan 64 en een macht van 2 zijn. |

## <a name="store-configuration"></a>Configuratie van gegevensarchief
Store-configuraties worden gebruikt voor het configureren van het lokale archief dat wordt gebruikt om vast te leggen van de status die wordt gerepliceerd.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. Deze sectie wordt gesproken over aanvullende configuraties die beschikbaar zijn voor het afstemmen van het lokale archief.

### <a name="section-name"></a>De sectienaam van de
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Configuratienamen
| Naam | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |milliseconden |200 |Hiermee stelt u het interval voor duurzame lokale archief doorvoeracties batchverwerking maximum. |
| MaxVerPages |Het aantal pagina 's |16384 |Het maximum aantal pagina's de versie van de lokale database worden opgeslagen. Bepaalt het maximum aantal openstaande transacties. |

## <a name="sample-configuration-file"></a>Voorbeeld van configuratiebestand
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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

