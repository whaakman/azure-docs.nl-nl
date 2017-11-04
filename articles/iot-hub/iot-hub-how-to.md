---
title: Azure IoT Hub hoe | Microsoft Docs
description: Als een ontwikkelaar hoe gebruik ik de verschillende functies van de IoT Hub?
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: dobett
ms.openlocfilehash: b451ea2d42f0ff2f64746bcb296fb9128472fea8
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="how-to-use-azure-iot-hub"></a>Het gebruik van Azure IoT Hub

U beschikt over verschillende opties voor informatie over het ontwikkelen voor de service IoT-Hub:

* Lees de conceptuele artikelen die de functies van IoT Hub in detail beschrijven.
* Voer een van de zelfstudies die betrekking hebben op de verschillende functies van IoT-Hub.

## <a name="developer-guide"></a>Ontwikkelaarsgids

Als een ontwikkelaar vindt u gedetailleerde conceptuele informatie over IoT-Hub in de [Developer Guide][lnk-devguide]. Deze handleiding bevat:

* Gedetailleerde beschrijvingen van de IoT Hub-functies waarmee u kunt voor meer informatie over het gebruik ervan.
* Instructies over het kiezen wanneer meerdere opties beschikbaar zijn.

## <a name="tutorials"></a>Zelfstudies

Als u liever voor meer informatie over specifieke functies van de IoT Hub praktijkoefeningen doorloopt, zijn er verschillende zelfstudies om uit te kiezen. Veel van deze zelfstudies zijn beschikbaar in meerdere programmeertalen. Deze zelfstudies zijn onder andere:

- [IoT Hub apparaat-naar-cloud-berichten met behulp van routes verwerken][lnk-routes-tutorial]. Deze zelfstudie laat zien hoe u regels voor het doorsturen van IoT-Hub aan de verzending van apparaat-naar-cloud-berichten in een eenvoudig en gebaseerd op configuratie-hulpprogramma gebruiken.

- [Cloud-naar-apparaat-berichten verzenden met IoT Hub][lnk-c2d-tutorial]. Deze zelfstudie laat zien hoe u berichten via IoT Hub cloud naar apparaat verzenden en ontvangen van berichten van de cloud-naar-apparaat op een apparaat.

- [Uploaden van bestanden van apparaten naar de cloud met IoT Hub][lnk-upload-tutorial]. Deze zelfstudie laat zien hoe u de mogelijkheden voor het uploaden van bestand van IoT-Hub.

- [Aan de slag met apparaat horende][lnk-twin-tutorial]. Deze zelfstudie maakt u kennis met apparaat horende, gemelde eigenschappen gewenste eigenschappen en labels. Apparaat horende kunt u waarden synchroniseren met uw apparaten.

- [Directe methoden gebruiken][lnk-methods-tutorial]. Deze zelfstudie laat zien hoe u directe methoden te gebruiken. U voegt een handler voor een directe methode in uw gesimuleerde apparaat en de directe methode uit IoT Hub worden aangeroepen.

- [Aan de slag met Apparaatbeheer][lnk-dm-tutorial]. Deze zelfstudie laat zien hoe u de beheerfuncties van de belangrijkste apparaten zoals horende en rechtstreekse methoden. Deze functies kunt u op afstand opnieuw opstarten van uw gesimuleerde apparaat.

- [Gewenste eigenschappen gebruiken om apparaten te configureren][lnk-properties-tutorial]. Deze zelfstudie ziet dat u hoe het apparaat kunnen gebruiken van twin gewenst en gerapporteerd eigenschappen, tot op afstand configureert uw apparaat.

- [Apparaattaken gebruiken om te zetten van een apparaat firmware-update][lnk-jobs-tutorial]. Deze zelfstudie laat zien hoe u de beheerfuncties van de belangrijkste apparaten zoals horende en rechtstreekse methoden. U informatie over het gebruik van deze functies op afstand van uw apparaat firmware bijwerken.

- [Planning en broadcast taken][lnk-schedule-tutorial]. Deze zelfstudie laat zien hoe u de gewenste eigenschappen en direct methoden gebruiken om te communiceren met meerdere apparaten op een gepland tijdstip.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de IoT Hub-service, de [Developer Guide][lnk-devguide].

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-routes-tutorial]: ./iot-hub-csharp-csharp-process-d2c.md
[lnk-c2d-tutorial]: ./iot-hub-csharp-csharp-c2d.md
[lnk-upload-tutorial]: ./iot-hub-csharp-csharp-file-upload.md
[lnk-twin-tutorial]: ./iot-hub-node-node-twin-getstarted.md
[lnk-methods-tutorial]: ./iot-hub-node-node-direct-methods.md
[lnk-dm-tutorial]: ./iot-hub-node-node-device-management-get-started.md
[lnk-properties-tutorial]: ./iot-hub-node-node-twin-how-to-configure.md
[lnk-jobs-tutorial]: ./iot-hub-node-node-firmware-update.md
[lnk-schedule-tutorial]: ./iot-hub-node-node-schedule-jobs.md