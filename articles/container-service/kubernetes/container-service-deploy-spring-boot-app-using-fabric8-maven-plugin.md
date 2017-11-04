---
title: Een opstartinstallatiekopie Spring-app met behulp van de Fabric8 Maven-invoegtoepassing implementeren
description: Deze zelfstudie wordt u echter de stappen voor het implementeren van een toepassing Spring opstarten op Microsoft Azure met behulp van de invoegtoepassing Fabric8 voor Apache Maven.
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/15/2017
ms.author: yuwzho;robmcm
ms.custom: 
ms.openlocfilehash: 6ec48caef0eebeacf260b0dbc91b175368faea69
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a>Een opstartinstallatiekopie Spring-app met behulp van de Fabric8 Maven-invoegtoepassing implementeren

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

**[Fabric8]**  is een open source-oplossing die is gebaseerd op  **[Kubernetes]**, waarmee ontwikkelaars maken van toepassingen in Linux containers.

Deze zelfstudie leert u met behulp van de invoegtoepassing Fabric8 voor Maven te ontwikkelen voor het implementeren van een toepassing met een Linux-host in de [Azure Container Service (ACS)].

## <a name="prerequisites"></a>Vereisten

U moet de stappen in deze zelfstudie hebt voltooid, hebt de volgende vereisten:

* Een Azure-abonnement; Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee] of zich aanmelden voor een [gratis Azure-account].
* De [Azure-opdrachtregelinterface (CLI)].
* Een up-to-date [Java Developer Kit (JDK)].
* Apache van [Maven] bouwen tool (versie 3).
* Een [Git] client.
* Een [Docker] client.

> [!NOTE]
>
> Vanwege de virtualisatie-vereisten van deze zelfstudie, kan niet u de stappen in dit artikel volgen op een virtuele machine; u moet een fysieke computer gebruiken met virtualisatiefuncties ingeschakeld.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Maken van de opstartinstallatiekopie Spring op web-app Docker aan de slag

De volgende stappen maakt u bij het bouwen van een webtoepassing Spring opstarten en lokaal te testen.

1. Open een opdrachtprompt en maak een lokale map houdt uw toepassing en wijzig in die map; bijvoorbeeld:
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   -- of--
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. Kloon de [Spring opstarten op het Docker aan de slag] voorbeeldproject naar de map.
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Wijzig de directory aan het project voltooide; bijvoorbeeld:
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   -- of--
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. Maven gebruiken om te bouwen en uitvoeren van de voorbeeld-app.
   ```shell
   mvn clean package spring-boot:run
   ```

1. De web-app testen door te bladeren naar http://localhost: 8080 of met de volgende `curl` opdracht:
   ```shell
   curl http://localhost:8080
   ```

   U ziet een **Docker HelloWorld** bericht wordt weergegeven.

   ![Voorbeeld van een toepassing lokaal bladeren][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a>Installeer de opdrachtregelinterface Kubernetes en maak een Azure-resourcegroep met de Azure CLI

1. Open een opdrachtprompt.

1. Typ de volgende opdracht voor het aanmelden bij uw Azure-account:
   ```azurecli
   az login
   ```
   Volg de instructies voor de aanmelding te voltooien
   
   De Azure CLI wordt een lijst weergeven van uw accounts; bijvoorbeeld:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. Als u nog geen Kubernetes-opdrachtregelinterface (`kubectl`) geïnstalleerd, kunt u installeren met de Azure CLI; bijvoorbeeld:
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > Linux-gebruikers hebben mogelijk tot het voorvoegsel van deze opdracht met `sudo` sinds deze de CLI Kubernetes naar implementeert `/usr/local/bin`.
   >
   > Als u al hebt `kubectl`) geïnstalleerd en uw versie van `kubectl` is te oud is, wordt er een foutbericht weergegeven dat vergelijkbaar is met het volgende voorbeeld wanneer u probeert de stappen die verderop in dit artikel uit te voeren:
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > Als dit gebeurt, moet u opnieuw installeren `kubectl` de nieuwste versie.
   >

1. Maak een resourcegroep voor de Azure-resources die u in deze zelfstudie gebruiken wilt; bijvoorbeeld:
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   Waar:  
      * *wingtiptoys kubernetes* is een unieke naam voor de resourcegroep  
      * *westeurope* is een juiste geografische locatie voor uw toepassing  

   De Azure CLI wordt weergegeven de resultaten van het maken van uw resource groep; bijvoorbeeld:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a>Maak een Kubernetes-cluster met de Azure CLI

1. Maken van een cluster Kubernetes in uw nieuwe resourcegroep; bijvoorbeeld:  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   Waar:  
      * *wingtiptoys kubernetes* is de naam van de resourcegroep uit eerder in dit artikel  
      * *wingtiptoys cluster* is een unieke naam voor uw cluster Kubernetes
      * *wingtiptoys* is een unieke naam DNS-naam voor uw toepassing

   De Azure CLI wordt weergegeven de resultaten van het maken van uw cluster; bijvoorbeeld:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. Uw referenties voor het nieuwe cluster Kubernetes downloaden bijvoorbeeld:  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. Controleer of de verbinding met de volgende opdracht:
   ```shell 
   kubectl get nodes
   ```

   U ziet een lijst met knooppunten en status op het volgende voorbeeld:

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a>Maken van een container voor persoonlijke Azure-register met de Azure CLI

1. Een register persoonlijke Azure-container maken in de resourcegroep voor het hosten van uw installatiekopie Docker; bijvoorbeeld:
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   Waar:  
      * *wingtiptoys kubernetes* is de naam van de resourcegroep uit eerder in dit artikel  
      * *wingtiptoysregistry* is een unieke naam voor uw persoonlijke register
      * *westeurope* is een juiste geografische locatie voor uw toepassing  

   De Azure CLI wordt weergegeven de resultaten van het maken van uw register; bijvoorbeeld:  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

1. Het wachtwoord voor uw register container ophalen met de Azure CLI.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   De Azure CLI, ziet u het wachtwoord voor uw register. bijvoorbeeld:  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Navigeer naar de configuratiemap voor uw installatie Maven (standaard ~/.m2/ of C:\Users\username\.m2) en open de *settings.xml* bestand met een teksteditor.

1. Uw Azure-Container register-URL, gebruikersnaam en wachtwoord toevoegen aan een nieuwe `<server>` verzameling in de *settings.xml* bestand.
De `id` en `username` zijn de naam van het register. Gebruik de `password` waarde uit de vorige opdracht (zonder aanhalingstekens).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten (bijvoorbeeld '*C:\SpringBoot\gs-spring-boot-docker\complete*"of"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete* '), en open de *pom.xml* bestand met een teksteditor.

1. Update de `<properties>` verzameling in de *pom.xml* -bestand met de waarde van de server aanmelding voor uw Azure-Container-register.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Update de `<plugins>` verzameling in de *pom.xml* bestand zodat de `<plugin>` bevat van de server en het register aanmeldingsnaam voor uw Azure-Container-register.

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten en voer de volgende Maven-opdracht voor het bouwen van de Docker-container en de installatiekopie van het push naar het register:

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   Maven wordt weergegeven de resultaten van uw build; bijvoorbeeld:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a>Configureer uw app Spring opstarten om de invoegtoepassing Fabric8 Maven gebruiken

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten (bijvoorbeeld: '*C:\SpringBoot\gs-spring-boot-docker\complete*"of"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / volledige*'), en open de *pom.xml* bestand met een teksteditor.

1. Update de `<plugins>` verzameling in de *pom.xml* bestand naar de Fabric8 Maven-invoegtoepassing toevoegen:

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. Ga naar de map van de belangrijkste bron voor uw toepassing Spring opstarten (bijvoorbeeld: '*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*"of"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / Voltooi/src/main*'), en maak een nieuwe map met de naam '*fabric8*'.

1. Maakt drie YAML fragment-bestanden in de nieuwe *fabric8* map:

   a. Maak een bestand met de naam **deployment.yml** met de volgende inhoud:
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   b. Maak een bestand met de naam **secrets.yml** met de volgende inhoud:
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   c. Maak een bestand met de naam **service.yml** met de volgende inhoud:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. Voer de volgende Maven-opdracht om het bestand Kubernetes resource lijst samen te stellen:

   ```shell
   mvn fabric8:resource
   ```

   Met deze opdracht worden samengevoegd voor alle Kubernetes resource yaml bestanden onder de *src/main/fabric8* map op een YAML-bestand met een lijst van de resource Kubernetes, die rechtstreeks Kubernetes-cluster of exporteren naar een grafiek helm kan worden toegepast.

   Maven wordt weergegeven de resultaten van uw build; bijvoorbeeld:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. Voer de volgende Maven-opdracht toe te passen van het bronbestand van de lijst met uw cluster Kubernetes:

   ```shell
   mvn fabric8:apply
   ```

   Maven wordt weergegeven de resultaten van uw build; bijvoorbeeld:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. Zodra de app is geïmplementeerd met het cluster, query uitvoeren op het externe IP-adres met de `kubectl` toepassing; bijvoorbeeld:
   ```shell
   kubectl get svc -w
   ```

   `kubectl`uw interne en externe IP-adressen; wordt weergegeven bijvoorbeeld:
   
   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```
   
   U kunt het externe IP-adres gebruiken voor het openen van uw toepassing in een webbrowser.

   ![Voorbeeld van een toepassing extern bladeren][SB02]

## <a name="delete-your-kubernetes-cluster"></a>Verwijder uw cluster Kubernetes

Als uw cluster Kubernetes niet langer nodig is, kunt u de `az group delete` opdracht om te verwijderen van de resourcegroep die u alle van de bijbehorende resources verwijdert; bijvoorbeeld:

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het gebruik van opstarten Spring toepassingen in Azure:

* [Een toepassing Spring opstart in de Azure App Service implementeren](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Een toepassing Spring opstarten op Linux in de Azure Container Service implementeren](container-service-deploy-spring-boot-app-on-linux.md)
* [Een toepassing Spring opstarten op een Cluster Kubernetes in de Azure Containerservice implementeren](container-service-deploy-spring-boot-app-on-kubernetes.md)

Voor meer informatie over het gebruik van Azure met Java raadpleegt u het [Azure Java-ontwikkelaarscentrum] en de [Java Tools voor Visual Studio Team Services].

Zie voor meer informatie over het opstarten van de versie die voorjaar op Docker-voorbeeldproject [Spring opstarten op het Docker aan de slag].

Zie voor meer informatie over aan de slag met uw eigen toepassingen Spring opstartinstallatiekopie, de **Spring Initializr** op <https://start.spring.io/>.

Zie voor meer informatie over aan de slag met het maken van een eenvoudige Spring Boot-toepassing de Initializr Spring op <https://start.spring.io/>.

Zie voor meer voorbeelden voor het aangepaste Docker-installatiekopieën gebruiken met Azure [met een aangepaste Docker-installatiekopie voor Azure-Web-App op Linux].

<!-- URL List -->

[Azure-opdrachtregelinterface (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java-ontwikkelaarscentrum]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[met een aangepaste Docker-installatiekopie voor Azure-Web-App op Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools voor Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring opstarten op het Docker aan de slag]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB02.png
