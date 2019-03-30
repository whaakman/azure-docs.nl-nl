---
title: Java APM- en controlehulpprogramma's op Linux - Azure App Service configureren
description: Informatie over het verzenden van Logboeken en metrische gegevens voor uw Java-toepassingen die worden uitgevoerd op App Service Linux NewRelic en de App Dynamics APM-providers
services: app-service\web
author: rloutlaw
manager: angerobe
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 03/21/2019
ms.author: astay;routlaw
ms.custom: seodec18
ms.openlocfilehash: e6a22258266bda18c9ff79590d88e70d512f6c77
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630148"
---
# <a name="how-to-application-performance-monitoring-tools-with-java-apps-on-azure-app-service-on-linux"></a>Uitleg: Hulpprogramma's met Java-toepassingen in Azure App Service on Linux bewaking van toepassingsprestaties

Dit artikel leest hoe u Java-toepassingen die zijn geïmplementeerd in Azure App Service op Linux met de NewRelic en AppDynamics bewaking van toepassingsprestaties (APM)-platforms verbinding maakt.

## <a name="configure-new-relic"></a>New Relic configureren

1. Maak een account NewRelic op [NewRelic.com](https://newrelic.com/signup)
2. De Java-agent downloaden van NewRelic, is er een bestandsnaam die vergelijkbaar is met `newrelic-java-x.x.x.zip`.
3. Kopieer de licentiesleutel van uw, moet u het later opnieuw configureren van de agent.
4. [Voeg SSH toe aan uw App Service-exemplaar](/azure/app-service/containers/app-service-linux-ssh-support) en maak een nieuwe map `/home/site/wwwroot/apm`.
5. De uitgepakte NewRelic Java-agentbestanden uploaden naar een map onder `/home/site/wwwroot/apm`. De bestanden voor de agent moet `/home/site/wwwroot/apm/newrelic`.
6. Wijzig het YAML-bestand op `/home/site/wwwroot/apm/newrelic/newrelic.yml` en vervang de waarde van de licentie voor tijdelijke aanduidingen door uw eigen licentiesleutel.
7. In Azure portal, blader naar uw toepassing in App Service en maak een nieuwe instelling van de toepassing.
    - Als uw app **Java SE**, maken van een omgevingsvariabele met de naam `JAVA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Als u **Tomcat**, maken van een omgevingsvariabele met de naam `CATALINA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Als u **WildFly**, Zie de documentatie van New Relic [hier](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) voor instructies over het installeren van de agent voor Java- en JBoss configuratie.
    - Als u al een omgevingsvariabele voor `JAVA_OPTS` of `CATALINA_OPTS`, toevoegen de `javaagent` optie aan het einde van de huidige waarde.

## <a name="configure-appdynamics"></a>AppDynamics configureren

1. Maak een account met AppDynamics op [AppDynamics.com](https://www.appdynamics.com/community/register/)
1. De Java-agent downloaden vanaf de website van AppDynamics, de bestandsnaam is vergelijkbaar met `AppServerAgent-x.x.x.xxxxx.zip`
1. [Voeg SSH toe aan uw App Service-exemplaar](/azure/app-service/containers/app-service-linux-ssh-support) en maak een nieuwe map `/home/site/wwwroot/apm`.
1. De Java-agentbestanden uploaden naar een map onder `/home/site/wwwroot/apm`. De bestanden voor de agent moet `/home/site/wwwroot/apm/appdynamics`.
1. In Azure portal, blader naar uw toepassing in App Service en maak een nieuwe instelling van de toepassing.
    - Als u **Java SE**, maken van een omgevingsvariabele met de naam `JAVA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` waar `<YOUR_SITE_NAME>` is de naam van uw App Service.
    - Als u **Tomcat**, maken van een omgevingsvariabele met de naam `CATALINA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` waar `<YOUR_SITE_NAME>` is de naam van uw App Service.
    - Als u **WildFly**, Zie de documentatie van AppDynamics [hier](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) voor instructies over het installeren van de agent voor Java- en JBoss configuratie.
