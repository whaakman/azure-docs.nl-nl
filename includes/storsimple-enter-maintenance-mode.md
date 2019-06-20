---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d35f0ef783a2c48f8211657bc8829635c19495aa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176062"
---
#### <a name="to-enter-maintenance-mode"></a>Om in te voeren van onderhoudsmodus
1. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**.
2. Typ het wachtwoord. Is het standaardwachtwoord **Wachtwoord1**.
3. Typ het volgende achter de opdrachtprompt
   
     `Enter-HcsMaintenanceMode`
4. Hier ziet u een waarschuwingsbericht wordt weergegeven waarin staat dat onderhoudsmodus wordt alle i/o-aanvragen worden onderbroken en de verbinding met de klassieke Azure portal-server, en u wordt gevraagd om bevestiging. Type **Y** onderhoudsmodus invoeren.
   
    Beide controllers wordt opnieuw opgestart. Wanneer de computer opnieuw opgestart is, wordt een ander bericht weergegeven dat aangeeft dat het apparaat in de onderhoudsmodus is.

