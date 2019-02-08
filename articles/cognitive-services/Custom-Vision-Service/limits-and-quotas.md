---
title: Prijzen en beperkingen - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Meer informatie over de limieten en quota voor de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: anroth
ms.openlocfilehash: 8e96447f2a1b5b68ebf99ab4006fe43937f4c75f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884423"
---
# <a name="pricing-and-limits"></a>Prijzen en beperkingen

Er zijn drie lagen van sleutels voor de Custom Vision service. Beperkte proefversie projectresources zijn gekoppeld aan uw aangepaste Vision-aanmelding (dat wil zeggen, een Azure Active Directory-account of een MSA-account). Ze zijn bedoeld voor korte proefversies van de service moet worden gebruikt. U kunt zich registreren voor een F0 (gratis) of een nieuw S0 (standaard)-abonnement via de Azure-portal. Zie het bijbehorende [pagina met prijzen van Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) voor meer informatie over prijzen en transacties.

Accounts die zijn gemaakt tijdens de vroege gratis preview vóór de introductie van Azure-previews (1 maart 2018), behouden hun vorige quota voor beperkte proefversies. 

Het aantal trainingsafbeeldingen per project en tags per project worden naar verwachting verhoogd na verloop van tijd voor S0-projecten.

||**Beperkte proefversie**|**F0**|**S0**|
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
|Maximale afbeeldingsgrootte (training installatiekopie uploaden) |6 MB|6 MB|6 MB|
|Maximale afbeeldingsgrootte (voorspelling)|4 MB|4 MB|4 MB|


