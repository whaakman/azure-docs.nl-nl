---
title: Een Ruby- en een MySQL-web-app maken in Azure App Service op Linux | Microsoft Docs
description: Informatie over het werkend krijgen van een Ruby-app in Azure, met verbinding naar een MySQL-database in Azure.
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: ruby
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 951e66e47cf8fbe9d2cdf1606a8d63054bcada13
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Een Ruby en MySQL-web-app maken in Azure App Service op Linux

[App Service on Linux](app-service-linux-intro.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie onder het Linux-besturingssysteem. In deze zelfstudie wordt getoond hoe u een Ruby-web-app maakt en deze verbinding laat maken met een MySQL-database. Wanneer u klaar bent, hebt u een [Ruby on Rails](http://rubyonrails.org/)-app die op App Service op Linux wordt uitgevoerd.

![Ruby on Rails-app uitgevoerd in Azure App Service](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een MySQL-database in Azure maken
> * Een Ruby on Rails-app verbinden met MySQL
> * De app implementeren in Azure
> * Het gegevensmodel bijwerken en de app opnieuw implementeren
> * Logboeken met diagnostische gegevens vanaf Azure streamen
> * De app in Azure Portal beheren

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Git installeren](https://git-scm.com/)
* [Ruby 2.3 installeren](https://www.ruby-lang.org/documentation/installation/)
* [Ruby on Rails 5.1 installeren](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [MySQL installeren en starten](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Lokale MySQL voorbereiden

In deze stap maakt u een database in uw lokale MySQL-server voor gebruik in deze zelfstudie.

### <a name="connect-to-local-mysql-server"></a>Verbinding maken met lokale MySQL-server

Maak in een terminalvenster verbinding met uw lokale MySQL-server. U kunt dit terminalvenster gebruiken om alle opdrachten in deze zelfstudie uit te voeren.

```bash
mysql -u root -p
```

Als u wordt gevraagd om een wachtwoord, voert u het wachtwoord in voor het account `root`. Als u het wachtwoord van uw rootaccount niet meer weet, bekijkt u [MySQL: het opnieuw instellen van het hoofdwachtwoord](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Als uw opdracht succesvol is uitgevoerd, wordt uw MySQL-server uitgevoerd. Als dit niet het geval is, zorgt u ervoor dat de lokale MySQL-server is gestart door de [Stappen na installatie van MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) te volgen.

Sluit uw serververbinding door `quit` te typen.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Maak een lokale Ruby on Rails-app
In deze stap krijgt u een Ruby on Rails-voorbeeld-app, configureert u de databaseverbinding en voert u deze lokaal uit. 

### <a name="clone-the-sample"></a>Het voorbeeld klonen

In het terminalvenster, `cd` in een werkmap.

Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd` naar de gekloonde adreslijst. Installeer de vereiste pakketten.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>MySQL-verbinding configureren

Open in de opslagplaats _config/database.yml_ en geef de lokale MySQL rootgebruikersnaam en het wachtwoord (regel 13). Als u MySQL heeft geïnstalleerd met behulp van een hulpprogramma zoals [Homebrew](https://brew.sh/), zijn de referenties in het bestand al ingesteld op de standaardwaarden (dit is `root` en een leeg wachtwoord).

```txt
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock
```

### <a name="run-the-sample-locally"></a>Het voorbeeld lokaal uitvoeren

Voer [de Rails-migraties](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) uit om de tabellen te maken die de toepassing nodig heeft. Als u wilt zien welke tabellen worden gemaakt in de migraties, zoek dan in de map _db/migreren_ in de Git-opslagplaats.

```bash
rake db:create
rake db:migrate
```

Voer de toepassing uit.

```bash
rails server
```

Ga naar `http://localhost:3000` in een browser. Voeg een paar taken op de pagina toe.

![Ruby on Rails is geslaagd om verbinding te maken met MySQL](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Typ `Ctrl + C` in de terminal om de Rails-server te stoppen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>MySQL in Azure maken

In deze stap maakt u een MySQL-database in [Azure Database for MySQL (Preview)](/azure/mysql). Later configureert u de Ruby on Rails-toepassing om verbinding te maken met deze database.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Een MySQL-server maken

Een server in Azure Database for MySQL (Preview) maken met de opdracht [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create).

Vervang in de volgende opdracht de naam van uw MySQL-server waarin u de tijdelijke aanduiding _&lt;mysql_server_name>_ ziet (geldige tekens zijn `a-z`, `0-9`, en `-`). Deze naam maakt deel uit van de hostnaam van de MySQL-server (`<mysql_server_name>.mysql.database.azure.com`), hij moet globaal uniek zijn.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

Wanneer de MySQL-server is gemaakt, toont de Azure CLI informatie die lijkt op de informatie in het volgende voorbeeld:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Een serverfirewall configureren

Maak een firewallregel voor uw MySQL-server om clientverbindingen toe te staan met behulp van de opdracht [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create).

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure Database for MySQL (Preview) beperkt momenteel de verbindingen niet tot alleen Azure-services. Omdat IP-adressen in Azure dynamisch toegewezen worden, is het beter om te zorgen dat alle IP-adressen actief zijn. De service is in preview. Betere methoden voor het beveiligen van uw database zijn gepland.
>

### <a name="connect-to-production-mysql-server-locally"></a>Verbinding maken met lokale MySQL-server in productie

Maak in het terminalvenster verbinding met de MySQL-server in Azure. Gebruik de waarde die u eerder hebt opgegeven voor _&lt;mysql_server_name>_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Gebruik _My5up3r$tr0ngPa$w0rd!_, dat u hebt opgegeven toen u de database-server maakte, wanneer u wordt gevraagd om een wachtwoord.

### <a name="create-a-production-database"></a>Maak een productiedatabase

Op de prompt `mysql` maakt u een database.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Een gebruiker met machtigingen maken

Maak een databasegebruiker met de naam _railsappuser_ en geef deze alle bevoegdheden in de `sampledb`-database.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

Sluit uw serververbinding door `quit` te typen.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>App verbinden met Azure MySQL

In deze stap verbindt u de Ruby on Rails-toepassing met de MySQL-database die u hebt gemaakt in Azure Database for MySQL (Preview).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Verbinding met de database configureren

Open in de opslagplaats _config/database.yml_. Aan de onderkant van het bestand kunt u de productievariabelen vervangen door de volgende code. Gebruik voor de tijdelijke aanduiding _&lt;mysql_server_name>_ de naam van de MySQL-server die u hebt gemaakt.

```txt
production:
  <<: *default
  host: <%= ENV['DB_HOST'] %>
  port: 3306
  database: <%= ENV['DB_DATABASE'] %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  sslca: ssl/BaltimoreCyberTrustRoot.crt.pem
```

Sla de wijzigingen op.

> [!NOTE]
> De `sslca` is toegevoegd en verwijst naar een bestaand _.pem_-bestand in de voorbeeldopslagplaats. Azure Database for MySQL dwingt standaard SSL-verbindingen van clients af. Met dit `.pem`-certificaat maakt u SSL-verbindingen met Azure Database for MySQL. Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../../mysql/howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.
>

### <a name="test-the-application-locally"></a>De toepassing lokaal testen

Stel de volgende omgevingsvariabelen in de lokale terminal in:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

Voer Rails-databasemigraties uit met de productiewaarden die u zojuist hebt geconfigureerd voor het maken van de tabellen in de MySQL-database in Azure Database for MySQL (Preview). 

```bash
rake db:migrate RAILS_ENV=production
```

Wanneer dit wordt uitgevoerd in de productieomgeving, heeft de Rails-toepassing vooraf gecompileerde assets nodig. Genereer de vereiste assets met de volgende opdracht:

```bash
rake assets:precompile
```

De Rails-productieomgeving gebruikt ook geheimen om de beveiliging beheren. Genereer een geheime sleutel.

```bash
rails secret
```

Sla de geheime sleutel op naar de respectieve variabelen die worden gebruikt door de productieomgeving van Rails. Voor uw gemak gebruikt u dezelfde sleutel voor beide variabelen.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

Schakel de productieomgeving van Rails in om JavaScript- en CSS-bestanden te verzenden.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Voer de voorbeeldtoepassing uit in de productieomgeving.

```bash
rails server -e production
```

Navigeer naar `http://localhost:3000`. Als de pagina zonder fouten wordt geladen, maakt de Ruby on Rails-toepassing verbinding met de MySQL-database in Azure.

Voeg een paar taken op de pagina toe.

![Ruby on Rails maakt verbinding met Azure Database for MySQL (Preview)](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Typ `Ctrl + C` in de terminal om de Rails-server te stoppen.

### <a name="commit-your-changes"></a>Uw wijzigingen doorvoeren

Voer de volgende Git-opdrachten uit om uw wijzigingen door te voeren:

```bash
git add .
git commit -m "database.yml updates"
```

Uw app kan worden geïmplementeerd.

## <a name="deploy-to-azure"></a>Implementeren in Azure

In deze stap implementeert u de met MySQL verbonden Rails-toepassing naar Azure App Service.

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Een webtoepassing maken

Maak in Cloud Shell een web-app in het App Service-plan van `myAppServicePlan` met de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). 

Vervang in het volgende voorbeeld `<app_name>` door een unieke naam (geldige tekens zijn `a-z`, `0-9`, en `-`). De runtime is ingesteld op `RUBY|2.3`, die de [standaard Ruby-installatiekopie implementeert](https://hub.docker.com/r/appsvc/ruby/). Voer [`az webapp list-runtimes`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes) uit als u alle ondersteunde runtimes wilt zien. 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "RUBY|2.3" --deployment-local-git
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

U hebt een nieuwe lege web-app gemaakt, met Git-implementatie ingeschakeld.

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de eigenschap `deploymentLocalGitUrl`, met de indeling `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Sla deze URL op, want u hebt deze later nodig.
>

### <a name="configure-database-settings"></a>Database-instellingen configureren

In App Service stelt u omgevingsvariabelen in als _app-instellingen_ met behulp van de opdracht [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) in de Cloud Shell.

De volgende opdracht in de Cloud Shell configureert de app-instellingen `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, en `DB_PASSWORD`. Vervang de tijdelijke aanduidingen _&lt;appname>_ en _&lt;mysql_server_name>_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Rails-omgevingsvariabelen configureren

Genereer in de lokale terminal een nieuwe geheime sleutel voor de productieomgeving van Rails in Azure.

```bash
rails secret
```

Configureer de variabelen die zijn vereist voor de productieomgeving van Rails.

In de volgende opdracht voor Cloud Shell vervangt u de twee tijdelijke aanduidingen _&lt;output_of_rails_secret>_ door de nieuwe geheime sleutel die u hebt gegenereerd in de lokale terminal.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"` geeft de standaard Ruby-container opdracht tot het vooraf compileren van assets bij elke Git-implementatie.

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

Voeg in het lokale terminalvenster een externe Azure-instantie toe aan uw lokale Git-opslagplaats.

```bash
git remote add azure <paste_copied_url_here>
```

Push naar de externe Azure-instantie om de Ruby on Rails-toepassing te implementeren. U wordt gevraagd naar het wachtwoord dat u eerder hebt opgegeven als onderdeel van het maken van de implementatiegebruiker.

```bash
git push azure master
```

Tijdens de implementatie communiceert Azure App Service de voortgang naar Git.

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

### <a name="browse-to-the-azure-web-app"></a>Bladeren naar de Azure-web-app

Blader naar `http://<app_name>.azurewebsites.net` en voeg een paar taken toe aan de lijst.

![Ruby on Rails-app uitgevoerd in Azure App Service](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Gefeliciteerd, u voert een gegevensgestuurde Ruby on Rails-app uit in Azure App Service.

## <a name="update-model-locally-and-redeploy"></a>Het model lokaal bijwerken en opnieuw implementeren

In deze stap maakt u een eenvoudige wijziging in het `task`-gegevensmodel en de web-app en publiceert u de update vervolgens naar Azure.

Voor het takenscenario wijzigt u de toepassing, zodat u een taak als voltooid kunt markeren.

### <a name="add-a-column"></a>Een kolom toevoegen

Ga in de terminal naar de hoofdmap van de Git-opslagplaats.

Genereer een nieuwe migratie die een Booleaanse kolom met de naam `Done` toevoegt aan de tabel `Tasks`:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Met deze opdracht genereert u een nieuw migratiebestand in de map _db/migreren_.


Voer in de terminal Rais-databasemigraties uit om de wijziging in de lokale database door te voeren.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Toepassingslogica bijwerken

Open het bestand *app/controllers/tasks_controller.rb*. Zoek de volgende regel aan het einde van het bestand:

```rb
params.require(:task).permit(:Description)
```

Wijzig deze regel zodanig dat de nieuwe parameter `Done` inbegrepen is.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>De weergaven bijwerken

Open het bestand *app/views/tasks/_form.html.erb*, dit is het formulier Bewerken.

Zoek de regel `<%=f.error_span(:Description) %>` en voeg de volgende code direct eronder in:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Open het bestand *app/views/tasks/show.html.erb*, dit is de pagina met Weergaven van één enkel gegevensrecord. 

Zoek de regel `<dd><%= @task.Description %></dd>` en voeg de volgende code direct eronder in:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Open het bestand *app/views/tasks/index.html.erb*, dit is de Index-pagina voor alle records.

Zoek de regel `<th><%= model_class.human_attribute_name(:Description) %></th>` en voeg de volgende code direct eronder in:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

Zoek in hetzelfde bestand de regel `<td><%= task.Description %></td>` en voeg de volgende code direct eronder in:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>De wijzigingen lokaal testen

Voer in de lokale terminal de Rails-server uit.

```bash
rails server
```

Om de status van de taak te zien wijzigen, gaat u naar `http://localhost:3000` om items toe te voegen of te bewerken.

![Selectievakje toegevoegd aan de taak](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Typ `Ctrl + C` in de terminal om de Rails-server te stoppen.

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren in Azure

Voer in de terminal Rails-databasemigraties voor de productie-omgeving uit, om de wijziging in de Azure-database te maken.

```bash
rake db:migrate RAILS_ENV=production
```

Voer uw wijzigingen door in Git en push de codewijzigingen vervolgens naar Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Zodra `git push` voltooid is, gaat u naar de Azure-web-app en probeert u de nieuwe functionaliteit uit.

![In Azure gepubliceerde model- en databasewijzigingen](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Als u taken heeft toegevoegd, worden deze in de database bewaard. Als u het schema bijwerkt, blijven bestaande gegevens intact.

## <a name="manage-the-azure-web-app"></a>De Azure-web-app beheren

Ga naar [Azure Portal](https://portal.azure.com) om de web-app te beheren die u hebt gemaakt.

Klik in het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/tutorial-php-mysql-app/access-portal.png)

De pagina Overzicht van uw web-app wordt weergegeven. Hier kunt u algemene beheertaken uitvoeren, zoals stoppen, starten, opnieuw starten, bladeren en verwijderen.

Het linkermenu bevat een aantal pagina's voor het configureren van uw app.

![App Service-pagina in Azure Portal](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een MySQL-database in Azure maken
> * Een Ruby on Rails-app verbinden met MySQL
> * De app implementeren in Azure
> * Het gegevensmodel bijwerken en de app opnieuw implementeren
> * Logboeken met diagnostische gegevens vanaf Azure streamen
> * De app in Azure Portal beheren

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan een web-app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
