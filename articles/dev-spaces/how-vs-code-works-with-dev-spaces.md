---
title: De werking van Visual Studio Code met Azure Dev spaties
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: conceptual
description: De werking van Visual Studio Code met Azure Dev spaties
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: a7ec20908b75ae07532c16daab8950ace9cd67ae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712148"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>De werking van Visual Studio Code met Azure Dev spaties

U kunt Visual Studio Code en de [Azure Dev spaties extensie][azds-extension] om voor te bereiden, uitvoeren en fouten opsporen in uw services met Azure Dev spaties. Met Visual Studio Code en de extensie Azure Dev spaties, kunt u het volgende doen:

* Activa genereren voor het uitvoeren en het opsporen van fouten in services in AKS
* Uw services voor Java, Node.js en .NET Core in een dev-ruimte worden uitgevoerd
* Rechtstreeks fouten opsporen in uw Java, Node.js en .NET Core-services die worden uitgevoerd in een dev-ruimte

## <a name="generate-assets"></a>Genereren van assets

Visual Studio Code en de extensie Azure Dev spaties genereren van de volgende elementen voor uw project:

* Docker-bestanden voor Java-toepassingen met behulp van Maven, Node.js-toepassingen en .NET Core-toepassingen
* Helm-grafieken voor vrijwel elke taal met een docker-bestand
* Een `azds.yaml` -bestand, dat is de [Azure Dev spaties configuratiebestand][azds-yaml] voor uw project
* Een `.vscode` map met de configuratie voor het starten van Visual Studio Code van uw project voor Java-toepassingen met behulp van Maven, Node.js-toepassingen en .NET Core-toepassingen

Het Dockerfile, Helm-diagram en `azds.yaml` bestanden zijn de dezelfde activa die worden gegenereerd bij het uitvoeren van `azds prep`. Deze bestanden kunnen ook worden gebruikt buiten Visual Studio code om uit te voeren van uw project in AKS, zoals die wordt uitgevoerd `azds up`. De `.vscode` map wordt alleen gebruikt door Visual Studio code voor uw project in AKS uitvoeren vanuit Visual Studio Code.

## <a name="run-your-service-in-aks"></a>Uw service uitvoeren in AKS

Nadat u de activa gegenereerd voor uw project, kunt u uw services voor Java, Node.js en .NET Core in een bestaande dev-adresruimte uitvoeren vanuit Visual Studio Code. In de *Debug* pagina van Visual Studio Code, kunt u de configuratie van de lancering van aanroepen de `.vscode` directory om uit te voeren van uw project.

U moet uw AKS-cluster maken en inschakelen van Azure Dev spaties in het cluster buiten Visual Studio Code. Bijvoorbeeld, kunt u de Azure CLI of de Azure-portal te doen voor deze installatie. U kunt opnieuw gebruiken van bestaande docker-bestanden, de Helm-grafieken en `azds.yaml` bestanden gemaakt buiten Visual Studio Code, zoals de activa die worden gegenereerd door het uitvoeren van `azds prep`. Als u activa buiten Visual Studio Code gegenereerd opnieuw gebruiken, moet u nog steeds hebben een `.vscode` directory. Dit `.vscode` directory kan opnieuw worden gegenereerd door Visual Studio code en de extensie Azure Dev spaties en uw bestaande activa niet worden overschreven.

Voor .NET Core-projecten, hebt u de [ C# extensie][csharp-extension] installed to run your .NET service from Visual Studio Code. Also for Java projects using Maven, you must have the [Java Debugger for Azure Dev Spaces extension][java-extension] is geïnstalleerd, evenals [Maven geïnstalleerd en geconfigureerd][maven] om uit te voeren van uw Java de service van Visual Studio Code.

## <a name="debug-your-service-in-aks"></a>Fouten opsporen in uw service in AKS

Nadat u uw project starten, kunt u uw Java, Node.js en .NET Core-services actief in een ruimte dev rechtstreeks vanuit Visual Studio Code fouten opsporen. De configuratie starten in de `.vscode` directory kunt u de extra foutopsporing informatie voor het uitvoeren van een service bij foutopsporing in een dev-ruimte is ingeschakeld. Visual Studio Code ook gekoppeld aan het proces fouten opsporen in de container die wordt uitgevoerd in uw dev-spaties, zodat u kunt instellen onderbreking, variabelen inspecteren en andere foutopsporing bewerkingen uitvoeren.


## <a name="use-visual-studio-code-with-azure-dev-spaces"></a>Visual Studio Code gebruiken met Azure Dev-opslagruimten

U kunt Visual Studio Code en de Azure Dev spaties-extensie die werken met Azure Dev spaties in de volgende quickstarts zien:

* [Ontwikkelen met Java][quickstart-java]
* [Ontwikkelen met .NET][quickstart-netcore]
* [Ontwikkelen met Node.js][quickstart-node]

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md