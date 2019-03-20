---
title: Maken en uitvoeren van taken in uw toepassing met Azure IoT Central | Microsoft Docs
description: Azure IoT Central-taken kunnen bulksgewijs beheermogelijkheden voor apparaten, zoals een apparaateigenschap bijwerkt, instelling of een opdracht is uitgevoerd.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 03/18/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: ec7033719316bb186408ea78f6dabac43c383491
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58199848"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Maken en uitvoeren van een taak in uw Azure IoT Central-toepassing

U kunt Microsoft Azure IoT Central gebruiken voor het beheren van uw verbonden apparaten op schaal met behulp van taken. Taken kunt u bulksgewijs updates van apparaateigenschappen, instellingen en opdrachten. In dit artikel leert u hoe u aan de slag met behulp van taken in uw eigen toepassing.

## <a name="create-and-run-a-job"></a>Maken en uitvoeren van een taak

Deze sectie leest u hoe u maken en een taak uitvoert. Hier ziet u hoe u kunt de snelheid van de ventilator voor meerdere gekoeld verkoopautomaten verhogen.

1. Navigeer naar de taken in het navigatiedeelvenster.

1. Selecteer **+ nieuw** om een nieuwe taak te maken.

    ![Nieuwe taak maken](./media/howto-run-a-job/createnewjob.png)

1. Voer een naam en beschrijving in voor de taak die u maakt.

1. Selecteer die het apparaat zo ingesteld dat u wilt dat uw taak toe te passen op. Nadat het apparaat te selecteren hebt ingesteld, ziet u de rechterkant te vullen met de apparaten in de set met apparaten. Als u een apparaatset verbroken selecteert, er zijn geen apparaten worden weergegeven en ziet u een bericht dat het instellen van uw apparaat verbroken is.

1. Kies vervolgens het type van de taak voor het definiëren van (een instelling, eigenschap of opdracht). Selecteer **+** naast het type taak ingeschakeld en uw bewerkingen toevoegen.

    ![Taak configureren](./media/howto-run-a-job/configurejob.png)

1. Kies de apparaten die u wilt de taak wordt uitgevoerd aan de rechterkant. Als u het selectievakje boven selecteert, worden alle apparaten geselecteerd in het hele apparaat instellen. Door het selectievakje in de buurt **naam**, alle apparaten in de huidige pagina zijn geselecteerd.

1. Na het selecteren van uw apparaten, kiest u **uitvoeren** of **opslaan**. De taak wordt nu weergegeven op de hoofdpagina **taken** pagina. In deze weergave ziet u de momenteel actieve taak en de geschiedenis van een eerder taken uitvoeren. De actieve taak wordt altijd weergegeven boven aan de lijst. Uw opgeslagen taak kan worden geopend opnieuw op elk gewenst moment om door te gaan bewerken of uit te voeren.

    ![Taak weergeven](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > U kunt de geschiedenis van uw eerder uitgevoerde taken weergeven voor 30 dagen.

1. Als u een overzicht van uw taak, selecteert u de taak om weer te geven in de lijst. In dit overzicht bevat de taakdetails, apparaten en statussen van een apparaat. In dit overzicht kunt u ook selecteren **taakdetails downloaden** voor het downloaden van een CSV-bestand van uw taakdetails, met inbegrip van de apparaten en hun waarden voor status. Deze informatie kan nuttig zijn voor het oplossen van problemen zijn.

    ![Apparaatstatus weergeven](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Een actieve taak stoppen

Als u wilt stoppen met een actieve taak, selecteert u deze en kies **stoppen** in het venster. De taak status is gewijzigd in overeenstemming met de taak is gestopt.

   ![Taak stoppen](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Een gestopte taak uitvoeren

Als u wilt een taak die momenteel gestopt uitvoert, selecteert u de gestopte taak. Kies **uitvoeren** in het venster. De taak status is gewijzigd in overeenstemming met de taak wordt nu opnieuw uitgevoerd.

   ![Taak hervatten](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>Een taak voor het kopiëren

Als u wilt kopiëren van een bestaande taak die u hebt gemaakt, selecteert u deze in de belangrijkste taken pagina en selecteer **kopie**. Een nieuwe kopie van de taakconfiguratie van de wordt geopend om te bewerken. U kunt opslaan of de nieuwe taak uitvoeren. Als wijzigingen zijn aangebracht aan de set geselecteerde apparaat, zijn ze weerspiegeld in deze gekopieerde taak voor u om te bewerken.

   ![Taak voor het kopiëren](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>Status van de taak weergeven

Nadat een taak is gemaakt, de **Status** updates van de kolom met de meest recente statusbericht van de taak. De volgende tabel bevat de van de mogelijke statuswaarden:

| Statusbericht       | Betekenis van status                                          |
| -------------------- | ------------------------------------------------------- |
| Voltooid            | Deze taak is uitgevoerd op alle apparaten.              |
| Mislukt               | Deze taak is mislukt en niet volledig worden uitgevoerd op apparaten.  |
| In behandeling              | Deze taak nog niet nog gestart op apparaten wordt uitgevoerd.         |
| In uitvoering              | Deze taak wordt momenteel uitgevoerd op apparaten.             |
| Gestopt              | Deze taak is handmatig gestopt door een gebruiker.           |

Het statusbericht wordt gevolgd door een overzicht van de apparaten in de taak. De volgende tabel bevat de waarden van de status van mogelijke apparaten:

| Statusbericht       | Betekenis van status                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Geslaagd            | Het aantal apparaten dat de taak is uitgevoerd op.       |
| Mislukt               | Het aantal apparaten dat de taak is mislukt op worden uitgevoerd.       |

### <a name="view-the-device-status"></a>Status van het apparaat weergeven

Als u wilt de status van de taak en de betreffende apparaten weergeven, selecteert u de taak. Selecteer om te downloaden van een CSV-bestand met de taakdetails, met inbegrip van de lijst met apparaten en hun statuswaarden **downloaden taakdetails**. Naast de naam van elk apparaat ziet u een van de volgende statusberichten:

| Statusbericht       | Betekenis van status                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Voltooid            | De taak is uitgevoerd op dit apparaat.                                     |
| Mislukt               | De taak is mislukt op dit apparaat worden uitgevoerd. Het foutbericht ziet u meer informatie.  |
| In behandeling              | De taak niet is nog uitgevoerd op dit apparaat.                                   |

> [!NOTE]
> Als een apparaat is verwijderd, u kunt het apparaat niet selecteren en deze wordt weergegeven als verwijderd met de apparaat-ID.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u taken maken in uw toepassing met Azure IoT Central, hier andere de volgende stappen te volgen:

- [Apparaatsets gebruiken](howto-use-device-sets.md)
- [Uw apparaten beheren](howto-manage-devices.md)
- [Versie van de sjabloon voor het apparaat](howto-version-devicetemplate.md)
