---
title: Modus wijzigen StorSimple-apparaat | Microsoft Docs
description: Beschrijft de modus voor de StorSimple-apparaat en wordt uitgelegd hoe u Windows PowerShell voor StorSimple om de apparatuurmodus te wijzigen.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: dd160ede1189b0de544c8cf5db3b13228d212419
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>Wijzig de apparatuurmodus op uw StorSimple-apparaat

Dit artikel bevat een korte beschrijving van de verschillende modi waarin uw StorSimple-apparaat kan worden uitgevoerd. Uw StorSimple-apparaat kan worden gebruikt in de drie beschikbare modi: standaard, onderhoud en herstel.

Na het lezen van dit artikel wordt u het volgende weten:

* Welke de StorSimple-apparaat-modi
* Hoe om te achterhalen welke modus de StorSimple-apparaat bevindt zich in
* Het wijzigen van de normale naar de onderhoudsmodus en *omgekeerd*

De bovenstaande beheertaken kunnen alleen worden uitgevoerd via de Windows PowerShell-interface van uw StorSimple-apparaat.

## <a name="about-storsimple-device-modes"></a>Over de modi van StorSimple-apparaat

Uw StorSimple-apparaat kan werken in de modus Normaal, onderhoud of herstellen. Elk van deze modi wordt kort hieronder beschreven.

### <a name="normal-mode"></a>Normale modus

Dit is gedefinieerd als de normale operationele modus voor volledig geconfigureerde StorSimple-apparaat. Standaard moet uw apparaat in de normale modus.

### <a name="maintenance-mode"></a>Onderhoudsmodus

Soms moet het StorSimple-apparaat kan worden geplaatst in de onderhoudsmodus. Deze modus kunt u onderhoud uitvoeren op het apparaat en verstoren updates, zoals die betrekking hebben op firmware van de schijf installeren.

U kunt het systeem plaatsen in de onderhoudsmodus alleen via de Windows PowerShell voor StorSimple. Alle i/o-aanvragen zijn in deze modus onderbroken. Services zoals niet-vluchtige RAM-geheugen (NVRAM) of de clusteringservice ook worden gestopt. Beide domeincontrollers worden opnieuw opgestart wanneer u in of uit deze modus. Wanneer u de onderhoudsmodus afsluit, worden alle services wordt hervat en moeten in orde. Dit kan enkele minuten duren.

> [!NOTE]
> **Onderhoudsmodus wordt alleen ondersteund op een apparaat naar behoren werkt. Dit wordt niet ondersteund op een apparaat waarop een of beide van de domeincontrollers niet functioneren.**


### <a name="recovery-mode"></a>Modus voor herstel

Herstelmodus kan als 'Veilige modus voor Windows met netwerkondersteuning' worden beschreven. Herstelmodus stelt het team van Microsoft Support en kan ze voor het uitvoeren van diagnostische gegevens op het systeem. Het voornaamste doel van de herstelmodus is voor het ophalen van het systeemlogboek in Logboeken.

Als uw systeem in de herstelmodus overgaat wordt, neemt u contact op met Microsoft Support voor volgende stappen. Ga voor meer informatie naar [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md).

> [!NOTE]
> **U kunt het apparaat in herstelmodus plaatsen. Als het apparaat in orde is, probeert op te halen van het apparaat in een status waarin medewerkers van Microsoft Support Bekijk deze herstelmodus.**

## <a name="determine-storsimple-device-mode"></a>Modus van StorSimple-apparaat bepalen

#### <a name="to-determine-the-current-device-mode"></a>Om te bepalen van de huidige apparatuurmodus

1. Meld u aan bij de seriële console van het apparaat door de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Bekijk het bannerbericht aangegeven in het menu van de seriële console van het apparaat. Dit bericht expliciet geeft aan of het apparaat in de modus voor onderhoud of herstel. Als het bericht geen specifieke gegevens met betrekking tot de systeem-modus bevat, is het apparaat in de normale modus.

## <a name="change-the-storsimple-device-mode"></a>De modus StorSimple-apparaat wijzigen

U kunt het StorSimple-apparaat plaatsen in de onderhoudsmodus (van de normale modus) voor het uitvoeren van onderhoud of onderhoud modus updates installeren. Voer de volgende procedures om in of uit de onderhoudsmodus.

> [!IMPORTANT]
> Voordat u de onderhoudsmodus invoert, of beide apparaatcontrollers in orde zijn met het openen van de **apparaatinstellingen > Hardware health** voor uw apparaat in de Azure portal. Als een of beide domeincontrollers niet in orde, neem dan contact op met Microsoft Support voor de volgende stappen. Ga voor meer informatie naar [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md).
 

#### <a name="to-enter-maintenance-mode"></a>Onderhoudsmodus invoeren

1. Meld u aan bij de seriële console van het apparaat door de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**. Geef desgevraagd de **wachtwoord apparaatbeheerder**. Is het standaardwachtwoord: `Password1`.
3. Typ het volgende achter de opdrachtprompt 
   
    `Enter-HcsMaintenanceMode`
4. Hier ziet u een waarschuwingsbericht weergegeven waarin staat dat onderhoudsmodus wordt verstoord alle i/o-aanvragen en de verbinding met de Azure portal-server en wordt u gevraagd om bevestiging. Type **Y** onderhoudsmodus invoeren.
5. Beide domeincontrollers wordt opnieuw opgestart. Wanneer het opnieuw opstarten voltooid is, wordt de seriële console banner aangegeven dat het apparaat in de onderhoudsmodus is. Hieronder ziet u een voorbeeld van de uitvoer.

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="to-exit-maintenance-mode"></a>Om af te sluiten van onderhoudsmodus

1. Meld u bij de seriële console van het apparaat. Controleer de van het bannerbericht aangegeven dat uw apparaat in de onderhoudsmodus.
2. Typ het volgende achter de opdrachtprompt:
   
    `Exit-HcsMaintenanceMode`
3. Een waarschuwing en een bevestigingsbericht wordt weergegeven. Type **Y** om af te sluiten van de onderhoudsmodus.
4. Beide domeincontrollers wordt opnieuw opgestart. Wanneer de computer opnieuw opgestart is, de seriële console banner geeft aan dat het apparaat in de normale modus. Hieronder ziet u een voorbeeld van de uitvoer.

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure to install on each controller could result in data corruption. Exiting maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to exit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [normaal en onderhoud modus updates toepassen](storsimple-update-device.md) op uw StorSimple-apparaat.

