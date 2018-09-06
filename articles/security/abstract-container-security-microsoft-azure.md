---
title: Beveiliging van de abstracte-Container in Microsoft Azure
description: Samenvatting voor de beveiliging van de Container in technische document van Microsoft Azure.
author: TomShinder
ms.author: TomSh
ms.date: 09/05/2018
ms.topic: article
ms.service: security
ms.openlocfilehash: 2ce3aec3b7a588076584ebdf400f8cafcdeb02fe
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842929"
---
# <a name="container-security-in-microsoft-azure"></a>Beveiliging van de container in Microsoft Azure
## <a name="abstract"></a>Samenvatting

Containertechnologie wordt veroorzaakt door een structurele wijziging in de wereld cloud computing. Containers maken het mogelijk meerdere exemplaren van een toepassing uitvoeren op één exemplaar van een besturingssysteem, waardoor resources efficiënter gebruiken. Containers bieden organisaties consistentie en flexibiliteit. Deze inschakelen continue implementatie omdat de toepassing kan worden ontwikkeld op een desktop, in een virtuele machine getest en vervolgens is geïmplementeerd voor productie in de cloud. Containers bieden flexibiliteit, gestroomlijnde bewerkingen, schaalbaarheid en lagere kosten door optimalisatie van de resource.

Omdat containertechnologie relatief nieuw is, hebben veel IT-professionals beveiligingsproblemen over het ontbreken van zichtbaarheid en het gebruik in een productieomgeving. Ontwikkelteams zijn vaak geen informatie over aanbevolen procedures voor beveiliging. Dit technische document kunt u beveiliging operations-teams en ontwikkelaars bij het selecteren van methoden voor het beveiligen van de ontwikkeling van containers en implementaties op de Microsoft Azure-platform.

In dit artikel wordt beschreven, containers, containerimplementatie en beheer en systeemeigen platform-services. Ook wordt beschreven runtime beveiligingsproblemen die met het gebruik van containers op het Azure-platform optreden. In de cijfers en voorbeelden, in dit document gaat in op Docker als de container-model en Kubernetes als de containerorchestrator. De meeste van de aanbevelingen voor beveiliging ook van toepassing op andere container modellen van Microsoft-partners op de Azure-platform.

[De whitepaper downloaden](https://azure.microsoft.com/mediahandler/files/resourcefiles/container-security-in-microsoft-azure/Open%20Container%20Security%20in%20Microsoft%20Azure.pdf)