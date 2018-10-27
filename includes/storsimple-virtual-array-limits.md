---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 658fd9178495f14274c85eab2129c9dcd3be7693
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166157"
---
| **Limiet-ID** | **Limiet** | **Opmerkingen** |
| --- | --- | --- |
| Totale capaciteit (met inbegrip van de cloud) |Tot wel 64 TB per virtueel apparaat |U kunt een volledige virtuele StorSimple-matrix naar een andere lege matrix een failover. Als u probeert te herstellen op hetzelfde apparaat, zorgt u ervoor dat er voldoende ruimte op het apparaat om deze bewerking te voltooien. Nadat u 32 TB hebt overschreden, kunt u niet terugzetten op hetzelfde apparaat. |
| Maximum aantal opslagaccountreferenties per apparaat |1 | |
| Maximale aantal volumes/shares |16 | |
| Minimale grootte van een gelaagde share |500 GB | |
| Minimale grootte van een gelaagd volume |500 GB | |
| Maximale grootte van een gelaagde share |20 TB | |
| Maximale grootte van een gelaagd volume |5 TB | |
| Minimale grootte van een lokaal vastgemaakt share |50 GB | |
| Minimale grootte van een lokaal vastgemaakt volume |50 GB | |
| Maximale grootte van een lokaal vastgemaakt share |2 TB | |
| Maximale grootte van een lokaal vastgemaakt volume |200 GB | |
| Maximum aantal verbindingen iSCSI-initiators |512 | |
| Maximum aantal access control records per apparaat |64 | |
| Maximum aantal back-ups moeten worden behouden door het virtuele apparaat in *.backups* map voor de bestandsserver |5 |Dit omvat de meest recente geplande (gegenereerd door het standaard back-upbeleid) en handmatige back-ups. |
| Maximum aantal geplande back-ups moeten worden behouden door het apparaat |55 |30 dagelijkse back-ups<br>12 maandelijkse back-ups<br>13 jaarlijkse back-ups |
| Maximum aantal handmatige back-ups moeten worden behouden door het apparaat |45 | |
| Maximum aantal bestanden per share voor een bestandsserver |1 miljoen |Bij het uitvoeren van een apparaat herstellen, zijn de hersteltijden in verhouding met het aantal bestanden in alle shares op het apparaat. |
| Maximum aantal bestanden per volume voor een iSCSI-server |1 miljoen | |
| Maximum aantal bestanden per virtuele matrix |4 miljoen | |
| Herstellen van de tijd herstellen |Snel herstellen |Het herstel is gebaseerd op de heatmap en is afhankelijk van de volumegrootte.<br>Back-upbewerkingen kunnen optreden wanneer een herstelbewerking uitgevoerd wordt. |

