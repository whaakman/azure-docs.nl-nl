---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 720288aff462b0590bb9da509096a9305b9b6cc7
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55888596"
---
#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Voor het installeren van updates voor de onderhoudsmodus via Windows PowerShell voor StorSimple
1. Als u dit nog niet hebt gedaan, toegang tot de seriële console van het apparaat en selecteer optie 1, **Meld u aan met volledige toegang tot de**. 
2. Typ het wachtwoord. Is het standaardwachtwoord **Wachtwoord1**.
3. Typ in de opdrachtprompt:
   
     `Get-HcsUpdateAvailability` 
4. U krijgt een bericht als er updates beschikbaar zijn en of de updates verstorende of niet zijn. Als u wilt toepassen van updates waarvoor de computer, moet u het apparaat in de onderhoudsmodus plaatsen. Zie [stap 2: Voer onderhoudsmodus](../articles/storsimple/storsimple-update-device.md#step2) voor instructies.
5. Wanneer het apparaat in de onderhoudsmodus bevindt is, bij de opdrachtprompt, typt u: `Start-HcsUpdate`
6. U wordt gevraagd om bevestiging. Nadat u de updates hebt bevestigd, worden ze geïnstalleerd op de domeincontroller die u momenteel toegang hebben tot. Nadat de updates zijn geïnstalleerd, wordt de controller wordt opnieuw opgestart. 
7. De status van updates controleren. Type:
   
    `Get-HcsUpdateStatus`
   
    Als de `RunInProgress` is `True`, de update nog steeds bezig is. Als `RunInProgress` is `False`, betekent dit dat de update is voltooid.  
8. Wanneer de update is geïnstalleerd op de huidige controller en deze opnieuw is opgestart, verbinding maken met de andere controller en voer stap 1 t/m 6.
9. Nadat de beide controllers zijn bijgewerkt, de onderhoudsmodus afsluit. Zie [stap 4: Onderhoudsmodus afsluiten](../articles/storsimple/storsimple-update-device.md#step4) voor instructies.

