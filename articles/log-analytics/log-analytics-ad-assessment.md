---
title: Optimalisatie van uw Active Directory-omgeving met Azure Log Analytics | Microsoft Docs
description: U kunt de beoordeling van Active Directory-oplossing gebruiken voor het evalueren van de risico's en de status van uw server-omgevingen met regelmatige tussenpozen.
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
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97368f0b9e89ffd0cd982b6e8670d5a1f62ad42c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimalisatie van uw Active Directory-omgeving met de beoordeling van Active Directory-oplossing in Log Analytics

![Symbool van AD-evaluatie](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

U kunt de beoordeling van Active Directory-oplossing gebruiken voor het evalueren van de risico's en de status van uw server-omgevingen met regelmatige tussenpozen. In dit artikel helpt u bij het installeren en gebruiken van de oplossing zodat u corrigerende maatregelen op mogelijke problemen kunt uitvoeren.

Deze oplossing biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor uw geïmplementeerde serverinfrastructuur. De aanbevelingen worden onderverdeeld in vier focusgebieden, waarmee u snel het risico begrijpt en actie ondernemen.

De aanbevelingen zijn gebaseerd op de kennis en ervaring door Microsoft engineers van duizenden klant bezoeken. Elke aanbeveling bevat richtlijnen over waarom er een probleem mogelijk interessant voor u en het implementeren van de voorgestelde wijzigingen.

U kunt focusgebieden die het meest belangrijk zijn voor uw organisatie en het bijhouden van uw progressie ten opzichte van een omgeving met risico gratis en goed wordt uitgevoerd.

Nadat u de oplossing hebt toegevoegd en een beoordeling voltooid, samenvattende is informatie voor focusgebieden wordt weergegeven op de **AD Assessment** dashboard voor de infrastructuur in uw omgeving. De volgende secties wordt beschreven hoe u de informatie te gebruiken op de **AD Assessment** dashboard, waar u kunt weergeven en vervolgens te aanbevolen acties voor de infrastructuur van de Active Directory-server.

![afbeelding van de beoordeling van de SQL-tegel](./media/log-analytics-ad-assessment/ad-tile.png)

![afbeelding van de beoordeling van de SQL-dashboard](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om te installeren en configureren van de oplossingen.

* Agents moeten worden geïnstalleerd op domeincontrollers die lid van het domein zijn moet worden geëvalueerd.
* De beoordeling van Active Directory-oplossing is een ondersteunde versie van .NET Framework 4 vereist (4.5.2 of hoger) op elke computer die een OMS-agent is geïnstalleerd.
* De beoordeling van Active Directory-oplossing toevoegen aan uw OMS-werkruimte van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.

  > [!NOTE]
  > Nadat u de oplossing hebt toegevoegd, wordt het bestand AdvisorAssessment.exe toegevoegd aan servers met agents. Configuratiegegevens is gelezen en vervolgens doorgestuurd naar de OMS-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en registreert de gegevens van de cloudservice.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory Assessment verzameling Gegevensdetails

Active Directory-Assessment verzamelt gegevens uit de volgende bronnen met behulp van de agents die u hebt ingeschakeld:

- Register-collectors
- LDAP-collectors
- .NET framework
- Gebeurtenis logboekverzamelaars
- Active Directory Service interfaces (ADSI)
- Windows Powershell
- Bestand gegevens collectors
- Windows Management Instrumentation (WMI)
- DCDIAG hulpprogramma API
- API voor File Replication Service (NTFRS)
- Aangepaste C#-code


De volgende tabel bevat de methoden van de collectie voor agents, of Operations Manager (SCOM) is vereist, en hoe vaak gegevens worden verzameld door een agent.

| Platform | Directe Agent | SCOM-agents | Azure Storage | SCOM vereist? | SCOM-agent gegevens die worden verzonden via de beheergroep | Frequentie van de verzameling |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |7 dagen |

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

## <a name="use-assessment-focus-area-recommendations"></a>Gebruik assessment focus gebied aanbevelingen
Voordat u een oplossing voor evaluatie in OMS gebruiken kunt, moet u de oplossing geïnstalleerd hebben. Voor meer informatie over het installeren van oplossingen, Zie [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). Nadat deze is geïnstalleerd, kunt u de samenvatting van aanbevelingen weergeven met behulp van de tegel Assessment op de pagina overzicht in OMS.

De beoordelingen samengevatte compatibiliteit voor uw infrastructuur en inzoomen in aanbevelingen weergeven.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een focusgebied bekijken en corrigerende actie ondernemen
1. Op de **overzicht** pagina, klikt u op de **Assessment** tegel voor uw serverinfrastructuur.
2. Op de **Assessment** pagina, Controleer de overzichtsgegevens in een van de focus gebied blades en klik op een om aanbevelingen voor het desbetreffende focusgebied weer te geven.
3. Op een van de focus gebiedspagina's, kunt u de prioriteit aanbevelingen voor uw omgeving weergeven. Klik op een aanbeveling onder **objecten van invloed op een** voor meer informatie over waarom de aanbeveling is gemaakt.  
    ![afbeelding van Assessment aanbevelingen](./media/log-analytics-ad-assessment/ad-focus.png)
4. U kunt ondernemen corrigerende maatregelen voorgesteld in **voorgestelde acties**. Wanneer het item is opgelost, hoger beoordelingen records die acties aanbevolen zijn uitgevoerd en de naleving-score wordt verhoogd. Gecorrigeerde items worden weergegeven als **doorgegeven objecten**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren
Als u hebt de aanbevelingen die u wilt negeren, kunt u een tekstbestand dat OMS wordt gebruikt om te voorkomen dat de aanbevelingen worden weergegeven in de resultaten van uw beoordeling.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Voor het identificeren van de aanbevelingen die u worden genegeerd
1. Aanmelden bij uw werkruimte en logboek zoekopdracht openen. Gebruik de volgende query uit om de lijst met aanbevelingen die zijn mislukt voor computers in uw omgeving.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   Hier volgt een schermafbeelding met de query-logboek: ![aanbevelingen is mislukt](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)
2. Kies de aanbevelingen die u wilt negeren. Gebruik de waarden voor RecommendationId zult u in de volgende procedure.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Maken en gebruiken van een tekstbestand IgnoreRecommendations.txt
1. Maak een bestand met de naam IgnoreRecommendations.txt.
2. Plakken of typ elke RecommendationId voor elke aanbeveling dat u wilt dat Log Analytics negeren op een afzonderlijke regel en vervolgens opslaan en sluit het bestand.
3. Plaats het bestand in de volgende map op elke computer waarop u OMS aanbevelingen negeren.
   * Op computers met Microsoft Monitoring Agent (rechtstreeks of via de Operations Manager verbonden) - *SystemDrive*: \Program Files\Microsoft Agent\Agent bewaking
   * Op de beheerserver van Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te controleren dat de aanbevelingen worden genegeerd
Na de volgende evaluatie wordt uitgevoerd, standaard elke 7 dagen geplande, de opgegeven aanbevelingen zijn gemarkeerd *genegeerd* en worden niet weergegeven op het dashboard assessment.

1. U kunt de volgende logboek zoekquery's gebruiken voor een lijst met alle aanbevelingen genegeerd.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Als u later besluit dat u wilt zien genegeerd aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of u kunt RecommendationIDs verwijderen uit deze.

## <a name="ad-assessment-solutions-faq"></a>AD-evaluatie oplossingen Veelgestelde vragen
*Hoe vaak wordt een evaluatie uitgevoerd?*

* De evaluatie wordt uitgevoerd om de zeven dagen.

*Is er een manier om te configureren hoe vaak de beoordeling wordt uitgevoerd?*

* Momenteel niet.

*Als een andere server wordt gedetecteerd nadat ik een evaluatie-oplossing hebt toegevoegd, wordt deze gecontroleerd?*

* Ja, zodra deze is gedetecteerd. deze wordt beoordeeld van vervolgens, elke 7 dagen.

*Als een server buiten werking wordt gesteld, wanneer deze verwijderd uit de beoordeling?*

* Als een server komt niet met het verzenden van gegevens voor drie weken, wordt deze verwijderd.

*Wat is de naam van het proces dat het verzamelen van gegevens biedt?*

* AdvisorAssessment.exe

*Hoe lang duurt het voor gegevens moeten worden verzameld?*

* De verzameling actuele gegevens op de server duurt ongeveer 1 uur. Kan het langer duren op servers die een groot aantal Active Directory-servers hebt.

*Is er een manier om te configureren wanneer gegevens worden verzameld?*

* Momenteel niet.

*Waarom worden alleen de top 10 aanbevelingen weergegeven?*

* In plaats van zodat u overweldigend uitputtende lijst met taken, is het raadzaam dat u zich richten op de prioriteit aanbevelingen eerst adressering. Nadat u deze oplossen, wordt extra aanbevelingen beschikbaar worden. Als u liever voor een gedetailleerd overzicht, kunt u alle aanbevelingen logboek zoekactie kunt weergeven.

*Is er een manier om een aanbeveling negeren?*

* Ja, Zie [aanbevelingen negeren](#ignore-recommendations) sectie hierboven.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md) om gedetailleerde gegevens voor de Nalevingsbeoordeling van AD en aanbevelingen weer te geven.
