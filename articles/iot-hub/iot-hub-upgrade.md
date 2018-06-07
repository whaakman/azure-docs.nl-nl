---
title: Upgrade van Azure IoT Hub | Microsoft Docs
description: Wijzig de prijs- en schaalniveau laag voor IoT Hub meer mogelijkheden voor messaging en apparaat ophalen.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: 472115f166adc5385b6f46b2f3ac5ef75a6cde92
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34637273"
---
# <a name="how-to-upgrade-your-iot-hub"></a>Het bijwerken van uw IoT-hub

Wanneer uw IoT-oplossing groeit, is Azure IoT Hub is klaar om te helpen u opschalen. Azure IoT Hub biedt twee lagen, (B) basic en standard (S) voor klanten die wilt verschillende functies gebruiken. Binnen elke laag zijn drie grootten (1, 2 en 3) om te bepalen het aantal berichten dat per dag kan worden verzonden. 

Wanneer u meer mogelijkheden hebt en meer apparaten hebben, zijn er drie manieren om aan te passen aan uw behoeften van uw IoT-hub:

* Eenheden binnen de IoT-hub toevoegen. Elke aanvullende eenheid in een B1 IoT hub kan bijvoorbeeld op een extra 400.000 berichten per dag. 
* Wijzig de grootte van de IoT-hub. Bijvoorbeeld, migreren van de laag B1 aan de laag B2 naar vergroot de hoeveelheid berichten die per eenheid per dag kan ondersteunen.
* Upgrade uitvoeren naar een hogere laag. Bijvoorbeeld een upgrade van de laag B1 naar de laag S1 voor dezelfde capaciteit messaging, maar de geavanceerde functies die in de prijscategorie standard komen.

Deze wijzigingen kunnen optreden zonder dat bestaande bewerkingen worden onderbroken.

Als u verlagen van uw IoT-hub wilt, kunt u eenheden verwijderen en Reduceer de grootte van de IoT-hub. U kunt niet echter downgraden met een lagere tier. U kunt bijvoorbeeld uit de laag S2 aan de laag S1, maar niet op de laag S2 aan de laag B1 verplaatsen. 

Deze voorbeelden zijn bedoeld om te begrijpen hoe het aanpassen van uw IoT-hub als uw oplossing voor wijzigingen. Voor specifieke informatie over de mogelijkheden van elke laag u altijd moet verwijzen naar [prijzen van Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Upgrade van uw bestaande IoT-hub 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar uw IoT-hub. 
2. Selecteer **prijs- en schaalcategorie**. 

   ![Prijzen en schaal](./media/iot-hub-upgrade/pricing-scale.png)

3. Als u de laag voor uw hub, selecteert **prijs-en schaalniveau**. De nieuwe laag kiezen en klik vervolgens op **Selecteer**.

   ![Prijzen en schaal](./media/iot-hub-upgrade/select-tier.png)

4. Als u wilt wijzigen van het aantal eenheden in uw hub, voert u een nieuwe waarde onder **IoT Hub-eenheden**. 
5. Selecteer **opslaan** uw wijzigingen op te slaan. 

Uw IoT-hub is nu aangepast en uw configuraties zijn niet gewijzigd. 

## <a name="next-steps"></a>Volgende stappen

Kunt u meer informatie over [het kiezen van de juiste laag van de IoT Hub](iot-hub-scaling.md). 

