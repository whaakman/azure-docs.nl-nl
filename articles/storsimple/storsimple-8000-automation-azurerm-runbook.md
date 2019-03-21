---
title: Azure Automation-Runbook gebruiken voor het beheren van StorSimple-apparaten | Microsoft Docs
description: Informatie over het gebruik van Azure Automation-Runbook voor het automatiseren van taken voor StorSimple
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/23/2017
ms.author: alkohli
ms.openlocfilehash: 93c77b5f678c4e6b3170d2c7612bef3f104f0b6b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58002595"
---
# <a name="use-azure-automation-runbooks-to-manage-storsimple-devices"></a>Azure Automation-runbooks gebruiken voor het beheren van StorSimple-apparaten

Dit artikel wordt beschreven hoe Azure Automation-runbooks worden gebruikt voor het beheren van uw StorSimple 8000-apparaat in Azure portal. Een voorbeeldrunbook is om u te helpen u bij de stappen voor het configureren van uw omgeving voor het uitvoeren van dit runbook opgenomen.


## <a name="configure-add-and-run-azure-runbook"></a>Configureer, toevoegen en Azure-runbook uitvoeren

Deze sectie wordt een voorbeeld van de Windows PowerShell-script voor StorSimple en details van de verschillende stappen die nodig zijn voor het script in een runbook importeren en publiceren en het runbook uit te voeren.

### <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u hebt:

* een actief Azure-abonnement dat is gekoppeld aan uw StorSimple Device Manager-service geregistreerd bij een StorSimple 8000-apparaat.


* Windows PowerShell 5.0 op uw computer is geïnstalleerd (of uw Windows-Server als host voor uw StorSimple optreden als een).


### <a name="create-automation-runbook-module-in-windows-powershell"></a>Automation-runbook-module maken in Windows PowerShell

Voor het maken van een automation-module voor de StorSimple 8000 series-Apparaatbeheer, moet u de volgende stappen uitvoeren:

1. Start Windows PowerShell. Maak een nieuwe map en wijzig de map naar de nieuwe map.

    ```
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```    
2. [NuGet-CLI downloaden](https://www.nuget.org/downloads) onder de map in de vorige stap hebt gemaakt. Er zijn verschillende versies van _nuget.exe_. Kies de versie die overeenkomt met de SDK. Elke downloadkoppeling verwijst rechtstreeks naar een _.exe_ bestand. Zorg ervoor dat u met de rechtermuisknop op en sla het bestand op uw computer in plaats van deze uitvoert vanuit de browser.

    U kunt ook de volgende opdracht uit om te downloaden en opslaan van het script in dezelfde map die u eerder hebt gemaakt, uitvoeren.
    
    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```
3. Download de SDK afhankelijk is.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```    
4. Het script downloaden uit het voorbeeld van GitHub-project.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1 -Out Monitor-Backups.ps1

    ```
5. Maak een Azure Automation-Runbook-Module voor het beheer van StorSimple 8000-serie-apparaten. Typ op de Windows Powershell-venster de volgende opdrachten:

    ```
            # set path variables
            $downloadDir = "C:\scripts\StorSimpleSDKTools"
            $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"

            #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
            mkdir "$moduleDir"
            copy "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
            copy "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir

            #Don't change the name of the Archive
            compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip

    ```

6. Controleren of een automation-module-zip-bestand is gemaakt `C:\scripts\StorSimpleSDKTools`.

    ![verify-automation-module](./media/storsimple-8000-automation-azurerm-runbook/verify-automation-module.png)

7. De volgende uitvoer wordt weergegeven wanneer het automation-module is gemaakt via de Windows PowerShell. 

    ```
    -----------------------------------------
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\WINDOWS\system32> mkdir C:\scripts\StorSimpleSDKTools

        Directory: C:\scripts

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:43 AM                StorSimpleSDKTools


    PS C:\WINDOWS\system32> cd c:\scripts\StorSimpleSDKTools
    PS C:\scripts\StorSimpleSDKTools> wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorS
    impleSDKTools\nuget.exe
    PS C:\scripts\StorSimpleSDKTools> C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8
    000series

    -------------------------------------------
    CUT              CUT  
    -------------------------------------------
    Successfully installed 'Microsoft.Azure.Management.Storsimple8000series 1.0.0' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 1.77 sec
    PS C:\scripts\StorSimpleSDKTools> C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.Active
    Directory -Version 2.28.3

    -------------------------------------------
    CUT              CUT  
    -------------------------------------------
    Successfully installed 'Microsoft.IdentityModel.Clients.ActiveDirectory 2.28.3' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 927.64 ms
    PS C:\scripts\StorSimpleSDKTools> C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Aut
    hentication -Version 2.2.9-preview

    -------------------------------------------
    CUT              CUT  
    -------------------------------------------
    Successfully installed 'Microsoft.Rest.ClientRuntime.Azure.Authentication 2.2.9-preview' to C:\scripts\StorSimpleSDKTool
    s
    Executing nuget actions took 717.48 ms
    PS C:\scripts\StorSimpleSDKTools> wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Mo
    nitor-Backups.ps1 -Out Monitor-Backups.ps1
    PS C:\scripts\StorSimpleSDKTools> # set path variables
    PS C:\scripts\StorSimpleSDKTools>             $downloadDir = "C:\scripts\StorSimpleSDKTools"
    PS C:\scripts\StorSimpleSDKTools>             $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.Sto
    rSimple8000Series"
    PS C:\scripts\StorSimpleSDKTools>
    PS C:\scripts\StorSimpleSDKTools>             #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Se
    ries"
    PS C:\scripts\StorSimpleSDKTools>             mkdir "$moduleDir"

        Directory: C:\scripts\StorSimpleSDKTools\AutomationModule


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:48 AM                Microsoft.Azure.Management.StorSimple8000Series

    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\
    lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Mic
    rosoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft
    .Rest.ClientRuntime*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $
    moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9
    -preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\l
    ib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>
    PS C:\scripts\StorSimpleSDKTools>             #Don't change the name of the Archive
    PS C:\scripts\StorSimpleSDKTools>             compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Manag
    ement.StorSimple8000Series.zip
    PS C:\scripts\StorSimpleSDKTools>

    -------------------------------------------

    ```

### <a name="import-publish-and-run-automation-runbook"></a>Importeren, publiceren en Automation-runbook uitvoeren

1. Een automation uitvoeren als-account maken in Azure portal. Om dit te doen, gaat u naar **Azure marketplace > Alles** en zoek vervolgens **Automation**. Selecteer **Automation-accounts**.

    ![Search-automatisering](./media/storsimple-8000-automation-azurerm-runbook/automation1.png)

2. In de **Automation-Account toevoegen** blade:

   1. Geef de **naam** van uw Automation-account.
   2. Selecteer de **abonnement** gekoppeld aan uw StorSimple Device Manager-service.
   3. Maak een nieuwe resourcegroep of een bestaande resourcegroep selecteren.
   4. Selecteer een **locatie** (indien mogelijk hetzelfde als wanneer de service wordt uitgevoerd).
   5. Laat de standaardwaarde **uitvoeren als-account maken** optie is geselecteerd.
   5. (Optioneel) Controleer **vastmaken aan dashboard**. Klik op **Create**.

       ![create-automation-account](./media/storsimple-8000-automation-azurerm-runbook/create-automation-account.png)

      Nadat het automation-account is gemaakt, wordt u gewaarschuwd. Voor meer informatie over het maken van een Automation-account, gaat u naar [uitvoeren als-account maken](https://docs.microsoft.com/azure/automation/automation-create-runas-account).

3. Om ervoor te zorgen dat de service StorSimple Device Manager toegang het automatiseringsaccount hebt gemaakt tot kunt, moet u de juiste machtigingen toewijzen aan het automation-account. Ga naar **toegangsbeheer** in uw StorSimple Device Manager-service. Klik op **+ toevoegen** en geef de naam van uw Azure Automation-Account. **Sla** de instellingen op.

    ![add-permissions-automation-account](./media/storsimple-8000-automation-azurerm-runbook/goto-add-roles.png)

4. In het nieuwe account, gaat u naar **gedeelde Resources > Modules** en klikt u op **+ toevoegen module**.

5. In de **toevoegen module** blade, blader naar de locatie van het ZIP-module en selecteer en open de module. Klik op **OK**.

    ![module toevoegen](./media/storsimple-8000-automation-azurerm-runbook/add-module.png)

6. Ga naar **procesautomatisering > Runbooks en klik op + toevoegen van een runbook**. In de **runbook toevoegen** blade, klikt u op **een bestaand runbook importeren**. Verwijzen naar het Windows PowerShell-script-bestand voor de **Runbook-bestand**. Het runbooktype wordt automatisch geselecteerd. Geef een naam en een optionele beschrijving voor het runbook. Klik op **Create**.

    ![module toevoegen](./media/storsimple-8000-automation-azurerm-runbook/import-runbook.png)

7. Het runbook wordt toegevoegd aan de lijst met runbooks. Selecteer en klikt u op dit runbook.

    ![click-new-runbook](./media/storsimple-8000-automation-azurerm-runbook/verify-runbook-created.png)

8. Bewerk het runbook en klik op **testvenster**. Geef de parameters op, zoals de naam van uw StorSimple Device Manager-service, de naam van het StorSimple-apparaat en het abonnement. **Start** de test. Het rapport wordt gegenereerd wanneer de uitvoering voltooid is. Ga voor meer informatie naar [hoe u een runbook testen](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook).

    ![test-runbook](./media/storsimple-8000-automation-azurerm-runbook/test-runbook.png)

9. Inspecteer de uitvoer van het runbook in het deelvenster. Als voldaan, sluit u het deelvenster. Klik op **publiceren** en wanneer u hierom wordt gevraagd om bevestiging, bevestigen en het runbook publiceren.

    ![publish-runbook](./media/storsimple-8000-automation-azurerm-runbook/publish-runbook.png)


## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).