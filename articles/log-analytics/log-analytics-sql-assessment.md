---
title: Optimalisatie van uw SQL Server-omgeving met Azure Log Analytics | Microsoft Docs
description: U kunt met Azure Log Analytics, de oplossing statuscontrole van SQL gebruiken om het risico en de status van uw omgeving op een vast interval vast te stellen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 2dcbd0cab055e8a2b2128b1f83f5c080a0887b85
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122517"
---
# <a name="optimize-your-sql-environment-with-the-sql-server-health-check-solution-in-log-analytics"></a>Optimalisatie van uw SQL-omgeving met de oplossing statuscontrole van SQL Server in Log Analytics

![SQL-statuscontrole symbool](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

U kunt de oplossing statuscontrole van SQL gebruiken om het risico en de status van uw server-omgevingen op een vast interval vast te stellen. In dit artikel helpt u bij het installeren van de oplossing zodat u corrigerende maatregelen voor potentiële problemen kunt uitvoeren.

Deze oplossing biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor uw geïmplementeerde serverinfrastructuur. De aanbevelingen zijn onderverdeeld in zes gebieden waarmee u snel inzicht in het risico en neemt u corrigerende maatregelen.

De aanbevelingen zijn gebaseerd op de kennis en ervaring met Microsoft-engineers van duizenden klanten bezoeken. Elke aanbeveling bevat richtlijnen over waarom een probleem voor u mogelijk belangrijk en het implementeren van de voorgestelde wijzigingen.

U kunt gebieden die het meest relevant zijn voor uw organisatie en de voortgang naar het uitvoeren van een risico's gratis en gezonde omgeving bijhouden.

Nadat u de oplossing hebt toegevoegd en een beoordeling voltooid, samenvatting is informatie voor focusgebieden wordt weergegeven op de **SQL-statuscontrole** dashboard voor de infrastructuur in uw omgeving. De volgende secties wordt beschreven hoe u gebruik van de informatie op de **SQL-statuscontrole** dashboard, waar u kunt weergeven en vervolgens ondernemen aanbevolen acties voor uw SQL Server-infrastructuur.

![afbeelding van SQL-statuscontrole tegel](./media/log-analytics-sql-assessment/sql-healthcheck-summary-tile.png)

![afbeelding van SQL-statuscontrole-dashboard](./media/log-analytics-sql-assessment/sql-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Vereisten

* De oplossing statuscontrole van SQL vereist een ondersteunde versie van .NET Framework 4 zijn geïnstalleerd op elke computer die de Microsoft Monitoring Agent (MMA) geïnstalleerd is.  De MMA-agent wordt gebruikt door System Center 2016 - Operations Manager en Operations Manager 2012 R2 en de Log Analytics-service.  
* De oplossing biedt ondersteuning voor SQL Server-versie 2012, 2014 en 2016.
* Een Log Analytics-werkruimte om toe te voegen van de oplossing SQL-statuscontrole van de Azure marketplace in Azure portal.  Als u wilt installeren van de oplossing, moet u een beheerder of Inzender in het Azure-abonnement.

  > [!NOTE]
  > Nadat u de oplossing hebt toegevoegd, wordt het bestand AdvisorAssessment.exe toegevoegd aan servers met agents. Configuratiegegevens is gelezen en vervolgens verzonden naar de Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en de cloudservice registreert de gegevens.
  >
  >

Als u de statuscontrole voor uw SQL Server-servers, hiervoor een agent en de verbinding met Log Analytics met behulp van een van de volgende ondersteunde methodes:

1. Installeer de [Microsoft Monitoring Agent (MMA)](log-analytics-windows-agent.md) als de server niet door System Center 2016 - Operations Manager of Operations Manager 2012 R2 bewaakt wordt.
2. Als deze wordt bewaakt met System Center 2016 - Operations Manager of Operations Manager 2012 R2 en de beheergroep is niet geïntegreerd met de service Log Analytics, op de server een multihomed met Log Analytics voor het verzamelen van gegevens en door te sturen naar de service en nog steeds kan worden toegepast worden bewaakt door Operations Manager.  
3. Anders, als uw Operations Manager-beheergroep is geïntegreerd met de service, moet u de domeincontrollers voor het verzamelen van gegevens toevoegen door de service die de stappen onder [toevoegen door agents beheerde computers](log-analytics-om-agents.md#connecting-operations-manager-to-log-analytics) nadat u hebt ingeschakeld de oplossing in uw werkruimte.  

De agent op uw SQL-Server die rapporten in een Operations Manager-beheergroep, verzamelt gegevens, worden doorgestuurd naar de toegewezen beheerserver, en vervolgens rechtstreeks vanaf een beheerserver verzonden naar de Log Analytics-service.  De gegevens worden niet naar de Operations Manager-databases geschreven.  

Als de SQL-Server wordt bewaakt door Operations Manager, moet u een Operations Manager runas-account configureren. Zie [Operations Manager run as-accounts voor Log Analytics](#operations-manager-run-as-accounts-for-log-analytics) hieronder voor meer informatie.

## <a name="sql-health-check-data-collection-details"></a>Details van gegevens verzameling SQL-statuscontrole
SQL-statuscontrole verzamelt gegevens uit de volgende bronnen met behulp van de agent die u hebt ingeschakeld:

* Windows Management Instrumentation (WMI)
* Register
* Prestatiemeteritems
* Resultaten van SQL Server dynamische Beheerweergave weergeven

Gegevens worden verzameld op de SQL Server en doorgestuurd naar Log Analytics in de zeven dagen.

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Operations Manager run as-accounts voor Log Analytics
Log Analytics maakt gebruik van de Operations Manager-agent en beheer van de groep voor het verzamelen en verzenden van gegevens naar de service Log Analytics. Log Analytics gebouwd op de management packs voor workloads voor toevoegen waarde-services. Elke werkbelasting vereist workload-specifieke machtigingen voor het uitvoeren van management packs in een andere beveiligingscontext, zoals een domeingebruikersaccount. U moet referenties opgeven door het configureren van een Operations Manager runas-account.

Gebruik de volgende informatie om in te stellen van de Operations Manager runas-account voor SQL-Serverstatus controleren.

### <a name="set-the-run-as-account-for-sql-health-check"></a>Uitvoeren als-account voor SQL-statuscontrole instellen
 Als u al van het SQL Server managementpack gebruikmaakt, moet u de configuratie van dat uitvoeren als gebruiken.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>Het SQL Run As-account in de Operations-console configureren
> [!NOTE]
> Werkstromen in het managementpack wordt standaard uitgevoerd in de beveiligingscontext van het lokale systeemaccount gebruikt. Als u met behulp van de Microsoft Monitoring Agent rechtstreeks verbonden met de service in plaats van rechtstreeks rapportage aan een Operations Manager-beheergroep, slaat u de stappen 1-5 hieronder en voer de T-SQL- of PowerShell-voorbeeld, op te geven NT AUTHORITY\SYSTEM als de de naam van de gebruiker.
>
>

1. Open de Operations-console in Operations Manager, en klik vervolgens op **beheer**.
2. Onder **Run As-configuratie**, klikt u op **profielen**, en open **OMS SQL-evaluatie Run As-profiel**.
3. Op de **Run As-Accounts** pagina, klikt u op **toevoegen**.
4. Selecteer een Windows uitvoeren als-account met de referenties die nodig zijn voor SQL Server, of klik op **nieuw** een te maken.

   > [!NOTE]
   > Het Run As-accounttype moet Windows. Uitvoeren als-account moet ook deel uit van de lokale groep Administrators op alle Windows-Servers die als host fungeert voor SQL Server-exemplaren.
   >
   >
5. Klik op **Opslaan**.
6. Wijzig en voer de volgende T-SQL-voorbeeld op elke SQL Server-exemplaar voor het verlenen van minimale machtigingen die vereist zijn voor het uitvoeren als-Account om uit te voeren van de statuscontrole. U hoeft echter te doen als een uitvoeren als-Account al deel uit van de serverrol sysadmin op de SQL Server-exemplaren maakt.

```
    ---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```

#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Het configureren van het SQL Run As-account met behulp van Windows PowerShell
Open een PowerShell-venster en voer het volgende script uit nadat u deze hebt bijgewerkt met uw gegevens:

```
    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Inzicht krijgen in hoe aanbevelingen met prioriteit wordt toegepast
Elke aanbeveling krijgt een waarde valutaconversie in gevallen waarin het relatieve belang van de aanbeveling. Alleen de tien belangrijkste aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Hoe het gewicht worden berekend
Wegingen zijn samengevoegde waarden op basis van drie belangrijke factoren:

* De *kans* dat een probleem dat is geïdentificeerd ervoor zorgen problemen dat. Een grotere kans is gelijk aan een grotere totale score voor de aanbeveling.
* De *impact* van het probleem op uw organisatie als dit leidt een probleem tot. Er is een hogere impact gelijk aan een grotere totale score voor de aanbeveling.
* De *inspanning* vereist de aanbeveling kunt implementeren. Er is een hogere inspanning gelijk aan een kleinere totale score voor de aanbeveling.

Het gewicht voor elke aanbeveling wordt uitgedrukt als een percentage van de totale score voor elke aandachtsgebied beschikbaar. Bijvoorbeeld, als u een aanbeveling in de beveiliging en naleving aandachtsgebied heeft een score van 5%, vergroten uitvoering van deze aanbeveling de algehele beveiliging en naleving score van 5%.

### <a name="focus-areas"></a>Focusgebieden
**Beveiliging en naleving** -deze aandachtsgebied bevat aanbevelingen voor potentiële beveiligingsrisico's opsporen en schendingen van bedrijfsbeleid en technische, juridische en wettelijke nalevingsvereisten.

**Beschikbaarheid en bedrijfscontinuïteit** -deze aandachtsgebied bevat aanbevelingen voor beschikbaarheid van de service, de tolerantie van uw infrastructuur en zakelijke beveiliging.

**Prestaties en schaalbaarheid** -deze aandachtsgebied bevat aanbevelingen voor uw organisatie het IT-infrastructuur groeien, zorg ervoor dat uw IT-omgeving voldoet aan de huidige prestatievereisten en kan reageren op veranderende infrastructuur behoeften.

**Upgrade, migratie en implementatie van** -deze aandachtsgebied bevat aanbevelingen voor het upgraden, migreren en implementeren van SQL Server op uw bestaande infrastructuur.

**Bewerkingen en bewaking** -deze aandachtsgebied bevat aanbevelingen voor het stroomlijnen van uw IT-activiteiten, preventief onderhoud implementeren en prestaties te maximaliseren.

**Wijzigings- en Configuratiebeheer** -deze aandachtsgebied bevat aanbevelingen om u te helpen beschermen van dagelijkse handelingen, ervoor te zorgen dat wijzigingen geen negatieve invloed hebben op uw infrastructuur wijzigen besturingselement procedures, vast te stellen en te volgen en controleren systeemconfiguraties.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u erop gericht om 100% te beoordelen in elke aandachtsgebied?
Dat hoeft niet. De aanbevelingen zijn gebaseerd op de kennis en ervaringen die zijn verkregen dankzij het Microsoft-technici via duizenden klanten bezoeken. Echter geen twee server-infrastructuur zijn hetzelfde, en specifieke aanbevelingen mogelijk meer of minder relevant voor u. Bijvoorbeeld, enkele aanbevelingen voor beveiliging mogelijk minder relevant zijn als uw virtuele machines worden niet blootgesteld aan Internet. Aanbevelingen voor sommige beschikbaarheid mogelijk minder relevant zijn voor services met lage prioriteit ad-hoc gegevens verzamelen en rapporteren. Problemen die tot een volwassen bedrijf van belang zijn mogelijk minder belangrijk voor een lid is. Kunt u om te bepalen welke focusgebieden zijn uw prioriteiten en kijken hoe uw scores na verloop van tijd veranderen.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. U moet deze handleiding gebruiken om te beoordelen of het implementeren van de aanbeveling geschikt voor u is, gezien de aard van uw IT-services en de zakelijke eisen van uw organisatie.

## <a name="use-health-check-focus-area-recommendations"></a>Gebruik Health Check focus gebied aanbevelingen
Voordat u een oplossing voor evaluatie in Log Analytics gebruiken kunt, moet u de oplossing die is geïnstalleerd hebben.  Nadat deze is geïnstalleerd, kunt u het overzicht van de aanbevelingen weergeven met behulp van de SQL-statuscontrole tegel op de pagina van de oplossing in Azure portal.

De beoordelingen samengevatte naleving voor uw infrastructuur en aanbevelingen voor inzoomen in weergeven.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een aandachtsgebied weergeven en neemt u corrigerende maatregelen
1. Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
3. In het deelvenster voor abonnementen van Log Analytics een werkruimte selecteren en klik vervolgens op de **overzicht** tegel.  
4. Op de **overzicht** pagina, klikt u op de **SQL-statuscontrole** tegel.
5. Op de **statuscontrole** pagina, Controleer de samenvattingsinformatie in een van de focus gebied blades en klik vervolgens op een om aanbevelingen voor die aandachtsgebied weer te geven.
6. Op een van de focus gebiedspagina's, kunt u de aanbevelingen met prioriteit gemaakt voor uw omgeving bekijken. Klik op een aanbeveling onder **betrokken objecten** voor meer informatie over waarom de aanbeveling is gedaan.<br><br> ![afbeelding van aanbevelingen voor de SQL-statuscontrole](./media/log-analytics-sql-assessment/sql-healthcheck-dashboard-02.png)<br>
7. U kunt ondernemen corrigerende maatregelen voorgesteld in **voorgestelde acties**. Wanneer het item is opgelost, record hoger beoordelingen die aanbevolen acties zijn uitgevoerd en worden uw nalevingsscore verhoogd. Gecorrigeerde items worden weergegeven als **doorgegeven objecten**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren
Als u aanbevelingen die u wilt negeren hebt, kunt u een tekstbestand dat door Log Analytics wordt gebruikt om te voorkomen dat de aanbevelingen wordt weergegeven in de resultaten van de evaluatie maken.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Voor het identificeren van de aanbevelingen die u worden genegeerd
1. In de Azure-portal op de pagina van de Log Analytics-werkruimte voor de geselecteerde werkruimte, klikt u op de **zoeken in logboeken** tegel.
2. Gebruik de volgende query uit om de lijst met aanbevelingen die zijn mislukt voor computers in uw omgeving.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe met Log Analytics-querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query's gewijzigd in het volgende.
    >
    > `SQLAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Hier volgt een schermopname van de zoeken in Logboeken-query:<br><br> ![mislukte aanbevelingen](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)<br>

3. Kies de aanbevelingen die u wilt negeren. U gebruikt de waarden voor RecommendationId in de volgende procedure.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Het maken en gebruiken van een tekstbestand IgnoreRecommendations.txt
1. Maak een bestand met de naam IgnoreRecommendations.txt.
2. Plak of typ elke RecommendationId voor elke aanbeveling dat u wilt dat in Log Analytics op een afzonderlijke regel negeren en vervolgens opslaan en sluiten van het bestand.
3. Plaats het bestand in de volgende map op elke computer waarop u Log Analytics voor het negeren van aanbevelingen.
   * Op computers met de Microsoft Monitoring Agent (verbonden rechtstreeks of via de Operations Manager) - *SystemDrive*: \Program Files\Microsoft Monitoring Agent\Agent
   * Op de beheerserver van Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server
   * Op de beheerserver van Operations Manager 2016 - *SystemDrive*: \Program Files\Microsoft System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te controleren of dat er aanbevelingen worden genegeerd
1. Na de volgende evaluatie wordt uitgevoerd, wordt standaard elke zeven dagen geplande, wordt de opgegeven aanbevelingen zijn gemarkeerd als genegeerd en wordt niet weergegeven op het dashboard van de evaluatie.
2. U kunt de volgende zoeken in Logboeken-query's gebruiken om de genegeerde aanbevelingen weer te geven.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe met Log Analytics-querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query's gewijzigd in het volgende.
    >
    > `SQLAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Als u later besluit dat u wilt zien genegeerde aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of kunt u RecommendationIDs verwijderen uit deze.

## <a name="sql-health-check-solution-faq"></a>SQL-statuscontrole oplossing Veelgestelde vragen
*Hoe vaak wordt een controle van gatewayservicestatus uitgevoerd?*

* De controle is elke zeven dagen uitgevoerd.

*Is er een manier om te configureren hoe vaak de controle wordt uitgevoerd?*

* Momenteel niet.

*Als een andere server wordt gedetecteerd nadat ik de oplossing SQL-statuscontrole hebt toegevoegd, wordt deze gecontroleerd?*

* Ja, zodra deze gedetecteerd wordt deze gecontroleerd van vervolgens op elke zeven dagen.

*Als een server uit bedrijf genomen is, wanneer wordt deze verwijderd uit de statuscontrole?*

* Als een server komt niet met het verzenden van gegevens voor drie weken, wordt deze verwijderd.

*Wat is de naam van het proces dat het verzamelen van gegevens wordt?*

* AdvisorAssessment.exe

*Hoe lang duurt het voordat gegevens worden verzameld?*

* Het verzamelen van de werkelijke gegevens op de server duurt ongeveer 1 uur. Het kan langer duren op servers die een groot aantal databases of SQL-exemplaren.

*Welk type gegevens worden verzameld?*

* De volgende typen gegevens worden verzameld:
  * WMI
  * Register
  * Prestatiemeteritems
  * SQL dynamische beheerweergave (DMV).

*Is er een manier om te configureren wanneer gegevens worden verzameld?*

* Momenteel niet.

*Waarom heb ik het configureren van uitvoeren als-Account?*

* Voor SQL Server, worden een klein aantal SQL-query's uitgevoerd. Zodat ze om uit te voeren, moet een Run As-Account met machtigingen voor SQL VIEW SERVER STATE worden gebruikt.  Bovendien zijn om een query uitvoeren op WMI, lokale beheerdersreferenties zijn vereist.

*Waarom worden alleen de top 10 aanbevelingen weergegeven?*

* In plaats van zodat u enorme uitputtende lijst van taken, is het raadzaam dat u zich richten op de aanbevelingen met prioriteit eerst adressering. Nadat u deze kunt oplossen, worden extra aanbevelingen beschikbaar. Als u liever de gedetailleerde lijst, kunt u alle aanbevelingen met behulp van de Log Analytics zoeken in Logboeken kunt weergeven.

*Is er een manier om een aanbeveling negeren?*

* Ja, Zie [aanbevelingen negeren](#ignore-recommendations) hierboven.

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in logboeken](log-analytics-log-searches.md) voor meer informatie over het analyseren van gedetailleerde gegevens van de SQL-statuscontrole en aanbevelingen.
