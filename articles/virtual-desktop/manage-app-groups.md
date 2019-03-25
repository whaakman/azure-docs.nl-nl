---
title: App-groepen beheren voor Windows Virtual Desktop Preview - Azure
description: Beschrijft hoe u voor het instellen van Windows Virtual Desktop Preview tenants in Azure Active Directory.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 2baabe6837d25cce5e9f5d9e1071af8417fe5f4d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401859"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Zelfstudie: App-groepen beheren voor Windows Virtual Desktop Preview

De standaard-app-groep gemaakt voor een nieuwe groep van de host Windows Virtual Desktop Preview publiceert ook het volledige bureaublad. Bovendien kunt u een of meer RemoteApp-toepassingsgroepen voor de host-groep maken. Volg deze zelfstudie als u wilt een RemoteApp-app-groep maken en publiceren van afzonderlijke apps in het menu Start.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een RemoteApp-groep maken.
> * Toegang verlenen tot RemoteApps.

Voordat u begint, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) de virtuele Windows-bureaublad-module gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

## <a name="create-a-remoteapp-group"></a>Een RemoteApp-groep maken

1. Voer de volgende PowerShell-cmdlet voor het maken van een nieuwe lege RemoteApp-groep.

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. (Optioneel) Als u wilt controleren of dat de groep van toepassingen is gemaakt, kunt u de volgende cmdlet als u wilt zien van een lijst met alle toepassingsgroepen voor de groep host uitvoeren.

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Voer de volgende cmdlet om een lijst van start menu apps op de installatiekopie van de virtuele machine van de host-pool. Noteer de waarden voor **FilePath**, **IconPath**, **IconIndex**, en andere belangrijke informatie voor de toepassing die u wilt publiceren.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```

4. Voer de volgende cmdlet voor het publiceren van een nieuwe RemoteApp aan de groep van toepassingen in stap 1 hebt gemaakt.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

5. (Optioneel) Voer de volgende cmdlet om de toepassing op basis van appalias te installeren. appalias zichtbaar wanneer u de uitvoer uit stap 3 uitvoert.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> <remoteappname> -AppAlias <appalias>
   ```

6. Om te controleren dat de app is gepubliceerd, moet u de volgende cmdlet uitvoeren.

   ```powershell
   Get-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname>
   ```

7. Herhaal stappen 1-5 voor elke toepassing die u wilt publiceren voor deze app-groep.
8. Voer de volgende cmdlet om gebruikers toegang tot de RemoteApps in de app-groep.

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname> -UserPrincipalName <userupn>
   ```

## <a name="next-steps"></a>Volgende stappen

Als u uw app-groepen hebt gemaakt, kunt u service-principals maken en rollen toewijzen aan uw gebruikers. Als u wilt weten hoe u dit doet, Zie de zelfstudie voor informatie over het maken van service-principals en roltoewijzingen met PowerShell.

> [!div class="nextstepaction"]
> [Service-principals en roltoewijzingen maken met PowerShell](create-service-principal-role-powershell.md)
