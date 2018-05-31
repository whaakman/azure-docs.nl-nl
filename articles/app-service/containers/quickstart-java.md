---
title: Een quickstart voor het maken van een Java web-app in Azure App Service in Linux
description: In deze quickstart implementeert u in een paar minuten uw eerste Java-app (Hallo wereld) in Azure App Service in Linux.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/07/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: 2018f5b7051f2b6906372dad3319c763974b93b1
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34355182"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Quickstart: Een Java web-app maken in Azure App Service in Linux

App Service on Linux biedt momenteel een preview-functie ter ondersteuning van Java-web-apps. Zie de [aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie over previews. 

[App Service in Linux](app-service-linux-intro.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie via het Linux-besturingssysteem. Deze quickstart laat u zien hoe u de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) met de [Maven-invoegtoepassing voor Azure Web Apps (preview)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) gebruikt om een Java-web-app met een ingebouwde Linux-installatiekopie te implementeren.

![Voorbeeld-app die wordt uitgevoerd in Azure](media/quickstart-java/java-hello-world-in-browser.png)

[Het implementeren van Java-web-apps in een Linux-container in de cloud met behulp van de Azure Toolkit voor IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-hello-world-web-app-linux) is een alternatieve methode om uw Java-app te implementeren in uw eigen container.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart: 

* [Azure CLI 2.0 of hoger](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) moet lokaal zijn geïnstalleerd.
* [Apache Maven](http://maven.apache.org/).



## <a name="create-a-java-app"></a>Een Java-app maken

Voer de volgende opdracht uit met behulp van Maven om een nieuwe *helloworld*-web-app te maken:  

    mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp

Ga naar de nieuwe *helloworld*-projectmap en bouw alle modules met behulp van de volgende opdracht:

    mvn verify

Met deze opdracht worden alle modules geverifieerd en gemaakt, waaronder het *helloworld.war*-bestand in de submap *helloworld/doel*.


## <a name="deploying-the-java-app-to-app-service-on-linux"></a>De Java-app implementeren in App Service on Linux

Er zijn meerdere implementatieopties voor het implementeren van uw Java-web-apps in App Service in Linux. Het gaat om deze opties:

* [Implementeren via de Maven-invoegtoepassing voor Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)
* [Implementeren via ZIP of WAR](https://docs.microsoft.com/azure/app-service/app-service-deploy-zip)
* [Implementeren via FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)

In deze quickstart gebruikt u de Maven-invoegtoepassing voor Azure Web Apps. Dit heeft voordelen in die zin dat het eenvoudig te gebruiken is vanuit Maven is, en de benodigde Azure-resources voor u worden gemaakt (resourcegroep, app-serviceplan en web-app).

### <a name="deploy-with-maven"></a>Implementeren met Maven

Voor implementeren vanuit Maven voegt u de volgende invoegtoepassingsdefinitie toe binnen het element `<build>` van het bestand *pom.xml*:

```xml
    <plugins>
      <plugin>
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-webapp-maven-plugin</artifactId> 
        <version>1.1.0</version>
        <configuration> 
          <resourceGroup>YOUR_RESOURCE_GROUP</resourceGroup> 
          <appName>YOUR_WEB_APP</appName> 
          <linuxRuntime>tomcat 9.0-jre8</linuxRuntime>
          <deploymentType>ftp</deploymentType> 
          <resources> 
              <resource> 
                  <directory>${project.basedir}/target</directory> 
                  <targetPath>webapps</targetPath> 
                  <includes> 
                      <include>*.war</include> 
                  </includes> 
                  <excludes> 
                      <exclude>*.xml</exclude> 
                  </excludes> 
              </resource> 
          </resources> 
        </configuration>
      </plugin>
    </plugins>
```    

Werk de volgende tijdelijke aanduidingen bij in de configuratie van de invoegtoepassing:

| Tijdelijke aanduiding | Beschrijving |
| ----------- | ----------- |
| `YOUR_RESOURCE_GROUP` | Naam voor de nieuwe resourcegroep waarin de web-app moet worden gemaakt. Door alle resources voor een app in een groep te plaatsen, kunt u ze samen beheren. Als u de resourcegroep verwijdert, worden bijvoorbeeld alle resources verwijderd die bij de app behoren. Wijzig deze waarde in een unieke nieuwe resourcegroepnaam, bijvoorbeeld *TestResources*. U gebruikt deze resourcegroepnaam om alle Azure-resources in een volgende sectie op te schonen. |
| `YOUR_WEB_APP` | De app-naam maakt deel uit van de hostnaam voor de web-app wanneer deze in Azure is geïmplementeerd (YOUR_WEB_APP.azurewebsites.net). Wijzig deze waarde in een unieke naam voor de nieuwe Azure-web-app, die uw Java-app host, bijvoorbeeld *contoso*. |

Het element `linuxRuntime` van de configuratiebesturingselementen bepaalt welke ingebouwde Linux-installatiekopie wordt gebruikt met uw toepassing.

Voer de volgende opdracht uit en volg alle instructies om te verifiëren bij de Azure CLI:

    az login

Implementeer uw Java-app in de web-app met de volgende opdracht:

    mvn clean package azure-webapp:deploy


Zodra de implementatie is voltooid, bladert u naar de geïmplementeerde toepassing door de volgende URL te volgen in uw webbrowser.

```bash
http://<app_name>.azurewebsites.net/helloworld
```

De Java-voorbeeldcode wordt uitgevoerd in een web-app met een ingebouwde installatiekopie.

![Voorbeeld-app die wordt uitgevoerd in Azure](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Gefeliciteerd!** U hebt uw eerste Java-app geïmplementeerd in App Service on Linux.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u Maven gebruikt voor het maken van een Java-web-app en hebt u vervolgens de Java-web-app geïmplementeerd in App Service in Linux. Volg de onderstaande koppeling voor meer informatie over het gebruik van Java in Azure.

> [!div class="nextstepaction"]
> [Azure for Java Developers](https://docs.microsoft.com/java/azure/) (Azure voor Java-ontwikkelaars)

