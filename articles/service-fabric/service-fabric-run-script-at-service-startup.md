---
title: Een script uitvoeren wanneer een Azure Service Fabric-service wordt gestart | Microsoft Docs
description: Leer hoe u een beleid voor een toegangspunt voor Service Fabric service-instellingen configureren en een script uitvoeren bij het opstarten service tijd.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: atsenthi
ms.openlocfilehash: 76be814e0dd4c054fc3a873716dbfe395eeeb2dc
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660388"
---
# <a name="run-a-service-startup-script-as-a-local-user-or-system-account"></a>Het opstartscript van een service uitvoeren als lokale gebruiker of systeemaccount
Voordat een uitvoerbaar bestand Service Fabric-service wordt gestart is het mogelijk nodig is om uit te voeren werk configuratie of configuratie nodig.  Bijvoorbeeld: configureren van omgevingsvariabelen. U kunt opgeven dat een script uit te voeren voordat de uitvoerbare-service wordt gestart in het servicemanifest voor de service. Het uitvoerbare installatieprogramma uitgevoerd door het configureren van een RunAs-beleid voor het service-instelling toegangspunt dat u kunt wijzigen welk account onder.  Het beginpunt van een afzonderlijke installatie kunt u verhoogde configuratie voor een korte periode, zodat de ServiceHost uitvoerbare niet wilt uitvoeren met hoge bevoegdheden voor langere tijd worden uitgevoerd.

Het setup-toegangspunt (**SetupEntryPoint** in de [servicemanifest](service-fabric-application-and-service-manifests.md)) is een privileged ingangspunt gebruikt dat standaard wordt uitgevoerd met dezelfde referenties als Service Fabric (meestal de  *NetworkService* account) voordat u een ander toegangspunt. Het uitvoerbare bestand dat is opgegeven door **EntryPoint** is doorgaans de ServiceHost langlopende. De **EntryPoint** uitvoerbaar bestand wordt uitgevoerd na de **SetupEntryPoint** uitvoerbaar bestand is afgesloten. Het resulterende proces wordt bewaakt en opnieuw opgestart, en wordt opnieuw gestart met **SetupEntryPoint** als dit ooit wordt beëindigd of vastloopt. 

## <a name="configure-the-service-setup-entry-point"></a>Het toegangspunt voor service-instellingen configureren
Hier volgt een eenvoudige service manifest voorbeeld voor een stateless service die Hiermee geeft u een installatiescript *MySetup.bat* in de service **SetupEntryPoint**.  **Argumenten** wordt gebruikt voor het argumenten doorgeven aan het script als deze wordt uitgevoerd.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyStatelessServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest.</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyStatelessServiceType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <Arguments>MyValue</Arguments>
        <WorkingFolder>Work</WorkingFolder>        
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyStatelessService.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```
## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Het beleid voor een toegangspunt voor service-instellingen configureren
Het uitvoerbare bestand van service-instelling vermelding punt wordt standaard uitgevoerd onder dezelfde referenties als Service Fabric (meestal de *NetworkService* account).  In het toepassingsmanifest, kunt u de beveiligingsmachtigingen voor het uitvoeren van het opstartscript onder een lokale systeemaccount of een administrator-account wijzigen.

### <a name="configure-the-policy-by-using-a-local-system-account"></a>Het beleid configureren met behulp van een lokale systeemaccount
De volgende application manifest voorbeeld ziet u hoe het configureren van het toegangspunt voor service-instelling moet worden uitgevoerd in de gebruiker administrator-account (SetupAdminUser).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

Maak eerst een **Principals** sectie met een gebruikersnaam, zoals SetupAdminUser. Het gebruikersaccount SetupAdminUser is lid van de groep Administrators-systeem.

Vervolgens onder de **ServiceManifestImport** sectie, configureer een beleid voor het toepassen van deze principal **SetupEntryPoint**. Dit beleid laat Service Fabric wordt de **MySetup.bat** -bestand wordt uitgevoerd het moet worden uitgevoerd als SetupAdminUser (met beheerdersbevoegdheden). Nadat u hebt *niet* een beleid toegepast op de belangrijkste toegangspunt, de code in **MyServiceHost.exe** wordt uitgevoerd onder het systeem **NetworkService** account. Dit is het standaardaccount dat alle service-toegangspunten worden uitgevoerd.

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Het beleid configureren met behulp van de lokale systeemaccount
Vaak is het beter om uit te voeren van het opstartscript met behulp van een lokale systeemaccount in plaats van een administrator-account. Het RunAs-beleid doorgaans uitgevoerd als een lid van de groep Administrators werkt niet goed omdat computers User Access Control (UAC) is standaard ingeschakeld. In dergelijke gevallen is de aanbeveling de SetupEntryPoint uitgevoerd als LocalSystem, in plaats van als een lokale gebruiker toegevoegd aan de groep Administrators. Het volgende voorbeeld ziet u de SetupEntryPoint uitgevoerd als LocalSystem instellen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

> [!NOTE]
> Voor Linux-clusters, een service of de installatie uit te voeren ingangspunt als **hoofdmap**, kunt u de **AccountType** als **LocalSystem**.

## <a name="run-a-script-from-the-setup-entry-point"></a>Een script uitvoeren vanaf het beginpunt van setup
Nu een begin script toevoegen aan het project moet worden uitgevoerd in de administrator-bevoegdheden. 

In Visual Studio met de rechtermuisknop op het service-project en voeg een nieuw bestand met de naam *MySetup.bat*.

Vervolgens, zorg ervoor dat de *MySetup.bat* bestand is opgenomen in het servicepakket. Standaard is deze niet. Selecteer het bestand, klik met de rechtermuisknop om het contextmenu en kies **eigenschappen**. Zorg ervoor dat in het dialoogvenster Eigenschappen **naar uitvoermap kopiëren** is ingesteld op **kopiëren indien nieuwer**. Zie de volgende schermafbeelding.

![Visual Studio CopyToOutput voor SetupEntryPoint batch-bestand][image1]

Nu bewerken de *MySetup.bat* -bestand en voeg de volgende opdrachten een systeemomgevingsvariabele instellen en een tekstbestand uitvoer:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable %*
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Vervolgens bouwen en implementeren van de oplossing op een lokaal ontwikkelcluster. Nadat de service is gestart, zoals wordt weergegeven in [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), kunt u zien dat het bestand MySetup.bat geslaagd in een op twee manieren. Open een PowerShell-opdrachtprompt en typ:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Noteer de naam van het knooppunt waar de service is geïmplementeerd en aan de slag [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md). Bijvoorbeeld, knooppunt 2. Vervolgens gaat u naar de map application exemplaar werk te zoeken naar de out.txt-bestand waarin de waarde van **TestVariable**. Bijvoorbeeld, als deze service is geïmplementeerd op knooppunt 2, klikt u vervolgens kunt gaat u naar dit pad voor de **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

## <a name="run-powershell-commands-from-a-setup-entry-point"></a>PowerShell-opdrachten uitvoeren vanaf een setup-toegangspunt
PowerShell wilt uitvoeren vanaf de **SetupEntryPoint** punt, die u kunt uitvoeren **PowerShell.exe** in een batch-bestand die naar een PowerShell-bestand verwijst. Eerst een PowerShell-bestand bijvoorbeeld aan het service-project toevoegen **MySetup.ps1**. Houd er rekening mee om in te stellen de *kopiëren indien nieuwer* eigenschap zodat het bestand ook in het servicepakket opgenomen is. Het volgende voorbeeld toont een voorbeeld van batch-bestand dat een PowerShell-bestand met de naam MySetup.ps1 die Hiermee stelt u de omgevingsvariabele van een systeem met de naam begint **TestVariable**.

MySetup.bat om te beginnen een PowerShell-bestand:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

Voeg het volgende om in te stellen van een omgevingsvariabele van het systeem in het PowerShell-bestand:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Standaard, wanneer het batchbestand wordt uitgevoerd, gekeken naar de map met de naam **werken** voor bestanden. In dit geval wanneer MySetup.bat wordt uitgevoerd, willen we het bestand MySetup.ps1 niet vinden in dezelfde map, dit de toepassing is met deze **codepakket** map. Als u wilt deze map wijzigen, stelt u de werkmap:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="debug-a-startup-script-locally-using-console-redirection"></a>Fouten opsporen in een opstartscript lokaal via console-omleiding
Soms is het handig voor foutopsporing worden gebruikt om te zien van de console-uitvoer van een setup-script is uitgevoerd. U kunt een beleid voor console-omleiding instellen op het setup-toegangspunt in het servicemanifest, die de uitvoer wordt geschreven naar een bestand. De uitvoer in een bestand wordt geschreven naar de map met de naam **log** op het clusterknooppunt waarop de toepassing wordt geïmplementeerd en uitgevoerd. 

> [!WARNING]
> Gebruik nooit het beleid voor console-omleiding in een toepassing die is geïmplementeerd in een productieomgeving omdat dit kan invloed hebben op de failover van de toepassing. *Alleen* Gebruik deze optie voor lokale ontwikkeling en foutopsporing.  
> 
> 

Het volgende voorbeeld van de service-manifest ziet u de console-omleiding met een waarde FileRetentionCount instellen:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Als u nu het bestand MySetup.ps1 schrijven wijzigt een **Echo** opdracht, dit wordt geschreven naar het uitvoerbestand voor foutopsporing:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

> [!WARNING]
> Nadat u uw script opsporen, onmiddellijk het beleid voor deze console-omleiding te verwijderen.



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de toepassing en Servicebeveiliging](service-fabric-application-and-service-security.md)
* [Inzicht krijgen in het toepassingsmodel](service-fabric-application-model.md)
* [Resources specificeren in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
