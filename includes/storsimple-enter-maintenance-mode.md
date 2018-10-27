---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9bd2ee708f7d27cff5d07ca7f86d925ca6d2741d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164911"
---
<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Om in te voeren van onderhoudsmodus
1. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**.
2. Typ het wachtwoord. Is het standaardwachtwoord **Wachtwoord1**.
3. Typ het volgende achter de opdrachtprompt
   
     `Enter-HcsMaintenanceMode`
4. Hier ziet u een waarschuwingsbericht wordt weergegeven waarin staat dat onderhoudsmodus wordt alle i/o-aanvragen worden onderbroken en de verbinding met de klassieke Azure portal-server, en u wordt gevraagd om bevestiging. Type **Y** onderhoudsmodus invoeren.
   
    Beide controllers wordt opnieuw opgestart. Wanneer de computer opnieuw opgestart is, wordt een ander bericht weergegeven dat aangeeft dat het apparaat in de onderhoudsmodus is.

