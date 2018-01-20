---
title: Van goedkeuringswerkstromen voor het verwerken van aanvragen mailinglijst - Azure Logic Apps bouwen | Microsoft Docs
description: Deze zelfstudie laat zien hoe u automatische goedkeuringswerkstromen voor het verwerken van mailinglijst abonnementen met Azure Logic Apps maken
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: 26ef6f69ef2f2d50628f4d0b021159526c9a04a7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-mailing-list-requests-with-a-logic-app"></a>Mailinglijst aanvragen met een logische app beheren

Logische Apps van Azure kunt u werkstromen automatiseren en integratie van gegevens over Azure-services, Microsoft-services, andere software as a service (SaaS)-apps en on-premises systemen. Deze zelfstudie laat zien hoe u kunt samenstellen een [logische app](../logic-apps/logic-apps-overview.md) processen verzoeken om abonnementen voor een verzendlijst beheerd door de [MailChimp](https://mailchimp.com/) service.
Deze logische app bewaakt een e-mailaccount voor deze aanvragen, stuurt deze aanvragen voor goedkeuring en goedgekeurde leden worden toegevoegd aan de adreslijst.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een lege, logische app maken.
> * Toevoegen van een trigger die e-mailberichten voor verzoeken om abonnementen bewaakt.
> * Toevoegen van een actie die wordt verzonden e-mailberichten voor het goedkeuren of weigeren van deze aanvragen.
> * Een voorwaarde waarmee wordt gecontroleerd of het goedkeuringsantwoord toevoegen.
> * Toevoegen van een actie die goedgekeurde leden worden toegevoegd aan de adreslijst.
> * Een voorwaarde waarmee wordt gecontroleerd of deze leden nu deel uit van de lijst toevoegen.
> * Een actie die wordt verzonden e-mailberichten bevestigen of deze leden nu deel uit van de lijst toevoegen.

Wanneer u bent klaar, ziet uw logische app deze werkstroom op hoog niveau:

![Op hoog niveau klaar logische app](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-overview.png)

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">meld u dan aan voor een gratis Azure-account</a> voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een MailChimp-account. Maak een lijst met de naam 'test-leden-ML' waarin uw logische app e-mailadressen voor goedgekeurde leden kunt toevoegen. Als u een account geen [aanmelden voor een gratis account](https://login.mailchimp.com/signup/) en meer informatie over [het maken van een lijst](https://us17.admin.mailchimp.com/lists/#). 

* Een e-mailaccount met Outlook van Office 365 of Outlook.com, die ondersteuning bieden voor werkstromen voor goedkeuring. In dit artikel maakt gebruik van Outlook van Office 365. Als u een ander e-mailaccount gebruikt, wordt de algemene stappen hetzelfde blijven, maar uw gebruikersinterface iets anders uitzien.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Aanmelden bij de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> met de referenties van uw Azure-account.

## <a name="create-your-logic-app"></a>Uw logische app maken

1. Kies in het Azure-menu de optie **Nieuw** > **Enterprise Integration** > **Logische app**.

   ![Logische app maken](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app.png)

2. Onder **maken logische app**, bevatten deze informatie over uw logische app weergegeven en beschreven. Als u klaar bent, kiest u **Vastmaken aan dashboard** > **Maken**.

   ![Geef informatie op logic app](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Naam** | LA-MailingList | De naam voor uw logische app | 
   | **Abonnement** | <*your-Azure-subscription-name*> | De naam voor uw Azure-abonnement | 
   | **Resourcegroep** | LA-MailingList-RG | De naam voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) gebruikt om verwante resources te organiseren | 
   | **Locatie** | VS - oost 2 | De regio waar informatie over uw logische app wordt opgeslagen | 
   | **Log Analytics** | Uit | Houd de **uit** instellen voor het vastleggen van diagnostische gegevens. | 
   |||| 

3. Nadat u uw app wordt geïmplementeerd in Azure, wordt de ontwerpfunctie van Logic Apps wordt geopend en ziet u een pagina met een video inleiding en sjablonen voor algemene logic app patronen. Kies onder **Sjablonen** de optie **Lege logische app**.

   ![Sjabloon voor lege logische app kiezen](./media/tutorial-process-mailing-list-subscriptions-workflow/choose-logic-app-template.png)

Vervolgens voegt u een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) die luistert naar binnenkomende e-mailberichten met verzoeken om abonnementen.
Elke logische app moet beginnen met een trigger, die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde voldoet aan de nieuwe gegevens. Zie voor meer informatie [uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-trigger-to-monitor-emails"></a>Trigger voor het bewaken van e-mailberichten toevoegen

1. Voer op de designer 'als u e-mailbericht ontvangt' in het zoekvak. Selecteer de trigger voor uw e-mailprovider:  **< *uw op e-mailprovider*>: wanneer een nieuw e-mailbericht**
   
   ![Selecteer deze trigger voor e-mailprovider: 'Wanneer u een nieuw e-mailadres binnenkomt'](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-new-email.png)

   * Voor werk- of schoolaccounts van Azure selecteert u Outlook van Office 365.
   * Selecteer Outlook.com voor persoonlijke Microsoft-accounts.

2. Als u wordt gevraagd om referenties, aanmelden bij uw e-mailaccount zodat Logic Apps kunnen geen verbinding met uw e-mailaccount maken.

3. Geef de criteria die de trigger controleert nu in alle nieuwe e-mailbericht.

   1. Geef de map, interval en frequentie voor het controleren van e-mailberichten.

      ![Geef een map, interval en frequentie voor het controleren van e-mails](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-up-email.png)

      | Instelling | Waarde | Beschrijving | 
      | ------- | ----- | ----------- | 
      | **Folder** | Postvak in | De e-map bewaken | 
      | **Interval** | 1 | Het aantal intervallen moet worden gewacht tussen controles | 
      | **Frequentie** | Uur | Controleert de tijdseenheid voor elk interval tussen  | 
      |  |  |  | 

   2. Kies **geavanceerde opties weergeven**. In de **onderwerp Filter** Voer deze tekst voor de trigger te vinden in het onderwerp van e-mail:```subscribe-test-members-ML```

      ![Geavanceerde opties instellen](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-advanced-options.png)

4. Als u wilt verbergen van de trigger gegevens nu, klikt u op de titelbalk van de trigger.

   ![Samenvouwen vorm details verbergen](./media/tutorial-process-mailing-list-subscriptions-workflow/collapse-trigger-shape.png)

5. Sla uw logische app. Kies **Opslaan** op de werkbalk van de ontwerper.

   Uw logische app is nu live maar geen reactie dan Controleer uw binnenkomende e-mailadres. 
   Een actie die reageert wanneer de trigger wordt geactiveerd, dus toevoegen.

## <a name="send-approval-email"></a>Goedkeurings-e-mail verzenden

Nadat u hebt een trigger, voegt u toe een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) die een e-mailbericht wilt goedkeuren of afwijzen van de aanvraag verzendt. 

1. Kies onder de trigger **+ een nieuwe stap** > **een actie toevoegen**. Zoek naar 'goedkeuring' en selecteert u deze actie:  **< *uw op e-mailprovider*>-goedkeuring e-mail verzenden**

   ![Kies ' < uw-e-provider > - goedkeuring per E-mail '](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-send-approval-email.png)

2. Bevatten informatie voor deze actie wordt weergegeven en beschreven: 

   ![Goedkeuring ingesteld e-mailinstellingen](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-approval-email-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Aan** | <*approver-email-address*> | De goedkeurder e-mailadres. U kunt uw eigen adres gebruiken voor testdoeleinden. | 
   | **Opties voor gebruikers** | Goedkeuren, Weigeren | De antwoordopties die de goedkeurder kunt kiezen. Standaard de goedkeurder kunt ofwel 'goedkeuren' of 'Negeren' als de reactie. | 
   | **Onderwerp** | Lid-aanvraag voor de test leden ML goedkeuren | Een onderwerp beschrijvende e | 
   |  |  |  | 

   Op dit moment negeren de dynamische inhoud lijst of inline-parameter die wordt weergegeven wanneer u in specifieke invoervakken klikt. 
   Deze lijst kunt u parameters selecteren uit eerdere acties die u als invoer in uw werkstroom gebruiken kunt. 
   De breedte van uw browser bepaalt welke lijst wordt weergegeven. 
 
4. Sla uw logische app.

Vervolgens voegt u een voorwaarde om te controleren van de goedkeurder gekozen antwoord.

## <a name="check-approval-response"></a>Goedkeuringsantwoord controleren

1. Onder de **goedkeuring per E-mail** actie, kies **+ een nieuwe stap** > **een voorwaarde toevoegen**.

   De vorm van de voorwaarde wordt weergegeven, samen met alle beschikbare parameters die u als invoer voor uw werkstroom opnemen kunt. 

2. Wijzig de naam van de voorwaarde van een betere beschrijving.

   1. Kies op de titelbalk van de voorwaarde **weglatingstekens** (**...** ) knop > **naam**.

      Bijvoorbeeld, als uw browser in smalle weergave:

      ![Naam voorwaarde wijzigen](./media/tutorial-process-mailing-list-subscriptions-workflow/condition-rename.png)

      Als uw browser in de weergave voor het gehele is en de lijst met dynamische inhoud blokkeert de toegang tot de knop met weglatingstekens, sluit u de lijst door te kiezen **dynamische inhoud toevoegen** binnen de voorwaarde.

   2. Wijzig de naam van de voorwaarde van deze beschrijving:```If request approved```

3. Maken van een voorwaarde waarmee wordt gecontroleerd of de goedkeurder geselecteerd **goedkeuren**:

   1. Klik in de voorwaarde in de **kiest u een waarde** vak die aan de linkerkant (wide browserweergave) of op de voorgrond (browserweergave smalle).
   Selecteer in de lijst met parameters of de lijst met dynamische inhoud, de **SelectedOption** veld onder **goedkeuring per E-mail**.

      Bijvoorbeeld, als u in de weergave voor het gehele werkt, de voorwaarde ziet eruit als in dit voorbeeld:

      ![Selecteer onder 'Verzenden goedkeuring e', 'SelectedOption'](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response.png)

   2. Selecteer in het vak vergelijking operator deze operator: **is gelijk aan**

   3. In de onderste (smalle weergave) of naar rechts (wide weergave) **kiest u een waarde** vak, voert u deze waarde:```Approve```

      Wanneer u bent klaar, ziet de voorwaarde in dit voorbeeld:

      ![Volledige voorwaarde](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response-2.png)

4. Sla uw logische app.

Geef vervolgens de actie die uw logische app uitvoert wanneer de revisor de aanvraag goedkeurt. 

## <a name="add-member-to-mailchimp-list"></a>Lid toevoegen aan lijst van MailChimp

Voeg nu een actie waarmee de goedgekeurde lid wordt toegevoegd aan uw adreslijst.

1. In de voorwaarde **als de waarde true** vertakking, kiest u **een actie toevoegen**.
Zoek naar 'mailchimp' en selecteert u deze actie: **MailChimp - lid toevoegen aan de lijst**

   ![Selecteer "MailChimp - lid toevoegen aan de lijst '](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member.png)

3. Als u wordt gevraagd of u aan te melden bij uw MailChimp-account, kunt u aanmelden met uw MailChimp-referenties.

4. Geef hier informatie voor deze actie wordt weergegeven en beschreven:

   ![Geef informatie op voor 'Lid toevoegen aan lijst'](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Lijst-Id** | test-members-ML | De naam voor uw adreslijst MailChimp | 
   | **Status** | geabonneerd | De abonnementstatus voor het nieuwe lid. Zie voor meer informatie <a href="https://developer.mailchimp.com/documentation/mailchimp/guides/manage-subscribers-with-the-mailchimp-api/" target="_blank">beheren abonnees met de API van MailChimp</a>. | 
   | **E-mailadres** | <*new-member-email-address*> | Selecteer in de lijst met parameters of lijst van dynamische inhoud, **van** onder **wanneer een nieuw e binnenkomt**, die wordt doorgegeven in het e-mailadres voor het nieuwe lid. 
   |  |  |  | 

5. Sla uw logische app.

Voeg vervolgens een voorwaarde zodat u controleren kunt of het nieuwe lid geworden van uw adreslijst. Op die manier uw logische app waarschuwt u of deze bewerking is geslaagd of mislukt.

## <a name="check-for-success-or-failure"></a>Controleren op slagen of mislukken

1. In de **als de waarde true** vertakking onder de **lid toevoegen aan lijst** actie, kies **meer...**   >  **Een voorwaarde toevoegen**.

2. Wijzig de naam van de voorwaarde van deze beschrijving:```If add member succeeded```

3. Bouw een voorwaarde waarmee wordt gecontroleerd of het goedgekeurde lid slaagt of mislukt in uw adreslijst toevoegen:

   1. Klik in de voorwaarde in de **kiest u een waarde** vak die aan de linkerkant (wide browserweergave) of op de voorgrond (browserweergave smalle).
   Selecteer in de lijst met parameters of de lijst met dynamische inhoud, de **Status** veld onder **lid toevoegen aan lijst**.

      Bijvoorbeeld, als u in de weergave voor het gehele werkt, de voorwaarde ziet eruit als in dit voorbeeld:

      ![Onder 'Lid toevoegen aan de lijst', 'Status' selecteren](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member.png)

   2. Selecteer in het vak vergelijking operator deze operator: **is gelijk aan**

   3. In de onderste (smalle weergave) of naar rechts (wide weergave) **kiest u een waarde** vak, voert u deze waarde:```subscribed```

   Wanneer u bent klaar, ziet de voorwaarde in dit voorbeeld:

   ![Voltooide voorwaarde](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member-2.png)

Vervolgens stelt u de e-mailberichten verzenden wanneer het goedgekeurde lid slaagt of mislukt in lid te worden van uw adreslijst.

## <a name="send-email-if-member-added"></a>E-mailbericht verzenden als lid toegevoegd

1. In de **als de waarde true** vertakking voor de voorwaarde **als geslaagd lid toevoegen**, kies **een actie toevoegen**.

   ![In filialen 'als de waarde true' voor de voorwaarde, kiest u 'Een actie toevoegen'](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success.png)

2. Zoek naar 'e-mail in outlook verzenden' en selecteert u deze actie:  **< *uw op e-mailprovider*>-e-mailbericht verzenden**

   ![Actie voor 'Stuurt u een e-mailbericht' toevoegen](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-2.png)

3. Wijzig de naam van de actie met deze beschrijving:```Send email on success```

4. Bevatten informatie voor deze actie wordt weergegeven en beschreven:

   ![Geef informatie op voor succes e-mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Aan** | <*your-email-address*> | Het e-mailadres voor de locatie waar u het succes e-mailbericht verzenden. Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken. | 
   | **Onderwerp** | <*subject-for-success-email*> | Het onderwerp voor de e-mail geslaagd. Deze tekst invoeren voor deze zelfstudie, en selecteer het opgegeven veld onder **lid toevoegen aan lijst** uit de parameterlijst of dynamische inhoud: <p>"Success! Lid toegevoegd aan 'test leden ML': **e-mailadres**' | 
   | **Hoofdtekst** | <*body-for-success-email*> | De inhoud van de instantie voor het succes e-mailbericht. Deze tekst invoeren voor deze zelfstudie en selecteer onder de opgegeven velden **lid toevoegen aan lijst** uit de parameterlijst of dynamische inhoud:  <p>' Nieuw lid geworden van 'test leden ML': **e-mailadres**'</br>' Lid deelnamestatus: **Status**' | 
   | | | | 

5. Sla uw logische app.

## <a name="send-email-if-member-not-added"></a>E-mailbericht verzenden als lid is niet toegevoegd

1. In de **indien false** vertakking voor de voorwaarde **als geslaagd lid toevoegen**, kies **een actie toevoegen**.

   ![In 'als false"vertakking kiezen voor de voorwaarde 'Een actie toevoegen'](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed.png)

2. Zoek naar 'e-mail in outlook verzenden' en selecteert u deze actie:  **< *uw op e-mailprovider*>-e-mailbericht verzenden**

   ![Actie voor 'Stuurt u een e-mailbericht' toevoegen](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-2.png)

3. Wijzig de naam van de actie met deze beschrijving:```Send email on failure```

4. Geef hier informatie voor deze actie wordt weergegeven en beschreven:

   ![Geef informatie op voor e-mailadres is mislukt](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Aan** | <*your-email-address*> | Het e-mailadres voor het verzenden van de e-mail is mislukt. Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken. | 
   | **Onderwerp** | <*subject-for-failure-email*> | Het onderwerp voor de e-mail is mislukt. Deze tekst invoeren voor deze zelfstudie, en selecteer het opgegeven veld onder **lid toevoegen aan lijst** uit de parameterlijst of dynamische inhoud: <p>' Is mislukt, lid is niet toegevoegd aan 'test leden ML': **e-mailadres**' | 
   | **Hoofdtekst** | <*body-for-failure-email*> | De inhoud van de hoofdtekst voor de e-mail is mislukt. Voor deze zelfstudie, voert u deze tekst: <p>"Lid al bestaat mogelijk. Controleer uw MailChimp-account." | 
   | | | | 

5. Sla uw logische app. 

Test vervolgens uw logische app, nu er ongeveer als in dit voorbeeld volgt:

 ![Voltooide logische app](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-complete.png)

## <a name="run-your-logic-app"></a>Voer uw logische app

1. Zelf een e-aanvraag om lid van uw adreslijst te verzenden.
Wacht totdat de aanvraag in je postvak in wordt weergegeven.

3. Kies **Uitvoeren** op de werkbalk in de ontwerper als u de logische app handmatig wilt uitvoeren. 

   Als uw e-mailadres een onderwerp dat overeenkomt met de trigger onderwerp filter heeft, verzendt uw logische app dat u e-voor het goedkeuren van de Abonnementaanvraag.

4. Kies in de e-mail goedkeuring **goedkeuren**.

5. Als e-mailadres van de abonnee niet op uw mailinglijst bestaat, wordt uw logische app wordt toegevoegd die persoon e-mailadres en verzendt u een e-mail als volgt:

   ![E-mailadres geslaagd](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-success.png)

   Als uw logische app niet de abonnee toevoegen, krijgt u een e-mailbericht zoals in dit voorbeeld:

   ![Mislukte e](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-failed.png)

   Als u een e-mailberichten geen krijgt, controleert u uw e-mailadres ongewenste map. 
   Uw ongewenste e-mail mogelijk dit soort e-mails omleiden. 
   Als u niet zeker weet of uw logische app correct wordt uitgevoerd, kunt u [Problemen met uw logische app oplossen](../logic-apps/logic-apps-diagnosing-failures.md) raadplegen.

Gefeliciteerd, u hebt nu gemaakt en een logische app uitgevoerd die is geïntegreerd informatie over Azure, Microsoft-services en andere SaaS-apps.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep die uw logische app en de bijbehorende resources bevat. Op het Azure hoofdmenu, gaat u naar **resourcegroepen**, en selecteert u de resourcegroep voor uw logische app. Kies **resourcegroep verwijderen**. Voer de naam van de resourcegroep als bevestiging en kies **verwijderen**.

!["Overview" > 'Delete bronnengroep'](./media/tutorial-process-mailing-list-subscriptions-workflow/delete-resource-group.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een logische app die wordt beheerd goedkeuringen voor mailinglijst aanvragen gemaakt. Nu informatie over het bouwen van een logische app, die worden verwerkt en opgeslagen e-mailbijlagen door integratie van Azure-services, zoals Azure Storage en Azure Functions.

> [!div class="nextstepaction"]
> [E-mailbijlagen proces](../logic-apps/tutorial-process-email-attachments-workflow.md)