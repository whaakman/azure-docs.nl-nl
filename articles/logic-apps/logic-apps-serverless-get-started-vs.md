---
title: Bouw serverloze apps met Azure Logic Apps en Azure Functions in Visual Studio
description: Bouwen, implementeren en beheren van uw eerste serverloze app met Azure Logic Apps en Azure Functions in Visual Studio
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 04/02/2019
ms.openlocfilehash: 39b44668a89ce0c77c09a7fa20dc4d95b2164bf4
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862995"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Uw eerste serverloze app bouwen met Azure Logic Apps en Azure Functions - Visual Studio

U kunt snel ontwikkelen en implementeren van cloud-apps met behulp van de serverloze hulpprogramma's en mogelijkheden in Azure, zoals [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Azure Functions](../azure-functions/functions-overview.md). In dit artikel laat zien hoe om te beginnen het bouwen van een serverloze app, die gebruikmaakt van een logische app die een Azure-functie in Visual Studio-aanroepen. Zie voor meer informatie over de serverloze oplossingen in Azure, [Serverloze Azure Functions en Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt een serverloze app bouwen in Visual Studio, moet u deze items:

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Download en installeer deze hulpprogramma's als u ze nog niet hebt:

  * <a href="https://aka.ms/download-visual-studio" target="_blank">Met Visual Studio 2019, 2017 of 2015 - Community edition of hoger</a>. 
  Deze snelstart maakt gebruik van Visual Studio Community 2017, dit is gratis.

    > [!IMPORTANT]
    > Wanneer u Visual Studio 2019 of 2017 installeert, zorg ervoor dat u selecteert de **Azure-ontwikkeling** werkbelasting.
    > Voor Visual Studio 2019, Cloud Explorer Logic App Designer kunt openen in de Azure-portal, maar kan nog niet ingesloten Logic App Designer openen.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">Microsoft Azure SDK voor .NET (2.9.1 of hoger)</a>. Meer informatie over <a href="https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet">Azure SDK voor .NET</a>.

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Azure Logic Apps-hulpprogramma's voor de versie van Visual Studio die u wilt:

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019" target="_blank">Visual Studio 2019</a>

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017" target="_blank">Visual Studio 2017</a>

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015" target="_blank">Visual Studio 2015</a>
  
    U kunt hulpprogramma's van Azure Logic Apps ofwel rechtstreeks vanuit Visual Studio Marketplace downloaden en installeren, of leer <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">deze extensie te installeren vanuit Visual Studio</a>. 
    Zorg ervoor dat u Visual Studio opnieuw opstart na de installatie.

  * <a href="https://www.npmjs.com/package/azure-functions-core-tools" target="_blank">Azure Functions Core Tools</a> voor lokaal functies voor foutopsporing

* Toegang tot het web tijdens het gebruik van de ingebouwde ontwerpfunctie van Logic App

  De ontwerpfunctie moet over een internetverbinding beschikken om resources te maken in Azure en de eigenschappen en gegevens van connectoren in uw logische app te lezen. 
  Als u bijvoorbeeld de Dynamics CRM Online-connector gebruikt, controleert de ontwerpfunctie uw CRM-exemplaar voor de beschikbare standaardregels en aangepaste eigenschappen.

## <a name="create-resource-group-project"></a>Resource group-project maken

Als u wilt beginnen, maakt u een [Azure-resourcegroepproject](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) voor uw serverloze app. In Azure maakt u resources binnen een resourcegroep, een logische verzameling die u gebruikt is voor het organiseren, beheren en implementatie van resources voor een volledige app als een enkel actief. Voor een serverloze app in Azure, wordt in de resourcegroep resources bevat voor zowel Azure Logic Apps en Azure Functions. Meer informatie over [Azure-resourcegroepen en resources](../azure-resource-manager/resource-group-overview.md).

1. Start Visual Studio en meld u aan met uw Azure-account.

1. In het menu **File** selecteert u **New** > **Project**.

   ![Nieuw project maken in Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. Onder **Installed**, selecteert u **Visual C#** of **Visual Basic**. Selecteer **Cloud** > **Azure Resource Group**.

   > [!NOTE]
   > Als de **Cloud** categorie of **Azure-resourcegroep** project niet bestaat, zorg ervoor dat u de Azure SDK voor Visual Studio hebt geïnstalleerd.

   Als u Visual Studio 2019, volg deze stappen:

   1. In de **een nieuw project maken** Schakel de **Azure-resourcegroep** projectsjabloon, maken voor een visueel element C# of Visual Basic, en kies **volgende**.

   1. Geef de naam voor de Azure-resourcegroep die u wilt gebruiken en andere projectinformatie. Wanneer u klaar bent, kiest u **Maken**.

1. Geef uw project een naam en een locatie en kies vervolgens **OK**.

   Visual Studio vraagt u een sjabloon selecteren uit de lijst met sjablonen. 
   Dit voorbeeld wordt een Azure-Quickstart-sjabloon, zodat u een serverloze app met een logische app en een aanroep van een Azure-functie kunt ontwikkelen.

   > [!TIP]
   > In scenario's waar u uw oplossing gebruikersapparaat in een Azure-resourcegroep niet wilt, kunt u het leeg **logische App** sjabloon die u zojuist hebt gemaakt een lege logische app.

1. Uit de **sjablonen vanaf deze locatie weergeven** in de lijst met **Azure Quickstart (github.com/Azure/azure-quickstart-templates)**.

1. Typ 'logica-app' als filter in het zoekvak. Selecteer deze sjabloon in de resultaten: **101-logic-app-and-function-app**

   ![Azure quickstart-sjabloon selecteren](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio maakt en een oplossing voor uw resourcegroepproject wordt geopend. 
   De geselecteerde Azure Quickstart-sjabloon maakt u een sjabloon voor de implementatie met de naam `azuredeploy.json` binnen uw resourcegroepproject. Deze sjabloon voor de implementatie bevat de definitie voor een eenvoudige logische app die wordt geactiveerd op een HTTP-aanvraag, een Azure function aanroept en retourneert het resultaat als een HTTP-antwoord.

   ![Nieuwe serverless oplossing](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Vervolgens moet u uw oplossing implementeren op Azure voordat u kunt openen van de sjabloon voor de implementatie en bekijk de resources voor uw serverloze app.

## <a name="deploy-your-solution"></a>Uw oplossing implementeren

Voordat u uw logische app met de ontwerper van logische App in Visual Studio openen kunt, moet u een Azure-resourcegroep die al geïmplementeerd in Azure hebben. De ontwerpfunctie voor kunt verbindingen met resources en services vervolgens in uw logische app maken. Implementeer uw oplossing in Visual Studio naar de Azure-portal voor deze taak.

1. Klik in Solution Explorer in het snelmenu van het resourceproject, selecteer **implementeren** > **nieuw**.

   ![Nieuwe implementatie voor de resourcegroep maken](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Als u nog niet is geselecteerd, selecteert u uw Azure-abonnement en de resourcegroep waar u wilt implementeren. Kies **implementeren**.

   ![Implementatie-instellingen](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Als de **Parameters bewerken** verschijnt, geeft u de resourcenaam moet worden gebruikt voor uw logische app en uw Azure-functie-app tijdens de implementatie en sla vervolgens uw instellingen. Zorg ervoor dat u gebruikt een wereldwijd unieke naam voor uw functie-app.

   ![Namen voor uw logische app en de functie-app](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Wanneer Visual Studio wordt gestart voor implementatie aan de opgegeven resourcegroep, de status van de implementatie van uw oplossing wordt weergegeven in de Visual Studio **uitvoer** venster. 
   Nadat de implementatie is voltooid, is uw logische app live in Azure portal.

## <a name="edit-logic-app-in-visual-studio"></a>Logische app in Visual Studio bewerken

Nu dat uw oplossing wordt geïmplementeerd voor de resourcegroep, opent u uw logische app met Logic App Designer zodat u kunt bewerken en wijzigen van uw logische app.

1. Klik in Solution Explorer van de `azuredeploy.json` van bestand snelkoppeling in het menu **openen met Logic App Designer**.

   !["Azuredeploy.json" in Logic App Designer openen](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Na de **eigenschappen logische App** verschijnt en als u nog niet is geselecteerd, klikt u onder **abonnement**, selecteer uw Azure-abonnement. Onder **resourcegroep**, selecteer de resourcegroep en de locatie waar u uw oplossing hebt geïmplementeerd, en kies vervolgens **OK**.

   ![Eigenschappen logische app](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Nadat de ontwerper van logische App wordt geopend, kunt u doorgaan met het toevoegen van stappen of de werkstroom wijzigen en uw updates opslaan.

   ![Geopende logische app in Logic App Designer](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Azure Functions-project maken

Uw Functions-project en de functie maken met JavaScript, Python, F#, PowerShell, Batch of Bash, volg de stappen in het artikel [werken met Azure Functions Core Tools](../azure-functions/functions-run-local.md). Voor het ontwikkelen van uw Azure-functie met C# binnen uw oplossing, kunt u een C#-klassenbibliotheek gebruiken met de volgende stappen in het artikel [een .NET-klassebibliotheek als een functie-App publiceren](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>Functies van Visual Studio implementeren

De sjabloon voor de implementatie wordt geïmplementeerd voor alle Azure-functies die u hebt in uw oplossing uit de Git-opslagplaats die opgegeven door de variabelen in de `azuredeploy.json` bestand. Als u maken en ontwerpen van uw Functions-project in uw oplossing, kunt u controleren dat project in de Git-bronbeheer, zoals GitHub of Azure DevOps, en werk vervolgens de `repo` variabele zodat de sjabloon implementeert uw Azure-functie.

## <a name="manage-logic-apps-and-view-run-history"></a>Beheren van logic apps en de uitvoeringsgeschiedenis weergeven

Voor logic apps al geïmplementeerd in Azure, kunt u nog steeds bewerken, beheren, uitvoeringsgeschiedenis weergeven en uitschakelen van deze apps in Visual Studio.

1. Uit de **weergave** menu van Visual Studio, open **Cloud Explorer**.

1. Onder **alle abonnementen**, selecteer het Azure-abonnement dat is gekoppeld aan de logische apps die u wilt beheren en kies **toepassen**.

1. Onder **Logic Apps**, selecteert u uw logische app. Selecteer in het snelmenu van die app, **openen met Logic App-Editor**.

U kunt nu de al gepubliceerde logische app downloaden naar uw resourcegroepproject. Dus Hoewel hebt u een logische app in Azure portal gestart mogelijk, kunt u nog steeds importeren en beheren van de app in Visual Studio. Zie voor meer informatie, [logische apps beheren met Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Volgende stappen

* [Logische apps beheren met Visual Studio](manage-logic-apps-with-visual-studio.md)
