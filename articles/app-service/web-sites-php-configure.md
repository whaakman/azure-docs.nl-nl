---
title: PHP configureren in Azure App Service WebApps
description: Informatie over het configureren van de standaard PHP-installatie of het toevoegen van een aangepaste PHP-installatie voor Web-Apps in Azure App Service.
services: app-service
documentationcenter: php
author: msangapu
manager: cfowler
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/11/2018
ms.author: msangapu
ms.openlocfilehash: 3dbd41756ede8577fee43d9758e39eb36130a6ab
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>PHP configureren in Azure App Service WebApps

## <a name="introduction"></a>Inleiding

Deze handleiding laat zien hoe het configureren van de ingebouwde PHP-runtime voor Web-Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), geeft u een aangepaste PHP-runtime en uitbreidingen inschakelen. Voor het gebruik van App Service moet zich aanmelden voor de [gratis proefversie]. Als u optimaal van deze handleiding, moet u eerst een PHP-web-app maken in App Service.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a>Procedure: ingebouwde PHP-versie wijzigen

PHP 5.6 is standaard geïnstalleerd en wordt onmiddellijk beschikbaar voor gebruik bij het maken van een App Service-web-app. De beste manier om de beschikbare versie revisie, de standaardconfiguratie en de ingeschakelde extensies is voor het implementeren van een script dat roept de [phpinfo()] functie.

De versies van PHP 7.0 en PHP 7.2 zijn ook beschikbaar, maar niet standaard ingeschakeld. Voer een van deze methoden voor het bijwerken van de PHP-versie:

### <a name="azure-portal"></a>Azure Portal

1. Blader naar uw web-app in de [Azure-portal](https://portal.azure.com) en klik op de **instellingen** knop.

    ![Instellingen voor Web-App][settings-button]
1. Van de **instellingen** blade Selecteer **toepassingsinstellingen** en kiest u de nieuwe versie van PHP.

    ![Toepassingsinstellingen][application-settings]
1. Klik op de **opslaan** knop aan de bovenkant van de **Web-appinstellingen** blade.

    ![Configuratie-instellingen opslaan][save-button]

### <a name="azure-powershell-windows"></a>Azure PowerShell (Windows)

1. Open Azure PowerShell en meld u aan bij uw account:

        PS C:\> Connect-AzureRmAccount
1. Stel de PHP-versie voor de web-app.

        PS C:\> Set-AzureWebsite -PhpVersion {5.6 | 7.0 | 7.2} -Name {app-name}
1. De PHP-versie is nu ingesteld. U kunt deze instellingen controleren:

        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-cli-20-linux-mac-windows"></a>Azure CLI 2.0 (Linux, Mac, Windows)

Voor het gebruik van de Azure-opdrachtregelinterface, moet u [2.0 voor Azure CLI installeren](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) op uw computer.

1. Open Terminal en meld u aan bij uw account.

        az login

1. Controleer de lijst met ondersteunde runtimes.

        az webapp list-runtimes | grep php

1. Stel de PHP-versie voor de web-app.

        az webapp config set --php-version {5.6 | 7.0 | 7.1 | 7.2} --name {app-name} --resource-group {resource-group-name}

1. De PHP-versie is nu ingesteld. U kunt deze instellingen controleren:

        az webapp show --name {app-name} --resource-group {resource-group-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Procedure: de ingebouwde PHP-configuraties wijzigen

Voor een ingebouwde PHP-runtime kunt u een van de configuratieopties wijzigen met de volgende stappen. (Zie voor meer informatie over richtlijnen php.ini [lijst van php.ini richtlijnen].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>Het wijzigen van PHP\_INI\_gebruiker, PHP\_INI\_PERDIR, PHP\_INI\_alle configuratie-instellingen

1. Voeg een [. user.ini] bestand naar de hoofdmap.
1. Configuratieinstellingen toevoegen aan de `.user.ini` bestand met dezelfde syntaxis als u wilt gebruiken in een `php.ini` bestand. Bijvoorbeeld, als u wilt inschakelen op de `display_errors` ingesteld en de `upload_max_filesize` instelt op 10M, uw `.user.ini` bestand zou deze tekst bevatten:

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
1. Implementeer uw web-app.
1. Start opnieuw op de web-app. (Opnieuw opstarten is nodig omdat de frequentie welke PHP leest `.user.ini` bestanden wordt bepaald door de `user_ini.cache_ttl` instelling, die is een systeemniveau-instelling en 300 seconden (5 minuten) is standaard. Opnieuw starten van de web-app forceert PHP lezen van de nieuwe instellingen in de `.user.ini` bestand.)

Als alternatief voor het gebruik van een `.user.ini` -bestand, kunt u de [ini_set()] functie in scripts in te stellen van de configuratieopties die niet op systeemniveau richtlijnen.

### <a name="changing-phpinisystem-configuration-settings"></a>Het wijzigen van PHP\_INI\_SYSTEM configuratie-instellingen

1. Een App-instelling toevoegen aan uw Web-App met de sleutel `PHP_INI_SCAN_DIR` en waarde `d:\home\site\ini`
1. Maak een `settings.ini` bestand met behulp van de Kudu-Console (http://&lt;-sitenaam&gt;. scm.azurewebsite.net) in de `d:\home\site\ini` directory.
1. Configuratieinstellingen toevoegen aan de `settings.ini` bestand met dezelfde syntaxis als u wilt gebruiken in een bestand php.ini. Bijvoorbeeld, als u deze wilde wijst de `curl.cainfo` instellen voor een `*.crt` bestands- en instellen 'wincache.maxfilesize' tot 512 kB uw `settings.ini` bestand zou deze tekst bevatten:

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
1. Als u wilt de wijzigingen laden, start opnieuw op uw Web-App.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Hoe: uitbreidingen in de standaard PHP-runtime inschakelen

Zoals beschreven in de vorige sectie, de beste manier om de standaard PHP-versie, de standaardconfiguratie en de extensies ingeschakeld is voor het implementeren van een script dat roept [phpinfo()]. Extra extensies inschakelen met de volgende stappen:

### <a name="configure-via-ini-settings"></a>Via ini-instellingen configureren

1. Voeg een `ext` map naar de `d:\home\site` directory.
1. Plaatsen `.dll` extensie bestanden in de `ext` directory (bijvoorbeeld `php_xdebug.dll`). Zorg ervoor dat de extensies compatibel is met de standaardversie van PHP en zijn VC9 en niet-thread-safe (nts) compatibel zijn.
1. Een App-instelling toevoegen aan uw Web-App met de sleutel `PHP_INI_SCAN_DIR` en waarde `d:\home\site\ini`
1. Maak een `ini` bestanden per `d:\home\site\ini` aangeroepen `extensions.ini`.
1. Configuratieinstellingen toevoegen aan de `extensions.ini` bestand met dezelfde syntaxis als u wilt gebruiken in een bestand php.ini. Bijvoorbeeld, als u wilt inschakelen van de extensies MongoDB en XDebug uw `extensions.ini` bestand zou deze tekst bevatten:

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
1. Uw Web-App voor het laden van de wijzigingen opnieuw opstarten

### <a name="configure-via-app-setting"></a>Via App-instelling configureren

1. Voeg een `bin` map naar de hoofdmap.
1. Plaatsen `.dll` extensie bestanden in de `bin` directory (bijvoorbeeld `php_xdebug.dll`). Zorg ervoor dat de extensies compatibel is met de standaardversie van PHP en zijn VC9 en niet-thread-safe (nts) compatibel zijn.
1. Implementeer uw web-app.
1. Blader naar uw web-app in de Azure portal en klik op de **instellingen** knop.

    ![Instellingen voor Web-App][settings-button]
1. Van de **instellingen** blade Selecteer **toepassingsinstellingen** en schuif naar de **appinstellingen** sectie.
1. In de **appinstellingen** sectie, maakt u een **PHP_EXTENSIONS** sleutel. De waarde voor deze sleutel wordt een pad relatief ten opzichte van hoofdmap website: **bin\your-ext-file**.

    ![De extensie in de app-instellingen inschakelen][php-extensions]
1. Klik op de **opslaan** knop aan de bovenkant van de **Web-appinstellingen** blade.

    ![Configuratie-instellingen opslaan][save-button]

Zend extensies worden ook ondersteund met behulp van een **PHP_ZENDEXTENSIONS** sleutel. Zodat meerdere extensies bevatten een door komma's gescheiden lijst met `.dll` bestanden voor de waarde van de app-instelling.

## <a name="how-to-use-a-custom-php-runtime"></a>Procedure: een aangepaste PHP-runtime gebruiken

App Service Web Apps kunt een PHP-runtime die u opgeeft voor het uitvoeren van PHP-scripts gebruiken in plaats van de standaard PHP-runtime. De runtime die u opgeeft, kan worden geconfigureerd met een `php.ini` -bestand dat u ook opgeven. Een aangepaste PHP-runtime gebruiken met Web-Apps, deze stappen te volgen.

1. Verkrijgen van een niet-thread-safe, VC9 of VC11 compatibele versie van PHP voor Windows. Recente versies van PHP voor Windows vindt u hier: [ http://windows.php.net/download/ ]. Oudere versies vindt u in het archief hier: [ http://windows.php.net/downloads/releases/archives/ ].
1. Wijzig de `php.ini` -bestand voor de runtime. Alle configuratieinstellingen die systeem-niveau-only richtlijnen worden genegeerd door de Web-Apps. (Zie voor meer informatie over system-niveau-only richtlijnen [lijst van php.ini richtlijnen]).
1. Eventueel extensies toevoegen aan uw PHP-runtime en inschakelen in de `php.ini` bestand.
1. Voeg een `bin` tot uw hoofdmap en de opslag van de map waarin uw PHP-runtime in deze map (bijvoorbeeld `bin\php`).
1. Implementeer uw web-app.
1. Blader naar uw web-app in de Azure portal en klik op de **instellingen** knop.

    ![Instellingen voor Web-App][settings-button]
1. Van de **instellingen** blade Selecteer **toepassingsinstellingen** en schuif naar de **Handlertoewijzingen** sectie. Voeg `*.php` veld naar de extensie en voeg het pad naar de `php-cgi.exe` uitvoerbare. Als u uw PHP-runtime in de `bin` directory in de hoofdmap van uw toepassing, het pad is `D:\home\site\wwwroot\bin\php\php-cgi.exe`.

    ![Handler in Handlertoewijzingen opgeven][handler-mappings]
1. Klik op de **opslaan** knop aan de bovenkant van de **Web-appinstellingen** blade.

    ![Configuratie-instellingen opslaan][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Hoe: Composer automatiseren met behulp van Azure

Standaard geen App Service reactie met composer.json, als u in uw PHP-project hebt. Als u [Git-implementatie](app-service-deploy-local-git.md), kunt u composer.json tijdens de verwerking inschakelen `git push` doordat de Composer-extensie.

> [!NOTE]
> U kunt [stem voor uitstekende Composer ondersteuning in App Service hier](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
>

1. In uw PHP web-app blade in de [Azure-portal](https://portal.azure.com), klikt u op **extra** > **extensies**.

    ![Azure portal-instellingenblade Composer automatiseren met behulp van Azure inschakelen](./media/web-sites-php-configure/composer-extension-settings.png)
2. Klik op **toevoegen**, klikt u vervolgens op **Composer**.

    ![Als u wilt Composer automatiseren met behulp van Azure Composer-extensie toevoegen](./media/web-sites-php-configure/composer-extension-add.png)
3. Klik op **OK** juridische voorwaarden accepteren. Klik op **OK** om opnieuw toe te voegen van de extensie.

    De **extensies geïnstalleerd** blade ziet u de Composer-extensie.
    ![Accepteer de juridische voorwaarden om in te schakelen Composer automatiseren met behulp van Azure](./media/web-sites-php-configure/composer-extension-view.png)
4. Nu in een terminalvenster op uw lokale machine uitvoeren `git add`, `git commit`, en `git push` aan uw Web-App. U ziet dat Composer afhankelijkheden die zijn gedefinieerd in composer.json wordt geïnstalleerd.

    ![GIT-implementatie met Composer automatiseren met behulp van Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie de [PHP-ontwikkelaarscentrum](/develop/php/).

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
>

[gratis proefversie]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[lijst van php.ini richtlijnen]: http://www.php.net/manual/en/ini.list.php
[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png
