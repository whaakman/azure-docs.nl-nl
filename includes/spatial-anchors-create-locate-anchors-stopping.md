---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232422"
---
## <a name="pause-reset-or-stop-the-session"></a>Onderbreken of stoppen van de sessie opnieuw instellen

Stoppen van de sessie kunt u tijdelijk aanroepen `Stop()`. In dat geval stopt alle Volgers en verwerking van de omgeving, zelfs als u ProcessFrame() aanroepen. Vervolgens kunt u aanroepen `Start()` om de verwerking. Wanneer hervat, wordt omgevingsgegevens al is vastgelegd in de sessie bijgehouden.
