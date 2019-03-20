---
title: Betrouwbaarheidsscore in Azure Security Center | Microsoft Docs
description: " Meer informatie over het werken met de betrouwbaarheidsscore van Azure Security Center. "
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e88198f8-2e16-409d-a0b0-a62e68c2f999
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rkarlin
ms.openlocfilehash: 64bab5c1b99720eecb189834d7a11802cf919ca3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58106777"
---
# <a name="alert-confidence-score"></a>Betrouwbaarheidsscore waarschuwing 

Azure Security Center geeft u inzicht in de resources die u in Azure uitvoert en waarschuwt u wanneer er potentiële problemen gedetecteerd. Het volume van waarschuwingen kan lastig zijn voor een operations-beveiligingsteam voor een afzonderlijk adres en moet de prioriteit voor het onderzoeken van waarschuwingen. Onderzoeken van waarschuwingen kan complex en tijdrovend zijn, en als gevolg hiervan bepaalde waarschuwingen worden genegeerd.

De betrouwbaarheidsscore in Security Center kan helpen bij het sorteren van uw team en prioriteren van waarschuwingen. Security Center wordt automatisch toegepast branche aanbevolen procedures, intelligente algoritmen en processen die worden gebruikt door analisten om te bepalen of een bedreiging vormt een legitieme aanvraag is en u nuttige inzichten in de vorm van een betrouwbaarheidsscore biedt.

## <a name="how-the-confidence-score-is-triggered"></a>Hoe de betrouwbaarheidsscore wordt geactiveerd

Waarschuwingen worden gegenereerd wanneer er verdachte processen worden gedetecteerd die worden uitgevoerd op uw virtuele machines. Security Center controleert en analyseert deze waarschuwingen op Windows-machines uitvoeren in Azure. Deze geautomatiseerde controles en correlaties met behulp van geavanceerde algoritmen voor meerdere entiteiten en gegevensbronnen in de organisatie, en al uw Azure-resources uitvoert, en geeft weer met een score die een maateenheid voor hoe zeker Security Center is is vertrouwen dat is de waarschuwing authentiek is en moet worden onderzocht.

## <a name="understanding-the-confidence-score"></a>Informatie over de betrouwbaarheidsscore

De betrouwbaarheidsscore tussen 1 en 100-bereiken en Hiermee geeft u het vertrouwen van dat Security Center heeft dat de waarschuwing moet worden onderzocht. Hoe hoger de score is, het meer vertrouwen Security Center is dat de waarschuwing geeft aan dat legitieme schadelijke activiteiten. De betrouwbaarheidsscore bevat een lijst van de belangrijkste redenen waarom de waarschuwing de betrouwbaarheidsscore ontvangen. De betrouwbaarheidsscore gemakkelijker voor beveiligingsanalisten prioriteren van hun reactie op waarschuwingen en de meeste aanvallen eerst drukken, uiteindelijk verminderen de hoeveelheid tijd die nodig is om te reageren op aanvallen en schendingen aanpakken.

De betrouwbaarheidsscore weergeven:
- Open de blade beveiligingswaarschuwingen in de portal van Security Center.
-  De waarschuwingen en incidenten zijn ingedeeld van hoogste naar laagste, wat betekent dat het meer vertrouwen Security Center is dat een waarschuwing een bedreiging wordt beschouwd, hoe dichter dat naar de bovenkant van de pagina is vertegenwoordigt. 


 ![Betrouwbaarheidsscore][1]

De gegevens die hebben bijgedragen aan Security Center van vertrouwen in een waarschuwing weergeven:
- In de beveiliging onder blade waarschuwing **vertrouwen**, bekijk de metingen die hebben bijgedragen aan de betrouwbaarheidsscore en Verkrijg inzichten met betrekking tot de waarschuwing. Dit biedt u meer inzicht in de aard van de activiteiten die de waarschuwing heeft veroorzaakt.

  ![Verdachte betrouwbaarheidsscore][2]

Betrouwbaarheidsscore gebruikt Security Center om de prioriteit van waarschuwing classificeren in uw omgeving te. De betrouwbaarheidsscore bespaart u tijd en moeite door automatisch onderzoeken van waarschuwingen, toepassen van best practices voor de bedrijfstak en intelligente algoritmen en die fungeert als een virtuele analist om te bepalen welke bedreigingen van een echte zijn en waar u zich concentreren uw aandacht nodig hebt.


## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt uitgelegd hoe u de betrouwbaarheidsscore om de prioriteit van waarschuwing onderzoek te gebruiken. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.



<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png
