---
title: Limieten en quota-Custom Vision Service
titleSuffix: Azure Cognitive Services
description: Meer informatie over de limieten en quota voor de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: anroth
ms.openlocfilehash: 37921c655cc3c5de5c3c5079eda47fb7513fdf9f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560947"
---
# <a name="limits-and-quotas"></a>Limieten en quota

Er zijn twee lagen met sleutels voor de Custom Vision-service. U kunt zich aanmelden voor een F0 (gratis) of een S0 (standaard)-abonnement via de Azure Portal. Bekijk de pagina met overeenkomende [Cognitive Services prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) voor meer informatie over prijzen en trans acties.

Het aantal trainings afbeeldingen per project en Tags per project wordt naar verwachting in de loop van de tijd voor S0-projecten verhoogd.

||**F0**|**S0**|
|-----|-----|-----|
|Projecten|2|100|
|Trainings afbeeldingen per project |5,000|100,000|
|Voor spellingen/maand|10.000 |Onbeperkt|
|Tags/project|50|500|
|Iteraties |10|10|
|Min gelabelde afbeeldingen per tag, classificatie (50 + aanbevolen) |5|5|
|Min gelabelde afbeeldingen per tag, object detectie (50 + aanbevolen)|15|15|
|Hoe lang de Voorspellings afbeeldingen zijn opgeslagen|30 dagen|30 dagen|
|[](https://go.microsoft.com/fwlink/?linkid=865445) Voorspellings bewerkingen met opslag (trans acties per seconde)|2|10|
|[](https://go.microsoft.com/fwlink/?linkid=865445) Voorspellings bewerkingen zonder opslag (trans acties per seconde)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API-aanroepen per seconde)|2|10|
|[Andere API](https://go.microsoft.com/fwlink/?linkid=865446) -aanroepen (Trans acties per seconde)|10|10|
|Maximale afbeeldings grootte (uploaden van trainings afbeelding) |6 MB|6 MB|
|Maximale afbeeldings grootte (voor spelling)|4 MB|4 MB|
|Afbeelding van het maximum aantal regio's per object detectie|200|200|
|Afbeelding van maximum aantal labels per classificatie|30|30|
