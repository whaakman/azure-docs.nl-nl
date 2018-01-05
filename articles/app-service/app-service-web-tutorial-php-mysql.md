---
title: Een PHP- en MySQL web-app in Azure bouwen | Microsoft Docs
description: Informatie over het ophalen van een PHP-app in Azure, met verbinding met een MySQL-database in Azure AD werkt.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 10/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: bcbe59d5e2f085f055b99b715bcbcd91d9845f2d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Een PHP- en MySQL web-app in Azure bouwen

> [!NOTE]
> In dit artikel implementeert een app in App Service in Windows. Voor de implementatie in App Service _Linux_, Zie [bouwen van een PHP- en MySQL web-app in Azure App Service op Linux](./containers/tutorial-php-mysql-app.md).
>

[Azure Web Apps](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze zelfstudie laat zien hoe een PHP-web-app in Azure maken en te verbinden met een MySQL-database. Wanneer u klaar bent, hebt u een [Laravel](https://laravel.com/) app uitgevoerd op Azure App Service Web Apps.

![PHP-app uitgevoerd in Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een MySQL-database maken in Azure
> * Een PHP-app verbinden met MySQL
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Diagnostische logboeken van de stroom van Azure
> * De app in de Azure portal beheren

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Git installeren](https://git-scm.com/)
* [Installeren van PHP 5.6.4 of hoger](http://php.net/downloads.php)
* [Composer installeren](https://getcomposer.org/doc/00-intro.md)
* De volgende Laravel behoeften van de PHP-uitbreidingen inschakelen: OpenSSL, PDO MySQL, Mbstring, Tokenizer, XML
* [Installeren en starten van MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Lokale MySQL voorbereiden

In deze stap maakt u een database in uw lokale MySQL-server voor gebruik in deze zelfstudie.

### <a name="connect-to-local-mysql-server"></a>Verbinding maken met lokale MySQL-server

In een terminalvenster verbinding maken met uw lokale MySQL-server. Alle opdrachten kunt uitvoeren in deze zelfstudie kunt u deze terminalvenster.

```bash
mysql -u root -p
```

Als u wordt gevraagd om een wachtwoord, voert u het wachtwoord voor de `root` account. Als je het wachtwoord van je root-account, Zie [MySQL: het opnieuw instellen van het hoofdwachtwoord](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Als uw opdracht is uitgevoerd, wordt uw MySQL-server uitgevoerd. Als dit niet het geval is, zorg ervoor dat de lokale MySQL-server is gestart door de [MySQL na de installatie stappen](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database-locally"></a>Lokaal een database maken

Op de `mysql` vragen, maakt u een database.

```sql 
CREATE DATABASE sampledb;
```

Uw serververbinding sluiten door te typen `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Een PHP-app lokaal maken
In deze stap een voorbeeldtoepassing Laravel ophalen, de databaseverbinding configureren en het lokaal uitvoeren. 

### <a name="clone-the-sample"></a>Klonen van de steekproef

In het terminalvenster `cd` in een werkmap.

Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd`de gekloonde-adreslijst.
Installeer de vereiste pakketten.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>MySQL-verbinding configureren

Maak een tekstbestand met de naam in de hoofdmap van de opslagplaats *.env*. Kopieer de volgende variabelen in de *.env* bestand. Vervang de  _&lt;root_password >_ aanduiding voor items met de MySQL-hoofdgebruiker wachtwoord.

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

Voor informatie over hoe Laravel gebruikt de _.env_ bestand, Zie [Laravel omgeving configuratie](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-the-sample-locally"></a>Het voorbeeld lokaal uitvoeren

Voer [Laravel database migraties](https://laravel.com/docs/5.4/migrations) moeten de toepassing van de tabellen maken. Als u wilt zien welke tabellen worden gemaakt in de migratie, zoeken in de _database/migraties_ map in de Git-opslagplaats.

```bash
php artisan migrate
```

Genereer een nieuwe sleutel van de Laravel-toepassing.

```bash
php artisan key:generate
```

Voer de toepassing uit.

```bash
php artisan serve
```

Ga naar `http://localhost:8000` in een browser. Een paar taken op de pagina toevoegen.

![PHP verbonden is met MySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Als u wilt stoppen van de PHP-server, typ `Ctrl + C` in de terminal.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>MySQL in Azure maken

In deze stap maakt u een MySQL-database in [Azure Database voor MySQL (Preview)](/azure/mysql). Later kunt configureren u de PHP-toepassing verbinding maken met deze database.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Een MySQL-server maken

In de Cloud-Shell, maakt u een server in Azure-Database voor MySQL (Preview) met de [az mysql-server maken](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) opdracht.

Vervang de naam van uw MySQL-server waarin u ziet in de volgende opdracht de  _&lt;mysql_server_name >_ tijdelijke aanduiding (geldige tekens zijn `a-z`, `0-9`, en `-`). Deze naam maakt deel uit van de MySQL-server de hostnaam (`<mysql_server_name>.database.windows.net`), moet worden globaal uniek zijn.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

> [!NOTE]
> Aangezien er verschillende referenties aan te denken dat in deze zelfstudie zijn om verwarring te voorkomen dat `--admin-user` en `--admin-password` zijn ingesteld op dummy-waarden. Volg in een productieomgeving aanbevolen beveiligingsprocedures wanneer u ervoor kiest een juiste gebruikersnaam en wachtwoord voor uw server MySQL in Azure.
>
>

Wanneer de MySQL-server is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Een firewall configureren

In de Cloud-Shell, maakt u een firewallregel voor uw MySQL-server clientverbindingen toestaat met behulp van de [az mysql server-firewallregel maken](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) opdracht.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure-Database voor MySQL (Preview) biedt geen momenteel verbindingen alleen voor Azure-services beperken. Zoals IP-adressen in Azure dynamisch toegewezen worden, is het beter om te zorgen dat alle IP-adressen. De service zich in preview. Betere methoden voor het beveiligen van uw database zijn gepland.
>
>

### <a name="connect-to-production-mysql-server-locally"></a>Verbinding maken met lokaal MySQL-server in productie

In het lokale terminalvenster verbinding maken met de server MySQL in Azure. Gebruik de waarde die u eerder hebt opgegeven voor  _&lt;mysql_server_name >_. Wanneer u wordt gevraagd om een wachtwoord, gebruiken _My5up3r tr0ngPa$ $w0rd!_, die u hebt opgegeven toen u de database hebt gemaakt in Azure.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

### <a name="create-a-production-database"></a>Een productiedatabase maken

Op de `mysql` vragen, maakt u een database.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Een gebruiker met machtigingen maken

Maken van een databasegebruiker aangeroepen _phpappuser_ en geef deze alle bevoegdheden in de `sampledb` database. Nogmaals, ter vereenvoudiging van de zelfstudie gebruikt _MySQLAzure2017_ als het wachtwoord.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

De verbinding met de sluiten door te typen `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>App verbinden met Azure MySQL

In deze stap maakt u verbinding maakt het PHP-toepassing aan de MySQL-database die u hebt gemaakt in Azure-Database voor MySQL (Preview).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Verbinding met de database configureren

Maak in de hoofdmap van de opslagplaats een _. env.production_ -bestand en kopieer de volgende variabelen in de App. Vervang de tijdelijke aanduiding  _&lt;mysql_server_name >_ in beide *DB_HOST* en *DB_USERNAME*.

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

Sla de wijzigingen op.

> [!TIP]
> Dit bestand is voor het beveiligen van uw MySQL-verbindingsgegevens al uitgesloten van de Git-opslagplaats (Zie _.gitignore_ in de hoofdmap van de opslagplaats). Later kunt u informatie over het configureren van omgevingsvariabelen in App Service verbinding maken met uw database in Azure-Database voor MySQL (Preview). Met omgevingsvariabelen, hoeft u niet de *.env* bestand in App Service.
>

### <a name="configure-ssl-certificate"></a>SSL-certificaat configureren

Azure-Database voor MySQL afgedwongen standaard SSL-verbindingen van clients. Voor verbinding met uw MySQL-database in Azure, moet u een _.pem_ SSL-certificaat.

Open _config/database.php_ en voeg de _sslmode_ en _opties_ parameters `connections.mysql`, zoals wordt weergegeven in de volgende code.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

Voor informatie over het genereren van dit _certificate.pem_, Zie [configureren van SSL-verbindingen in uw toepassing veilig verbinding kunnen maken met Azure-Database voor MySQL](../mysql/howto-configure-ssl.md).

> [!TIP]
> Het pad _/ssl/certificate.pem_ verwijst naar een bestaand _certificate.pem_ bestand in de Git-opslagplaats. Dit bestand is voor uw gemak geleverd in deze zelfstudie. Voor de beste praktijken, u moet niet worden doorgevoerd uw _.pem_ certificaten in broncodebeheer. 
>

### <a name="test-the-application-locally"></a>De toepassing lokaal testen

Voer Laravel database migraties met _. env.production_ als het bestand omgeving maken van de tabellen in uw MySQL-database in Azure-Database voor MySQL (Preview). Vergeet niet dat _. env.production_ heeft de verbindingsgegevens wilt toevoegen aan uw MySQL-database in Azure.

```bash
php artisan migrate --env=production --force
```

_. env.production_ nog een geldige App-sleutel niet is voorzien. Een nieuw wachtwoord voor het genereren in de terminal.

```bash
php artisan key:generate --env=production --force
```

Uitvoeren van de voorbeeldtoepassing met _. env.production_ als het bestand omgeving.

```bash
php artisan serve --env=production
```

Navigeer naar `http://localhost:8000`. Als de pagina wordt geladen zonder fouten, wordt de PHP-toepassing maakt verbinding met de MySQL-database in Azure.

Een paar taken op de pagina toevoegen.

![PHP wel verbinding kunt maken met Azure-Database voor MySQL (Preview)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Typ om te stoppen PHP, `Ctrl + C` in de terminal.

### <a name="commit-your-changes"></a>Uw wijzigingen

Voer de volgende Git-opdrachten uw wijzigingen:

```bash
git add .
git commit -m "database.php updates"
```

Uw app is gereed om te worden geïmplementeerd.

## <a name="deploy-to-azure"></a>Implementeren in Azure

In deze stap maakt implementeren u de MySQL verbonden PHP-toepassing in Azure App Service.

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

<a name="create"></a>
### <a name="create-a-web-app"></a>Een webtoepassing maken

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-php-no-h.md)] 

### <a name="configure-database-settings"></a>Database-instellingen configureren

Zoals eerder uiteengezet, kunt u verbinding met uw Azure-MySQL-database met omgevingsvariabelen in App Service.

In de Cloud-Shell, stelt u omgevingsvariabelen als _appinstellingen_ met behulp van de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) opdracht.

De volgende opdracht uit de app-instellingen configureert `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, en `DB_PASSWORD`. Vervang de tijdelijke aanduidingen  _&lt;appname >_ en  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

U kunt de PHP [getenv](http://www.php.net/manual/function.getenv.php) methode voor toegang tot de instellingen. de code Laravel gebruikt een [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper boven de PHP `getenv`. Bijvoorbeeld, de MySQL-configuratie in _config/database.php_ vergelijkbaar is met de volgende code:

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a>Omgevingsvariabelen Laravel configureren

Laravel moet de sleutel van een toepassing in App Service. U kunt deze configureren met app-instellingen.

Gebruik in het lokale terminalvenster `php artisan` voor het genereren van een nieuwe Toepassingssleutel zonder op te slaan naar _.env_.

```bash
php artisan key:generate --show
```

In de Cloud-Shell de Toepassingssleutel instellen in de App Service-web-app met behulp van de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) opdracht. Vervang de tijdelijke aanduidingen  _&lt;appname >_ en  _&lt;outputofphpartisankey: genereren >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`Hiermee geeft u Laravel foutopsporing om informatie te retourneren wanneer de geïmplementeerde web-app fouten optreden. Als een productietoepassing wordt uitgevoerd, stelt u deze naar `false`, die is veiliger.

### <a name="set-the-virtual-application-path"></a>Het pad van de virtuele toepassing instellen

Het pad van de virtuele toepassing voor de web-app ingesteld. Deze stap is vereist omdat de [Laravel toepassing lifecycle](https://laravel.com/docs/5.4/lifecycle) begint in de _openbare_ map in plaats van de hoofdmap van de toepassing. Andere PHP-frameworks waarvan de levenscyclus van starten in de hoofdmap werken zonder handmatige configuratie van het pad van de virtuele toepassing.

Stel in de Cloud-Shell pad van de virtuele toepassing met behulp van de [az resource update](/cli/azure/resource#update) opdracht. Vervang de  _&lt;appname >_ tijdelijke aanduiding.

```azurecli-interactive
az resource update --name web --resource-group myResourceGroup --namespace Microsoft.Web --resource-type config --parent sites/<app_name> --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" --api-version 2015-06-01
```

Azure App Service wijst standaard het hoofdpad voor de virtuele toepassing (_/_) naar de hoofdmap van de geïmplementeerde toepassing-bestanden (_sites\wwwroot_).

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

Voeg in het lokale terminalvenster een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang  _&lt;plakken\_gekopieerd\_url\_hier >_ met de URL van de externe Git die u hebt genoteerd in [maken van een web-app](#create).

```bash
git remote add azure <paste_copied_url_here>
```

Push naar de Azure-externe voor het implementeren van de PHP-toepassing. U wordt gevraagd om het wachtwoord dat u eerder hebt opgegeven als onderdeel van het maken van de gebruiker voor de implementatie.

```bash
git push azure master
```

Azure App Service communiceert tijdens de implementatie van de voortgang met Git.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

> [!NOTE]
> U merkt dat het implementatieproces installeert [Composer](https://getcomposer.org/) pakketten aan het einde. App Service kan niet worden uitgevoerd deze automatische tijdens de standaardimplementatie van de, zodat deze opslagplaats voorbeeld heeft drie extra bestanden in de hoofdmap in te schakelen:
>
> - `.deployment`-Dit bestand vertelt App Service om uit te voeren `bash deploy.sh` als het script voor aangepaste implementatie.
> - `deploy.sh`-Het script voor aangepaste implementatie. Als u het bestand bekijkt, ziet u dat deze wordt uitgevoerd `php composer.phar install` nadat `npm install`.
> - `composer.phar`-De Composer-Pakketbeheer.
>
> Deze aanpak kunt u een stap toevoegen aan uw implementatie op basis van Git in App Service. Zie voor meer informatie [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-to-the-azure-web-app"></a>Blader naar de Azure-web-app

Blader naar `http://<app_name>.azurewebsites.net` en een paar taken toevoegen aan de lijst.

![PHP-app uitgevoerd in Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

Gefeliciteerd, u een PHP-gegevensgestuurde app in Azure App Service uitvoert.

## <a name="update-model-locally-and-redeploy"></a>Lokaal model bijwerken en implementeren

In deze stap maakt u een eenvoudige wijziging in de `task` gegevensmodel en de Web-App en de update vervolgens publiceren naar Azure.

Voor het scenario taken wijzigen u de toepassing, zodat u kunt een taak als voltooid markeren.

### <a name="add-a-column"></a>Een kolom toevoegen

Navigeer naar de hoofdmap van de Git-opslagplaats in het lokale terminalvenster.

Genereren van de databasemigratie van een nieuwe voor de `tasks` tabel:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Deze opdracht geeft u de naam van de migratiebestand dat wordt gegenereerd. Dit bestand in _database/migraties_ en open het bestand.

Vervang de `up` methode met de volgende code:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

De bovenstaande code wordt toegevoegd een Booleaanse kolom in de `tasks` tabel met de naam `complete`.

Vervang de `down` methode met de volgende code voor de actie ongedaan maken:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

Voer in het lokale terminalvenster Laravel database migraties om de wijziging in de lokale database.

```bash
php artisan migrate
```

Op basis van de [Laravel naamgevingsconventie](https://laravel.com/docs/5.4/eloquent#defining-models), het model `Task` (Zie _app/Task.php_) wordt toegewezen aan de `tasks` tabel standaard.

### <a name="update-application-logic"></a>Logica van de toepassing bijwerken

Open de *routes/web.php* bestand. De toepassing definieert de routes en zakelijke logica hier.

Toevoegen aan het einde van het bestand een route met de volgende code:

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

De bovenstaande code maakt een eenvoudige update naar het gegevensmodel door het uitschakelen van de waarde van `complete`.

### <a name="update-the-view"></a>De weergave bijwerken

Open de *resources/views/tasks.blade.php* bestand. Zoeken naar de `<tr>` tag te openen en vervang ze door:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

De bovenstaande code wordt de rijkleur, afhankelijk van of de taak voltooid is.

U hebt de volgende code in de volgende regel:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Vervang de volledige regel door de volgende code:

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

De bovenstaande code wordt toegevoegd voor de verzendknop die verwijst naar de route die u eerder hebt gedefinieerd.

### <a name="test-the-changes-locally"></a>De wijzigingen lokaal testen

In het lokale terminalvenster uitvoering van de ontwikkelingsserver van de hoofdmap van de Git-opslagplaats.

```bash
php artisan serve
```

Om te zien van de status van de taak wijzigen, gaat u naar `http://localhost:8000` en schakel het selectievakje.

![Toegevoegde selectievakje in als de taak](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

Typ om te stoppen PHP, `Ctrl + C` in de terminal.

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren naar Azure

Voer in het lokale terminalvenster Laravel database migraties met de productie-verbindingsreeks te maken van de wijziging in de Azure-database.

```bash
php artisan migrate --env=production --force
```

Voer de wijzigingen in Git en vervolgens de codewijzigingen pushen naar Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Eenmaal de `git push` is voltooid, gaat u naar de Azure-web-app en testen van de nieuwe functionaliteit.

![Model en de database-wijzigingen die zijn gepubliceerd naar Azure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Als u alle taken toegevoegd, worden ze in de database bewaard. Updates voor het schema van de laten bestaande gegevens intact.

## <a name="stream-diagnostic-logs"></a>Diagnostische logboeken

Als de PHP-toepassing in Azure App Service wordt uitgevoerd, kunt u de logboeken van de console doorgesluisd naar uw terminal opvragen. Op die manier kunt u de dezelfde diagnostische berichten op te sporen toepassingsfouten ophalen.

Gebruik voor het starten van de streaming-logboek de [az webapp logboek tail](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) opdracht in de Cloud-Shell.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
```

Eenmaal streaming-logboek is gestart, vernieuwt u de Azure-web-app in de browser sommige webverkeer ophalen. U ziet nu de logboeken van de console is doorgegeven naar de terminal. Als u de logboeken van console onmiddellijk niet ziet, controleert u opnieuw in 30 seconden.

Typ om te stoppen met het logboek op elk gewenst moment op streaming, `Ctrl` + `C`.

> [!TIP]
> Een PHP-toepassing kunt gebruiken de standaard [error_log()](http://php.net/manual/function.error-log.php) naar uitvoer naar de console. De voorbeeldtoepassing gebruikt deze benadering in _app/Http/routes.php_.
>
> Als een kader web [Laravel gebruikt Monolog](https://laravel.com/docs/5.4/errors) als de provider voor logboekregistratie. Zie voor het ophalen van Monolog uitvoer berichten naar de console [PHP: het gebruik van monolog aan te melden console (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).
>
>

## <a name="manage-the-azure-web-app"></a>De Azure-web-app beheren

Ga naar [Azure Portal](https://portal.azure.com) om de web-app te beheren die u hebt gemaakt.

Klik in het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-tutorial-php-mysql/access-portal.png)

De pagina Overzicht van uw web-app wordt weergegeven. Hier kunt u eenvoudige beheertaken zoals stoppen, starten, opnieuw opstarten, bladeren en verwijderen uitvoeren.

Het menu links biedt pagina's voor het configureren van uw app.

![App Service-pagina in Azure Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een MySQL-database maken in Azure
> * Een PHP-app verbinden met MySQL
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Diagnostische logboeken van de stroom van Azure
> * De app in de Azure portal beheren

Ga naar de volgende zelfstudie voor informatie over het toewijzen van een aangepaste DNS-naam aan een web-app.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md)
