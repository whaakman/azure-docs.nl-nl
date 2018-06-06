---
title: Waarschuwingen in de Azure-Monitor Unified | Microsoft Docs
description: Beschrijving van unified waarschuwingen in Azure, waarmee u voor het beheren van waarschuwingen en regels waarschuwingen alle Azure-services.
author: manishsm-msft
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: mamit,bwren
ms.custom: ''
ms.openlocfilehash: dac8052a96c98130891a9ea31994198e331e314d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726462"
---
# <a name="unified-alerts-in-azure-monitor"></a>Unified waarschuwingen in de Azure-Monitor

## <a name="overview"></a>Overzicht

> [!NOTE]
>  Een nieuwe geïntegreerde ervaring waarschuwing waarmee u voor het beheren van waarschuwingen uit meerdere abonnementen en waarschuwing statussen en slimme groepen is momenteel beschikbaar in public preview introduceert. Zie de [laatste sectie van dit artikel](#enhanced-unified-alerts-experience-public-preview) voor een beschrijving van deze gebruikerservaring verbeterde en het proces is ingeschakeld.


In dit artikel beschrijft de uniforme waarschuwing ervaring in Azure-Monitor. De [waarschuwing ervaring](monitoring-overview-alerts.md) beschikbaar is via de **waarschuwingen (klassiek)** optie in het menu van Azure-Monitor. 

## <a name="features-of-the-unified-alert-experience"></a>Functies van de geïntegreerde ervaring voor waarschuwing

De geïntegreerde ervaring biedt de volgende voordelen via de klassieke ervaring:

-   **Betere notification system**: waarschuwingen gebruik Unified [actiegroepen]( https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups), die groepen meldingen en acties die opnieuw kunnen worden gebruikt in meerdere waarschuwingen worden genoemd. 
- **Geïntegreerde ervaring** : U kunt waarschuwingen beheren en waarschuwing regels voor metrische gegevens, logboeken en activiteit zich aanmelden via Azure Monitor, logboekanalyse en Application Insights op één plek. 
- **Weergave gestart logboekanalyse waarschuwingen in Azure portal** -waarschuwingen van logboekanalyse met andere waarschuwingen uit andere bronnen in de Azure-portal bekijken. Eerder waren deze in een afzonderlijke portal.
- **Scheiding van Fired waarschuwingen en waarschuwingsregels** -waarschuwing regels nu onderscheiden zich van waarschuwingen. Een waarschuwingsregel is de definities van een voorwaarde die een waarschuwing activeert. Een waarschuwing is een exemplaar van een waarschuwingsregel starten.
- **Betere werkstroom** -de uniforme waarschuwing ervaring leidt u door het configuratieproces van een waarschuwingsregel.
 
De volgende verbeteringen hebben metrische waarschuwingen via de klassieke metrische waarschuwingen:

-   **Verbeterde latentie**: metrische waarschuwingen zo vaak als elke één minuut kunnen uitvoeren. Klassieke metrische waarschuwingen wordt altijd uitgevoerd met een frequentie van 5 minuten. Logboek waarschuwingen hebben nog steeds een vertraging die langer dan een minuut vanwege de tijd vergt om op te nemen van de logboeken is. 
-   **Ondersteuning voor multidimensionale metrische gegevens**: U kunt een waarschuwing op dimensionale metrische gegevens, zodat u kunt een specifiek exemplaar van de metrische gegevens controleren.
-   **Meer controle over metrische voorwaarden**: U kunt uitgebreidere waarschuwingsregels die ondersteuning bieden voor bewaking van de maximale, minimale gemiddelde en totale waarden van de metrische gegevens definiëren.
-   **Bewaking van meerdere metrische gegevens gecombineerd**: U kunt maximaal twee metrische gegevens met een enkele regel bewaken. Er wordt een waarschuwing gegeven als beide metrische gegevens in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
-   **Metrische gegevens uit logboeken** (beperkte openbare preview): sommige logboek bij Log Analytics-gegevens nu kunnen worden uitgepakt en geconverteerd worden naar Azure Monitor metrische gegevens en vervolgens een melding op net als andere metrische gegevens. 


## <a name="alert-rules"></a>Waarschuwingsregels
De ervaring voor de geïntegreerde waarschuwingen maakt gebruik van de volgende concepten waarschuwingsregels scheiden van waarschuwingen tijdens de ontwerpmogelijkheden samen te voegen tussen de verschillende typen waarschuwingen.

| Item | Definitie |
|:---|:---|
| Waarschuwingsregel | De definitie van de conditie van een waarschuwing wilt maken. Bestaat uit een _doelresource_, _signaal_, _criteria_, en _logica_. Een waarschuwingsregel is alleen beschikbaar als het zich in een _ingeschakeld_ status.
| Doelbron | Definieert de specifieke resources en signalen beschikbaar voor waarschuwingen. Een doel is een Azure-resource.<br>Voorbeelden: virtuele machine, storage-account, virtuele-machineschaalset, werkruimte voor logboekanalyse, Application Insights-resource |
| Signaal | Bron van gegevens die door de doelbron. Ondersteunde signaaltypen zijn *metriek*, *activiteitenlogboek*, *Application Insights*, en *logboek*. |
| Criteria | Combinatie van _signaal_ en _logica_ toegepast op een doelbron.<br>Voorbeelden: Percentage CPU > 70%, serverreactietijd > 4 ms, aantal resultaten van een logboek query > 100 enzovoort. |
| Logica | Gebruiker gedefinieerde logica wilt controleren of het signaal binnen bereik en-waarden verwacht. |
| Bewerking | De bewerking uit te voeren wanneer de waarschuwing wordt geactiveerd. Meerdere acties kunnen optreden wanneer een waarschuwing wordt geactiveerd. Deze waarschuwingen ondersteuning voor groepen in te grijpen.<br>Voorbeelden: e-mailen van een e-mailadres voor het aanroepen van een webhook-URL. |
| Monitorconditie | Hiermee wordt aangegeven of de voorwaarde die een waarschuwing voor metrische gemaakt vervolgens is opgelost. Metrische waarschuwingsregels voorbeeld van een bepaalde metriek met regelmatige tussenpozen. Als u de criteria in de waarschuwingsregel wordt voldaan, wordt een nieuwe waarschuwing gemaakt met een voorwaarde van Fired.  Wanneer de metriek is door actieve opnieuw als de criteria wordt nog steeds voldaan, wordt er niets gebeurt.  Als u de criteria echter niet wordt voldaan, wordt de voorwaarde van de waarschuwing gewijzigd naar opgelost. De volgende keer dat de criteria wordt voldaan, wordt een waarschuwing voor een andere gemaakt met een voorwaarde van Fired. |


## <a name="alert-pages"></a>Waarschuwing pagina 's
Unified waarschuwingen bevatten één kunt u uw Azure waarschuwingen bekijken en beheren. De volgende secties worden de functies van elke afzonderlijke pagina van de uniforme wijze van werken.

### <a name="alerts-overview-page"></a>Overzichtspagina van waarschuwingen
**Waarschuwingen** overzichtspagina geeft een samengevoegde samenvatting weer van alle gestarte waarschuwingen en het totale aantal regels voor waarschuwingen ingeschakeld. De statistische functies wijzigen van de abonnementen of filterparameters updates en de waarschuwingen lijst wordt gestart.

 ![overzicht van waarschuwingen](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Waarschuwingsregels management
**Regels** één pagina voor het beheren van alle regels voor waarschuwingen over uw Azure-abonnementen. Hierin worden alle regels voor waarschuwingen en kan worden gesorteerd op basis van doelresources, resourcegroepen, regelnaam of status. Waarschuwingsregels kunnen ook worden bewerkte nd- of uitgeschakeld op deze pagina.

 ![waarschuwingen-regels](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="creating-an-alert-rule"></a>Een waarschuwingsregel maken
Waarschuwingen worden geschreven in een consistente manier ongeacht de bewakingsservice of type aangegeven. Alle waarschuwingen geactiveerd en gerelateerde details zijn beschikbaar in één pagina.
 
U kunt een nieuwe waarschuwingsregel maken met de volgende drie stappen:
1. Kies de _doel_ voor de waarschuwing.
1. Selecteer de _signaal_ uit de beschikbare signalen voor het doel.
1. Geef de _logica_ moet worden toegepast op gegevens uit het signaal.
 
Deze vereenvoudigde ontwerpproces vereist niet langer dat gebruikers weten de bewaking bron- of signalen ondersteund voordat u een Azure-resource. De lijst met beschikbare signalen automatisch gefilterd op basis van de doelbron geselecteerd en helpt u bij het definiëren van de logica van de waarschuwingsregel.

U kunt meer informatie over het maken van regels voor waarschuwingen in [maken, weergeven en beheren van waarschuwingen via Azure Monitor](monitor-alerts-unified-usage.md).

Waarschuwingen zijn beschikbaar in verschillende Azure-services controleren. Zie voor meer informatie over de en het gebruik van elk van deze services [bewaking Azure-toepassingen en bronnen](./monitoring-overview.md). De volgende tabel bevat een overzicht van de soorten waarschuwingsregels beschikbaar in Azure en wat wordt momenteel ondersteund door de uniforme ervaring van de waarschuwing.

| **Bron van de monitor** | **Signaaltype**  | **Beschrijving** | 
|-------------|----------------|-------------|
| Monitor voor Azure | Gegevens  | Ook wel [near-realtime metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md), ze ondersteunen metrische voorwaarden zo vaak als 1 minuut evalueren en toestaan voor meerdere metrische en multidimensionale metrische regels. Een lijst met ondersteunde resourcetypen is beschikbaar in [nieuwere metrische waarschuwingen voor Azure-services in de Azure portal](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>[Klassieke metrische waarschuwingen](monitoring-overview-alerts.md) worden niet ondersteund in de nieuwe ervaring voor waarschuwingen. U vindt deze onder waarschuwingen (klassiek) in de Azure portal. De klassieke waarschuwingen ondersteuning voor bepaalde typen metrische gegevens die nog niet is verplaatst naar de nieuwe waarschuwingen. Zie voor een volledige lijst [ondersteund metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Logboeken  | Meldingen ontvangen of geautomatiseerde acties uitgevoerd wanneer een zoekquery logboek aan bepaalde criteria voldoet. Waarschuwingen in logboekanalyse zijn [wordt gekopieerd naar de nieuwe ervaring](monitoring-alerts-extend.md). Een [voorbeeld bekijken van *logboekanalyse Logboeken als metrische gegevens* ](monitoring-alerts-extend-tool.md) is beschikbaar. De preview kunt u bepaalde typen logboeken nemen en deze converteren naar metrische gegevens, waar u kunt vervolgens een melding op deze met de nieuwe ervaring voor waarschuwingen. Het voorbeeld is handig als u niet-Azure-logboeken die u wilt ophalen naast systeemeigen Azure Monitor metrische gegevens hebt. |
| Activiteitenlogboeken | Activiteitenlogboek | Bevat de records van alle maken, bijwerken en verwijderen die zijn gemaakt door het geselecteerde doel. |
| Service Health | Activiteitenlogboek  | Niet ondersteund in unified waarschuwingen. Zie [activiteit logboek waarschuwingen maken op servicemeldingen](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Logboeken  | Logboeken met de details van de prestaties van uw toepassing bevat. Analytics-query kunt u de voorwaarden voor de acties moeten worden uitgevoerd op basis van toepassingsgegevens definiëren. |
| Application Insights | Gegevens | Niet ondersteund in unified waarschuwingen. Zie [metrische waarschuwingen]. (.. /Application-Insights/App-Insights-Alerts.MD) |
| Application Insights | Webtests voor beschikbaarheid | Niet ondersteund in unified waarschuwingen.  Zie [Web test waarschuwingen](../application-insights/app-insights-monitor-web-app-availability.md). Beschikbaar voor een website geïnstrumenteerd om gegevens te verzenden naar Application Insights. U ontvangt een melding wanneer de beschikbaarheid of reactiesnelheid van een website lager dan de verwachtingen is. |

## <a name="enhanced-unified-alerts-public-preview"></a>Verbeterde unified waarschuwingen (openbare Preview)

Een uitgebreide waarschuwingen van geïntegreerde ervaring werd uitgebracht in public preview voor Azure Monitor op 1 juni 2018. Deze ervaring die is gebaseerd op de voordelen van [unified waarschuwingen](#overview) maart 2018 uitgebracht en biedt de mogelijkheid te beheren en samenvoegen van afzonderlijke waarschuwingen naast Waarschuwingsstatus wijzigen. Deze sectie beschrijft de nieuwe functies en navigeren in de nieuwe waarschuwing pagina's in de Azure portal.

### <a name="features-enhanced-unified-alerts"></a>Verbeterde functies unified waarschuwingen

De nieuwe ervaring biedt de volgende functies die niet beschikbaar zijn in de klassieke uniforme wijze van werken:

- **Waarschuwingen weergeven voor abonnementen** -u kunt nu bekijken en beheren van afzonderlijke exemplaren van waarschuwingen over meerdere abonnementen in één weergave.
- **De status van waarschuwingen beheren** -waarschuwingen hebben nu een status die aangeeft of de is bevestigd voor gesloten.
- **Waarschuwingen met slim groepen indelen** -Smart groepen automatisch gegroepeerd gerelateerde waarschuwingen zodat u ze als een set in plaats van afzonderlijk kunt beheren.

### <a name="enable-enhanced-unified-alerts"></a>Uitgebreide unified waarschuwingen inschakelen
Schakel de nieuwe ervaring voor unified waarschuwing door te klikken op de banner boven aan de pagina waarschuwingen. Dit proces maakt een waarschuwing store die de afgelopen 30 dagen van gestarte waarschuwingen alle ondersteunde services bevat. Zodra de nieuwe ervaring is ingeschakeld, kunt u heen en weer schakelen tussen de nieuwe en het oude ervaring door te klikken op de koptekst.

> [!NOTE]
>  Duurt enkele minuten duren voordat de nieuwe ervaring in eerste instantie zijn ingeschakeld.

![Banner](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Alle abonnementen die u toegang tot hebt worden geregistreerd wanneer u de nieuwe ervaring inschakelt. Hoewel het hele abonnement is ingeschakeld, zich alleen gebruikers die de nieuwe ervaring geselecteerd weer te geven. Andere gebruikers met toegang tot het abonnement moeten de ervaring afzonderlijk inschakelen.

Inschakelen van de nieuwe ervaring van de waarschuwing heeft geen gevolgen voor de configuratie van het actiegroepen of meldingen in uw regels voor waarschuwingen. Alleen verandert de manier waarop u weergeven en beheren van gestarte exemplaren van de waarschuwingen in de Azure portal.

### <a name="smart-groups"></a>Smart groepen
Smart groepen minder ruis doordat u gerelateerde waarschuwingen beheren als één eenheid in plaats van het beheer van afzonderlijke waarschuwingen. U kunt de details van de smartcard groepen weergeven en stelt de status vergelijkbaar met een waarschuwing. Elke waarschuwing is lid van slechts één smart groep.

Smart groepen worden automatisch gemaakt met behulp van machine learning-gerelateerde waarschuwingen die één probleem vertegenwoordigen combineren. Wanneer een waarschuwing is gemaakt, wordt deze door de algoritme toegevoegd aan een nieuwe groep van de smartcard of een bestaande smart groep op basis van gegevens zoals historische patronen, vergelijkbaar met eigenschappen en gelijkenis van structuur. Op dit moment wordt de algoritme alleen rekening wordt gehouden waarschuwingen van dezelfde monitor service binnen een abonnement. Smart groepen kunnen maximaal 99% van de waarschuwing ruis via deze consolidatie verminderen. U kunt de reden dat waarschuwingen zijn opgenomen in een groep in de detailpagina smartcard groep weergeven.

De naam van een smartcard groep is de naam van de eerste waarschuwing. U kunt maken of wijzig de naam van een smartcard groep.


### <a name="alert-states"></a>Waarschuwing statussen
Verbeterde unified waarschuwingen introduceert het concept van waarschuwingsstatus. U kunt de status van een waarschuwing om op te geven waar deze zich in het omzettingsproces instellen.  Wanneer een waarschuwing is gemaakt, heeft een status van *nieuw*. U kunt de status wijzigen wanneer u een waarschuwing en wanneer u het hebt gesloten hebt bevestigd. Eventuele wijzigingen worden opgeslagen in de geschiedenis van de waarschuwing.

De volgende statussen worden ondersteund.

| Status | Beschrijving |
|:---|:---|
| Nieuw | Het probleem is zojuist gedetecteerd en nog niet herzien. |
| Bevestigd | Een beheerder heeft gecontroleerd van de waarschuwing en is gestart op deze. |
| Gesloten | Het probleem is opgelost. Zodra een waarschuwing is gesloten, u kunt deze opnieuw openen mijn wijzigen naar een andere status. |

De status van een waarschuwing is anders dan de bewakingsvoorwaarde. Metrische waarschuwingsregels voorwaarde voor een waarschuwing kunnen instellen _opgelost_ wanneer de foutstatus niet langer wordt voldaan. Waarschuwing status wordt ingesteld door de gebruiker en is onafhankelijk van de conditie van de monitor. Hoewel het systeem kan de bewakingsvoorwaarde opgelost ingesteld, toestand van de melding niet worden gewijzigd totdat de gebruiker deze wijzigt.

#### <a name="changing-the-state-of-an-alert-or-smart-group"></a>Wijzigen van de status van een waarschuwing of smartcard-groep
U kunt de status van een afzonderlijke waarschuwing wijzigen of meerdere waarschuwingen samen door het instellen van de status van een smartcard groep beheren.

De status van een waarschuwing wijzigen door te klikken op **waarschuwing in een statuswijziging** in de detailweergave voor de waarschuwing of wijzig de status voor een smartcard groep door te klikken op **smart groep in een statuswijziging** in de detailweergave. U kunt de status van meerdere items in één keer wijzigen door ze in een lijst te selecteren en te klikken op **wijziging van de status** boven aan de pagina. In beide gevallen selecteert u een nieuwe status uit de vervolgkeuzelijst en geef optioneel een opmerking. Als u één item wijzigt, hebt u ook een optie voor de dezelfde wijzigingen toepassen op alle waarschuwingen in de groep van de smartcard.

![Status wijzigen](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>pagina waarschuwingen
De standaardpagina waarschuwingen bevat een samenvatting van waarschuwingen die worden gemaakt binnen een bepaalde periode. Het totaal aantal waarschuwingen voor elke ernst wordt weergegeven met kolommen identificeren van het totale aantal waarschuwingen in elke status voor elke ernst. Klik op een van de ernst openen de [alle waarschuwingen](#all-alerts-page) pagina gefilterd op die ernst.

![pagina waarschuwingen](media/monitoring-overview-unified-alerts/alerts-page.png)

U kunt deze weergave filteren door waarden te selecteren in de vervolgkeuzelijsten boven aan de pagina.

| Kolom | Beschrijving |
|:---|:---|
| Abonnement | Selecteer maximaal 5 Azure-abonnementen. Alleen waarschuwingen in de geselecteerde abonnementen zijn opgenomen in de weergave. |
| Resourcegroep | Selecteer één resourcegroep. Alleen waarschuwingen met doelen in de geselecteerde resourcegroep zijn opgenomen in de weergave. |
| Tijdsbereik | Alleen de waarschuwingen die zijn gestart binnen de geselecteerde periode worden opgenomen in de weergave. Ondersteunde waarden zijn uit het verleden uur, afgelopen 24 uur, afgelopen 7 dagen en afgelopen 30 dagen. |

Klik op de volgende waarden boven aan de pagina waarschuwingen om een andere pagina te openen.

| Waarde | Beschrijving |
|:---|:---|
| Totaal aantal waarschuwingen | Totaal aantal waarschuwingen die overeenkomen met de geselecteerde criteria. Klik op deze waarde om te openen van de weergave alle waarschuwingen met geen filter. |
| Smart groepen | Totaal aantal smart groepen die zijn gemaakt op basis van de waarschuwingen die overeenkomen met de geselecteerde criteria. Klik op deze waarde om de lijst slimme groepen in de weergave alle waarschuwingen.
| Totaal aantal waarschuwingsregels | Totale aantal regels voor waarschuwingen in de geselecteerde abonnement en de resource-groep. Klik op deze waarde om de weergave van de regels op de geselecteerde abonnementen en de resourcegroep is gefilterd te openen.


### <a name="all-alerts-page"></a>Pagina met alle waarschuwingen 
De pagina alle waarschuwingen kunt u een overzicht van waarschuwingen die zijn gemaakt in de geselecteerde periode. U kunt ofwel een lijst van de afzonderlijke waarschuwingen of een lijst van de smartcard groepen met de waarschuwingen weergeven. Klik op de banner boven aan de pagina om te schakelen tussen de weergaven.

![Pagina met alle waarschuwingen](media/monitoring-overview-unified-alerts/all-alerts-page.png)

U kunt de weergave filteren op de volgende waarden selecteren in de vervolgkeuzelijsten boven aan de pagina.

| Kolom | Beschrijving |
|:---|:---|
| Abonnement | Selecteer maximaal 5 Azure-abonnementen. Alleen waarschuwingen in de geselecteerde abonnementen zijn opgenomen in de weergave. |
| Resourcegroep | Selecteer één resourcegroep. Alleen waarschuwingen met doelen in de geselecteerde resourcegroep zijn opgenomen in de weergave. |
| Resourcetype | Selecteer een of meer resourcetypen. Alleen waarschuwingen met doelen van het geselecteerde type worden opgenomen in de weergave. In deze kolom is alleen beschikbaar wanneer u een resourcegroep is opgegeven. |
| Resource | Selecteer een resource. Alleen waarschuwingen met resources als doel, zijn opgenomen in de weergave. In deze kolom is alleen beschikbaar wanneer een resourcetype is opgegeven. |
| Severity | Selecteer de ernst van een waarschuwing of *alle* waarschuwingen van alles opnemen. |
| Monitorconditie | Selecteer een voorwaarde voor de monitor of *alle* om op te nemen van waarschuwingen van de voorwaarden. |
| Waarschuwingsstatus | Selecteer een waarschuwing staat of selecteer *alle* om op te nemen van waarschuwingen van statussen. |
| Service bewaken | Selecteer een service of selecteer *alle* om op te nemen van alle services. Alleen de waarschuwingen die zijn gemaakt door regels die gebruikmaken van die service als doel, zijn opgenomen. |
| Tijdsbereik | Alleen de waarschuwingen die zijn gestart binnen de geselecteerde periode worden opgenomen in de weergave. Ondersteunde waarden zijn uit het verleden uur, afgelopen 24 uur, afgelopen 7 dagen en afgelopen 30 dagen. |

Klik op **kolommen** boven aan de pagina om te selecteren welke kolommen om weer te geven. U kunt een van de kolommen met uitzondering van verwijderen 

### <a name="alert-detail-page"></a>De pagina over Waarschuwingsdetails
De detailpagina waarschuwing wordt weergegeven wanneer u een waarschuwing op. Het biedt details van de waarschuwing en kunt u de status te wijzigen.

![Waarschuwingsdetails](media/monitoring-overview-unified-alerts/alert-detail.png)

De pagina over Waarschuwingsdetails bevat de volgende secties.

| Sectie | Beschrijving |
|:---|:---|
| Essentials | Geeft de eigenschappen en andere belangrijke informatie over de waarschuwing. |
| Geschiedenis | Hier worden de acties die door de waarschuwing en wijzigingen in de waarschuwing. Dit is momenteel beperkt tot de status verandert. |
| Smartcard-groep | Informatie over de smartcard groep de waarschuwing is opgenomen in. De **aantal waarschuwingen** verwijst naar het aantal waarschuwingen dat is opgenomen in de groep van de smartcard. Dit omvat de andere waarschuwingen die zijn opgenomen in dezelfde smartcard dezelfde groep die zijn gemaakt in de afgelopen 30 dagen.  Dit is ongeacht de time-filter op de pagina van de lijst met waarschuwingen. Klik op een waarschuwing om de details weer te geven. |
| Meer informatie | Geeft meer contextuele informatie voor de waarschuwing die doorgaans specifiek naar het type van de bron waarmee de waarschuwing is gemaakt. |


### <a name="smart-group-detail-page"></a>De detailpagina smartcard-groep
De detailpagina voor Smart groep wordt weergegeven wanneer u een smart groep op. Het biedt details van de smartcard groep, met inbegrip van de redenering gebruikt voor het maken van de groep, en kunt u de status te wijzigen.
 
![De details van smartcard](media/monitoring-overview-unified-alerts/smart-group-detail.png)


De detailpagina voor Smart groep bevat de volgende secties.

| Sectie | Beschrijving |
|:---|:---|
| Waarschuwingen | Geeft een lijst van de afzonderlijke waarschuwingen die zijn opgenomen in de groep van de smartcard. Klik op een waarschuwing om de pagina Details van de waarschuwing te openen. |
| Geschiedenis | Geeft een lijst van elke actie op die door de smartcard groep en alle wijzigingen aan. Dit is momenteel beperkt tot de statuswijzigingen in de en waarschuwingen lidmaatschap. |

## <a name="next-steps"></a>Volgende stappen
- [Informatie over het gebruik van de nieuwe ervaring voor waarschuwingen maken, weergeven en beheren van waarschuwingen](monitor-alerts-unified-usage.md)
- [Meer informatie over logboek waarschuwingen in de gebruikerservaring van waarschuwingen](monitor-alerts-unified-log.md)
- [Meer informatie over metrische waarschuwingen in de gebruikerservaring van waarschuwingen](monitoring-near-real-time-metric-alerts.md)
- [Meer informatie over activiteit logboek waarschuwingen in de gebruikerservaring van waarschuwingen](monitoring-activity-log-alerts-new-experience.md)
