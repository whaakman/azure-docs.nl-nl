---
ms.openlocfilehash: 8e02067b32d9404a5bbdf7362b2cc32712b96250
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57907647"
---
## <a name="pause-reset-or-stop-the-session"></a>Onderbreken of stoppen van de sessie opnieuw instellen

Als u wilt stoppen van de sessie kunt u tijdelijk Stop() aanroepen. In dat geval stopt alle Volgers en verwerking van de omgeving, zelfs als u ProcessFrame() aanroepen. U kunt vervolgens aanroepen Start() om door te gaan met de verwerking. Wanneer hervat, wordt omgevingsgegevens al is vastgelegd in de sessie bijgehouden.
