---
title: Beheren van meerdere omgevingen in Service Fabric | Microsoft Docs
description: "Service Fabric-toepassingen kunnen worden uitgevoerd op clusters die in grootte van de ene machine naar duizenden computers variëren. In sommige gevallen wordt u uw toepassing anders voor deze uiteenlopende omgevingen configureren. In dit artikel wordt beschreven hoe u verschillende groepen van toepassingsparameters per omgeving definiëren."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 671cc9b0f7b7b37fcf5b052f7e34bc98e66b2838
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a>Parameters voor de toepassing voor omgevingen met meerdere beheren
U kunt Azure Service Fabric-clusters maken met behulp van een willekeurige plaats van een tot vele duizenden computers. Hoewel binaire bestanden van toepassingen zonder dat aanpassingen nodig over deze breed scala aan omgevingen uitvoeren kunnen, u vaak de toepassing verschillend, afhankelijk van het aantal machines die u om te implementeert configureren.

Een eenvoudig voorbeeld kunt u beter `InstanceCount` voor een stateless service. Wanneer u toepassingen in Azure uitvoert, wilt u in het algemeen Stel deze parameter in op de speciale waarde '1'. Deze configuratie zorgt ervoor dat uw service wordt uitgevoerd op elk knooppunt in het cluster (of elk knooppunt in het knooppunttype als u een beperking voor de plaatsing hebt ingesteld). Deze configuratie is echter niet geschikt voor een cluster met één computer omdat er geen meerdere processen luistert op hetzelfde eindpunt op een enkele computer. In plaats daarvan doorgaans ingesteld `InstanceCount` aan '1'.

## <a name="specifying-environment-specific-parameters"></a>Omgeving-specifieke parameters opgeven
De oplossing voor dit configuratieprobleem is een set met parameters standaardservices en toepassingsbestanden parameter die de parameterwaarden van deze voor een bepaalde omgeving invullen. Standaard-services en parameters voor de toepassing worden geconfigureerd in de toepassing en service manifesten. De schemadefinitie voor de bestanden ServiceManifest.xml en ApplicationManifest.xml is geïnstalleerd met de Service Fabric SDK en hulpprogramma's voor *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Standaard-services
Service Fabric-toepassingen bestaan uit een verzameling van service-exemplaren. Het is mogelijk dat u een lege toepassing maken en vervolgens alle service-exemplaren dynamisch maken, hebben de meeste toepassingen een set van core services die altijd moet worden gemaakt wanneer de toepassing wordt gestart. Deze worden aangeduid als 'standaardservices '. Ze worden opgegeven in het toepassingsmanifest met tijdelijke aanduidingen voor de per-omgeving configuratie opgenomen tussen vierkante haken:

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

Elk van de benoemde parameters moet zijn gedefinieerd binnen het element Parameters van het toepassingsmanifest:

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

Het kenmerk DefaultValue geeft de waarde moet worden gebruikt in het ontbreken van een meer specifieke parameter voor een bepaalde omgeving.

> [!NOTE]
> Niet alle parameters van de service-exemplaar zijn geschikt voor configuratie van per-omgeving. In het bovenstaande voorbeeld de LowKey en HighKey waarden voor de service partitieschema zijn expliciet is gedefinieerd voor alle exemplaren van de service omdat het partitiebereik een functie van het domein van de gegevens, niet de omgeving is.
> 
> 

### <a name="per-environment-service-configuration-settings"></a>Configuratie-instellingen voor de per-omgeving
De [Service Fabric-toepassingsmodel](service-fabric-application-model.md) , kunnen services omvatten configuratiepakketten met aangepaste sleutel-waardeparen die kunnen gelezen tijdens runtime worden. De waarden van deze instellingen kunnen ook worden onderscheiden door omgeving door te geven een `ConfigOverride` in het toepassingsmanifest.

Stel dat u hebt de volgende instelling in het bestand Config\Settings.xml voor de `Stateful1` service:

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
U kunt deze waarde voor een specifieke toepassingsomgeving paar overschrijven, maakt een `ConfigOverride` wanneer het importeren van het servicemanifest in het toepassingsmanifest.

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
Deze parameter kan vervolgens worden geconfigureerd door omgeving zoals hierboven. U kunt dit doen door deze in de sectie parameters van het toepassingsmanifest te declareren en omgeving-specifieke waarden opgeven in de parameter voor toepassingsbestanden.

> [!NOTE]
> In het geval van een service van configuratie-instellingen, er zijn drie locaties waar de waarde van een sleutel kan worden ingesteld: het configuratiepakket service manifest voor de toepassing en de parameter-bestand van de toepassing. Service Fabric kiest altijd via de parameter toepassingsbestand eerste (indien opgegeven), klikt u vervolgens het toepassingsmanifest en ten slotte het configuratiepakket.
> 
> 

### <a name="setting-and-using-environment-variables"></a>Instellen en het gebruik van omgevingsvariabelen 
U kunt opgeven en deze omgevingsvariabelen worden ingesteld in het bestand ServiceManifest.xml en vervolgens uitschakelen in het bestand ApplicationManifest.xml per exemplaar op basis van een.
Het volgende voorbeeld ziet u twee omgevingsvariabelen, één met een waarde die is ingesteld en de andere wordt overschreven. Parameters voor de toepassing kunt u waarden voor omgevingsvariabelen variabelen ingesteld op dezelfde manier als deze zijn gebruikt voor onderdrukkingen in configuratie.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```
U kunt de omgevingsvariabelen in de ApplicationManifest.xml overschrijven, verwijst naar het codepakket in de ServiceManifest met de `EnvironmentOverrides` element.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 Zodra het benoemde service-exemplaar is gemaakt, kunt u de omgevingsvariabelen openen vanuit code. bijvoorbeeld In C# kunt u het volgende doen

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a>Service Fabric-omgevingsvariabelen
Service Fabric is ingebouwd in de omgevingsvariabelen instellen voor elke service-exemplaar. De volledige lijst met omgevingsvariabelen lager is dan, waarbij de waarden in vet zijn degene die u wilt gebruiken in uw service, de andere wordt gebruikt door de Service Fabric-runtime. 

* Fabric_ApplicationHostId
* Fabric_ApplicationHostType
* Fabric_ApplicationId
* **Fabric_ApplicationName**
* Fabric_CodePackageInstanceId
* **Fabric_CodePackageName**
* **TypeEndpoint Fabric_Endpoint_ [YourServiceName]**
* **Fabric_Folder_App_Log**
* **Fabric_Folder_App_Temp**
* **Fabric_Folder_App_Work**
* **Fabric_Folder_Application**
* Fabric_NodeId
* **Fabric_NodeIPOrFQDN**
* **Fabric_NodeName**
* Fabric_RuntimeConnectionAddress
* Fabric_ServicePackageInstanceId
* Fabric_ServicePackageName
* Fabric_ServicePackageVersionInstance
* FabricPackageFileName

De belows code ziet u hoe u de Service Fabric-omgevingsvariabelen
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
Hieronder vindt u voorbeelden van omgevingsvariabelen voor een toepassingstype aangeroepen `GuestExe.Application` aangeroepen met een servicetype `FrontEndService` wanneer uitvoeren op uw lokale dev-computer.

* **Fabric_ApplicationName fabric:/GuestExe.Application =**
* **Fabric_CodePackageName = Code**
* **Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**
* **Fabric_NodeIPOrFQDN = localhost**
* **Fabric_NodeName _Node_2 =**

### <a name="application-parameter-files"></a>Parameter voor toepassingsbestanden
Het Service Fabric-toepassingsproject kan een of meer parameter voor toepassingsbestanden bevatten. Elk van definieert de specifieke waarden voor de parameters die zijn gedefinieerd in het toepassingsmanifest:

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
Een nieuwe toepassing bevat drie parameter voor toepassingsbestanden, met de naam Local.1Node.xml Local.5Node.xml en Cloud.xml:

![Parameter voor toepassingsbestanden in Solution Explorer][app-parameters-solution-explorer]

Voor het maken van een parameterbestand gewoon kopiëren en plakken van een bestaande en een nieuwe naam geven.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Omgeving-specifieke parameters identificeren tijdens de implementatie
Tijdens de implementatie moet u de juiste parameter-bestand toe te passen aan uw toepassing kiezen. U kunt dit doen via het dialoogvenster Publish in Visual Studio of PowerShell.

### <a name="deploy-from-visual-studio"></a>Implementeren vanuit Visual Studio
U kunt kiezen uit de lijst met beschikbare parameterbestanden wanneer u uw toepassing in Visual Studio publiceert.

![Een parameterbestand kiezen in het dialoogvenster Publish][publishdialog]

### <a name="deploy-from-powershell"></a>Implementeren vanaf PowerShell
De `Deploy-FabricApplication.ps1` PowerShell-script is opgenomen in de projectsjabloon toepassing accepteert een publicatieprofiel als een parameter en de PublishProfile bevat een verwijzing naar de parameters-bestand van de toepassing.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over een aantal van de belangrijkste concepten die worden beschreven in dit onderwerp, de [technisch overzicht van Service Fabric](service-fabric-technical-overview.md). Zie voor meer informatie over de mogelijkheden van andere app-beheer die beschikbaar in Visual Studio zijn [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
