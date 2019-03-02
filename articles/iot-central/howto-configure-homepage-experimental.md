---
title: Configureren van het dashboard Azure IoT Central | Microsoft Docs
description: Als een opbouwfunctie voor expressies, informatie over het configureren van het standaarddashboard voor de Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 02/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 5d769de62ccae1b1480e4da9e9643b63633073a4
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57215554"
---
# <a name="configure-the-application-dashboard"></a>Configureren van het dashboard

De **Dashboard** is de pagina waarop wordt geladen wanneer gebruikers die toegang tot de toepassing hebben gaat u naar de URL van de toepassing. Als u een geselecteerd de **voorbeeld Contoso** of **voorbeeld Devkits** toepassingssjabloon voor het maken van uw toepassing, uw toepassing heeft een vooraf gedefinieerde dashboard. Als u ervoor kiest de **aangepaste toepassing** toepassingssjabloon, uw dashboard is leeg.

> [!NOTE]
> Gebruikers kunnen ook [hun eigen persoonlijke dashboards maken](howto-personalize-dashboard-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) gebruiken in plaats van het dashboard van de toepassing standaard.

## <a name="add-tiles"></a>Tegels toevoegen

De volgende schermafbeelding ziet u het dashboard in een toepassing gemaakt op basis van de **voorbeeld Contoso** sjabloon. De standaard-dashboard voor uw toepassing aanpassen, klikt u op **bewerken** aan de bovenkant van de pagina.

![Dashboard voor toepassingen op basis van de sjabloon 'Contoso voorbeeld'](media/howto-configure-homepage-experimental/image1.png)

Te klikken op **bewerken**, wordt het deelvenster van de bibliotheek dashboard geopend. De bibliotheek bevat de tegels en dashboard primitieven die kunt u het dashboard aanpassen.

![Bibliotheek voor het dashboard](media/howto-configure-homepage-experimental/image2.png)

Bijvoorbeeld, u kunt toevoegen een **apparaatinstellingen en eigenschappen** tegel om een selectie van de huidige instellingen en eigenschappen van de waarden voor een apparaat weer te geven. Om dit te doen, selecteert u eerst een **apparaat sjabloon** Selecteer vervolgens een **apparaatexemplaar**. Nadat u dat de tegel geeft een titel en selecteer een **instelling** of een **eigenschap** om weer te geven. De volgende schermafbeelding ziet u instellingen en eigenschappen die zijn geselecteerd om toe te voegen aan de tegel. Klik op **gedaan** om op te slaan van de wijziging aan het dashboard.

!['Apparaatdetails configureren' formulier met details voor instellingen en eigenschappen](media/howto-configure-homepage-experimental/image3.png)

Nu als een operator het standaarddashboard toepassing bekijkt, ze de nieuwe tegel met zien de **ingesteld temperatuur** instellen voor het apparaat:

![Tabblad met de weergegeven instellingen en eigenschappen voor de tegel 'Dashboard'](media/howto-configure-homepage-experimental/image4.png)

U kunt andere tegeltypen in de bibliotheek om te ontdekken hoe u de toepassing standaarddashboard verder aanpassen verkennen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het configureren van uw Azure IoT Central standaarddashboard van toepassing, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Leer hoe u installatiekopieën voorbereidt en uploadt](howto-prepare-images-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
