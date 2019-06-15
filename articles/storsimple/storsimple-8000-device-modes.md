---
title: StorSimple-apparaatmodus wijzigen | Microsoft Docs
description: Beschrijving van de StorSimple-apparaat-modi en wordt uitgelegd hoe u Windows PowerShell voor StorSimple gebruiken om te wijzigen van de modus van het apparaat.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e55964beff48df6ce24d99c01975d39b662f1612
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60576086"
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>De apparatuurmodus op uw StorSimple-apparaat wijzigen

Dit artikel bevat een korte beschrijving van de verschillende modi waarin uw StorSimple-apparaat kan worden uitgevoerd. Uw StorSimple-apparaat kunt werken in drie modi: normaal, onderhoud en herstel.

Na het lezen van dit artikel, wordt u het volgende weten:

* Wat de StorSimple-apparaat-modi
* Hoe om te achterhalen welke modus het StorSimple-apparaat is in
* Het wijzigen van een normale naar de onderhoudsmodus en *omgekeerd*

De bovenstaande beheertaken kunnen alleen worden uitgevoerd via de Windows PowerShell-interface van uw StorSimple-apparaat.

## <a name="about-storsimple-device-modes"></a>Over de modi van de StorSimple-apparaat

Uw StorSimple-apparaat kan werken in de modus voor normale, het onderhoud of herstel. Elk van deze modi wordt kort hieronder beschreven.

### <a name="normal-mode"></a>Normale modus

Dit wordt gedefinieerd als de normale operationele modus voor een volledig geconfigureerde StorSimple-apparaat. Standaard moet uw apparaat in de normale modus.

### <a name="maintenance-mode"></a>Onderhoudsmodus

Soms kan het StorSimple-apparaat moet in de onderhoudsmodus worden geplaatst. In deze modus kunt u onderhoud uit te voeren op het apparaat en het installeren van updates waarvoor de computer, zoals die zijn gerelateerd aan de schijffirmware.

U kunt het systeem plaatsen in de onderhoudsmodus alleen via de Windows PowerShell voor StorSimple. Alle i/o-aanvragen zijn in deze modus onderbroken. Services, zoals niet-vluchtige RAM-geheugen (NVRAM) of de clustering-service worden ook gestopt. Beide controllers zijn opnieuw gestart wanneer u opgeven of deze modus sluit. Wanneer u de onderhoudsmodus afsluit, worden alle services wordt hervat en moeten in orde. Dit kan enkele minuten duren.

> [!NOTE]
> **In de onderhoudsmodus wordt alleen ondersteund op een apparaat naar behoren werkt. Het wordt niet ondersteund op een apparaat waarop een of beide van de controllers niet werken.**


### <a name="recovery-mode"></a>Modus voor herstel

Herstelmodus kan worden aangeduid met 'Windows veilige modus met netwerkondersteuning'. Herstelmodus praat met het team van Microsoft Support en kan ze uit te voeren van diagnostische gegevens op het systeem. Het voornaamste doel van de herstelmodus is om op te halen van het systeemlogboek in Logboeken.

Als uw systeem in de herstelmodus gaat, neemt u contact op met Microsoft Support voor de volgende stappen. Ga voor meer informatie naar [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md).

> [!NOTE]
> **U kunt het apparaat kan niet plaatsen in de herstelmodus. Als het apparaat in orde is, probeert de herstelmodus om op te halen van het apparaat in een status waarin medewerkers van Microsoft Support, deze kunnen bekijken.**

## <a name="determine-storsimple-device-mode"></a>StorSimple-apparaatmodus bepalen

#### <a name="to-determine-the-current-device-mode"></a>Om te bepalen van de huidige modus van het apparaat

1. Meld u aan bij de seriële console van het apparaat met de volgende stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Bekijk het bannerbericht aangegeven in het menu van de seriële console van het apparaat. Dit bericht geeft expliciet aan of het apparaat zich in de modus voor onderhoud of herstel. Als het bericht bevat geen specifieke gegevens die betrekking hebben op het systeem-modus, wordt het apparaat is in de normale modus.

## <a name="change-the-storsimple-device-mode"></a>De StorSimple-apparaatmodus wijzigen

U kunt het StorSimple-apparaat in de onderhoudsmodus (van de normale modus) voor het uitvoeren van onderhoud of installeren van updates voor de onderhoudsmodus plaatsen. Voer de volgende procedures om in de of de onderhoudsmodus afsluit.

> [!IMPORTANT]
> Voordat u de onderhoudsmodus invoert, te controleren of beide apparaatcontrollers in orde zijn door het openen van de **apparaatinstellingen > hardwarestatus** voor uw apparaat in Azure portal. Als een of beide controllers niet in orde, neem dan contact op met Microsoft Support voor de volgende stappen. Ga voor meer informatie naar [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md).
 

#### <a name="to-enter-maintenance-mode"></a>Om in te voeren in de onderhoudsmodus

1. Meld u aan bij de seriële console van het apparaat met de volgende stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**. Als u hierom wordt gevraagd, geeft u de **beheerderswachtwoord**. Is het standaardwachtwoord: `Password1`.
3. Typ het volgende achter de opdrachtprompt 
   
    `Enter-HcsMaintenanceMode`
4. Hier ziet u een waarschuwingsbericht wordt weergegeven waarin staat dat in de onderhoudsmodus wordt alle i/o-aanvragen worden onderbroken en de verbinding met de Azure-portal-server, en wordt u gevraagd om bevestiging. Type **Y** in te voeren in de onderhoudsmodus.
5. Beide controllers wordt opnieuw opgestart. Wanneer de computer opnieuw opgestart is, kan de banner van de seriële console wordt aangegeven dat het apparaat in de onderhoudsmodus bevindt is. Hieronder ziet u een voorbeeld van de uitvoer.

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

#### <a name="to-exit-maintenance-mode"></a>Om af te sluiten in de onderhoudsmodus

1. Meld u op de seriële console van het apparaat. Controleer of in het bannerbericht aangegeven dat het apparaat in de onderhoudsmodus bevindt.
2. Typ in de opdrachtprompt:
   
    `Exit-HcsMaintenanceMode`
3. Een waarschuwingsbericht wordt weergegeven en een bevestigingsbericht weergegeven. Type **Y** om af te sluiten in de onderhoudsmodus.
4. Beide controllers wordt opnieuw opgestart. Wanneer de computer opnieuw opgestart is, de banner van de seriële console geeft aan dat het apparaat in de normale modus. Hieronder ziet u een voorbeeld van de uitvoer.

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

Meer informatie over het [toepassen van updates voor de normale en onderhoud](storsimple-update-device.md) op uw StorSimple-apparaat.

