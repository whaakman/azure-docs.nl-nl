---
title: Een Windows-ontwikkelomgeving instellen voor het bouwen van Service Fabric Mesh-apps | Microsoft Docs
description: Stel uw Windows-ontwikkelomgeving in, zodat u een Service Fabric Mesh-toepassing kunt maken en deze implementeren voor Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: 70c32f5e54fa7e71c0884ceba48c84af782b3f41
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57777190"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Uw Windows-ontwikkelomgeving instellen voor het bouwen van Service Fabric-apps

Als u Azure Service Fabric Mesh-toepassingen wilt bouwen en uitvoeren op een Windows-ontwikkelmachine, hebt u het volgende nodig:

* Docker
* Visual Studio 2017
* Service Fabric Mesh runtime
* Service Fabric Mesh-SDK en -hulpprogramma's.

Plus een van de volgende versies van Windows:

* Windows 10 (Enterprise, Professional of Education) versie 1709 (Fall Creators update) of 1803 (Windows 10 April 2018 update)
* Windows Server versie 1709
* Windows Server versie 1803

Met behulp van de volgende instructies kunt u alles wat nodig is installeren op basis van uw Windows-versie.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 is vereist voor het implementeren van Service Fabric Mesh-toepassingen. [Installeer versie 15.6.0][download-visual-studio] of hoger en schakel de volgende werkbelastingen in:

* ASP.NET-ontwikkeling en webontwikkeling
* Azure-ontwikkeling

## <a name="install-docker"></a>Docker installeren

Als Docker al is geïnstalleerd, moet u nagaan of u wel de meest recente versie hebt. Als er een nieuwe versie van Docker beschikbaar is, wordt u waarschijnlijk vanzelf gevraagd of u die wilt installeren, maar u dient handmatig te controleren of u de meest recente versie hebt.

#### <a name="install-docker-on-windows-10"></a>Docker installeren in Windows 10

Download en installeer de nieuwste versie van [Docker Community Edition voor Windows][download-docker] ter ondersteuning van de in containers verpakte Service Fabric-apps die door Service Fabric Mesh worden gebruikt.

Tijdens de installatie selecteert u **Use Windows containers instead of Linux containers** als daarom wordt gevraagd.

Als Hyper-V niet is ingeschakeld op uw computer, kunt u dit doen tijdens de installatie van Docker. Klik op **OK** om dit te doen als u hierom wordt gevraagd.

#### <a name="install-docker-on-windows-server-2016"></a>Docker installeren in Windows Server 2016

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

> [!IMPORTANT]
> Docker **moet** worden uitgevoerd voordat u een cluster kunt bouwen.
> Test of Docker wordt uitgevoerd door een terminalvenster te openen en `docker ps` uit te voeren om te zien of er een fout optreedt. Als er geen fout wordt aangegeven, wordt Docker uitgevoerd en kunt u een cluster gaan bouwen.

> [!Note]
> Als u een ontwikkelcomputer met Windows Fall Creators update (versie 1709) gebruikt, kunt u alleen Docker-installatiekopieën voor Windows-versie 1709 gebruiken.
> Als u een ontwikkelcomputer met Windows 10 april 2018 update (versie 1803) gebruikt, kunt u Docker-installatiekopieën voor Windows-versie 1709 en 1803 gebruiken.

Als u Visual Studio gebruikt, kunt u dit gedeelte overslaan, omdat Visual Studio een lokaal cluster voor u maakt als u er geen hebt.

Voor de beste prestaties bij het opsporen van fouten tijdens het maken en uitvoeren van één Service Fabric-app tegelijk, maakt u een lokaal ontwikkelcluster met één knooppunt. Als u meerdere toepassingen tegelijk uitvoert, maakt u een lokaal ontwikkelcluster met vijf knooppunten. Het cluster moet worden uitgevoerd wanneer u een Service Fabric Mesh-project implementeert of er fouten in opspoort.

Als u de runtime, SDK's en hulpprogramma's van Visual Studio Docker hebt geïnstalleerd en Docker wordt uitgevoerd, maakt u een ontwikkelcluster.

1. Sluit het PowerShell-venster.
2. Open als beheerder een nieuw PowerShell-venster met verhoogde bevoegdheid. Deze stap is nodig om de Service Fabric-modules te laden die onlangs zijn geïnstalleerd.
3. Voer de volgende PowerShell-opdracht uit om een ontwikkelcluster te maken:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateMeshCluster -CreateOneNodeCluster
    ```
4. Voer de volgende PowerShell-opdracht uit om het lokale hulpprogramma voor clusterbeheer te starten:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```
5. Zodra het hulpprogramma voor het beheer van het Service-cluster wordt uitgevoerd (dit wordt weergegeven in het systeemvak), klikt u er met de rechtermuisknop op en klikt u op **Lokaal cluster starten**.

![Afbeelding 1: Het lokale cluster starten](./media/service-fabric-mesh-howto-setup-developer-environment-sdk/start-local-cluster.png)

U kunt nu Service Fabric Mesh-toepassingen gaan maken.

## <a name="next-steps"></a>Volgende stappen

Lees de zelfstudie [Create an Azure Service Fabric app](service-fabric-mesh-tutorial-create-dotnetcore.md) (Een Azure Service Fabric-app maken).

Zoek antwoorden op [veelgestelde vragen en bekende problemen](service-fabric-mesh-faq.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://aka.ms/sfmesh_vs2017tools
[download-visual-studio]: https://www.visualstudio.com/downloads/