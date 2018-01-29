---
title: Optimalisatie van uw Active Directory-omgeving met Azure Log Analytics | Microsoft Docs
description: U kunt de oplossing voor Active Directory-Serverstatus controleren gebruiken voor het evalueren van de risico's en de status van uw omgevingen op een vast interval.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte;banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a5e803cadfd08c42e12e6e34feee1c2d0d091d70
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-health-check-solution-in-log-analytics"></a>Optimalisatie van uw Active Directory-omgeving met de oplossing voor Active Directory-Serverstatus controleren in Log Analytics

![AD Health Check symbool](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

U kunt de oplossing voor Active Directory-Serverstatus controleren gebruiken voor het evalueren van de risico's en de status van uw server-omgevingen met regelmatige tussenpozen. In dit artikel helpt u bij het installeren en gebruiken van de oplossing zodat u corrigerende maatregelen op mogelijke problemen kunt uitvoeren.

Deze oplossing biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor uw geïmplementeerde serverinfrastructuur. De aanbevelingen worden onderverdeeld in vier focusgebieden, waarmee u snel het risico begrijpt en actie ondernemen.

De aanbevelingen zijn gebaseerd op de kennis en ervaring door Microsoft engineers van duizenden klant bezoeken. Elke aanbeveling bevat richtlijnen over waarom er een probleem mogelijk interessant voor u en het implementeren van de voorgestelde wijzigingen.

U kunt focusgebieden die het meest belangrijk zijn voor uw organisatie en het bijhouden van uw progressie ten opzichte van een omgeving met risico gratis en goed wordt uitgevoerd.

Nadat u de oplossing hebt toegevoegd en een controle voltooid, samenvatting is-informatie voor focusgebieden wordt weergegeven op de **AD Health Check** dashboard voor de infrastructuur in uw omgeving. De volgende secties wordt beschreven hoe u de informatie te gebruiken op de **AD Health Check** dashboard, waar u kunt weergeven en vervolgens te aanbevolen acties voor de infrastructuur van de Active Directory-server.  

![afbeelding van de tegel controle van de AD-serverstatus](./media/log-analytics-ad-assessment/ad-healthcheck-summary-tile.png)

![afbeelding van dashboard AD-Serverstatus controleren](./media/log-analytics-ad-assessment/ad-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Vereisten

* De oplossing voor Active Directory-Serverstatus controleren vereist een ondersteunde versie van .NET Framework 4.5.2 of hoger geïnstalleerd op elke computer die de Microsoft Monitoring Agent (MMA) geïnstalleerd is.  De agent MMA wordt gebruikt door System Center 2016 - Operations Manager en Operations Manager 2012 R2 en de Log Analytics-service. 
* De oplossing biedt ondersteuning voor domeincontrollers met Windows Server 2008 en 2008 R2, Windows Server 2012 en 2012 R2 en Windows Server 2016.
* Een werkruimte voor logboekanalyse toevoegen van de oplossing voor Active Directory-Serverstatus controleren vanuit Azure marketplace in de Azure portal.  Er is geen verdere configuratie nodig.

  > [!NOTE]
  > Nadat u de oplossing hebt toegevoegd, wordt het bestand AdvisorAssessment.exe toegevoegd aan servers met agents. Configuratiegegevens is gelezen en vervolgens doorgestuurd naar het Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en registreert de gegevens van de cloudservice.
  >
  >

Als u wilt uitvoeren van de statuscontrole op basis van uw domeincontrollers die lid van het domein zijn moet worden geëvalueerd, vereisen ze een agent en de connectiviteit met logboekanalyse met een van de volgende ondersteunde methodes:

1. Installeer de [Microsoft Monitoring Agent (MMA)](log-analytics-windows-agent.md) als de domeincontroller niet door System Center 2016 - Operations Manager of Operations Manager 2012 R2 bewaakt wordt.
2. Als deze wordt bewaakt met System Center 2016 - Operations Manager of Operations Manager 2012 R2 en de beheergroep is niet geïntegreerd met de Log Analytics-service, de domeincontroller kan worden multihomed met logboekanalyse voor het verzamelen van gegevens en door te sturen naar de service- en nog steeds worden bewaakt door Operations Manager.  
3. Anders, als uw Operations Manager-beheergroep is geïntegreerd met de service, moet u de domeincontrollers voor het verzamelen van gegevens toevoegen door de service die de stappen onder [toevoegen door agents beheerde computers](log-analytics-om-agents.md#connecting-operations-manager-to-oms) nadat u hebt ingeschakeld de oplossing in uw werkruimte.  

De agent op uw domeincontroller welke rapporten aan een beheergroep van Operations Manager verzamelt gegevens, worden doorgestuurd naar de toegewezen beheerserver en vervolgens rechtstreeks vanaf een beheerserver verzonden naar de Log Analytics-service.  De gegevens niet naar de Operations Manager-database geschreven.  

## <a name="active-directory-health-check-data-collection-details"></a>Active Directory-Serverstatus controleren verzameling Gegevensdetails

Active Directory-Serverstatus controleren verzamelt gegevens uit de volgende bronnen met behulp van de agent die u hebt ingeschakeld:

- Register 
- LDAP 
- .NET Framework
- Gebeurtenislogboek 
- Active Directory Service interfaces (ADSI)
- Windows PowerShell
- Bestandsgegevens 
- Windows Management Instrumentation (WMI)
- DCDIAG hulpprogramma API
- API voor File Replication Service (NTFRS)
- Aangepaste C#-code

Gegevens worden verzameld op de domeincontroller en doorgestuurd naar Log Analytics elke zeven dagen.  

## <a name="understanding-how-recommendations-are-prioritized"></a>Begrijpen hoe de aanbevelingen zijn geplaatst
Elke aanbeveling krijgt een weging-waarde die het relatieve belang van de aanbeveling identificeert. Alleen de 10 meest belangrijke aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Hoe gewichten worden berekend
Wegingen zijn statistische waarden op basis van de drie belangrijkste factoren:

* De *kans* problemen zorgt ervoor dat een probleem dat is aangetroffen. Er is een grotere kans gelijk aan een grotere totale score voor de aanbeveling.
* De *impact* van het probleem op uw organisatie als dit leidt een probleem tot. Er is een hogere impact gelijk aan een grotere totale score voor de aanbeveling.
* De *inspanning* vereist voor het implementeren van de aanbeveling. Er is een hogere inspanning gelijk aan een kleinere totale score voor de aanbeveling.

Waarden voor elke aanbeveling wordt uitgedrukt als percentage van de totale score beschikbaar voor elke focusgebied. Bijvoorbeeld, als een aanbeveling in het gebied van beveiliging en naleving focus heeft een score van 5%, verhoogd implementatie van deze aanbeveling uw algemene beveiliging en naleving score door 5%.

### <a name="focus-areas"></a>Focusgebieden
**Beveiliging en naleving** -focus ziet u hier aanbevelingen voor potentiële beveiligingsrisico's en schendingen, bedrijfsbeleid en aan technische, juridische en wettelijke vereisten.

**Beschikbaarheid en zakelijke continuïteit** -focus ziet u hier aanbevelingen voor de beschikbaarheid van de service, de tolerantie van uw infrastructuur en de bescherming van zakelijke.

**Prestaties en schaalbaarheid** -focus ziet u hier aanbevelingen om u te helpen uw organisatie IT-infrastructuur toenemen, zorg ervoor dat uw IT-omgeving voldoet aan de huidige prestatievereisten en kunnen reageren op veranderende behoeften van de infrastructuur.

**Een upgrade uitvoert, migratie en implementatie van** -focus ziet u hier aanbevelingen voor het upgraden, migreren en implementeren van Active Directory voor uw bestaande infrastructuur.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u zijn gericht voor de beoordeling van 100% in elk focusgebied?
Dat hoeft niet. De aanbevelingen zijn gebaseerd op de kennis en ervaring opgedaan door Microsoft engineers in duizenden klant bezoeken. Echter geen twee server-infrastructuren zijn hetzelfde en specifieke aanbevelingen mogelijk meer of minder relevant zijn voor u. Enkele aanbevelingen voor beveiliging worden minder relevant als uw virtuele machines worden niet blootgesteld aan Internet. Enkele aanbevelingen beschikbaarheid mogelijk minder relevant zijn voor services die de ad-hoc gegevensverzameling met lage prioriteit en rapportage bieden. Problemen die belangrijk voor een goed ontwikkelde bedrijf zijn is mogelijk minder belangrijk voor een beginnend bedrijf bent. U kunt om te bepalen welke focusgebieden zijn uw prioriteiten en bekijk hoe uw scores gedurende een bepaalde periode wijzigen.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. U moet deze aanwijzingen gebruiken om te beoordelen of de aanbeveling te implementeren geschikt voor u, als basis van de aard van uw IT-services en de zakelijke behoeften van uw organisatie is.

## <a name="use-health-check-focus-area-recommendations"></a>Gebruik health focus gebied aanbevelingen controleren
Nadat deze is geïnstalleerd, kunt u de samenvatting van aanbevelingen weergeven met behulp van de tegel Serverstatus controleren op de oplossingenpagina in de Azure-portal.

De beoordelingen samengevatte compatibiliteit voor uw infrastructuur en inzoomen in aanbevelingen weergeven.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een focusgebied bekijken en corrigerende actie ondernemen
3. Klik op de **overzicht** tegel voor uw werkruimte voor logboekanalyse in de Azure portal.
4. Op de **overzicht** pagina, klikt u op de **Active Directory-Serverstatus controleren** tegel. 
5. Op de **Health Check** pagina, Controleer de overzichtsgegevens in een van de focus gebied blades en klik op een om aanbevelingen voor het desbetreffende focusgebied weer te geven.
6. Op een van de focus gebiedspagina's, kunt u de prioriteit aanbevelingen voor uw omgeving weergeven. Klik op een aanbeveling onder **objecten van invloed op een** voor meer informatie over waarom de aanbeveling is gemaakt.<br><br> ![afbeelding van aanbevelingen Serverstatus controleren](./media/log-analytics-ad-assessment/ad-healthcheck-dashboard-02.png)
7. U kunt ondernemen corrigerende maatregelen voorgesteld in **voorgestelde acties**. Wanneer het item is opgelost, hoger beoordelingen records die acties aanbevolen zijn uitgevoerd en de naleving-score wordt verhoogd. Gecorrigeerde items worden weergegeven als **doorgegeven objecten**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren
Als u hebt de aanbevelingen die u wilt negeren, kunt u een tekstbestand dat Log Analytics gebruikt om te voorkomen dat de aanbevelingen worden weergegeven in de resultaten van uw beoordeling.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Voor het identificeren van de aanbevelingen die u worden genegeerd
1. Klik in de Azure-portal op de pagina van de werkruimte Log Analytics voor de geselecteerde werkruimte op het **logboek zoeken** tegel.
2. Gebruik de volgende query uit om de lijst met aanbevelingen die zijn mislukt voor computers in uw omgeving.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```
    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
    >
    > `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Hier volgt een schermopname de logboek-zoekopdracht wordt weergegeven:<br><br> ![mislukte aanbevelingen](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

3. Kies de aanbevelingen die u wilt negeren. Gebruik de waarden voor RecommendationId zult u in de volgende procedure.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Maken en gebruiken van een tekstbestand IgnoreRecommendations.txt
1. Maak een bestand met de naam IgnoreRecommendations.txt.
2. Plakken of typ elke RecommendationId voor elke aanbeveling dat u wilt dat Log Analytics negeren op een afzonderlijke regel en vervolgens opslaan en sluit het bestand.
3. Plaats het bestand in de volgende map op elke computer waarop u logboekanalyse voor het negeren van aanbevelingen.
   * Op computers met Microsoft Monitoring Agent (rechtstreeks of via de Operations Manager verbonden) - *SystemDrive*: \Program Files\Microsoft Agent\Agent bewaking
   * Op de beheerserver van Operations Manager 2012 R2 - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server 
   * Op de beheerserver van Operations Manager 2016 - *SystemDrive*: \Program Files\Microsoft System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te controleren dat de aanbevelingen worden genegeerd
Na de volgende health controle wordt uitgevoerd, standaard elke zeven dagen geplande, de opgegeven aanbevelingen zijn gemarkeerd *genegeerd* en worden niet weergegeven op het dashboard.

1. U kunt de volgende logboek zoekquery's gebruiken voor een lijst met alle aanbevelingen genegeerd.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
    >[!NOTE]
    > Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
    >
    > `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Als u later besluit dat u wilt zien genegeerd aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of u kunt RecommendationIDs verwijderen uit deze.

## <a name="ad-health-check-solutions-faq"></a>AD Health Veelgestelde vragen over het selectievakje oplossingen
*Hoe vaak wordt een controle uitgevoerd?*

* De controle elke zeven dagen uitgevoerd.

*Is er een manier om te configureren hoe vaak de statuscontrole wordt uitgevoerd?*

* Op dit moment niet.

*Als een andere server voor wordt gedetecteerd nadat ik heb een oplossing van de controle van status toegevoegd zal worden gecontroleerd*

* Ja, zodra deze is gedetecteerd dat wordt gecontroleerd van vervolgens, elke zeven dagen.

*Als een server buiten werking wordt gesteld, wanneer deze verwijderd uit de statuscontrole?*

* Als een server komt niet met het verzenden van gegevens voor drie weken, wordt deze verwijderd.

*Wat is de naam van het proces dat het verzamelen van gegevens biedt?*

* AdvisorAssessment.exe

*Hoe lang duurt het voor gegevens moeten worden verzameld?*

* De verzameling actuele gegevens op de server duurt ongeveer 1 uur. Kan het langer duren op servers die een groot aantal Active Directory-servers hebt.

*Is er een manier om te configureren wanneer gegevens worden verzameld?*

* Op dit moment niet.

*Waarom worden alleen de top 10 aanbevelingen weergegeven?*

* In plaats van zodat u overweldigend uitputtende lijst met taken, is het raadzaam dat u zich richten op de prioriteit aanbevelingen eerst adressering. Nadat u deze oplossen, wordt extra aanbevelingen beschikbaar worden. Als u liever voor een gedetailleerd overzicht, kunt u alle aanbevelingen logboek zoekactie kunt weergeven.

*Is er een manier om een aanbeveling negeren?*

* Ja, Zie [aanbevelingen negeren](#ignore-recommendations) sectie hierboven.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md) voor informatie over het analyseren van gedetailleerde gegevens AD-Serverstatus controleren en aanbevelingen.
