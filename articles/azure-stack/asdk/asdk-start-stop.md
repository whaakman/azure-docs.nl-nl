---
title: Starten en stoppen van de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Informatie over het starten en naar beneden Azure Stack Development Kit (ASDK) af te sluiten.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: dfb565803746ecdda9b36a4e12a3c3f2b4d9e0d0
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Starten en stoppen van de Azure Stack Development Kit (ASDK)
Het is niet raadzaam gewoon de hostcomputer ASDK opnieuw te starten. In plaats daarvan moet u de procedures in dit artikel correct afsluiten en opnieuw starten van services ASDK volgen. 

## <a name="stop-azure-stack"></a>Azure Stack stoppen 
Om op de Stack van Azure-services en de hostcomputer ASDK correct wordt afgesloten, gebruikt u de volgende PowerShell-opdrachten:

1. Meld u aan als AzureStack\CloudAdmin op de hostcomputer ASDK.
2. Open PowerShell als beheerder (niet PowerShell ISE).
3. Voer de volgende opdrachten een bevoorrechte eindpunt (PEP)-sessie tot stand brengen: 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Gebruik vervolgens in de sessie PEP de **Stop AzureStack** cmdlet Stack Azure-services stoppen en afsluiten van de hostcomputer ASDK:

   ```powershell
   Stop-AzureStack
   ```
5. Bekijk de uitvoer van PowerShell om te controleren of dat alle Azure-Stack-services zijn is afgesloten voordat de hostcomputer ASDK wordt afgesloten. Het afsluitproces duurt enkele minuten.

## <a name="start-azure-stack"></a>Start Azure Stack 
ASDK services automatisch moeten worden gestart als de computer wordt gestart. Echter ASDK infrastructuur services starten van de tijd is afhankelijk van de prestaties van de hardwareconfiguratie van ASDK host computer. Het kan enkele uren voor alle services die met succes opnieuw opstarten in sommige gevallen duren.

Ongeacht hoe de ASDK is afgesloten, moet u de volgende stappen uit om te controleren of alle Azure-Stack-services zijn gestart en volledig operationeel nadat de computer is ingeschakeld: 

1. Schakel op de hostcomputer ASDK. 
2. Meld u aan als AzureStack\CloudAdmin op de hostcomputer ASDK.
3. Open PowerShell als beheerder (niet PowerShell ISE).
4. Voer de volgende opdrachten een bevoorrechte eindpunt (PEP)-sessie tot stand brengen:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Voer vervolgens de volgende opdrachten om te controleren van de starten van de status van Azure-Stack-services in de sessie PEP:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Controleer de uitvoer om ervoor te zorgen dat Azure Stack-services zijn opnieuw gestart.

Zie voor meer informatie over de aanbevolen procedures voor het correct afsluiten en opnieuw opstarten van de Stack van het Azure-services, [starten en stoppen Azure Stack](.\.\azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Problemen met opstarten en afsluiten 
Voer deze stappen als Azure-Stack-services niet binnen twee uur na power op de hostcomputer ASDK starten:

1. Meld u aan als AzureStack\CloudAdmin op de hostcomputer ASDK.
2. Open PowerShell als beheerder (niet PowerShell ISE).
3. Voer de volgende opdrachten een bevoorrechte eindpunt (PEP)-sessie tot stand brengen:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Voer vervolgens de volgende opdrachten om te controleren van de starten van de status van Azure-Stack-services in de sessie PEP:

   ```powershell
   Test-AzureStack
   ```
5. Controleer de uitvoer en los eventuele fouten. Zie voor meer informatie [uitvoeren van een validatietest van Azure-Stack](.\.\azure-stack-diagnostic-test.md).
6. Start services van Azure-Stack in de sessie PEP opnieuw met de **Start AzureStack** cmdlet:

   ```powershell
   Start-AzureStack
   ```

Als met **Start AzureStack** resulteert in een fout optreedt, gaat u naar de [Azure Stack ondersteuningsforum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) ophalen ASDK ondersteuning voor het oplossen van problemen. 

## <a name="next-steps"></a>Volgende stappen 
Meer informatie over Azure-Stack diagnostische hulpprogramma en uitgeven van logboekregistratie, Zie [diagnostische hulpprogramma's voor een Azure-Stack](.\.\azure-stack-diagnostics.md).
