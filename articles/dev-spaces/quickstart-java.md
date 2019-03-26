---
title: Ontwikkelen met Java op Kubernetes met Azure Dev spaties
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
ms.author: zarhoads
ms.date: 03/22/2019
ms.topic: quickstart
description: Snelle ontwikkeling van Kubernetes met containers, microservices en Java in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Java, Helm, NET service, service mesh-routering, kubectl, k8s
manager: jeconnoc
ms.openlocfilehash: 26f82427ff06608de39381b4ecc45d318212a8a0
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419010"
---
# <a name="quickstart-develop-with-java-on-kubernetes-using-azure-dev-spaces"></a>Quickstart: Ontwikkelen met Java op Kubernetes met Azure Dev spaties

In deze handleiding leert u het volgende:

- Azure Dev Spaces instellen met een beheerd Kubernetes-cluster in Azure.
- Ontwikkel iteratief code in containers met behulp van Visual Studio Code en de opdrachtregel.
- Fouten opsporen in de code in de adresruimte van uw dev vanuit Visual Studio Code.


## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen account hebt, kunt u [een gratis account aanmaken](https://azure.microsoft.com/free).
- [Visual Studio Code geïnstalleerd](https://code.visualstudio.com/download).
-  De [Azure Dev spaties](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) en [foutopsporing voor Java voor Azure Dev spaties](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds) uitbreidingen voor Visual Studio Code is geïnstalleerd.
- [Azure CLI geïnstalleerd](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Maven geïnstalleerd en geconfigureerd](https://maven.apache.org).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Een Azure Kubernetes Service-cluster maken

U moet maken van een AKS-cluster in een [ondersteunde regio](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams). De onderstaande opdrachten maakt u een resourcegroep met de naam *MyResourceGroup* en een AKS-cluster met de naam *MyAKS*.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-count 1 --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Azure Dev spaties inschakelen in uw AKS-cluster

Gebruik de `use-dev-spaces` opdracht uit om te ontwikkelen spaties inschakelen in uw AKS-cluster en volg de aanwijzingen. De onderstaande opdracht schakelt u Dev opslagruimten op de *MyAKS* -cluster in de *MyResourceGroup* groeperen en maakt u een *standaard* dev-ruimte.

```cmd
$ az aks use-dev-spaces -g MyResourceGroup -n MyAKS

'An Azure Dev Spaces Controller' will be created that targets resource 'MyAKS' in resource group 'MyResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'MyAKS' in resource group 'MyResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

## <a name="get-sample-application-code"></a>Voorbeeldcode voor toepassing

In dit artikel gebruikt u de [Azure Dev spaties voorbeeldtoepassing](https://github.com/Azure/dev-spaces) te laten zien hoe u Azure Dev spaties.

De toepassing uit GitHub te klonen en navigeer naar de *dev-ruimten/samples/java/ophalen-gestart/webfrontend* directory:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/java/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>De toepassing voorbereiden

De Docker en Helm-grafiek activa genereren voor het uitvoeren van de toepassing in Kubernetes met behulp van de `azds prep` opdracht:

```cmd
azds prep --public
```

U moet uitvoeren de `prep` opdracht uit vanaf de *dev-ruimten/samples/java/ophalen-gestart/webfrontend* directory voor het genereren van de activa van de grafiek Docker en Helm correct.

## <a name="build-and-run-code-in-kubernetes"></a>Code schrijven en uitvoeren in Kubernetes

Ontwikkelen en uitvoeren van uw code in AKS met behulp van de `azds up` opdracht:

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...3s
Installing Helm chart...8s
Waiting for container image build...28s
Building container image...
Step 1/8 : FROM maven:3.5-jdk-8-slim
Step 2/8 : EXPOSE 8080
Step 3/8 : WORKDIR /usr/src/app
Step 4/8 : COPY pom.xml ./
Step 5/8 : RUN /usr/local/bin/mvn-entrypoint.sh     mvn package -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true --fail-never
Step 6/8 : COPY . .
Step 7/8 : RUN mvn package -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true
Step 8/8 : ENTRYPOINT ["java","-jar","target/webfrontend-0.1.0.jar"]
Built container image in 37s
Waiting for container...57s
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:54256
...
```

U kunt zien dat de service wordt uitgevoerd door het openen van de openbare URL, die wordt weergegeven in de uitvoer van de `azds up` opdracht. In dit voorbeeld wordt de openbare URL is *http://webfrontend.1234567890abcdef1234.eus.azds.io/*.

Als u stopt de `azds up` met behulp van de opdracht *Ctrl + c*, de service wordt nog uitgevoerd in AKS en de openbare URL blijven beschikbaar.

## <a name="update-code"></a>Code bijwerken

Voor het implementeren van een bijgewerkte versie van uw service, kunt u elk bestand in uw project bijwerken en opnieuw de `azds up` opdracht. Bijvoorbeeld:

1. Als `azds up` is nog steeds uitgevoerd, drukt u op *Ctrl + c*.
1. Update [16 in regel `src/main/java/com/ms/sample/webfrontend/Application.java` ](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/webfrontend/src/main/java/com/ms/sample/webfrontend/Application.java#L16) aan:
    
    ```java
    return "Hello from webfrontend in Azure!";
    ```

1. Sla uw wijzigingen op.
1. Voer de `azds up` opdracht:

    ```cmd
    $ azds up
    Using dev space 'default' with target 'MyAKS'
    Synchronizing files...1s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Navigeer naar uw actieve service en houd rekening met uw wijzigingen.
1. Druk op *Ctrl + c* stoppen de `azds up` opdracht.

## <a name="enable-visual-studio-code-to-debug-in-kubernetes"></a>Inschakelen van Visual Studio Code om op te sporen in Kubernetes

Open Visual Studio Code, klik op *bestand* vervolgens *openen...* , gaat u naar de *dev-ruimten/samples/java/ophalen-gestart/webfrontend* directory en op *Open*.

U hebt nu de *webfrontend* project openen in Visual Studio Code, die u hebt uitgevoerd met behulp van dezelfde service is de `azds up` opdracht. Fouten opsporen in deze service in met behulp van Visual Studio Code, in plaats van met behulp van AKS `azds up` rechtstreeks, moet u dit project voor Visual Studio Code gebruiken om te communiceren met de adresruimte van uw dev voorbereiden.

Open het Opdrachtenpalet in Visual Studio Code, te klikken op *weergave* vervolgens *Command Palette*. Begin met typen `Azure Dev Spaces` en klikt u op `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.

![Configuratiebestanden voorbereiden voor Azure Dev spaties](./media/common/command-palette.png)

Wanneer Visual Studio Code u om uw basisinstallatiekopieën en blootgestelde poorten te configureren vraagt, kiest u `Azul Zulu OpenJDK for Azure (Free LTS)` voor de basisinstallatiekopie en `8080` voor de beschikbaar gemaakte poort.

![Selecteer de basisinstallatiekopie](media/get-started-java/select-base-image.png)

![Selecteer beschikbaar gestelde poort](media/get-started-java/select-exposed-port.png)

Met deze opdracht zorgt ervoor dat uw project om uit te voeren in Azure Dev spaties rechtstreeks vanuit Visual Studio Code. Genereert ook een *.vscode* map bij foutopsporing in configuratie in de hoofdmap van uw project.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio"></a>Ontwikkelen en uitvoeren van code in Kubernetes vanuit Visual Studio

Klik op de *Debug* aan de linkerkant en klik op het pictogram *starten Java programma (AZDS)* aan de bovenkant.

![Java-programma te starten](media/get-started-java/debug-configuration.png)

Met deze opdracht bouwt en de service wordt uitgevoerd in Azure Dev spaties in de modus voor foutopsporing. De *Terminal* venster onderaan ziet u de uitvoer van build en URL's voor uw service met Azure Dev spaties. De *Foutopsporingsconsole* ziet u de logboekuitvoer van het.

> [!Note]
> Als er geen Azure Dev spaties opdrachten in de *Command Palette*, zorg ervoor dat u hebt geïnstalleerd het [Visual Studio Code-extensie voor Azure Dev spaties](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Controleer ook of u hebt geopend de *dev-ruimten/samples/java/ophalen-gestart/webfrontend* map in Visual Studio Code.

Klik op *Debug* vervolgens *Stop Debugging* naar het foutopsporingsprogramma stopt.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Instellen en het gebruik van onderbrekingspunten voor foutopsporing

Start de service tijdens de foutopsporing modus met *starten Java programma (AZDS)*.

Ga terug naar de *Explorer* weergeven door te klikken op *weergave* vervolgens *Explorer*. Open `src/main/java/com/ms/sample/webfrontend/Application.java` en klik ergens op regel 16 om te zetten de cursor. Om in te stellen van een onderbrekingspunt bereikt *F9* of klik op *Debug* vervolgens *onderbrekingspunt*.

Open uw service in een browser en er geen bericht wordt weergegeven. Ga terug naar Visual Studio Code en bekijk regel 16 is gemarkeerd. Het onderbrekingspunt die u instelt, heeft de service op de regel 16 onderbroken. Hervat de service, bereikt *F5* of klik op *Debug* vervolgens *doorgaan*. Ga terug naar uw browser en u ziet dat het bericht wordt nu weergegeven.

Tijdens het uitvoeren van uw service in Kubernetes met een foutopsporingsprogramma die is gekoppeld, hebt u volledige toegang tot informatie zoals de aanroepstack, lokale variabelen en informatie over de uitzondering voor foutopsporing.

Het onderbrekingspunt verwijderen door de cursor op regel 16 in `src/main/java/com/ms/sample/webfrontend/Application.java` en op *F9*.

## <a name="update-code-from-visual-studio-code"></a>Code van de update van Visual Studio Code

Terwijl de service wordt uitgevoerd in de modus voor foutopsporing, werkt u regel 16 in `src/main/java/com/ms/sample/webfrontend/Application.java`. Bijvoorbeeld:
```java
return "Hello from webfrontend in Azure while debugging!";
```

Sla het bestand op. Klik op *fouten opsporen in* vervolgens *opnieuw foutopsporing* of in de *werkbalk Foutopsporing*, klikt u op de *opnieuw foutopsporing* knop.

![Foutopsporing vernieuwen](media/get-started-java/debug-action-refresh.png)

Open uw service in een browser en bekijk dat uw bijgewerkte bericht wordt weergegeven.

In plaats van opnieuw te bouwen en opnieuw implementeren van een nieuwe containerinstallatiekopie telkens wanneer de code wijzigingen zijn aangebracht, Azure Dev spaties incrementeel gecompileerd code binnen de bestaande container voor een snellere bewerken/debug-lus.

## <a name="clean-up-your-azure-resources"></a>Opschonen van uw Azure-resources

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u Azure Dev spaties kunt complexere toepassingen ontwikkelen in meerdere containers, en hoe kunt u gezamenlijke ontwikkeling vereenvoudigen door te werken met verschillende versies of vertakkingen van uw code in verschillende spaties.

> [!div class="nextstepaction"]
> [Werken met meerdere containers en teamontwikkeling](multi-service-java.md)
