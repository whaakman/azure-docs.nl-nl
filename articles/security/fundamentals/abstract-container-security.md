---
title: Abstracte-container beveiliging in Microsoft Azure
description: Samen vatting van de container beveiliging in Microsoft Azure technisch document.
author: TomShinder
ms.author: TomSh
ms.date: 09/05/2018
ms.topic: article
ms.service: security
ms.subservice: security-fundamentals
ms.openlocfilehash: b70744f403c483448a844d3f3decf6ce26f48f06
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727782"
---
# <a name="container-security-in-microsoft-azure"></a>Beveiliging van de container in Microsoft Azure
## <a name="abstract"></a>Samenvatting

Container technologie veroorzaakt een structurele wijziging in de cloud computing wereld. Containers maken het mogelijk meerdere exemplaren van een toepassing uit te voeren op één exemplaar van een besturings systeem, waardoor resources efficiënter worden gebruikt. Containers bieden organisaties consistentie en flexibiliteit. Ze maken continue implementatie mogelijk, omdat de toepassing kan worden ontwikkeld op een bureau blad, getest in een virtuele machine en vervolgens wordt geïmplementeerd voor productie in de Cloud. Containers bieden flexibiliteit, gestroomlijnde bewerkingen, schaal baarheid en lagere kosten vanwege optimalisatie van resources.

Omdat de container technologie relatief nieuw is, hebben veel IT-professionals beveiligings problemen over het gebrek aan zicht baarheid en het gebruik in een productie omgeving. Ontwikkel teams zijn vaak niet op de hoogte van best practices voor beveiliging. Dit technisch document kan beveiligings-en ontwikkel aars helpen bij het selecteren van benaderingen om de ontwikkeling en implementaties van containers op het Microsoft Azure platform te beveiligen.

In dit artikel worden containers, container implementatie en-beheer en systeem eigen platform Services beschreven. Er worden ook runtime-beveiligings problemen beschreven die zich voordoen bij het gebruik van containers op het Azure-platform. In afbeeldingen en voor beelden is dit document gericht op docker als container model en Kubernetes als container-Orchestrator. De meeste beveiligings aanbevelingen zijn ook van toepassing op andere container modellen van micro soft-partners op het Azure-platform.

[Het technisch document downloaden](https://azure.microsoft.com/mediahandler/files/resourcefiles/container-security-in-microsoft-azure/Open%20Container%20Security%20in%20Microsoft%20Azure.pdf)