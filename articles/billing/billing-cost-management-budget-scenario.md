---
title: Azure-facturering en budgetscenario voor het beheer van kosten | Microsoft Docs
description: Informatie over het gebruik van Azure automatation om virtuele machines op basis van specifieke budgetdrempels af te sluiten.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: db93f546-6b56-4b51-960d-1a5bf0274fc8
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 7/25/2018
ms.author: erikre
ms.openlocfilehash: 08cdce23fe298bdac932d1ca8abb8166f4d7bbbd
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094810"
---
# <a name="manage-costs-with-azure-budgets"></a>Kosten beheren met Azure Budgets

Kostenbeheer is een essentieel onderdeel tot het maximaliseren van de waarde van uw investering in de cloud. Er zijn verschillende scenario's waarin kosten zichtbaarheid, rapportage en orchestration kosten op basis van essentieel belang voor voortdurende klandizie bewerkingen. [Azure Cost Management-API's](https://docs.microsoft.com/rest/api/consumption/) bieden een set API's ter ondersteuning van elk van deze scenario's. De API's bieden informatie over het gebruik, zodat u kunt om gedetailleerde exemplaar kosten weer te geven.

Budgetten worden vaak gebruikt als onderdeel van de kosten te beheren. Budgetten kunnen worden gericht in Azure. U kunt bijvoorbeeld uw budget weergeven op basis van het abonnement, resourcegroepen of een verzameling resources beperken. Naast het gebruik van de budgetten API om u te informeren via e-mail wanneer een budgetdrempelwaarde is bereikt, kunt u [Azure Monitor-actiegroepen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) voor het activeren van een gecoördineerd set acties als gevolg van een gebeurtenis budget.

Een veelvoorkomend scenario budgetten voor een klant met een niet-kritieke workload kan bijvoorbeeld optreden wanneer u ze wilt beheren op basis van een budget en ook gaan naar een voorspelbare kosten bij het onderzoeken van de maandelijkse factuur. Dit scenario is vereist voor sommige orchestration kosten op basis van de resources die deel uitmaken van de Azure-omgeving. In dit scenario wordt is een maandelijkse budget van $1000 voor het abonnement ingesteld. Meldingsdrempels worden ook ingesteld voor het activeren van een aantal indelingen. In dit scenario begint met een drempelwaarde 80% kosten die alle virtuele machines in de resourcegroep stopt **optioneel**. Klik vervolgens op de drempelwaarde voor 100% kosten, worden alle VM-exemplaren gestopt.
Om dit scenario configureert, voltooit u de volgende acties door de stappen in elke sectie van deze zelfstudie te volgen. 

Deze acties die zijn opgenomen in deze zelfstudie kunnen u:

- Maak een Azure Automation-Runbook als u wilt stoppen van VM's met behulp van webhooks.
- Maken van een logische Azure-App wordt geactiveerd op basis van de drempelwaarde van het budget en aanroepen van het runbook met de juiste parameters.
- Maak een Azure Monitor-actiegroep die worden geconfigureerd voor het activeren van de logische App van Azure wanneer de budgetdrempel wordt voldaan.
- Het budget van de Azure maken met de gewenste drempelwaarden en wire-aan de actiegroep.

## <a name="create-an-azure-automation-runbook"></a>Een Azure Automation-Runbook maken

[Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) is een service waarmee u de meeste beheertaken voor uw resource script en deze taken worden uitgevoerd als gepland of on-demand. Als onderdeel van dit scenario, maakt u een [Azure Automation-runbook](https://docs.microsoft.com/azure/automation/automation-runbook-types) die wordt gebruikt voor het stoppen van VM's. Gebruikt u de [Azure V2 VM stoppen's](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafisch runbook vanuit de [galerie](https://docs.microsoft.com/azure/automation/automation-runbook-gallery) om te maken van dit scenario. Met dit runbook importeren in uw Azure-account en deze publiceren, kunt u zich niet meer virtuele machines wanneer een budgetdrempelwaarde is bereikt. 

### <a name="create-an-azure-automation-account"></a>Een Azure Automation-account maken

1.  Gebruik de referenties van uw Azure-account om u aan melden bij het [Azure Portal](https://portal.azure.com/).
2.  Klik op de **een resource maken** knop gevonden in de linkerbovenhoek van Azure.
3.  Selecteer **beheerhulpprogramma** > **Automation**.
    > [!NOTE]
    > Als u een Azure-account niet hebt, kunt u een [gratis account](https://azure.microsoft.com/free/).
4.  Voer gegevens over uw account. Voor **Azure uitvoeren als-account maken**, kiest u **Ja** automatisch inschakelen van de instellingen die nodig zijn voor verificatie bij Azure vereenvoudigen.
5.  Als u daarmee klaar bent, klikt u op **Maken** om met de implementatie van het Automation-account te beginnen.

### <a name="import-the-stop-azure-v2-vms-runbook"></a>Importeer het runbook stoppen Azure V2-VM 's

Met behulp van een [Azure Automation-runbook](https://docs.microsoft.com/azure/automation/automation-runbook-types), importeert de [Azure V2 VM stoppen's](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafisch runbook vanuit de galerie.

1.  Gebruik de referenties van uw Azure-account om u aan melden bij het [Azure Portal](https://portal.azure.com/).
2.  Open uw Automation-account en selecteer **alle services** > **Automation-Accounts**. Selecteer vervolgens uw Automation-Account.
3.  Klik op **runbookgalerie** uit de **procesautomatisering** sectie.
4.  Stel de **Galeriebron** naar **Script Center** en selecteer **OK**.
5.  Zoek en selecteer de [Azure V2 VM stoppen's](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) galerie-item in Azure portal.
6.  Klik op de **importeren** knop om weer te geven de **importeren** blade en selecteer **OK**. De overzichtsblade van het runbook wordt weergegeven.
7.  Als het runbook kan tijdens het importeren is voltooid, selecteert u **bewerken** om de optie voor editor en publiceren van een grafisch runbook weer te geven.
    
    ![Azure - grafisch runbook bewerken](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-01.png)
8.  Klik op de **publiceren** om het runbook publiceren en selecteer vervolgens **Ja** wanneer hierom wordt gevraagd. Wanneer u een runbook publiceert, moet u een bestaande gepubliceerde versie met de conceptversie overschrijven. In dit geval hebt u geen gepubliceerde versie omdat u het runbook hebt gemaakt. 

    Zie voor meer informatie over het publiceren van een runbook [maken van een grafisch runbook](https://docs.microsoft.com/azure/automation/automation-first-runbook-graphical).

## <a name="create-webhooks-for-the-runbook"></a>Webhooks voor het runbook maken

Met behulp van de [Azure V2 VM stoppen's](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafisch runbook, maakt u twee Webhooks voor het runbook in Azure Automation starten via een afzonderlijke HTTP-aanvraag. De eerste webhook roept het runbook met een budgetdrempelwaarde 80% met de naam van de resource als een parameter, zodat de optionele virtuele machines moet worden gestopt. Tweede webhook roept vervolgens het runbook zonder parameters (op 100%), die alle resterende VM-exemplaren stopt.

1.  Uit de **Runbooks** pagina in de [Azure-portal](https://portal.azure.com/), klikt u op de **StopAzureV2Vm** runbook dat wordt weergegeven de overzichtsblade van het runbook. 
2.  Klik op **Webhook** aan de bovenkant van de pagina te openen de **Webhook toevoegen** blade.
3.  Klik op **nieuwe webhook maken** openen de **een nieuwe webhook maken** blade.
4.  Stel de **naam** van de Webhook te **optioneel**. De **ingeschakeld** eigenschap moet **Ja**. De **verloopt** waarde hoeft niet te worden gewijzigd. Zie voor meer informatie over de eigenschappen van de Webhook [Details van een webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#details-of-a-webhook).
5.  Klik naast de URL-waarde, op het kopieerpictogram om te kopiëren van de URL van de webhook. 
    > [!IMPORTANT]
    > Sla de URL van de webhook met de naam **optioneel** op een veilige plaats. U gebruikt de URL verderop in deze zelfstudie. Uit veiligheidsoverwegingen als u de webhook maakt u niet weergeven of de URL opnieuw ophalen.
6.  Klik op **OK** te maken van de nieuwe webhook.
7.  Klik op **configureren parameters en uitvoerinstellingen** waarden om weer te geven van de parameter voor het runbook. 
    > [!NOTE]
    > Als het runbook verplichte parameters heeft, klikt u vervolgens bent u niet de webhook te maken tenzij er waarden zijn verstrekt.
8.  Klik op **OK** te accepteren van de webhook-parameterwaarden.
9.  Klik op **maken** te maken van de webhook.
10. Volg vervolgens de stappen hierboven om het maken van een tweede webhook met de naam **voltooid**. 
    > [!IMPORTANT]
    > Zorg ervoor dat u beide webhook-URL's voor gebruik verderop in deze zelfstudie. Uit veiligheidsoverwegingen als u de webhook maakt u niet weergeven of de URL opnieuw ophalen.

U hebt nu twee geconfigureerde webhooks die elke beschikbaar met de URL's die u hebt opgeslagen. 

![Webhooks - optioneel en is voltooid](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-02.png)

U bent nu klaar met de Azure Automation-instellingen. U kunt de webhooks testen met een eenvoudige Postman-test om te valideren dat de webhook werkt. Vervolgens moet u de logische App voor het indelen van maken.

## <a name="create-an-azure-logic-app-for-orchestration"></a>Een logische App van Azure voor het indelen van maken

Logische Apps te bouwen, plannen en processen als werkstromen automatiseren, zodat u apps, gegevens, systemen en services voor bedrijven of organisaties kunt integreren. In dit scenario, de [logische App](https://docs.microsoft.com/azure/logic-apps/) maken van wordt iets meer dan alleen aanroep doen het automation-webhook die u hebt gemaakt. 

Budgetten kunnen worden ingesteld voor het activeren van een melding wanneer een opgegeven drempelwaarde wordt voldaan. U kunt meerdere drempelwaarden om te worden geïnformeerd op en de logische App demonstreert de mogelijkheid voor verschillende acties op basis van de drempelwaarde die is voldaan. In dit voorbeeld stelt u een scenario waarin u een aantal meldingen krijgt, is de eerste melding daarover voor wanneer 80% van het budget is bereikt en de tweede melding is wanneer 100% van het budget is bereikt. De logische app wordt gebruikt om alle virtuele machines in de resourcegroep af te sluiten. Eerst de **optioneel** drempelwaarde wordt bereikt op 80% en vervolgens de tweede drempelwaarde wordt bereikt wanneer alle virtuele machines in het abonnement wordt uitgeschakeld.

Logische apps kunt u een voorbeeldschema bieden voor de HTTP-trigger, maar moet u om in te stellen de **Content-Type** header. Omdat de actiegroep aangepaste headers voor de webhook niet heeft, moet u de nettolading in een aparte stap parseer. U gebruikt de **parseren** actie en deze opgeven met een voorbeeld-nettolading.

### <a name="create-the-logic-app"></a>De logische app maken

De logische app worden verschillende acties worden uitgevoerd. De volgende lijst biedt een op hoog niveau set acties die door de logische app wordt uitgevoerd:
- Herkent wanneer er een HTTP-aanvraag wordt ontvangen
- Het parseren van de doorgegeven in JSON-gegevens om te bepalen van de drempelwaarde die is bereikt
- Gebruik een voorwaardelijke statement om te controleren of het drempelbedrag heeft bereikt 80% of meer van het budget bereik, maar niet groter zijn dan of gelijk aan 100%.
    - Als deze drempelwaardehoeveelheid is bereikt, stuurt u een HTTP POST met behulp van de webhook met de naam **optioneel**. Deze actie wordt de virtuele machines in de groep 'Optioneel' afgesloten.
- Gebruik een voorwaardelijke statement om te controleren of het drempelbedrag heeft bereikt of overschreden voor 100% van de Budgetwaarde.
    - Als het drempelbedrag is bereikt, stuurt u een HTTP POST met behulp van de webhook met de naam **voltooid**. Deze actie wordt afgesloten met alle resterende virtuele machines.

De volgende stappen nodig zijn om te maken van de logische app die de bovenstaande stappen uit:

1.  In de [Azure-portal](https://portal.azure.com/), selecteer **een resource maken** > **integratie** > **logische App**.
    
    ![Azure - resource voor de logische App selecteren](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03.png)
2.  In de **logische app maken** blade, geef de details wilt maken van uw logische app, selecteer **vastmaken aan dashboard**, en klikt u op **maken**. 
    
    ![Azure - een logische App maken](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03a.png)

Nadat Azure uw logische app, implementeert het **ontwerper van logische Apps** wordt geopend en ziet u een blade met een inleidende video en veelgebruikte triggers. 

### <a name="add-a-trigger"></a>Een trigger toevoegen

Elke logische app moet beginnen met een trigger, die wordt geactiveerd wanneer er een bepaalde gebeurtenis plaatsvindt of wanneer er aan een bepaalde voorwaarde is voldaan. Telkens wanneer de trigger wordt geactiveerd, maakt de Logic Apps-engine een exemplaar van een logische app dat wordt gestart en de werkstroom uitvoert. Acties zijn de stappen die na de trigger plaatsvinden. 

1.  Onder **sjablonen** van de **ontwerper van logische Apps** blade, kiest u **lege logische App**.
2.  Toevoegen een [trigger](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts) door te voeren 'http-aanvraag in de **ontwerper van logische Apps** zoekvak om te zoeken en selecteer de trigger met de naam **aanvragen – als een HTTP-aanvraag wordt ontvangen**.
    
    ![Azure - logische app - Http-trigger](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-04.png) 
3.  Selecteer **nieuwe stap** > **een actie toevoegen**. 
    
    ![Azure - nieuwe stap - een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-05.png) 
4.  Zoeken naar 'JSON parseren' de **ontwerper van logische Apps** zoekvak om te zoeken en selecteer de **gegevensbewerkingen - JSON parseren** [actie](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts). 
    
    ![Azure - logische app - Voeg de actie JSON parseren](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-06.png) 
5.  Voer '-nettolading in' als de **inhoud** naam voor het parseren van JSON-nettolading of gebruik de tag 'Hoofdtekst' van dynamische inhoud.
6.  Selecteer de **voorbeeldnettolading gebruiken voor het genereren van schema** optie in de **JSON parseren** vak.
    
    ![Azure - logische app - gebruik voorbeeld JSON-gegevens om het schema genereren](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-07.png) 
7.  Plak de volgende JSON-voorbeeld-nettolading in het tekstvak: `{"schemaId":"AIP Budget Notification","data":{"SubscriptionName":"CCM - Microsoft Azure Enterprise - 1","SubscriptionId":"<GUID>","SpendingAmount":"100","BudgetStartDate":"6/1/2018","Budget":"50","Unit":"USD","BudgetCreator":"email@contoso.com","BudgetName":"BudgetName","BudgetType":"Cost","ResourceGroup":"","NotificationThresholdAmount":"0.8"}}`
    
    Het tekstvak wordt weergegeven als het volgende:
        
    ![Azure - logische app - de voorbeeld-JSON-nettolading](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-08.png) 
8.  Klik op **Gereed**.

### <a name="add-the-first-conditional-action"></a>De eerste voorwaardelijke actie toevoegen

Gebruik een voorwaardelijke statement om te controleren of het drempelbedrag heeft bereikt 80% of meer van het budget bereik, maar niet groter zijn dan of gelijk aan 100%. Als deze drempelwaardehoeveelheid is bereikt, stuurt u een HTTP POST met behulp van de webhook met de naam **optioneel**. Deze actie wordt afgesloten met de virtuele machines in de **optioneel** groep.

1.  Selecteer **nieuwe stap** > **een voorwaarde toevoegen**.
    
    ![Azure - logische app - een voorwaarde toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-09.png) 
2.  In de **voorwaarde** vak, klikt u op het tekstvak waarin **een waarde kiezen** om een lijst met beschikbare waarden weer te geven.
    
    ![Vak voor Azure - logische app - voorwaarde](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-10.png) 
        
3.  Klik op **expressie** boven aan de lijst en voer de volgende expressie in de expressie-editor: `float()`
    
    ![Azure - logische app - gegevenstype met drijvende komma expressie](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-11.png) 
        
4.  Selecteer **dynamische inhoud**, plaats de cursor binnen de (haakje) en selecteer **NotificationThresholdAmount** in de lijst voor het vullen van de volledige expressie. 
    
    De expressie wordt niet het volgende:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`
    
5.  Selecteer **OK** om in te stellen van de expressie. 
6.  Selecteer **is groter dan of gelijk zijn aan** in de vervolgkeuzelijst van het **voorwaarde**.
7.  In de **een waarde kiezen** vak van de voorwaarde `.8`.
    
    ![Azure - logische app - gegevenstype met drijvende komma expressie met een waarde](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-12.png) 
        
8.  Klik op **toevoegen** > **rij toevoegen** in het vak voorwaarde om toe te voegen een extra onderdeel van de voorwaarde.
9.  In de **voorwaarde** vak, klikt u op het tekstvak waarin **een waarde kiezen**.
10. Klik op **expressie** boven aan de lijst en voer de volgende expressie in de expressie-editor: `float()`
11. Selecteer **dynamische inhoud**, plaats de cursor binnen de (haakje) en selecteer **NotificationThresholdAmount** in de lijst voor het vullen van de volledige expressie. 
12. Selecteer **OK** om in te stellen van de expressie. 
13. Selecteer **is minder dan** in de vervolgkeuzelijst van het **voorwaarde**.
14. In de **een waarde kiezen** vak van de voorwaarde `1`.
    
    ![Azure - logische app - gegevenstype met drijvende komma expressie met een waarde](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-13.png) 
        
15. In de **als de waarde true** Schakel **een actie toevoegen**. Voegt u een HTTP POST-bewerking die optioneel VM's wordt afgesloten.
    
    ![Azure - logische app - een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-14.png) 
        
16. Voer **HTTP** om te zoeken naar de HTTP-actie en selecteer de **HTTP: HTTP** actie.
    
    ![Azure - logische app - toevoegen HTTP-actie](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-15.png) 
        
17. Selecteer **Post** als de voor de **methode** waarde.
18. Voer de URL voor de webhook met de naam **optioneel** die eerder in deze zelfstudie als de **Uri** waarde.
    
    ![HTTP-actie voor de Azure - logische app - URI](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-16.png) 
        
19. Selecteer **een actie toevoegen** in de **als de waarde true** vak. Voegt u een e-mailactie waarbij een hoogte van de ontvanger die de optionele VM's zijn afgesloten e-mailbericht wordt verzonden.
20. Zoek naar 'e-mail verzenden' en selecteer een *e-mailbericht verzenden* actie op basis van de e-mailservice die u gebruikt.
        
    ![Azure - logische app - verzenden e-mailactie](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-17.png) 
    
    Selecteer **Outlook.com** voor persoonlijke Microsoft-accounts. Selecteer **Office 365 Outlook** voor werk- of schoolaccounts van Azure. Als u nog geen verbinding hebt, wordt u gevraagd om u aan te melden bij uw e-mailaccount. Logic Apps maakt een verbinding met uw e-mailaccount.
        
    U wilt toestaan dat de logische App toegang tot uw e-mail-informatie.
    
    ![Aankondiging van Azure - logische app - toegang](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-18.png) 
        
21. Toevoegen de **naar**, **onderwerp**, en **hoofdtekst** tekst voor het e-mailbericht dat de ontvanger die de optionele VM's zijn afgesloten. Gebruik de **BudgetName** en de **NotificationThresholdAmount** dynamische inhoud voor het vullen van de velden onderwerp en hoofdtekst.
    
    ![Details van de Azure - logische app - e-mailadres](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-19.png) 
 
### <a name="add-the-second-conditional-action"></a>De tweede voorwaardelijke actie toevoegen

Gebruik een voorwaardelijke statement om te controleren of het drempelbedrag heeft bereikt of overschreden voor 100% van de Budgetwaarde. Als het drempelbedrag is bereikt, stuurt u een HTTP POST met behulp van de webhook met de naam **voltooid**. Deze actie wordt afgesloten met alle resterende virtuele machines.

1.  Selecteer **nieuwe stap** > **een voorwaarde toevoegen**.
    
    ![Azure - logische app - actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-20.png) 
        
2.  In de **voorwaarde** vak, klikt u op het tekstvak waarin **een waarde kiezen** om een lijst met beschikbare waarden weer te geven.
3.  Klik op **expressie** boven aan de lijst en voer de volgende expressie in de expressie-editor: `float()`
4.  Selecteer **dynamische inhoud**, plaats de cursor binnen de (haakje) en selecteer **NotificationThresholdAmount** in de lijst voor het vullen van de volledige expressie. 
    
    De expressie wordt niet het volgende:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`
        
5.  Selecteer **OK** om in te stellen van de expressie. 
6.  Selecteer **is groter dan of gelijk zijn aan** in de vervolgkeuzelijst van het **voorwaarde**.
7.  In de **kiest u een waardevak** invoeren van de voorwaarde `1`.
    
    ![Azure - logische app - voorwaarde waarde](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-21.png) 
        
8.  In de **als de waarde true** Schakel **een actie toevoegen**. Voegt u een HTTP POST-bewerking die wordt afgesloten met de resterende VM's.
    
    ![Azure - logische app - een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-22.png) 
    
9.  Voer **HTTP** om te zoeken naar de HTTP-actie en selecteer de **HTTP: HTTP** actie.
10. Selecteer **Post** als de voor de **methode** waarde.
11. Voer de URL voor de webhook met de naam **voltooid** die eerder in deze zelfstudie als de **Uri** waarde.
    
    ![Azure - logische app - een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-23.png) 
        
12. Selecteer **een actie toevoegen** in de **als de waarde true** vak. Voegt u een e-mailactie waarbij een hoogte van de ontvanger die de resterende VM's zijn afgesloten e-mailbericht wordt verzonden.
13. Zoek naar 'e-mail verzenden' en selecteer een *e-mailbericht verzenden* actie op basis van de e-mailservice die u gebruikt.
14. Toevoegen de **naar**, **onderwerp**, en **hoofdtekst** tekst voor het e-mailbericht dat de ontvanger die de optionele VM's zijn afgesloten. Gebruik de **BudgetName** en de **NotificationThresholdAmount** dynamische inhoud voor het vullen van de velden onderwerp en hoofdtekst.
    
    ![Informatie voor Azure - logische app - verzenden per e-mail verzenden](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-24.png) 
        
15. Klik op **opslaan** aan de bovenkant van de **Logic App Designer** blade.

### <a name="logic-app-summary"></a>Overzicht van logische App

Hier ziet u hoe uw logische App eruit ziet wanneer u klaar bent. In de meeste algemene van scenario's waarin u een indeling op basis van een drempelwaarde niet moet u rechtstreeks de automatiseringsscript uit kan aanroepen **Monitor** en slaan de **logische App** stap.

   ![Azure - logische app - voltooid weergeven](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-25.png) 
 
Bij het opslaan van uw logische app is een URL gegenereerd dat kunt u zich aan te roepen. U gebruikt deze URL in de volgende sectie van deze zelfstudie. 

## <a name="create-an-azure-monitor-action-group"></a>Een actiegroep van Azure Monitor maken

Een actiegroep is een verzameling van voorkeuren voor meldingen die u definieert. Wanneer een waarschuwing wordt geactiveerd, kan een bepaalde actiegroep die u ontvangt van de waarschuwing door op de hoogte worden gesteld. Een Azure-waarschuwing wordt gegeven van een melding op basis van specifieke voorwaarden proactief en biedt de mogelijkheid om actie te ondernemen. Een waarschuwing kunt gegevens uit meerdere bronnen, met inbegrip van metrische gegevens en Logboeken gebruiken.

Actiegroepen zijn de enige eindpunt dat u kan worden geïntegreerd met uw budget. U kunt meldingen in een aantal kanalen instellen, maar voor dit scenario dat u wordt de nadruk gelegd op de logische App die u eerder in deze zelfstudie hebt gemaakt. 

### <a name="create-an-action-group-in-azure-monitor"></a>Een actiegroep maken in Azure Monitor

Wanneer u de actiegroep maakt, wijst u aan de logische App die u eerder in deze zelfstudie hebt gemaakt. 

1.  Als u niet al bent aangemeld op de [Azure-portal](https://portal.azure.com/), voor aanmelding bij en selecteer **alle services** > **Monitor**. 
2.  Selecteer **acties groepen** uit de **instelling** sectie.
3.  Selecteer **een actiegroep toevoegen** uit de **actiegroepen** blade.
4.  Toevoegen en controleer of de volgende items:
    - Naam van de actiegroep
    - Korte naam
    - Abonnement
    - Resourcegroep
    
    ![Azure - logische app - een actiegroep toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-26.png) 
        
5.  Binnen de **actiegroep toevoegen** in het deelvenster een LogicApp-actie toevoegen. Naam van de actie **Budget BudgetLA**. In de **logische App** venster de **abonnement** en de **resourcegroep**. Selecteer vervolgens de **logische app** die u eerder in deze zelfstudie hebt gemaakt.
6.  Klik op **OK** om in te stellen van de logische App. Selecteer **OK** in de **actiegroep toevoegen** deelvenster om de actiegroep te maken.
    
U bent klaar met alle ondersteunende onderdelen die nodig zijn voor het indelen van effectief uw budget. Nu hoeft u maken van het budget en configureer deze voor het gebruik van de actiegroep die u hebt gemaakt.

## <a name="create-the-azure-budget"></a>De Azure Budget maken 

Budgetten geen op dit moment een portal-ervaring in Azure. U bent echter kunnen aanroepen van een REST-API's, Powershell-cmdlets, of de CLI gebruiken. De volgende procedure maakt gebruik van de REST-API. Voordat u de REST-API aanroept, moet u een verificatietoken. Voor het maken van een verificatietoken, kunt u de [ARMClient](https://github.com/projectkudu/ARMClient) project. De **ARMClient** kunt u verifiëren zelf aan de Azure Resource Manager en ophalen van een token voor de API's aanroepen. 

### <a name="create-an-authentication-token"></a>Maken van een verificatietoken

1.  Navigeer naar de [ARMClient](https://github.com/projectkudu/ARMClient) project op GitHub.
2.  Kloon de opslagplaats om een lokale kopie.
3.  Open het project in Visual Studio en bouw het.
4.  Wanneer de build voltooid is, wordt het uitvoerbare bestand moet de *\bin\debug* map.
5.  De ARMClient worden uitgevoerd. Open een opdrachtprompt en navigeer naar de *\bin\debug* map vanuit de hoofdmap van het project.
6.  Als u wilt aanmelden en verifiëren, voer de volgende opdracht uit vanaf de opdrachtprompt:<br>
    `ARMClient login prod`
7.  Kopieer de **abonnement-guid** uit de uitvoer.
8.  Als u wilt een verificatietoken naar het Klembord te kopiëren, voer de volgende opdracht bij de opdrachtprompt, maar zorg ervoor dat u de gekopieerde abonnements-ID van de bovenstaande stap: <br>
    `ARMClient token <subscription GUID from previous step>`
    
    Nadat u de bovenstaande stap hebt voltooid, ziet u het volgende:<br>
    **Token naar Klembord gekopieerd.**
9.  Sla het token moet worden gebruikt voor de stappen in de volgende sectie van deze zelfstudie.

### <a name="create-the-budget"></a>Maken van het Budget

Vervolgens configureert u **Postman** een budget maken door het aanroepen van de Azure Consumption REST-API's. Postman is een API-ontwikkelomgeving. U wordt omgeving en verzameling bestanden importeren in Postman. De verzameling bevat gegroepeerde definities van HTTP-aanvragen die Azure Consumption REST-API's aanroepen. Het omgevingsbestand bevat variabelen die worden gebruikt door de verzameling.

1.  Downloaden en open de [Postman REST-client](https://www.getpostman.com/) voor het uitvoeren van de REST API's.
2.  Maak een nieuwe aanvraag in Postman.
    
    ![Postman - een nieuwe aanvraag maken](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-27.png) 
        
3.  Sla de nieuwe aanvraag als een verzameling, zodat de nieuwe aanvraag geen invloed op het heeft.
    
    ![Postman - Sla de nieuwe aanvraag](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-28.png) 
        
4.  Wijzigen van de aanvraag van een `Get` naar een `Put` actie.
5.  Wijzigen van de volgende URL te vervangen `{subscriptionId}` met de **abonnements-ID** die u in de vorige sectie van deze zelfstudie gebruikt. Wijzig ook de URL om op te nemen 'SampleBudget' als de waarde voor `{budgetName}`: `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2018-03-31`
6.  Selecteer de **Headers** tabblad in Postman.
7.  Toevoegen van een nieuwe **sleutel** met de naam 'Autorisatie'.
8.  Stel de **waarde** naar het token dat is gemaakt met de ArmClient aan het einde van de laatste sectie. 
9.  Selecteer **hoofdtekst** tabblad in Postman. 
10. Selecteer de **onbewerkte** knop optie.
11. Plak in het tekstvak de onderstaande voorbeelddefinitie budget, moet u echter vervangen de **subscriptionid**, **budgetname**, en **actiongroupname** parameters met uw abonnements-id, een unieke naam voor uw budget en de naam van de actie die u in zowel de URL en de hoofdtekst van de aanvraag gemaakt:
    
    ```
        {
            "properties": {
                "category": "Cost",
                "amount": 100.00,
                "timeGrain": "Monthly",
                "timePeriod": {
                "startDate": "2018-06-01T00:00:00Z",
                "endDate": "2018-10-31T00:00:00Z"
                },
                "filters": {
                },
            "notifications": {
                "Actual_GreaterThan_80_Percent": {
                    "enabled": true,
                    "operator": "GreaterThan",
                    "threshold": 80,
                    "contactEmails": [
                    ],
                    "contactRoles": [
                    ],
                    "contactGroups": [
                    "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}
                    ]
                }, 
            "Actual_EqualTo_100_Percent": {
                    "operator": "EqualTo",
                    "threshold": 100,
                    "contactGroups": [
                "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}"
                    ]
                }
            }
        }
    ```
12. Druk op **verzenden** om de aanvraag te verzenden.

U hebt nu alle onderdelen die u nodig hebt om aan te roepen de [budgetten API](https://docs.microsoft.com/rest/api/consumption/budgets). De API-verwijzing voor budgetten heeft aanvullende informatie op de specifieke aanvragen, met inbegrip van het volgende:
    - **budgetName** -meerdere budgetten worden ondersteund.  Budgetnamen moeten uniek zijn.
    - **categorie** -moet een **kosten** of **gebruik**. De API ondersteunt zowel kosten en gebruik budgetten.
    - **timeGrain** -een budget per maand, kwartaal of jaar. Hiermee stelt u de hoeveelheid aan het einde van de periode.
    - **filters** -Filters kunt u beperken van het budget naar een specifieke set bronnen binnen de geselecteerde scope. Een filter kan bijvoorbeeld een verzameling van resourcegroepen voor een abonnement op budget.
    - **meldingen** – bepaalt de details van melding en drempelwaarden. U kunt meerdere drempels instellen en geef een e-mailadres of een actiegroep een melding te ontvangen.

## <a name="summary"></a>Samenvatting

Door deze zelfstudie te volgen, hebt u het volgende geleerd:
- Het maken van een Azure Automation-Runbook als u wilt stoppen van VM's.
- Over het maken van een logische Azure-App die wordt geactiveerd op basis van de drempelwaarden budget en aanroepen van het bijbehorende runbook met de juiste parameters.
- Over het maken van een actiegroep voor Azure Monitor die is geconfigureerd voor het activeren van de logische App van Azure wanneer de budgetdrempel wordt voldaan.
- Over het maken van het budget van de Azure met de gewenste drempelwaarden en wire-aan de actiegroep.

U hebt nu een volledig functionele budget voor uw abonnement wordt afgesloten omlaag uw virtuele machines wanneer u uw geconfigureerde budgetdrempels bereikt. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure-facturering scenario's, [facturerings- en beheerscenario's automatiseren kosten](billing-cost-management-automation-scenarios.md).
