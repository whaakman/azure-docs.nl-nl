---
title: Azure Service Fabric Docker Compose (Preview) | Microsoft Docs
description: Azure Service Fabric accepteert de indeling Docker Compose gemakkelijker bestaande containers indelen met behulp van Service Fabric. Ondersteuning voor Docker Compose is momenteel in preview.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 72ff8e99453d3067492d50c3e35396dba9f23c12
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="use-docker-volume-plug-ins-and-logging-drivers-in-your-container"></a>Docker volume-invoegtoepassingen en stuurprogramma's voor logboekregistratie in de container gebruiken
Azure Service Fabric ondersteunt het opgeven van [Docker volume invoegtoepassingen](https://docs.docker.com/engine/extend/plugins_volume/) en [Docker-logboekregistratie stuurprogramma's](https://docs.docker.com/engine/admin/logging/overview/) voor uw containerservice. U kunt deze persistent maken uw gegevens in [Azure Files](https://azure.microsoft.com/services/storage/files/) wanneer uw container wordt verplaatst of opnieuw gestart op een andere host.

Volume stuurprogramma's voor Linux-containers worden momenteel ondersteund. Als u Windows-containers gebruikt, kunt u een volume toewijzen aan een Azure-bestanden [SMB3 share](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) zonder een volume-stuurprogramma. Werk uw virtuele machines (VM's) in het cluster naar de nieuwste versie van Windows Server 1709 voor deze toewijzing.


## <a name="install-the-docker-volumelogging-driver"></a>Installeer het stuurprogramma voor de Docker-volume/logboekregistratie

Als het volume/logboekregistratie Docker stuurprogramma is niet geïnstalleerd op de machine, kunt u deze handmatig installeren met behulp van de RDP/SSH-protocollen. U kunt de installatie met deze protocollen via uitvoeren een [virtuele-machineschaalset opstarten script](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) of een [entrypoint script](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service).

Een voorbeeld van het script voor het installeren van de [Docker volume stuurprogramma voor Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) is als volgt:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

> [!NOTE]
> Windows Server 2016 Datacenter biedt geen ondersteuning voor toewijzing SMB koppelingen naar containers ([die wordt alleen ondersteund op Windows Server versie 1709](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-storage)). Deze beperking wordt voorkomen dat volume netwerktoewijzing en stuurprogramma's voor Azure Files volume op versies ouder dan 1709. 
>   


## <a name="specify-the-plug-in-or-driver-in-the-manifest"></a>Geef de invoegtoepassing of stuurprogramma in het manifest
De invoegtoepassingen zijn opgegeven in het toepassingsmanifest als volgt:

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
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
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

De **bron** tag voor de **Volume** element verwijst naar de bronmap. De bronmap mag een map in de virtuele machine die als host fungeert voor de containers of een permanente externe opslag. De **bestemming** label is de locatie die het **bron** is toegewezen aan in de actieve container. De bestemming kan dus een locatie die al in de container bestaat niet.

Parameters voor de toepassing worden ondersteund voor volumes, zoals wordt weergegeven in het bovenstaande codefragment manifest (zoekt `MyStoreVar` gebruiken voor een voorbeeld).

Wanneer u een invoegtoepassing volume opgeeft, maakt Service Fabric automatisch het volume met behulp van de opgegeven parameters. De **bron** label is de naam van het volume en de **stuurprogramma** tag Hiermee geeft u het volume van invoegtoepassing stuurprogramma. Opties kunnen worden opgegeven met behulp van de **DriverOption** labelen als volgt:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Als een stuurprogramma Docker-logboek is opgegeven, hebt u voor het implementeren van agents (of containers) voor het afhandelen van de logboeken in het cluster. De **DriverOption** code kan worden gebruikt om opties opgeven voor het logboek-stuurprogramma.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg het artikel voor het implementeren van containers naar een Service Fabric-cluster, [implementeren van een Service Fabric-container](service-fabric-deploy-container.md).
