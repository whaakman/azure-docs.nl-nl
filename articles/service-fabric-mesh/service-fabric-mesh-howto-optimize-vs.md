---
title: Prestaties van Visual Studio optimaliseren voor Azure Service Fabric Mesh-projecten | Microsoft Docs
description: Prestaties van Visual Studio optimaliseren voor Azure Service Fabric Mesh-apps
services: service-fabric-mesh
keywords: prestaties bij foutopsporing optimaliseren
author: dkkapur
ms.author: dekapur
ms.date: 11/29/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: c7c9f9e72ae7ec2807c8fea08a8cc8d3e8a9e382
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804804"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Prestaties van Visual Studio optimaliseren voor Service Fabric Mesh-projecten

In dit artikel ziet u hoe u de prestaties van Visual Studio kunt optimaliseren voor Service Fabric Mesh-projecten, zodat de eerste keer dat u foutopsporing (F5) uitvoert veel sneller verloopt.  

## <a name="change-visual-studio-settings"></a>Visual Studio-instellingen wijzigen
 
In Visual Studio kunt u onder **Tools** > **Options**  > **Service Fabric Mesh Tools** > **General** de volgende instellingen aanpassen:

- Met **Pull required Docker images on project open** (Vereiste Docker-installatiekopieën ophalen bij openen van project) verloopt uw eerste foutopsporing (F5) sneller, omdat het downloaden van de installatiekopie wordt gestart tijdens het laden van het project.  
- Met **Deploy application on project open** (Toepassing implementeren bij openen van project) kan uw eerste foutopsporing (F5) sneller verlopen, omdat het implementeren wordt gestart zodra het project is geopend.  
- Met **Remove application on project close** (Toepassing verwijderen bij sluiten van project) worden resources (CPU, RAM) die zijn toegewezen aan de app, vrijgemaakt door de Mesh-app te verwijderen wanneer het project wordt gesloten.  

Als u in het uitvoervenster van Service Fabric berichten over Visual Studio ziet met daarin termen als 'pulling images', 'warming up' of 'removing application', heeft dit te maken met bovenstaande instellingen. U kunt deze instellingen uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Lees de [zelfstudie Fouten opsporen in een Mesh-app](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)