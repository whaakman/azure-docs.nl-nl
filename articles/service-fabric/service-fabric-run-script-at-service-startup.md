---
title: Een script uitvoeren wanneer een Azure Service Fabric-service wordt gestart | Microsoft Docs
description: Informatie over het configureren van een beleid voor een toegangspunt voor installatie van Service Fabric-service en een script uitvoeren op de service start up tijd.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: bd2bb0d05029237242b42225a2c846c78a7c6de9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
# <a name="run-a-service-startup-script-as-a-local-user-or-system-account"></a>Het opstartscript van een service uitvoeren als lokale gebruiker of systeemaccount
Voordat u een uitvoerbaar bestand Service Fabric-service wordt mogelijk werk configuratie of installatie uitvoeren.  Bijvoorbeeld, configureren van omgevingsvariabelen. U kunt opgeven dat een script uit te voeren voordat het uitvoerbare bestand van de service wordt gestart in het servicemanifest voor de service. Het uitvoerbare installatiebestand uitgevoerd door het configureren van een RunAs-beleid voor het installatieprogramma toegangspunt dat kunt u welk account onder.  Een toegangspunt voor de afzonderlijke instellingen kunt u hoge privilged configuratie voor een korte periode uitvoeren zodat de ServiceHost uitvoerbare hoeven niet te worden uitgevoerd met hoge machtigingen voor langere tijd.

Het ingangspunt setup (**entrypoint** in de [servicemanifest](service-fabric-application-and-service-manifests.md)) is een bevoorrechte toegangspunt die standaard wordt uitgevoerd met dezelfde referenties als Service Fabric (meestal de  *NetworkService* account) voordat andere toegangspunt. Het uitvoerbare bestand dat is opgegeven door **EntryPoint** is meestal de ServiceHost langlopende. De **EntryPoint** uitvoerbaar bestand wordt uitgevoerd na de **entrypoint** uitvoerbaar bestand correct wordt afgesloten. Het resulterende proces wordt bewaakt en opnieuw opgestart, en opnieuw begint met **entrypoint** ooit wordt beëindigd als of als deze is vastgelopen. 

## <a name="configure-the-service-setup-entry-point"></a>Configureer het toegangspunt voor service-instellingen
Hieronder volgt een voorbeeld van een eenvoudige service manifest voor een stateless service waarmee u een installatiescript *MySetup.bat* in de service **entrypoint**.  **Argumenten** wordt gebruikt voor het doorgeven van argumenten aan het script als deze wordt uitgevoerd.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyStatelessServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
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
Het uitvoerbare bestand van service-instelling vermelding punt wordt standaard uitgevoerd onder dezelfde referenties als Service Fabric (meestal de *NetworkService* account).  U kunt de beveiligingsmachtigingen voor het uitvoeren van het opstartscript onder het lokale systeemaccount of een administrator-account wijzigen in het toepassingsmanifest.

### <a name="configure-the-policy-by-using-a-local-system-account"></a>Het beleid configureren met behulp van een lokale systeemaccount
De volgende application manifest voorbeeld laat zien hoe het toegangspunt voor installatie van service worden uitgevoerd met administrator-account van de gebruiker (SetupAdminUser) configureren.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

Maak eerst een **Principals** sectie met een gebruikersnaam, zoals SetupAdminUser. Het gebruikersaccount SetupAdminUser is lid van de groep Administrators.

Vervolgens onder de **ServiceManifestImport** sectie, het configureren van een beleid om toe te passen deze principal die kan worden **entrypoint**. Dit beleid vertelt Service Fabric dat wanneer de **MySetup.bat** -bestand wordt worden uitgevoerd als SetupAdminUser (met beheerdersbevoegdheden) moet worden uitgevoerd. Omdat u hebt *niet* een beleid toegepast op de belangrijkste toegangspunt, de code in **MyServiceHost.exe** wordt uitgevoerd onder het systeem **NetworkService** account. Dit is het standaardaccount op dat alle toegangspunten van de service worden uitgevoerd.

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Het beleid configureren met behulp van lokaal systeemaccounts
Vaak is het raadzaam om uit te voeren van het opstartscript met een lokale systeemaccount in plaats van een administrator-account. Het RunAs-beleid wordt uitgevoerd als een lid van de groep Administrators doorgaans werkt niet goed omdat de computers hebben gebruikers toegang Gebruikersaccountbeheer (UAC) is standaard ingeschakeld. In dergelijke gevallen wordt de aanbeveling de entrypoint uitgevoerd als LocalSystem, in plaats van als een lokale gebruiker toegevoegd aan de groep Administrators. Het volgende voorbeeld ziet u de entrypoint worden uitgevoerd als LocalSystem instellen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

## <a name="run-a-script-from-the-setup-entry-point"></a>Een script uitvoeren vanaf het ingangspunt setup
Nu een begin script toevoegen aan het project worden uitgevoerd met administratorbevoegdheden. 

In Visual Studio met de rechtermuisknop op de service-project en voeg een nieuw bestand genaamd *MySetup.bat*.

Controleer vervolgens of de *MySetup.bat* bestand is opgenomen in het servicepakket. Standaard is dit niet. Selecteer het bestand, met de rechtermuisknop om het ophalen van het contextmenu en kiest u **eigenschappen**. Zorg ervoor dat in het dialoogvenster Eigenschappen **naar uitvoermap kopiëren** is ingesteld op **kopiëren indien nieuwer**. Zie de volgende schermafbeelding.

![Visual Studio CopyToOutput voor batchbestand entrypoint][image1]

Nu bewerken de *MySetup.bat* bestand en voeg de volgende opdrachten stelt u een systeemomgevingsvariabele en uitvoer van een tekstbestand:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable %*
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Vervolgens maken en implementeren van de oplossing voor een lokaal ontwikkelcluster. Nadat de service is gestart, zoals wordt weergegeven in [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), kunt u zien dat het bestand MySetup.bat geslaagd op een twee manieren. Open een PowerShell-opdrachtprompt en typ:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Noteer de naam van het knooppunt waar de service is geïmplementeerd en worden gestart in [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md). Bijvoorbeeld, knooppunt 2. Vervolgens gaat u naar de map application exemplaar werk te zoeken naar de out.txt-bestand waarin de waarde van **TestVariable**. Bijvoorbeeld, als deze service is geïmplementeerd op knooppunt 2, klikt u vervolgens gaat u naar dit pad voor de **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

## <a name="run-powershell-commands-from-a-setup-entry-point"></a>PowerShell-opdrachten uitvoeren vanaf een toegangspunt setup.
Om PowerShell uit de **entrypoint** punt, u kunt uitvoeren **PowerShell.exe** in een batchbestand dat naar een PowerShell-bestand verwijst. Eerst een PowerShell-bestand bijvoorbeeld aan de service-project toevoegen **MySetup.ps1**. Vergeet niet de *kopiëren indien nieuwer* eigenschap zodat het bestand ook in het servicepakket opgenomen is. Het volgende voorbeeld ziet u een batch voorbeeldbestand die een PowerShell-bestand aangeroepen MySetup.ps1 die Hiermee stelt u een systeemomgevingsvariabele aangeroepen begint **TestVariable**.

MySetup.bat een PowerShell-bestand te starten:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

Voeg de volgende om in te stellen van een systeemomgevingsvariabele in het bestand PowerShell:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Standaard wanneer het batchbestand wordt uitgevoerd, wordt er gezocht op de map application **werken** voor bestanden. In dit geval wanneer MySetup.bat wordt uitgevoerd, willen we het MySetup.ps1-bestand niet vinden in dezelfde map, die de toepassing met deze **codepakket** map. Om deze map wijzigen, stelt u de map:
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

## <a name="debug-a-startup-script-locally-using-console-redirection"></a>Fouten opsporen in een opstartscript lokaal via de console-omleiding
Soms is het handig voor foutopsporing om te zien van de console-uitvoer van een setup-script uitvoeren. U kunt een console-beleid voor omleiding instellen op het setup-toegangspunt in het servicemanifest van de, die de uitvoer naar een bestand wordt geschreven. De bestandsuitvoer is geschreven naar de map application aangeroepen **logboek** op het clusterknooppunt waar de toepassing wordt geïmplementeerd en uitgevoerd. 

> [!WARNING]
> Gebruik het beleid voor omleiding console nooit in een toepassing die wordt geïmplementeerd in productie, omdat dit de failover van de toepassing kan beïnvloeden. *Alleen* Gebruik deze optie voor lokale ontwikkeling en foutopsporing.  
> 
> 

De volgende service manifest voorbeeld ziet u de consoleomleiding met een waarde FileRetentionCount instellen:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Als u nu het bestand MySetup.ps1 schrijven wijzigt een **Echo** uitvoert, en dit wordt geschreven naar het uitvoerbestand voor foutopsporing:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

> [!WARNING]
> Nadat u uw script debug, onmiddellijk deze Omleidingsbeleid console te verwijderen.



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de toepassing en Servicebeveiliging](service-fabric-application-and-service-security.md)
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een toepassing implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
