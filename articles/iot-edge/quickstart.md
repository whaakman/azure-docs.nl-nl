---
title: 'Snelstartgids: een Azure IoT Edge-apparaat maken in Windows | Microsoft Docs'
description: In deze snelstartgids leert u hoe u een IoT Edge-apparaat maakt en daarna kant-en-klare code op afstand implementeert vanuit Azure Portal.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/19/2019
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: bc859dc1e33abfee765a8f5b0f2a65bc24b7c2dc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226943"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Snelstartgids: Uw eerste IoT Edge-module van Azure Portal naar een Windows-apparaat implementeren - preview

In deze snelstart gebruikt u de cloudinterface van Azure IoT Edge om vooraf geschreven code op afstand naar een IoT Edge-apparaat te implementeren. Deze taak wordt uitgevoerd, eerst maken en configureren van een Windows virtuele machine om te werken als een IoT Edge-apparaat, kunt u vervolgens een module te implementeren.

In deze snelstart leert u de volgende zaken:

1. Een IoT Hub maken.
2. Een IoT Edge-apparaat registreren in uw IoT-hub.
3. De IoT Edge-runtime op uw apparaat installeren en starten.
4. Op afstand een module op een IoT Edge-apparaat implementeren en telemetrie naar IoT Hub verzenden.

![Diagram - Snelstartarchitectuur voor apparaat en cloud](./media/quickstart/install-edge-full.png)

De module die u in deze snelstart implementeert, is een gesimuleerde sensor waarmee temperatuur-, luchtvochtigheids- en drukgegevens worden gegenereerd. De andere Azure IoT Edge-zelfstudies bouwen voort op het werk dat u hier doet door modules te implementeren waarmee de gesimuleerde gegevens worden geanalyseerd voor zakelijke inzichten.

> [!NOTE]
> De IoT Edge-runtime op Windows bevindt zich in [openbare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U gebruikt de Azure CLI om veel van de stappen in deze snelstart uit te voeren, en Azure IoT heeft een extensie om extra functionaliteit in te schakelen.

Voeg de Azure IoT-extensie toe aan het exemplaar van Cloud Shell.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Vereisten

Cloudresources:

* Een resourcegroep voor het beheren van alle resources die u in deze snelstart maakt.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

IoT Edge-apparaat:

* Een Windows virtuele machine om te fungeren als uw IoT Edge-apparaat. U kunt deze virtuele machine met behulp van de volgende opdracht en vervangt maken *{password}* met een beveiligd wachtwoord:

  ```azurecli-interactive
  az vm create --resource-group IoTEdgeResources --name EdgeVM --image MicrosoftWindowsDesktop:Windows-10:rs5-pro:latest --admin-username azureuser --admin-password {password} --size Standard_DS1_v2
  ```

  Het maken en starten van de nieuwe virtuele machine kan een paar minuten duren. Vervolgens kunt u een RDP-bestand voor gebruik bij het verbinden met uw virtuele machine downloaden:

  1. Navigeer naar uw nieuwe Windows virtuele machine in Azure portal.
  1. Selecteer **Verbinden**.
  1. Op de **RDP** tabblad **RDP-bestand downloaden**.

  Dit bestand niet openen met verbinding met extern bureaublad verbinding maken met uw Windows-machine met de beheerdersnaam van de en het wachtwoord hebt opgegeven met de `az vm create` opdracht.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Begin met de snelstart door een IoT Hub met Azure CLI te maken.

![Diagram - Een IoT-hub maken in de cloud](./media/quickstart/create-iot-hub.png)

Het gratis niveau van IoT Hub werkt voor deze snelstart. Als u in het verleden IoT Hub hebt gebruikt en al een gratis hub hebt gemaakt, kunt u die IoT-hub gebruiken. Elk abonnement biedt toegang tot slechts één gratis IoT-hub.

Met de volgende code wordt een gratis **F1**-hub gemaakt in de resourcegroep **IoTEdgeResources**. Vervang *{hub_name}* door een unieke naam voor uw IoT-hub.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1
   ```

   Als er een fout optreedt omdat er al één gratis hub in uw abonnement is, wijzigt u de SKU in **S1**. Als u het foutbericht ontvangt dat de naam van de IoT Hub niet beschikbaar is, betekent dit dat iemand anders al een hub met die naam heeft. Probeer een andere naam.

## <a name="register-an-iot-edge-device"></a>Een IoT Edge-apparaat registreren

Registreer een IoT Edge-apparaat bij uw net gemaakte IoT Hub.
![Diagram - Een apparaat registreren met een IoT Hub-entiteit](./media/quickstart/register-device.png)

Maak een apparaat-id voor uw gesimuleerde apparaat, zodat het met uw IoT-hub kan communiceren. De apparaat-id is opgeslagen in de cloud, en u gebruikt een unieke apparaatverbindingsreeks om een fysiek apparaat te koppelen aan een apparaat-id.

Omdat IoT Edge-apparaten zich anders gedragen en anders kunnen worden beheerd dan gewone IoT-apparaten, declareert u deze identiteit met een `--edge-enabled`-vlag als een identiteit van een IoT Edge-apparaat.

1. Voer in Azure Cloud Shell de volgende opdracht in om een ​​apparaat met de naam **myEdgeDevice** in uw hub te maken.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

   Als u een foutbericht over iothubowner-beleidssleutels ontvangt, controleer dan of in de cloudshell de meest recente versie van de azure-cli-iot-ext-extensie wordt uitgevoerd.

2. Haal de verbindingsreeks voor uw apparaat op, zodat uw fysieke apparaat aan de bijbehorende identiteit in IoT Hub wordt gekoppeld.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Kopieer de waarde van de sleutel `connectionString` uit de JSON-uitvoer en sla deze op. Deze waarde is de verbindingsreeks van het apparaat. In de volgende sectie gaat u deze verbindingsreeks gebruiken om de IoT Edge-runtime te configureren.

   ![Verbindingsreeks ophalen uit de CLI-uitvoer](./media/quickstart/retrieve-connection-string.png)

## <a name="install-and-start-the-iot-edge-runtime"></a>De IoT Edge-runtime installeren en starten

Installeer de Azure IoT Edge-runtime op uw IoT Edge-apparaat en configureer deze met een apparaatverbindingsreeks.
![Diagram - De runtime op apparaat starten](./media/quickstart/start-runtime.png)

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. Deze bevat drie onderdelen. De **IoT Edge-beveiligingsdaemon** wordt telkens gestart wanneer een IoT Edge-apparaat wordt opgestart en start het apparaat op door de IoT Edge-agent te starten. De **IoT Edge-agent** beheert de implementatie en bewaking van modules op het IoT Edge-apparaat, inclusief de IoT Edge-hub. De **IoT Edge-hub** verzorgt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub.

Het installatiescript bevat ook de containerengine Moby waarmee de containerinstallatiekopieën op uw IoT Edge-apparaat worden beheerd.

Tijdens de installatie van de runtime wordt u naar de apparaatverbindingsreeks gevraagd. Gebruik de tekenreeks die u hebt opgehaald via de Azure CLI. Deze tekenreeks koppelt uw fysieke apparaat aan de IoT Edge-apparaat-id in Azure.

### <a name="connect-to-your-iot-edge-device"></a>Verbinding maken met uw IoT Edge-apparaat

De stappen in deze sectie alle plaatsvinden op uw IoT Edge-apparaat, zodat u dat de virtuele machine nu via Extern bureaublad verbinding wilt maken.

### <a name="prepare-your-device-for-containers"></a>Uw apparaat voorbereiden voor containers

Het script voor installatie installeert automatisch de Moby-engine op uw apparaat voor de installatie van IoT Edge. Voorbereiden van het apparaat door het inschakelen van de Containers-functie.

1. Zoek in de startbalk **schakelt u Windows-onderdelen in- of uitschakelen** en open het Configuratiescherm van het besturingselement.
1. Zoek en selecteer **Containers**.
1. Selecteer **OK**.

Wanneer u klaar bent, moet u opnieuw opstarten van Windows om de wijzigingen door te voeren, maar u kunt dit doen vanuit de extern bureaublad-sessie in plaats van de virtuele machine van Azure portal opnieuw te starten.

### <a name="download-and-install-the-iot-edge-service"></a>De IoT Edge-service downloaden en installeren

Gebruik PowerShell om de IoT Edge-runtime te downloaden en te installeren. Gebruik de apparaatverbindingsreeks die u hebt opgehaald via IoT Hub om uw apparaat te configureren.

1. Voer PowerShell op uw IoT Edge-apparaat uit als administrator.

2. Download en installeer de IoT Edge-service op uw apparaat.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Windows
   ```

3. Als u wordt gevraagd naar een **DeviceConnectionString**, geeft u de tekenreeks op die u in de vorige sectie hebt gekopieerd. Plaats geen aanhalingstekens rond de verbindingsreeks.

### <a name="view-the-iot-edge-runtime-status"></a>De IoT Edge runtime-status bekijken

Controleer of de runtime goed is geïnstalleerd en geconfigureerd.

1. Controleer de status van de IoT Edge-service.

   ```powershell
   Get-Service iotedge
   ```

2. Als u problemen met de service moet oplossen, haalt u de servicelogboeken op.

   ```powershell
   # Displays logs from today, newest at the bottom.

   Get-WinEvent -ea SilentlyContinue `
    -FilterHashtable @{ProviderName= "iotedged";
      LogName = "application"; StartTime = [datetime]::Today} |
    select TimeCreated, Message |
    sort-object @{Expression="TimeCreated";Descending=$false} |
    format-table -autosize -wrap
   ```

3. Bekijk alle modules die op uw IoT Edge-apparaat worden uitgevoerd. Aangezien de service net voor het eerst is gestart, zou u moeten zien dat alleen de **edgeAgent**-module actief is. De edgeAgent-module wordt standaard uitgevoerd en helpt bij het installeren en starten van aanvullende modules die u op uw apparaat implementeert.

   ```powershell
   iotedge list
   ```

   ![Eén module op uw apparaat bekijken](./media/quickstart/iotedge-list-1.png)

Het duurt enkele minuten duren voordat de installatie is voltooid en de IoT Edge agent-module te starten.

Uw IoT Edge-apparaat is nu geconfigureerd. Het is gereed voor de uitvoering van modules die in de cloud zijn geïmplementeerd.

## <a name="deploy-a-module"></a>Een module implementeren

Beheer uw Azure IoT Edge-apparaat vanuit de cloud om een module te implementeren waarmee telemetriegegevens worden verzonden naar IoT Hub.
![Diagram - Module implementeren vanuit cloud op apparaat](./media/quickstart/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

In deze snelstart hebt u een IoT Edge-apparaat geregistreerd en de IoT Edge-runtime erop geïnstalleerd. Vervolgens hebt u de Azure-portal gebruikt om een IoT Edge-module te implementeren om te worden uitgevoerd op het apparaat zonder dat er wijzigingen aan het apparaat zelf hoefden te worden aangebracht.

In dit geval worden met de module die u hebt gepusht voorbeeldgegevens gemaakt die u voor testen kunt gebruiken. De gesimuleerde temperatuursensormodule genereert omgevingsgegevens die u later kunt gebruiken voor testen. De gesimuleerde sensor bewaakt zowel een machine als de omgeving rond die machine. Deze sensor kan zich bijvoorbeeld in een serverruimte, in een fabriek of op een windturbine bevinden. Het bericht bevat informatie over de omgevingstemperatuur, de luchtvochtigheid, de machinetemperatuur en de druk, evenals een tijdstempel. In de IoT Edge-zelfstudies worden gegevens van deze module gebruikt als testgegevens voor analyses.

Bevestig dat de module die vanuit de cloud is geïmplementeerd, op uw IoT Edge -apparaat wordt uitgevoerd.

```powershell
iotedge list
```

   ![Drie modules op uw apparaat bekijken](./media/quickstart/iotedge-list-2.png)

Bekijk de berichten die vanaf de temperatuursensormodule naar de cloud worden verzonden.

```powershell
iotedge logs SimulatedTemperatureSensor -f
```

   >[!TIP]
   >IoT Edge-opdrachten zijn hoofdlettergevoelig wanneer u naar modulenamen verwijst.

   ![De gegevens van uw module bekijken](./media/quickstart/iotedge-logs.png)

U kunt de berichten ook zien binnenkomen bij uw IoT Hub door de [Azure IoT Hub Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (voorheen Azure IoT Toolkit-extensie) te gebruiken.

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt doorgaan met de IoT Edge-zelfstudies, kunt u het apparaat gebruiken dat u hebt geregistreerd en ingesteld in deze snelstart. Anders kunt u de Azure-resources die u hebt gemaakt en de IoT Edge-runtime van uw apparaat verwijderen.

### <a name="delete-azure-resources"></a>Azure-resources verwijderen

Als u uw virtuele machine en IoT-hub in een nieuwe resourcegroep hebt gemaakt, kunt u die groep en alle bijbehorende resources verwijderen. Controleer de inhoud van de resourcegroep zorgvuldig om te na te gaan of er niets is dat u wilt behouden. Als u niet de hele groep wilt verwijderen, kunt u in plaats daarvan afzonderlijke resources verwijderen.

Verwijder de groep **IoTEdgeResources**.

   ```azurecli-interactive
   az group delete --name IoTEdgeResources
   ```

### <a name="remove-the-iot-edge-runtime"></a>De IoT Edge-runtime verwijderen

Als u de installaties van uw apparaat wilt verwijderen, gebruikt u de volgende opdrachten.  

Verwijder de IoT Edge-runtime. Als u van plan ben IoT Edge opnieuw te installeren, laat u de parameters `-DeleteConfig` en `-DeleteMobyDataRoot` weg, zodat u de installatie opnieuw kunt uitvoeren met de configuratie die u zojuist hebt ingesteld.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon -DeleteConfig -DeleteMobyDataRoot
   ```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een IoT Edge-apparaat gemaakt en de Azure IoT Edge-cloudinterface gebruikt om code te implementeren op het apparaat. U hebt nu een testapparaat waarmee ruwe gegevens over de omgeving worden gegenereerd.

U kunt nu doorgaan met elke andere zelfstudie om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Sensorgegevens filteren met behulp van een Azure-functie](tutorial-deploy-function.md)
