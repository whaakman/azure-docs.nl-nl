---
title: Java APM- en controlehulpprogramma's met Azure App Service op Linux configureren
description: Informatie over het verzenden van Logboeken en metrische gegevens voor uw Java-toepassingen die worden uitgevoerd op App Service Linux NewRelic en de App Dynamics APM-providers
services: app-service\web
author: rloutlaw
manager: angerobe
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 11/27/2018
ms.author: astay;routlaw
ms.openlocfilehash: 06ae71fea1b85a74d87588d2635038c64c8540cc
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52500307"
---
# <a name="how-to-application-performance-monitoring-tools-with-java-apps-on-azure-app-service-on-linux"></a>Instructies: Toepassingsprestaties controlehulpprogramma's met Java-toepassingen in Azure App Service on Linux

Dit artikel leest hoe u Java-toepassingen die zijn geïmplementeerd in Azure App Service op Linux met de NewRelic en AppDynamics bewaking van toepassingsprestaties (APM)-platforms verbinding maakt.

## <a name="configure-new-relic"></a>New Relic configureren
1. Maak een account NewRelic op [NewRelic.com](https://newrelic.com/signup)
1. De Java-agent downloaden van NewRelic, is er een bestandsnaam die vergelijkbaar is met `newrelic-java-x.x.x.zip`.
1. Kopieer de licentiesleutel van uw, moet u het later opnieuw configureren van de agent.
1. [Voeg SSH toe aan uw App Service-exemplaar](/azure/app-service/containers/app-service-linux-ssh-support) en maak een nieuwe map `/home/site/wwwroot/apm`. 
1. De uitgepakte NewRelic Java-agentbestanden uploaden naar een map onder `/home/site/wwwroot/apm`. De bestanden voor de agent moet `/home/site/wwwroot/apm/newrelic`.
1. Wijzig het YAML-bestand op `/home/site/wwwroot/apm/newrelic/newrelic.yml` en vervang de waarde van de licentie voor tijdelijke aanduidingen door uw eigen licentiesleutel.
1. In Azure portal, blader naar uw toepassing in App Service en maak een nieuwe instelling van de toepassing.
    - Als uw app **Java SE**, maken van een omgevingsvariabele met de naam `JAVA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Als u **Tomcat**, maken van een omgevingsvariabele met de naam `CATALINA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Als u al een omgevingsvariabele voor `JAVA_OPTS` of `CATALINA_OPTS`, toevoegen de `javaagent` optie aan het einde van de huidige waarde.

## <a name="configure-appdynamics"></a>AppDynamics configureren
1. Maak een account met AppDynamics op [AppDynamics.com](https://www.appdynamics.com/community/register/)
1. De Java-agent downloaden vanaf de website van AppDynamics, de bestandsnaam is vergelijkbaar met `AppServerAgent-x.x.x.xxxxx.zip`
1. [Voeg SSH toe aan uw App Service-exemplaar](/azure/app-service/containers/app-service-linux-ssh-support) en maak een nieuwe map `/home/site/wwwroot/apm`. 
1. De Java-agentbestanden uploaden naar een map onder `/home/site/wwwroot/apm`. De bestanden voor de agent moet `/home/site/wwwroot/apm/appdynamics`.
1. IIn Azure portal, blader naar uw toepassing in App Service en maak een nieuwe instelling van de toepassing.
    - Als u **Java SE**, maken van een omgevingsvariabele met de naam `JAVA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` waar `<YOUR_SITE_NAME>` is de naam van uw App Service.
    - Als u **Tomcat**, maken van een omgevingsvariabele met de naam `CATALINA_OPTS` met de waarde `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` waar `<YOUR_SITE_NAME>` is de naam van uw App Service.
