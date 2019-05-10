---
title: IoT Central beheren vanuit Azure portal | Microsoft Docs
description: IoT Central beheren vanuit Azure portal.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/14/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: c72de0ef874659a5d7840689e38bd7857c25b840
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65464113"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>IoT Central beheren vanuit Azure portal

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

In plaats van het maken en beheren van IoT Central-toepassingen van de IoT Central [Toepassingsbeheer](https://aka.ms/iotcentral) pagina, kunt u de [Azure-portal](https://portal.azure.com) voor het beheren van uw toepassingen.

## <a name="create-iot-central-applications"></a>IoT Central-toepassingen maken

Voor het maken van een toepassing, gaat u naar de [Azure-portal](https://ms.portal.azure.com) en selecteer **een resource maken** in het hoofdgedeelte voor navigatie-menu aan de linkerkant.

![Beheerportal: nav-menu](media/howto-manage-iot-central-from-portal/image0.png)

Typ in de zoekbalk typt, **IoT Central**.

![Beheerportal: zoeken](media/howto-manage-iot-central-from-portal/image0a1.png)

Selecteer de **IoT Central Application** regelitem in de lijst met zoekresultaten.

![Beheerportal: zoekresultaten](media/howto-manage-iot-central-from-portal/image0b1.png)

Selecteer nu **Maken**.

![Beheerportal: IoT Central-resource](media/howto-manage-iot-central-from-portal/image0c1.png)

Vul de velden in het formulier. Dit formulier is vergelijkbaar met het formulier invullen om toepassingen te maken op het IoT Central [Toepassingsbeheer](https://aka.ms/iotcentral) pagina. Zie voor meer informatie de [maken van een toepassing IoT Central](quick-deploy-iot-central.md) Quick Start.

![Beheerportal: IoT Central-resource maken](media/howto-manage-iot-central-from-portal/image1a.png)  

Na het invullen van alle velden, selecteer **maken**.

## <a name="manage-existing-iot-central-applications"></a>Bestaande IoT Central toepassingen beheren

Als u al een Azure IoT Central-toepassing verwijderen of verplaatsen naar een ander abonnement of resourcegroep groep in Azure portal hebt.

> [!NOTE]
> U kunt proefversie toepassingen in Azure portal niet zien, omdat ze niet gekoppeld aan uw abonnement zijn.

Selecteer om te beginnen, **alle resources** in het hoofdgedeelte voor navigatie-menu aan de linkerkant. Gebruik het zoekvak typt de naam van uw toepassing te vinden in de lijst met resources. Selecteer vervolgens de IoT Central-toepassing die u wilt beheren.

![Beheerportal: resourcebeheer](media/howto-manage-iot-central-from-portal/image2a.png)

Ga naar de toepassing, selecteert u de URL van IoT Central-toepassing.

![Beheerportal: resourcebeheer](media/howto-manage-iot-central-from-portal/image3.png)

Om de toepassing naar een andere resourcegroep verplaatsen, selecteert u **wijzigen** naast de resourcegroep. Op de **resources verplaatsen** pagina, kies de resourcegroep die u wilt migreren van deze toepassing.

![Beheerportal: resourcebeheer](media/howto-manage-iot-central-from-portal/image4a.png)

Selecteer voor het verplaatsen van de toepassing naar een ander abonnement, de **wijzigen** koppeling naast het abonnement. Kies het abonnement waarvoor u wilt migreren van deze toepassing in het dialoogvenster dat wordt weergegeven.

![Beheerportal: resourcebeheer](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u Azure IoT Central om toepassingen te beheren vanuit Azure portal, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw toepassing beheren](howto-administer.md)