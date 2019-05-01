---
title: Een taak in de cloud Azure Stream Analytics maken in Visual Studio Code (Preview)
description: In deze Quick Start laat zien hoe u aan de slag met het maken van een Stream Analytics-taak, het configureren van de invoer en uitvoer en het definiëren van een query met Visual Studio Code.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/06/2019
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: dd6d527020bbf5e2fb510fa9605af408673e89dd
ms.sourcegitcommit: e729629331ae10097a081a03029398525f4147a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64514234"
---
# <a name="quickstart-create-an-azure-stream-analytics-cloud-job-in-visual-studio-code-preview"></a>Quickstart: Een taak in de cloud Azure Stream Analytics maken in Visual Studio Code (Preview)

Deze quickstart laat zien hoe u maakt en een Stream Analytics-taak uitvoeren met de Azure Stream Analytics-extensie voor Visual Studio Code. De voorbeeldtaak leest streaminggegevens vanaf een IoT Hub-apparaat. U definieert een taak die de gemiddelde temperatuur bij meer dan 27° berekent en de resulterende uitvoergebeurtenissen naar een nieuw bestand in blobopslag schrijft.

## <a name="before-you-begin"></a>Voordat u begint

* Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) aan.

* Meld u aan bij [Azure Portal](https://portal.azure.com/).

* Installeer [Visual Studio Code](https://code.visualstudio.com/).

* Download de [Azure Stream Analytics-extensie voor VS Code](https://usqldownload.blob.core.windows.net/ext/asa/vscode-asa-0.0.2.vsix).

## <a name="install-the-azure-stream-analytics-extension"></a>De Azure Stream Analytics-extensie installeren

Azure Stream Analytics-extensie installeren uit het persoonlijke VSIX-pakket dat u hebt gedownload.

1. Open Visual Studio Code.

2. Van **extensies** in het linkerdeelvenster, selecteer het beletselteken **(...)**  in de rechterbovenhoek. Selecteer vervolgens **installeren vanaf VSIX**.

   ![Vanaf VSIX in Visual Studio Code installeren](./media/quick-create-vs-code/install-vsix.png)

3. Kies de extensie die u hebt gedownload als een vereiste en selecteer **installeren**.  Dit kan enkele seconden duren.

4. Wanneer de installatie is voltooid, selecteert u **nu opnieuw laden** in het pop-upvenster als u wordt gevraagd.

5. Controleer **Azure Stream Analytics-hulpprogramma's** zichtbaar is in uw **ingeschakeld extensies**.

   ![Azure Stream Analytics-hulpprogramma's onder ingeschakelde uitbreidingen in Visual Studio Code](./media/quick-create-vs-code/enabled-extensions.png)

## <a name="activate-the-azure-stream-analytics-extension"></a>Activeren van de Azure Stream Analytics-extensie

1. Selecteer de **Azure** pictogram op de balk van de activiteit VS Code. **Stream Analytics** zichtbaar in de zijbalk. Onder **Stream Analytics**, selecteer **aanmelden bij Azure**. 

   ![Aanmelden bij Azure in Visual Studio Code](./media/quick-create-vs-code/azure-sign-in.png)

2. Wanneer u bent aangemeld, is de naam van uw Azure-account wordt weergegeven op de statusbalk in de linkerbenedenhoek van het venster VS Code.

> [!NOTE]
> Azure Stream Analytics-hulpprogramma's wordt automatisch in de volgende keer dat zich als u niet afmelden. Als uw account verificatie met twee factoren heeft, verdient het aanbeveling telefonische verificatie in plaats van gebruik van een PINCODE te gebruiken.
> Als u problemen met resources hebt, kunt afmelden en meldt u zich meestal opnieuw. Als u wilt afmelden, voer de opdracht `Azure: Sign Out`.

## <a name="prepare-the-input-data"></a>De invoergegevens voorbereiden

Voordat u de Stream Analytics-taak definieert, moet u de gegevens voorbereiden die later worden geconfigureerd als de taakinvoer. Voltooi de volgende stappen om de invoergegevens voor te bereiden die zijn vereist voor de taak:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer **Een resource maken** > **Internet of Things** > **IoT Hub**.

3. Voer in het deelvenster **IoT Hub** de volgende informatie in:
   
   |**Instelling**  |**Voorgestelde waarde**  |**Beschrijving**  |
   |---------|---------|---------|
   |Abonnement  | \<Uw abonnement\> |  Selecteer het Azure-abonnement dat u wilt gebruiken. |
   |Resourcegroep   |   asaquickstart-resourcegroup  |   Selecteer **Nieuwe maken** en voer een naam voor de nieuwe resourcegroep voor uw account in. |
   |Regio  |  \<Selecteer de regio die het dichtst bij uw gebruikers is gelegen\> | Selecteer een geografische locatie waar u de IoT-hub kunt hosten. Gebruik de locatie die het dichtst bij uw gebruikers is. |
   |Naam van de IoT-hub  | MyASAIoTHub  |   Selecteer een naam voor de IoT-hub.   |

   ![Een IoT Hub maken](./media/quick-create-vs-code/create-iot-hub.png)

4. Selecteer **Volgende: Grootte instellen en schaal aanpassen**.

5. Kies uw **prijs- en schaalcategorie**. Selecteer voor deze quickstart de categorie **F1 - Gratis** als deze nog beschikbaar is voor uw abonnement. Als de categorie Gratis niet beschikbaar is, kiest u de laagste beschikbare categorie. Zie [Prijsinformatie IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/) voor meer informatie.

   ![Grootte en schaal van de IoT-hub aanpassen](./media/quick-create-vs-code/iot-hub-size-and-scale.png)

6. Selecteer **Controleren + maken**. Controleer de informatie van de IoT-hub en klik op **Maken**. Het kan enkele minuten duren voordat de IoT-hub is gemaakt. U kunt de voortgang bewaken via het deelvenster **Meldingen**.

7. Klik in het IoT Hub-navigatiemenu onder **IoT-apparaten** op **Toevoegen**. Voeg een **Apparaat-id** toe en klik op **Opslaan**.

   ![Een apparaat toevoegen aan uw IoT-hub](./media/quick-create-vs-code/add-device-iot-hub.png)

8. Zodra het apparaat is gemaakt, wordt het geopend vanuit de lijst **IoT-apparaten**. Kopieer de **Verbindingsreeks -- primaire sleutel** en sla deze in een kladblok op voor later gebruik.

   ![Verbindingsreeks voor IoT Hub-apparaat kopiëren](./media/quick-create-vs-code/save-iot-device-connection-string.png)

## <a name="create-blob-storage"></a>Blob-opslag maken

1. Selecteer in de linkerbovenhoek in Azure Portal **Een resource maken** > **Storage** > **Storage-account**.

2. Voer in het deelvenster **Opslagaccount maken** een opslagaccountnaam, locatie en resourcegroep in. Kies dezelfde locatie en resourcegroep als de IoT-hub die u hebt gemaakt. Klik vervolgens op **Controleren en maken** om het account te maken.

   ![Een opslagaccount maken](./media/quick-create-vs-code/create-storage-account.png)

3. Zodra het opslagaccount is gemaakt, selecteert u in het deelvenster **Overzicht** de tegel **Blobs**.

   ![Overzicht van opslagaccounts](./media/quick-create-vs-code/blob-storage.png)

4. Selecteer op de pagina **Blob Service** de optie **Container** en geef een naam op voor de container, bijvoorbeeld *container1*. Laat **Niveau openbare toegang** staan op **Privé (geen anonieme toegang)** en selecteer **OK**.

   ![Blob-container maken](./media/quick-create-vs-code/create-blob-container.png)

## <a name="create-a-stream-analytics-project"></a>Een Stream Analytics-project maken

1. Druk in Visual Studio Code, op **Ctrl + Shift + P** het opdrachtenpalet te openen. Typ vervolgens **ASA** en selecteer **ASA: Nieuw Project maken**.

   ![Nieuw project maken](./media/quick-create-vs-code/create-new-project.png)

2. Voer de naam van uw project, zoals **myASAproj** en selecteer een map voor uw project.

    ![Naam van het project maken](./media/quick-create-vs-code/create-project-name.png)

3. Het nieuwe project wordt toegevoegd aan uw werkruimte. Een ASA-project bestaat uit het query-script **(*.asaql)**, een **JobConfig.json** -bestand en een **asaproj.json** configuratiebestand.

   ![Stream Analytics-projectbestanden in VS Code](./media/quick-create-vs-code/asa-project-files.png)

4. Het configuratiebestand asaproj.json bevat de invoer, uitvoer en taak bestand configuratiegegevens die nodig zijn voor het indienen van de ASA-taak naar Azure.

   ![Stream Analytics-taak-configuratiebestand in VS Code](./media/quick-create-vs-code/job-configuration.png)

> [!Note]
> Bij het toevoegen van invoer en uitvoer van het opdrachtenpalet, wordt de bijbehorende paden worden toegevoegd aan **asaproj.json** automatisch. Als u toevoegt of rechtstreeks in- of uitvoer op schijf verwijderen, moet u handmatig toevoegen of verwijderen van **asaproj.json**. U kunt kiezen om de invoer en uitvoer in een plaats en ernaar te verwijzen in verschillende taken door de paden op te geven in elk **asaproj.json**.

## <a name="define-an-input"></a>Invoer definiëren

1. Selecteer **Ctrl + Shift + P** het opdrachtenpalet te openen en voeren **ASA: Invoer toevoegen**.

   ![Stream Analytics-invoer in VS Code toevoegen](./media/quick-create-vs-code/add-input.png)

2. Kies **IoT-Hub** voor het invoertype.

   ![IoT-Hub als invoer optie selecteren](./media/quick-create-vs-code/iot-hub.png)

3. Kies de ASA-query-script dat wordt gebruikt voor de invoer. Het moet wordt automatisch gevuld met het pad naar **myASAproj.asaql**.

   ![Selecteer een ASA-script in Visual Studio Code](./media/quick-create-vs-code/asa-script.png)

4. Voer de naam van het bestand voor invoer als **IotHub.json**.

5. Bewerken **IoTHub.json** met de volgende waarden. Houd de standaardwaarden voor de velden niet die hieronder worden vermeld. De CodeLens gebruiken om u te helpen bij het invoeren van een tekenreeks of selecteert u in een vervolgkeuzelijst weergegeven.

   |Instelling|Voorgestelde waarde|Description|
   |-------|---------------|-----------|
   |Name|Invoer|Voer een unieke naam in voor de invoer van de taak.|
   |IotHubNamespace|MyASAIoTHub|Kies uw IoT-hub, of voer de naam ervan in. Namen van IoT-hubs worden automatisch gedetecteerd als ze worden gemaakt in hetzelfde abonnement.|
   |Eindpunt|Berichten| |
   |SharedAccessPolicyName|iothubowner| |

## <a name="define-an-output"></a>Uitvoer definiëren

1. Selecteer **Ctrl + Shift + P** het opdrachtenpalet te openen. Voer vervolgens **ASA: Uitvoer toevoegen**.

   ![Stream Analytics-uitvoer in VS Code toevoegen](./media/quick-create-vs-code/add-output.png)

2. Kies **Blob-opslag** voor het Sink-type.

3. Kies de ASA-query-script dat wordt gebruikt voor deze invoer.

4. Voer de naam van het uitvoerbestand als **BlobStorage.json**.

5. Bewerken **BlobStorage.json** met de volgende waarden. Houd de standaardwaarden voor de velden niet die hieronder worden vermeld. De CodeLens gebruiken om u te helpen bij het invoeren van een tekenreeks of selecteert u in een vervolgkeuzelijst weergegeven.

   |Instelling|Voorgestelde waarde|Description|
   |-------|---------------|-----------|
   |Name|Uitvoer| Voer een unieke naam voor uitvoer van de taak.|
   |Opslagaccount|asaquickstartstorage|Kies of typ de naam van uw opslagaccount. Namen van opslagaccounts worden automatisch gedetecteerd als ze worden gemaakt in hetzelfde abonnement.|
   |Container|container1|Selecteer de bestaande container die u in uw opslagaccount hebt gemaakt.|
   |Padpatroon|output|Voer de naam in van een bestandspad dat in de container moet worden gemaakt.|

## <a name="define-the-transformation-query"></a>De transformatiequery definiëren

1. Open **myASAproj.asaql** van de projectmap.

2. Voeg de volgende query toe:

   ```sql
   SELECT * 
   INTO Output
   FROM Input
   HAVING Temperature > 27
   ```

## <a name="compile-script"></a>Script compileren

Script compilatie doet twee dingen: Controleer de syntaxis en de Azure Resource Manager-sjablonen voor automatische implementatie te genereren.

Er zijn twee manieren voor het activeren van de compilatie script:

1. Het script van de werkruimte te selecteren en vervolgens compileren vanuit het opdrachtenpalet te activeren. 

   ![Gebruik het opdrachtenpalet van VS Code voor het compileren van het script](./media/quick-create-vs-code/compile-script1.png)

2. Klik met de rechtermuisknop op het script en selecteer **ASA: script compileren**.

    ![Met de rechtermuisknop op de ASA-script voor het compileren](./media/quick-create-vs-code/compile-script2.png)

3. Na de compilatie, vindt u de twee gegenereerde Azure Resource Manager-sjablonen in **implementeren** map van uw project. Deze twee bestanden worden gebruikt voor automatische implementatie.

## <a name="submit-a-stream-analytics-job-to-azure"></a>Een Stream Analytics-taak naar Azure verzenden

1. Selecteer in de script-editor-venster van Visual Studio Code **selecteren bij uw abonnementen**.

   ![Selecteer in de tekst van uw abonnementen in script-editor](./media/quick-create-vs-code/select-subscription.png)

2. Selecteer uw abonnement in de lijst met het pop-upvenster.

3. Selecteer **selecteert u een taak**. Kies maken vervolgens een nieuwe taak.

4. Voer de taaknaam van uw **myASAjob** en volg de instructies voor het kiezen van de resourcegroep en locatie.

5. Selecteer **verzenden naar Azure**. De logboeken vindt u in het uitvoervenster weergegeven. 

6. Wanneer de taak is gemaakt, kunt u deze zien in de Stream Analytics-Verkenner.

## <a name="run-the-iot-simulator"></a>De IoT-simulator uitvoeren

1. Open de [Raspberry Pi Azure IoT Online Simulator](https://azure-samples.github.io/raspberry-pi-web-simulator/) in een nieuw browsertabblad of -venster.

2. Vervang de tijdelijke aanduiding in regel 15 door de verbindingsreeks van het Azure IoT Hub-apparaat die u hebt opgeslagen in de vorige sectie.

3. Klik op **Run**. De uitvoer geeft de sensorgegevens en berichten weer die worden verzonden naar de IoT-hub.

   ![Raspberry Pi Azure IoT Online Simulator](./media/quick-create-vs-code/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-output"></a>Stream Analytics-taak starten en uitvoer controleren

1. Stream Analytics Explorer openen in Visual Studio Code en zoek uw taak,**myASAJob**.

2. Met de rechtermuisknop op de taaknaam. Selecteer **Start** in het contextmenu.

3. Kies **nu** in het pop-upvenster om de taak te starten.

4. Houd er rekening mee de taak de status is gewijzigd in **met**. Met de rechtermuisknop op de taaknaam van de en kies **Open taak weergeven in Portal** om te zien van de invoer en uitvoer van de gebeurtenis metrische gegevens. Dit kan enkele minuten duren.

5. De resultaten weergeven, opent u de blob-opslag in de Visual Studio Code-extensie of in Azure portal.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u een resourcegroep niet meer nodig hebt, verwijdert u de resourcegroep, de streamingtaak en alle gerelateerde resources. Door de taak te verwijderen, voorkomt u dat de streaming-eenheden die door de taak worden verbruikt, in rekening worden gebracht. Als u denkt dat u de taak in de toekomst nog gaat gebruiken, kunt u deze stoppen en later opnieuw starten wanneer dat nodig is. Als u deze taak niet meer gaat gebruiken, verwijdert u alle resources die in deze snelstart zijn gemaakt. Daarvoor voert u de volgende stappen uit:

1. Selecteer in het menu aan de linkerkant in Azure Portal de optie **Resourcegroepen** en selecteer vervolgens de resource die u hebt gemaakt.  

2. Selecteer op de pagina van uw resourcegroep de optie **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en selecteer vervolgens **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een eenvoudige Stream Analytics-taak met behulp van Visual Studio Code geïmplementeerd. U kunt ook implementeren met Stream Analytics-taken met behulp van de [Azure-portal](stream-analytics-quick-create-portal.md), [PowerShell](stream-analytics-quick-create-powershell.md), en Visual Studio (stream-analytics-quick-maken-vs.md). 

Ga voor meer informatie over Azure Stream Analytics-hulpprogramma's voor Visual Studio naar het volgende artikel:

> [!div class="nextstepaction"]
> [Visual Studio gebruiken om Azure Stream Analytics-taken weer te geven](stream-analytics-vs-tools.md)