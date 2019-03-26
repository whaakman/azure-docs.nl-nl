---
title: Ontwikkelen met .NET Core in AKS met Azure Dev-ruimten en Visual Studio 2017
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
ms.author: zarhoads
ms.date: 03/22/2019
ms.topic: quickstart
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s
manager: jeconnoc
ms.custom: vs-azure
ms.workload: azure-vs
ms.openlocfilehash: 0ae2b264e689270743bc8e4aa5024a4b99eb6626
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418836"
---
# <a name="quickstart-develop-with-net-core-on-kubernetes-with-azure-dev-spaces-visual-studio-2017"></a>Quickstart: Ontwikkelen met .NET Core in Kubernetes met Azure Dev spaties (Visual Studio 2017)

In deze handleiding leert u het volgende:

- Azure Dev Spaces instellen met een beheerd Kubernetes-cluster in Azure.
- Ontwikkel iteratief code in containers met behulp van Visual Studio 2017.
- Fouten opsporen in code die wordt uitgevoerd in uw cluster met behulp van Visual Studio 2017.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen account hebt, kunt u [een gratis account aanmaken](https://azure.microsoft.com/free).
- Visual Studio 2017 op Windows met de ontwikkeling van Web-werkbelasting geïnstalleerd. Als u deze niet hebt geïnstalleerd, downloadt u deze [hier](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- [Visual Studio-hulpprogramma's voor Kubernetes](https://aka.ms/get-vsk8stools) geïnstalleerd.

## <a name="create-an-azure-kubernetes-service-cluster"></a>Een Azure Kubernetes Service-cluster maken

Moet u een AKS-cluster in een [ondersteunde regio](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams). Een cluster maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
1. Selecteer *+ een resource maken > Kubernetes-Service*. 
1. Voer de _abonnement_, _resourcegroep_, _Kubernetes-clusternaam_, _regio_, _Kubernetes-versie_, en _DNS-naamvoorvoegsel_.

    ![Maken van AKS in de Azure-portal](media/get-started-netcore-visualstudio/create-aks-portal.png)

1. Klik op *Controleren + maken*.
1. Klik op *Create*.

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Azure Dev spaties inschakelen in uw AKS-cluster

Navigeer naar uw AKS-cluster in Azure portal en klikt u op *Dev spaties*. Wijziging *inschakelen Dev spaties* naar *Ja* en klikt u op *opslaan*.

![Ontwikkel spaties inschakelen in Azure portal](media/get-started-netcore-visualstudio/enable-dev-spaces-portal.png)

## <a name="create-a-new-aspnet-web-app"></a>Een nieuwe ASP.NET-web-app maken

1. Open Visual Studio 2017.
1. Een nieuw project maken.
1. Kies *ASP.NET Core-webtoepassing* en geef uw project de naam *webfrontend*.
1. Klik op *OK*.
1. Wanneer u hierom wordt gevraagd, kiest u *webtoepassing (Model-View-Controller)* voor de sjabloon.
1. Selecteer *.NET Core* en *ASP.NET Core 2.0* aan de bovenkant.
1. Klik op *OK*.

## <a name="connect-your-project-to-your-dev-space"></a>Verbinding maken met uw project naar uw dev-ruimte

Selecteer in het project, **Azure Dev spaties** in de vervolgkeuzelijst van de instellingen starten zoals hieronder wordt weergegeven.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Selecteer in het dialoogvenster Azure Dev spaties uw *abonnement* en *Azure Kubernetes-Cluster*. Laat *ruimte* ingesteld op *standaard* en schakel de *openbaar toegankelijke* selectievakje. Klik op *OK*.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Dit proces wordt geïmplementeerd voor uw service aan de *standaard* dev ruimte met een openbaar toegankelijke URL. Als u een cluster kiest dat niet is geconfigureerd om te werken met Azure Dev Spaces, ziet u een bericht waarin wordt gevraagd of u dit wilt configureren. Klik op *OK*.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

De openbare URL voor de service wordt uitgevoerd de *standaard* dev ruimte wordt weergegeven in de *uitvoer* venster:

```cmd
Starting warmup for project 'webfrontend'.
Waiting for namespace to be provisioned.
Using dev space 'default' with target 'MyAKS'
...
Successfully built 1234567890ab
Successfully tagged webfrontend:devspaces-11122233344455566
Built container image in 39s
Waiting for container...
36s

Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:62266
Completed warmup for project 'webfrontend' in 125 seconds.
```

In het bovenstaande voorbeeld wordt de openbare URL is http://webfrontend.1234567890abcdef1234.eus.azds.io/. Navigeer naar de openbare URL van uw service en communiceren met de service wordt uitgevoerd in de adresruimte van uw dev.

## <a name="update-code"></a>Code bijwerken

Als Visual Studio 2017 nog steeds met uw dev-ruimte verbonden is, klikt u op de knop stoppen. Wijzig regel 20 in `Controllers/HomeController.cs` aan:
    
```csharp
ViewData["Message"] = "Your application description page in Azure.";
```

Sla uw wijzigingen op en start uw service met **Azure Dev spaties** in de vervolgkeuzelijst start-instellingen. De openbare URL van uw service in een browser en klik op openen *over*. Bekijk of uw bijgewerkte bericht wordt weergegeven.

In plaats van opnieuw te bouwen en opnieuw implementeren van een nieuwe containerinstallatiekopie telkens wanneer de code wijzigingen zijn aangebracht, Azure Dev spaties incrementeel gecompileerd code binnen de bestaande container voor een snellere bewerken/debug-lus.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Instellen en het gebruik van onderbrekingspunten voor foutopsporing

Als Visual Studio 2017 nog steeds met uw dev-ruimte verbonden is, klikt u op de knop stoppen. Open `Controllers/HomeController.cs` en klik ergens op regel 20 en plaats de cursor bevat. Om in te stellen van een onderbrekingspunt bereikt *F9* of klik op *Debug* vervolgens *onderbrekingspunt*. Voor het starten van uw service in de foutopsporingsmodus in de adresruimte van uw dev, bereikt *F5* of klik op *Debug* vervolgens *Start Debugging*.

Open uw service in een browser en er geen bericht wordt weergegeven. Ga terug naar Visual Studio 2017 en bekijk regel 20 is gemarkeerd. Het onderbrekingspunt die u instelt, heeft de service op regel 20 onderbroken. Hervat de service, bereikt *F5* of klik op *Debug* vervolgens *doorgaan*. Ga terug naar uw browser en u ziet dat het bericht wordt nu weergegeven.

Tijdens het uitvoeren van uw service in Kubernetes met een foutopsporingsprogramma die is gekoppeld, hebt u volledige toegang tot informatie zoals de aanroepstack, lokale variabelen en informatie over de uitzondering voor foutopsporing.

Het onderbrekingspunt verwijderen door de cursor op regel 20 in `Controllers/HomeController.cs` en op *F9*.

## <a name="clean-up-your-azure-resources"></a>Opschonen van uw Azure-resources

Navigeer naar de resourcegroep in Azure portal en klikt u op *resourcegroep verwijderen*. U kunt ook kunt u de [az aks verwijderen](/cli/azure/aks#az-aks-delete) opdracht:

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Werken met meerdere containers en teamontwikkeling](multi-service-netcore-visualstudio.md)
