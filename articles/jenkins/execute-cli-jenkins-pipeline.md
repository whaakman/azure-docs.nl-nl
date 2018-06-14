---
title: Uitvoeren van de Azure CLI met Jenkins | Microsoft Docs
description: Informatie over het gebruik van Azure CLI voor het implementeren van een Java-web-app in Azure in Jenkins pijplijn
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
ms.openlocfilehash: 2b568bd22858a42178e2821e0e97a3b4ebdfccd5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
ms.locfileid: "28926927"
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>Implementeren in Azure App Service met Jenkins en de Azure CLI
Als u wilt een Java-web-app implementeren in Azure, kunt u Azure CLI in [Jenkins pijplijn](https://jenkins.io/doc/book/pipeline/). In deze zelfstudie maakt u een CI/CD-pijplijn op een virtuele machine in Azure. U leert onder andere:

> [!div class="checklist"]
> * Een Jenkins-VM maken
> * Jenkins configureren
> * Een web-app maken in Azure
> * Voorbereiden van een GitHub-opslagplaats
> * Jenkins pijplijn maken
> * Uitvoeren van de pijplijn en controleer of de web-app

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Maak en configureer Jenkins exemplaar
Als u nog geen een model Jenkins, begint u met de [oplossingssjabloon](install-jenkins-solution-template.md), waaronder de vereiste [Azure-referenties](https://plugins.jenkins.io/azure-credentials) invoegtoepassing standaard. 

De referentie van de Azure-invoegtoepassing kunt u Microsoft Azure service-principal referenties opslaan in Jenkins. In versie 1.2, hebben we de ondersteuning toegevoegd, zodat deze Jenkins Pipeline de Azure-referenties krijgt. 

Controleer of dat u versie 1.2 of hoger hebt:
* Klik in het dashboard Jenkins op **Jenkins beheren -> Manager van de invoegtoepassing ->** en zoek naar **Azure referentie**. 
* De invoegtoepassing bijwerken als de versie ouder dan 1.2 is.

Java-JDK en Maven zijn ook vereist in het model Jenkins. Als u wilt installeren, aanmelden bij Jenkins master via SSH en voer de volgende opdrachten:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a>Azure-service-principal tot Jenkins referentie toevoegen

Een Azure-referentie is nodig voor het uitvoeren van Azure CLI.

* Klik in het dashboard Jenkins op **referenties -> systeem ->**. Klik op **globale credentials(unrestricted)**.
* Klik op **toevoegen referenties** om toe te voegen een [Microsoft Azure service-principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) door het invullen van de abonnements-ID, de Client-ID, het Clientgeheim en de OAuth 2.0-Tokeneindpunt. Geef een ID voor gebruik in de volgende stap.

![Referenties toevoegen](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Een Azure App Service voor het implementeren van de Java-web-app maken

Maken van een Azure App Service-abonnement met de **vrije** prijzen met behulp van laag de [az appservice-abonnement maken](/cli/azure/appservice/plan#az_appservice_plan_create) CLI-opdracht. Het plan appservice definieert de fysieke resources gebruikt voor het hosten van uw apps. Alle toepassingen die zijn toegewezen aan een appservice-abonnement kunt u deze resources, zodat u kosten opslaan bij het hosten van meerdere apps delen. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
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

 Gebruik de [az webapp maken](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) CLI-opdracht voor het maken van de definitie van een web-app in de `myAppServicePlan` App Service-abonnement. De definitie van de web-app biedt een URL voor toegang tot uw toepassing met en configureert u diverse opties voor het implementeren van uw code naar Azure. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
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

Instellen van de Java runtime-configuratie die uw app met moet de [az appservice web config update](/cli/azure/appservice/web/config#az_appservice_web_config_update) opdracht.

De volgende opdracht configureert u de web-app uit te voeren op een recente Java 8 JDK en [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>Voorbereiden van een GitHub-opslagplaats
Open de [eenvoudige Java-Web-App voor Azure](https://github.com/azure-devops/javawebappsample) opslagplaats. Om te vertakken de opslagplaats op uw eigen GitHub-account, klikt u op de **Fork** knop in de rechterbovenhoek.

* Open in de GitHub-webgebruikersinterface, **Jenkinsfile** bestand. Klik op het potloodpictogram om dit bestand voor het bijwerken van de resourcegroep en de naam van uw web-app op regel 20 en 21 respectievelijk te bewerken.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Wijzig regel 23 verwijzings-ID in uw exemplaar Jenkins bijwerken

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Jenkins pijplijn maken
Jenkins in een webbrowser openen, klikt u op **Nieuw Item**. 

* Geef een naam op voor de taak en selecteer **pijplijn**. Klik op **OK**.
* Klik op de **pijplijn** volgende tabblad. 
* Voor **definitie**, selecteer **Pipeline-script uit SCM**.
* Voor **SCM**, selecteer **Git**.
* Geef de GitHub-URL voor uw opslagplaats Gevorkte: https:\<uw Gevorkte opslagplaats\>.git
* Klik op **opslaan**

## <a name="test-your-pipeline"></a>Test uw pijplijn
* Ga naar de pijplijn die u hebt gemaakt, klikt u op **nu bouwen**
* Een build binnen een paar seconden moet slagen, en kunt u gaat u naar de build en klikt u op **Console-uitvoer** om de details te bekijken

## <a name="verify-your-web-app"></a>Controleer of uw web-app
Om te controleren of het WAR-bestand is geïmplementeerd in uw web-app. Open een webbrowser:

* Ga naar http://&lt;app_naam >.azurewebsites.net/api/calculator/ping  
U ziet:

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Ga naar http://&lt;app_naam >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (vervangen door &lt;x > en &lt;y > met willekeurige getallen) om op te halen van de som van de x en y

![Rekenmachine: toevoegen](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a>Implementeren in Azure-Web-App op Linux
Nu dat u hoe u Azure CLI gebruiken in uw Jenkins pijplijn weet, kunt u het script voor het implementeren van een Azure-Web-App op Linux kunt wijzigen.

Een andere manier voor de implementatie, is het gebruik van Docker biedt ondersteuning voor Web-App op Linux. Als u wilt implementeren, moet u een Dockerfile die uw web-app met service runtime worden verpakt in een Docker-installatiekopie opgeven. De invoegtoepassing wordt vervolgens de installatiekopie van het bouwen, dit doorgeven aan een Docker-register en de installatiekopie implementeren op uw web-app.

* Volg de stappen [hier](../app-service/containers/quickstart-nodejs.md) voor het maken van een Azure-Web-App uitgevoerd op Linux.
* Docker installeren op uw exemplaar Jenkins door de instructies in deze [artikel](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Een register-Container maken in de Azure-portal met behulp van de stappen [hier](/azure/container-registry/container-registry-get-started-azure-cli).
* In dezelfde [eenvoudige Java-Web-App voor Azure](https://github.com/azure-devops/javawebappsample) opslagplaats forked, bewerk de **Jenkinsfile2** bestand:
    * Regel 18-21, respectievelijk naar de namen van de resourcegroep, web-app en ACR bijwerken. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Regel 24-, update \<azsrvprincipal\> aan de referentie-ID
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Maak een nieuwe Jenkins pijplijn zoals u bij het implementeren in Azure-web-app in Windows, alleen deze keer hebt gebruikt **Jenkinsfile2** in plaats daarvan.
* Uw nieuwe taak uitvoeren.
* Om te controleren, in de Azure CLI uitvoeren:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    U krijgen het volgende resultaat:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Ga naar http://&lt;app_naam >.azurewebsites.net/api/calculator/ping. U ziet het bericht: 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Ga naar http://&lt;app_naam >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (vervangen door &lt;x > en &lt;y > met willekeurige getallen) om op te halen van de som van de x en y
    
## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een Jenkins pijplijn die de broncode in GitHub-repo-uitgecheckt geconfigureerd. Maven om samen te stellen van een war-bestand wordt uitgevoerd en vervolgens met behulp van Azure CLI implementeren in Azure App Service. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Jenkins-VM maken
> * Jenkins configureren
> * Een web-app maken in Azure
> * Voorbereiden van een GitHub-opslagplaats
> * Jenkins pijplijn maken
> * Uitvoeren van de pijplijn en controleer of de web-app
