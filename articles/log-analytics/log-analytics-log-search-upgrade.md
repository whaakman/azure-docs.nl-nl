---
title: Azure-logboekanalyse bijwerken naar nieuwe logboek zoekopdracht | Microsoft Docs
description: De nieuwe Log Analytics query language bijna hier is en u kunt deelnemen aan de openbare preview.  In dit artikel beschrijft de voordelen van de nieuwe taal en het converteren van de werkruimte.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 36a08cab2b1d409d2de382a07cfd7259a1c94e13
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-log-analytics-upgrade-to-new-log-search"></a>Azure Log Analytics upgraden naar de nieuwe logboek zoekopdracht

De nieuwe Log Analytics query language is hier en uw werkruimte moet worden bijgewerkt om te profiteren van deze.  U kunt zelf het bijwerken van uw werkruimte of wacht totdat deze automatisch worden bijgewerkt tijdens de implementatie-periode die begint eind oktober en doorloopt het einde van het jaar.  In dit artikel beschrijft de voordelen van de nieuwe taal en het converteren van de werkruimte.  

## <a name="why-the-new-language"></a>Waarom de nieuwe taal?
We begrijpen dat er pijn in een overgang is en wij zijn niet de querytaal voor het leuk ervan te wijzigen.  Er zijn diverse redenen die deze wijziging aanzienlijke waarde voor onze klanten logboekanalyse biedt.

- **Eenvoudige maar krachtige.** De nieuwe taal is gemakkelijker te begrijpen en vergelijkbaar met SQL met constructies meer zoals natuurlijke taal dan de oudere taal.
- **Volledige piping taal.**  De nieuwe taal heeft uitgebreidere piping mogelijkheden dan de oudere taal.  Vrijwel elke uitvoer te sluizen naar een andere opdracht voor het maken van complexe query's dan voorheen mogelijk zijn.
- **Extracties zoektijd veld.**  De nieuwe taal ondersteunt meer geavanceerde runtime berekende velden dan de oudere taal.  U kunt complexe berekeningen gebruiken voor uitgebreide velden en vervolgens gebruikt u de berekende velden voor aanvullende opdrachten, inclusief samenvoegingen en aggregaties.
- **Geavanceerde joins.**  De nieuwe taal biedt meer geavanceerde joins dan de oudere taal, inclusief de mogelijkheid voor het lid van de tabellen op meerdere velden, inner en outer joins te gebruiken en zich op uitgebreide velden.
- **Functies voor datum/tijd.**  De nieuwe taal heeft dan de oudere taal geavanceerdere functies van de datum/tijd.
- **Smart Analytics.**  De nieuwe taal beschikt over geavanceerde algoritmen om te evalueren patronen in de gegevenssets en vergelijkt u verschillende sets van gegevens.
- **Geavanceerde Analytics-portal.**  De portal geavanceerde analyses biedt analysefuncties niet beschikbaar in de portal Log Analytics, met inbegrip van meerdere regels bewerken van query's, aanvullende visualisaties en geavanceerde diagnostische gegevens.
- **Consistentie met andere toepassingen.**  De nieuwe taal en de Portal voor geavanceerde analyses worden al gebruikt voor analyses in Application Insights.  Implementeert voor logboekanalyse consistentie alle Azure-services.
- **Integratie met Power BI.** Query's in de nieuwe taal kunnen worden geëxporteerd naar Power BI Desktop, zodat u de uitgebreide gegevenstransformatie-mogelijkheden kunt gebruiken.
- **Nog veel meer.** Raadpleeg de [Azure Log Analytics Query Language](https://docs.loganalytics.io) site voor meer informatie en zelfstudies over de nieuwe taal.


## <a name="when-can-i-upgrade"></a>Wanneer kan ik upgraden?
De upgrade wordt teruggedraaid uit in alle Azure-regio's zodat het mogelijk beschikbaar in bepaalde gebieden vóór andere.  Weet u wanneer uw werkruimte beschikbaar moet worden bijgewerkt wanneer er een banner aan de bovenkant van uw werkruimte uitnodiging om bij te werken.

![Upgrade 1](media/log-analytics-log-search-upgrade/upgrade-01a.png)

Als uw werkruimte automatisch bijgewerkt wordt, ziet u een banner waarmee wordt aangegeven dat is bijgewerkt met een samenvatting te identificeren of problemen zijn opgetreden.

 ![Automatische upgrade](media/log-analytics-log-search-upgrade/auto-upgrade.png)


## <a name="what-happens-after-the-upgrade"></a>Wat gebeurt er na de upgrade?
De volgende wijzigingen aangebracht in uw werkruimte wanneer deze wordt omgezet:

- Alle opgeslagen zoekopdrachten, regels voor waarschuwingen en weergaven die u hebt gemaakt met de ontwerpfunctie voor weergaven worden automatisch geconverteerd naar de nieuwe taal.  Zoekopdrachten die zijn opgenomen in oplossingen niet automatisch worden geconverteerd, maar ze zijn in plaats daarvan geconverteerd onderweg wanneer u ze opent.  
- [Mijn dashboard](log-analytics-dashboards.md) wordt afgeschaft voor [ontwerper](log-analytics-view-designer.md) en [Azure Dashboards](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards.md).  Tegels die u hebt toegevoegd aan mijn Dashboard nog steeds beschikbaar zijn, maar ze zijn alleen-lezen.
- [Power BI-integratie](log-analytics-powerbi.md) is vervangen door een nieuw proces.  Eventuele bestaande Power BI-schema's die u hebt gemaakt wordt uitgeschakeld en moet u deze vervangen door het nieuwe proces.
- Reacties van [waarschuwing acties](log-analytics-alerts-actions.md) met webhooks en runbooks hebben een nieuwe indeling en mogelijk moet u uw waarschuwingsregels worden dienovereenkomstig bijgewerkt.
- Bekijk de [Veelgestelde vragen over het logboek zoeken](log-analytics-log-search-faq.md) voor veelgestelde vragen over de upgrade.

## <a name="how-do-i-know-if-there-were-any-issues-from-the-upgrade"></a>Hoe weet ik of er problemen van de upgrade zijn?
Nadat de upgrade is voltooid, zal er een **Upgrade samenvatting** sectie in de instellingen van de werkruimte.  Controleer in deze sectie voor informatie over de upgrade en om te controleren of er problemen die zijn opgetreden.

 ![Upgradeoverzicht](media/log-analytics-log-search-upgrade/upgrade-summary.png)

## <a name="how-do-i-manually-perform-the-upgrade"></a>Hoe voer ik de upgrade handmatig uit?
Wanneer u de banner aan de bovenkant van de portal ziet, kunt u uw werkruimte bijwerken.  

1.  Het upgradeproces start door te klikken op de banner waarin staat dat **meer informatie en upgrade**.

    ![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>

2.  Lees de aanvullende informatie over de upgrade op de pagina met informatie over de upgrade.

    ![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>

3.  Klik op **nu bijwerken** bij te werken.

    ![Upgrade 4](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>Een melding in de rechterbovenhoek weergegeven, de status.
    
    ![Upgrade 5](media/log-analytics-log-search-upgrade/upgrade-05.png)

4.  Dat is alles.  Bespreek naar de pagina logboek zoeken en bekijk uw bijgewerkte werkruimte.

    ![Upgrade 6](media/log-analytics-log-search-upgrade/upgrade-06.png)

Als er een probleem dat ervoor zorgt de upgrade dat mislukken optreden, gaat u naar de [discussieforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) en uw vraag plaatsen of [Maak een ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md) vanuit de Azure-portal.

## <a name="how-do-i-learn-the-new-language"></a>Hoe ik meer informatie over de nieuwe taal?
Omdat deze wordt gebruikt door meerdere services gemaakte we een [externe site voor het hosten van de documentatie](https://docs.loganalytics.io/) voor de nieuwe taal.  Dit omvat zelfstudies, voorbeelden en volledige informatie over te komen aan de slag. U kunt een zelfstudie over de nieuwe taal op doorlopen [aan de slag met query's](https://go.microsoft.com/fwlink/?linkid=856078) en toegang tot de Naslaggids op [querytaal van logboekanalyse](https://go.microsoft.com/fwlink/?linkid=856079).  

Als u wilt de nieuwe taal in een demo-omgeving met inbegrip van een aantal voorbeeldgegevens uitproberen, hebt u een overzicht van de [playground omgeving](https://portal.loganalytics.io/demo#/discover/home).

Als u al bekend met de verouderde Log Analytics query language echter bent, kunt u de converter taal die wordt toegevoegd aan uw werkruimte als onderdeel van de upgrade.  Typ in de verouderde query en klik vervolgens op **converteren** de vertaalde versie te zien.  U kunt vervolgens klikt u op de zoekknop dat de zoekopdracht of het exemplaar wordt uitgevoerd en de geconverteerde query voor het gebruik van ergens anders zoals een waarschuwingsregel te plakken.  U kunt ook kijken hebben onze [referentieoverzicht](log-analytics-log-search-transition.md) die rechtstreeks algemene query's van de verouderde taal worden vergeleken.

![Taal converter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Volgende stappen
- Bekijk een [zelfstudie over de nieuwe taal](https://go.microsoft.com/fwlink/?linkid=856078).
- Doorlopen een [zelfstudie over het gebruik van de portal zoeken logboek](log-analytics-log-search-log-search-portal.md) met de nieuwe querytaal.
- Maak kennis met de nieuwe [Advanced Analytics-portal](https://go.microsoft.com/fwlink/?linkid=856587).
