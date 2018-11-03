---
title: Afbeeldingen uploaden naar uw Azure IoT Central-toepassing | Microsoft Docs
description: Als een opbouwfunctie voor expressies, informatie over het voorbereiden en afbeeldingen uploaden naar uw Azure IoT Central-toepassing.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 18c44a3d91a4964d054c8e142394da7d69772ed0
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960698"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Voorbereiden en afbeeldingen uploaden naar uw Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe, als een opbouwfunctie voor expressies, kunt u uw Microsoft Azure IoT Central-toepassing uploaden van aangepaste installatiekopieën. U kunt bijvoorbeeld een apparaatdashboard met een afbeelding van het apparaat.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT Central-toepassing. Zie voor meer informatie de [maken van een toepassing Quick Start](quick-deploy-iot-central.md).
1. Een hulpprogramma voor het schalen en het formaat van afbeeldingsbestanden.

## <a name="choose-where-to-use-custom-images"></a>Kies waar u aangepaste installatiekopieën kunt gebruiken

U kunt aangepaste installatiekopieën toevoegen aan de volgende locaties in een Azure IoT Central-toepassing:

* De **Toepassingsbeheer** pagina

    ![Een installatiekopie op de pagina voor het beheer van toepassingen](media/howto-prepare-images/applicationmanager.png)

* De startpagina

    ![Een installatiekopie op de startpagina](media/howto-prepare-images/homepage.png)

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
| **Toepassingsbeheer** | 268 x 160 pixels |
| Apparaat-sjabloon | 64 x 64 pixels |
| Startpagina en dashboardtegels | De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px |

Voor de beste weergave in de toepassing, moet u de installatiekopieën die overeenkomen met de dimensies die wordt weergegeven in de vorige tabel maken.

## <a name="upload-the-images"></a>De afbeeldingen uploaden

De volgende secties wordt beschreven hoe u de te gebruiken in de verschillende locaties installatiekopieën uploaden:

### <a name="application-manager"></a>Toepassingsbeheer

Het uploaden van een installatiekopie moet worden gebruikt op de **Toepassingsbeheer**, gaat u naar de **toepassingsinstellingen** pagina in de **beheer** sectie. U moet een beheerder om deze taak te voltooien:

![Afbeelding van de toepassing uploaden](media/howto-prepare-images/uploadapplicationmanager.png)

Klik op de afbeelding uploaden en kies vervolgens het bestand te uploaden vanaf uw lokale computer.

### <a name="home-page"></a>Startpagina

Als u wilt uploaden een afbeelding moet worden gebruikt op de startpagina, gaat u naar de **startpagina** van uw toepassing en de ontwerpmodus schakeloptie op. U moet een opbouwfunctie om deze taak te voltooien:

![Startpagina-installatiekopie uploaden](media/howto-prepare-images/uploadhomepage.png)

Klik op de afbeelding uploaden en kies vervolgens het bestand te uploaden vanaf uw lokale computer.

Nadat de installatiekopie uploadt, kunt u het formaat terwijl de ontwerpmodus is ingeschakeld.

### <a name="device-template"></a>Apparaat-sjabloon

Als u wilt uploaden een afbeelding moet worden gebruikt op een apparaat-sjabloon, gaat u naar **Device Explorer**, kies de sjabloon van het apparaat en klik vervolgens op een apparaat, en schakel ontwerpmodus op. U moet een opbouwfunctie om deze taak te voltooien:

![Installatiekopie van het apparaat sjabloon uploaden](media/howto-prepare-images/uploaddevicetemplate.png)

Klik op de afbeelding uploaden en kies vervolgens het bestand te uploaden vanaf uw lokale computer.

### <a name="device-dashboard"></a>Apparaatdashboard

Als u wilt uploaden een afbeelding moet worden gebruikt op een apparaatdashboard, gaat u naar **Device Explorer**, kiest u de sjabloon van het apparaat en klik vervolgens op een apparaat. Kies vervolgens de **Dashboard** pagina en switch ontwerpmodus op. U moet een opbouwfunctie om deze taak te voltooien:

![Installatiekopie van het dashboard apparaat uploaden](media/howto-prepare-images/uploaddevicedashboard.png)

Klik op de afbeelding uploaden en kies vervolgens het bestand te uploaden vanaf uw lokale computer.

Nadat de installatiekopie uploadt, kunt u het formaat en verplaatsen terwijl **ontwerpmodus** is ingeschakeld.

### <a name="device-set-dashboard"></a>Apparaat instellen dashboard

Als u wilt uploaden een afbeelding moet worden gebruikt op een apparaat instellen-dashboard, gaat u naar **Apparaatsets** en kies de apparaatset en klik vervolgens op een apparaat. Kies vervolgens de **Dashboard** pagina en switch **ontwerpmodus** op:

![Uploaden van dashboard installatiekopie van het apparaat instellen](media/howto-prepare-images/uploaddevicesetdashboard.png)

Klik op de afbeelding uploaden en kies vervolgens het bestand te uploaden vanaf uw lokale computer.

Nadat de installatiekopie uploadt, kunt u het formaat en verplaatst terwijl de ontwerpmodus is ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u installatiekopieën voorbereidt en uploadt naar uw Azure IoT Central-toepassing, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Beheer van apparaten in uw Azure IoT Central-toepassing](howto-manage-devices.md)