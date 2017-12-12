---
title: Vervangen van een fysieke schijf in Azure-Stack | Microsoft Docs
description: Geeft een overzicht van het proces voor het vervangen van een fysieke schijf in Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: a95617a8dd2a8f296164c672e2b4b2628574ce5a
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Een fysieke schijf in de Azure-Stack vervangen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

In dit artikel beschrijft het algemene proces ter vervanging van een fysieke schijf in Azure-Stack. Als een fysieke schijf uitvalt, moet u deze zo snel mogelijk vervangen.

U kunt deze procedure gebruiken voor geïntegreerde systemen, en development kit implementaties die hot verwisselbare schijven hebben.

Nieuwe werkelijke schijf stappen varieert gebaseerd op uw hardwareleverancier oorspronkelijke leveranciers (OEM). Raadpleeg uw leverancier veld FRU (replaceable unit)-documentatie voor gedetailleerde stappen die specifiek voor uw systeem zijn. 

## <a name="review-disk-alert-information"></a>Controleer waarschuwing schijfinformatie
Als een schijf uitvalt, ontvangt u een waarschuwing dat aangeeft dat de verbinding verbroken met een fysieke schijf is. 

 ![Waarschuwing weer verbinding met de fysieke schijf is verbroken](media/azure-stack-replace-disk/DiskAlert.png)

Als u de waarschuwing opent, wordt de beschrijving van de waarschuwing bevat het scale unit-knooppunt en de locatie van de exacte fysieke sleuf voor de schijf die u moet vervangen. Azure Stack verder helpt u bij het identificeren van de defecte schijf gemaakt met behulp van LED indicator mogelijkheden.

 ## <a name="replace-the-disk"></a>Vervang de schijf

Volg de OEM-hardwareleverancier FRU instructies voor de werkelijke schijf is vervangen.

Om te voorkomen dat het gebruik van een niet-ondersteunde schijf in een geïntegreerd systeem, blokkeert het systeem worden niet ondersteund door de leverancier van uw schijven. Als u probeert een niet-ondersteunde schijf gebruiken, een nieuwe waarschuwing geeft aan dat een schijf heeft in quarantaine vanwege een niet-ondersteunde model of de firmware.

Nadat u de schijf vervangt, wordt Azure Stack automatisch de nieuwe schijf detecteert en het herstelproces virtuele schijf wordt gestart.  
 
 ## <a name="check-the-status-of-virtual-disk-repair"></a>Controleer de status van de virtuele schijf herstellen
 
 Nadat u de schijf vervangt, kunt u de status van de virtuele schijf controleren en voortgang taak herstellen met behulp van de bevoegde eindpunt. Volg deze stappen vanaf elke computer die een netwerkverbinding met het bevoorrechte eindpunt heeft.

1. Open een Windows PowerShell-sessie en verbinding maken met de bevoorrechte eindpunt.
    ````PowerShell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```` 
  
2. Voer de volgende opdracht om weer te geven van de status van de virtuele schijf:
    ````PowerShell
        Get-VirtualDisk -CimSession s-cluster
    ````
   ![De uitvoer van de PowerShell-opdracht Get-VirtualDisk](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Voer de volgende opdracht om weer te geven van de huidige taakstatus van opslag:
    ```PowerShell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ````
      ![De uitvoer van de PowerShell-opdracht Get-StorageJob](media/azure-stack-replace-disk/GetStorageJobOutput.png)

## <a name="troubleshoot-virtual-disk-repair"></a>Reparatie van de virtuele schijf oplossen

De virtuele schijf repareren taak wordt weergegeven als vastgelopen, voer de volgende opdracht om de taak opnieuw starten:
  ````PowerShell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ```` 
