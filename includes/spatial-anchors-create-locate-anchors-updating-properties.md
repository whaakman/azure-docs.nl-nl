---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66110512"
---
## <a name="update-properties"></a>De eigenschappen bijwerken

Voor het bijwerken van de eigenschappen van een anker, gebruikt u de `UpdateAnchorProperties()` methode. Als twee of meer apparaten wilt bijwerken van eigenschappen voor het anker dezelfde op hetzelfde moment, gebruiken we een model optimistische gelijktijdigheid. Wat betekent dat de eerste schrijfactie wint.  Alle andere schrijfbewerkingen krijgt een foutmelding "Gelijktijdigheid": een vernieuwing van de eigenschappen nodig zou zijn voordat u doorgaat.
