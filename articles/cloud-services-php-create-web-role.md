---
title: Azure-web- en werkrollen rollen maken voor PHP
description: Een handleiding voor het maken van PHP-web- en werkrollen rollen in een Azure-cloudservice en het configureren van de PHP-runtime.
services: ''
documentationcenter: php
author: msangapu
manager: cfowler
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/11/2018
ms.author: msangapu
ms.openlocfilehash: 83834104dd73e4381947903196ad35c3497b64a1
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425673"
---
# <a name="create-php-web-and-worker-roles"></a>PHP-web- en -werkrollen maken

## <a name="overview"></a>Overzicht

Deze handleiding wordt laten zien hoe u PHP web- of werkrollen maken in een Windows-ontwikkelomgeving, kies een specifieke versie van PHP in de 'ingebouwde' beschikbare versies, de PHP-configuratie wijzigen, uitbreidingen inschakelen en ten slotte implementeren naar Azure. Ook wordt beschreven hoe u een web- of werkrollen rol voor het gebruik van een PHP-runtime (met aangepaste configuratie en -extensies) die u configureert.

Azure biedt drie rekenmodellen voor het uitvoeren van toepassingen: Azure App Service, Azure Virtual Machines en Azure Cloud Services. Alle drie modellen ondersteunen PHP. Cloudservices, waaronder web-en medewerkerrollen, biedt *platform als een service (PaaS)*. Binnen een cloudservice biedt een Webrol een speciale Internet Information Services (IIS)-webserver voor het hosten van front-end webtoepassingen. Een werkrol kan asynchrone langlopende of permanente taken uitvoeren onafhankelijk van de gebruikersinteractie of invoer.

Zie voor meer informatie over deze opties [Computinghostingopties verstrekt door Azure](cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>De Azure SDK voor PHP downloaden

De [Azure SDK voor PHP](php-download-sdk.md) bestaat uit verschillende onderdelen. In dit artikel worden twee algoritmen gebruikt: Azure PowerShell en de Azure-emulators. Deze twee onderdelen kunnen worden geïnstalleerd via de Microsoft Web Platform Installer. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

## <a name="create-a-cloud-services-project"></a>Een Cloud Services-project maken

De eerste stap bij het maken van een PHP-web- of worker-rol is het maken van een Azure Service-project. een Azure Service-project fungeert als een logische container voor web-en werkrollen, en van het project bevat [servicedefinition (.csdef)] en [serviceconfiguratie (.cscfg)] bestanden.

Voor het maken van een nieuw project voor Azure-Service, Azure PowerShell als beheerder uitvoeren en voer de volgende opdracht uit:

    PS C:\>New-AzureServiceProject myProject

Met deze opdracht maakt u een nieuwe map (`myProject`) waaraan u web-en werkrollen kunt toevoegen.

## <a name="add-php-web-or-worker-roles"></a>PHP-web- of werkrollen rollen toevoegen

Een PHP-Webrol toevoegen aan een project, voer de volgende opdracht uit in de hoofdmap van het project:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Voor een werkrol, gebruikt u deze opdracht:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> De `roleName` parameter is optioneel. Als deze wordt weggelaten, wordt de naam van de rol automatisch gegenereerd. De eerste Webrol die gemaakt worden `WebRole1`, is de tweede `WebRole2`, enzovoort. De eerste werkrol gemaakt worden `WorkerRole1`, is de tweede `WorkerRole2`, enzovoort.
>
>

## <a name="specify-the-built-in-php-version"></a>Geef de ingebouwde PHP-versie

Wanneer u een PHP-web- of werkrollen rol aan een project toevoegt, worden de configuratiebestanden van het project gewijzigd zodat PHP wordt geïnstalleerd op elke web- of werkrollen exemplaar van uw toepassing wanneer deze is geïmplementeerd. Als u wilt zien van de versie van PHP die standaard worden geïnstalleerd, moet u de volgende opdracht uitvoeren:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

De uitvoer van de bovenstaande opdracht ziet er vergelijkbaar met wat hieronder wordt weergegeven. In dit voorbeeld wordt de `IsDefault` vlag is ingesteld op `true` voor PHP 5.3.17, waarmee wordt aangegeven dat dit de standaard-PHP-versie geïnstalleerd is.

```
Runtime Version     PackageUri                      IsDefault
------- -------     ----------                      ---------
Node 0.6.17         http://nodertncu.blob.core...   False
Node 0.6.20         http://nodertncu.blob.core...   True
Node 0.8.4          http://nodertncu.blob.core...   False
IISNode 0.1.21      http://nodertncu.blob.core...   True
Cache 1.8.0         http://nodertncu.blob.core...   True
PHP 5.3.17          http://nodertncu.blob.core...   True
PHP 5.4.0           http://nodertncu.blob.core...   False
```

U kunt de PHP-runtimeversie instellen op een van de PHP-versies die worden vermeld. Bijvoorbeeld, om in te stellen de PHP-versie (voor een rol met de naam van de `roleName`) naar 5.4.0, gebruik de volgende opdracht:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> Beschikbare versies van PHP kunnen in de toekomst wijzigen.
>
>

## <a name="customize-the-built-in-php-runtime"></a>Aanpassen van de ingebouwde PHP-runtime

Hebt u volledige controle over de configuratie van de PHP-runtime die wordt geïnstalleerd wanneer u de stappen hierboven, met inbegrip van de wijziging van `php.ini` instellingen en -extensies in te schakelen.

Volg deze stappen voor het aanpassen van de ingebouwde PHP-runtime:

1. Toevoegen van een nieuwe map met de naam `php`, naar de `bin` map van uw Webrol. Voor een werkrol, moet u deze naar de hoofdmap van de rol toevoegen.
2. In de `php` map, maakt u een andere map met de naam `ext`. Plaats een `.dll` extensiebestanden (bijvoorbeeld `php_mongo.dll`) die u wilt inschakelen in deze map.
3. Voeg een `php.ini` van het bestand in de `php` map. Een aangepaste extensie inschakelen en instellen van een PHP-richtlijnen in dit bestand. Bijvoorbeeld, als u wilt schakelen `display_errors` op en schakel de `php_mongo.dll` uitbreiding, de inhoud van uw `php.ini` bestand zou als volgt:

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> Alle instellingen die u niet expliciet instelt in de `php.ini` bestand dat u wordt automatisch verstrekt worden ingesteld op hun standaardwaarden. Echter, houd er rekening mee dat u een complete toevoegen kunt `php.ini` bestand.
>
>

## <a name="use-your-own-php-runtime"></a>Gebruik uw eigen PHP-runtime

In sommige gevallen, in plaats van een ingebouwde PHP-runtime selecteren en te configureren als hierboven beschreven, kunt u uw eigen PHP-runtime bieden. U kunt bijvoorbeeld de dezelfde PHP-runtime gebruiken in een web- of werkrol die u in uw ontwikkelomgeving gebruikt. Dit maakt het eenvoudiger om ervoor te zorgen dat de toepassing niet gedrag in uw productieomgeving veranderen zullen.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Een Webrol voor het gebruik van uw eigen PHP-runtime configureren

Volg deze stappen voor het configureren van een Webrol voor het gebruik van een PHP-runtime die u opgeeft:

1. Een Azure Service-project maken en toevoegen van een PHP-Webrol, zoals eerder in dit onderwerp wordt beschreven.
2. Maak een `php` map in de `bin` map die in de hoofdmap van uw Webrol, en vervolgens uw PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enzovoort) toe te voegen aan de `php` map.
3. (OPTIONEEL) Als uw PHP-runtime maakt gebruik van de [Microsoft Drivers voor PHP voor SQL Server][sqlsrv drivers], moet u uw Webrol installeren configureren [SQL Server Native Client 2012] [ sql native client] wanneer deze is ingericht. Om dit te doen, toevoegen de [sqlncli.msi x64 installatieprogramma] naar de `bin` map in de hoofdmap van uw Webrol. Het opstartscript die worden beschreven in de volgende stap wordt het installatieprogramma op de achtergrond worden uitgevoerd wanneer de rol is ingericht. Als uw PHP-runtime niet voor de Microsoft-Drivers voor PHP voor SQL Server gebruikt wordt, kunt u de volgende regel verwijderen uit het script dat wordt weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Definieer een opstarttaak die wordt geconfigureerd [Internet Information Services (IIS)] [ iis.net] uw PHP-runtime gebruiken voor het afhandelen van aanvragen voor `.php` pagina's. Om dit te doen, opent u de `setup_web.cmd` bestand (in de `bin` -bestand van de hoofdmap van uw Webrol) in een teksteditor en vervang de inhoud door het volgende script:

    ```cmd
    @ECHO ON
    cd "%~dp0"

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
    SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"
    ```
5. Uw toepassingsbestanden toevoegt aan de hoofdmap van uw Webrol. Dit is de hoofdmap van de webserver.
6. Publiceren van uw toepassing, zoals beschreven in de [uw toepassing publiceren](#publish-your-application) onderstaande sectie.

> [!NOTE]
> De `download.ps1` script (in de `bin` map van de hoofdmap van de Webrol) kan worden verwijderd nadat u de stappen beschreven voor het gebruik van uw eigen PHP-runtime.
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Een werkrol voor het gebruik van uw eigen PHP-runtime configureren

Volg deze stappen voor het configureren van een werkrol voor het gebruik van een PHP-runtime die u opgeeft:

1. Een Azure Service-project maken en toevoegen van een PHP-werkrol, zoals eerder in dit onderwerp wordt beschreven.
2. Maak een `php` map in de hoofdmap van de worker-rol, en vervolgens uw PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enzovoort) toevoegen aan de `php` map.
3. (OPTIONEEL) Als uw PHP-runtime wordt gebruikt [Microsoft Drivers voor PHP voor SQL Server][sqlsrv drivers], moet u configureren van uw werkrol voor het installeren van [SQL Server Native Client 2012] [ sql native client] wanneer deze is ingericht. Om dit te doen, voeg de [sqlncli.msi x64 installatieprogramma] naar de hoofdmap van de werkrol. Het opstartscript die worden beschreven in de volgende stap wordt het installatieprogramma op de achtergrond worden uitgevoerd wanneer de rol is ingericht. Als uw PHP-runtime niet voor de Microsoft-Drivers voor PHP voor SQL Server gebruikt wordt, kunt u de volgende regel verwijderen uit het script dat wordt weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Definieer een opstarttaak die wordt toegevoegd de `php.exe` uitvoerbaar bestand voor de werkrol van de variabele PATH. wanneer de rol is ingericht. Om dit te doen, opent u de `setup_worker.cmd` (in de hoofdmap van de werkrol) in een teksteditor het bestand en vervang de inhoud door het volgende script:

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service to the web root directory...
    icacls ..\ /grant "Network Service":(OI)(CI)W
    if %ERRORLEVEL% neq 0 goto error
    echo OK

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    setx Path "%PATH%;%~dp0php" /M

    if %ERRORLEVEL% neq 0 goto error

    echo SUCCESS
    exit /b 0

    :error

    echo FAILED
    exit /b -1
    ```
5. Uw toepassingsbestanden toevoegt aan de hoofdmap van uw werkrol.
6. Publiceren van uw toepassing, zoals beschreven in de [uw toepassing publiceren](#publish-your-application) onderstaande sectie.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Uw toepassing uitvoert in de berekenings- en emulatoren

De Azure-emulators bieden een lokale omgeving waarin u uw Azure-toepassing testen kunt voordat u deze naar de cloud implementeren. Er zijn enkele verschillen tussen de emulator en de Azure-omgeving. Zie voor meer informatie over dit beter, [de Azure-opslagemulator gebruiken voor ontwikkelen en testen](storage/common/storage-use-emulator.md).

Houd er rekening mee dat u PHP zijn geïnstalleerd lokaal voor het gebruik van de rekenemulator moet hebben. De rekenemulator wordt uw lokale PHP-installatie voor uw toepassing gebruiken.

Als u wilt uw project in de emulator uitvoert, dan de volgende opdracht vanuit de hoofdmap van uw project:

    PS C:\MyProject> Start-AzureEmulator

Hier ziet u uitvoer die vergelijkbaar is met dit:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

U kunt uw toepassing die in de emulator wordt uitgevoerd door een webbrowser openen en te bladeren naar het lokale adres weergegeven in de uitvoer zien (`http://127.0.0.1:81` in het bovenstaande voorbeeld van uitvoer).

Als u wilt stoppen met de emulator, deze opdracht wordt uitgevoerd:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Uw toepassing publiceren

Voor het publiceren van uw toepassing, moet u eerst importeren uw publicatie-instellingen met behulp van de [importeren AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/servicemanagement/azure/import-azurepublishsettingsfile) cmdlet. Vervolgens kunt u uw toepassing met behulp van de [Publish-AzureServiceProject](https://docs.microsoft.com/powershell/module/servicemanagement/azure/publish-azureserviceproject) cmdlet. Zie voor meer informatie over het ondertekenen van [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie de [PHP-ontwikkelaarscentrum](https://azure.microsoft.com/develop/php/).

[install ps and emulators]: https://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[servicedefinition (.csdef)]: https://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[serviceconfiguratie (.cscfg)]: https://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: https://www.iis.net/
[sql native client]: https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation
[sqlsrv drivers]: https://php.net/sqlsrv
[sqlncli.msi x64 installatieprogramma]: https://go.microsoft.com/fwlink/?LinkID=239648
