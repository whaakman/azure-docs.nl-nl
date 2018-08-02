---
title: 'Snelstart: maken en implementeren van een web-app in Azure Service Fabric NET | Microsoft Docs'
description: Deze quickstart laat zien hoe u een ASP.NET Core-website maken en deze publiceren naar Azure Service Fabric NET.
services: service-fabric-mesh
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
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: d14420a363cfea23c86f63533a4ea89c5f2fd06f
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412913"
---
# <a name="quickstart-create-and-deploy-a-web-app-to-azure-service-fabric-mesh"></a>QuickStart: Maken en implementeren van een web-app in Azure Service Fabric NET

Azure Service Fabric Mesh is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen kunnen implementeren zonder virtuele machines, opslag of netwerken hoeven te beheren.

In deze snelstartgids maakt u een nieuwe Service Fabric NET toepassing die bestaat uit een ASP.NET Core web-app uitvoeren op het lokale ontwikkelcluster en publiceert u het uit te voeren op Azure.

U hebt een Azure-abonnement nodig. Als u niet hebt, kunt u eenvoudig een gratis Azure-abonnement maken [Maak een gratis account](https://azure.microsoft.com/free/) voordat u begint. U moet ook [uw ontwikkelomgeving instellen](service-fabric-mesh-howto-setup-developer-environment-sdk.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="create-a-service-fabric-mesh-project"></a>Een Service Fabric Mesh-project maken

Open Visual Studio en selecteer **bestand** > **nieuw** > **Project...**

In het dialoogvenster **New Project** typt u `mesh` in het vak **Search**. Selecteer de sjabloon **Service Fabric Mesh Application**. (Als u de sjabloon niet ziet, controleert u of u de Mesh-SDK en de previewversie van de hulpmiddelen van Visual Studio hebt geïnstalleerd, zoals beschreven in [set up your development environment](service-fabric-mesh-howto-setup-developer-environment-sdk.md) (uw ontwikkelomgeving instellen).) 

In de **naam** in het vak **ServiceFabricMesh1** en in de **locatie** vak, stelt u het pad naar de map van de waarin de bestanden voor het project wordt opgeslagen.

Controleer of het selectievakje **Create directory for solution** is ingeschakeld en klik op **OK** om het Service Fabric Mesh-project te maken.

![Dialoogvenster Nieuw project van Service Fabric Mesh in Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-project.png)

### <a name="create-a-service"></a>Een service maken

Nadat u op **OK**, wordt de **New Service Fabric Service** dialoogvenster wordt weergegeven. Selecteer de **ASP.NET Core** projecttype, controleert u of **Container OS** is ingesteld op **Windows** en klikt u op **OK** het ASP.NET Core-project maken . 

![Dialoogvenster Nieuw project van Service Fabric Mesh in Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-service-fabric-service.png)

De **nieuwe ASP.NET Core-webtoepassing** dialoogvenster wordt weergegeven. Selecteer **webtoepassing** en klik vervolgens op **OK**.

![Nieuwe ASP.NET Core-toepassing in Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-aspnetcore-app.png)

Visual Studio maakt het toepassingsproject net van Service Fabric en de ASP.NET Core-project.

## <a name="build-and-publish-to-your-local-cluster"></a>Bouwen en publiceren naar uw lokale cluster

Een Docker-installatiekopie is automatisch gemaakt en gepubliceerd naar uw lokale cluster zodra het project wordt geladen. Dit proces kan enige tijd duren. U kunt de voortgang van de Service Fabric-hulpprogramma's in de **uitvoer** venster als u wilt dat door het selecteren van de **Service Fabric Tools** item in de **uitvoer** venster vervolgkeuzelijst. U kunt blijven werken terwijl de docker-installatiekopie wordt geïmplementeerd.

Nadat het project is gemaakt, klikt u op **F5** fouten opsporen in uw service lokaal. Als de lokale implementatie is voltooid en Visual Studio uw project wordt uitgevoerd, wordt een browservenster geopend met een voorbeeld-webpagina.

Wanneer u klaar bent door de geïmplementeerde service bladert, stop de foutopsporing van uw project door te drukken **Shift + F5** in Visual Studio.

## <a name="publish-to-azure"></a>Publiceren naar Azure

Voor het publiceren van uw Service Fabric-NET-project naar Azure, met de rechtermuisknop op de **net van Service Fabric-project** in Visual studio en selecteer **publiceren...**

![Visual studio met de rechtermuisknop op net van Service Fabric-project](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-right-click-publish.png)

U ziet een **Service Fabric-toepassing publiceren** dialoogvenster.

![Dialoogvenster in Visual Studio over Service Fabric Mesh-publicatie](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-dialog.png)

Selecteer uw Azure-account en -abonnement. Kies een **Locatie**. In dit artikel wordt gebruikgemaakt van **US - oost**.

Selecteer onder **Resourcegroep** **\<Nieuwe resourcegroep maken...>**. De **resourcegroep maken** dialoogvenster wordt weergegeven. Stel de **groepsnaam voor Accountresources** en **locatie**.  Deze snelstartgids maakt gebruik van de **VS-Oost** locatie en de namen van de groep **sfmeshTutorial1RG** (als uw organisatie meerdere mensen met behulp van hetzelfde abonnement heeft, kies de naam van een unieke resource-groep).  Klik op **maken** om te maken van de resourcegroep en gaat u terug naar het dialoogvenster publiceren.

![Dialoogvenster in Visual Studio over nieuwe resourcegroep in Service Fabric Mesh](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-resource-group-dialog.png)

In het dialoogvenster **Service Fabric-toepassing publiceren** selecteert u onder **Azure Container Registry** **\<Nieuw containerregister maken...>**. In het dialoogvenster **Containerregister maken** gebruikt u een unieke naam voor de **Containerregisternaam**. Geef een **locatie** (maakt gebruik van deze snelstartgids **VS-Oost**). Selecteer in de vervolgkeuzelijst de **Resourcegroep** die u eerder hebt gemaakt, bijvoorbeeld **sfmeshTutorial1RG**. Stel de **SKU** naar **Basic** en klik vervolgens op **maken** om terug te keren naar het dialoogvenster publiceren.

![Dialoogvenster in Visual Studio over nieuwe resourcegroep in Service Fabric Mesh](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-container-registry-dialog.png)

Klik in het dialoogvenster publiceren op de **publiceren** knop om uw Service Fabric-NET toepassing naar Azure te implementeren.

Wanneer u voor het eerst in Azure implementeert, wordt de docker-installatiekopie naar de Azure Container Registry (ACR) gepushed, wat afhankelijk van de grootte van de kopie even kan duren. Volgende publicaties voor hetzelfde project zullen sneller verlopen. U kunt de voortgang van de implementatie door te selecteren **Service Fabric Tools** in de Visual Studio **uitvoer** venster vervolgkeuzelijst. Nadat de implementatie is voltooid, toont de uitvoer van de **Service Fabric Tools** het IP-adres en de poort van uw toepassing in de vorm van een URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMesh1\Web1\bin\Any CPU\Release\netcoreapp2.0\Web1.dll
Uploading the images to Azure Container Registry...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://...
```

Open een webbrowser en navigeer naar de URL om te zien van de website die wordt uitgevoerd in Azure:

![Actieve Service Fabric NET-webtoepassing](media/service-fabric-mesh-tutorial-deploy-dotnetcore/deployed-web-project.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u alle resources die u voor deze Quick Start hebt gemaakt. Nadat u een nieuwe resourcegroep voor het hosten van de ACR- en Service Fabric-NET-serviceresources hebt gemaakt, kunt u de resourcegroep, is een eenvoudige manier om te verwijderen van alle resources die zijn gekoppeld aan deze veilig verwijderen.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Connect-AzureRmAccount
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

U kunt ook kunt u de resourcegroep verwijderen [vanuit Azure portal](https://portal.azure.com).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het maken en implementeren van Service Fabric Mesh-toepassingen, gaat u naar de zelfstudie.
> [!div class="nextstepaction"]
> [Een web-app voor meerdere services maken, foutvrij maken en implementeren in Service Fabric Mesh](service-fabric-mesh-tutorial-create-dotnetcore.md)