---
title: Een Docker-Host configureren met VirtualBox | Microsoft Docs
description: Stapsgewijze instructies voor het configureren van een standaard Docker-exemplaar met behulp van Docker-Machine en VirtualBox
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: e9465afb560a73d74f853c19094b3ee75b8c470c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Een Docker-Host met VirtualBox configureren
## <a name="overview"></a>Overzicht
In dit artikel begeleidt u bij het configureren van een standaard Docker-exemplaar met behulp van Docker-Machine en VirtualBox. Als u de [Docker voor Windows beta](http://beta.docker.com/), deze configuratie is niet nodig.

## <a name="prerequisites"></a>Vereisten
De volgende hulpprogramma's moeten worden geïnstalleerd.

* [Docker-werkset](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>De Docker-client configureren met Windows PowerShell
Voor het configureren van een client Docker gewoon opent u Windows PowerShell en voer de volgende stappen uit:

1. Maak een standaardexemplaar van de docker-host.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Controleer of dat het standaardexemplaar is geconfigureerd en actief zijn. (U ziet een exemplaar met de naam 'default' uitgevoerd.
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker-machine ls-uitvoer][0]
3. Als de huidige host instellen en configureren van de shell.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. De actieve Docker-containers worden weergegeven. De lijst mag niet leeg zijn.
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps-uitvoer][1]

> [!NOTE]
> Telkens wanneer die u opnieuw opstarten van uw ontwikkelcomputer, moet u opnieuw opstarten van uw lokale docker-host.
> Geef hiertoe de volgende opdracht achter de opdrachtprompt: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
