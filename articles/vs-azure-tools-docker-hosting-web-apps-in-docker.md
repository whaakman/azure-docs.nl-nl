---
title: Een ASP.NET Core Linux Docker-container implementeren op een externe Docker-host | Microsoft Docs
description: Informatie over het gebruik van Visual Studio Tools voor Docker een ASP.NET Core web-app implementeren naar een Docker-container uitgevoerd op een virtuele machine van Azure Docker Host Linux
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 60efffd9313f6972ae46fd1925d999597d3c6ba2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Een ASP.NET-container implementeren op een externe Docker-host
## <a name="overview"></a>Overzicht
Docker is een lichtgewicht container-engine, qua enkele manieren voor een virtuele machine, kunt u hosttoepassingen en services.
Deze zelfstudie leert u met behulp van de [Visual Studio Tools for Docker](https://docs.microsoft.com/dotnet/articles/core/docker/visual-studio-tools-for-docker) uitbreiding van een ASP.NET Core-app implementeren in een Docker-host op Azure met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten
Het volgende is vereist om deze zelfstudie te voltooien:

* Maak een virtuele machine in Azure Docker-Host, zoals beschreven in [docker-machine gebruiken met Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Installeer de nieuwste versie van [Visual Studio](https://www.visualstudio.com/downloads/)
* Download de [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
* Installeer [Docker voor Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Een ASP.NET Core web-app maken
De volgende stappen helpen u bij het maken van een eenvoudige ASP.NET Core-app die wordt gebruikt in deze zelfstudie.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker-ondersteuning toevoegen
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Gebruik het DockerTask.ps1 PowerShell-Script
1. Open een PowerShell-opdrachtprompt naar de hoofdmap van uw project. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Valideren van de externe host wordt uitgevoerd. U ziet de status actief = 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Maakt de app met de - Build-parameter
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. De app uitvoeren met behulp van de - parameter uitvoeren
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Wanneer de docker is voltooid, ziet u resultaten die vergelijkbaar is met het volgende:
   
   ![Uw app weergeven][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
