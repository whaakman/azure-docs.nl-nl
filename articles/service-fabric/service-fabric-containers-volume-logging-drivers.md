---
title: Azure Service Fabric Docker Compose Preview | Microsoft Docs
description: Azure Service Fabric accepteert Docker Compose indeling u indeelt exsiting containers met behulp van Service Fabric te vereenvoudigen. Deze ondersteuning is momenteel in preview.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 955f84e5656bbf568234cbaf69faa4dd0a741206
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="using-volume-plugins-and-logging-drivers-in-your-container"></a>Met behulp van volume-invoegtoepassingen en stuurprogramma's voor logboekregistratie in de container
Service Fabric ondersteunt het opgeven van [Docker volume plugins](https://docs.docker.com/engine/extend/plugins_volume/) en [Docker-logboekregistratie stuurprogramma's](https://docs.docker.com/engine/admin/logging/overview/) voor uw containerservice.  Hierdoor kunt u voor het persistent maken van uw gegevens in [Azure Files](https://azure.microsoft.com/en-us/services/storage/files/) zelfs als u container wordt verplaatst of opnieuw op een andere host gestart.

Er zijn momenteel alleen volume stuurprogramma's voor Linux-containers zoals hieronder wordt weergegeven.  Als u Windows-containers gebruikt, is het mogelijk een volume toewijzen aan een Azure-bestanden [SMB3 share](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) zonder een volume-stuurprogramma met de meest recente 1709-versie van Windows Server. Hiervoor moeten uw virtuele Machines in uw cluster bijwerken naar de versie van Windows Server 1709.


## <a name="install-volumelogging-driver"></a>Installeer het stuurprogramma voor volume/logboekregistratie

Als het volume/logboekregistratie Docker stuurprogramma is niet geïnstalleerd op de machine, handmatig wilt installeren via RDP/SSH-ing bij de computer via een [VMSS opstarten script](https://azure.microsoft.com/en-us/resources/templates/201-vmss-custom-script-windows/) of met behulp van een [entrypoint](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-model#describe-a-service) script. U kiest een van de methoden die zijn vermeld, kunt u een script voor het installeren van de [Docker Volume stuurprogramma voor Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/):


```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

## <a name="specify-the-plugin-or-driver-in-the-manifest"></a>Geef de invoegtoepassing of het stuurprogramma in het manifest
De invoegtoepassingen zijn opgegeven in het toepassingsmanifest, zoals wordt weergegeven in het manifest van de volgende:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
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
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
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

In het voorgaande voorbeeld de `Source` tag voor de `Volume` verwijst naar de bronmap. De bronmap wordt mogelijk een map in de virtuele machine die als host fungeert voor de containers of een permanente externe opslag. De `Destination` label is de locatie die het `Source` is toegewezen aan in de actieve container.  De bestemming kan dus niet zijn een al bestaande locatie in de container.

Wanneer u een invoegtoepassing volume opgeeft, maakt Service Fabric automatisch het volume met de opgegeven parameters. De `Source` label is de naam van het volume en de `Driver` tag geeft de stuurprogramma-invoegtoepassing voor volume. Opties kunnen worden opgegeven met behulp van de `DriverOption` tag zoals weergegeven in het volgende fragment:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Als een stuurprogramma Docker-logboek is opgegeven, is het voor het implementeren van agents (of containers) voor het afhandelen van de logboeken in het cluster.  De `DriverOption` label kan worden gebruikt om op te geven, evenals stuurprogramma-opties in logboek.

Raadpleeg de volgende artikelen voor het implementeren van containers naar een Service Fabric-cluster:


[Een Service Fabric-container implementeren](service-fabric-deploy-container.md)

