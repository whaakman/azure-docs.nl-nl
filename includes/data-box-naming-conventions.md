---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: d965b89659a9e3dc01035221a729f094c336eb5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244640"
---
| Entiteit                                       | Conventies   |
|----------------------------------------------|-----------------------------------------------------|
| Namen van containers voor blok-blobs en pagina-blobs | Moet een geldige DNS-naam die tussen de 3 en 63 tekens lang zijn. <br>  Moet beginnen met een letter of cijfer. <br> Mag alleen kleine letters, cijfers en afbreekstreepjes (-). <br> Elk koppelteken (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. <br> Opeenvolgende streepjes zijn niet toegestaan in namen. |
| De namen van shares voor Azure files                  | Hetzelfde als hierboven                                          |
| Directory- en bestandsnamen voor Azure files     |<li> Aanvraag te behouden, niet-hoofdlettergevoelige en mag niet groter zijn dan 255 tekens. </li><li> Mag niet eindigen met de schuine streep (/). </li><li>Indien opgegeven, worden deze automatisch verwijderd. </li><li> Volgende tekens zijn niet toegestaan: <code>" \\ / : \| < > * ?</code></li><li> Gereserveerde tekens voor URL's moeten op de juiste wijze van een escape-teken zijn voorzien. </li><li> Ongeldige tekens voor URL-pad zijn niet toegestaan. Code-punten zoals \\uE000 zijn niet geldig Unicode-tekens. Sommige ASCII of Unicode-tekens, zoals stuurcodes (0x00-0x1F \\u0081, enzovoort), zijn ook niet toegestaan. Tekenreeksen in HTTP/1.1 Zie RFC 2616, sectie 2.2 voor regels voor Unicode: Eenvoudige regels en RFC 3987. </li><li> Volgende bestandsnamen zijn niet toegestaan: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, punt-teken (.), en twee punten tekens (.).</li>|
| Blobnamen voor blok-blob en pagina-blob      | </li><li>Blobnamen zijn hoofdlettergevoelig en kunnen elke combinatie van tekens bevatten. </li><li>Een blobnaam moet 1 tot 1024 tekens bevatten. </li><li>Gereserveerde tekens voor URL's moeten op de juiste wijze van een escape-teken zijn voorzien. </li><li>Het aantal padsegmenten dat de blobnaam omvat, mag niet meer dan 254 zijn. Een padsegment is de tekenreeks tussen opeenvolgende scheidingstekens (bijvoorbeeld de slash '/') die overeenkomt met de naam van een virtuele map.</li> |
