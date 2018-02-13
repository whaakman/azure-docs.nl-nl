---
title: Uw System Center Operations Manager-omgeving met een Azure-logboekanalyse optimaliseren | Microsoft Docs
description: De System Center Operations Manager Health Check-oplossing kunt u de risico's en de status van uw omgevingen te beoordelen op een vast interval.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 86484ca2bc7dc14035f48b8f7b1514a4fc471b74
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-health-check-preview-solution"></a>Optimalisatie van uw omgeving met de oplossing voor System Center Operations Manager Health controleren (Preview)

![Symbool van System Center Operations Manager Health controleren](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

U kunt de oplossing voor System Center Operations Manager Health controleren gebruiken voor het evalueren van de risico's en de status van uw beheergroep van System Center Operations Manager op een vast interval. In dit artikel helpt u bij het installeren, configureren en gebruiken van de oplossing, zodat u corrigerende maatregelen op mogelijke problemen kunt uitvoeren.

Deze oplossing biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor uw geïmplementeerde serverinfrastructuur. De aanbevelingen worden onderverdeeld in vier focusgebieden, waarmee u snel het risico begrijpt en herstelacties uitvoeren.

De aanbevelingen zijn gebaseerd op de kennis en ervaring door Microsoft engineers van duizenden klant bezoeken. Elke aanbeveling bevat richtlijnen over waarom er een probleem mogelijk interessant voor u en het implementeren van de voorgestelde wijzigingen.

U kunt focusgebieden die het meest belangrijk zijn voor uw organisatie en het bijhouden van uw progressie ten opzichte van een omgeving met risico gratis en goed wordt uitgevoerd.

Nadat u de oplossing hebt toegevoegd en een beoordeling uitgevoerd, samenvattende wordt informatie voor focusgebieden wordt weergegeven op de **System Center Operations Manager Health controleren** dashboard voor uw infrastructuur. De volgende secties wordt beschreven hoe u de informatie te gebruiken op de **System Center Operations Manager Health controleren** dashboard, waar u kunt weergeven en vervolgens te aanbevolen acties voor uw Operations Manager-omgeving.

![System Center Operations Manager-oplossing tegel](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-tile.png)

![Dashboard voor System Center Operations Manager Health controleren](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-dashboard-01.png)

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

De oplossing werkt met Microsoft System Operations Manager 2012 Service Pack (SP) 1 en 2012 R2.

Gebruik de volgende informatie om te installeren en configureren van de oplossing.

 - Voordat u de oplossing Serverstatus controleren in Log Analytics gebruiken kunt, moet u de oplossing geïnstalleerd hebben. Installeren van de oplossing van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview).

 - Na het toevoegen van de oplossing naar de werkruimte de **System Center Operations Manager Health controleren** tegel op het dashboard u ontvangt een aanvullende configuratie vereist. Klik op de tegel en volg de configuratiestappen die worden vermeld op de pagina

 ![Dashboardtegel voor System Center Operations Manager](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

> [!NOTE]
> Configuratie van System Center Operations Manager kan worden gedaan met behulp van een script door de stappen in de configuratiepagina van de oplossing in logboekanalyse genoemd.

 Voor het configureren van de beoordeling via de Operations Manager, Operations-console, moet u de onderstaande stappen uitvoeren in de volgende volgorde:
1. [De Run As-account instellen voor System Center Operations Manager Health controleren](#operations-manager-run-as-accounts-for-log-analytics)  
2. [De System Center Operations Manager Health controleren regel configureren](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>System Center Operations Manager assessment Gegevensdetails-verzameling

De evaluatie van System Center Operations Manager verzamelt gegevens uit de volgende bronnen:

* Register
* Windows Management Instrumentation (WMI)
* Gebeurtenislogboek
* Bestandsgegevens
* Rechtstreeks vanuit de Operations Manager met behulp van PowerShell en SQL-query's van een beheerserver die u hebt opgegeven.  

Gegevens worden verzameld op de beheerserver en doorgestuurd naar Log Analytics elke zeven dagen.  

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Operations Manager run as-accounts voor logboekanalyse

Log Analytics bouwt voort op de management packs voor werkbelastingen voor toevoegen waarde-services. Elke werkbelasting vereist werklastspecifiek bevoegdheden voor het uitvoeren van management packs in een andere beveiligingscontext, zoals een domeingebruikersaccount. Configureer een Manager Operations die Run As-account met beschermde referenties. Zie voor meer informatie, [het maken van een Run As-account](https://technet.microsoft.com/library/hh321655(v=sc.12).aspx) in de Operations Manager-documentatie.

Gebruik de volgende informatie in te stellen van de Operations Manager Run As-account voor voor System Center Operations Manager Health controleren.

### <a name="set-the-run-as-account"></a>De Run As-account instellen

De Run As-account moet voldoen aan de volgende vereisten voordat u verdergaat:

* Een domeingebruikersaccount die lid is van de lokale groep Administrators op alle servers ondersteunen alle functies van Operations Manager - beheerserver, de SQL-Server met de operationele, de datawarehouse en de ACS-database, rapportage, Web-console en de gatewayserver.
* Bewerking Manager beheerdersrol voor de beheergroep wordt beoordeeld
* Als het account geen sysadmin-rechten voor SQL heeft, wordt uitgevoerd de [script](#sql-script-to-grant-granular-permissions-to-the-run-as-account) gedetailleerde machtigen om het account op elk exemplaar van SQL Server die als host fungeert voor een of meer van de Operations Manager-databases.

1. Selecteer in de Operations Manager-Console de **beheer** navigatieknop klikt.
2. Onder **Run As-configuratie**, klikt u op **Accounts**.
3. In de **uitvoeren als-Account** Wizard op de **inleiding** pagina op **volgende**.
4. Op de **algemene eigenschappen** pagina **Windows** in de **Run As-Account-type:** lijst.
5. Typ een weergavenaam in de **weergavenaam** tekst en typt u desgewenst een beschrijving in de **beschrijving** vak en klik vervolgens op **volgende**.
6. Op de **Distributiebeveiliging** pagina **veiliger**.
7. Klik op **Create**.  

Nu dat de Run As-account is gemaakt, moet de doel-management-servers in de beheergroep en die zijn gekoppeld aan een vooraf gedefinieerde Run As-profiel zodat werkstromen worden uitgevoerd met de referenties.  

1. Onder **Run As-configuratie**, **Accounts**, dubbelklik in het deelvenster met resultaten op het account dat u eerder hebt gemaakt.
2. Op de **distributie** tabblad **toevoegen** voor de **selecteerde computers** en toe te voegen van de beheerserver om het account te distribueren.  Klik op **OK** tweemaal de wijzigingen wilt opslaan.
3. Onder **Run As-configuratie**, klikt u op **profielen**.
4. Zoeken naar de *SCOM Assessment profiel*.
5. Naam van het profiel moet: *Microsoft System Center Advisor SCOM Assessment Run As-profiel*.
6. Met de rechtermuisknop op en de eigenschappen ervan bijwerken en toevoegen van de laatst gemaakte Run As-Account u eerder hebt gemaakt.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>SQL-script gedetailleerde machtigen om de Run As-account

De volgende SQL-script voor het verlenen van de vereiste machtigingen om het Run As-account op de SQL Server-exemplaar gebruikt door Operations Manager hosten van de operationele, de datawarehouse en de ACS-database te worden uitgevoerd.

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

### <a name="configure-the-health-check-rule"></a>De controle van statusregel configureren

De System Center Operations Manager Health controleren oplossing management pack bevat een regel met naam *Microsoft System Center Advisor SCOM Assessment Assessment regel uitvoeren*. Deze regel is verantwoordelijk voor het uitvoeren van de statuscontrole. Regel voor het inschakelen en configureer de frequentie, gebruik de onderstaande procedures.

De Microsoft System Center Advisor SCOM Assessment uitvoeren Assessment regel is standaard uitgeschakeld. Als u wilt de statuscontrole uitvoeren, moet u de regel op een beheerserver inschakelen. Gebruik de volgende stappen.

#### <a name="enable-the-rule-for-a-specific-management-server"></a>De regel voor een specifieke beheerserver inschakelen

1. In de **ontwerpen** werkruimte van de Operations Manager, Operations-console, zoekt u de regel *Microsoft System Center Advisor SCOM Assessment Assessment regel uitvoeren* in de **regels** deelvenster.
2. Selecteer de gateway met de tekst in de zoekresultaten *Type: beheerserver*.
3. Met de rechtermuisknop op de regel en klik vervolgens op **overschrijft** > **voor een specifiek object van klasse: beheerserver**.
4.  Selecteer in de beschikbare lijst met beheerservers, de beheerserver waar de regel moet worden uitgevoerd.  Dit moet u eerder hebt geconfigureerd om te koppelen van het Run As-account met dezelfde beheerserver.
5.  Zorg ervoor dat u onderdrukkingwaarde wijzigen in **True** voor de **ingeschakeld** parameterwaarde.<br><br> ![parameter overschrijven](./media/log-analytics-scom-assessment/rule.png)

    Configureer de run frequentie met de volgende procedure terwijl u nog steeds in dit venster.

#### <a name="configure-the-run-frequency"></a>Configureer de frequentie uitvoeren

De beoordeling is standaard geconfigureerd om te worden uitgevoerd elke 10.080 minuten (of zeven dagen). U kunt de waarde moet minimaal de waarde van 1440 minuten (of één dag) overschrijven. De waarde geeft de afstand van de minimale tijd tussen opeenvolgende beoordeling wordt uitgevoerd. U kunt met het overschrijven van het interval, gebruik de volgende stappen.

1. In de **ontwerpen** werkruimte van de Operations Manager-console, zoekt u de regel *Microsoft System Center Advisor SCOM Assessment Assessment regel uitvoeren* in de **regels** sectie.
2. Selecteer de gateway met de tekst in de zoekresultaten *Type: beheerserver*.
3. Met de rechtermuisknop op de regel en klik vervolgens op **de regel onderdrukken** > **voor alle objecten van klasse: beheerserver**.
4. Wijzig de **Interval** parameterwaarde op de waarde van de gewenste interval. In het volgende voorbeeld wordt de waarde ingesteld op 1440 minuten (één dag).<br><br> ![de parameter interval](./media/log-analytics-scom-assessment/interval.png)<br>  

    Als de waarde is ingesteld en maximaal 1440 minuten, klikt u vervolgens de regel wordt uitgevoerd in een interval van één dag. In dit voorbeeld wordt de regel de intervalwaarde worden genegeerd en wordt uitgevoerd met een frequentie van één dag.


## <a name="understanding-how-recommendations-are-prioritized"></a>Begrijpen hoe de aanbevelingen zijn geplaatst

Elke aanbeveling krijgt een weging-waarde die het relatieve belang van de aanbeveling identificeert. Alleen de 10 meest belangrijke aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Hoe gewichten worden berekend

Wegingen zijn statistische waarden op basis van de drie belangrijkste factoren:

- De *kans* dat er een probleem vastgesteld problemen tot leidt. Er is een grotere kans gelijk aan een grotere totale score voor de aanbeveling.
- De *impact* van het probleem op uw organisatie als dit leidt een probleem tot. Er is een hogere impact gelijk aan een grotere totale score voor de aanbeveling.
- De *inspanning* vereist voor het implementeren van de aanbeveling. Er is een hogere inspanning gelijk aan een kleinere totale score voor de aanbeveling.

Waarden voor elke aanbeveling wordt uitgedrukt als percentage van de totale score beschikbaar voor elke focusgebied. Bijvoorbeeld, als een aanbeveling in het gebied van beschikbaarheid en zakelijke continuïteit focus heeft een score van 5%, verhoogd implementatie van deze aanbeveling uw algemene beschikbaarheid en zakelijke continuïteit score door 5%.

### <a name="focus-areas"></a>Focusgebieden

**Beschikbaarheid en zakelijke continuïteit** -focus ziet u hier aanbevelingen voor de beschikbaarheid van de service, de tolerantie van uw infrastructuur en de bescherming van zakelijke.

**Prestaties en schaalbaarheid** -focus ziet u hier aanbevelingen om u te helpen uw organisatie IT-infrastructuur toenemen, zorg ervoor dat uw IT-omgeving voldoet aan de huidige prestatievereisten en kunnen reageren op het wijzigen van de infrastructuur behoeften.

**Upgrade-, migratie- en implementatie** -focus ziet u hier aanbevelingen voor het upgraden, migreren en implementeren van SQL Server op uw bestaande infrastructuur.

**Bewerkingen en bewaking** -focus ziet u hier aanbevelingen om te helpen uw IT-bewerkingen te stroomlijnen, preventief onderhoud implementeren en prestaties.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u zijn gericht voor de beoordeling van 100% in elk focusgebied?

Dat hoeft niet. De aanbevelingen zijn gebaseerd op de kennis en ervaring opgedaan door Microsoft engineers in duizenden klant bezoeken. Echter geen twee server-infrastructuren zijn hetzelfde en specifieke aanbevelingen mogelijk meer of minder relevant zijn voor u. Enkele aanbevelingen voor beveiliging worden minder relevant als uw virtuele machines worden niet blootgesteld aan Internet. Enkele aanbevelingen beschikbaarheid mogelijk minder relevant zijn voor services die de ad-hoc gegevensverzameling met lage prioriteit en rapportage bieden. Problemen die belangrijk voor een goed ontwikkelde bedrijf zijn is mogelijk minder belangrijk voor een beginnend bedrijf bent. U kunt om te bepalen welke focusgebieden zijn uw prioriteiten en bekijk hoe uw scores gedurende een bepaalde periode wijzigen.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. Deze aanwijzingen gebruiken om te beoordelen of de aanbeveling te implementeren geschikt voor u, als basis van de aard van uw IT-services en de zakelijke behoeften van uw organisatie is.

## <a name="use-health-check-focus-area-recommendations"></a>Gebruik health focus gebied aanbevelingen controleren

Voordat u een oplossing voor het controleren van health in Log Analytics gebruiken kunt, moet u de oplossing geïnstalleerd hebben. Voor meer informatie over het installeren van oplossingen, Zie [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). Nadat deze is geïnstalleerd, kunt u de samenvatting van aanbevelingen weergeven met behulp van de tegel System Center Operations Manager Health controleren op de pagina overzicht in de OMS-portal.

De beoordelingen samengevatte compatibiliteit voor uw infrastructuur en inzoomen in aanbevelingen weergeven.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een focusgebied bekijken en corrigerende actie ondernemen
1. Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
3. Selecteer een werkruimte in het deelvenster abonnementen van logboekanalyse en klik vervolgens op de **OMS-Portal** tegel.  
4. Op de **overzicht** pagina, klikt u op de **System Center Operations Manager Health controleren** tegel.
5. Op de **System Center Operations Manager Health controleren** pagina, Controleer de overzichtsgegevens in een van de focus gebied blades en klik op een om aanbevelingen voor het desbetreffende focusgebied weer te geven.
6. Op een van de focus gebiedspagina's, kunt u de prioriteit aanbevelingen voor uw omgeving weergeven. Klik op een aanbeveling onder **objecten van invloed op een** voor meer informatie over waarom de aanbeveling is gemaakt.<br><br> ![focusgebied](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-dashboard-02.png)<br>
7. U kunt ondernemen corrigerende maatregelen voorgesteld in **voorgestelde acties**. Wanneer het item is opgelost, record hoger beoordelingen die aanbevolen acties zijn uitgevoerd en de naleving-score wordt verhoogd. Gecorrigeerde items worden weergegeven als **doorgegeven objecten**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren

Als u hebt de aanbevelingen die u wilt negeren, kunt u een tekstbestand dat Log Analytics gebruikt om te voorkomen dat de aanbevelingen worden weergegeven in de resultaten van uw beoordeling.

[!INCLUDE[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>Voor het identificeren van de aanbevelingen die u wilt negeren
1. Klik in de Azure-portal op de pagina van de werkruimte Log Analytics voor de geselecteerde werkruimte op het **logboek zoeken** tegel.
2. Gebruik de volgende query uit om de lijst met aanbevelingen die zijn mislukt voor computers in uw omgeving.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Hier volgt een schermopname de logboek-zoekopdracht wordt weergegeven:<br><br> ![zoeken in logboeken](./media/log-analytics-scom-assessment/scom-log-search.png)<br>

3. Kies de aanbevelingen die u wilt negeren. Gebruik de waarden voor RecommendationId zult u in de volgende procedure.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Maken en gebruiken van een tekstbestand IgnoreRecommendations.txt

1. Maak een bestand met de naam IgnoreRecommendations.txt.
2. Plakken of typ elke RecommendationId voor elke aanbeveling dat u wilt dat Log Analytics negeren op een afzonderlijke regel en vervolgens opslaan en sluit het bestand.
3. Plaats het bestand in de volgende map op elke computer waarop u logboekanalyse voor het negeren van aanbevelingen.
4. Op de beheerserver van Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te controleren dat de aanbevelingen worden genegeerd

1. Na de volgende evaluatie wordt uitgevoerd, standaard elke zeven dagen geplande, wordt de opgegeven aanbevelingen zijn gemarkeerd genegeerd en wordt niet weergegeven in het statusdashboard van controle.
2. U kunt de volgende logboek zoekquery's gebruiken voor een lijst met alle aanbevelingen genegeerd.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Ignore" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Als u later besluit dat u wilt zien genegeerd aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of u kunt RecommendationIDs verwijderen uit deze.

## <a name="system-center-operations-manager-health-check-solution-faq"></a>System Center Operations Manager Health Check-oplossing Veelgestelde vragen

*Ik heb de oplossing Health Check toegevoegd aan mijn werkruimte voor logboekanalyse. Maar de aanbevelingen niet weergegeven. Waarom niet?* Na het toevoegen van de oplossing voor de volgende stappen weergave gebruiken de aanbevelingen in het dashboard logboekanalyse.  

- [De Run As-account instellen voor System Center Operations Manager Health controleren](#operations-manager-run-as-accounts-for-log-analytics)  
- [De System Center Operations Manager Health controleren regel configureren](#configure-the-health-check-rule)


*Is er een manier om te configureren hoe vaak de controle wordt uitgevoerd?* Ja. Zie [Configureer de run frequentie](#configure-the-run-frequency).

*Als een andere server wordt gedetecteerd nadat ik de evaluatie van System Center Operations Manager-oplossing hebt toegevoegd, wordt er gecontroleerd?* Ja, na de detectie wordt gecontroleerd daarna standaard elke zeven dagen.

*Wat is de naam van het proces dat het verzamelen van gegevens biedt?* AdvisorAssessment.exe

*Waar wordt het proces AdvisorAssessment.exe uitgevoerd?* AdvisorAssessment.exe wordt uitgevoerd onder het proces van de Health-service van de beheerserver waarop de regel selectievakje is ingeschakeld. Met dit proces, is detectie van uw hele omgeving via externe gegevensverzameling bereikt.

*Hoe lang duurt het voor het verzamelen van gegevens?* Verzamelen van gegevens op de server duurt ongeveer één uur. Kan het langer duren in omgevingen met veel Operations Manager-exemplaren of -databases.

*Wat gebeurt er als ik het interval van de evaluatie en maximaal 1440 minuten instellen?* De beoordeling is vooraf geconfigureerd voor uitvoering op maximaal één keer per dag. Als u de waarde voor interval voor een waarde minder dan 1440 minuten overschrijven, gebruikt de beoordeling 1440 minuten als de waarde voor interval.

*Hoe weet ik of er vereiste fouten zijn?* Als de statuscontrole is uitgevoerd en er geen resultaten, is het waarschijnlijk dat bepaalde vereisten voor de controle is mislukt. U kunt query's uitvoeren: `Operation Solution=SCOMAssessment` en `SCOMAssessmentRecommendation FocusArea=Prerequisites` in logboek zoekopdracht om te zien van de vereisten is mislukt.

*Er is een `Failed to connect to the SQL Instance (….).` bericht vereiste storingen in. Wat is het probleem?* AdvisorAssessment.exe, het proces dat gegevens verzamelt, wordt uitgevoerd onder het proces van de Health-service op de beheerserver. Als onderdeel van de statuscontrole, wordt het proces probeert te maken met de SQL-Server waarop de Operations Manager-database aanwezig is. Deze fout kan optreden als firewallregels de verbinding met de SQL Server-exemplaar blokkeert.

*Welk type gegevens worden verzameld?* De volgende typen gegevens worden verzameld: - gegevens van WMI - gegevens - gegevens van EventLog - Operations Manager registergegevens via Windows PowerShell, SQL-query's en bestanden verzamelen van informatie.

*Waarom heb ik een Run As-Account configureren?* Met Operations Manager, worden verschillende SQL-query's uitgevoerd. Voordat ze worden uitgevoerd, moet u een Run As-Account gebruiken met de vereiste machtigingen beschikt. Bovendien zijn lokale beheerdersreferenties vereist voor query uitvoeren op WMI.

*Waarom worden alleen de top 10 aanbevelingen weergegeven?* In plaats van geeft u een uitgebreide en overweldigend lijst van taken, is het raadzaam dat u zich richten op de prioriteit aanbevelingen eerst adressering. Nadat u deze oplossen, wordt extra aanbevelingen beschikbaar worden. Als u liever voor een gedetailleerd overzicht, kunt u alle aanbevelingen logboek zoekactie kunt weergeven.

*Is er een manier om een aanbeveling negeren?* Ja, Zie de [aanbevelingen negeren](#Ignore-recommendations).


## <a name="next-steps"></a>Volgende stappen

- [Zoeken in een logboek](log-analytics-log-searches.md) voor informatie over het analyseren van gedetailleerde gegevens voor System Center Operations Manager Health controleren en aanbevelingen.
