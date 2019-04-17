---
title: Genereren van een apparaat-verbindingsreeks voor Azure IoT Central | Microsoft Docs
description: Als de ontwikkelaar van een apparaat, hoe ik genereren een verbindingsreeks voor apparaten die verbinding maken met een IoT Central-toepassing moet?
author: dominicbetts
ms.author: dobett
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f302cbfa7152ae30be434f560c0c39056d40f9f4
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615758"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Genereren van een apparaatverbindingsreeks verbinding maakt met een Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe u als ontwikkelaar apparaat voor het genereren van een verbindingsreeks voor een apparaat dat u moet verbinding maken met een IoT Central-toepassing. De procedure in dit artikel wordt beschreven hoe u snel verbinding maken met een enkel apparaat met behulp van een shared access signature (SAS). Deze methode is nuttig wanneer u experimenteren met IoT Central of testen van apparaten. Zie voor alternatieve methoden voor het gebruik in een productieomgeving [verbindingsmogelijkheden voor apparaten in Azure IoT Central](concepts-connectivity.md).

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

- Een Azure IoT Central-toepassing. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
- Een ontwikkelcomputer met [Node.js](https://nodejs.org/) versie 8.0.0 of hoger is geïnstalleerd. U kunt uitvoeren `node --version` vanaf de opdrachtregel om uw versie te controleren. Node.js is beschikbaar voor een groot aantal verschillende besturingssystemen.

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

De volgende stappen wordt beschreven hoe u aan de informatie die u nodig hebt voor het genereren van een SAS-verbindingsreeks voor een apparaat:

1. In de **Device Explorer**, het echte apparaat dat u wilt verbinding maken met uw toepassing niet vinden:

    ![Selecteer een echt apparaat](media/howto-generate-connection-string/real-devices.png)

1. Op de **apparaat** weergeeft, schakelt **Connect**:

    ![Selecteer verbinding maken](media/howto-generate-connection-string/connect.png)

1. Noteer de details van de verbinding, **bereik-ID**, **apparaat-ID**, en **apparaat primaire sleutel**moeten worden gebruikt in de volgende stappen uit:

    ![Verbindingsdetails](media/howto-generate-connection-string/device-connect.png)

    U kunt de waarden kopiëren vanaf deze pagina om op te slaan.

## <a name="generate-the-connection-string"></a>De verbindingsreeks genereren

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een verbindingsreeks voor een echt apparaat verbinden met uw Azure IoT Central-toepassing hebt gegenereerd, worden hier de voorgestelde volgende stappen:

* [Voorbereiden en verbinding maken met een apparaat DevKit (C)](howto-connect-devkit.md)
* [Voorbereiden en verbinding maken met een Raspberry Pi (Python)](howto-connect-raspberry-pi-python.md)
* [Voorbereiden en verbinding maken met een Raspberry Pi (C#)](howto-connect-raspberry-pi-csharp.md)
* [Voorbereiden en verbinding maken met een Windows 10 IoT core-apparaat (C#)](howto-connect-windowsiotcore.md)
* [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs.md)