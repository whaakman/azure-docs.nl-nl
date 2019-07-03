---
title: Genereren van gegevens met gesimuleerde apparaat - Machine Learning in Azure IoT Edge | Microsoft Docs
description: Maak virtuele apparaten die gesimuleerde telemetrie die later kan worden gebruikt met het trainen van een machine learning-model genereren.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 666172e3685b923ca0d0e5fa02878341fcd0a216
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543875"
---
# <a name="tutorial-generate-simulated-device-data"></a>Zelfstudie: Gesimuleerde apparaatgegevens genereren

> [!NOTE]
> In dit artikel maakt deel uit van een reeks voor een zelfstudie over het gebruik van Azure Machine Learning op IoT Edge. Als u rechtstreeks in dit artikel zijn ontvangen, u wordt aangeraden te beginnen met de [eerste artikel](tutorial-machine-learning-edge-01-intro.md) in de reeks voor de beste resultaten.

In dit artikel gebruiken we machine learning Traininggegevens voor het simuleren van een apparaat verzenden van telemetrie naar IoT Hub. Zoals vermeld in de inleiding, deze end-to-end zelfstudie wordt gebruikgemaakt van de [Turbofan engine verslechtering van de simulatie data set](https://c3.nasa.gov/dashlink/resources/139/) voor het simuleren van gegevens uit een verzameling van vliegtuig-engines voor trainings- en testdoeleinden.

Vanaf het bijbehorende bestand Leesmij.txt weten we dat:

* De gegevens bestaat uit meerdere multidimensionale tijdreeks
* Elke gegevensset is onderverdeeld in trainings- en testset subsets
* Elke tijdreeksen afkomstig is van een andere engine
* Elke motor begint met verschillende graden van de eerste slijtage en productie-VARIANT

Voor deze zelfstudie gebruiken we de subset van de training gegevens van een enkele gegevensset (FD003).

Elke motor zijn in feite een onafhankelijke IoT-apparaat. Ervan uitgaande dat u geen een verzameling van de turbofan internetverbinding motoren beschikbaar hebt, maakt u een software-aanduiding voor deze apparaten.

De simulator is een C# programma dat gebruikmaakt van de IoT Hub-API's via een programma virtuele apparaten registreren bij IoT Hub. We lezen van de gegevens voor elk apparaat van de subset van de NASA geleverde gegevens en deze verzenden naar uw IoT-hub met behulp van een gesimuleerd IoT-apparaat. De code voor dit gedeelte van de zelfstudie kan worden gevonden in de map DeviceHarness van de opslagplaats.

Het project DeviceHarness is een .NET core-project die zijn geschreven in C# die bestaat uit vier klassen:

* **Programma:** Het toegangspunt voor de uitvoering van die verantwoordelijk is voor het verwerken van invoer van de gebruiker en de algehele coördinatie.
* **TrainingFileManager:** die verantwoordelijk is voor het lezen en parseren van het bestand van de geselecteerde gegevens.
* **CycleData:** vertegenwoordigt één rij met gegevens in een bestand geconverteerd naar berichtindeling.
* **TurbofanDevice:** die verantwoordelijk is voor het maken van een IoT-apparaat die overeenkomt met een enkel apparaat (tijdreeks) in de gegevens en verzenden van gegevens naar IoT Hub via de IoT-apparaat.

De taken die worden beschreven in dit artikel duurt ongeveer 20 minuten om te voltooien.

De werkelijke gelijk aan het werk in deze stap zou waarschijnlijk worden uitgevoerd door de apparaat-ontwikkelaars als cloudontwikkelaars.

## <a name="configure-visual-studio-code-and-build-deviceharness-project"></a>Configureren van Visual Studio Code en bouw DeviceHarness project

1. Open een extern-bureaubladsessie met uw virtuele machine, zoals in het vorige artikel wordt gedemonstreerd.

1. Open Visual Studio Code.

1. Selecteer in Visual Studio Code, **bestand** > **map openen...** .

1. In de **map** tekstvak, voer `C:\source\IoTEdgeAndMlSample\DeviceHarness` en klikt u op de **map selecteren** knop.

   Als OmniSharp fouten worden weergegeven in het uitvoervenster weergegeven, moet u verwijderen de C# -extensie, sluit en open VS Code, de installatie van de C# extensie en klik vervolgens opnieuw het venster laden.

1. Omdat u extensies op deze computer voor het eerst gebruikt, wordt de bepaalde uitbreidingen bijgewerkt en de bijbehorende afhankelijkheden installeren. U mogelijk gevraagd om bij te werken van extensie. Als dit het geval is, selecteert u **Reload Window**.

1. U wordt gevraagd om toe te voegen vereiste assets voor DeviceHarness. Selecteer **Ja** aan toe te voegen.

   * De melding duurt een paar seconden worden weergegeven.
   * Als u deze melding hebt gemist, controleert u het ' belpictogram ' in de rechterbenedenhoek.

   ![VS Code-extensie pop-upvenster](media/tutorial-machine-learning-edge-03-generate-data/add-required-assets.png)

1. Selecteer **herstellen** om terug te zetten van de pakketafhankelijkheden.

   ![VS Code terugzetten prompt](media/tutorial-machine-learning-edge-03-generate-data/restore-package-dependencies.png)

1. Valideren dat uw omgeving goed is ingesteld door een build activeren `Ctrl + Shift + B` of **Terminal** > **bouwen taak uitvoeren**.

1. U wordt gevraagd om te selecteren van de build-taak uit te voeren. Selecteer **bouwen**.

1. De build wordt uitgevoerd en voert een bericht weergegeven.

   ![Uitvoerbericht build is voltooid](media/tutorial-machine-learning-edge-03-generate-data/build-success.png)

1. U kunt hiervan de standaard-build-taak kunt maken met het selecteren van **Terminal** > **taak standaard bouwen configureren...**  en het kiezen van **bouwen** achter de opdrachtprompt.

## <a name="connect-to-iot-hub-and-run-deviceharness"></a>Verbinding maken met IoT Hub en DeviceHarness uitvoeren

Nu dat we het project bouwen hebben, verbinden met uw IoT-hub voor toegang tot de verbindingsreeks en de voortgang van het genereren van de gegevens.

### <a name="sign-in-to-azure-in-visual-studio-code"></a>Aanmelden bij Azure in Visual Studio Code

1. Meld u aan bij uw Azure-abonnement in Visual Studio Code door het openen van het opdrachtenpalet `Ctrl + Shift + P` of **weergave** > **Command Palette...** .

1. Bij de prompt zoeken voor en selecteert u een **Azure: Meld u aan**.

1. Een browservenster wordt geopend en u wordt gevraagd uw referenties. Wanneer u wordt omgeleid naar een pagina geslaagd, kunt u de browser sluiten.

### <a name="connect-to-your-iot-hub-and-retrieve-hub-connection-string"></a>Verbinding maken met uw IoT-hub en hub-verbindingsreeks ophalen

1. Selecteer in het onderste gedeelte van de Verkenner van Visual Studio Code, de **Azure IoT Hub-apparaten** frame uit te vouwen.

1. Klik in het uitgebreide frame op **IoT-Hub selecteren**.

1. Wanneer u hierom wordt gevraagd, selecteert u uw Azure-abonnement en vervolgens uw IoT-hub.

1. Klik op de **Azure IoT Hub-apparaten** frame en klikt u op **...**  meer acties. Selecteer **kopie IoT Hub-verbindingsreeks**.

   ![IoT Hub-verbindingsreeks kopiëren](media/tutorial-machine-learning-edge-03-generate-data/copy-hub-connection-string.png)

### <a name="run-the-deviceharness-project"></a>Voer het project DeviceHarness

1. Selecteer **weergave** > **Terminal** openen van de Visual Studio Code-terminal.

   Als u een prompt niet ziet, selecteert u Enter.

1. Voer `dotnet run` in de terminal.

1. Wanneer u hierom wordt gevraagd voor de IoT Hub-verbindingsreeks, plak de verbindingsreeks in de vorige sectie hebt gekopieerd.

1. In de **Azure IoT Hub-apparaten** frame, klikt u op de vernieuwknop.

   ![Lijst met IoT Hub-apparaten vernieuwen](media/tutorial-machine-learning-edge-03-generate-data/refresh-hub-device-list.png)

1. Houd er rekening mee dat apparaten worden toegevoegd aan de IoT-Hub en dat de apparaten worden weergegeven in het groen om aan te geven dat de gegevens worden verzonden via dat apparaat.

1. U vindt de berichten worden verzonden naar de hub met de rechtermuisknop op elk apparaat en selecteert u **Start Monitoring ingebouwde gebeurtenis eindpunt**. De berichten worden weergegeven in het deelvenster Uitvoer in Visual Studio Code.

1. Beëindigen van de controle door te klikken in de **Azure IoT Hub Toolkit** deelvenster Uitvoer en kies **Stop Monitoring ingebouwde gebeurtenis eindpunt**.

1. Zodat de toepassing volledig, die een paar minuten duurt worden uitgevoerd.

## <a name="check-iot-hub-for-activity"></a>IoT Hub voor activiteit controleren

De gegevens die worden verzonden door de DeviceHarness ging naar uw IoT-hub. Het is gemakkelijk om te controleren of dat gegevens uw hub met behulp van de Azure portal heeft bereikt.

1. Open de [Azure-portal](https://portal.azure.com/) en navigeer naar uw IoT-hub.

1. Op de overzichtspagina ziet u dat gegevens zijn verzonden naar de hub:  

   ![Apparaat naar cloud-berichten in IoT Hub weergeven](media/tutorial-machine-learning-edge-03-generate-data/iot-hub-usage.png)

## <a name="validate-data-in-azure-storage"></a>Valideren van gegevens in Azure Storage

De gegevens die we zojuist hebt verzonden naar uw IoT hub is doorgestuurd naar de opslagcontainer die we in het vorige artikel hebt gemaakt. Bekijk de gegevens in onze storage-account.

1. Ga in Azure Portal naar uw opslagaccount.

1. Selecteer in de navigator storage account **Opslagverkenner (preview)** .

1. Selecteer in de storage explorer **Blobcontainers** vervolgens **devicedata**.

1. Klik in het inhoudsvenster op in de map voor de naam van de IoT-hub, en vervolgens het jaar, de maand, de dag en het uur. Hier ziet u verschillende mappen voor het aantal minuten wanneer de gegevens zijn geschreven.

   ![Mappen weergeven in blob-opslag](media/tutorial-machine-learning-edge-03-generate-data/confirm-data-storage-results.png)

1. Klik op een van deze mappen om te zoeken met het label gegevensbestanden **00** en **01** overeenkomt met de partitie.

1. De bestanden zijn geschreven in [Avro](https://avro.apache.org/) indeling, maar te dubbelklikken op een van deze bestanden wordt een nieuw browsertabblad geopend en de gegevens gedeeltelijk weergegeven. Als in plaats daarvan wordt u gevraagd het bestand te openen in een programma, kunt u VS Code en het correct worden weergegeven.

1. Er is niet nodig om te proberen te lezen of te interpreteren van de gegevens nu; uitvoeren in het volgende artikel.

## <a name="next-steps"></a>Volgende stappen

In dit artikel gebruikt we een .NET Core-project maken van een set virtuele apparaten en gegevens verstuurt via deze apparaten via onze IoT-Hub en in een Azure Storage-container. Dit project simuleert een Praktijkscenario waar de gegevens met sensorwaarden, serverlogs, operationele instellingen, fout signalen en modi, enzovoort, naar een IoT Hub en ten minste een gecureerde opslag voor het verzenden van fysieke apparaten. Als er onvoldoende gegevens zijn verzameld, we gebruiken met het trainen van modellen die de resterende levensduur (RUL) van het apparaat, die worden toegelicht in het volgende artikel.

Doorgaan met het volgende artikel voor het trainen van een machine learning-model met de gegevens.

> [!div class="nextstepaction"]
> [Trainen en implementeren van een Azure Machine Learning-model](tutorial-machine-learning-edge-04-train-model.md)
