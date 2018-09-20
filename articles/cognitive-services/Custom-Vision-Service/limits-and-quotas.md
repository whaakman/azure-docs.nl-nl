---
title: Limieten en quota - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Meer informatie over de limieten en quota voor de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: 53884cfc2683832d9df2b215c92b6b683ba29d2c
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363460"
---
# <a name="limits-and-quotas"></a>Limieten en quota

Er zijn drie lagen van sleutels voor Custom Vision Service. F0 en S0-resources worden verkregen via de Azure-portal. Meer informatie over prijzen en transacties definities zijn op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  F0 projecten kunnen worden bijgewerkt naar S0-projecten.

Beperkte proefversie projectresources zijn gekoppeld aan uw aangepaste Vision-aanmelding (dat wil zeggen, een AAD-account of MSA-account.) Ze zijn bedoeld voor korte proefversies van de service moet worden gebruikt.  Accounts die zijn gemaakt tijdens de vroege gratis preview vóór de introductie van Azure-previews (1 maart 2018) behouden hun vorige quota voor beperkte proefversies. 

||**Beperkte proefversie**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Projecten|2|2|100|
|Trainingsafbeeldingen per project|5.000|5.000|50,000|
|Voorspellingen / maand|10.000 |10.000|Onbeperkt|
|Tags / project|50|50|250|
|Iteraties |10|10|10|
|Minimale gelabelde afbeeldingen per Tag, classificatie (meer dan 50 aanbevolen) |5|5|5|
|Minimale gelabelde afbeeldingen per Tag, detectie van Object (meer dan 50 aanbevolen)|15|15|15|
|Hoe lang voorspelling installatiekopieën worden opgeslagen|30 dagen|30 dagen|30 dagen|
|[Voorspelling](https://go.microsoft.com/fwlink/?linkid=865445) bewerkingen met storage (transacties Per seconde)|2|2|10|
|[Voorspelling](https://go.microsoft.com/fwlink/?linkid=865445) bewerkingen zonder opslag (transacties Per seconde)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API-aanroepen Per seconde)|2|2|10|
|[Andere API-aanroepen](https://go.microsoft.com/fwlink/?linkid=865446) (transacties Per seconde)|10|10|10|
|Maximale afbeeldingsgrootte (training installatiekopie uploaden) |6MB|6MB|6MB|
|Maximale afbeeldingsgrootte (voorspelling)|4MB|4MB|4MB|

Beperkingen met betrekking tot *# trainingsafbeeldingen per project* en *# Tags/project* worden verwacht na verloop van tijd voor S0-projecten worden verhoogd. 
