---
title: Probleem oplossing-persoonlijker
titleSuffix: Azure Cognitive Services
description: Vragen over het personaliseren van problemen vindt u in dit artikel.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: article
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 7f7a6a08b86d21287c644f6a851d465d97f32e74
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663685"
---
# <a name="personalizer-troubleshooting"></a>Problemen met persoonlijker oplossen

In dit artikel vindt u antwoorden op veelgestelde vragen over het oplossen van problemen met persoonlijke instellingen.

## <a name="learning-loop"></a>Learning-lus

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>De learning-lus lijkt niet te leren. Hoe kan ik dit oplossen?

Voor de learning-lus moeten enkele duizenden belonings aanroepen worden uitgevoerd voordat rang gegeven worden. 

Als u niet zeker weet hoe uw leer proces momenteel zich gedraagt, voert u een [offline-evaluatie](concepts-offline-evaluation.md)uit en past u het gecorrigeerde leer beleid toe. 

## <a name="next-steps"></a>Volgende stappen

[De update frequentie van het model configureren](how-to-settings.md#model-update-frequency)