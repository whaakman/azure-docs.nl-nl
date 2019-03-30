---
title: Een Azure Service Fabric-service uitgevoerd onder systeem- en lokale beveiligingsaccounts | Microsoft Docs
description: Leer hoe u een Service Fabric-toepassing uitgevoerd onder systeem- en lokale accounts.  Beveiligings-principals maken en toepassen van het Run As-beleid voor het veilig uitvoeren van uw services.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: aljo
ms.openlocfilehash: 28cd1162d7cae2b3a16062bdf18a2971e1f05aad
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664434"
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Een service uitvoeren als een lokale gebruikersaccount of local system-account
Met behulp van Azure Service Fabric, kunt u toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen. Standaard wordt Service Fabric-toepassingen uitvoeren onder het account dat de Fabric.exe-proces wordt uitgevoerd onder. Service Fabric biedt ook de mogelijkheid voor het uitvoeren van toepassingen met een lokale gebruiker of systeem-account. Ondersteunde lokaal systeem accounttypen zijn **LocalUser**, **NetworkService**, **LocalService**, en **LocalSystem**.  Als u Service Fabric op een zelfstandige cluster van Windows uitvoert, kunt u een service onder uitvoeren [Active Directory-domeinaccounts](service-fabric-run-service-as-ad-user-or-group.md) of [groep beheerde serviceaccounts](service-fabric-run-service-as-gmsa.md).

In het toepassingsmanifest, definieert u de gebruikersaccounts die zijn vereist voor het uitvoeren van services of beveiligde resources in de **Principals** sectie. U kunt ook definiëren en gebruikersgroepen maken, zodat een of meer gebruikers samen kunnen worden beheerd. Dit is handig als er meerdere gebruikers voor verschillende toegangspunten toegepast en ze nodig hebben algemene machtigingen die beschikbaar op het groepeerniveau van de zijn.  De gebruikers worden vervolgens waarnaar wordt verwezen in een Run as-beleid wordt toegepast op een bepaalde service of alle services in de toepassing. 

Standaard wordt het RunAs-beleid toegepast op de belangrijkste toegangspunt.  U kunt ook een RunAs-beleid toepassen op het toegangspunt instellen als u wilt [bepaalde instellingsbewerkingen hoge bevoegdheden met een systeemaccount worden uitgevoerd](service-fabric-run-script-at-service-startup.md), of beide main en toegangspunten instellen.  

> [!NOTE] 
> Als u een Run as-beleid op een service toepassen en het servicemanifest eindpunt resources met de HTTP-protocol verklaart, moet u een **SecurityAccessPolicy**.  Zie voor meer informatie, [toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="run-a-service-as-a-local-user"></a>Een service uitvoeren als een lokale gebruiker
U kunt een lokale gebruiker die kan worden gebruikt om te helpen beveiligen van een service in de toepassing kunt maken. Wanneer een **lokalegebruiker** accounttype is opgegeven in de sectie principals van het manifest van de toepassing, Service Fabric lokale gebruikersaccounts maakt op computers waarop de toepassing wordt geïmplementeerd. Standaard deze accounts niet dezelfde naam als die zijn opgegeven in het toepassingsmanifest hebben (bijvoorbeeld *Customer3* in het volgende voorbeeld van de toepassing-manifest). In plaats daarvan worden ze dynamisch worden gegenereerd en willekeurige wachtwoorden hebben.

In de **RunAsPolicy** sectie voor een **ServiceManifestImport**, geeft u het gebruikersaccount van de **Principals** sectie om uit te voeren met het servicepakket code.  Het volgende voorbeeld ziet u hoe u een lokale gebruiker maken en een RunAs-beleid toepassen op de belangrijkste toegangspunt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

## <a name="create-a-local-user-group"></a>Maak een lokale gebruikersgroep
U kunt groepen maken en een of meer gebruikers toevoegen aan de groep. Dit is handig als er meerdere gebruikers voor verschillende toegangspunten toegepast en ze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn hebt. De volgende application manifest voorbeeld ziet u een lokale groep met de naam *LocalAdminGroup* die beheerdersrechten heeft. Twee gebruikers *Customer1* en *Customer2*, leden van de lokale groep zijn aangebracht. In de **ServiceManifestImport** sectie, een uitvoeren als beleid wordt toegepast om uit te voeren de *Stateful1Pkg* codepakket als *Customer2*.  Een ander Run as-beleid wordt toegepast om uit te voeren de *Web1Pkg* codepakket als *Customer1*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Een standaardbeleid toepassen op alle service codepakketten
U gebruikt de **DefaultRunAsPolicy** sectie om op te geven van een standaard-gebruikersaccount voor de code van alle pakketten die niet zijn een specifieke **RunAsPolicy** gedefinieerd. Als de meeste van de codepakketten die zijn opgegeven in het servicemanifest gebruikt door een toepassing worden uitgevoerd met dezelfde gebruiker moet, kunt de toepassing alleen een standaardbeleid voor uitvoeren als met dat gebruikersaccount definiëren. Het volgende voorbeeld geeft aan dat als een codepakket heeft geen een **RunAsPolicy** opgegeven, het codepakket moet worden uitgevoerd onder de **MyDefaultAccount** gebruiker die is opgegeven in de sectie beveiligings-principals.  Ondersteunde typen zijn lokalegebruiker, Netwerkservice, lokaal systeem en LocalService.  Als u een lokale gebruiker of service, ook de accountnaam en wachtwoord opgeven.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

## <a name="debug-a-code-package-locally-using-console-redirection"></a>Fouten opsporen in een codepakket lokaal via console-omleiding
Soms is het handig is voor de foutopsporing om te zien van de console-uitvoer van een actieve service. U kunt een beleid voor console-omleiding instellen op het toegangspunt dat in het servicemanifest, die de uitvoer wordt geschreven naar een bestand. De uitvoer in een bestand wordt geschreven naar de map met de naam **log** op het clusterknooppunt waarop de toepassing wordt geïmplementeerd en uitgevoerd. 

> [!WARNING]
> Gebruik nooit het beleid voor console-omleiding in een toepassing die is geïmplementeerd in een productieomgeving omdat dit kan invloed hebben op de failover van de toepassing. *Alleen* Gebruik deze optie voor lokale ontwikkeling en foutopsporing.  
> 
> 

De volgende service manifest voorbeeld wordt met een waarde FileRetentionCount console-omleiding inschakelen:

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
* [Inzicht krijgen in het toepassingsmodel](service-fabric-application-model.md)
* [Resources specificeren in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
