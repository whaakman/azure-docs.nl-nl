---
title: Meer informatie over Azure Security Center voor IoT-kosten | Microsoft Docs
description: Meer informatie over de kosten van Azure Security Center voor IoT en hoe u deze kunt beheren.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ef839708-4574-4a40-bc45-07005f8e9daf
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 603df1def011232ad2120c37ad1ba256f2a30526
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596514"
---
# <a name="pricing-and-associated-costs"></a>Prijs en bijbehorende kosten

In dit artikel wordt uitgelegd Azure Security Center voor IoT-prijs model, worden alle gekoppelde kosten samenvatten en wordt uitgelegd hoe u deze kunt beheren.

## <a name="pricing"></a>Prijzen

Het prijs model Azure Security Center voor IoT bestaat uit twee delen en wordt gefactureerd wanneer een IoT Hub is [ingeschakeld](quickstart-onboard-iot-hub.md) in azure Security Center voor IOT:

- Kosten per apparaat-ingebouwde beveiligings mogelijkheden op basis van de analyse van IoT Hub Logboeken.

- Kosten per bericht: Verbeterde beveiligings mogelijkheden op basis van beveiligings berichten van IoT Edge of blad apparaten.

  >[!Note]
  > Beveiligings berichten ontstaan ook het quotum verbruik op IoT Hub.

Zie [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/)voor meer informatie.

## <a name="associated-costs"></a>Gekoppelde kosten

Azure Security Center voor IoT heeft twee soorten gekoppelde kosten, die geen deel uitmaken van de prijzen voor direct:

- Quota verbruik IoT Hub

- Opslag kosten Log Analytics

U kunt de gekoppelde kosten verlagen door de instellingen van bepaalde functies te wijzigen.

Uw instellingen wijzigen:

1. Open IoT Hub.

2. Klik onder **beveiliging**op **overzicht**.

3. Klik op **Instellingen**.

De volgende tabel bevat een samen vatting van de bijbehorende kosten en implicaties van elke optie.

|     | Gebruik | Opmerking |
| --- | --- | --- |
| **Quota verbruik IoT Hub** |  |
| Taak voor het exporteren van een [apparaat](https://docs.microsoft.com/azure/iot-hub/iot-hub-bulk-identity-mgmt#export-devices) (dubbele export) | Eenmaal per dag | _Verzameling van dubbele meta gegevens_ uitschakelen |
| **Opslag Log Analytics** |  |
| Aanbeveling en waarschuwingen van het apparaat| Beveiligings aanbeveling en waarschuwingen die worden gegenereerd door de service | Niet optioneel |
| Onbewerkte beveiligings gegevens| Onbewerkte beveiligings gegevens van IoT-apparaten, verzameld door beveiligings agenten | _Beveiligings gebeurtenissen_ voor het opslaan van onbewerkte apparaten uitschakelen |

>[!Important]
> Uitbellen heeft ernstige gevolgen voor de beschik bare beveiligings functies.
  
| Afmelden | Gevolgen |
| --- | --- |
| _Verzameling van dubbele meta gegevens_ | [Aangepaste waarschuwingen](quickstart-create-custom-alerts.md) uitschakelen |
| | Aanbevelingen van IoT Edge-manifest uitschakelen |
| | Aanbevelingen en waarschuwingen op basis van apparaat-id's uitschakelen |
| _Beveiligings gebeurtenissen voor onbewerkte apparaten opslaan_ | Details van de algemene aanbevelingen voor het besturings systeem van het apparaat zijn niet beschikbaar |
| | Er zijn geen gegevens over [waarschuwingen](concept-security-alerts.md) en [aanbevelings](concept-recommendations.md) onderzoeken beschikbaar |
|


## <a name="see-also"></a>Zie ook

- Toegang tot uw onbewerkte [beveiligings gegevens](how-to-security-data-access.md)
- [Een apparaat onderzoeken](how-to-investigate-device.md)
- [Beveiligings aanbevelingen](concept-recommendations.md) begrijpen en verkennen
- [Beveiligings waarschuwingen](concept-security-alerts.md) begrijpen en verkennen
