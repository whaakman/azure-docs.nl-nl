---
title: Service Fabric Cluster Resource Manager - Plaatsingsbeleid | Microsoft Docs
description: Overzicht van de van extra plaatsingsbeleid en de regels voor Service Fabric-Services
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c240643d2a7ce98ddd7f7871eeef654cced953f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="placement-policies-for-service-fabric-services"></a>Plaatsingsbeleid voor service fabric-services
Plaatsing beleidsregels zijn extra regels die kunnen worden gebruikt om te bepalen van de plaatsing van de service in een aantal specifieke, minder algemene scenario's. Er zijn enkele voorbeelden van de scenario's:

- Service Fabric-cluster omvat geografische afstanden, zoals meerdere lokale datacenters of tussen Azure-regio 's
- Uw omgeving omvat meerdere gebieden van geopolitieke of een juridische of enige andere geval waar u beleid hebt grenzen die u wilt afdwingen
- Er zijn communicatie prestaties of de latentie-overwegingen vanwege grote afstanden of het gebruik van een tragere of minder betrouwbaar netwerkkoppelingen
- U moet ervoor zorgen bepaalde werkbelastingen geplaatste als een zo goed mogelijke poging met andere werkbelastingen of in de buurt aan klanten

De meeste van deze vereisten zijn afgestemd op de fysieke indeling van het cluster, weergegeven als de domeinen met fouten van het cluster. 

De geavanceerde plaatsing beleidsregels die helpen bij de volgende scenario's:

1. Ongeldige domeinen
2. Vereiste domeinen
3. Voorkeursdomeinen
4. Replica verpakking verbieden

De meeste van de volgende besturingselementen kan worden geconfigureerd via de eigenschappen van het knooppunt en plaatsingsbeperkingen, maar sommige ingewikkelder. Voor een eenvoudiger dingen het Service Fabric Cluster Resource Manager biedt deze extra plaatsingsbeleid. Plaatsing beleidsregels zijn geconfigureerd op basis van service per benoemde exemplaar. Ze kunnen ook dynamisch worden bijgewerkt.

## <a name="specifying-invalid-domains"></a>Ongeldige domeinen opgeven
De **InvalidDomain** plaatsing beleid kunt u opgeven dat een bepaalde Foutdomein ongeldig voor een specifieke service is. Dit beleid zorgt ervoor dat een bepaalde service nooit wordt uitgevoerd op een bepaald gebied, bijvoorbeeld voor geopolitieke of zakelijke beleidsredenen. Meerdere ongeldige domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

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
## <a name="specifying-required-domains"></a>Vereiste domeinen opgeven
Het vereiste domeinbeleid plaatsing vereist dat de service alleen toegestaan in het opgegeven domein is. Meerdere vereist domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

<center>
![Voorbeeld van een domein te verstrekken][Image2]
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

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a>Een voorkeur domein voor de primaire replica's van een stateful service opgeven
Het primaire domein voor de voorkeur geeft het foutdomein voor de primaire in plaatsen. De primaire belandt in dit domein als alles goed. Als het domein of de primaire replica mislukt of is afgesloten, wordt de primaire verplaatst naar een andere locatie in het ideale geval in hetzelfde domein. Als deze nieuwe locatie niet in het gewenste domein, verplaatst de Cluster Resource Manager terug naar het gewenste domein zo snel mogelijk. Deze instelling is natuurlijk alleen wel zinvol voor stateful services. Dit beleid is vooral nuttig in clusters die zijn spanned over verschillende Azure-regio's of meerdere datacenters maar services die liever plaatsing op een bepaalde locatie hebben. Primaire houden dicht bij hun gebruikers- of andere services bieden helpt lagere latentie met name voor leesbewerkingen die zijn verwerkt door de primaire standaard.

<center>
![Primaire Voorkeursdomeinen en Failover][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Replica distributiepunten vereisen en verbieden verpakking
Replica's _normaal_ verdeeld over de fout en upgrade domeinen als het cluster goed. Er zijn echter gevallen waarbij meer dan één replica voor een bepaalde partitie uiteindelijk tijdelijk ingepakte in één domein. Bijvoorbeeld, Stel dat het cluster negen knooppunten in drie domeinen met fouten, fd heeft: / 0, fd: / 1 en fd: / 2. Stel dat uw service drie replica's heeft. Stel dat de knooppunten die zijn gebruikt voor deze replica's in fd: / 1 en fd: / 2 werd afgesloten. De Cluster Resource Manager liever normaal andere knooppunten in die dezelfde domeinen met fouten. In dit geval Stel vanwege capaciteitsproblemen met de van dat de andere knooppunten in die domeinen zijn geldig. Als de Cluster Resource Manager builds vervangingen voor deze replica's, zou het moeten Kies knooppunten in fd: / 0. Echter, doen _die_ maakt een situatie waarin de beperking van het Foutdomein wordt geschonden. Inpakken van replica's verhoogt kan de kans dat de hele replicaset uitvallen of verloren gaan. 

> [!NOTE]
> Voor meer informatie over de beperkingen en beperking in het algemeen uitchecken van prioriteiten [in dit onderwerp](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Als u ooit zag een health-bericht, zoals '`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`', en vervolgens hebt u dit probleem of ongeveer het bereikt. Meestal slechts één of twee replica's zijn bijeengebracht tijdelijk. Zolang er minder dan een quorum van replica's in een bepaald domein zijn, bent u veilig. Verpakking zelden voorkomt, maar dit kan gebeuren en meestal deze situaties tijdelijke omdat de knooppunten keert u terug. Als de knooppunten omlaag blijven en de Cluster Resource Manager moet vervangingen bouwen, zijn meestal er andere knooppunten beschikbaar in het ideale foutdomeinen.

Sommige werkbelastingen liever altijd met het doelaantal replica's, zelfs als ze worden verpakt in minder domeinen. Deze werkbelastingen zijn gokken tegen fouten in de totale gelijktijdige permanente domeinnaam en lokale staat gewoonlijk kunnen herstellen. Andere werkbelastingen in plaats daarvan voert u de uitvaltijd ouder is dan het risico op juistheid of verlies van gegevens. De meeste productie-workloads uitvoeren met meer dan drie replica's, meer dan drie domeinen met fouten en veel geldig knooppunten per domein met fouten. Daarom kan het standaardgedrag domein verpakking standaard. Het standaardgedrag kunt normale taakverdeling en failover voor het afhandelen van deze uitzonderlijke gevallen, zelfs als dit betekent dat de tijdelijke domein verpakken.

Als u uitschakelen voor een bepaalde werkbelasting samenhangende wilt, kunt u de `RequireDomainDistribution` beleid op de service. Wanneer deze dit beleid is ingesteld, wordt de Cluster Resource Manager dat geen twee replica's uit dezelfde partitie worden uitgevoerd in hetzelfde domein of upgrade van de fout.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nu het mogelijk om deze configuraties voor services in een cluster dat is niet geografisch omspannen gebruiken? U kunt, maar er is een goede reden te. De vereist, is ongeldig en aanbevolen domeinconfiguraties moeten worden vermeden, tenzij ze in de scenario's nodig hebt. Niet dat u eventuele om te proberen voor een bepaalde werkbelasting in één rack uit te voeren of om de voorkeur sommige segment van uw lokale cluster op een andere te forceren. Verschillende hardwareconfiguraties moeten worden verdeeld over de domeinen met fouten en verwerkt via de normale plaatsingsbeperkingen en eigenschappen van het knooppunt.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
