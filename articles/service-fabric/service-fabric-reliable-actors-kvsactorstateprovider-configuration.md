---
title: KVSActorStateProvider instellingen wijzigen in Azure Service Fabric-actoren | Microsoft Docs
description: Meer informatie over het configureren van stateful Azure Service Fabric-actoren van het type KVSActorStateProvider.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: ''
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: a512bb4adc4e410ef2300811ffa4142348d5ee33
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57871815"
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Reliable Actors--KVSActorStateProvider configureren
U kunt de standaardconfiguratie van KVSActorStateProvider wijzigen door het veranderen van de settings.xml-bestand dat wordt gegenereerd in de hoofdmap van het Microsoft Visual Studio-pakket in de Config-map voor de opgegeven actor.

De Azure Service Fabric-runtime vooraf gedefinieerde namen in het bestand settings.xml zoekt en neemt de configuratiewaarden tijdens het maken van de onderliggende runtimeonderdelen.

> [!NOTE]
> Voer **niet** verwijderen of wijzigen van de namen van de volgende configuraties in het settings.xml-bestand dat wordt gegenereerd in Visual Studio-oplossing.
> 
> 

## <a name="replicator-security-configuration"></a>Configuratie van de replicatie-beveiliging
Beveiligingsconfiguraties replicatie worden gebruikt om Beveilig het communicatiekanaal dat wordt gebruikt tijdens de replicatie. Dit betekent dat services elkaars replicatieverkeer, ervoor te zorgen dat de gegevens die maximaal beschikbaar worden gesteld ook beveiligd is niet kunnen zien.
Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.

> [!IMPORTANT]
> Op Linux-knooppunten moeten certificaten een PEM-indeling. Zie voor meer informatie over het zoeken naar en configureren van certificaten voor Linux [certificaten configureren op Linux](./service-fabric-configure-certificates-linux.md). 
> 

### <a name="section-name"></a>Sectienaam
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Configuratie van de replicatie
Configuraties voor replicatie configureren de replicatie dat verantwoordelijk is voor het maken van de status van de Actor State-Provider zeer betrouwbare.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. In deze sectie wordt besproken aanvullende configuraties die beschikbaar zijn voor het afstemmen van de replicatie.

### <a name="section-name"></a>Sectienaam
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Configuratienamen
| Name | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Seconden |0.015 |De periode waarvoor de replicatie op de secundaire Wacht na de ontvangst van een bewerking voor het verzenden van een bevestiging terug naar de primaire. Alle andere bevestigingen voor bewerkingen die worden verwerkt binnen dit interval worden verzonden als één antwoord verzonden. |
| ReplicatorEndpoint |N/A |Er is geen standaard--de vereiste parameter |IP-adres en poort op die de primaire/secundaire replicatie wordt gebruikt om te communiceren met andere distributeurs in de replica instellen. Dit moet verwijzen naar een TCP-resource-eindpunt in het servicemanifest. Raadpleeg [servicemanifest-resources](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van eindpunt resources in het servicemanifest. |
| RetryInterval |Seconden |5 |De periode waarna de replicatie opnieuw een bericht verzendt als er een bevestiging voor een bewerking niet ontvangt. |
| MaxReplicationMessageSize |Bytes |50 MB |Maximale grootte van de replicatiegegevens die kunnen worden overgebracht in één bericht. |
| MaxPrimaryReplicationQueueSize |Aantal bewerkingen |1024 |Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicatie ontvangt een bevestiging van de secundaire distributeurs. Deze waarde moet groter zijn dan 64 en een macht van 2. |
| MaxSecondaryReplicationQueueSize |Aantal bewerkingen |2048 |Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking wordt vrijgemaakt nadat u de status maximaal beschikbaar is via persistentie. Deze waarde moet groter zijn dan 64 en een macht van 2. |

## <a name="store-configuration"></a>Store-configuratie
Store-configuraties worden gebruikt voor het configureren van het lokale archief dat wordt gebruikt om vast te leggen de status die wordt gerepliceerd.
De standaardconfiguratie wordt gegenereerd door de sjabloon voor Visual Studio en moet voldoende zijn. In deze sectie wordt besproken aanvullende configuraties die beschikbaar zijn voor het afstemmen van het lokale archief.

### <a name="section-name"></a>Sectienaam
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Configuratienamen
| Name | Eenheid | Standaardwaarde | Opmerkingen |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |Milliseconden |200 |Hiermee stelt u het interval voor duurzame lokale archief doorvoeringen batchverwerking maximum. |
| MaxVerPages |Aantal pagina 's |16384 |Het maximum aantal pagina's de versie van de lokale database opslaan. Bepaalt het maximum aantal openstaande transacties. |

## <a name="sample-configuration-file"></a>Voorbeeldconfiguratiebestand
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
De parameter BatchAcknowledgementInterval bepaalt replicatielatentie. Een waarde van '0' resulteert in de laagst mogelijke latentie leiden doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Hoe hoger de waarde voor BatchAcknowledgementInterval, hoe hoger de algemene replicatie doorvoer, leiden van hogere latentie van de bewerking. Dit wordt rechtstreeks omgezet in de latentie van transacties doorgevoerd.

