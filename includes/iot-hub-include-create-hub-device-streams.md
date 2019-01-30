---
title: include-bestand (preview apparaatstreams)
description: include-bestand (preview apparaatstreams)
author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 01/15/2019
ms.author: rezas
ms.custom: include file
ms.openlocfilehash: 53d8df7e2366cfbf2f62e79fc99c8ef2f9b48ba1
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830809"
---
In deze sectie wordt beschreven hoe u een IoT-hub maakt met behulp van de [Azure-portal](https://portal.azure.com).

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Kies +**Een resource maken** en vervolgens **Internet of Things**.

3. Klik in de lijst rechts op **IoT Hub**. U krijgt het eerste scherm te zien om een IoT-hub te maken.

   ![Schermopname waarin het maken van een hub in de Azure-portal wordt getoond](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-1.png)

   Vul de velden in:

   **Abonnement**: Selecteer het abonnement dat u voor de IoT-hub wilt gebruiken.

   **Resourcegroep**: Maak een nieuwe resourcegroep of gebruik een bestaande. Als u een nieuwe wilt maken, klikt u op **Nieuwe maken** en vult u de gewenste naam in. Als u een bestaande resourcegroep wilt gebruiken, klikt u op **Bestaande gebruiken** en selecteert u de resourcegroep in de vervolgkeuzelijst. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../articles/azure-resource-manager/resource-group-portal.md) voor meer informatie.

   **Regio**: Dit is de regio voor de locatie van uw hub. Zorg dat u een ondersteunde regio selecteert (bijvoorbeeld US - centraal of US - centraal EUAP).

   **Naam van de IoT Hub**: Vul de naam voor de IoT Hub in. Deze naam moet wereldwijd uniek zijn. Als de door u opgegeven naam beschikbaar is, verschijnt er een groen vinkje.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

4. Klik op **Volgende: grootte en schaal** om verder te gaan met het maken van uw IoT-hub.

   ![Schermafbeelding waarop het instellen van de grootte en schaal voor een nieuwe IoT-hub in de Azure-portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-2-free.png)


   Op dit scherm kunt u de standaardwaarden gebruiken en onderaan op **Controleren en maken** klikken. 

   **Prijs- en schaalniveau**: zorg dat u een standaardlaag (S1, S2 en S3) of gratis laag (F1) selecteert. Deze keuze kan ook worden bepaald door de grootte van uw fleet en de niet-streaming workloads die u in de hub verwacht (zoals telemetrieberichten). Zo is de gratis laag bedoeld voor testen en evalueren. Hiermee kunnen 500 apparaten met de IoT-hub worden verbonden en maximaal 8000 berichten per dag verzonden. Met de gratis optie kunt u voor elk Azure-abonnement één IoT Hub maken. 

   **IoT Hub-eenheden**: Deze keuze is afhankelijk van niet-streaming workloads die u in uw hub verwacht. Nu kunt u 1 selecteren.

   Zie [De juiste laag kiezen voor uw IoT-hub](../articles/iot-hub/iot-hub-scaling.md) voor informatie over andere opties.

5. Klik op **Controleren en maken** om uw keuzes te beoordelen. U krijgt iets te zien wat lijkt op dit scherm.

   ![Schermafbeelding met het beoordelen van informatie voor het maken van de nieuwe IoT-hub](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-3-free.png)

6. Klik op **Maken** om de nieuwe IoT-hub te maken. De hub wordt binnen enkele minuten gemaakt.
