---
title: Geautomatiseerde werkstromen maken met Visual Studio - Azure Logic Apps
description: Het automatiseren van taken, bedrijfsprocessen en werkstromen voor bedrijfsintegratie met behulp van Azure Logic Apps en Visual Studio
services: logic-apps
ms.service: logic-apps
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.topic: quickstart
ms.custom: mvc
ms.reviewer: klam, LADocs
ms.suite: integration
ms.date: 04/25/2019
ms.openlocfilehash: afa539bc3369e4f9d9ecf27340436e0be70a03ad
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190607"
---
# <a name="quickstart-create-automated-tasks-processes-and-workflows-with-azure-logic-apps---visual-studio"></a>Quickstart: Geautomatiseerde taken, processen en werkstromen maken met Azure Logic Apps - Visual Studio

Met [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en Visual Studio kunt u werkstromen maken voor het automatiseren van taken en processen om apps, gegevens, systemen en services van bedrijven en organisaties te integreren. Deze quickstart laat zien hoe u kunt ontwerpen en deze werkstromen kunt maken met het maken van logische apps in Visual Studio en implementeren van deze apps naar Azure. Maar u deze taken in Azure portal uitvoeren kunt, wordt Visual Studio kunt u uw logische apps voor het besturingselement voor de gegevensbron, verschillende versies publiceren en maken van Azure Resource Manager-sjablonen voor verschillende implementatieomgevingen kunt toevoegen.

Als u geen ervaring hebt met Azure Logic Apps en alleen de basisconcepten wilt, kunt u de [Quick Start voor het maken van een logische app in Azure portal](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic App Designer werkt op dezelfde manier in Azure portal en Visual Studio.

In deze snelstartgids gebruikt maken u dezelfde logische app met Visual Studio als de Azure portal-snelstartgids. Deze logische app bewaakt de RSS-feed van een website en verzendt een e-mail voor elk nieuw item op dat kanaal. Uw voltooide logische app ziet eruit als deze werkstroom op hoog niveau:

![Voltooide logische app](./media/quickstart-create-logic-apps-with-visual-studio/overview.png)

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Download en installeer deze hulpprogramma's als u ze nog niet hebt:

  * [Met Visual Studio 2019, 2017 of 2015 - Community edition of hoger](https://aka.ms/download-visual-studio). 
  In deze snelstartgids maakt gebruik van Visual Studio Community 2017.

    > [!IMPORTANT]
    > Wanneer u Visual Studio 2019 of 2017 installeert, zorg ervoor dat u selecteert de **Azure-ontwikkeling** werkbelasting.

  * [Microsoft Azure SDK voor .NET (2.9.1 of hoger)](https://azure.microsoft.com/downloads/). 
  Meer informatie over [Azure SDK voor .NET](https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet).

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Azure Logic Apps-hulpprogramma's voor de versie van Visual Studio die u wilt:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)
  
    U kunt hulpprogramma's van Azure Logic Apps ofwel rechtstreeks vanuit Visual Studio Marketplace downloaden en installeren, of leer [deze extensie te installeren vanuit Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
    Zorg ervoor dat u Visual Studio opnieuw opstart na de installatie.

* Toegang tot het web tijdens het gebruik van de ingebouwde ontwerpfunctie van Logic App

  De ontwerpfunctie moet een internetverbinding beschikken om resources te maken in Azure en eigenschappen en gegevens van connectoren in uw logische app lezen. 
  Voor Dynamics CRM Online-verbindingen, bijvoorbeeld, controleert de ontwerpfunctie uw CRM-exemplaar voor standaardregels en aangepaste eigenschappen.

* Een e-mailaccount dat door Logic Apps wordt ondersteund, bijvoorbeeld Office 365 Outlook, Outlook.com of Gmail. Voor andere providers [kunt u hier de lijst met connectors bekijken](https://docs.microsoft.com/connectors/). In dit voorbeeld maakt gebruik van Office 365 Outlook. Als u een ander e-mailaccount gebruikt, zijn de algemene stappen hetzelfde, maar ziet de gebruikersinterface er misschien iets anders uit.

## <a name="create-azure-resource-group-project"></a>Een Azure-resourcegroepproject maken

Om te beginnen, maakt u een [Azure-resourcegroepproject](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Meer informatie over [Azure-resourcegroepen en resources](../azure-resource-manager/resource-group-overview.md).

1. Start Visual Studio. Meld u aan met uw Azure-account.

1. In het menu **File** selecteert u **New** > **Project**. (Toetsenbord: Ctrl+Shift+N)

   ![In het menu File selecteert u New > Project](./media/quickstart-create-logic-apps-with-visual-studio/create-new-visual-studio-project.png)

1. Onder **Installed**, selecteert u **Visual C#** of **Visual Basic**. Selecteer **Cloud** > **Azure Resource Group**. Geef uw project een naam, bijvoorbeeld:

   ![Een Azure-resourcegroepproject maken](./media/quickstart-create-logic-apps-with-visual-studio/create-azure-cloud-service-project.png)

   > [!NOTE]
   > Als **Cloud** of **Azure-resourcegroep** niet wordt weergegeven, zorg ervoor dat u de Azure SDK voor Visual Studio installeren.

   Als u Visual Studio 2019, volg deze stappen:

   1. In de **een nieuw project maken** Schakel de **Azure-resourcegroep** -project voor Visual C# of Visual Basic. Kies **Volgende**.

   1. Geef een naam voor de Azure-resourcegroep die u wilt gebruiken en andere projectinformatie. Kies **Maken**.

1. Selecteer in de lijst met sjablonen, de **logische App** sjabloon. Kies **OK**.

   ![Sjabloon voor Logic App kiezen](./media/quickstart-create-logic-apps-with-visual-studio/select-logic-app-template.png)

   Nadat Visual Studio het project maakt, wordt Solution Explorer geopend en ziet u uw oplossing. 
   In uw oplossing de **LogicApp.json** bestand slaat de definitie van uw logische app niet alleen maar is ook een Azure Resource Manager-sjabloon die u voor implementatie gebruiken kunt.

   ![Solution Explorer toont de nieuwe logische app-oplossing en het implementatie-bestand](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-solution-created.png)

## <a name="create-blank-logic-app"></a>Een lege, logische app maken

Wanneer u hebt uw Azure-resourcegroep-project, maakt u uw logische app met de **lege logische App** sjabloon.

1. Open in Solution Explorer de **LogicApp.json** snelmenu van het bestand. Selecteer **Openen met Logic App-ontwerpfunctie**. (Toetsenbord: Ctrl+L)

   ![Open het .json-bestand van de logische app met de Logic App-ontwerpfunctie](./media/quickstart-create-logic-apps-with-visual-studio/open-logic-app-designer.png)

   Visual Studio vraagt u om uw Azure-abonnement en een Azure-resourcegroep voor het maken en implementeren van resources voor uw logische app en verbindingen.

1. Voor **abonnement**, selecteer uw Azure-abonnement. Voor **resourcegroep**, selecteer **nieuw** te maken van een nieuwe Azure-resourcegroep.

   ![Azure-abonnement, resourcegroep en resourcelocatie selecteren](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-location.png)

   | Instelling | Voorbeeldwaarde | Description |
   | ------- | ------------- | ----------- |
   | Lijst met gebruikersprofielen | Contoso <br> jamalhartnett@contoso.com | Standaard het account dat u gebruikt voor aanmelden |
   | **Abonnement** | Betalen per gebruik <br> (jamalhartnett@contoso.com) | De naam voor uw Azure-abonnement en het bijbehorende account |
   | **Resourcegroep** | MyLogicApp-RG <br> (US - west) | De Azure-resourcegroep en locatie voor het opslaan en implementeren van resources van uw logische app |
   | **Locatie** | MyLogicApp-RG2 <br> (US - west) | Een andere locatie als u de locatie van de resourcegroep niet wilt gebruiken |
   ||||

1. Ontwerper van logische Apps opent een pagina ziet u een inleiding video en veelgebruikte triggers. Scroll omlaag voorbij de video en triggers **sjablonen**, en selecteer **lege logische App**.

   ![Selecteer Lege logische app](./media/quickstart-create-logic-apps-with-visual-studio/choose-blank-logic-app-template.png)

## <a name="build-logic-app-workflow"></a>Een werkstroom voor uw logische app compileren

Vervolgens voegt u toe een RSS [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) die wordt geactiveerd wanneer een nieuw feed-item wordt weergegeven. Elke logische app begint met een trigger, die wordt geactiveerd wanneer specifieke criteria wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, maakt de Logic Apps-engine een instantie van een logische app die de werkstroom uitvoert.

1. In Logic App Designer, onder het zoekvak typt, kiest u **alle**.
Typ 'rss' in het zoekvak. Selecteer deze trigger uit de lijst met triggers: **Wanneer een feeditem wordt gepubliceerd - RSS**

   ![Uw logische app bouwen door toevoeging van een trigger en acties](./media/quickstart-create-logic-apps-with-visual-studio/add-trigger-logic-app.png)

1. Nadat de trigger wordt weergegeven in de ontwerpfunctie, hebt u de logische app door de werkstroom te volgen stappen als bij het bouwen de [Azure portal-snelstartgids](../logic-apps/quickstart-create-first-logic-app-workflow.md#add-rss-trigger), keer daarna terug naar dit artikel. Wanneer u klaar bent, ziet uw logische app eruit zoals in dit voorbeeld:

   ![Voltooide logische app](./media/quickstart-create-logic-apps-with-visual-studio/finished-logic-app.png)

1. Sla uw Visual Studio-oplossing. (Toetsenbord: Ctrl + S)

## <a name="deploy-logic-app-to-azure"></a>Logische app implementeren in Azure

Voordat u kunt uitvoeren en testen van uw logische app, implementeert u de app naar Azure vanuit Visual Studio.

1. Selecteer in Solution Explorer in het snelmenu van uw project **Implementeren** > **Nieuw**. Meld u aan met uw Azure-account als u daarom wordt gevraagd.

   ![Een logische app-implementatie maken](./media/quickstart-create-logic-apps-with-visual-studio/create-logic-app-deployment.png)

1. Behoud de standaardwaarde Azure-abonnement, resource groep en andere instellingen voor deze implementatie. Kies **implementeren**.

   ![Logische app implementeren naar een Azure-resourcegroep](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-deployment.png)

1. Als de **Parameters bewerken** verschijnt, geef de naam van een resource voor uw logische app. Uw instellingen opslaan.

   ![Naam van de implementatie opgeven voor de logische app](./media/quickstart-create-logic-apps-with-visual-studio/edit-parameters-deployment.png)

   Wanneer de implementatie wordt gestart, wordt de status van de implementatie van uw app weergegeven in het **Uitvoer**-venster van Visual Studio. Als de status niet wordt weergegeven, opent u de lijst **Uitvoer tonen van** en selecteert u uw Azure-resourcegroep.

   ![Uitvoer implementatiestatus](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-output-window.png)

   Als uw geselecteerde connectors invoer moeten, wordt een PowerShell-venster wordt geopend op de achtergrond en vraagt om eventuele benodigde wachtwoorden of de geheime codes. Nadat u deze gegevens hebt ingevoerd, wordt de implementatie voortgezet.

   ![PowerShell-venster](./media/quickstart-create-logic-apps-with-visual-studio/logic-apps-powershell-window.png)

   Nadat de implementatie is voltooid, wordt uw logische app Live in Azure portal en wordt uitgevoerd op de opgegeven planning (elke minuut). Als de trigger nieuwe feed items vindt, de trigger wordt geactiveerd, deze toepassing maakt een werkstroomexemplaar dat uw logische app de acties worden uitgevoerd. Uw logische app verzendt een e-mail voor elk nieuw item. Als de trigger geen nieuwe items vindt, de trigger wordt niet gestart en 'overslaan' instantiëren van de werkstroom. Uw logische app wordt er gewacht tot het volgende interval voordat er controle plaatsvindt.

   Hier vindt u voorbeeld e-mails die deze logische app verzendt. 
   Als u geen een e-mailberichten ontvangt, controleert u de map Ongewenste e-mail.

   ![Outlook verzendt een e-mail voor elk nieuw RSS-item](./media/quickstart-create-logic-apps-with-visual-studio/outlook-email.png)

Gefeliciteerd, u hebt gemaakt en geïmplementeerd van uw logische app met Visual Studio. Zie [Logische apps beheren met Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md) om uw logische app te beheren en de uitvoeringsgeschiedenis te inspecteren.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met uw logische app, verwijdert u de resourcegroep die uw logische app en verwante resources bevat.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met hetzelfde account dat is gebruikt voor het maken van uw logische app.

1. Selecteer **Resourcegroepen** in het hoofdmenu van Azure.
Selecteer de resourcegroep van uw logische app en selecteer **overzicht**.

1. Kies op de pagina **Overzicht** de optie **Resourcegroep verwijderen**. Voer ter bevestiging de naam van de resourcegroep in en kies **Verwijderen**.

   !['Resourcegroepen' > 'Overzicht' > 'Resourcegroep verwijderen'](./media/quickstart-create-logic-apps-with-visual-studio/delete-resource-group.png)

1. Verwijder de Visual Studio-oplossing van uw lokale computer.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u uw logische app met Visual Studio gebouwd, geïmplementeerd en uitgevoerd. Voor meer informatie over het beheren en uitvoeren van geavanceerde implementatie voor logische apps met Visual Studio, Zie de volgende artikelen:

> [!div class="nextstepaction"]
> * [Logische apps beheren met Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md)