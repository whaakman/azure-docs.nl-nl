---
title: Optimalisatie van uw Active Directory-omgeving met Azure Monitor | Microsoft Docs
description: De oplossing statuscontrole van Active Directory kunt u het risico en de status van uw omgeving beoordelen op een vast interval.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/27/2017
ms.author: magoedte
ms.openlocfilehash: 3b5da6c9046fc694bd5eb0f55cf031b82b6d0103
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370530"
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-health-check-solution-in-azure-monitor"></a>Optimalisatie van uw Active Directory-omgeving met de oplossing statuscontrole van Active Directory in Azure Monitor

![AD Health Check symbool](./media/ad-assessment/ad-assessment-symbol.png)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

De oplossing statuscontrole van Active Directory kunt u het risico en de status van uw server-omgevingen evalueren op een vast interval. Dit artikel helpt u bij het installeren en gebruiken van de oplossing zodat u corrigerende maatregelen voor potentiële problemen kunt uitvoeren.

Deze oplossing biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor uw geïmplementeerde serverinfrastructuur. De aanbevelingen zijn onderverdeeld in vier gebieden, waarmee u snel inzicht in het risico en actie te ondernemen.

De aanbevelingen zijn gebaseerd op de kennis en ervaring met Microsoft-engineers van duizenden klanten bezoeken. Elke aanbeveling bevat richtlijnen over waarom een probleem voor u mogelijk belangrijk en het implementeren van de voorgestelde wijzigingen.

U kunt gebieden die het meest relevant zijn voor uw organisatie en de voortgang naar het uitvoeren van een risico's gratis en gezonde omgeving bijhouden.

Nadat u de oplossing hebt toegevoegd en een controle voltooid, samenvattende is informatie voor focusgebieden wordt weergegeven op de **AD Health Check** dashboard voor de infrastructuur in uw omgeving. De volgende secties wordt beschreven hoe u gebruik van de informatie op de **AD Health Check** dashboard, waar u kunt weergeven en vervolgens ondernemen aanbevolen acties voor de infrastructuur van uw Active Directory-server.  

![afbeelding van de tegel AD-statuscontrole](./media/ad-assessment/ad-healthcheck-summary-tile.png)

![afbeelding van AD Health Check-dashboard](./media/ad-assessment/ad-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Vereisten

* De oplossing statuscontrole van Active Directory vereist een ondersteunde versie van .NET Framework 4.5.2 of hoger geïnstalleerd op elke computer die de Microsoft Monitoring Agent (MMA) geïnstalleerd is.  De MMA-agent wordt gebruikt door System Center 2016 - Operations Manager en Operations Manager 2012 R2 en Azure Monitor.
* De oplossing biedt ondersteuning voor domeincontrollers met Windows Server 2008 en 2008 R2, Windows Server 2012 en 2012 R2 en Windows Server 2016.
* Een Log Analytics-werkruimte toe te voegen van de oplossing statuscontrole van Active Directory in Azure marketplace in Azure portal.  Er is geen verdere configuratie nodig.

  > [!NOTE]
  > Nadat u de oplossing hebt toegevoegd, wordt het bestand AdvisorAssessment.exe toegevoegd aan servers met agents. Configuratiegegevens is gelezen en vervolgens naar Azure Monitor in de cloud verzonden voor verwerking. Logica wordt toegepast op de ontvangen gegevens en de cloudservice registreert de gegevens.
  >
  >

Als u de statuscontrole voor uw domeincontrollers die lid van het domein zijn moet worden geëvalueerd, hiervoor een agent en de verbinding met Azure Monitor met een van de volgende ondersteunde methodes:

1. Installeer de [Microsoft Monitoring Agent (MMA)](../../azure-monitor/platform/agent-windows.md) als de domeincontroller niet door System Center 2016 - Operations Manager of Operations Manager 2012 R2 bewaakt wordt.
2. Als deze wordt bewaakt met System Center 2016 - Operations Manager of Operations Manager 2012 R2 en de beheergroep is niet geïntegreerd met Azure Monitor, kan de domeincontroller worden met Azure Monitor voor het verzamelen van gegevens en door te sturen naar de service en nog steeds multihomed worden bewaakt door Operations Manager.  
3. Anders, als uw Operations Manager-beheergroep is geïntegreerd met de service, moet u de domeincontrollers voor het verzamelen van gegevens toevoegen door de service die de stappen onder [toevoegen door agents beheerde computers](../../azure-monitor/platform/om-agents.md#connecting-operations-manager-to-azure-monitor) nadat u hebt ingeschakeld de oplossing in uw werkruimte.  

De agent op uw domeincontroller welke rapporten in een Operations Manager-beheergroep, verzamelt gegevens, doorstuurt naar de toegewezen beheerserver, en vervolgens rechtstreeks vanaf een beheerserver verzonden naar Azure Monitor.  De gegevens worden niet naar de Operations Manager-databases geschreven.  

## <a name="active-directory-health-check-data-collection-details"></a>Active Directory-statuscontrole gegevens verzameling details

Statuscontrole van Active Directory, worden gegevens verzameld uit de volgende bronnen met behulp van de agent die u hebt ingeschakeld:

- Register
- LDAP
- .NET Framework
- Gebeurtenislogboek
- Active Directory Service interfaces (ADSI)
- Windows Powershell
- Gegevens uit een bestand
- Windows Management Instrumentation (WMI)
- DCDIAG hulpprogramma API
- File Replication Service (NTFRS) API
- Aangepaste C#-code

Gegevens worden verzameld op de domeincontroller en doorgestuurd naar Azure Monitor om de zeven dagen.  

## <a name="understanding-how-recommendations-are-prioritized"></a>Inzicht krijgen in hoe aanbevelingen met prioriteit wordt toegepast
Elke aanbeveling krijgt een waarde valutaconversie in gevallen waarin het relatieve belang van de aanbeveling. Alleen de 10 belangrijkste aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Hoe het gewicht worden berekend
Wegingen zijn samengevoegde waarden op basis van drie belangrijke factoren:

* De *kans* problemen zorgt ervoor dat een probleem dat is geïdentificeerd. Een grotere kans is gelijk aan een grotere totale score voor de aanbeveling.
* De *impact* van het probleem op uw organisatie als dit leidt een probleem tot. Er is een hogere impact gelijk aan een grotere totale score voor de aanbeveling.
* De *inspanning* vereist de aanbeveling kunt implementeren. Er is een hogere inspanning gelijk aan een kleinere totale score voor de aanbeveling.

Het gewicht voor elke aanbeveling wordt uitgedrukt als een percentage van de totale score voor elke aandachtsgebied beschikbaar. Bijvoorbeeld, als een aanbeveling in de beveiliging en naleving aandachtsgebied heeft een score van 5%, verhoogd uitvoering van deze aanbeveling uw algemene beveiliging en naleving score van 5%.

### <a name="focus-areas"></a>Focusgebieden
**Beveiliging en naleving** -deze aandachtsgebied bevat aanbevelingen voor potentiële beveiligingsrisico's opsporen en schendingen van bedrijfsbeleid en technische, juridische en wettelijke nalevingsvereisten.

**Beschikbaarheid en bedrijfscontinuïteit** -deze aandachtsgebied bevat aanbevelingen voor beschikbaarheid van de service, de tolerantie van uw infrastructuur en zakelijke beveiliging.

**Prestaties en schaalbaarheid** -deze aandachtsgebied bevat aanbevelingen voor uw organisatie het IT-infrastructuur groeien, zorg ervoor dat uw IT-omgeving voldoet aan de huidige prestatievereisten en kan reageren op veranderende infrastructuur behoeften.

**Upgrade, migratie en implementatie van** -deze aandachtsgebied bevat aanbevelingen voor het upgraden, migreren en implementeren van Active Directory met uw bestaande infrastructuur.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u erop gericht om 100% te beoordelen in elke aandachtsgebied?
Dat hoeft niet. De aanbevelingen zijn gebaseerd op de kennis en ervaringen die zijn verkregen dankzij het Microsoft-technici via duizenden klanten bezoeken. Echter geen twee server-infrastructuur zijn hetzelfde, en specifieke aanbevelingen mogelijk meer of minder relevant voor u. Bijvoorbeeld, enkele aanbevelingen voor beveiliging mogelijk minder relevant zijn als uw virtuele machines worden niet blootgesteld aan Internet. Aanbevelingen voor sommige beschikbaarheid mogelijk minder relevant zijn voor services met lage prioriteit ad-hoc gegevens verzamelen en rapporteren. Problemen die tot een volwassen bedrijf van belang zijn mogelijk minder belangrijk voor een lid is. Kunt u om te bepalen welke focusgebieden zijn uw prioriteiten en kijken hoe uw scores na verloop van tijd veranderen.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. U moet deze handleiding gebruiken om te beoordelen of het implementeren van de aanbeveling geschikt voor u is, gezien de aard van uw IT-services en de zakelijke eisen van uw organisatie.

## <a name="use-health-check-focus-area-recommendations"></a>Gebruik statuscontrole focus gebied aanbevelingen
Nadat deze is geïnstalleerd, kunt u het overzicht van de aanbevelingen weergeven met behulp van de tegel status controleren op de pagina van de oplossing in Azure portal.

De beoordelingen samengevatte naleving voor uw infrastructuur en aanbevelingen voor inzoomen in weergeven.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een aandachtsgebied weergeven en neemt u corrigerende maatregelen
[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

1. Op de **overzicht** pagina, klikt u op de **statuscontrole van Active Directory** tegel.
1. Op de **statuscontrole** pagina, Controleer de samenvattingsinformatie in een van de focus gebied blades en klik vervolgens op een om aanbevelingen voor die aandachtsgebied weer te geven.
1. Op een van de focus gebiedspagina's, kunt u de aanbevelingen met prioriteit gemaakt voor uw omgeving bekijken. Klik op een aanbeveling onder **betrokken objecten** voor meer informatie over waarom de aanbeveling is gedaan.<br><br> ![afbeelding van aanbevelingen voor de statuscontrole](./media/ad-assessment/ad-healthcheck-dashboard-02.png)
1. U kunt ondernemen corrigerende maatregelen voorgesteld in **voorgestelde acties**. Wanneer het item is opgelost, hoger evaluaties records die acties aanbevolen zijn uitgevoerd en worden uw nalevingsscore verhoogd. Gecorrigeerde items worden weergegeven als **doorgegeven objecten**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren
Als u aanbevelingen die u wilt negeren hebt, kunt u een tekstbestand dat door Azure Monitor wordt gebruikt om te voorkomen dat de aanbevelingen wordt weergegeven in de resultaten van de evaluatie maken.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Voor het identificeren van de aanbevelingen die u worden genegeerd
[!INCLUDE [azure-monitor-log-queries](../../../includes/azure-monitor-log-queries.md)]

Gebruik de volgende query uit om de lijst met aanbevelingen die zijn mislukt voor computers in uw omgeving.

```
ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation
```

Hier volgt een schermafbeelding van het logboekquery:<br><br> ![mislukte aanbevelingen](media/ad-assessment/ad-failed-recommendations.png)

Kies de aanbevelingen die u wilt negeren. U gebruikt de waarden voor RecommendationId in de volgende procedure.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Het maken en gebruiken van een tekstbestand IgnoreRecommendations.txt
1. Maak een bestand met de naam IgnoreRecommendations.txt.
2. Plak of typ elke RecommendationId voor elke aanbeveling dat u wilt dat Azure Monitor op een afzonderlijke regel negeren en vervolgens opslaan en sluiten van het bestand.
3. Plaats het bestand in de volgende map op elke computer waar u Azure-Monitor voor het negeren van aanbevelingen.
   * Op computers met de Microsoft Monitoring Agent (verbonden rechtstreeks of via de Operations Manager) - *SystemDrive*: \Program Files\Microsoft Monitoring Agent\Agent
   * Op de beheerserver van Operations Manager 2012 R2 - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server
   * Op de beheerserver van Operations Manager 2016 - *SystemDrive*: \Program Files\Microsoft System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te controleren of dat er aanbevelingen worden genegeerd
Na de volgende health controle wordt uitgevoerd, wordt standaard elke zeven dagen geplande, zijn gemarkeerd als de opgegeven aanbevelingen *genegeerd* en worden niet weergegeven op het dashboard.

1. U kunt de volgende logboeken-query's gebruiken om de genegeerde aanbevelingen weer te geven.

    ```
    ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```

2. Als u later besluit dat u wilt zien genegeerde aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of kunt u RecommendationIDs verwijderen uit deze.

## <a name="ad-health-check-solutions-faq"></a>AD Health solutions Veelgestelde vragen over het selectievakje
*Hoe vaak wordt een controle van gatewayservicestatus uitgevoerd?*

* De controle is elke zeven dagen uitgevoerd.

*Is er een manier om te configureren hoe vaak de statuscontrole wordt uitgevoerd?*

* Momenteel niet.

*Als een andere server voor wordt gedetecteerd nadat ik heb een oplossing status van selectievakje toegevoegd zal worden gecontroleerd*

* Ja, zodra deze gedetecteerd wordt deze gecontroleerd van vervolgens op elke zeven dagen.

*Als een server uit bedrijf genomen is, wanneer wordt deze verwijderd uit de statuscontrole?*

* Als een server komt niet met het verzenden van gegevens voor drie weken, wordt deze verwijderd.

*Wat is de naam van het proces dat het verzamelen van gegevens wordt?*

* AdvisorAssessment.exe

*Hoe lang duurt het voordat gegevens worden verzameld?*

* Het verzamelen van de werkelijke gegevens op de server duurt ongeveer 1 uur. Het kan langer duren op servers die een groot aantal Active Directory-servers hebben.

*Is er een manier om te configureren wanneer gegevens worden verzameld?*

* Momenteel niet.

*Waarom worden alleen de top 10 aanbevelingen weergegeven?*

* In plaats van zodat u enorme uitputtende lijst van taken, is het raadzaam dat u zich richten op de aanbevelingen met prioriteit eerst adressering. Nadat u deze kunt oplossen, worden extra aanbevelingen beschikbaar. Als u liever de gedetailleerde lijst, kunt u met behulp van een query voor toegang tot alle aanbevelingen weergeven.

*Is er een manier om een aanbeveling negeren?*

* Ja, Zie [aanbevelingen negeren](#ignore-recommendations) hierboven.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [logboeken-query's van Azure Monitor](../log-query/log-query-overview.md) voor meer informatie over het analyseren van gedetailleerde gegevens AD Health Check en aanbevelingen.
