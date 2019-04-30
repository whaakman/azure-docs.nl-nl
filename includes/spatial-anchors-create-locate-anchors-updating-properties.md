---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232518"
---
## <a name="update-properties"></a>Eigenschappen bijwerken

Voor het bijwerken van de eigenschappen van een anker, gebruikt u de `UpdateAnchorProperties()` methode. Als twee of meer apparaten wilt bijwerken van eigenschappen voor het anker dezelfde op hetzelfde moment, gebruiken we een model optimistische gelijktijdigheid. Wat betekent dat de eerste schrijfactie wint.  Alle andere schrijfbewerkingen krijgt een foutmelding "Gelijktijdigheid": een vernieuwing van de eigenschappen nodig zou zijn voordat u doorgaat.
