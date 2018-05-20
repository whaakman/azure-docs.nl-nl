---
title: Afbeeldingen uploaden naar uw Azure IoT centrale toepassing | Microsoft Docs
description: Informatie over het voorbereiden en installatiekopieën uploaden naar uw Azure IoT centrale toepassing als een opbouwfunctie.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: a43f2dd780604235ada1d8e3ae8a20563042fbaa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Voorbereiden en installatiekopieën uploaden naar uw Azure IoT centrale toepassing

Dit artikel wordt beschreven hoe als hulpmiddel, kunt u uw Microsoft Azure IoT centrale toepassing aangepaste installatiekopieën uploaden. U kunt bijvoorbeeld een apparaat-dashboard met een afbeelding van het apparaat.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT centrale toepassing. Zie voor meer informatie [maken van uw Azure IoT centrale toepassing](howto-create-application.md).
1. Een hulpprogramma voor het schalen en vergroten of verkleinen afbeeldingsbestanden.

## <a name="choose-where-to-use-custom-images"></a>Kies waar u aangepaste installatiekopieën gebruiken

U kunt aangepaste installatiekopieën toevoegen aan de volgende locaties in een Azure IoT centrale toepassing:

* De **Toepassingsbeheer** pagina

    ![Afbeelding op pagina voor het beheer van toepassingen](media/howto-prepare-images/applicationmanager.png)

* De startpagina

    ![De installatiekopie op de startpagina](media/howto-prepare-images/homepage.png)

* Een sjabloon van het apparaat

    ![Afbeelding van apparaat-sjabloon](media/howto-prepare-images/devicetemplate.png)

* Een tegel op een apparaat-dashboard

    ![De installatiekopie op de apparaattegel](media/howto-prepare-images/devicetile.png)

* Stel dashboard in een tegel op een apparaat

    ![De installatiekopie op apparaat set tegel](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>De afbeeldingen voorbereiden

U kunt in alle vier locaties, PNG, GIF of JPEG-installatiekopieën gebruiken.

De volgende tabel geeft een overzicht van de grootte van de installatiekopie die u kunt gebruiken:

| Locatie | Grootten |
| -------- | ------ |
| **Toepassingsbeheer** | 268 x 160 px |
| Apparaat-sjabloon | 64 x 64 px |
| Startpagina en dashboard tegels | De kleinste grootte tegel is 200 x 200 px, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px |

Voor de beste weergave in de toepassing, moet u de installatiekopieën die overeenkomen met de dimensies die wordt weergegeven in de vorige tabel maken.

## <a name="upload-the-images"></a>De afbeeldingen uploaden

De volgende secties wordt beschreven hoe de afbeeldingen te gebruiken op verschillende locaties uploaden:

### <a name="application-manager"></a>Toepassingsbeheer

Voor het uploaden van een installatiekopie gebruiken op de **Toepassingsbeheer**, gaat u naar de **toepassingsinstellingen** pagina in de **beheer** sectie. U moet een beheerder om deze taak te voltooien:

![Upload de installatiekopie van toepassing](media/howto-prepare-images/uploadapplicationmanager.png)

Klik op de installatiekopie van het uploaden en kies vervolgens het bestand te uploaden naar uw lokale machine.

### <a name="home-page"></a>Startpagina

Als u wilt gebruiken op de startpagina van een afbeelding uploadt, gaat u naar de **startpagina** van uw toepassing en de ontwerpmodus switch op. U moet een opbouwfunctie om deze taak te voltooien:

![Startpagina installatiekopie uploaden](media/howto-prepare-images/uploadhomepage.png)

Klik op de installatiekopie van het uploaden en kies vervolgens het bestand te uploaden naar uw lokale machine.

Nadat de installatiekopie uploadt, kunt u het formaat terwijl ontwerpmodus is ingeschakeld.

### <a name="device-template"></a>Apparaat-sjabloon

Als u wilt uploaden een afbeelding van een apparaat gebruikt, gaat u naar **apparaat Explorer**, kiest u de sjabloon van het apparaat en klik vervolgens op een apparaat en ontwerpmodus overschakelen op. U moet een opbouwfunctie om deze taak te voltooien:

![Apparaat sjablooninstallatiekopie uploaden](media/howto-prepare-images/uploaddevicetemplate.png)

Klik op de installatiekopie van het uploaden en kies vervolgens het bestand te uploaden naar uw lokale machine.

### <a name="device-dashboard"></a>Apparaat-dashboard

Als u wilt uploaden een installatiekopie gebruiken op een apparaat-dashboard, gaat u naar **apparaat Explorer**, kiest u de sjabloon van het apparaat en een apparaat. Kies vervolgens de **Dashboard** ontwerpmodus pagina en de switch op. U moet een opbouwfunctie om deze taak te voltooien:

![Apparaat dashboard installatiekopie uploaden](media/howto-prepare-images/uploaddevicedashboard.png)

Klik op de installatiekopie van het uploaden en kies vervolgens het bestand te uploaden naar uw lokale machine.

Nadat de installatiekopie uploadt, kunt u het formaat en verplaatsen terwijl **ontwerpmodus** is ingeschakeld.

### <a name="device-set-dashboard"></a>Apparaat instellen dashboard

Als u wilt uploaden een installatiekopie gebruiken op een apparaat set dashboard, gaat u naar **Apparaatsets** en kies de apparaatset en een apparaat. Kies vervolgens de **Dashboard** pagina en switch **ontwerpmodus** op:

![Apparaat set dashboard installatiekopie uploaden](media/howto-prepare-images/uploaddevicesetdashboard.png)

Klik op de installatiekopie van het uploaden en kies vervolgens het bestand te uploaden naar uw lokale machine.

Nadat de installatiekopie uploadt, kunt u het formaat en verplaatst terwijl de ontwerpmodus is ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe voorbereiden en installatiekopieën uploaden naar uw Azure IoT centrale toepassing, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Beheer van apparaten in uw Azure IoT centrale toepassing](howto-manage-devices.md)