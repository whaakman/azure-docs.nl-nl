---
title: Probleem oplossing-persoonlijker
titleSuffix: Azure Cognitive Services
description: Vragen over het personaliseren van problemen vindt u in dit artikel.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 9f4c4129217923f7fb32996f7447ed09a034f888
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955215"
---
# <a name="personalizer-troubleshooting"></a>Problemen met persoonlijker oplossen

In dit artikel vindt u antwoorden op veelgestelde vragen over het oplossen van problemen met persoonlijke instellingen.

## <a name="learning-loop"></a>Learning-lus

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>De learning-lus lijkt niet te leren. Hoe kan ik dit oplossen?

Voor de learning-lus moeten enkele duizenden belonings aanroepen worden uitgevoerd voordat rang gegeven worden. 

Als u niet zeker weet hoe uw leer proces momenteel zich gedraagt, voert u een [offline-evaluatie](concepts-offline-evaluation.md)uit en past u het gecorrigeerde leer beleid toe. 

## <a name="next-steps"></a>Volgende stappen

[De update frequentie van het model configureren](how-to-settings.md#model-update-frequency)