---
title: Een Windows-ontwikkelomgeving instellen voor het bouwen van Service Fabric Mesh-apps | Microsoft Docs
description: Stel uw Windows-ontwikkelomgeving in, zodat u een Service Fabric Mesh-toepassing kunt maken en deze implementeren voor Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 08/08/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: 0531985cbab9c10b4df8ea3f27ac6c7903790da5
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978227"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Uw Windows-ontwikkelomgeving instellen voor het bouwen van Service Fabric-apps

Als u Azure Service Fabric Mesh-toepassingen wilt bouwen en uitvoeren op een Windows-ontwikkelmachine, moet u de Service Fabric-runtime, SDK en hulpprogramma's installeren.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Ondersteunde versies van besturingssystemen

De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

* Windows 10 (Enterprise, Professional of Education)
* Windows Server 2016

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 is vereist voor het implementeren van Service Fabric Mesh-toepassingen. [Installeer versie 15.6.0][download-visual-studio] of hoger en schakel de volgende werkbelastingen in:

* ASP.NET-ontwikkeling en webontwikkeling
* Azure-ontwikkeling

## <a name="install-docker"></a>Docker installeren

#### <a name="windows-10"></a>Windows 10

Download en installeer de nieuwste versie van [Docker Community Edition voor Windows][download-docker] ter ondersteuning van de in containers verpakte Service Fabric-apps die door Service Fabric Mesh worden gebruikt.

Tijdens de installatie selecteert u **Use Windows containers instead of Linux containers** als daarom wordt gevraagd.

Als Hyper-V niet is ingeschakeld op uw computer, kunt u dit doen tijdens de installatie van Docker. Klik op **OK** om dit te doen als u hierom wordt gevraagd.

#### <a name="windows-server-2016"></a>Windows Server 2016

Als Hyper-V niet is ingeschakeld, opent u PowerShell als een beheerder en voert u de volgende opdracht uit om Hyper-V in te schakelen. Start vervolgens uw computer opnieuw op. Zie [Docker Enterprise Edition for Windows Server][download-docker-server] (Docker Enterprise Edition voor Windows Server) voor meer informatie.

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Start de computer opnieuw op.

Open PowerShell als een beheerder en voer de volgende opdracht uit om Docker te installeren:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>SDK's en hulpprogramma's

Installeer de Service Fabric Mesh-runtime, SDK en hulpprogramma's in de volgende volgorde.

1. Installeer de [Service Fabric Mesh SDK][download-sdkmesh] met behulp van Web Platform Installer. Hierdoor worden ook de Microsoft Azure Service Fabric SDK en -runtime geïnstalleerd.
2. Installeer de [extensie Visual Studio Service Fabric Mesh Tools (preview)][download-tools] vanaf Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Een cluster bouwen

Als u Visual Studio gebruikt, kunt u dit gedeelte overslaan, omdat Visual Studio een lokaal cluster voor u maakt als u er geen hebt.

Voor de beste prestaties bij het opsporen van fouten tijdens het maken en uitvoeren van Service Fabric-apps wordt u aangeraden een lokaal ontwikkelcluster met één knooppunt te maken. Dit cluster moet worden uitgevoerd wanneer u een Service Fabric-NET-project implementeert of er fouten in opspoort.

Docker **moet** worden uitgevoerd voordat u een cluster kunt bouwen. Test of Docker wordt uitgevoerd door een terminalvenster te openen en `docker ps` uit te voeren om te zien of er een fout optreedt. Als er geen fout wordt aangegeven, wordt Docker uitgevoerd en kunt u een cluster gaan bouwen.

Als u de runtime, SDK's en hulpprogramma's van Visual Studio hebt geïnstalleerd, maakt u een ontwikkelcluster.

1. Sluit het PowerShell-venster.
2. Open als beheerder een nieuw PowerShell-venster met verhoogde bevoegdheid. Deze stap is nodig om de Service Fabric-modules te laden die onlangs zijn geïnstalleerd.
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

Zoek antwoorden op [veelgestelde vragen](service-fabric-mesh-faq.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/