---
title: Service Fabric-Azure-bestanden Volume stuurprogramma (Preview) | Microsoft Docs
description: Service Fabric ondersteunt het gebruik van Azure Files met back-volumes in de container. Dit is momenteel in preview.
services: service-fabric
documentationcenter: other
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: other
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/10/2018
ms.author: aljo, subramar
ms.openlocfilehash: f92c8a7cca70dd9de6389c201d9589c7a31ce25f
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57726988"
---
# <a name="service-fabric-azure-files-volume-driver-preview"></a>Service Fabric-stuurprogramma voor Azure Files-Volume (Preview)
De invoegtoepassing Azure Files-volume is een [Docker volume invoegtoepassing](https://docs.docker.com/engine/extend/plugins_volume/) waarmee de [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) op basis van volumes voor Docker-containers. Deze invoegtoepassing Docker-volume wordt geleverd als een Service Fabric-toepassing die kan worden geïmplementeerd naar Service Fabric-clusters. Het doel is voor Azure Files op basis van volumes voor andere toepassingen met Service Fabric-containers die zijn geïmplementeerd in het cluster.

> [!NOTE]
> Versie 6.4.571.9590 van de invoegtoepassing Azure Files-volume is een preview-versie die beschikbaar is in dit document. Als een preview-versie, is het **niet** ondersteund voor gebruik in productieomgevingen.
>

## <a name="prerequisites"></a>Vereisten
* De Windows-versie van de invoegtoepassing Azure Files-volume werkt op [Windows Server versie 1709](https://docs.microsoft.com/windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10 versie 1709](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1709) of latere besturingssystemen. De Linux-versie van de invoegtoepassing Azure Files-volume werkt op alle versies van besturingssystemen wordt ondersteund door Service Fabric.

* De invoegtoepassing Azure Files-volume werkt alleen op Service Fabric versie 6.2 en hoger.

* Volg de instructies in de [documentatie voor Azure Files](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share) om een bestandsshare voor de Service Fabric-containertoepassing te gebruiken als een volume te maken.

* U moet [Powershell gebruiken met de module Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started) of [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) geïnstalleerd.

* Als u van Hyper-v-containers gebruikmaakt, moeten de volgende codefragmenten worden toegevoegd in het ClusterManifest (lokale cluster) of de instelling fabricSettings sectie in het ARM-sjabloon (Azure-cluster) of ClusterConfig.json (zelfstandige cluster). U moet de naam van het volume en de poort waarop het volume naar het cluster luistert. 

In het ClusterManifest, het volgende moet worden toegevoegd in de sectie Hosting. In dit voorbeeld wordt de naam van het volume is **sfazurefile** en de poort die deze naar op het cluster luistert is **19100**.  

``` xml 
<Section Name="Hosting">
  <Parameter Name="VolumePluginPorts" Value="sfazurefile:19100" />
</Section>
```

In de instelling fabricSettings-sectie van de ARM-sjabloon (voor Azure-implementaties) of ClusterConfig.json (voor zelfstandige implementaties) moet het volgende codefragment worden toegevoegd. 

```json
"fabricSettings": [
  {
    "name": "Hosting",
    "parameters": [
      {
          "name": "VolumePluginPorts",
          "value": "sfazurefile:19100"
      }
    ]
  }
]
```


## <a name="deploy-the-service-fabric-azure-files-application"></a>De bestanden van de Azure Service Fabric-toepassing implementeren

De Service Fabric-toepassing waarmee de volumes voor uw containers kan worden gedownload vanaf de volgende [koppeling](http://download.microsoft.com/download/C/0/3/C0373AA9-DEFA-48CF-9EBE-994CA2A5FA2F/AzureFilesVolumePlugin.6.4.571.9590.zip). De toepassing kan worden geïmplementeerd in het cluster via [PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications), [CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle-sfctl) of [FabricClient APIs](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications-fabricclient).

1. Via de opdrachtregel, wijzig de map naar de hoofdmap van het toepassingspakket dat is gedownload.

    ```powershell
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Kopieer het toepassingspakket naar de installatiekopieopslag de opdracht hieronder uitvoeren met de juiste waarde voor [ApplicationPackagePath] en [ImageStoreConnectionString]:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Het toepassingstype te registreren

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. De toepassing maken In de opdracht voor het maken van de onderstaande toepassing, houd er rekening mee de **ListenPort** parametr aplikace. Deze waarde opgegeven voor de parameter voor deze toepassing is de poort op waarop de invoegtoepassing Azure Files-volume voor aanvragen van de Docker-daemon luistert. Het is belangrijk om ervoor te zorgen dat de poort voor de toepassing overeenkomen met de VolumePluginPorts in het ClusterManifest opgegeven en niet in strijd met een andere poort die gebruikmaken van het cluster of van uw toepassingen.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.4.571.9590 -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.4.571.9590 --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]

> Toewijzing van SMB-koppelingen naar containers biedt geen ondersteuning voor Windows Server 2016 Datacenter ([die wordt alleen ondersteund op Windows Server versie 1709](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-storage)). Deze beperking voorkomt dat netwerktoewijzing volume en stuurprogramma's voor Azure Files-volume op versies ouder dan 1709.
>   

### <a name="deploy-the-application-on-a-local-development-cluster"></a>De toepassing op een lokaal ontwikkelcluster implementeren
Het standaardaantal-service-exemplaar voor de toepassing Azure Files-volume-invoegtoepassing wordt -1, wat inhoudt dat er een exemplaar van de service op elk knooppunt in het cluster geïmplementeerd is. Bij het implementeren van de toepassing Azure Files-volume-invoegtoepassing op een lokaal ontwikkelcluster, moet er echter het aantal service-exemplaren worden opgegeven als 1. Dit kan worden gedaan de **InstanceCount** parametr aplikace. De opdracht voor het implementeren van de toepassing Azure Files-volume-invoegtoepassing op een lokaal ontwikkelcluster is daarom:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.4.571.9590 -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.4.571.9590 --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```
## <a name="configure-your-applications-to-use-the-volume"></a>Configureren van uw toepassingen op het volume gebruiken
Het volgende fragment toont hoe een Azure-bestanden op basis van volume kan worden opgegeven in het toepassingsmanifest van uw toepassing. Het specifieke element van belang zijn, is de **Volume** tag:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
            <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
                <DriverOption Name="storageAccountFQDN" Value="" />
            </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

De naam van het stuurprogramma voor de invoegtoepassing Azure Files-volume is **sfazurefile**. Deze waarde is ingesteld voor de **stuurprogramma** kenmerk van de **Volume** element in het toepassingsmanifest.

In de **Volume** -element in het bovenstaande codefragment de invoegtoepassing Azure Files-volume vereist de volgende codes:
- **Bron** -dit is de naam van het volume. De gebruiker kan een willekeurige naam voor het volume kiezen.
- **Bestemming** -dit label is de locatie die het volume is gekoppeld aan binnen de container die wordt uitgevoerd. Dus kan niet de bestemming een locatie die al in uw container bestaat worden

Zoals wordt weergegeven in de **DriverOption** elementen in het bovenstaande codefragment de volume-invoegtoepassing voor Azure Files ondersteunt de volgende stuurprogrammaopties:
- **Sharenaam** -naam van de bestanden van de Azure-bestandsshare waarmee het volume voor de container.
- **storageAccountName** : naam van de Azure storage-account met de Azure Files-bestand delen.
- **storageAccountKey** -toegangssleutel voor de Azure storage-account met de Azure Files-share.
- **storageAccountFQDN** -domeinnaam die is gekoppeld aan de storage-account. Als storageAccountFQDN niet opgegeven is, wordt met behulp van de standaard-suffix(.file.core.windows.net) met storageAccountName domeinnaam gevormd.  

    ```xml
    - Example1: 
        <DriverOption Name="shareName" Value="myshare1" />
        <DriverOption Name="storageAccountName" Value="myaccount1" />
        <DriverOption Name="storageAccountKey" Value="mykey1" />
        <!-- storageAccountFQDN will be "myaccount1.file.core.windows.net" -->
    - Example2: 
        <DriverOption Name="shareName" Value="myshare2" />
        <DriverOption Name="storageAccountName" Value="myaccount2" />
        <DriverOption Name="storageAccountKey" Value="mykey2" />
        <DriverOption Name="storageAccountFQDN" Value="myaccount2.file.core.chinacloudapi.cn" />
    ```

## <a name="using-your-own-volume-or-logging-driver"></a>Met behulp van uw eigen volume of de stuurprogramma-logboekregistratie
Service Fabric kunt u ook het gebruik van uw eigen aangepaste [volume](https://docs.docker.com/engine/extend/plugins_volume/) of [logboekregistratie](https://docs.docker.com/engine/admin/logging/overview/) stuurprogramma's. Als het volume/logboekregistratie Docker stuurprogramma is niet geïnstalleerd op het cluster, kunt u deze handmatig installeren met behulp van de RDP/SSH-protocollen. U kunt de installatie met deze protocollen via uitvoeren een [virtuele-machineschaalset opstarten script](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) of een [SetupEntryPoint script](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model).

Een voorbeeld van het script voor het installeren van de [Docker volume stuurprogramma voor Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) is als volgt:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

In uw toepassingen, voor het gebruik van het volume of de registratie een stuurprogramma hebt geïnstalleerd, dan hebt u om op te geven van de juiste waarden in de **Volume** en **LogConfig** elementen onder  **ContainerHostPolicies** in uw manifest van de toepassing.

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

Wanneer u een invoegtoepassing volume opgeeft, maakt Service Fabric automatisch het volume met behulp van de opgegeven parameters. De **bron** taggen voor de **Volume** element is de naam van het volume en de **stuurprogramma** tag Hiermee geeft u het volume van invoegtoepassing stuurprogramma. De **bestemming** tag is de locatie die het **bron** is toegewezen aan binnen de container die wordt uitgevoerd. De bestemming kan dus een locatie die al in de container bestaat niet. Opties kunnen worden opgegeven met behulp van de **DriverOption** taggen als volgt:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Parameters voor de toepassing worden ondersteund voor volumes, zoals wordt weergegeven in het bovenstaande fragment van een clustermanifest (zoek naar `MyStorageVar` gebruiken voor een voorbeeld).

Als een Docker-log-stuurprogramma is opgegeven, hebt u voor de implementatie van agents (of containers) voor het afhandelen van de logboeken in het cluster. De **DriverOption** tag kan worden gebruikt om op te geven van de opties voor het logboek-stuurprogramma.

## <a name="next-steps"></a>Volgende stappen
* Als u wilt zien van container-voorbeelden, met inbegrip van het volume-stuurprogramma, gaat u naar de [voorbeelden van Service Fabric-container](https://github.com/Azure-Samples/service-fabric-containers)
* Raadpleeg het artikel voor het implementeren van containers in een Service Fabric-cluster, [een container in Service Fabric implementeren](service-fabric-deploy-container.md)
