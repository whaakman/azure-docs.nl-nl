---
title: Maken, bouwen en implementeren van logische apps in Visual Studio - Azure Logic Apps | Microsoft Docs
description: Visual Studio-projecten maken zodat u kunt ontwerpen, bouwen en implementeren van Azure Logic Apps.
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: e7f5cf483d22e4c60dedbe5176ceb0bc8b2b6e66
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Ontwerpen, bouwen en implementeren van Azure Logic Apps in Visual Studio

Hoewel de [Azure-portal](https://portal.azure.com/) biedt een goede manier om te maken en beheren van Azure Logic Apps, u kunt Visual Studio gebruiken voor het ontwerpen, bouwen en implementeren van uw logische apps. Visual Studio biedt uitgebreide hulpmiddelen zoals de Logic App-ontwerper voor u logic apps maken, implementatie en automatisering sjablonen configureren en implementeren met elke omgeving. 

Aan de slag met Azure Logic Apps meer [uw eerste logische app maken in de Azure-portal](logic-apps-create-a-logic-app.md).

## <a name="installation-steps"></a>Installatiestappen

Als u wilt installeren en configureren van Visual Studio-hulpprogramma's voor Azure Logic Apps, de volgende stappen uit.

### <a name="prerequisites"></a>Vereisten

* [Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) of Visual Studio 2015
* [Nieuwste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 of hoger)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Toegang tot het Internet wanneer u de ontwerpfunctie

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Installeer Visual Studio-hulpprogramma's voor Azure Logic Apps

Nadat u de vereisten:

1. Open Visual Studio. Op de **extra** selecteert u **uitbreidingen en Updates**.
2. Vouw de **Online** categorie zodat u kunt online zoeken.
3. Blader of zoek naar **Logic Apps** totdat u **Azure Logic Apps-Tools voor Visual Studio**.
4. Als u wilt downloaden en installeren van de extensie, klikt u op **downloaden**.
5. Visual Studio starten na de installatie.

> [!NOTE]
> U kunt ook downloaden [Azure Logic Apps-Tools voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) en de [Azure Logic Apps-Tools voor Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) rechtstreeks vanuit Visual Studio Marketplace.

Nadat u de installatie hebt voltooid, kunt u de Azure-resourcegroepproject met Logic App-ontwerper.

## <a name="create-your-project"></a>Uw project maken

1. Op de **bestand** menu, gaat u naar **nieuw**, en selecteer **Project**. Of als uw project toevoegen aan een bestaande oplossing, gaat u naar **toevoegen**, en selecteer **nieuw Project**.

    ![Menu Bestand](./media/logic-apps-deploy-from-vs/filemenu.png)

2. In de **nieuw Project** venster vinden **Cloud**, en selecteer **Azure-resourcegroep**. Naam van uw project en klik op **OK**.

    ![Nieuw project toevoegen](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Selecteer de **logische App** sjabloon die u maakt een lege logic app-implementatiesjabloon moet worden gebruikt. Nadat u de sjabloon hebt geselecteerd, klikt u op **OK**.

    ![Selecteer de sjabloon voor logische Apps](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    U hebt nu uw app-project logica toegevoegd aan uw oplossing. 
    Uw implementatiebestand moet in Solution Explorer, worden weergegeven.

    ![Implementatiebestand](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>Uw logische app maken met Logic App-ontwerper

Wanneer u een Azure-resourcegroepproject met een logische app hebt, kunt u de ontwerpfunctie voor Logic App openen in Visual Studio om uw werkstroom te maken. 

> [!NOTE]
> De ontwerpfunctie vereist een internetverbinding beschikken om de query-connectors voor de beschikbare eigenschappen en -gegevens. Als u de Dynamics CRM Online-connector gebruikt, vraagt de ontwerpfunctie voor uw CRM-exemplaar om weer te geven van de beschikbare aangepaste en de standaardeigenschappen.

1. Met de rechtermuisknop op uw `<template>.json` bestand en selecteer **openen met Logic App-ontwerper**. (`Ctrl+L`)

2. Kies uw Azure-abonnement, resourcegroep en locatie voor de implementatiesjabloon.

    > [!NOTE]
    > Het ontwerpen van een logische app, maakt resources van de API-verbinding die query voor eigenschappen tijdens de ontwerp. Visual Studio gebruikt de geselecteerde resourcegroep voor deze verbindingen maken tijdens de ontwerpfase. Als u wilt weergeven of wijzigen van de API-verbindingen, gaat u naar de Azure-portal en bladert u naar **API verbindingen**.

    ![Abonnement kiezen](./media/logic-apps-deploy-from-vs/designer_picker.png)

    De ontwerpfunctie maakt gebruik van de definitie van de `<template>.json` -bestand voor rendering.

4. Maken en ontwerpen van uw logische app. Uw implementatiesjabloon is bijgewerkt met de wijzigingen.

    ![App-ontwerper logica in Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

Visual Studio voegt `Microsoft.Web/connections` bronnen zijn voor uw bronbestand voor de verbindingen uw logische app vereist is voor werking. De eigenschappen van deze verbinding kunnen worden ingesteld bij het implementeren van, en nadat u hebt geïmplementeerd beheerd **API verbindingen** in de Azure portal.

### <a name="switch-to-json-code-view"></a>Overschakelen naar de codeweergave JSON

Als u wilt weergeven in de JSON-weergave voor uw logische app, selecteer de **codeweergave** tabblad aan de onderkant van de ontwerpfunctie.

Als u wilt overschakelen naar de volledige resource JSON, met de rechtermuisknop op de `<template>.json` bestand en selecteer **Open**.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Verwijzingen voor afhankelijke resources toevoegen aan Visual Studio-implementatiesjablonen

Als u wilt dat uw logische app om te verwijzen naar afhankelijke resources, kunt u [Azure Resource Manager-sjabloonfuncties](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in uw sjabloon logic app-implementatie. Bijvoorbeeld, kunt u uw logische app om te verwijzen naar een Azure-functie of integratiepakket account die u samen met uw logische app wilt implementeren. Volg deze richtlijnen over het gebruik van parameters in de implementatiesjabloon voor, zodat de Logic App Designer correct wordt gerenderd. 

U kunt logische app parameters in deze soorten triggers en acties:

*   Onderliggende werkstroom
*   Functie-app
*   APIM aanroep
*   De URL van de runtime verbinding API
*   Verbindingspad API

En u kunt een sjabloonfuncties, zoals parameters, variabelen, resourceId, concat, enzovoort. Dit is hoe u de resource-ID van het Azure-functie kunt vervangen:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

En waar u parameters wilt gebruiken:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Als een ander voorbeeld kunt u de Service Bus-verzendbewerking bericht voorzien:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl is optioneel en kan worden verwijderd uit de sjabloon, indien aanwezig.
> 


> [!NOTE] 
> Voor Logic App Designer werken wanneer u parameters gebruiken, moet u standaardwaarden, bijvoorbeeld opgeven:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>Uw logische app opslaan

Als u wilt uw logische app op elk gewenst moment opslaan, gaat u naar **bestand** > **opslaan**. (`Ctrl+S`) 

Als uw logische app fouten heeft bij het opslaan van uw app, worden deze weergegeven in de Visual Studio **uitvoer** venster.

## <a name="deploy-your-logic-app-from-visual-studio"></a>Uw logische app vanuit Visual Studio implementeren

Na het configureren van uw app kunt u rechtstreeks vanuit Visual Studio in een paar stappen implementeren. 

1. Met de rechtermuisknop op uw project in Solution Explorer en Ga naar **implementeren** > **nieuwe implementatie...**

    ![Nieuwe implementatie](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. Wanneer u wordt gevraagd, moet u zich aanmelden bij uw Azure-abonnement. 

3. Nu moet u de details voor de resourcegroep waar u uw logische app implementeren. Wanneer u bent klaar, selecteert u **implementeren**.

    > [!NOTE]
    > Zorg ervoor dat u het juiste bestand voor de sjabloon en parameters voor de resourcegroep selecteert. Als u implementeren in een productieomgeving wilt, bijvoorbeeld het parameterbestand productie kiezen.

    ![Implementeren in resourcegroep](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    Status van de implementatie wordt weergegeven in de **uitvoer** venster. 
    U moet selecteren **Azure inrichting** in de **tonen de uitvoer van** lijst.

    ![Implementatie statusuitvoer](./media/logic-apps-deploy-from-vs/output.png)

U kunt in de toekomst bewerken van uw logische app in broncodebeheer en Visual Studio gebruiken voor het implementeren van nieuwe versies.

> [!NOTE]
> Als u de definitie van de Azure-portal rechtstreeks wijzigt, worden deze wijzigingen overschreven wanneer u vanuit Visual Studio opnieuw implementeert. 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Uw logische app toevoegen aan een bestaande resourcegroep-project

Als u een bestaand project voor de resourcegroep hebt, kunt u uw logische app toevoegen aan het project in het venster JSON-overzicht. U kunt ook een andere logische app naast de app die u eerder hebt gemaakt toevoegen.

1. Open het `<template>.json`-bestand.

2. De JSON-overzicht om venster te openen, gaat u naar **weergave** > **overige vensters** > **JSON-overzicht**.

3. Als u wilt een resource toevoegen aan het sjabloonbestand, klikt u op **Resource toevoegen** aan de bovenkant van het venster JSON-overzicht. Of in het venster JSON-overzicht met de rechtermuisknop op **resources**, en selecteer **nieuwe Resource toevoegen**.

    ![Venster JSON-overzicht](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. In de **Resource toevoegen** in het dialoogvenster, zoeken en selecteert u **logische App**. Naam van uw logische app en kies **toevoegen**.

    ![Resource toevoegen](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Volgende stappen

* [Logic apps beheren met Visual Studio Cloud Explorer](logic-apps-manage-from-vs.md)
* [Algemene voorbeelden en scenario's weergeven](logic-apps-examples-and-scenarios.md)
* [Meer informatie over het automatiseren van bedrijfsprocessen met Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Meer informatie over het integreren van uw systemen met Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
