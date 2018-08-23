---
title: Bouw serverloze apps met Visual Studio | Microsoft Docs
description: Bouwen, implementeren en beheren van uw eerste serverloze app met Azure Logic Apps en Azure Functions in Visual Studio
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.reviewer: jehollan, LADocs
ms.suite: integration
ms.custom: vs-azure
ms.topic: article
ms.date: 08/01/2018
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.openlocfilehash: ed56e5f2a4a0d9e3a5e8d8c80e0fd20dbfb098bb
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444783"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Uw eerste serverloze app bouwen met Azure Logic Apps en Azure Functions - Visual Studio

U kunt snel ontwikkelen en implementeren van cloud-apps met behulp van de serverloze hulpprogramma's en mogelijkheden in Azure, zoals [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Azure Functions](../azure-functions/functions-overview.md). In dit artikel laat zien hoe om te beginnen het bouwen van een serverloze app, die gebruikmaakt van een logische app die een Azure-functie in Visual Studio-aanroepen. Zie voor meer informatie over de serverloze oplossingen in Azure, [Serverloze Azure Functions en Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt een serverloze app bouwen in Visual Studio, moet u deze items:

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* [Visual Studio 2017](https://www.visualstudio.com/vs/) of Visual Studio 2015 - Community, Professional of Enterprise

* [Microsoft Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 of hoger)

* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

* [Azure Logic Apps-hulpprogramma's voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) of de [versie van Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio)

  U kunt downloaden en installeren van Azure Logic Apps-hulpprogramma's rechtstreeks vanuit Visual Studio Marketplace, of [informatie over het installeren van deze extensie uit in Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
  Zorg ervoor dat u Visual Studio opnieuw opstart na de installatie. 

* [Azure Functions Core Tools](https://www.npmjs.com/package/azure-functions-core-tools) voor lokaal functies voor foutopsporing

* Toegang tot het web tijdens het gebruik van de ontwerper van logische App ingesloten in Visual Studio

  De ontwerpfunctie moet over een internetverbinding beschikken om resources te maken in Azure en de eigenschappen en gegevens van connectoren in uw logische app te lezen. 
  Als u bijvoorbeeld de Dynamics CRM Online-connector gebruikt, controleert de ontwerpfunctie uw CRM-exemplaar voor de beschikbare standaardregels en aangepaste eigenschappen.

## <a name="create-resource-group-project"></a>Resource group-project maken

Als u wilt beginnen, maakt u een [Azure-resourcegroepproject](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) voor uw serverloze app. In Azure maakt u resources binnen een resourcegroep, een logische verzameling die u gebruikt is voor het organiseren, beheren en implementatie van resources voor een volledige app als een enkel actief. Voor een serverloze app in Azure, wordt in de resourcegroep resources bevat voor zowel Azure Logic Apps en Azure Functions. Meer informatie over [Azure-resourcegroepen en resources](../azure-resource-manager/resource-group-overview.md).

1. Start Visual Studio en meld u aan met uw Azure-account. 

1. In het menu **File** selecteert u **New** > **Project**. 

   ![Nieuw project maken in Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. Onder **Installed**, selecteert u **Visual C#** of **Visual Basic**. Selecteer **Cloud** > **Azure Resource Group**.

   Als de **Cloud** categorie of **Azure-resourcegroep** project niet bestaat, zorg ervoor dat u de Azure SDK voor Visual Studio hebt geïnstalleerd.

1. Geef uw project een naam en een locatie en kies vervolgens **OK**. 

   Visual Studio vraagt u een sjabloon te selecteren. 
   U kunt beginnen met een lege, logische App of andere sjabloon, maar in dit voorbeeld wordt een Azure-Quickstart-sjabloon voor het bouwen van een serverloze app met een logische app en een aanroep van een Azure-functie.

   Als u wilt alleen een logische app maken in Visual Studio, selecteer de **logische App** sjabloon. Deze sjabloon maakt een lege logische app die wordt geopend in de Logic App Designer zonder te hoeven gebruikersapparaat van uw oplossing in een Azure-resourcegroep.

1. Onder **sjablonen vanaf deze locatie weergeven**, selecteer **Azure Quickstart (github/Azure/azure-quickstart-sjablonen)**. 

1. Typ in het zoekvak 'logica-app' als filter, en selecteert u deze serverloze quickstart-sjabloon en kies **OK**: **101-logic-app-and-function-app**

   ![Azure quickstart-sjabloon selecteren](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio maakt en een oplossing voor uw resourcegroepproject wordt geopend. 
   De quickstart-sjabloon die u hebt geselecteerd wordt gemaakt van een sjabloon voor de implementatie met de naam `azuredeploy.json` binnen uw resourcegroepproject. 
   Deze sjabloon voor de implementatie bevat de definitie voor een eenvoudige logische app die wordt geactiveerd op een HTTP-aanvraag, een Azure function aanroept en retourneert het resultaat als een HTTP-antwoord. 
   
   ![Nieuwe serverless oplossing](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Vervolgens moet u uw oplossing implementeren op Azure voordat u kunt openen van de sjabloon voor de implementatie en bekijk de resources voor uw serverloze app. 

## <a name="deploy-your-solution"></a>Uw oplossing implementeren

Voordat u uw logische app met de ontwerper van logische App in Visual Studio openen kunt, moet u een Azure-resourcegroep die al geïmplementeerd in Azure hebben. De ontwerpfunctie voor kunt verbindingen met resources en services vervolgens in uw logische app maken. Implementeer uw oplossing in Visual Studio naar de Azure-portal voor deze taak.

1. Open het snelmenu van het resourceproject in Solution Explorer en selecteer vervolgens **implementeren** > **nieuw**.

   ![Nieuwe implementatie voor de resourcegroep maken](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Als u nog niet is geselecteerd, selecteert u uw Azure-abonnement en de resourcegroep waar u wilt implementeren. Kies **implementeren**.

   ![Implementatie-instellingen](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Als de **Parameters bewerken** verschijnt, geeft u de resourcenaam moet worden gebruikt voor uw logische app en uw Azure-functie-app tijdens de implementatie en sla vervolgens uw instellingen. Zorg ervoor dat u gebruikt een wereldwijd unieke naam voor uw functie-app.

   ![Namen voor uw logische app en de functie-app](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Wanneer Visual Studio wordt gestart voor implementatie aan de opgegeven resourcegroep, de status van de implementatie van uw oplossing wordt weergegeven in de Visual Studio **uitvoer** venster. 
   Nadat de implementatie is voltooid, is uw logische app live in Azure portal.

## <a name="edit-logic-app-in-visual-studio"></a>Logische app in Visual Studio bewerken

Nu dat uw oplossing wordt geïmplementeerd voor de resourcegroep, opent u uw logische app met de ontwerper van logische App, zodat u kunt bewerken en wijzigen van uw logische app.

1. Open in Solution Explorer de `azuredeploy.json` het snelmenu van het bestand en selecteer vervolgens **openen met Logic App Designer**.

   !["Azuredeploy.json" in Logic App Designer openen](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Na de **eigenschappen logische App** verschijnt en als u nog niet is geselecteerd, klikt u onder **abonnement**, selecteer uw Azure-abonnement. Onder **resourcegroep**, selecteer de resourcegroep en de locatie waar u uw oplossing hebt geïmplementeerd, en kies vervolgens **OK**.

   ![Eigenschappen logische app](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Nadat de ontwerper van logische App wordt geopend, kunt u doorgaan met het toevoegen van stappen of de werkstroom wijzigen en uw updates opslaan.

   ![Geopende logische app in Logic App Designer](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Azure Functions-project maken

Volg de stappen in het artikel voor het maken van uw Functions-project en de functie met JavaScript, Python, F #, PowerShell, Batch of Bash, [werken met Azure Functions Core Tools](../azure-functions/functions-run-local.md). Voor het ontwikkelen van uw Azure-functie met C# binnen uw oplossing, kunt u een C#-klassenbibliotheek gebruiken met de volgende stappen in het artikel [een .NET-klassebibliotheek als een functie-App publiceren](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>Functies van Visual Studio implementeren

De sjabloon voor de implementatie wordt geïmplementeerd voor alle Azure-functies die u hebt in uw oplossing uit de Git-opslagplaats die opgegeven door de variabelen in de `azuredeploy.json` bestand. Als u maken en ontwerpen van uw Functions-project in uw oplossing, kunt u controleren dat project in de Git-bronbeheer, zoals GitHub of Visual Studio Team Services, en werk vervolgens de `repo` variabele zodat de sjabloon implementeert uw Azure-functie.

## <a name="manage-logic-apps-and-view-run-history"></a>Beheren van logic apps en de uitvoeringsgeschiedenis weergeven

Voor logic apps al geïmplementeerd in Azure, kunt u nog steeds bewerken, beheren, uitvoeringsgeschiedenis weergeven en uitschakelen van deze apps in Visual Studio. 

1. Uit de **weergave** menu van Visual Studio, open **Cloud Explorer**. 

1. Onder **alle abonnementen**, selecteer het Azure-abonnement dat is gekoppeld aan de logische apps die u wilt beheren en kies **toepassen**.

1. Onder **Logic Apps**, selecteert u uw logische app. Selecteer in het snelmenu van die app, **openen met Logic App-Editor**. 

U kunt nu de al gepubliceerde logische app downloaden naar uw resourcegroepproject. Dus Hoewel hebt u een logische app in Azure portal gestart mogelijk, kunt u nog steeds importeren en beheren van de app in Visual Studio. Zie voor meer informatie, [logische apps beheren met Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Volgende stappen

* [Een serverloos sociale dashboard maken](logic-apps-scenario-social-serverless.md)
* [Logische apps beheren met Visual Studio](manage-logic-apps-with-visual-studio.md)
* [Definitietaal van de werkstroom voor logische App](logic-apps-workflow-definition-language.md)