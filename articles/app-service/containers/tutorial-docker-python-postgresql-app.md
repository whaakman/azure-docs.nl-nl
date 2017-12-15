---
title: Een Docker Python en PostgreSQL web-app in Azure bouwen | Microsoft Docs
description: Informatie over het ophalen van een Docker-Python-app in Azure, werkt met verbinding met een PostgreSQL-database.
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 11/29/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: 0bd4f390e4507fccd1ca564c48c0f321412e229d
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Een Docker Python en PostgreSQL web-app in Azure bouwen

Web-App voor Containers biedt een zeer schaalbaar, zelf patch webhosting-service. Deze zelfstudie laat zien hoe een eenvoudige Docker Python-web-app maken in Azure. U kunt deze app verbinding met een PostgreSQL-database. Wanneer u klaar bent, hebt u een Python Flask-toepassing uitgevoerd binnen een Docker-container op [op Linux-App Service](app-service-linux-intro.md).

![Docker Python Flask-app in App Service op Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

U kunt de onderstaande stappen volgen op Mac OS. Linux- en Windows-instructies zijn hetzelfde in de meeste gevallen, maar de verschillen worden niet beschreven in deze zelfstudie.
 
## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

1. [Git installeren](https://git-scm.com/)
1. [Python installeren](https://www.python.org/downloads/)
1. [Installeren en uitvoeren van PostgreSQL](https://www.postgresql.org/download/)
1. [Docker-Community Edition installeren](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Lokale PostgreSQL-installatie testen en een database maken

Open het terminalvenster en voer `psql postgres` verbinding maken met uw lokale PostgreSQL-server.

```bash
psql postgres
```

Als de verbinding geslaagd is, wordt uw PostgreSQL-database wordt uitgevoerd. Als dit niet het geval is, zorg ervoor dat de lokale PostgresQL-database is gestart door de stappen op [Downloads - PostgreSQL Core distributie](https://www.postgresql.org/download/).

Maken van een database met de naam *eventregistration* en instellen van de gebruiker van een aparte database met de naam *manager* met wachtwoord *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Type *\q* om af te sluiten van de PostgreSQL-client. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Lokale Python Flask-toepassing maken

In deze stap moet u het lokale Python Flask-project instellen.

### <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Open een terminalvenster en `CD` in een werkmap.

Voer de volgende opdrachten de voorbeeld-opslagplaats klonen en gaat u naar de *0,1 initialapp* release.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Deze repository voorbeeld bevat een [Flask](http://flask.pocoo.org/) toepassing. 

### <a name="run-the-application"></a>De toepassing uitvoeren

> [!NOTE] 
> In een latere stap kunt u dit proces met het bouwen van een Docker-container voor gebruik met de productiedatabase vereenvoudigen.

Installeer de vereiste pakketten en start de toepassing.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Wanneer de app volledig wordt geladen is, ziet u iets soortgelijks als in het volgende bericht:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Ga naar `http://127.0.0.1:5000` in een browser. Klik op **registreren!** en een testgebruiker maken.

![Python Flask-toepassing die lokaal wordt uitgevoerd](./media/tutorial-docker-python-postgresql-app/local-app.png)

De voorbeeldtoepassing Flask slaat gebruikersgegevens in de database. Als u succesvol bij het registreren van een gebruiker, wordt uw app schrijven van gegevens naar de lokale PostgreSQL-database.

Als u wilt de Flask-server op elk gewenst moment stoppen, typt u Ctrl + C in de terminal. 

## <a name="create-a-production-postgresql-database"></a>Een productie-PostgreSQL-database maken

In deze stap maakt u een PostgreSQL-database in Azure. Wanneer uw app wordt geïmplementeerd naar Azure, wordt deze cloud-database gebruikt.

### <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Nu gaat u de Azure CLI 2.0 gebruiken om de resources die nodig zijn voor het hosten van uw toepassing Python in Web-App voor Containers te maken.  Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/?view=azure-cli-latest#az_login) en volg de instructies op het scherm.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md) met de opdracht [az group create](/cli/azure/group?view=azure-cli-latest#az_group_create).

[!INCLUDE [Resource group intro](../../../includes/resource-group.md)]

Het volgende voorbeeld maakt een resourcegroep in de regio VS-West:

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

Gebruik de [az appservice lijst-locaties](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) Azure CLI-opdracht naar de lijst met beschikbare locaties.

### <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken

Maak een PostgreSQL-server met de [az postgres server maken](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create) opdracht.

In de volgende opdracht te vervangen door een unieke naam voor de  *\<postgresql_name >* tijdelijke aanduiding en een gebruiker een naam voor de  *\<admin_username >* tijdelijke aanduiding. Naam van de server wordt gebruikt als onderdeel van uw eindpunt PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), zodat de naam moet uniek zijn in alle servers in Azure. De gebruikersnaam is voor de gebruikersaccount van de oorspronkelijke database-beheerder. U wordt gevraagd een wachtwoord voor deze gebruiker kiest.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

Wanneer de Azure-Database voor PostgreSQL-server is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>Een firewallregel maken voor de Azure-Database voor PostgreSQL-server

Voer de volgende opdracht in de Azure CLI om toegang te verlenen tot de database van alle IP-adressen.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

De Azure CLI bevestigt de firewall-regel maken met de uitvoer lijkt op het volgende voorbeeld:

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-to-the-database"></a>Verbinding maken met uw Python Flask-toepassing met de database

In deze stap maakt u verbinding maken uw Python Flask-voorbeeldtoepassing voor de Azure-Database voor PostgreSQL-server die u hebt gemaakt.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Een lege database maken en een nieuwe gebruiker van de database-toepassing instellen

Maak een databasegebruiker met toegang tot één database. U gebruikt deze referenties om te voorkomen dat de toepassing volledige toegang geven tot de server.

Verbinding maken met de database (u wordt gevraagd om uw wachtwoord admin).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

De database en de gebruiker van de CLI PostgreSQL maken.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Type *\q* om af te sluiten van de PostgreSQL-client.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Test de toepassing lokaal op de Azure-PostgreSQL-database

Ga terug nu naar de *app* map van de gekloonde Github-opslagplaats, kunt u de Python Flask-toepassing uitvoeren door de omgevingsvariabelen database bij te werken.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Wanneer de app volledig wordt geladen is, ziet u iets soortgelijks als in het volgende bericht:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Ga naar http://127.0.0.1:5000 in een browser. Klik op **registreren!** en maak de registratie van een test. U schrijft gegevens nu met de database in Azure.

![Python Flask-toepassing die lokaal wordt uitgevoerd](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>De toepassing wordt uitgevoerd vanuit een Docker-Container

De Docker een installatiekopie van de container maken.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker geeft een bevestiging dat deze de container is gemaakt.

```bash
Successfully built 7548f983a36b
```

Omgevingsvariabelen database toevoegen aan een variabele bestand omgeving *db.env*. De app maakt verbinding met de Azure-Database voor productie-PostgreSQL-database.

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

Voer de app uit in de Docker-container. De volgende opdracht geeft de variabele omgeving-bestand en de Flask standaardpoort 5000 wordt toegewezen aan de lokale poort 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

De uitvoer is vergelijkbaar met wat u eerder hebt gezien. De databasemigratie van de oorspronkelijke wordt echter niet meer hoeft te worden uitgevoerd en daarom wordt overgeslagen.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

De database bevat al de registratie die u eerder hebt gemaakt.

![Docker-container op basis van een Python Flask-toepassing die lokaal wordt uitgevoerd](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>De Docker-container uploaden naar een container-register

In deze stap kunt u de Docker-container uploaden naar een container-register. Gebruik Azure Container register, maar u kunt ook andere populaire protocollen zoals Docker-Hub.

### <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken

Vervang in de volgende opdracht voor het maken van een container register  *\<registry_name >* met een unieke Azure container register-naam van uw keuze.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Uitvoer

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>De referenties van het register voor pushen en installatiekopieën van Docker binnenhalen ophalen

Als u wilt weergeven register referenties, eerst Administrator-modus inschakelen

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Ziet u twee wachtwoorden. Noteer de gebruikersnaam en het eerste wachtwoord.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Upload uw Docker-container in Azure Container register

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>De Docker Python Flask-toepassing in Azure implementeren

In deze stap maakt implementeren u om uw toepassing Docker-container op basis van een Python Flask in Azure App Service.

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

Maak een App Service-plan met de opdracht [az appservice plan create](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create).

[!INCLUDE [app-service-plan](../../../includes/app-service-plan-linux.md)]

Het volgende voorbeeld wordt een op basis van Linux-App Service-plan met de naam *myAppServicePlan* met behulp van de prijzen voor S1 trapsgewijs:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Wanneer de App Service-abonnement is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

### <a name="create-a-web-app"></a>Een webtoepassing maken

Maak een WebApp in de *myAppServicePlan* App Service-abonnement met de [az webapp maken](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) opdracht.

De web-app hebt u een hosting ruimte om uw code te implementeren en biedt een URL op voor u de gedistribueerde toepassing weergeven. Gebruik te maken van de web-app.

Vervang in de volgende opdracht, de  *\<app_naam >* aanduiding voor items met een unieke app-naam. Deze naam is onderdeel van de standaard-URL voor de web-app zodat de naam moet uniek zijn in alle apps in Azure App Service.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-the-database-environment-variables"></a>De omgevingsvariabelen database configureren

Eerder in de zelfstudie u omgevingsvariabelen verbinding maken met uw PostgreSQL-database gedefinieerd.

In App Service, stelt u omgevingsvariabelen als _appinstellingen_ met behulp van de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) opdracht.

Het volgende voorbeeld geeft de details van de database-verbinding als de app-instellingen. Gebruikt ook de *poort* poort 5000 kaart variabele van uw Docker-Container voor het ontvangen van HTTP-verkeer op poort 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Docker-container implementatie configureren

AppService kan automatisch downloaden en uitvoeren van een Docker-container.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Wanneer u de Docker-container bijwerken of de instellingen wijzigt, start u de app opnieuw. Opnieuw opstarten zorgt ervoor dat alle instellingen worden toegepast en de meest recente container wordt opgehaald uit het register.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Blader naar de Azure-web-app 

Blader naar de geïmplementeerde web-app met behulp van uw webbrowser. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> De web-app langer laden omdat de container moet worden gedownload en gestart nadat de configuratie van de container is gewijzigd.

Er is eerder geregistreerde gasten die zijn opgeslagen in de Azure-productiedatabase in de vorige stap.

![Docker-container op basis van een Python Flask-toepassing die lokaal wordt uitgevoerd](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Gefeliciteerd!** U uitvoert een Docker-container gebaseerde Python Flask-app in Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Update-gegevensmodel en de implementatie opnieuw uit

In deze stap maakt toevoegen u het aantal deelnemers aan elke gebeurtenisregistratie door het bijwerken van het model van de Gast.

Bekijk de *0,2 migratie* uitgebracht met de volgende git-opdracht:

```bash
git checkout tags/0.2-migration
```

Deze release wordt al de vereiste wijzigingen aangebracht in weergaven, domeincontrollers en model. Dit omvat ook de databasemigratie van een gegenereerd *alembic* (`flask db migrate`). Hier ziet u alle wijzigingen die via de volgende git-opdracht:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Uw wijzigingen lokaal testen

Voer de volgende opdrachten voor het testen van uw wijzigingen door lokaal uit te voeren van de flask-server.

Mac / Linux:
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Ga naar http://127.0.0.1:5000 in uw browser om de wijzigingen weer te geven. Een test maken.

![Docker-container op basis van een Python Flask-toepassing die lokaal wordt uitgevoerd](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren naar Azure

De nieuwe docker-installatiekopie bouwen, dit doorgeven aan het register van de container en start de app opnieuw.

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Navigeer naar uw Azure-web-app en probeer opnieuw om de nieuwe functionaliteit. Maak een andere gebeurtenisregistratie.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask-app in Azure App Service](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Uw Azure-web-app beheren

Ga naar de [Azure-portal](https://portal.azure.com) om te zien van de web-app die u hebt gemaakt.

Klik vanuit het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/tutorial-docker-python-postgresql-app/app-resource.png)

Standaard ziet u de portal voor uw web-app **overzicht** pagina. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina's worden weergegeven de andere configuratie dat kunt u openen.

![App Service-pagina in Azure Portal](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende zelfstudie voor informatie over het toewijzen van een aangepaste DNS-naam aan uw web-app.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
