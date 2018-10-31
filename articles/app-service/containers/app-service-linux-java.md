---
title: Ondersteuning voor Java voor Azure App Service on Linux | Microsoft Docs
description: Developer's guide voor het implementeren van Java-apps met Azure App Service on Linux.
keywords: Azure appservice, web-app, linux, oss, java
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/29/2018
ms.author: routlaw
ms.openlocfilehash: 1639194741fb343391f80790d0a2cc7f392218ac
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238112"
---
# <a name="java-developers-guide-for-app-service-on-linux"></a>Java developer's guide for App Service op Linux

Azure App Service on Linux kunt Java-ontwikkelaars voor het snel bouwen, implementeren en schalen van hun Tomcat of Standard-editie van Java (SE) verpakt in een volledig beheerde service voor Linux-gebaseerde web-apps. Implementeer toepassingen in Maven-invoegtoepassingen vanaf de opdrachtregel of in een editor, zoals IntelliJ, Eclipse of Visual Studio Code.

Deze handleiding bevat de belangrijkste concepten en instructies voor het Java-ontwikkelaars voor Linux in App Service gebruiken. Als u Azure App Service voor Linux nooit hebt gebruikt, dient u te lezen via de [Java-quickstart](quickstart-java.md) eerste. Algemene vragen over het gebruik van App Service voor Linux die niet specifiek zijn voor de Java-ontwikkeling worden beantwoord het [Veelgestelde vragen over App Service Linux](app-service-linux-faq.md).

## <a name="logging-and-debugging-apps"></a>Logboekregistratie en foutopsporing van apps

Rapporten, verkeer visualisaties en de gezondheid van controles zijn beschikbaar voor eeach app via de Azure-portal. Zie de [overzicht van Azure App Service-diagnostics](/azure/app-service/app-service-diagnostics) voor meer informatie over hoe u toegang tot en gebruik deze diagnostische hulpprogramma's.

### <a name="ssh-console-access"></a>Toegang tot de SSH-console 

SSH-verbinding met het Linux-omgeving uitvoeren van uw app is beschikbaar. Zie [SSH-ondersteuning voor Azure App Service on Linux](/azure/app-service/containers/app-service-linux-ssh-support) voor volledige instructies voor het verbinding maken met het Linux-systeem via uw webbrowser of de lokale terminal.

### <a name="streaming-logs"></a>Streaminglogboeken

Voor het snel opsporen en oplossen, kunt u Logboeken streamen naar de console met de Azure CLI. Configureren van de CLI met de `az webapp log config` logboekregistratie inschakelen:

```azurecli-interactive
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
 --web-server-logging filesystem
```

Vervolgens stream logboeken naar uw console met `az webapp log tail`:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```

Zie voor meer informatie, [Streaminglogboeken met de Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#streaming-with-azure-command-line-interface).

### <a name="app-logging"></a>App-registratie

Schakel [toepassingslogboeken](/azure/app-service/web-sites-enable-diagnostic-log#enablediag) via Azure portal of [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) App Service configureren voor de standard-console-uitvoer en foutstromen standard-console van uw toepassing schrijven naar de lokale bestandssysteem of Azure Blob Storage. Logboekregistratie voor het lokale bestandssysteem voor App Service exemplaar uitgeschakeld 12 uur nadat deze is geconfigureerd. Als u langere bewaartermijn nodig hebt, configureert u de toepassing naar uitvoer schrijven naar een Blob storage-container.

Als uw toepassing gebruikmaakt van [Logback](https://logback.qos.ch/) of [Log4j](https://logging.apache.org/log4j) voor tracering, kunt u doorsturen deze traceringen ter beoordeling naar Azure Application Insights met behulp van de logboekregistratie framework configuratie-instructies in [Traceerlogboeken in Application Insights Java verkennen](/azure/application-insights/app-insights-java-trace-logs). 

## <a name="customization-and-tuning"></a>Aanpassing en afstemmen

Azure App Service voor Linux ondersteunt buiten het vak afstemmen en aanpassen via de Azure Portal en de CLI. Raadpleeg de volgende artikelen voor de configuratie van specifieke niet-Java-web-app:

- [App Service-instellingen configureren](/azure/app-service/web-sites-configure?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Een aangepast domein instellen](/azure/app-service/app-service-web-tutorial-custom-domain?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [SSL inschakelen](/azure/app-service/app-service-web-tutorial-custom-ssl?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Een CDN toevoegen](/azure/cdn/cdn-add-to-web-app?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)

### <a name="set-java-runtime-options"></a>Java runtime-opties instellen

Toegewezen geheugen of andere JVM-runtime-opties instellen in de Tomcat- en Java SE-omgevingen, stelt u de JAVA_OPTS zoals hieronder wordt weergegeven als een [toepassingsinstelling](/azure/app-service/web-sites-configure#app-settings). App Service Linux geeft deze instelling als een omgevingsvariabele aan de Java-runtime wanneer deze wordt gestart.

In de Azure-portal onder **toepassingsinstellingen** voor de web-app, maakt u een nieuwe appinstelling met de naam `JAVA_OPTS` die bevat de aanvullende instellingen, zoals `$JAVA_OPTS -Xms512m -Xmx1204m`.

Voor het configureren van de app-instelling van de Azure App Service Linux Maven-invoegtoepassing toevoegen/waarde-codes in de sectie Azure-invoegtoepassing. Het volgende voorbeeld wordt een specifieke minimale en maximale Java heapsize:

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Xms512m -Xmx1204m</value> 
    </property> 
</appSettings> 
```

Ontwikkelaars van één toepassing uitvoert met een implementatiesite in hun App Service-plan kunnen de volgende opties gebruiken:

- Instanties van B1 en S1:-Xms1024m-Xmx1024m
- Instanties van B2 en S2:-Xms3072m-Xmx3072m
- B3 en S3-instanties:-Xms6144m-Xmx6144m


Wanneer de instellingen voor de heap afstemmen, bekijk de details van uw App Service-plan en rekening gehouden met meerdere toepassingen en implementatiesite moeten vinden van de optimale toewijzing van geheugen.

### <a name="turn-on-web-sockets"></a>Websockets inschakelen

Inschakelen van ondersteuning voor websockets in de Azure portal in de **toepassingsinstellingen** voor de toepassing. U moet de toepassing voor de instelling van kracht opnieuw hebt gestart.

Schakel web socket-ondersteuning met behulp van de Azure CLI met de volgende opdracht:

```azurecli-interactive
az webapp config set -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} --web-sockets-enabled true 
```

Start uw toepassing:

```azurecli-interactive
az webapp stop -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} 
az webapp start -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME}  
```

### <a name="set-default-character-encoding"></a>Standaard-tekencodering instellen 

In de Azure-portal onder **toepassingsinstellingen** voor de web-app, maakt u een nieuwe appinstelling met de naam `JAVA_OPTS` met waarde `$JAVA_OPTS -Dfile.encoding=UTF-8`.

U kunt ook de app-instelling met behulp van de App Service-Maven-invoegtoepassing configureren. De instelling naam / waarde-tags toevoegen in de configuratie van de invoegtoepassing: 

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Dfile.encoding=UTF-8</value> 
    </property> 
</appSettings> 
```

## <a name="secure-application"></a>Beveiligde toepassingen

Java-toepassingen die worden uitgevoerd in App Service for Linux hebben dezelfde set [best practices voor beveiliging](/azure/security/security-paas-applications-using-app-services) als andere toepassingen. 

### <a name="authenticate-users"></a>Gebruikers verifiëren

Instellen van app-verificatie in de Azure Portal met de **verificatie en autorisatie** optie. Van daaruit kunt u verificatie met behulp van Azure Active Directory of sociale aanmeldingen als Facebook, Google of GitHub. Configuratie van Azure portal werkt alleen bij het configureren van een enkele verificatieprovider.  Zie voor meer informatie, [configureren van uw App Service-app voor het gebruik van Azure Active Directory-aanmelding](/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication) en de bijbehorende artikelen voor andere id-providers.

Als u nodig hebt om in te schakelen van meerdere providers voor aanmelden, volg de instructies in de [App Service-verificatie aanpassen](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to) artikel.

 Spring Boot-ontwikkelaars kunnen gebruikmaken van de [Azure Active Directory Spring Boot starter](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) voor het beveiligen van toepassingen met bekende Spring Security aantekeningen en API's.

### <a name="configure-tlsssl"></a>TLS/SSL configureren

Volg de instructies in de [een bestaand aangepast SSL-certificaat binden](/azure/app-service/app-service-web-tutorial-custom-ssl) voor het uploaden van een bestaand SSL-certificaat en bindt dit aan de domeinnaam van uw toepassing. Standaard worden uw toepassing nog steeds kunt HTTP-verbindingen-Volg de specifieke stappen in de zelfstudie voor het afdwingen van SSL en TLS.

## <a name="tomcat"></a>Tomcat 

### <a name="connecting-to-data-sources"></a>Verbinding maken met gegevensbronnen

>[!NOTE]
> Als uw toepassing gebruikmaakt van de Spring-Framework of Spring Boot, kunt u de database-verbindingsgegevens voor de Spring gegevens JPA als omgevingsvariabelen instellen [in uw toepassing eigenschappenbestand]. Gebruik vervolgens [app-instellingen](/azure/app-service/web-sites-configure#app-settings) voor het definiëren van deze waarden voor uw toepassing in de Azure portal of de CLI.

De voorbeeld-configuratie-fragmenten in deze sectie gebruiken MySQL-database. Zie voor meer informatie, de configuratie van documenten voor [MySQL](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-usagenotes-tomcat.html) , [SQL Server JDBC](https://docs.microsoft.com/en-us/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server?view=sql-server-2017), en [PostgreSQL](https://jdbc.postgresql.org/documentation/head/index.html).

Voor het configureren van Tomcat voor het gebruik van beheerde verbindingen met databases met behulp van Java-Database Connectivity (JDBC) of de Java-persistentie API (JPA), moet u eerst de CATALINA_OPTS omgevingsvariabele gelezen door Tomcat bij het opstarten aanpassen. Stel deze waarden via een app-instelling in App Service-Maven-invoegtoepassing:

```xml
<appSettings> 
    <property> 
        <name>CATALINA_OPTS</name> 
        <value>"$CATALINA_OPTS -Dmysqluser=${mysqluser} -Dmysqlpass=${mysqlpass} -DmysqlURL=${mysqlURL}"</value> 
    </property> 
</appSettings> 
```

Of een equivalente App-Service instellen vanuit Azure portal.

Bepaal vervolgens of de gegevensbron moet beschikbaar zijn gesteld slechts aan één toepassing of aan alle toepassingen die worden uitgevoerd op de App Service-plan.

Voor gegevensbronnen op toepassingsniveau: 

1. Voeg een `context.xml` als deze niet bestaat in uw webtoepassing en toe te voegen de `META-INF` map van het WAR-bestand wanneer het project wordt gemaakt.

2. In dit bestand voegt een `Context` padvermelding voor de gegevensbron een koppeling naar een adres JNDI. de

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Bijwerken van uw toepassing `web.xml` naar de gegevensbron in uw toepassing gebruiken.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

Voor gedeelde bronnen op serverniveau:

1. Kopieer de inhoud van `/usr/local/tomcat/conf` in `/home/tomcat` exemplaar met behulp van SSH als u nog niet een configuratie er hebt in uw App Service Linux.

2. Toevoegen van de context aan uw `server.xml`

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Bijwerken van uw toepassing `web.xml` naar de gegevensbron in uw toepassing gebruiken.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

4. Zorg ervoor dat de JDBC-stuurprogramma's beschikbaar voor de Tomcat-classloader zijn door ze in te plaatsen de `/home/tomcat/lib` directory. Als u wilt deze bestanden uploaden naar uw App Service-exemplaar, kunt u de volgende stappen uitvoeren:  
    1. De uitbreiding voor de Azure App Service-webpp installeren:

      ```azurecli-interactive
      az extension add –name webapp
      ```

    2. Voer de volgende CLI-opdracht voor het maken van een SSH-tunnel vanuit uw lokale systeem naar App Service:

      ```azurecli-interactive
      az webapp remote-connection create –g [resource group] -n [app name] -p [local port to open]
      ```

    3. Verbinding maken met de lokale tunneling poort met uw SFTP-client en de bestanden te uploaden de `/home/tomcat/lib` map.

5. De App Service Linux-toepassing opnieuw hebt gestart. Tomcat wordt opnieuw ingesteld `CATALINA_HOME` naar `/home/tomcat` en de bijgewerkte configuratie en -klassen gebruiken.

## <a name="docker-containers"></a>Docker-containers

Voor het gebruik van de JDK Zulu Azure wordt ondersteund in uw containers, zorg ervoor dat voor het ophalen en gebruiken van de vooraf gemaakte installatiekopieën die worden vermeld op [downloadpagina van Azul](https://www.azul.com/downloads/azure-only/zulu/#docker) of gebruik de `Dockerfile` voorbeelden uit de [Microsoft Java GitHub-opslagplaats](https://github.com/Microsoft/java/tree/master/docker).

## <a name="runtime-availability-and-statement-of-support"></a>Runtime-beschikbaarheid en een overzicht van ondersteuning

App Service for Linux ondersteunt twee runtimes voor het beheerde hosten van Java-webtoepassingen:

- De [Tomcat-servletcontainer](http://tomcat.apache.org/) voor het uitvoeren van toepassingen die zijn verpakt als web-archiefbestanden (WAR). Ondersteunde versies zijn 8.5 en 9.0.
- Java SE runtime-omgeving voor het uitvoeren van toepassingen geleverd als archief voor Java (JAR)-bestanden. De enige ondersteunde primaire versie is Java 8.

## <a name="java-runtime-statement-of-support"></a>Ondersteuning voor statusverklaring van Java-runtime 

### <a name="jdk-versions-and-maintenance"></a>JDK versies en onderhoud

Azure ondersteunde Java Development Kit (JDK) is [Zulu](https://www.azul.com/downloads/azure-only/zulu/) geleverd via [Azul Systems](https://www.azul.com/).

Belangrijke versieupdates wordt geleverd door de nieuwe runtime-opties in Azure App Service voor Linux. Klanten hebben bijgewerkt naar deze nieuwere versies van Java door het configureren van de App Service-implementatie en zijn verantwoordelijk voor het testen en ervoor te zorgen dat de grote update aan hun eisen voldoen.

Ondersteunde JDK worden automatisch gevuld op basis van een kwartaal in januari, April, juli en oktober van elk jaar.

### <a name="security-updates"></a>Beveiligingsupdates

Patches en oplossingen voor bekende beveiligingsproblemen wordt uitgebracht zodra deze beschikbaar van Azul Systems. Een beveiligingslek in de 'belangrijke' is gedefinieerd door een basis score van 9.0 of hoger op de [NIST algemene beveiligingsproblemen Scoring-systeem, versie 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Afschaffing en buiten gebruik stellen

Als een ondersteunde Java-runtime wordt beëindigd, Azure-ontwikkelaars met behulp van de betrokken runtime krijgt een kennisgeving over afschaffing ten minste zes maanden voordat de runtime is buiten gebruik gesteld.

### <a name="local-development"></a>Lokale ontwikkeling

Ontwikkelaars kunnen de productie-editie van Azul Zulu Enterprise JDK voor lokale ontwikkeling van downloaden [site voor het downloaden van Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Ontwikkelingsondersteuning voor

Ondersteuning voor het product voor de [Azure wordt ondersteund Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) is beschikbaar via bij het ontwikkelen voor Azure of [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) met een [gekwalificeerde Azure-ondersteuningsplan](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Runtime-ondersteuning

Ontwikkelaars kunnen [opent u een probleem](/azure/azure-supportability/how-to-create-azure-support-request) met de JDK Azul Zulu via ondersteuning voor Azure als ze beschikken over een [gekwalificeerde ondersteuningsplan](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure voor Java-ontwikkelaars](/java/azure/) center om te zoeken voor Azure-snelstartgidsen, zelfstudies en naslagdocumentatie voor Java.
