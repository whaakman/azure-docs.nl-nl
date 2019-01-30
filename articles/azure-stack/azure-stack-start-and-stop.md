---
title: Starten en stoppen van de Azure Stack | Microsoft Docs
description: Informatie over het starten en afsluiten van Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: b8d13377b2fb1545ecf88645a5e19bf5eab24c74
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238132"
---
# <a name="start-and-stop-azure-stack"></a>Starten en stoppen van de Azure Stack
U moet de procedures in dit artikel voor het correct afsluiten en opnieuw opstarten van Azure Stack-services. Afsluiten wordt fysiek uit de gehele Azure Stack-omgeving aansturen. Opstarten bevoegdheden op alle functies van de infrastructuur en tenantresources stuurt naar de energiestatus die in het geval vóór afsluiten was.

## <a name="stop-azure-stack"></a>Stop Azure Stack 

Azure Stack afgesloten met de volgende stappen uit:

1. Alle workloads die worden uitgevoerd op een van de omgeving van uw Azure Stack-tenantresources voor het afsluiten van de toekomstige voorbereiden. 

2. Open een bevoegde eindpunt sessie (PEP) van een computer met toegang tot het netwerk naar de Azure Stack ERCS VM's. Zie voor instructies [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md).

3. Uit de PEP uitvoeren:

    ```powershell
      Stop-AzureStack
    ```

4. Wachten op alle fysieke Azure Stack-knooppunten te power uitschakelen.

> [!Note]  
> U kunt de status van een fysiek knooppunt controleren door de instructies van de Original Equipment Manufacturer (OEM) die uw Azure Stack-hardware opgegeven. 

## <a name="start-azure-stack"></a>Start Azure Stack 

Azure Stack beginnen met de volgende stappen uit. Volg deze stappen, ongeacht hoe Azure Stack is gestopt.

1. Inschakelen van elk van de fysieke knooppunten in uw Azure Stack-omgeving. Controleer of u de kracht van instructies voor de fysieke knooppunten door de instructies van de Original Equipment Manufacturer (OEM) die de hardware voor uw Azure Stack opgegeven.

2. Wacht totdat de infrastructuurservices van Azure Stack wordt gestart. Azure Stack-infrastructuur-services kunnen vereisen dat twee uur aan het beginproces is voltooid. U kunt controleren of de begin-status van Azure Stack met de [ **Get-ActionStatus** cmdlet](#get-the-startup-status-for-azure-stack).

3. Zorg ervoor dat al uw tenantresources weer terug bent op de status die in het geval vóór afsluiten was. Workloads die worden uitgevoerd op tenantbronnen moet mogelijk opnieuw na het opstarten worden geconfigureerd door de manager van de werkbelasting.

## <a name="get-the-startup-status-for-azure-stack"></a>De status gestart ophalen voor Azure Stack

Haal het opstarten van de voor de routine voor het opstarten van Azure Stack met de volgende stappen uit:

1. Een bevoegde Endpoint-sessie openen van een computer met toegang tot het netwerk naar de Azure Stack ERCS VM's.

2. Uit de PEP uitvoeren:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Problemen oplossen met opstarten en afsluiten van Azure Stack

Voer de volgende stappen uit als de infrastructuur en tenant-services niet is 2 uur nadat u stroom wordt gestart op uw Azure Stack-omgeving. 

1. Een bevoegde Endpoint-sessie openen van een computer met toegang tot het netwerk naar de Azure Stack ERCS VM's.

2. Uitvoeren: 

    ```powershell
      Test-AzureStack
      ```

3. Controleer de uitvoer en los eventuele statusfouten. Zie voor meer informatie, [uitvoeren van een validatietest van Azure Stack](azure-stack-diagnostic-test.md).

4. Uitvoeren:

    ```powershell
      Start-AzureStack
    ```

5. Als met **Start AzureStack** resulteert in een storing optreedt, neem contact op met de klantondersteuning van Microsoft voor Services. 

## <a name="next-steps"></a>Volgende stappen 

Meer informatie over [diagnostische hulpprogramma's voor Azure Stack](azure-stack-diagnostics.md)
