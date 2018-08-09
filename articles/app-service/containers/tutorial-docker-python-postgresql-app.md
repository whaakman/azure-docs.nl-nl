---
title: Een Python- en PostgreSQL-web-app in Azure App Service maken | Microsoft Docs
description: Informatie over het uitvoeren van een Python-app op basis van gegevens in Azure, gekoppeld aan een PostgreSQL-database.
services: app-service\web
documentationcenter: python
author: berndverst
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: ce84498ab89891bd7b96cfcc6b0c7ac029c93cbd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423076"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Een Docker Python- en PostgreSQL-web-app maken in Azure

Web App for Containers biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze tutorial laat zien hoe u een gegevensgestuurde Python-webapp maakt, met PostgreSQL als de back-enddatabase. Als u klaar bent, hebt u een Python Flask-toepassing die wordt uitgevoerd binnen een Docker-container in [App Service onder Linux](app-service-linux-intro.md).

![Docker Python Flask-app in App Service onder Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een PostgreSQL-database maken in Azure
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

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Lokale installatie van PostgreSQL testen en een database maken

Voer in het lokale terminalvenster `psql` uit om verbinding te maken met de lokale PostgreSQL-server.

```bash
sudo -u postgres psql
```

Als de verbinding is geslaagd, wordt de PostgreSQL-database uitgevoerd. Zo niet, controleer dan of de lokale PostgreSQL-database wordt gestart door de stappen te volgen op [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).

Maak een database met de naam *eventregistration* en stel een afzonderlijke databasegebruiker in met de naam *manager* en het wachtwoord *supersecretpass*.

```sql
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Typ `\q` om de PostgreSQL-client af te sluiten. 

<a name="step2"></a>

## <a name="create-local-python-app"></a>Lokale Python-app maken

In deze stap stelt u het lokale Python Flask-project in.

### <a name="clone-the-sample-app"></a>De voorbeeld-app klonen

Open het terminalvenster en `CD` naar een werkmap.

Voer de volgende opdrachten uit om een kloon te maken van de voorbeeldopslagplaats en ga naar de release *0.1-initialapp*.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Deze voorbeeldopslagplaats bevat een [Flask](http://flask.pocoo.org/)-toepassing. 

### <a name="run-the-app-locally"></a>De app lokaal uitvoeren

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

Als de app is geladen, ziet u een bericht zoals dit:

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

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken

Maak een PostgreSQL-server met de opdracht [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) in Cloud Shell.

In de volgende opdracht vervangt u *\<postgresql_name>* door een unieke servernaam en *\<admin_username >* en *\<admin_password>* door de gewenste gebruikersreferenties. De servernaam wordt gebruikt als onderdeel van het PostgreSQL-eindpunt (`https://<postgresql_name>.postgres.database.azure.com`). De naam moet dus uniek zijn voor alle servers in Azure. De gebruikersreferenties zijn voor het gebruikersaccount van de databasebeheerder. 

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name GP_Gen4_2
```

Wanneer de Azure Database for PostgreSQL-server wordt gemaakt, toont de Azure CLI informatie die lijkt op het volgende voorbeeld:

```json
{
  "administratorLogin": "<admin_username>",
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

### <a name="create-a-firewall-rule-for-the-postgresql-server"></a>Een firewallregel maken voor de PostgreSQL-server

Voer in Cloud Shell de volgende Azure CLI-opdracht uit om vanaf alle IP-adressen toegang te verlenen tot de database. Als zowel het begin-IP-adres als het eind-IP-adres zijn ingesteld op `0.0.0.0`, wordt de firewall alleen geopend voor andere Azure-resources. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAzureIPs
```

> [!TIP] 
> U kunt uw firewallregel nog beperkender maken door [alleen de uitgaande IP-adressen te gebruiken die in uw app worden gebruikt](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

Voer in Cloud Shell de opdracht opnieuw uit om toegang tot de database vanaf uw lokale computer mogelijk te maken door *\<you_ip_address>* te vervangen door [uw lokale IPv4 IP-adres](https://whatismyipaddress.com/). 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<you_ip_address> --end-ip-address=<you_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>De Python-app verbinden met de productiedatabase

In deze stap verbindt u de Flask-voorbeeld-app met de Azure Database for PostgreSQL-server die u hebt gemaakt.

### <a name="create-empty-database-and-user-access"></a>Lege database maken en gebruikerstoegang

Maak in Cloud Shell verbinding met de database door `psql` uit te voeren. Wanneer u wordt gevraagd om uw beheerderswachtwoord, gebruikt u hetzelfde wachtwoord dat u hebt opgegeven in [Een Azure Database for PostgreSQL-server maken](#create-an-azure-database-for-postgresql-server).

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

### <a name="test-app-connectivity-to-production-database"></a>De app-verbinding met de productiedatabase testen

Ga terug naar het lokale terminalvenster en voer de volgende opdrachten uit om de Flask-databasemigratie en de Flask-server uit te voeren.

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

Ga naar http://localhost:5000 in een browser. Klik op **Registreren!** en maak een testregistratie. Er worden nu gegevens naar de database in Azure geschreven.

![Lokaal uitgevoerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/local-app.png)

## <a name="upload-app-to-a-container-registry"></a>App uploaden naar een containerregister

In deze stap maakt u een Docker-installatiekopie en uploadt u deze naar Azure Container Registry. U kunt ook populaire registers zoals Docker Hub gebruiken.

### <a name="build-the-docker-image-and-test-it"></a>De Docker-installatiekopie bouwen en testen

Bouw in het lokale terminalvenster de Docker-installatiekopie.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker toont een bevestiging dat de container is gemaakt.

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

Voer de installatiekopie lokaal uit in een Docker-container. De volgende opdracht specificeert de map met het omgevingsvariabelebestand en wijst standaard-Flask-poort 5000 toe aan lokale poort 5000.

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

Nu u hebt gecontroleerd dat de container lokaal werkt, verwijdert u _db.env_. In Azure App Service gebruikt u app-instellingen om de omgevingsvariabelen te definiëren.  

### <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken

Maak in Cloud Shell een register in Azure Container Registry met de volgende opdracht. Vervang *\<registry_name>* door een unieke registernaam.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

### <a name="retrieve-registry-credentials"></a>Registerreferenties ophalen

Voer in Cloud Shell de volgende opdrachten uit om de registerreferenties op te halen. U hebt deze nodig om de installatiekopieën te pushen en op te halen.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

U ziet twee wachtwoorden in de uitvoer. Noteer de gebruikersnaam (die standaard de registernaam is) en het eerste wachtwoord.

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

### <a name="upload-docker-image-to-registry"></a>Docker-installatiekopie uploaden naar register

Meld u vanuit het lokale terminalvenster aan bij uw nieuwe register met `docker`. Als u daarom wordt gevraagd, voert u het wachtwoord in dat u hebt opgehaald.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Push uw Docker-installatiekopie naar het register.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="create-web-app-with-uploaded-image"></a>Web-app maken met de geüploade installatiekopie

In deze stap maakt u een app in Azure App Service en configureert u deze om de geüploade Docker-installatiekopie te gebruiken in Azure Container Registry.

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Een webtoepassing maken

Maak in Cloud Shell een web-app in het App Service-plan *myAppServicePlan* met de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create).

Met de volgende opdracht vervangt u de tijdelijke aanduiding *\<app_name>* door een unieke naam voor de app. Deze naam maakt deel uit van de standaard-URL voor de web-app, dus de naam moet uniek zijn voor alle apps in Azure App Service.

```azurecli-interactive
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

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

Eerder in de zelfstudie hebt u omgevingsvariabelen gedefinieerd om verbinding te kunnen maken met de PostgreSQL-database.

In App Service stelt u omgevingsvariabelen in als _app settings_ met behulp van de opdracht [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set).

In het volgende voorbeeld worden de details van de databaseverbinding als app-instellingen opgegeven. Het gebruikt ook de variabele *WEBSITES_PORT* voor de containerpoort 5000, waarmee de container HTTP-verkeer op poort 80 kan ontvangen.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" WEBSITES_PORT=5000
```

### <a name="configure-custom-container-deployment"></a>Aangepaste containerimplementatie configureren

Hoewel u de naam van de containerinstallatiekopie al hebt opgegeven, moet u nog wel de aangepaste register-URL en de gebruikersreferenties opgeven. Voer in Cloud Shell de opdracht [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) uit.

```azurecli-interactive
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Start de app opnieuw in Cloud Shell. Door de app opnieuw te starten, worden alle instellingen toegepast en wordt de meest recente container uit het register gehaald.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Bladeren naar de Azure-web-app 

Blader naar de geïmplementeerde web-app. 

```bash 
http://<app_name>.azurewebsites.net 
```

> [!NOTE]
> Het duurt even voordat de web-app wordt gestart, omdat de container moet worden gedownload en uitgevoerd wanneer de app voor het eerst wordt aangevraagd. Als u in het begin na een lange tijd een foutmelding ziet, vernieuwt u de pagina.

U ziet eerder geregistreerde gasten die in de vorige stap in de Azure-productiedatabase zijn opgeslagen.

![Lokaal uitgevoerde op Docker-container gebaseerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Gefeliciteerd!** U voert een Python-app uit in Azure Web App for Containers.

## <a name="update-data-model-and-redeploy"></a>Gegevensmodel bijwerken en opnieuw implementeren

In deze stap voegt u het aantal deelnemers aan elke gebeurtenisregistratie toe door het `Guest`-model bij te werken.

Check in het lokale terminalvenster de release *0.2-migration* uit met de volgende git-opdracht:

```bash
git checkout tags/0.2-migration
```

In deze release zijn de noodzakelijke wijzigingen aan het model, de weergaven en de controllers al aangebracht. De release bevat tevens een databasemigratie die is gegenereerd via *alembic* (`flask db migrate`). Met de volgende git-opdracht kunt u alle wijzigingen bekijken:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>De wijzigingen lokaal testen

Voer in het lokale terminalvenster de volgende opdrachten uit om uw wijzigingen lokaal te testen door de Flask-server uit te voeren.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Ga in de browser naar http://localhost:5000 om de wijzigingen weer te geven. Maak een testregistratie.

![Lokaal uitgevoerde op Docker-container gebaseerde Python Flask-toepassing](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren in Azure

Bouw in het lokale terminalvenster de nieuwe Docker-installatiekopie en push deze naar uw register.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

Start de app opnieuw in Cloud Shell om ervoor te zorgen dat de nieuwste container uit het register wordt gehaald.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
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
