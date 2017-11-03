---
title: Azure web- en werkrollen rollen maken voor PHP | Microsoft Docs
description: Een handleiding voor PHP-web- en werkrollen rollen in een Azure-cloudservice maken en configureren van de PHP-runtime.
services: 
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 214fdcfe20f3fa4ebcbe41308404f8b7e7d15310
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-php-web-and-worker-roles"></a>PHP-web- en werkrollen rollen maken
## <a name="overview"></a>Overzicht
Deze handleiding leert u hoe u PHP-web- of worker-functies maken in een Windows-ontwikkelomgeving, een specifieke versie van PHP kiezen uit de beschikbare 'ingebouwde' versies, de PHP-configuratie wijzigen, uitbreidingen inschakelen en ten slotte implementeren naar Azure. Ook wordt beschreven hoe u configureert een web- of worker-rol voor het gebruik van een PHP-runtime (met aangepaste configuratie en -extensies) die u opgeeft.

## <a name="what-are-php-web-and-worker-roles"></a>Wat zijn de PHP-web- en werkrollen functies?
Azure biedt drie compute-modellen voor het uitvoeren van toepassingen: Azure App Service, Azure virtuele Machines en Azure Cloud Services. Alle drie modellen ondersteunen PHP. Cloudservices, waaronder web-en werkrollen, biedt *platform als een service (PaaS)*. Binnen een cloudservice biedt een Webrol een speciale server Internet Information Services (IIS) naar host front-end webtoepassingen. Een werkrol kan asynchrone langlopende of permanente taken onafhankelijk van de gebruikersinteractie of invoer uitvoeren.

Zie voor meer informatie over deze opties [Compute hosting-opties die worden geleverd door Azure](cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>De Azure SDK voor PHP downloaden
De [Azure SDK voor PHP] bestaat uit verschillende onderdelen. In dit artikel wordt gebruik van deze twee: Azure PowerShell en de Azure-emulator. Deze twee componenten kunnen worden geïnstalleerd via het Webplatforminstallatieprogramma van Microsoft. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

## <a name="create-a-cloud-services-project"></a>Een Cloud Services-project maken
De eerste stap bij het maken van een PHP-web- of worker-rol is een Azure-Service-project maken. een Azure-Service-project fungeert als een logische container voor web-en werkrollen, en van het project bevat [definitie (.csdef)] en [serviceconfiguratie (.cscfg)] bestanden.

Maak een nieuw Azure-Service-project, Azure PowerShell uitvoeren als beheerder en voer de volgende opdracht:

    PS C:\>New-AzureServiceProject myProject

Met deze opdracht wordt een nieuwe map maken (`myProject`) waaraan u web-en werkrollen kunt toevoegen.

## <a name="add-php-web-or-worker-roles"></a>PHP-web- of worker functies toevoegen
Als een PHP-Webrol aan een project toevoegen, voer de volgende opdracht uit in de hoofdmap van het project:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Gebruik deze opdracht voor een werkrol:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> De `roleName` parameter is optioneel. Als u dit weglaat, wordt de naam van de rol automatisch gegenereerd. De eerste Webrol gemaakt worden `WebRole1`, de tweede worden `WebRole2`, enzovoort. De eerste werkrol die gemaakt worden `WorkerRole1`, de tweede worden `WorkerRole2`, enzovoort.
>
>

## <a name="specify-the-built-in-php-version"></a>Geef de ingebouwde PHP-versie
Wanneer u een PHP-web- of worker-rol op een project toevoegt, worden de configuratiebestanden van het project gewijzigd zodat PHP wordt geïnstalleerd op elke web- of worker-exemplaar van uw toepassing als deze is geïmplementeerd. Overzicht van de versie van PHP die standaard worden geïnstalleerd, moet u de volgende opdracht uitvoeren:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

De uitvoer van de bovenstaande opdracht ziet er ongeveer als wat wordt hieronder weergegeven. In dit voorbeeld wordt de `IsDefault` vlag is ingesteld op `true` voor PHP 5.3.17, die aangeeft dat ze zal de versie PHP is standaard geïnstalleerd.

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

U kunt de PHP-runtimeversie instellen op een van de PHP-versies die worden vermeld. Bijvoorbeeld, de versie PHP instellen (voor een rol met de naam `roleName`) naar 5.4.0, gebruik de volgende opdracht:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> Beschikbare PHP-versies kunnen in de toekomst wijzigen.
>
>

## <a name="customize-the-built-in-php-runtime"></a>De ingebouwde PHP-runtime aanpassen
Hebt u volledige controle over de configuratie van de PHP-runtime die wordt geïnstalleerd wanneer u de stappen hierboven, met inbegrip van wijziging van `php.ini` instellingen en het inschakelen van extensies.

Volg deze stappen voor het aanpassen van de ingebouwde PHP-runtime:

1. Voeg een nieuwe map met de naam `php`toe aan de `bin` map van uw Webrol. Voor een worker-rol, moet u het toevoegen aan de hoofdmap van de rol.
2. In de `php` map maken van een andere map met de naam `ext`. Plaats een `.dll` extensiebestanden (bijv, `php_mongo.dll`) die u wilt inschakelen in deze map.
3. Voeg een `php.ini` van het bestand in de `php` map. Eventuele aangepaste uitbreidingen inschakelen en instellen van een PHP-richtlijnen in dit bestand. Bijvoorbeeld, als u wilt inschakelen `display_errors` op en schakel de `php_mongo.dll` uitbreiding, de inhoud van uw `php.ini` bestand zou als volgt zijn:

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> Alle instellingen die u niet expliciet instelt in de `php.ini` bestand dat u wordt automatisch verstrekt worden ingesteld op hun standaardwaarden. Houd er echter rekening mee dat u een complete kunt toevoegen `php.ini` bestand.
>
>

## <a name="use-your-own-php-runtime"></a>Uw eigen PHP-runtime gebruiken
In sommige gevallen, in plaats van een ingebouwde PHP-runtime selecteren en te configureren als hierboven beschreven, wilt u mogelijk Geef uw eigen PHP-runtime. U kunt bijvoorbeeld dezelfde PHP-runtime gebruiken in een web- of worker-rol die u in uw ontwikkelomgeving gebruikt. Dit maakt het eenvoudiger om ervoor te zorgen dat de toepassing heeft geen invloed op gedrag in uw productieomgeving.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Configureer een Webrol voor het gebruik van uw eigen PHP-runtime
Volg deze stappen voor het configureren van een Webrol voor het gebruik van een PHP-runtime die u opgeeft:

1. Een Azure-Service-project maken en toevoegen van een PHP-Webrol, zoals eerder is beschreven in dit onderwerp.
2. Maak een `php` map in de `bin` map die is in de hoofdmap van uw Webrol, en vervolgens uw PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enz.) toe te voegen aan de `php` map.
3. (OPTIONEEL) Als uw PHP-runtime gebruikt de [Microsoft-Drivers voor PHP voor SQL Server][sqlsrv drivers], moet u uw Webrol installeren configureren [SQL Server Native Client 2012] [ sql native client] wanneer deze is ingericht. U doet dit door toevoegen de [sqlncli.msi x64 installatieprogramma] naar de `bin` map in de hoofdmap van uw Webrol. Het opstartscript beschreven in de volgende stap wordt het installatieprogramma achtergrond uitgevoerd wanneer de rol is ingericht. Als uw PHP-runtime niet voor de Microsoft-Drivers voor PHP voor SQL Server gebruikt wordt, kunt u de volgende regel verwijderen uit het script wordt weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Definieer een starten van de taak die wordt geconfigureerd [Internet Information Services (IIS)] [ iis.net] voor het gebruik van uw PHP-runtime voor de verwerking van aanvragen voor `.php` pagina's. Om dit te doen, opent u de `setup_web.cmd` bestand (in de `bin` -bestand van de hoofdmap van de Webrol) in een teksteditor en vervang de inhoud door het volgende script:

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
5. U kunt uw toepassingsbestanden toevoegt aan de hoofdmap van uw Webrol. Dit is de hoofdmap van de webserver.
6. Uw toepassing te publiceren, zoals beschreven in de [uw toepassing publiceren](#publish-your-application) hieronder.

> [!NOTE]
> De `download.ps1` script (in de `bin` map van de Webrol hoofdmap) kan worden verwijderd nadat u de stappen die hierboven worden beschreven voor het gebruik van uw eigen PHP-runtime.
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Configureer een werkrol voor het gebruik van uw eigen PHP-runtime
Volg deze stappen voor het configureren van een werkrol voor het gebruik van een PHP-runtime die u opgeeft:

1. Een Azure-Service-project maken en toevoegen van een PHP-werkrol zoals eerder is beschreven in dit onderwerp.
2. Maak een `php` map in de hoofdmap van de worker-rol, en vervolgens uw PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enzovoort) toevoegen aan de `php` map.
3. (OPTIONEEL) Als uw PHP-runtime gebruikt [Microsoft-Drivers voor PHP voor SQL Server][sqlsrv drivers], moet u configureren van uw werkrol voor het installeren van [SQL Server Native Client 2012] [ sql native client] wanneer deze is ingericht. U doet dit door toevoegen de [sqlncli.msi x64 installatieprogramma] naar de hoofdmap van de worker-rol. Het opstartscript beschreven in de volgende stap wordt het installatieprogramma achtergrond uitgevoerd wanneer de rol is ingericht. Als uw PHP-runtime niet voor de Microsoft-Drivers voor PHP voor SQL Server gebruikt wordt, kunt u de volgende regel verwijderen uit het script wordt weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Definieer een starten van de taak die wordt toegevoegd uw `php.exe` uitvoerbare aan de werkrol omgevingsvariabele PATH wanneer de rol is ingericht. Om dit te doen, opent u de `setup_worker.cmd` -bestand (in de hoofdmap van de werkrol) in een teksteditor en vervang de inhoud door het volgende script:

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
5. U kunt uw toepassingsbestanden toevoegt aan de hoofdmap van uw werkrol.
6. Uw toepassing te publiceren, zoals beschreven in de [uw toepassing publiceren](#publish-your-application) hieronder.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Uw toepassing uitvoeren in de berekenings- en emulators
De Azure-emulators bieden een lokale omgeving waarin u uw Azure-toepassing testen kunt voordat u deze naar de cloud implementeert. Er zijn een aantal verschillen tussen de emulators en de Azure-omgeving. Om dit beter te begrijpen, Zie [de Azure-opslagemulator gebruiken voor ontwikkeling en tests](storage/common/storage-use-emulator.md).

Houd er rekening mee dat u PHP is geïnstalleerd lokaal voor het gebruik van de rekenemulator nodig hebt. De rekenemulator gebruikt uw lokale PHP-installatie uw toepassing uit te voeren.

Als u wilt uw project in de emulator uitvoert, voer de volgende opdracht uit de hoofddirectory van uw project:

    PS C:\MyProject> Start-AzureEmulator

Hier ziet u uitvoer is vergelijkbaar met het volgende:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

U kunt zien dat uw toepassing die wordt uitgevoerd in de emulator door het openen van een webbrowser en bladeren naar het lokale adres wordt weergegeven in de uitvoer (`http://127.0.0.1:81` in het bovenstaande voorbeeld van uitvoer).

Als u wilt de emulators stoppen, voert u deze opdracht uit:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Uw toepassing publiceren
Voor het publiceren van uw toepassing, moet u eerst importeren uw publicatie-instellingen met behulp van de [importeren AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet. Vervolgens u uw toepassing publiceren met behulp van kunt de [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet. Zie voor meer informatie over het ondertekenen van [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie de [PHP-ontwikkelaarscentrum](/develop/php/).

[Azure SDK voor PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[definitie (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[serviceconfiguratie (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installatieprogramma]: http://go.microsoft.com/fwlink/?LinkID=239648
