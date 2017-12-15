---
title: Een Java- en MySQL web-app in Azure bouwen
description: Informatie over het ophalen van een Java-app die is verbonden met de Azure MySQL-database-service in Azure App service werkt.
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: ad53575b655ebec5a134c8d76b963708caf14334
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Een Java- en MySQL web-app in Azure bouwen

> [!NOTE]
> In dit artikel implementeert een app in App Service in Windows. Voor de implementatie in App Service _Linux_, Zie [een beperkte Spring Boot-app implementeren in Azure](/java/azure/spring-framework/deploy-containerized-spring-boot-java-app-with-maven-plugin).
>

Deze zelfstudie laat zien hoe u een Java-web-app in Azure maken en te verbinden met een MySQL-database. Wanneer u klaar bent, hebt u een [Spring Boot](https://projects.spring.io/spring-boot/) toepassing opslaan van gegevens in [Azure Database voor MySQL](https://docs.microsoft.com/azure/mysql/overview) uitgevoerd op [Azure App Service Web Apps](app-service-web-overview.md).

![Java-app uitgevoerd in Azure App service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een MySQL-database maken in Azure
> * Een voorbeeld-app verbinden met de database
> * De app implementeren in Azure
> * De app bijwerken en opnieuw implementeren
> * Diagnostische logboeken van de stroom van Azure
> * De app in de Azure portal controleren


## <a name="prerequisites"></a>Vereisten

1. [Download en installeer Git](https://git-scm.com/)
1. [Download en installeer de JDK die Java 7 of hoger](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Downloaden, installeren en starten van MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Lokale MySQL voorbereiden 

In deze stap maakt u een database in een lokale MySQL-server voor gebruik in de app lokaal op uw computer te testen.

### <a name="connect-to-mysql-server"></a>Verbinding maken met de MySQL-server

In een terminalvenster verbinding maken met uw lokale MySQL-server. Alle opdrachten kunt uitvoeren in deze zelfstudie kunt u deze terminalvenster.

```bash
mysql -u root -p
```

Als u wordt gevraagd om een wachtwoord, voert u het wachtwoord voor de `root` account. Als je het wachtwoord van je root-account, Zie [MySQL: het opnieuw instellen van het hoofdwachtwoord](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Als uw opdracht is uitgevoerd, wordt al uw MySQL-server uitgevoerd. Als dit niet het geval is, zorg ervoor dat de lokale MySQL-server is gestart door de [MySQL na de installatie stappen](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database"></a>Een database maken 

In de `mysql` vragen, maakt u een database en een tabel voor de taakitems.

```sql
CREATE DATABASE tododb;
```

Uw serververbinding sluiten door te typen `quit`.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Maken en uitvoeren van de voorbeeld-app 

In deze stap voorbeeldapp Spring opstarten klonen, configureert voor het gebruik van de lokale MySQL-database en uitvoeren op uw computer. 

### <a name="clone-the-sample"></a>Klonen van de steekproef

Navigeer naar een werkmap in een terminalvenster en kloon de opslagplaats voorbeeld. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>De app voor het gebruik van de MySQL-database configureren

Update de `spring.datasource.password` en de waarde *spring-boot-mysql-todo/src/main/resources/application.properties* met hetzelfde root-wachtwoord gebruikt om de MySQL-prompt te openen:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Bouwen en uitvoeren van de steekproef

Bouwen en uitvoeren van het voorbeeld met de Maven-wrapper die is opgenomen in de opslagplaats:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Open uw browser naar `http://localhost:8080` om te zien in het voorbeeld in te grijpen. Zoals u taken aan de lijst toevoegen, gebruikt u de volgende SQL-opdrachten in de MySQL-prompt om de gegevens die zijn opgeslagen in de MySQL weer te geven.

```SQL
use testdb;
select * from todo_item;
```

Stop de toepassing te raken `Ctrl` + `C` in de terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Een Azure-MySQL-database maken

In deze stap maakt u een [Azure Database voor MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instantie met de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). U configureert de voorbeeldtoepassing later op deze database gebruiken in de zelfstudie.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [resourcegroep](../azure-resource-manager/resource-group-overview.md) met de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container waar verwante resources, zoals web-apps, databases en storage-accounts worden geïmplementeerd en beheerd. 

Het volgende voorbeeld maakt een resourcegroep in de regio Noord-Europa:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

Om te zien van de mogelijke waarden die u hebt, kunnen gebruiken voor `--location`, gebruiken de [az appservice lijst-locaties](/cli/azure/appservice#list-locations) opdracht.

### <a name="create-a-mysql-server"></a>Een MySQL-server maken

In de Cloud-Shell, maakt u een server in Azure-Database voor MySQL (Preview) met de [az mysql-server maken](/cli/azure/mysql/server#create) opdracht.    
Vervangen door uw eigen unieke MySQL-servernaam waarin u zien hoe de `<mysql_server_name>` tijdelijke aanduiding. Deze naam maakt deel uit van de hostnaam van uw MySQL-server, `<mysql_server_name>.mysql.database.azure.com`, zodat het moet globaal uniek zijn. Ook vervangen `<admin_user>` en `<admin_password>` met uw eigen waarden.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user <admin_user> --admin-password <admin_password>
```

Wanneer de MySQL-server is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Een firewall configureren

In de Cloud-Shell, maakt u een firewallregel voor uw MySQL-server clientverbindingen toestaat met behulp van de [az mysql server-firewallregel maken](/cli/azure/mysql/server/firewall-rule#create) opdracht. 

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure-Database voor MySQL (Preview) kunnen niet automatisch momenteel verbindingen van Azure-services. Als het IP-adressen in Azure worden dynamisch toegewezen, is het beter om alle IP-adressen inschakelen voor nu. Wanneer de service de preview blijft, kunt u betere methoden voor het beveiligen van uw database wordt ingeschakeld.

## <a name="configure-the-azure-mysql-database"></a>De Azure MySQL-database configureren

In het lokale terminalvenster verbinding maken met de server MySQL in Azure. Gebruik de waarde die u eerder hebt opgegeven voor `<admin_user>` en `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Een database maken 

In de `mysql` vragen, maakt u een database en een tabel voor de taakitems.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Een gebruiker met machtigingen maken

Een databasegebruiker maken en hieraan alle bevoegdheden de `tododb` database. Vervang de tijdelijke aanduidingen `<Javaapp_user>` en `<Javaapp_password>` met uw eigen unieke app-naam.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Uw serververbinding sluiten door te typen `quit`.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Het voorbeeld in Azure App Service implementeren

Maken van een Azure App Service-abonnement met de **vrije** prijzen met behulp van laag de [az appservice-abonnement maken](/cli/azure/appservice/plan#create) CLI-opdracht. Het plan appservice definieert de fysieke resources gebruikt voor het hosten van uw apps. Alle toepassingen die zijn toegewezen aan een appservice-abonnement kunt u deze resources, zodat u kosten opslaan bij het hosten van meerdere apps delen. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Wanneer het plan klaar is, ziet u de Azure CLI vergelijkbare uitvoer naar het volgende voorbeeld:

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

### <a name="create-an-azure-web-app"></a>Een Azure-Web-app maken

 In de Cloud-Shell gebruiken de [az webapp maken](/cli/azure/appservice/web#create) CLI-opdracht voor het maken van de definitie van een web-app in de `myAppServicePlan` App Service-abonnement. De definitie van de web-app biedt een URL voor toegang tot uw toepassing met en configureert u diverse opties voor het implementeren van uw code naar Azure. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Vervang de `<app_name>` aanduiding voor items met uw eigen unieke app-naam. Deze unieke naam maakt deel uit van de standaard-domeinnaam voor de web-app zodat de naam moet uniek zijn in alle apps in Azure. Voordat u het beschikbaar aan uw gebruikers stellen, kunt u een vermelding van de naam van aangepast domein toewijzen aan de web-app.

Wanneer de definitie van de web-app klaar is, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld: 

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

In de Cloud-Shell de Java runtime configuratie instellen dat uw app met moet de [az appservice web config update](/cli/azure/appservice/web/config#update) opdracht.

De volgende opdracht configureert u de web-app uit te voeren op een recente Java 8 JDK en [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>De app voor het gebruik van de Azure SQL database configureren

Voordat u de voorbeeld-app, stelt u toepassingsinstellingen op de web-app de Azure-MySQL-database die u hebt gemaakt in Azure gebruiken. Deze eigenschappen worden blootgesteld aan de webtoepassing als omgevingsvariabelen en de waarden in de application.properties binnen de verpakte web-app overschreven. 

In de Cloud-Shell toepassingsinstellingen met [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) in de CLI:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_PASSWORD=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>FTP-implementatiereferenties ophalen 
U kunt uw toepassing in Azure App service op verschillende manieren, met inbegrip van de FTP-, lokale Git, GitHub, Visual Studio Team Services en BitBucket implementeren. In dit voorbeeld FTP voor het implementeren van de. WAR-bestand is gebouwd eerder op uw lokale machine in Azure App Service.

Gebruiken om te bepalen wat referenties door te geven in een FTP-opdracht in de Web-App, [implementatie in az appservice web-lijst-publicatie-profielen](https://docs.microsoft.com/cli/azure/appservice/web/deployment#az_appservice_web_deployment_list_publishing_profiles) opdracht in de Cloud-Shell: 

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

### <a name="upload-the-app-using-ftp"></a>Uploaden van de app met FTP

Uw favoriete FTP-hulpprogramma gebruiken voor het implementeren van de. WAR-bestand in de */site/wwwroot/webapps* map op het adres van de server die afkomstig zijn uit de `URL` veld in de vorige opdracht. Verwijder de bestaande map van de standaard (ROOT)-toepassing en vervang de bestaande ROOT.war met de. WAR-bestand in de eerder in de zelfstudie wordt gebouwd.

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

Blader naar `http://<app_name>.azurewebsites.net/` en een paar taken toevoegen aan de lijst. 

![Java-app uitgevoerd in Azure App service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Gefeliciteerd!** U uitvoert een Java-gegevensgestuurde app in Azure App Service.

## <a name="update-the-app-and-redeploy"></a>De app bijwerken en opnieuw implementeren

Bijwerken van de toepassing te nemen van een extra kolom in de takenlijst voor welke dag het item is gemaakt. Spring opstarten verwerkt het databaseschema bijwerken voor u als de wijzigingen in het gegevensmodel zonder uw bestaande databaserecords zelf te wijzigen.

1. Open op uw lokale systeem *src/main/java/com/example/fabrikam/TodoItem.java* en voeg de volgende import toe aan de klasse:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Voeg een `String` eigenschap `timeCreated` naar *src/main/java/com/example/fabrikam/TodoItem.java*, deze met een tijdstempel bij het maken van een object te initialiseren. Getters/setters toevoegen voor de nieuwe `timeCreated` eigenschap tijdens het bewerken van dit bestand.

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

3. Update *src/main/java/com/example/fabrikam/TodoDemoController.java* met een regel in de `updateTodo` methode om in te stellen de tijdstempel:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Ondersteuning voor het nieuwe veld in de sjabloon Thymeleaf toevoegen. Update *src/main/resources/templates/index.html* met een nieuwe tabel-header voor het tijdstempel en een nieuw veld om de waarde van het tijdstempel in elke tabelrij gegevens weer te geven.

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

5. De toepassing bouwen:

    ```bash
    mvnw clean package 
    ```

6. FTP-de update. WAR als voorheen kunt u verwijdert de bestaande *site/wwwroot/webapps/ROOT* directory en *ROOT.war*, en vervolgens de bijgewerkte uploaden. WAR-bestand als ROOT.war. 

Bij het vernieuwen van de app een **Aanmaaktijd** kolom nu zichtbaar is. Wanneer u een nieuwe taak toevoegt, wordt de app de tijdstempel automatisch gevuld. Uw bestaande taken blijven ongewijzigd en werken met de app, zelfs als het onderliggende gegevensmodel is gewijzigd. 

![Java-app bijgewerkt met een nieuwe kolom](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Diagnostische logboeken 

Als uw Java-toepassing in Azure App Service wordt uitgevoerd, kunt u de logboeken van de console doorgesluisd rechtstreeks aan uw terminal opvragen. Op die manier kunt u de dezelfde diagnostische berichten op te sporen toepassingsfouten ophalen.

Gebruik voor het starten van de streaming-logboek de [az webapp logboek tail](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) opdracht in de Cloud-Shell.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Uw Azure-web-app beheren

Ga naar de Azure-portal om te zien van de web-app die u hebt gemaakt.

Hiervoor moet u zich aanmelden bij [https://portal.azure.com](https://portal.azure.com).

Klik vanuit het linkermenu op **App Service** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Standaard toont de blade van uw web-app de pagina **Overzicht**. Deze pagina geeft u een overzicht van hoe uw app presteert. U kunt hier ook beheertaken zoals stoppen, starten, opnieuw opstarten en verwijderen uitvoeren. De tabbladen aan de linkerkant van de blade tonen de verschillende configuratiepagina's die u kunt openen.

![App Service-blade in Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Deze tabbladen op de blade bevatten de vele handige functies die kunt u toevoegen aan uw web-app. De volgende lijst bevat slechts enkele van de mogelijkheden:
* Een aangepaste DNS-naam toewijzen
* Een aangepast SSL-certificaat binden
* Continue implementatie inschakelen
* Omhoog en omlaag schalen
* Gebruikersverificatie toevoegen

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet voor een andere zelfstudie hoeft (Zie [Vervolgstappen](#next)), kunt u ze verwijderen met de volgende opdracht in de Cloud-Shell: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Volgende stappen

> [!div class="checklist"]
> * Een MySQL-database maken in Azure
> * Een voorbeeld Java-app verbinden met de MySQL
> * De app implementeren in Azure
> * De app bijwerken en opnieuw implementeren
> * Diagnostische logboeken van de stroom van Azure
> * De app in de Azure portal beheren

Ga naar de volgende zelfstudie voor informatie over het toewijzen van een aangepaste DNS-naam naar de app.

> [!div class="nextstepaction"] 
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md)
