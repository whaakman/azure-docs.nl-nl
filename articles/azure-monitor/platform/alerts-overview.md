---
title: Overzicht van waarschuwingen en meldingen bewaking in Azure
description: Overzicht van waarschuwingen in Azure. Waarschuwingen, klassieke waarschuwingen, de interface van de waarschuwingen.
author: rboucher
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 18a63497cb0df2ade495dfb721aaa881aa4e6ff7
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464108"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Overzicht van waarschuwingen in Microsoft Azure 

Dit artikel wordt beschreven welke waarschuwingen zijn, hun voordelen, en hoe u aan de slag met behulp van deze.  


## <a name="what-are-alerts-in-microsoft-azure"></a>Wat zijn meldingen in Microsoft Azure?
Waarschuwingen waarschuwen u proactief te wanneer belangrijke voorwaarden vindt u in uw bewakingsgegevens. Hiermee kunt u identificeren en oplossen van problemen voordat de gebruikers van uw systeem ziet u deze. 

Dit artikel worden de waarschuwingen uniforme in Azure Monitor, nu met Log Analytics en Application Insights. De [waarschuwing ervaring](alerts-overview.md) Waarschuwingstypen worden genoemd **klassieke waarschuwingen**. U kunt deze oudere werkwijze en oudere Waarschuwingstype weergeven door te klikken op **klassieke waarschuwingen weergeven** aan de bovenkant van de pagina van de waarschuwing. 

## <a name="overview"></a>Overzicht

In het onderstaande diagram geeft de stroom van waarschuwingen. 

![De Waarschuwingsstroom](media/alerts-overview/Azure-Monitor-Alerts.svg)

Regels voor waarschuwingen worden gescheiden van waarschuwingen en de actie die moet worden uitgevoerd wanneer een waarschuwing wordt geactiveerd. 

**Waarschuwingsregel** -de waarschuwing regel bevat het doel en de criteria voor waarschuwingen. De waarschuwingsregel kan zich in een ingeschakeld of uitgeschakeld. Waarschuwingen alleen wordt geactiveerd wanneer dit is ingeschakeld. 

De belangrijkste kenmerken van een waarschuwingsregel zijn:

**Doelresource** - definieert u het bereik en signalen beschikbaar voor waarschuwingen. Een doel kan een Azure-resource zijn. Voorbeeld van doelen: een virtuele machine, een storage-account, een virtuele-machineschaalset, een Log Analytics-werkruimte of een Application Insights-resource. Voor bepaalde resources (zoals virtuele Machines), u kunt meerdere resources opgeven als het doel van de waarschuwingsregel.

**Signaal** - worden gegenereerd door de doelresource-signalen en van verschillende typen kunnen zijn. Metrische gegevens, activiteit logboek, Application Insights en Log.

**Criteria** - Criteria is een combinatie van signaal en logica toegepast op een doelbron. Voorbeelden: 
   - Percentage CPU > 70%
   - Serverreactietijd > 4 ms 
   - Resultaattelling van een logboek query > 100

**Naam melding** – een specifieke naam voor de waarschuwingsregel die door de gebruiker

**Beschrijving van waarschuwing** – een beschrijving op voor de waarschuwingsregel die door de gebruiker

**Ernst** : de ernst van de waarschuwing als de criteria die zijn opgegeven in de waarschuwingsregel wordt voldaan. Ernst kan variëren van 0 tot en met 4.

**Actie** - een specifieke actie wordt ondernomen wanneer de waarschuwing wordt geactiveerd. Zie voor meer informatie, [actiegroepen](../../azure-monitor/platform/action-groups.md).

## <a name="what-you-can-alert-on"></a>Wat u kunt waarschuwingen op

U kunt waarschuwing bij Logboeken en metrische gegevens, zoals beschreven in [bewaking gegevensbronnen](../../azure-monitor/platform/data-sources-reference.md). Deze omvatten, maar zijn niet beperkt tot:
- Metrische waarden
- Meld u zoekquery 's
- Gebeurtenissen in het activiteitenlogboek
- Status van de onderliggende Azure-platform
- Tests voor de website van beschikbaarheid

## <a name="manage-alerts"></a>Waarschuwingen beheren
U kunt de status van een waarschuwing om op te geven waar deze zich in het proces van het probleem zou moeten instellen. Als de criteria die zijn opgegeven in de waarschuwingsregel wordt voldaan, een waarschuwing wordt gemaakt of geactiveerd, heeft de status van *nieuw*. U kunt de status wijzigen wanneer u erkent dat een waarschuwing en wanneer u deze sluiten. Alle wijzigingen worden opgeslagen in de geschiedenis van de waarschuwing.

De volgende statussen worden ondersteund.

| Status | Description |
|:---|:---|
| Nieuw | Het probleem alleen is gedetecteerd en nog niet zijn beoordeeld. |
| Bevestigd | Een beheerder heeft gecontroleerd van de waarschuwing en werken op deze gestart. |
| Gesloten | Het probleem is opgelost. Nadat een waarschuwing is gesloten, kunt u deze opnieuw openen door deze te wijzigen naar een andere status. |

**Waarschuwing status** is anders en afhankelijk van de **voorwaarde controleren**. Waarschuwingsstatus is ingesteld door de gebruiker. Bewakingsvoorwaarde is ingesteld door het systeem. Wanneer een waarschuwing wordt geactiveerd, van de waarschuwing bewakingsvoorwaarde is ingesteld op *geactiveerd*. Als de onderliggende voorwaarde waarvoor de waarschuwing moet worden gestart wist, de bewakingsvoorwaarde is ingesteld op *opgelost*. Toestand van de melding is niet gewijzigd totdat de gebruiker deze wijzigt. Informatie over [het wijzigen van de status van uw waarschuwingen en slimme groepen](https://aka.ms/managing-alert-smart-group-states).

## <a name="smart-groups"></a>Slimme groepen 
Slimme groepen zijn beschikbaar als preview. 

Slimme groepen zijn aggregaties van waarschuwingen op basis van machine learning-algoritmen, die kunnen helpen waarschuwingsruis te verminderen en hulpmiddel bij probleemoplossing. [Meer informatie over slimme groepen](https://aka.ms/smart-groups) en [over het beheren van uw slimme groepen](https://aka.ms/managing-smart-groups).


## <a name="alerts-experience"></a>Ervaring voor waarschuwingen 
De standaardpagina van de waarschuwingen bevat een samenvatting van waarschuwingen die zijn gemaakt binnen een bepaalde periode. Het totaal aantal waarschuwingen voor alle incidenten met ernst met kolommen die de id van het totale aantal waarschuwingen in elke status voor alle incidenten met ernst wordt weergegeven. Selecteer een van de ernstcategorieën openen de [alle waarschuwingen](#all-alerts-page) pagina gefilterd op basis van die ernst.

Het niet weergeven of oudere bijhouden [klassieke waarschuwingen](#classic-alerts). U kunt abonnementen wijzigen of filteren van parameters voor het bijwerken van de pagina. 

![Pagina met waarschuwingen](media/alerts-overview/alerts-page.png)

U kunt deze weergave filteren op waarden selecteren in het vervolgkeuzemenu's aan de bovenkant van de pagina.

| Kolom | Description |
|:---|:---|
| Abonnement | Selecteer maximaal vijf Azure-abonnementen. Alleen waarschuwingen in de geselecteerde abonnementen zijn opgenomen in de weergave. |
| Resourcegroep | Selecteer één resourcegroep bestaan. Alleen waarschuwingen met doelen in de geselecteerde resourcegroep zijn opgenomen in de weergave. |
| Tijdsbereik | Alleen waarschuwingen geactiveerd in het geselecteerde tijdvenster zijn opgenomen in de weergave. Ondersteunde waarden zijn het afgelopen uur, de afgelopen 24 uur, de afgelopen 7 dagen en de afgelopen 30 dagen. |

Selecteer de volgende waarden aan de bovenkant van de pagina met waarschuwingen om een andere pagina te openen.

| Waarde | Description |
|:---|:---|
| Totaal aantal waarschuwingen | Het totale aantal waarschuwingen dat voldoet aan de geselecteerde criteria. Selecteer deze waarde in de weergave alle waarschuwingen openen met geen filter. |
| Slimme groepen | Het totale aantal slimme groepen die zijn gemaakt op basis van de waarschuwingen die overeenkomen met de geselecteerde criteria. Selecteer deze waarde aan de groepslijst met slimme openen in de weergave alle waarschuwingen.
| Totaal aantal waarschuwingsregels | Het totale aantal regels voor waarschuwingen in het geselecteerde abonnement en resourcegroep. Selecteer deze waarde om de weergave van de regels gefilterd op het geselecteerde abonnement en resourcegroep te openen.


## <a name="manage-alert-rules"></a>Waarschuwingsregels beheren
Klik op **Waarschuwingsregels beheren** om weer te geven de **regels** pagina. **Regels** is één plek voor het beheren van alle regels voor waarschuwingen voor uw Azure-abonnementen. Het geeft een lijst van alle regels voor waarschuwingen en kan worden gesorteerd op basis van de doelresources, resourcegroepen, regelnaam of status. Regels voor waarschuwingen kunnen ook worden bewerkt, ingeschakeld of uitgeschakeld op deze pagina.  

 ![waarschuwingen-regels](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Een waarschuwingsregel maken
Waarschuwingen kunnen worden geschreven in een consistente manier, ongeacht de bewakingsservice of type signaal. Alle waarschuwingen geactiveerd en gerelateerde details zijn beschikbaar in één pagina.
 
U kunt een nieuwe waarschuwingsregel maken met de volgende drie stappen:
1. Kies de _doel_ voor de waarschuwing.
1. Selecteer de _signaal_ uit de beschikbare signalen voor het doel.
1. Geef de _logische_ moet worden toegepast op gegevens uit het signaal.
 
Deze vereenvoudigde ontwerpproces moet langer u weten wat de bron controleren of vergelijken met seintjes die worden ondersteund voor het selecteren van een Azure-resource. De lijst met beschikbare signalen wordt automatisch gefilterd op basis van de doelresource die u selecteert. Ook op basis van die zijn gericht, worden u geholpen bij het definiëren van de logica van de waarschuwingsregel automatisch.  

U kunt meer informatie over het maken van regels voor waarschuwingen in [maken, weergeven en beheren van waarschuwingen via Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

Waarschuwingen zijn beschikbaar voor de verschillende Azure-services controleren. Zie voor informatie over hoe en wanneer het gebruik van elk van deze services [bewaking van Azure-toepassingen en bronnen](../../azure-monitor/overview.md). De volgende tabel bevat een overzicht van de typen regels voor waarschuwingen die beschikbaar in Azure zijn. Het bevat ook wat er wordt momenteel ondersteund in welke waarschuwingen ervaring.

Voorheen moest Azure Monitor, Application Insights, Log Analytics en status van de Service mogelijkheden voor afzonderlijke waarschuwingen. Overwerk, Azure, is verbeterd en combinatie van de gebruikersinterface en de verschillende methoden van waarschuwingen. Dankzij deze consolidatie is nog steeds in behandeling. Als gevolg hiervan, zijn er nog enkele waarschuwingen mogelijkheden niet nog in het nieuwe waarschuwingensysteem.  

| **Bron van de monitor** | **Signaaltype**  | **Beschrijving** | 
|-------------|----------------|-------------|
| Servicestatus | Activiteitenlogboek  | Wordt niet ondersteund. Zie [waarschuwingen voor activiteitenlogboek maken voor servicemeldingen](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).  |
| Application Insights | Webtests voor beschikbaarheid | Wordt niet ondersteund. Zie [webtestwaarschuwingen](../../azure-monitor/app/monitor-web-app-availability.md). Beschikbaar voor elke website die geïnstrumenteerd om gegevens te verzenden naar Application Insights. Een melding ontvangen wanneer de beschikbaarheid of reactiesnelheid van een website lager dan de verwachtingen is. |


## <a name="all-alerts-page"></a>Pagina met alle waarschuwingen 
Klik op het totaal aantal waarschuwingen om te zien van de pagina met alle waarschuwingen. Hier vindt u een lijst van waarschuwingen die zijn gemaakt binnen de geselecteerde periode. U kunt een lijst van de afzonderlijke waarschuwingen of een lijst met de slimme groepen zijn met de waarschuwingen weergeven. Selecteer de banner boven aan de pagina om te schakelen tussen weergaven.

![Pagina met alle waarschuwingen](media/alerts-overview/all-alerts-page.png)

U kunt de weergave filteren op de volgende waarden selecteren in het vervolgkeuzemenu's aan de bovenkant van de pagina.

| Kolom | Description |
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

## <a name="alert-detail-page"></a>De detailpagina van waarschuwing
De detailpagina van de waarschuwing wordt weergegeven wanneer u een waarschuwing selecteert. Het biedt details van de waarschuwing en kunt u de status te veranderen.

![Waarschuwingsdetails](media/alerts-overview/alert-detail2.png)

De detailpagina van de waarschuwing bevat de volgende secties.

| Sectie | Description |
|:---|:---|
| Essentials | Geeft de eigenschappen en andere belangrijke informatie over de waarschuwing. |
| Geschiedenis | Geeft een lijst van elke actie op die door de waarschuwing en eventuele wijzigingen in de waarschuwing. Momenteel beperkt tot de status verandert. |
| Slimme groep | Informatie over de slimme groep de waarschuwing is opgenomen in. De *aantal waarschuwingen* verwijst naar het aantal waarschuwingen die zijn opgenomen in de slimme groep. Bevat andere waarschuwingen in dezelfde groep slimme die zijn gemaakt in de afgelopen 30 dagen, ongeacht het tijdfilter in de pagina van de lijst met waarschuwingen. Selecteer een waarschuwing om de details weer te geven. |
| Meer informatie | Geeft meer contextuele informatie voor de waarschuwing, die doorgaans specifiek is voor het type bron waarmee de waarschuwing is gemaakt. |


## <a name="classic-alerts"></a>Klassieke waarschuwingen 

De metrische gegevens en activiteitenlogboeken logboek van Azure Monitor mogelijkheid juni 2018 waarschuwingen heet 'Waarschuwingen (klassiek)'. 

Zie voor meer informatie, [klassieke waarschuwingen](./../../azure-monitor/platform/alerts-classic.overview.md)


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over slimme groepen](https://aka.ms/smart-groups)
- [Meer informatie over actiegroepen](../../azure-monitor/platform/action-groups.md)
- [Uw waarschuwing exemplaren in Azure beheren](https://aka.ms/managing-alert-instances)
- [Slimme groepen beheren](https://aka.ms/managing-smart-groups)






