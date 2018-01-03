---
title: Virtuele machines starten/stoppen tijdens rustige uren oplossing (preview) | Microsoft Docs
description: Deze oplossing voor het beheer van virtuele machine wordt gestart en uw virtuele machines van Azure Resource Manager stopt volgens een schema en proactief bewaakt van logboekanalyse.
services: automation
documentationCenter: 
authors: eslesar
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: magoedte
ms.openlocfilehash: 4424cbb83bdb31c60e15d62f9387b4050611a98d
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="startstop-vms-during-off-hours-solution-preview-in-azure-automation"></a>Virtuele machines starten/stoppen tijdens rustige uren oplossing (preview) in Azure Automation

De starten/stoppen virtuele machines buiten kantooruren oplossing wordt gestart en uw virtuele Azure-machines op de gebruiker gedefinieerde schema's gestopt insights via Azure Log Analytics en biedt optionele e-mailberichten worden verzonden via [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Het ondersteunt zowel Azure Resource Manager en classic VM's voor de meeste scenario's. 

Deze oplossing biedt een automatiseringsoptie gedecentraliseerde voor gebruikers die hun kosten verminderen met behulp van bronnen in zonder server, lage kosten. Met deze oplossing kunt u het volgende doen:

* Plannen van virtuele machines starten en stoppen.
* Plannen van virtuele machines starten en stoppen in oplopende volgorde met behulp van Azure-codes (wordt niet ondersteund voor klassieke virtuele machines).
* Automatische stop VM's op basis van het lage CPU-gebruik.

## <a name="prerequisites"></a>Vereisten

- De runbooks werken met een [Uitvoeren als-account voor Azure](automation-offering-get-started.md#authentication-methods).  De Run As-account is de gewenste verificatiemethode omdat deze wordt certificaatverificatie gebruikt in plaats van een wachtwoord dat mogelijk verlopen of regelmatig wordt gewijzigd.  

- Deze oplossing beheert alleen virtuele machines die zich in hetzelfde abonnement als uw Azure Automation-account.  

- Deze oplossing is geïmplementeerd alleen voor de volgende Azure-regio's: Australië-Zuidoost, Canada centraal, India centraal, VS-Oost, Japan-Oost, Zuidoost-Azië, VK Zuid en West-Europa.  
    
    > [!NOTE]
    > De runbooks voor het beheren van de planning van de VM kan richten op virtuele machines in elke regio.  

- Selecteer voor het verzenden van e-mailmeldingen wanneer de VM-runbooks starten en stoppen tijdens de voorbereiding van Azure Marketplace voltooien **Ja** SendGrid implementeren. 

    > [!IMPORTANT]
    > SendGrid is een service van derden. Voor ondersteuning contact op met [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Beperkingen als sendgrid zijn:

    * Maximaal één SendGrid-account per gebruiker per abonnement.
    * Maximaal twee SendGrid-accounts per abonnement.

Als u een eerdere versie van deze oplossing hebt geïmplementeerd, moet u deze eerst verwijderen uit uw account voordat u implementeert deze release.  

## <a name="solution-components"></a>Oplossingsonderdelen

Deze oplossing bevat vooraf geconfigureerde runbooks, schema's en integratie met Log Analytics, zodat u kunt het opstarten en afsluiten van uw virtuele machines naar de behoeften van uw bedrijf aanpassen. 

### <a name="runbooks"></a>Runbooks

De volgende tabel bevat de runbooks die zijn geïmplementeerd voor uw Automation-account.  U moet geen wijzigingen aanbrengen aan de runbook-code. Schrijven in plaats hiervan uw eigen runbook voor nieuwe functionaliteit.

> [!IMPORTANT]
> Voer niet direct uit een willekeurig runbook met 'onderliggende' toegevoegd aan de naam.
>

Alle bovenliggende runbooks bevatten de *WhatIf* parameter. Als de waarde **True**, *WhatIf* met gedetailleerde informatie over het exacte gedrag ondersteunt het runbook wordt uitgevoerd wanneer uitgevoerd zonder de *WhatIf* parameter en valideert de juiste virtuele machines worden gericht.  Een runbook worden alleen de gedefinieerde acties uitgevoerd wanneer de *WhatIf* parameter is ingesteld op **False**. 

|**Runbook** | **Parameters** | **Beschrijving**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Alleen aangeroepen vanuit het bovenliggende runbook. Waarschuwingen maakt op basis van per resource voor AutoStop scenario.| 
|AutoStop_CreateAlert_Parent | WhatIf: True of False <br> VMList | Maken of bijwerken van Azure waarschuwingsregels op virtuele machines in de doelgroepen abonnement of resourcegroep. <br> VMList: Door komma's gescheiden lijst met virtuele machines.  Bijvoorbeeld: *vm1 vm2, vm3*.| 
|AutoStop_Disable | geen | Hiermee schakelt u AutoStop waarschuwingen en standaardschema.| 
|AutoStop_StopVM_Child | WebHookData | Alleen aangeroepen vanuit het bovenliggende runbook. Waarschuwingsregels aanroepen voor dit runbook om te stoppen van de virtuele machine.|  
|Bootstrap_Main | geen | Één keer worden gebruikt voor het instellen van de bootstrap configuraties zoals webhookURI, die doorgaans niet toegankelijk vanuit Azure Resource Manager. Dit runbook wordt automatisch verwijderd na de geslaagde implementatie.|  
|ScheduledStartStop_Child | VMName <br> Actie: Stoppen of starten <br> resourceGroupName | Alleen aangeroepen vanuit het bovenliggende runbook. Wordt uitgevoerd of het stoppen of starten voor de geplande stoppen.|  
|ScheduledStartStop_Parent | Actie: Stoppen of starten <br> WhatIf: True of False | Dit geldt voor alle VM's in het abonnement. Bewerk de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames** alleen uitvoeren op deze brongroepen doel. U kunt ook specifieke virtuele machines uitsluiten door het bijwerken van de **External_ExcludeVMNames** variabele. *WhatIf* gedraagt zich hetzelfde als in andere runbooks.|  
|SequencedStartStop_Parent | Actie: Stoppen of starten <br> WhatIf: True of False | Maken van labels met de naam **klikvolgorde** en **SequenceStop** op elke virtuele machine die u starten/stoppen-sequence-activiteit wilt. De waarde van de tag moet een positief geheel getal (1, 2, 3) die overeenkomt met de volgorde waarin u wilt starten of stoppen. *WhatIf* gedraagt zich hetzelfde als in andere runbooks. <br> **Opmerking**: virtuele machines moet binnen de resourcegroepen die zijn gedefinieerd als External_Start_ResourceGroupNames External_Stop_ResourceGroupNames en External_ExcludeVMNames in Azure Automation-variabelen. Ze hebben de bijbehorende codes voor acties om te laten treden.|

### <a name="variables"></a>Variabelen

De volgende tabel bevat de variabelen die in uw Automation-account gemaakt.  Wijzig variabelen die worden voorafgegaan door alleen **externe**. Wijzigen van variabelen voorafgegaan door **intern** bijwerkingen veroorzaakt.  

|**Variabele** | **Beschrijving**|
---------|------------|
|External_AutoStop_Condition | De voorwaardelijke operator zijn vereist voor het configureren van de voorwaarde voordat een waarschuwing. Acceptabele waarden zijn **groter dan**, **GreaterThanOrEqual**, **LessThan**, en **LessThanOrEqual**.|  
|External_AutoStop_Description | De waarschuwing voor de virtuele machine stoppen als het CPU-percentage de drempelwaarde overschrijdt.|  
|External_AutoStop_MetricName | De naam van de prestaties metriek waarvoor Azure waarschuwingsregel is geconfigureerd.| 
|External_AutoStop_Threshold | De drempelwaarde voor de Azure waarschuwingsregel opgegeven in de variabele *External_AutoStop_MetricName*. Percentagewaarden kunnen variëren van 1 tot 100.|  
|External_AutoStop_TimeAggregationOperator | De tijd aggregatie-operator die wordt toegepast op de grootte van het geselecteerde venster om de voorwaarde te evalueren. Acceptabele waarden zijn **gemiddelde**, **Minimum**, **maximale**, **totale**, en **laatste**.|  
|External_AutoStop_TimeWindow | De grootte van het venster waarin Azure geselecteerde metrische gegevens voor het genereren van een signaal analyseert. Deze parameter accepteert invoer in de notatie timespan. Mogelijke waarden liggen tussen 5 minuten en 6 uur.|  
|External_EmailFromAddress | Hiermee geeft u de afzender van het e-mailbericht.|  
|External_EmailSubject | Hiermee wordt de tekst voor de onderwerpregel van het e-mailbericht opgegeven.|  
|External_EmailToAddress | Hiermee geeft u de ontvangers van het e-mailbericht. Scheid namen met een komma.|  
|External_ExcludeVMNames | VM-namen moeten worden uitgesloten, Voer namen scheiden met een komma zonder spaties.|  
|External_IsSendEmail | Hiermee geeft u de optie voor het verzenden van e-mailmelding na voltooiing.  Geef **Ja** of **Nee** niet om e-mail te verzenden.  Deze optie moet **Nee** als u e-mailmeldingen niet hebt ingeschakeld tijdens de initiële implementatie.|  
|External_Start_ResourceGroupNames | Hiermee geeft u een of meer resourcegroepen waarden scheiden met een komma, gericht voor acties gestart.|  
|External_Stop_ResourceGroupNames | Hiermee geeft u een of meer resourcegroepen waarden scheiden met een komma, gericht voor stop-acties.|  
|Internal_AutomationAccountName | Hiermee geeft u de naam van het Automation-account.|  
|Internal_AutoSnooze_WebhookUri | Hiermee geeft u de dat webhook URI voor het scenario AutoStop aangeroepen.|  
|Internal_AzureSubscriptionId | Hiermee geeft u de Azure-abonnement-ID.|  
|Internal_ResourceGroupName | Hiermee geeft u de Resourcegroepnaam van Automation-account.|  
|Internal_SendGridAccountName | Hiermee geeft u de naam van de SendGrid-account.|  
|Internal_SendGridPassword | Hiermee geeft u het wachtwoord voor de SendGrid-account.|  

<br>

In alle scenario's, de **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, en **External_ExcludeVMNames** variabelen nodig zijn. gericht op virtuele machines, met uitzondering van met een door komma's gescheiden lijst van virtuele machines voor de **AutoStop_CreateAlert_Parent** runbook. Dat wil zeggen, moeten uw virtuele machines zich bevinden in doel resourcegroepen gebruiken om te worden gestart en acties moeten plaatsvinden stoppen. De logica werkt iets zoals Azure beleid dat u kunt het abonnement of resourcegroep doelgroep en acties overgenomen door de zojuist gemaakte virtuele machines. Deze benadering zo voorkomt u dat een afzonderlijke planning voor elke virtuele machine te onderhouden en beheren van wordt gestart en gestopt in schaal.

### <a name="schedules"></a>Planningen

De volgende tabel geeft een lijst van elk van de standaardplanningen in uw Automation-account hebt gemaakt.  U kunt deze wijzigen of uw eigen aangepaste schema's maken.  Standaard elk van deze zijn uitgeschakeld, behalve voor **Scheduled_StartVM** en **Scheduled_StopVM**.

U moet alle schema's, niet inschakelen omdat dit overlappende planning acties mogelijk maken. Het is raadzaam om te bepalen welke optimalisaties die u wilt uitvoeren en dienovereenkomstig wijzigen.  Zie de voorbeeldscenario's in de overzichtssectie voor verdere uitleg.   

|**De naam van de planning** | **Frequentie** | **Beschrijving**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Om de 8 uur | Kan het runbook AutoStop_CreateAlert_Parent om de 8 uur die op zijn beurt Hiermee stopt u de waarden op basis van een VM in External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames en External_ExcludeVMNames in Azure Automation-variabelen worden uitgevoerd.  U kunt ook een door komma's gescheiden lijst met virtuele machines opgeven met behulp van de parameter VMList.|  
|Scheduled_StopVM | De gebruiker gedefinieerde dagelijks | Het runbook Scheduled_Parent uitvoert met een parameter van het *stoppen* elke dag op de opgegeven tijd.  Automatisch alle virtuele machines die voldoen aan de regels die zijn gedefinieerd door asset variabelen gestopt. U moet het bijbehorende schema inschakelen **geplande-StartVM**.|  
|Scheduled_StartVM | De gebruiker gedefinieerde dagelijks | Het runbook Scheduled_Parent uitvoert met een parameter van het *Start* elke dag op de opgegeven tijd.  Alle virtuele machines die voldoen aan de regels die zijn gedefinieerd door de juiste variabelen wordt automatisch gestart.  U moet het bijbehorende schema inschakelen **geplande StopVM**.|
|Gesequentieerd StopVM | 1:00 uur (UTC), elke vrijdag | Het runbook Sequenced_Parent uitvoert met een parameter van het *stoppen* elke vrijdag op de opgegeven tijd.  Sequentieel (oplopend) stopt alle virtuele machines met een code van **SequenceStop** gedefinieerd door de betreffende variabelen.  Raadpleeg het gedeelte Runbooks voor meer informatie over labelwaarden en activa-variabelen.  U moet het bijbehorende schema inschakelen **geordende StartVM**.|
|Gesequentieerd StartVM | 13:00 uur (UTC), elke maandag | Het runbook Sequenced_Parent uitvoert met een parameter van het *Start* elke maandag op de opgegeven tijd. Sequentieel alle VM's (aflopend) begint met een code van **klikvolgorde** gedefinieerd door de betreffende variabelen.  Raadpleeg het gedeelte Runbooks voor meer informatie over labelwaarden en activa-variabelen.  U moet het bijbehorende schema inschakelen **geordende StopVM**.|

<br>

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit om toe te voegen van de starten/stoppen virtuele machines tijdens rustige uren oplossing voor uw Automation-account en configureer vervolgens de variabelen voor het aanpassen van de oplossing.

1. Klik in Azure Portal op **Nieuw**.<br> ![Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. Typ in het deelvenster Marketplace een trefwoord zoals **Start** of **starten/stoppen**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. U kunt ook Typ trefwoorden in een of meer van de volledige naam van de oplossing en druk op Enter.  Selecteer **starten/stoppen virtuele machines [Preview] buiten kantooruren** in de zoekresultaten.  
3. In de **starten/stoppen virtuele machines [Preview] buiten kantooruren** deelvenster voor de geselecteerde oplossing, Controleer de overzichtsgegevens en klik vervolgens op **maken**.  
4. De **oplossing toevoegen** deelvenster wordt weergegeven. U wordt gevraagd de oplossing configureren voordat u deze in uw Automation-abonnement importeren kunt.<br><br> ![Blade Oplossing toevoegen voor VM-beheer](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Op de **oplossing toevoegen** deelvenster **werkruimte**. Selecteer een OMS-werkruimte die gekoppeld aan de dezelfde Azure-abonnement dat u het Automation-account in. Als u een werkruimte geen hebt, selecteert u **nieuwe werkruimte maken**. Op de **OMS-werkruimte** deelvenster, voer de volgende stappen: 
   - Geef een naam op voor de nieuwe **OMS-werkruimte**.
   - Selecteer een **abonnement** om te koppelen aan door te selecteren in de vervolgkeuzelijst als de standaard geselecteerd niet geschikt is.
   - Voor **resourcegroep**, kunt u een nieuwe resourcegroep maken of een bestaande set selecteren.  
   - Selecteer een **locatie**.  Op dit moment de enige locaties beschikbaar zijn **Australië-Zuidoost**, **Canada centraal**, **centrale India**, **VS-Oost**, **Japan-Oost**, **Zuidoost-Azië**, **VK Zuid**, en **West-Europa**.
   - Selecteer een **prijscategorie**.  De oplossing biedt twee lagen: gratis en betaalde OMS.  Gratis laag heeft een limiet van de hoeveelheid gegevens die worden verzameld per dag, de bewaartermijn en de runbook-taak runtime minuten.  De betaalde categorie OMS heeft geen limiet op de hoeveelheid gegevens die dagelijks wordt verzameld.  

        > [!NOTE]
        > Hoewel de laag Per GB (zelfstandig) betaald wordt weergegeven als een optie, is het niet van toepassing.  Als u selecteert en doorgaan met het maken van deze oplossing in uw abonnement, mislukt dit.  Dit wordt opgelost bij de officiële release van deze oplossing.<br>Deze oplossing alleen gebruikt taak minuten voor automation en het logboek opnamesnelheid.  Dit biedt extra OMS-knooppunten niet toevoegen aan uw omgeving.  

6. Na het opgeven van de vereiste informatie op de **OMS-werkruimte** deelvenster, klikt u op **maken**.  U kunt de voortgang onder volgen **meldingen** in het menu dat keert u terug naar de **oplossing toevoegen** deelvenster wanneer u klaar bent.  
7. Op de **oplossing toevoegen** deelvenster **Automation-account**.  Als u een nieuwe OMS-werkruimte maakt, moet u ook een nieuw Automation-account worden gekoppeld aan deze maken.  Selecteer **een Automation-account maken**, en klik op de **toevoegen Automation-account** deelvenster biedt het volgende: 
  - Voer in het veld **Naam** de naam van het Automation-account in.

    Alle andere opties worden automatisch ingevuld op basis van de OMS-werkruimte geselecteerd. Deze opties kunnen niet worden gewijzigd.  Een Uitvoeren als-account voor Azure is de standaardmethode voor verificatie voor de runbooks die zijn opgenomen in deze oplossing.  Nadat u op **OK**, de configuratieopties die worden gevalideerd en het Automation-account is gemaakt.  U kunt de voortgang bijhouden onder **Meldingen** in het menu. 

    U kunt ook een bestaand Uitvoeren als-account voor Automation selecteren.  Houd er rekening mee dat het account dat u selecteert al aan een andere OMS-werkruimte kan niet worden gekoppeld. Als deze al is gekoppeld, kunt u het bericht en moet een ander Automation Run As-account selecteren of maak een nieuwe.<br><br> ![Automation-account is al gekoppeld aan OMS-werkruimte](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Ten slotte op de **oplossing toevoegen** deelvenster **configuratie**. De **Parameters** deelvenster wordt weergegeven.<br><br> ![Deelvenster parameters voor oplossing](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  Hier wordt gevraagd:  
   - Geef de **doel ResourceGroup namen**. Dit zijn de namen van resourcegroepen met virtuele machines die worden beheerd door deze oplossing.  U kunt meer dan één naam invoeren en scheid elk met een komma (waarden zijn niet hoofdlettergevoelig).  Jokertekens worden ondersteund als de bewerking moet worden gericht op VM's in alle resourcegroepen in het abonnement.
   - Geef de **VM uitsluiten lijst (tekenreeks)**. Dit is de naam van een of meer virtuele machines van de doelresourcegroep.  U kunt meer dan één naam invoeren en scheid elk met een komma (waarden zijn niet hoofdlettergevoelig).  Met een jokerteken wordt ondersteund.
   - Selecteer een **planning**. Dit is een terugkerende datum en tijd voor het starten en stoppen van de virtuele machines in de doel-resourcegroepen.  De planning is standaard geconfigureerd voor de UTC-tijdzone. Als u een andere regio is niet beschikbaar.  Zie voor meer informatie over het configureren van de planning voor de tijdzone van uw specifieke na het configureren van de oplossing [wijzigen van de planning voor opstarten en afsluiten](#modifying-the-startup-and-shutdown-schedule).
   - Voor het ontvangen van **e-mailmeldingen** van SendGrid, accepteer de standaardwaarde van **Ja** en geef een geldig e-mailadres. Als u selecteert **Nee** maar toch op een later tijdstip dat u wilt ontvangen van e-mailmeldingen, kunt u bijwerken de **External_EmailToAddress** variabele met geldige e-mailadressen, gescheiden door komma's, en vervolgens wijzigen van de variabele **External_IsSendEmail** met de waarde **Ja**.  

9. Nadat u de oorspronkelijke instellingen vereist voor de oplossing hebt geconfigureerd, schakelt u **maken**.  Nadat alle instellingen zijn geverifieerd, wordt de oplossing is geïmplementeerd voor uw abonnement.  Dit proces kan enkele seconden duren en u kunt de voortgang onder volgen **meldingen** in het menu. 

## <a name="collection-frequency"></a>Verzamelingsfrequentie

Automation-logboek en taak stroom taakgegevens wordt ingenomen in de opslagplaats Log Analytics om de 5 minuten.  

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u het beheersysteem VM toevoegen de **StartStopVMView** tegel wordt toegevoegd aan uw dashboard in uw werkruimte voor logboekanalyse vanuit de Azure-portal.  Deze tegel geeft een telling en een grafische weergave van de runbooktaken voor de oplossing die zijn gestart en is voltooid.<br><br> ![Tegel Weergave StartStopVM voor VM-beheer](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

In uw Automation-account, kunt u toegang tot en beheren van de oplossing door het selecteren van de **werkruimte** optie. Selecteer op de pagina logboekanalyse **oplossingen** in het linkerdeelvenster.  Op de **oplossingen** pagina, selecteert u de oplossing **Start Stop VM [werkruimte]** uit de lijst.<br><br> ![Lijst met oplossingen in Log Analytics](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Als u de oplossing wordt weergegeven de **Start Stop VM [werkruimte]** deelvenster oplossing. Hier kunt u belangrijke details, zoals controleren de **StartStopVM** tegel. Als in uw werkruimte voor logboekanalyse geeft deze tegel een telling en een grafische weergave van de runbooktaken voor de oplossing die zijn gestart en hebben voltooid.<br><br> ![Automation-updatebeheer oplossingenpagina](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Hier kunt kunt u verdere analyse uitvoeren van de Taakrecords door te klikken op de tegel ring. Dashboard van de oplossing toont Taakgeschiedenis en vooraf gedefinieerde logboek zoekquery's. Overschakelen naar de portal Log Analytics Advanced om te zoeken, gebaseerd op uw zoekquery's.  

Alle bovenliggende runbooks bevatten de *WhatIf* parameter. Als de waarde **True**, deze ondersteunt met gedetailleerde informatie over het exacte gedrag, het runbook wordt uitgevoerd wanneer uitgevoerd zonder de *WhatIf* parameter en valideert dat de juiste virtuele machines zijn wordt benaderd. Runbooks alleen hun gedefinieerde acties uitvoeren wanneer de *WhatIf* parameter is ingesteld op **False**.  


### <a name="scenario-1-daily-startstop-vms-across-a-subscription-or-target-resource-groups"></a>Scenario 1: Dagelijks starten/stoppen virtuele machines via een abonnement of resourcegroepen zijn gericht 

Dit is de standaardconfiguratie bij het implementeren van de oplossing.  U kunt bijvoorbeeld configureren om te stoppen alle virtuele machines via een abonnement wanneer u werk 's avonds laat en start deze in de ochtend wanneer u weer op kantoor zijn. Wanneer u de planning configureert **geplande-StartVM** en **geplande StopVM** tijdens de implementatie van deze starten en stoppen van gerichte virtuele machines.
>[!NOTE]
>De tijdzone is uw huidige tijdzone wanneer u de parameter planning time configureert. Het is echter opgeslagen in UTC-notatie in Azure Automation.  U beschikt niet over een tijdzoneconversie doen als dit wordt afgehandeld tijdens de implementatie.

U bepalen welke VM's zijn in het bereik van het configureren van de volgende variabelen: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, en **External_ ExcludeVMNames**.  

>[!NOTE]
> De waarde voor **ResourceGroup doelnamen** wordt opgeslagen als de waarde voor beide **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames**. U kunt elk van deze variabelen toe te passen verschillende resourcegroepen wijzigen voor verdere samenvattingen.  Gebruik voor de startactie, **External_Start_ResourceGroupNames**, en gebruik voor actie bij stoppen, **External_Stop_ResourceGroupNames**. Virtuele machines worden automatisch toegevoegd aan het begin en stop de schema's.

Als u wilt testen en valideren van uw configuratie, start u handmatig de **ScheduledStartStop_Parent** runbook en stel de parameter actie op **start** of **stoppen** en de WHATIF parameter voor **True**.<br><br> ![Parameters voor het runbook configureren](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)


 De geplande actie bekijken en breng eventueel benodigde wijzigingen voordat u implementeert tegen productie virtuele machines.  U kunt het runbook handmatig uitvoeren met de parameter die is ingesteld op **False**, of u kunt de planning Automation **schema-StartVM** en **planning StopVM** uitvoeren automatisch na de voorgeschreven planning.

### <a name="scenario-2-sequence-the-startstop-vms-across-a-subscription-by-using-tags"></a>Scenario 2: De sequencer gebruikt voor het starten/stoppen virtuele machines via een abonnement met behulp van tags
In een omgeving die twee of meer onderdelen op meerdere virtuele machines die een gedistribueerde werkbelasting ondersteunen, is ondersteunen de volgorde waarin onderdelen zijn gestart en gestopt in volgorde belangrijk.  U kunt dit doen door de volgende stappen uit te voeren:

1. Voeg een **klikvolgorde** en een **SequenceStop** label met een positief geheel getal naar virtuele machines die zijn gericht in **External_Start_ResourceGroupNames** en  **External_Stop_ResourceGroupNames** variabelen.  De begintijd en eindtijd acties worden uitgevoerd in oplopende volgorde.  Zie voor informatie over het labelen van een virtuele machine, [code van een virtuele Windows-Machine in Azure](../virtual-machines/windows/tag.md) en [code van een virtuele Linux-Machine in Azure](../virtual-machines/linux/tag.md).
2. Wijzigen van de planningen **geordende StartVM** en **geordende StopVM** op de datum en tijd die aan uw vereisten voldoen en de planning inschakelt.  

Als u wilt testen en valideren van uw configuratie, start u handmatig de **SequencedStartStop_Parent** runbook. Stel de parameter actie op **start** of **stoppen** en de parameter WHATIF **True**.<br><br> ![Parameters voor het runbook configureren](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Voorbeeld bekijken van de actie en breng eventueel benodigde wijzigingen voordat u implementeert tegen productie virtuele machines.  Bij gereed, handmatig het uitvoeren van het runbook met de parameter die is ingesteld op **False**, of laat de Automation-planning **geordende StartVM** en **geordende StopVM** uitvoeren automatisch na de voorgeschreven planning.  

### <a name="scenario-3-automate-startstop-vms-across-a-subscription-based-on-cpu-utilization"></a>Scenario 3: Automatiseren starten/stoppen virtuele machines via een abonnement op basis van CPU-gebruik
Deze oplossing kunt de kosten van het uitvoeren van virtuele machines in uw abonnement op Azure VM's die niet worden gebruikt tijdens perioden van niet-piekuren, zoals na uur, evalueren en automatisch afgesloten als het processorgebruik minder dan x % beheren.  

Standaard is de oplossing vooraf geconfigureerd voor het evalueren van de metriek percentage CPU of Gemiddeld gebruik 5 procent is of minder.  Hiermee wordt bepaald door de volgende variabelen en kan worden gewijzigd als de standaardwaarden voldoen niet aan uw vereisten:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

U kunt inschakelen die gericht is op de actie tegen een abonnement en resourcegroep of die gericht is op een specifieke lijst met virtuele machines, maar niet beide.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>De actie stoppen voor een abonnement en de resource-groep als doel

1. Configureer de **External_Stop_ResourceGroupNames** en **External_ExcludeVMNames** variabelen om op te geven van de doel-virtuele machines.  
2. Inschakelen en werk de **Schedule_AutoStop_CreateAlert_Parent** planning.
3. Voer de **AutoStop_CreateAlert_Parent** runbook met de parameter ACTION is ingesteld op **start** en ingesteld op whatIf **True** om uw wijzigingen te bekijken.

#### <a name="target-the-stop-action-by-vm-list"></a>Doel van de actie stoppen door VM-lijst

1. Voer de **AutoStop_CreateAlert_Parent** runbook met de parameter ACTION is ingesteld op **start**, toevoegen van een door komma's gescheiden lijst met virtuele machines in de *VMList* parameter en stel vervolgens de Parameter WHATIF **True**. De wijzigingen bekijken.  
2. Configureer de **External_ExcludeVMNames** parameter met een door komma's gescheiden lijst met virtuele machines (VM1, VM2, VM3).
3. Dit scenario worden niet door de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupnames** variabelen.  In dit scenario moet u uw eigen schema Automation maken. Zie voor meer informatie [plannen van een runbook in Azure Automation](../automation/automation-schedules.md).

Nu u een planning voor het stoppen van virtuele machines op basis van CPU-gebruik hebt, moet u om in te schakelen op een van de volgende planningen te starten.

* Doel actie door abonnement en resourcegroep starten.  Zie de stappen in [Scenario 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) voor het testen en het inschakelen van **geplande-StartVM** schema's.
* Doel actie door abonnement, resourcegroep en label starten.  Zie de stappen in [Scenario 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) voor het testen en het inschakelen van **geordende StartVM** schema's.


### <a name="configuring-email-notifications"></a>E-mailmeldingen configureren

E-mailmeldingen configureren nadat de oplossing is geïmplementeerd, wijzigt u de volgende drie variabelen:

* External_EmailFromAddress: Geef het e-mailadres van de afzender.
* External_EmailToAddress: Een door komma's gescheiden lijst met e-mailadressen opgeven (user@hotmail.com, user@outlook.com) voor het ontvangen van e-mailmeldingen.
* External_IsSendEmail: Ingesteld op **Ja** om e-mails te ontvangen. E-mailmeldingen uitschakelen, waarde instelt op **Nee**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Wijzigen van de planningen voor opstarten en afsluiten

Het beheren van de schema's voor opstarten en afsluiten in deze oplossing volgt dezelfde stappen zoals wordt beschreven in [plannen van een runbook in Azure Automation](automation-schedules.md).     

## <a name="log-analytics-records"></a>Log Analytics-records

Automation maakt twee soorten records in de opslagplaats OMS: Logboeken taak en taak stromen.

### <a name="job-logs"></a>Taaklogboeken

Eigenschap | Beschrijving|
----------|----------|
Caller |  Wie de bewerking heeft gestart.  Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken.|
Category | Classificatie van het type gegevens.  Voor Automation is de waarde JobLogs.|
CorrelationId | De GUID die de correlatie-id van de runbooktaak is.|
JobId | De GUID die de id van de runbooktaak is.|
operationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd.  De waarde is voor automatisering kunt taak.|
resourceId | Hiermee wordt het resourcetype in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
ResourceGroup | Hiermee wordt de resourcegroepnaam van de runbooktaak opgegeven.|
ResourceProvider | Hiermee wordt de Azure-service opgegeven waarmee de resources worden geleverd die u kunt implementeren en beheren.  Voor Automation is de waarde Azure Automation.|
ResourceType | Hiermee wordt het resourcetype in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
resultType | De status van de runbooktaak.  Mogelijke waarden zijn:<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>- Geslaagd|
resultDescription | Hiermee wordt resultaatstatus van de runbooktaak beschreven.  Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid|
RunbookName | Hiermee wordt de naam van het runbook opgegeven.|
SourceSystem | Hiermee wordt het bronsysteem voor de verzonden gegevens opgegeven.  Voor automatisering heeft de waarde OpsManager|
StreamType | Hiermee wordt het type gebeurtenis opgegeven. Mogelijke waarden zijn:<br>- Uitgebreid<br>- Uitvoer<br>- Fout<br>- Waarschuwing|
SubscriptionId | Hiermee wordt de abonnements-id van de taak opgegeven.
Time | Datum en tijd van uitvoering van de runbooktaak.|


### <a name="job-streams"></a>Taakstromen

Eigenschap | Beschrijving|
----------|----------|
Caller |  Wie de bewerking heeft gestart.  Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken.|
Category | Classificatie van het type gegevens.  Voor Automation is de waarde JobStreams.|
JobId | De GUID die de id van de runbooktaak is.|
operationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd.  De waarde is voor automatisering kunt taak.|
ResourceGroup | Hiermee wordt de resourcegroepnaam van de runbooktaak opgegeven.|
resourceId | Hiermee wordt de resource-id in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
ResourceProvider | Hiermee wordt de Azure-service opgegeven waarmee de resources worden geleverd die u kunt implementeren en beheren.  Voor Automation is de waarde Azure Automation.|
ResourceType | Hiermee wordt het resourcetype in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
resultType | Het resultaat van de runbooktaak op het moment dat de gebeurtenis werd gegenereerd. Er is een mogelijke waarde:<br>- Wordt uitgevoerd|
resultDescription | Bevat de uitvoerstroom van het runbook.|
RunbookName | De naam van het runbook.|
SourceSystem | Hiermee wordt het bronsysteem voor de verzonden gegevens opgegeven.  De waarde is voor automatisering kunt OpsManager.|
StreamType | Het type taakstroom. Mogelijke waarden zijn:<br>-Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid|
Time | Datum en tijd van uitvoering van de runbooktaak.|

Wanneer u een logboek zoekquery waarmee categorierecords van uitvoert **JobLogs** of **JobStreams**, kunt u de **JobLogs** of **JobStreams**waarmee u een reeks tegels van de updates die zijn geretourneerd door de zoekopdracht samen te vatten bekijken.

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

De volgende tabel bevat voorbeeldzoekopdrachten in logboeken voor taakrecords die worden verzameld met deze oplossing. 

Query’s uitvoeren | Beschrijving|
----------|----------|
Taken voor runbook ScheduledStartStop_Parent die met succes voltooid vinden | Categorie Zoek == "JobLogs" &#124; waar (RunbookName_s == "ScheduledStartStop_Parent") &#124; waar (ResultType == "Voltooid") &#124; overzicht van AggregatedValue = count() door ResultType, bin (TimeGenerated, 1 uur) &#124; TimeGenerated desc sorteren|
Taken voor runbook SequencedStartStop_Parent die met succes voltooid vinden | Categorie Zoek == "JobLogs" &#124; waar (RunbookName_s == "SequencedStartStop_Parent") &#124; waar (ResultType == "Voltooid") &#124; overzicht van AggregatedValue = count() door ResultType, bin (TimeGenerated, 1 uur) &#124; TimeGenerated desc sorteren

## <a name="removing-the-solution"></a>Verwijderen van de oplossing

Als u dat u niet langer wilt gebruiken van de oplossing besluit, kunt u deze verwijderen uit het Automation-account.  Verwijderen van de oplossing, verwijdert alleen de runbooks. De variabelen die zijn gemaakt toen u de oplossing is toegevoegd of schema's worden niet verwijderd.  Deze assets u handmatig verwijderen moet als u deze niet met andere runbooks gebruikt.  

Als u wilt verwijderen van de oplossing, kunt u de volgende stappen uitvoeren:

1.  Selecteer in uw Automation-account **werkruimte** in het linkerdeelvenster.  
2.  Op de **oplossingen** pagina, selecteert u de oplossing **Start Stop VM [werkruimte]**.  Op de **VMManagementSolution [werkruimte]** pagina in het menu, selecteer **verwijderen**.<br><br> ![Oplossing voor VM-Mgmt verwijderen](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  In de **oplossing verwijderen** venster bevestigen dat u wilt verwijderen van de oplossing.
4.  Terwijl de informatie is geverifieerd en de oplossing wordt verwijderd, u kunt de voortgang onder volgen **meldingen** in het menu.  U keert terug naar de **oplossingen** pagina nadat het proces voor het verwijderen van de oplossing is gestart.  

De Automation-account en de werkruimte voor logboekanalyse niet verwijderd als onderdeel van dit proces.  Als u niet behouden van de werkruimte voor logboekanalyse wilt, moet u deze handmatig verwijderen.  Dit kan worden bereikt vanaf de Azure-portal:

1.    Selecteer in de Azure portal startscherm **logboekanalyse**.
2. Op de **logboekanalyse** deelvenster, selecteer de werkruimte.
3. Selecteer **verwijderen** vanuit het menu op de werkruimte instellingen.  
      
## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van verschillende zoekquery's en de Automation-taak logboeken met Log Analytics, [zoekopdrachten aanmelden met logboekanalyse](../log-analytics/log-analytics-log-searches.md).
- Zie [Runbooktaken bijhouden](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details.
- Zie voor meer informatie over Log Analytics en verzameling gegevensbronnen, [verzamelen van Azure storage-gegevens in het overzicht van logboekanalyse](../log-analytics/log-analytics-azure-storage.md).






   

