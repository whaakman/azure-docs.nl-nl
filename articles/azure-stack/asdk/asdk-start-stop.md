---
title: Starten en stoppen van de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Informatie over het starten en afsluiten omlaag Azure Stack Development Kit (ASDK).
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
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0b1affb708a8d481a1378fa691b6547865b0b214
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963586"
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Starten en stoppen van de Azure Stack Development Kit (ASDK)
Het verdient aanbeveling niet gewoon de hostcomputer ASDK opnieuw te starten. In plaats daarvan moet u de procedures in dit artikel voor het correct afsluiten en opnieuw opstarten van services ASDK volgen. 

## <a name="stop-azure-stack"></a>Azure Stack stoppen 
Als u wilt op Azure Stack-services en de hostcomputer ASDK correct wordt afgesloten, gebruik de volgende PowerShell-opdrachten:

1. Meld u als AzureStack\CloudAdmin op de hostcomputer ASDK.
2. Open PowerShell als beheerder (niet PowerShell ISE).
3. Voer de volgende opdrachten een bevoegde eindpunt (PEP)-sessie tot stand brengen: 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Gebruik vervolgens in de sessie PEP de **Stop-AzureStack** cmdlet Azure Stack-services stoppen en afsluiten de computer van de host ASDK:

   ```powershell
   Stop-AzureStack
   ```
5. Controleer de uitvoer PowerShell om te controleren of dat alle Azure Stack-services zijn is afgesloten voordat de hostcomputer ASDK wordt afgesloten. Het afsluitproces duurt enkele minuten.

## <a name="start-azure-stack"></a>Start Azure Stack 
ASDK services automatisch moeten worden gestart wanneer de computer wordt gestart. ASDK infrastructuur services opstarttijd is echter afhankelijk van de prestaties van de configuratie van de computer hardware voor de ASDK-host. Duurt enkele uren voor alle services die in sommige gevallen met succes opnieuw.

Ongeacht hoe de ASDK is afgesloten, moet u de volgende stappen uit om te controleren of alle Azure Stack-services zijn gestart en volledig operationeel nadat de computer wordt ingeschakeld: 

1. De stroom op de hostcomputer ASDK. 
2. Meld u als AzureStack\CloudAdmin op de hostcomputer ASDK.
3. Open PowerShell als beheerder (niet PowerShell ISE).
4. Voer de volgende opdrachten een bevoegde eindpunt (PEP)-sessie tot stand brengen:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Voer vervolgens de volgende opdrachten om te controleren of de opstartstatus van Azure Stack-services in de sessie PEP:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Controleer de uitvoer om ervoor te zorgen dat Azure Stack-services zijn opnieuw is opgestart.

Zie voor meer informatie over de aanbevolen procedures voor het correct afsluiten en opnieuw opstarten van Azure Stack-services, [starten en stoppen Azure Stack](../azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Problemen oplossen met opstarten en afsluiten 
Deze stappen uitvoert als Azure Stack-services is niet binnen twee uur nadat u op de hostcomputer ASDK power gestart:

1. Meld u als AzureStack\CloudAdmin op de hostcomputer ASDK.
2. Open PowerShell als beheerder (niet PowerShell ISE).
3. Voer de volgende opdrachten een bevoegde eindpunt (PEP)-sessie tot stand brengen:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Voer vervolgens de volgende opdrachten om te controleren of de opstartstatus van Azure Stack-services in de sessie PEP:

   ```powershell
   Test-AzureStack
   ```
5. Controleer de uitvoer en los eventuele fouten. Zie voor meer informatie, [uitvoeren van een validatietest van Azure Stack](../azure-stack-diagnostic-test.md).
6. Azure Stack-services uit binnen de sessie PEP opnieuw door het uitvoeren van de **Start AzureStack** cmdlet:

   ```powershell
   Start-AzureStack
   ```

Als met **Start AzureStack** resulteert in een storing optreedt, gaat u naar de [ondersteuningsforum voor Azure Stack](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) om op te halen ASDK ondersteuning voor probleemoplossing. 

## <a name="next-steps"></a>Volgende stappen 
Meer informatie over het diagnostisch hulpprogramma voor Azure Stack en uitgeven van logboekregistratie, Zie [diagnostische hulpprogramma's voor Azure Stack](../azure-stack-diagnostics.md).
