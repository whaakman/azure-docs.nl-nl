---
title: Maken en uitvoeren van taken in uw toepassing met Azure IoT Central | Microsoft Docs
description: Azure IoT Central-taken kunnen bulksgewijs beheermogelijkheden voor apparaten, zoals een apparaateigenschap bijwerkt, instelling of een opdracht is uitgevoerd.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 09/15/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 35db7bf87c7b72fc31d820c9058b1df8415bd553
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031302"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Maken en uitvoeren van een taak in uw Azure IoT Central-toepassing

U kunt Microsoft Azure IoT Central gebruiken voor het beheren van uw verbonden apparaten op schaal met behulp van taken. De functionaliteit van de taken kunt u bulksgewijze updates van apparaateigenschappen, instellingen en opdrachten uitvoeren. In dit artikel begeleidt u stapsgewijs door van hoe u aan de slag met behulp van taken in uw eigen toepassing.

## <a name="create-and-run-a-job"></a>Maken en uitvoeren van een taak

Deze sectie leest u hoe u maken en een taak uitvoert. Elke stap verloopt via een voorbeeld waarin wordt gedemonstreerd hoe u een taak voor gekoeld Verkoopautomaat-apparaten die u moet beschikken over de snelheid van de ventilator verhoogd uit te voeren.

1. Navigeer naar de taken in het navigatiedeelvenster.

1. Klik op **+ nieuw** om te beginnen met het maken van een nieuwe taak.

    ![Nieuwe taak maken](./media/howto-run-a-job/createnewjob.png)

1. Voer een naam en beschrijving die u helpen identificeren de taak die u maakt.

1. Selecteer die het apparaat zo ingesteld dat u wilt dat de taak moet worden toegepast op. Nadat het apparaat te selecteren hebt ingesteld, ziet u de rechterkant te vullen met de apparaten binnen het geselecteerde apparaat. Als u een apparaatset verbroken selecteert, er zijn geen apparaten worden weergegeven en ziet u een bericht waarin wordt uitgelegd dat het instellen van uw apparaat verbroken is.

1. Kies vervolgens het type taak dat is gedefinieerd (een instelling, eigenschap of opdracht). Klik op **+** naast het type taak ingeschakeld en de gewenste bewerkingen toevoegen.

    ![Taak configureren](./media/howto-run-a-job/configurejob.png)

1. Aan de rechterkant, kies de apparaten die u wilt de taak wordt uitgevoerd. Door te klikken op het bovenste selectievakje, zijn alle apparaten in de hele apparaat geselecteerd. Door te klikken op het selectievakje in de buurt van de naam, worden alle apparaten in de huidige pagina geselecteerd.

1. Zodra uw gewenste apparaten zijn geselecteerd, kiest u **uitvoeren**. De taak wordt nu weergegeven op de hoofdpagina **taken** pagina. In deze weergave bent u kan uw huidige actieve taak en de geschiedenis van een eerder taken uitvoeren. De actieve taak wordt altijd weergegeven boven aan de lijst.

    ![Taak uitvoeren](./media/howto-run-a-job/runjob.png)

    ![Taak weergeven](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > U kunt zich de geschiedenis van uw eerder uitgevoerde taken weergeven voor 30 dagen.

1. Als u een overzicht van uw taak, klikt u op de naam van de taak die u wilt weergeven in de lijst. In dit overzicht bevat de taakdetails, apparaten en apparaatstatussen.

    ![Apparaatstatus weergeven](./media/howto-run-a-job/viewdevicestatus.png)

### <a name="stop-a-running-job"></a>Een actieve taak stoppen

Als u stoppen van een taak die momenteel wordt uitgevoerd wilt, klikt u op de naam van de actieve taak die u wilt stoppen. Kies de **stoppen** knop in het deelvenster. U ziet dat de status van de taak is gewijzigd om aan te geven dat de taak is gestopt.

> [!NOTE]
> Wanneer een taak is gestopt, kan niet opnieuw opgestart. U moet een andere taak maken met de gewenste operations en apparaten.

## <a name="view-the-job-status"></a>Status van de taak weergeven

Als een taak eenmaal is gemaakt, de **Status** kolom wordt bijgewerkt met de meest recente statusbericht van de taak. De statusberichten betekenen het volgende:

| Statusbericht       | Betekenis van status                                          |
| -------------------- | ------------------------------------------------------- |
| Voltooid            | Deze taak is uitgevoerd op alle apparaten.              |
| Mislukt               | Deze taak is mislukt en niet volledig worden uitgevoerd op apparaten.  |
| In behandeling              | Deze taak nog niet is gestart op apparaten wordt uitgevoerd.        |
| In uitvoering              | Deze taak wordt momenteel uitgevoerd op apparaten.             |
| Gestopt              | Deze taak is handmatig gestopt door een gebruiker.           |

Het statusbericht wordt gevolgd door een overzicht van de apparaten in de taak. Deze apparaatstatussen betekenen het volgende:

| Statusbericht       | Betekenis van status                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Geslaagd            | Het aantal apparaten waarop de taak met succes is uitgevoerd op.  |
| Mislukt               | Het aantal apparaten waarop de taak is mislukt op worden uitgevoerd.      |

### <a name="view-the-device-status"></a>Status van het apparaat weergeven

Als u wilt de status van elk apparaat weergeven in de taak, klikt u op de taaknaam. Hier ziet u de details van de job en alle van de apparaten die een deel van deze specifieke taak zijn. Naast de naam van elk apparaat ziet u een van de volgende statusberichten:

| Statusbericht       | Betekenis van status                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Voltooid            | De taak is uitgevoerd op dit apparaat.                                     |
| Mislukt               | De taak is mislukt op dit apparaat worden uitgevoerd. Het bijbehorende foutbericht ziet u meer informatie.  |
| In behandeling              | De taak is nog niet uitgevoerd op dit apparaat.                                  |

> [!NOTE]
> Als een apparaat is verwijderd, kunt u zich niet om het apparaat te selecteren en deze wordt weergegeven als verwijderd met de apparaat-ID.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u taken maken in uw toepassing met Azure IoT Central, hier andere de volgende stappen te volgen:

- [Apparaatsets gebruiken](howto-use-device-sets.md)
- [Uw apparaten beheren](howto-manage-devices.md)
- [Versie van de sjabloon voor het apparaat](howto-version-devicetemplate.md)
