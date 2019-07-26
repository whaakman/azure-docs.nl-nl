---
title: IoT Central beheren via de Azure Portal | Microsoft Docs
description: IoT Central beheren vanuit de Azure Portal.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 07/11/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 7893c8e8b8d67b4b63bd9d6bb5a71552e95c9125
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850251"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>IoT Central beheren via de Azure Portal

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

In plaats van IoT Central-toepassingen te maken en beheren via de pagina [toepassings beheer](https://aka.ms/iotcentral) van IOT Central kunt u de [Azure Portal](https://portal.azure.com) gebruiken om uw toepassingen te beheren.

## <a name="create-iot-central-applications"></a>IoT Central-toepassingen maken

Als u een toepassing wilt maken, gaat u naar de [Azure Portal](https://ms.portal.azure.com) en selecteert u **een resource maken** in het hoofd menu navigatie aan de linkerkant.

![Beheer Portal: navigatie menu](media/howto-manage-iot-central-from-portal/image0.png)

Typ **IOT Central**in de zoek balk.

![Beheer Portal: zoeken](media/howto-manage-iot-central-from-portal/image0a1.png)

Selecteer het regel item **IOT Central toepassing** in de zoek resultaten.

![Beheerportal: Zoek resultaten](media/howto-manage-iot-central-from-portal/image0b1.png)

Selecteer nu **Maken**.

![Beheer Portal: IoT Central resource](media/howto-manage-iot-central-from-portal/image0c1.png)

Vul alle velden in het formulier in. Dit formulier is vergelijkbaar met het formulier dat u invult om toepassingen te maken op de pagina IoT Central [toepassings beheer](https://aka.ms/iotcentral) . Zie de Snelstartgids [een IOT Central-toepassing maken](quick-deploy-iot-central.md) voor meer informatie.

![Beheer Portal: IoT Central-resource maken](media/howto-manage-iot-central-from-portal/image1a.png)  

Nadat u alle velden hebt ingevuld, selecteert u **maken**.

## <a name="manage-existing-iot-central-applications"></a>Bestaande IoT Central-toepassingen beheren

Als u al een Azure IoT Central-toepassing hebt, kunt u deze verwijderen of verplaatsen naar een ander abonnement of een andere resource groep in de Azure Portal.

> [!NOTE]
> U kunt geen proef toepassingen weer geven in de Azure Portal omdat deze niet zijn gekoppeld aan uw abonnement.

Om aan de slag te gaan, selecteert u **alle resources** in het hoofd menu navigatie aan de linkerkant. Gebruik het zoekvak om de naam van uw toepassing te typen om deze te zoeken in de lijst met resources. Selecteer vervolgens de IoT Central toepassing die u wilt beheren.

![Beheer Portal: resource beheer](media/howto-manage-iot-central-from-portal/image2a.png)

Als u naar de toepassing wilt navigeren, selecteert u de IoT Central toepassings-URL.

![Beheer Portal: resource beheer](media/howto-manage-iot-central-from-portal/image3.png)

Als u de toepassing wilt verplaatsen naar een andere resource groep, selecteert u **wijzigen** naast de resource groep. Kies op de pagina **resources verplaatsen** de resource groep waar u deze toepassing naartoe wilt migreren.

![Beheer Portal: resource beheer](media/howto-manage-iot-central-from-portal/image4a.png)

Als u de toepassing naar een ander abonnement wilt verplaatsen,  selecteert u de wijzigings koppeling naast het abonnement. Kies het abonnement waarnaar u deze toepassing wilt migreren in het dialoog venster dat wordt weer gegeven.

![Beheer Portal: resource beheer](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u Azure IoT Central-toepassingen kunt beheren vanuit de Azure Portal, volgt u de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw toepassing beheren](howto-administer.md)