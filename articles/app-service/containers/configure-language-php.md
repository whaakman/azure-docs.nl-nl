---
title: PHP - Azure App Service-apps configureren | Microsoft Docs
description: Informatie over het configureren van PHP-apps werken in Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: ed6a50ee68d39e6e0d01b405eb02edd6d4c93613
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407592"
---
# <a name="configure-a-linux-php-app-for-azure-app-service"></a>Een Linux-PHP-app configureren voor Azure App Service

Deze handleiding wordt beschreven hoe u de ingebouwde PHP-runtime voor web-apps, mobiele back-ends en API apps in Azure App Service configureert.

Deze handleiding bevat de belangrijkste concepten en instructies voor het PHP-ontwikkelaars die gebruikmaken van een ingebouwde Linux-container in App Service. Als u Azure App Service nog nooit hebt gebruikt, volgt u de [PHP-quickstart](quickstart-php.md) en [PHP met MySQL zelfstudie](tutorial-php-mysql-app.md) eerste.

## <a name="show-php-version"></a>PHP-versie weergeven

Als u wilt weergeven van de huidige PHP-versie, kunt u de volgende opdracht uitvoeren de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Als u wilt weergeven van alle ondersteunde versies van PHP, kunt u de volgende opdracht uitvoeren de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep PHP
```

## <a name="set-php-version"></a>PHP-versie instellen

Voer de volgende opdracht uit de [Cloud Shell](https://shell.azure.com) in te stellen de PHP-versie op 7.2:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "PHP|7.2"
```

## <a name="run-composer"></a>Composer uitvoeren

Standaard Kudu wordt niet uitgevoerd [Composer](https://getcomposer.org/). Om in te schakelen Composer-automatisering tijdens de Kudu-implementatie, moet u opgeven een [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

Vanuit het lokale terminalvenster, wijzig de map naar de hoofdmap van uw opslagplaats. Ga als volgt de [opdrachtregel installatiestappen](https://getcomposer.org/download/) downloaden *composer.phar*.

Voer de volgende opdrachten uit:

```bash
npm install kuduscript -g
kuduscript --php --scriptType bash --suppressPrompt
```

Hoofdmap van uw opslagplaats heeft nu twee nieuwe bestanden in aanvulling op *composer.phar*: *.deployment* en *deploy.sh*. Deze bestanden werkt zowel voor Windows en Linux-versies van het App Service.

Open *deploy.sh* en zoek de `Deployment` sectie. Vervang het hele gedeelte met de volgende code:

```bash
##################################################################################################################################
# Deployment
# ----------

echo PHP deployment

# 1. KuduSync
if [[ "$IN_PLACE_DEPLOYMENT" -ne "1" ]]; then
  "$KUDU_SYNC_CMD" -v 50 -f "$DEPLOYMENT_SOURCE" -t "$DEPLOYMENT_TARGET" -n "$NEXT_MANIFEST_PATH" -p "$PREVIOUS_MANIFEST_PATH" -i ".git;.hg;.deployment;deploy.sh"
  exitWithMessageOnError "Kudu Sync failed"
fi

# 3. Initialize Composer Config
initializeDeploymentConfig

# 4. Use composer
echo "$DEPLOYMENT_TARGET"
if [ -e "$DEPLOYMENT_TARGET/composer.json" ]; then
  echo "Found composer.json"
  pushd "$DEPLOYMENT_TARGET"
  php composer.phar install $COMPOSER_ARGS
  exitWithMessageOnError "Composer install failed"
  popd
fi
##################################################################################################################################
```

Al uw wijzigingen doorvoeren en implementeer uw code opnieuw. Composer moet nu worden uitgevoerd als onderdeel van de implementatie van automation.

## <a name="customize-start-up"></a>Opstarten aanpassen

Standaard worden de ingebouwde PHP-container de Apache-server uitgevoerd. Bij het opstarten, deze wordt uitgevoerd `apache2ctl -D FOREGROUND"`. U kunt een verschillende opdracht als u dat wilt, uitvoeren bij het opstarten, door de volgende opdracht uit te voeren de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

## <a name="access-environment-variables"></a>Toegang tot omgevingsvariabelen

In App Service, kunt u [app-instellingen](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings) buiten de app-code. Vervolgens kunt u ze met behulp van de standaard openen [getenv()](https://secure.php.net/manual/function.getenv.php) patroon. Voor toegang tot bijvoorbeeld de app-instelling `DB_HOST` gebruikt u de volgende code:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>De hoofdmap van site wijzigen

De webframework van uw keuze kan een submap gebruiken als de hoofdmap van de site. Bijvoorbeeld, [Laravel](https://laravel.com/), maakt gebruik van de `public/` submap als de hoofdmap van de site.

De standaardinstallatiekopie van PHP voor App Service maakt gebruik van Apache, en deze kunt u aanpassen voor uw app in de hoofdmap van de site niet. Deze beperking omzeilen, Voeg een *.htaccess* -bestand naar de hoofdmap van uw opslagplaats met de volgende inhoud:

```
<IfModule mod_rewrite.c>
    RewriteEngine on

    RewriteRule ^.*$ /public/$1 [NC,L,QSA]
</IfModule>
```

Als u echter liever niet *.htaccess* herschrijft, kunt u uw Laravel-toepassing in plaats daarvan met een [aangepaste Docker-installatiekopie](quickstart-docker-go.md) implementeren.

## <a name="detect-https-session"></a>HTTPS-sessie detecteren

In App Service vindt [SSL-beëindiging](https://wikipedia.org/wiki/TLS_termination_proxy) plaats in de load balancers voor het netwerk, zodat alle HTTPS-aanvragen uw app bereiken als niet-versleutelde HTTP-aanvragen. Inspecteer de header `X-Forwarded-Proto` als de app-logica moet controleren of de aanvragen van gebruikers al dan niet zijn versleuteld.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Populaire webframeworks bieden toegang tot de `X-Forwarded-*`-informatie in het patroon van de standaard-app. In [CodeIgniter](https://codeigniter.com/) wordt met [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) standaard de waarde van `X_FORWARDED_PROTO` gecontroleerd.

## <a name="customize-phpini-settings"></a>Php.ini-instellingen aanpassen

Als u wijzigingen aanbrengen in uw PHP-installatie moet, kunt u een van de [php.ini richtlijnen](https://www.php.net/manual/ini.list.php) door deze stappen te volgen.

> [!NOTE]
> De beste manier om te zien van de PHP-versie en de huidige *php.ini* configuratie is om aan te roepen [phpinfo()](https://php.net/manual/function.phpinfo.php) in uw app.
>

### <a name="customize-non-phpinisystem-directives"></a>Niet-PHP_INI_SYSTEM richtlijnen aanpassen

Om aan te passen PHP_INI_USER PHP_INI_PERDIR en PHP_INI_ALL richtlijnen (Zie [php.ini richtlijnen](https://www.php.net/manual/ini.list.php)), Voeg een *.htaccess* bestand naar de hoofdmap van uw app.

In de *.htaccess* bestand, voeg de instructies toe met behulp van de `php_value <directive-name> <value>` syntaxis. Bijvoorbeeld:

```
php_value upload_max_filesize 1000M
php_value post_max_size 2000M
php_value memory_limit 3000M
php_value max_execution_time 180
php_value max_input_time 180
php_value display_errors On
php_value upload_max_filesize 10M
```

Implementeer uw app met de wijzigingen opnieuw en start deze opnieuw. Als u deze met Kudu implementeren (bijvoorbeeld met behulp van [Git](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)), het automatisch opnieuw opgestart na de implementatie.

Als alternatief voor het gebruik van *.htaccess*, kunt u [ini_set()](https://www.php.net/manual/function.ini-set.php) in uw app om aan te passen die niet-PHP_INI_SYSTEM-richtlijnen.

### <a name="customize-phpinisystem-directives"></a>Richtlijnen voor PHP_INI_SYSTEM aanpassen

Om aan te passen PHP_INI_SYSTEM richtlijnen (Zie [php.ini richtlijnen](https://www.php.net/manual/ini.list.php)), u kunt geen gebruiken de *.htaccess* benadering. App Service biedt een mechanisme voor afzonderlijke met de `PHP_INI_SCAN_DIR` app-instelling.

Voer eerst de volgende opdracht uit de [Cloud Shell](https://shell.azure.com) om toe te voegen een app-instelling met de naam `PHP_INI_SCAN_DIR`:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="/usr/local/etc/php/conf.d:/home/site/ini"
```

`/usr/local/etc/php/conf.d` is de standaardmap waarin *php.ini* bestaat. `/home/site/ini` de aangepaste map waarin u een aangepaste voegt *.ini* bestand. U dient u de waarden met een `:`.

Navigeer naar de web-SSH-sessie met uw Linux-container (`https://cephalin-container.scm.azurewebsites.net/webssh/host`).

Maak een map in `/home/site` met de naam `ini`, maakt u een *.ini* -bestand in de `/home/site/ini` directory (bijvoorbeeld *settings.ini)* met de richtlijnen die u wilt aanpassen. Gebruik dezelfde syntaxis als u wilt gebruiken in een *php.ini* bestand. 

> [!TIP]
> In de ingebouwde Linux-containers in App Service, */home* wordt gebruikt als permanente gedeelde opslag. 
>

Bijvoorbeeld, wijzig de waarde van [expose_php](https://php.net/manual/ini.core.php#ini.expose-php) Voer de volgende opdrachten uit:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

De wijzigingen van kracht, start de app opnieuw.

## <a name="enable-php-extensions"></a>PHP-uitbreidingen inschakelen

De ingebouwde PHP-installaties bevatten de meest gebruikte uitbreidingen. Kunt u extra extensies op dezelfde manier dat u [php.ini richtlijnen aanpassen](#customize-php_ini_system-directives).

> [!NOTE]
> De beste manier om te zien van de PHP-versie en de huidige *php.ini* configuratie is om aan te roepen [phpinfo()](https://php.net/manual/function.phpinfo.php) in uw app.
>

Extra extensies inschakelen door de volgende stappen:

Toevoegen een `bin` map naar de hoofdmap van uw app en put de `.so` extensiebestanden erin (bijvoorbeeld *mongodb.so*). Zorg ervoor dat de extensies compatibel met de PHP-versie in Azure en worden VC9 en compatibel is met niet-thread-safe (nts zijn).

Implementeer uw wijzigingen.

Volg de stappen in [PHP_INI_SYSTEM aanpassen richtlijnen](#customize-php_ini_system-directives), de extensies toevoegen aan de aangepaste *.ini* bestand met de [extensie](https://www.php.net/manual/ini.core.php#ini.extension) of [zend_extension ](https://www.php.net/manual/ini.core.php#ini.zend-extension) richtlijnen.

```ini
extension=/home/site/wwwroot/bin/mongodb.so
zend_extension=/home/site/wwwroot/bin/xdebug.so
```

De wijzigingen van kracht, start de app opnieuw.

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH-sessie openen in browser

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Probleemoplossing

Wanneer een werkende PHP-app zich anders in App Service gedraagt of fouten heeft, probeert u het volgende:

- [Toegang tot de logboekstream](#access-diagnostic-logs).
- De app lokaal testen in productiemodus. App Service voert uw Node.js-apps in de productiemodus voor, dus u ervoor zorgen moet dat uw project werkt zoals verwacht in productiemodus lokaal. Bijvoorbeeld:
    - Afhankelijk van uw *composer.json*, verschillende pakketten kunnen worden geïnstalleerd voor de productiemodus (`require` versus `require-dev`).
    - Bepaalde web-frameworks kunnen statische bestanden in productiemodus anders implementeren.
    - Bepaalde web-frameworks kunnen aangepaste opstartscripts gebruiken bij het uitvoeren in productiemodus.
- Het uitvoeren van uw app in App Service in de foutopsporingsmodus. Bijvoorbeeld, in [Laravel](https://meanjs.org/), kunt u uw app zo foutopsporingsberichten in productie door [instelling de `APP_DEBUG` app-instelling op `true` ](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="robots933456"></a>robots933456

Mogelijk ziet u het volgende bericht in de logboeken voor containers:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

U kunt dit bericht negeren. `/robots933456.txt` is een dummy URL-pad dat App Service gebruikt om te controleren of de container kan aanvragen. Een 404-antwoord is gewoon geeft aan dat het pad niet bestaat, maar weten laat dat de container in orde is en gereed om te reageren op aanvragen voor App-Service.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: PHP-app met MySQL](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [App Service Linux Veelgestelde vragen](app-service-linux-faq.md)
