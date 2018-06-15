---
title: Azure Service Fabric resource governance voor containers en -services | Microsoft Docs
description: Azure Service Fabric kunt u limieten voor services die worden uitgevoerd binnen of buiten containers opgeven.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 49c7e2c99cce13880781a67806543b1cde0c12b6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208009"
---
# <a name="resource-governance"></a>Resourcebeheer 

Wanneer u meerdere services op hetzelfde knooppunt of cluster uitvoert, is het mogelijk dat één service kan worden gebruikt voor het gebruik van meer bronnen, andere services in het proces voldoende bronnen kunnen beschikken. Dit probleem wordt aangeduid als het probleem 'ruis neighbor'. Azure Service Fabric kunt de ontwikkelaar reserveringen en limieten per service en om te garanderen resources beperkt gebruik van bronnen op te geven.

> Voordat u met dit artikel doorgaat, wordt u aangeraden dat u met raken de [Service Fabric-toepassingsmodel](service-fabric-application-model.md) en de [Service Fabric-hostingmodel](service-fabric-hosting-model.md).
>

## <a name="resource-governance-metrics"></a>Metrische gegevens voor het beheer van resources 

Resource governance wordt ondersteund in Service Fabric in overeenstemming met de [servicepakket](service-fabric-application-model.md). De resources die zijn toegewezen aan het servicepakket kunnen verder worden onderverdeeld tussen code pakketten. De resource-limieten die zijn opgegeven betekent ook dat de reservering van de resources. Service Fabric ondersteunt het opgeven van CPU en geheugen per pakket van de service, met twee ingebouwde [metrische gegevens](service-fabric-cluster-resource-manager-metrics.md):

* *CPU* (metrische naam `servicefabric:/_CpuCores`): een logische core die beschikbaar is op de hostmachine. Alle kernen op alle knooppunten worden gewogen hetzelfde.

* *Geheugen* (metrische naam `servicefabric:/_MemoryInMB`): geheugen wordt uitgedrukt in megabytes en het fysieke geheugen die beschikbaar is op de machine is gekoppeld.

Voor deze twee metrieken [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) houdt cluster totale capaciteit, de belasting van elk knooppunt in het cluster en de resterende resources in het cluster. Deze twee metrische gegevens zijn equivalent aan een andere gebruiker of een aangepaste metrische gegevens. Alle bestaande functies kunnen worden gebruikt met hen:
* Het cluster kan worden [met gelijke taakverdeling](service-fabric-cluster-resource-manager-balancing.md) volgens deze twee metrische gegevens (standaardinstelling).
* Het cluster kan worden [gedefragmenteerd](service-fabric-cluster-resource-manager-defragmentation-metrics.md) volgens deze twee metrische gegevens.
* Wanneer [met een beschrijving van een cluster](service-fabric-cluster-resource-manager-cluster-description.md), gebufferde capaciteit kan worden ingesteld voor deze twee metrische gegevens.

[Rapportage van de dynamische belasting bij](service-fabric-cluster-resource-manager-metrics.md) wordt niet ondersteund voor deze metrische gegevens en worden voor deze metrische gegevens zijn gedefinieerd tijdens het maken worden geladen.

## <a name="resource-governance-mechanism"></a>Resource governance mechanisme

De Service Fabric-runtime biedt momenteel geen reservering voor bronnen. Wanneer een proces of een container wordt geopend, wordt de runtime de resource-limieten ingesteld op de belasting die zijn gedefinieerd tijdens het maken. Bovendien wordt de runtime geweigerd voor het openen van de nieuwe servicepakketten die beschikbaar zijn wanneer de resources zijn overschreden. Om beter te begrijpen hoe dit werkt, gaat u nu een voorbeeld van een knooppunt met twee CPU-kernen (mechanisme voor geheugen toezicht komt overeen):

1. Eerst wordt een container geplaatst op het knooppunt één CPU-kern aanvraagt. De runtime Hiermee opent u de container en de CPU-limiet ingesteld op één kern. De container niet mogelijk gebruik van meer dan één kern.

2. Vervolgens een replica van een service op het knooppunt wordt geplaatst en het bijbehorende servicepakket Hiermee geeft u een limiet van één CPU-kern. De runtime het codepakket geopend en wordt de CPU-limiet ingesteld op één kern.

Op dit moment is de som van limieten gelijk zijn aan de capaciteit van het knooppunt. Een proces en een container worden uitgevoerd met één kern en interactie met elkaar niet aangaan. Service Fabric plaatsen niet meer containers of replica's wanneer ze bij het opgeven van de CPU-limiet.

Er zijn echter twee situaties waarin andere processen voor CPU concurreren kunnen. In deze situaties mogelijk een proces en een container van ons voorbeeld het probleem ruis neighbor optreden:

* *De combinatie van services geregeld en niet-bepaald en containers*: als een gebruiker maakt een service zonder eventuele resource governance opgegeven, de runtime ziet het als geen resources verbruikt en kunt plaatsen op het knooppunt in het voorbeeld. In dit geval, verbruikt dit nieuwe proces effectief sommige CPU ten koste van de services die al wordt uitgevoerd op het knooppunt. Er zijn twee oplossing voor dit probleem. Niet door elkaar geregeld en niet-bepaald services in hetzelfde cluster of gebruik [plaatsingsbeperkingen](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) zodanig dat deze twee soorten services niet op dezelfde set knooppunten eindigen.

* *Als een ander proces wordt gestart op het knooppunt buiten de Service Fabric (bijvoorbeeld een OS-service)*: In dit geval kan het proces buiten de Service Fabric ook marketingafdeling voor CPU met bestaande services. De oplossing voor dit probleem is voor het instellen van knooppunt capaciteitswaarden correct naar de account voor OS-overhead, zoals wordt weergegeven in de volgende sectie.

## <a name="cluster-setup-for-enabling-resource-governance"></a>Configuratie voor het inschakelen van resource governance van een cluster

Wanneer een knooppunt wordt gestart en lid van het cluster, Service Fabric detecteert de beschikbare hoeveelheid geheugen en het beschikbare aantal kernen en de capaciteit van het knooppunt voor die twee resources wordt ingesteld. 

Als u wilt laten bufferruimte voor het besturingssysteem en voor andere processen kunnen worden uitgevoerd op het knooppunt, Service Fabric gebruikt alleen 80% van de beschikbare bronnen op het knooppunt. Dit percentage is configureerbaar en kan worden gewijzigd in het clustermanifest. 

Hier volgt een voorbeeld van hoe instrueren Service Fabric om 50% van de beschikbare CPU-capaciteit en 70% van het beschikbare geheugen te gebruiken: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Als u volledig handmatige installatie van knooppunt capaciteit nodig hebt, kunt u het mechanisme voor reguliere gebruiken voor de beschrijving van de knooppunten in het cluster. Hier volgt een voorbeeld van het instellen van het knooppunt met vier kernen en 2 GB geheugen: 

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Wanneer automatische detectie van de beschikbare bronnen is ingeschakeld en capaciteit knooppunt worden handmatig gedefinieerd in het clustermanifest, controleert Service Fabric of het knooppunt voldoende bronnen ter ondersteuning van de capaciteit die de gebruiker heeft gedefinieerd:
* Als knooppunt-capaciteit die zijn gedefinieerd in het manifest kleiner dan of gelijk zijn aan de beschikbare bronnen op het knooppunt, Service Fabric gebruikt vervolgens de capaciteit die zijn opgegeven in het manifest.

* Als knooppunt-capaciteit die zijn gedefinieerd in het manifest groter is dan de beschikbare bronnen, Service Fabric de beschikbare bronnen worden gebruikt als knooppunt-capaciteit.

Automatische detectie van de beschikbare bronnen kan worden uitgeschakeld als het is niet vereist. Als u wilt uitschakelen, de volgende instelling te wijzigen:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

Voor optimale prestaties, moet de volgende instelling ook worden ingeschakeld in het clustermanifest: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specify-resource-governance"></a>Resource governance opgeven 

Resource governance limieten zijn opgegeven in het toepassingsmanifest (ServiceManifestImport sectie), zoals wordt weergegeven in het volgende voorbeeld:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>

  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric sums the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```
  
In dit voorbeeld wordt het servicepakket aangeroepen **ServicePackageA** één kern opgehaald op de knooppunten waar het wordt geplaatst. Dit servicepakket bevat twee code-pakketten (**CodeA1** en **CodeA2**), en geeft u zowel de `CpuShares` parameter. De verhouding van CpuShares 512:256 verdeelt de kern tussen de twee code-pakketten. 

Dus in dit voorbeeld CodeA1 twee derde van een kern opgehaald en CodeA2 opgehaald een derde van een kern (en een reservering soft-garantie van dezelfde). Als CpuShares niet zijn opgegeven voor code-pakketten, verdeelt Service Fabric de kernen evenveel ertussen.

Geheugenlimieten zijn absolute, zodat u beide pakketten code zijn beperkt tot 1024 MB aan geheugen (en een reservering soft-garantie van dezelfde). Code-pakketten (containers of processen) kunnen niet meer geheugen dan deze limiet en probeert te doen in dat geval resulteert in een uitzondering-geheugen toewijzen. Voor een effectieve handhaving van resourcebeperkingen moeten voor alle pakketten binnen een servicepakket geheugenlimieten zijn opgegeven.

### <a name="using-application-parameters"></a>Met behulp van toepassingsparameters

Bij het opgeven van de resource governance is het mogelijk te gebruiken [toepassingsparameters](service-fabric-manage-multiple-environment-app-configuration.md) voor het beheren van configuraties met meerdere app. Het volgende voorbeeld ziet u het gebruik van parameters voor de toepassing:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>

  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="4" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="2048" />
    <Parameter Name="MemoryB" DefaultValue="2048" />
  </Parameters>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="[CpuSharesA]" MemoryInMB="[MemoryA]" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="[CpuSharesB]" MemoryInMB="[MemoryB]" />
    </Policies>
  </ServiceManifestImport>
```

In dit voorbeeld zijn standaardwaarden voor parameters ingesteld voor de productie-omgeving, waarbij elk pakket Service krijgt 4 kernen en 2 GB geheugen. Het is mogelijk standaardwaarden wijzigen met de parameter voor toepassingsbestanden. In dit voorbeeld wordt kan een parameterbestand worden gebruikt voor het testen van de toepassing lokaal door waar deze minder resources dan in de productieomgeving zou krijgen: 

```xml
<!-- ApplicationParameters\Local.xml -->

<Application Name="fabric:/TestApplication1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="1024" />
    <Parameter Name="MemoryB" DefaultValue="1024" />
  </Parameters>
</Application>
```

> [!IMPORTANT] 
> Geven resource governance met toepassingsparameters is beschikbaar vanaf met Service Fabric versie 6.1.<br> 
>
> Als parameters voor de toepassing worden gebruikt voor het opgeven van de resource governance, kan niet naar een versie voorafgaand aan versie 6.1 Service Fabric worden verlaagd. 


## <a name="other-resources-for-containers"></a>Andere bronnen voor containers
Naast de CPU en geheugen is het mogelijk andere limieten voor containers opgeven. Deze limieten zijn opgegeven op het niveau van codepakket en worden toegepast wanneer de container wordt gestart. In tegenstelling tot met CPU en geheugen, won't Cluster Resource Manager is niet op de hoogte gebracht van deze bronnen en eventuele capaciteit controles uitvoeren of taakverdeling voor deze. 

* *MemorySwapInMB*: de hoeveelheid swap-geheugen die een container kunt gebruiken.
* *MemoryReservationInMB*: de zachte limiet voor het geheugen governance die alleen wanneer er conflicten geheugen wordt gedetecteerd op het knooppunt wordt afgedwongen.
* *CpuPercent*: het percentage van CPU die de container kunt gebruiken. Als het CPU-limieten voor het servicepakket zijn opgegeven, wordt deze parameter effectief genegeerd.
* *MaximumIOps*: het maximumaantal IOPS op dat een container kunt gebruiken (lezen en schrijven).
* *MaximumIOBytesps*: de maximale i/o (in bytes per seconde) die een container kunt gebruiken (lezen en schrijven).
* *BlockIOWeight*: het IO-blok gewicht voor ten opzichte van andere containers.

Deze resources kunnen worden gecombineerd met de CPU en geheugen. Hier volgt een voorbeeld van hoe u aanvullende bronnen voor containers opgeven:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuPercent="5"
            MemorySwapInMB="4084" MemoryReservationInMB="1024" MaximumIOPS="20" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="next-steps"></a>Volgende stappen
* Lees voor meer informatie over Cluster Resource Manager [introductie van de Service Fabric-cluster resourcemanager](service-fabric-cluster-resource-manager-introduction.md).
* Meer informatie over de toepassingsmodel, servicepakketten en code-pakketten en hoe de replica's toewijzen aan deze--lezen [Model van een toepassing in Service Fabric](service-fabric-application-model.md).
