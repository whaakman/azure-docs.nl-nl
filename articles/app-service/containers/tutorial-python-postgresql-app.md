---
title: Een Python-app bouwen met PostgreSQL in Linux - Azure App Service | Microsoft Docs
description: Informatie over het uitvoeren van een Python-app op basis van gegevens in Azure, gekoppeld aan een PostgreSQL-database.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: beverst;cephalin
ms.custom: seodec18
ms.openlocfilehash: 11918ecf741ef31354cb281d3e439585506683f4
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417224"
---
# <a name="build-a-python-and-postgresql-app-in-azure-app-service"></a>Een Python- en PostgreSQL-app in Azure App Service maken

[App Service on Linux](app-service-linux-intro.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze tutorial laat zien hoe u een gegevensgestuurde Python-app maakt, met PostgreSQL als de back-enddatabase. Als u klaar bent, hebt u een Django-toepassing die wordt uitgevoerd in App Service in Linux.

![Python Django-app in App Service in Linux](./media/tutorial-python-postgresql-app/django-admin-azure.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een PostgreSQL-database maken in Azure
> * Een Python-app verbinden met PostgreSQL
> * De app implementeren in Azure
> * Diagnostische logboeken weergeven
> * De app in Azure Portal beheren

> [!NOTE]
> Kijk [hier](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) voordat u een Azure Database for PostgreSQL gaat maken om te zien welke compute-generatie beschikbaar is in uw regio.

U kunt de stappen in dit artikel volgen op macOS. De instructies voor Windows en Linux zijn meestal gelijk, maar de verschillen worden in deze zelfstudie niet uitgewerkt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

1. [Git installeren](https://git-scm.com/)
2. [Python installeren](https://www.python.org/downloads/)
3. [PostgreSQL installeren en uitvoeren](https://www.postgresql.org/download/)

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

Maak een database met de naam *pollsdb* en stel een afzonderlijke databasegebruiker in met de naam *manager* en het wachtwoord *supersecretpass*.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Typ `\q` om de PostgreSQL-client af te sluiten.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Lokale Python-app maken

In deze stap stelt u het lokale Python Django-project in.

### <a name="clone-the-sample-app"></a>De voorbeeld-app klonen

Open het terminalvenster en `CD` naar een werkmap.

Voer de volgende opdrachten uit om de voorbeeldopslagplaats te klonen.

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

Deze voorbeeldopslagplaats bevat een [Django](https://www.djangoproject.com/)-toepassing. Het is dezelfde gegevensgestuurde app die u krijgt door de [zelfstudie Aan de slag in de Django-documentatie](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) te volgen. Deze zelfstudie bevat geen inhoudelijke informatie over Django, maar laat zien hoe u een Django-app (of andere gegevensgestuurde Python-app) kunt implementeren en uitvoeren in App Service.

### <a name="configure-environment"></a>Omgeving configureren

Maak een virtuele Python-omgeving en gebruik een script om de instellingen voor de databaseverbinding in te stellen.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
source ./env.sh

# PowerShell
py -3 -m venv venv
venv\scripts\activate
.\env.ps1
```

De omgevingsvariabelen die worden gedefinieerd in *env.sh* en *env.ps1* worden in _azuresite/settings.py_ gebruikt om de database-instellingen te definiëren.

### <a name="run-app-locally"></a>De app lokaal uitvoeren

Installeer de vereiste pakketten, [voer Django-migraties uit](https://docs.djangoproject.com/en/2.1/topics/migrations/) en [maak een gebruiker met beheerdersrechten](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user).

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

Zodra de gebruiker met beheerdersrechten is gemaakt, voert u de Django-server uit.

```bash
python manage.py runserver
```

Wanneer de app volledig is geladen, ziet u een bericht zoals dit:

```bash
Performing system checks...

System check identified no issues (0 silenced).
October 26, 2018 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Ga naar `http://localhost:8000` in een browser. Als het goed is, ziet u nu het bericht `No polls are available.`. 

Navigeer naar `http://localhost:8000/admin` en meld u aan met de gebruiker met beheerdersrechten die u hebt gemaakt in de vorige stap. Klik naast **Vragen** op **Toevoegen** en maak een poll-vraag met enkele keuzen.

![Lokaal uitgevoerde Python Django-toepassing](./media/tutorial-python-postgresql-app/django-admin-local.png)

Als u opnieuw naar `http://localhost:8000` navigeert ziet u de poll-vraag weergegeven.

Met de Django-voorbeeldtoepassing worden gebruikersgegevens opgeslagen in de database. Als het gelukt is om een poll-vraag toe te voegen, worden met de app gegevens naar de lokale PostgreSQL-database geschreven.

U kunt op elk gewenst moment in het terminalvenster op Ctrl+C drukken om de Django-server te stoppen.

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
> Met deze instelling worden netwerkverbindingen toegestaan vanaf alle IP-adressen binnen het Azure-netwerk. Voor gebruik in productieomgevingen moet u proberen om de meest beperkende firewallregels te configureren door [alleen de uitgaande IP-adressen te gebruiken die uw app gebruikt](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

Voer in Cloud Shell de opdracht opnieuw uit om toegang vanaf uw lokale computer mogelijk te maken door *\<your_ip_address>* te vervangen door [uw lokale IPv4 IP-adres](http://www.whatsmyip.org/).

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>De Python-app verbinden met de productiedatabase

In deze stap verbindt u de Django-voorbeeld-app met de Azure Database for PostgreSQL-server die u hebt gemaakt.

### <a name="create-empty-database-and-user-access"></a>Lege database maken en gebruikerstoegang

Maak in Cloud Shell verbinding met de database door de onderstaande opdracht uit te voeren. Wanneer u wordt gevraagd om uw beheerderswachtwoord, gebruikt u hetzelfde wachtwoord dat u hebt opgegeven in [Een Azure Database for PostgreSQL-server maken](#create-an-azure-database-for-postgresql-server).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Net zoals bij uw lokale Postgres-server, maakt u de database en de gebruiker op de Azure-Postgres-server.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Typ `\q` om de PostgreSQL-client af te sluiten.

> [!NOTE]
> Het wordt aanbevolen om databasegebruikers te maken met beperkte machtigingen voor specifieke toepassingen, in plaats van de gebruiker met beheerdersrechten te gebruiken. In dit voorbeeld heeft de gebruiker `manager` volledige rechten voor _alleen_ de database `pollsdb`.

### <a name="test-app-connectivity-to-production-database"></a>De app-verbinding met de productiedatabase testen

Wijzig in het lokale terminalvenster de omgevingsvariabelen voor de database (die u eerder hebt geconfigureerd door *env.sh* of  *env.ps1* uit te voeren):

```bash
# Bash
export DBHOST="<postgresql_name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql_name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"

# PowerShell
$Env:DBHOST = "<postgresql_name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql_name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

Voer Django-migratie uit in de Azure-database en maak een gebruiker met beheerdersrechten.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Zodra de gebruiker met beheerdersrechten is gemaakt, voert u de Django-server uit.

```bash
python manage.py runserver
```

Navigeer opnieuw naar `http://localhost:8000`. Als het goed is, ziet u het bericht `No polls are available.` opnieuw. 

Navigeer naar `http://localhost:8000/admin` en meld u aan met de gebruiker met beheerdersrechten die u hebt gemaakt, en maak een poll-vraag zoals u eerder hebt gedaan.

![Lokaal uitgevoerde Python Django-toepassing](./media/tutorial-python-postgresql-app/django-admin-local.png)

Als u opnieuw naar `http://localhost:8000` navigeert ziet u de poll-vraag weergegeven. Er worden nu via de app gegevens naar de database in Azure geschreven.

## <a name="deploy-to-azure"></a>Implementeren in Azure

In deze step implementeert u de met Postgres verbonden Python-toepassing naar Azure App Service.

### <a name="configure-repository"></a>Opslagplaats configureren

In Django wordt de `HTTP_HOST`-header gevalideerd in binnenkomende aanvragen. U moet een Fully Qualified Domain Name voor de app toevoegen aan de toegestane hosts om ervoor te zorgen dat de Django-app werkt in App Service. Open _azuresite/settings.py_ en ga naar de `ALLOWED_HOSTS`-instelling. Wijzig de regel in:

```python
ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
```

Django biedt geen ondersteuning voor [het gebruiken van statische bestanden in een productieomgeving](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/). U moet dit dus handmatig inschakelen. Voor deze zelfstudie gebruikt u [WhiteNoise](https://whitenoise.evans.io/en/stable/). Het WhiteNoise-pakket is al opgenomen in _requirements.txt_. U hoeft alleen maar Django te configureren om dit pakket te gebruiken. 

Ga in _azuresite/settings.py_ naar de `MIDDLEWARE`-instelling en voeg de middleware `whitenoise.middleware.WhiteNoiseMiddleware` toe aan de lijst, net onder de middleware `django.middleware.security.SecurityMiddleware`. De `MIDDLEWARE`-instelling ziet er ongeveer als volgt uit:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
]
```

Voeg aan het eind van _azuresite/settings.py_ de volgende regels toe.

```python
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

Zie de [WhiteNoise-documentatie](https://whitenoise.evans.io/en/stable/) voor meer informatie over het configureren van WhiteNoise.

> [!IMPORTANT]
> In de sectie met database-instellingen worden de aanbevolen beveiligingsprocedures gevolgd voor het gebruik van omgevingsvariabelen. Zie de [Django-documentatie: controlelijst voor implementaties](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/) voor de volledige aanbevelingen voor implementaties.


Voer de wijzigingen door in de opslagplaats.

```bash
git commit -am "configure for App Service"
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
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 775 bytes | 0 bytes/s, done.
Total 7 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6520eeafcc'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Python deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
. 
. 
. 
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git 
   06b6df4..6520eea  master -> master
```  

_requirements.txt_ in de hoofdmap van de opslagplaats is zichtbaar voor de App Service-implementatieserver. Vervolgens wordt Python-pakketbeheer automatisch uitgevoerd na `git push`.

### <a name="browse-to-the-azure-app"></a>Naar de Azure-app bladeren

Blader naar de geïmplementeerde app. Het duurt even voordat de web-app wordt gestart omdat de container moet worden gedownload en uitgevoerd wanneer de app voor het eerst wordt aangevraagd. Als er een time-out optreedt op de pagina of als er een foutbericht wordt weergegeven, wacht u een paar minuten en vernieuwt u de pagina.

```bash
http://<app_name>.azurewebsites.net
```

Als het goed is, ziet u de poll-vraag die u eerder hebt gemaakt. 

In App Service wordt een Django-project gedetecteerd in de opslagplaats door in elke submap te zoeken naar een _wsgi.py_ die standaard wordt gemaakt met `manage.py startproject`. Zodra dit bestand is gevonden, wordt het in de Django-app geladen. Zie [Ingebouwde Python-installatiekopie configureren](how-to-configure-python.md) voor meer informatie over het laden van Python-apps in App Service.

Navigeer naar `<app_name>.azurewebsites.net` en meld u aan met de gebruiker met beheerdersrechten die u hebt gemaakt. Als u wilt, kunt u meer poll-vragen maken.

![Lokaal uitgevoerde Python Django-toepassing](./media/tutorial-python-postgresql-app/django-admin-azure.png)

**Gefeliciteerd!** U hebt een Python Flask-app gemaakt die wordt uitgevoerd in App Service voor Linux.

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

In App Service in Linux worden apps binnen een container uitgevoerd vanuit een standaard-Docker-installatiekopie. U hebt vanuit de container toegang tot de consolelogboeken. Als u deze logboeken wilt openen, schakelt u eerst logboekregistratie in door de volgende opdracht uit te voeren in Cloud Shell:

```azurecli-interactive
az webapp log config --name <app_name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Zodra logboekregistratie is ingeschakeld, voert u de volgende opdracht uit om de logboekstream te zien:

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
```

## <a name="manage-your-app-in-the-azure-portal"></a>De app beheren in de Azure-portal

Ga naar de [Azure-portal](https://portal.azure.com) om de app te zien die u hebt gemaakt.

Klik in het linkermenu op **App Services** en klik op de naam van uw Azure-app.

![Navigatie naar Azure-app in de portal](./media/tutorial-python-postgresql-app/app-resource.png)

In de portal wordt standaard de pagina **Overzicht** van de app weergegeven. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina tonen de verschillende configuratiepagina's die u kunt openen.

![App Service-pagina in Azure Portal](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een PostgreSQL-database maken in Azure
> * Een Python-app verbinden met PostgreSQL
> * De app implementeren in Azure
> * Diagnostische logboeken weergeven
> * De app in Azure Portal beheren

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure App Service](../app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Ingebouwde Python-installatiekopie configureren en fouten oplossen](how-to-configure-python.md)

