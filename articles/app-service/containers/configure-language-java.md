---
title: Apps in Linux Java - Azure App Service configureren | Microsoft Docs
description: Informatie over het configureren van Java-apps in Azure App Service op Linux uitgevoerd.
keywords: Azure appservice, web-app, linux, oss, java, java ee, jee, javaee
services: app-service
author: bmitchell287
manager: douge
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/26/2019
ms.author: brendm
ms.custom: seodec18
ms.openlocfilehash: 51ca597208b582e95fd305886dcf163744825eee
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509641"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Een Linux Java-app configureren voor Azure App Service

Azure App Service on Linux kunt Java-ontwikkelaars voor het snel bouwen, implementeren en schalen van hun Tomcat of Standard-editie van Java (SE) verpakt in een volledig beheerde service voor Linux-gebaseerde web-apps. Implementeer toepassingen in Maven-invoegtoepassingen vanaf de opdrachtregel of in een editor, zoals IntelliJ, Eclipse of Visual Studio Code.

Deze handleiding bevat de belangrijkste concepten en instructies voor het Java-ontwikkelaars die gebruikmaken van een ingebouwde Linux-container in App Service. Als u Azure App Service nog nooit hebt gebruikt, volgt u de [Java-quickstart](quickstart-java.md) en [Java met PostgreSQL zelfstudie](tutorial-java-enterprise-postgresql-app.md) eerste.

## <a name="deploying-your-app"></a>Implementeren van uw app

U kunt [Maven-invoegtoepassing voor Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) zowel .war als JAR-bestanden implementeren. Implementatie met populaire IDE's wordt ook ondersteund met [Azure Toolkit voor IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij) of [Azure Toolkit voor Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

Uw implementatiemethode wordt anders, afhankelijk van uw archieftype:

- Als u wilt implementeren WAR-bestanden op Tomcat, gebruikt u de `/api/wardeploy/` eindpunt naar het archiefbestand plaatsen. Zie voor meer informatie over deze API [deze documentatie](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file).
- Voor het implementeren van de JAR-bestanden in de Java SE-installatiekopieën, gebruikt u de `/api/zipdeploy/` eindpunt van de Kudu-site. Zie voor meer informatie over deze API [deze documentatie](https://docs.microsoft.com/azure/app-service/deploy-zip#rest).

Implementeer uw .war of JAR met FTP niet. De FTP-programma is ontworpen om te uploaden opstartscripts, afhankelijkheden of andere runtimebestanden. Het is niet de beste keuze voor het implementeren van web-apps.

## <a name="logging-and-debugging-apps"></a>Logboekregistratie en foutopsporing van apps

Rapporten, verkeer visualisaties en de gezondheid van controles zijn beschikbaar voor elke app via de Azure-portal. Zie voor meer informatie, [overzicht van Azure App Service-diagnostics](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>Toegang tot de SSH-console

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Diagnostische logboeken streamen

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Zie voor meer informatie, [Streaminglogboeken met de Azure CLI](../troubleshoot-diagnostic-logs.md#streaming-with-azure-cli).

### <a name="app-logging"></a>App-registratie

Schakel [toepassingslogboeken](../troubleshoot-diagnostic-logs.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#enablediag) via Azure portal of [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) App Service configureren voor de standard-console-uitvoer en foutstromen standard-console van uw toepassing schrijven naar de lokale bestandssysteem of Azure Blob Storage. Logboekregistratie voor het lokale bestandssysteem voor App Service exemplaar uitgeschakeld 12 uur nadat deze is geconfigureerd. Als u langere bewaartermijn nodig hebt, configureert u de toepassing naar uitvoer schrijven naar een Blob storage-container. Uw Java- en Tomcat-app-logboeken kunnen u vinden in de */home/logboekbestanden/toepassing/* directory.

Als uw toepassing gebruikmaakt van [Logback](https://logback.qos.ch/) of [Log4j](https://logging.apache.org/log4j) voor tracering, kunt u doorsturen deze traceringen ter beoordeling naar Azure Application Insights met behulp van de logboekregistratie framework configuratie-instructies in [Traceerlogboeken in Application Insights Java verkennen](/azure/application-insights/app-insights-java-trace-logs).

### <a name="troubleshooting-tools"></a>Hulpprogramma's voor probleemoplossing

De ingebouwde Java-installatiekopieën op basis van de [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) besturingssysteem. Gebruik de `apk` Pakketbeheer voor het installeren van eventuele problemen oplossen-hulpprogramma's of opdrachten.

### <a name="flight-recorder"></a>Zwarte doos

Alle Linux Java-installatiekopieën op App Service hebben Zulu zwarte doos is geïnstalleerd, zodat u kunt eenvoudig verbinding met de JVM maken en start een profiler op te nemen of een heap-dump genereren.

#### <a name="timed-recording"></a>Getimede opnemen

Aan de slag, voeg SSH toe aan uw App Service en uitvoeren de `jcmd` opdracht voor een overzicht van alle Java-processen die worden uitgevoerd. Naast jcmd zelf ziet u uw Java-toepassing die wordt uitgevoerd met een proces-ID (pid).

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

Voer de onderstaande opdracht om te beginnen met opnemen van JVM 30 seconden. Hierdoor wordt de JVM-profiel en een JFR-bestand met de naam *jfr_example.jfr* in de basismap. (116 vervangen door de pid van uw Java-app.)

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

Tijdens de tweede interval van 30, kunt u de opname valideren door te voeren is plaatsvinden `jcmd 116 JFR.check`. Hiermee wordt alle opnamen voor het opgegeven Java-proces weergegeven.

#### <a name="continuous-recording"></a>Continue opnemen

U kunt Zulu zwarte doos continu profiel van uw Java-toepassing met minimale impact op de runtime-prestaties ([bron](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)). Voer de volgende Azure CLI-opdracht voor het maken van een App-instelling met de naam JAVA_OPTS met de vereiste configuratie om dit te doen. De inhoud van de App-instelling JAVA_OPTS worden doorgegeven aan de `java` opdracht wanneer uw app wordt gestart.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Zie voor meer informatie de [Jcmd naslaginformatie](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190).

### <a name="analyzing-recordings"></a>Analyseren van opnamen

Gebruik [FTPS](../deploy-ftp.md) uw JFR-bestand te downloaden naar uw lokale computer. Voor het analyseren van het bestand JFR, download en installeer [Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/). Zie voor instructies over Zulu Mission Control, de [Azul documentatie](https://docs.azul.com/zmc/) en de [installatie-instructies](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

## <a name="customization-and-tuning"></a>Aanpassing en afstemmen

Azure App Service voor Linux ondersteunt buiten het vak afstemmen en aanpassen via de Azure portal en CLI. Raadpleeg de volgende artikelen voor de configuratie van niet-Java-specifieke web-app:

- [App-instellingen configureren](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)
- [Een aangepast domein instellen](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [SSL inschakelen](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Een CDN toevoegen](../../cdn/cdn-add-to-web-app.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [De Kudu-site configureren](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Java runtime-opties instellen

Toegewezen geheugen of andere JVM-runtime-opties instellen in de Tomcat- en Java SE-omgevingen, maakt u een [app-instelling](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) met de naam `JAVA_OPTS` met de opties. App Service Linux geeft deze instelling als een omgevingsvariabele aan de Java-runtime wanneer deze wordt gestart.

In de Azure-portal onder **toepassingsinstellingen** voor de web-app, maakt u een nieuwe appinstelling met de naam `JAVA_OPTS` die bevat de aanvullende instellingen, zoals `-Xms512m -Xmx1204m`.

Voor het configureren van de app-instelling van de Maven-invoegtoepassing toevoegen/waarde-codes in de sectie Azure-invoegtoepassing. Het volgende voorbeeld wordt een specifieke minimale en maximale Java heap-grootte:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Ontwikkelaars van één toepassing uitvoert met een implementatiesite in hun App Service-plan kunnen de volgende opties gebruiken:

- Instanties van B1 en S1: `-Xms1024m -Xmx1024m`
- Instanties van B2 en S2: `-Xms3072m -Xmx3072m`
- B3 en S3-exemplaren: `-Xms6144m -Xmx6144m`

Wanneer de instellingen voor de heap afstemmen, bekijk de details van uw App Service-plan en rekening gehouden met meerdere toepassingen en implementatiesite moeten vinden van de optimale toewijzing van geheugen.

Als u een JAR-toepassing implementeert, wordt de naam van *app.jar* zodat uw app correct kan de ingebouwde installatiekopie kan worden geïdentificeerd. (De Maven-invoegtoepassing wordt automatisch deze naam.) Als u niet wilt wijzigen van de JAR naar *app.jar*, kunt u een shell-script met de opdracht om uit te voeren van de JAR uploaden. Plak het volledige pad naar dit script in de [opstartbestand](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-faq#startup-file) tekstvak in de sectie configuratie van de portal.

### <a name="turn-on-web-sockets"></a>Websockets inschakelen

Inschakelen van ondersteuning voor websockets in de Azure portal in de **toepassingsinstellingen** voor de toepassing. U moet de toepassing voor de instelling van kracht opnieuw hebt gestart.

Schakel web socket-ondersteuning met behulp van de Azure CLI met de volgende opdracht:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Start uw toepassing:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Standaard-tekencodering instellen

In de Azure-portal onder **toepassingsinstellingen** voor de web-app, maakt u een nieuwe appinstelling met de naam `JAVA_OPTS` met waarde `-Dfile.encoding=UTF-8`.

U kunt ook de app-instelling met behulp van de App Service-Maven-invoegtoepassing configureren. De instelling naam / waarde-tags toevoegen in de configuratie van de invoegtoepassing:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Time-out voor opstarten aanpassen

Als uw Java-toepassing erg groot is, moet u de tijdslimiet voor opstarten vergroten. Om dit te doen, maakt u een toepassingsinstelling `WEBSITES_CONTAINER_START_TIME_LIMIT` en stel deze in op het aantal seconden dat App Service voordat een time-out optreedt wachten moet. De maximumwaarde is `1800` seconden.

## <a name="secure-applications"></a>Beveiligde toepassingen

Java-toepassingen die worden uitgevoerd in App Service for Linux hebben dezelfde set [best practices voor beveiliging](/azure/security/security-paas-applications-using-app-services) als andere toepassingen.

### <a name="authenticate-users"></a>Gebruikers verifiëren

Instellen van app-verificatie in Azure portal met de **verificatie en autorisatie** optie. Van daaruit kunt u verificatie met behulp van Azure Active Directory of sociale aanmeldingen als Facebook, Google of GitHub. Configuratie van Azure portal werkt alleen bij het configureren van een enkele verificatieprovider. Zie voor meer informatie, [configureren van uw App Service-app voor het gebruik van Azure Active Directory-aanmelding](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) en de bijbehorende artikelen voor andere id-providers. Als u nodig hebt om in te schakelen van meerdere providers voor aanmelden, volg de instructies in de [App Service-verificatie aanpassen](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) artikel.

#### <a name="tomcat"></a>Tomcat

Uw toepassing Tomcat van de gebruiker toegang tot claims rechtstreeks vanaf de servlet Tomcat door het gebruik van de Principal-object op een kaartobject. Het object Map wordt elke claimtype worden toegewezen aan een verzameling van de claims voor dat type. In de onderstaande code, `request` is een exemplaar van `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Nu kunt u controleren de `Map` -object voor een specifieke claim. Bijvoorbeeld het volgende codefragment doorloopt de claimtypen en de inhoud van elke verzameling wordt afgedrukt.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Gebruikers afmelden en andere acties worden uitgevoerd, Raadpleeg de documentatie over [gebruik van App Service-verificatie en autorisatie](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to). Er is ook officiële documentatie over de Tomcat [HttpServletRequest interface](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) en de bijbehorende methoden. De volgende servlet methoden ook zijn gemigreerd, is afhankelijk van uw App Service-configuratie:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Als u wilt deze functie uitschakelen, maakt u een toepassingsinstelling met de naam `WEBSITE_AUTH_SKIP_PRINCIPAL` met een waarde van `1`. Als wilt uitschakelen alle servlet filters toegevoegd door App Service, maakt u een instelling met de naam `WEBSITE_SKIP_FILTERS` met een waarde van `1`.

#### <a name="spring-boot"></a>Spring Boot

Spring Boot-ontwikkelaars kunnen gebruikmaken van de [Azure Active Directory Spring Boot starter](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) voor het beveiligen van toepassingen met bekende Spring Security aantekeningen en API's. Zorg ervoor dat u het verhogen van de maximale header-grootte in uw *application.properties* bestand. Een waarde van het is raadzaam `16384`.

### <a name="configure-tlsssl"></a>TLS/SSL configureren

Volg de instructies in de [een bestaand aangepast SSL-certificaat binden](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) voor het uploaden van een bestaand SSL-certificaat en bindt dit aan de domeinnaam van uw toepassing. Standaard worden uw toepassing nog steeds kunt HTTP-verbindingen-Volg de specifieke stappen in de zelfstudie voor het afdwingen van SSL en TLS.

### <a name="use-keyvault-references"></a>Key Vault verwijzingen gebruiken

[Met Azure Key Vault](../../key-vault/key-vault-overview.md) biedt gecentraliseerd beheer van geheim met de geschiedenis voor het beleid en de controle van toegang. U kunt geheimen (zoals wachtwoorden of tekenreeksen voor databaseverbindingen) in Key Vault opslaan en toegang tot deze geheimen in uw toepassing via omgevingsvariabelen.

Eerst, volg de instructies voor [uw apptoegang verlenen tot Key Vault](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) en [maken van een Key Vault-verwijzing naar het geheim in een toepassingsinstelling](../app-service-key-vault-references.md#reference-syntax). U kunt valideren dat de verwijzing wordt omgezet naar het geheim met afdrukken via de omgevingsvariabele tijdens het op afstand openen van de App Service-terminal.

Als u wilt deze geheime gegevens in uw configuratiebestand Spring of Tomcat invoeren, gebruikt u de omgeving variabele injectie syntaxis (`${MY_ENV_VAR}`). Voor de Spring-configuratiebestanden, Zie de deze documentatie over [externalized configuraties](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

## <a name="configure-apm-platforms"></a>APM-platforms configureren

Deze sectie wordt beschreven hoe u Java-toepassingen die zijn geïmplementeerd in Azure App Service op Linux met de NewRelic en AppDynamics bewaking van toepassingsprestaties (APM)-platforms verbinding maakt.

[Configureren van New Relic](#configure-new-relic)
[AppDynamics configureren](#configure-appdynamics)

### <a name="configure-new-relic"></a>New Relic configureren

1. Maak een account NewRelic op [NewRelic.com](https://newrelic.com/signup)
2. De Java-agent downloaden van NewRelic, is er een bestandsnaam die vergelijkbaar is met *newrelic-java-x.x.x.zip*.
3. Kopieer de licentiesleutel van uw, moet u het later opnieuw configureren van de agent.
4. [Voeg SSH toe aan uw App Service-exemplaar](app-service-linux-ssh-support.md) en maak een nieuwe map */home/site/wwwroot/apm*.
5. De uitgepakte NewRelic Java-agentbestanden uploaden naar een map onder */home/site/wwwroot/apm*. De bestanden voor de agent moet */home/site/wwwroot/apm/newrelic*.
6. Wijzig het YAML-bestand op */home/site/wwwroot/apm/newrelic/newrelic.yml* en vervang de waarde van de licentie voor tijdelijke aanduidingen door uw eigen licentiesleutel.
7. In Azure portal, blader naar uw toepassing in App Service en maak een nieuwe instelling van de toepassing.
    - Als uw app **Java SE**, maken van een omgevingsvariabele met de naam `JAVA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Als u **Tomcat**, maken van een omgevingsvariabele met de naam `CATALINA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Als u **WildFly**, Zie de documentatie van New Relic [hier](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) voor instructies over het installeren van de agent voor Java- en JBoss configuratie.
    - Als u al een omgevingsvariabele voor `JAVA_OPTS` of `CATALINA_OPTS`, toevoegen de `javaagent` optie aan het einde van de huidige waarde.

### <a name="configure-appdynamics"></a>AppDynamics configureren

1. Maak een account met AppDynamics op [AppDynamics.com](https://www.appdynamics.com/community/register/)
2. De Java-agent downloaden vanaf de website van AppDynamics, de bestandsnaam is vergelijkbaar met *AppServerAgent x.x.x.xxxxx.zip*
3. [Voeg SSH toe aan uw App Service-exemplaar](app-service-linux-ssh-support.md) en maak een nieuwe map */home/site/wwwroot/apm*.
4. De Java-agentbestanden uploaden naar een map onder */home/site/wwwroot/apm*. De bestanden voor de agent moet */home/site/wwwroot/apm/appdynamics*.
5. In Azure portal, blader naar uw toepassing in App Service en maak een nieuwe instelling van de toepassing.
    - Als u **Java SE**, maken van een omgevingsvariabele met de naam `JAVA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` waar `<app-name>` is de naam van uw App Service.
    - Als u **Tomcat**, maken van een omgevingsvariabele met de naam `CATALINA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` waar `<app-name>` is de naam van uw App Service.
    - Als u **WildFly**, Zie de documentatie van AppDynamics [hier](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) voor instructies over het installeren van de agent voor Java- en JBoss configuratie.

## <a name="configure-jar-applications"></a>JAR-toepassingen configureren

### <a name="starting-jar-apps"></a>Starten van de JAR-Apps

Standaard, App Service wordt verwacht dat de JAR-toepassing naar de naam *app.jar*. Als dat zo deze naam is, wordt deze automatisch worden uitgevoerd. Voor gebruikers van Maven, kunt u de naam van de JAR instellen door op te nemen `<finalName>app</finalName>` in de `<build>` sectie van uw *pom.xml*. [U kunt doen in Gradle hetzelfde](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) door in te stellen de `archiveFileName` eigenschap.

Als u een andere naam voor uw JAR gebruiken wilt, moet u ook opgeven de [opstartopdracht](app-service-linux-faq.md#built-in-images) die uw JAR-bestand wordt uitgevoerd. Bijvoorbeeld `java -jar my-jar-app.jar`. U kunt de waarde instellen voor de opdracht voor opstarten in de Portal, onder configuratie > algemene instellingen, of met een toepassingsinstelling met de naam `STARTUP_COMMAND`.

### <a name="server-port"></a>Serverpoort

App Service Linux worden binnenkomende aanvragen gerouteerd naar poort 80, zodat uw toepassing op poort 80 ook luisteren moet. U kunt dit doen in de configuratie van uw toepassing (zoals de Spring *application.properties* bestand), of in uw opstartopdracht (bijvoorbeeld `java -jar spring-app.jar --server.port=80`). Raadpleeg de volgende documentatie voor algemene Java-frameworks:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Play-Framework](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Gegevensbronnen

### <a name="tomcat"></a>Tomcat

Deze instructies zijn van toepassing op alle databaseverbindingen. U moet tijdelijke aanduidingen invullen met de gekozen database stuurprogramma klassenaam en het JAR-bestand. Opgegeven is een tabel met klassenamen en Stuurprogrammadownloads voor algemene databases.

| Database   | De naam van de stuurprogramma-klasse                             | JDBC Driver                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Downloaden](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Download](https://dev.mysql.com/downloads/connector/j/) (Selecteer "Platform onafhankelijke") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Downloaden](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

Voor het configureren van Tomcat voor het gebruik van Java-Database Connectivity (JDBC) of de Java-persistentie API (JPA), eerst aanpassen aan de `CATALINA_OPTS` omgevingsvariabele die Tomcat bij het opstarten worden ingelezen. Stel deze waarden via een app-instelling in de [App Service-Maven-invoegtoepassing](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Of stel de omgevingsvariabelen de **configuratie** > **toepassingsinstellingen** pagina in de Azure portal.

Bepaal vervolgens als de gegevensbron moet beschikbaar zijn voor één toepassing of voor alle toepassingen die worden uitgevoerd op de Tomcat-servlet.

#### <a name="application-level-data-sources"></a>Op toepassingsniveau gegevensbronnen

1. Maak een *context.xml* -bestand in de *META-INF /* map van uw project. Maak de *META-INF /* map als deze niet bestaat.

2. In *context.xml*, Voeg een `Context` element op de gegevensbron een koppeling naar een adres JNDI. Vervang de `driverClassName` tijdelijke aanduiding met de klassenaam van het stuurprogramma van de bovenstaande tabel.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Bijwerken van uw toepassing *web.xml* naar de gegevensbron in uw toepassing gebruiken.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Gedeelde bronnen op serverniveau

1. Kopieer de inhoud van */usr/local/tomcat/conf* in */home/tomcat/conf* exemplaar met behulp van SSH als u nog niet een configuratie er hebt in uw App Service Linux.

    ```bash
    mkdir -p /home/tomcat
    cp -a /usr/local/tomcat/conf /home/tomcat/conf
    ```

2. Toevoegen van een contextelement in uw *server.xml* binnen de `<Server>` element.

    ```xml
    <Server>
    ...
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ...
    </Server>
    ```

3. Bijwerken van uw toepassing *web.xml* naar de gegevensbron in uw toepassing gebruiken.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finalize-configuration"></a>Configuratie voltooien

Ten slotte, plaatst u de stuurprogramma-JAR-bestanden in het klassepad Tomcat en uw App-Service opnieuw.

1. Zorg ervoor dat de JDBC-stuurprogramma's beschikbaar voor de Tomcat-classloader zijn door ze in te plaatsen de */home/tomcat/lib* directory. (Maak deze map als deze niet al bestaat.) Als u wilt deze bestanden uploaden naar uw App Service-exemplaar, kunt u de volgende stappen uitvoeren:

    1. In de [Cloud Shell](https://shell.azure.com), de uitbreiding voor de Web-App installeren:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. Voer de volgende CLI-opdracht voor het maken van een SSH-tunnel vanuit uw lokale systeem naar App Service:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. Verbinding maken met de lokale tunneling poort met uw SFTP-client en de bestanden te uploaden de */home/tomcat/lib* map.

    U kunt ook een FTP-client gebruiken voor het uploaden van het JDBC-stuurprogramma. Volg deze [instructies voor het ophalen van de referenties van uw FTP-](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

2. Als u een gegevensbron op serverniveau hebt gemaakt, moet u de App Service Linux-toepassing opnieuw starten. Tomcat wordt opnieuw ingesteld `CATALINA_HOME` naar `/home/tomcat/conf` en gebruikt u de bijgewerkte configuratie.

### <a name="spring-boot"></a>Spring Boot

Als u wilt verbinding maken met gegevensbronnen in Spring Boot-toepassingen, stellen we voor het maken van verbindingstekenreeksen en injecteren ze naar uw *application.properties* bestand.

1. In de sectie "Configuratie" van de App Service-pagina instellen van een naam op voor de tekenreeks en plak uw JDBC-verbindingsreeks in het waardeveld instellen van het type in 'Aangepast'. U kunt deze verbindingsreeks (optioneel) instellen als site-instelling.

    Deze verbindingsreeks is toegankelijk voor de toepassing als een omgevingsvariabele met de naam `CUSTOMCONNSTR_<your-string-name>`. Bijvoorbeeld, de verbindingsreeks die eerder is gemaakt de naam `CUSTOMCONNSTR_exampledb`.

2. In uw *application.properties* bestand, verwijzen naar deze verbindingsreeks met de naam van de omgevingsvariabele. In ons voorbeeld gebruiken we het volgende.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Raadpleeg de [Spring Boot-documentatie over gegevenstoegang](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) en [externalized configuraties](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) voor meer informatie over dit onderwerp.

## <a name="configure-java-ee-wildfly"></a>Configureren van Java EE (WildFly)

> [!NOTE]
> Java Enterprise Edition voor App Service Linux is momenteel in Preview. Deze stack is **niet** aanbevolen voor productie gerichte werk. informatie over onze Java SE en Tomcat-stacks.

Azure App Service on Linux kunnen Java-ontwikkelaars voor het bouwen, implementeren en schalen van Java-bedrijfstoepassingen (Java EE) op een volledig beheerde service op basis van Linux.  De onderliggende Java Enterprise runtime-omgeving is de open-source [WildFly](https://wildfly.org/) -toepassingsserver.

Deze sectie bevat de volgende subgedeelten:

- [Schaal met App Service](#scale-with-app-service)
- [Configuratie van de toepassingsserver aanpassen](#customize-application-server-configuration)
- [Modules en afhankelijkheden voor netwerkapparaten installeren](#install-modules-and-dependencies)
- [Gegevensbronnen configureren](#configure-data-sources)
- [Messaging providers inschakelen](#enable-messaging-providers)
- [Sessie beheer van caching configureren](#configure-session-management-caching)

### <a name="scale-with-app-service"></a>Schaal met App Service

De toepassingsserver voor WildFly in App Service op Linux wordt uitgevoerd in de zelfstandige modus, niet in de domeinconfiguratie van een. Wanneer u het App Service-Plan uitschaalt, wordt elke instantie WildFly is geconfigureerd als een zelfstandige server.

Uw toepassing horizontaal of verticaal schalen met [regels voor schalen](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md) en door [uw aantal instanties verhoogt](../web-sites-scale.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="customize-application-server-configuration"></a>Configuratie van de toepassingsserver aanpassen

Web-App-instanties zijn staatloos, zodat elke nieuw exemplaar gestart bij het opstarten voor de ondersteuning van de WildFly-configuratie die nodig zijn voor de toepassing moet worden geconfigureerd.
U kunt een opstartscript Bash-script voor het aanroepen van de CLI WildFly te schrijven:

- Instellen van gegevensbronnen
- Messaging-providers configureren
- Andere modules en afhankelijkheden toevoegen aan de configuratie van de WildFly.

Het script wordt uitgevoerd wanneer WildFly actief en werkend is, maar voordat de toepassing wordt gestart. Het script moet worden gebruikt de [JBOSS CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) aangeroepen vanuit */opt/jboss/wildfly/bin/jboss-cli.sh* configureren van de toepassingsserver met een configuratie- of wijzigingen nodig zijn nadat de server wordt gestart.

Gebruik de interactieve modus van de CLI niet WildFly configureren. In plaats daarvan u krijgt u een script van opdrachten voor CLI JBoss met behulp van de `--file` opdracht, bijvoorbeeld:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

FTP gebruiken voor het uploaden van het opstartscript naar een locatie in uw App Service-exemplaar onder uw */home* Active directory, zoals */home/site/deployments/tools*. Zie voor meer informatie, [uw app implementeren in Azure App Service met behulp van FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

Stel de **opstartscript** veld in de Azure-portal naar de locatie van uw shell opstartscript bijvoorbeeld */home/site/deployments/tools/your-startup-script.sh*.

Geef [app-instellingen](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) in de configuratie van de toepassing om door te geven van omgevingsvariabelen voor gebruik in het script. Toepassingsinstellingen houden en verbindingsreeksen en andere geheimen die nodig zijn voor het configureren van uw toepassing buiten-versiebeheer.

### <a name="install-modules-and-dependencies"></a>Modules en afhankelijkheden voor netwerkapparaten installeren

Voor het installeren van modules en de bijbehorende afhankelijkheden in het klassepad van de WildFly via de CLI JBoss, moet u de volgende bestanden in hun eigen directory maken. Sommige modules en afhankelijkheden moet mogelijk aanvullende configuratie, zoals JNDI naming of andere API-specifieke configuratie, zodat deze lijst een minimale set is van wat u moet een afhankelijkheid configureren in de meeste gevallen.

- Een [XML-module descriptor](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Dit XML-bestand definieert de naam, de kenmerken en de afhankelijkheden van uw module. Dit [module.xml voorbeeldbestand](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) definieert een Postgres-module, het JDBC-afhankelijkheid van de JAR-bestand en andere vereiste module-afhankelijkheden.
- Eventuele benodigde JAR-bestandsafhankelijkheden voor uw module.
- Een script met uw JBoss CLI-opdrachten voor het configureren van de nieuwe module. Dit bestand bevat de opdrachten die worden uitgevoerd door de CLI JBoss de server voor het gebruik van de afhankelijkheid configureren. Zie voor documentatie over de opdrachten voor het toevoegen van modules, gegevensbronnen en messaging-providers, [dit document](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- Een opstartscript Bash aanroepen van de CLI JBoss en voer het script uit in de vorige stap. Dit bestand wordt uitgevoerd wanneer uw App Service-exemplaar opnieuw wordt opgestart of wanneer nieuwe exemplaren zijn ingericht in een scale-out. Deze opstartscript is waar u kunt uitvoeren alle andere configuraties voor uw toepassing zoals de opdrachten JBoss worden doorgegeven aan de JBoss CLI. Dit bestand is ten minste één opdracht uw script JBoss CLI-opdracht doorgeven aan de CLI JBoss:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Zodra u de bestanden en inhoud voor uw module hebt, volgt u de stappen hieronder om de module aan de toepassingsserver WildFly toevoegen.

1. FTP gebruiken voor het uploaden van uw bestanden naar een locatie in uw App Service-exemplaar onder uw */home* Active directory, zoals */home/site/deployments/tools*. Zie voor meer informatie, [uw app implementeren in Azure App Service met behulp van FTP/S](../deploy-ftp.md).
2. In de **configuratie** > **algemene instellingen** pagina van de Azure-portal, stel de **opstartscript** veld naar de locatie van uw startup shell-script voor voorbeeld */home/site/deployments/tools/startup.sh*.
3. Uw App Service-exemplaar opnieuw opstarten door op de **opnieuw** knop in de **overzicht** sectie van de portal of met de Azure CLI.

### <a name="configure-data-sources"></a>Gegevensbronnen configureren

Als u wilt configureren WildFly/JBoss voor toegang tot een gegevensbron, moet u het algemene proces die hierboven worden beschreven in de sectie 'modules installeren en afhankelijkheden' gebruiken. De volgende sectie bevat specifieke informatie opgeven over dit proces voor PostgreSQL, MySQL en SQL Server-gegevensbronnen.

In deze sectie wordt ervan uitgegaan dat u hebt al een app, een App Service-exemplaar en een exemplaar van de service Azure Database. De onderstaande instructies raadpleegt u de naam van uw App Service, de resourcegroep en de verbindingsgegevens van uw database. U vindt deze informatie op de Azure-portal.

Als u liever het gehele proces vanaf het begin met behulp van een voorbeeld-app doorlopen, Zie [zelfstudie: Een Java EE en Postgres web-app in Azure bouwen](tutorial-java-enterprise-postgresql-app.md).

De volgende stappen wordt uitgelegd dat de vereisten voor het verbinden van uw bestaande App Service en de database.

1. Download het JDBC-stuurprogramma voor [PostgreSQL](https://jdbc.postgresql.org/download.html), [MySQL](https://dev.mysql.com/downloads/connector/j/), of [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server). Pak het gedownloade archief om op te halen van het stuurprogramma JAR-bestand.

2. Maak een bestand met een naam, zoals *module.xml* en voeg de volgende code toe. Vervang de `<module name>` tijdelijke aanduiding voor (met inbegrip van de punthaken) door `org.postgres` voor PostgreSQL, `com.mysql` voor MySQL, of `com.microsoft` voor SQL Server. Vervang `<JDBC .jar file path>` met de naam van de JAR-bestand uit de vorige stap, met inbegrip van het volledige pad naar de locatie u plaatst het bestand in uw App Service-exemplaar. Dit kan een willekeurige locatie onder zijn de */home* directory.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

3. Maak een bestand met een naam, zoals *datasource-commands.cli* en voeg de volgende code toe. Vervang `<JDBC .jar file path>` met de waarde die u in de vorige stap hebt gebruikt. Vervang `<module file path>` met de naam en het pad van de App Service uit de vorige stap, bijvoorbeeld */home/module.xml*.

    **PostgreSQL**

    ```console
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)

    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker

    reload --use-current-server-config=true
    ```

    **MySQL**

    ```console
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)

    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter

    reload --use-current-server-config=true
    ```

    **SQL Server**

    ```console
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)

    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter

    reload --use-current-server-config=true
    ```

    Dit bestand wordt uitgevoerd door het opstartscript die worden beschreven in de volgende stap. Deze het JDBC-stuurprogramma wordt geïnstalleerd als een module WildFly, maakt u de bijbehorende WildFly-gegevensbron en wordt opnieuw geladen de server om te controleren of dat de wijzigingen van kracht.

4. Maak een bestand met een naam, zoals *startup.sh* en voeg de volgende code toe. Vervang `<JBoss CLI script>` met de naam van het bestand dat u in de vorige stap hebt gemaakt. Zorg ervoor dat u het volledige pad naar de locatie wordt u het bestand plaatsen in uw App Service-exemplaar, bijvoorbeeld */home/datasource-commands.cli*.

    ```bash
    #!/usr/bin/env bash
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=<JBoss CLI script>
    ```

5. FTP gebruiken voor het uploaden van het JDBC JAR-bestand, de module XML-bestand, de JBoss CLI-script en het opstartscript naar uw App Service-exemplaar. Deze bestanden op de locatie die u hebt opgegeven in de vorige stappen, zoals plaatst */home*. Zie voor meer informatie over FTP [uw app implementeren in Azure App Service met behulp van FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

6. De Azure CLI gebruiken om toe te voegen aan uw App-Service die de verbindingsgegevens van uw database bevatten. Vervang `<resource group>` en `<webapp name>` door de waarden door uw App-Service wordt gebruikt. Vervang `<database server name>`, `<database name>`, `<admin name>`, en `<admin password>` met de verbindingsgegevens van uw database. Uw App Service- en database-gegevens krijgt u vanuit Azure portal.

    **PostgreSQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:postgresql://<database server name>:5432/<database name>?ssl=true \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **MySQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **SQL Server:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

    De waarden DATABASE_CONNECTION_URL zijn verschillend voor elke database-server, en anders dan de waarden in de Azure portal. De URL-indelingen weergegeven hier (en in de bovenstaande codefragmenten) zijn vereist voor gebruik door WildFly:

    * **PostgreSQL:** `jdbc:postgresql://<database server name>:5432/<database name>?ssl=true`
    * **MySQL:** `jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT`
    * **SQL Server:** `jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;`

7. In de Azure-portal, gaat u naar uw App Service en zoek de **configuratie** > **algemene instellingen** pagina. Stel de **opstartscript** veld aan de naam en locatie van het opstartscript bijvoorbeeld */home/startup.sh*.

De volgende keer dat de App-Service opnieuw is opgestart, wordt het uitvoeren van het opstartscript en de benodigde configuratiestappen uitvoeren. Als u wilt testen dat deze configuratie correct wordt uitgevoerd, kunt u toegang tot uw App Service met behulp van SSH en voer vervolgens het opstartscript zelf vanuit de Bash-prompt. U kunt ook de App Service-logboeken bekijken. Zie voor meer informatie over deze opties [logboekregistratie en foutopsporing van apps](#logging-and-debugging-apps).

Vervolgens moet u de configuratie van de WildFly voor uw app bijwerken en opnieuw te implementeren. Voer de volgende stappen uit:

1. Open de *src/main/resources/META-INF/persistence.xml* -bestand voor uw app en het zoeken naar de `<jta-data-source>` element. Vervang de inhoud ervan zoals hier wordt weergegeven:

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

2. Opnieuw maken en implementeren van uw app met de volgende opdracht bij de Bash-prompt:

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

3. Uw App Service-exemplaar opnieuw opstarten door op de **opnieuw** knop in de **overzicht** gedeelte van de Azure-portal of met behulp van de Azure CLI.

Uw App Service-exemplaar is nu geconfigureerd voor toegang tot de database.

Zie voor meer informatie over het configureren van de verbinding met de database met WildFly [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource), of [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898).

### <a name="enable-messaging-providers"></a>Messaging providers inschakelen

Bericht gestuurde bonen met behulp van Service Bus als het mechanisme voor tekstberichten inschakelen:

1. Gebruik de [Apache QPId JMS messaging-bibliotheek](https://qpid.apache.org/proton/index.html). Met deze afhankelijkheid in uw pom.xml (of andere build-bestand) bevatten voor de toepassing.

2. Maak [Service Bus-bronnen](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Maak een Azure Service Bus-naamruimte en wachtrij in die naamruimte en een gedeeld toegangsbeleid met verzenden en ontvangen van de mogelijkheden.

3. De sleutel voor gedeelde toegang geven aan uw code door een URL-codering de primaire sleutel van uw beleid of [gebruik de SDK van Service Bus](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Volg de stappen die worden beschreven in de sectie Modules installeren en afhankelijkheden met uw module XML descriptor, .jar afhankelijkheden, JBoss CLI-opdrachten en opstartscript voor de provider JMS. Naast de vier bestanden moet u ook een XML-bestand dat de naam van de JNDI voor de JMS wachtrij en onderwerp bepaalt maken. Zie [deze opslagplaats](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) voor referentie-configuratiebestanden.

### <a name="configure-session-management-caching"></a>Sessie beheer van caching configureren

Standaard App Service on Linux sessiecookies affiniteit wordt gebruikt om ervoor te zorgen dat aanvragen van clients met bestaande sessies worden gerouteerd hetzelfde exemplaar van uw toepassing. Dit standaardgedrag is geen configuratie vereist, maar kent enkele beperkingen:

- Als een exemplaar van de toepassing opnieuw wordt opgestart of omlaag geschaald, wordt de status van de gebruiker-sessie in de toepassingsserver, gaan verloren.
- Als toepassingen time-outinstellingen lang sessie of een vast aantal gebruikers hebt, duurt het even voordat autoscaled nieuwe exemplaren te ontvangen omdat alleen nieuwe sessies worden doorgestuurd naar de zojuist gestarte exemplaren.

U kunt configureren dat WildFly voor het gebruik van een externe sessie-store, zoals [Azure Cache voor Redis](/azure/azure-cache-for-redis/). U moet [uitschakelen van de ARR-affiniteit voor bestaande exemplaar](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) configuratie van de sessie cookies op basis van routering uitgeschakeld en wordt de geconfigureerde WildFly sessie store werken zonder tussenkomst toestaan.

## <a name="docker-containers"></a>Docker-containers

Voor het gebruik van de JDK Zulu Azure wordt ondersteund in uw containers, zorg ervoor dat voor het ophalen en de vooraf gemaakte installatiekopieën te gebruiken, zoals beschreven in de [Azul Zulu Enterprise ondersteund voor Azure-downloadpagina](https://www.azul.com/downloads/azure-only/zulu/) of gebruik de `Dockerfile` voorbeelden uit de [Microsoft Java GitHub-opslagplaats](https://github.com/Microsoft/java/tree/master/docker).

## <a name="statement-of-support"></a>Overzicht van ondersteuning

### <a name="runtime-availability"></a>Runtime-beschikbaarheid

App Service for Linux ondersteunt twee runtimes voor het beheerde hosten van Java-webtoepassingen:

- De [Tomcat-servletcontainer](https://tomcat.apache.org/) voor het uitvoeren van toepassingen die zijn verpakt als web-archiefbestanden (WAR). Ondersteunde versies zijn 8.5 en 9.0.
- Java SE runtime-omgeving voor het uitvoeren van toepassingen geleverd als archief voor Java (JAR)-bestanden. Ondersteunde versies zijn Java 8 en 11.

### <a name="jdk-versions-and-maintenance"></a>JDK versies en onderhoud

Azul Zulu Enterprise builds van OpenJDK zijn kosteloze, meerdere platforms, gereed voor productie verdeling van de OpenJDK voor Azure en Azure Stack ondersteund door Microsoft en Azul Systems. Ze bevatten de onderdelen voor het bouwen en uitvoeren van Java SE toepassingen. U kunt de JDK van installeren [Java JDK installatie](https://aka.ms/azure-jdks).

Ondersteunde JDK worden automatisch gevuld op basis van een kwartaal in januari, April, juli en oktober van elk jaar.

### <a name="security-updates"></a>Beveiligingsupdates

Patches en oplossingen voor bekende beveiligingsproblemen wordt uitgebracht zodra deze beschikbaar van Azul Systems. Een beveiligingslek in de 'belangrijke' is gedefinieerd door een basis score van 9.0 of hoger op de [NIST algemene beveiligingsproblemen Scoring-systeem, versie 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Afschaffing en buiten gebruik stellen

Als een ondersteunde Java-runtime wordt beëindigd, Azure-ontwikkelaars met behulp van de betrokken runtime krijgt een kennisgeving over afschaffing ten minste zes maanden voordat de runtime is buiten gebruik gesteld.

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure voor Java-ontwikkelaars](/java/azure/) center om te zoeken voor Azure-snelstartgidsen, zelfstudies en naslagdocumentatie voor Java.

Algemene vragen over het gebruik van App Service voor Linux die niet specifiek zijn voor de Java-ontwikkeling worden beantwoord het [Veelgestelde vragen over App Service Linux](app-service-linux-faq.md).
