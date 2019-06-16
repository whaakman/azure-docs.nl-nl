---
ms.openlocfilehash: 52dfbfca5f79a7f92848ea39eddc00aa10f05ff1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66110636"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Zoek een cloud ruimtelijke anker

Zoek een eerder geüploade cloud ruimtelijke anker is kunnen een van de belangrijkste redenen voor het gebruik van de ruimtelijke ankers Azure-bibliotheek. Als u wilt zoeken ruimtelijke cloudankers, moet u de id weten. Anker-id's kunnen worden opgeslagen in de back-end-service van uw toepassing en toegankelijk is op alle apparaten die goed bij deze kunnen worden geverifieerd. Voor een voorbeeld van deze Zie [zelfstudie: Ruimtelijke ankers delen tussen apparaten](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Exemplaar maken van een `AnchorLocateCriteria` object, stelt u de id's die u zoekt en roep de `CreateWatcher` methode voor de sessie door te geven uw `AnchorLocateCriteria`.
