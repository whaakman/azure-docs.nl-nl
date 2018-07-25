---
title: Een Windows-ontwikkelomgeving voor Service Fabric Mesh-apps instellen | Microsoft Docs
description: Stel uw Windows-ontwikkelomgeving in, zodat u een Service Fabric Mesh-toepassing kunt maken en deze implementeren voor Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 96549696013a2dd94741090a0a017b57a3b1e19e
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125158"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-applications"></a>Uw Windows-ontwikkelomgeving instellen om Service Fabric-toepassingen te maken

Als u Azure Service Fabric-apps wilt maken en uitvoeren op een Windows-ontwikkelmachine, moet u de Service Fabric-runtime, SDK en hulpprogramma's installeren.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Ondersteunde versies van besturingssystemen

De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

* Windows 10 (Enterprise, Professional of Education)
* Windows Server 2016

## <a name="enable-hyper-v"></a>Hyper-V inschakelen

Hyper-V moet zijn ingeschakeld als u Service Fabric-apps wilt maken. 

### <a name="windows-10"></a>Windows 10

Open PowerShell als een beheerder en voer de volgende opdracht uit:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

Start de computer opnieuw op. Zie [Install Hyper-V on Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) (Hyper-V installeren op Windows 10) voor meer informatie over het inschakelen van Hyper-V.

### <a name="windows-server-2016"></a>Windows Server 2016

Open PowerShell als een beheerder en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Start de computer opnieuw op. Zie [Install the Hyper-V role on Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server) (De Hyper-V-functie installeren op Windows Server 2016) voor meer informatie over het inschakelen van Hyper-V.

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 is vereist voor het implementeren van Service Fabric-apps. [Installeer versie 15.6.0][download-visual-studio] of hoger en schakel de volgende werkbelastingen in:

- ASP.NET-ontwikkeling en webontwikkeling
- Azure-ontwikkeling

## <a name="docker"></a>Docker

Installeer Docker ter ondersteuning van de door Service Fabric Mesh gebruikte Service Fabric-apps met containers.

### <a name="windows-10"></a>Windows 10

Download en installeer de nieuwste versie van [Docker Community Edition for Windows][download-docker]. 

Tijdens de installatie selecteert u **Use Windows containers instead of Linux containers** als daarom wordt gevraagd. Vervolgens dient u zich af te melden en u opnieuw aan te melden. Als u zich opnieuw hebt aangemeld en u Hyper-V nog niet had ingeschakeld, kunt u worden gevraagd Hyper-V in te schakelen. U dient Hyper-V in te schakelen en de computer opnieuw op te starten.

Als u de computer opnieuw hebt opgestart, wordt u gevraagd de functie **Containers** in te schakelen. Schakel deze functie in en start de computer opnieuw op.

### <a name="windows-server-2016"></a>Windows Server 2016

Gebruik de volgende PowerShell-opdrachten om Docker te installeren. Zie [Docker Enterprise Edition for Windows Server][download-docker-server] (Docker Enterprise Edition voor Windows Server) voor meer informatie.

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Start de computer opnieuw op.

## <a name="sdk-and-tools"></a>SDK's en hulpprogramma's

Installeer de Service Fabric Mesh-runtime, SDK en hulpprogramma's in een afhankelijke volgorde.

1. Installeer de [Service Fabric Mesh SDK][download-sdkmesh] met behulp van Web Platform Installer. Hierdoor worden ook de Microsoft Azure Service Fabric SDK en -runtime geïnstalleerd.
2. Installeer de [extensie Visual Studio Service Fabric Tools (preview)][download-tools] vanaf Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Een cluster bouwen

Voor de beste prestaties bij het opsporen van fouten tijdens het maken en uitvoeren van Service Fabric-apps wordt u aangeraden een lokaal ontwikkelcluster met één knooppunt te maken. Dit cluster moet worden uitgevoerd wanneer u een Service Fabric-NET-project implementeert of er fouten in opspoort.

Docker **moet** worden uitgevoerd voordat u een cluster kunt bouwen. Test of Docker wordt uitgevoerd door een terminalvenster te openen en `docker ps` uit te voeren om te zien of er een fout optreedt. Als er geen fout wordt aangegeven, wordt Docker uitgevoerd en kunt u een cluster gaan bouwen.

Als u de runtime, SDK's en hulpprogramma's van Visual Studio hebt geïnstalleerd, maakt u een ontwikkelcluster.

1. Sluit het PowerShell-venster.
2. Open als beheerder een nieuw PowerShell-venster met verhoogde bevoegdheid. Deze stap is nodig om de Service Fabric-modules te laden die u hebt geïnstalleerd.
3. Voer de volgende PowerShell-opdracht uit om een ontwikkelcluster te maken:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster -UseMachineName
    ```

4. Voer de volgende PowerShell-opdracht uit om het lokale hulpprogramma voor clusterbeheer te starten:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```

U kunt nu Service Fabric Mesh-toepassingen gaan maken.

## <a name="next-steps"></a>Volgende stappen

Lees de zelfstudie [Create an Azure Service Fabric app](service-fabric-mesh-tutorial-create-dotnetcore.md) (Een Azure Service Fabric-app maken).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/
