---
title: Vervangen van een fysieke schijf in Azure Stack | Microsoft Docs
description: Geeft een overzicht van het proces voor het vervangen van een fysieke schijf in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2019
ms.author: mabrigg
ms.openlocfilehash: b1fb1166e05eba209eddf72d97d20011c9ee4b78
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103000"
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Vervangen van een fysieke schijf in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In dit artikel beschrijft het algemene proces ter vervanging van een fysieke schijf in Azure Stack. Als een fysieke schijf is mislukt, moet u deze zo snel mogelijk vervangen.

U kunt deze procedure gebruiken voor geïntegreerde systemen, en voor development kit implementaties met hot swappable schijven.

Nieuwe werkelijke schijf stappen verschillen, is afhankelijk van uw hardwareleverancier oorspronkelijke leveranciers (OEM). Zie van uw leverancier veld FRU (replaceable unit)-documentatie voor gedetailleerde stappen die specifiek voor uw systeem zijn.

## <a name="review-disk-alert-information"></a>Bekijk informatie over de waarschuwingen van schijf
Wanneer u een schijf uitvalt, ontvangt u een waarschuwing die aangeeft dat de verbinding verbroken met een fysieke schijf is.

 ![Waarschuwing weergeven verbinding met de fysieke schijf is verbroken](media/azure-stack-replace-disk/DiskAlert.png)

Als u de waarschuwing opent, wordt de beschrijving van de waarschuwing bevat het scale unit-knooppunt en de exacte fysieke sleuf locatie voor de schijf die u moet vervangen. Meer Azure Stack helpt u bij het identificeren van de niet-werkende schijf met behulp van LED indicator mogelijkheden.

 ## <a name="replace-the-disk"></a>De schijf vervangen

Ga als volgt uw OEM-hardwareleverancier FRU instructies voor het werkelijke schijf is vervangen.

> [!note]
> Vervang schijven voor een scale unit knooppunt tegelijk. Wacht totdat de taken voor het herstellen van virtuele schijf om te voltooien voordat u doorgaat met de volgende scale unit-knooppunt

Als u wilt voorkomen dat het gebruik van een niet-ondersteunde schijf in een geïntegreerd systeem, blokkeert het systeem worden niet ondersteund door de leverancier van uw schijven. Als u probeert om een niet-ondersteunde schijf te gebruiken, een nieuwe waarschuwing geeft aan dat een schijf is in quarantaine vanwege een niet-ondersteunde model of de firmware.

Nadat u de schijf vervangt, Azure Stack automatisch de nieuwe schijf wordt gedetecteerd en wordt het reparatieproces virtuele schijf gestart.
 
 ## <a name="check-the-status-of-virtual-disk-repair"></a>Controleer de status van de virtuele schijf herstellen
 
 Nadat u de schijf vervangt, kunt u de status van de virtuele schijf controleren en taakvoortgang herstellen met behulp van het eindpunt van de bevoegdheden. Volg deze stappen vanaf elke computer die een netwerkverbinding met het eindpunt van de bevoegdheden heeft.

1. Open een Windows PowerShell-sessie en maak verbinding met het eindpunt van de bevoegdheden.
    ```PowerShell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ``` 
  
2. Voer de volgende opdracht om de status van virtuele schijf weer te geven:
    ```PowerShell
        Get-VirtualDisk -CimSession s-cluster
    ```
   ![PowerShell-uitvoer van de opdracht Get-VirtualDisk](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Voer de volgende opdracht om de huidige status van de opslag-taak weergeven:
    ```PowerShell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ```
      ![PowerShell-uitvoer van de opdracht Get-StorageJob](media/azure-stack-replace-disk/GetStorageJobOutput.png)

## <a name="troubleshoot-virtual-disk-repair"></a>Herstel van virtuele schijf oplossen

Als de virtuele schijf herstellen weergegeven taak vastgelopen, uitvoeren van de volgende opdracht uit om de taak opnieuw starten:
  ```PowerShell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ``` 
