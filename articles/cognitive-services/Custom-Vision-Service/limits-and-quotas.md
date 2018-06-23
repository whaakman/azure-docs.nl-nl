---
title: Limieten en quota's voor aangepaste visie Service - cognitieve Azure-Services | Microsoft Docs
description: Meer informatie over de grenzen en quota voor Azure Cognitives Services aangepaste visie Service.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: 44666d5d7f2a51e4017c704205d21b1f6d06908c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345631"
---
# <a name="limits-and-quotas"></a>Limieten en quota

Er zijn drie lagen van de sleutels voor aangepaste visie Service. F0 en S0 bronnen worden verkregen via de Azure-portal. Meer informatie over prijzen en transacties definities zijn op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  F0 projecten kunnen worden bijgewerkt naar S0 projecten.

Beperkte proefversie projectresources zijn gekoppeld aan uw aangepaste visie-aanmelding (dat wil zeggen, een AAD-account of MSA account). Ze zijn bedoeld voor korte proefversies van de service moet worden gebruikt.  Accounts die zijn gemaakt tijdens de vroege gratis preview vóór de introductie van Azure previews (1 maart 2018) behouden hun vorige quota voor beperkte proefversies. 

||**Beperkte proefversie**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Projecten|2|2|100|
|Training afbeeldingen per project|5.000|5.000|50,000|
|Voorspellingen / maand|10.000 |10.000|Onbeperkt|
|Tags / project|50|50|250|
|Herhalingen |10|10|10|
|Minimale gelabeld afbeeldingen per Tag classificatie (50 + aanbevolen) |5|5|5|
|Minimale gelabeld afbeeldingen per Tag Object detectie (50 + aanbevolen)|15|15|15|
|Hoe lang voorspelling afbeeldingen die zijn opgeslagen|30 dagen|30 dagen|30 dagen|
|[Voorspelling](https://go.microsoft.com/fwlink/?linkid=865445) bewerkingen met opslag (transacties Per seconde)|2|2|10|
|[Voorspelling](https://go.microsoft.com/fwlink/?linkid=865445) operations zonder opslag (transacties Per seconde)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API-aanroepen Per seconde)|2|2|10|
|[Andere API-aanroepen](https://go.microsoft.com/fwlink/?linkid=865446) (transacties Per seconde)|10|10|10|
|Afbeeldingsgrootte Max (training installatiekopie uploaden) |6MB|6MB|6MB|
|Afbeeldingsgrootte Max (voorspelling)|4MB|4MB|4MB|

Beperkingen met betrekking tot *# training afbeeldingen per project* en *# Tags of een ander project* worden verhoogd gedurende een bepaalde periode voor S0 projecten worden verwacht. 
