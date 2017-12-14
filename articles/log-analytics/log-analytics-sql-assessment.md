---
title: Optimalisatie van uw SQL Server-omgeving met Azure Log Analytics | Microsoft Docs
description: Met Azure Log Analytics kunt u de SQL-Serverstatus controleren oplossing voor het evalueren van de risico's en de status van uw omgevingen op een vast interval.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte;banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 04a5959d69cd42e77317161d743be7d778e3186d
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="optimize-your-sql-environment-with-the-sql-server-health-check-solution-in-log-analytics"></a>Optimalisatie van uw SQL-omgeving met de oplossing voor SQL Server-Serverstatus controleren in Log Analytics

![Symbool SQL-Serverstatus controleren](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

De SQL-Serverstatus controleren oplossing kunt u de risico's en de status van uw server-omgevingen te beoordelen op een vast interval. Dit artikel helpt u bij het installeren van de oplossing zodat u corrigerende maatregelen op mogelijke problemen kunt uitvoeren.

Deze oplossing biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor uw geïmplementeerde serverinfrastructuur. De aanbevelingen worden onderverdeeld in zes focusgebieden waarmee u snel het risico begrijpt en herstelacties uitvoeren.

De aanbevelingen zijn gebaseerd op de kennis en ervaring door Microsoft engineers van duizenden klant bezoeken. Elke aanbeveling bevat richtlijnen over waarom er een probleem mogelijk interessant voor u en het implementeren van de voorgestelde wijzigingen.

U kunt focusgebieden die het meest belangrijk zijn voor uw organisatie en het bijhouden van uw progressie ten opzichte van een omgeving met risico gratis en goed wordt uitgevoerd.

Nadat u de oplossing hebt toegevoegd en een beoordeling voltooid, samenvattende is informatie voor focusgebieden wordt weergegeven op de **SQL-Serverstatus controleren** dashboard voor de infrastructuur in uw omgeving. De volgende secties wordt beschreven hoe u de informatie te gebruiken op de **SQL-Serverstatus controleren** dashboard, waar u kunt weergeven en vervolgens te aanbevolen acties voor de infrastructuur van SQL Server.

![afbeelding van de tegel controle van de SQL-serverstatus](./media/log-analytics-sql-assessment/sql-healthcheck-summary-tile.png)

![afbeelding van dashboard SQL-Serverstatus controleren](./media/log-analytics-sql-assessment/sql-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Vereisten

* De SQL-Serverstatus controleren oplossing vereist een ondersteunde versie van .NET Framework 4 is geïnstalleerd op elke computer die de Microsoft Monitoring Agent (MMA) geïnstalleerd is.  De agent MMA wordt gebruikt door System Center 2016 - Operations Manager en Operations Manager 2012 R2 en de Log Analytics-service.  
* De oplossing biedt ondersteuning voor SQL Server versie 2012, 2014 en 2016.
* Een werkruimte voor logboekanalyse toevoegen van de oplossing SQL-Serverstatus controleren vanuit Azure marketplace in de Azure portal.  Om de oplossing hebt geïnstalleerd, moet u een beheerder of bijdrager in de Azure-abonnement. 

  > [!NOTE]
  > Nadat u de oplossing hebt toegevoegd, wordt het bestand AdvisorAssessment.exe toegevoegd aan servers met agents. Configuratiegegevens is gelezen en vervolgens doorgestuurd naar het Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en registreert de gegevens van de cloudservice.
  >
  >

Als u wilt de statuscontrole op basis van uw servers met SQL Server uitvoeren, vereisen ze een agent en de connectiviteit met logboekanalyse met een van de volgende ondersteunde methodes:

1. Installeer de [Microsoft Monitoring Agent (MMA)](log-analytics-windows-agent.md) als de server niet door System Center 2016 - Operations Manager of Operations Manager 2012 R2 bewaakt wordt.
2. Als deze wordt bewaakt met System Center 2016 - Operations Manager of Operations Manager 2012 R2 en de beheergroep is niet geïntegreerd met de Log Analytics-service, kan de server worden multihomed met logboekanalyse voor het verzamelen van gegevens en door te sturen naar de service en nog steeds bewaakt door Operations Manager.  
3. Anders, als uw Operations Manager-beheergroep is geïntegreerd met de service, moet u de domeincontrollers voor het verzamelen van gegevens toevoegen door de service die de stappen onder [toevoegen door agents beheerde computers](log-analytics-om-agents.md#connecting-operations-manager-to-oms) nadat u hebt ingeschakeld de oplossing in uw werkruimte.  

De agent op de SQL Server welke rapporten aan een beheergroep van Operations Manager verzamelt gegevens, worden doorgestuurd naar de toegewezen beheerserver en vervolgens rechtstreeks vanaf een beheerserver verzonden naar de Log Analytics-service.  De gegevens niet naar de Operations Manager-database geschreven.  

Als de SQL Server wordt bewaakt door Operations Manager, moet u een Operations Manager Run As-account voor configureren. Zie [Operations Manager run as-accounts voor logboekanalyse](#operations-manager-run-as-accounts-for-log-analytics) hieronder voor meer informatie.

## <a name="sql-health-check-data-collection-details"></a>Gegevensdetails verzameling SQL-Serverstatus controleren
SQL-Serverstatus controleren verzamelt gegevens uit de volgende bronnen met behulp van de agent die u hebt ingeschakeld: 

* Windows Management Instrumentation (WMI) 
* Register 
* Prestatiemeteritems
* Resultaten van SQL Server dynamische Beheerweergave weergeven 

Gegevens worden verzameld op de SQL Server en doorgestuurd naar Log Analytics elke zeven dagen.

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Operations Manager run as-accounts voor logboekanalyse
Log Analytics gebruikt de Operations Manager-agent en beheer beheergroep te verzamelen en verzenden van gegevens naar de Log Analytics-service. Log Analytics builds van management packs voor werkbelastingen voor toevoegen waarde-services. Elke werkbelasting vereist werklastspecifiek bevoegdheden voor het uitvoeren van management packs in een andere beveiligingscontext, zoals een domeingebruikersaccount. U moet referentie-informatie opgeven door een Operations Manager Run As-account voor configureren.

Gebruik de volgende informatie in te stellen van de Manager Operations die Run As-account voor SQL-Serverstatus controleren.

### <a name="set-the-run-as-account-for-sql-health-check"></a>Instellen van het Run As-account voor SQL-Serverstatus controleren
 Als u al van het SQL Server management pack gebruikmaakt, moet u die Run As-configuratie gebruiken.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>Het SQL Run As-account configureren in de Operations-console
> [!NOTE]
> Werkstromen in het management pack wordt standaard uitgevoerd in de beveiligingscontext van het lokale systeemaccount. Als u Microsoft Monitoring Agent rechtstreeks verbonden met de service gebruikt in plaats van rechtstreeks rapportage aan een beheergroep van Operations Manager stappen 1-5 hieronder overslaan en de T-SQL- of PowerShell-voorbeeld uitvoert, geven NT AUTHORITY\SYSTEM als de de gebruikersnaam.
>
>

1. Open de Operations-console in Operations Manager en klik vervolgens op **beheer**.
2. Onder **Run As-configuratie**, klikt u op **profielen**, en open **OMS SQL Assessment Run As-profiel**.
3. Op de **Run As-Accounts** pagina, klikt u op **toevoegen**.
4. Selecteer een Windows Run As-account met de referenties die nodig zijn voor SQL Server, of klik op **nieuw** een maken.

   > [!NOTE]
   > Het type Run As-account moet Windows. De Run As-account moet ook deel uitmaken van de lokale groep Administrators op alle Windows-Servers die als host fungeert voor SQL Server-exemplaren.
   >
   >
5. Klik op **Opslaan**.
6. Wijzigen en vervolgens de volgende T-SQL-voorbeeld wordt uitgevoerd op elke SQL Server-exemplaar minimale machtigingen vereist voor het Run As-Account om uit te voeren van de statuscontrole. U hoeft echter niet te doen als een Run As-Account al deel uit van de serverrol sysadmin op de SQL Server-exemplaren maakt.

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

#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Voor het configureren van het SQL Run As-account met behulp van Windows PowerShell
Open een PowerShell-venster en voer het volgende script nadat u deze met uw gegevens hebt bijgewerkt:

```
    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Begrijpen hoe de aanbevelingen zijn geplaatst
Elke aanbeveling krijgt een weging-waarde die het relatieve belang van de aanbeveling identificeert. Alleen de tien belangrijkste aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Hoe gewichten worden berekend
Wegingen zijn statistische waarden op basis van de drie belangrijkste factoren:

* De *kans* dat er een probleem vastgesteld problemen tot leidt. Er is een grotere kans gelijk aan een grotere totale score voor de aanbeveling.
* De *impact* van het probleem op uw organisatie als dit leidt een probleem tot. Er is een hogere impact gelijk aan een grotere totale score voor de aanbeveling.
* De *inspanning* vereist voor het implementeren van de aanbeveling. Er is een hogere inspanning gelijk aan een kleinere totale score voor de aanbeveling.

Waarden voor elke aanbeveling wordt uitgedrukt als percentage van de totale score beschikbaar voor elke focusgebied. Bijvoorbeeld, als een aanbeveling in het gebied van beveiliging en naleving focus heeft een score van 5%, wordt implementatie van deze aanbeveling verhoogd, uw algemene beveiliging en naleving score door 5%.

### <a name="focus-areas"></a>Focusgebieden
**Beveiliging en naleving** -focus ziet u hier aanbevelingen voor potentiële beveiligingsrisico's en schendingen, bedrijfsbeleid en aan technische, juridische en wettelijke vereisten.

**Beschikbaarheid en zakelijke continuïteit** -focus ziet u hier aanbevelingen voor de beschikbaarheid van de service, de tolerantie van uw infrastructuur en de bescherming van zakelijke.

**Prestaties en schaalbaarheid** -focus ziet u hier aanbevelingen om u te helpen uw organisatie IT-infrastructuur toenemen, zorg ervoor dat uw IT-omgeving voldoet aan de huidige prestatievereisten en kunnen reageren op veranderende behoeften van de infrastructuur.

**Een upgrade uitvoert, migratie en implementatie van** -focus ziet u hier aanbevelingen voor het upgraden, migreren en implementeren van SQL Server op uw bestaande infrastructuur.

**Bewerkingen en bewaking** -focus ziet u hier aanbevelingen om te helpen uw IT-bewerkingen te stroomlijnen, preventief onderhoud implementeren en prestaties.

**Wijzigings- en Configuratiebeheer** -focus ziet u hier aanbevelingen om u te helpen beveiligen dagelijkse taken, zorg ervoor dat wijzigingen geen negatieve invloed zijn op uw infrastructuur, voor het vaststellen controleprocedures wijzigen, en voor het volgen en controleren basissysteemconfiguraties.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u zijn gericht voor de beoordeling van 100% in elk focusgebied?
Dat hoeft niet. De aanbevelingen zijn gebaseerd op de kennis en ervaring opgedaan door Microsoft engineers in duizenden klant bezoeken. Echter geen twee server-infrastructuren zijn hetzelfde en specifieke aanbevelingen mogelijk meer of minder relevant zijn voor u. Enkele aanbevelingen voor beveiliging worden minder relevant als uw virtuele machines worden niet blootgesteld aan Internet. Enkele aanbevelingen beschikbaarheid mogelijk minder relevant zijn voor services die de ad-hoc gegevensverzameling met lage prioriteit en rapportage bieden. Problemen die belangrijk voor een goed ontwikkelde bedrijf zijn is mogelijk minder belangrijk voor een beginnend bedrijf bent. U kunt om te bepalen welke focusgebieden zijn uw prioriteiten en bekijk hoe uw scores gedurende een bepaalde periode wijzigen.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. U moet deze aanwijzingen gebruiken om te beoordelen of de aanbeveling te implementeren geschikt voor u, als basis van de aard van uw IT-services en de zakelijke behoeften van uw organisatie is.

## <a name="use-health-check-focus-area-recommendations"></a>Gebruik Health Check focus gebied aanbevelingen
Voordat u een oplossing voor evaluatie in Log Analytics gebruiken kunt, moet u de oplossing geïnstalleerd hebben.  Nadat deze is geïnstalleerd, kunt u de samenvatting van aanbevelingen weergeven met behulp van de tegel SQL-Serverstatus controleren op de oplossingenpagina in de Azure-portal.

De beoordelingen samengevatte compatibiliteit voor uw infrastructuur en inzoomen in aanbevelingen weergeven.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een focusgebied bekijken en corrigerende actie ondernemen
1. Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com). 
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
3. Selecteer een werkruimte in het deelvenster abonnementen van logboekanalyse en klik vervolgens op de **OMS-Portal** tegel.  
4. Op de **overzicht** pagina, klikt u op de **SQL-Serverstatus controleren** tegel. 
5. Op de **Health Check** pagina, Controleer de overzichtsgegevens in een van de focus gebied blades en klik op een om aanbevelingen voor het desbetreffende focusgebied weer te geven.
6. Op een van de focus gebiedspagina's, kunt u de prioriteit aanbevelingen voor uw omgeving weergeven. Klik op een aanbeveling onder **objecten van invloed op een** voor meer informatie over waarom de aanbeveling is gemaakt.<br><br> ![afbeelding van aanbevelingen voor SQL-Serverstatus controleren](./media/log-analytics-sql-assessment/sql-healthcheck-dashboard-02.png)<br>
7. U kunt ondernemen corrigerende maatregelen voorgesteld in **voorgestelde acties**. Wanneer het item is opgelost, record hoger beoordelingen die aanbevolen acties zijn uitgevoerd en de naleving-score wordt verhoogd. Gecorrigeerde items worden weergegeven als **doorgegeven objecten**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren
Als u hebt de aanbevelingen die u wilt negeren, kunt u een tekstbestand dat OMS wordt gebruikt om te voorkomen dat de aanbevelingen worden weergegeven in de resultaten van uw beoordeling.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Voor het identificeren van de aanbevelingen die u worden genegeerd
1. Klik in de Azure-portal op de pagina van de werkruimte Log Analytics voor de geselecteerde werkruimte op het **logboek zoeken** tegel.
2. Gebruik de volgende query uit om de lijst met aanbevelingen die zijn mislukt voor computers in uw omgeving.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
    >
    > `SQLAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Hier volgt een schermopname de logboek-zoekopdracht wordt weergegeven:<br><br> ![mislukte aanbevelingen](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)<br>

3. Kies de aanbevelingen die u wilt negeren. Gebruik de waarden voor RecommendationId zult u in de volgende procedure.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Maken en gebruiken van een tekstbestand IgnoreRecommendations.txt
1. Maak een bestand met de naam IgnoreRecommendations.txt.
2. Plakken of typ elke RecommendationId voor elke aanbeveling dat u wilt dat Log Analytics negeren op een afzonderlijke regel en vervolgens opslaan en sluit het bestand.
3. Plaats het bestand in de volgende map op elke computer waarop u logboekanalyse voor het negeren van aanbevelingen.
   * Op computers met Microsoft Monitoring Agent (rechtstreeks of via de Operations Manager verbonden) - *SystemDrive*: \Program Files\Microsoft Agent\Agent bewaking
   * Op de beheerserver van Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server
   * Op de beheerserver van Operations Manager 2016 - *SystemDrive*: \Program Files\Microsoft System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te controleren dat de aanbevelingen worden genegeerd
1. Na de volgende evaluatie wordt uitgevoerd, standaard elke 7 dagen geplande, wordt de opgegeven aanbevelingen zijn gemarkeerd genegeerd en worden niet weergegeven op het dashboard assessment.
2. U kunt de volgende logboek zoekquery's gebruiken voor een lijst met alle aanbevelingen genegeerd.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
    >
    > `SQLAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Als u later besluit dat u wilt zien genegeerd aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of u kunt RecommendationIDs verwijderen uit deze.

## <a name="sql-health-check-solution-faq"></a>SQL-Serverstatus controleren oplossing Veelgestelde vragen
*Hoe vaak wordt een controle uitgevoerd?*

* De controle elke zeven dagen uitgevoerd.

*Is er een manier om te configureren hoe vaak de controle wordt uitgevoerd?*

* Momenteel niet.

*Als een andere server wordt gedetecteerd nadat ik de SQL-Serverstatus controleren oplossing hebt toegevoegd, wordt er gecontroleerd?*

* Ja, zodra deze is gedetecteerd dat wordt gecontroleerd van vervolgens, elke zeven dagen.

*Als een server buiten werking wordt gesteld, wanneer deze verwijderd uit de statuscontrole?*

* Als een server komt niet met het verzenden van gegevens voor drie weken, wordt deze verwijderd.

*Wat is de naam van het proces dat het verzamelen van gegevens biedt?*

* AdvisorAssessment.exe

*Hoe lang duurt het voor gegevens moeten worden verzameld?*

* De verzameling actuele gegevens op de server duurt ongeveer 1 uur. Kan het langer duren op servers met een groot aantal SQL-exemplaren of -databases.

*Welk type gegevens worden verzameld?*

* De volgende typen gegevens zijn verzameld:
  * WMI
  * Register
  * Prestatiemeteritems
  * SQL dynamische beheerweergaven (DMV).

*Is er een manier om te configureren wanneer gegevens worden verzameld?*

* Momenteel niet.

*Waarom heb ik een Run As-Account configureren?*

* Voor SQL Server, worden een klein aantal SQL-query's uitgevoerd. Voordat ze worden uitgevoerd, moet een Run As-Account met machtigingen VIEW SERVER STATE naar SQL worden gebruikt.  Bovendien zijn om WMI een query uitvoert, lokale beheerdersreferenties zijn vereist.

*Waarom worden alleen de top 10 aanbevelingen weergegeven?*

* In plaats van zodat u overweldigend uitputtende lijst met taken, is het raadzaam dat u zich richten op de prioriteit aanbevelingen eerst adressering. Nadat u deze oplossen, wordt extra aanbevelingen beschikbaar worden. Als u liever voor een gedetailleerd overzicht, kunt u alle aanbevelingen voor het logboek logboekanalyse zoeken kunt weergeven.

*Is er een manier om een aanbeveling negeren?*

* Ja, Zie [aanbevelingen negeren](#ignore-recommendations) sectie hierboven.

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in een logboek](log-analytics-log-searches.md) voor informatie over het analyseren van gedetailleerde gegevens van de SQL-Serverstatus controleren en aanbevelingen.
