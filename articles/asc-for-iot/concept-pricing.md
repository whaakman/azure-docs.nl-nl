---
title: Inzicht in Azure Security Center voor IoT kosten Preview | Microsoft Docs
description: Meer informatie over de kosten die zijn gekoppeld aan Azure Security Center voor IoT, en hoe u ze kunt beheren.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: ef839708-4574-4a40-bc45-07005f8e9daf
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: e2822b0f2f665220f215b85800081fa2fa7943f9
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758041"
---
# <a name="pricing-and-associated-costs"></a>Prijs en bijbehorende kosten

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel wordt uitgelegd van Azure Security Center (ASC) voor IoT-prijsmodel, bevat een overzicht van alle bijbehorende kosten en wordt uitgelegd hoe u ze kunt beheren.

## <a name="pricing"></a>Prijzen

De ASC voor IoT prijsmodel uit twee bestaat delen en wordt in rekening gebracht zodra een IoT-Hub is [ingeschakeld](quickstart-onboard-iot-hub.md) in ASC voor IoT:

- Kosten per apparaat - mogelijkheden voor ingebouwde beveiliging op basis van de analyse van Logboeken van de IoT Hub.

- Kosten per bericht - mogelijkheden voor verbeterde beveiliging op basis van beveiligingsberichten van IoT Edge of leaf-apparaten.

  >[!Note]
  > Beveiligingsberichten wordt ook quotumverbruik op IoT-Hub in rekening gebracht.

Zie voor meer informatie, [prijzen van Security Center](https://azure.microsoft.com/en-us/pricing/details/security-center/).

## <a name="associated-costs"></a>Bijbehorende kosten

ASC voor IoT heeft twee soorten bijbehorende kosten, die geen deel uitmaken van de directe prijzen:

- IoT Hub quotumverbruik

- Meld u Analytics-opslagkosten

U kunt de bijbehorende kosten verlagen door wanneer u geen gebruik van bepaalde functies door de instellingen wijzigen.

De instellingen te wijzigen:

1. Open IoT Hub.

2. Onder **Security**, klikt u op **overzicht**.

3. Klik op **Instellingen**.

De volgende tabel bevat een overzicht van de bijbehorende kosten en de implicaties van elke optie.

|     | Gebruik | Opmerking |
| --- | --- | --- |
| **IoT Hub quotumverbruik** |  |
| [Exporteren van apparaat](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-bulk-identity-mgmt#export-devices) taak (dubbele exporteren) | Eenmaal per dag | Uitschakelen _metagegevensverzameling twin_ |
| **Log Analytics-opslag** |  |
| Apparaat aanbevelingen en waarschuwingen| Beveiligingsaanbeveling en waarschuwingen die zijn gegenereerd door de service | Niet optioneel |
| Onbewerkte beveiligingsgegevens| Onbewerkte beveiligingsgegevens van IoT-apparaten die worden verzameld door beveiliging-agents | Uitschakelen _beveiligingsgebeurtenissen van niet-gecodeerd apparaat opslaan_ |

>[!Important]
> Uitschrijft, heeft ernstige gevolgen voor beschikbare beveiligingsfuncties.
  
| Afmelden | Gevolgen |
| --- | --- |
| _Dubbele metagegevensverzameling_ | Uitschakelen [aangepaste waarschuwingen](quickstart-create-custom-alerts.md) |
| | IoT Edge manifest aanbevelingen uitschakelen |
| | Apparaat-id op basis van aanbevelingen en waarschuwingen uitschakelen |
| _Store-gecodeerd apparaat beveiligingsgebeurtenissen_ | Meer informatie over apparaat OS algemene aanbevelingen zijn niet beschikbaar |
| | Informatie over [waarschuwing](concept-security-alerts.md) en [aanbeveling](concept-recommendations.md) onderzoeken zijn niet beschikbaar |


## <a name="see-also"></a>Zie ook

- Toegang tot uw [onbewerkte beveiligingsgegevens](how-to-security-data-access.md)
- [Een apparaat onderzoeken](how-to-investigate-device.md)
- Begrijpen en verkennen [aanbevelingen voor beveiliging](concept-recommendations.md)
- Begrijpen en verkennen [beveiligingswaarschuwingen](concept-security-alerts.md)
