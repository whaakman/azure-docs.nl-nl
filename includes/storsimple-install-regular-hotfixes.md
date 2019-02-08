---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8cc5dbb907c342b766cebe6da36cf580ddac5e2c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55888765"
---
#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Installeren van reguliere hotfixes via Windows PowerShell voor StorSimple
1. Verbinding maken met de seriële console van het apparaat. Zie voor meer informatie, [stap 1: Verbinding maken met de seriële console](../articles/storsimple/storsimple-update-device.md#step1).
2. Selecteer in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**. Typ het wachtwoord. Is het standaardwachtwoord **Wachtwoord1**.
3. Typ in de opdrachtprompt:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Met deze opdracht geldt alleen voor reguliere hotfixes. U deze opdracht uitvoert op slechts één domeincontroller, maar beide controllers wordt bijgewerkt.
    > Merkt u wellicht een failover van de controller tijdens het updateproces; echter, de failover heeft geen invloed op beschikbaarheid van het systeem of de bewerking.

4. Als u wordt gevraagd, geeft u het pad naar de gedeelde netwerkmap waarin de hotfixbestanden.
5. U wordt gevraagd om bevestiging. Type **Y** om door te gaan met de hotfixinstallatie.

