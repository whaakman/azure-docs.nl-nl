---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: df7326cb8e671d0f71924e813a1354dfef1e20c7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345047"
---
De gegevens geretourneerd met de verwachte waarde en het standaard hogere en lagere marges. In de praktijk, kunt u een parameter [gevoeligheid] definiëren en vervolgens met (ExpectedValue + gevoeligheid * UpperMargin) als de bovengrens en (ExpectedValue - gevoeligheid * LowerMargin) als de ondergrens voor het afstemmen van de afwijkingsdetectie wijst door uzelf. De waarde van de [gevoeligheid] moet groter zijn dan 1 zijn. Hieronder vindt u enkele diagrammen voor afstemmen.

> [!NOTE]
> De diagrammen worden gegenereerd door de voorbeeldtoepassing. Ze zijn gemaakt door een afzonderlijk hulpprogramma met de voorbeeldtoepassing.

![Verfijnen: gevoeligheid = 1.0](../media/sensitivity_1.png)
![verfijnen: gevoeligheid 1.5 =](../media/sensitivity_1.5.png)
![verfijnen: gevoeligheid = 2](../media/sensitivity_2.png)
![verfijnen: gevoeligheid 3.5 =](../media/sensitivity_3.5.png)