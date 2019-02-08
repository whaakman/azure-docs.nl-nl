---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: dc50f94ae9b207961a71480c2fc172e88db79cf4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55888661"
---
#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>Reguliere om updates te installeren via Windows PowerShell voor StorSimple
1. Open de seriële console van het apparaat en selecteer optie 1, **Meld u aan met volledige toegang tot de**. Typ het wachtwoord. Is het standaardwachtwoord *Wachtwoord1*. 
2. Typ in de opdrachtprompt:
   
     `Get-HcsUpdateAvailability`
   
    U krijgt een bericht als er updates beschikbaar zijn en of de updates verstorende of niet zijn.
3. Typ in de opdrachtprompt:
   
     `Start-HcsUpdate`
   
    Het updateproces wordt gestart.

> [!IMPORTANT]
> * Met deze opdracht geldt alleen voor regelmatige updates. U deze opdracht uitvoert op slechts één domeincontroller, maar beide controllers wordt bijgewerkt. 
> * Merkt u wellicht een failover van de controller tijdens het updateproces; echter, de failover heeft geen invloed op beschikbaarheid van het systeem of de bewerking.
> 
> 

