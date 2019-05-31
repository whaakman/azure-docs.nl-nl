---
title: Opmerkingen bij de release van Microsoft Azure StorSimple Virtual Array Update 1.2 | Microsoft Docs
description: Beschrijving van essentiële openstaande problemen en oplossingen voor de StorSimple Virtual Array Update 1.2 uitgevoerd.
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 05/29/2019
ms.author: alkohli
ms.openlocfilehash: ea7e4801dfaad533403c0f927a03735ae409cc52
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420601"
---
# <a name="storsimple-virtual-array-update-12-release-notes"></a>Opmerkingen bij de release van de StorSimple Virtual Array Update 1.2

De volgende releaseopmerkingen identificeren de kritieke problemen voor het openen en de problemen opgelost voor Microsoft Azure StorSimple Virtual Array updates.

Opmerkingen bij de release worden voortdurend bijgewerkt. Als er kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw StorSimple Virtual Array implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 1.2 komt overeen met de softwareversie **10.0.10311.0**.

> [!IMPORTANT]
> - Updates opnieuw moet worden opgestart en uw apparaat opnieuw opstarten. Als i/o's uitgevoerd worden, heeft het apparaat leidt tot downtime. Ga voor gedetailleerde instructies voor pakketten die worden gebruikt voor het toepassen van deze update naar [Update 1.2 downloaden](#download-update-12).
> - Update 1.2 is beschikbaar voor u via de Azure-portal alleen als uw apparaat wordt uitgevoerd in de Update 1.0 of 1.1.

## <a name="whats-new-in-update-12"></a>Wat is er nieuw in Update 1.2

Deze update bevat de volgende oplossingen voor problemen:

- Verbeterde tolerantie bij het verwerken van verwijderde bestanden.
- Verbeterde afhandeling van uitzonderingen in de code opstartpad naar lagere fouten in de back-ups, herstel, leesbewerkingen in de cloud en geautomatiseerde vrijmaken van ruimte.

## <a name="download-update-12"></a>Update 1.2 downloaden

Deze update te downloaden, gaat u naar de [Microsoft Update-catalogus](https://www.catalog.update.microsoft.com/Home.aspx) -server en de KB4502035 downloadpakket. Dit pakket bevat de volgende pakketten:

 - **KB4493446** die cumulatieve Windows-Updates bevat voor 2012 R2 tot April 2019. Voor meer informatie over wat is inbegrepen in deze rollup, gaat u naar [April maandelijkse security updatepakket](https://support.microsoft.com/help/4493446/windows-8-1-update-kb4493446).
 - **KB3011067** is een zelfstandige pakket voor Microsoft Update-bestand WindowsTH-KB3011067-x64. Dit bestand wordt gebruikt om bij te werken software van het apparaat.

KB4502035 downloaden en volg deze instructies voor [toepassen van de update via lokale webinterface](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="issues-fixed-in-update-12"></a>Problemen opgelost in Update 1.2

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Verwijderen| In de vorige versies van de software is het een probleem bij het gebruik van het apparaat niet hebt gewijzigd, zelfs wanneer de bestanden zijn verwijderd. Dit probleem is opgelost in deze versie. Warmtemeting codepad gedaan toleranter bij het verwerken van verwijderde bestanden.|
| 2 |Afhandeling van uitzonderingen| In de vorige versies van de software is er een probleem dat na het systeem opnieuw is opgestart, dat kan leiden tot fouten in de back-ups, herstel, lezen vanuit de cloud, en het vrijmaken van ruimte wordt geautomatiseerd. Deze release bevat wijzigingen over hoe de uitzonderingen zijn verwerkt in de opstartmap.|

## <a name="known-issues-in-update-12"></a>Bekende problemen in Update 1.2

Er zijn geen nieuwe problemen zijn release hebt genoteerd in Update 1.2. Alle problemen release vermeld worden overgedragen uit vorige versies. Als u wilt zien van de samenvatting van de bekende problemen die uit de vorige versies, gaat u naar [bekende problemen in Update 1.1](storsimple-virtual-array-update-11-release-notes.md#known-issues-in-update-11).

## <a name="next-steps"></a>Volgende stappen

KB4502035 downloaden en [toepassen van de update via lokale webinterface](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="references"></a>Verwijzingen

Wilt u een oudere releaseopmerking? Ga naar:
* [Opmerkingen bij de StorSimple Virtual Array Update 1.1 Release](storsimple-virtual-array-update-11-release-notes.md)
* [Opmerkingen bij de StorSimple Virtual Array Update 1.0 Release](storsimple-virtual-array-update-1-release-notes.md)
* [Opmerkingen bij de StorSimple Virtual Array Update 0.6 Release](storsimple-virtual-array-update-06-release-notes.md)
* [StorSimple Virtual Array Update 0.5 Release Notes](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple Virtual Array Update 0.4 Release Notes](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple Virtual Array Update 0.3 Release Notes](storsimple-ova-update-03-release-notes.md)
* [StorSimple Virtual Array Update 0.1 en 0.2 opmerkingen bij de Release](storsimple-ova-update-01-release-notes.md)
* [Opmerkingen bij de Release van StorSimple Virtual Array algemene beschikbaarheid](storsimple-ova-pp-release-notes.md)
