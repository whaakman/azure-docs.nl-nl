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
ms.openlocfilehash: ce06effbce12abb6271e050829d3218f4fbbfbf4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902855"
---
# <a name="limits-and-quotas"></a>Limieten en quota

Er zijn drie lagen van sleutels voor Custom Vision Service. F0 en S0-resources worden verkregen via de Azure-portal. Meer informatie over prijzen en transacties definities zijn op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  F0 projecten kunnen worden bijgewerkt naar S0-projecten.

Beperkte proefversie projectresources zijn gekoppeld aan uw aangepaste Vision-aanmelding (dat wil zeggen, een AAD-account of MSA-account.) Ze zijn bedoeld voor korte proefversies van de service moet worden gebruikt.  Accounts die zijn gemaakt tijdens de vroege gratis preview vóór de introductie van Azure-previews (1 maart 2018) behouden hun vorige quota voor beperkte proefversies. 

||**Beperkte proefversie**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Projecten|2|2|100|
|Trainingsafbeeldingen per project, classificatie|5.000|5.000|50,000|
|Trainingsafbeeldingen per project, detectie van Object|5.000|5.000|10.000|
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
