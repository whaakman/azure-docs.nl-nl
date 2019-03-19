---
title: Ondersteuning voor Java Enterprise op Linux - Azure App Service | Microsoft Docs
description: Developer's guide voor het implementeren van Java Enterprise-apps met behulp van Wildfly met Azure App Service on Linux.
keywords: Azure appservice, web-app, linux, oss, java, wildfly, enterprise, java ee, jee, javaee
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
ms.custom: seodec18
ms.openlocfilehash: 98e00eb382962d2364adda93b1a5b24fdef832eb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58101768"
---
# <a name="java-enterprise-guide-for-app-service-on-linux"></a>Java ondernemingsgids voor App Service onder Linux

> [!NOTE] 
> Java Enterprise Edition voor App Service Linux is momenteel in Preview. Deze stack is **niet** aanbevolen voor productie gerichte werk. Raadpleeg de [Java developer's guide](app-service-linux-java.md) voor meer informatie over onze Java SE en Tomcat-stacks.

Azure App Service on Linux kunnen Java-ontwikkelaars voor het bouwen, implementeren en schalen van Java-bedrijfstoepassingen (Java EE) op een volledig beheerde service op basis van Linux.  De onderliggende Java Enterprise runtime-omgeving is de open-source [Wildfly](https://wildfly.org/) -toepassingsserver.

Deze handleiding bevat de belangrijkste concepten en instructies voor het Java-ontwikkelaars voor Linux in App Service gebruiken. Als u Java-toepassingen met Azure App Service voor Linux nooit hebt geïmplementeerd, moet u uitvoeren de [Java-quickstart](quickstart-java.md) eerste. Vragen over App Service voor Linux die niet specifiek zijn voor Java Enterprise worden beantwoord het [Java developer's guide](app-service-linux-java.md) en de [Veelgestelde vragen over App Service Linux](app-service-linux-faq.md).

## <a name="scale-with-app-service"></a>Schaal met App Service 

De toepassingsserver voor WildFly in App Service op Linux wordt uitgevoerd in de zelfstandige modus, niet in de domeinconfiguratie van een. Wanneer u het App Service-Plan uitschaalt, wordt elke instantie WildFly is geconfigureerd als een zelfstandige server.

 Uw toepassing horizontaal of verticaal schalen met [regels voor schalen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started?toc=%2Fazure%2Fapp-service%2Fcontainers%2Ftoc.json) en door [uw aantal instanties verhoogt](https://docs.microsoft.com/azure/app-service/web-sites-scale?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json). 

## <a name="customize-application-server-configuration"></a>Configuratie van de toepassingsserver aanpassen

Web-App-instanties zijn staatloos, zodat elke nieuw exemplaar gestart bij het opstarten voor de ondersteuning van de Wildfly-configuratie die nodig zijn voor de toepassing moet worden geconfigureerd.
U kunt een opstartscript Bash-script voor het aanroepen van de CLI WildFly te schrijven:

- Instellen van gegevensbronnen
- Messaging-providers configureren
- Andere modules en afhankelijkheden toevoegen aan de configuratie van de Wildfly.

  Het script wordt uitgevoerd wanneer Wildfly actief en werkend is, maar voordat de toepassing wordt gestart. Het script moet worden gebruikt de [JBOSS CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) aangeroepen vanuit `/opt/jboss/wildfly/bin/jboss-cli.sh` application server configureren met een configuratie- of wijzigingen nodig zijn nadat de server wordt gestart. 

Gebruik de interactieve modus van de CLI niet Wildfly configureren. In plaats daarvan u krijgt u een script van opdrachten voor CLI JBoss met behulp van de `--file` opdracht, bijvoorbeeld:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Uploaden van het opstartscript naar `/home/site/deployments/tools` in uw App Service-exemplaar. Zie [dit document](/azure/app-service/deploy-configure-credentials#userscope) voor instructies over het verkrijgen van uw FTP-referenties. 

Stel de **opstartscript** veld in de Azure-portal naar de locatie van uw shell opstartscript bijvoorbeeld `/home/site/deployments/tools/your-startup-script.sh`.

Geef [toepassingsinstellingen](/azure/app-service/web-sites-configure#application-settings) in de configuratie van de toepassing om door te geven van omgevingsvariabelen voor gebruik in het script. Toepassingsinstellingen houden en verbindingsreeksen en andere geheimen die nodig zijn voor het configureren van uw toepassing buiten-versiebeheer.

## <a name="modules-and-dependencies"></a>Modules en afhankelijkheden

Voor het installeren van modules en de bijbehorende afhankelijkheden in het klassepad van de Wildfly via de CLI JBoss, moet u de volgende bestanden in hun eigen directory maken.  Sommige modules en afhankelijkheden moet mogelijk aanvullende configuratie, zoals JNDI naming of andere API-specifieke configuratie, zodat deze lijst een minimale set is van wat u moet een afhankelijkheid configureren in de meeste gevallen.

- Een [XML-module descriptor](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Dit XML-bestand definieert de naam, de kenmerken en de afhankelijkheden van uw module. Dit [module.xml voorbeeldbestand](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) definieert een Postgres-module, het JDBC-afhankelijkheid van de JAR-bestand en andere vereiste module-afhankelijkheden.
- Eventuele benodigde JAR-bestandsafhankelijkheden voor uw module.
- Een script met uw JBoss CLI-opdrachten voor het configureren van de nieuwe module. Dit bestand bevat de opdrachten die worden uitgevoerd door de CLI JBoss de server voor het gebruik van de afhankelijkheid configureren. Zie voor documentatie over de opdrachten voor het toevoegen van modules, gegevensbronnen en messaging-providers, [dit document](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
-  Een opstartscript Bash aanroepen van de CLI JBoss en voer het script uit in de vorige stap. Dit bestand wordt uitgevoerd wanneer uw App Service-exemplaar opnieuw wordt opgestart of wanneer nieuwe exemplaren zijn ingericht in een scale-out.  Deze opstartscript is waar u kunt uitvoeren alle andere configuraties voor uw toepassing zoals de opdrachten JBoss worden doorgegeven aan de JBoss CLI. Dit bestand is ten minste één opdracht uw script JBoss CLI-opdracht doorgeven aan de CLI JBoss: 
   
```bash
`/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli` 
``` 

Zodra u de bestanden en inhoud voor uw module hebt, volgt u de stappen hieronder om de module aan de toepassingsserver Wildfly toevoegen. 

1. Uw bestanden naar de FTP- `/home/site/deployments/tools` in uw App Service-exemplaar. Zie dit document voor instructies over het gebruik van uw FTP-referenties. 
2. In de blade toepassingsinstellingen van de Azure-portal en het "Opstartscript" veld instellen op de locatie van uw shell opstartscript bijvoorbeeld `/home/site/deployments/tools/your-startup-script.sh` .
3. Uw App Service-exemplaar opnieuw opstarten door op de **opnieuw** knop in de **overzicht** sectie van de Portal of met de Azure CLI.

## <a name="data-sources"></a>Gegevensbronnen

Volg dezelfde procedure die hierboven worden beschreven in de sectie Modules installeren en afhankelijkheden Wildfly configureren voor een verbinding met de gegevensbron. U kunt dezelfde stappen voor elke Azure-Database-service.

1. Download het JDBC-stuurprogramma voor de smaak van uw database. Voor het gemak hier worden de stuurprogramma's voor [Postgres](https://jdbc.postgresql.org/download.html) en [MySQL](https://dev.mysql.com/downloads/connector/j/). Het downloaden om het JAR-bestand uitpakken.
2. Volg de stappen overzicht in '-Modules en afhankelijkheden' maakt en uploadt uw XML-module descriptor, JBoss CLI-script, opstartscript en JDBC JAR-afhankelijkheid.


Meer informatie over het configureren van Wildfly met [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7) , [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource), en [SQL-Database](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898) beschikbaar is. U kunt deze aangepaste instructies samen met de algemene benadering hierboven gegevensbrondefinities toevoegen aan uw server.

## <a name="messaging-providers"></a>Messaging-providers

Bericht gestuurde bonen met behulp van Service Bus als het mechanisme voor tekstberichten inschakelen:

1. Gebruik de [Apache QPId JMS messaging-bibliotheek](https://qpid.apache.org/proton/index.html). Met deze afhankelijkheid in uw pom.xml (of andere build-bestand) bevatten voor de toepassing.

2.  Maak [Service Bus-bronnen](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Maak een Azure Service Bus-naamruimte en wachtrij in die naamruimte en een gedeeld toegangsbeleid met verzenden en ontvangen van de mogelijkheden.

3. De sleutel voor gedeelde toegang geven aan uw code door een URL-codering de primaire sleutel van uw beleid of [gebruik de SDK van Service Bus](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Volg de stappen die worden beschreven in de sectie Modules installeren en afhankelijkheden met uw module XML descriptor, .jar afhankelijkheden, JBoss CLI-opdrachten en opstartscript voor de provider JMS. Naast de vier bestanden moet u ook een XML-bestand dat de naam van de JNDI voor de JMS wachtrij en onderwerp bepaalt maken. Zie [deze opslagplaats](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) voor referentie-configuratiebestanden.


## <a name="configure-session-management-caching"></a>Sessie beheer van caching configureren

Standaard App Service on Linux sessiecookies affiniteit wordt gebruikt om ervoor te zorgen dat aanvragen van clients met bestaande sessies worden gerouteerd hetzelfde exemplaar van uw toepassing. Dit standaardgedrag is geen configuratie vereist, maar kent enkele beperkingen:

- Als een exemplaar van de toepassing opnieuw wordt opgestart of omlaag geschaald, wordt de status van de gebruiker-sessie in de toepassingsserver, gaan verloren.
- Als toepassingen time-outinstellingen lang sessie of een vast aantal gebruikers hebt, duurt het even voordat autoscaled nieuwe exemplaren te ontvangen omdat alleen nieuwe sessies worden doorgestuurd naar de zojuist gestarte exemplaren.

U kunt configureren dat Wildfly voor het gebruik van een externe sessie-store, zoals [Azure Cache voor Redis](/azure/azure-cache-for-redis/). U moet [uitschakelen van de ARR-affiniteit voor bestaande exemplaar](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) configuratie van de sessie cookies op basis van routering uitgeschakeld en wordt de geconfigureerde Wildfly sessie store werken zonder tussenkomst toestaan.

## <a name="enable-web-sockets"></a>Websockets inschakelen

Websockets zijn standaard ingeschakeld in App Service. Als u wilt aan de slag met WebSockets in uw toepassing, verwijzen naar [in deze Quick Start](https://github.com/wildfly/quickstart/tree/master/websocket-hello).

## <a name="logs-and-troubleshooting"></a>Logboeken en oplossen van problemen

App Service biedt hulpprogramma's om u te helpen bij het oplossen van problemen met uw toepassing.

-   Logboekregistratie inschakelen door te klikken op **diagnostische logboeken** in het navigatiedeelvenster links. Klik op **bestandssysteem** instellen van uw quotum en retentie opslagperiode en sla uw wijzigingen. U vindt deze logboeken onder `/home/LogFiles/`.
-   [SSH gebruiken om te verbinden met het exemplaar van de](app-service-linux-ssh-support.md) om Logboeken voor het uitvoeren van toepassingen weer te geven.
-   Selectievakje diagnostische logboeken de **diagnostische logboeken** deelvenster van de Portal of met behulp van de Azure CLI-opdracht: ` az webapp log tail --name <your-app-name> --resource-group <your-apps-resource-group> `
