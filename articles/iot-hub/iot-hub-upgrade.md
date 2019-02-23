---
title: Upgrade van Azure IoT Hub | Microsoft Docs
description: De prijs- en schaalniveau voor IoT Hub kunt u meer mogelijkheden voor berichten als Apparaatbeheer wijzigen.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: 6f6cf6de9b1f12451bf28a9bd7fc7077c1c6b1cd
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673450"
---
# <a name="how-to-upgrade-your-iot-hub"></a>Upgrade uitvoeren van uw IoT-hub

Als uw IoT-oplossing groeit, is Azure IoT Hub is klaar om te helpen u omhoog schalen. Azure IoT Hub biedt twee lagen, (B) basic en standard (S), voor klanten die u wilt de verschillende functies gebruiken. Binnen elke laag zijn drie grootten (1, 2 en 3) om te bepalen het aantal berichten dat elke dag kan worden verzonden. 

Wanneer u meer apparaten en meer mogelijkheden nodig hebben, zijn er drie manieren om aan te passen van uw IoT-hub aan de behoeften van uw behoeften:

* Eenheden in de IoT-hub toevoegen. Elke extra eenheid in een B1 IoT-hub kan bijvoorbeeld op een extra 400.000 berichten per dag. 
* Wijzig de grootte van de IoT-hub. Bijvoorbeeld, migreren van de laag B1 naar de laag B2 te verhogen van het aantal berichten dat per eenheid per dag kan worden ondersteund.
* Upgrade uitvoeren naar een hogere laag. Bijvoorbeeld een upgrade van de laag B1 naar de prijscategorie S1 voor toegang tot geavanceerde functies met dezelfde messaging capaciteit.

Deze wijzigingen kunnen optreden zonder dat bestaande bewerkingen wordt onderbroken.

Als u downgraden van uw IoT-hub wilt, kunt u eenheden verwijderen en Reduceer de grootte van de IoT-hub, maar u kunt niet downgraden naar een lagere laag. U kunt bijvoorbeeld verplaatsen van de S2-laag naar de S1-laag, maar niet vanaf de S2-laag naar de laag B1. Slechts één type [edition](https://azure.microsoft.com/pricing/details/iot-hub/) binnen een laag per IoT Hub kan worden gekozen. U kunt bijvoorbeeld een IoT-Hub maken met meerdere eenheden van S1, maar niet met een combinatie van eenheden van verschillende versies, zoals S1 en B3, of S1 en S2.

Deze voorbeelden zijn bedoeld om te begrijpen hoe u uw IoT-hub als de oplossing wijzigingen aanpast. Voor specifieke informatie over de mogelijkheden van elke laag, u moet altijd verwijzen naar [prijzen van Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Upgrade van uw bestaande IoT-hub 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar uw IoT-hub. 
2. Selecteer **prijs- en schaalniveau**. 

   ![Prijzen en schaal](./media/iot-hub-upgrade/pricing-scale.png)

3. Als u wilt wijzigen van de laag voor uw hub, selecteer **prijs- en schaalcategorie**. De nieuwe laag kiezen en klik vervolgens op **Selecteer**.

   ![Prijs- en schaalniveau](./media/iot-hub-upgrade/select-tier.png)

4. Als u wilt wijzigen van het aantal eenheden in uw hub, voer een nieuwe waarde onder **IoT Hub-eenheden**. 
5. Selecteer **opslaan** uw wijzigingen op te slaan. 

Uw IoT-hub is nu aangepast en de configuraties zijn niet gewijzigd. De maximale partitielimiet voor basic-laag IoT Hub is 8 en voor standard-laag is 32. De meeste IoT-Hubs hoeft slechts vier partities. De limiet van de partitie wordt gekozen bij IoT Hub wordt gemaakt en is gekoppeld aan de apparaat-naar-cloud-berichten naar het aantal gelijktijdige lezers van deze berichten. Deze waarde blijft ongewijzigd wanneer u van basic-laag naar de standard-laag migreert. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie krijgen over [u bij het kiezen van de juiste IoT Hub-laag](iot-hub-scaling.md). 

