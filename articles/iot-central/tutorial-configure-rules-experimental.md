---
title: Regels en acties configureren in Azure IoT Central | Microsoft Docs
description: Deze zelfstudie laat u zien hoe u als bouwer op telemetrie gebaseerde regels en acties in uw Azure IoT Central-toepassing kunt configureren.
author: ankitscribbles
ms.author: ankitgup
ms.date: 01/28/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: d8a5ca6285624720e23a4986917ab5e715f6ebfa
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768010"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central-new-ui-design"></a>Zelfstudie: Regels en acties voor uw apparaat configureren in Azure IoT Central (nieuwe gebruikersinterface)

*Dit artikel is van toepassing op operators, opbouwfuncties en beheerders.*

In deze zelfstudie maakt u een regel die een e-mail verzendt wanneer de temperatuur in een aangesloten airconditioningapparaat hoger is dan 32 &deg;C.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een regel op basis van telemetrie maken
> * Een actie toevoegen

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Vereisten

Voer voordat u begint de zelfstudie [Een nieuw apparaattype definiëren in uw toepassing](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) uit om de apparaatsjabloon **Aangesloten airconditioner** te maken om mee te werken.

## <a name="create-a-telemetry-based-rule"></a>Een regel op basis van telemetrie maken

1. Als u een ​​nieuwe op telemetrie gebaseerde regel wilt toevoegen aan uw toepassing, kiest u in het navigatiemenu aan de linkerkant **Apparaatsjablonen**:

    ![De pagina Apparaatsjablonen](media/tutorial-configure-rules-experimental/templatespage1.png)

    U ziet de apparaatsjabloon **Aangesloten airconditioner (1.0.0)**  die u in de vorige zelfstudie hebt gemaakt.

2. U kunt de apparaatsjabloon aanpassen door op de sjabloon **Aangesloten airconditioner** te klikken die u in de vorige zelfstudie hebt gemaakt.

3. Als u een regel op basis van telemetrie wilt toevoegen in de weergave **Regels**, kiest u **Regels**, klikt u op **+ Nieuwe regel** en kiest u **Telemetrie**:

    ![Weergave Regels](media/tutorial-configure-rules-experimental/newrule.png)

5. Gebruik de informatie in de volgende tabel om uw regel te definiëren:

    | Instelling                                      | Waarde                             |
    | -------------------------------------------- | ------------------------------    |
    | Naam                                         | Waarschuwing temperatuur airconditioner |
    | Regel inschakelen voor alle apparaten van deze sjabloon | Aan                                |
    | Voorwaarde                                    | Temperatuur is hoger dan 32    |
    | Aggregatie                                  | Geen                              |

    ![Voorwaarde temperatuurregel](media/tutorial-configure-rules-experimental/temperaturerule.png)

    Klik vervolgens op **Opslaan**.

## <a name="add-an-action"></a>Een actie toevoegen

Wanneer u een regel definieert, definieert u ook een actie die moet worden uitgevoerd wanneer aan de voorwaarden van de regel wordt voldaan. In deze zelfstudie maakt u een regel met een actie die een e-mailmelding verstuurt.

1. Als u een **Actie**wilt toevoegen, moet u de regel eerst **Opslaan**. Daarna schuift u omlaag in het deelvenster **Telemetrieregel configureren** en kiest u **+** naast **Acties** en selecteert u vervolgens **E-mail**:

    ![Actie temperatuurregel](media/tutorial-configure-rules-experimental/addaction.png)

2. Gebruik de informatie in de volgende tabel om uw actie te definiëren:

    | Instelling   | Waarde                          |
    | --------- | ------------------------------ |
    | Handeling        | Uw e-mailadres             |
    | Opmerkingen     | De temperatuur van de airconditioner heeft de drempelwaarde overschreden. |

    > [!NOTE]
    > Om een e-mailmelding te ontvangen, moet het e-mailadres een [gebruikers-ID in de toepassing](howto-administer-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) zijn, en moet die gebruiker zich minimaal één keer hebben aangemeld bij de toepassing.

    ![Actie temperatuurregel](media/tutorial-configure-rules-experimental/temperatureaction.png)

3. Klik op **Opslaan**. De regel wordt weergegeven op de pagina **Regels**.

## <a name="test-the-rule"></a>De regel testen

Kort nadat u de regel hebt opgeslagen, wordt deze actief. Wanneer aan de voorwaarden in de regel wordt voldaan, verzendt uw toepassing een bericht naar het e-mailadres dat u in de actie hebt opgegeven.

![E-mailactie](media/tutorial-configure-rules-experimental/email.png)

> [!NOTE]
> Als u klaar bent met testen, schakelt u de regel uit om geen waarschuwingen meer te ontvangen in uw postvak.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Een regel op basis van telemetrie maken
> * Een actie toevoegen

Nu u een regel op basis van drempelwaarden hebt gedefinieerd, is de voorgestelde volgende stap [het aanpassen van de weergaven van de operator](tutorial-customize-operator-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

Zie voor meer informatie over verschillende soorten regels in Azure IoT Central en over het parametriseren van de regeldefinitie:
* [Een telemetrieregel maken en meldingen instellen](howto-create-telemetry-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
* [Een gebeurtenisregel maken en meldingen instellen](howto-create-event-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

<!-- Next tutorials in the sequence -->
