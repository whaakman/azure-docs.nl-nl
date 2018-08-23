---
title: Een Java- en MySQL-web-app bouwen in Azure
description: Informatie over het ophalen van een Java-app die is verbonden met de Azure MySQL-databaseservice die in Azure App Service werkt.
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 5e1d7a88e5a1a8ab60a01aea6ca42e850ac6e0e3
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "41918460"
---
# <a name="tutorial-build-a-java-and-mysql-web-app-in-azure"></a>Zelfstudie: een Java- en MySQL-web-app bouwen in Azure

> [!NOTE]
> In dit artikel gaat u een app implementeren in App Service onder Windows. Zie [Een beperkte Spring Boot-app implementeren in Azure](/java/azure/spring-framework/deploy-containerized-spring-boot-java-app-with-maven-plugin) voor de implementatie naar App Service op _Linux_.
>

In deze zelfstudie wordt getoond hoe u een Java-web-app in Azure maakt en deze verbinding laat maken met een MySQL-database. Wanneer u klaar bent, hebt u een [Spring Boot](https://projects.spring.io/spring-boot/)-toepassing die gegevens opslaat in [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) en wordt uitgevoerd op [Azure App Service Web Apps](app-service-web-overview.md).

![Java-app uitgevoerd in Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een MySQL-database in Azure maken
> * Een voorbeeld-app verbinden met de database
> * De app implementeren in Azure
> * De app bijwerken en opnieuw implementeren
> * Logboeken met diagnostische gegevens vanaf Azure streamen
> * De app in de Azure Portal controleren

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

1. [Download en installeer Git](https://git-scm.com/)
1. [Download en installeer de Java 7 JDK of hoger](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Download, installeer en start MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Lokale MySQL voorbereiden 

In deze stap maakt u een database in een lokale MySQL-server voor gebruik bij het lokaal testen van de app op uw computer.

### <a name="connect-to-mysql-server"></a>Verbinding maken met MySQL-server

Maak in een terminalvenster verbinding met uw lokale MySQL-server. U kunt dit terminalvenster gebruiken om alle opdrachten in deze zelfstudie uit te voeren.

```bash
mysql -u root -p
```

Als u wordt gevraagd om een wachtwoord, voert u het wachtwoord in voor het `root`-account. Als u het wachtwoord van uw rootaccount niet meer weet, bekijkt u [MySQL: het opnieuw instellen van het hoofdwachtwoord](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Als uw opdracht succesvol is uitgevoerd, wordt uw MySQL-server al uitgevoerd. Als dit niet het geval is, zorgt u ervoor dat de lokale MySQL-server is gestart door de [Stappen na installatie van MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) te volgen.

### <a name="create-a-database"></a>Een database maken 

Maak in de prompt `mysql` een database en een tabel voor de taakitems.

```sql
CREATE DATABASE tododb;
```

Sluit uw serververbinding door `quit` te typen.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>De voorbeeld-app maken en uitvoeren 

In deze stap kloont u een Spring-opstart-app, configureert u deze voor het gebruik van de lokale MySQL-database en voert u hem uit op uw computer. 

### <a name="clone-the-sample"></a>Het voorbeeld klonen

Navigeer naar een werkmap in het terminalvenster en kloon de voorbeeldopslagplaats. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>De app configureren voor het gebruik van de MySQL database

Werk de `spring.datasource.password` en de waarde in *spring-boot-mysql-todo/src/main/resources/application.properties* bij met hetzelfde rootwachtwoord dat is gebruikt om de MySQL-prompt te openen:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

Bouw het voorbeeld en voer het uit met de Maven-wrapper die is opgenomen in de opslagplaats:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Open `http://localhost:8080` in uw browser om het voorbeeld in actie te zien. Gebruik de volgende SQL-opdrachten in de MySQL-prompt om de gegevens  weer te geven die zijn opgeslagen in de MySQL-prompt wanneer u taken aan de lijst toevoegt.

```SQL
use testdb;
select * from todo_item;
```

Stop de toepassing door `Ctrl`+`C` te gebruiken in de terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Een Azure MySQL-database maken

In deze stap maakt u een [Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md)-instantie met de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). U configureert de voorbeeldtoepassing later in de zelfstudie om deze database te gebruiken.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [`az group create`](/cli/azure/group#az-group-create). Een Azure-resourcegroep is een logische container waarin verwante resources, zoals web-apps, databases en opslagaccounts, worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio Europa - noord:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

Gebruik de opdracht [`az appservice list-locations`](/cli/azure/appservice#list-locations) om te zien welke waarden u kunt gebruiken voor `--location`.

### <a name="create-a-mysql-server"></a>Een MySQL-server maken

Maak vanuit Cloud Shell een server in Azure Database for MySQL met behulp van de opdracht [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az-mysql-server-create).

Vervang in de volgende opdracht de tijdelijke aanduiding *\<mysql_server_name>* door een unieke servernaam, de tijdelijke aanduiding *\<admin_user>* door een gebruikersnaam, en de tijdelijke aanduiding *\<admin_password>* door een wachtwoord. De servernaam wordt gebruikt als onderdeel van het PostgreSQL-eindpunt (`https://<mysql_server_name>.mysql.database.azure.com`). De naam moet dus uniek zijn voor alle servers in Azure.

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql_server_name>--location "West Europe" --admin-user <admin_user> --admin-password <server_admin_password> --sku-name GP_Gen4_2
```

> [!NOTE]
> Aangezien er in deze zelfstudie verschillende referenties aan de orde komen, zijn `--admin-user` en `--admin-password` ingesteld op dummy-waarden om verwarring te voorkomen. Volg in een productieomgeving de aanbevolen beveiligingsprocedures bij het kiezen van een goede gebruikersnaam en goed wachtwoord voor uw MySQL-server in Azure.
>
>

Wanneer de MySQL-server is gemaakt, toont de Azure CLI informatie die lijkt op de informatie in het volgende voorbeeld:

```json
{
  "location": "westeurope",
  "name": "<mysql_server_name>",
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

Maak in de Cloud Shell een firewallregel voor uw MySQL-server om clientverbindingen toe te staan met behulp van de opdracht [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create). Als zowel het IP-beginadres als het IP-eindadres zijn ingesteld op 0.0.0.0, wordt de firewall alleen geopend voor andere Azure-resources. 

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP] 
> U kunt uw firewallregel nog beperkender maken door [alleen de uitgaande IP-adressen te gebruiken die in uw app worden gebruikt](app-service-ip-addresses.md#find-outbound-ips).
>

## <a name="configure-the-azure-mysql-database"></a>De Azure MySQL-database configureren

Maak in het lokale terminalvenster verbinding met de MySQL-server in Azure. Gebruik de waarde die u eerder hebt opgegeven voor _&lt;mysql_server_name>_. Wanneer u wordt gevraagd om een wachtwoord, gebruikt u het wachtwoord dat u hebt opgegeven bij het maken van de database in Azure.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Een database maken 

Maak in de prompt `mysql` een database en een tabel voor de taakitems.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Een gebruiker met machtigingen maken

Maak een databasegebruiker en geef deze alle bevoegdheden in de `tododb`-database. Vervang de tijdelijke aanduidingen `<Javaapp_user>` en `<Javaapp_password>` met uw eigen unieke app-naam.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Sluit uw serververbinding door `quit` te typen.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Het voorbeeld naar Azure App Service implementeren

Maak een Azure App Service-plan met de prijscategorie **GRATIS** met behulp van de CLI-opdracht [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create). Het App Service-plan definieert de fysieke resources die gebruikt worden voor het hosten van uw apps. Alle toepassingen die zijn toegewezen aan een App Service-plan delen deze resources, zodat u kosten kunt besparen als u meerdere apps host. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Wanneer het plan klaar is, laat de Azure CLI uitvoer zien die vergelijkbaar is met het volgende voorbeeld:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Een Azure-web-app maken

Gebruik in de Cloud Shell de CLI-opdracht [`az webapp create`](/cli/azure/webapp#az-webapp-create) voor het maken van de definitie van een web-app in het `myAppServicePlan` App Service-plan. De definitie van de web-app biedt een URL voor toegang tot uw toepassing en configureert diverse opties voor het implementeren van uw code naar Azure. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Vervang de tijdelijke aanduiding `<app_name>` met uw eigen unieke app-naam. Deze naam maakt deel uit van de standaarddomeinnaam voor de web-app, daarom moet de naam uniek zijn voor alle apps in Azure. U kunt een aangepaste domeinnaam toewijzen aan de web-app voordat u deze beschikbaar maakt voor uw gebruikers.

Wanneer de definitie van de web-app is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld: 

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

### <a name="configure-java"></a>Java configureren 

Stel in de Cloud Shell de Java-runtime-configuratie in die uw app nodig heeft met de opdracht [`az webapp config set`](/cli/azure/webapp/config#az-webapp-config-set).

De volgende opdracht configureert de web-app om op een recente Java 8 JDK en [Apache Tomcat](http://tomcat.apache.org/) 8.0 te worden uitgevoerd.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>De app voor het gebruik van de Azure SQL database configureren

Voordat u de voorbeeld-app uitvoert, stelt u toepassingsinstellingen op de web-app in om de Azure MySQL-database te gebruiken die u in Azure hebt gemaakt. Deze eigenschappen worden als omgevingsvariabelen blootgesteld aan de webtoepassing en overschrijven de waarden in de application.properties binnen de verpakte web-app. 

Stel in de Cloud Shell de toepassingsinstellingen in met [`az webapp config appsettings`](https://docs.microsoft.com/cli/azure/webapp/config/appsettings) in de CLI:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_PASSWORD=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>FTP-implementatiereferenties verkrijgen 
U kunt uw web-app op verschillende manieren in Azure App Service implementeren, zoals met FTP, lokale Git, GitHub, Visual Studio Team Services en Bitbucket. In dit voorbeeld gebruikt u FTP voor het implementeren van het .WAR-bestand dat eerder op uw lokale machine in Azure App Service is gebouwd.

Gebruik de opdracht [`az appservice web deployment list-publishing-profiles`](https://docs.microsoft.com/cli/azure/webapp/deployment#az-appservice-web-deployment-list-publishing-profiles) in de Cloud Shell om te bepalen welke referenties u door moet geven in een FTP-opdracht in de web-app: 

```azurecli-interactive
az webapp deployment list-publishing-profiles --name <app_name> --resource-group myResourceGroup --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a>De app uploaden met FTP

Gebruik uw favoriete FTP-hulpprogramma voor het implementeren van het .WAR-bestand in de map */site/wwwroot/webapps* op het adres van de server dat afkomstig is uit het veld `URL` in de vorige opdracht. Verwijder de bestaande de standaard (ROOT)-toepassingsmap en vervang de bestaande ROOT.war met het .WAR-bestand dat in de eerder in de zelfstudie is gebouwd.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a>De web-app testen

Blader naar `http://<app_name>.azurewebsites.net/` en voeg een paar taken toe aan de lijst. 

![Java-app uitgevoerd in Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Gefeliciteerd!** U voert een gegevensgestuurde Java-app uit in Azure App Service.

## <a name="update-the-app-and-redeploy"></a>De app bijwerken en opnieuw implementeren

Werk de toepassing bij om een extra kolom in de takenlijst op te nemen, voor op welke dag het item is gemaakt. Spring Boot verwerkt het bijwerken van het databaseschema voor u, omdat het gegevensmodel verandert zonder uw bestaande databaserecords te wijzigen.

1. Open op uw lokale systeem *src/main/java/com/example/fabrikam/TodoItem.java* en voeg de volgende importen toe aan de klasse:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Voeg een `String`-eigenschap `timeCreated` toe aan *src/main/java/com/example/fabrikam/TodoItem.java* om deze te initialiseren met een tijdstempel bij het maken van een object. Voeg getters/setters toe voor de nieuwe eigenschap `timeCreated` tijdens het bewerken van dit bestand.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Werk *src/main/java/com/example/fabrikam/TodoDemoController.java* bij met een regel in de methode `updateTodo` om de tijdstempel in te stellen:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Voeg ondersteuning toe voor het nieuwe veld in het sjabloon `Thymeleaf`. Werk *src/main/resources/templates/index.html* bij met een nieuwe tabelheader voor de tijdstempel en een nieuw veld om de waarde van de tijdstempel in elke gegevensrij van de tabel weer te geven.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Herbouw de toepassing:

    ```bash
    mvnw clean package 
    ```

6. FTP de bijgewerkte .WAR als voorheen, verwijder de bestaande map *site/wwwroot/webapps/ROOT* en *ROOT.war*, en upload vervolgens het bijgewerkte WAR-bestand als ROOT.war. 

Bij het vernieuwen van de app is nu de kolom **Aanmaaktijd** zichtbaar. Wanneer u een nieuwe taak toevoegt, vult de app automatisch de tijdstempel. Uw bestaande taken blijven ongewijzigd en werken met de app, zelfs als het onderliggende gegevensmodel is gewijzigd. 

![Java-app bijgewerkt met een nieuwe kolom](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Diagnostische logboeken streamen 

Terwijl uw Java-toepassing in Azure App Service wordt uitgevoerd, kunt u de consolelogboeken doorgesluisd krijgen naar uw terminal. Op die manier krijgt u de dezelfde diagnostische berichten om toepassingsfouten op te sporen.

Gebruik voor het starten van logboekstreaming de opdracht [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail) in de Cloud Shell.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Uw Azure-web-app beheren

Ga naar [Azure Portal](https://portal.azure.com) om de web-app te zien die u hebt gemaakt.

Klik vanuit het linkermenu op **App Service** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Standaard toont de pagina van uw web-app de pagina **Overzicht**. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina tonen de verschillende configuratiepagina's die u kunt openen.

![App Service-pagina in Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Deze tabbladen op de pagina bevatten de vele handige functies die u kunt toevoegen aan uw web-app. De volgende lijst bevat slechts enkele van de mogelijkheden:
* Een aangepaste DNS-naam toewijzen
* Een aangepast SSL-certificaat binden
* Continue implementatie inschakelen
* Omhoog en omlaag schalen
* Gebruikersverificatie toevoegen

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet voor een andere zelfstudie nodig hebt (zie [Volgende stappen](#next)), kunt u ze verwijderen door de volgende opdracht in de Cloud Shell uit te voeren: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Volgende stappen

> [!div class="checklist"]
> * Een MySQL-database in Azure maken
> * Een voorbeeld Java-app verbinden met de MySQL
> * De app implementeren in Azure
> * De app bijwerken en opnieuw implementeren
> * Logboeken met diagnostische gegevens vanaf Azure streamen
> * De app in Azure Portal beheren

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan een web-app kunt toewijzen.

> [!div class="nextstepaction"] 
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md)
