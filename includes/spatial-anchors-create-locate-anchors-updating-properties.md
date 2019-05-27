---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110512"
---
## <a name="update-properties"></a>Eigenschappen bijwerken

Voor het bijwerken van de eigenschappen van een anker, gebruikt u de `UpdateAnchorProperties()` methode. Als twee of meer apparaten wilt bijwerken van eigenschappen voor het anker dezelfde op hetzelfde moment, gebruiken we een model optimistische gelijktijdigheid. Wat betekent dat de eerste schrijfactie wint.  Alle andere schrijfbewerkingen krijgt een foutmelding "Gelijktijdigheid": een vernieuwing van de eigenschappen nodig zou zijn voordat u doorgaat.
