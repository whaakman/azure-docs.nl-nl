---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55888557"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Voor het installeren van hotfixes in de onderhoudsmodus via Windows PowerShell voor StorSimple
> [!IMPORTANT]
> In de onderhoudsmodus bevindt moet u eerst de hotfix toepassen op een domeincontroller en klik vervolgens op de andere controller.
> 
> 

1. Plaats het apparaat in de onderhoudsmodus. Zie [stap 2: Voer onderhoudsmodus](../articles/storsimple/storsimple-update-device.md#step2) voor instructies over het invoeren van de onderhoudsmodus.
2. Als u wilt de hotfix hebt toegepast, typt u:
   
     `Start-HcsHotfix` 
3. Als u wordt gevraagd, geeft u het pad naar de gedeelde netwerkmap waarin de hotfixbestanden.
4. U wordt gevraagd om bevestiging. Type **Y** om door te gaan met de hotfixinstallatie.
5. Nadat u de hotfix hebt toegepast op een domeincontroller, moet u zich aanmelden bij de andere controller. Pas de hotfix toe als u dit hebt gedaan voor de vorige controller.
6. Nadat u de hotfixes worden toegepast, de onderhoudsmodus afsluit. Zie [stap 4: Onderhoudsmodus afsluiten](../articles/storsimple/storsimple-update-device.md#step4) voor instructies.

