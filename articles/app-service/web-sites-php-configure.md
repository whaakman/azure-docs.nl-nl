---
title: PHP configureren in Azure App Service WebApps
description: Informatie over het configureren van de standaard-PHP-installatie of het toevoegen van een aangepaste PHP-installatie voor Web-Apps in Azure App Service.
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
ms.openlocfilehash: 6e7de0a7b580c0028982895487117ab98d0cd612
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503448"
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>PHP configureren in Azure App Service WebApps

## <a name="introduction"></a>Inleiding

Deze handleiding wordt beschreven hoe u de ingebouwde PHP-runtime configureren voor Web-Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)geeft u een aangepaste PHP-runtime en -extensies inschakelen. Voor het gebruik van App Service, zich aanmelden voor de [gratis proefversie]. Voor de meeste van deze handleiding, moet u eerst een PHP-web-app maken in App Service.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a>Hoe: wijzigen van de ingebouwde PHP-versie

PHP 5.6 is standaard geïnstalleerd en wordt onmiddellijk beschikbaar voor gebruik bij het maken van een App Service-web-app. De beste manier om te zien van de revisie beschikbare versie, de standaardconfiguratie en de ingeschakelde extensies is implementeren van een script dat roept de [phpinfo()] functie.

PHP 7.0 en PHP 7.2 versies zijn ook beschikbaar, maar worden niet standaard ingeschakeld. Volg een van deze methoden voor het bijwerken van de PHP-versie:

### <a name="azure-portal"></a>Azure Portal

1. Blader naar uw web-app in de [Azure-portal](https://portal.azure.com) en klik op de **instellingen** knop.

    ![Web-Appinstellingen][settings-button]
1. Uit de **instellingen** Selecteer **toepassingsinstellingen** en kiest u de nieuwe versie van PHP.

    ![Toepassingsinstellingen][application-settings]
1. Klik op de **opslaan** knop aan de bovenkant van de **Web-app-instellingen** blade.

    ![Configuratie-instellingen opslaan][save-button]

### <a name="azure-powershell-windows"></a>Azure PowerShell (Windows)

1. Open Azure PowerShell, en meld u aan bij uw account:

        PS C:\> Connect-AzureRmAccount
1. De PHP-versie voor de web-app ingesteld.

        PS C:\> Set-AzureWebsite -PhpVersion {5.6 | 7.0 | 7.2} -Name {app-name}
1. De PHP-versie is nu ingesteld. U kunt deze instellingen bevestigen:

        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-cli-20-linux-mac-windows"></a>Azure CLI 2.0 (Linux, Mac, Windows)

Voor het gebruik van de Azure Command-Line Interface, moet u [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) op uw computer.

1. Open Terminal, en meld u aan bij uw account.

        az login

1. Controleer de lijst met ondersteunde runtimes.

        az webapp list-runtimes | grep php

1. De PHP-versie voor de web-app ingesteld.

        az webapp config set --php-version {5.6 | 7.0 | 7.1 | 7.2} --name {app-name} --resource-group {resource-group-name}

1. De PHP-versie is nu ingesteld. U kunt deze instellingen bevestigen:

        az webapp show --name {app-name} --resource-group {resource-group-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Hoe: wijzigen van de ingebouwde PHP-configuraties

Voor een ingebouwde PHP-runtime, kunt u een van de configuratieopties wijzigen door de volgende stappen. (Zie voor meer informatie over richtlijnen php.ini [Lijst van php.ini-richtlijnen].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>Wijzigen van PHP\_INI\_gebruiker, PHP\_INI\_PERDIR, PHP\_INI\_alle configuratie-instellingen

1. Voeg een [. user.ini] -bestand naar de hoofdmap.
1. Configuratieinstellingen toevoegen aan de `.user.ini` bestand met dezelfde syntaxis als u wilt gebruiken in een `php.ini` bestand. Bijvoorbeeld, als u wilt inschakelen op de `display_errors` instellen en de `upload_max_filesize` instelt op 10M, uw `.user.ini` bestand zou deze tekst bevatten:

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
1. Uw web-app implementeren.
1. Start opnieuw op de web-app. (Opnieuw opstarten is nodig omdat de frequentie met welke PHP leest `.user.ini` bestanden wordt bepaald door de `user_ini.cache_ttl` instelling, die een niveau systeeminstelling is en is 300 seconden (5 minuten) standaard. Opnieuw opstarten van de web-app wordt gedwongen PHP te lezen van de nieuwe instellingen in de `.user.ini` bestand.)

Als alternatief voor het gebruik van een `.user.ini` -bestand, kunt u de [ini_set()] functie in scripts voor het instellen van configuratie-opties die niet op systeemniveau richtlijnen.

### <a name="changing-phpinisystem-configuration-settings"></a>Wijzigen van PHP\_INI\_SYSTEEMCONFIGURATIE-instellingen

1. Een App-instelling toevoegen aan uw Web-App met de sleutel `PHP_INI_SCAN_DIR` en waarde `d:\home\site\ini`
1. Maak een `settings.ini` bestand met behulp van de Kudu-Console (http://&lt;-sitenaam&gt;. scm.azurewebsite.net) in de `d:\home\site\ini` directory.
1. Configuratieinstellingen toevoegen aan de `settings.ini` bestand met dezelfde syntaxis als u wilt gebruiken in een `php.ini` bestand. Bijvoorbeeld, als u wilt verwijzen de `curl.cainfo` instelt op een `*.crt` bestands- en 'wincache.maxfilesize' instellen tot 512 kB, uw `settings.ini` bestand zou deze tekst bevatten:

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
1. Als u wilt de wijzigingen laden, start opnieuw op uw Web-App.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Hoe: in de standaard-PHP-runtime-extensies inschakelen

Zoals vermeld in de vorige sectie, de beste manier om te zien van de standaard-PHP-versie, de standaardconfiguratie en de ingeschakelde extensies is om een script waarmee wordt aangeroepen te implementeren [phpinfo()]. Extra extensies inschakelen door de volgende stappen:

### <a name="configure-via-ini-settings"></a>Configureren via de ini-instellingen

1. Voeg een `ext` map naar de `d:\home\site` directory.
1. Plaats `.dll` extensie bestanden in de `ext` directory (bijvoorbeeld `php_xdebug.dll`). Zorg ervoor dat de extensies compatibel met de standaardversie van PHP en zijn VC9 en compatibel is met niet-thread-safe (nts zijn).
1. Een App-instelling toevoegen aan uw Web-App met de sleutel `PHP_INI_SCAN_DIR` en waarde `d:\home\site\ini`
1. Maak een `ini` in het bestand `d:\home\site\ini` met de naam `extensions.ini`.
1. Configuratieinstellingen toevoegen aan de `extensions.ini` bestand met dezelfde syntaxis als u wilt gebruiken in een `php.ini` bestand. Bijvoorbeeld, als u wilt inschakelen van de extensies MongoDB en XDebug uw `extensions.ini` bestand zou deze tekst bevatten:

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
1. Start opnieuw op uw Web-App voor het laden van de wijzigingen.

### <a name="configure-via-app-setting"></a>Configureren via de App-instelling

1. Voeg een `bin` map naar de hoofdmap.
1. Plaats `.dll` extensie bestanden in de `bin` directory (bijvoorbeeld `php_xdebug.dll`). Zorg ervoor dat de extensies compatibel met de standaardversie van PHP en zijn VC9 en compatibel is met niet-thread-safe (nts zijn).
1. Uw web-app implementeren.
1. Blader naar uw web-app in Azure portal en klik op de **instellingen** knop.

    ![Web-Appinstellingen][settings-button]
1. Uit de **instellingen** Selecteer **toepassingsinstellingen** en schuif naar de **App-instellingen** sectie.
1. In de **App-instellingen** sectie, maakt u een **PHP_EXTENSIONS** sleutel. De waarde voor deze sleutel is een pad relatief ten opzichte van hoofdmap van website: **bin\your-ext-file**.

    ![De extensie in de app-instellingen inschakelen][php-extensions]
1. Klik op de **opslaan** knop aan de bovenkant van de **Web-app-instellingen** blade.

    ![Configuratie-instellingen opslaan][save-button]

Zend-extensies worden ook ondersteund met behulp van een **PHP_ZENDEXTENSIONS** sleutel. Zodat meerdere extensies bevatten een lijst met door komma's gescheiden van `.dll` bestanden voor de waarde van de app-instelling.

## <a name="how-to-use-a-custom-php-runtime"></a>Hoe: een aangepaste PHP-runtime gebruiken

In plaats van de standaard-PHP-runtime, kunt App Service Web Apps gebruiken van een PHP-runtime die u opgeeft voor het uitvoeren van PHP-scripts. De runtime die u opgeeft kan worden geconfigureerd door een `php.ini` -bestand dat u ook opgeven. Een aangepaste PHP-runtime gebruiken met Web-Apps, deze stappen te volgen.

1. Verkrijgen van een niet-thread-safe, VC9 of VC11 compatibele versie van PHP voor Windows. Recente versies van PHP voor Windows vindt u hier: [ http://windows.php.net/download/ ]. Oudere versies vindt u in het archief hier: [ http://windows.php.net/downloads/releases/archives/ ].
1. Wijzig de `php.ini` -bestand voor de runtime. Alle configuratieinstellingen die system-niveau-only richtlijnen worden genegeerd door de Web-Apps. (Zie voor meer informatie over het system-niveau-only richtlijnen [Lijst van php.ini-richtlijnen]).
1. (Optioneel) extensies toevoegen aan uw PHP-runtime en kunnen ze in de `php.ini` bestand.
1. Voeg een `bin` naar de hoofdmap en put de map waarin uw PHP-runtime in deze map (bijvoorbeeld `bin\php`).
1. Uw web-app implementeren.
1. Blader naar uw web-app in Azure portal en klik op de **instellingen** knop.

    ![Web-Appinstellingen][settings-button]
1. Uit de **instellingen** Selecteer **toepassingsinstellingen** en schuif naar de **Handlertoewijzingen** sectie. Voeg `*.php` veld naar de extensie en voeg het pad naar de `php-cgi.exe` uitvoerbaar bestand. Als u uw PHP-runtime in de `bin` -map in de hoofdmap van uw toepassing, het pad is `D:\home\site\wwwroot\bin\php\php-cgi.exe`.

    ![Handler in Handlertoewijzingen opgeven][handler-mappings]
1. Klik op de **opslaan** knop aan de bovenkant van de **Web-app-instellingen** blade.

    ![Configuratie-instellingen opslaan][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Hoe: Composer-automatisering in Azure inschakelen

Standaard, App Service doet het nog niets met composer.json, als u in uw PHP-project hebt. Als u [Git-implementatie](app-service-deploy-local-git.md), kunt u composer.json verwerken tijdens `git push` doordat de Composer-extensie.

> [!NOTE]
> U kunt [stem voor eersteklas Composer-ondersteuning in App Service hier](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
>

1. In uw PHP web-app blade in de [Azure-portal](https://portal.azure.com), klikt u op **extra** > **extensies**.

    ![Azure portal-instellingen-blade om in te schakelen Composer-automatisering in Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. Klik op **toevoegen**, klikt u vervolgens op **Composer**.

    ![Composer-extensie om in te schakelen Composer-automatisering in Azure toevoegen](./media/web-sites-php-configure/composer-extension-add.png)
3. Klik op **OK** om juridische voorwaarden te accepteren. Klik op **OK** opnieuw toe te voegen van de extensie.

    De **extensions hebt geïnstalleerd** blade ziet u de Composer-extensie.
    ![Accepteer de juridische voorwaarden om in te schakelen Composer-automatisering in Azure](./media/web-sites-php-configure/composer-extension-view.png)
4. Nu in een terminalvenster op uw lokale computer uitvoeren `git add`, `git commit`, en `git push` aan uw Web-App. U ziet dat Composer afhankelijkheden die zijn gedefinieerd in composer.json wordt geïnstalleerd.

    ![GIT-implementatie met Composer-automatisering in Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie de [PHP-ontwikkelaarscentrum](/develop/php/).

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
>

[gratis proefversie]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[Lijst van php.ini-richtlijnen]: http://www.php.net/manual/en/ini.list.php
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
