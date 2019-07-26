---
title: Azure billing and cost management-budget scenario | Microsoft Docs
description: Meer informatie over het gebruik van Azure Automation om Vm's af te sluiten op basis van specifieke budget drempels.
services: billing
documentationcenter: ''
author: bandersmsft
manager: dougeby
editor: ''
tags: billing
ms.assetid: db93f546-6b56-4b51-960d-1a5bf0274fc8
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 03/13/2019
ms.author: banders
ms.openlocfilehash: 37f129526cb184a2eeee9e36028e8f00b5bbc247
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443463"
---
# <a name="manage-costs-with-azure-budgets"></a>Kosten beheren met Azure Budgets

Cost Control is een essentieel onderdeel voor het maximaliseren van de waarde van uw investering in de Cloud. Er zijn verschillende scenario's waarin de berekenings-, rapportage-en kostengebaseerde indeling van de kosten van belang is voor voortgezet bedrijfs activiteiten. [Azure Cost Management-api's](https://docs.microsoft.com/rest/api/consumption/) bieden een set api's ter ondersteuning van elk van deze scenario's. De Api's bieden gebruiks gegevens, zodat u gedetailleerde kosten voor het exemplaar niveau kunt weer geven.

Budgetten worden vaak gebruikt als onderdeel van kosten beheer. Budgetten kunnen binnen het bereik van Azure worden ingedeeld. U kunt bijvoorbeeld uw budget weergave beperken op basis van het abonnement, de resource groepen of een verzameling resources. Naast het gebruik van de budget-API om u per e-mail op de hoogte te stellen wanneer een budget drempel is bereikt, kunt u [Azure monitor actie groepen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) gebruiken om een gegroepeerde reeks acties te activeren als gevolg van een budget gebeurtenis.

Een veelvoorkomend budget scenario voor een klant die een niet-kritieke werk belasting uitvoert, kan zich voordoen wanneer ze willen beheren op basis van een budget en ook een voorspel bare prijs krijgen bij het bekijken van de maandelijkse factuur. Dit scenario vereist enkele op kosten gebaseerde indeling van resources die deel uitmaken van de Azure-omgeving. In dit scenario wordt een maandelijks budget van $1000 voor het abonnement ingesteld. Daarnaast worden meldings drempels ingesteld om enkele indelingen te activeren. Dit scenario begint met een kosten drempel van 80%, waardoor alle virtuele machines in de resource groep **optioneel**worden gestopt. Op de kosten drempel van 100% worden alle VM-exemplaren gestopt.
Als u dit scenario wilt configureren, voert u de volgende acties uit door de stappen in elke sectie van deze zelf studie te volgen.

Met deze acties die zijn opgenomen in deze zelf studie kunt u:

- Maak een Azure Automation Runbook om Vm's te stoppen met behulp van webhooks.
- Maak een Azure Logic-app die moet worden geactiveerd op basis van de waarde van de budget drempel en roep het runbook aan met de juiste para meters.
- Maak een Azure Monitor actie groep die wordt geconfigureerd om de Azure Logic-app te activeren wanneer aan de budget drempel wordt voldaan.
- Maak het Azure-budget met de gewenste drempel waarden en draad het aan de actie groep.

## <a name="create-an-azure-automation-runbook"></a>Een Azure Automation Runbook maken

[Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) is een service waarmee u het meren deel van uw resource beheer taken kunt uitvoeren en die taken als gepland of op aanvraag uit te voeren. Als onderdeel van dit scenario maakt u een [Azure Automation runbook](https://docs.microsoft.com/azure/automation/automation-runbook-types) dat wordt gebruikt om vm's te stoppen. U gebruikt de [Azure v2 vm's](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) van de virtuele machine stoppen uit de [Galerie](https://docs.microsoft.com/azure/automation/automation-runbook-gallery) om dit scenario te maken. Als u dit runbook in uw Azure-account importeert en publiceert, kunt u Vm's stoppen wanneer een budget drempelwaarde wordt bereikt.

### <a name="create-an-azure-automation-account"></a>Een Azure Automation-account maken

1. Gebruik de referenties van uw Azure-account om u aan melden bij het [Azure Portal](https://portal.azure.com/).
2. Klik op de knop **een resource maken** in de linkerbovenhoek van Azure.
3. Selecteer**automatisering**van **beheer hulpprogramma's** > .
   > [!NOTE]
   > Als u geen Azure-account hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)maken.
4. Voer uw account gegevens in. Als u een **uitvoeren als-account voor Azure wilt maken**, kiest u **Ja** om automatisch de instellingen in te scha kelen die nodig zijn om verificatie naar Azure te vereenvoudigen.
5. Als u daarmee klaar bent, klikt u op **Maken** om met de implementatie van het Automation-account te beginnen.

### <a name="import-the-stop-azure-v2-vms-runbook"></a>Importeer het runbook Azure v2 Vm's stoppen

Importeer met behulp van een [Azure Automation runbook](https://docs.microsoft.com/azure/automation/automation-runbook-types)het grafische Runbook van [Azure v2 vm's stoppen](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) vanuit de galerie.

1.  Gebruik de referenties van uw Azure-account om u aan melden bij het [Azure Portal](https://portal.azure.com/).
2.  Open uw Automation-account door **alle services** > Automation-**accounts**te selecteren. Selecteer vervolgens uw Automation-account.
3.  Klik in de sectie **proces automatisering** op **Runbooks galerie** .
4.  Stel de **Galerie bron** in op **script centrum** en selecteer **OK**.
5.  Zoek en selecteer het item [Azure v2 vm's galerie stoppen](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) binnen het Azure Portal.
6.  Klik op de knop **importeren** om de Blade **importeren** weer te geven en selecteer **OK**. De Blade runbook Overview wordt weer gegeven.
7.  Zodra het proces is voltooid, selecteert u **bewerken** om de grafische runbook-editor en de publicatie optie weer te geven.

    ![Azure-grafisch runbook bewerken](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-01.png)
8.  Klik op de knop **publiceren** om het runbook te publiceren en selecteer vervolgens **Ja** wanneer u hierom wordt gevraagd. Wanneer u een runbook publiceert, overschrijft u de bestaande gepubliceerde versie met de concept versie. In dit geval hebt u geen gepubliceerde versie omdat u het runbook hebt gemaakt.

    Zie [een grafisch Runbook maken](https://docs.microsoft.com/azure/automation/automation-first-runbook-graphical)voor meer informatie over het publiceren van een runbook.

## <a name="create-webhooks-for-the-runbook"></a>Webhooks maken voor het runbook

Met behulp van het [Azure v2 vm's](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafisch runbook stoppen maakt u twee webhooks om het runbook in azure Automation te starten via één HTTP-aanvraag. De eerste webhook roept het runbook aan met een budget drempel van 80% met de naam van de resource groep als een para meter, waardoor de optionele Vm's kunnen worden gestopt. Vervolgens roept de tweede webhook het runbook zonder para meters aan (om 100%), waardoor alle resterende VM-exemplaren worden gestopt.

1. Klik op de pagina **Runbooks** in de [Azure Portal](https://portal.azure.com/)op het runbook van de **StopAzureV2Vm** waarin de Blade overzicht van het runbook wordt weer gegeven.
2. Klik  boven aan de pagina op webhook om de Blade **webhook toevoegen** te openen.
3. Klik op **nieuwe webhook maken** om de Blade **nieuwe webhook maken** te openen.
4. Stel de **naam** van de webhook in op **optioneel**. De eigenschap **enabled** moet **Ja**zijn. De  verlooptde waarde hoeft niet te worden gewijzigd. Meer informatie over eigenschappen van webhooks vindt u [in de details van een webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#details-of-a-webhook).
5. Klik naast de URL-waarde op het pictogram kopiëren om de URL van de webhook te kopiëren.
   > [!IMPORTANT]
   > Sla de URL van de webhook op met de naam **optioneel** op een veilige plaats. U gebruikt de URL verderop in deze zelf studie. Uit veiligheids overwegingen kunt u de URL na het maken van de webhook niet opnieuw weer geven of ophalen.
6. Klik op **OK** om de nieuwe webhook te maken.
7. Klik op **para meters configureren en voer instellingen uit** om parameter waarden voor het runbook weer te geven.
   > [!NOTE]
   > Als het runbook verplichte para meters bevat, kunt u de webhook pas maken als er waarden zijn opgegeven.
8. Klik op **OK** om de waarden voor de webhook-para meter te accepteren.
9. Klik op **maken** om de webhook te maken.
10. Volg vervolgens de bovenstaande stappen om een tweede webhook met de naam **voltooid**te maken.
    > [!IMPORTANT]
    > Zorg ervoor dat u beide webhook-Url's opslaat voor gebruik verderop in deze zelf studie. Uit veiligheids overwegingen kunt u de URL na het maken van de webhook niet opnieuw weer geven of ophalen.

U hebt nu twee geconfigureerde webhooks die elk beschikbaar zijn met behulp van de Url's die u hebt opgeslagen.

![Webhooks-optioneel en volledig](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-02.png)

U bent nu klaar met de Azure Automation Setup. U kunt de webhooks testen met een eenvoudige postman-test om te controleren of de webhook werkt. Vervolgens moet u de logische app voor indeling maken.

## <a name="create-an-azure-logic-app-for-orchestration"></a>Een Azure Logic-app maken voor indeling

Logic Apps u helpen bij het bouwen, plannen en automatiseren van processen als werk stromen, zodat u apps, gegevens, systemen en services in ondernemingen of organisaties kunt integreren. In dit scenario gaat de [logische app](https://docs.microsoft.com/azure/logic-apps/) die u maakt iets meer doen dan alleen de Automation-webhook aanroepen die u hebt gemaakt.

Budgetten kunnen worden ingesteld om een melding te activeren wanneer aan een opgegeven drempel wordt voldaan. U kunt meerdere drempel waarden opgeven om op de hoogte te worden gesteld en de logische app laat zien hoe u verschillende acties uitvoert op basis van de drempel waarde. In dit voor beeld gaat u een scenario instellen waarin u een aantal meldingen krijgt, de eerste melding is voor wanneer 80% van het budget is bereikt en de tweede melding wanneer 100% van het budget is bereikt. De logische app wordt gebruikt om alle virtuele machines in de resource groep af te sluiten. Ten eerste wordt de **optionele** drempel waarde bereikt om 80%. vervolgens wordt de tweede drempel waarde bereikt waarbij alle virtuele machines in het abonnement worden afgesloten.

Met Logic apps kunt u een voorbeeld schema voor de HTTP-trigger opgeven, maar moet u de **Content-type-** header instellen. Omdat de actie groep geen aangepaste headers voor de webhook heeft, moet u de payload in een afzonderlijke stap parseren. U gebruikt de actie **parseren** en geeft deze een voor beeld-nettolading.

### <a name="create-the-logic-app"></a>De logische app maken

Met de logische app worden verschillende acties uitgevoerd. De volgende lijst bevat een reeks acties op hoog niveau die door de logische app worden uitgevoerd:
- Herkent wanneer een HTTP-aanvraag wordt ontvangen
- De door gegeven JSON-gegevens parseren om te bepalen welke drempel waarde is bereikt
- Gebruik een voorwaardelijke instructie om te controleren of het drempel bedrag 80% of meer van het budget bereik heeft bereikt, maar niet groter dan of gelijk aan 100%.
    - Als deze drempel waarde is bereikt, verzendt u een HTTP POST met de webhook met de naam **optioneel**. Met deze actie worden de virtuele machines in de groep ' optioneel ' afgesloten.
- Gebruik een voorwaardelijke instructie om te controleren of het drempel bedrag 100% van de budget waarde heeft bereikt of overschreden.
    - Als de drempel waarde is bereikt, verzendt u een HTTP POST met de webhook met de naam **volledig**. Met deze actie worden alle resterende Vm's afgesloten.

De volgende stappen zijn nodig om de logische app te maken waarmee de bovenstaande stappen worden uitgevoerd:

1.  Selecteer in [Azure Portal](https://portal.azure.com/) **Een resource maken** > **Integratie** > **Logische app**.

    ![Azure: de resource van de logische app selecteren](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03.png)
2.  Geef op de Blade **logische app maken** de details op die u nodig hebt om uw logische app te maken, selecteer vastmaken **aan dash board**en klik op **maken**.

    ![Azure: een logische app maken](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03a.png)

Nadat Azure uw logische app heeft geïmplementeerd, wordt de **Logic apps Designer** geopend en ziet u een Blade met een introductie video en veelgebruikte triggers.

### <a name="add-a-trigger"></a>Een trigger toevoegen

Elke logische app moet beginnen met een trigger, die wordt geactiveerd wanneer er een bepaalde gebeurtenis plaatsvindt of wanneer er aan een bepaalde voorwaarde is voldaan. Telkens wanneer de trigger wordt geactiveerd, maakt de Logic Apps-engine een exemplaar van een logische app dat wordt gestart en de werkstroom uitvoert. acties zijn alle stappen die na de trigger plaatsvinden.

1.  Kies onder **sjablonen** van de blade **Logic apps Designer** de optie **lege logische app**.
2.  Voeg een [trigger](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts) toe door HTTP-aanvraag in te voeren in het zoekvak van **Logic apps Designer** om de trigger met de naam Request te zoeken en te selecteren, **wanneer er een HTTP-aanvraag wordt ontvangen**.

    ![Azure-Logic-app-http-trigger](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-04.png)
3.  Selecteer **nieuwe stap** > **een actie toevoegen**.

    ![Azure-nieuwe stap-een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-05.png)
4.  Zoek naar ' JSON parseren ' in het zoekvak van **Logic apps Designer** om de [actie](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts) **gegevens bewerkingen-JSON parseren** te zoeken en te selecteren.

    ![Azure-Logic-app-JSON-actie parseren toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-06.png)
5.  Voer ' payload ' in als de naam van de **inhoud** voor de JSON-nettolading van de parser of gebruik de code ' Body ' van dynamische inhoud.
6.  Selecteer de optie **voor beeld van nettolading gebruiken om een schema te genereren** in het vak **JSON parseren** .

    ![Azure-Logic-app-voor beeld-JSON-gegevens gebruiken voor het genereren van een schema](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-07.png)
7.  Plak de volgende JSON-voorbeeld lading in het tekstvak:`{"schemaId":"AIP Budget Notification","data":{"SubscriptionName":"CCM - Microsoft Azure Enterprise - 1","SubscriptionId":"<GUID>","SpendingAmount":"100","BudgetStartDate":"6/1/2018","Budget":"50","Unit":"USD","BudgetCreator":"email@contoso.com","BudgetName":"BudgetName","BudgetType":"Cost","ResourceGroup":"","NotificationThresholdAmount":"0.8"}}`

    Het tekstvak wordt als volgt weer gegeven:

    ![Azure-Logic-app: de voor beeld-JSON-nettolading](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-08.png)
8.  Klik op **Gereed**.

### <a name="add-the-first-conditional-action"></a>De eerste voorwaardelijke actie toevoegen

Gebruik een voorwaardelijke instructie om te controleren of het drempel bedrag 80% of meer van het budget bereik heeft bereikt, maar niet groter dan of gelijk aan 100%. Als deze drempel waarde is bereikt, verzendt u een HTTP POST met de webhook met de naam **optioneel**. Met deze actie worden de Vm's in de **optionele** groep afgesloten.

1.  Selecteer **nieuwe stap** > **een voor waarde toevoegen**.

    ![Azure-Logic-app-een voor waarde toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-09.png)
2.  Klik in het vak **voor waarde** op het tekstvak met **Kies een waarde** om een lijst met beschik bare waarden weer te geven.

    ![Het vak Azure-Logic-app-voor waarde](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-10.png)

3.  Klik boven aan de lijst op **expressie** en voer de volgende expressie in in de expressie-editor:`float()`

    ![Azure-Logic-app-float-expressie](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-11.png)

4.  Selecteer **dynamische inhoud**, plaats de cursor tussen de haakjes () en selecteer **NotificationThresholdAmount** in de lijst om de volledige expressie in te vullen.

    De expressie ziet er als volgt uit:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`

5.  Selecteer **OK** om de expressie in te stellen.
6.  Select **is groter dan of gelijk aan** in de vervolg keuzelijst van de **voor waarde**.
7.  In het vak **Kies een waarde** van de voor waarde `.8`voert u in.

    ![Azure-Logic app-float-expressie met een waarde](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-12.png)

8.  Klik op**rij toevoegen** in het vak voor waarde om een aanvullend deel van de voor waarde toe te voegen.  > 
9.  Klik in het vak **voor waarde** op het tekstvak met **een waarde kiezen**.
10. Klik boven aan de lijst op **expressie** en voer de volgende expressie in in de expressie-editor:`float()`
11. Selecteer **dynamische inhoud**, plaats de cursor tussen de haakjes () en selecteer **NotificationThresholdAmount** in de lijst om de volledige expressie in te vullen.
12. Selecteer **OK** om de expressie in te stellen.
13. Select **is kleiner dan** in de vervolg keuzelijst van de **voor waarde**.
14. In het vak **Kies een waarde** van de voor waarde `1`voert u in.

    ![Azure-Logic app-float-expressie met een waarde](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-13.png)

15. Selecteer **een actie toevoegen**in het vak **Indien waar** . U voegt een HTTP POST-actie toe waarmee optionele Vm's worden afgesloten.

    ![Azure-Logic-app-een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-14.png)

16. Voer **http** in om te zoeken naar de http-actie en selecteer de **http-http-** actie.

    ![Azure-Logic-app-HTTP-actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-15.png)

17. Selecteer **bericht** als de waarde voor de **methode** .
18. Voer de URL in voor de webhook met de naam **optioneel** die u eerder in deze zelf studie hebt gemaakt als de **URI** -waarde.

    ![Azure-Logic-app-HTTP-actie-URI](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-16.png)

19. Selecteer **een actie toevoegen** in het vak **Indien waar** . U voegt een e-mail actie toe waarmee een e-mail bericht wordt verzonden dat de ontvanger de optionele Vm's heeft afgesloten.
20. Zoek naar ' e-mail verzenden ' en selecteer een actie voor het *verzenden van e-mail* op basis van de e-mail service die u gebruikt.

    ![Azure-Logic-app-e-mail actie verzenden](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-17.png)

    Selecteer **Outlook.com** voor persoonlijke Microsoft-accounts. Selecteer **Office 365 Outlook** voor werk- of schoolaccounts van Azure. Als u nog geen verbinding hebt, wordt u gevraagd om u aan te melden bij uw e-mailaccount. Logic Apps maakt een verbinding met uw e-mailaccount.

    U moet de logische app toestaan om toegang te krijgen tot uw e-mail gegevens.

    ![Azure-Logic app-toegangs kennisgeving](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-18.png)

21. Voeg de tekst **toe aan**de sectie aan, **onderwerp**en **hoofd** voor het e-mail bericht waarmee de ontvanger wordt gewaarschuwd dat de optionele vm's zijn afgesloten. Gebruik de **budget** -en **NotificationThresholdAmount** om de velden onderwerp en hoofd tekst in te vullen.

    ![Azure-Logic-app-e-mail gegevens](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-19.png)

### <a name="add-the-second-conditional-action"></a>De tweede voorwaardelijke actie toevoegen

Gebruik een voorwaardelijke instructie om te controleren of het drempel bedrag 100% van de budget waarde heeft bereikt of overschreden. Als de drempel waarde is bereikt, verzendt u een HTTP POST met de webhook met de naam **volledig**. Met deze actie worden alle resterende Vm's afgesloten.

1.  Selecteer **nieuwe stap** > **een voor waarde toevoegen**.

    ![Azure-Logic-app-actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-20.png)

2.  Klik in het vak **voor waarde** op het tekstvak met **Kies een waarde** om een lijst met beschik bare waarden weer te geven.
3.  Klik boven aan de lijst op **expressie** en voer de volgende expressie in in de expressie-editor:`float()`
4.  Selecteer **dynamische inhoud**, plaats de cursor tussen de haakjes () en selecteer **NotificationThresholdAmount** in de lijst om de volledige expressie in te vullen.

    De expressie ziet er als volgt uit:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`

5.  Selecteer **OK** om de expressie in te stellen.
6.  Select **is groter dan of gelijk aan** in de vervolg keuzelijst van de **voor waarde**.
7.  In het **vak Kies een waarde** van de voor waarde `1`voert u in.

    ![Azure-Logic app-waarde voor waarde instellen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-21.png)

8.  Selecteer **een actie toevoegen**in het vak **Indien waar** . U voegt een HTTP POST-actie toe waarmee alle resterende Vm's worden afgesloten.

    ![Azure-Logic-app-een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-22.png)

9.  Voer **http** in om te zoeken naar de http-actie en selecteer de **http-http-** actie.
10. Selecteer **bericht** als de waarde voor de **methode** .
11. Voer de URL voor de webhook met de naam **volledig** in die u eerder in deze zelf studie hebt gemaakt als de **URI** -waarde.

    ![Azure-Logic-app-een actie toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-23.png)

12. Selecteer **een actie toevoegen** in het vak **Indien waar** . U voegt een e-mail actie toe waarmee een e-mail wordt verzonden met de ontvanger dat de resterende Vm's zijn afgesloten.
13. Zoek naar ' e-mail verzenden ' en selecteer een actie voor het *verzenden van e-mail* op basis van de e-mail service die u gebruikt.
14. Voeg de tekst **toe aan**de sectie aan, **onderwerp**en **hoofd** voor het e-mail bericht waarmee de ontvanger wordt gewaarschuwd dat de optionele vm's zijn afgesloten. Gebruik de **budget** -en **NotificationThresholdAmount** om de velden onderwerp en hoofd tekst in te vullen.

    ![Azure-Logic-app-e-mail gegevens verzenden](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-24.png)

15. Klik op **Opslaan** boven aan de Blade **Logic app Designer** .

### <a name="logic-app-summary"></a>Overzicht van logische apps

Hier ziet uw logische app eruit als u klaar bent. In de meeste scenario's waarbij u geen indeling op basis van drempel hebt, kunt u het Automation-script rechtstreeks aanroepen vanuit de **monitor** en de stap van de **logische app** overs Laan.

   ![Azure-Logic-app-volledige weer gave](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-25.png)

Wanneer u de logische app hebt opgeslagen, wordt er een URL gegenereerd die u kunt aanroepen. U gebruikt deze URL in de volgende sectie van deze zelf studie.

## <a name="create-an-azure-monitor-action-group"></a>Een Azure Monitor actie groep maken

Een actie groep is een verzameling voor keuren voor meldingen die u definieert. Wanneer een waarschuwing wordt geactiveerd, kan een specifieke actie groep de waarschuwing ontvangen door te worden gewaarschuwd. Een Azure-waarschuwing genereert proactief een melding op basis van specifieke voor waarden en biedt de mogelijkheid om actie te ondernemen. Een waarschuwing kan gegevens uit meerdere bronnen gebruiken, met inbegrip van metrieken en Logboeken.

Actie groepen zijn het enige eind punt dat u wilt integreren met uw budget. U kunt meldingen instellen in een aantal kanalen, maar voor dit scenario gaat u de focus op de logische app die u eerder in deze zelf studie hebt gemaakt.

### <a name="create-an-action-group-in-azure-monitor"></a>Een actie groep maken in Azure Monitor

Wanneer u de actie groep maakt, gaat u naar de logische app die u eerder in deze zelf studie hebt gemaakt.

1.  Als u nog niet bent aangemeld bij de [Azure Portal](https://portal.azure.com/), meldt u zich aan en selecteert u **alle services** > **monitor**.
2.  Selecteer **acties groepen** in de sectie **instelling** .
3.  Selecteer **een actie groep toevoegen** op de Blade **actie groepen** .
4.  Voeg de volgende items toe en controleer deze:
    - Naam van de actiegroep
    - Korte naam
    - Subscription
    - Resource group

    ![Azure-Logic-app-een actie groep toevoegen](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-26.png)

5.  Voeg in het deel venster **actie groep toevoegen** een LogicApp-actie toe. Noem het actie **budget-BudgetLA**. Selecteer in het deel venster **logische app** het **abonnement** en de **resource groep**. Selecteer vervolgens de **logische app** die u eerder in deze zelf studie hebt gemaakt.
6.  Klik op **OK** om de logische app in te stellen. Selecteer vervolgens **OK** in het deel venster **actie groep toevoegen** om de actie groep te maken.

U bent klaar met alle ondersteunende onderdelen die nodig zijn om uw budget effectief te organiseren. Nu hoeft u alleen maar het budget te maken en te configureren voor het gebruik van de actie groep die u hebt gemaakt.

## <a name="create-the-azure-budget"></a>Het Azure-budget maken

U kunt een budget maken in de Azure Portal met behulp van de [functie budget](../cost-management/tutorial-acm-create-budgets.md) in cost management. U kunt ook een budget maken met behulp van REST Api's, Power shell-cmdlets of de CLI gebruiken. De volgende procedure maakt gebruik van de REST API. Voordat u de REST API aanroept, hebt u een autorisatie token nodig. U kunt het [ARMClient](https://github.com/projectkudu/ARMClient) -project gebruiken om een autorisatie token te maken. Met de **ARMClient** kunt u zichzelf verifiëren bij de Azure Resource Manager en een token ontvangen om de api's aan te roepen.

### <a name="create-an-authentication-token"></a>Een verificatie token maken

1.  Navigeer naar het [ARMClient](https://github.com/projectkudu/ARMClient) -project op github.
2.  Kloon de opslag plaats om een lokale kopie op te halen.
3.  Open het project in Visual Studio en bouw het.
4.  Zodra de build is voltooid, moet het uitvoer bare bestand zich in de map *\bin\debug* bevindt.
5.  Voer de ARMClient uit. Open een opdracht prompt en ga naar de map *\bin\debug* van de hoofdmap van het project.
6.  Voer bij de opdracht prompt de volgende opdracht in om u aan te melden en te verifiëren:<br>
    `ARMClient login prod`
7.  Kopieer de **GUID** van het abonnement uit de uitvoer.
8.  Als u een autorisatie token naar het klem bord wilt kopiëren, typt u de volgende opdracht bij de opdracht prompt, maar u moet de gekopieerde abonnements-ID gebruiken uit de bovenstaande stap: <br>
    `ARMClient token <subscription GUID from previous step>`

    Nadat u de bovenstaande stap hebt voltooid, ziet u het volgende:<br>
    **Het token is naar het klem bord gekopieerd.**
9.  Sla het token op dat moet worden gebruikt voor de stappen in de volgende sectie van deze zelf studie.

### <a name="create-the-budget"></a>Het budget maken

Vervolgens configureert u postman  om een budget te maken door de rest api's van het Azure-verbruik aan te roepen. Postman is een API-ontwikkel omgeving. U kunt omgevings-en verzamelings bestanden importeren in een bericht. De verzameling bevat gegroepeerde definities van HTTP-aanvragen die Azure-verbruiks REST-Api's aanroepen. Het omgevings bestand bevat variabelen die door de verzameling worden gebruikt.

1.  Down load en open de [postman rest-client](https://www.getpostman.com/) om de rest-api's uit te voeren.
2.  Maak in postman een nieuwe aanvraag.

    ![Postman: een nieuwe aanvraag maken](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-27.png)

3.  Sla de nieuwe aanvraag op als een verzameling, zodat de nieuwe aanvraag er niets op heeft.

    ![Postman: de nieuwe aanvraag opslaan](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-28.png)

4.  Wijzig de aanvraag van een `Get` in een `Put` actie.
5.  Wijzig de volgende URL door te `{subscriptionId}` vervangen door de **abonnements-id** die u in het vorige gedeelte van deze zelf studie hebt gebruikt. Wijzig ook de URL zodanig dat "SampleBudget" als waarde voor `{budgetName}`:`https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2018-03-31`
6.  Selecteer het tabblad **headers** in het veld postman.
7.  Voeg een nieuwe **sleutel** met de naam autorisatie toe.
8.  Stel de **waarde** in op het token dat is gemaakt met behulp van de ArmClient aan het einde van de laatste sectie.
9.  Selecteer het tabblad **hoofd tekst** in het bericht.
10. Selecteer de  optie onbewerkte knop.
11. Plak in het tekstvak onder voor beeld van een budget definitie, maar u moet de para meters **subscriptionid**, **budget**naam en **actiongroupname** vervangen door uw abonnements-id, een unieke namen voor uw budget en de naam van de actie groep u hebt gemaakt in de URL en de hoofd tekst van de aanvraag:

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

U hebt nu alle onderdelen die u nodig hebt om de [budgetten-API](https://docs.microsoft.com/rest/api/consumption/budgets)aan te roepen. De API-verwijzing voor budgetten bevat extra informatie over de specifieke aanvragen, waaronder de volgende:
    - **budgetnaam** -meerdere budgetten worden ondersteund.  Budget namen moeten uniek zijn.
    - **categorie** -moet de **kosten** of het **gebruik**zijn. De API ondersteunt zowel kosten als voor het gebruik van budgetten.
    - **timeGrain** : een maandelijks, driemaandelijks of jaarlijks budget. Het aantal wordt opnieuw ingesteld aan het einde van de periode.
    - **filters** -filters bieden u de mogelijkheid om het budget te beperken tot een specifieke set resources binnen de geselecteerde scope. Een filter kan bijvoorbeeld een verzameling Resource groepen zijn voor een budget op abonnements niveau.
    - **meldingen** : bepaalt de meldings Details en drempel waarden. U kunt meerdere drempel waarden instellen en een e-mail adres of actie groep opgeven om een melding te ontvangen.

## <a name="summary"></a>Samenvatting

Door deze zelf studie te volgen, hebt u het volgende geleerd:
- Een Azure Automation Runbook maken om Vm's te stoppen.
- Een Azure Logic-app maken die wordt geactiveerd op basis van de budget drempel waarden en het gerelateerde runbook aanroepen met de juiste para meters.
- Een Azure Monitor actie groep maken die is geconfigureerd om de Azure Logic-app te activeren wanneer aan de budget drempel wordt voldaan.
- Het Azure-budget maken met de gewenste drempel waarden en deze aan de actie groep draad.

U hebt nu een volledig functioneel budget voor uw abonnement waarmee uw Vm's worden afgesloten wanneer u uw geconfigureerde budget drempels bereikt.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure-facturerings scenario's [Facturering en kosten beheer automatiserings scenario's](billing-cost-management-automation-scenarios.md).
