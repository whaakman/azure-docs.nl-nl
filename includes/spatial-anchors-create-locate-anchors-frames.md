---
ms.openlocfilehash: 10eb16dc1757255c77ed8967dc132f42660e070f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110552"
---
## <a name="provide-frames-to-the-session"></a>Geef frames aan de sessie

De sessie ruimtelijke anker werkt via het toewijzen van de vrije ruimte rond de gebruiker. Hiermee voorkomt u om te bepalen waar ankers zich bevinden. Mobiele platforms (iOS en Android) vereist een systeemeigen aanroep van de camerafeed om op te halen frames uit AR-bibliotheek van uw platform. Daarentegen scannen HoloLens voortdurend de omgeving, zodat er geen nodig voor een specifieke aanroepen, zoals met mobiele is.