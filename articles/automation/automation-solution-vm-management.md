---
title: Virtuele machines starten/stoppen tijdens rustige uren oplossing | Microsoft Docs
description: Met de VM-beheeroplossing worden uw virtuele machines voor Azure Resource Manager gestart en gestopt volgens een planning en proactief bewaakt vanuit Log Analytics.
services: automation
documentationCenter: 
authors: georgewallace
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: magoedte
ms.openlocfilehash: e6f1189b9729c57718a5cd6d6f6a583b94f6f142
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Virtuele machines starten/stoppen tijdens rustige uren oplossing in Azure Automation

De starten/stoppen virtuele machines buiten kantooruren oplossing wordt gestart en uw virtuele Azure-machines op de gebruiker gedefinieerde schema's gestopt insights via Log Analytics en biedt optionele e-mailberichten verzendt dankzij het gebruik van [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Het ondersteunt zowel Azure Resource Manager en classic VM's voor de meeste scenario's. 

Deze oplossing biedt de mogelijkheid van een gedecentraliseerde automation voor klanten die willen hun bronnen in zonder server, lage kosten te verlagen. Inclusief:

* Plannen van virtuele machines te starten/stoppen
* Plannen van virtuele machines te starten/stoppen in oplopende volgorde met Azure-Tags (wordt niet ondersteund voor klassieke VM's)
* Automatische stop die VM 's op basis van de lage CPU-gebruik

## <a name="prerequisites"></a>Vereisten

- De runbooks werken met een [Uitvoeren als-account voor Azure](automation-offering-get-started.md#authentication-methods).  Het Uitvoeren als-account is de aanbevolen verificatiemethode omdat hiervoor verificatie via een certificaat wordt gebruikt, in plaats van een wachtwoord dat mogelijk vervalt of regelmatig wordt gewijzigd.  

- Deze oplossing kan alleen beheren voor virtuele machines die zich in hetzelfde abonnement als waar het Automation-account zich bevindt.  

- Deze oplossing is alleen worden geïmplementeerd op de volgende Azure-regio's - Australië-Zuidoost, Canada centraal, India centraal, VS-Oost, Japan-Oost, Zuidoost-Azië, VK Zuid en West-Europa.  
    
    > [!NOTE]
    > De runbooks voor het beheren van de planning van de VM kan richten op virtuele machines in elke regio.  

- Voor het verzenden van e-mailmeldingen wanneer de VM-runbooks starten en stoppen tijdens de voorbereiding uit Azure Marketplace voltooien, moet u Selecteer **Ja** SendGrid implementeren. 

    > [!IMPORTANT]
    > SendGrid is een service van derden, voor ondersteuning bij SendGrid Neem contact op met [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Beperkingen van SendGrid zijn als volgt:

    * Maximaal één SendGrid-account per gebruiker per abonnement
    * Maximaal twee SendGrid-accounts per abonnement

Als u een eerdere versie van deze oplossing hebt geïmplementeerd, moet u eerst verwijderen uit uw account voordat u implementeert deze release.  

## <a name="solution-components"></a>Oplossingsonderdelen

Deze oplossing bevat vooraf geconfigureerde runbooks, schema's en integratie met logboekanalyse waarmee u het opstarten en afsluiten van uw virtuele machines met suite aanpassen uw bedrijf nodig heeft. 

### <a name="runbooks"></a>Runbooks

De onderstaande lijst de runbooks die zijn geïmplementeerd voor uw Automation-account.  Het is niet raadzaam dat u wijzigingen in de runbook-code aanbrengen, maar uw eigen runbook voor nieuwe functionaliteit in plaats van te schrijven.

> [!NOTE]
> Niet uitvoeren rechtstreeks een willekeurig runbook met de naam 'Onderliggende' toegevoegd aan het einde van de naam.
>

Alle bovenliggende runbooks bevat de *WhatIf* parameter, die tijdens ingesteld op **True**, ondersteunt met gedetailleerde informatie over het exacte gedrag, het runbook wordt uitgevoerd wanneer uitgevoerd zonder de *WhatIf* de parameter en de juiste valideert gericht van virtuele machines.  Runbooks worden alleen de gedefinieerde acties uitgevoerd wanneer de *WhatIf* parameter is ingesteld op **False**. 

|**Runbook** | **Parameters** | **Beschrijving**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Naam van het bovenliggende runbook. Waarschuwingen maakt op basis van per resource voor AutoStop scenario.| 
|AutoStop_CreateAlert_Parent | WhatIf: True of False <br> VMList | Maken of bijwerken van Azure waarschuwingsregels op virtuele machines in de doelgroepen abonnement of resourcegroep. <br> VMList: Door komma's gescheiden lijst met VM's.  Bijvoorbeeld: *vm1 vm2, vm3*| 
|AutoStop_Disable | geen | AutoStop waarschuwingen en standaardschema uitschakelen.| 
|AutoStop_StopVM_Child | WebHookData | Bovenliggend runbook alleen aangeroepen. Waarschuwingsregels dit runbook aanroepen en wordt het werk van de virtuele machine wordt gestopt.|  
|Bootstrap_Main | geen | Eenmaal op set-up bootstrap configuraties zoals webhookURI dat doorgaans niet toegankelijk vanuit Azure Resource Manager gebruikt. Dit runbook wordt automatisch verwijderd als implementatie met succes is geworden.|  
|ScheduledStartStop_Child | VMName <br> Actie: Stoppen of starten <br> resourceGroupName | Bovenliggend runbook alleen aangeroepen. Biedt de werkelijke uitvoering van of het stoppen of starten voor geplande stoppen.|  
|ScheduledStartStop_Parent | Actie: Stoppen of starten <br> WhatIf: True of False | Dit wordt doorgevoerd op alle virtuele machines in het abonnement tenzij u bewerkt de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames** die wordt alleen uitgevoerd op deze beperken Resourcegroepen zijn gericht. U kunt ook specifieke virtuele machines uitsluiten door het bijwerken van de **External_ExcludeVMNames** variabele. WhatIf gedraagt zich hetzelfde als in andere runbooks.|  
|SequencedStartStop_Parent | Actie: Stoppen of starten <br> WhatIf: True of False | Maakt u een label aangeroepen **klikvolgorde** en een andere code aangeroepen **SequenceStop** op elke virtuele machine die u starten van de reeks wilt\\activiteit voor het stoppen. De waarde van de tag moet een positief geheel getal (1,2,3) die overeenkomt met de volgorde die u wilt starten\\stoppen in oplopende volgorde. WhatIf gedraagt zich hetzelfde als in andere runbooks. <br> **Opmerking: Virtuele machines moeten binnen bronnengroepen gedefinieerde External_Start_ResourceGroupNames External_Stop_ResourceGroupNames en External_ExcludeVMNames in Azure Automation-variabelen en de bijbehorende codes voor acties om te laten treden.**|

### <a name="variables"></a>Variabelen

De onderstaande lijst de variabelen die in uw Automation-account gemaakt.  Het is raadzaam dat u alleen variabelen met het voorvoegsel wijzigen **externe**, variabelen wijzigen voorafgegaan door **intern** wordt bijwerkingen veroorzaken.  

|**Variabele** | **Beschrijving**|
---------|------------|
|External_AutoStop_Condition | Dit is de voorwaardelijke operator is vereist voor het configureren van de voorwaarde voordat een waarschuwing. Acceptabele waarden zijn **groter dan**, **GreaterThanOrEqual**, **LessThan**, **LessThanOrEqual**.|  
|External_AutoStop_Description | Waarschuwing voor de virtuele machine stoppen als het CPU-percentage de drempelwaarde overschrijdt.|  
|External_AutoStop_MetricName | Naam van de prestaties metriek Azure waarschuwingsregel is om te worden geconfigureerd voor.| 
|External_AutoStop_Threshold | Drempelwaarde voor de Azure waarschuwingsregel opgegeven in de variabele *External_AutoStop_MetricName*. Percentagewaarden kunnen variëren van 1 tot 100.|  
|External_AutoStop_TimeAggregationOperator | De tijd aggregatieoperator die wordt toegepast op de grootte van het geselecteerde venster om de voorwaarde te evalueren. Acceptabele waarden zijn **gemiddelde**, **Minimum**, **maximale**, **totale**, **laatste**.|  
|External_AutoStop_TimeWindow | De grootte van het venster gedurende welke Azure geselecteerde metrische gegevens voor activering van een waarschuwing worden geanalyseerd. Deze parameter accepteert invoer in de notatie timespan. Mogelijke waarden zijn van vijf minuten tot zes uur.|  
|External_EmailFromAddress | Hiermee geeft u de afzender van het e-mailbericht.|  
|External_EmailSubject | Hiermee wordt de tekst voor de onderwerpregel van het e-mailbericht opgegeven.|  
|External_EmailToAddress | Hiermee worden de ontvangers van het e-mailbericht opgegeven. Scheid namen met een komma.|  
|External_ExcludeVMNames | Geef de namen van de VM moeten worden uitgesloten scheiden van namen met een komma zonder spaties.|  
|External_IsSendEmail | Hiermee geeft u een optie voor het verzenden van e-mailmelding na voltooiing.  Geef **Ja** of **Nee** niet om e-mail te verzenden.  Deze optie moet **Nee** als u niet SendGrid hebt gemaakt tijdens de initiële implementatie.|  
|External_Start_ResourceGroupNames | Hiermee geeft u een of meer resourcegroepen scheiden met een komma, gericht voor begin acties waarden.|  
|External_Stop_ResourceGroupNames | Hiermee geeft u een of meer resourcegroepen waarden scheiden met komma, gericht voor Stop-acties.|  
|Internal_AutomationAccountName | Hiermee geeft u de naam van het Automation-account.|  
|Internal_AutoSnooze_WebhookUri | Hiermee geeft u de dat webhook URI voor het scenario AutoStop aangeroepen.|  
|Internal_AzureSubscriptionId | Hiermee geeft u de Azure-abonnement-id.|  
|Internal_ResourceGroupName | Hiermee geeft u de Resourcegroepnaam van Azure Automation-account.|  
|Internal_SendGridAccountName | Hiermee geeft u de naam van de SendGrid-account.|  
|Internal_SendGridPassword | Hiermee geeft u het wachtwoord voor de SendGrid-account.|  

<br>

In alle scenario's, de **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, en **External_ExcludeVMNames** variabelen nodig zijn. gericht op virtuele machines met uitzondering van het bieden van een door komma's gescheiden lijst met virtuele machines voor de **AutoStop_CreateAlert_Parent** runbook. Dat wil zeggen, moeten uw virtuele machines zich bevinden in doel resourcegroepen voor acties starten/stoppen om te worden uitgevoerd. De logica werkt iets zoals Azure beleid dat u kunt het abonnement of de resource-groep als doel en acties zijn overgenomen door de zojuist gemaakte virtuele machines. Deze aanpak zo voorkomt u dat een afzonderlijke planning voor elke virtuele machine te onderhouden en beheren van starten/stoppen in schaal.

### <a name="schedules"></a>Planningen

De volgende tabel geeft een lijst van elk van de standaardplanningen in uw Automation-account hebt gemaakt.  U kunt deze wijzigen of uw eigen aangepaste schema's maken.  Elk van deze schema's zijn standaard uitgeschakeld, behalve voor **Scheduled_StartVM** en **geplande StopVM**.

Het wordt niet aanbevolen om alle's inschakelen, omdat dit een overlap bij welke planning een actie kan worden gemaakt.  In plaats daarvan is het aanbevolen om te bepalen welke optimalisaties die u wilt uitvoeren en dienovereenkomstig wijzigen.  Zie de voorbeeldscenario's in de overzichtssectie voor verdere uitleg.   

|**ScheduleName** | **Frequentie** | **Beschrijving**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Elke acht uur wordt uitgevoerd | Het runbook AutoStop_CreateAlert_Parent wordt uitgevoerd om de 8 uur, die op zijn beurt wordt beëindigd op basis waarden van de virtuele machine in 'External_Start_ResourceGroupNames', 'External_Stop_ResourceGroupNames' en 'External_ExcludeVMNames' in Azure Automation-variabelen.  U kunt ook een door komma's gescheiden lijst met virtuele machines met de parameter 'VMList' opgeven.|  
|Scheduled_StopVM | De gebruiker gedefinieerde, elke dag | Het runbook Scheduled_Parent uitvoert met een 'Stop'-parameter elke dag op de opgegeven tijd.  Alle van de virtuele machine die voldoen aan de regels die zijn gedefinieerd via Asset variabelen automatisch gestopt.  Het beste inschakelen van de zus plant, geplande-StartVM.|  
|Scheduled_StartVM | De gebruiker gedefinieerde, elke dag | Het runbook Scheduled_Parent uitvoert met een parameter van het *Start* elke dag op de opgegeven tijd.  Alle van de virtuele machine die voldoen aan de regels die zijn gedefinieerd en deel uitmaken van de betreffende variabelen wordt automatisch worden gestart.  Het is raadzaam inschakelen van de zus planning, **geplande StopVM**.|
|Gesequentieerd StopVM | 1:00 uur (UTC), elke vrijdag | Het runbook Sequenced_Parent uitvoert met een parameter van het *stoppen* elke vrijdag om de opgegeven tijd.  Sequentieel wordt (oplopend) stop alle VM's met een code van **SequenceStop** gedefinieerd en deel uit van de betreffende variabelen.  Raadpleeg het gedeelte voor meer informatie over labelwaarden en asset variabelen Runbooks.  Dit is het beste inschakelen van de zus planning, **geordende StartVM**.|
|Gesequentieerd StartVM | 13:00 uur (UTC), elke maandag | Het runbook Sequenced_Parent uitvoert met een parameter van het *Start* elke maandag op de opgegeven tijd.  Sequentieel wordt (aflopend) start alle VM's met een code van **klikvolgorde** gedefinieerd en deel uit van de betreffende variabelen.  Raadpleeg het gedeelte voor meer informatie over labelwaarden en asset variabelen Runbooks.  Het is raadzaam inschakelen van de zus planning, **geordende StopVM**.|

<br>

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit om de oplossing voor VM's starten/stoppen buiten kantooruren [Preview] toe te voegen aan uw Automation-account en vervolgens de variabelen te configureren om de oplossing aan te passen.

1. Klik in Azure Portal op **Nieuw**.<br> ![Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. Typ in het deelvenster Marketplace **VM starten**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **starten/stoppen virtuele machines [Preview] buiten kantooruren** in de zoekresultaten.  
3. In de **starten/stoppen virtuele machines [Preview] buiten kantooruren** deelvenster voor de geselecteerde oplossing, Controleer de overzichtsgegevens en klik vervolgens op **maken**.  
4. De **oplossing toevoegen** deelvenster wordt weergegeven waarin u wordt gevraagd de oplossing configureren voordat u deze in uw Automation-abonnement importeren kunt.<br><br> ![Blade Oplossing toevoegen voor VM-beheer](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Op de **oplossing toevoegen** blade Selecteer **werkruimte** en hier het selecteren van een OMS-werkruimte die is gekoppeld aan de dezelfde Azure-abonnement dat u het Automation-account in of maak een nieuwe werkruimte.  Als u een werkruimte niet hebt, kunt u **nieuwe werkruimte maken** en klik op de **OMS-werkruimte** deelvenster Voer de volgende stappen: 
   - Geef een naam op voor de nieuwe **OMS-werkruimte**.
   - Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
   - Voor **Resourcegroep** kunt u een nieuwe resourcegroep maken of een bestaande resourcegroep selecteren.  
   - Selecteer een **locatie**.  Momenteel de enige locaties opgegeven voor selectie zijn **Australië-Zuidoost**, **Canada centraal**, **centrale India**, **VS-Oost**, **Japan-Oost**, **Zuidoost-Azië**, **VK Zuid**, en **West-Europa**.
   - Selecteer een **prijscategorie**.  De oplossing wordt in twee categorieën aangeboden: gratis categorie en betaalde categorie OMS.  De gratis categorie heeft een limiet op de hoeveelheid gegevens die dagelijks wordt verzameld, op de retentieperiode en op de runtimeminuten van runbooktaken.  De betaalde categorie OMS heeft geen limiet op de hoeveelheid gegevens die dagelijks wordt verzameld.  

        > [!NOTE]
        > Terwijl de laag Per GB (zelfstandig) betaald wordt weergegeven als een optie, is het niet van toepassing.  Als u de optie selecteert en doorgaat met het maken van deze oplossing in uw abonnement, mislukt dit.  Dit wordt opgelost bij de officiële release van deze oplossing.<br>Als u deze oplossing gebruikt, worden alleen Automation-taakminuten en logopname gebruikt.  Met de oplossing worden geen aanvullende OMS-knooppunten aan uw omgeving toegevoegd.  

6. Nadat u de vereiste gegevens hebt opgegeven op de blade **OMS-werkruimte**, klikt u op **Maken**.  Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu.  U keert automatisch terug naar de blade **Oplossing toevoegen**.  
7. Selecteer op de blade **Oplossing toevoegen** de optie **Automation-account**.  Als u een nieuwe OMS-werkruimte maakt, moet u ook een nieuw Automation-account maken dat wordt gekoppeld aan de nieuwe OMS-werkruimte die eerder is opgegeven, inclusief uw Azure-abonnement, resourcegroep en regio.  U kunt **Een Automation-account maken** selecteren en het volgende opgeven op de blade **Automation-account toevoegen**: 
  - Voer in het veld **Naam** de naam van het Automation-account in.

    Alle overige opties worden automatisch ingevuld op basis van de geselecteerde OMS-werkruimte. Deze opties kunnen niet worden gewijzigd.  Een Uitvoeren als-account voor Azure is de standaardmethode voor verificatie voor de runbooks die zijn opgenomen in deze oplossing.  Nadat u op **OK** hebt geklikt, worden de configuratieopties gevalideerd en wordt het Automation-account gemaakt.  U kunt de voortgang bijhouden onder **Meldingen** in het menu. 

    U kunt ook een bestaand Uitvoeren als-account voor Automation selecteren.  Let op: het account dat u selecteert, mag niet al aan een andere OMS-werkruimte zijn gekoppeld, anders wordt er een bericht weergegeven in de blade om u op de hoogte te stellen.  Als het account al is gekoppeld, moet u een ander Uitvoeren als-account voor Automation selecteren of een nieuw account maken.<br><br> ![Automation-account is al gekoppeld aan OMS-werkruimte](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Selecteer ten slotte op de blade **Oplossing toevoegen** de optie **Configuratie**. De blade **Parameters** wordt weergegeven.<br><br> ![Deelvenster parameters voor oplossing](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  Op de blade **Parameters** wordt u het volgende gevraagd:  
   - Geef de **namen van de doel-ResourceGroup** op. Dit is een resourcegroepnaam die VM's bevat die met deze oplossing moeten worden beheerd.  U kunt meer dan één naam invoeren en scheid elk met een komma (waarden zijn niet hoofdlettergevoelig).  Jokertekens worden ondersteund als de bewerking moet worden gericht op VM's in alle resourcegroepen in het abonnement.
   - Specity de **VM uitsluiten lijst (tekenreeks)**, dit is de naam van op of meer virtuele machines van de doelresourcegroep.  U kunt meer dan één naam invoeren en scheid elk met een komma (waarden zijn niet hoofdlettergevoelig).  Met een jokerteken wordt ondersteund.
   - Selecteer een **planning**. Dit is een periodieke datum en tijd voor het starten en stoppen van de VM's in de doelresourcegroep(en).  Standaard de planning is geconfigureerd voor de UTC-tijdzone en selecteren van een andere regio is niet beschikbaar.  Als u wilt de planning voor de tijdzone van uw specifieke na het configureren van de oplossing configureren, Zie [wijzigen van de planning voor opstarten en afsluiten](#modifying-the-startup-and-shutdown-schedule) hieronder.
   - Voor het ontvangen van **e-mailmeldingen** van SendGrid, accepteer de standaardwaarde van **Ja** en geef een geldig e-mailadres.  Als u selecteert **Nee** en later besluit u wilt ontvangen van e-mailmeldingen, moet u de oplossing opnieuw vanuit Azure marketplace opnieuw te implementeren.  

10. Zodra u klaar bent met het configureren van de eerste instellingen die zijn vereist voor de oplossing, selecteert u **Maken**.  Alle instellingen worden gevalideerd en vervolgens wordt geprobeerd de oplossing te implementeren in uw abonnement.  Dit proces kan enkele seconden duren en u kunt de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="collection-frequency"></a>Verzamelingsfrequentie

Automation-taak logboek- en stream-gegevens is om de vijf minuten in de opslagplaats logboekanalyse ingenomen.  

## <a name="using-the-solution"></a>De oplossing gebruiken

Als u het VM-beheersysteem in de werkruimte voor logboekanalyse vanuit de Azure-Portal toevoegt de **StartStopVM weergave** tegel is toegevoegd aan uw dashboard.  Deze tegel wordt weergegeven voor een aantal en de grafische weergave van de runbooks taken voor de oplossing die zijn gestart en is voltooid.<br><br> ![Tegel Weergave StartStopVM voor VM-beheer](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

In uw Automation-account, kunt u toegang tot en beheren van de oplossing door het selecteren van de **werkruimte** optie en selecteer op de pagina logboekanalyse **oplossingen** in het linkerdeelvenster.  Op de **oplossingen** pagina, selecteert u de oplossing **Start Stop VM [werkruimte]** uit de lijst.<br><br> ![Lijst met oplossingen in Log Analytics](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Als u de oplossing wordt weergegeven de **Start Stop VM [werkruimte]** oplossing blade belangrijke informatie zoals bekijken te de **StartStopVM** tegel, zoals in de werkruimte voor logboekanalyse die Geeft een aantal en de grafische weergave van de runbooktaken voor de oplossing die zijn gestart en zijn met succes voltooid.<br><br> ![Automation-updatebeheer oplossingenpagina](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Hier kunt u ook uitvoeren verdere analyse van de Taakrecords door te klikken op de tegel ring en vanuit het dashboard van oplossing Taakgeschiedenis, vooraf gedefinieerde logboek zoekquery's te zien en overschakelen naar de portal Log Analytics Advanced om te zoeken op basis van uw zoekopdrachten.  

Alle bovenliggende runbooks bevat de *WhatIf* parameter, die tijdens ingesteld op **True**, ondersteunt met gedetailleerde informatie over het exacte gedrag, het runbook wordt uitgevoerd wanneer uitgevoerd zonder de *WhatIf* de parameter en de juiste valideert gericht van virtuele machines.  Runbooks worden alleen de gedefinieerde acties uitgevoerd wanneer de *WhatIf* parameter is ingesteld op **False**.  


### <a name="scenario-1-daily-stopstart-vms-across-a-subscription-or-target-resource-groups"></a>Scenario 1: Dagelijks starten/stoppen virtuele machines via een abonnement of resourcegroepen zijn gericht 

Dit is de standaardconfiguratie bij het implementeren van de oplossing.  U kunt bijvoorbeeld configureren om te stoppen alle virtuele machines via een abonnement 's avonds wanneer u laat werk en start deze in de ochtend wanneer u weer op kantoor zijn. Wanneer u de planning configureert ** geplande-StartVM ' en **geplande StopVM** tijdens de implementatie van deze starten en stoppen van gerichte virtuele machines.
>[!NOTE]
>De tijdzone is uw huidige tijdzone bij het configureren van de parameter planning time; maar deze is opgeslagen in UTC-notatie in Azure Automation.  U beschikt niet over een tijdzoneconversie doen als dit wordt afgehandeld tijdens de implementatie.

U bepalen welke VM's zijn in het bereik van het configureren van de twee variabelen - **External_Start_ResourceGroupNames**, ** External_Stop_ResourceGroupNames, en **External_ExcludeVMNames**.  

>[!NOTE]
> De waarde voor variabele **ResourceGroup doelnamen**' wordt opgeslagen als de waarde voor beide **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames** variabelen. U kunt elk van deze variabelen toe te passen verschillende resourcegroepen wijzigen voor verdere samenvattingen.  Voor begin actie gebruik **External_Start_ResourceGroupNames** en voor stoppen actie gebruik **External_Stop_ResourceGroupNames** in plaats daarvan. Nieuwe virtuele machines worden automatisch toegevoegd aan het begin en stop de schema's.

Als u wilt testen en valideren van uw configuratie, start u handmatig de **ScheduledStartStop_Parent** runbook en stel de *actie* -parameter voor **start** of **stoppen**  en de *WhatIf* -parameter voor **true**.<br><br> ![Parameters voor het runbook configureren](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Hiermee kunt u een voorbeeld bekijken van de actie die zou plaatsvinden en breng de gewenste wijzigingen zo nodig voordat u implementeert tegen productie virtuele machines.  Wanneer u vertrouwd bent, kunt u het runbook handmatig uitvoeren met de parameter die is ingesteld op **false** of laat de Automation-planning **schema-StartVM** en **schema-StopVM**Voer de volgende automatisch de voorgeschreven planning.

### <a name="scenario-2-sequence-the-stopstart-vms-across-a-subscription-using-tags"></a>Scenario 2: De sequencer gebruikt voor het stoppen/starten virtuele machines via een abonnement met tags
In een omgeving die twee of meer onderdelen op meerdere virtuele machines die een gedistribueerde werkbelasting ondersteunen, is ondersteunen de volgorde die onderdelen gestart/gestopt in volgorde zijn belangrijk.  U kunt dit doen door de volgende stappen uit te voeren.

1. Toevoegen van een **klikvolgorde** en **SequenceStop** label met een positief geheel getal voor VM's in uw abonnement die zijn gericht in **External_Start_ResourceGroupNames**en  **External_Stop*ResourceGroupNames** variabelen.  De begintijd en eindtijd acties worden uitgevoerd in oplopende volgorde.  Zie voor informatie over het labelen van een virtuele machine, [code van een virtuele Windows-Machine in Azure](../virtual-machines/windows/tag.md) en [code van een virtuele Linux-Machine in Azure](../virtual-machines/linux/tag.md)
2. Wijzigen van de planningen **geordende StartVM** en **geordende StopVM** op de datum en tijd om te voldoen aan uw behoeften en de planning inschakelt.  

Als u wilt testen en valideren van uw configuratie, start u handmatig de **SequencedStartStop_Parent** runbook en stel de *actie* -parameter voor **start** of **stoppen**  en de *WhatIf* -parameter voor **True**.<br><br> ![Parameters voor het runbook configureren](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Hiermee kunt u een voorbeeld bekijken van de actie die zou plaatsvinden en breng de gewenste wijzigingen zo nodig voordat u implementeert tegen productie virtuele machines.  Wanneer u vertrouwd bent, kunt u het runbook handmatig uitvoeren met de parameter die is ingesteld op **false** of laat de Automation-planning **geordende StartVM** en **geordende-StopVM**Voer de volgende automatisch de voorgeschreven planning.  

### <a name="scenario-3-auto-stopstart-vms-across-a-subscription-based-on-cpu-utilization"></a>Scenario 3: Automatisch stoppen/starten virtuele machines via een abonnement op basis van CPU-gebruik
Om te helpen de kosten van het uitvoeren van virtuele machines in uw abonnement beheren, moet deze oplossing kunt door Azure VM's die niet worden gebruikt tijdens perioden van niet-piekuren, zoals na uur en automatisch ze afsluiten als processorgebruik is lager dan x % te evalueren.  

Standaard worden de oplossing is vooraf geconfigureerd voor het evalueren van de metriek Percentage CPU en als Gemiddeld gebruik 5% of minder.  Hiermee wordt bepaald door de volgende variabelen en kan worden gewijzigd als de standaardwaarden voldoen niet aan uw vereisten:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

U kunt alleen inschakelen op zoek bent naar de actie op basis van een abonnement en resourcegroep of specifieke lijst met virtuele machines, maar niet beide.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>De actie stoppen voor een abonnement en de resource-groep als doel

1. Configureer de **External_Stop_ResourceGroupNames** en **External_ExcludeVMNames** variabelen om op te geven van de doel-virtuele machines.  
2. Inschakelen en werk de **Schedule_AutoStop_CreateAlert_Parent** planning.
3. Voer de **AutoStop_CreateAlert_Parent** runbook met de *actie* parameter ingesteld op **start** en de *WhatIf* parameter ingesteld op  **De waarde True** om uw wijzigingen te bekijken.

#### <a name="target-stop-action-by-vm-list"></a>Actie bij stoppen doel door VM-lijst

1. Voer de **AutoStop_CreateAlert_Parent** runbook met de *actie* parameter ingesteld op **start**, toevoegen van een door komma's gescheiden lijst met virtuele machines in de *VMList* parameter, en de *WhatIf* parameter ingesteld op **True** om uw wijzigingen te bekijken.  
2. Configureer de **External_ExcludeVMNames** parameter met een door komma's gescheiden lijst met virtuele machines (VM1, VM2, VM3).
3. Dit scenario worden niet door de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupnames** varabies.  U moet u eigen Automation planning maken voor dit scenario. Zie voor meer informatie [plannen van een runbook in Azure Automation](../automation/automation-schedules.md).

Nu u hebt een planning voor virtuele machines stoppen op basis van CPU-gebruik, moet u om in te schakelen op een van de onderstaande schema's te starten.

* Doel actie door abonnement en resourcegroep starten.  Zie de stappen in [Scenario 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) voor het testen en het inschakelen van **geplande-StartVM** planning.
* Doel actie door abonnement, resourcegroep, en de Tag starten.  Zie de stappen in [Scenario #2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) plannen voor het testen en het inschakelen van 'Geordende-StartVM'.


### <a name="configuring-e-mail-notifications"></a>E-mailmeldingen configureren

E-mailmeldingen configureren nadat de oplossing is geïmplementeerd, kunt u de volgende drie variabelen wijzigen:

* External_EmailFromAddress - Geef het e-mailadres van afzender
* External_EmailToAddress - een door komma's gescheiden lijst met e-mailadressen (user@hotmail.com, user@outlook.com) voor het ontvangen van e-mailmeldingen
* External_IsSendEmail - als ingesteld op **Ja**, u ontvangen van e-mailberichten en e-mailmeldingen uitschakelen stelt u de waarde in op **Nee**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Wijzigen van de planningen voor opstarten en afsluiten

Het beheren van de schema's voor opstarten en afsluiten in deze oplossing volgt dezelfde stappen zoals wordt beschreven in [plannen van een runbook in Azure Automation](automation-schedules.md).     

## <a name="log-analytics-records"></a>Log Analytics-records

Met Automation worden twee typen records gemaakt in de OMS-opslagplaats.

### <a name="job-logs"></a>Taaklogboeken

Eigenschap | Beschrijving|
----------|----------|
Caller |  Wie de bewerking heeft gestart.  Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken.|
Category | Classificatie van het type gegevens.  Voor Automation is de waarde JobLogs.|
CorrelationId | De GUID die de correlatie-id van de runbooktaak is.|
JobId | De GUID die de id van de runbooktaak is.|
operationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd.  Voor Automation is de waarde Job.|
resourceId | Hiermee wordt het resourcetype in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
ResourceGroup | Hiermee wordt de resourcegroepnaam van de runbooktaak opgegeven.|
ResourceProvider | Hiermee wordt de Azure-service opgegeven waarmee de resources worden geleverd die u kunt implementeren en beheren.  Voor Automation is de waarde Azure Automation.|
ResourceType | Hiermee wordt het resourcetype in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
resultType | De status van de runbooktaak.  Mogelijke waarden zijn:<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>- Geslaagd|
resultDescription | Hiermee wordt resultaatstatus van de runbooktaak beschreven.  Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid|
RunbookName | Hiermee wordt de naam van het runbook opgegeven.|
SourceSystem | Hiermee wordt het bronsysteem voor de verzonden gegevens opgegeven.  Voor Automation is de waarde :OpsManager|
StreamType | Hiermee wordt het type gebeurtenis opgegeven. Mogelijke waarden zijn:<br>- Uitgebreid<br>- Uitvoer<br>- Fout<br>- Waarschuwing|
SubscriptionId | Hiermee wordt de abonnements-id van de taak opgegeven.
Time | Datum en tijd van uitvoering van de runbooktaak.|


### <a name="job-streams"></a>Taakstromen

Eigenschap | Beschrijving|
----------|----------|
Caller |  Wie de bewerking heeft gestart.  Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken.|
Category | Classificatie van het type gegevens.  Voor Automation is de waarde JobStreams.|
JobId | De GUID die de id van de runbooktaak is.|
operationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd.  Voor Automation is de waarde Job.|
ResourceGroup | Hiermee wordt de resourcegroepnaam van de runbooktaak opgegeven.|
resourceId | Hiermee wordt de resource-id in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
ResourceProvider | Hiermee wordt de Azure-service opgegeven waarmee de resources worden geleverd die u kunt implementeren en beheren.  Voor Automation is de waarde Azure Automation.|
ResourceType | Hiermee wordt het resourcetype in Azure opgegeven.  Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
resultType | Het resultaat van de runbooktaak op het moment dat de gebeurtenis werd gegenereerd.  Mogelijke waarden zijn:<br>- Wordt uitgevoerd|
resultDescription | Bevat de uitvoerstroom van het runbook.|
RunbookName | De naam van het runbook.|
SourceSystem | Hiermee wordt het bronsysteem voor de verzonden gegevens opgegeven.  Voor Automation is de waarde OpsManager|
StreamType | Het type taakstroom. Mogelijke waarden zijn:<br>- Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid|
Time | Datum en tijd van uitvoering van de runbooktaak.|

Wanneer u een zoekopdracht in logboeken uitvoert, waarmee records met de categorie **JobLogs** of **JobStreams** worden geretourneerd, kunt u de weergave **JobLogs** of **JobStreams** selecteren. Hiermee wordt een set tegels weergegeven waarop de updates worden samengevat die door de zoekopdracht worden geretourneerd.

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

De volgende tabel bevat voorbeeldzoekopdrachten in logboeken voor taakrecords die worden verzameld met deze oplossing. 

Query’s uitvoeren | Beschrijving|
----------|----------|
Taken voor runbook ScheduledStartStop_Parent die zijn met succes voltooid vinden | Categorie Zoek == "JobLogs" &#124; waar (RunbookName_s == "ScheduledStartStop_Parent") &#124; waar (ResultType == "Voltooid") &#124; overzicht van AggregatedValue = count() door ResultType, bin (TimeGenerated, 1 uur) &#124; TimeGenerated desc sorteren|
Taken voor runbook SequencedStartStop_Parent die zijn met succes voltooid vinden | Categorie Zoek == "JobLogs" &#124; waar (RunbookName_s == "SequencedStartStop_Parent") &#124; waar (ResultType == "Voltooid") &#124; overzicht van AggregatedValue = count() door ResultType, bin (TimeGenerated, 1 uur) &#124; TimeGenerated desc sorteren

## <a name="removing-the-solution"></a>Verwijderen van de oplossing

Als u dat u niet langer de oplossing verdere gebruiken besluit, kunt u deze verwijderen uit het Automation-account.  De oplossing alleen verwijdert, worden de runbooks, zullen niet worden verwijderd de schema's of variabelen die zijn gemaakt toen u de oplossing is toegevoegd.  Deze assets u handmatig verwijderen moet als u deze niet met andere runbooks gebruikt.  

Als u wilt verwijderen van de oplossing, kunt u de volgende stappen uitvoeren:

1.  Selecteer in uw Automation-account **werkruimte** in het linkerdeelvenster.  
2.  Op de **oplossingen** pagina, selecteert u de oplossing **Start Stop VM [werkruimte]**.  Op de **VMManagementSolution [werkruimte]** pagina van het menu klikt u op **verwijderen**.<br><br> ![Oplossing voor VM-Mgmt verwijderen](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  In de **oplossing verwijderen** venster bevestigen u wilt verwijderen van de oplossing.
4.  Terwijl de informatie is geverifieerd en de oplossing wordt verwijderd, u kunt de voortgang onder volgen **meldingen** in het menu.  U wordt teruggeleid naar de **oplossingen** pagina nadat het proces voor het verwijderen van de oplossing is gestart.  

De Automation-account en de werkruimte voor logboekanalyse niet verwijderd als onderdeel van dit proces.  Als u niet behouden van de werkruimte voor logboekanalyse wilt, moet u deze handmatig verwijderen.  Dit kan ook worden bereikt vanaf de Azure portal.   Selecteer in het-beginscherm in de Azure portal **logboekanalyse** en klik vervolgens op de **logboekanalyse** blade, selecteer de werkruimte en klik op **verwijderen** in het menu op de blade van de werkruimte instellingen.  
      
## <a name="next-steps"></a>Volgende stappen

- Zie [Log searches in Log Analytics (Logboekzoekopdrachten in Log Analytics)](../log-analytics/log-analytics-log-searches.md) voor meer informatie over het maken van verschillende zoekquery's en het controleren van de Automation-taaklogboeken met Log Analytics
- Zie [Track a runbook job (Runbooktaken bijhouden)](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details
- Zie voor meer informatie over Log Analytics en verzameling gegevensbronnen, [verzamelen van Azure storage-gegevens in het overzicht van de Log Analytics](../log-analytics/log-analytics-azure-storage.md)






   

