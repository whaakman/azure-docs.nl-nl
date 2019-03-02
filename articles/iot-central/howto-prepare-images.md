---
title: Afbeeldingen uploaden naar uw Azure IoT Central-toepassing | Microsoft Docs
description: Als een opbouwfunctie voor expressies, informatie over het voorbereiden en afbeeldingen uploaden naar uw Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 10/31/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f0e4d1badd0ed992c96f6624813e575d0f286d8b
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57216795"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Voorbereiden en afbeeldingen uploaden naar uw Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe, als een opbouwfunctie voor expressies, kunt u uw Azure IoT Central-toepassing uploaden van aangepaste installatiekopieën. U kunt bijvoorbeeld een apparaatdashboard met een afbeelding van het apparaat.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT Central-toepassing. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
1. Een hulpprogramma voor het schalen en het formaat van afbeeldingsbestanden.

## <a name="choose-where-to-use-custom-images"></a>Kies waar u aangepaste installatiekopieën kunt gebruiken

U kunt aangepaste installatiekopieën toevoegen aan de volgende locaties in een Azure IoT Central-toepassing:

* De **Toepassingsbeheer** pagina

    ![Een installatiekopie op de pagina voor het beheer van toepassingen](media/howto-prepare-images/applicationmanager.png)

* Het dashboard

    ![Afbeelding in toepassingsdashboard](media/howto-prepare-images/homepage.png)

* Een apparaat-sjabloon

    ![Afbeelding van apparaat-sjabloon](media/howto-prepare-images/devicetemplate.png)

* Een tegel op een apparaatdashboard

    ![Een installatiekopie op de tegel van apparaat](media/howto-prepare-images/devicetile.png)

* Dashboard een tegel op een apparaat instellen

    ![Afbeelding op de tegel van apparaat instellen](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>De installatiekopieën voorbereiden

Op alle vier locaties, kunt u PNG-, GIF- of JPEG-afbeeldingen.

De volgende tabel geeft een overzicht van de grootte van de installatiekopie die u kunt gebruiken:

| Locatie | Grootten |
| -------- | ------ |
| Toepassingsbeheer | 268x160 px |
| Apparaatsjabloon | 64 x 64 pixels |
| Dashboard-tegels | De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px |

Voor de beste weergave in de toepassing, moet u de installatiekopieën die overeenkomen met de dimensies die wordt weergegeven in de vorige tabel maken.

## <a name="upload-the-images"></a>De afbeeldingen uploaden

De volgende secties wordt beschreven hoe u de installatiekopieën in de verschillende locaties te uploaden:

### <a name="application-manager"></a>Toepassingsbeheer

Een afbeelding uploaden op de **Toepassingsbeheer**, gaat u naar de **toepassingsinstellingen** pagina in de **beheer** sectie. U moet een beheerder om deze taak te voltooien:

![Afbeelding van de toepassing uploaden](media/howto-prepare-images/uploadapplicationmanager.png)

Klik op de tegel de installatiekopie van toepassing op uw voorbereide installatiekopie uploaden (268 x 160 pixels) van uw lokale computer.

### <a name="application-dashboard"></a>Toepassingsdashboard

Als u wilt uploaden een afbeelding op het dashboard, gaat u naar de **Dashboard** van uw toepassing en klik op **bewerken**. U moet een opbouwfunctie om deze taak te voltooien:

![Toepassing dashboard installatiekopie uploaden](media/howto-prepare-images/uploadhomepage.png)

Onder de afbeelding configureren, klikt u op de tegel de installatiekopie van het uploaden van uw voorbereide installatiekopie van uw lokale computer. De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px.

**Sla** de geüploade installatiekopie. U kunt het formaat van het in de bewerkingsmodus. Klik op **Done** als u klaar bent. 

### <a name="device-template"></a>Apparaatsjabloon

Als u wilt uploaden een installatiekopie van een sjabloon van het apparaat, gaat u naar **Device Explorer**, kiest u de sjabloon van het apparaat en klik vervolgens op een apparaat. U moet een opbouwfunctie om deze taak te voltooien:

![Installatiekopie van het apparaat sjabloon uploaden](media/howto-prepare-images/uploaddevicetemplate.png)

Klik op de tegel van de installatiekopie naar uw voorbereide installatiekopie uploaden (64 x 64 pixels) van uw lokale computer. 

### <a name="device-dashboard"></a>Apparaatdashboard

Als u wilt uploaden een installatiekopie op een apparaatdashboard, gaat u naar **Device Explorer**, kiest u de sjabloon van het apparaat en klik vervolgens op een apparaat. Kies vervolgens de **Dashboard** pagina en klik op **sjabloon bewerken**. U moet een opbouwfunctie om deze taak te voltooien:

![Installatiekopie van het dashboard apparaat uploaden](media/howto-prepare-images/uploaddevicedashboard.png)

Onder de afbeelding configureren, klikt u op de tegel van de installatiekopie en kies vervolgens het bestand te uploaden vanaf uw lokale computer. De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px.

**Sla** de geüploade installatiekopie. U kunt vergroten of verkleinen en de positie in de bewerkingsmodus. Klik op **Done** als u klaar bent.

### <a name="device-set-dashboard"></a>Apparaat instellen dashboard

Als u wilt uploaden een installatiekopie op een apparaat instellen-dashboard, gaat u naar **Apparaatsets** en kies de apparaatset en klik vervolgens op een apparaat. Kies vervolgens de **Dashboard** pagina en klik op **bewerken**:

![Uploaden van dashboard installatiekopie van het apparaat instellen](media/howto-prepare-images/uploaddevicesetdashboard.png)

Onder de afbeelding configureren, klikt u op de tegel de installatiekopie van het uploaden van uw voorbereide installatiekopie van uw lokale computer. De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px.

**Sla** de geüploade installatiekopie. U kunt vergroten of verkleinen en de positie in de bewerkingsmodus. Klik op **Done** als u klaar bent.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u installatiekopieën voorbereidt en uploadt naar uw Azure IoT Central-toepassing, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Beheer van apparaten in uw Azure IoT Central-toepassing](howto-manage-devices.md)