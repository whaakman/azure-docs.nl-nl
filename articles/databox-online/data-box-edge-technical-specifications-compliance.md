---
title: Microsoft Azure Data Box Edge technische specificaties en naleving | Microsoft Docs
description: Meer informatie over de technische specificaties en de naleving voor uw Azure Data Box-Edge
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/12/2019
ms.author: alkohli
ms.openlocfilehash: 8ef35709e90c0a58cc0ff8df1afb6e864adc0a23
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/15/2019
ms.locfileid: "57994706"
---
# <a name="azure-data-box-edge-technical-specifications-preview"></a>Technische specificaties voor Azure Data Box Edge (preview)

De hardware-onderdelen van uw Microsoft Azure Data Box Edge-apparaat voldoen aan de technische specificaties en wettelijk verplichte standaarden die worden beschreven in dit artikel. De technische specificaties beschrijven de Power supply units (PSUs), opslagcapaciteit, behuizingen en uitwerking standaarden. 

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Bekijk de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 

## <a name="power-supply-unit-specifications"></a>Power supply eenheid specificaties

De gegevens in het Edge-apparaat heeft twee 100-240 V Power supply eenheden (PSUs) met hoge prestaties fans. De twee PSUs bieden een configuratie met redundante voeding. Als een voeding mislukt, wordt het apparaat blijft normaal functioneren op de andere voeding, totdat de mislukte-module is vervangen. De volgende tabel bevat de technische specificaties van de PSUs.

| Specificatie           | 750 W PSU                  |
|-------------------------|----------------------------|
| Maximale Uitgangsstroom    |  750 W                     |
| Frequentie               | 50/60 Hz                   |
| Bereikselectie voltage | Automatisch variëren: 100-240 V AC |
| Hot pluggable           | Ja                        |

<!--## Power consumption statistics

The following table lists the typical power consumption data (actual values may vary from the published) for the Data Box Edge device.-->

## <a name="storage-specifications"></a>Opslag-specificaties

De gegevens in het Edge-apparaten hebben 10 X 2,5" NVMe SSD's, elk met een capaciteit van versie 1.6 TB. Van deze SSD's, 2 besturingssysteemschijven en de andere 8 gegevensschijven. De totale bruikbare capaciteit voor het apparaat is ongeveer 12,5 TB. De volgende tabel bevat de details voor de opslagcapaciteit van het apparaat.

|     Specificatie                          |     Value             |
|--------------------------------------------|-----------------------|
|    Aantal SSD-schijven (SSD's)     |    8                  |
|    Één SSD-capaciteit                     |    1,6 TB             |
|    Totale capaciteit                          |    12,8 TB            |
|    Totale bruikbare capaciteit *                  |    ~ 12.5 TB            |

**Maak ruimte is gereserveerd voor intern gebruik.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Behuizing dimensies en gewicht specificaties

De volgende tabellen worden de verschillende behuizing-specificaties voor dimensies en gewicht.

### <a name="enclosure-dimensions"></a>Behuizing dimensies

De volgende tabel bevat de afmetingen van de behuizing in millimeters en inch.

|     Systeembehuizing     |     Millimeters     |     Inches     |
|-------------------|---------------------|----------------|
|    Hoogte         |    304.8            |    12          |
|    Breedte          |    660.4            |    26          |
|    Diepte          |    1041.4           |    41          |

### <a name="enclosure-weight"></a>Behuizing-gewicht

Het pakket apparaat weegt 66 lbs. en twee personen verwerkt; het is vereist. Het gewicht van het apparaat, is afhankelijk van de configuratie van de behuizing.

|     Systeembehuizing                                 |     Gewicht          |
|-----------------------------------------------|---------------------|
|    Totaal gewicht, met inbegrip van het inpakken       |    66 lbs.          |
|    Gewicht van het apparaat                       |    48,3 lbs.        |

## <a name="enclosure-environment-specifications"></a>Behuizing omgeving specificaties

In deze sectie bevat de specificaties die betrekking hebben op de behuizing-omgeving, zoals temperatuur, vochtigheid en hoogte.

### <a name="temperature-and-humidity"></a>Temperatuur en vochtigheid

|     Systeembehuizing         |     De omgevingstemperatuur bereik     |     De relatieve luchtvochtigheid     |     Maximale dauwpunt     |
|-----------------------|--------------------------------------|--------------------------------------|---------------------------|
|    Operationeel        |    10 TEMPERATUUR - 35° C (50° F - 86° F)         |    10% tot 80% niet-voegen tot.         |    29°C (84°F)            |
|    Niet-operationeel    |    De GC-40 tot 65 ° C (-40 ° F - 149 ° F)     |    5-95% niet-voegen tot.          |    33 GC (91° F)            |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luchtstroom, hoogte, schudt, trillingen, richting, veiligheid en EMC

|     Systeembehuizing                           |     Operationele specificaties                                                                                                                                                                                         |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Luchtstroom                              |    Systeem luchtstroom is voor naar achteren. Systeem moet worden gebruikt met een low-pressure, achterzijde uitlaatgas installatie. <!--Back pressure created by rack doors and obstacles should not exceed 5 pascals (0.5 mm water gauge).-->    |
|    Maximale hoogte, operationele        |    3048 meters (10.000 voet) met hoogste temperatuur ongedaan maken geclassificeerd bepaald door [temperatuur ongedaan maken waardering specificaties](#operating-temperature-de-rating-specifications).                                                                                |
|    Maximale hoogte, niet-operationeel    |    12.000 meters (39,370 voet)                                                                                                                                                                                         |
|    Schudt, operationele                   |    6 G voor 11 milliseconden in 6 richtingen                                                                                                                                                                         |
|    Schudt, niet-operationeel               |    71 G voor 2 milliseconden in 6 richtingen                                                                                                                                                                           |
|    Trillingen, operationele               |    0,26 G<sub>RMS</sub> 5 Hz tot 350 Hz willekeurige                                                                                                                                                                                     |
|    Trillingen, niet-operationeel           |    1.88 G<sub>RMS</sub> 10 Hz tot 500 Hz gedurende 15 minuten (alle zes zijden getest.)                                                                                                                                                  |
|    De afdrukstand en koppelen             |    19-inch rek monteren                                                                                                                                                                                        |
|    Veiligheid en -goedkeuringen                 |    EN 60950-1:2006 A1:2010 + A2:2013 + A11:2009 + A12:2011 / IEC 60950-1:2005 ed2 + A1:2009 + A2:2013 EN 62311:2008                                                                                                                                                                       |
|    EMC                                  |    FCC A, ICES-003 <br>NL 55032:2012 / CISPR 32:2012  <br>NL 55032:2015 / CISPR 32:2015  <br>NL 55024:2010 + A1:2015 / CISPR 24:2010 + A1:2015  <br>NL 61000-3-2:2014 / IEC 61000-3-2:2014 (klasse D)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013                                                                                                                                                                                         |
|    Energie             |    Verordening (EU) Nee 617/2013                                                                                                                                                                                        |
|    RoHS           |    EN 50581:2012                                                                                                                                                                                        |


### <a name="operating-temperature-de-rating-specifications"></a>Ongedaan maken waardering specificaties temperatuur

|     Operationele temperatuur waardering ongedaan maken     |     De omgevingstemperatuur bereik                                                         |
|--------------------------------------------|------------------------------------------------------------------------------------------|
|    Tot 35 GC (95° F)                       |    Maximale temperatuur verminderd door 1° C/300 m (1° F/547 ft) boven 950 m (3,117 ft).    |
|    35° C tot 40° C (95° F tot 104° F)            |    Maximale temperatuur verminderd door 1° C/175 m (1° F/319 ft) boven 950 m (3,117 ft).    |
|    40° C tot 45° C (104° F tot 113° F)           |    Maximale temperatuur verminderd door 1° C/125 m (1° F/228 ft) boven 950 m (3,117 ft).    |


## <a name="next-steps"></a>Volgende stappen

- [Uw Azure Data Box-Edge implementeren](data-box-edge-deploy-prep.md)
