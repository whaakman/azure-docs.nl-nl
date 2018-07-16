---
title: Technische specificaties StorSimple | Microsoft Docs
description: Beschrijft de technische specificaties en wettelijk verplichte standaarden compatibiliteitsinformatie voor de StorSimple-hardware-onderdelen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 855ea6c34082b859bb5b5b6e69b3e3f2fa54eb4a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056460"
---
# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Technische specificaties en naleving voor de StorSimple-apparaat

## <a name="overview"></a>Overzicht

De hardware-onderdelen van uw Microsoft Azure StorSimple-apparaat voldoen aan de technische specificaties en wettelijk verplichte standaarden die worden beschreven in dit artikel. De technische specificaties beschrijven de stroom en koeling Modules (PCMs), schijven, opslagcapaciteit en behuizingen. De informatie over naleving bevat informatie over zaken als de internationale standaarden, veiligheid en uitstoot en kabels.

## <a name="power-and-cooling-module-specifications"></a>Voeding en koeling Module specificaties

Het StorSimple-apparaat heeft twee 100-240 V dual-ventilator, SBB-compatibele Power koeling Modules (PCMs). Dit biedt een configuratie met redundante voeding. Als een PCM mislukt, wordt het apparaat blijft normaal functioneren op de andere PCM totdat de mislukte-module is vervangen.

De behuizing EBOD maakt gebruik van een 580 W PCM en primaire behuizing gebruikt een 764 W PCM. De volgende tabellen worden de technische specificaties die zijn gekoppeld aan de PCMs.

| Specificatie | 580 W PCM (EBOD) | 764 W PCM (primair) |
| --- | --- | --- |
| Maximale Uitgangsstroom |580 W |764 |
| Frequentie |50/60 Hz |50/60 Hz |
| Bereikselectie voltage |Automatisch variëren: 90 – 264 V AC, 47/63 Hz |Automatisch variëren: 90-264 V AC, 47/63 Hz |
| Maximale stroomoverbelasting huidige |20 EEN |20 EEN |
| Correctie van energiebeheer van meerdere factoren |> 95% nominale ingangsvoltage |> 95% nominale ingangsvoltage |
| Harmonischen |Voldoet aan de EN61000-3-2 |Voldoet aan de EN61000-3-2 |
| Uitvoer |5V stand-by spanning \@ 2.0 A |5V stand-by spanning \@ 2.7 A |
| + 5V \@ 42 A |+ 5V \@ 40 A | |
| + 12V \@ 38 A |+ 12V \@ 38 A | |
| Hot pluggable |Ja |Ja |
| Switches en -LED 's |AC aan/uit-schakelaar en vier status-LED 's |AC aan/uit-schakelaar en zes status-LED 's |
| Behuizing koeling |Ene koeling fans met variabele ventilatorbesturing voor snelheid |Ene koeling fans met variabele ventilatorbesturing voor snelheid |

## <a name="power-consumption-statistics"></a>Power verbruiksstatistieken

De volgende tabel bevat de gebruikelijke power verbruiksgegevens (werkelijke waarden kunnen afwijken van de gepubliceerde) voor de verschillende modellen van StorSimple-apparaat.

| Voorwaarden | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC |
| --- | --- | --- | --- | --- | --- | --- |
|  Fans traag, schijven niet actief |1,45 A |0.31 kW |1057.76 BTU/uur |3.19 A |0.34 kW |1160.13 BTU/uur |
|  Fans vertragen, toegang tot stations |1,54 A |0,33 kW |1126.01 BTU/uur |3.27 A |0.36 kW |1228.37 BTU/uur |
|  Snel, stations voor fans is niet actief, twee PSUs aangestuurd |2.14 A |0.49 kW |1671.95 BTU/uur |4,99 A |0.54 kW |1842.56 BTU/uur |
|  Fans snelle, niet-actieve stations, een voeding mogelijk gemaakt een niet-actieve |2.05 A |0.48 kW |1637.83 BTU/uur |4,58 A |0,50 kW |1706.07 BTU/uur |
|  Snel, fans-stations wordt benaderd, twee PSUs aangestuurd |2, 26 G A |0.51 kW |1740.19 BTU/uur |4,95 A |0.54 kW |1842.56 BTU/uur |
|  Snelle fans, stations die toegang hebben tot, een voeding mogelijk gemaakt een niet-actieve |2.14 A |0.49 kW |1671.95 BTU/uur |4.81 A |0.53 kW |1808.44 BTU/uur |

## <a name="disk-drive-specifications"></a>Schijf-specificaties

Uw StorSimple-apparaat ondersteunt maximaal 12 3,5-inch formulier factor SAS Serial Attached SCSI ()-schijven. De werkelijke stations kunnen een combinatie van SSD-schijven (SSD's) of harde schijven (HDD's), afhankelijk van de configuratie van het product zijn. De 12 schijfstation sleuven bevinden zich in een configuratie met 3 van 4 vóór de behuizing. De behuizing EBOD kunt u extra opslag voor een andere 12 harde schijven. Dit zijn altijd HDD's.

## <a name="storage-specifications"></a>Opslag-specificaties

De StorSimple-apparaten hebben een combinatie van harde schijven en SSD-schijven voor zowel het 8100- en 8600. De totale bruikbare capaciteit voor het 8100- en 8600 zijn ongeveer 15 TB en 38 TB. De volgende tabel worden de details van SSD en HDD-cloud-capaciteit in de context van de capaciteit van de oplossing StorSimple.

| Apparaatmodel / capaciteit | 8100 | 8600 |
| --- | --- | --- |
| Aantal harde schijven (HDD's) |8 |19 |
| Aantal SSD-schijven (SSD's) |4 |5 |
| Één schijfcapaciteit |4 TB |4 TB |
| Één SSD-capaciteit |400 GB |800 GB |
| Ongebruikte capaciteit |4 TB |4 TB |
| Harde schijf aan bruikbare capaciteit |14 TB |36 TB |
| SSD aan bruikbare capaciteit |800 GB |2 TB |
| Totale bruikbare capaciteit * |~ 15 TB |~ 38 TB |
| Oplossing voor maximale capaciteit (met inbegrip van de cloud) |200 TB |500 TB |

<sup>* </sup>- *De totale bruikbare capaciteit bevat de beschikbare capaciteit voor gegevens, metagegevens en buffers. U kunt lokaal vastgemaakte volumes inrichten tot maximaal 8,5 TB op het 8100-apparaat of tot 22,5 TB op een groter 8600-apparaat. Ga voor meer informatie naar [StorSimple lokaal vastgemaakt volumes](storsimple-8000-local-volume-faq.md).*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Behuizing dimensies en gewicht specificaties

De volgende tabellen worden de verschillende behuizing-specificaties voor dimensies en gewicht.

### <a name="enclosure-dimensions"></a>Behuizing dimensies

De volgende tabel bevat de afmetingen van de behuizing in millimeters en inch.

| Systeembehuizing | Millimeters | Inches |
| --- | --- | --- |
| Hoogte |87.9 |3.46 |
| Breedte in flens koppelen |483 |19.02 |
| Breedte in de hoofdtekst van de behuizing |443 |17.44 |
| Diepte van flens front koppelen aan het uiteinde van de hoofdtekst van de behuizing van |577 |22.72 |
| Diepte van bewerkingen deelvenster naar verst uiteinde van de behuizing |630.5 |24.82 |
| Diepte van flens koppelen aan verst uiteinde van bijlage |603 |23.74 |

### <a name="enclosure-weight"></a>Behuizing-gewicht

Afhankelijk van de configuratie, een volledig geladen primaire behuizing wegen van 21 aan 33 kg en twee personen verwerkt; het is vereist.

| Systeembehuizing | Gewicht |
| --- | --- |
| Maximale gewicht (afhankelijk van de configuratie) |30 kg – 33 kg |
| Leeg (Er zijn geen stations die zijn aangebracht) |21: 23 kg |

## <a name="enclosure-environment-specifications"></a>Behuizing omgeving specificaties

In deze sectie bevat de specificaties die betrekking hebben op de behuizing-omgeving. De temperatuur, vochtigheid, hoogte, schudt, trillingen, richting, veiligheid en elektromagnetische compatibiliteit (EMC) zijn opgenomen in deze categorie.

### <a name="temperature-and-humidity"></a>Temperatuur en vochtigheid

| Systeembehuizing | De omgevingstemperatuur bereik | De relatieve luchtvochtigheid | Maximale NAT thermometerbolletje |
| --- | --- | --- | --- |
| Operationeel |5 TEMPERATUUR - 35° C (41° F - 95° F) |20% tot 80% niet-voegen tot- |28° C (82° F) |
| Niet-operationeel |-40 TEMPERATUUR - 70° C (40° F - 158° F) |5-100% niet-voegen tot |29 GC (84° F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luchtstroom, hoogte, schudt, trillingen, richting, veiligheid en EMC

| Systeembehuizing | Operationele specificaties |
| --- | --- |
| Luchtstroom |Systeem luchtstroom is voor naar achteren. Systeem moet worden gebruikt met een low-pressure, achterzijde uitlaatgas installatie. Zwaar wordt belast zijn gemaakt door rackdeuren en obstakels mag niet meer dan 5 Pascal (0,5 mm water meter). |
| Hoogte, operationele |-30-meters voor 3045 meters (voet naar 10.000 voet-100) met maximale temperatuur ongedaan maken door 5 ° C hierboven 7000 voet beoordeeld. |
| Hoogte, niet-operationeel |-305 meters op 12.192 meters (-1,000 voet naar 40.000 voet) |
| Schudt, operationele |5g 10 ms ½ sinus |
| Schudt, niet-operationeel |30g 10 ms ½ sinus |
| Trillingen, operationele |0.21g RMS 5 tot 500 Hz willekeurige |
| Trillingen, niet-operationeel |1.04g RMS 2 tot 200 Hz willekeurige |
| Trillingen, verplaatsing |3g 2 tot 200 Hz sinus |
| De afdrukstand en koppelen |19-inch rek monteren (2 EIA eenheden) |
| Rackrails |Aan de minimale 700 mm (31.50 inch) diepte rekken compatibel zijn met IEC 297 |
| Veiligheid en -goedkeuringen |CE en UL EN 61000-3, IEC 61000-3, UL 61000-3 |
| EMC |EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Internationale standaarden naleving

Uw Microsoft Azure StorSimple-apparaat voldoet aan de volgende internationale standaarden:  

* CE - EN 60950-1
* CB rapport IEC 60950-1
* UL en cUL UL 60950-1

## <a name="safety-compliance"></a>Naleving van de veiligheid

Uw Microsoft Azure StorSimple-apparaat voldoet aan de volgende classificaties voor veiligheid:

* Goedkeuring van systeem product: UL, cUL, CE
* Naleving van de veiligheid: UL 60950, IEC 60950, nl 60950

## <a name="emc-compliance"></a>Naleving van EMC

Uw Microsoft Azure StorSimple-apparaat voldoet aan de volgende classificaties van EMC.

### <a name="emissions"></a>Uitstoot

Het apparaat is compatibel met EMC voor die wordt uitgevoerd en uitgezonden niveaus.

* Uitgevoerde uitstoot beperken niveaus: CFR 47 onderdeel 15B-klasse A EN55022 klasse A CISPR klasse A
* Uitgezonden uitstoot beperken niveaus: CFR 47 onderdeel 15B-klasse A EN55022 klasse A CISPR klasse A

### <a name="harmonics-and-flicker"></a>Harmonischen en knipperen

Het apparaat voldoet aan EN61000-3-2/3.

### <a name="immunity-limit-levels"></a>Ongevoeligheid limiet niveaus

Het apparaat voldoet aan EN55024.

## <a name="ac-power-cord-compliance"></a>AC power kabel naleving

De plug en de volledige power kabel assembly moeten voldoen aan de normen die geschikt is voor het land waarin het apparaat wordt gebruikt moeten, en ze veiligheid goedkeuringen die geaccepteerd in dat land zijn. De volgende tabellen worden standaarden voor de Verenigde Staten en Europa.

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC-stroomkabels - Verenigde Staten (moet zich NRTL vermeld)

| Onderdeel | Specificatie |
| --- | --- |
| Type van de kabel |SV of SVT, 18 AWG minimaal, 3 conductor 2.0 meter maximale lengte |
| Plug |NEMA 5 tot 15P basiskennis van het type bijlage plug beoordeeld 120 V, 10 A; of IEC 320 C14, 250 V, 10 A |
| Socket |IEC 320 C-13, 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>AC-stroomkabels - Europa

| Onderdeel | Specificatie |
| --- | --- |
| Type van de kabel |Geharmoniseerde, H05-VVF-3G1.0 |
| Socket |IEC 320 C-13, 250 V, 10 A |

## <a name="supported-network-cables"></a>Ondersteunde netwerkkabels

Voor de 10 GbE-netwerkinterfaces DATA 2 en DATA 3, verwijzen naar de [lijst met ondersteunde netwerkkabels en modules](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te implementeren van een StorSimple-apparaat in uw datacenter. Zie voor meer informatie, [implementeren van uw on-premises apparaat](storsimple-8000-deployment-walkthrough-u2.md).

