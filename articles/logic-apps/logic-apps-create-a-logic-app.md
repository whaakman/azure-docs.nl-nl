---
title: Geautomatiseerde werkstromen maken tussen systemen en cloudservices - Azure Logic Apps | Microsoft Docs
description: Logische apps maken om werkstromen voor systeemintegratie en EAI-scenario's (Enterprise Application Integration) te automatiseren
author: ecfan
manager: anneta
editor: 
services: logic-apps
keywords: werkstroom, cloud-apps, cloudservices, bedrijfsprocessen, systeemintegratie, enterprise application integration, EAI
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/20/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 5906605192f9b03f612e6ca3a445434a23713d7f
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="automate-your-first-workflow-to-process-data-with-a-logic-app"></a>Uw eerste werkstroom automatiseren om gegevens te verwerken met een logische app

Om systemen en services sneller te integreren voor uw organisatie, kunt u werkstromen en bedrijfsprocessen automatiseren met [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md). In deze snelstartgids wordt uitgelegd hoe u eenvoudig geautomatiseerde werkstromen kunt bouwen en uitvoeren door een logische app te maken. De voorbeeld-app laat zien hoe u een werkstroom automatiseert die de RSS-feed van een website op nieuwe items controleert en een e-mailbericht verzendt voor elk item.

In dit voorbeeld verzendt de logische app een e-mailbericht zoals in dit voorbeeld:

![E-mail wordt verzonden voor nieuw RSS-feeditem](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Dit is de werkstroom van de logische app op hoog niveau die u maakt:

![Overzicht: voorbeeld van logische app](./media/logic-apps-create-a-logic-app/logic-app-simple-overview.png)

In deze snelstartgids leert u de volgende zaken:

> [!div class="checklist"]
> * Een lege, logische app maken.
> * Een trigger toevoegen voor het starten van de werkstroom wanneer een nieuw item verschijnt in de RSS-feed.
> * Een actie toevoegen om een e-mail te verzenden met details over het RSS-feeditem.
> * De werkstroom van uw logische app uitvoeren.

Als u nog geen abonnement op Azure hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een e-mailaccount van een e-mailprovider die door Azure Logic Apps wordt ondersteund voor het verzenden van meldingen. U kunt bijvoorbeeld Office 365 Outlook, Outlook.com of Gmail gebruiken. Voor andere ondersteunde e-mailconnectors [bekijkt u de lijst met connectors](https://docs.microsoft.com/connectors/). In deze snelstartgids wordt Outlook van Office 365 gebruikt.

  > [!TIP]
  > Als u een persoonlijk [Microsoft-account](https://account.microsoft.com/account) hebt, beschikt u over een Outlook.com-account. En als u een werk- of schoolaccount hebt van Azure, hebt u een Office 365 Outlook-account.

* Een koppeling naar de RSS-feed van een website. In dit voorbeeld wordt de [RSS-feed voor de populairste artikelen van de website van Reuters](http://feeds.reuters.com/reuters/topNews) gebruikt: `http://feeds.reuters.com/reuters/topNews`

Het is in deze snelstartgids niet vereist om code te schrijven, maar Logic Apps biedt ondersteuning voor andere scenario's die gebruikmaken van code, bijvoorbeeld het uitvoeren van uw eigen code vanuit een logische app met [Azure Functions](../azure-functions/functions-overview.md).

## <a name="create-a-blank-logic-app"></a>Een lege, logische app maken 

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). 

2. Kies in het Azure-menu de optie **Nieuw** > **Enterprise Integration** > **Logische app**.

   ![Azure Portal, Nieuw, Enterprise Integration, Logische app](./media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Maak de logische app aan de hand van de instellingen die in de tabel onder deze afbeelding staan opgegeven:

   ![Gegevens van logische app opgeven](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *naam-van-uw-logische-app* | Geef een unieke naam voor de logische app op. | 
   | **Abonnement** | *naam-van-uw-Azure-abonnement* | Selecteer het Azure-abonnement dat u wilt gebruiken. | 
   | **Resourcegroep** | *naam-van-uw-Azure-resourcegroep* | Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) voor deze logische app en om alle resources die zijn gekoppeld aan deze app te organiseren. | 
   | **Locatie** | *uw-Azure-datacenter-regio* | Selecteer de regio van het datacenter voor het implementeren van uw logische app, bijvoorbeeld VS - west. | 
   | **Log Analytics** | Uit | Schakel registratie in diagnoselogboek in voor uw logische app. Voor deze snelstartgids gebruiken we echter de instelling **Uit**. | 
   |||| 

4. Wanneer u klaar bent, selecteert u **Vastmaken aan dashboard**. Op die manier wordt uw logische app automatisch weergegeven op uw Azure-dashboard en wordt geopend na de implementatie. Kies **Maken**.

   > [!NOTE]
   > Als u de logische app niet wilt vastmaken, moet u deze handmatig zoeken en openen na de implementatie, zodat u kunt doorgaan.

   Nadat Azure uw logische app heeft geïmplementeerd, wordt de ontwerper voor logische apps geopend en ziet u een pagina met een inleidende video. 
   Onder de video ziet u sjablonen voor veelvoorkomende patronen voor logische apps. 
   In deze snelstartgids bouwt u uw logische app van voren af aan. 

5. Schuif voorbij de inleidende video en veelvoorkomende triggers. Kies onder **Sjablonen** de optie **Lege logische app**.

   ![Sjabloon voor lege logische app kiezen](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   In de ontwerper voor Logic Apps worden beschikbare connectors en de bijbehorende triggers getoond. U gebruikt deze voor het starten van de werkstromen voor uw logische app.

   ![Triggers voor logische app](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="add-a-trigger-to-detect-new-items"></a>Een trigger voor het detecteren van nieuwe items toevoegen

Elke werkstroom van een logische app begint met een [trigger](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). De trigger wordt geactiveerd als een bepaalde gebeurtenis plaatsvindt of als nieuwe gegevens voldoen aan de voorwaarde die u hebt ingesteld. Telkens wanneer de trigger wordt geactiveerd, maakt de Logic Apps-engine een exemplaar van een logische app dat wordt gestart en de werkstroom uitvoert.

1. Typ 'rss' als filter in het zoekvak. Selecteer deze trigger: **RSS - Wanneer een feeditem wordt gepubliceerd** 

   ![Selecteer de trigger: 'RSS - Wanneer een feeditem wordt gepubliceerd'](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Geef de koppeling op voor de RSS-feed die u wilt bijhouden, bijvoorbeeld `http://feeds.reuters.com/reuters/topNews`. Stel het interval en de frequentie voor het terugkeerpatroon in. In dit voorbeeld controleert de feed om de vijf minuten.

   ![Trigger instellen met RSS-feed, frequentie en interval](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

   Logic Apps maakt een verbinding met de RSS-feed.

   > [!TIP]
   > Als u de weergave in de ontwerpfunctie wilt vereenvoudigen, kunt u gegevens in een vorm samenvouwen en verbergen. Hiervoor klikt u binnen de titelbalk van de vorm.

3. Sla uw werk op. Kies **Opslaan** op de werkbalk van de ontwerper. 

   ![Uw logische app opslaan](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Uw logische app is nu live, maar kan alleen de RSS-feed controleren. Dus gaan we een actie toevoegen die reageert wanneer de trigger wordt geactiveerd.

## <a name="add-an-action-to-send-email"></a>Een actie voor het verzenden van e-mails toevoegen

Nu u een trigger hebt, gaat u een [actie](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) toevoegen waardoor een e-mail wordt verzonden zodra een nieuw item in de RSS-feed wordt weergegeven. De werkstroom voert deze actie uit nadat de trigger wordt geactiveerd.

1. Kies in de ontwerper van logische apps, onder de trigger, **+ nieuw stap** > **Een actie toevoegen**.

   ![Een actie toevoegen](./media/logic-apps-create-a-logic-app/add-new-action.png)

   De ontwerpfunctie geeft de acties weer die uw logische app kan uitvoeren als de trigger wordt geactiveerd.

   ![Selecteren in de actielijst](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Voer 'e-mail verzenden' als filter in het zoekvak in. Zoek en selecteer de e-mailconnector die u wilt gebruiken. Selecteer vervolgens de actie 'e-mail verzenden' voor de connector. Bijvoorbeeld: 

   * Voor werk- of schoolaccounts van Azure selecteert u Outlook van Office 365. 
   * Selecteer Outlook.com voor persoonlijke Microsoft-accounts. 
   * Selecteer Gmail voor Gmail-accounts. 

   In deze snelstartgids wordt Outlook van Office 365 gebruikt. 
   Als u een andere e-mailprovider gebruikt, blijven de stappen gelijk, maar uw gebruikersinterface kan er anders uitzien. 

   ![Selecteer deze actie: 'Office 365 Outlook - Een e-mail verzenden'](./media/logic-apps-create-a-logic-app/actions.png)

3. Wanneer u wordt gevraagd om referenties, meldt u zich aan met de gebruikersnaam en het wachtwoord voor uw e-mailaccount. 

   Logic Apps maakt een verbinding met uw e-mailaccount.

4. Geef nu de gegevens op die u wilt opnemen in de e-mail. 

   1. Voer het e-mailadres van de ontvanger in het vak **Aan** in. 
   Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken.

   2. Voer het onderwerp van de e-mail in het vak **Onderwerp** in. 
   Voor dit voorbeeld voert u 'Nieuw RSS-item' in zoals wordt weergegeven:

      ![Het onderwerp van de e-mail invoeren](./media/logic-apps-create-a-logic-app/logic-app-add-subject.png)

      Wanneer u in het bewerkvak klikt, wordt **Lijst met dynamische inhoud toevoegen** geopend. Hiermee kunt u beschikbare gegevensvelden selecteren en deze opnemen in uw actie. 
      Als de lijst met dynamische inhoud niet wordt geopend, klikt u onder het respectieve bewerkvak op **Dynamische inhoud toevoegen**.

   3. Selecteer uit de lijst **Dynamische inhoud toevoegen** de optie **Feedtitel**. Hiermee wordt de titel van het item in het e-mailbericht toegevoegd.

      ![Het onderwerp van de e-mail invoeren](./media/logic-apps-create-a-logic-app/logic-app-select-field.png)

      Wanneer u klaar bent, ziet het onderwerp van de e-mail eruit zoals in dit voorbeeld:

      ![Feedtitel toegevoegd](./media/logic-apps-create-a-logic-app/added-feed-title.png)

      > [!NOTE] 
      > Als u een veld selecteert waarin een matrix is opgeslagen, zoals **categorie-item**, wordt in de ontwerper automatisch een lus 'Voor elke' toegevoegd rond de actie die naar het veld verwijst. Op die manier kan de actie voor elk matrixitem worden uitgevoerd. 
      > 
      > Als u de lus wilt verwijderen, kiest u de puntjes (**...** ) op de titelbalk van de lus. Kies vervolgens **Verwijderen**.

   4. Voer in het vak **Hoofdtekst** de inhoud voor de hoofdtekst van de e-mail in. 
   Voer voor dit voorbeeld deze tekst in en selecteer deze velden:

      ![Inhoud voor de hoofdtekst van de e-mail toevoegen](./media/logic-apps-create-a-logic-app/logic-app-complete.png)

      | Veld | Beschrijving | 
      | ----- | ----------- | 
      | **Feedtitel** | Geeft de titel van het item weer. | 
      | **Feed gepubliceerd op** | Geeft de publicatiedatum en -tijd weer. | 
      | **Primaire feedkoppeling** | Geeft de URL voor het item weer. | 
      ||| 

      > [!TIP]
      > Als u lege regels wilt toevoegen in een invoervak, drukt u op Shift + Enter. 
      
5. Sla uw werk op. Kies **Opslaan** op de werkbalk van de ontwerper.

   ![Logische app die klaar is](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

## <a name="run-your-logic-app-workflow"></a>De werkstroom van uw logische app uitvoeren

Kies **Uitvoeren** op de werkbalk in de ontwerper als u de logische app handmatig wilt uitvoeren. U kunt anders wachten tot uw logische app wordt uitgevoerd volgens de planning die u hebt ingesteld.

![Logische app uitvoeren](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

Als de RSS-feed nieuwe items heeft, verzendt uw logische app een e-mailbericht voor elk nieuw item. Dit is bijvoorbeeld een voorbeeldbericht in Outlook dat deze logische app verzendt:

![E-mail wordt verzonden voor nieuw RSS-feeditem](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Als de feed geen nieuwe items heeft, slaat uw logische app de actie over waarmee een e-mail wordt verzonden. Er wordt vervolgens gewacht op het volgende interval voordat er opnieuw een controle plaatsvindt. 

> [!TIP]
> Als u geen een e-mailberichten ontvangt, controleert u de map Ongewenste e-mail. Als u niet zeker weet of uw logische app correct wordt uitgevoerd, kunt u [Problemen met uw logische app oplossen](../logic-apps/logic-apps-diagnosing-failures.md) raadplegen.

U hebt nu uw eerste logische app gemaakt en uitgevoerd. In deze snelstartgids hebt u geleerd hoe u snel en eenvoudig geautomatiseerde werkstromen voor het integreren van systemen en services kunt maken.

## <a name="clean-up-resources"></a>Resources opschonen

Uw logische app blijft actief en maakt mogelijk kosten op uw Azure-abonnement totdat u de app uitschakelt of verwijdert. Als u verbindingen voor uw logische app maakt, blijven deze bestaan, zelfs nadat u uw logische app verwijdert. 

Als u klaar bent, denkt u er dan aan dat u resources waarvoor u geen kosten wilt maken of die u niet wilt behouden, uitschakelt of verwijdert. Verwijder de Azure-resourcegroep die u hebt gemaakt voor deze logische app om alle resources die u hebt gemaakt voor deze snelstartgids te verwijderen. 

### <a name="delete-resource-group"></a>Resourcegroep verwijderen

Als u geen zaken die betrekking hebben op uw logische app wilt behouden, verwijdert u de resourcegroep die u hebt gemaakt voor deze snelstartgids en alle bijbehorende resources. Meer informatie over [het beheren van Azure-resourcegroepen](../azure-resource-manager/resource-group-portal.md#manage-resources).

1. Kies **Resourcegroepen** in het menu van Azure.

2. Kies daarna de resourcegroep die u wilt verwijderen. Kies in het menu Resourcegroep de optie **Overzicht**, als deze optie nog niet is geselecteerd. 

3. Controleer alle resources in de groep die u wilt verwijderen. Als u klaar bent, kiest u **Resourcegroep verwijderen** op de werkbalk van de resourcegroep.

### <a name="turn-off-logic-app"></a>De logische app uitschakelen

Als u uw logische app niet meer wilt uitvoeren, maar uw werk niet wilt verwijderen, schakelt u uw app uit. 

Kies **Overzicht** in het menu van de logische app. Kies **Uitschakelen** op de werkbalk.

  ![De logische app uitschakelen](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

### <a name="delete-logic-app"></a>Logische app verwijderen

U kunt uw logische app verwijderen, maar alle andere gerelateerde resources behouden, zoals de verbindingen die u hebt gemaakt.

1. Kies **Overzicht** in het menu van de logische app. Kies **Verwijderen** op de werkbalk. 

   ![Uw logische app verwijderen](./media/logic-apps-create-a-logic-app/delete-logic-app.png)

   > [!TIP]
   > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

2. Bevestig dat u uw logische app wilt verwijderen en kies vervolgens **Verwijderen**.

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Werkstromen voor logische apps maken met vooraf gedefinieerde sjablonen](../logic-apps/logic-apps-create-logic-apps-from-templates.md)