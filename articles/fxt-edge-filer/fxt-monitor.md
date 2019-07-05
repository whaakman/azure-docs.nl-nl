---
title: De Azure FXT Edge Filer bewaken
description: Status van uw hardware voor de cache van Azure FXT Edge Filer hybride opslag controleren
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: e7395c69d99884a5c662e545a69778ed195aec55
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543154"
---
# <a name="monitor-azure-fxt-edge-filer-hardware-status"></a>Status van uw hardware Azure FXT Edge Filer bewaken

Het Azure FXT Edge Filer hybride opslagsysteem cache heeft meerdere status verlichting ingebouwd in het chassis om te begrijpen hoe de hardware functioneert beheerders te helpen.

## <a name="system-health-status"></a>Status van systeemstatus

Gebruiken voor het bewaken van cachebewerkingen op een hoger niveau van de software Configuratiescherm **Dashboard** pagina, zoals beschreven in de [Control Panel Dashboard Guide](https://azure.github.io/Avere/legacy/dashboard/4_7/html/ops_dashboard_index.html)

## <a name="hardware-status-leds"></a>Status van uw hardware LED 's

Deze sectie wordt uitgelegd dat de verschillende status verlichting ingebouwd in de Azure FXT Edge Filer-hardware.

### <a name="hard-drive-status-leds"></a>Status van de harde schijf LED 's

![afbeelding van voorgrond harde schijf, horizontaal, met bijschrift labels 2 (linkerbovenhoek), 1 (linkerbenedenhoek) en 3 (rechts)](media/fxt-monitor/fxt-drive-callouts.png)

Elke schijf voor de provider heeft twee status-LED's: een indicator voor netwerkactiviteit (1) en een statusindicator (2). 

* Activiteit (1)-LED lichten in het station wordt gebruikt.  
* De status LED (2) geeft aan dat de voorwaarde van de schijf met behulp van de codes in de onderstaande tabel.

| Station status LED-status              | Betekenis  |
|-------------------------------------|----------------------------------------------------------|
| Flitsen groen is twee keer per seconde      | Identificeren van station *of* <br> Station voorbereiden voor verwijderen  |
| Uitgeschakeld (uitgeschakeld lampje)                         | Systeem is niet voltooid voor opstarten *of* <br>Station is gereed om te worden verwijderd |
| Flitsen groen, geel, en zijn uitgeschakeld       | Stationsfout wordt voorspeld   |
| Oranje flitsen vier keer per seconde | Station is mislukt   |
| Groen                         | Schijf is online |

Aan de rechterkant van het station (3) heeft de capaciteit van het station en andere informatie.

Station nummers worden afgedrukt op de ruimte tussen de stations. In de Azure FXT Edge Filer, station 0 station links bovenin en station 1 is direct eronder. In dit patroon blijft nummering. 

![foto van een harde schijf in het chassis FXT, van schijf cijfers en -capaciteit labels bay](media/fxt-drives-photo.png)

## <a name="left-control-panel"></a>Links het Configuratiescherm

De linker front-Configuratiescherm heeft verschillende statusindicatoren LED (1) en geen integriteitsindicator grote verlicht system (2). 

![links statuspaneel met 1 labels statusindicatoren aan de linkerkant en labels van de grote system integriteitsindicator lichte aan de rechterkant 2](media/fxt-monitor/fxt-control-panel-left.jpg)

### <a name="control-panel-status-indicators"></a>Besturingselement voor deelvenster statusindicatoren 

De statusinformatie aan de linkerkant weergegeven effen lichte geel als er een fout is opgetreden in dat systeem. De volgende tabel beschrijft mogelijke oorzaken en oplossingen voor de fouten. 

Als u nog steeds de fout nadat u hebt geprobeerd deze oplossingen hebt, neem dan contact op met de ondersteuning voor hulp. 

| Pictogram | Description | Foutvoorwaarde | Mogelijke oplossingen |
|----------------|---------------|--------------------|----------------------|
| ![het pictogram van station](media/fxt-monitor/fxt-hd-icon.jpg) | Stationsstatus van | Fout station | Controleer het logboek voor systeemgebeurtenissen om te bepalen of het station een fout heeft of <br>Voer de juiste online diagnostische test; het systeem opnieuw opstarten en diagnostische gegevens van embedded (ePSA), of <br>Als de stations zijn geconfigureerd in een RAID, het systeem opnieuw opstarten en voert u de host-adapter configuratie hulpprogramma 's |
|![pictogram temperatuur](media/fxt-monitor/fxt-temp-icon.jpg) | Status van de temperatuur | Warmte fout - bijvoorbeeld een fan is mislukt of omgevingstemperatuur valt buiten het bereik | Controleer de volgende adresseerbare voorwaarden: <br>Een ventilator ontbreekt of is mislukt <br>Van het systeem dekking, lucht shroud, geheugenmodule leeg of terug opvullende haakje is verwijderd <br>De omgevingstemperatuur is te hoog <br>Externe luchtstroom vorm |
|![pictogram elektriciteit](media/fxt-monitor/fxt-electric-icon.jpg) | Elektrische status | Elektrische fout - bijvoorbeeld spanning buiten het bereik, is mislukt voor voeding, of een spoorwegregelgever mislukte voltage |  Controleer het gebeurtenislogboek van systeem of systeemberichten voor dit specifieke probleem. Als er een probleem voeding, de status van de voeding LED controleren en de voeding vervangen indien nodig. | 
|![geheugen-pictogram](media/fxt-monitor/fxt-memory-icon.jpg) | Geheugenstatus | Geheugenfout | Controleer het logboek voor systeemgebeurtenissen of systeemberichten voor de locatie van het geheugen van de mislukte de geheugenmodule vervangen. |
|![PCIe-pictogram](media/fxt-monitor/fxt-pcie-icon.jpg) | PCIe-status | Fout bij PCIe-kaart | Opnieuw opstarten van het systeem. PCIe netwerkkaartstuurprogramma's; bijwerken installeren van de kaart |


### <a name="system-health-status-indicator"></a>System health statusindicator

De grote branden knop aan de rechterkant van het Configuratiescherm van links geeft aan dat de algehele systeemstatus en wordt ook gebruikt als een eenheid locator licht in de modus voor systeem-ID.

Druk op de status van het bestandssysteem en de ID-knop om over te schakelen tussen de systeem-ID en system health modus.

|Status van de systeemstatus-status | Voorwaarde |
|-------------------------------------------|-----------------------------------------------|
| Effen blauw | Normale werking: het systeem is ingeschakeld, werken normaal, en het systeem-ID-modus is niet actief. <br/>Druk op de status van het bestandssysteem en de ID-knop als u wilt overschakelen naar de systeem-ID-modus. |
| Knipperende blauw | Systeem-ID-modus is actief. Druk op de status van het bestandssysteem en de systeem-ID knop als u wilt overschakelen naar de system health-modus. |
| Effen oranje | Het systeem zich in de failsafe bevindt. Als het probleem zich blijft voordoen, neem contact op met Microsoft Customer Service and Support. |
| Oranje knipperende | Systeemfout. Controleer het logboek voor systeemgebeurtenissen voor specifieke foutberichten. Voor informatie over de gebeurtenis- en foutberichten die worden gegenereerd door de firmware van de computer en de agents die onderdelen van het systeem bewaken, Zie de pagina voor zoeken naar de Code van fouten op qrl.dell.com. |


