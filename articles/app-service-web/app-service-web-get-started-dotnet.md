---
title: Een ASP.NET-web-app maken in Azure | Microsoft Docs
description: Leer hoe u web-apps uitvoert in Azure App Service door de standaard-ASP.NET-web-app te implementeren.
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 2b447bcc5930550af3996cb40925ab59d203dc7c
ms.contentlocale: nl-nl
ms.lasthandoff: 06/20/2017

---
<a id="create-an-aspnet-web-app-in-azure" class="xliff"></a>

# Een ASP.NET-web-app maken in Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) biedt een uiterst schaalbare webhostingservice met self-patchfunctie.  Deze Quickstart laat zien hoe uw eerste ASP.NET-web-app implementeert in Azure Web Apps. Als u klaar bent, hebt u een resourcegroep die bestaat uit een App Service-plan en een Azure-web-app met een geïmplementeerde webtoepassing.

![ASP.NET-web-app in Azure App Service](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

<a id="prerequisites" class="xliff"></a>

## Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* Installeer [Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) met de volgende workloads:
    - **ASP.NET- en web-ontwikkeling**
    - **Azure-ontwikkeling**

    ![ASP.NET- en web-ontwikkeling en Azure-ontwikkeling (onder Web en cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-an-aspnet-web-app" class="xliff"></a>

## Een ASP.NET-web-app maken

Maak in Visual Studio een project door **Bestand > Nieuw > Project** te selecteren. 

Selecteer **Visual C# > Web > ASP.NET-webtoepassing (.NET Framework)** in het dialoogvenster **Nieuw project**.

Geef de toepassing de naam _myFirstAzureWebApp_ en selecteer vervolgens **OK**.
   
![Het dialoogvenster Nieuw project](./media/app-service-web-get-started-dotnet/new-project.png)

U kunt elk type ASP.NET-web-app implementeren in Azure. Voor deze Quickstart selecteert u de sjabloon **MVC** en stelt u de verificatie in op **Geen verificatie**.
      
Selecteer **OK**.

![Het dialoogvenster Nieuw ASP.NET-project](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

Selecteer in het menu **Fouten opsporen > Starten zonder foutopsporing** om de web-app lokaal uit te voeren.

![De app lokaal uitvoeren](./media/app-service-web-get-started-dotnet/local-web-app.png)

<a id="publish-to-azure" class="xliff"></a>

## Publiceren naar Azure

Klik in **Solution Explorer** met de rechtermuisknop op het project **myFirstAzureWebApp**-en selecteer **Publiceren**.

![Publiceren vanuit Solution Explorer](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Zorg ervoor dat **Microsoft Azure App Service** is geselecteerd en selecteer dan **Publiceren**.

![Publiceren vanaf de projectoverzichtspagina](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Hiermee opent u het dialoogvenster **App Service maken**, waarmee u alle Azure-resources kunt maken die nodig zijn voor het uitvoeren van uw ASP.NET-web-app in Azure.

<a id="sign-in-to-azure" class="xliff"></a>

## Aanmelden bij Azure

Selecteer **Een account toevoegen** in het dialoogvenster **App Service maken** en meld u vervolgens aan bij uw Azure-abonnement. Als u al bent aangemeld, moet u ervoor zorgen dat dit account uw Azure-abonnement bevat. U kunt het aangemelde account selecteren om het juiste account toe te voegen.

> [!NOTE]
> Als u al bent aangemeld, selecteert u **Maken** nog niet.
>
>
   
![Aanmelden bij Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Als u bent aangemeld, kunt u in dit dialoogvenster alle resources gaan maken die u nodig hebt voor een Azure-web-app.

<a id="create-a-resource-group" class="xliff"></a>

## Een resourcegroep maken

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Selecteer **Nieuw** naast **Resourcegroep**.

Geef de resourcegroep de naam **myResourceGroup** en selecteer **OK**.

<a id="create-an-app-service-plan" class="xliff"></a>

## Een App Service-plan maken

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Selecteer **Nieuw** naast **App Service-plan**. 

Gebruik in het dialoogvenster **App Service-plan configureren** de instellingen uit de tabel na de onderstaande schermafbeelding.

![Een App Service-plan maken](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Instelling | Voorgestelde waarde | Beschrijving |
|-|-|-|
|App Service-plan| myAppServicePlan | De naam van het App Service-plan. |
| Locatie | West-Europa | Het datacenter waar de web-app wordt gehost. |
| Grootte | Gratis | De [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/) bepaalt de hosting-functies. |

Selecteer **OK**.

<a id="create-and-publish-the-web-app" class="xliff"></a>

## De web-app maken en publiceren

Typ in het veld **Web-app-naam** een unieke app-naam (geldige tekens zijn `a-z`, `0-9` en `-`). De URL van de web-app is `http://<app_name>.azurewebsites.net`, waarbij `<app_name>` de naam van uw web-app is. 

U kunt de automatisch gegenereerde naam accepteren, die uiteraard uniek is.

Selecteer **Maken** om de Azure-resources te gaan maken.

![Naam van web-app configureren](./media/app-service-web-get-started-dotnet/web-app-name.png)

Zodra de wizard is voltooid, wordt de ASP.NET-web-app naar Azure gepubliceerd. Daarna wordt de app gestart in de standaardbrowser.

![Gepubliceerde ASP.NET-web-app in Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

De URL maakt gebruik van de web-appnaam die u eerder hebt opgegeven, met de notatie `http://<app_name>.azurewebsites.net`. 

Gefeliciteerd, uw ASP.NET-web-app wordt live uitgevoerd in Azure App Service.

<a id="update-the-app-and-redeploy" class="xliff"></a>

## De app bijwerken en opnieuw implementeren

Open vanuit de **Solution Explorer** _Views\Home\Index.cshtml_.

Zoek ergens bovenaan de HTML-tag `<div class="jumbotron">` en vervang het volledige element door de volgende code:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Als u opnieuw wilt implementeren naar Azure, klikt u in **Solution Explorer** met de rechtermuisknop op het project **myFirstAzureWebApp** en selecteert u **Publiceren**.

Selecteer **Publiceren** op de publicatiepagina.

Als het publiceren is voltooid, start Visual Studio een browser waarin de URL van de web-app wordt geladen.

![Bijgewerkte ASP.NET-web-app in Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

<a id="manage-the-azure-web-app" class="xliff"></a>

## De Azure-web-app beheren

Ga naar <a href="https://portal.azure.com" target="_blank">Azure Portal</a> om de web-app te beheren.

Selecteer in het linkermenu **App Services** en selecteer de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-get-started-dotnet/access-portal.png)

De pagina Overzicht van uw web-app wordt weergegeven. Hier kunt u algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. 

![App Service-blade in Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Het linkermenu bevat een aantal pagina's voor het configureren van uw app. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

<a id="next-steps" class="xliff"></a>

## Volgende stappen

> [!div class="nextstepaction"]
> [ASP.NET met SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)

