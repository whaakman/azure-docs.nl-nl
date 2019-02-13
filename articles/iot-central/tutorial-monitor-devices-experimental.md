---
title: Uw apparaten bewaken in Azure IoT Central | Microsoft Docs
description: Gebruik als operator uw Azure IoT Central-toepassing om uw apparaten te bewaken.
author: dominicbetts
ms.author: dobett
ms.date: 02/01/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 9480565643887b5a9a4d644ba3173b365eaea29c
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767770"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Zelfstudie: Azure IoT Central gebruiken om uw apparaten te bewaken

In deze zelfstudie leert u hoe u als operator uw Microsoft Azure IoT Central toepassing kunt gebruiken om uw apparaten te bewaken en instellingen te wijzigen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een melding ontvangen
> * Een probleem onderzoeken
> * Een probleem oplossen

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet de bouwer ten minste de eerste drie zelfstudies voor bouwers voltooien om de Azure IoT Central-toepassing te maken:

* [Een nieuw apparaattype definiëren](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Regels en acties voor uw apparaat configureren](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [De weergaven van de operator aanpassen](tutorial-customize-operator-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

## <a name="receive-a-notification"></a>Een melding ontvangen

Azure IoT Central verzendt meldingen over apparaten als e-mailberichten. De bouwer heeft een regel toegevoegd om een ​​melding te verzenden wanneer de temperatuur in een aangesloten airconditioningapparaat een drempel overschrijdt. Controleer de e-mails die zijn verzonden naar het account dat de bouwer heeft gekozen voor het ontvangen van meldingen.

Open het e-mailbericht dat u aan het einde van de zelfstudie [Regels en acties voor uw apparaat configureren](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) hebt ontvangen. Kies in het e-mailbericht **Klik hier om uw apparaat te openen**:

![E-mailwaarschuwing](media/tutorial-monitor-devices-experimental/email.png)

De **Apparaat**-pagina voor het gesimuleerde apparaat **Aangesloten airconditioner-1** dat u in de vorige zelfstudies hebt gemaakt, wordt in uw browser geopend:

![Apparaat dat het e-mailbericht met de melding heeft geactiveerd](media/tutorial-monitor-devices-experimental/sourcedevice.png)

## <a name="investigate-an-issue"></a>Een probleem onderzoeken

Als operator kunt u informatie over het apparaat bekijken op de pagina’s **Metingen**, **Instellingen**, **Eigenschappen**, **Regels** en  **Dashboard**. De builder heeft het **Dashboard** aangepast om belangrijke informatie weer te geven over een aangesloten airconditioningapparaat.

Kies de weergave **Dashboard** om informatie over het apparaat te bekijken.

![Apparaatdashboard](media/tutorial-monitor-devices-experimental/initial_screen.png)

De grafiek op het dashboard toont een grafische voorstelling van de temperatuur van het apparaat. U kunt ook de huidige doeltemperatuur voor het apparaat bekijken in de tegel **​​Doeltemperatuur instellen**. U besluit dat de doeltemperatuur te hoog is.

## <a name="remediate-an-issue"></a>Een probleem oplossen

Gebruik de pagina **Instellingen** om de doeltemperatuur van het apparaat te wijzigen:

1. Kies **Instellingen**. Wijzig **Temperatuur instellen** in 75. Kies **Update** om de nieuwe doeltemperatuur naar het apparaat te verzenden. Wanneer het apparaat de instellingswijziging bevestigt, verandert de status van de instellingswaarde in **Gesynchroniseerd**:

    ![Instellingen bijwerken](media/tutorial-monitor-devices-experimental/change_settings.png)

2. Kies **Dashboard** en controleer de waarde van de nieuwe instelling:

    ![Bijgewerkt apparaatdashboard](media/tutorial-monitor-devices-experimental/new_settings.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="nextstepaction"]
> * Een melding ontvangen
> * Een probleem onderzoeken
> * Een probleem oplossen

Nu u weet hoe u uw apparaat controleert, is de voorgestelde volgende stap [Een apparaat toevoegen](tutorial-add-device-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).