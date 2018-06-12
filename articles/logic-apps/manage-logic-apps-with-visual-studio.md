---
title: Logic apps beheren met Visual Studio - Azure Logic Apps | Microsoft Docs
description: Logische apps en andere Azure activa met Visual Studio Cloud Explorer beheren
author: ecfan
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.date: 03/15/2018
ms.author: estfan; LADocs
ms.openlocfilehash: b4d7f557923a67ae0c9fc513cd2b4fe7555241be
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301113"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Logic apps beheren met Visual Studio

U kunt maken, bewerken, beheren en implementeren van logische apps in de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, u kunt Visual Studio ook gebruiken als u toevoegen van logische apps wilt voor het besturingselement voor de gegevensbron, verschillende versies publiceren en maken [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sjablonen voor verschillende omgevingen. Met Visual Studio Cloud Explorer kunt u vinden en beheren van uw logische apps samen met andere Azure-resources. Bijvoorbeeld, kunt u openen, downloaden, bewerken, uitvoeren, uitvoeringsgeschiedenis, uitschakelen en inschakelen logic apps die al zijn geïmplementeerd in de Azure portal bekijken. Als u geen ervaring met het werken met Azure Logic Apps in Visual Studio, ontdek [logic apps maken met Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Overschreven door de versie van die in de Azure portal-app implementeren of publiceren van een logische app vanuit Visual Studio. Dus als u wijzigingen in de Azure portal die u wilt behouden aanbrengt, controleert u of u [vernieuwen van de logische app in Visual Studio](#refresh) vanuit de Azure-portal voordat u de volgende keer dat u implementeert of publiceren vanuit Visual Studio.

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

* Toegang tot het web tijdens het gebruik van de ontwerpfunctie van Logic Apps

  De ontwerpfunctie moet over een internetverbinding beschikken om resources te maken in Azure en de eigenschappen en gegevens van connectoren in uw logische app te lezen. 
  Als u bijvoorbeeld de Dynamics CRM Online-connector gebruikt, controleert de ontwerpfunctie uw CRM-exemplaar voor de beschikbare standaardregels en aangepaste eigenschappen.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Uw logische apps zoeken

U kunt alle logische apps die zijn gekoppeld aan uw Azure-abonnement en worden geïmplementeerd in de Azure portal met behulp van de Cloud Explorer vinden in Visual Studio.

1. Open Visual Studio. Op de **weergave** selecteert u **Cloud Explorer**.

2. Kies in de Cloud Explorer **accountbeheer**. Selecteer de Azure-abonnement gekoppeld aan uw logische apps en kies vervolgens **toepassen**. Bijvoorbeeld:

   ![Kies 'Accountbeheer'](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

2. Op basis van of u nu zoekt door **resourcegroepen** of **brontypen**, als volgt te werk:

   * **Resourcegroepen**: onder uw Azure-abonnement Cloud Explorer toont de brongroepen die gekoppeld aan dat abonnement zijn. 
   Vouw de resourcegroep die uw logische app bevat, en selecteer vervolgens uw logische app.

   * **Brontypen**: vouw onder uw Azure-abonnement **Logic Apps**. Nadat de Cloud Explorer toont alle geïmplementeerde logic apps die gekoppeld aan uw abonnement zijn, selecteert u uw logische app.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Openen in Visual Studio

U kunt logische apps eerder hebt gemaakt en geïmplementeerd rechtstreeks via de Azure portal of als Azure Resource Manager-projecten met Visual Studio openen in Visual Studio.

1. Cloud Explorer openen en zoeken van uw logische app. 

2. Selecteer op de logische app snelmenu **geopend met Logic App Editor**.

   Dit voorbeeld ziet u logische apps per resourcetype, zodat uw logische apps worden weergegeven onder de **Logic Apps** sectie.

  ![Open geïmplementeerde logische app in Azure portal](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Nadat de logische app wordt geopend in Logic Apps Designer onderaan in de ontwerpfunctie, kunt u kiezen **codeweergave** zodat u de onderliggende structuur van logic app-definitie kunt bekijken. 
   Als u maken van een implementatiesjabloon voor de logische app wilt, ontdek [het downloaden van een Azure Resource Manager-sjabloon](#download-logic-app) voor die app logica. Meer informatie over [Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Downloaden van Azure

U kunt downloaden logic apps uit de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> en deze opslaan als [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sjablonen. U kunt lokaal de sjablonen met Visual Studio bewerken en aanpassen van logic apps voor verschillende omgevingen. Automatisch downloaden van logische apps *parameterizes* de definities binnen [Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment), die ook notatie JSON (JavaScript Object) gebruiken.

1. In Visual Studio Cloud Explorer openen en vervolgens zoekt en de logic app selecteren die u wilt downloaden uit Azure.

2. Selecteer in het snelmenu van de app, **geopend met Logic App Editor**.

   De ontwerpfunctie voor Logic App wordt geopend en ziet u de logische app. 
   Als de onderliggende definitie en onder aan het designer-structuur van de logische app wilt bekijken kiezen **codeweergave**. 

3. Kies op de werkbalk designer **downloaden**.

   ![Kies 'Downloaden'](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. Wanneer u wordt gevraagd om een locatie, bladert u naar die locatie en opslaan van de Resource Manager-sjabloon voor de definitie van logische apps in JSON (.json)-bestandsindeling. 

De definitie van de logische app wordt weergegeven in de `resources` subsectie in het Resource Manager-sjabloon. U kunt nu de definitie van logic Apps en Resource Manager-sjabloon met Visual Studio bewerken. U kunt ook de sjabloon als een Azure Resource Manager-project toevoegen aan een Visual Studio-oplossing. Meer informatie over [Resource Manager voor logische apps in Visual Studio-projecten](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Vernieuwen van Azure

Als u uw logische app in de Azure portal bewerken en deze wijzigingen wilt behouden, ervoor zorgen dat de versie van de app in Visual Studio met deze wijzigingen wordt vernieuwd. 

* In Visual Studio op de werkbalk Logic App-ontwerper kiezen **vernieuwen**.

  -of-

* Open uw logische app snelmenu in Visual Studio Cloud Explorer en selecteer **vernieuwen**. 

![Vernieuwen van de logische app met updates](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Logic app-updates publiceren

Wanneer u klaar bent om uw logische app-updates implementeren vanuit Visual Studio voor Azure, op de werkbalk Logic App-ontwerper kiezen **publiceren**.

![Bijgewerkte logic app publiceren](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Uw logische app uitvoeren handmatig

U kunt handmatig een logische app geïmplementeerd in Azure vanuit Visual Studio activeren. Kies op de werkbalk Logic App-ontwerper **uitvoeren Trigger**.

![Handmatig logische app uitvoeren](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Uitvoeringsgeschiedenis controleren

Controleer de status en analyseren van problemen met logic app wordt uitgevoerd, kunt u de details bekijken, zoals in- en uitgangen voor die wordt uitgevoerd in Visual Studio.

1. Open uw logische app snelmenu in Cloud Explorer en selecteer **Open uitvoeringsgeschiedenis**.

   ![Open de geschiedenis uitvoeren](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

2. Om de details voor een specifieke groep weergeven, dubbelklikt u op een uitvoering. Bijvoorbeeld:

   ![Gedetailleerde uitvoeringsgeschiedenis](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Als u wilt sorteren in de tabel op de eigenschap, kiest u de kolomkop voor die eigenschap. 

3. Vouw de stappen waarvan u de invoer en uitvoer die u wilt bekijken. Bijvoorbeeld:

   ![Weergave in- en uitgangen voor elke stap](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Logische app in- of uitschakelen

U kunt de trigger van het activeren van de volgende keer wanneer de triggervoorwaarde wordt voldaan stoppen zonder uw logische app te verwijderen. Uw logische app uit te schakelen voorkomt u dat de engine Logic Apps maken en toekomstige workflowexemplaren voor uw logische app uitgevoerd.
Open uw logische app snelmenu in Cloud Explorer en selecteer **uitschakelen**.

![Uw logische app uitschakelen](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

Wanneer u klaar bent voor uw logische app bewerking hervatten, kunt u uw logische app opnieuw activeren. Open uw logische app snelmenu in Cloud Explorer en selecteer **inschakelen**.

![Uw logische app inschakelen](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Uw logische app verwijderen

Als u wilt verwijderen van uw logische app vanuit de Azure-portal in de Cloud Explorer open uw logische app snelkoppeling menu en selecteer **verwijderen**.

![Uw logische app verwijderen](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe geïmplementeerde logic apps beheren met Visual Studio. Vervolgens leert over het aanpassen van logic app-definities voor implementatie:

> [!div class="nextstepaction"]
> [Auteur logic app-definities in JSON](../logic-apps/logic-apps-author-definitions.md)
