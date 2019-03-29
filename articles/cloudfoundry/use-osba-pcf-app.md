---
title: Een Open Service Broker for Azure-database gebruiken met een toepassing in Pivotal Cloud Foundry
author: zr-msft
manager: jeconnoc
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 01/11/2019
ms.topic: tutorial
description: Legt uit hoe u een Pivotal Cloud Foundry-toepassing configureert om een Open Service Broker for Azure-database te gebruiken
keywords: Pivotal Cloud Foundry, Cloud Foundry, Open Service Broker, Open Service Broker for Azure
tags: Cloud-Foundry
ms.openlocfilehash: d553cd09ef42e47e3a10fb96039063b8aae665cb
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258858"
---
# <a name="tutorial-use-an-open-service-broker-for-azure-database-with-an-application-on-pivotal-cloud-foundry"></a>Zelfstudie: Een Open Service Broker for Azure-database gebruiken met een toepassing in Pivotal Cloud Foundry

Door Open Service Broker for Azure met een Pivotal Cloud Foundry-exemplaar te gebruiken kunt u services, zoals databases, rechtstreeks inrichten vanuit Cloud Foundry CLI en uw Pivotal Cloud Foundry-exemplaar. U kunt deze services ook verbinden met een toepassing die wordt uitgevoerd in uw Pivotal Cloud Foundry-exemplaar. Als u op deze manier een toepassing verbindt met een service, hoeft u geen code of configuratie bij te werken binnen de toepassing. In dit artikel wordt uitgelegd hoe u een Open Service Broker for Azure-service gebruikt voor een database met een toepassing in Pivotal Cloud Foundry.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De ruimte voor uw toepassing voorbereiden in Pivotal Cloud Foundry.
> * Een voorbeeldtoepassingsbron klonen uit GitHub.
> * De toepassing voorbereiden voor implementatie.
> * De toepassing implementeren.
> * Een database maken met behulp van Open Service Broker for Azure.
> * De database verbinden met uw toepassing.

## <a name="prerequisites"></a>Vereisten

Voordat u verdergaat, moet u:

* [Pivotal Cloud Foundry hebben geïnstalleerd en geconfigureerd](create-cloud-foundry-on-azure.md)
* [Open Service Broker for Azure hebben geïnstalleerd en geconfigureerd](https://network.pivotal.io/products/azure-open-service-broker-pcf) met uw Cloud Foundry-exemplaar

Hier ziet u een voorbeeld van het Ops Manager-scherm in Pivotal Cloud Foundry met de tegel Open Service Broker for Azure geïnstalleerd en geconfigureerd:

![Pivotal Cloud Foundry met Open Service Broker for Azure geïnstalleerd](media/pcf-ops-manager.png)

## <a name="prepare-your-application-space-in-pivotal-cloud-foundry"></a>De ruimte voor uw toepassing voorbereiden in Pivotal Cloud Foundry

Als u uw toepassing wilt implementeren in uw Pivotal Cloud Foundry-exemplaar, moet u zijn aangemeld met het opdrachtregelprogramma `cf`. U moet ook de beoogde organisatie en ruimte weten.

Gebruik `cf target` om te verifiëren dat u bent aangemeld en om de ruimte weer te geven die u wilt gebruiken. In het onderstaande voorbeeld ziet u een gebruiker die al is aangemeld als *beheerder*, met de organisatie *myorg* en met als doel de ruimte *dev*:

```cmd
$ cf target

api endpoint:   https://api.system.40.85.111.222.cf.pcfazure.com
api version:    2.120.0
user:           admin
org:            myorg
space:          dev
```

Gebruik `cf login` om u aan te melden:

```cmd
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Gebruik `cf create-org` en `cf create-space` om een nieuwe organisatie en ruimte te maken:

```cmd 
cf create-org myorg
cf create-space dev -o myorg
```

Gebruik `cf target` om een ruimte te zoeken:

```cmd
cf target -o myorg -s dev
```

## <a name="get-application-code"></a>Toepassingscode ophalen

Als een toepassing moet gebruikmaken van Open Service Broker for Azure, moet de toepassing een gegevensbron gebruiken, zoals een database. In dit artikel gebruiken we de [Spring Music-voorbeeldtoepassing van Cloud Foundry](https://github.com/cloudfoundry-samples/spring-music) om een toepassing te demonstreren die gebruikmaakt van een gegevensbron.

Kloon de toepassing uit GitHub en navigeer naar de bijbehorende map:

```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring music
```

> [!TIP]
> Als u de gegevensbronnen voor deze toepassing wilt weergeven, opent u het bestand [src/main/resources/application.yml](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/application.yml).


## <a name="prepare-the-application-for-deployment"></a>De toepassing voorbereiden voor implementatie

Voordat u de toepassing kunt implementeren in uw Pivotal Cloud Foundry-exemplaar, moet u deze bouwen. Voor demonstratiedoeleinden moeten we ook *DEBUG*-logboekregistratie inschakelen waarmee de verbindingsgegevens van de gegevensbron worden geregistreerd.

Als u *DEBUG*-logboekregistratie wilt inschakelen voor de verbindingsgegevens van de database, voegt u de onderstaande yaml-eigenschap toe aan het eind van de *application.yml*:

```yaml
---
logging:
  level:
    com.zaxxer.hikari: DEBUG
```

De voorbeeldtoepassing maakt gebruik van gradle om de toepassing te bouwen in een uitvoerbaar JAR-bestand van Spring Boot. Het uitvoerbare JAR-bestand wordt geïmplementeerd in uw Pivotal Cloud Foundry-exemplaar. Ga als volgt te werk om de toepassing te bouwen:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```


## <a name="deploy-your-application"></a>Uw toepassing implementeren

Gebruik de opdracht `cf push` om de toepassing te implementeren in het Pivotal Cloud Foundry-exemplaar:

```cmd
cf push

Pushing from manifest to org myorg / space dev as admin...
Using manifest file /path/to/spring-music/manifest.yml
Getting app info...
Creating app with these attributes...
+ name:       spring-music
...
Waiting for app to start...

name:              spring-music
requested state:   started
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
     state     since                  cpu    memory         disk           details
#0   running   2018-12-06T21:24:06Z   0.0%   313.3M of 1G   170.7M of 1G
```

Kopieer de waarde bij *routes* die wordt weergegeven in de uitvoer van `cf push`. De route is de URL die u gebruikt voor toegang tot de actieve toepassing. Bijvoorbeeld:

```cmd
...
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
```


Nadat de toepassing is geïmplementeerd, kunt u de logboeken van de toepassing bekijken om de verbindings-URL te zien die wordt gebruikt voor de toepassing. Met de onderstaande opdracht worden de logboeken van de toepassing weergegeven en wordt `grep` gebruikt om te zoeken naar de *jdbcUrl*-configuratie.

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
```

Opmerking: de toepassing gebruikt *h2:mem:testdb* uit de in-memory database. Een Spring-toepassing wordt automatisch geconfigureerd voor gebruik van een in-memory database wanneer er een afhankelijkheid van een in-memory database op het klassepad is en [automatische configuratie](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-auto-configuration.html) is ingeschakeld. Voor de voorbeeldtoepassing is de [afhankelijkheid h2 in-memory database geconfigureerd](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L49) en automatische configuratie is ingeschakeld in [src/main/java/org/cloudfoundry/samples/music/Application.java](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/java/org/cloudfoundry/samples/music/Application.java#L8).

Gebruik de route van de toepassing om deze te vinden met een browser. De route, of URL, wordt weergegeven in de uitvoer van de opdracht `cf push`.

> [!TIP]
> U kunt de URL van de toepassing ook weergeven door `cf apps` uit te voeren.

Als u naar de toepassing bent genavigeerd met behulp van de browser, kunt u met de toepassing werken door een aantal bestaande albums te verwijderen en een aantal nieuwe albums te maken. De voorbeeldtoepassing maakt gebruik van de in-memory database om uw wijzigingen op te slaan. U ziet ook dat er een aantal [standaardgegevens](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json) zijn ingevuld in de toepassing. 

![Spring Music-app met standaardgegevens](media/music-app.png)

Omdat uw toepassing gebruikmaakt van in-memory database, worden uw wijzigingen niet behouden als de toepassing opnieuw wordt gestart of geïmplementeerd. Om te laten zien dat wijzigingen niet worden behouden nadat u wijzigingen hebt aangebracht, faseert u de toepassing opnieuw met behulp van `cf restage`:

```cmd
cf restage spring-music
```

Nadat de toepassing opnieuw is gefaseerd, navigeert u met een browser naar de toepassing met behulp van dezelfde URL. U ziet dat wijzigingen die u hebt aangebracht, zijn verwijderd en de standaardgegevens worden weergegeven.

![Spring Music-app met standaardgegevens](media/music-app.png)

## <a name="create-a-database"></a>Een database maken

Gebruik de opdracht `cf create-service` om een permanente database in Azure te maken met behulp van Open Service Broker. Met de onderstaande opdracht wordt een PostgreSQL-database ingericht in Azure in de resourcegroep *MyResourceGroup* in de regio *eastus*. Meer informatie over *resourcegroep*, *locatie* en andere Azure-specifieke JSON-parameters is beschikbaar in de [naslagdocumentatie voor de PostgreSQL-module](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md#provision):

```cmd
cf create-service azure-postgresql-9-6 general-purpose mypgsql -c '{"resourceGroup":"MyResourceGroup", "location":"eastus"}'
```

De database wordt beschikbaar gemaakt in uw Pivotal Cloud Foundry-exemplaar als een service met de naam *mypgsql*. Nadat de database volledig is ingericht, wat enkele minuten duurt, kunt u deze verbinden met de toepassing. Als u wilt controleren of het inrichten van de database is voltooid, gebruikt u de opdracht `cf services`:

```cmd
cf services

Getting services in org myorg / space dev as admin...

name      service                plan              bound apps     last operation
mypgsql   azure-postgresql-9-6   general-purpose                  create succeeded
```

De waarde bij *laatste bewerking* voor de service is *Maken voltooid* wanneer het inrichten is voltooid.

## <a name="bind-the-database-to-your-application"></a>De database verbinden met uw toepassing

Gebruik de opdracht `bind-service` om de service te verbinden met uw toepassing.

```cmd
cf bind-service spring-music mypgsql
```

Als u de service hebt verbonden met de toepassing, moet u de toepassing opnieuw faseren om de wijzigingen door te voeren.

```cmd
cf restage spring-music
```

De toepassing is [geconfigureerd voor het gebruik van Spring-cloudconnectors](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L45...L46) waardoor uw Pivotal Cloud Foundry-exemplaar kan gebruikmaken van [automatische herconfiguratie](https://docs.cloudfoundry.org/buildpacks/java/configuring-service-connections/spring-service-bindings.html#auto) in de gegevensbron van de toepassing. In dit geval wordt uw toepassing opnieuw geconfigureerd met het Pivotal Cloud Foundry-exemplaar om een service waaraan het is verbonden, te gebruiken als gegevensbron wanneer de toepassing opnieuw wordt gefaseerd.

Nadat de toepassing opnieuw is gefaseerd, wordt *mypgsql* gebruikt om gegevens op te slaan in plaats van de in-memory database.

Alle wijzigingen die u aanbrengt, worden nu behouden tijdens het opnieuw opstarten en opnieuw implementeren. U ziet ook de verbindings-URL die de toepassing gebruikt, als u de logboeken van de toepassing opnieuw weergeeft.

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
2018-12-07T14:48:58.10-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:48:58.107 DEBUG 16 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:postgresql://12345678-aaaa-bbbb-cccc-1234567890ab.postgres.database.azure.com:5432/123456789?user=aabbcc1122@12345678-aaaa-bbbb-cccc-1234567890ab&password=<masked>&&sslmode=require
```

U ziet dan twee vermeldingen:

* De oorspronkelijke waarde *h2:mem:testdb*.
* De bijgewerkte waarde voor de PostgreSQL-database nadat de database opnieuw is gefaseerd.

Als u wilt controleren of gegevens worden behouden in de PostgreSQL-database, navigeert u in de browser naar de toepassing, brengt u enkele wijzigingen aan, en faseert u de toepassing vervolgens opnieuw:

```cmd
cf restage spring-music
```

Nadat de toepassing opnieuw is gefaseerd, navigeert u met een browser naar de toepassing met behulp van dezelfde URL. U ziet nu dat de door u aangebrachte wijzigingen er nog steeds zijn.

## <a name="cleanup"></a>Opschonen

Als u uw toepassing wilt loskoppelen van de database, kunt u de verbinding verbreken met behulp van de opdracht `cf unbind-service`.

```cmd
cf unbind-service spring-music mypgsql
```

Net zoals wanneer u de toepassing verbindt met een service, moet u de toepassing nu opnieuw faseren om deze wijzigingen door te voeren. Met deze actie blijven zowel *mypgsql* als de toepassing behouden, maar de toepassing gebruikt vanaf nu de in-memory database in plaats van *mypgsql*.

Gebruik de opdracht `cf delete-service` om de database te verwijderen. *U kunt deze actie niet ongedaan maken. Wees er daarom zeker van dat u de database wilt verwijderen voordat u verdergaat.*

```cmd
cf delete-service mypgsql
```

Ga als volgt te werk om de toepassing te verwijderen uit uw Pivotal Cloud Foundry-exemplaar:
 
```cmd
cf delete spring-music
```

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie ging over het implementeren van een toepassing in Pivotal Cloud Foundry en over het maken van een database met behulp van Open Service Broker for Azure. De zelfstudie bevatte ook informatie over het verbinden van uw database aan uw toepassing binnen het Pivotal Cloud Foundry-exemplaar. Voor meer informatie over het implementeren van toepassingen in Cloud Foundry in Azure, raadpleegt u:

* [Cloud Foundry in Azure](../virtual-machines/linux/cloudfoundry-get-started.md)
* [Uw eerste app implementeren in Cloud Foundry in Microsoft Azure](../virtual-machines/linux/cloudfoundry-deploy-your-first-app.md)