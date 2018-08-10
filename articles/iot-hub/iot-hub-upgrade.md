---
title: Upgrade van Azure IoT Hub | Microsoft Docs
description: De prijs- en schaalniveau voor IoT Hub kunt u meer mogelijkheden voor berichten als Apparaatbeheer wijzigen.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: e1342ed574d84ed5b4edd5060c2d6d3ec8bca1a8
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003108"
---
# <a name="how-to-upgrade-your-iot-hub"></a>Upgrade uitvoeren van uw IoT-hub

Als uw IoT-oplossing groeit, is Azure IoT Hub is klaar om te helpen u omhoog schalen. Azure IoT Hub biedt twee lagen, (B) basic en standard (S), voor klanten die u wilt de verschillende functies gebruiken. Binnen elke laag zijn drie grootten (1, 2 en 3) om te bepalen het aantal berichten dat elke dag kan worden verzonden. 

Wanneer u meer apparaten en meer mogelijkheden nodig hebben, zijn er drie manieren om aan te passen van uw IoT-hub aan de behoeften van uw behoeften:

* Eenheden in de IoT-hub toevoegen. Elke extra eenheid in een B1 IoT-hub kan bijvoorbeeld op een extra 400.000 berichten per dag. 
* Wijzig de grootte van de IoT-hub. Bijvoorbeeld, migreren van de laag B1 naar de laag B2 en vergroot de opslagmogelijkheid voor berichten die per eenheid per dag kan ondersteunen.
* Upgrade uitvoeren naar een hogere laag. Bijvoorbeeld: upgraden van de laag B1 naar de S1-laag voor dezelfde capaciteit messaging, maar met de geavanceerde functies die worden geleverd in de laag standaard.

Deze wijzigingen kunnen optreden zonder dat bestaande bewerkingen wordt onderbroken.

Als u downgraden van uw IoT-hub wilt, kunt u eenheden verwijderen en Reduceer de grootte van de IoT-hub. U kunt geen echter downgraden naar een lagere laag. U kunt bijvoorbeeld verplaatsen van de S2-laag naar de S1-laag, maar niet vanaf de S2-laag naar de laag B1. Let ook op dat slechts één type [edition](https://azure.microsoft.com/pricing/details/iot-hub/) binnen een laag per IoT Hub kan worden gekozen. U kunt bijvoorbeeld een IoT-Hub maken met meerdere eenheden van S1, maar niet met een combinatie van eenheden van verschillende versies, zoals S1 en B3, of S1 en S2.

Deze voorbeelden zijn bedoeld om te begrijpen hoe u uw IoT-hub als de oplossing wijzigingen aanpast. Voor specifieke informatie over de mogelijkheden van elke laag u moet altijd verwijzen naar [prijzen van Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Upgrade van uw bestaande IoT-hub 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar uw IoT-hub. 
2. Selecteer **prijs- en schaalniveau**. 

   ![Prijzen en schaal](./media/iot-hub-upgrade/pricing-scale.png)

3. Als u wilt wijzigen van de laag voor uw hub, selecteer **prijs- en schaalcategorie**. De nieuwe laag kiezen en klik vervolgens op **Selecteer**.

   ![Prijzen en schaal](./media/iot-hub-upgrade/select-tier.png)

4. Als u wilt wijzigen van het aantal eenheden in uw hub, voer een nieuwe waarde onder **IoT Hub-eenheden**. 
5. Selecteer **opslaan** uw wijzigingen op te slaan. 

Uw IoT-hub is nu aangepast en de configuraties zijn niet gewijzigd. Houd er rekening mee dat de maximale partitielimiet voor basic-laag IoT Hub 8 is en voor de standard-laag 32 is. De meeste IoT-Hubs hoeft slechts 4 partities. De limiet van de partitie wordt gekozen bij IoT Hub wordt gemaakt en is gekoppeld aan de apparaat-naar-cloud-berichten naar het aantal gelijktijdige lezers van deze berichten. Deze waarde blijft ongewijzigd wanneer u van basic-laag naar de standard-laag migreert. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie krijgen over [u bij het kiezen van de juiste IoT Hub-laag](iot-hub-scaling.md). 

