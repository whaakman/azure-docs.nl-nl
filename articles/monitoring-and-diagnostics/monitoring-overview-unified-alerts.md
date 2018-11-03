---
title: Geïntegreerde waarschuwingen in Azure Monitor
description: Beschrijving van geïntegreerde waarschuwingen in Azure waarmee u kunt voor het beheren van waarschuwingen en regels voor waarschuwingen voor Azure-services.
author: manishsm-msft
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: mamit
ms.component: alerts
ms.openlocfilehash: 30b2d60868702c6113612668b8e4cf9975aa2c40
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50962194"
---
# <a name="unified-alerts-in-azure-monitor"></a>Geïntegreerde waarschuwingen in Azure Monitor

## <a name="overview"></a>Overzicht

> [!NOTE]
>  Een nieuwe waarschuwing uniforme waarmee u voor het beheren van waarschuwingen bij meerdere abonnementen en introduceert waarschuwing Staten en slimme groepen is momenteel beschikbaar in openbare preview. Zie de laatste sectie van dit artikel voor een beschrijving van deze verbeterde ervaring en het proces voor het inschakelen van deze.


Dit artikel beschrijft de waarschuwing uniforme in Azure Monitor. De [waarschuwing ervaring](monitoring-overview-alerts.md) is beschikbaar via de **waarschuwingen (klassiek)** optie in het menu Azure Monitor. 

## <a name="features-of-the-unified-alert-experience"></a>Functies van de waarschuwing uniforme

De geïntegreerde ervaring biedt de volgende voordelen via de klassieke ervaring:

-   **Betere notification system**: [actiegroepen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) zijn benoemde groepen van meldingen en acties die in meerdere waarschuwingen kunnen worden hergebruikt. 
- **Geïntegreerde ervaring**: waarschuwingen en waarschuwingsregels voor metrische gegevens, logboeken en activiteitenlogboeken in Azure Monitor, Log Analytics en Application Insights kunnen worden beheerd op één plek. 
- **Weergave geactiveerd Log Analytics-waarschuwingen in Azure portal**: Log Analytics-waarschuwingen kunnen nu worden weergegeven met waarschuwingen van andere bronnen in Azure portal. Eerder waren waarschuwingen uit andere bronnen in een afzonderlijke portal.
- **Scheiding van de geactiveerde waarschuwingen en waarschuwingsregels**: regels voor waarschuwingen nu worden onderscheiden van waarschuwingen. Een waarschuwingsregel is de definitie van een voorwaarde die een waarschuwing wordt geactiveerd. Een waarschuwing is een exemplaar van een waarschuwingsregel starten.
- **Betere werkstroom**: de Uniform waarschuwing ervaring leidt u door het proces van het configureren van een waarschuwingsregel.
 
Metrische waarschuwingen hebben de volgende verbeteringen over klassieke metrische waarschuwingen:

-   **Verbeterde latentie**: metrische waarschuwingen zo vaak als één keer per minuut kunnen worden uitgevoerd. Klassieke metrische waarschuwingen wordt altijd uitgevoerd met een frequentie van elke 5 minuten. Waarschuwingen hebben nog steeds een vertraging die langer dan een minuut vanwege de tijd die nodig zijn om op te nemen van de logboeken. 
-   **Ondersteuning voor multidimensionale metrische gegevens**: U kunt waarschuwing bij eendimensionale metrische gegevens, wat betekent dat u een specifiek exemplaar van de metrische gegevens kunt controleren.
-   **Meer controle over metrische voorwaarden**: U kunt uitgebreidere waarschuwingsregels die ondersteuning bieden voor bewaking van de maximale, minimale, gemiddelde en totale waarden van metrische gegevens definiëren.
-   **Bewaking van meerdere metrische gegevens gecombineerd**: U kunt maximaal twee metrische gegevens met een enkele regel controleren. Een waarschuwing wordt geactiveerd als beide metrische gegevens in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
-   **Metrische gegevens uit logboeken** (beperkte openbare preview-versie): sommige log-gegevens die u in Log Analytics nu kunnen worden geëxtraheerd en omgezet in Azure Monitor metrische gegevens en vervolgens een melding op net als andere metrische gegevens. 


## <a name="alert-rules"></a>Waarschuwingsregels
De ervaring voor geïntegreerde waarschuwingen maakt gebruik van de volgende concepten voor het scheiden van regels voor waarschuwingen van waarschuwingen tijdens de werking van de ontwerphandleiding voor verschillende typen waarschuwingen.

| Item | Definitie |
|:---|:---|
| Waarschuwingsregel | De definitie van de voorwaarde om een waarschuwing te maken. Een waarschuwingsregel bestaat uit een _doelresource_, _signaal_, _criteria_, en _logische_. Een waarschuwingsregel is alleen beschikbaar als het zich in een _ingeschakeld_ staat.
| Doelbron | Definieert de specifieke resources en signalen die beschikbaar voor waarschuwingen zijn. Een doel kan een Azure-resource zijn.<br><br>Voorbeelden: virtuele machine, opslagaccount, virtuele-machineschaalset, Log Analytics-werkruimte, Application Insights-resource |
| Signaal | De bron van gegevens die door de doelresource. Ondersteunde signaaltypen zijn *Metric*, *activiteitenlogboek*, *Application Insights*, en *Log*. |
| Criteria | Combinatie van _signaal_ en _logische_ toegepast op een doelbron.<br><br>Voorbeelden: Percentage CPU > 70%, serverreactietijd > 4 ms, resultaattelling van een logbestand > 100 query, enzovoort |
| Logica | Gebruiker gedefinieerde logica om te controleren of het signaal binnen verwacht de bereikwaarden. |
| Bewerking | Actie om uit te voeren wanneer de waarschuwing wordt geactiveerd. Meerdere acties kunnen optreden wanneer een waarschuwing wordt geactiveerd. Deze waarschuwingen ondersteunen Actiegroepen.<br><br>Voorbeelden: e-mailen om u te e-mailadres, aanroepen van een webhook-URL |
| Monitorconditie | Geeft aan of de voorwaarde die een metrische waarschuwing is opgelost. Waarschuwingsregels voor metrische gegevens voorbeeld van een bepaalde meetwaarde met regelmatige intervallen. Als de criteria in de waarschuwingsregel wordt voldaan, wordt een nieuwe waarschuwing gemaakt met de voorwaarde van 'gestart'.  Wanneer de metrische gegevens, verzameld als nog steeds aan de criteria wordt voldaan, gebeurt er niets.  Als niet aan de criteria wordt voldaan, is klikt u vervolgens de voorwaarde van de waarschuwing gewijzigd in 'omgezet'. De volgende keer dat de criteria wordt voldaan, wordt een nieuwe waarschuwing gemaakt met de voorwaarde 'gestart'. |


## <a name="alert-pages"></a>Waarschuwing-pagina 's
Geïntegreerde waarschuwingen bevatten één plek om uw Azure waarschuwingen weergeven en beheren. De volgende secties worden de functies van elke afzonderlijke pagina van de geïntegreerde ervaring.

### <a name="alerts-overview-page"></a>Overzichtspagina van waarschuwingen
De **waarschuwingen** overzichtspagina bevat een samengevoegde overzicht van alle geactiveerde waarschuwingen en het totale aantal ingeschakelde waarschuwingsregels. Wijzigen van de abonnementen of de filterparameters werkt de statistische functies en de waarschuwingen geactiveerd lijst.

 ![overzicht van waarschuwingen](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Regels voor waarschuwingen beheren
**Regels** bestaat uit één pagina voor het beheren van alle regels voor waarschuwingen voor uw Azure-abonnementen. Het geeft een lijst van alle regels voor waarschuwingen en kan worden gesorteerd op basis van de doelresources, resourcegroepen, regelnaam of status. Regels voor waarschuwingen kunnen ook worden bewerkt, ingeschakeld of uitgeschakeld op deze pagina.

 ![waarschuwingen-regels](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Een waarschuwingsregel maken
Waarschuwingen kunnen worden geschreven in een consistente manier, ongeacht de bewakingsservice of type signaal. Alle waarschuwingen geactiveerd en gerelateerde details zijn beschikbaar in één pagina.
 
U kunt een nieuwe waarschuwingsregel maken met de volgende drie stappen:
1. Kies de _doel_ voor de waarschuwing.
1. Selecteer de _signaal_ uit de beschikbare signalen voor het doel.
1. Geef de _logische_ moet worden toegepast op gegevens uit het signaal.
 
Deze vereenvoudigde ontwerpproces moet langer u weten wat de bron controleren of vergelijken met seintjes die worden ondersteund voor het selecteren van een Azure-resource. De lijst met beschikbare signalen is automatisch gefilterd op basis van de doelresource die u hebt geselecteerd en die u begeleidt bij het definiëren van de logica van de waarschuwingsregel.

U kunt meer informatie over het maken van regels voor waarschuwingen in [maken, weergeven en beheren van waarschuwingen via Azure Monitor](alert-metric.md).

Waarschuwingen zijn beschikbaar voor de verschillende Azure-services controleren. Zie voor informatie over hoe en wanneer het gebruik van elk van deze services [bewaking van Azure-toepassingen en bronnen](../azure-monitor/overview.md). De volgende tabel bevat een overzicht van de typen regels voor waarschuwingen die beschikbaar in Azure zijn. Het bevat ook wat er wordt momenteel ondersteund door de waarschuwing uniforme.

| **Bron van de monitor** | **Signaaltype**  | **Beschrijving** | 
|-------------|----------------|-------------|
| Azure Monitor | Gegevens  | Ook wel genoemd [bijna-realtime metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md), ze ondersteunen net zo vaak als elke minuut metrische voorwaarden evalueren en dat er meerdere metrische gegevens en multi-dimensionale metrische regels. Een lijst met ondersteunde resourcetypen is beschikbaar in [nieuwere metrische waarschuwingen voor Azure-services in Azure portal](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>[Klassieke metrische waarschuwingen](monitoring-overview-alerts.md) worden niet ondersteund in de nieuwe ervaring voor waarschuwingen. U vindt deze onder waarschuwingen (klassiek) in Azure portal. De klassieke waarschuwingen ondersteunen sommige metrische typen die nog niet zijn verplaatst naar de nieuwere waarschuwingen. Zie voor een volledige lijst [ondersteunde metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Logboeken  | Meldingen ontvangen of geautomatiseerde acties worden uitgevoerd wanneer een zoekquery logboek voldoet aan bepaalde criteria voldoen. Waarschuwingen in Log Analytics zijn [wordt gekopieerd naar de nieuwe ervaring](monitoring-alerts-extend.md). Een [preview van *Log Analytics-Logboeken als metrische gegevens* ](monitoring-alerts-extend-tool.md) beschikbaar is. De Preview-versie kunt u bepaalde typen logboeken nemen en te converteren naar metrische gegevens, waar u kunt vervolgens een melding op deze met behulp van de nieuwe ervaring voor waarschuwingen. De Preview-versie is handig als u hebt niet-Azure-logboeken die u wilt ophalen, samen met systeemeigen Azure Monitor-metrische gegevens. |
| Activiteitenlogboeken | Activiteitenlogboek | Bevat de records van alle maken, bijwerken en verwijderen van acties die zijn gemaakt door het geselecteerde doel. |
| Servicestatus | Activiteitenlogboek  | Niet ondersteund in geïntegreerde waarschuwingen. Zie [waarschuwingen voor activiteitenlogboek maken voor servicemeldingen](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Logboeken  | Logboeken met de details van de prestaties van uw toepassing bevat. U kunt de voorwaarden voor welke acties moeten worden uitgevoerd op basis van toepassingsgegevens te definiëren met behulp van de analytics-query. |
| Application Insights | Gegevens | Niet ondersteund in geïntegreerde waarschuwingen. Zie [metrische waarschuwingen](../application-insights/app-insights-alerts.md). |
| Application Insights | Webtests voor beschikbaarheid | Niet ondersteund in geïntegreerde waarschuwingen.  Zie [webtestwaarschuwingen](../application-insights/app-insights-monitor-web-app-availability.md). Beschikbaar voor elke website die geïnstrumenteerd om gegevens te verzenden naar Application Insights. Een melding ontvangen wanneer de beschikbaarheid of reactiesnelheid van een website lager dan de verwachtingen is. |

## <a name="enhanced-unified-alerts-public-preview"></a>Verbeterde geïntegreerde waarschuwingen (preview-versie)

Een verbeterde waarschuwingen van geïntegreerde ervaring is uitgebracht in openbare preview-versie van Azure Monitor op 1 juni 2018. Deze ervaring is gebaseerd op de voordelen van [waarschuwingen van geïntegreerde](#overview), die zijn uitgebracht in maart 2018 en hier vindt u de mogelijkheid om te beheren en aggregeren van afzonderlijke waarschuwingen en het wijzigen van de status van de waarschuwing. Deze sectie beschrijft de nieuwe functies en hoe u de nieuwe waarschuwing pagina's in de Azure-portal navigeert.

### <a name="enhanced-unified-alerts"></a>Verbeterde geïntegreerde waarschuwingen

Deze nieuwe ervaring biedt de volgende functies die niet beschikbaar zijn in de klassieke uniforme:

- **Waarschuwingen weergeven voor abonnementen**: U kunt nu bekijken en beheren van afzonderlijke exemplaren van waarschuwingen voor meerdere abonnementen in één weergave.
- **De status van waarschuwingen beheren**: waarschuwingen hebben nu een status die aangeeft of ze hebben is erkend als gesloten.
- **Waarschuwingen met slimme groepen organiseren**: slimme groepen automatisch groeperen, zodat u ze als een set in plaats van afzonderlijk beheren kunt worden waarschuwingen over.

### <a name="enable-enhanced-unified-alerts"></a>Verbeterde geïntegreerde waarschuwingen inschakelen
De nieuwe ervaring voor geïntegreerde waarschuwingen inschakelen door het selecteren van de banner boven aan de pagina met waarschuwingen. Dit proces maakt een waarschuwing store die de afgelopen 30 dagen van de geactiveerde waarschuwingen over de ondersteunde services bevat. Nadat de nieuwe ervaring is ingeschakeld, kunt u heen en weer schakelen tussen de nieuwe en het oude ervaring door te selecteren van deze banner.

> [!NOTE]
>  Het duurt enkele minuten duren voordat de nieuwe ervaring in eerste instantie zijn ingeschakeld.

![Banner](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Alle abonnementen die u toegang tot hebt worden geregistreerd wanneer u de nieuwe ervaring inschakelt. Hoewel het hele abonnement is ingeschakeld, kunnen alleen gebruikers die de nieuwe ervaring selecteert het bekijken. Andere gebruikers met toegang tot het abonnement moeten de ervaring afzonderlijk inschakelen.

Inschakelen van de nieuwe ervaring voor waarschuwingen, heeft dit geen invloed op de configuratie van actiegroepen of meldingen in uw regels voor waarschuwingen. Alleen verandert de manier waarop u weergeven en beheren van gestarte exemplaren van de waarschuwingen in Azure portal.

### <a name="smart-groups"></a>Slimme groepen
Slimme groepen reduceren ruis doordat u gerelateerde waarschuwingen als één eenheid in plaats van afzonderlijke meldingen beheren. U kunt de details van slimme groepen weergeven en de status op dezelfde manier worden ingesteld op hoe u met waarschuwingen kunt. Elke waarschuwing is lid van slechts één slimme groep.

Slimme groepen worden automatisch gemaakt met behulp van machine learning voor het combineren van gerelateerde waarschuwingen die staan voor één probleem. Wanneer een waarschuwing wordt gemaakt, wordt deze door de algoritme toegevoegd aan een nieuwe slimme groep of een bestaande slimme groep op basis van gegevens, zoals historische patronen, vergelijkbare eigenschappen en vergelijkbare structuur. 

Het algoritme beschouwt op dit moment alleen waarschuwingen van de dezelfde monitor-service binnen een abonnement. Slimme groepen kunnen maximaal 99% van de waarschuwing ruis via deze samenvoeging te verminderen. U kunt de reden dat er waarschuwingen zijn opgenomen in een groep in de detailpagina van het smart-groep weergeven.

De naam van een slimme groep is de naam van de eerste waarschuwing. U kunt maken of wijzig de naam van een slimme groep.


### <a name="alert-states"></a>Statussen
Verbeterde waarschuwingen van geïntegreerde introduceert het concept van waarschuwingsstatus. U kunt de status van een waarschuwing om op te geven waar deze zich in het proces van het probleem zou moeten instellen. Wanneer een waarschuwing wordt gemaakt, heeft deze een status *nieuw*. U kunt de status wijzigen wanneer u erkent dat een waarschuwing en wanneer u deze sluiten. Alle wijzigingen worden opgeslagen in de geschiedenis van de waarschuwing.

De volgende statussen worden ondersteund.

| Status | Beschrijving |
|:---|:---|
| Nieuw | Het probleem alleen is gedetecteerd en nog niet zijn beoordeeld. |
| Bevestigd | Een beheerder heeft gecontroleerd van de waarschuwing en werken op deze gestart. |
| Gesloten | Het probleem is opgelost. Nadat een waarschuwing is gesloten, kunt u deze opnieuw openen door deze te wijzigen naar een andere status. |

De status van een waarschuwing is anders dan de bewakingsvoorwaarde. Waarschuwingsregels voor metrische gegevens een voorwaarde van een waarschuwing kunnen instellen _opgelost_ wanneer het probleem niet meer wordt voldaan. Waarschuwing status wordt ingesteld door de gebruiker en onafhankelijk van de bewakingsvoorwaarde is. Hoewel het systeem de monitor-voorwaarde 'omgezet' instellen kan, toestand van de melding niet worden gewijzigd totdat de gebruiker deze wijzigt.

#### <a name="change-the-state-of-an-alert-or-smart-group"></a>Wijzig de status van een waarschuwing of smart-groep
U kunt de status van een afzonderlijke waarschuwing wijzigen of meerdere waarschuwingen samen door in te stellen van de status van een slimme groep beheren.

De status van een waarschuwing wijzigen door te selecteren **Waarschuwingsstatus wijzigen** in de detailweergave van de waarschuwing. Of de status voor een slimme groep wijzigen door te selecteren **wijzigen slimme groepsstatus** in de detailweergave. Wijzig de status van meerdere items in één keer door ze eerst te selecteren in een lijst weergeven en vervolgens de optie **status wijzigen** aan de bovenkant van de pagina. 

In beide gevallen wordt een nieuwe status te selecteren in de vervolgkeuzelijst. Geef een optionele opmerking. Als u één item wijzigt, hebt u ook een optie voor de dezelfde wijzigingen toepassen op alle waarschuwingen in de slimme groep.

![Status wijzigen](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>Pagina met waarschuwingen
De standaardpagina van de waarschuwingen bevat een samenvatting van waarschuwingen die zijn gemaakt binnen een bepaalde periode. Het totaal aantal waarschuwingen voor alle incidenten met ernst met kolommen die de id van het totale aantal waarschuwingen in elke status voor alle incidenten met ernst wordt weergegeven. Selecteer een van de ernstcategorieën openen de [alle waarschuwingen](#all-alerts-page) pagina gefilterd op basis van die ernst.

![Pagina met waarschuwingen](media/monitoring-overview-unified-alerts/alerts-page.png)

U kunt deze weergave filteren op waarden selecteren in het vervolgkeuzemenu's aan de bovenkant van de pagina.

| Kolom | Beschrijving |
|:---|:---|
| Abonnement | Selecteer maximaal vijf Azure-abonnementen. Alleen waarschuwingen in de geselecteerde abonnementen zijn opgenomen in de weergave. |
| Resourcegroep | Selecteer één resourcegroep bestaan. Alleen waarschuwingen met doelen in de geselecteerde resourcegroep zijn opgenomen in de weergave. |
| Tijdsbereik | Alleen waarschuwingen geactiveerd in het geselecteerde tijdvenster zijn opgenomen in de weergave. Ondersteunde waarden zijn het afgelopen uur, de afgelopen 24 uur, de afgelopen 7 dagen en de afgelopen 30 dagen. |

Selecteer de volgende waarden aan de bovenkant van de pagina met waarschuwingen om een andere pagina te openen.

| Waarde | Beschrijving |
|:---|:---|
| Totaal aantal waarschuwingen | Het totale aantal waarschuwingen dat voldoet aan de geselecteerde criteria. Selecteer deze waarde in de weergave alle waarschuwingen openen met geen filter. |
| Slimme groepen | Het totale aantal slimme groepen die zijn gemaakt op basis van de waarschuwingen die overeenkomen met de geselecteerde criteria. Selecteer deze waarde aan de groepslijst met slimme openen in de weergave alle waarschuwingen.
| Totaal aantal waarschuwingsregels | Het totale aantal regels voor waarschuwingen in het geselecteerde abonnement en resourcegroep. Selecteer deze waarde om de weergave van de regels gefilterd op het geselecteerde abonnement en resourcegroep te openen.


### <a name="all-alerts-page"></a>Pagina met alle waarschuwingen 
Met behulp van de pagina met alle waarschuwingen, vindt u een lijst van waarschuwingen die zijn gemaakt binnen de geselecteerde periode. U kunt een lijst van de afzonderlijke waarschuwingen of een lijst met de slimme groepen zijn met de waarschuwingen weergeven. Selecteer de banner boven aan de pagina om te schakelen tussen weergaven.

![Pagina met alle waarschuwingen](media/monitoring-overview-unified-alerts/all-alerts-page.png)

U kunt de weergave filteren op de volgende waarden selecteren in het vervolgkeuzemenu's aan de bovenkant van de pagina.

| Kolom | Beschrijving |
|:---|:---|
| Abonnement | Selecteer maximaal vijf Azure-abonnementen. Alleen waarschuwingen in de geselecteerde abonnementen zijn opgenomen in de weergave. |
| Resourcegroep | Selecteer één resourcegroep bestaan. Alleen waarschuwingen met doelen in de geselecteerde resourcegroep zijn opgenomen in de weergave. |
| Resourcetype | Selecteer een of meer resourcetypen. Alleen waarschuwingen met doelen van het geselecteerde type worden opgenomen in de weergave. Deze kolom is alleen beschikbaar nadat u een resourcegroep is opgegeven. |
| Resource | Selecteer een resource. Alleen waarschuwingen met die bron als doel zijn opgenomen in de weergave. Deze kolom is alleen beschikbaar nadat u een resourcetype is opgegeven. |
| Severity | Selecteer de ernst van een waarschuwing of selecteer *alle* om op te nemen van waarschuwingen van alle ernstcategorieën. |
| Monitorconditie | Selecteer een controleconditie *alle* om op te nemen van waarschuwingen van voorwaarden. |
| Waarschuwingsstatus | Selecteer een waarschuwingsstatus *alle* om op te nemen van waarschuwingen van de volgende statussen. |
| Service bewaken | Selecteer een service of selecteer *alle* om op te nemen alle services. Alleen de waarschuwingen die zijn gemaakt door regels die gebruikmaken van service als een doel zijn opgenomen. |
| Tijdsbereik | Alleen waarschuwingen geactiveerd in het geselecteerde tijdvenster zijn opgenomen in de weergave. Ondersteunde waarden zijn het afgelopen uur, de afgelopen 24 uur, de afgelopen 7 dagen en de afgelopen 30 dagen. |

Selecteer **kolommen** aan de bovenkant van de pagina om te selecteren welke kolommen om weer te geven. 

### <a name="alert-detail-page"></a>De detailpagina van waarschuwing
De detailpagina van de waarschuwing wordt weergegeven wanneer u een waarschuwing selecteert. Het biedt details van de waarschuwing en kunt u de status te veranderen.

![Waarschuwingsdetails](media/monitoring-overview-unified-alerts/alert-detail.png)

De detailpagina van de waarschuwing bevat de volgende secties.

| Sectie | Beschrijving |
|:---|:---|
| Essentials | Geeft de eigenschappen en andere belangrijke informatie over de waarschuwing. |
| Geschiedenis | Geeft een lijst van elke actie op die door de waarschuwing en eventuele wijzigingen in de waarschuwing. Dit is momenteel beperkt tot de status verandert. |
| Slimme groep | Informatie over de slimme groep de waarschuwing is opgenomen in. De *aantal waarschuwingen* verwijst naar het aantal waarschuwingen die zijn opgenomen in de slimme groep. Dit omvat de andere waarschuwingen in dezelfde groep slimme die zijn gemaakt in de afgelopen 30 dagen.  Dit is, ongeacht het tijdfilter in de pagina van de lijst met waarschuwingen. Selecteer een waarschuwing om de details weer te geven. |
| Meer informatie | Geeft meer contextuele informatie voor de waarschuwing, die doorgaans specifiek is voor het type bron waarmee de waarschuwing is gemaakt. |


### <a name="smart-group-detail-page"></a>Details van slimme groepspagina
De detailpagina van het Smart-groep wordt weergegeven wanneer u een slimme groep selecteert. Het bevat informatie over de slimme groep, met inbegrip van de redenering die is gebruikt voor het maken van de groep, en kunt u de status te veranderen.
 
![Details van slimme](media/monitoring-overview-unified-alerts/smart-group-detail.png)


De detailpagina van het smart groep bevat de volgende secties.

| Sectie | Beschrijving |
|:---|:---|
| Waarschuwingen | Geeft een lijst van de afzonderlijke waarschuwingen die zijn opgenomen in de slimme groep. Selecteer een waarschuwing om de Waarschuwingsdetails-pagina te openen. |
| Geschiedenis | Geeft een lijst van elke actie op die door de slimme groep en alle wijzigingen die zijn aangebracht. Dit is momenteel beperkt tot statuswijzigingen in de en waarschuwingen lidmaatschap. |

## <a name="next-steps"></a>Volgende stappen
- [Informatie over het gebruik van de nieuwe ervaring voor waarschuwingen te maken, weergeven en beheren van waarschuwingen](alert-metric.md)
- [Meer informatie over waarschuwingen in de ervaring voor waarschuwingen](monitor-alerts-unified-log.md)
- [Meer informatie over metrische waarschuwingen in de ervaring voor waarschuwingen](monitoring-near-real-time-metric-alerts.md)
- [Meer informatie over waarschuwingen voor activiteitenlogboeken in de ervaring voor waarschuwingen](alert-activity-log.md)
