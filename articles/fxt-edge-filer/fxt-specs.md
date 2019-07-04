---
title: Microsoft Azure FXT Edge Filer specificaties | Microsoft Docs
description: Fysieke en omgevingsbeveiliging specificaties voor Azure FXT Edge Filer hardware
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: overview
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 0679bce8eae515aa6b90e34fcfd15ee9b4e56b31
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542876"
---
# <a name="azure-fxt-edge-filer-specifications"></a>Azure FXT Edge Filer specificaties

In dit artikel wordt uitgelegd dat de hardwarespecificaties voor Azure FXT Edge Filer hardwareknooppunten. In de praktijk, drie of meer knooppunten worden ze samen worden geconfigureerd voor het systeem geclusterde cache.

## <a name="hardware-specifications"></a>Hardwarespecificaties

| Onderdeel | FXT 6600 | FXT 6400 |
|----------|-----------|-----------|
| CPU-kernen |  16 | 16 |
| DRAM  | 1536 GB | 768 GB |
| Netwerkpoorten | 6 x 25/10 Gb + 2 x 1 Gb | 6 x 25/10 Gb + 2 x 1 Gb |
| NVMe SSD-capaciteit | 25,6 TB | 12,8 TB |

## <a name="drive-specifications"></a>Stationsspecificaties

Het systeem heeft tien station inch, toegankelijk is vanaf het begin. Elke ingevulde station is met het label aan de rechterkant met informatie over capaciteit. 

Station nummers worden afgedrukt op de ruimte tussen de stations. In de Azure FXT Edge Filer, station 0 station links bovenin en station 1 is direct eronder.

![foto van een harde schijf in het chassis FXT, van schijf cijfers en -capaciteit labels bay](media/fxt-drives-photo.png)

| Getallen van station    |  Gebruiken   |  Specificaties |
|------------------|--------|-----------------|
| 0, 1             | OS     | 480 GB SATA SSD |
| 2, 3, 4, 5, 6, 7, 8, 9 | Data   | FXT 6600: 3.2 TB NVMe SSD <br> FXT 6400: 1.6 TB NVMe SSD |


## <a name="dimensions-and-weight"></a>Afmetingen en gewicht

De Azure FXT Edge Filer is ontworpen om aan te passen op een rek standard 19-inch apparatuur en is een rackeenheid hoge (1U). 

<!-- 10x2.5 inches version -->

| Filer dimensies           |                          |
|-----------------------------|--------------------------|
| Hoogte                      | 42.8 mm (1.68 inch)    |
| Breedte (inclusief rack klinken) | 482.0 mm (18.97 inch)  |
| Breedte - hoofdbehuizing      | 434.0 mm (17.08 inch) |
| Diepte - klinken rack aan achterzijde hoofdbehuizing                   | 733.82 mm (29.61 inch) |
| Diepte - rack klinken naar verst achterzijde toegestane                 | 772.67 mm (30.42 inch) |
| Diepte - rack klinken naar verst front toegestane, zonder omlijsting | 22.0 mm (0.87 inch)  |
| Diepte - rack klinken naar verst front toegestane, met omlijsting    | 35.84 mm (1.41 inch) |

| Gewicht | |
|-----------------|----------------------|
| Knooppunt gewicht (zonder verpakking, zonder accessoires) | 40 lbs (18.1 kg) |
| NET gewicht (zonder verpakking, met inbegrip van accessoires) | 51 lbs (23.1 kg)|
| Bruto gewicht (zoals verzonden, omvat alle pakketten) |  64 lbs (29.0 kg) |

### <a name="shipping-dimensions"></a>Verzending van dimensies

| Pakket-dimensie | Millimeters | Inches |
|-------------------|-------------|--------|
| Hoogte            | 311.2       | 12.25" |
| Breedte             | 642.8       | 25.31" |
| Lengte            | 1,051.1     | 41.38" |

## <a name="power-and-thermal-specifications"></a>Kracht en warmte-specificaties

In deze sectie biedt power beoordelingen en afmetingen voor de Azure FXT Edge Filer.

### <a name="nameplate-ratings"></a>Naamplaatje

| Naamplaatje classificaties voor FXT 6000 reeks modellen |
|----------------|
| 100 - 240V~    |
| 10A - 5A (X2)  |
| 50/60Hz         |

<!-- matches the Dell regulatory label exactly -->

### <a name="power-and-thermal-measurements"></a>Power en warmte-metingen 

Azure FXT Edge Filer-knooppunten gebruiken variabele snelheid ventilatoren, zodat power is afhankelijk van de temperatuur en belasting. Maximale ventilatorsnelheden kunnen worden bereikt op bepaalde combinaties van hoge belasting en verhoogde omgevingstemperatuur. 

Deze grafieken geven energieverbruik en hitte-metingen voor veelgebruikte spanning-frequentie combinaties. 

| FXT 6600 power bij omgevingstemperatuur <br />(22° C, 71.6° F) | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Voltage (V) | 100 | 120 | 208 | 230 | 240 | 
| Frequentie (Hz) | 60 | 60 | 60 | 50 | 50 |
| Huidige (A) | 5.02 | 4.16 |2.40 | 2.20 | 2.16 |
| Zichtbare Power (VA) | 502 | 499 | 499 | 506 | 518|
| Power Factor | 0,99 | 0,99 |0.98 | 0.98 | 0.98 |
| Echte kracht (S) | 497 |494 | 489 | 496 | 508 |
| Warmtedissipatie (BTU/uur) |1696 | 1686 | 1669 | 1692 | 1733 |

| FXT 6600 power snelheden maximale-ventilator | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Voltage (V) | 100 |120 | 208 | 230 | 240| 
| Frequentie (Hz) | 60 | 60 | 60 | 50 | 50 |
| Huidige (A) | 5.98 | 5.01 | 2.81 | 2.55 | 2.48 |
| Zichtbare Power (VA) | 598 | 601 | 584 | 587 | 595 |
| Power Factor | 0,99 | 0,99 | 0.98 | 0.98 | 0.98 |
| Echte kracht (S) | 592 | 595 | 573 | 575 | 583 |
| Warmtedissipatie (BTU/uur) | 2020 |2031 | 1954 | 1961 | 1990 |

| FXT 6400 power bij omgevingstemperatuur <br />(22° C, 71.6° F) | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Voltage (V) | 100 | 120 | 208 | 230 | 240 |
| Frequentie (Hz) |60 | 60 | 60 | 50 | 50 |
| Huidige (A) | 4.63 | 3.86 | 2.24 | 2.04 | 1.94 |
| Zichtbare Power (VA) | 463 | 463 | 466 | 469 | 466 |
| Power Factor | 0,99 | 0,99 | 0.98 | 0.98 | 0.98 | 
| Echte kracht (S) | 458 | 459 | 457 | 460 | 456 |
| Warmtedissipatie (BTU/uur) | 1564 | 1565 | 1558 | 1569 | 1557 |

| FXT 6400 power snelheden maximale-ventilator | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz |
|---------|---|---|---|---|---|
| Voltage (V) | 100 | 120 | 208 | 230 | 240 |
| Frequentie (Hz) | 60 | 60 | 60 | 50 | 50 |
| Huidige (A) | 5.15 | 4.28 | 2.48 | 2.28 | 2.13 |
| Zichtbare Power (VA) | 515 | 514 | 516 | 524 | 511 |
| Power Factor | 0,99 | 0,99 | 0.98 | 0.98 | 0.98 |
| Echte kracht (S) | 510 | 508 | 506 | 514 | 501 |
| Warmtedissipatie (BTU/uur) | 1740 | 1735 | 1725 | 1753 | 1709 |

## <a name="environmental-requirements"></a>Omgevingsvereisten

In deze sectie geeft de specificaties voor de omgeving van de hardware.

### <a name="temperature-and-humidity"></a>Temperatuur en vochtigheid

| Uitwerking kenmerk   | Bedrijfsgebied                   | Niet in bedrijf         |
|---------------------------|-----------------------------------|-----------------------------|
| De omgevingstemperatuur bereik | 10° C tot 35° C (50-86° F)          | De GC-40 tot 65 ° C (-40-149 ° F) |
| De relatieve luchtvochtigheid | 10% tot 80% niet-voegen tot          | 5-95% niet-voegen tot     |
| Maximale dauwpunt         | 29°C (84°F)                       | 33 GC (91° F)                 |
| Hoogte                  | maximaal 3048 meters (10.000 voet), afhankelijk van temperatuur waardering ongedaan maken zoals hieronder vermeld. | 12.000 meters (39,370 voet) |

> [!NOTE] 
> **Hoogte temperatuur ongedaan maken score:** Maximale temperatuur verminderd door 1° C/300 m (1° F/547 ft) boven 950 m (3,117 ft).

### <a name="airflow-shock-and-vibration"></a>Luchtstroom schudt en geluid 

| Kenmerk         | Specificatie |
|-------------------|---------------|
| Luchtstroom                    | Systeem luchtstroom is voor naar achteren. Systeem moet worden beheerd met een low-pressure achterzijde uitlaatgas-installatie. |
| Schudt, operationele         | 6 G voor 11 milliseconden (getest in 6 richtingen) |
| Schudt, niet-operationeel     | 71 G voor 2 milliseconden (getest in 6 richtingen) |
| Trillingen, operationele     | 0,26 G<sub>RMS</sub> 5 Hz tot 350 Hz willekeurige         |
| Trillingen, niet-operationeel | 1.88 G<sub>RMS</sub> 10 Hz tot 500 Hz gedurende 15 minuten (alle zes zijden getest)  |

## <a name="safety-regulation-compliance"></a>Veiligheid verordening naleving 

Azure FXT Edge Filer voldoet aan de vermelde regelgeving. 

| Category       | Specificatie van regelgeving | 
|----------------|--------------------------|
| Algemene veiligheid | EN 60950-1:2006 A1:2010 + A2:2013 + A11:2009 + A12:2011 / IEC 60950-1:2005 ed2 + A1:2009 + A2:2013 <br>NL 62311:2008 | 
| EMC            | FCC A, ICES-003  <br>NL 55032:2012 / CISPR 32:2012  <br>NL 55032:2015 / CISPR 32:2015  <br>NL 55024:2010 + A1:2015 / CISPR 24:2010 + A1:2015  <br>NL 61000-3-2:2014 / IEC 61000-3-2:2014 (klasse D)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013 |
| Energie         | Verordening (EU) Nee 617/2013  |
| RoHS           |    EN 50581:2012   |