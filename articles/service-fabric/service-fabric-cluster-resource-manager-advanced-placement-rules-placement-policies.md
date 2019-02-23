---
title: Service Fabric Cluster Resource Manager - beleid voor Serviceplaatsing | Microsoft Docs
description: Overzicht van de van extra plaatsingsbeleid en regels voor het Service Fabric-Services
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 2858628874dc9955db5084ef5732d85acd6e7fc1
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729782"
---
# <a name="placement-policies-for-service-fabric-services"></a>Beleid voor serviceplaatsing voor service fabric-services
Beleid voor serviceplaatsing zijn extra regels die kunnen worden gebruikt om te bepalen van de plaatsing van services in sommige scenario's voor specifieke, minder gebruikte. Enkele voorbeelden van deze scenario's zijn:

- Uw Service Fabric-cluster omvat geografische afstand, zoals meerdere on-premises datacenters of Azure-regio 's
- Uw omgeving omvat meerdere gebieden van geopolitieke of een juridische of enige andere gevallen waarin u werkt met beleid grenzen die u nodig hebt om af te dwingen
- Er zijn communicatie prestaties of de latentie-overwegingen vanwege de grote afstand of het gebruik van langzamer of minder betrouwbaar netwerkkoppelingen
- U wilt houden van bepaalde werkbelastingen geplaatst als een best-effort, met andere werkbelastingen of in de buurt voor klanten

De meeste van deze vereisten uitgelijnd met de fysieke indeling van het cluster, weergegeven als de domeinen met fouten van het cluster. 

De plaatsing van Geavanceerde beleidsregels die helpen bij de volgende scenario's:

1. Ongeldige domeinen
2. Vereiste domeinen
3. Voorkeur domeinen
4. Replica packing verbieden

De meeste van de volgende besturingselementen kan worden geconfigureerd via de eigenschappen van het knooppunt en plaatsingsbeperkingen, maar sommige ingewikkelder zijn. De Service Fabric Cluster Resource Manager biedt eenvoudiger dingen, deze extra plaatsingsbeleid. Beleid voor serviceplaatsing zijn geconfigureerd op basis van per-met de naam van service-exemplaar. Ze kunnen ook dynamisch worden bijgewerkt.

## <a name="specifying-invalid-domains"></a>Ongeldige domeinen op te geven
De **InvalidDomain** plaatsing beleid kunt u opgeven dat een bepaalde Foutdomein ongeldig voor een specifieke service is. Dit beleid zorgt ervoor dat een bepaalde service nooit wordt uitgevoerd in een bepaald gebied, bijvoorbeeld voor voor geopolitieke of zakelijke beleidsredenen. Meerdere ongeldige domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

<center>
![Voorbeeld van een domein is ongeldig][Image1]
</center>

Code:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Vereiste domeinen op te geven
Het beleid van de plaatsing vereist domein is vereist dat de service aanwezig alleen in het opgegeven domein is. Meerdere vereiste domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

<center>
![Voorbeeld van de vereiste domein][Image2]
</center>

Code:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a>Een voorkeur voor de primaire replica's van een stateful service-domein op te geven
Het primaire domein met de voorkeur geeft aan dat het foutdomein voor het plaatsen van de primaire in. De primaire eindigt om in dit domein wanneer alles in orde is. Als het domein of de primaire replica mislukt of wordt afgesloten, wordt de primaire verplaatst naar een andere locatie in het ideale geval in hetzelfde domein. Als deze nieuwe locatie zich niet in de gewenste domein, met Cluster Resource Manager worden verplaatst deze terug naar het gewenste domein zo snel mogelijk. Deze instelling maakt op een natuurlijke manier alleen geschikt zijn voor stateful services. Dit beleid is vooral nuttig zijn in de clusters die zijn liep Azure-regio's of meerdere datacenters, maar zijn services die liever plaatsing op een bepaalde locatie. Primaire houden dicht bij hun gebruikers- of andere services bieden helpt lagere latentie met name voor lezen, die standaard worden verwerkt door voorverkiezingen uit te brengen.

<center>
![Voorkeur primaire domeinen en Failover][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(primaryDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Replica distributie vereisen en verpakking verbieden
Replica's _normaal_ verdeeld over fout- en upgradedomeinen als het cluster in orde is. Er zijn echter gevallen waar meer dan één replica voor een bepaalde partitie er uiteindelijk tijdelijk verpakt in een enkel domein. Bijvoorbeeld, laten we zeggen dat het cluster negen knooppunten, in drie foutdomeinen, fd: / 0, fd: / 1 en fd: / 2. Stel dat uw service drie replica's heeft. Laten we zeggen dat de knooppunten die zijn gebruikt voor deze replica's in fd: / 1 en fd: / 2 werd afgesloten. Normaal gesproken liever met Cluster Resource Manager de andere knooppunten in de dezelfde domeinen met fouten. In dit geval, stel vanwege capaciteitsproblemen dat geen van de andere knooppunten in de domeinen zijn geldig. Als de Cluster Resource Manager gebaseerd vervangingen voor deze replica's, moeten deze zou knooppunten kiezen in fd: / 0. Echter, doen _die_ maakt u een situatie waarbij het Foutdomein-beperking is geschonden. Replica's verhoogt kan de kans dat het hele replicaset uitvallen of verloren. 

> [!NOTE]
> Voor meer informatie over de beperkingen en beperking in het algemeen, bekijk van prioriteiten [in dit onderwerp](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Als u ooit een bericht hebt gezien status, zoals '`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`', en vervolgens deze voorwaarde of er iets is bereikt. Meestal slechts één of twee replica's worden bij elkaar verpakt tijdelijk. Zo lang er zijn minder dan een quorum van replica's in een bepaald domein, u kunt veilig. Verpakking is zeldzaam, maar dit kan gebeuren en deze situaties zijn meestal tijdelijke omdat de knooppunten keert u terug. Als de knooppunten naar beneden blijven en met Cluster Resource Manager moet vervangingen bouwen, zijn meestal er andere knooppunten beschikbaar in het ideale foutdomeinen.

Sommige werkbelastingen liever altijd met het doelgetal van replica's, zelfs als ze zijn verpakt in minder domeinen. Deze werkbelastingen worden verwacht tegen storingen in totaal aantal gelijktijdige permanent domein en lokale staat gewoonlijk kunnen herstellen. Andere werkbelastingen rekening in plaats daarvan de downtime ouder is dan het risico op juistheid of het verlies van gegevens. De meeste productieworkloads uitvoeren met meer dan drie replica's, meer dan drie foutdomeinen en veel geldig knooppunten per domein met fouten. Als gevolg hiervan kan het standaardgedrag domein packing standaard. Het standaardgedrag kunt normale taakverdeling en failover voor het afhandelen van dergelijke uitzonderlijke gevallen, zelfs als dit betekent dat tijdelijke domein verpakken.

Als u uitschakelen van dergelijke verpakking voor een bepaalde workload wilt, kunt u opgeven de `RequireDomainDistribution` beleid op de service. Wanneer dit beleid is ingesteld, met Cluster Resource Manager zorgt ervoor dat er geen twee replica's van dezelfde partitie worden uitgevoerd in hetzelfde domein bevinden of upgrade van de fout.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nu, zou het niet mogelijk met gebruik van deze configuraties voor services in een cluster dat is niet geografisch omspannen? U kunt de onderstaande, maar er is een goede reden te. De domeinconfiguraties vereist, ongeldig en voorkeur moeten worden vermeden, tenzij de moeten de gebruiker de scenario's. Het logisch niet om te proberen om af te dwingen een bepaalde workload om uit te voeren in een enkele rack of sommige segment van uw lokale cluster liever via een andere. Configuraties van andere hardware moeten worden verdeeld over foutdomeinen en verwerkt via normale plaatsingsbeperkingen en eigenschappen van het knooppunt.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
