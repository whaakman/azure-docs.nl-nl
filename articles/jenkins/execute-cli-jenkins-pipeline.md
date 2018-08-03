---
title: De Azure CLI met Jenkins uitvoeren | Microsoft Docs
description: Informatie over het gebruik van Azure CLI een Java-web-app implementeren in Azure in Jenkins-pijplijn
services: app-service\web
documentationcenter: ''
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 1796e9f76e39334c8bbdd03463a0f91e9b47cb17
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421301"
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>Implementeren in Azure App Service met Jenkins en Azure CLI
Als u wilt een Java-web-app implementeren in Azure, kunt u Azure CLI in [Jenkins pijplijn](https://jenkins.io/doc/book/pipeline/). In deze zelfstudie maakt u een CI/CD-pijplijn op een virtuele machine in Azure. U leert onder andere:

> [!div class="checklist"]
> * Een Jenkins-VM maken
> * Jenkins configureren
> * Een web-app maken in Azure
> * Een GitHub-opslagplaats voorbereiden
> * Jenkins-pijplijn maken
> * Voer de pijplijn en controleer of de web-app

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Maken en configureren van Jenkins-exemplaar
Als u nog geen een Jenkins-master, begint u met de [oplossingssjabloon](install-jenkins-solution-template.md), waaronder de vereiste [Azure Credentials](https://plugins.jenkins.io/azure-credentials) invoegtoepassing standaard. 

De invoegtoepassing Azure Credential biedt u referenties voor Microsoft Azure service-principal in Jenkins opslaan. In versie 1.2, we ondersteuning toegevoegd, zodat die Jenkins-pijplijn tot de Azure-referenties krijgen kunt. 

Zorg ervoor dat u versie 1.2 of hoger hebt:
* Klik in het dashboard van Jenkins op **Manage Jenkins-invoegtoepassing Manager > ->** en zoek naar de **Azure Credential**. 
* De invoegtoepassing bijwerken als de versie ouder dan 1.2 is.

Java JDK en Maven zijn ook vereist in de Jenkins-master. Als u wilt installeren, meld u aan bij de Jenkins-master met behulp van SSH en voer de volgende opdrachten uit:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a>Azure service-principal toevoegen aan de Jenkins-referentie

Een Azure-referentie is nodig voor het uitvoeren van Azure CLI.

* Klik in het dashboard van Jenkins op **referenties -> systeem ->**. Klik op **globale credentials(unrestricted)**.
* Klik op **referenties toevoegen** om toe te voegen een [Microsoft Azure service-principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) door het invullen van de abonnements-ID, Client-ID, Client Secret en OAuth 2.0 Token Endpoint. Geef een ID voor gebruik in de volgende stap.

![Referenties toevoegen](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Een Azure App Service voor het implementeren van de Java-web-app maken

Maak een Azure App Service-plan met de **gratis** prijzen laag met behulp van de [az appservice plan maken](/cli/azure/appservice/plan#az-appservice-plan-create) CLI-opdracht. Het App Service-plan definieert de fysieke resources die gebruikt worden voor het hosten van uw apps. Alle toepassingen die zijn toegewezen aan een App Service-plan delen deze resources, zodat u kosten kunt besparen als u meerdere apps host. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
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

 Gebruik de [az webapp maken](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) CLI-opdracht voor het maken van de definitie van een web-app in de `myAppServicePlan` App Service-plan. De definitie van de web-app biedt een URL voor toegang tot uw toepassing en configureert diverse opties voor het implementeren van uw code naar Azure. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
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

Instellen van de configuratie van de Java-runtime die uw app nodig met heeft de [az appservice web config update](/cli/azure/appservice/web/config#az-appservice-web-config-update) opdracht.

De volgende opdracht configureert de web-app om op een recente Java 8 JDK en [Apache Tomcat](http://tomcat.apache.org/) 8.0 te worden uitgevoerd.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>Een GitHub-opslagplaats voorbereiden
Open de [eenvoudige Java-Web-App voor Azure](https://github.com/azure-devops/javawebappsample) opslagplaats. Als u wilt splitsen in de opslagplaats naar uw eigen GitHub-account, klikt u op de **Fork** knop in de rechterbovenhoek.

* Open in GitHub-Webgebruikersinterface, **Jenkinsfile** bestand. Klik op het potloodpictogram om dit bestand voor het bijwerken van de resourcegroep en de naam van uw web-app op regel 20 en 21 respectievelijk te bewerken.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Wijzig regel 23 referentie-ID in uw Jenkins-exemplaar bijwerken

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Jenkins-pijplijn maken
Jenkins in een webbrowser openen, klikt u op **Nieuw Item**. 

* Geef een naam op voor de taak en selecteer **pijplijn**. Klik op **OK**.
* Klik op de **pijplijn** volgende tabblad. 
* Voor **definitie**, selecteer **Pipeline-script uit SCM**.
* Voor **SCM**, selecteer **Git**.
* Voer de GitHub-URL voor uw Gevorkte opslagplaats: https:\<uw Gevorkte opslagplaats\>.git
* Klik op **Opslaan**.

## <a name="test-your-pipeline"></a>Test uw pijplijn
* Ga naar de pijplijn die u hebt gemaakt, klikt u op **Build Now**
* Een build in een paar seconden moet slagen, en kunt u gaat u naar de build en klikt u op **Console-uitvoer** om de details te bekijken

## <a name="verify-your-web-app"></a>Controleer of uw web-app
Om te controleren of het WAR-bestand is geïmplementeerd in uw web-app. Open een webbrowser:

* Ga naar http://&lt;app_name >.azurewebsites.net/api/calculator/ping  
U ziet:

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Ga naar http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Vervang &lt;x > en &lt;y > met willekeurige getallen) om op te halen van de som van de x- en y

![Calculator: toevoegen](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a>Implementeren naar Azure WebApp on Linux
Nu dat u hoe u Azure CLI gebruiken in uw Jenkins-pijplijn weet, kunt u het script te implementeren naar een Azure-Web-App op Linux kunt wijzigen.

Web App on Linux biedt ondersteuning voor een andere manier voor de implementatie, is het gebruik van Docker. Als u wilt implementeren, moet u een docker-bestand dat uw web-app met service-runtime-in een Docker-installatiekopie pakketten opgeven. De invoegtoepassing wordt vervolgens de installatiekopie van het bouwen, deze naar een Docker-register pushen en de installatiekopie implementeert op uw web-app.

* Volg de stappen [hier](../app-service/containers/quickstart-nodejs.md) te maken van een Azure-Web-App die wordt uitgevoerd op Linux.
* Docker installeren op uw Jenkins-instantie door de instructies in deze [artikel](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Een Containerregister maken in Azure portal met behulp van de stappen [hier](/azure/container-registry/container-registry-get-started-azure-cli).
* In dezelfde [eenvoudige Java-Web-App voor Azure](https://github.com/azure-devops/javawebappsample) opslagplaats u gesplitst, bewerken de **Jenkinsfile2** bestand:
    * Lijn van 18-21, bij te werken aan de namen van uw resourcegroep, web-app en ACR respectievelijk. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Regel 24 uur per dag, update \<azsrvprincipal\> naar uw referentie-ID
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Een nieuwe pijplijn met Jenkins maken als u bij het implementeren in Azure-web-app in Windows, alleen deze keer gebruikt **Jenkinsfile2** in plaats daarvan.
* Uw nieuwe taak uitvoeren.
* Als u wilt controleren, in de Azure CLI, voert u de volgende uit:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Krijgt u het volgende resultaat:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Ga naar http://&lt;app_name >.azurewebsites.net/api/calculator/ping. U ziet het bericht: 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Ga naar http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Vervang &lt;x > en &lt;y > met willekeurige getallen) om op te halen van de som van de x- en y
    
## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een Jenkins-pijplijn die de broncode in GitHub-repo uitgecheckt geconfigureerd. Maven voor het bouwen van een war-bestand wordt uitgevoerd en vervolgens Azure CLI gebruikt om te implementeren in Azure App Service. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Jenkins-VM maken
> * Jenkins configureren
> * Een web-app maken in Azure
> * Een GitHub-opslagplaats voorbereiden
> * Jenkins-pijplijn maken
> * Voer de pijplijn en controleer of de web-app
