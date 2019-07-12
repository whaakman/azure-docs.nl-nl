---
title: Een toepassing implementeren met een Dockerfile op Kubernetes met Azure Dev spaties
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: quickstart
description: Een microservice met een docker-bestand in AKS met Azure Dev opslagruimten implementeren
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s
manager: gwallace
ms.openlocfilehash: a272047955564ba745d4cf521e560af9d24cfb9e
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706839"
---
# <a name="quickstart-develop-an-application-with-a-dockerfile-on-kubernetes-using-azure-dev-spaces"></a>Quickstart: Een toepassing ontwikkelt met een Dockerfile op Kubernetes met Azure Dev spaties
In deze handleiding leert u het volgende:

- Azure Dev Spaces instellen met een beheerd Kubernetes-cluster in Azure.
- Ontwikkelen en uitvoeren van code in containers met behulp van de opdrachtregel.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free) maken.
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

In dit artikel gebruikt u de [Azure Dev spaties voorbeeldtoepassing](https://github.com/Azure/dev-spaces) te laten zien hoe u Azure Dev spaties. De voorbeeldtoepassing is geschreven in Go, maar u kunt bijna elke taal, zolang u een geldige docker-bestand voor uw toepassing in een container plaatsen en uitvoeren op AKS opgeven.

De toepassing uit GitHub te klonen en navigeer naar de *dev-ruimten/samples/golang/ophalen-gestart/webfrontend* directory:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/golang/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>De toepassing voorbereiden

Als uw toepassing uitvoeren op Azure Dev spaties, moet u een docker-bestand en Helm-grafiek. Voor sommige talen, zoals [Java][java-quickstart], [.NET core][netcore-quickstart], en [Node.js][nodejs-quickstart], de Azure Dev spaties client hulpprogramma's kunt genereren alle activa die u nodig hebt. Voor veel andere talen, zoals Go, PHP en Python, het Helm-diagram op de client hulpprogramma's kunt genereren, zolang u een geldige docker-bestand kunt opgeven.

De voorbeeld-App bevat een ongeldig Dockerfile. Voor het genereren van de activa Helm-grafiek voor het uitvoeren van de toepassing in Kubernetes, gebruikt u de `azds prep` opdracht:

```cmd
azds prep --public
```

U moet uitvoeren de `prep` opdracht uit vanaf de *dev-ruimten/samples/golang/ophalen-gestart/webfrontend* directory voor het genereren van de Helm-grafiek activa correct.

# <a name="build-and-run-code-in-kubernetes"></a>Code schrijven en uitvoeren in Kubernetes

Ontwikkelen en uitvoeren van uw code in AKS met behulp van de `azds up` opdracht:

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...3s
Installing Helm chart...2s
Waiting for container image build...39s
Building container image...
Step 1/7 : FROM golang:1.10
Step 2/7 : EXPOSE 80
Step 3/7 : WORKDIR /go/src
Step 4/7 : COPY src .
Step 5/7 : WORKDIR /go/src/app
Step 6/7 : RUN go install app
Step 7/7 : ENTRYPOINT /go/bin/app
Built container image in 41s
Waiting for container...13s
Service 'webfrontend' port 'http' is available at http://default.webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available via port forwarding at http://localhost:50199
...
```

U kunt zien dat de service wordt uitgevoerd door de openbare URL openen en te navigeren naar de `/api` pad. De openbare URL wordt weergegeven in de uitvoer van de `azds up` opdracht. In dit voorbeeld wordt de openbare URL is `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/` en navigeert u naar `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/api`.

Als u stopt de `azds up` met behulp van de opdracht *Ctrl + c*, de service wordt nog uitgevoerd in AKS en de openbare URL blijven beschikbaar.

## <a name="update-code"></a>Code bijwerken

Voor het implementeren van een bijgewerkte versie van uw service, kunt u elk bestand in uw project bijwerken en opnieuw de `azds up` opdracht. Bijvoorbeeld:

1. Als `azds up` is nog steeds uitgevoerd, drukt u op *Ctrl + c*.
1. Update [29 in regel `src/app/main.go` ](https://github.com/Azure/dev-spaces/blob/master/samples/golang/getting-started/webfrontend/src/app/main.go#L29) aan:
    
    ```golang
        fmt.Fprintf(w, "Hello from webfrontend in Azure")
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

## <a name="clean-up-your-azure-resources"></a>Opschonen van uw Azure-resources

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u Azure Dev spaties kunt complexere toepassingen ontwikkelen in meerdere containers, en hoe kunt u gezamenlijke ontwikkeling vereenvoudigen door te werken met verschillende versies of vertakkingen van uw code in verschillende spaties.

> [!div class="nextstepaction"]
> [Teamontwikkeling in ruimten voor het ontwikkelen van Azure][team-quickstart]


[java-quickstart]: quickstart-java.md
[nodejs-quickstart]: quickstart-nodejs.md
[netcore-quickstart]: quickstart-netcore.md
[team-quickstart]: quickstart-team-development.md
[supported-regions]: about.md#supported-regions-and-configurations