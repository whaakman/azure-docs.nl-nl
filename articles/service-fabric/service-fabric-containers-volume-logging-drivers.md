---
title: Service Fabric Azure bestanden Volume stuurprogramma (Preview) | Microsoft Docs
description: Service Fabric ondersteunt het gebruik van Azure-bestanden naar back-volumes van de container. Dit is momenteel in preview.
services: service-fabric
documentationcenter: other
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: other
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 4/30/2018
ms.author: subramar
ms.openlocfilehash: 2d98cff1a5869091aa81097bbb34da6e525a2ad5
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-azure-files-volume-driver-preview"></a>Service Fabric-stuurprogramma voor Azure Files Volume (Preview)
De volume-invoegtoepassing voor Azure Files is een [Docker volume invoegtoepassing](https://docs.docker.com/engine/extend/plugins_volume/) biedt [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) op basis van volumes voor Docker-containers. Deze invoegtoepassing Docker-volume wordt geleverd als een Service Fabric-toepassing die kan worden geïmplementeerd naar Service Fabric-clusters. Het doel is te bieden dat volumes voor andere toepassingen van Service Fabric-container die in het cluster worden geïmplementeerd op basis van een Azure-bestanden.

> [!NOTE]
> Versie 6.255.389.9494 van de volume-invoegtoepassing voor Azure Files is een preview-versie die beschikbaar is in dit document. Als een preview-versie is **niet** ondersteund voor gebruik in productieomgevingen.
>

## <a name="prerequisites"></a>Vereisten
* De Windows-versie van de volume-invoegtoepassing voor Azure Files werkt op [Windows Server versie 1709](https://docs.microsoft.com/en-us/windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10 versie 1709](https://docs.microsoft.com/en-us/windows/whats-new/whats-new-windows-10-version-1709) of latere besturingssystemen. De Linux-versie van de volume-invoegtoepassing voor Azure Files werkt op alle versies van besturingssystemen wordt ondersteund door Service Fabric.

* Volg de instructies in de [documentatie voor Azure Files](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share) om een bestandsshare voor de container Service Fabric-toepassing moet worden gebruikt als een volume te maken.

* U moet [Powershell met de module Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started) of [SFCTL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cli) geïnstalleerd.

## <a name="deploy-the-service-fabric-azure-files-application"></a>De bestanden van de Azure Service Fabric-toepassing implementeren

De Service Fabric-toepassing met de volumes voor uw containers kan worden gedownload vanaf de volgende [koppeling](https://aka.ms/sfvolume). De toepassing kan worden toegepast op het cluster via [PowerShell](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications), [CLI](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-lifecycle-sfctl) of [FabricClient APIs](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications-fabricclient).

1. Via de opdrachtregel, map wijzigen naar de hoofdmap van het toepassingspakket dat is gedownload. 

    ```powershell 
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Kopieer het toepassingspakket naar het archief van de installatiekopie uitvoeren van de onderstaande opdracht met de juiste waarde voor [ApplicationPackagePath] en [ImageStoreConnectionString]:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Het toepassingstype registreren

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. De toepassing maken In de opdracht voor het maken van de onderstaande toepassing, wordt de **ListenPort** parameter van de toepassing. Deze waarde opgegeven voor de parameter van deze toepassing is de poort op waarop de volume-invoegtoepassing van Azure-bestanden voor aanvragen van de Docker-daemon luistert. Het is belangrijk om ervoor te zorgen dat de poort die is opgegeven voor de toepassing geen conflict met een andere poort die gebruikmaken van het cluster of uw toepassingen.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]

> Windows Server 2016 Datacenter biedt geen ondersteuning voor toewijzing SMB koppelingen naar containers ([die wordt alleen ondersteund op Windows Server versie 1709](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-storage)). Deze beperking wordt voorkomen dat volume netwerktoewijzing en stuurprogramma's voor Azure Files volume op versies ouder dan 1709. 
>   

### <a name="deploy-the-application-on-a-local-development-cluster"></a>De toepassing op een lokaal ontwikkelcluster implementeren
De standaard service-exemplaren voor de toepassing Azure Files volume invoegtoepassing is -1, wat betekent dat er een exemplaar van de service op elk knooppunt in het cluster is geïmplementeerd. Bij het implementeren van de toepassing Azure Files volume-invoegtoepassing op een lokaal ontwikkelcluster, moet er echter het aantal service-exemplaren worden opgegeven als 1. U kunt dit doen de **InstanceCount** parameter van de toepassing. De opdracht voor het implementeren van de toepassing Azure Files volume-invoegtoepassing op een lokaal ontwikkelcluster is daarom:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```
## <a name="configure-your-applications-to-use-the-volume"></a>Configureer uw toepassingen kunnen gebruikmaken van het volume
Het volgende fragment toont hoe een volume op basis van Azure-bestanden kan worden opgegeven in het toepassingsmanifest van uw toepassing. Het specifieke element van belang is de **Volume** tag:

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

De naam van het stuurprogramma voor de volume-invoegtoepassing voor Azure Files is **sfazurefile**. Deze waarde is ingesteld voor de **stuurprogramma** kenmerk van de **Volume** element in het toepassingsmanifest.

In de **Volume** element in het bovenstaande codefragment de volume-invoegtoepassing van Azure-bestanden vereist de volgende codes: 
- **Bron** -dit verwijst naar de bronmap die kan worden een map in de virtuele machine die als host fungeert voor de containers of een permanente externe opslag
- **Bestemming** -deze code is de locatie die het **bron** is toegewezen aan in de actieve container. Dus kan een locatie die al in de container bestaat niet in uw bestemming

Zoals wordt weergegeven in de **DriverOption** elementen in het bovenstaande codefragment de volume-invoegtoepassing van Azure-bestanden ondersteunt de volgende stuurprogrammaopties:

- **Sharenaam** -naam van de Azure-bestanden van een bestandsshare met het volume voor de container
- **storageAccountName** - naam van de Azure storage-account waarin het bestand Azure-bestanden delen
- **storageAccountKey** -toegangssleutel voor de Azure-opslagaccount waarin de bestanden van de Azure-bestandsshare

Alle bovenstaande stuurprogrammaopties zijn vereist. 

## <a name="using-your-own-volume-or-logging-driver"></a>Met behulp van uw eigen volume of de stuurprogramma-logboekregistratie
Service Fabric kunt ook het gebruik van uw eigen aangepaste volume of de stuurprogramma's voor logboekregistratie. Als het volume/logboekregistratie Docker stuurprogramma is niet geïnstalleerd op het cluster, kunt u deze handmatig installeren met behulp van de RDP/SSH-protocollen. U kunt de installatie met deze protocollen via uitvoeren een [virtuele-machineschaalset opstarten script](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) of een [entrypoint script](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service).

Een voorbeeld van het script voor het installeren van de [Docker volume stuurprogramma voor Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) is als volgt:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

In uw toepassingen voor het gebruik van het volume of de logboekregistratie van een stuurprogramma hebt geïnstalleerd, u moet opgeven van de juiste waarden in de **Volume** en **LogConfig** elementen onder  **ContainerHostPolicies** in uw toepassingsmanifest. 

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

## <a name="next-steps"></a>Volgende stappen
* Als u wilt zien van de container voorbeelden, met inbegrip van het stuurprogramma volume gaat u naar de [voorbeelden van Service Fabric-container](https://github.com/Azure-Samples/service-fabric-containers)
* Raadpleeg het artikel voor het implementeren van containers naar een Service Fabric-cluster, [een container in de infrastructuur van de Service implementeren](service-fabric-deploy-container.md)


