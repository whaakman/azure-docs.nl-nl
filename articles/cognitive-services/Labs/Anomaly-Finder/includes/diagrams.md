---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: a1cda1cea2089363331ae437cb7ad802429779f4
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888693"
---
De gegevens die worden geretourneerd met de verwachte waarde en de standaard boven- en ondermarges. In de praktijk kunt u een parameter [sensitivity] definiëren en vervolgens (ExpectedValue + sensitivity * UpperMargin) gebruiken als de bovengrens en (ExpectedValue - sensitivity * LowerMargin) gebruiken als de ondergrens om het anomaliepunt zelf af te stemmen. De waarde van de [sensitivity] moet groter zijn dan 1. Hieronder vindt u enkele diagrammen voor het afstemmen.

> [!NOTE]
> De diagrammen worden niet door de voorbeeldtoepassing gegenereerd. Ze worden door een afzonderlijk hulpprogramma met de voorbeeldtoepassing gemaakt.

![Afstemmen: sensitivity = 1.0](../media/sensitivity_1.png)
![Afstemmen: sensitivity = 1.5](../media/sensitivity_1.5.png)
![Afstemmen: sensitivity = 2](../media/sensitivity_2.png)
![Afstemmen: sensitivity = 3.5](../media/sensitivity_3.5.png)