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
ms.openlocfilehash: 096daa28b7548401adc857c3c6c8327ef1d1eb00
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57769401"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central-new-ui-design"></a>Zelfstudie: Regels en acties voor uw apparaat configureren in Azure IoT Central (nieuwe gebruikersinterface)

*Dit artikel is van toepassing op operators, opbouwfuncties en beheerders.*

In deze zelfstudie maakt u een regel die een e-mail verzendt wanneer de temperatuur in een aangesloten airconditioningapparaat hoger is dan 32 &deg;C.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een regel op basis van telemetrie maken
> * Een actie toevoegen

## <a name="prerequisites"></a>Vereisten

Voer voordat u begint de zelfstudie [Een nieuw apparaattype definiëren in uw toepassing](tutorial-define-device-type.md) uit om de apparaatsjabloon **Aangesloten airconditioner** te maken om mee te werken.

## <a name="create-a-telemetry-based-rule"></a>Een regel op basis van telemetrie maken

1. Selecteer om een nieuwe regel op basis van telemetrie toe aan uw toepassing, in het navigatiemenu links **Apparaatsjablonen**:

    ![De pagina Apparaatsjablonen](media/tutorial-configure-rules/templatespage1.png)

    U ziet de apparaatsjabloon **Aangesloten airconditioner (1.0.0)**  die u in de vorige zelfstudie hebt gemaakt.

2. Voor het aanpassen van de sjabloon voor het apparaat, selecteert u de **airconditioner verbonden** sjabloon die u in de vorige zelfstudie hebt gemaakt.

3. Toevoegen van een regel op basis van telemetrie in de **regels** weergave, selecteer **regels**, selecteer **+ nieuwe regel**, en selecteer vervolgens **telemetrie**:

    ![Weergave Regels](media/tutorial-configure-rules/newrule.png)

5. Gebruik de informatie in de volgende tabel om uw regel te definiëren:

    | Instelling                                      | Waarde                             |
    | -------------------------------------------- | ------------------------------    |
    | Name                                         | Waarschuwing temperatuur airconditioner |
    | Regel inschakelen voor alle apparaten van deze sjabloon | Aan                                |
    | Voorwaarde                                    | Temperatuur is hoger dan 32    |
    | Aggregatie                                  | Geen                              |

    ![Voorwaarde temperatuurregel](media/tutorial-configure-rules/temperaturerule.png)

    Selecteer vervolgens **Opslaan**.

## <a name="add-an-action"></a>Een actie toevoegen

Wanneer u een regel definieert, definieert u ook een actie die moet worden uitgevoerd wanneer aan de voorwaarden van de regel wordt voldaan. In deze zelfstudie maakt u een regel met een actie die een e-mailmelding verstuurt.

1. Als u een **Actie**wilt toevoegen, moet u de regel eerst **Opslaan**. Daarna schuift u omlaag in het deelvenster **Telemetrieregel configureren** en kiest u **+** naast **Acties** en selecteert u vervolgens **E-mail**:

    ![Actie temperatuurregel](media/tutorial-configure-rules/addaction.png)

2. Gebruik de informatie in de volgende tabel om uw actie te definiëren:

    | Instelling   | Value                          |
    | --------- | ------------------------------ |
    | Handeling        | Uw e-mailadres             |
    | Opmerkingen     | De temperatuur van de airconditioner heeft de drempelwaarde overschreden. |

    > [!NOTE]
    > Om een e-mailmelding te ontvangen, moet het e-mailadres een [gebruikers-ID in de toepassing](howto-administer.md) zijn, en moet die gebruiker zich minimaal één keer hebben aangemeld bij de toepassing.

    ![Actie temperatuurregel](media/tutorial-configure-rules/temperatureaction.png)

3. Selecteer **Opslaan**. De regel wordt weergegeven op de pagina **Regels**.

## <a name="test-the-rule"></a>De regel testen

Kort nadat u de regel hebt opgeslagen, wordt deze actief. Wanneer aan de voorwaarden in de regel wordt voldaan, verzendt uw toepassing een bericht naar het e-mailadres dat u in de actie hebt opgegeven.

> [!NOTE]
> Als u klaar bent met testen, schakelt u de regel uit om geen waarschuwingen meer te ontvangen in uw postvak.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Een regel op basis van telemetrie maken
> * Een actie toevoegen

Nu u een regel op basis van drempelwaarden hebt gedefinieerd, is de voorgestelde volgende stap [het aanpassen van de weergaven van de operator](tutorial-customize-operator.md).

Zie voor meer informatie over verschillende soorten regels in Azure IoT Central en over het parametriseren van de regeldefinitie:
* [Een telemetrieregel maken en meldingen instellen](howto-create-telemetry-rules.md).
* [Een gebeurtenisregel maken en meldingen instellen](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->