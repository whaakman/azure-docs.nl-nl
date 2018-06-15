---
title: Een Python- en PostgreSQL-web-app in Azure maken | Microsoft Docs
description: Informatie over het laten functioneren van een Python-app in Azure, gekoppeld aan een PostgreSQL-database.
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/25/2018
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: 49ec67d06446d6c48e45aef90e2bd528a1b541a9
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
ms.locfileid: "33203507"
---
# <a name="tutorial-build-a-python-and-postgresql-web-app-in-azure"></a>Zelfstudie: een Python- en PostgreSQL-web-app in Azure maken

> [!NOTE]
> In dit artikel gaat u een app implementeren in App Service onder Windows. Zie [Een Docker Python- en PostgreSQL-web-app in Azure maken](./containers/tutorial-docker-python-postgresql-app.md) voor een implementatie in App Service onder _Linux_.
>

[Azure App Service](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. In deze zelfstudie leert u hoe u een Python-web-app in Azure maakt. U verbindt deze app met een PostgreSQL-database. Als u klaar bent, hebt u een Python Flask-toepassing die wordt uitgevoerd in App Service.

![Python Flask-app in App Service onder Linux](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een PostgreSQL-database maken in Azure
> * Een Python-app voor MySQL maken
> * De app implementeren in Azure
> * Het gegevensmodel bijwerken en de app opnieuw implementeren
> * De app in Azure Portal beheren

U kunt de stappen in deze zelfstudie volgen op macOS. De instructies voor Windows en Linux zijn meestal gelijk, maar de verschillen worden in deze zelfstudie niet uitgewerkt.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

1. [Git installeren](https://git-scm.com/)
1. [Python installeren](https://www.python.org/downloads/)
1. [PostgreSQL installeren en uitvoeren](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Installatie van PostgreSQL testen en een database maken

Open het terminalvenster en voer `psql` uit om verbinding te maken met de lokale PostgreSQL-server.

```bash
sudo -u postgres psql
```

Als de verbinding is geslaagd, wordt de PostgreSQL-database uitgevoerd. Zo niet, controleer dan of de lokale PostgreSQL-database wordt gestart door de stappen te volgen op [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).

Maak een database met de naam *eventregistration* en stel een afzonderlijke databasegebruiker in met de naam *manager* en het wachtwoord *supersecretpass*.

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

Voer de volgende opdrachten uit om de voorbeeld-opslagplaats te klonen en keer vervolgens terug naar de commit voor de eerste app (vóór `modelChange`).

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app
cd flask-postgresql-app
git revert modelChange --no-edit
```

Deze voorbeeldopslagplaats bevat een [Flask](http://flask.pocoo.org/)-toepassing. 

### <a name="run-the-application"></a>De toepassing uitvoeren

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

![Lokaal uitgevoerde Python Flask-toepassing](./media/app-service-web-tutorial-python-postgresql/local-app.png)

De Flask-voorbeeldtoepassing slaat gebruikersgegevens op in de database. Als u een gebruiker hebt geregistreerd, schrijft de app gegevens naar de lokale PostgreSQL-database.

Als u de Flask-server wilt stoppen, drukt u op Ctrl+C in het terminalvenster. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-postgresql-in-azure"></a>PostgreSQL maken in Azure

In deze stap maakt u een PostgreSQL-database in Azure. Als de app is geïmplementeerd in Azure, wordt deze cloud-database gebruikt.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-postgresql-server"></a>Een PostgreSQL-server maken

Maak een PostgreSQL-server met de opdracht [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create).

Vervang in de volgende opdracht de tijdelijke aanduiding *\<postgresql_name>* door een unieke servernaam, de tijdelijke aanduiding *\<admin_username>* door een gebruikersnaam, en de tijdelijke aanduiding *\<admin_password>* door een wachtwoord. De servernaam wordt gebruikt als onderdeel van het PostgreSQL-eindpunt (`https://<postgresql_name>.postgres.database.azure.com`). De naam moet dus uniek zijn voor alle servers in Azure.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <server_admin_password> --sku-name GP_Gen4_2
```

Wanneer de Azure Database for PostgreSQL-server wordt gemaakt, toont de Azure CLI informatie die lijkt op het volgende voorbeeld:

```json
{
  "location": "westeurope",
  "name": "<postgresql_server_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "additionalProperties": {},
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  ...   +  
  -  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Een serverfirewall configureren

Voer de volgende Azure CLI-opdracht uit om vanaf alle IP-adressen toegang te verlenen tot de database. Als zowel het IP-beginadres als het IP-eindadres zijn ingesteld op 0.0.0.0, wordt de firewall alleen geopend voor andere Azure-resources. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0 --name AllowAzureIPs
```

De Azure CLI bevestigt dat de firewallregel is gemaakt met uitvoer die lijkt op die in het volgende voorbeeld:

```json
{
  "additionalProperties": {},
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
 "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

> [!TIP] 
> U kunt uw firewallregel nog beperkender maken door [alleen de uitgaande IP-adressen te gebruiken die in uw app worden gebruikt](app-service-ip-addresses.md#find-outbound-ips).
>

### <a name="create-a-production-database-and-user"></a>Maak een productiedatabase en gebruiker

Maak een databasegebruiker met toegang tot slechts één database. U gebruikt deze referenties om te voorkomen dat de toepassing volledige toegang tot de server krijgt.

Maak verbinding met de database (u wordt om uw beheerderswachtwoord gevraagd).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <admin_username>@<postgresql_name> postgres
```

Maak de database en de gebruiker vanaf de PostgreSQL-CLI.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Typ `\q` om de PostgreSQL-client af te sluiten.

### <a name="test-app-locally-with-azure-postgresql"></a>App lokaal testen met Azure PostgreSQL

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

Ga naar http://localhost:5000 in een browser. Klik op **Registreren!** en maak een testregistratie. Er worden nu gegevens naar de database in Azure geschreven.

![Lokaal uitgevoerde Python Flask-toepassing](./media/app-service-web-tutorial-python-postgresql/local-app.png)

## <a name="deploy-to-azure"></a>Implementeren in Azure

In deze step implementeert u de met PostgreSQL verbonden Python-toepassing naar Azure App Service.

De Git-opslagplaats bevat al de volgende bestanden die nodig zijn om de Flask-web-app in App Service uit te voeren:

- `.deployment`: hiermee wordt het aangepaste implementatiescript opgegeven.
- `deploy.cmd`: het implementatiescript. Hier wordt `pip install` uitgevoerd.
- `web.config`: hiermee wordt opgegeven welk invoerpunt-script wordt uitgevoerd in een `httpPlatformHandler` in IIS.
- `run_waitress_server.py`: het invoerpunt-script. Hiermee wordt de Flask-web-app in een [ `waitress` ](https://docs.pylonsproject.org/projects/waitress)-server gestart.

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

<a name="create"></a>
### <a name="create-a-web-app"></a>Een webtoepassing maken

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)] 

### <a name="install-python"></a>Python installeren

In deze stap installeert u Python 3.6.2 met [site-extensies](https://www.siteextensions.net/packages?q=Tags%3A%22python%22) in App Service. U gebruikt de referenties die u hebt geconfigureerd in [Een implementatiegebruiker configureren](#configure-a-deployment-user) voor verificatie op basis van het REST-eindpunt.

Voer in de Cloud Shell de volgende opdracht uit om de informatie over het Python 3.6.2-pakket op te halen. Vervang  *\<deployment_user>* door de gebruikersnaam van de implementatie die u hebt geconfigureerd en  *\<app_name>* door de naam van uw app. Gebruik, wanneer u daarnaar wordt gevraagd, het implementatiewachtwoord dat u hebt geconfigureerd.

```bash
packageinfo=$(curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/extensionfeed/python362x86)
```

Voer in de Cloud Shell de volgende opdracht uit om het Python-pakket te installeren. Vervang  *\<deployment_user>* door de gebruikersnaam van de implementatie die u hebt geconfigureerd en  *\<app_name>* door de naam van uw app. Gebruik, wanneer u daarnaar wordt gevraagd, het implementatiewachtwoord dat u hebt geconfigureerd.

```bash
curl -X PUT -u <deployment_user> -H "Content-Type: application/json" -d '$packageinfo' https://<app_name>.scm.azurewebsites.net/api/siteextensions/python362x86
```

In de uitvoer van de opdracht kunt u zien dat Python geïnstalleerd is op het pad `D:\home\python362x86\python.exe`.

### <a name="configure-database-settings"></a>Database-instellingen configureren

Eerder in de zelfstudie hebt u omgevingsvariabelen gedefinieerd om verbinding te kunnen maken met de PostgreSQL-database.

In App Service stelt u omgevingsvariabelen in als _app settings_ met behulp van de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set).

In het volgende voorbeeld worden de details van de databaseverbinding als app-instellingen opgegeven. Vervang  *\<app_name>* door de naam van uw app en  *\<postgresql_name>* door de naam van uw PostgreSQL-server.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

### <a name="browse-to-the-azure-web-app"></a>Bladeren naar de Azure-web-app 

Blader naar de geïmplementeerde web-app via uw webbrowser. 

```bash 
http://<app_name>.azurewebsites.net 
```

U ziet eerder geregistreerde gasten die in de vorige stap in de Azure-productiedatabase zijn opgeslagen.

![Lokaal uitgevoerde Python Flask-toepassing](./media/app-service-web-tutorial-python-postgresql/docker-app-deployed.png)

**Gefeliciteerd!** U voert een Python Flask-app uit in Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Gegevensmodel bijwerken en opnieuw implementeren

In deze stap voegt u het aantal deelnemers aan elke gebeurtenisregistratie toe door het `Guest`-model bij te werken.

Bekijk de bestanden die zijn gemarkeerd door de `modelChange`-commit:

```bash
git checkout modelChange -- *
```

In deze release zijn al de noodzakelijke wijzigingen aan weergaven, controllers en het model aangebracht. De release bevat tevens een databasemigratie die is gegenereerd via *alembic* (`flask db migrate`). U kunt de wijzigingen aan alle bestanden zien in de [GitHub-commitweergave](https://github.com/Azure-Samples/flask-postgresql-app/commit/139a53023688631c3cc2caefd70086f4722ecd7e).

### <a name="test-your-changes-locally"></a>De wijzigingen lokaal testen

Voer de volgende opdrachten uit om uw wijzigingen lokaal te testen door de Flask-server uit te voeren.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Ga in de browser naar http://localhost:5000 om de wijzigingen weer te geven. Maak een testregistratie.

![Lokaal uitgevoerde Python Flask-toepassing](./media/app-service-web-tutorial-python-postgresql/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren in Azure

Leg in het lokale terminalvenster alle wijzigingen vast in Git en push de codewijzigingen vervolgens naar Azure.

```bash
git add .
git commit -m "updated data model"
git push azure master
```

Ga naar de Azure-web-app en probeer de nieuwe functionaliteit opnieuw uit. Maak een andere gebeurtenisregistratie.

```bash 
http://<app_name>.azurewebsites.net 
```

![Python Flask-app in Azure App Service](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Uw Azure-web-app beheren

Ga naar [Azure Portal](https://portal.azure.com) om de web-app te zien die u hebt gemaakt.

Klik vanuit het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-tutorial-python-postgresql/app-resource.png)

In de portal wordt standaard de pagina **Overzicht** van de web-app getoond. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina tonen de verschillende configuratiepagina's die u kunt openen.

![App Service-pagina in Azure Portal](./media/app-service-web-tutorial-python-postgresql/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw web-app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md)
