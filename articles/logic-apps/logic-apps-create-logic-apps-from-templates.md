---
title: Werkstromen maken met sjablonen - Azure Logic Apps | Microsoft Docs
description: Werkstromen sneller bouwen met behulp van sjablonen voor logische apps in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.topic: article
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.date: 10/15/2017
ms.openlocfilehash: 134a8f9625b45a8196ebd47f10286093f6ba0d46
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57193446"
---
# <a name="create-logic-app-workflows-from-prebuilt-templates"></a>Het maken van de logic app workflows met vooraf gedefinieerde sjablonen

Logic Apps biedt om u snel werkstromen maken, sjablonen, die zijn vooraf gemaakte logische apps die veelgebruikte patronen volgen. Deze sjablonen gebruiken, zoals bepaald of bewerk ze zodat ze aansluiten bij uw scenario.

Hier volgen enkele Sjablooncategorieën:

| Het sjabloontype | Description | 
| ------------- | ----------- | 
| Sjablonen van Enterprise cloud | Voor het integreren van Azure Blob-, Dynamics CRM, Salesforce, Box, en andere connectors bevat voor de cloud voor ondernemingen. Bijvoorbeeld, kunt u deze sjablonen kunt organiseren zakelijke potentiële klanten of back-up van uw zakelijke bestandsgegevens. | 
| Hun eigen sjablonen | De persoonlijke productiviteit te verbeteren door in te stellen van dagelijkse herinneringen ontvangen, schakelen belangrijk werkitems in takenlijsten en automatisering van langdurige taken op een enkele gebruiker goedkeuringsstap. | 
| Sjablonen voor consumenten-cloud | Voor integratie van sociale mediaservices, zoals Twitter, Slack, en e-mailbericht. Dit is handig voor versterking van sociale media initiatieven marketing. Deze sjablonen ook bevatten taken, zoals kopiëren, cloud die de productiviteit toeneemt door de tijd te besparen op traditioneel terugkerende taken. | 
| Enterprise integration pack-sjablonen | Voor het configureren van VETER (valideren, extraheren, transformeren, verrijken, route) pijplijnen, ontvangen een X12 EDI-document via AS2 en transformeren naar XML en X12, EDIFACT, verwerking en AS2-berichten. | 
| Protocol patroon sjablonen | Voor het implementeren van protocol patronen zoals request response via HTTP en integraties in FTP- en SFTP. Deze sjablonen gebruiken zoals u verstrekt, of bouw op deze voor complexe protocol patronen. | 
||| 

Als u nog geen abonnement op Azure hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/) voordat u begint. Zie voor meer informatie over het bouwen van een logische app [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-logic-apps-from-templates"></a>Logische apps maken met sjablonen

1. Als u niet hebt gedaan, meld u aan de [Azure-portal](https://portal.azure.com "Azure-portal").

2. Kies in het hoofdmenu van Azure **Een resource maken** > **Bedrijfsintegratie** > **Logische app**.

   ![Azure Portal, Nieuw, Enterprise Integration, Logische app](./media/logic-apps-create-logic-apps-from-templates/azure-portal-create-logic-app.png)

3. Maak de logische app aan de hand van de instellingen die in de tabel onder deze afbeelding staan opgegeven:

   ![Gegevens van logische app opgeven](./media/logic-apps-create-logic-apps-from-templates/logic-app-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Naam** | *naam-van-uw-logische-app* | Geef een unieke naam voor de logische app op. | 
   | **Abonnement** | *naam-van-uw-Azure-abonnement* | Selecteer het Azure-abonnement dat u wilt gebruiken. | 
   | **Resourcegroep** | *naam-van-uw-Azure-resourcegroep* | Maak of Selecteer een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) voor deze logische app en alle resources die zijn gekoppeld aan deze app te organiseren. | 
   | **Locatie** | *uw-Azure-datacenter-regio* | Selecteer de regio van het datacenter voor het implementeren van uw logische app, bijvoorbeeld US - west. | 
   | **Log Analytics** | **Uit** (standaard) of **op** | Schakel [registratie in diagnoselogboek](../logic-apps/logic-apps-monitor-your-logic-apps.md#turn-on-diagnostics-logging-for-your-logic-app) voor uw logische app via [logboeken van Azure Monitor](../log-analytics/log-analytics-overview.md). Vereist dat u al een Log Analytics-werkruimte. | 
   |||| 

4. Wanneer u klaar bent, selecteert u **Vastmaken aan dashboard**. Op die manier wordt uw logische app automatisch weergegeven op uw Azure-dashboard en wordt geopend na de implementatie. Kies **Maken**.

   > [!NOTE]
   > Als u de logische app niet wilt vastmaken, moet u deze handmatig zoeken en openen na de implementatie, zodat u kunt doorgaan.

   Nadat Azure uw logische app heeft geïmplementeerd, wordt de ontwerper voor logische apps geopend en ziet u een pagina met een inleidende video. 
   Onder de video ziet u sjablonen voor veelvoorkomende patronen voor logische apps. 

5. Schuif voorbij de inleiding video en veelvoorkomende triggers op **sjablonen**. Kies een vooraf gedefinieerde sjabloon. Bijvoorbeeld:

   ![Een sjabloon voor logische app kiezen](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   > [!TIP]
   > Kies voor het maken van uw logische app helemaal, **lege logische App**.

   Wanneer u een vooraf gedefinieerde sjabloon selecteren, vindt u meer informatie over deze sjabloon. 
   Bijvoorbeeld:

   ![Kies een vooraf gemaakte sjabloon](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

6. Als u wilt doorgaan met de geselecteerde sjabloon, kies **Gebruik deze sjabloon**. 

7. Op basis van de connectors in de sjabloon, wordt u gevraagd om uit te voeren op een van deze stappen:

   * Meld u aan met uw referenties aan systemen of -services waarnaar wordt verwezen door de sjabloon.

   * Verbindingen maken voor de services en systemen waarnaar wordt verwezen door de sjabloon. Geef een naam op voor de verbinding een verbinding wilt maken, en indien nodig, selecteert u de resource die u wilt gebruiken. 

   * Als u deze verbindingen al ingesteld, kiest u **doorgaan**.

   Bijvoorbeeld:

   ![Verbindingen maken](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Wanneer u klaar bent, wordt uw logische app wordt geopend en wordt weergegeven in de ontwerper van logische Apps.

   > [!TIP]
   > Als u wilt terugkeren naar de logboeken van de sjabloon, kies **sjablonen** op de werkbalk van de ontwerper. Met deze actie verwijdert niet-opgeslagen wijzigingen, zodat er wordt een waarschuwing weergegeven om te bevestigen van uw aanvraag.

8. Doorgaan met het ontwikkelen van uw logische app.

   > [!NOTE] 
   > Veel-sjablonen bevatten connectors die mogelijk al zijn vooraf ingevuld vereiste eigenschappen. Sommige sjablonen kunnen echter nog steeds vereist dat u waarden opgeven voordat u de logische app correct kunt implementeren. Als u probeert te implementeren zonder het voltooien van de ontbrekende eigenschapsvelden, krijgt u een foutbericht weergegeven. 

## <a name="update-logic-apps-with-templates"></a>Logische apps bijwerken met sjablonen

1. In de [Azure-portal](https://portal.azure.com "Azure-portal"), zoeken en openen van uw logische app in de ontwerper van logische App.

2. Kies op de werkbalk van de ontwerper **sjablonen**. Met deze actie verwijdert niet-opgeslagen wijzigingen, zodat er wordt een waarschuwing weergegeven, zodat u bevestigen kunt dat u wilt doorgaan. Om te bevestigen, kies **OK**. Bijvoorbeeld:

   ![Kies "Sjablonen"](./media/logic-apps-create-logic-apps-from-templates/logic-app-update-existing-with-template.png)

3. Schuif voorbij de inleiding video en veelvoorkomende triggers op **sjablonen**. Kies een vooraf gedefinieerde sjabloon. Bijvoorbeeld:

   ![Een sjabloon voor logische app kiezen](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   Wanneer u een vooraf gedefinieerde sjabloon selecteren, vindt u meer informatie over deze sjabloon. 
   Bijvoorbeeld:

   ![Kies een vooraf gemaakte sjabloon](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

4. Als u wilt doorgaan met de geselecteerde sjabloon, kies **Gebruik deze sjabloon**. 

5. Op basis van de connectors in de sjabloon, wordt u gevraagd om uit te voeren op een van deze stappen:

   * Meld u aan met uw referenties aan systemen of -services waarnaar wordt verwezen door de sjabloon.

   * Verbindingen maken voor de services en systemen waarnaar wordt verwezen door de sjabloon. Geef een naam op voor de verbinding een verbinding wilt maken, en indien nodig, selecteert u de resource die u wilt gebruiken. 

   * Als u deze verbindingen al ingesteld, kiest u **doorgaan**.

   ![Verbindingen maken](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Uw logische app nu wordt geopend en wordt weergegeven in de ontwerper van logische Apps.

8. Doorgaan met het ontwikkelen van uw logische app. 

   > [!TIP]
   > Als u de wijzigingen nog niet hebt opgeslagen, kunt u uw werk negeren en terug naar de vorige logische app. Kies op de werkbalk van de ontwerper **negeren**.

> [!NOTE] 
> Veel-sjablonen bevatten connectors die mogelijk zijn al vooraf is ingevuld vereiste eigenschappen. Sommige sjablonen kunnen echter nog steeds vereist dat u waarden opgeven voordat u de logische app correct kunt implementeren. Als u probeert te implementeren zonder het voltooien van de ontbrekende eigenschapsvelden, krijgt u een foutbericht weergegeven.

## <a name="deploy-logic-apps-built-from-templates"></a>Logische apps die zijn gebouwd op basis van sjablonen implementeren

Nadat u uw wijzigingen aan de sjabloon aanbrengt, kunt u uw wijzigingen opslaan. Deze actie wordt ook automatisch uw logische app publiceert.

Kies **Opslaan** op de werkbalk van de ontwerper.

![Opslaan en publiceren van uw logische app](./media/logic-apps-create-logic-apps-from-templates/logic-app-save.png)  

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bouwen van logische apps via voorbeelden, scenario's, verhalen van klanten en -scenario's.

> [!div class="nextstepaction"]
> [Gebruik logic app-voorbeelden, scenario's en rondleidingen](../logic-apps/logic-apps-examples-and-scenarios.md)