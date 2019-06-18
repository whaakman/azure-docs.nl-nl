---
ms.openlocfilehash: 10eb16dc1757255c77ed8967dc132f42660e070f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66110552"
---
## <a name="provide-frames-to-the-session"></a>Geef frames aan de sessie

De sessie ruimtelijke anker werkt via het toewijzen van de vrije ruimte rond de gebruiker. Hiermee voorkomt u om te bepalen waar ankers zich bevinden. Mobiele platforms (iOS en Android) vereist een systeemeigen aanroep van de camerafeed om op te halen frames uit AR-bibliotheek van uw platform. Daarentegen scannen HoloLens voortdurend de omgeving, zodat er geen nodig voor een specifieke aanroepen, zoals met mobiele is.