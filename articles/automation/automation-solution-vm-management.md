<properties
    pageTitle="Oplossing voor VM's starten/stoppen buiten kantooruren [Preview] | Microsoft Azure"
    description="Met de VM-beheeroplossing worden uw virtuele machines voor Azure Resource Manager gestart en gestopt volgens een planning en proactief bewaakt vanuit Log Analytics."
    services="automation"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/07/2016"
    ms.author="magoedte"/>


# Oplossing voor VM's starten/stoppen buiten kantooruren [Preview] in Automation

Met de oplossing voor het starten en stoppen van VM's buiten kantooruren [Preview] worden uw virtuele machines voor Azure Resource Manager volgens een door de gebruiker gedefinieerde planning gestart en gestopt. De oplossing biedt inzicht in het slagen van de Automation-taken waarmee uw virtuele machines worden gestart en gestopt met OMS Log Analytics.  

## Vereisten

- De runbooks werken met een [Uitvoeren als-account voor Azure](automation-sec-configure-azure-runas-account.md).  Het Uitvoeren als-account is de aanbevolen verificatiemethode omdat hiervoor verificatie via een certificaat wordt gebruikt, in plaats van een wachtwoord dat mogelijk vervalt of regelmatig wordt gewijzigd.  

- Met deze oplossing kunnen alleen VM's worden beheerd die zich in dezelfde abonnement- en resourcegroep bevinden als het Automation-account.  

- Deze oplossing wordt alleen in de volgende Azure-regio's geïmplementeerd: Australië - zuidoost, VS - oost, Zuidoost-Azië en West-Europa.  De runbooks waarmee de VM-planning wordt beheerd, kunnen op VM's in elke regio zijn gericht.  

- Voor het verzenden van e-mailmeldingen wanneer de VM-runbooks voor starten en stoppen zijn voltooid, is een Office 365-abonnement voor bedrijven vereist.  

## Oplossingsonderdelen

Deze oplossing bestaat uit de volgende resources die worden geïmporteerd en toegevoegd aan uw Automation-account.

### Runbooks

Runbook | Beschrijving|
--------|------------|
CleanSolution-MS-Mgmt-VM | Met dit runbook worden alle opgenomen resources en planningen verwijderd wanneer u de oplossing verwijdert uit uw abonnement.|  
SendMailO365-MS-Mgmt | Met dit runbook wordt een e-mail verzonden via Office 365 Exchange.|
StartByResourceGroup-MS-Mgmt-VM | Dit runbook is bedoeld om VM's (zowel klassieke als op ARM gebaseerde VM's) te starten die zich in een opgegeven lijst met Azure-resourcegroepen bevinden.
StopByResourceGroup-MS-Mgmt-VM | Dit runbook is bedoeld om VM's (zowel klassieke als op ARM gebaseerde VM's) te stoppen die zich in een opgegeven lijst met Azure-resourcegroepen bevinden.|
<br>

### Variabelen

Variabele | Beschrijving|
---------|------------|
Runbook **SendMailO365-MS-Mgmt** ||
SendMailO365-IsSendEmail-MS-Mgmt | Hiermee wordt opgegeven of met de runbooks StartByResourceGroup-MS-Mgmt-VM en StopByResourceGroup-MS-Mgmt-VM e-mailmeldingen kunnen worden verzonden na voltooiing.  Selecteer **True** om e-mailwaarschuwingen in te schakelen en **False** om e-mailwaarschuwingen uit te schakelen. De standaardwaarde is **False**.| 
Runbook **StartByResourceGroup-MS-Mgmt-VM** ||
StartByResourceGroup-ExcludeList-MS-Mgmt-VM | Geef VM-namen op die moeten worden uitgesloten van de beheerbewerking. Scheid de namen met een puntkomma (;). Waarden zijn hoofdlettergevoelig en het jokerteken (sterretje) wordt ondersteund.|
StartByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Tekst die kan worden toegevoegd aan het begin van de hoofdtekst van het e-mailbericht.|
StartByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Hiermee wordt de naam van het Automation-account opgegeven dat het e-mailrunbook bevat.  **Deze variabele mag niet worden gewijzigd.**|
StartByResourceGroup-SendMailO365-EmailRunbookName-MS-Mgmt | Hiermee wordt de naam van het e-mailrunbook opgegeven.  Dit wordt gebruikt voor de runbooks StartByResourceGroup-MS-Mgmt-VM en StopByResourceGroup-MS-Mgmt-VM om e-mailberichten te verzenden.  **Deze variabele mag niet worden gewijzigd.**|
StartByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Hiermee wordt de naam van de resourcegroep opgegeven die het e-mailrunbook bevat.  **Deze variabele mag niet worden gewijzigd.**|
StartByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Hiermee wordt de tekst voor de onderwerpregel van het e-mailbericht opgegeven.|  
StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Hiermee worden de ontvangers van het e-mailbericht opgegeven.  Voer afzonderlijke namen in met behulp van puntkomma (;).|
StartByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Geef VM-namen op die moeten worden uitgesloten van de beheerbewerking. Scheid de namen met een puntkomma (;). Waarden zijn hoofdlettergevoelig en het jokerteken (sterretje) wordt ondersteund.  De standaardwaarde (sterretje) omvat alle resourcegroepen in het abonnement.|
StartByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Hiermee wordt het abonnement opgegeven dat VM's bevat die met deze oplossing moeten worden beheerd.  Dit moet hetzelfde abonnement zijn als het abonnement waarin het Automation-account van deze oplossing zich bevindt.|
Runbook **StopByResourceGroup-MS-Mgmt-VM** ||
StopByResourceGroup-ExcludeList-MS-Mgmt-VM | Geef VM-namen op die moeten worden uitgesloten van de beheerbewerking. Scheid de namen met een puntkomma (;). Waarden zijn hoofdlettergevoelig en het jokerteken (sterretje) wordt ondersteund.|
StopByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Tekst die kan worden toegevoegd aan het begin van de hoofdtekst van het e-mailbericht.|
StopByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Hiermee wordt de naam van het Automation-account opgegeven dat het e-mailrunbook bevat.  **Deze variabele mag niet worden gewijzigd.**|
StopByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Hiermee wordt de naam van de resourcegroep opgegeven die het e-mailrunbook bevat.  **Deze variabele mag niet worden gewijzigd.**|
StopByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Hiermee wordt de tekst voor de onderwerpregel van het e-mailbericht opgegeven.|  
StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Hiermee worden de ontvangers van het e-mailbericht opgegeven.  Voer afzonderlijke namen in met behulp van puntkomma (;).|
StopByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Geef VM-namen op die moeten worden uitgesloten van de beheerbewerking. Scheid de namen met een puntkomma (;). Waarden zijn hoofdlettergevoelig en het jokerteken (sterretje) wordt ondersteund.  De standaardwaarde (sterretje) omvat alle resourcegroepen in het abonnement.|
StopByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Hiermee wordt het abonnement opgegeven dat VM's bevat die met deze oplossing moeten worden beheerd.  Dit moet hetzelfde abonnement zijn als het abonnement waarin het Automation-account van deze oplossing zich bevindt.|  
<br>

### Planningen

Planning | Beschrijving|
---------|------------|
StartByResourceGroup-schema-MS-Mgmt | Planning voor runbook StartByResourceGroup, die de VM’s opstart die door deze oplossing worden beheerd.|
StopByResourceGroup-Schedule-MS-Mgmt | Planning voor runbook StopByResourceGroup, die de VM’s afsluit die door deze oplossing worden beheerd.|

### Referenties

Referentie | Beschrijving|
-----------|------------|
O365Credential | Hiermee wordt een geldig Office 365-gebruikersaccount opgegeven om e-mailberichten te verzenden.  Alleen vereist als de variabele SendMailO365-IsSendEmail-MS-Mgmt is ingesteld op **True**.

## Configuratie

Voer de volgende stappen uit om de oplossing voor VM's starten/stoppen buiten kantooruren [Preview] toe te voegen aan uw Automation-account en vervolgens de variabelen te configureren om de oplossing aan te passen.

1. Selecteer in het beginscherm in de Azure Portal de tegel **Marketplace**.  Als de tegel niet meer aan het beginscherm is vastgemaakt, selecteert u **Nieuw** in het linker navigatiedeelvenster.  
2. Typ in de Marketplace-blade **VM starten** in het zoekvak en selecteer vervolgens de oplossing **VM's starten/stoppen buiten kantooruren [Preview]** in de zoekresultaten.  
3. Controleer de samenvattingsgegevens in de blade **VM's starten/stoppen buiten kantooruren [Preview]** voor de geselecteerde oplossing en klik vervolgens op **Maken**.  
4. De blade **Oplossing toevoegen** wordt weergegeven. Hierin wordt u gevraagd de oplossing te configureren voordat u deze in uw Automation-abonnement kunt importeren.<br><br> ![Blade Oplossing toevoegen voor VM-beheer](media/automation-solution-vm-management/vm-management-solution-add-solution-blade.png)<br><br>
5.  Selecteer op de blade **Oplossing toevoegen** de optie **Werkruimte**. Hier selecteert u een OMS-werkruimte die is gekoppeld aan hetzelfde Azure-abonnement als dat van het Automation-account of u maakt een nieuwe OMS-werkruimte.  Als u geen OMS-werkruimte hebt, kunt u **Nieuwe werkruimte maken** selecteren en op de blade **OMS-werkruimte** het volgende uitvoeren: 
   - Geef een naam op voor de nieuwe **OMS-werkruimte**.
   - Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
   - Voor **Resourcegroep** kunt u een nieuwe resourcegroep maken of een bestaande resourcegroep selecteren.  
   - Selecteer een **locatie**.  Momenteel zijn de enige locaties die kunnen worden geselecteerd **Australië - zuidoost**, **VS - oost**, **Zuidoost-Azië** en **West-Europa**.
   - Selecteer een **prijscategorie**.  De oplossing wordt in twee categorieën aangeboden: gratis categorie en betaalde categorie OMS.  De gratis categorie heeft een limiet op de hoeveelheid gegevens die dagelijks wordt verzameld, op de retentieperiode en op de runtimeminuten van runbooktaken.  De betaalde categorie OMS heeft geen limiet op de hoeveelheid gegevens die dagelijks wordt verzameld.  

        > [AZURE.NOTE]
        > Hoewel de betaalde categorie voor een zelfstandige oplossing als optie wordt weergegeven, is deze niet van toepassing.  Als u de optie selecteert en doorgaat met het maken van deze oplossing in uw abonnement, mislukt dit.  Dit wordt opgelost bij de officiële release van deze oplossing.<br>Als u deze oplossing gebruikt, worden alleen Automation-taakminuten en logopname gebruikt.  Met de oplossing worden geen aanvullende OMS-knooppunten aan uw omgeving toegevoegd.  

6. Nadat u de vereiste gegevens hebt opgegeven op de blade **OMS-werkruimte**, klikt u op **Maken**.  Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu.  U keert automatisch terug naar de blade **Oplossing toevoegen**.  
7. Selecteer op de blade **Oplossing toevoegen** de optie **Automation-account**.  Als u een nieuwe OMS-werkruimte maakt, moet u ook een nieuw Automation-account maken dat wordt gekoppeld aan de nieuwe OMS-werkruimte die eerder is opgegeven, inclusief uw Azure-abonnement, resourcegroep en regio.  U kunt **Een Automation-account maken** selecteren en het volgende opgeven op de blade **Automation-account toevoegen**: 
  - Voer in het veld **Naam** de naam van het Automation-account in.

    Alle overige opties worden automatisch ingevuld op basis van de geselecteerde OMS-werkruimte. Deze opties kunnen niet worden gewijzigd.  Een Uitvoeren als-account voor Azure is de standaardmethode voor verificatie voor de runbooks die zijn opgenomen in deze oplossing.  Nadat u op **OK** hebt geklikt, worden de configuratieopties gevalideerd en wordt het Automation-account gemaakt.  U kunt de voortgang bijhouden onder **Meldingen** in het menu. 

    U kunt ook een bestaand Uitvoeren als-account voor Automation selecteren.  Let op: het account dat u selecteert, mag niet al aan een andere OMS-werkruimte zijn gekoppeld, anders wordt er een bericht weergegeven in de blade om u op de hoogte te stellen.  Als het account al is gekoppeld, moet u een ander Uitvoeren als-account voor Automation selecteren of een nieuw account maken.<br><br> ![Automation-account is al gekoppeld aan OMS-werkruimte](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Selecteer ten slotte op de blade **Oplossing toevoegen** de optie **Configuratie**. De blade **Parameters** wordt weergegeven.  Op de blade **Parameters** wordt u het volgende gevraagd:  
   - Geef de **namen van de doel-ResourceGroup** op. Dit is een resourcegroepnaam die VM's bevat die met deze oplossing moeten worden beheerd.  U kunt meer dan één naam invoeren en de namen scheiden met een puntkomma (de waarden zijn hoofdlettergevoelig).  Jokertekens worden ondersteund als de bewerking moet worden gericht op VM's in alle resourcegroepen in het abonnement.
   - Selecteer een **planning**. Dit is een periodieke datum en tijd voor het starten en stoppen van de VM's in de doelresourcegroep(en).  

10. Zodra u klaar bent met het configureren van de eerste instellingen die zijn vereist voor de oplossing, selecteert u **Maken**.  Alle instellingen worden gevalideerd en vervolgens wordt geprobeerd de oplossing te implementeren in uw abonnement.  Dit proces kan enkele seconden duren en u kunt de voortgang bijhouden onder **Meldingen** in het menu. 

## Verzamelingsfrequentie

Taaklogboek- en taakstroomgegevens voor Automation worden elke vijf minuten in de OMS-opslagplaats opgenomen.  

## De oplossing gebruiken

Wanneer u de VM-beheeroplossing toevoegt, wordt in de OMS-werkruimte de tegel **Weergave StartStopVM** toegevoegd aan uw OMS-dashboard.  Op deze tegel worden een telling en een grafische weergave getoond van de runbooktaken voor de oplossing die zijn gestart en zijn voltooid.<br><br> ![Tegel Weergave StartStopVM voor VM-beheer](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

In het Automation-account kunt u de oplossing openen en beheren door de tegel **Oplossingen** te selecteren en door vervolgens op de blade **Oplossingen** de oplossing **Start-Stop-VM[Workspace]** in de lijst te selecteren.<br><br> ![Lijst met Automation-oplossingen](media/automation-solution-vm-management/vm-management-solution-autoaccount-solution-list.png)  

Als u de oplossing selecteert, wordt de oplossingsblade **Start-Stop-VM[Workspace]** weergegeven. Hier kunt u belangrijke details controleren zoals de tegel **StartStopVM**, zoals in uw OMS-werkruimte, waarop een telling en grafische weergave worden getoond van de runbooktaken voor de oplossing die zijn gestart en die zijn voltooid.<br><br> ![Blade VM-oplossing Automation](media/automation-solution-vm-management/vm-management-solution-solution-blade.png)  

Vanaf hier kunt u ook uw OMS-werkruimte openen en verdere analyse van de taakrecords uitvoeren.  Klik op **Alle instellingen** en selecteer in de blade **Instellingen** de optie **Snel starten**. Kies vervolgens op de blade **Snel starten** voor **OMS-portal**.   Hiermee opent u een nieuw tabblad of een nieuwe browsersessie en stelt u uw OMS-werkruimte voor die aan Automation-account en abonnement is gekoppeld.  


### E-mailmeldingen configureren

Als u e-mailmeldingen wilt inschakelen voor wanneer de runbooks voor het starten en stoppen van VM's zijn voltooid, moet u de referentie **O365Credential** wijzigen en ook minimaal de volgende variabelen:

 - SendMailO365-IsSendEmail-MS-Mgmt
 - StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt
 - StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt

Als u de referentie **O365Credential** wilt configureren, voert u de volgende stappen uit:

1. Klik in het Automation-account op **Alle instellingen** bovenaan het venster. 
2. Selecteer op de blade **Instellingen** onder de sectie **Automation-resources** de optie **Assets**. 
3. Selecteer op de blade **Assets** de tegel **Referentie** en selecteer op de blade **Referentie** de optie **O365Credential**.  
4. Voer een geldige gebruikersnaam en geldig wachtwoord voor Office 365 in en klik vervolgens op **Opslaan** om uw wijzigingen op te slaan.  

Als u de variabelen wilt configureren die eerder zijn besproken, voert u de volgende stappen uit:

1. Klik in het Automation-account op **Alle instellingen** bovenaan het venster. 
2. Selecteer op de blade **Instellingen** onder de sectie **Automation-resources** de optie **Assets**. 
3. Selecteer op de blade **Assets** de tegel **Variabelen** en selecteer op de blade **Variabelen** de variabele die hierboven wordt vermeld. Wijzig vervolgens de waarde ervan door de omschrijving te volgen die ervoor is opgegeven in de eerdere sectie [Variabelen](##variables).  
4. Klik op **Opslaan** om de wijzigingen van de variabele op te slaan.   

### De planning voor opstarten en afsluiten wijzigen

Voor het beheren van de planning voor opstarten en afsluiten in deze oplossing volgt u dezelfde stappen zoals beschreven in [Een runbook in Azure Automation plannen](automation-scheduling-a-runbook.md).  Denk eraan dat u de planningsconfiguratie niet kunt wijzigen.  U moet de bestaande planning uitschakelen, dan een nieuwe maken en deze vervolgens koppelen aan de runbook **StartByResourceGroup-MS-Mgmt-VM** of **StopByResourceGroup-MS-Mgmt-VM** waarop u de planning wilt toepassen.   

## Log Analytics-records

Met Automation worden twee typen records gemaakt in de OMS-opslagplaats.

### Taaklogboeken

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


### Taakstromen

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

## Voorbeeldzoekopdrachten in logboeken

De volgende tabel bevat voorbeeldzoekopdrachten in logboeken voor taakrecords die worden verzameld met deze oplossing. 

Query’s uitvoeren | Beschrijving|
----------|----------|
Taken zoeken voor runbook StartVM die zijn voltooid | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Succeeded &#124; measure count() by JobId_g|
Taken zoeken voor runbook StopVM die zijn voltooid | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Failed &#124; measure count() by JobId_g
Taakstatus gedurende een periode weergeven voor runbooks StartVM en StopVM | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" OR "StopByResourceGroup-MS-Mgmt-VM" NOT(ResultType="started") | measure Count() by ResultType interval 1day|

## Volgende stappen

- Zie [Log searches in Log Analytics (Logboekzoekopdrachten in Log Analytics)](../log-analytics/log-analytics-log-searches.md) voor meer informatie over het maken van verschillende zoekquery's en het controleren van de Automation-taaklogboeken met Log Analytics
- Zie [Track a runbook job (Runbooktaken bijhouden)](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details
- Zie [Collecting Azure storage data in Log Analytics overview (Overzicht Azure-opslaggegevens verzamelen in Log Analytics)](../log-analytics/log-analytics-azure-storage.md) voor meer informatie over OMS Log Analytics en gegevensverzamelingsbronnen






   




<!--HONumber=Oct16_HO3-->


