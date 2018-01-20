---
title: Werkstromen voor het verwerken van e-mailberichten en bijlagen - Azure Logic Apps bouwen | Microsoft Docs
description: Deze zelfstudie laat zien hoe u geautomatiseerde werkstromen voor het verwerken van e-mailberichten en bijlagen met Azure Logic Apps, Azure Storage en Azure Functions maakt
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
ms.openlocfilehash: 210731ce2e792452650b7a92cfc542c78a0e8014
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="process-emails-and-attachments-with-a-logic-app"></a>Verwerken e-mailberichten en bijlagen met een logische app

Logische Apps van Azure kunt u werkstromen automatiseren en integratie van gegevens over Azure-services, Microsoft-services, andere software as a service (SaaS)-apps en on-premises systemen. Deze zelfstudie laat zien hoe u kunt samenstellen een [logische app](../logic-apps/logic-apps-overview.md) dat binnenkomende e-mailberichten en eventuele bijlagen worden verwerkt. Deze logica app processen die inhoud slaat de inhoud naar Azure-opslag en meldingen voor de beoordeling van die inhoud worden verzonden. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van [Azure storage](../storage/common/storage-introduction.md) en Opslagverkenner voor het controleren van e-mailberichten en bijlagen opgeslagen.
> * Maak een [Azure functie](../azure-functions/functions-overview.md) die HTML verwijdert uit de e-mailberichten. Deze zelfstudie bevat de code die u voor deze functie kunt gebruiken.
> * Een lege, logische app maken.
> * Toevoegen van een trigger die e-mailberichten voor bijlagen bewaakt.
> * Toevoegen van een voorwaarde waarmee wordt gecontroleerd of e-mailberichten bijlagen hebben.
> * Een actie die de Azure-functie aanroept wanneer een e-mailbericht bijlagen heeft toevoegen.
> * Een actie die wordt gemaakt van storage-blobs voor e-mailberichten en bijlagen toevoegen.
> * Een actie dat e-mailmeldingen verzendt toevoegen.

Wanneer u bent klaar, ziet uw logische app deze werkstroom op hoog niveau:

![Op hoog niveau klaar logische app](./media/tutorial-process-email-attachments-workflow/overview.png)

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">meld u dan aan voor een gratis Azure-account</a> voordat u begint. 

## <a name="prerequisites"></a>Vereisten

* Een e-mailaccount van een e-provider wordt ondersteund door logische Apps zoals Outlook van Office 365, Outlook.com of Gmail. Voor andere providers [controleren de connectors hier lijst](https://docs.microsoft.com/connectors/).

  Deze logische app gebruikmaakt van een Outlook van Office 365-account. 
  Als u een ander e-mailaccount gebruikt, wordt de algemene stappen hetzelfde blijven, maar uw gebruikersinterface iets anders uitzien.

* Download en installeer de <a href="http://storageexplorer.com/" target="_blank">gratis Microsoft Azure Storage Explorer</a>. Dit hulpprogramma kunt u controleren of uw storage-container correct is ingesteld.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Aanmelden bij de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> met de referenties van uw Azure-account.

## <a name="set-up-storage-to-save-attachments"></a>Opslag van het opslaan van bijlagen instellen

U kunt binnenkomende e-mailberichten en bijlagen opslaan als blobs in een [Azure storage-container](../storage/common/storage-introduction.md). 

1. Voordat u kunt een opslagcontainer maken [een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) met deze instellingen:

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Naam** | attachmentstorageacct | De naam voor uw storage-account | 
   | **Implementatiemodel** | Resource Manager | De [implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md) voor het beheren van resources implementeren | 
   | **Account-type** | Algemeen doel | De [opslagaccounttype](../storage/common/storage-introduction.md#types-of-storage-accounts) | 
   | **Prestaties** | Standard | Deze instelling geeft de ondersteunde gegevenstypen en media voor het opslaan van gegevens. Zie [typen opslagaccounts](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Replicatie** | Lokaal redundante opslag (LRS) | Deze instelling bepaalt u hoe uw gegevens worden gekopieerd, opgeslagen, beheerd en gesynchroniseerd. Zie [replicatie](../storage/common/storage-introduction.md#replication). | 
   | **Veilige overdracht vereist** | Uitgeschakeld | Deze instelling geeft de beveiliging die vereist zijn voor het aanvragen van verbindingen. Zie [vereisen veilige overdracht](../storage/common/storage-require-secure-transfer.md). | 
   | **Abonnement** | <*your-Azure-subscription-name*> | De naam voor uw Azure-abonnement | 
   | **Resourcegroep** | LA-zelfstudie-RG | De naam voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) gebruikt voor het organiseren en verwante resources beheren. <p>**Opmerking:** een resourcegroep binnen een bepaalde regio bestaat. Hoewel de items in deze zelfstudie mogelijk niet beschikbaar in alle regio's, kunt u proberen te gebruiken dezelfde regio indien mogelijk. | 
   | **Locatie** | VS - oost 2 | De regio waar informatie over uw storage-account wordt opgeslagen | 
   | **Virtuele netwerken configureren** | Uitgeschakeld | Voor deze zelfstudie houden de **uitgeschakelde** instelling. | 
   |||| 

   U kunt ook [Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) of [Azure CLI](../storage/common/storage-quickstart-create-storage-account-cli.md).
  
2. Nadat Azure storage-account heeft geïmplementeerd, kunt u uw storage-account toegangssleutel ophalen:

   1. Account van uw opslag menu onder **instellingen**, kies **toegangssleutels**. 
   2. Zoeken naar **key1** onder **standaard sleutels** en de naam van uw opslagaccount.

      ![Kopiëren en opslaan van de naam van het opslagaccount en de sleutel](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   U kunt ook [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) of [Azure CLI](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az_storage_account_keys_list). 

3. Maak een opslagcontainer die voor uw e-mailbijlagen.
   
   1. In het menu storage-account op de **overzicht** deelvenster kiezen **Blobs** onder **Services**, en kies vervolgens **+ Container**.

   2. Voer 'bijlagen' als de containernaam. Onder **openbare toegangsniveau**, selecteer **Container (anonieme leestoegang voor containers en blobs)**, en kies **OK**.

   U kunt ook [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer), of [Azure CLI](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create). 
   Wanneer u bent klaar, kunt u uw storage-container vinden in uw opslagaccount hier in de Azure portal:

   ![Voltooide storage-container](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

Opslagverkenner wordt vervolgens verbinding maken met uw storage-account.

## <a name="set-up-storage-explorer"></a>Opslagverkenner instellen

Nu Opslagverkenner verbinding met uw storage-account, zodat u bevestigen kunt dat uw logische app correct bijlagen opslaan als blobs in uw storage-container.

1. Open Microsoft Azure Storage Explorer. Als u Opslagverkenner vraagt u om een verbinding met Azure storage, kiest u **gebruik van een naam van het opslagaccount en de sleutel** > **volgende**.
Als er geen prompt wordt weergegeven, **account toevoegen** op de werkbalk explorer.

2. Onder **toevoegen met de naam en sleutel**, voer de naam van uw opslagaccount en de toegangssleutel die u eerder hebt opgeslagen. Kies **volgende** > **verbinding**.

3. Controleer of uw opslagaccount en container goed worden weergegeven in Opslagverkenner:

   1. Onder **Explorer**, vouw **(lokaal en Attached)** > 
    **Opslagaccounts** > **attachmentstorageaccount** > 
    **Blob-Containers**.

   2. Controleer of de container 'bijlagen' nu voorkomt. 
   Bijvoorbeeld:

      ![Opslagverkenner - storage-container bevestigen](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Maak vervolgens een [Azure functie](../azure-functions/functions-overview.md) om HTML te verwijderen van binnenkomende e-mail.

## <a name="create-a-function-to-clean-html"></a>Maak een functie om op te ruimen HTML

Nu gebruiken het codefragment geleverd door deze stappen voor het maken van een Azure-functie waarmee HTML uit elk binnenkomende e-mailbericht worden verwijderd. Op die manier wordt de inhoud van e-mail schonere en gemakkelijker te verwerken. Vervolgens kunt u deze functie aanroepen vanuit uw logische app.

1. Voordat u kunt een functie maken [maken van een functie-app](../azure-functions/functions-create-function-app-portal.md) met deze instellingen:

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Naam van app** | CleanTextFunctionApp | Een globaal unieke en beschrijvende naam voor uw app functie | 
   | **Abonnement** | <*your-Azure-subscription-name*> | De dezelfde Azure-abonnement dat u eerder hebt gebruikt | 
   | **Resourcegroep** | LA-zelfstudie-RG | De dezelfde Azure-resourcegroep die u eerder hebt gebruikt | 
   | **Hosting-Plan** | Verbruiksabonnement | Deze instelling bepaalt hoe u wilt toewijzen en scale-resources, zoals rekenkracht voor het uitvoeren van de functie-app. Zie [vergelijking hosting plannen](../azure-functions/functions-scale.md). | 
   | **Locatie** | VS - oost 2 | Dezelfde regio die u eerder hebt gebruikt | 
   | **Storage** | cleantextfunctionstorageacct | Maak een opslagaccount voor de functie-app. Gebruik alleen kleine letters en cijfers. <p>**Opmerking:** dit opslagaccount bevat de functie-apps en verschilt van uw eerder gemaakte storage-account voor e-mailbijlagen. | 
   | **Application Insights** | Uit | Hiermee schakelt u toepassingsbewaking met [Application Insights](../application-insights/app-insights-overview.md), maar voor deze zelfstudie houden de **uit** instelling. | 
   |||| 

   Als de functie-app niet automatisch wordt geopend na de implementatie, vindt u uw app in de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>. Kies in het Azure hoofdmenu **App Services**, en selecteert u de functie-app.

   ![Gemaakte functie-app](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Als **App Services** niet weergegeven in het menu van Azure, gaat u naar **meer services** in plaats daarvan. Zoek in het zoekvak en selecteer **functie Apps**. Zie voor meer informatie [uw functie maken](../azure-functions/functions-create-first-azure-function.md).

   U kunt ook [Azure CLI](../azure-functions/functions-create-first-azure-function-azure-cli.md), of [PowerShell en Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md).

2. Onder **functie Apps**, vouw **CleanTextFunctionApp**, en selecteer **functies**. Kies op de werkbalk functies **+ een nieuwe functie**.

   ![Nieuwe functie maken](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. Onder **Kies een sjabloon onder of Ga naar de Quick Start**, selecteer de **HttpTrigger - C#** functie sjabloon.

   ![Functie-sjabloon selecteren](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

4. Onder **naam van de functie**, voer ```RemoveHTMLFunction```. Onder **HTTP-trigger** > **autorisatieniveau**, gebruik de standaardwaarde **functie** waarde en kies **maken**.

   ![Een naam voor de functie opgeven](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Nadat de editor wordt geopend, moet u de sjablooncode vervangen door deze code, die de HTML-code verwijdert en retourneert de resultaten naar de aanroeper:

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });

   }
   ```

6. Als u bent klaar, kiest u **opslaan**. Als u wilt testen uw functie, kies **testen** onder de pijl (**<**) pictogram aan de rechterkant van de editor. 

   ![Open het deelvenster 'Test'](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. In de **Test** deelvenster onder **aanvraagtekst**, voert u deze regel en kies **uitvoeren**.

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![Uw functie testen](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   De **uitvoer** venster ziet dit resultaat van de functie:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

Nadat u hebt gecontroleerd dat de functie werkt, moet u uw logische app maken. Hoewel deze zelfstudie ziet u het maken van een functie die HTML verwijdert uit de e-mailberichten, Logic Apps heeft ook een **HTML naar tekst** connector.

## <a name="create-your-logic-app"></a>Uw logische app maken

1. Kies in het Azure hoofdmenu **nieuw** > **Enterprise Integration** > **logische App**.

   ![Logische app maken](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. Onder **maken logische app**, bevatten deze informatie over uw logische app weergegeven en beschreven. Als u klaar bent, kiest u **Vastmaken aan dashboard** > **Maken**.

   ![Geef informatie op logic app](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Naam** | LA ProcessAttachment | De naam voor uw logische app | 
   | **Abonnement** | <*your-Azure-subscription-name*> | De dezelfde Azure-abonnement dat u eerder hebt gebruikt | 
   | **Resourcegroep** | LA-zelfstudie-RG | De dezelfde Azure-resourcegroep die u eerder hebt gebruikt |
   | **Locatie** | VS - oost 2 | Dezelfde regio die u eerder hebt gebruikt | 
   | **Log Analytics** | Uit | Voor deze zelfstudie houden de **uit** instelling. | 
   |||| 

3. Nadat u uw app wordt geïmplementeerd in Azure, wordt de ontwerpfunctie van Logic Apps wordt geopend en ziet u een pagina met een video inleiding en sjablonen voor algemene logic app patronen. Kies onder **Sjablonen** de optie **Lege logische app**.

   ![Sjabloon voor lege logische app kiezen](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Vervolgens voegt u een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) die luistert naar binnenkomende e-mailberichten met bijlagen. Elke logische app moet beginnen met een trigger, die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde voldoet aan de nieuwe gegevens. Zie voor meer informatie [uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>Inkomende e-mail controleren

1. Voer op de designer 'als u e-mailbericht ontvangt' in het zoekvak. Selecteer deze trigger voor uw e-mailprovider:  **< *uw e-provider*>: wanneer een nieuw e-mailadres binnenkomt**, bijvoorbeeld:

   ![Selecteer deze trigger voor e-mailprovider: 'Wanneer u een nieuw e-mailadres binnenkomt'](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Voor werk- of schoolaccounts van Azure selecteert u Outlook van Office 365. 
   * Selecteer Outlook.com voor persoonlijke Microsoft-accounts. 

2. Als u wordt gevraagd om referenties, aanmelden bij uw e-mailaccount zodat Logic Apps verbinding met uw e-mailaccount maken kunnen.

3. Bieden nu de criteria die gebruikmaakt van de trigger voor het filteren van nieuwe e-mail.

   1. Geef de map, interval en frequentie voor het controleren van e-mailberichten.

      ![Geef een map, interval en frequentie voor het controleren van e-mails](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Instelling | Waarde | Beschrijving | 
      | ------- | ----- | ----------- | 
      | **Folder** | Postvak in | De map e-mail om te controleren | 
      | **Interval** | 1 | Het aantal intervallen moet worden gewacht tussen controles | 
      | **Frequentie** | Minuut | Controleert de tijdseenheid voor elk interval tussen | 
      |  |  |  | 
  
   2. Kies **geavanceerde opties weergeven** en deze instellingen opgeven:

      | Instelling | Waarde | Beschrijving | 
      | ------- | ----- | ----------- | 
      | **Bijlage** | Ja | Alleen de e-mails met bijlagen worden opgehaald. <p>**Opmerking:** de trigger e-mailberichten niet worden verwijderd uit je account, alleen nieuwe berichten controleren en alleen e-mailberichten die overeenkomen met het filter onderwerp verwerken. | 
      | **Bijlagen opnemen** | Ja | Ophalen van de bijlagen als invoer voor uw werkstroom in plaats van alleen controleren voor bijlagen. | 
      | **Onderwerp-Filter** | ```Business Analyst 2 #423501``` | De tekst te vinden in het onderwerp van e-mail | 
      |  |  |  | 

4. Als u wilt verbergen van de trigger gegevens nu, klikt u in de titelbalk van de trigger.

   ![Samenvouwen vorm details verbergen](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Sla uw logische app. Kies **Opslaan** op de werkbalk van de ontwerper.

   Uw logische app is nu live maar niet doen andere Controleer uw e-mailberichten. 
   Vervolgens voegt u een voorwaarde waarmee wordt aangegeven criteria om door te gaan van de werkstroom.

## <a name="check-for-attachments"></a>Controleer voor bijlagen

1. Kies onder de trigger **+ een nieuwe stap** > **een voorwaarde toevoegen**.

   Wanneer de vorm van de voorwaarde wordt weergegeven, wordt standaard de parameterlijst of de lijst met dynamische inhoud wordt weergegeven en ziet u de parameters van de vorige stap die u als invoer van de werkstroom opnemen kunt. 
   De breedte van uw browser bepaalt welke lijst wordt weergegeven.

2. Wijzig de naam van de voorwaarde van een betere beschrijving.

   1. Kies op de titelbalk van de voorwaarde **weglatingstekens** (**...** ) knop > **naam**.

      Bijvoorbeeld, als uw browser in smalle weergave:

      ![Naam voorwaarde wijzigen](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

      Als uw browser in de weergave voor het gehele is en de lijst met dynamische inhoud blokkeert de toegang tot de knop met weglatingstekens, sluit u de lijst door te kiezen **dynamische inhoud toevoegen** binnen de voorwaarde. 
      
      ![Lijst met dynamische inhoud sluiten](./media/tutorial-process-email-attachments-workflow/close-dynamic-content-list.png)

   2. Wijzig de naam van de voorwaarde van deze beschrijving:```If email has attachments and key subject phrase```

3. Beschrijf de voorwaarde aan de hand van een expressie. 

   1. Kies in de vorm voorwaarde **bewerken in de geavanceerde modus**.

      ![Voorwaarde in de geavanceerde modus bewerken](./media/tutorial-process-email-attachments-workflow/edit-advanced-mode.png)

   2. Voer in het tekstvak in deze expressie:

      ```@equals(triggerBody()?['HasAttachment'], bool('true'))```

      Deze expressie vergelijkt de **heeftbijlage** eigenschapswaarde van de trigger-instantie, die het e-mailbericht in deze zelfstudie maakt met het Booleaanse object is ```True```. 
      Als beide waarden gelijk zijn, het e-mailbericht heeft ten minste één bijlage, het doorlopen van de voorwaarde, en de werkstroom blijft.

      De voorwaarde is nu ziet eruit als in dit voorbeeld:

      ![Voorwaarde-expressie](./media/tutorial-process-email-attachments-workflow/condition-expression.png)

   3. Kies **bewerken in de standaardmodus**. De expressie nu opgelost als volgt te werk:

      ![Opgelost expressie](./media/tutorial-process-email-attachments-workflow/condition-expression-resolved.png)

      > [!NOTE]
      > Handmatig bouwen van een expressie, moet u in de standaardmodus werken en de dynamische lijst openen zodat u met de opbouwfunctie werken kunt. Onder **expressie**, kunt u functies. Onder **dynamische inhoud**, kunt u de parametervelden moeten worden gebruikt in deze functies.
      > Deze zelfstudie ziet later handmatig maken expressies.

4. Sla uw logische app.

### <a name="test-your-condition"></a>De voorwaarde testen

Nu controleren of de voorwaarde correct werkt:

1. Als uw logische app al wordt uitgevoerd, kies **uitvoeren** op de werkbalk van de ontwerpfunctie.

   Deze stap wordt uw logische app handmatig gestart zonder te wachten tot het opgegeven interval is verstreken. 
   Echter, gebeurt er niets totdat het test-e-mailbericht in je postvak in. 

2. Stuur uzelf een e-mailbericht aan deze criteria voldoet:

   * Uw e-mailadres onderwerp is de tekst die u hebt opgegeven in de trigger **onderwerp filter**:```Business Analyst 2 #423501```

   * Uw e-mailadres heeft één bijlage. 
   Op dit moment een leeg tekstbestand maken en dat bestand koppelen aan uw e-mailadres.

   Als u het e-mailbericht ontvangt, controleert uw logische app voor bijlagen en de tekst opgegeven onderwerp.
   Als de voorwaarde voldoet, wordt de trigger wordt geactiveerd en zorgt ervoor dat de Logic Apps-engine voor het maken van een logische app-exemplaar en de werkstroom starten. 

3. Om te controleren dat de trigger wordt geactiveerd en de logische app geslaagd, in het menu logic app is kiezen **overzicht**.

   ![Controleer de geschiedenis van de trigger en wordt uitgevoerd](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Als uw logische app niet hebt geactiveerd of ondanks een geslaagde trigger, Zie [problemen met uw logische app](../logic-apps/logic-apps-diagnosing-failures.md).

Vervolgens definieert u welk acties te ondernemen voor het **als de waarde true** vertakking. Voor het opslaan van het e-mailbericht en eventuele bijlagen, Verwijder HTML-code uit de hoofdtekst van de e-mail en blobs maken in de storage-container voor de e-mail en bijlagen.

> [!NOTE]
> Uw logische app heeft geen verder niets te doen voor de **indien false** vertakking als een e-mailbericht geen bijlagen. Als een extra oefening nadat u klaar bent met deze zelfstudie kunt u toevoegen een geschikte actie op die u maken wilt voor de **indien false** vertakking.

## <a name="call-the-removehtmlfunction"></a>De RemoveHTMLFunction aanroepen

1. Kies in het menu logic app **Logic App-ontwerper**. In de **als de waarde true** vertakking, kiest u **een actie toevoegen**.

2. Zoek naar 'azure functions' en selecteert u deze actie: **Azure Functions: Kies een Azure-functie**

   ![Selecteer actie 'Azure Functions - Kies een Azure-functie'](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Selecteer uw eerder gemaakte functie-app: **CleanTextFunctionApp**

   ![Uw Azure-functie-app selecteren](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Nu uw functie te selecteren: **RemoveHTMLFunction**

   ![Selecteer uw Azure-functie](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Wijzig de naam van de vorm van de functie met deze beschrijving:```Call RemoveHTMLFunction to clean email body``` 

6. Voer in de vorm van de functie, de invoer voor de functie voor het verwerken. Geef het e-mailbericht hier weergegeven en beschreven:

   ![Geef de hoofdtekst van de aanvraag voor de functie verwacht](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   1. Onder **aanvraagtekst**, voert u deze tekst: 
   
      ```{ "emailBody": ``` 

      Totdat u dit item in de volgende stappen zijn voltooid, wordt er een fout over ongeldige JSON weergegeven.
      Wanneer u deze functie eerder getest, gebruikt de invoer voor deze functie opgegeven notatie JSON (JavaScript Object). 
      Dezelfde indeling moet dus ook door de aanvraagtekst gebruiken. 

   2. Selecteer in de lijst met parameters of de lijst met dynamische inhoud, de **hoofdtekst** veld onder **wanneer een nieuw e-mailadres binnenkomt**.
   Na de **hoofdtekst** veld, voegt de sluitaccolade:```}```

      ![Geef de hoofdtekst van de aanvraag voor het doorgeven aan de functie](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing2.png)

      Deze vermelding verschijnt in de definitie van logische apps in deze indeling:

      ```{ "emailBody": "@triggerBody()?['Body']" }```

7. Sla uw logische app.

Vervolgens voegt u een actie die wordt gemaakt van een blob in uw storage-container voor het opslaan van het e-mailbericht.

## <a name="create-blob-for-email-body"></a>Blob voor de hoofdtekst van de e-mail maken

1. Kies onder de vorm Azure functie **een actie toevoegen**. 

2. Onder **kiest u een actie**, 'blob' zoekt en selecteert u deze actie: **Azure Blob Storage-blob maken**

   ![Actie voor het maken van blob voor de hoofdtekst van de e-mail toevoegen](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Als u geen verbinding met een Azure storage-account hebt, maak een verbinding met uw storage-account met deze instellingen als weergegeven en beschreven hier. Als u bent klaar, kiest u **maken**.

   ![Maak verbinding met de storage-account](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Verbindingsnaam** | AttachmentStorageConnection | Een beschrijvende naam voor de verbinding | 
   | **Storage-Account** | attachmentstorageacct | De naam voor het opslagaccount dat u eerder hebt gemaakt voor het opslaan van bijlagen | 
   |||| 

4. Wijzig de naam van de **maken blob** actie met deze beschrijving:```Create blob for email body```

5. In de **maken blob** actie, deze informatie geven, en selecteer deze parameters voor het maken van de blob als weergegeven en beschreven:

   ![Blob-gegevens voor de hoofdtekst van de e-mail opgeven](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Pad naar de map** | /Attachments | Het pad en de naam van de container die u eerder hebt gemaakt. U kunt ook bladeren en selecteer een container. | 
   | **BLOB-naam** | **Van** veld | Als de blob-naam in de naam van de e-mailadres afzender doorgeven. Selecteer in de lijst met parameters of lijst van dynamische inhoud, **van** onder **wanneer een nieuw e-mailadres binnenkomt**. | 
   | **BLOB-inhoud** | **Inhoud** veld | Als de blob-inhoud in de hoofdtekst HTML gratis e doorgeven. Selecteer in de lijst met parameters of lijst van dynamische inhoud, **hoofdtekst** onder **RemoveHTMLFunction aanroepen op te schonen hoofdtekst van de e-mail**. |
   |||| 

6. Sla uw logische app. 

### <a name="check-attachment-handling"></a>Controleer de afhandeling van bijlagen

Nu controleren of uw logische app omgaat met e-mailberichten de manier waarop u hebt opgegeven:

1. Als uw logische app al wordt uitgevoerd, kies **uitvoeren** op de werkbalk van de ontwerpfunctie.

2. Stuur uzelf een e-mailbericht aan deze criteria voldoet:

   * Uw e-mailadres onderwerp is de tekst die u hebt opgegeven in de trigger **onderwerp filter**:```Business Analyst 2 #423501```

   * Uw e-mailadres heeft ten minste één bijlage. 
   Op dit moment een leeg tekstbestand maken en dat bestand koppelen aan uw e-mailadres.

   * Uw e-mailadres heeft bijvoorbeeld de inhoud van sommige test in de hoofdtekst van: 

     ```
     Testing my logic app
     ```

   Als uw logische app niet hebt geactiveerd of ondanks een geslaagde trigger, Zie [problemen met uw logische app](../logic-apps/logic-apps-diagnosing-failures.md).

3. Controleer of uw logische app het e-mailbericht naar de juiste opslagcontainer opgeslagen. 

   1. Vouw in Opslagverkenner **(lokaal en Attached)** > 
    **Opslagaccounts** > **attachmentstorageacct (extern)** > 
    **Blob-Containers** > **bijlagen**.

   2. Controleer de **bijlagen** container voor het e-mailbericht. 

      Op dit punt wordt het e-mailbericht wordt weergegeven in de container omdat de logische app nog de bijlagen niet verwerken.

      ![Selectievakje Opslagverkenner voor opgeslagen e-mail](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. Wanneer u bent klaar, verwijdert u het e-mailbericht in Opslagverkenner.

4. Optioneel, voor het testen van de **indien false** vertakking, welke heeft nothing op dit moment kunt u sturen een e-mail die niet voldoet aan de criteria.

Vervolgens voegt u een lus voor het verwerken van alle e-mailbijlagen.

## <a name="process-attachments"></a>Proces-bijlagen

Deze logica app gebruikmaakt van een **voor elk** lus voor het verwerken van elke bijlage in het e-mailbericht.

1. Onder de **maken blob voor de hoofdtekst van de e-mail** vorm, kiest u **... Meer**, en selecteert u deze opdracht: **toevoegen een voor elk**

   !["Voor elk' lus toevoegen](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Wijzig de naam van de lus met deze beschrijving:```For each email attachment```

3. Nu de gegevens voor de lus verwerken opgeven. Klik in de **uitvoer selecteren uit de vorige stappen** vak. Selecteer in de lijst met parameters of in de lijst met dynamische inhoud **bijlagen**. 

   ![Selecteer 'Bijlagen'](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   De **bijlagen** veld een matrix met de bijlagen die zijn opgenomen in een e-mailbericht wordt doorgegeven. 
   De **voor elk** lus herhaalt de acties op elk item dat wordt doorgegeven met de matrix.

4. Sla uw logische app.

Vervolgens voegt u de actie die wordt elke bijlage opgeslagen als een blob in uw **bijlagen** storage-container.

## <a name="create-blobs-for-attachments"></a>Blobs voor bijlagen maken

1. In de **voor elk** herhalen, kies **een actie toevoegen** zodat u kunt de taak uit te voeren op elke gevonden bijlage opgeven.

   ![Actie lus toevoegen](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. Onder **kiest u een actie**, zoek naar 'blob' en selecteer vervolgens deze actie: **Azure Blob Storage-blob maken**

   ![Actie voor het maken van blob toevoegen](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Wijzig de naam van de **maken blob 2** actie met deze beschrijving:```Create blob for each email attachment```

4. In de **blob maken voor elk e-mailbijlage** actie, deze informatie geven, en selecteer de parameters voor het maken van elke blob als weergegeven en beschreven:

   ![Blob-informatie opgeven](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Instelling | Waarde | Beschrijving | 
   | ------- | ----- | ----------- | 
   | **Pad naar de map** | /Attachments | Het pad en de naam van de container u eerder hebt gemaakt. U kunt ook bladeren naar en selecteer een container. | 
   | **BLOB-naam** | **Naam** veld | Selecteer in de lijst met parameters of lijst van dynamische inhoud, **naam** om door te geven in de naam van de bijlage voor de blob-naam. | 
   | **BLOB-inhoud** | **Inhoud** veld | Selecteer in de lijst met parameters of lijst van dynamische inhoud, **inhoud** om door te geven in de bijlage-inhoud voor de blob-inhoud. |
   |||| 

5. Sla uw logische app. 

### <a name="check-attachment-handling"></a>Controleer de afhandeling van bijlagen

Vervolgens controleren of uw logische app verwerkt de bijlagen van de manier waarop u hebt opgegeven:

1. Als uw logische app al wordt uitgevoerd, kies **uitvoeren** op de werkbalk van de ontwerpfunctie.

2. Stuur uzelf een e-mailbericht aan deze criteria voldoet:

   * Uw e-mailadres onderwerp is de tekst die u hebt opgegeven in de trigger **onderwerp filter**:```Business Analyst 2 #423501```

   * Uw e-mailadres heeft ten minste twee bijlagen. 
   Nu twee bestanden met een lege tekenreeks maken en deze bestanden toevoegen aan uw e-mailadres.

   Als uw logische app niet hebt geactiveerd of ondanks een geslaagde trigger, Zie [problemen met uw logische app](../logic-apps/logic-apps-diagnosing-failures.md).

3. Controleer of uw logische app e-mail en bijlagen naar de juiste storage-container opgeslagen. 

   1. Vouw in Opslagverkenner **(lokaal en Attached)** > 
    **Opslagaccounts** > **attachmentstorageacct (extern)** > 
    **Blob-Containers** > **bijlagen**.

   2. Controleer de **bijlagen** container voor zowel het e-mailadres en de bijlagen.

      ![Controleer voor opgeslagen e-mail en bijlagen](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. Wanneer u klaar bent, de e-mail en bijlagen in Opslagverkenner verwijderen.

Vervolgens wordt een actie toevoegen zodat uw logische app verzendt e-mail om te controleren van de bijlagen.

## <a name="send-email-notifications"></a>E-mailmeldingen verzenden

1. In de **als de waarde true** vertakking onder de **voor elk e-mailbijlage** herhalen, kies **een actie toevoegen**. 

   ![Actie onder "voor elke" toevoegen lus](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. Onder **kiest u een actie**, zoek naar 'e-mailbericht verzenden' en selecteer vervolgens de actie 'e-mail verzenden' voor de e-mailprovider die u wilt. Als u wilt filteren op de lijst met acties voor een bepaalde service, kunt u de connector eerst onder **Connectors**.

   ![Selecteer de actie 'e-mail verzenden' voor uw e-mailprovider](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Voor werk- of schoolaccounts van Azure selecteert u Outlook van Office 365. 
   * Selecteer Outlook.com voor persoonlijke Microsoft-accounts. 

3. Als u wordt gevraagd om referenties, aanmelden bij uw e-mailaccount zodat Logic Apps een verbinding met uw e-mailaccount maakt.

4. Wijzig de naam van de **e-mailbericht verzenden** actie met deze beschrijving:```Send email for review```

5. Geef de informatie voor deze actie en selecteer de velden die u wilt opnemen in de e-mail als weergegeven en beschreven. Als u lege regels wilt toevoegen in een invoervak, drukt u op Shift + Enter.  

   Bijvoorbeeld, als u werkt met de lijst met dynamische inhoud:

   ![E-mailmelding verzenden](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Als u een veld met het verwachte kan niet in de lijst vinden, selecteert u **Zie voor meer informatie** naast **wanneer een nieuw e-mailadres binnenkomt** in de lijst met dynamische inhoud of aan het einde van de parameterlijst.

   | Instelling | Waarde | Opmerkingen | 
   | ------- | ----- | ----- | 
   | **Aan** | <*recipient-email-address*> | Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken. | 
   | **Onderwerp**  | ```ASAP - Review applicant for position: ```**Onderwerp** | Het onderwerp van de e-mail die u wilt opnemen. Selecteer in de lijst met parameters of lijst van dynamische inhoud, de **onderwerp** veld onder **wanneer een nieuw e-mailadres binnenkomt**. | 
   | **Hoofdtekst** | ```Please review new applicant:``` <p>```Applicant name: ```**Uit** <p>```Application file location: ```**Pad** <p>```Application email content: ``` **Body** | De inhoud voor het e-mailbericht. Selecteer in de lijst met parameters of lijst van dynamische inhoud, deze velden: <p>-De **van** veld onder **wanneer een nieuw e-mailadres binnenkomt** </br>-De **pad** veld onder **maken blob voor de hoofdtekst van e-mail** </br>-De **hoofdtekst** veld onder **RemoveHTMLFunction aanroepen op te schonen hoofdtekst van e-mail** | 
   |||| 

   Als u per ongeluk selecteert u een veld een matrix, zoals bevat **inhoud**, dit is een matrix met bijlagen, de designer voegt automatisch een lus 'voor elk' rond de actie die verwijst naar dat veld. 
   Op die manier kan de actie voor elk matrixitem worden uitgevoerd. 
   Als u wilt verwijderen van de lus, verwijder het veld voor de matrix verplaatsen de verwijzende actie buiten de lus, kies het beletselteken (**...** ) op de titel van de lus, en kies **verwijderen**.
     
6. Sla uw logische app. 

Test vervolgens uw logische app nu ziet eruit als in dit voorbeeld:

![Voltooide logische app](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>Voer uw logische app

1. Stuur uzelf een e-mailbericht aan deze criteria voldoet:

   * Uw e-mailadres onderwerp is de tekst die u hebt opgegeven in de trigger **onderwerp filter**:```Business Analyst 2 #423501```

   * Uw e-mailadres heeft op een of meer bijlagen. 
   U kunt een leeg tekstbestand van de vorige test hergebruiken. 
   Koppel een resume-bestand voor een meer realistische scenario.

   * Het e-mailbericht heeft deze tekst die u kunt kopiëren en plakken:

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Voer uw logische app. Als dit lukt, verzendt uw logische app een e-mailbericht dat op het volgende voorbeeld lijkt:

   ![E-mailmelding verzonden door de logische app](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Als u een e-mailberichten geen krijgt, controleert u uw e-mailadres ongewenste map. 
   Uw ongewenste e-mail mogelijk dit soort e-mails omleiden. 
   Als u niet zeker weet of uw logische app correct wordt uitgevoerd, kunt u [Problemen met uw logische app oplossen](../logic-apps/logic-apps-diagnosing-failures.md) raadplegen.

Gefeliciteerd, u hebt nu gemaakt en uitvoeren van een logische app die taken automatiseert alle andere Azure-services en aangepaste code aanroepen.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep die uw logische app en de bijbehorende resources bevat. Op het Azure hoofdmenu, gaat u naar **resourcegroepen**, en selecteert u de resourcegroep voor uw logische app. Kies **resourcegroep verwijderen**. Voer de naam van de resourcegroep als bevestiging en kies **verwijderen**.

![Logic app-resourcegroep verwijderen](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een logische app, die worden verwerkt en opgeslagen e-mailbijlagen door integratie van Azure-services, zoals Azure Storage en Azure Functions gemaakt. Nu meer informatie over andere connectors die u gebruiken kunt voor het bouwen van logic apps.

> [!div class="nextstepaction"]
> [Meer informatie over connectors voor Logic Apps](../connectors/apis-list.md)