---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 560c9c177bfa693580979101e5b9343fcff7fe40
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166298"
---
### <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Installeren van updates voor de onderhoudsmodus via Windows PowerShell voor StorSimple

Wanneer u updates voor de onderhoudsmodus op de StorSimple-apparaat toepassen, worden alle i/o-aanvragen zijn onderbroken. Services, zoals niet-vluchtige RAM-geheugen (NVRAM) of de clustering-service wordt gestopt. Beide controllers start opnieuw op wanneer u opgeven of deze modus sluit. Wanneer u deze modus afsluit, worden alle services hervatten en in orde zijn. (Dit kan enkele minuten duren.)

> [!IMPORTANT]
> * Voordat het in de onderhoudsmodus bevindt, controleert u of beide apparaatcontrollers in orde in Azure portal. Als de controller niet in orde is en [contact opnemen met Microsoft ondersteuning](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) voor de volgende stappen.
> * Wanneer u in de onderhoudsmodus bevindt bent, moet u eerst bijwerken één domeincontroller en vervolgens de andere controller.

1. PuTTY gebruiken om verbinding maken met de seriële console. Volg de gedetailleerde instructies in [PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). Druk op de opdrachtregel op **Enter**. Selecteer optie 1, **Meld u aan met volledige toegang tot de**.

2. Als u wilt de controller in de onderhoudsmodus plaatst, typt u:
    
    `Enter-HcsMaintenanceMode`

    Beide controllers starten in de onderhoudsmodus.

3. Installeer de updates voor de onderhoudsmodus. Type:

    `Start-HcsUpdate`

    U wordt gevraagd om bevestiging. Nadat u de updates hebt bevestigd, worden ze geïnstalleerd op de domeincontroller die u momenteel toegang hebben tot. Nadat de updates zijn geïnstalleerd, wordt de controller wordt opnieuw opgestart.

4. De status van updates controleren. Meld u aan de peer-controller als de huidige controller is bijgewerkt en kan niet voor het verwerken van alle andere opdrachten. Type:

    `Get-HcsUpdateStatus`

    Als de `RunInProgress` is `True`, de update nog steeds bezig is. Als `RunInProgress` is `False`, betekent dit dat de update is voltooid.

5. Nadat de updates van de schijffirmware met succes zijn toegepast en de bijgewerkte controller opnieuw is opgestart, Controleer of de versie van de schijffirmware. Op de bijgewerkte controller, typt u:

    `Get-HcsFirmwareVersion`
   
    De verwachte schijf firmware-versies zijn:  `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`

6. De onderhoudsmodus afsluiten. Typ de volgende opdracht voor elke apparaatcontroller:

    `Exit-HcsMaintenanceMode`

    De controllers starten opnieuw op wanneer u de onderhoudsmodus afsluit.

7. Ga terug naar de Azure-portal. De portal kan niet worden weergegeven dat u de updates voor de onderhoudsmodus voor 24 uur hebt geïnstalleerd.