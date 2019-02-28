---
title: Azure Service Fabric Mesh-voorbeelden zoeken | Microsoft Docs
description: Meer informatie over hoe u verschillende Service Fabric Mesh-voorbeeldtoepassingen kunt vinden.
services: service-fabric-mesh
keywords: ''
author: v-vasuke
ms.author: v-vasuke
ms.date: 12/03/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: 12fbc0df8df56426679faef02b2b3fd285d391dc
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805185"
---
# <a name="find-service-fabric-mesh-samples"></a>Service Fabric Mesh-voorbeelden zoeken

Deze tabel biedt een overzicht van de beschikbare Service Fabric Mesh-voorbeeldtoepassingen. De broncode in die voorbeelden laten zien hoe u een bepaald scenario kunt uitvoeren met behulp van het Service Fabric-resourcemodel.

Zie de [GitHub-pagina met voorbeeldsjablonen](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md) voor meer informatie over het direct implementeren van sjablonen naar Azure.


|Voorbeeldsjabloon|Scenariobeschrijving|Broncode|Ontwikkelhulpprogramma's|
|------------|--------------------|----------|----------------------|
| [Hallo wereld-app](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Statische webpagina die wordt gehost in een container. Voor Linux wordt nginx gebruikt, voor Windows wordt IIS gebruikt. | [Broncode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Geen vereisten |
| [Teller-app voor Azure Files-volumes](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter) | Sla de status op door een koppeling aan te brengen met op Azure Files gebaseerde volumes in de container. <br><br> **Opmerking:** voor deze sjabloon is vereist dat er al een Azure Files-bestandsshare is ingericht. Zie hiervoor de [instructies](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share) | [Broncode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio Mesh Tooling |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolist) | Maak een toepassing met een front-end- en back-endservice die gebruikmaken van DNS-omzetting. Als zelfstudie [hier](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-tutorial-create-dotnetcore) te vinden. | [Broncode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Visual Studio Mesh Tooling |
| [Visuele objecten-app](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Microservices upgraden en de schaal ervan in een toepassing aanpassen. | [Broncode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Visual Studio Mesh Tooling |
| [Stem-app](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/voting) | Maak een toepassing met een front-end- en back-endservice die gebruikmaken van DNS-omzetting. | [Broncode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Visual Studio Mesh Tooling voor de Windows-versie, VS Code/dotnet-cli kan worden gebruikt voor de Linux-versie |
| [Teller-app voor betrouwbare Service Fabric-volumes](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter)| Sla de status op door een koppeling aan te brengen met op Service Fabric Reliable Disk gebaseerde volumes in de container.| [Broncode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio Mesh Tooling |
