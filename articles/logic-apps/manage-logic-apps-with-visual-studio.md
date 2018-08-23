---
title: Logische apps beheren met Visual Studio - Azure Logic Apps | Microsoft Docs
description: Logic apps en andere Azure-assets met Visual Studio Cloud Explorer beheren
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.custom: mvc
ms.date: 03/15/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: d4de75238e48b8eb955095b5a3823f2fed799fae
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42445640"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Logische apps beheren met Visual Studio

Hoewel u maken kunt, bewerken, beheren en implementeren van logische apps in de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, u kunt ook Visual Studio gebruiken als u toevoegen van logische apps wilt voor het besturingselement voor de gegevensbron, verschillende versies publiceren en maken [Azure-Resource Manager](../azure-resource-manager/resource-group-overview.md) sjablonen voor verschillende implementatieomgevingen. U kunt met Visual Studio Cloud Explorer, zoeken en beheren van uw logische apps, samen met andere Azure-resources. Bijvoorbeeld, kunt u openen, downloaden, bewerken, uitvoeren, uitvoeringsgeschiedenis, uitschakelen en inschakelen logische apps die al zijn geïmplementeerd in Azure portal weergeven. Als u geen ervaring met het werken met Azure Logic Apps in Visual Studio, ontdek [over het maken van logische apps met Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Implementeren of een logische app vanuit Visual Studio publiceren overschrijft de versie van de app in Azure portal. Dus als u wijzigingen in de Azure-portal die u wilt behouden aanbrengt, zorg ervoor dat u [vernieuwen van de logische app in Visual Studio](#refresh) vanuit de Azure-portal voordat u de volgende keer dat u implementeert of publiceren vanuit Visual Studio.

<a name="requirements"></a>

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Download en installeer deze hulpprogramma's als u ze nog niet hebt: 

  * <a href="https://www.visualstudio.com/downloads" target="_blank">Visual Studio 2017 of Visual Studio 2015 - Community edition of hoger</a>. 
  Deze snelstart maakt gebruik van Visual Studio Community 2017, dit is gratis.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">Azure SDK (2.9.1 of hoger)</a> en <a href="https://github.com/Azure/azure-powershell#installation" target="_blank">Azure PowerShell</a>

  * <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551" target="_blank">Azure Logic Apps-hulpprogramma's voor Visual Studio 2017</a> of de <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio" target="_blank">versie van Visual Studio 2015</a> 
  
    U kunt hulpprogramma's van Azure Logic Apps ofwel rechtstreeks vanuit Visual Studio Marketplace downloaden en installeren, of leer <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">deze extensie te installeren vanuit Visual Studio</a>. 
    Zorg ervoor dat u Visual Studio opnieuw opstart na de installatie.

* Toegang tot het web tijdens het gebruik van de ingesloten ontwerper van logische Apps

  De ontwerpfunctie moet over een internetverbinding beschikken om resources te maken in Azure en de eigenschappen en gegevens van connectoren in uw logische app te lezen. 
  Als u bijvoorbeeld de Dynamics CRM Online-connector gebruikt, controleert de ontwerpfunctie uw CRM-exemplaar voor de beschikbare standaardregels en aangepaste eigenschappen.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Uw logische apps zoeken

In Visual Studio vindt u alle logic-apps die zijn gekoppeld aan uw Azure-abonnement en worden geïmplementeerd in Azure portal met behulp van Cloud Explorer.

1. Open Visual Studio. Op de **weergave** in het menu **Cloud Explorer**.

2. Kies in de Cloud Explorer, **accountbeheer**. Selecteer het Azure-abonnement dat is gekoppeld aan uw logische apps, en kies vervolgens **toepassen**. Bijvoorbeeld:

   ![Kies "Accountbeheer"](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

2. Op basis van of u nu zoekt door **resourcegroepen** of **resourcetypen**, als volgt te werk:

   * **Resourcegroepen**: onder uw Azure-abonnement, Cloud Explorer ziet u de resourcegroepen die gekoppeld aan dat abonnement zijn. 
   Vouw de resourcegroep die uw logische app bevat, en selecteer vervolgens uw logische app.

   * **Resourcetypen**: onder uw Azure-abonnement, vouw **Logic Apps**. Nadat u Cloud Explorer ziet alle geïmplementeerde logic-apps die gekoppeld aan uw abonnement zijn, selecteert u uw logische app.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Openen in Visual Studio

U kunt logische apps eerder hebt gemaakt en geïmplementeerd via Azure portal of Azure Resource Manager-projecten met Visual Studio openen in Visual Studio.

1. Cloud Explorer openen en zoeken van uw logische app. 

2. Selecteer in het snelmenu van de logische app, **openen met Logic App-Editor**.

   In dit voorbeeld ziet u logische apps per resourcetype, zodat uw logische apps worden weergegeven onder de **Logic Apps** sectie.

  ![Open geïmplementeerde logische app in Azure portal](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Nadat de logische app wordt geopend in de ontwerper van logische Apps, aan de onderkant van het ontwerpprogramma, kunt u kiezen **codeweergave** zodat u de onderliggende definitie structuur van de logische app kunt controleren. 
   Als u maken van een sjabloon voor de implementatie voor de logische app wilt, krijgt u informatie [het downloaden van een Azure Resource Manager-sjabloon](#download-logic-app) voor deze logische app. Meer informatie over [Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Downloaden van Azure

U kunt logische apps downloaden de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> en slaat u ze als [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sjablonen. U kunt vervolgens lokaal bewerken de sjablonen met Visual Studio en aanpassen van logische apps voor verschillende implementatieomgevingen. Automatisch downloaden van logische apps *parameterizes* hun definities in [Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment), die ook JavaScript Object Notation (JSON) gebruiken.

1. In Visual Studio Cloud Explorer openen en zoek en selecteer de logische app die u wilt downloaden van Azure.

2. Selecteer in het snelmenu van die app, **openen met Logic App-Editor**.

   Logic App Designer wordt geopend en ziet u de logische app. 
   Als u wilt controleren van de onderliggende definitie van logische app en de structuur, aan de onderkant van het ontwerpprogramma, kies **codeweergave**. 

3. Kies op de werkbalk van de ontwerper **downloaden**.

   ![Kies 'Downloaden'](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. Wanneer u wordt gevraagd een locatie, blader naar die locatie en het Resource Manager-sjabloon voor de definitie van de logische app opslaan in JSON (.json)-bestandsindeling. 

De definitie van uw logische app wordt weergegeven in de `resources` subsectie in het Resource Manager-sjabloon. U kunt nu de definitie van de logische app en Resource Manager-sjabloon met Visual Studio bewerken. U kunt ook de sjabloon als een Azure Resource Manager-project toevoegen aan een Visual Studio-oplossing. Meer informatie over [Resource Manager voor logische apps in Visual Studio-projecten](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Vernieuwen van Azure

Als u uw logische app in Azure portal bewerken en deze wijzigingen wilt behouden, zorg ervoor dat de versie van de app in Visual Studio met deze wijzigingen wordt vernieuwd. 

* In Visual Studio op de werkbalk Logic App Designer kiezen **vernieuwen**.

  -of-

* Open het snelmenu van uw logische app in Visual Studio Cloud Explorer en selecteer **vernieuwen**. 

![Vernieuwen van de logische app met updates](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Logic app-updates publiceren

Wanneer u klaar bent voor uw logische app-updates implementeren vanuit Visual Studio naar Azure, op de werkbalk Logic App Designer Kies **publiceren**.

![Bijgewerkte logische app publiceren](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Uw logische app uitvoeren handmatig

U kunt een logische app geïmplementeerd in Azure vanuit Visual Studio handmatig activeren. Kies op de werkbalk Logic App Designer **Trigger uitvoeren**.

![Logische app handmatig uitvoeren](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Uitvoeringsgeschiedenis controleren

Als u wilt de status controleren en diagnosticeren van problemen met runs voor logic Apps, kunt u de details bekijken, zoals invoer en uitvoer voor degenen die wordt uitgevoerd in Visual Studio.

1. In Cloud Explorer het snelmenu van uw logische app openen en selecteer **Open uitvoeringsgeschiedenis**.

   ![Open de geschiedenis uitvoeren](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

2. Als u wilt weergeven van de details voor een specifieke uitvoering, dubbelklikt u op een uitvoering. Bijvoorbeeld:

   ![Gedetailleerde uitvoeringsgeschiedenis](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Als u wilt sorteren in de tabel door de eigenschap, kiest u de kolomkop voor die eigenschap. 

3. Vouw de stappen die invoer en uitvoer die u wilt bekijken. Bijvoorbeeld:

   ![Bekijk invoer en uitvoer voor elke stap](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Logische app in- of uitschakelen

Zonder uw logische app te verwijderen, kunt u de trigger uit het activeren van de volgende keer wanneer de triggervoorwaarde wordt voldaan stoppen. Uw logische app uitschakelen wordt voorkomen dat de Logic Apps-engine van het maken en toekomstige werkstroomexemplaren voor uw logische app uitvoeren.
In Cloud Explorer het snelmenu van uw logische app openen en selecteer **uitschakelen**.

![Uw logische app uitschakelen](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

> [!NOTE]
> Wanneer u een logische app uitschakelt, wordt geen nieuwe uitvoeringen worden geïnstantieerd. Alle wordt uitgevoerd en actieve uitvoeringen wordt voortgezet totdat ze zijn voltooid, wat tijd in beslag kan duren. 

Wanneer u gereed voor de logische app hervatten bent, kunt u uw logische app opnieuw activeren. In Cloud Explorer het snelmenu van uw logische app openen en selecteer **inschakelen**.

![Uw logische app inschakelen](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Uw logische app verwijderen

Als u wilt uw logische app verwijderen uit de Azure portal, in Cloud Explorer het snelmenu van uw logische app openen en selecteer **verwijderen**.

![Uw logische app verwijderen](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

> [!NOTE]
> Wanneer u een logische app verwijdert, wordt geen nieuwe uitvoeringen worden geïnstantieerd. Alle wordt uitgevoerd en actieve uitvoeringen zijn geannuleerd. Als u duizenden wordt uitgevoerd hebt, mogelijk annulering aanzienlijke tijd in beslag nemen. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u voor het beheren van geïmplementeerde logische apps met Visual Studio. Vervolgens kunt u meer over het aanpassen van definities voor logische Apps voor implementatie:

> [!div class="nextstepaction"]
> [Definities voor logische Apps maken in JSON](../logic-apps/logic-apps-author-definitions.md)
