---
title: Een Python- en PostgreSQL-web-app in Azure App Service maken | Microsoft Docs
description: Informatie over het uitvoeren van een Python-app op basis van gegevens in Azure, gekoppeld aan een PostgreSQL-database.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 09/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: f4ce197d541b8573e38fd85dcebb575c8ee99f59
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435731"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Een Docker Python- en PostgreSQL-web-app maken in Azure

[App Service on Linux](app-service-linux-intro.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze tutorial laat zien hoe u een gegevensgestuurde Python-webapp maakt, met PostgreSQL als de back-enddatabase. Als u klaar bent, hebt u een Python Flask-toepassing die wordt uitgevoerd binnen een Docker-container in App Service onder Linux.

![Docker Python Flask-app in App Service onder Linux](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een PostgreSQL-database maken in Azure
> * Een Python-app verbinden met PostgreSQL
> * De app implementeren in Azure
> * Diagnostische logboeken weergeven
> * Het gegevensmodel bijwerken en de app opnieuw implementeren
> * De app in Azure Portal beheren

U kunt de stappen in dit artikel volgen op macOS. De instructies voor Windows en Linux zijn meestal gelijk, maar de verschillen worden in deze zelfstudie niet uitgewerkt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

1. [Git installeren](https://git-scm.com/)
1. [Python installeren](https://www.python.org/downloads/)
1. [PostgreSQL installeren en uitvoeren](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Lokale installatie van PostgreSQL testen en een database maken

Voer in het lokale terminalvenster `psql` uit om verbinding te maken met de lokale PostgreSQL-server.

```bash
sudo -u postgres psql postgres
```

Als er een foutbericht weergegeven dat vergelijkbaar is met `unknown user: postgres`, is het mogelijk dat uw PostgreSQL-installatie is geconfigureerd met uw aangemelde gebruikersnaam. Probeer in dat geval de volgende opdracht.

```bash
psql postgres
```

Als de verbinding is geslaagd, wordt de PostgreSQL-database uitgevoerd. Zo niet, controleer dan of de lokale PostgreSQL-database wordt gestart door de instructies voor uw besturingssysteem te volgen op [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).

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

Voer de volgende opdrachten uit om de voorbeeldopslagplaats te klonen.

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app.git
cd flask-postgresql-app
```

Deze voorbeeldopslagplaats bevat een [Flask](http://flask.pocoo.org/)-toepassing.

### <a name="run-the-app-locally"></a>De app lokaal uitvoeren

Installeer de vereiste pakketten en start de toepassing.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run

# PowerShell
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
Set-Item Env:FLASK_APP ".\app.py"
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
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

![Lokaal uitgevoerde Python Flask-toepassing](./media/tutorial-python-postgresql-app/local-app.png)

De Flask-voorbeeldtoepassing slaat gebruikersgegevens op in de database. Als u een gebruiker hebt geregistreerd, schrijft de app gegevens naar de lokale PostgreSQL-database.

Als u de Flask-server wilt stoppen, typt u Ctrl+C in het terminalvenster.

## <a name="create-a-production-postgresql-database"></a>Productie-PostgreSQL-database maken

In deze stap maakt u een PostgreSQL-database in Azure. Als de app is geïmplementeerd in Azure, wordt deze cloud-database gebruikt.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken

Maak een PostgreSQL-server met de opdracht [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) in Cloud Shell.

In de volgende opdracht vervangt u *\<postgresql_name>* door een unieke servernaam en *\<admin_username >* en *\<admin_password>* door de gewenste gebruikersreferenties. De gebruikersreferenties zijn voor het gebruikersaccount van de databasebeheerder. De servernaam wordt gebruikt als onderdeel van het PostgreSQL-eindpunt (`https://<postgresql_name>.postgres.database.azure.com`). De naam moet dus uniek zijn voor alle servers in Azure.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name B_Gen4_1
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
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> Onthoud of noteer de waarden voor \<admin_username> en \<admin_password>. U hebt deze later nodig als u zich gaat aanmelden bij de Postgre-server en de bijbehorende databases.


### <a name="create-firewall-rules-for-the-postgresql-server"></a>Firewallregels maken voor de PostgreSQL-server

Voer in Cloud Shell de volgende Azure CLI-opdracht uit om toegang te verlenen tot de database vanaf Azure-resources.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Met deze instelling worden netwerkverbindingen toegestaan vanaf alle IP-adressen binnen het Azure-netwerk. Voor gebruik in productieomgevingen moet u proberen om de meest beperkende firewallregels te configureren door [alleen de uitgaande IP-adressen te gebruiken die uw app gebruikt](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

Voer in Cloud Shell de opdracht opnieuw uit om toegang vanaf uw lokale computer mogelijk te maken door *\<your_ip_address>* te vervangen door [uw lokale IPv4 IP-adres](http://www.whatsmyip.org/).

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>De Python-app verbinden met de productiedatabase

In deze stap verbindt u de Flask-voorbeeld-app met de Azure Database for PostgreSQL-server die u hebt gemaakt.

### <a name="create-empty-database-and-user-access"></a>Lege database maken en gebruikerstoegang

Maak vanuit het lokale terminalvenster verbinding met de database door de onderstaande opdracht uit te voeren. Wanneer u wordt gevraagd om uw beheerderswachtwoord, gebruikt u hetzelfde wachtwoord dat u hebt opgegeven in [Een Azure Database for PostgreSQL-server maken](#create-an-azure-database-for-postgresql-server).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Net zoals bij uw lokale Postgres-server, maakt u de database en de gebruiker op de Azure-Postgres-server.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Typ `\q` om de PostgreSQL-client af te sluiten.

> [!NOTE]
> Het wordt aanbevolen om databasegebruikers te maken met beperkte machtigingen voor specifieke toepassingen, in plaats van de gebruiker met beheerdersrechten te gebruiken. In dit voorbeeld heeft de gebruiker `manager` volledige rechten voor _alleen_ de database `eventregistration`.

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

![Lokaal uitgevoerde Python Flask-toepassing](./media/tutorial-python-postgresql-app/local-app.png)

## <a name="deploy-to-azure"></a>Implementeren in Azure

In deze step implementeert u de met Postgres verbonden Python-toepassing naar Azure App Service.

### <a name="configure-repository"></a>Opslagplaats configureren

De Git-implementatie-engine in App Service roept `pip`-automatisering aan wanneer er een bestand _application.py_ in de hoofdmap van de opslagplaats staat. In deze zelfstudie laat u de implementatie-engine de automatisering voor u uitvoeren. Navigeer in het lokale terminalvenster naar de hoofdmap van de opslagplaats, maak een dummy-bestand _application.py_ en voer de wijzigingen door met commit.

```bash
cd ..
touch application.py
git add .
git commit -m "ensure azure automation"
```

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Een webtoepassing maken 

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

Eerder in de zelfstudie hebt u omgevingsvariabelen gedefinieerd om verbinding te kunnen maken met de PostgreSQL-database.

In App Service stelt u omgevingsvariabelen in als _app-instellingen_ met behulp van de opdracht [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell.

In het volgende voorbeeld worden de details van de databaseverbinding als app-instellingen opgegeven. 

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

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

### <a name="configure-entry-point"></a>Ingangspunt configureren

Standaard zoekt de ingebouwde installatiekopie naar een bestand _wsgi.py_ of _application.py_ in de hoofdmap als het ingangspunt, maar uw ingangspunt is _app/app.py_. Het bestand _application.py_ dat u eerder hebt toegevoegd, is leeg en doet niets.

Voer in Cloud Shell de opdracht [`az webapp config set`](/cli/azure/webapp/config?view=azure-cli-latest#az-webapp-config-set) uit om een aangepast opstartscript in te stellen.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --startup-file "gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/app app:app"
```

De parameter `--startup-file` accepteert een aangepaste opdracht of het pad naar het bestand dat de aangepaste opdracht bevat. De aangepaste opdracht moet de volgende indeling hebben:

```
gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/<subdirectory> <module>:<variable>
```

In de aangepaste opdracht is `--chdir` vereist als uw ingangspunt zich in de hoofdmap bevindt en `<subdirectory>` de submap is. `<module>` de naam van het _.py_-bestand en `<variable>` de variabele in de module die uw web-app vertegenwoordigt.

### <a name="browse-to-the-azure-web-app"></a>Bladeren naar de Azure-web-app

Blader naar de geïmplementeerde web-app. Het duurt even voordat de web-app wordt gestart omdat de container moet worden gedownload en uitgevoerd wanneer de app voor het eerst wordt aangevraagd. Als er een time-out optreedt op de pagina of als er een foutbericht wordt weergegeven, wacht u een paar minuten en vernieuwt u de pagina.

```bash
http://<app_name>.azurewebsites.net
```

U ziet eerder geregistreerde gasten die in de vorige stap in de Azure-productiedatabase zijn opgeslagen.

![Python Flask-toepassing die wordt uitgevoerd in Azure](./media/tutorial-python-postgresql-app/docker-app-deployed.png)

**Gefeliciteerd!** U hebt een Python Flask-app gemaakt die wordt uitgevoerd in App Service voor Linux.

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

Omdat de Python-app wordt uitgevoerd in een container, hebt u via App Service voor Linux toegang tot de logboeken die door de console vanuit de container zijn gegenereerd. U vindt de logboeken op deze URL:

```
https://<app_name>.scm.azurewebsites.net/api/logs/docker
```

U ziet daar twee JSON-objecten, elk met een eigenschap `href`. Eén `href` verwijst naar de consolelogboeken van Docker (eindigt op `_docker.log`) en de andere `href` verwijst naar de consolelogboeken die zijn gegenereerd vanuit de Python-container. 

```json
[  
   {  
      "machineName":"RD0003FF61ACD0_default",
      "lastUpdated":"2018-09-27T16:48:17Z",
      "size":4766,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log"
   },
   {  
      "machineName":"RD0003FF61ACD0",
      "lastUpdated":"2018-09-27T16:48:19Z",
      "size":2589,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log"
   }
]
```

Kopieer de gewenste `href`-waarde naar een browservenster om naar de logboeken te navigeren. De logboeken worden niet gestreamd, wat betekent dat er enige vertraging kan optreden. Vernieuw de browserpagina om nieuwe logboeken te zien.

## <a name="update-data-model-and-redeploy"></a>Gegevensmodel bijwerken en opnieuw implementeren

In deze stap voegt u het aantal deelnemers aan elke gebeurtenisregistratie toe door het model `Guest` bij te werken en het model vervolgens opnieuw te implementeren naar Azure.

Gebruik in het lokale terminalvenster de volgende git-opdracht om bestanden uit te checken bij de vertakking `modelChange`:

```bash
git checkout origin/modelChange -- .
```

Hiermee worden al de noodzakelijke wijzigingen aangebracht aan het model, de weergaven en de controllers. De release bevat tevens een databasemigratie die is gegenereerd via *alembic* (`flask db migrate`). Met de volgende git-opdracht kunt u alle wijzigingen bekijken:

```bash
git diff master origin/modelChange
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

![Lokaal uitgevoerde op Docker-container gebaseerde Python Flask-toepassing](./media/tutorial-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren in Azure

Leg in het lokale terminalvenster alle wijzigingen vast in Git en push de codewijzigingen vervolgens naar Azure.

```bash 
git add . 
git commit -m "updated data model" 
git push azure master 
``` 

Ga naar de Azure-web-app en probeer de nieuwe functionaliteit opnieuw uit. Zorg ervoor dat u de pagina vernieuwt.

```bash
http://<app_name>.azurewebsites.net
```

![Docker Python Flask-app in Azure App Service](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-web-app-in-the-azure-portal"></a>Uw web-app beheren in Azure Portal

Ga naar [Azure Portal](https://portal.azure.com) om de web-app te zien die u hebt gemaakt.

Klik vanuit het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/tutorial-python-postgresql-app/app-resource.png)

In de portal wordt standaard de pagina **Overzicht** van de web-app getoond. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina tonen de verschillende configuratiepagina's die u kunt openen.

![App Service-pagina in Azure Portal](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een PostgreSQL-database maken in Azure
> * Een Python-app verbinden met PostgreSQL
> * De app implementeren in Azure
> * Diagnostische logboeken weergeven
> * Het gegevensmodel bijwerken en de app opnieuw implementeren
> * De app in Azure Portal beheren

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw web-app kunt toewijzen.

> [!div class="nextstepaction"]
> [Ingebouwde Python-installatiekopie configureren](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service-web-tutorial-custom-domain.md)

