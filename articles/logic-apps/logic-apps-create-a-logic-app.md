---
title: Uw eerste geautomatiseerde werkstroom maken tussen systemen en cloudservices - Azure Logic Apps | Microsoft Docs
description: Bedrijfsprocessen en werkstromen voor systeemintegratie en EAI-scenario's (Enterprise Application Integration) automatiseren door logische apps te maken en uit te voeren
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
ms.date: 09/18/2017
ms.author: LADocs; estfan
ms.openlocfilehash: d62255ba6e3d5bdfbd792a47f3a92d4c88876742
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-logic-app-for-automating-workflows-and-processes-through-the-azure-portal"></a>Uw eerste logische app maken voor het automatiseren van werkstromen en processen via Azure Portal

Zonder code te schrijven kunt u systemen en services integreren door geautomatiseerde werkstromen te bouwen en uit te voeren met [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md). Om te laten zien hoe makkelijk u taken met een werkstroom kunt automatiseren, wordt aan de hand van deze zelfstudie een eenvoudige logische app gemaakt die een RSS-feed op nieuwe inhoud op een website controleert en die een e-mail verzendt voor elk nieuw item in de feed. 

![Overzicht: eerste voorbeeld van logische app](./media/logic-apps-create-a-logic-app/logic-app-overview.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een lege, logische app maken.
> * Een trigger toevoegen om de logische app te starten als er een RSS-feeditem wordt gepubliceerd.
> * Een actie toevoegen om een e-mail te verzenden met details over het RSS-feeditem.
> * De logische app uitvoeren en testen.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Een e-mailaccount van [een e-mailprovider die door Azure Logic Apps wordt ondersteund](../connectors/apis-list.md) voor het verzenden van meldingen. U kunt bijvoorbeeld Office 365 Outlook, Outlook.com, Gmail of een andere ondersteunde provider gebruiken. In deze zelfstudie wordt Office 365 Outlook gebruikt.

  > [!TIP]
  > Als u een persoonlijk [Microsoft-account](https://account.microsoft.com/account) hebt, beschikt u over een Outlook.com-account. En als u een werk- of schoolaccount hebt van Azure, hebt u een Office 365 Outlook-account.

* Een koppeling naar de RSS-feed van een website. In dit voorbeeld wordt de [RSS-feed voor de populairste artikelen van de website CNN.com](http://rss.cnn.com/rss/cnn_topstories.rss) gebruikt: `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="1-create-a-blank-logic-app"></a>1. Een lege, logische app maken 

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal").

2. Kies in het Azure-menu de optie **Nieuw** > **Enterprise Integration** > **Logische app**.

   ![Azure Portal, Nieuw, Enterprise Integration, Logische app](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Maak de logische app aan de hand van de instellingen die in de tabel staan opgegeven.

   ![Gegevens van logische app opgeven](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *naam-van-uw-logische-app* | Geef een unieke naam voor de logische app op. | 
   | **Abonnement** | *uw-Azure-abonnement* | Selecteer het Azure-abonnement dat u wilt gebruiken. | 
   | **Resourcegroep** | *uw-Azure-resourcegroep* | Maak of selecteer een Azure-resourcegroep, zodat u gerelateerde Azure-resources makkelijker kunt organiseren en beheren. | 
   | **Locatie** | *uw-Azure-regio* | Selecteer de regio van het datacenter voor het implementeren van uw logische app. | 
   |||| 

4. Wanneer u klaar bent, selecteert u **Vastmaken aan dashboard** en kiest u vervolgens **Maken**.

   U hebt nu een Azure-resource voor uw logische app gemaakt. 
   Nadat de logische app is geïmplementeerd, toont de ontwerper van logische apps sjablonen voor algemene patronen, zodat u sneller aan de slag kunt.

   > [!NOTE] 
   > Wanneer u **Vastmaken aan dashboard** selecteert, wordt de logische app na implementatie weergegeven in het Azure-dashboard. De app wordt automatisch in de ontwerper van logische apps geopend. Gebeurt dat niet, dan kunt u de logische app handmatig zoeken en openen.

5. Kies voor dit voorbeeld onder **Sjablonen** de optie **Lege logische app**, zodat u de logische app helemaal zelf kunt ontwerpen.

   ![Sjabloon voor logische app kiezen](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   In de ontwerper van logische apps worden beschikbare [*connectors*](../connectors/apis-list.md) en de bijbehorende [*triggers*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) getoond. U gebruikt deze voor het starten van de werkstroom voor uw logische app.

   ![Triggers voor logische app](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="2-add-a-trigger-for-starting-the-workflow"></a>2. Een trigger toevoegen voor het starten van de werkstroom

Elke logische app moet beginnen met een [*trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). De trigger wordt geactiveerd als een bepaalde gebeurtenis plaatsvindt of als nieuwe gegevens voldoen aan de voorwaarde die u hebt ingesteld. De Logic Apps-engine maakt vervolgens een exemplaar van de logische app voor het uitvoeren van de werkstroom. Telkens wanneer de trigger wordt geactiveerd, wordt een ander, afzonderlijk exemplaar gemaakt waarin de werkstroom van de logische app wordt uitgevoerd.

1. Typ 'rss' als filter in het zoekvak. Selecteer deze trigger: **RSS - Wanneer een feeditem wordt gepubliceerd** 

   ![Selecteer de trigger: 'RSS - Wanneer een feeditem wordt gepubliceerd'](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Geef de koppeling op voor de RSS-feed van de website die u wilt bijhouden, bijvoorbeeld `http://rss.cnn.com/rss/cnn_topstories.rss`. Stel het interval en de frequentie voor het terugkeerpatroon in. In dit voorbeeld stelt u deze eigenschappen zodanig in dat de feed elke dag wordt gecontroleerd. 

   ![Trigger instellen met RSS-feed, frequentie en interval](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

3. Sla uw werk nu op. Kies **Opslaan** op de werkbalk van de ontwerper.
Als u de gegevens van de trigger wilt samenvouwen en verbergen, kiest u de titelbalk van de trigger.

   ![Uw logische app opslaan](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   De logische app is nu actief, maar controleert alleen nog maar op nieuwe items in de RSS-feed, tenzij u acties aan de werkstroom toevoegt. 

## <a name="3-add-an-action-that-responds-to-the-trigger"></a>3. Een actie toevoegen die op de trigger reageert

Voeg nu een [*actie*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) toe. Dit is een taak die door de werkstroom van de logische app wordt uitgevoerd. In dit voorbeeld wordt een actie toegevoegd waardoor een e-mail wordt verzonden zodra een nieuw item in de RSS-feed wordt weergegeven.

1. Kies in de ontwerper van logische apps, onder de trigger, **+ nieuw stap** > **Een actie toevoegen**.

   ![Een actie toevoegen](./media/logic-apps-create-a-logic-app/add-new-action.png)

   In Logic App Designer ziet u de [beschikbare connectors](../connectors/apis-list.md), zodat u een actie kunt kiezen die moet worden uitgevoerd wanneer de trigger wordt geactiveerd.

   ![Selecteren in actielijst](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Voer 'e-mail verzenden' als filter in het zoekvak in. Zoek en selecteer de bijbehorende connector op basis van uw e-mailprovider. Selecteer vervolgens de actie 'e-mail verzenden' voor de connector. Bijvoorbeeld: 

   * Selecteer voor een werk- of schoolaccount van Azure de Office 365 Outlook-connector. 
   * Selecteer voor een persoonlijk Microsoft-account de Outlook.com-connector. 
   * Selecteer voor een Gmail-account de Gmail-connector. 

   Hier wordt doorgegaan met de Office 365 Outlook-connector. 
   Als u een andere provider gebruikt, blijven de stappen gelijk, maar uw gebruikersinterface kan er anders uitzien. 

   ![Selecteer deze actie: 'Office 365 Outlook - Een e-mail verzenden'](./media/logic-apps-create-a-logic-app/actions.png)

3. Wanneer u wordt gevraagd om referenties, meldt u zich aan met de gebruikersnaam en het wachtwoord voor uw e-mailaccount. 

4. Geef de gegevens uit de tabel op en kies de velden die u in de e-mail wilt opnemen.

   | Handeling | Stappen | 
   | -- | ----- | 
   | Velden selecteren die voor de werkstroom beschikbaar zijn. | Klik in een invoervak, zodat de lijst **Dynamische inhoud** wordt geopend, of kies **Dynamische inhoud toevoegen**. | 
   | Andere beschikbare velden weergeven. | Kies in de lijst **Dynamische inhoud** de optie **Meer weergeven** voor elke sectie.  | 
   | Lege regels aan een invoervak toevoegen. | Druk op Shift+Enter. | 
   | De lijst **Dynamische inhoud** sluiten. | Kies opnieuw **Dynamische inhoud toevoegen**. | 
   ||| 

   ![Selecteer de gegevens die u in het e-mailbericht wilt opnemen](./media/logic-apps-create-a-logic-app/rss-action-setup.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Aan** | *ontvanger-e-mailadres* | Voer het e-mailadres van de ontvanger in. Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken. | 
   | **Onderwerp** | Nieuw bericht van CNN: **Feedtitel** | Voer de inhoud van het onderwerp van de e-mail in. <p>In deze zelfstudie voert u de voorgesteld tekst in en selecteert u het veld **Feedtitel** van de trigger, waarin de naam van het feeditem wordt getoond. | 
   | **Hoofdtekst** | Titel: **Feedtitel** <p>Publicatiedatum: **Primaire koppeling feed** <p>Koppeling: **Primaire feedkoppeling** | Voer de inhoud voor de hoofdtekst van de e-mail in. <p>In deze zelfstudie voert u de voorgesteld tekst in en selecteert u deze triggervelden: <p>- **Feedtitel**, waarin opnieuw de naam van het feeditem wordt getoond </br>- **Feed gepubliceerd op**, waarin de publicatiedatum en -tijd van het item wordt getoond </br>- **Primaire feedkoppeling**, waarin de URL naar het feeditem wordt getoond | 
   |||| 

   > [!NOTE] 
   > Als u een veld selecteert waarin een matrix is opgeslagen, wordt in de ontwerper automatisch een lus Voor elke toegevoegd rond de actie die naar de matrix verwijst. Op die manier wordt de actie uitgevoerd voor elk matrixitem.

5. Sla de wijzigingen op als u klaar bent. Kies **Opslaan** op de werkbalk van de ontwerper.

   ![Logische app die klaar is](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

   Ga door naar de volgende sectie als u de logische app nu wilt testen.

## <a name="4-run-and-test-your-workflow"></a>4. De werkstroom uitvoeren en testen

1. Kies **Uitvoeren** op de werkbalk in de ontwerper als u de logische app handmatig wilt uitvoeren om deze te testen. U kunt ook wachten totdat de logische app de opgegeven RSS-feed controleert volgens het schema dat u hebt ingesteld.

   ![Logische app uitvoeren](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

   Als de logische app nieuwe items vindt, wordt er door de app een e-mail verstuurd met de gegevens die u eerder hebt geselecteerd, bijvoorbeeld:

   ![E-mail wordt verzonden voor nieuw RSS-feeditem](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

   Als de logische app geen nieuwe items vindt, wordt de actie overgeslagen waarmee een e-mail wordt verzonden en gewacht op het volgende interval voordat er opnieuw een controle plaatsvindt. 

2. Als u de uitvoerings- en triggergeschiedenis van de logische app wilt bekijken, kiest u **Overzicht** in het menu van de logische app.
Als u meer details over een run wilt bekijken, kiest u de rij voor die run.

   ![Uitvoerings- en triggergeschiedenis van de logische app controleren en bekijken](./media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Als de verwachte gegevens niet worden weergegeven, klikt u op de werkbalk op **Vernieuwen**.

   In de weergave met de details van de run worden zowel de stappen weergegeven die zijn geslaagd als de stappen die zijn mislukt, ook als de run is mislukt. 

   ![Details bekijken van een run van een logische app](./media/logic-apps-create-a-logic-app/logic-app-run-details.png)

   Als u meer wilt weten over de status van uw logische app, over de uitvoerings- of triggergeschiedenis, of als u de logische app wilt diagnosticeren, raadpleegt u [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md) (Problemen met logische app oplossen).

3. Als u de in- en uitvoer van elke stap wilt bekijken, vouwt u de stap uit die u wilt bekijken. Aan de hand van deze gegevens kunt u problemen met de logische app vaststellen en oplossen. Bijvoorbeeld:

   ![Details van de stappen bekijken](./media/logic-apps-create-a-logic-app/logic-app-run-details-expanded.png)

   Zie [De logische app controleren](../logic-apps/logic-apps-monitor-your-logic-apps.md) voor meer informatie.

U hebt nu uw eerste logische app gemaakt en uitgevoerd. Dit voorbeeld laat zien hoe gemakkelijk u werkstromen kunt maken waarmee processen worden geautomatiseerd voor het integreren van systemen en services, en dat allemaal zonder code.

> [!NOTE]
> De logische app wordt uitgevoerd totdat u de app uitschakelt. Als u de app tijdelijk wilt uitschakelen, gaat u door naar de volgende sectie.

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie worden resources gebruikt en acties uitgevoerd die kosten voor uw Azure-abonnement met zich mee kunnen brengen. Als u klaar bent met de zelfstudie en het testen, denkt u er dan aan dat u resources waarvoor u geen kosten wilt maken, uitschakelt of verwijdert.

U kunt het uitvoeren van de logische app en het verzenden van e-mail beëindigen zonder de app te verwijderen. Kies **Overzicht** in het menu van de logische app. Kies **Uitschakelen** op de werkbalk.

![De logische app uitschakelen](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>Veelgestelde vragen

**V:** Wat kan ik verder doen met mijn logische app? </br>
**A:** U kunt andere taken uitvoeren, bijvoorbeeld de JSON-definitie bewerken en weergeven, het activiteitenlogboek controleren of de logische app verwijderen.

Als u andere beheertaken van de logische app zoekt, bekijkt u deze opdrachten in het menu van de logische app:

| Taak | Stappen | 
| ---- | ----- | 
| De status, uitvoerings- en triggergeschiedenis en algemene informatie van uw app bekijken | Kies **Overzicht**. | 
| De app bewerken | Kies **Logic App Designer**. | 
| De JSON-definitie van de werkstroom van uw app bekijken | Kies **Codeweergave van logische app**. | 
| Bewerkingen bekijken die zijn uitgevoerd in uw logische app | Kies **Activiteitenlogboek**. | 
| Eerdere versies voor uw logische app weergeven | Kies **Versies**. | 
| Uw app tijdelijk uitschakelen | Kies **Overzicht** en kies vervolgens **Uitschakelen** op de werkbalk. | 
| De app verwijderen | Kies **Overzicht** en kies vervolgens **Verwijderen** op de werkbalk. Voer de naam van de logische app in en kies **Verwijderen**. | 
||| 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt over Azure Logic Apps.

* Wilt u Azure Logic Apps en connectors helpen verbeteren? Stem mee of stuur uw ideeën in op de [Azure Logic Apps User Voice site](http://aka.ms/logicapps-wish) (Site met meningen van gebruikers over Azure Logic Apps).

## <a name="next-steps"></a>Volgende stappen

* [Een logische app maken in Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Voorwaarden toevoegen en werkstromen uitvoeren](../logic-apps/logic-apps-use-logic-app-features.md)
*   [Sjablonen voor logische app](../logic-apps/logic-apps-use-logic-app-templates.md)
* [Logische apps maken van Azure Resource Manager-sjablonen](../logic-apps/logic-apps-arm-provision.md)
* [Prijsmodel voor logische apps](../logic-apps/logic-apps-pricing.md) 
* [Prijzen voor Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps)
