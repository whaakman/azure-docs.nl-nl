---
title: 'Zelfstudie: een Service Fabric Mesh-toepassing implementeren in Service Fabric Mesh | Microsoft Docs'
description: Leer hoe u een Azure Service Fabric Mesh-toepassing publiceert die bestaat uit een ASP.NET Core-website die met een back-end-webservice communiceert.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/26/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 350749161260768071afbb47b854cb2e9184bd9d
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284724"
---
# <a name="tutorial-deploy-a-service-fabric-mesh-web-application"></a>Zelfstudie: een Service Fabric Mesh-webtoepassing implementeren

Deze zelfstudie is deel drie van een reeks en laat u zien hoe u een Azure Service Fabric Mesh-webtoepassing rechtstreeks vanuit Visual Studio implementeert.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De app publiceren in Azure.
> * De implementatiestatus van de toepassing controleren
> * Alle toepassingen bekijken die momenteel in uw abonnement zijn geïmplementeerd
> * De toepassingslogboeken bekijken
> * De resources opschonen die door de app zijn gebruikt.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * [Een Service Fabric Mesh-webtoepassing bouwen](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Fouten in de app lokaal opsporen](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * De app publiceren in Azure

U leert hoe u een Azure Service Fabric Mesh-app maakt die een ASP.NET-webfront-end- en een ASP.NET Core Web API-back-endservice bevat. Vervolgens spoort u de fouten in de app op in uw lokale ontwikkelcluster en publiceert u de app in Azure. Als u klaar bent, hebt u een eenvoudige taken-app waarmee een service-naar-service-oproep in een Service Fabric Mesh-webtoepassing wordt gedemonstreerd.

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, kunt u een [gratis account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

* Controleer of u [uw ontwikkelomgeving hebt ingesteld](service-fabric-mesh-howto-setup-developer-environment-sdk.md). Hieronder valt het installeren van de Service Fabric-runtime, SDK, Docker en Visual Studio 2017.

## <a name="download-the-to-do-sample-application"></a>De voorbeeldtoepassing voor taken downloaden

Als u in [deel twee van deze zelfstudiereeks](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md) niet de voorbeeldtoepassing voor taken hebt gemaakt, kunt u deze downloaden. Voer in een opdrachtvenster de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen op de lokale computer.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

U vindt de toepassing in de map `src\todolistapp`.

## <a name="publish-to-azure"></a>Publiceren naar Azure

Als u uw Service Fabric Mesh-project in Azure wilt publiceren, klikt u met de rechtermuisknop op **ServiceFabricMeshApp** in Visual Studio en selecteert u **Publiceren**.

Vervolgens ziet u het dialoogvenster **Service Fabric-toepassing publiceren**.

![Dialoogvenster in Visual Studio over Service Fabric Mesh-publicatie](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Selecteer uw Azure-account en -abonnement. Kies een **Locatie**. In dit artikel wordt gebruikgemaakt van **US - oost**.

Selecteer onder **Resourcegroep** **\<Nieuwe resourcegroep maken...>**. Er verschijnt een dialoogvenster waarin u uw nieuwe resourcegroep maakt. In dit artikel wordt de locatie **US - oost** gebruikt en krijgt de groep de naam **sfmeshTutorial1RG** (kies een unieke groepsnaam als meerdere mensen in uw organisatie gebruikmaken van hetzelfde abonnement).  Druk op **Maken** om de resourcegroep te maken en ga terug naar het publicatiedialoogvenster.

![Dialoogvenster in Visual Studio over nieuwe resourcegroep in Service Fabric Mesh](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-resource-group-dialog.png)

In het dialoogvenster **Service Fabric-toepassing publiceren** selecteert u onder **Azure Container Registry** **\<Nieuw containerregister maken...>**. In het dialoogvenster **Containerregister maken** gebruikt u een unieke naam voor de **Containerregisternaam**. Geef een **Locatie** op (in deze tutorial wordt gebruikgemaakt van **US - oost**). Selecteer in de vervolgkeuzelijst de **Resourcegroep** die u eerder hebt gemaakt, bijvoorbeeld **sfmeshTutorial1RG**. Stel de **SKU** in op **Basic** en druk vervolgens op **Maken** om terug te keren naar het publicatiedialoogvenster.

![Dialoogvenster in Visual Studio over nieuwe resourcegroep in Service Fabric Mesh](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-container-registry-dialog.png)

Als u een fout krijgt dat er voor uw abonnement geen resourceprovider is geregistreerd, kunt u er een registreren. Controleer eerst of de resourceprovider beschikbaar is voor uw abonnement:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Als de containerregisterprovider (`Microsoft.ContainerRegistry`) beschikbaar is, registreert u deze vanuit PowerShell:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

In het publicatiedialoogvenster drukt u op de knop **Publiceren** om uw Service Fabric-toepassing te implementeren in Azure.

Wanneer u voor het eerst in Azure implementeert, wordt de docker-installatiekopie naar de Azure Container Registry (ACR) gepushed, wat afhankelijk van de grootte van de kopie even kan duren. Volgende publicaties voor hetzelfde project zullen sneller verlopen. U kunt de voortgang van de implementatie bekijken door het deelvenster **Service Fabric Tools** in het Visual Studio-venster **Uitvoer** te selecteren. Nadat de implementatie is voltooid, toont de uitvoer van de **Service Fabric Tools** het IP-adres en de poort van uw toepassing in de vorm van een URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Open een webbrowser en navigeer naar de URL om de website in Azure uitgevoerd te zien worden.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI instellen 
U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken voor de resterende stappen. Installeer de Azure Service Fabric Mesh CLI-uitbreidingsmodule door de volgende [instructies](service-fabric-mesh-howto-setup-cli.md) te volgen.


## <a name="check-application-deployment-status"></a>De implementatiestatus van de toepassing controleren

Op dit punt is uw toepassing geïmplementeerd. Met de opdracht `app show` kunt u de status van de toepassing bekijken. 

De naam van de toepassing voor de zelfstudie-app is `ServiceMeshApp`. Haal de details van de toepassing op met de volgende opdracht:

```azurecli-interactive
az mesh app show --resource-group $rg --name ServiceMeshApp
```

## <a name="see-all-applications-currently-deployed-to-your-subscription"></a>Alle toepassingen bekijken die momenteel in uw abonnement zijn geïmplementeerd

U kunt de opdracht 'app list' gebruiken om een lijst met toepassingen die u in uw abonnement hebt geïmplementeerd op te halen.

```cli
az mesh app list --output table
```

## <a name="see-the-application-logs"></a>De toepassingslogboeken bekijken

Bekijk de logboeken voor de geïmplementeerde toepassing:

```azurecli-interactive
az mesh code-package-log get --resource-group $rg --application-name ServiceMeshApp --service-name todoservice --replica-name 0 --code-package-name ServiceMeshApp
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de resources die u hebt gemaakt niet langer nodig hebt, kunt u deze verwijderen. Aangezien u een nieuwe resourcegroep hebt gemaakt om zowel de ACR- als de Service Fabric Mesh-serviceresources te hosten, kunt u deze resourcegroep veilig verwijderen, waardoor alle bijbehorende resources worden verwijderd.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

U kunt de resourcegroep ook [vanuit de portal](../azure-resource-manager/resource-group-portal.md#delete-resource-group-or-resources) verwijderen. 

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende geleerd:
> [!div class="checklist"]
> * De app publiceren in Azure.
> * De implementatiestatus van de toepassing controleren
> * Alle toepassingen bekijken die u momenteel hebt geïmplementeerd in uw abonnement
> * De toepassingslogboeken bekijken
> * De resources opschonen die door de app zijn gebruikt.

Nu u een Service Fabric Mesh-toepassing hebt geïmplementeerd in Azure, kunt u het volgende proberen:

* Ontdek de [voorbeeld-app voor stemmen](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) om een ander voorbeeld van service-naar-service-communicatie te bekijken.
* Meer informatie over [Service Fabric-resources](service-fabric-mesh-service-fabric-resources.md)
* Meer informatie over de [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest