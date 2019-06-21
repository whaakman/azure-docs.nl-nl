---
title: Problemen oplossen
titleSuffix: Azure Cognitive Services
description: Vragen over Personalizer probleemoplossing vindt u in dit artikel.
author: edjez
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: article
ms.date: 06/15/2019
ms.author: edjez
ms.openlocfilehash: 5136bd295c12c4439a894b4dcf0b868d32ce43ca
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313148"
---
# <a name="personalizer-troubleshooting"></a>Personalizer oplossen van problemen

In dit artikel bevat antwoorden op veelgestelde vragen over Personalizer voor probleemoplossing.

## <a name="learning-loop"></a>Learning-lus

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>De lus learning lijkt niet te leren. Hoe kan ik dit oplossen?

De lus learning moet een paar duizend beloning aanroepen voordat positie aanroepen effectief te prioriteren. 

Als u niet hoe uw lus learning momenteel gedrag weet, voert u een [offline evaluatie](concepts-offline-evaluation.md), en het beleid gecorrigeerde learning toepassen. 

## <a name="next-steps"></a>Volgende stappen

[Configureer de frequentie van de update model](how-to-settings.md#model-update-frequency)