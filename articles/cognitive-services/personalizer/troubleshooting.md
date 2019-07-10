---
title: Probleemoplossing - Personalizer
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
ms.openlocfilehash: be6119d96b89622f45db1099a47e858a5893c2cb
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722247"
---
# <a name="personalizer-troubleshooting"></a>Personalizer oplossen van problemen

In dit artikel bevat antwoorden op veelgestelde vragen over Personalizer voor probleemoplossing.

## <a name="learning-loop"></a>Learning-lus

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>De lus learning lijkt niet te leren. Hoe kan ik dit oplossen?

De lus learning moet een paar duizend beloning aanroepen voordat positie aanroepen effectief te prioriteren. 

Als u niet hoe uw lus learning momenteel gedrag weet, voert u een [offline evaluatie](concepts-offline-evaluation.md), en het beleid gecorrigeerde learning toepassen. 

## <a name="next-steps"></a>Volgende stappen

[Configureer de frequentie van de update model](how-to-settings.md#model-update-frequency)