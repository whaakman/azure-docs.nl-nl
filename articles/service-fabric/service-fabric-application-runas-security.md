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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 62917a1d342158ec2114a9204ee1ca9e447284fa
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Een service uitvoeren als een lokale gebruikersaccount of de lokale systeemaccount
U kunt toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen met behulp van Azure Service Fabric. Service Fabric-toepassingen worden standaard uitgevoerd onder het account waaronder het Fabric.exe-proces wordt uitgevoerd onder. Service Fabric biedt tevens de mogelijkheid toepassingen uitvoeren in een lokale gebruikersaccount of het lokale systeemaccount gebruikt, doet u door een RunAs-beleid in het toepassingsmanifest. Ondersteunde lokaal systeem accounttypen zijn **LocalUser**, **NetworkService**, **LocalService**, en **LocalSystem**.

U kunt ook definiëren en gebruikersgroepen maken, zodat een of meer gebruikers kunnen worden toegevoegd aan elke groep samen worden beheerd. Dit is handig wanneer er meerdere gebruikers voor verschillende toegangspunten en deze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn.

> [!NOTE] 
> Als u een Run as-beleid op een service toepassen en de servicemanifest eindpunt resources met de HTTP-protocol declareert, moet u een **SecurityAccessPolicy**.  Zie voor meer informatie [toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="create-local-user-groups"></a>Lokale gebruikersgroepen maken
U kunt definiëren en gebruikersgroepen die toestaat dat een of meer gebruikers worden toegevoegd aan een groep maken. Dit is handig als er meerdere gebruikers voor verschillende toegangspunten en deze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn. Het volgende voorbeeld ziet u een lokale groep genaamd **LocalAdminGroup** die administrator-bevoegdheden heeft. Twee gebruikers, Customer1 en Customer2, worden leden van deze lokale groep gemaakt in het manifest voorbeeld van deze toepassing:

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
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
```

## <a name="create-local-users"></a>Lokale gebruikers maken
U kunt een lokale gebruiker die kan worden gebruikt om te helpen beveiligen van een service in de toepassing maken. Wanneer een **LocalUser** accounttype is opgegeven in de sectie principals van het toepassingsmanifest Service Fabric lokale gebruikersaccounts maakt op computers waarop de toepassing wordt geïmplementeerd. Standaard hoeft deze accounts niet dezelfde naam als die zijn opgegeven in het toepassingsmanifest (bijvoorbeeld Customer3 in het volgende application manifest voorbeeld). In plaats daarvan dynamisch worden gegenereerd en willekeurige wachtwoorden hebben.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Als een toepassing vereist dat het gebruikersaccount en wachtwoord niet hetzelfde zijn op alle machines (bijvoorbeeld naar NTLM-verificatie inschakelen), het clustermanifest moet instellen **NTLMAuthenticationEnabled** op true. Het clustermanifest moet ook opgeven voor een **NTLMAuthenticationPasswordSecret** die wordt gebruikt voor het genereren van hetzelfde wachtwoord alle machines.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

## <a name="assign-policies-to-the-service-code-packages"></a>Beleid toe te wijzen aan de service code pakketten
De **RunAsPolicy** sectie voor een **ServiceManifestImport** Hiermee geeft u het account van de sectie principals moet worden gebruikt voor het uitvoeren van een codepakket. Code-pakketten uit het servicemanifest wordt gekoppeld met een gebruikersaccount in de sectie principals. U kunt dit opgeven voor de installatie of de belangrijkste toegangspunten, of u kunt opgeven `All` toe te passen op beide. Het volgende voorbeeld ziet u verschillende soorten beleid wordt toegepast:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Als **EntryPointType** niet is opgegeven, de standaardwaarde is ingesteld op `EntryPointType=”Main”`. Geven **entrypoint** is vooral nuttig wanneer u wilt uitvoeren van bepaalde installatiebewerkingen hoge privileges met een systeemaccount. Zie voor meer informatie [een opstartscript service uitvoeren als een lokale gebruiker of systeem-account](service-fabric-run-script-at-service-startup.md). De code van de werkelijke kunt uitvoeren onder een account met kleine-bevoegdheden.

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Een standaardbeleid toepassen op alle pakketten voor service-code
U gebruikt de **DefaultRunAsPolicy** sectie om op te geven van een standaardgebruikersaccount voor alle code pakketten die beschikt niet over een specifieke **RunAsPolicy** gedefinieerd. Als de meeste van de code-pakketten die zijn opgegeven in het servicemanifest van de die wordt gebruikt door een toepassing moet worden uitgevoerd met dezelfde gebruiker, kunt de toepassing alleen een standaardbeleid RunAs met dat gebruikersaccount definiëren. Het volgende voorbeeld geeft op dat als een codepakket geen heeft een **RunAsPolicy** opgegeven, moet het codepakket uitgevoerd onder de **MyDefaultAccount** opgegeven in de sectie principals.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een toepassing implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
