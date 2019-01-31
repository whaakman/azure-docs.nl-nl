---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 79ae38db73d55021572d04f693e5cb809e9bd056
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228964"
---
De gegevens die worden geretourneerd met de verwachte waarde en de standaard boven- en ondermarges. In de praktijk kunt u een parameter [sensitivity] definiëren en vervolgens (ExpectedValue + sensitivity * UpperMargin) gebruiken als de bovengrens en (ExpectedValue - sensitivity * LowerMargin) gebruiken als de ondergrens om het anomaliepunt zelf af te stemmen. De waarde van de [sensitivity] moet groter zijn dan 1. Hieronder vindt u enkele diagrammen voor het afstemmen.

> [!NOTE]
> De diagrammen worden niet door de voorbeeldtoepassing gegenereerd. Ze worden gemaakt via een afzonderlijk hulpprogramma met behulp van de voorbeeldtoepassing.

![Afstemmen: sensitivity = 1.0](../media/sensitivity_1.png)
![Afstemmen: sensitivity = 1.5](../media/sensitivity_1.5.png)
![Afstemmen: sensitivity = 2](../media/sensitivity_2.png)
![Afstemmen: sensitivity = 3.5](../media/sensitivity_3.5.png)
