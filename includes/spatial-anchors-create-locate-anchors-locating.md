---
ms.openlocfilehash: 430302d8b1b2a01febbbe2f11057bb331ec80c28
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57907665"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Zoek een cloud ruimtelijke anker

Als u wilt zoeken ruimtelijke cloudankers, moet u de id weten. Anker-id's kunnen worden opgeslagen in de back-end-service van uw toepassing, en toegankelijk is voor alle apparaten die goed bij deze kunnen worden geverifieerd. Voor een voorbeeld van deze Zie [zelfstudie: Ruimtelijke ankers delen tussen apparaten](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Exemplaar van een object AnchorLocateCriteria maken, stelt u de id's die u zoekt, en de CreateWatcher-methode worden aangeroepen in de sessie door te geven uw AnchorLocateCriteria.
