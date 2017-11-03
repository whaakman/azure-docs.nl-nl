---
title: Maken van aangepaste connectors van Web-API's - Azure Logic Apps | Microsoft Docs
description: Aangepaste connectors van Web-API's maken
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 22bf335718721d29933557142b436e75778f8c86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-custom-connectors-from-web-apis"></a>Aangepaste connectors van Web-API's maken

Als u wilt maken van een aangepaste connector die u in Azure Logic Apps, Microsoft Flow of Microsoft PowerApps gebruiken kunt, maakt u eerst een Web-API die u kunt hosten met Azure-Web-Apps, verifiëren met Azure Active Directory en registreren als een connector met Logic Apps, stroom, of PowerApps. Deze zelfstudie laat zien hoe u deze taken uitvoeren met het bouwen van een ASP.NET Web API-app. Voor Zie patronen die weergeven kunt u code voor de connector triggers en acties, structureren [maken van aangepaste API's die u vanuit logic app werkstromen aanroepen kunt](../logic-apps/logic-apps-create-api-app.md).

## <a name="prerequisites"></a>Vereisten

* [Visual Studio 2013 of later](https://www.visualstudio.com/vs/). Deze zelfstudie wordt Visual Studio 2015.

* Code voor uw Web-API. Als u niet hebt, kunt u deze zelfstudie: [aan de slag met ASP.NET Web API 2 (C#)](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).

* Een Azure-abonnement. Als u geen een abonnement hebt, kunt u beginnen met een [gratis Azure-account](https://azure.microsoft.com/free/). Anders zich aanmelden voor een [abonnement met betalen naar gebruik](https://azure.microsoft.com/pricing/purchase-options/).

## <a name="create-and-deploy-an-aspnet-web-app-to-azure"></a>Maken en implementeren van een ASP.NET-web-app in Azure

Voor deze zelfstudie maakt u een Visual C# ASP.NET-webtoepassing. 

1. Open Visual Studio en kies vervolgens **bestand** > **nieuw Project**.

   1. Vouw **geïnstalleerde**, gaat u naar **sjablonen** > **Visual C#** > **Web**, en selecteer **ASP .NET-webtoepassings**.

   2. Geef een naam van het project, locatie en naam van de oplossing voor uw app, en kies vervolgens **OK**.

   Bijvoorbeeld:

   ![Een Visual C# ASP.NET-webtoepassing maken](./media/custom-connector-build-web-api-app-tutorial/visual-studio-new-project-aspnet-web-app.png)

2. In de **nieuwe ASP.NET-webtoepassing** de optie de **Web API** sjabloon. Als dat niet is geselecteerd, selecteert u **Host in de cloud**. Kies **verificatie wijzigen**.

   ![Selecteer 'Web-API-sjabloon, 'Host in de cloud', 'Wijziging verificatie'](./media/custom-connector-build-web-api-app-tutorial/visual-studio-web-api-template.png)

3. Selecteer **geen verificatie**, en kies **OK**. U kunt later verificatie instellen.

   ![Selecteer 'Geen verificatie'](./media/custom-connector-build-web-api-app-tutorial/visual-studio-change-authentication.png)

4. Wanneer de **nieuwe ASP.NET-webtoepassing** vak opnieuw weergegeven, kiest u **OK**. 

5. In de **Create App Service** vak, Controleer de hosting-instellingen in de tabel beschreven, de wijzigingen die u wilt gebruiken en kies **maken**. 

   Een [App Service-abonnement](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vertegenwoordigt een verzameling fysieke resources die worden gebruikt voor het hosten van apps in uw Azure-abonnement. Meer informatie over [-App Service](../app-service/app-service-value-prop-what-is.md).

   ![App Service maken](./media/custom-connector-build-web-api-app-tutorial/visual-studio-create-app-service.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | Uw Azure werk of schoolaccount of je persoonlijke Microsoft-account | *uw-gebruikersaccount* | Selecteer uw gebruikersaccount. | 
   | **De naam van de Web-App** | *aangepaste web-api-app-naam* of de standaardnaam | Voer de naam voor uw Web-API-app wordt gebruikt in uw app-URL, bijvoorbeeld: http://*web api-app-naam*. | 
   | **Abonnement** | *Naam van een Azure-abonnement* | Selecteer het Azure-abonnement dat u wilt gebruiken. | 
   | **Resourcegroep** | *Azure-resource-group-name* | Selecteer een bestaande Azure-resourcegroep of als u nog niet gedaan hebt, maakt u een resourcegroep. <p>**Opmerking**: een Azure-resourcegroep ordent Azure-resources in uw Azure-abonnement. | 
   | **App Service-abonnement** | *App-Service-plan-naam* | Selecteer een bestaand App Service-abonnement of als u nog niet gedaan hebt, maakt u een plan. | 
   |||| 

   Als u een App Service-Plan maakt, moet u deze instellingen opgeven:

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Locatie** | *implementatie-regio* | Selecteer de regio voor het implementeren van uw app. | 
   | **Grootte** | *App Service-plan grootte* | Selecteer de grootte van uw abonnement, waarmee wordt bepaald van de kosten en computercapaciteit bron voor uw service-abonnement. | 
   |||| 

   Als u alle andere resources die vereist zijn voor uw app instelt, kies **verkennen extra Azure-services**.

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Resourcetype** | *Azure-resource-type* | Selecteer en eventuele aanvullende resources die vereist zijn door uw app instellen. | 
   |||| 

6. Nadat Visual Studio het project implementeert, bouwt u de code voor uw app.

## <a name="create-an-openapi-swagger-file-that-describes-your-web-api"></a>Een OpenAPI (Swagger)-bestand met een beschrijving van uw Web-API maken

Als u wilt uw Web-API-app verbinden met Logic Apps, moet u een [OpenAPI (voorheen Swagger) bestand](http://swagger.io/) die uw API-bewerkingen worden beschreven. Kunt u uw eigen OpenAPI definitie voor uw API met de [online Swagger-editor](http://editor.swagger.io/), maar deze zelfstudie wordt gebruikgemaakt van een open-source hulpprogramma met de naam [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle/blob/master/README.md).

1. Als u nog niet gedaan hebt, moet u het Swashbuckle Nuget-pakket installeren in uw Visual Studio-project.

   1. Kies in Visual Studio **extra** > **NuGet Package Manager** > 
    **Package Manager Console**.

   2. In de **Package Manager Console**, gaat u naar de projectmap van uw app als u er niet al bent (uitvoeren `Set-Location "project-path"`), en voer deze PowerShell-cmdlet: 
   
      `Install-Package Swashbuckle`

      Bijvoorbeeld:

      ![Package Manager Console Swashbuckle installeren](./media/custom-connector-build-web-api-app-tutorial/visual-studio-package-manager-install-swashbuckle.png)

   > [!TIP]
   > Als u uw app uitvoeren na de installatie van Swashbuckle, wordt een bestand OpenAPI bij deze URL door Swashbuckle gegenereerd: 
   >
   > http://*{your-web-api-app-root-URL}*/swagger/docs/v1
   > 
   > Swashbuckle genereert ook een gebruikersinterface bij deze URL: 
   > 
   > http://*{your-web-api-app-root-URL}*  /swagger

3. Wanneer u klaar bent, kunt u uw Web-API-app publiceren naar Azure. Als u wilt publiceren vanuit Visual Studio, met de rechtermuisknop op het webproject in Solution Explorer, kiest u **publiceren...** , en volg de aanwijzingen.

   > [!IMPORTANT]
   > Dubbele bewerkings-id's moet een document OpenAPI ongeldig. Als u de C# voorbeeldsjabloon, de sjabloon gebruikt *herhaalt deze bewerkings-ID tweemaal*:`Values_Get` 
   > 
   > U kunt dit probleem oplossen door een exemplaar te wijzigen `Value_Get` en opnieuw publiceren.

4. Het document OpenAPI ophalen door te bladeren naar deze locatie: 

   http://*{your-web-api-app-root-URL}*/swagger/docs/v1

   U kunt ook downloaden een [voorbeeld OpenAPI document](https://pwrappssamples.blob.core.windows.net/samples/webAPI.json) uit deze zelfstudie. 
   Zorg ervoor dat u de opmerkingen verwijdert, welke beginnen met ' / / ', voordat u het document gebruiken.

5. De inhoud opslaan als een JSON-bestand. Op basis van uw browser kunt wellicht u Kopieer en plak de tekst in een leeg tekstbestand.

## <a name="next-steps"></a>Volgende stappen

* [Verificatie voor aangepaste connectors instellen](../logic-apps/custom-connector-azure-active-directory-authentication.md)











