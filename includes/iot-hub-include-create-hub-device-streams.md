---
title: include-bestand (preview apparaatstreams)
description: include-bestand (preview apparaatstreams)
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/14/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a02c16a75bbdf4827ce6a6c5f50751caf9c5aab8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445987"
---
In deze sectie wordt beschreven hoe u een IoT-hub maken met behulp van de [Azure-portal](https://portal.azure.com).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **een resource maken**, en selecteer vervolgens **Internet of Things**.

1. Selecteer in de lijst aan de rechterkant, **Iot-Hub**. De eerste pagina voor het maken van een IoT-hub wordt geopend.

   ![Het maken van een IoT-hub in Azure portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-01.png)

   Vul de velden in:

   a. In de **abonnement** vervolgkeuzelijst, selecteert u het abonnement moet worden gebruikt voor uw IoT-hub.

   b. Voor **resourcegroep**, een van de volgende handelingen uit: 
      * Voor het maken van een nieuwe resourcegroep selecteert **nieuw** en voer de naam die u wilt gebruiken. 
      * Voor het gebruik van een bestaande resourcegroep selecteren **gebruik bestaande** en selecteer vervolgens de resourcegroep in de vervolgkeuzelijst. 
      
        Zie [Azure Resource Manager-resourcegroepen beheren](../articles/azure-resource-manager/manage-resource-groups-portal.md) voor meer informatie.

   c. In de **regio** vervolgkeuzelijst, selecteert u de regio waarin u wilt dat uw hub te bevinden. Selecteer een regio die ondersteuning biedt voor de preview van IoT Hub apparaat-streams, ofwel **VS-midden** of **centrale EUAP ons**.

   d. In de **naam IoT-Hub** voert u de naam voor uw IoT-hub. De naam moet wereldwijd uniek zijn. Als de door u opgegeven naam beschikbaar is, verschijnt er een groen vinkje.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

1. Als u wilt doorgaan met het maken van uw IoT-hub, selecteer **volgende: Grootte en schaalgrootte**.

   ![Instellen van de grootte en schaalgrootte voor een nieuwe IoT-hub met behulp van de Azure portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-02.png)

   In dit deelvenster kunt u de standaardinstellingen accepteren en selecteer **revisie + maken** aan de onderkant. Houd rekening met de volgende opties:

   * In de **prijs- en schaalcategorie** vervolgkeuzelijst, selecteer een van de standaard-laag (**S1**, **S2**, of **S3**) of **F1: Gratis laag**. Deze keuze kan ook worden geleid door de grootte van uw vliegtuigen en de niet-streaming-werkbelastingen die u in de hub (bijvoorbeeld berichten over telemetrie verwacht). Zo is de gratis laag bedoeld voor testen en evalueren. Hiermee kunnen 500 apparaten met de IoT-hub worden verbonden en maximaal 8000 berichten per dag verzonden. Elk Azure-abonnement kunt maken van een IoT-hub in de gratis laag. 

   * Voor **nummer van de IoT Hub-eenheden**: Deze keuze is afhankelijk van niet-streaming werkbelasting die u in de hub verwacht. Nu kunt u 1.

   Zie voor meer informatie over opties voor laag [de juiste IoT hub-laag kiezen](../articles/iot-hub/iot-hub-scaling.md).

1. Als u wilt uw keuzes controleren, selecteert u de **bekijken + maken** tabblad. Het deelvenster dat wordt geopend is vergelijkbaar met het volgende:

   ![Informatie over het maken van de nieuwe IoT hub](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-03.png)

1. Voor het maken van uw nieuwe IoT-hub, selecteer **maken**. Het proces duurt enkele minuten.
