---
title: Microsoft Azure Data Box Disk instellen| Microsoft Docs
description: In deze zelfstudie leest u hoe u Azure Data Box Disk instelt
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/10/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 7f382e3b6e70aadf8c6a090a3d5c049f6b5c0337
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010358"
---
# <a name="tutorial-unpack-connect-and-unlock-azure-data-box-disk"></a>Zelfstudie: de Azure Data Box-schijf uitpakken, aansluiten en ontgrendelen

In deze zelfstudie wordt beschreven hoe u de Azure Data Box-schijf uitpakt, aansluit en ontgrendelt.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw Data Box-schijf uitpakken
> * De Data Box-schijf aansluiten en ontgrendelen.

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de [zelfstudie: Azure Data Box-schijf bestellen](data-box-disk-deploy-ordered.md) voltooid.
2. U hebt uw schijven ontvangen en de taakstatus in de portal is bijgewerkt naar **Geleverd**.
3. U beschikt over een hostcomputer waarop u het ontgrendelingsprogramma voor Data Box-schijven kunt installeren. Op uw hostcomputer moet
    - Een [ondersteund besturingssysteem](data-box-disk-system-requirements.md) worden uitgevoerd.
    - [Windows PowerShell 4 zijn geïnstalleerd](https://www.microsoft.com/download/details.aspx?id=40855).
    - [.NET Framework 4.5 zijn geïnstalleerd](https://www.microsoft.com/download/details.aspx?id=30653).

## <a name="unpack-your-disks"></a>Uw schijven uitpakken

 Voer de volgende stappen uit om uw schijven uit te pakken.

1. De Data Box-schijven worden verzonden in een kleine verzenddoos. Open de doos en haal de inhoud eruit. Controleer of de doos één tot vijf SSD-schijven (Solid-State Disks) en een USB-kabel per schijf bevat. Bekijk of er aan de doos is gerommeld en of deze is beschadigd. 

    ![Verzenddoos voor de Data Box-schijf](media/data-box-disk-deploy-set-up/data-box-disk-ship-package1.png)

2. Als er aan de verzenddoos is gerommeld of als deze ernstig is beschadigd, moet u de doos niet openen. Neem in dat geval contact op met Microsoft Ondersteuning om te bepalen of de schijven in een goede staat zijn of dat er vervangende schijven moeten worden verzonden.
3. Controleer of er op de doos een doorzichtige plastic hoes zit met daarin een verzendlabel (onder het huidige label) dat voor een retourzending kan worden gebruikt. Als dit label is zoekgeraakt of is beschadigd, kunt u in Azure Portal altijd een nieuw label downloaden en afdrukken. 

    ![Verzendlabel voor de Data Box-schijf](media/data-box-disk-deploy-set-up/data-box-disk-package-ship-label.png)

4. Bewaar de doos en het verpakkingsschuim voor de retourzending van de schijven.

## <a name="connect-and-unlock-your-disks"></a>De schijven aansluiten en ontgrendelen

Voer de volgende stappen uit om uw schijven aan te sluiten en te ontgrendelen.

1. Gebruik de meegeleverde kabel om de schijf aan te sluiten op een Windows-computer met een ondersteund besturingssysteem, zoals wordt aangegeven in de vereisten. 

    ![De Data Box-schijf aansluiten](media/data-box-disk-deploy-set-up/data-box-disk-connect-unlock.png)    
    
2. Ga in Azure Portal naar **Algemeen > Apparaatdetails**. 
3. Klik op **Ontgrendelingsprogramma voor Data Box-schijven downloaden**. 

    ![Ontgrendelingsprogramma voor Data Box-schijven downloaden](media/data-box-disk-deploy-set-up/data-box-disk-unload1.png)     

4. Pak het ontgrendelingsprogramma uit op de computer die u gebruikt om de gegevens te kopiëren.
5. Open op dezelfde computer een opdrachtpromptvenster of voer hierop Windows PowerShell uit als beheerder.
6. (Optioneel) Voer de opdracht voor systeemcontrole uit om na te gaan of de computer die u gebruikt voor het ontgrendelen van de schijf voldoet aan de besturingssysteemvereisten. Hieronder ziet u een voorbeeld van de uitvoer. 

    ```powershell
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /SystemCheck
    Successfully verified that the system can run the tool.
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ``` 

7. Ga in Azure Portal naar **Algemeen > Apparaatdetails**. Gebruik het kopieerpictogram om de wachtwoordcode te kopiëren.
8. Voer `DataBoxDiskUnlock.exe` uit en geef de wachtwoordcode op. De stationsletter die is toegewezen aan de schijf wordt weergegeven. Hieronder ziet u een voorbeeld van de uitvoer.

    ```powershell
    PS C:\WINDOWS\system32> cd C:\DataBoxDiskUnlockTool\DiskUnlock
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe
    Enter the passkeys (format: passkey1;passkey2;passkey3):
    testpasskey1
    
    Following volumes are unlocked and verified.
    Volume drive letters: D:
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```

9. Herhaal de stappen 6 tot 8 wanneer u in de toekomst opnieuw schijven plaatst. Gebruik de Help-opdracht als u hulp nodig hebt met het ontgrendelingsprogramma voor Data Box-schijven.   

    ```powershell
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /help
    USAGE:
    DataBoxUnlock /PassKeys:<passkey_list_separated_by_semicolon>
    
    Example: DataBoxUnlock /PassKeys:<your passkey>
    Example: DataBoxUnlock /SystemCheck
    Example: DataBoxUnlock /Help
    
    /PassKeys:       Get this passkey from Azure DataBox Disk order. The passkey unlocks your disks.
    /SystemCheck:    This option checks if your system meets the requirements to run the tool.
    /Help:           This option provides help on cmdlet usage and examples.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```  
10. Wanneer de schijf is ontgrendeld, kunt u de inhoud van de schijf bekijken.    

    ![Inhoud van de Data Box-schijf](media/data-box-disk-deploy-set-up/data-box-disk-content.png) 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie zijn verschillende onderwerpen besproken over de Azure Data Box-schijf, zoals:

> [!div class="checklist"]
> * Uw Data Box-schijf uitpakken
> * Data Box-schijven aansluiten en ontgrendelen


Ga naar de volgende zelfstudie om te lezen hoe u gegevens kopieert naar uw Data Box-schijf.

> [!div class="nextstepaction"]
> [Gegevens kopiëren naar uw Data Box-schijf](./data-box-disk-deploy-copy-data.md)

