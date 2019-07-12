---
title: Ontwikkelen met Node.js op Kubernetes met Azure Dev spaties
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: quickstart
description: Snelle ontwikkeling van Kubernetes met containers, microservices en Node.js op Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 3da6c015d46d2c83dd74c625e1e8eeaee81da2ae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707132"
---
# <a name="quickstart-develop-with-nodejs-on-kubernetes-using-azure-dev-spaces"></a>Quickstart: Ontwikkelen met Node.js op Kubernetes met Azure Dev spaties

In deze handleiding leert u het volgende:

- Azure Dev Spaces instellen met een beheerd Kubernetes-cluster in Azure.
- Iteratief code in containers met behulp van Visual Studio Code ontwikkelen.
- Fouten opsporen in de code in de adresruimte van uw dev vanuit Visual Studio Code.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free) maken.
- [Visual Studio Code geïnstalleerd](https://code.visualstudio.com/download).
- De [Azure Dev spaties](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) -extensie voor Visual Studio Code is geïnstalleerd.
- [Azure CLI geïnstalleerd](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Een Azure Kubernetes Service-cluster maken

U moet maken van een AKS-cluster in een [ondersteunde regio][supported-regions]. De onderstaande opdrachten maakt u een resourcegroep met de naam *MyResourceGroup* en een AKS-cluster met de naam *MyAKS*.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --disable-rbac --generate-ssh-keys
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

Kloon de toepassing vanuit GitHub.

```cmd
git clone https://github.com/Azure/dev-spaces
```

## <a name="prepare-the-sample-application-in-visual-studio-code"></a>Voorbereiden van de voorbeeld-App in Visual Studio Code

Open Visual Studio Code, klik op *bestand* vervolgens *openen...* , gaat u naar de *dev-ruimten/samples/nodejs/ophalen-gestart/webfrontend* directory en op *Open*.

U hebt nu de *webfrontend* project openen in Visual Studio Code. Als u wilt de toepassing uitvoert in de adresruimte van uw dev, genereren de met behulp van de extensie Azure Dev spaties in de opdracht Pallette Docker en Helm-grafiek-assets.

Open het Opdrachtenpalet in Visual Studio Code, te klikken op *weergave* vervolgens *Command Palette*. Begin met typen `Azure Dev Spaces` en klikt u op `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.

![Configuratiebestanden voorbereiden voor Azure Dev spaties](./media/common/command-palette.png)

Wanneer Visual Studio Code u uw openbare eindpunt configureren vraagt, kiest u `Yes` om in te schakelen van een openbaar eindpunt.

![Openbare eindpunt selecteren](media/common/select-public-endpoint.png)

Met deze opdracht zorgt ervoor dat uw project om uit te voeren in Azure Dev spaties door het genereren van een docker-bestand en Helm-grafiek. Genereert ook een *.vscode* map bij foutopsporing in configuratie in de hoofdmap van uw project.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio-code"></a>Ontwikkelen en uitvoeren van code in Kubernetes uit Visual Studio Code

Klik op de *Debug* aan de linkerkant en klik op het pictogram *start Server (AZDS)* aan de bovenkant.

![Server starten](media/get-started-node/debug-configuration-nodejs.png)

Met deze opdracht bouwt en de service wordt uitgevoerd in Azure Dev spaties. De *Terminal* venster onderaan ziet u de uitvoer van build en URL's voor uw service met Azure Dev spaties. De *Foutopsporingsconsole* ziet u de logboekuitvoer van het.

> [!Note]
> Als er geen Azure Dev spaties opdrachten in de *Command Palette*, zorg ervoor dat u hebt geïnstalleerd het [Visual Studio Code-extensie voor Azure Dev spaties](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Controleer ook of u hebt geopend de *dev-ruimten/samples/nodejs/ophalen-gestart/webfrontend* map in Visual Studio Code.

Hier ziet u de service wordt uitgevoerd door het openen van de openbare URL.

Klik op *Debug* vervolgens *Stop Debugging* naar het foutopsporingsprogramma stopt.

## <a name="update-code"></a>Code bijwerken

Voor het implementeren van een bijgewerkte versie van uw service, kunt u elk bestand in uw project bijwerken en opnieuw uitvoeren *Server starten*. Bijvoorbeeld:

1. Als uw toepassing nog steeds wordt uitgevoerd, klikt u op *Debug* vervolgens *Stop Debugging* om deze te stoppen.
1. Update [regel 13 `server.js` ](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13) aan:
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Sla uw wijzigingen op.
1. Voer *Server starten*.
1. Navigeer naar uw actieve service en houd rekening met uw wijzigingen.
1. Klik op *fouten opsporen in* vervolgens *Stop Debugging* stoppen van uw toepassing.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Instellen en het gebruik van onderbrekingspunten voor foutopsporing

Start uw service met *start Server (AZDS)* .

Ga terug naar de *Explorer* weergeven door te klikken op *weergave* vervolgens *Explorer*. Open `server.js` en klik ergens op regel 13 om te zetten de cursor. Om in te stellen van een onderbrekingspunt bereikt *F9* of klik op *Debug* vervolgens *onderbrekingspunt*.

Open uw service in een browser en er geen bericht wordt weergegeven. Ga terug naar Visual Studio Code en bekijk regel 13 is gemarkeerd. Het onderbrekingspunt die u instelt, heeft de service op de regel 13 onderbroken. Hervat de service, bereikt *F5* of klik op *Debug* vervolgens *doorgaan*. Ga terug naar uw browser en u ziet dat het bericht wordt nu weergegeven.

Tijdens het uitvoeren van uw service in Kubernetes met een foutopsporingsprogramma die is gekoppeld, hebt u volledige toegang tot informatie zoals de aanroepstack, lokale variabelen en informatie over de uitzondering voor foutopsporing.

Het onderbrekingspunt verwijderen door de cursor op regel 13 in `server.js` en op *F9*.

Klik op *Debug* vervolgens *Stop Debugging* naar het foutopsporingsprogramma stopt.

## <a name="update-code-from-visual-studio-code"></a>Code van de update van Visual Studio Code

Wijzigen van de foutopsporingsmodus *koppelen aan een Server (AZDS)* en start de service:

![](media/get-started-node/attach-nodejs.png)

Met deze opdracht bouwt en de service wordt uitgevoerd in Azure Dev spaties. Deze ook wordt gestart een [nodemon](https://nodemon.io) verwerken in de container en aangesloten VS Code op deze van uw service. De *nodemon* de keuze biedt voor automatisch opnieuw starten wanneer codewijzigingen bron worden gedaan, zodat sneller binnenste lus ontwikkeling die vergelijkbaar is met het ontwikkelen van op uw lokale computer.

Nadat de service wordt gestart, gaat u naar het met uw browser en ermee.

Terwijl de service wordt uitgevoerd, gaat u terug naar VS Code en werk regel 13 in `server.js`. Bijvoorbeeld:
```javascript
    res.send('Hello from webfrontend in Azure while debugging!');
```

Sla het bestand op en gaat u terug naar uw service in een browser. Communiceren met de service en u ziet dat uw bijgewerkte bericht wordt weergegeven.

Tijdens de uitvoering *nodemon*, het proces van het knooppunt automatisch opnieuw opgestart als u de codewijzigingen worden gedetecteerd. Dit proces automatisch opnieuw opstarten is vergelijkbaar met de ervaring van bewerken en de service op uw lokale computer, biedt een ontwikkelervaring van de binnenste lus opnieuw te starten.

## <a name="clean-up-your-azure-resources"></a>Opschonen van uw Azure-resources

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u Azure Dev spaties kunt complexere toepassingen ontwikkelen in meerdere containers, en hoe kunt u gezamenlijke ontwikkeling vereenvoudigen door te werken met verschillende versies of vertakkingen van uw code in verschillende spaties.

> [!div class="nextstepaction"]
> [Werken met meerdere containers en teamontwikkeling](multi-service-nodejs.md)


[supported-regions]: about.md#supported-regions-and-configurations