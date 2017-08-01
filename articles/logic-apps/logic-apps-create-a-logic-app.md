---
title: Uw eerste werkstroom maken tussen cloud-apps en cloudservices - Azure Logic Apps | Microsoft Docs
description: Bedrijfsprocessen voor systeemintegratie en EAI-scenario&quot;s (Enterprise Application Integration) automatiseren door werkstromen te maken en uit te voeren in Azure Logic Apps
author: jeffhollan
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
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 204bf123509729b60b55c306050cef54aa7fecc5
ms.contentlocale: nl-nl
ms.lasthandoff: 05/17/2017

---

# <a name="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services"></a>Uw eerste werkstroom voor logische apps maken om processen tussen cloud-apps en cloudservices te automatiseren

Zonder dat u code hoeft te schrijven, kunt u bedrijfsprocessen eenvoudiger en sneller automatiseren door werkstromen te maken en uit te voeren met [Azure Logic Apps](logic-apps-what-are-logic-apps.md). Het eerste voorbeeld laat zien hoe u een eenvoudige werkstroom voor logische apps maakt waarmee een RSS-feed wordt gecontroleerd op nieuwe inhoud op een website. Als er nieuwe items worden weergegeven in de feed van de website, verstuurt de logische app een e-mail vanuit een account van Outlook of Gmail.

U hebt deze items nodig om een logische app te maken en uit te voeren:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

  Uw Azure-abonnement wordt gebruikt voor facturering van het gebruik van de logische app. Lees hier meer informatie over [meten van gebruik](../logic-apps/logic-apps-pricing.md) en [prijzen](https://azure.microsoft.com/pricing/details/logic-apps) voor Azure Logic Apps.

Verder zijn nog deze items nodig voor het voorbeeld:

* Een account van Outlook.com, Office 365 Outlook of Gmail

    > [!TIP]
    > Als u een persoonlijk [Microsoft-account](https://account.microsoft.com/account) hebt, beschikt u over een Outlook.com-account. Als u een werk- of schoolaccount hebt van Azure, hebt u een **Office 365 Outlook**-account.

* Een koppeling naar de RSS-feed van een website. In dit voorbeeld wordt de [RSS-feed voor de populairste artikelen van de website CNN.com](http://rss.cnn.com/rss/cnn_topstories.rss) gebruikt: `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="add-a-trigger-that-starts-your-workflow"></a>Een trigger toevoegen voor het starten van de werkstroom

Een [*trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is een gebeurtenis waardoor de werkstroom voor uw logische app wordt gestart. Het is ook het eerste item dat uw logische app nodig heeft.

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal").

2. Kies in het menu links de optie **Nieuw** > **Enterprise Integration** > **Logische app** (zie hieronder):

     ![Azure Portal, Nieuw, Enterprise Integration, Logische app](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > U kunt ook **Nieuw** kiezen, `logic app` in het zoekvak invoeren en op Enter drukken. Kies vervolgens **Logische app** > **Maken**.

3. Voer een naam in voor de logische app en selecteer uw Azure-abonnement. Maak of selecteer vervolgens een Azure-resourcegroep, zodat u gerelateerde Azure-resources makkelijker kunt organiseren en beheren. Selecteer ten slotte de locatie van het datacenter voor het hosten van uw logische app. Als u hiermee klaar bent, selecteert u **Vastmaken aan dashboard** en klikt u op **Maken**.

     ![Details van logische app](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > Wanneer u **Vastmaken aan dashboard** selecteert, wordt de logische app na implementatie weergegeven in het Azure-dashboard. Bovendien wordt de app dan automatisch geopend. Als uw logische app niet in het dashboard wordt weergegeven, kiest u **Meer weergeven** op de tegel **Alle resources** en selecteert u de logische app. U kunt ook in het menu aan de linkerkant **Meer services** kiezen. Kies vervolgens **Logische apps** onder **Enterprise Integration** en selecteer dan de logische app.

4. Wanneer u een logische app voor het eerst opent, ziet u in Logic App Designer sjablonen die u als uitgangspunt kunt nemen. Kies voor dit voorbeeld **Lege logische app**, zodat u de logische app helemaal zelf kunt ontwerpen.

    Logic App Designer wordt geopend, met daarin de beschikbare services en *triggers* die u kunt gebruiken in uw logische app.

5. Typ `RSS` in het zoekvak en selecteer deze trigger: **RSS - Wanneer een feeditem wordt gepubliceerd**. 

    ![RSS-trigger](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. Voer de koppeling in voor de RSS-feed van de website die u wilt bijhouden. 

     U kunt ook andere waarden opgeven voor **Frequentie** en **Interval**. 
     Deze instellingen bepalen hoe vaak de logische app op nieuwe items controleert. Alle gevonden items tijdens die periode worden geretourneerd.

     In dit voorbeeld gaan we elke dag controleren of er populaire artikelen zijn gepost op de CNN-website.

     ![Trigger instellen met RSS-feed, frequentie en interval](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Sla uw werk nu op. (Kies hiervoor **Opslaan** op de opdrachtbalk.)

   ![Uw logische app opslaan](media/logic-apps-create-a-logic-app/save-logic-app.png)

   Wanneer u de app opslaat, is de app meteen live. Op dit moment controleert de logische app alleen op nieuwe items in de opgegeven RSS-feed. 
   We gaan dit voorbeeld zinvoller maken door een actie toe te voegen die de logische app uitvoert nadat de trigger is geactiveerd.

## <a name="add-an-action-that-responds-to-your-trigger"></a>Een actie toevoegen die op de trigger reageert

Een [*actie*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is een taak die wordt uitgevoerd tijdens de werkstroom voor de logische app. Nadat u een trigger hebt toegevoegd aan uw logische app, kunt u een actie toevoegen om bewerkingen uit te voeren op gegevens die met deze trigger worden gegenereerd. In ons voorbeeld gaan we nu een actie toevoegen waarmee een e-mail wordt verstuurd wanneer er nieuwe items in de RSS-feed van de website staan.

1. Kies in Logic App Designer **Nieuwe stap** > **Een actie toevoegen** onder de trigger (zie hieronder):

   ![Een actie toevoegen](media/logic-apps-create-a-logic-app/add-new-action.png)

   In Logic App Designer ziet u de [beschikbare connectors](../connectors/apis-list.md), zodat u een actie kunt kiezen die moet worden uitgevoerd wanneer de trigger wordt geactiveerd.

2. Volg afhankelijk van uw e-mailaccount de stappen voor Outlook of Gmail.

   * Als u een e-mail wilt verzenden met uw Outlook-account, typt u `outlook` in het zoekvak. Kies onder **Services** **Outlook.com** voor persoonlijke Microsoft-accounts of **Office 365 Outlook** voor werk- of schoolaccount van Azure. 
   Selecteer **Een e-mail verzenden** onder **Acties**.

       ![De actie Een e-mail verzenden voor Outlook](media/logic-apps-create-a-logic-app/actions.png)

   * Als u een e-mail wilt verzenden met uw Gmail-account, typt u `gmail` in het zoekvak. 
   Selecteer **E-mail verzenden** onder **Acties**.

       ![De actie E-mail verzenden voor Gmail](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. Wanneer u wordt gevraagd om referenties, meldt u zich aan met de gebruikersnaam en het wachtwoord voor uw e-mailaccount. 

4. Geef de details op voor deze actie, zoals het e-mailadres waarnaar de e-mail moet worden verstuurd, en kies de parameters voor de gegevens die u wilt opnemen in de e-mail, zoals:

   ![Selecteer de gegevens die u in het e-mailbericht wilt opnemen](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Als u Outlook hebt gekozen, kan uw logische app eruitzien zoals in dit voorbeeld:

    ![Logische app die klaar is](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.    Sla uw wijzigingen op. (Kies hiervoor **Opslaan** op de opdrachtbalk.)

6. U kunt de logische app nu handmatig uitvoeren om te kijken of alles werkt zoals verwacht. Kies hiervoor **Uitvoeren** op de opdrachtbalk. U kunt ook wachten totdat de logische app de opgegeven RSS-feed controleert volgens het schema dat u hebt ingesteld.

   Als de logische app nieuwe items vindt, wordt er door de app een e-mail verstuurd met de gegevens die u eerder hebt geselecteerd. 
   Als er geen nieuwe items worden gevonden, wordt de actie voor het versturen van een e-mail overgeslagen.

7. Als u de uitvoerings- en triggergeschiedenis van de logische app wilt controleren, kiest u **Overzicht** in het menu van de logische app.

   ![Uitvoerings- en triggergeschiedenis van de logische app controleren en bekijken](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Als de verwachte gegevens niet worden weergegeven, klikt u op de opdrachtbalk op **Vernieuwen**.

   Als u meer wilt weten over de status van uw logische app of over de uitvoerings- en triggergeschiedenis, raadpleegt u [Diagnose logic app failures](logic-apps-diagnosing-failures.md) (Problemen met logische app oplossen).

      > [!NOTE]
      > De logische app wordt uitgevoerd totdat u de app uitschakelt. Als u de app voorlopig wilt uitschakelen, kiest u **Overzicht** in het menu van de logische app. Kies op de opdrachtbalk de optie **Uitschakelen**.

U weet nu hoe u een logische app kunt maken en uitvoeren. Daarnaast hebt u geleerd hoe eenvoudig het is om werkstromen te maken voor het automatiseren van processen en hoe u cloud-apps en cloudservices kunt integreren. Allemaal zonder één regel code te hoeven schrijven.

## <a name="manage-your-logic-app"></a>Uw logische app beheren

Voorbeelden van taken om uw app te beheren, zijn het controleren van de status, het bewerken van instellingen, het bekijken van de geschiedenis, en het uitschakelen of zelfs verwijderen van de app.

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal").

2. Kies **Meer services** in het menu aan de linkerkant. Kies onder **Enterprise Integration** de optie **Logic Apps**. Selecteer uw logische app. 

   In het menu van de logische app vindt u deze beheertaken:

   |Taak|Stappen| 
   |:---|:---| 
   | De status, uitvoeringsgeschiedenis en algemene informatie voor uw app bekijken| Kies **Overzicht**.| 
   | De app bewerken | Kies **Logic App Designer**. | 
   | De JSON-definitie van de werkstroom van uw app bekijken | Kies **Codeweergave van logische app**. | 
   | Bewerkingen bekijken die zijn uitgevoerd in uw logische app | Kies **Activiteitenlogboek**. | 
   | Eerdere versies voor uw logische app weergeven | Kies **Versies**. | 
   | Uw app tijdelijk uitschakelen | Kies **Overzicht** en kies vervolgens **Uitschakelen** op de opdrachtbalk. | 
   | De app verwijderen | Kies **Overzicht** en kies vervolgens **Verwijderen** op de opdrachtbalk. Voer de naam van de logische app in en kies **Verwijderen**. | 

## <a name="get-help"></a>Help opvragen

Ga naar het [Forum voor Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Azure Logic Apps en connectors kunt u stemmen op ideeën of ideeën indien op de [site voor gebruikersfeedback van Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

*  [Voorwaarden toevoegen en werkstromen uitvoeren](../logic-apps/logic-apps-use-logic-app-features.md)
*     [Sjablonen voor logische app](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Logische apps maken van Azure Resource Manager-sjablonen](../logic-apps/logic-apps-arm-provision.md)

