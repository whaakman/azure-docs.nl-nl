---
title: Optimalisatie van uw System Center Operations Manager-omgeving met Azure Log Analytics | Microsoft Docs
description: U kunt de oplossing System Center Operations Manager Health Check gebruiken om het risico en de status van uw omgeving op een vast interval vast te stellen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: c17c14b015531bf3f978ac30b013c80ad5be779e
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51006512"
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-health-check-preview-solution"></a>Optimalisatie van uw omgeving met de oplossing System Center Operations Manager Health Check (Preview)

![System Center Operations Manager Health Check symbool](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

U kunt de oplossing System Center Operations Manager Health Check gebruiken om het risico en de status van uw System Center Operations Manager-beheergroep op een vast interval vast te stellen. Dit artikel helpt u bij het installeren, configureren en gebruiken van de oplossing zodat u corrigerende maatregelen voor potentiële problemen kunt uitvoeren.

Deze oplossing biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor uw geïmplementeerde serverinfrastructuur. De aanbevelingen zijn onderverdeeld in vier gebieden, waarmee u snel inzicht in het risico en neemt u corrigerende maatregelen.

De aanbevelingen zijn gebaseerd op de kennis en ervaring met Microsoft-engineers van duizenden klanten bezoeken. Elke aanbeveling bevat richtlijnen over waarom een probleem voor u mogelijk belangrijk en het implementeren van de voorgestelde wijzigingen.

U kunt gebieden die het meest relevant zijn voor uw organisatie en de voortgang naar het uitvoeren van een risico's gratis en gezonde omgeving bijhouden.

Nadat u de oplossing hebt toegevoegd en een evaluatie uitgevoerd, samenvattende wordt informatie voor focusgebieden wordt weergegeven op de **System Center Operations Manager Health Check** dashboard voor uw infrastructuur. De volgende secties wordt beschreven hoe u gebruik van de informatie op de **System Center Operations Manager Health Check** dashboard, waar u kunt weergeven en vervolgens ondernemen aanbevolen acties voor uw Operations Manager-omgeving.

![Tegel voor System Center Operations Manager-oplossing](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-tile.png)

![System Center Operations Manager Health Check-dashboard](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-dashboard-01.png)

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

De oplossing werkt met Microsoft System Operations Manager 2012 Service Pack (SP) 1 en 2012 R2.

Gebruik de volgende informatie om de oplossing te installeren en configureren.

 - Voordat u de oplossing statuscontrole in Log Analytics gebruiken kunt, moet u de oplossing die is geïnstalleerd hebben. Installeren van de oplossing van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview).

 - Na het toevoegen van de oplossing naar de werkruimte, de **System Center Operations Manager Health Check** tegel op het dashboard wordt een aanvullende configuratie vereist weergegeven. Klik op de tegel en volg de configuratiestappen die worden vermeld op de pagina

 ![System Center Operations Manager-dashboardtegel](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

> [!NOTE]
> Configuratie van System Center Operations Manager kan worden gedaan met behulp van een script met de volgende stappen die worden vermeld in de configuratiepagina van de oplossing in Log Analytics.

 Voor het configureren van de evaluatie via Operations Manager Operations-console, moet u de stappen hieronder uitvoeren in de volgende volgorde:
1. [Uitvoeren als-account instellen voor System Center Operations Manager Health Check](#operations-manager-run-as-accounts-for-log-analytics)  
2. [De System Center Operations Manager Health Check regel configureren](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>Details van System Center Operations Manager-evaluatie gegevens verzameling

De evaluatie van de System Center Operations Manager verzamelt gegevens uit de volgende bronnen:

* Register
* Windows Management Instrumentation (WMI)
* Gebeurtenislogboek
* Gegevens uit een bestand
* Rechtstreeks vanaf de Operations Manager met behulp van PowerShell en SQL-query's, vanaf een beheerserver die u hebt opgegeven.  

Gegevens worden verzameld op de beheerserver en doorgestuurd naar Log Analytics in de zeven dagen.  

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Operations Manager run as-accounts voor Log Analytics

Log Analytics bouwt voort op de management packs voor workloads voor toevoegen waarde-services. Elke werkbelasting vereist workload-specifieke machtigingen voor het uitvoeren van management packs in een andere beveiligingscontext, zoals een domeingebruikersaccount. Een Operations Manager runas-account configureren met referenties met bevoegdheden. Zie voor meer informatie, [over het maken van een uitvoeren als-account](https://technet.microsoft.com/library/hh321655(v=sc.12).aspx) in de Operations Manager-documentatie.

Gebruik de volgende informatie om in te stellen van de Operations Manager runas-account voor System Center Operations Manager Health Check.

### <a name="set-the-run-as-account"></a>Uitvoeren als-account instellen

Uitvoeren als-account moet voldoen aan de volgende vereisten voordat u doorgaat:

* Een domeingebruikersaccount die lid is van de lokale groep Administrators op alle servers die ondersteuning bieden een rol van Operations Manager - beheerserver, SQL Server die als host fungeert de operationele database, datawarehouse en ACS-database, rapportage, Web-console en Gateway-server.
* Bewerking Manager Administrator-rol voor de beheergroep worden beoordeeld
* Als het account heeft geen SQL sysadmin-rechten, voer de [script](#sql-script-to-grant-granular-permissions-to-the-run-as-account) om gedetailleerde machtigingen aan het account op elk exemplaar van SQL Server die als host fungeert voor een of meer van de Operations Manager-databases te verlenen.

1. Selecteer in de Operations Manager-Console de **beheer** navigatieknop.
2. Onder **Run As-configuratie**, klikt u op **Accounts**.
3. In de **uitvoeren als-Account** Wizard, op de **inleiding** pagina op **volgende**.
4. Op de **algemene eigenschappen** weergeeft, schakelt **Windows** in de **type Run As-Account:** lijst.
5. Typ een weergavenaam in de **weergavenaam** tekst in en typ desgewenst een beschrijving in de **beschrijving** vak en klik vervolgens op **volgende**.
6. Op de **Distributiebeveiliging** weergeeft, schakelt **veiliger**.
7. Klik op **Create**.  

Nu dat de Run As-account is gemaakt, moet de doel-beheerservers in de beheergroep en die zijn gekoppeld aan een vooraf gedefinieerde Run As-profiel, zodat werkstromen worden uitgevoerd met de referenties.  

1. Onder **Run As-configuratie**, **Accounts**, dubbelklik in het deelvenster met resultaten op het account dat u eerder hebt gemaakt.
2. Op de **distributie** tabblad **toevoegen** voor de **selecteerde computers** en toe te voegen van de beheerserver voor het distribueren van het account.  Klik op **OK** tweemaal de wijzigingen wilt opslaan.
3. Onder **Run As-configuratie**, klikt u op **profielen**.
4. Zoek de *SCOM-evaluatie profiel*.
5. Naam van het profiel moet zijn: *Microsoft System Center Advisor SCOM-evaluatie Run As-profiel*.
6. Met de rechtermuisknop op en de eigenschappen ervan bijwerken en toevoegen van de onlangs gemaakte uitvoeren als-Account u eerder hebt gemaakt.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>SQL-script om gedetailleerde machtigingen voor het uitvoeren als-account te verlenen

Voer het volgende SQL-script om de vereiste machtigingen verlenen voor het uitvoeren als-account op de SQL Server-exemplaar dat is gebruikt door Operations Manager die als host fungeert de operationele database, datawarehouse en ACS-database.

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```

### <a name="configure-the-health-check-rule"></a>De regel voor het selectievakje health configureren

De System Center Operations Manager Health Check solution managementpack bevat een regel met de naam *Microsoft System Center Advisor SCOM-evaluatie evaluatie van de regel uitvoeren*. Deze regel is verantwoordelijk voor het uitvoeren van de statuscontrole. De regel in te schakelen en configureer de frequentie, gebruik de onderstaande procedures.

De Microsoft System Center Advisor SCOM beoordeling uitvoeren evaluatie van de regel is standaard uitgeschakeld. Als u wilt de statuscontrole uitvoeren, moet u de regel op een beheerserver inschakelen. Gebruik de volgende stappen.

#### <a name="enable-the-rule-for-a-specific-management-server"></a>De regel voor een specifieke beheerserver inschakelen

1. In de **ontwerpen** werkruimte van de Operations Manager Operations-console, zoekt u de regel *Microsoft System Center Advisor SCOM-evaluatie evaluatie van de regel uitvoeren* in de **regels** deelvenster.
2. In de lijst met zoekresultaten de optie met de tekst *Type: beheerserver*.
3. Met de rechtermuisknop op de regel en klik vervolgens op **onderdrukkingen** > **voor een specifiek object van klasse: beheerserver**.
4.  Selecteer in de beschikbare lijst met beheerservers, de beheerserver waar de regel moet worden uitgevoerd.  Dit moet u eerder hebt geconfigureerd om te koppelen van de uitvoeren als-account met dezelfde beheerserver.
5.  Zorg ervoor dat u de waarde voor overschrijven om te wijzigen **waar** voor de **ingeschakeld** parameterwaarde.<br><br> ![parameter overschrijven](./media/log-analytics-scom-assessment/rule.png)

    Terwijl u nog steeds in dit venster, de frequentie van het type uitvoeren met behulp van de volgende procedure te configureren.

#### <a name="configure-the-run-frequency"></a>Configureer de frequentie uitvoeren

De evaluatie is standaard geconfigureerd voor het uitvoeren van elke 10.080 minuten (of zeven dagen). De waarde moet een minimumwaarde van 1440 minuten (of één dag), kunt u negeren. De waarde vertegenwoordigt de minimumtijd kloof tussen opeenvolgende evaluatie wordt uitgevoerd. Als u wilt overschrijven het interval, gebruik de volgende stappen.

1. In de **ontwerpen** werkruimte van de Operations Manager-console, zoekt u de regel *Microsoft System Center Advisor SCOM-evaluatie evaluatie van de regel uitvoeren* in de **regels** de sectie.
2. In de lijst met zoekresultaten de optie met de tekst *Type: beheerserver*.
3. Met de rechtermuisknop op de regel en klik vervolgens op **de regel onderdrukken** > **voor alle objecten van klasse: beheerserver**.
4. Wijzig de **Interval** parameterwaarde uw gewenste intervalwaarde. In het onderstaande voorbeeld is de waarde ingesteld op 1440 minuten (één dag).<br><br> ![interval voor parameter](./media/log-analytics-scom-assessment/interval.png)<br>  

    Als de waarde is ingesteld op minder dan 1440 minuten, klikt u vervolgens de regel wordt uitgevoerd in een interval van één dag. In dit voorbeeld de regel de intervalwaarde wordt genegeerd en wordt uitgevoerd met een frequentie van één dag.


## <a name="understanding-how-recommendations-are-prioritized"></a>Inzicht krijgen in hoe aanbevelingen met prioriteit wordt toegepast

Elke aanbeveling krijgt een waarde valutaconversie in gevallen waarin het relatieve belang van de aanbeveling. Alleen de 10 belangrijkste aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Hoe het gewicht worden berekend

Wegingen zijn samengevoegde waarden op basis van drie belangrijke factoren:

- De *kans* dat een probleem dat is geïdentificeerd ervoor zorgen problemen dat. Een grotere kans is gelijk aan een grotere totale score voor de aanbeveling.
- De *impact* van het probleem op uw organisatie als dit leidt een probleem tot. Er is een hogere impact gelijk aan een grotere totale score voor de aanbeveling.
- De *inspanning* vereist de aanbeveling kunt implementeren. Er is een hogere inspanning gelijk aan een kleinere totale score voor de aanbeveling.

Het gewicht voor elke aanbeveling wordt uitgedrukt als een percentage van de totale score voor elke aandachtsgebied beschikbaar. Bijvoorbeeld, als een aanbeveling in de beschikbaarheid en bedrijfscontinuïteit aandachtsgebied heeft een score van 5%, verhoogd uitvoering van deze aanbeveling uw algehele beschikbaarheid en bedrijfscontinuïteit score van 5%.

### <a name="focus-areas"></a>Focusgebieden

**Beschikbaarheid en bedrijfscontinuïteit** -deze aandachtsgebied bevat aanbevelingen voor beschikbaarheid van de service, de tolerantie van uw infrastructuur en zakelijke beveiliging.

**Prestaties en schaalbaarheid** -deze aandachtsgebied bevat aanbevelingen voor uw organisatie het IT-infrastructuur groeien, zorg ervoor dat uw IT-omgeving voldoet aan de huidige prestatievereisten en kan reageren op veranderende infrastructuur behoeften.

**Upgraden, migreren en implementatie** -deze aandachtsgebied bevat aanbevelingen voor het upgraden, migreren en implementeren van SQL Server op uw bestaande infrastructuur.

**Bewerkingen en bewaking** -deze aandachtsgebied bevat aanbevelingen voor het stroomlijnen van uw IT-activiteiten, preventief onderhoud implementeren en prestaties te maximaliseren.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u erop gericht om 100% te beoordelen in elke aandachtsgebied?

Dat hoeft niet. De aanbevelingen zijn gebaseerd op de kennis en ervaringen die zijn verkregen dankzij het Microsoft-technici via duizenden klanten bezoeken. Echter geen twee server-infrastructuur zijn hetzelfde, en specifieke aanbevelingen mogelijk meer of minder relevant voor u. Bijvoorbeeld, enkele aanbevelingen voor beveiliging mogelijk minder relevant zijn als uw virtuele machines worden niet blootgesteld aan Internet. Aanbevelingen voor sommige beschikbaarheid mogelijk minder relevant zijn voor services met lage prioriteit ad-hoc gegevens verzamelen en rapporteren. Problemen die tot een volwassen bedrijf van belang zijn mogelijk minder belangrijk voor een lid is. Kunt u om te bepalen welke focusgebieden zijn uw prioriteiten en kijken hoe uw scores na verloop van tijd veranderen.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. Gebruik deze richtlijnen om te evalueren of het implementeren van de aanbeveling geschikt voor u is, gezien de aard van uw IT-services en de zakelijke eisen van uw organisatie.

## <a name="use-health-check-focus-area-recommendations"></a>Gebruik statuscontrole focus gebied aanbevelingen

Voordat u een oplossing status controleren in Log Analytics gebruiken kunt, moet u de oplossing geïnstalleerd hebben. Meer informatie over het installeren van oplossingen, Zie [installeren van een oplossing voor](../monitoring/monitoring-solutions.md). Nadat deze is geïnstalleerd, kunt u het overzicht van de aanbevelingen weergeven met behulp van de System Center Operations Manager Health Check-tegel op de **overzicht** pagina voor uw werkruimte in de Azure-portal.

De beoordelingen samengevatte naleving voor uw infrastructuur en aanbevelingen voor inzoomen in weergeven.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een aandachtsgebied weergeven en neemt u corrigerende maatregelen
1. Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
3. In het deelvenster voor abonnementen van Log Analytics een werkruimte selecteren en klik vervolgens op de **werkruimte overzicht** menu-item.  
4. Op de **overzicht** pagina, klikt u op de **System Center Operations Manager Health Check** tegel.
5. Op de **System Center Operations Manager Health Check** pagina, Controleer de samenvattingsinformatie in een van de focus gebied blades en klik vervolgens op een om aanbevelingen voor die aandachtsgebied weer te geven.
6. Op een van de focus gebiedspagina's, kunt u de aanbevelingen met prioriteit gemaakt voor uw omgeving bekijken. Klik op een aanbeveling onder **betrokken objecten** voor meer informatie over waarom de aanbeveling is gedaan.<br><br> ![aandachtsgebied](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-dashboard-02.png)<br>
7. U kunt ondernemen corrigerende maatregelen voorgesteld in **voorgestelde acties**. Wanneer het item is opgelost, record hoger beoordelingen die aanbevolen acties zijn uitgevoerd en worden uw nalevingsscore verhoogd. Gecorrigeerde items worden weergegeven als **doorgegeven objecten**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren

Als u aanbevelingen die u wilt negeren hebt, kunt u een tekstbestand met Log Analytics wordt gebruikt om te voorkomen dat de aanbevelingen wordt weergegeven in de resultaten van de evaluatie maken.

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>Voor het identificeren van de aanbevelingen die u wilt negeren
1. In de Azure-portal op de pagina van de Log Analytics-werkruimte voor de geselecteerde werkruimte, klikt u op de **zoeken in logboeken** menu-item.
2. Gebruik de volgende query uit om de lijst met aanbevelingen die zijn mislukt voor computers in uw omgeving.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe met Log Analytics-querytaal](log-analytics-queries.md), en vervolgens de bovenstaande query's gewijzigd in het volgende.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Hier volgt een schermopname van de zoeken in Logboeken-query:<br><br> ![zoeken in logboeken](./media/log-analytics-scom-assessment/scom-log-search.png)<br>

3. Kies de aanbevelingen die u wilt negeren. U gebruikt de waarden voor RecommendationId in de volgende procedure.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Het maken en gebruiken van een tekstbestand IgnoreRecommendations.txt

1. Maak een bestand met de naam IgnoreRecommendations.txt.
2. Plak of typ elke RecommendationId voor elke aanbeveling dat u wilt dat in Log Analytics op een afzonderlijke regel negeren en vervolgens opslaan en sluiten van het bestand.
3. Plaats het bestand in de volgende map op elke computer waarop u Log Analytics voor het negeren van aanbevelingen.
4. Op de beheerserver van Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te controleren of dat er aanbevelingen worden genegeerd

1. Na de volgende evaluatie wordt uitgevoerd, wordt standaard elke zeven dagen geplande, wordt de opgegeven aanbevelingen zijn gemarkeerd als genegeerd en wordt niet weergegeven op het statusdashboard van selectievakje.
2. U kunt de volgende zoeken in Logboeken-query's gebruiken om de genegeerde aanbevelingen weer te geven.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe met Log Analytics-querytaal](log-analytics-queries.md), en vervolgens de bovenstaande query's gewijzigd in het volgende.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Ignore" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Als u later besluit dat u wilt zien genegeerde aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of kunt u RecommendationIDs verwijderen uit deze.

## <a name="system-center-operations-manager-health-check-solution-faq"></a>System Center Operations Manager Health Check oplossing Veelgestelde vragen

*Ik heb de oplossing statuscontrole toegevoegd aan mijn werkruimte voor logboekanalyse. Maar ik zie niet de aanbevelingen. Waarom niet?* Na het toevoegen van de oplossing, gebruikt u de volgende stappen weergave de aanbevelingen in de Log Analytics-dashboard.  

- [Uitvoeren als-account instellen voor System Center Operations Manager Health Check](#operations-manager-run-as-accounts-for-log-analytics)  
- [De System Center Operations Manager Health Check regel configureren](#configure-the-health-check-rule)


*Is er een manier om te configureren hoe vaak de controle wordt uitgevoerd?* Ja. Zie [configureren van de run frequentie](#configure-the-run-frequency).

*Als een andere server wordt gedetecteerd nadat ik de oplossing System Center Operations Manager-evaluatie hebt toegevoegd, wordt deze gecontroleerd?* Ja, na de detectie wordt deze gecontroleerd vanaf standaard elke zeven dagen.

*Wat is de naam van het proces dat het verzamelen van gegevens wordt?* AdvisorAssessment.exe

*Waar wordt het proces AdvisorAssessment.exe uitgevoerd?* AdvisorAssessment.exe wordt uitgevoerd onder het proces van de Health-service van de beheerserver waar de statusregel selectievakje is ingeschakeld. Met dit proces, is detectie van uw hele omgeving via externe gegevensverzameling bereikt.

*Hoe lang duurt het voor het verzamelen van gegevens?* Het verzamelen van gegevens op de server duurt ongeveer één uur. Het kan langer duren in omgevingen met veel exemplaren van de Operations Manager of databases.

*Wat gebeurt er als ik het interval van de evaluatie op minder dan 1440 minuten instellen?* De evaluatie is vooraf geconfigureerd om uit te voeren met een maximum van één keer per dag. Als u de waarde voor interval voor een waarde minder dan 1440 minuten overschrijft, gebruikt de evaluatie 1440 minuten als de waarde voor interval.

*Hoe weet ik of er vereiste fouten zijn?* Als de statuscontrole is uitgevoerd en er geen resultaten, is het waarschijnlijk dat enkele van de vereisten voor de controle is mislukt. U kunt query's uitvoeren: `Operation Solution=SCOMAssessment` en `SCOMAssessmentRecommendation FocusArea=Prerequisites` in zoeken in Logboeken om te zien van de vereisten is mislukt.

*Er is een `Failed to connect to the SQL Instance (….).` bericht in de vereiste fouten. Wat is het probleem?* AdvisorAssessment.exe, het proces dat gegevens verzamelt, waaronder het proces van de Health-service wordt uitgevoerd op de beheerserver. Als onderdeel van de statuscontrole, wordt het proces probeert verbinding maken met de SQL-Server waar de Operations Manager-database aanwezig is. Deze fout kan optreden als firewallregels de verbinding met de SQL Server-exemplaar blokkeert.

*Welk type gegevens worden verzameld?* De volgende typen gegevens worden verzameld: - WMI-gegevens - register gegevens - gegevens van EventLog - Operations Manager-gegevens via Windows PowerShell, SQL-query's en bestanden verzamelen van informatie.

*Waarom heb ik het configureren van uitvoeren als-Account?* Met Operations Manager, worden verschillende SQL-query's uitgevoerd. Zodat ze om uit te voeren, moet u een Run As-Account met machtigingen die nodig zijn. Bovendien zijn lokale beheerdersreferenties vereist voor query uitvoeren op WMI.

*Waarom worden alleen de top 10 aanbevelingen weergegeven?* In plaats van geeft u een uitgebreide en overweldigend lijst van taken, is het raadzaam dat u zich richten op de aanbevelingen met prioriteit eerst adressering. Nadat u deze kunt oplossen, worden extra aanbevelingen beschikbaar. Als u liever de gedetailleerde lijst, kunt u alle aanbevelingen met behulp van zoeken in Logboeken kunt weergeven.

*Is er een manier om een aanbeveling negeren?* Ja, Zie de [aanbevelingen negeren](#Ignore-recommendations).


## <a name="next-steps"></a>Volgende stappen

- [Zoeken in logboeken](log-analytics-queries.md) voor meer informatie over het analyseren van gedetailleerde gegevens voor System Center Operations Manager Health Check en aanbevelingen.
