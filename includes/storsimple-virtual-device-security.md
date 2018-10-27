---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: cb160a140b5c0cb184a5172da10ade0de37c4fed
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164223"
---
<!--v-sharos 10/13/2105 virtual device security-->

Houd rekening met de volgende beveiligingsoverwegingen wanneer u de virtuele StorSimple-apparaat:

* Het virtuele apparaat wordt beveiligd via uw Microsoft Azure-abonnement. Dit betekent dat als u van het virtuele apparaat gebruikmaakt en uw Azure-abonnement is aangetast, de gegevens die zijn opgeslagen op uw virtuele apparaat zijn ook gevoelig.
* De openbare sleutel van het certificaat dat wordt gebruikt voor het versleutelen van gegevens die zijn opgeslagen in Azure StorSimple wordt veilig beschikbaar gemaakt voor de klassieke Azure portal en de persoonlijke sleutel met de StorSimple-apparaat wordt bewaard. Op de virtuele StorSimple-apparaat, worden de openbare en persoonlijke sleutels worden opgeslagen in Azure.
* Het virtuele apparaat wordt gehost in Microsoft Azure datacenter.

