---
title: Werkstromen bouwen met de IoT Central-connector in Azure Logic Apps | Microsoft Docs
description: De IoT Central-connector gebruiken in Azure Logic Apps voor trigger werkstromen en maken, bijwerken en verwijderen van apparaten in werkstromen.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 77737db30c7a2c5c6350e4ae1ab70310036bd0e5
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57311352"
---
# <a name="build-workflows-with-the-iot-central-connector-in-azure-logic-apps"></a>Werkstromen bouwen met de IoT Central-connector in Azure Logic Apps

*In dit onderwerp is van toepassing op builders en beheerders.*

Gebruik Azure Logic Apps om geautomatiseerde, schaalbare werkstromen die apps en gegevens in cloudservices en on-premises systemen integreren. Met Azure Logic Apps heeft veel connectors voor veel Azure-services, Microsoft-services en andere Software-As-A-Service (SaaS)-producten. Met behulp van de IoT Central-connector in Azure Logic Apps, kunt u werkstromen activeren wanneer een regel wordt geactiveerd in IoT Central. U kunt ook de acties in de IoT Central-connector gebruiken om te maken van een apparaat, het bijwerken van de eigenschappen en instellingen van een apparaat of het verwijderen van een apparaat.

U kunt de IoT Central-connector gebruiken in Microsoft Flow. Zowel Azure Logic Apps en Microsoft Flow zijn designer-first integratieservices, maar enigszins doelgroepen. Stroom kantoormedewerkers om de zakelijke werkstromen die ze nodig hebben te bouwen. Logic Apps kan IT-professionals op het bouwen van de integraties die ze nodig hebben om gegevens te verbinden. Flow en Logic Apps vergelijken [hier](https://docs.microsoft.com/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs). Meer informatie over het bouwen van werkstromen met de IoT Central-connector in Microsoft Flow [hier](howto-add-microsoft-flow-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="prerequisites"></a>Vereisten

- Een betalen per gebruik-toepassing
- Een Azure-account en abonnement maken en beheren van Logic apps

## <a name="trigger-a-workflow-when-a-rule-is-triggered"></a>Een werkstroom wordt geactiveerd wanneer een regel wordt geactiveerd

Deze sectie leest u hoe u een bericht in Microsoft Teams plaatsen wanneer een regel wordt geactiveerd. U kunt uw werkstroom voor het gebruik van andere connectors dingen doen zoals verzenden een gebeurtenis naar uw Event hub, een nieuwe Azure DevOps-werkitem maken of een nieuwe rij invoegen in SQL server configureren.

1. Begin met [maken van een regel in IoT Central](howto-create-telemetry-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json). Nadat u de voorwaarden van de regel hebt opgeslagen, selecteert u de **Azure Logic Apps** tegel als een nieuwe actie. Selecteer **maken in Azure portal**. U gaat naar de Azure-portal een nieuwe logische app maken. Mogelijk moet u zich aanmelden bij uw Azure-account.

1. Voer de vereiste gegevens voor het maken van een nieuwe logische app. U kunt een Azure-abonnement voor het inrichten van de nieuwe logische app in. Het hoeft niet te zijn uw IoT Central-app is gemaakt in hetzelfde abonnement. Selecteer **Maken**.

    ![Logische app maken in Azure portal](./media/howto-build-azure-logic-apps-experimental/createinazureportal.png)

1. Nadat uw logische app is gemaakt, wordt u automatisch navigeren naar de ontwerper van logische Apps. Selecteer **lege logische App**. 

    ![Een lege, logische app maken](./media/howto-build-azure-logic-apps-experimental/blanklogicapp.png)

1. Zoek in de ontwerpfunctie voor 'iot central' en kies de **wanneer een regel wordt geactiveerd** trigger. Zich aanmelden bij de connector met het account dat u zich bij uw app voor IoT Central aanmeldt.

    ![Meld u aan bij IoT Central-connector](./media/howto-build-azure-logic-apps-experimental/addtrigger.png)

1. Na geslaagde aanmelding ziet u de velden worden weergegeven. Selecteer de **toepassing** en **regel** uit de vervolgkeuzelijsten.

    ![Toepassing en een regel voor verzamelen](./media/howto-build-azure-logic-apps-experimental/pickappandrule.png)

1. Voeg een nieuwe actie toe. Zoeken naar **plaatsen bericht teams** en kies **een bericht** van de Microsoft Teams-connector. Zich aanmelden bij de connector met het account dat u gebruikt in Microsoft Teams.

1. Kies in de actie, de **Team** en **kanaal**. Vul de **bericht** veld met wat u wilt dat elk bericht te zeggen hadden. U kunt opnemen *dynamische inhoud* van uw IoT Central-regel, geven belangrijke informatie zoals de naam van apparaat- en tijdstempel op de melding.
    > [!NOTE]
    > Selecteer de **meer** tekst in het venster met dynamische inhoud om op te halen van de meting en eigenschap waarden die de regel geactiveerd.

    ![Bewerken actie voor logische app met deelvenster met dynamische openen](./media/howto-build-azure-logic-apps-experimental/buildworkflow.png)

1. Wanneer u klaar bent voor het bewerken van de actie selecteren **opslaan**.

1. Als u terug naar uw IoT Central-app gaat, ziet u dat deze regel bevat een actie voor Azure Logic Apps onder acties.

U kunt altijd beginnen het bouwen van een werkstroom met behulp van de IoT Central-connector in Logic Apps in Azure Portal. U kunt vervolgens kiezen welke IoT Central-app en welke regel verbinding maken met en het nog steeds op dezelfde manier werkt. Er is niet nodig om te starten vanaf de pagina van de regels voor IoT Central telkens.

## <a name="create-update-and-delete-a-device-in-a-workflow"></a>Maken, bijwerken en verwijderen van een apparaat in een werkstroom

Wanneer u een werkstroom in uw logische app bouwt, kunt u een actie met de IoT Central-connector kunt toevoegen. De beschikbare acties zijn de **maken van een apparaat**, **bijwerken van een apparaat**, en **verwijderen van een apparaat**.

> [!NOTE]
> Voor **bijwerken van een apparaat** en **verwijderen van een apparaat**, moet u een ID van het bestaande apparaat dat u wilt bijwerken of verwijderen. Krijgt u de ID van het apparaat IoT Central in de **Device Explorer**

![Apparaat-ID van IoT Central device explorer](./media/howto-build-azure-logic-apps-experimental/iotcdeviceid.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u kunt Microsoft Flow gebruiken om werkstromen te bouwen, de voorgestelde volgende stap is het [apparaten beheren](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
