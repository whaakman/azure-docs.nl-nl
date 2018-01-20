---
title: Werkstromen maken van sjablonen - Azure Logic Apps | Microsoft Docs
description: Werkstromen sneller met behulp van sjablonen voor logic app bouwen
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49b4bbfda4518b03ef6080bec1e2a493933af4f5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-logic-app-workflows-from-prebuilt-templates"></a>Logic app werkstromen van vooraf gedefinieerde sjablonen maken

Logic Apps kunt u aan de slag sneller maken van werkstromen, sjablonen, die zijn vooraf gedefinieerde logic apps die veelgebruikte patronen volgen. Deze sjablonen te gebruiken, zoals bepaald of bewerk deze aanpassen aan uw scenario.

Hier volgen enkele Sjablooncategorieën:

| Het sjabloontype | Beschrijving | 
| ------------- | ----------- | 
| Enterprise cloud-sjablonen | Voor de integratie van Azure Blob, Dynamics CRM, Salesforce, vak en andere connectors bevat voor de behoeften van uw enterprise cloud. Bijvoorbeeld, kunt u deze sjablonen ordenen zakelijke potentiële klanten of back-up van uw zakelijke bestandsgegevens. | 
| Hun eigen sjablonen | Persoonlijke productiviteit verbeteren door het instellen van dagelijkse herinneringen, schakelen belangrijk werkitems in takenlijsten en automatiseren langdurige taken naar beneden op een enkele gebruiker goedkeuringsstap. | 
| Consumer cloud sjablonen | Voor services voor sociale media zoals Twitter, toegestane integreren en e-mailberichten. Dit is handig voor het versterking van sociale media initiatieven marketing. Deze sjablonen ook bevatten taken, zoals het kopiëren, cloud die verhoogt de productiviteit door de tijd te besparen op oudsher terugkerende taken. | 
| Enterprise integration pack-sjablonen | Voor het configureren van VETER (valideren, uitpakken, transformeren, aanvullen, routeren) pijplijnen, ontvangen een X12 EDI-document via AS2 en transformeren naar XML en X12, EDIFACT, verwerken en AS2-berichten. | 
| Patroon-protocol-sjablonen | Voor het implementeren van protocol patronen, zoals aanvragen en antwoorden over HTTP en integraties in FTP- en SFTP. Deze sjablonen te gebruiken, zoals bepaald of op deze bouwen voor complexe protocol patronen. | 
||| 

Als u nog geen abonnement op Azure hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/) voordat u begint. Zie voor meer informatie over het bouwen van een logische app [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-logic-apps-from-templates"></a>Logische apps van sjablonen maken

1. Als u nog niet gedaan hebt, meld u bij de [Azure-portal](https://portal.azure.com "Azure-portal").

2. Kies in het Azure-menu de optie **Nieuw** > **Enterprise Integration** > **Logische app**.

   ![Azure Portal, Nieuw, Enterprise Integration, Logische app](./media/logic-apps-create-logic-apps-from-templates/azure-portal-create-logic-app.png)

3. Maak de logische app aan de hand van de instellingen die in de tabel onder deze afbeelding staan opgegeven:

   ![Gegevens van logische app opgeven](./media/logic-apps-create-logic-apps-from-templates/logic-app-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Naam** | *naam-van-uw-logische-app* | Geef een unieke naam voor de logische app op. | 
   | **Abonnement** | *naam-van-uw-Azure-abonnement* | Selecteer het Azure-abonnement dat u wilt gebruiken. | 
   | **Resourcegroep** | *naam-van-uw-Azure-resourcegroep* | Maak of Selecteer een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) voor deze logische app, en om alle resources die zijn gekoppeld aan deze app te organiseren. | 
   | **Locatie** | *uw-Azure-datacenter-regio* | Selecteer de regio van het datacenter voor het implementeren van uw logische app, bijvoorbeeld VS - west. | 
   | **Log Analytics** | **Uit** (standaard) of **op** | Schakel [Diagnostische logboekregistratie](../logic-apps/logic-apps-monitor-your-logic-apps.md#turn-on-diagnostics-logging-for-your-logic-app) voor uw logische app via [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Vereist dat er al een [Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) werkruimte. | 
   |||| 

4. Wanneer u klaar bent, selecteert u **Vastmaken aan dashboard**. Op die manier wordt uw logische app automatisch weergegeven op uw Azure-dashboard en wordt geopend na de implementatie. Kies **Maken**.

   > [!NOTE]
   > Als u de logische app niet wilt vastmaken, moet u deze handmatig zoeken en openen na de implementatie, zodat u kunt doorgaan.

   Nadat Azure uw logische app heeft geïmplementeerd, wordt de ontwerper voor logische apps geopend en ziet u een pagina met een inleidende video. 
   Onder de video ziet u sjablonen voor veelvoorkomende patronen voor logische apps. 

5. Schuiven voorbij de inleiding video- en triggers **sjablonen**. Kies een vooraf gemaakte sjabloon. Bijvoorbeeld:

   ![Kies een sjabloon voor logische app](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   > [!TIP]
   > Voor het maken van uw logische app maken, kies **lege logische App**.

   Wanneer u een vooraf gemaakte sjabloon selecteert, kunt u meer informatie over deze sjabloon kunt weergeven. 
   Bijvoorbeeld:

   ![Kies een vooraf gemaakte sjabloon](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

6. Om door te gaan met de geselecteerde sjabloon kiezen **deze sjabloon gebruiken**. 

7. Op basis van de connectors in de sjabloon, wordt u gevraagd om uit te voeren op een van deze stappen:

   * Aanmelden met uw referenties naar systemen of -services waarnaar wordt verwezen door de sjabloon.

   * Verbindingen maken voor de services en systemen waarnaar wordt verwezen door de sjabloon. Geef een naam voor de verbinding een verbinding wilt maken, en indien nodig, selecteert u de resource die u wilt gebruiken. 

   * Als u deze verbindingen al ingesteld, kiest u **doorgaan**.

   Bijvoorbeeld:

   ![Verbindingen maken](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Wanneer u bent klaar, wordt uw logische app wordt geopend en wordt weergegeven in de ontwerpfunctie voor Logic Apps.

   > [!TIP]
   > Als u wilt terugkeren naar de viewer sjabloon, kies **sjablonen** op de werkbalk van de ontwerpfunctie. Deze actie verwijdert u niet-opgeslagen wijzigingen, dus er wordt een waarschuwing weergegeven om te bevestigen dat uw aanvraag.

8. Doorgaan met het bouwen van uw logische app.

   > [!NOTE] 
   > Veel sjablonen bevatten connectors die mogelijk al hebt vooraf ingevuld vereiste eigenschappen. Sommige sjablonen kunnen echter nog steeds vereist dat u waarden opgeven voordat u de logische app goed kunt implementeren. Als u probeert te implementeren zonder de ontbrekende eigenschapsvelden voltooien, krijgt u een foutbericht weergegeven. 

## <a name="update-logic-apps-with-templates"></a>Logic apps bijwerken met behulp van sjablonen

1. In de [Azure-portal](https://portal.azure.com "Azure-portal"), zoeken en openen van uw logische app in de Logic App-ontwerper.

2. Kies op de werkbalk designer **sjablonen**. Deze actie verwijdert u niet-opgeslagen wijzigingen, dus er wordt een waarschuwing weergegeven zodat u bevestigen kunt dat u wilt doorgaan. Om te bevestigen, kies **OK**. Bijvoorbeeld:

   ![Kies 'Sjablonen'](./media/logic-apps-create-logic-apps-from-templates/logic-app-update-existing-with-template.png)

3. Schuiven voorbij de inleiding video- en triggers **sjablonen**. Kies een vooraf gemaakte sjabloon. Bijvoorbeeld:

   ![Kies een sjabloon voor logische app](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   Wanneer u een vooraf gemaakte sjabloon selecteert, kunt u meer informatie over deze sjabloon kunt weergeven. 
   Bijvoorbeeld:

   ![Kies een vooraf gemaakte sjabloon](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

4. Om door te gaan met de geselecteerde sjabloon kiezen **deze sjabloon gebruiken**. 

5. Op basis van de connectors in de sjabloon, wordt u gevraagd om uit te voeren op een van deze stappen:

   * Aanmelden met uw referenties naar systemen of -services waarnaar wordt verwezen door de sjabloon.

   * Verbindingen maken voor de services en systemen waarnaar wordt verwezen door de sjabloon. Geef een naam voor de verbinding een verbinding wilt maken, en indien nodig, selecteert u de resource die u wilt gebruiken. 

   * Als u deze verbindingen al ingesteld, kiest u **doorgaan**.

   ![Verbindingen maken](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Uw logische app nu wordt geopend en wordt weergegeven in de ontwerpfunctie voor Logic Apps.

8. Doorgaan met het bouwen van uw logische app. 

   > [!TIP]
   > Als u uw wijzigingen niet hebt opgeslagen, kunt u uw werk te negeren en terug naar uw vorige logische app. Kies op de werkbalk designer **negeren**.

> [!NOTE] 
> Veel sjablonen bevatten connectors die mogelijk zijn al vooraf worden ingevuld vereiste eigenschappen. Sommige sjablonen kunnen echter nog steeds vereist dat u waarden opgeven voordat u de logische app goed kunt implementeren. Als u probeert te implementeren zonder de ontbrekende eigenschapsvelden voltooien, krijgt u een foutbericht weergegeven.

## <a name="deploy-logic-apps-built-from-templates"></a>Logic apps gebouwd op basis van sjablonen implementeren

Nadat u uw wijzigingen in de sjabloon aanbrengt, kunt u de wijzigingen opslaan. Deze actie worden ook automatisch uw logische app publiceert.

Kies **Opslaan** op de werkbalk van de ontwerper.

![Opslaan en uw logische app publiceren](./media/logic-apps-create-logic-apps-from-templates/logic-app-save.png)  

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bouwen van logic apps via voorbeelden, scenario's, ervaringen van klanten en scenario's.

> [!div class="nextstepaction"]
> [Bekijk logic app voorbeelden, scenario's en -scenario 's](../logic-apps/logic-apps-examples-and-scenarios.md)