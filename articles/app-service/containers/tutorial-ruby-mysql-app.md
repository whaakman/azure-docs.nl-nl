---
title: Een Ruby en MySQL web-app in Azure App Service maken op Linux | Microsoft Docs
description: Informatie over het ophalen van een Ruby-app in Azure, met verbinding met een MySQL-database in Azure AD werkt.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 03b2b4e8f8827a08e1414512d848bd5bed48d674
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Een Ruby en MySQL web-app in Azure App Service maken op Linux

[App Service on Linux](app-service-linux-intro.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie onder het Linux-besturingssysteem. Deze zelfstudie laat zien hoe een Ruby web-app maken en te verbinden met een MySQL-database. Wanneer u klaar bent, hebt u een [Ruby op Rails](http://rubyonrails.org/) app die op App Service op Linux wordt uitgevoerd.

![Ruby op Rails app uitgevoerd in Azure App Service](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een MySQL-database maken in Azure
> * Verbinding maken met een Ruby op Rails app MySQL
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Diagnostische logboeken van de stroom van Azure
> * De app in de Azure portal beheren

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Git installeren](https://git-scm.com/)
* [Ruby 2.3 installeren](https://www.ruby-lang.org/documentation/installation/)
* [Ruby op Rails 5.1 installeren](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [Installeren en starten van MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Lokale MySQL voorbereiden

In deze stap maakt u een database in uw lokale MySQL-server voor gebruik in deze zelfstudie.

### <a name="connect-to-local-mysql-server"></a>Verbinding maken met lokale MySQL-server

In een terminalvenster verbinding maken met uw lokale MySQL-server. Alle opdrachten kunt uitvoeren in deze zelfstudie kunt u deze terminalvenster.

```bash
mysql -u root -p
```

Als u wordt gevraagd om een wachtwoord, voert u het wachtwoord voor de `root` account. Als je het wachtwoord van je root-account, Zie [MySQL: het opnieuw instellen van het hoofdwachtwoord](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Als uw opdracht is uitgevoerd, wordt uw MySQL-server uitgevoerd. Als dit niet het geval is, zorg ervoor dat de lokale MySQL-server is gestart door de [MySQL na de installatie stappen](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

Uw serververbinding sluiten door te typen `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Maak een Ruby op Rails app lokaal
In deze stap een Ruby krijgen voor de voorbeeldtoepassing Rails, de databaseverbinding configureren en het lokaal uitvoeren. 

### <a name="clone-the-sample"></a>Klonen van de steekproef

In het terminalvenster `cd` in een werkmap.

Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd`de gekloonde-adreslijst. Installeer de vereiste pakketten.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>MySQL-verbinding configureren

Open in de opslagplaats _config/database.yml_ en de lokale MySQL root-gebruikersnaam en wachtwoord (regel 13). Als u met behulp van een hulpprogramma zoals MySQL geïnstalleerd [Homebrew](https://brew.sh/), en vervolgens de referenties in het bestand al is ingesteld op de standaardwaarden (dit is `root` en een leeg wachtwoord).

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

Voer [de migraties Rails](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) moeten de toepassing van de tabellen maken. Als u wilt zien welke tabellen worden gemaakt in de migratie, zoeken in de _db/migreren_ map in de Git-opslagplaats.

```bash
rake db:create
rake db:migrate
```

Voer de toepassing uit.

```bash
rails server
```

Ga naar `http://localhost:3000` in een browser. Een paar taken op de pagina toevoegen.

![Ruby op Rails verbonden is met MySQL](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Typ om te stoppen op de server Rails, `Ctrl + C` in de terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>MySQL in Azure maken

In deze stap maakt u een MySQL-database in [Azure Database voor MySQL (Preview)](/azure/mysql). Later, configureert u de Ruby op Rails toepassing verbinding maken met deze database.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Een MySQL-server maken

Een server in Azure-Database voor MySQL (Preview) te maken met de [az mysql-server maken](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) opdracht.

Vervang de naam van uw MySQL-server waarin u ziet in de volgende opdracht de  _&lt;mysql_server_name >_ tijdelijke aanduiding (geldige tekens zijn `a-z`, `0-9`, en `-`). Deze naam maakt deel uit van de MySQL-server de hostnaam (`<mysql_server_name>.mysql.database.azure.com`), moet worden globaal uniek zijn.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

Wanneer de MySQL-server is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld:

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

### <a name="configure-server-firewall"></a>Een firewall configureren

Maken van een firewallregel voor uw MySQL-server clientverbindingen toestaat met behulp van de [az mysql server-firewallregel maken](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) opdracht.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure-Database voor MySQL (Preview) biedt geen momenteel verbindingen alleen voor Azure-services beperken. Zoals IP-adressen in Azure dynamisch toegewezen worden, is het beter om te zorgen dat alle IP-adressen. De service zich in preview. Betere methoden voor het beveiligen van uw database zijn gepland.
>

### <a name="connect-to-production-mysql-server-locally"></a>Verbinding maken met lokaal MySQL-server in productie

In het terminalvenster verbinding maken met de server MySQL in Azure. Gebruik de waarde die u eerder hebt opgegeven voor  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Wanneer u wordt gevraagd om een wachtwoord, gebruiken _My5up3r tr0ngPa$ $w0rd!_, die u hebt opgegeven toen u de database-server gemaakt.

### <a name="create-a-production-database"></a>Een productiedatabase maken

Op de `mysql` vragen, maakt u een database.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Een gebruiker met machtigingen maken

Maken van een databasegebruiker aangeroepen _railsappuser_ en geef deze alle bevoegdheden in de `sampledb` database.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

De verbinding met de sluiten door te typen `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>App verbinden met Azure MySQL

In deze stap maakt u de Ruby op Rails toepassing naar de MySQL-database die u hebt gemaakt in Azure-Database voor MySQL (Preview).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Verbinding met de database configureren

Open in de opslagplaats _config/database.yml_. Aan de onderkant van het bestand, kunt u de productie-variabelen vervangen door de volgende code. Voor de  _&lt;mysql_server_name >_ tijdelijke aanduiding voor de naam van de MySQL-server die u hebt gemaakt.

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
> De `sslca` is toegevoegd en verwijst naar een bestaand _.pem_ bestand in de voorbeeld-opslagplaats. Azure-Database voor MySQL afgedwongen standaard SSL-verbindingen van clients. Dit `.pem` certificaat is hoe u SSL-verbindingen maken met Azure-Database voor MySQL. Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../../mysql/howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.
>

### <a name="test-the-application-locally"></a>De toepassing lokaal testen

Stel de volgende omgevingsvariabelen in de lokale terminal:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

Rails database migraties worden uitgevoerd met de productiewaarden die u zojuist hebt geconfigureerd voor het maken van de tabellen in de MySQL-database in Azure-Database voor MySQL (Preview). 

```bash
rake db:migrate RAILS_ENV=production
```

Wanneer dit wordt uitgevoerd in de productieomgeving, moet de toepassing Rails vooraf gecompileerde activa. Genereren van de vereiste activa met de volgende opdracht:

```bash
rake assets:precompile
```

De productieomgeving Rails gebruikt ook geheimen beveiliging beheren. Een geheime sleutel genereren.

```bash
rails secret
```

Sla de geheime sleutel naar de respectieve variabelen die worden gebruikt door de Rails productie-omgeving. Voor uw gemak gebruikt u dezelfde sleutel voor beide variabelen.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

Schakel in de productieomgeving Rails JavaScript en CSS-bestanden moeten worden verzonden.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Voer de voorbeeldtoepassing in de productieomgeving.

```bash
rails server -e production
```

Navigeer naar `http://localhost:3000`. Als de pagina wordt geladen zonder fouten, wordt de Ruby op Rails toepassing maakt verbinding met de MySQL-database in Azure.

Een paar taken op de pagina toevoegen.

![Ruby op Rails wel verbinding kunt maken met Azure-Database voor MySQL (Preview)](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Typ om te stoppen op de server Rails, `Ctrl + C` in de terminal.

### <a name="commit-your-changes"></a>Uw wijzigingen

Voer de volgende Git-opdrachten uw wijzigingen:

```bash
git add .
git commit -m "database.yml updates"
```

Uw app is gereed om te worden geïmplementeerd.

## <a name="deploy-to-azure"></a>Implementeren in Azure

In deze stap maakt implementeren u de toepassing Rails MySQL verbonden in Azure App Service.

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Een webtoepassing maken

Maak in de Cloud Shell een web-app in het `myAppServicePlan` App Service-plan met de opdracht [az webapp](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). 

Vervang in het volgende voorbeeld `<app_name>` door een unieke naam (geldige tekens zijn `a-z`, `0-9`, en `-`). De runtime is ingesteld op `RUBY|2.3`, waarbij geïmplementeerd de [Ruby installatiekopie standaard](https://hub.docker.com/r/appsvc/ruby/). Voer [az webapp list-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes) uit om alle ondersteunde runtimes te bekijken. 

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

In App Service, stelt u omgevingsvariabelen als _appinstellingen_ met behulp van de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) opdracht in de Cloud-Shell.

De volgende opdracht in de Cloud Shell configureert u de app-instellingen `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, en `DB_PASSWORD`. Vervang de tijdelijke aanduidingen  _&lt;appname >_ en  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Omgevingsvariabelen Rails configureren

In de lokale terminal in een nieuwe geheime sleutel genereren voor de productieomgeving Rails in Azure.

```bash
rails secret
```

De variabelen die zijn vereist voor de productieomgeving Rails configureren.

In de volgende opdracht voor Cloud-Shell, vervangt u de twee  _&lt;output_of_rails_secret >_ tijdelijke aanduidingen door de nieuwe geheime sleutel die u hebt gegenereerd in de lokale terminal.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"`Hiermee geeft u de standaard Ruby container voor het voorcompileren van activa op elke Git-implementatie.

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

Toevoegen in de lokale terminal in een Azure afstand naar uw lokale Git-opslagplaats.

```bash
git remote add azure <paste_copied_url_here>
```

Push naar de Azure-externe voor het implementeren van de Ruby op Rails toepassing. U wordt gevraagd om het wachtwoord dat u eerder hebt opgegeven als onderdeel van het maken van de gebruiker voor de implementatie.

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

### <a name="browse-to-the-azure-web-app"></a>Blader naar de Azure-web-app

Blader naar `http://<app_name>.azurewebsites.net` en een paar taken toevoegen aan de lijst.

![Ruby op Rails app uitgevoerd in Azure App Service](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Gefeliciteerd, u een Ruby gegevensgestuurde op Rails-app in Azure App Service uitvoert.

## <a name="update-model-locally-and-redeploy"></a>Lokaal model bijwerken en implementeren

In deze stap maakt u een eenvoudige wijziging in de `task` gegevensmodel en de Web-App en de update vervolgens publiceren naar Azure.

Voor het scenario taken wijzigen u de toepassing, zodat u kunt een taak als voltooid markeren.

### <a name="add-a-column"></a>Een kolom toevoegen

Ga in de terminal naar de hoofdmap van de Git-opslagplaats.

Genereren van een nieuwe migratie die wordt toegevoegd een Booleaanse kolom `Done` naar de `Tasks` tabel:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Met deze opdracht genereert u een nieuwe migratiebestand in de _db/migreren_ directory.


Voer in de terminal Rails database migraties om de wijziging in de lokale database.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Logica van de toepassing bijwerken

Open de *app/controllers/tasks_controller.rb* bestand. Aan het einde van het bestand, vinden de volgende regel:

```rb
params.require(:task).permit(:Description)
```

Wijzigen van deze regel zodanig dat de nieuwe `Done` parameter.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>De weergaven bijwerken

Open de *app/views/tasks/_form.html.erb* bestand, dat het formulier bewerken.

Zoek de regel `<%=f.error_span(:Description) %>` en voeg de volgende code direct onder:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Open de *app/views/tasks/show.html.erb* bestand, dat de pagina met één record weergeven. 

Zoek de regel `<dd><%= @task.Description %></dd>` en voeg de volgende code direct onder:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Open de *app/views/tasks/index.html.erb* bestand, dat de indexpagina voor alle records.

Zoek de regel `<th><%= model_class.human_attribute_name(:Description) %></th>` en voeg de volgende code direct onder:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

In hetzelfde bestand, zoek de regel `<td><%= task.Description %></td>` en voeg de volgende code direct onder:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>De wijzigingen lokaal testen

In de lokale terminal in de Rails-server wordt uitgevoerd.

```bash
rails server
```

Om te zien van de status van de taak wijzigen, gaat u naar `http://localhost:3000` en items toevoegen of bewerken.

![Toegevoegde selectievakje in als de taak](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Typ om te stoppen op de server Rails, `Ctrl + C` in de terminal.

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren naar Azure

Voer in de terminal Rails database migraties voor de productie-omgeving te maken van de wijziging in de Azure-database.

```bash
rake db:migrate RAILS_ENV=production
```

Voer de wijzigingen in Git en vervolgens de codewijzigingen pushen naar Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Eenmaal de `git push` is voltooid, gaat u naar de Azure-web-app en testen van de nieuwe functionaliteit.

![Model en de database-wijzigingen die zijn gepubliceerd naar Azure](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Als u alle taken toegevoegd, worden ze in de database bewaard. Updates voor het schema van de laten bestaande gegevens intact.

## <a name="manage-the-azure-web-app"></a>De Azure-web-app beheren

Ga naar [Azure Portal](https://portal.azure.com) om de web-app te beheren die u hebt gemaakt.

Klik in het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/tutorial-php-mysql-app/access-portal.png)

De pagina Overzicht van uw web-app wordt weergegeven. Hier kunt u eenvoudige beheertaken zoals stoppen, starten, opnieuw opstarten, bladeren en verwijderen uitvoeren.

Het menu links biedt pagina's voor het configureren van uw app.

![App Service-pagina in Azure Portal](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een MySQL-database maken in Azure
> * Verbinding maken met een Ruby op Rails app MySQL
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Diagnostische logboeken van de stroom van Azure
> * De app in de Azure portal beheren

Ga naar de volgende zelfstudie voor informatie over het toewijzen van een aangepaste DNS-naam aan een web-app.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
