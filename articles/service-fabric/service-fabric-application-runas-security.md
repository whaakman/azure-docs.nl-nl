---
title: Een Azure Service Fabric-service worden uitgevoerd onder system en lokale beveiligingsaccounts | Microsoft Docs
description: Informatie over het uitvoeren van een Service Fabric-toepassing onder systeem en voor lokale beveiligingsaccounts.  Beveiligings-principals maken en toepassen van het Run As-beleid voor het veilig uitvoeren van uw services.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: mfussell
ms.openlocfilehash: 33ca23834f35e631c6943ec22a88f4fe3dc853e1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Een service uitvoeren als een lokale gebruikersaccount of de lokale systeemaccount
U kunt toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen met behulp van Azure Service Fabric. Service Fabric-toepassingen worden standaard uitgevoerd onder het account waaronder het Fabric.exe-proces wordt uitgevoerd onder. Service Fabric biedt tevens de mogelijkheid toepassingen uitvoeren in een lokale gebruiker of systeem-account. Ondersteunde lokaal systeem accounttypen zijn **LocalUser**, **NetworkService**, **LocalService**, en **LocalSystem**.  Als u Service Fabric op een zelfstandige-cluster van Windows uitvoert, kunt u een service uit onder uitvoeren [Active Directory-domeinaccounts](service-fabric-run-service-as-ad-user-or-group.md) of [groep beheerde serviceaccounts](service-fabric-run-service-as-gmsa.md).

In het toepassingsmanifest, definieert u de gebruikersaccounts die zijn vereist voor het uitvoeren van de services of beveiligde bronnen in de **Principals** sectie. U kunt ook definiëren en gebruikersgroepen maken, zodat een of meer gebruikers tegelijk kunnen worden beheerd. Dit is handig als er meerdere gebruikers voor verschillende toegangspunten zijn en moeten algemene rechten die beschikbaar op het groepeerniveau van de zijn.  De gebruikers wordt vervolgens in een RunAs-beleid wordt toegepast op een bepaalde service of de services in de toepassing verwezen. 

Standaard wordt de RunAs-beleid toegepast op de belangrijkste toegangspunt.  U kunt ook een RunAs-beleid toepassen op het toegangspunt setup als u wilt [uitvoeren van bepaalde installatiebewerkingen hoge privileges met een systeemaccount](service-fabric-run-script-at-service-startup.md), of beide main en toegangspunten setup.  

> [!NOTE] 
> Als u een Run as-beleid op een service toepassen en de servicemanifest eindpunt resources met de HTTP-protocol declareert, moet u een **SecurityAccessPolicy**.  Zie voor meer informatie [toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="run-a-service-as-a-local-user"></a>Een service uitgevoerd als een lokale gebruiker
U kunt een lokale gebruiker die kan worden gebruikt om te helpen beveiligen van een service in de toepassing maken. Wanneer een **LocalUser** accounttype is opgegeven in de sectie principals van het toepassingsmanifest Service Fabric lokale gebruikersaccounts maakt op computers waarop de toepassing wordt geïmplementeerd. Standaard deze accounts niet dezelfde naam als die zijn opgegeven in het toepassingsmanifest hebben (bijvoorbeeld *Customer3* in het volgende application manifest voorbeeld). In plaats daarvan dynamisch worden gegenereerd en willekeurige wachtwoorden hebben.

In de **RunAsPolicy** sectie voor een **ServiceManifestImport**, geef het gebruikersaccount uit de **Principals** sectie het servicepakket code uitvoeren.  Het volgende voorbeeld ziet hoe u een lokale gebruiker gemaakt en een RunAs-beleid toepassen op de Hoofdingangspunt is:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="Customer3" />
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="create-a-local-user-group"></a>Een lokale gebruikersgroep maken
U kunt gebruikersgroepen maken en een of meer gebruikers toevoegen aan de groep. Dit is handig als er meerdere gebruikers voor verschillende toegangspunten en deze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn. De volgende application manifest-voorbeeld ziet u een lokale groep met de naam *LocalAdminGroup* die administrator-bevoegdheden heeft. Twee gebruikers *Customer1* en *Customer2*, lid worden van deze lokale groep. In de **ServiceManifestImport** sectie een RunAs-beleid wordt toegepast om uit te voeren de *Stateful1Pkg* codepakket als *Customer2*.  Een ander Run as-beleid wordt toegepast om uit te voeren de *Web1Pkg* codepakket als *Customer1*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer2" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="Stateful1" ServicePackageActivationMode="ExclusiveProcess">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Groups>
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <User Name="Customer1">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <User Name="Customer2">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Een standaardbeleid toepassen op alle pakketten voor service-code
U gebruikt de **DefaultRunAsPolicy** sectie om op te geven van een standaardgebruikersaccount voor alle code pakketten die beschikt niet over een specifieke **RunAsPolicy** gedefinieerd. Als de meeste van de code-pakketten die zijn opgegeven in het servicemanifest van de die wordt gebruikt door een toepassing moet worden uitgevoerd met dezelfde gebruiker, kunt de toepassing alleen een standaardbeleid RunAs met dat gebruikersaccount definiëren. Het volgende voorbeeld geeft op dat als een codepakket geen heeft een **RunAsPolicy** opgegeven, moet het codepakket uitgevoerd onder de **MyDefaultAccount** gebruiker die is opgegeven in de sectie principals.  Ondersteunde typen zijn lokalegebruiker, NetworkService of LocalSystem of LocalService.  Als u een lokale gebruiker of service, ook de accountnaam en wachtwoord opgeven.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <Policies>
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
  </Policies>
</ApplicationManifest>
```

## <a name="debug-a-code-package-locally-using-console-redirection"></a>Fouten opsporen in een codepakket lokaal via de console-omleiding
Soms is het nuttig zijn voor de foutopsporing om te zien van de console-uitvoer van een actieve service. U kunt een console-beleid voor omleiding instellen op het toegangspunt in het servicemanifest van de, die de uitvoer naar een bestand wordt geschreven. De bestandsuitvoer is geschreven naar de map application aangeroepen **logboek** op het clusterknooppunt waar de toepassing wordt geïmplementeerd en uitgevoerd. 

> [!WARNING]
> Gebruik het beleid voor omleiding console nooit in een toepassing die wordt geïmplementeerd in productie, omdat dit de failover van de toepassing kan beïnvloeden. *Alleen* Gebruik deze optie voor lokale ontwikkeling en foutopsporing.  
> 
> 

De volgende service manifest voorbeeld ziet u console met een FileRetentionCount waarde-omleiding inschakelen:

```xml
<CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
        <ConsoleRedirection FileRetentionCount="10"/>
      </ExeHost>
    </EntryPoint>
</CodePackage>

```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
