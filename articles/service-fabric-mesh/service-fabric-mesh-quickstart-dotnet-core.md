---
title: 'Snelstart: een app implementeren in Azure Service Fabric Mesh | Microsoft Docs'
description: Deze quickstart laat zien hoe u een ASP.NET Core-website maken en deze publiceren naar Azure Service Fabric NET.
services: service--fabric-mesh
documentationcenter: .net
author: tylermsft
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 93b540908391e8eedd9a1b318f3443f3e782c987
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076033"
---
# <a name="quickstart-create-and-deploy-a-web-app-to-azure-service-fabric-mesh"></a>QuickStart: Maken en implementeren van een web-app in Azure Service Fabric NET

Azure Service Fabric NET is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen implementeren zonder beheer van virtuele machines, opslag, of netwerken.

In deze snelstartgids maakt u een nieuwe Service Fabric NET toepassing die bestaat uit een ASP.NET Core web-app uitvoeren op het lokale ontwikkelcluster en publiceert u het uit te voeren op Azure.

U hebt een Azure-abonnement nodig. Als u niet hebt, kunt u eenvoudig een gratis Azure-abonnement maken [Maak een gratis account](https://azure.microsoft.com/free/) voordat u begint. U moet ook [uw ontwikkelomgeving instellen](service-fabric-mesh-howto-setup-developer-environment-sdk.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="create-a-service-fabric-mesh-project"></a>Een Service Fabric-NET-project maken

Open Visual Studio en selecteer **bestand** > **nieuw** > **Project...**

In de **nieuw Project** dialoogvenster **zoeken** vak aan de bovenkant, type `mesh`. Selecteer de **Service Fabric-NET toepassing** sjabloon. (Als u de sjabloon niet ziet, zorg ervoor dat u de NET-SDK geïnstalleerd en voorbeeld van de Visual Studio-hulpprogramma's, zoals beschreven in [uw ontwikkelomgeving instellen](service-fabric-mesh-howto-setup-developer-environment-sdk.md). 

In de **naam** in het vak **ServiceFabricMesh1** en in de **locatie** vak, stelt u het pad naar de map van de waarin de bestanden voor het project wordt opgeslagen.

Zorg ervoor dat **map maken voor oplossing** is ingeschakeld en klik op **OK** om de Service Fabric-NET-project te maken.

![Visual studio-Service Fabric net het dialoogvenster Nieuw project](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-project.png)

### <a name="create-a-service"></a>Een service maken

Nadat u op **OK**, wordt de **New Service Fabric Service** dialoogvenster wordt weergegeven. Selecteer de **ASP.NET Core** projecttype, controleert u of **Container OS** is ingesteld op **Windows** en druk op **OK** het ASP.NET Core-project maken . 

![Visual studio-Service Fabric net het dialoogvenster Nieuw project](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-service-fabric-service.png)

De **nieuwe ASP.NET Core-webtoepassing** dialoogvenster wordt weergegeven. Selecteer **webtoepassing** en druk vervolgens op **OK**.

![Visual studio nieuwe ASP.NET core-toepassing](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-aspnetcore-app.png)

Visual Studio maakt zowel het net van Service Fabric-toepassingsproject en het ASP.NET Core-project.

## <a name="build-and-publish-to-your-local-cluster"></a>Bouwen en publiceren naar uw lokale cluster

Een Docker-installatiekopie wordt automatisch gemaakt en gepubliceerd naar uw lokale cluster zodra het project wordt geladen. Dit proces kan enige tijd duren. U kunt de voortgang van de Service Fabric-hulpprogramma's in de **uitvoer** deelvenster als u wilt. Selecteer de **Service Fabric Tools** item in het deelvenster. U kunt blijven werken terwijl de docker-installatiekopie wordt geïmplementeerd.

Nadat het project is gemaakt, drukt u op **F5** fouten opsporen in uw service lokaal. Als de lokale implementatie is voltooid en Visual Studio uw project wordt uitgevoerd, wordt een browservenster geopend met een voorbeeld-webpagina.

Wanneer u klaar bent door de geïmplementeerde service bladert, kunt u voorkomen dat opsporen van fouten in uw project door te drukken **Shift + F5** in Visual Studio.

## <a name="publish-to-azure"></a>Publiceren naar Azure

Voor het publiceren van uw Service Fabric-NET-project naar Azure, met de rechtermuisknop op de **net van Service Fabric-project** in Visual studio en selecteer **publiceren...**

![Visual studio met de rechtermuisknop op net van Service Fabric-project](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-right-click-publish.png)

U ziet een **Service Fabric-toepassing publiceren** dialoogvenster.

![Visual studio-Service Fabric NET dialoogvenster publiceren](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-dialog.png)

Selecteer uw Azure-account en -abonnement. Kies een **locatie**. In dit artikel wordt gebruikgemaakt van **VS-Oost**.

Onder **resourcegroep**, selecteer  **\<nieuwe resourcegroep maken... >**. Er wordt een dialoogvenster weergegeven waarin u maakt een nieuwe resourcegroep. In dit artikel wordt de **VS-Oost** locatie en de namen van de groep **sfmeshTutorial1RG** (als uw organisatie meerdere mensen met behulp van hetzelfde abonnement heeft, kies een unieke groepsnaam).  Druk op **maken** om te maken van de resourcegroep en gaat u terug naar het dialoogvenster publiceren.

![Visual studio-Service Fabric net nieuwe resource groepsdialoogvenster](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-resource-group-dialog.png)

Klik in de **Service Fabric-toepassing publiceren** dialoogvenster onder **Azure Container Registry**, selecteer  **\<maken nieuwe Container Registry... >**. In de **Container Registry maken** dialoogvenster, gebruik een unieke naam voor de **containerregisternaam**. Geef een **locatie** (deze zelfstudie wordt gebruikgemaakt van **VS-Oost**). Selecteer de **resourcegroep** die u hebt gemaakt in de vorige stap in de vervolgkeuzelijst, bijvoorbeeld **sfmeshTutorial1RG**. Stel de **SKU** naar **Basic** en druk vervolgens op **maken** om terug te keren naar het dialoogvenster publiceren.

Als u een foutmelding dat krijgt een resourceprovider is niet geregistreerd voor uw abonnement en u deze kunt registreren. Eerst controleren of de resourceprovider beschikbaar voor uw abonnement is:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Als de container registry-provider (`Microsoft.ContainerRegistry`) beschikbaar is, registreert u dit vanuit Powershell:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

![Visual studio-Service Fabric net nieuwe resource groepsdialoogvenster](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-container-registry-dialog.png)

Druk in het dialoogvenster publiceren op de **publiceren** knop om uw Service Fabric-NET toepassing naar Azure te implementeren.

Wanneer u voor de eerste keer naar Azure publiceert, wordt de docker-installatiekopie naar Azure Container Registry (ACR) die het kost tijd, afhankelijk van de grootte van de afbeelding doorgestuurd. Volgende publiceert van hetzelfde project worden sneller. U kunt de voortgang van de implementatie door het selecteren van de **Service Fabric Tools** deelvenster in de Visual Studio **uitvoer** venster. Nadat de implementatie is voltooid, de **Service Fabric Tools** uitvoer wordt weergegeven de IP-adres en poort van uw toepassing in de vorm van een URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMesh1\Web1\bin\Any CPU\Release\netcoreapp2.0\Web1.dll
Uploading the images to Azure Container Registry...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://...
```

Open een webbrowser en navigeer naar de URL om te zien van de website die wordt uitgevoerd in Azure.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u alle resources die u voor deze Quick Start hebt gemaakt. Nadat u een nieuwe resourcegroep voor het hosten van de ACR- en Service Fabric-NET-serviceresources hebt gemaakt, kunt u deze resourcegroep is een eenvoudige manier om te verwijderen van alle resources die zijn gekoppeld aan deze veilig verwijderen.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

U kunt ook kunt u de resourcegroep verwijderen [vanuit Azure portal](https://portal.azure.com).

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over het maken en implementeren van Service Fabric-NET-toepassingen, gaat u naar de zelfstudie.
> [!div class="nextstepaction"]
> [Maken en implementeren van een webtoepassing voor meerdere services](service-fabric-mesh-tutorial-create-dotnetcore.md)
