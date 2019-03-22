---
title: Azure Service Fabric-resourcebeheer voor containers en services | Microsoft Docs
description: Azure Service Fabric kunt u opgeven voor services die binnen of buiten containers worden uitgevoerd.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: aljo, subramar
ms.openlocfilehash: 985d31ea5fff7989b70443848effb616eca47ae2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57844106"
---
# <a name="resource-governance"></a>Resourcebeheer

Wanneer u meerdere services op hetzelfde knooppunt of cluster uitvoert, is het mogelijk dat één service meer resources, alle andere services in het proces bandbreedte verbruikt mogelijk gebruiken. Dit probleem wordt aangeduid als het probleem 'noisy neighbor'. Azure Service Fabric kunt de ontwikkelaar om op te geven reserveringen en limieten per service om te garanderen van resources en gebruik van resources te beperken.

> Voordat u met dit artikel verdergaat, wordt aangeraden dat u bekend bent met de [Service Fabric-toepassingsmodel](service-fabric-application-model.md) en de [Service Fabric-hostingmodel](service-fabric-hosting-model.md).
>

## <a name="resource-governance-metrics"></a>Metrische gegevens voor het beheer van resources

Resourcebeheer wordt ondersteund in Service Fabric in overeenstemming met de [servicepakket](service-fabric-application-model.md). De resources die zijn toegewezen aan het servicepakket kunnen verder worden onderverdeeld tussen pakketten. De resourcelimieten die zijn opgegeven betekent ook dat de reservering van de resources. Service Fabric ondersteunt CPU en geheugen per service-pakket op te geven met twee ingebouwde [metrische gegevens](service-fabric-cluster-resource-manager-metrics.md):

* *CPU* (naam van de meetwaarde `servicefabric:/_CpuCores`): Een logische core die beschikbaar is op de hostmachine. Alle kernen voor alle knooppunten worden gewogen hetzelfde.

* *Geheugen* (naam van de meetwaarde `servicefabric:/_MemoryInMB`): Geheugen wordt uitgedrukt in megabytes en het fysieke geheugen die beschikbaar is op de machine wordt toegewezen.

Voor deze twee metrische gegevens, [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) totale clustercapaciteit, de belasting van elk knooppunt in het cluster en de resterende resources in het cluster worden bijgehouden. Deze twee metrische gegevens zijn gelijk aan een andere gebruiker of aangepaste metrische gegevens. Alle bestaande functies kunnen worden gebruikt met deze:

* Het cluster kan worden [met gelijke taakverdeling](service-fabric-cluster-resource-manager-balancing.md) op basis van deze twee metrische gegevens (standaardinstelling).
* Het cluster kan worden [gedefragmenteerd](service-fabric-cluster-resource-manager-defragmentation-metrics.md) op basis van deze twee metrische gegevens.
* Wanneer [een cluster beschrijven](service-fabric-cluster-resource-manager-cluster-description.md), gebufferde capaciteit kan worden ingesteld voor deze twee metrische gegevens.

[Dynamische werkbelastingsrapportage](service-fabric-cluster-resource-manager-metrics.md) wordt niet ondersteund voor deze metrische gegevens en wordt geladen voor deze metrische gegevens worden gedefinieerd tijdens het maken.

## <a name="resource-governance-mechanism"></a>Resource governance mechanisme

De Service Fabric-runtime biedt op dit moment geen reservering voor de resources. Wanneer een proces of een container wordt geopend, wordt de resourcelimieten in de runtime ingesteld op de belastingen die zijn gedefinieerd tijdens het maken. Bovendien worden de runtime geweigerd voor het openen van de nieuwe service-pakketten die beschikbaar zijn wanneer de resources zijn overschreden. Voor meer informatie over de werking van het proces, we nemen een voorbeeld van een knooppunt met twee CPU-kernen (mechanisme voor het beheer van geheugen is equivalent):

1. Eerst wordt een container geplaatst op het knooppunt, aanvragen van één CPU-kern. De runtime de container wordt geopend en wordt het CPU-limiet ingesteld op één kern. De container zich niet meer dan één core gebruikt.

2. Vervolgens een replica van een service op het knooppunt wordt geplaatst en de bijbehorende servicepakket Hiermee geeft u een limiet van één CPU-kern. De runtime het codepakket wordt geopend en wordt het CPU-limiet ingesteld op één kern.

Op dit moment is de som van de limieten gelijk zijn aan de capaciteit van het knooppunt. Een proces en een container worden uitgevoerd met één kern en geen interactie met elkaar. Service Fabric wordt niet meer containers of replica's plaatsen wanneer ze bij het opgeven van de CPU-limiet.

Er zijn echter twee situaties waarin andere processen om CPU wedijveren mogelijk. In deze gevallen kunnen een proces en een container uit ons voorbeeld het probleem ruis optreden:

* *Met een combinatie van beheerde en niet-beheerde services en containers*: Als een gebruiker een service zonder een resourcebeheer die is opgegeven maken, wordt de runtime ziet als er geen bronnen verbruikt en kunt plaatsen op het knooppunt in ons voorbeeld. In dit geval, verbruikt dit nieuwe proces effectief sommige CPU ten koste van de services die al op het knooppunt worden uitgevoerd. Er zijn twee oplossing voor dit probleem. Niet combineren bestuurbaar en niet-beheerde services in hetzelfde cluster of gebruik [plaatsingsbeperkingen](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) zodat deze twee soorten services niet op dezelfde set knooppunten terechtkomen.

* *Wanneer een ander proces wordt gestart op het knooppunt buiten een Service Fabric (bijvoorbeeld een OS-service)*: In dit geval marketingafdeling het proces buiten de Service Fabric ook voor CPU met bestaande services. De oplossing voor dit probleem is het instellen van knooppuntcapaciteiten correct aan het account voor OS-overhead, zoals wordt weergegeven in de volgende sectie.

## <a name="cluster-setup-for-enabling-resource-governance"></a>Configuratie voor het inschakelen van resourcebeheer van een cluster

Wanneer een knooppunt wordt gestart en toegevoegd aan het cluster, Service Fabric detecteert de beschikbare hoeveelheid geheugen en het aantal beschikbare kernen en stelt vervolgens de knooppuntcapaciteiten voor deze twee resources.

Als u wilt laten bufferruimte voor het besturingssysteem en voor andere processen kan worden uitgevoerd op het knooppunt, Service Fabric gebruikt maximaal 80% van de beschikbare resources op het knooppunt. Dit percentage is configureerbaar en kan worden gewijzigd in het clustermanifest.

Hier volgt een voorbeeld van hoe u vertelt u Service Fabric om 50% van de beschikbare CPU-capaciteit en 70% van het beschikbare geheugen te gebruiken:

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Als u volledige handmatige installatie van knooppuntcapaciteit nodig hebt, kunt u het normale mechanisme voor het beschrijven van de knooppunten in het cluster. Hier volgt een voorbeeld van het instellen van het knooppunt met vier kernen en 2 GB geheugen:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Wanneer automatische detectie van beschikbare bronnen is ingeschakeld en knooppuntcapaciteit worden handmatig gedefinieerd in het clustermanifest, Service Fabric controleert of het knooppunt voldoende resources voor ondersteuning van de capaciteit die de gebruiker heeft gedefinieerd:

* Als knooppuntcapaciteit die zijn gedefinieerd in het manifest kleiner dan of gelijk zijn aan de beschikbare bronnen op het knooppunt, klikt u vervolgens de capaciteiten die zijn opgegeven in het manifest Service Fabric gebruikt.

* Als de knooppuntcapaciteit die zijn gedefinieerd in het manifest groter zijn dan de beschikbare resources, Service Fabric de beschikbare resources gebruikt als knooppuntcapaciteit.

Automatische detectie van beschikbare resources kan worden uitgeschakeld als het is niet vereist. Als u wilt uitschakelen, de volgende instelling te wijzigen:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

Voor optimale prestaties moet moet de volgende instelling ook worden ingeschakeld in het clustermanifest:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" />
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```

## <a name="specify-resource-governance"></a>Resourcebeheer opgeven

Resource governance limieten zijn opgegeven in het toepassingsmanifest (ServiceManifestImport sectie), zoals wordt weergegeven in het volgende voorbeeld:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>

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

In dit voorbeeld wordt de service-pakket met de naam **ServicePackageA** opgehaald van één kern op de knooppunten waar het wordt geplaatst. Deze servicepakket bevat twee codepakketten met (**CodeA1** en **CodeA2**), en beide geeft u de `CpuShares` parameter. Het deel van de CpuShares 512:256 verdeelt de core tussen de twee pakketten.

Dus in dit voorbeeld CodeA1 twee derde van een kern opgehaald en CodeA2 wordt opgehaald van een derde van een kern (en een gegarandeerde flexibele reservering hierop). Als CpuShares niet voor codepakketten opgegeven zijn, wordt de cores daartussen gelijkmatig verdeeld in Service Fabric.

Geheugenlimieten zijn absoluut, dus beide pakketten beperkt tot 1024 MB aan geheugen (en een gegarandeerde flexibele reservering hierop zijn). Codepakketten (containers of processen) kunnen niet meer geheugen dan deze limiet en probeert te doen, leidt tot een uitzondering onvoldoende geheugen toewijzen. Voor een effectieve handhaving van resourcebeperkingen moeten voor alle pakketten binnen een servicepakket geheugenlimieten zijn opgegeven.

### <a name="using-application-parameters"></a>Met behulp van parameters voor de toepassing

Bij het opgeven van resourcebeheer is het mogelijk te gebruiken [toepassingsparameters](service-fabric-manage-multiple-environment-app-configuration.md) voor het beheren van meerdere app-configuraties. Het volgende voorbeeld ziet u het gebruik van parameters voor de toepassing:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>

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

In dit voorbeeld zijn standaardwaarden voor parameters ingesteld voor de productie-omgeving, waarbij elk pakket Service krijgt 4 kernen en 2 GB geheugen. Het is mogelijk standaardwaarden wijzigen met de parameter voor toepassingsbestanden. In dit voorbeeld wordt één parameterbestand kan worden gebruikt voor het testen van de toepassing lokaal, waar deze minder resources dan in productie zou krijgen:

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
> Het opgeven van resourcebeheer met parameters voor de toepassing is vanaf met Service Fabric versie 6.1 beschikbaar.<br>
>
> Als parameters voor de toepassing worden gebruikt om op te geven resourcebeheer, Service Fabric kan niet geen downgrade worden uitgevoerd op een eerdere versie dan versie 6.1.

## <a name="other-resources-for-containers"></a>Andere bronnen voor containers

Naast de CPU en geheugen is het mogelijk om op te geven van andere resourcelimieten voor containers. Deze limieten zijn opgegeven op het niveau van de code-pakket en worden toegepast wanneer de container wordt gestart. In tegenstelling tot met CPU en geheugen, wordt niet met Cluster Resource Manager is niet op de hoogte van deze resources, en elke capaciteit controles uitvoeren of taakverdeling voor hen.

* *MemorySwapInMB*: De hoeveelheid swap-geheugen die een container kunt gebruiken.
* *MemoryReservationInMB*: De dynamische limiet voor geheugen-beheer die wordt afgedwongen alleen wanneer er conflicten geheugen wordt gedetecteerd op het knooppunt.
* *CpuPercent*: Het percentage van CPU die de container kunt gebruiken. Als het CPU-limieten voor het servicepakket zijn opgegeven, wordt deze parameter effectief genegeerd.
* *MaximumIOps*: Het maximumaantal IOPS op dat een container kunt gebruiken (lezen en schrijven).
* *MaximumIOBytesps*: De maximale i/o (bytes per seconde) die een container kunt gebruiken (lezen en schrijven).
* *BlockIOWeight*: Het blok i/o-gewicht voor ten opzichte van andere containers.

Deze resources kunnen worden gecombineerd met CPU en geheugen. Hier volgt een voorbeeld van hoe u aanvullende bronnen voor containers:

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

* Lees voor meer informatie over het Cluster Resource Manager [Maak kennis met de Service Fabric-cluster resource manager](service-fabric-cluster-resource-manager-introduction.md).
* Voor meer informatie over het toepassingsmodel en servicepakketten en codepakketten en hoe de replica's worden toegewezen aan deze--lezen [een toepassing modelleren in Service Fabric](service-fabric-application-model.md).
