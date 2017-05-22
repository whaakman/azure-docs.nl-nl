---
title: Uw eerste ASP.NET-web-app in vijf minuten maken in Azure | Microsoft Docs
description: Ontdek door implementatie van een simpele ASP.NET-toepassing hoe eenvoudig het is om web-apps in App Service uit te voeren.
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
ms.date: 05/05/2017
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 773d616b2a5815158971d63d005fae605450ac98
ms.contentlocale: nl-nl
ms.lasthandoff: 05/08/2017


---
# <a name="create-your-first-aspnet-web-app-in-azure-in-five-minutes"></a>Uw eerste ASP.NET-web-app in vijf minuten maken in Azure

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Met deze Quickstart leert u in een paar minuten hoe u uw eerste ASP.NET-web-app implementeert in [Azure App Service](../app-service/app-service-value-prop-what-is.md). Wanneer u klaar bent, beschikt u over een eenvoudige web-app die wordt uitgevoerd in de cloud.

![ASP.NET-web-app in Azure App Service](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie laat u zien hoe u Visual Studio 2017 gebruikt voor het maken van een ASP.NET-web-app en het implementeren ervan in Azure. Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>Een ASP.NET-web-app maken

Maak in Visual Studio een project met `Ctrl`+`Shift`+`N`.

Klik in het dialoogvenster **Nieuw project** op **Visual C# > Web > ASP.NET-webtoepassing (.NET Framework)**.

Noem de toepassing **myFirstAzureWebApp** en klik vervolgens op **OK**.
   
![Het dialoogvenster Nieuw project](./media/app-service-web-get-started-dotnet/new-project.png)

U kunt elk type ASP.NET-web-app implementeren in Azure. Voor deze zelfstudie selecteert u de **MVC**-sjabloon en zorgt u ervoor dat de verificatie is ingesteld op **Geen verificatie**.
      
Klik op **OK**.

![Het dialoogvenster Nieuw ASP.NET-project](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

## <a name="publish-to-azure"></a>Publiceren naar Azure

Klik in de **Solution Explorer** met de rechtermuisknop op uw **myFirstAzureWebApp**-project en selecteer **Publiceren**.

![Publiceren vanuit Solution Explorer](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Zorg ervoor dat **Microsoft Azure App Service** is geselecteerd en klik op **Publiceren**.

![Publiceren vanaf de projectoverzichtspagina](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Hiermee opent u het dialoogvenster **App Service maken** waarmee u alle Azure-resources maakt die u nodig hebt voor het uitvoeren van uw ASP.NET-web-app in Azure.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Klik in het dialoogvenster **App Service maken** op **Een account toevoegen** en meld u vervolgens aan bij uw Azure-abonnement. Als u al bent aangemeld bij een Microsoft-account, moet u ervoor zorgen dat dit account uw Azure-abonnement bevat. Als het aangemelde Microsoft-account niet uw Azure-abonnement bevat, klikt u erop om het juiste account toe te voegen.
   
![Aanmelden bij Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Als u bent aangemeld, kunt u in dit dialoogvenster alle resources gaan maken die u nodig hebt voor uw Azure-web-app.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Eerst hebt u een _resourcegroep_ nodig. 

> [!NOTE] 
> Een resourcegroep is een logische container waarin Azure-resources, zoals web-apps, databases en opslagaccounts, worden geïmplementeerd en beheerd.
>
>

Klik naast **Resourcegroep** op **Nieuw**.

Noem uw resourcegroep **myResourceGroup** en klik op **OK**.

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken

Uw Azure-web-app heeft ook een _App Service-plan_ nodig. 

> [!NOTE]
> Een App Service-plan bestaat uit een verzameling van fysieke resources die worden gebruikt voor het hosten van uw apps. Alle apps die zijn toegewezen aan een App Service-plan, delen de gedefinieerde resources, zodat u kosten kunt besparen als u meerdere apps host. 
>
> In App Service-plannen wordt het volgende gedefinieerd:
>
> - Regio (Noord-Europa, VS - oost, Zuidoost-Azië)
> - Exemplaargrootte (klein, normaal, groot)
> - Schaal (een, twee of drie exemplaren enz.) 
> - SKU (Free, Shared, Basic, Standard, Premium)
>
>

Klik naast **App Service-plan** op **Nieuw**. 

Configureer in het dialoogvenster **App Service-plan configureren** het nieuwe App Service-plan met de volgende instellingen:

- **App Service-plan**: typ **myAppServicePlan**. 
- **Locatie**: kies **West-Europa** of een andere gewenste regio.
- **Grootte**: kies **Free** of een andere gewenste [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/).

Klik op **OK**.

![Een App Service-plan maken](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

## <a name="create-and-publish-the-web-app"></a>De web-app maken en publiceren

Het enige dat u nu nog moet doen, is uw web-app een naam geven. Typ in **Web-app-naam** een unieke app-naam. Deze naam wordt gebruikt als onderdeel van de standaard-DNS-naam voor uw app (`<app_name>.azurewebsites.net`). Daarom moet deze uniek zijn in alle apps in Azure. Later kunt u een aangepaste domeinnaam toewijzen aan uw app voordat u deze beschikbaar maakt voor uw gebruikers.

U kunt ook de automatisch gegenereerde naam, die al uniek is, accepteren.

Klik op **Maken** om te beginnen met het maken van de Azure-resources.

![Naam van web-app configureren](./media/app-service-web-get-started-dotnet/web-app-name.png)

Nadat de wizard het maken van de Azure-resources heeft voltooid, wordt uw ASP.NET-web-app automatisch voor de eerste keer naar Azure gepubliceerd. Vervolgens wordt de gepubliceerde Azure-web-app in uw standaardbrowser gestart.

![Gepubliceerde ASP.NET-web-app in Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

De URL maakt gebruik van de web-appnaam die u eerder hebt opgegeven, met de notatie `http://<app_name>.azurewebsites.net`. 

Gefeliciteerd, uw eerste ASP.NET-web-app wordt live uitgevoerd in Azure App Service.

## <a name="update-the-app-and-redeploy"></a>De app bijwerken en opnieuw implementeren

Het is heel eenvoudig om een app bij te werken en opnieuw te implementeren in Azure. Laten we een update van de startpagina uitvoeren.

Open vanuit de **Solution Explorer** **Views\Home\Index.cshtml**.

Zoek de HTML-tag `<div class="jumbotron">` bovenaan en vervang de volledige tag door de volgende code:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Als u opnieuw wilt implementeren naar Azure, klikt u in de **Solution Explorer** met de rechtermuisknop op uw **myFirstAzureWebApp**-project en selecteert u **Publiceren**.

Klik op de publicatiepagina op **Publiceren**.

Wanneer Visual Studio klaar is, start het de bijgewerkte Azure web-app in uw browser.

![Bijgewerkte ASP.NET-web-app in Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-your-new-azure-web-app"></a>Uw nieuwe Azure-web-app beheren

Ga naar Azure Portal om de web-app die u zojuist hebt gemaakt, te bekijken. 

Hiervoor moet u zich aanmelden bij [https://portal.azure.com](https://portal.azure.com).

Klik vanuit het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-get-started-dotnet/access-portal.png)

U bent aangekomen op de _blade_ van uw web-app (een portalpagina die horizontaal wordt geopend). 

Standaard toont de blade van uw web-app de pagina **Overzicht**. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen.  

![App Service-blade in Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

De tabbladen aan de linkerkant van de blade tonen de verschillende configuratiepagina's die u kunt openen. De volgende lijst bevat slechts enkele van de mogelijkheden:

- Een aangepaste DNS-naam toewijzen
- Een aangepast SSL-certificaat binden
- Continue implementatie inschakelen
- Omhoog en omlaag schalen
- Gebruikersverificatie toevoegen

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw eerste Azure-web-app wilt verwijderen, klikt u op **Verwijderen** op de pagina **Overzicht**. Er is echter een betere manier om alles wat u hebt gemaakt in deze snelstartprocedure, te verwijderen. Klik vanuit de pagina **Overzicht** van uw web-app op de resourcegroep om de blade ervan te openen. 

![Resourcegroep openen via App Service-blade](./media/app-service-web-get-started-dotnet/access-resource-group.png)

Op de blade van de resourcegroep ziet u het App Service-plan en de App Service-app die Visual Studio voor u heeft gemaakt. 

Klik boven aan de blade op **Verwijderen**. 

<!--![Delete resource group in Azure portal](./media/app-service-web-get-started-dotnet/delete-resource-group.png)-->

Bevestig dit op de bevestigingsblade door de naam van de resourcegroep **myResourceGroup** in het tekstvak in te typen en op **Verwijderen** te klikken.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het bouwen van een ASP.NET-app in Azure met SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)

> [!div class="nextstepaction"]
> [Voorbeelden van PowerShell-scripts voor web-apps bekijken](app-service-powershell-samples.md)

