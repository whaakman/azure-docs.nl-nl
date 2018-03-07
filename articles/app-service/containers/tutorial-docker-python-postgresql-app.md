---
title: Een Docker Python- en PostgreSQL-web-app in Azure maken | Microsoft Docs
description: Informatie over het laten functioneren van een Docker Python-app in Azure, gekoppeld aan een PostgreSQL-database.
services: app-service\web
documentationcenter: python
author: berndverst
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: c77b7c965f43dc9c4f76683aabeb6ff9c9d3a1c9
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Een Docker Python- en PostgreSQL-web-app in Azure maken

Web App for Containers biedt een uiterst schaalbare webhostingservice met self-patchfunctie. In deze zelfstudie leert u hoe u een Docker Python-web-app in Azure maakt. U verbindt deze app met een PostgreSQL-database. Als u klaar bent, hebt u een Python Flask-toepassing die wordt uitgevoerd binnen een Docker-container in [App Service onder Linux](app-service-linux-intro.md).

![Docker Python Flask-app in App Service onder Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een PostgreSQL-database in Azure maken
> * Een Python-app verbinden met PostgreSQL
> * De app implementeren in Azure
> * Het gegevensmodel bijwerken en de app opnieuw implementeren
> * De app in Azure Portal beheren

U kunt de stappen in dit artikel volgen op macOS. De instructies voor Windows en Linux zijn meestal gelijk, maar de verschillen worden in deze zelfstudie niet uitgewerkt.
 
[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

1. [Git installeren](https://git-scm.com/)
1. [Python installeren](https://www.python.org/downloads/)
1. [PostgreSQL installeren en uitvoeren](https://www.postgresql.org/download/)
1. [Docker Community Edition installeren](https://www.docker.com/community-edition)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Installatie van PostgreSQL testen en een database maken

Open het terminalvenster en voer `psql` uit om verbinding te maken met de lokale PostgreSQL-server.

```bash
sudo -u postgres psql
```

Als de verbinding is geslaagd, wordt de PostgreSQL-database uitgevoerd. Zo niet, controleer dan of de lokale PostgreSQL-database wordt gestart door de stappen te volgen op [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).

Maak een database met de naam *eventregistration* en stel een afzonderlijke databasegebruiker in met de naam *manager* en wachtwoord *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Typ `\q` om de PostgreSQL-client af te sluiten. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Lokale Python Flask-toepassing maken

In deze stap stelt u het lokale Python Flask-project in.

### <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Open het terminalvenster en `CD` naar een werkmap.

Voer de volgende opdrachten uit om een kloon te maken van de voorbeeldopslagplaats ga naar de release *0.1-initialapp*.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Deze voorbeeldopslagplaats bevat een [Flask](http://flask.pocoo.org/)-toepassing. 

### <a name="run-the-application"></a>De toepassing uitvoeren

> [!NOTE] 
> In een latere stap vereenvoudigt u dit proces door een Docker-container te maken voor gebruik met de productiedatabase.

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

Wanneer de app volledig is geladen, ziet u een bericht zoals dit:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Ga naar `http://localhost:5000` in een browser. Klik op **Registreren!** en maak een testgebruiker.

![Lokaal uitgevoerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/local-app.png)

De Flask-voorbeeldtoepassing slaat gebruikersgegevens op in de database. Als u een gebruiker hebt geregistreerd, schrijft de app gegevens naar de lokale PostgreSQL-database.

Als u de Flask-server wilt stoppen, typt u Ctrl+C in het terminalvenster. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-production-postgresql-database"></a>Productie-PostgreSQL-database maken

In deze stap maakt u een PostgreSQL-database in Azure. Als de app is geïmplementeerd in Azure, wordt deze cloud-database gebruikt.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken

Maak een PostgreSQL-server met de opdracht [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create).

Met de volgende opdracht vervangt u de tijdelijke aanduiding *\<postgresql_name>* door een unieke servernaam en de tijdelijke aanduiding *\<admin_username>* door een gebruikersnaam. De servernaam wordt gebruikt als onderdeel van het PostgreSQL-eindpunt (`https://<postgresql_name>.postgres.database.azure.com`). De naam moet dus uniek zijn voor alle servers in Azure. De gebruikersnaam is voor de gebruikersaccount van de oorspronkelijke databasebeheerder. U wordt gevraagd een wachtwoord voor deze gebruiker te kiezen.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>  --storage-size 51200
```

Wanneer de Azure Database for PostgreSQL-server wordt gemaakt, toont de Azure CLI informatie die lijkt op het volgende voorbeeld:

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

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>Een firewallregel maken voor de Azure Database for PostgreSQL-server

Voer de volgende Azure CLI-opdracht uit om vanaf alle IP-adressen toegang te verlenen tot de database.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

De Azure CLI bevestigt dat de firewallregel is gemaakt door middel van uitvoer die lijkt op die in het volgende voorbeeld:

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

## <a name="connect-your-python-flask-application-to-the-database"></a>De Python Flask-toepassing verbinden met de database

In deze stap verbindt u de Python Flask-voorbeeldtoepassing met de Azure Database for PostgreSQL-server die u hebt gemaakt.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Een lege database maken en een nieuwe gebruiker voor de databasetoepassing instellen

Maak een databasegebruiker met toegang tot één database. U gebruikt deze referenties om te voorkomen dat de toepassing volledige toegang tot de server krijgt.

Maak verbinding met de database (u wordt om uw beheerderswachtwoord gevraagd).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Maak de database en de gebruiker vanaf de PostgreSQL-CLI.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Typ `\q` om de PostgreSQL-client af te sluiten.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Toepassing lokaal tegen de Azure PostgreSQL-database testen

Ga terug naar de *app*-map van de gekloonde Github-opslagplaats. U kunt de Python Flask-toepassing nu uitvoeren door de omgevingsvariabelen van de database bij te werken.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Wanneer de app volledig is geladen, ziet u een bericht zoals dit:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Ga via een browser naar http://localhost:5000. Klik op **Registreren!** en maak een testregistratie. Er worden nu gegevens naar de database in Azure geschreven.

![Lokaal uitgevoerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>Toepassing uitvoeren vanaf een Docker-container

Bouw de installatiekopie van de Docker-container.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker bevestigt dat de container is gemaakt.

```bash
Successfully built 7548f983a36b
```

Voeg aan de hoofdmap van de opslagplaats een omgevingsvariabele toe met de naam _db.env_ en voeg vervolgens de volgende omgevingsvariabelen van de database toe. De app maakt verbinding met de Azure Database for PostgreSQL-productiedatabase.

```text
DBHOST=<postgresql_name>.postgres.database.azure.com
DBUSER=manager@<postgresql_name>
DBNAME=eventregistration
DBPASS=supersecretpass
```

Voer de app uit vanuit de Docker-container. De volgende opdracht specificeert de map met de omgevingsvariabele en wijst standaard-Flask-poort 5000 toe aan lokale poort 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

De uitvoer lijkt op wat u eerder hebt gezien. De oorspronkelijke databasemigratie hoeft echter niet meer te worden uitgevoerd en wordt dus overgeslagen.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

De database bevat al de registratie die u eerder hebt gemaakt.

![Lokaal uitgevoerde op Docker-container gebaseerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>Docker-container uploaden naar een containerregister

In deze stap uploadt u de Docker-container naar een containerregister. Gebruik Azure Container Registry, hoewel u ook andere populaire services kunt gebruiken, zoals Docker Hub.

### <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken

Met de volgende opdracht, waarmee u een containerregister maakt, vervangt u *\<registry_name>* door een unieke naam naar keuze voor een Azure-containerregister.

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

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>Registerreferenties ophalen om Docker-installatiekopieën te pushen en te pullen

Als u registerreferenties wilt weergeven, dient u eerst de beheermodus in te schakelen.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

U ziet twee wachtwoorden. Noteer de gebruikersnaam en het eerste wachtwoord.

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

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Docker-container uploaden naar Azure Container Registry

Meld u aan bij het register. Als u daarom wordt gevraagd, voert u het wachtwoord in dat u hebt opgehaald.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Push de Docker-installatiekopie naar het register.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>Docker Python Flask-toepassing implementeren in Azure

In deze stap implementeert u de op de Docker-container gebaseerde Python Flask-toepassing in Azure App Service.

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Een webtoepassing maken

Maak een web-app in het App Service-plan *myAppServicePlan* met de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create).

De web-app biedt u een hostingruimte voor het implementeren van onze code en een URL waarmee u de geïmplementeerde toepassing kunt bekijken. Gebruik te maken van de web-app.

Met de volgende opdracht vervangt u de tijdelijke aanduiding *\<app_name>* door een unieke naam voor de app. Deze naam maakt deel uit van de standaard-URL voor de web-app, dus de naam moet uniek zijn voor alle apps in Azure App Service.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-container-image-name "<registry_name>.azurecr.io/flask-postgresql-sample"
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

### <a name="configure-the-database-environment-variables"></a>De omgevingsvariabelen van de database configureren

Eerder in de zelfstudie hebt u omgevingsvariabelen gedefinieerd om verbinding te kunnen maken met de PostgreSQL-database.

In App Service stelt u omgevingsvariabelen in als _app settings_ met behulp van de opdracht [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set).

In het volgende voorbeeld worden de details van de databaseverbinding als app-instellingen opgegeven. Hierin wordt ook gebruikgemaakt van de variabele *PORT* om poort 5000 van de Docker-container te configureren voor het ontvangen van HTTP-verkeer van poort 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Implementatie van Docker-container configureren

App Service kan automatisch een Docker-container downloaden en uitvoeren.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Als u de Docker-container bijwerkt of de instellingen wijzigt, dient u de app opnieuw te starten. Door de app opnieuw te starten, worden alle instellingen toegepast en wordt de meest recente container uit het register gehaald.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Bladeren naar de Azure-web-app 

Blader naar de geïmplementeerde web-app via uw webbrowser. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> De web-app doet er langer over om te laden omdat de container moet worden gedownload en gestart als de configuratie ervan is gewijzigd.

U ziet eerder geregistreerde gasten die in de vorige stap in de Azure-productiedatabase zijn opgeslagen.

![Lokaal uitgevoerde op Docker-container gebaseerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Gefeliciteerd!** U voert een op de Docker-container gebaseerde Python Flask-app uit in Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Gegevensmodel bijwerken en opnieuw implementeren

In deze stap voegt u het aantal deelnemers aan elke gebeurtenisregistratie toe door het model Gast bij te werken.

Controleer release *0.2-migration* met de volgende git-opdracht:

```bash
git checkout tags/0.2-migration
```

In deze release zijn al de noodzakelijke wijzigingen aan weergaven, controllers en het model aangebracht. De release bevat tevens een databasemigratie die is gegenereerd via *alembic* (`flask db migrate`). Met de volgende git-opdracht kunt u alle wijzigingen bekijken:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>De wijzigingen lokaal testen

Voer de volgende opdrachten uit om uw wijzigingen lokaal te testen door de Flask-server uit te voeren.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Ga in de browser naar http://localhost:5000 om de wijzigingen weer te geven. Maak een testregistratie.

![Lokaal uitgevoerde op Docker-container gebaseerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren in Azure

Bouw de nieuwe Docker-installatiekopie, push deze naar het containerregister en start de app opnieuw.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Ga naar de Azure-web-app en probeer de nieuwe functionaliteit opnieuw uit. Maak een andere gebeurtenisregistratie.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask-app in Azure App Service](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Uw Azure-web-app beheren

Ga naar [Azure Portal](https://portal.azure.com) om de web-app te zien die u hebt gemaakt.

Klik vanuit het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/tutorial-docker-python-postgresql-app/app-resource.png)

In de portal wordt standaard de pagina **Overzicht** van de web-app getoond. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina tonen de verschillende configuratiepagina's die u kunt openen.

![App Service-pagina in Azure Portal](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Volgende stappen

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw web-app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
