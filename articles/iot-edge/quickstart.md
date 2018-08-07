---
title: 'Snelstart: Azure IoT Edge + Windows | Microsoft Docs'
description: Azure IoT Edge uitproberen door analyses uit te voeren op een gesimuleerd Edge-apparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/24/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1437c3552a7af5d5474cf3bdaabe95d5415af603
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414208"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Snelstart: Uw eerste IoT Edge-module van Azure Portal naar een Windows-apparaat implementeren - preview

In deze snelstart gebruikt u de cloudinterface van Azure IoT Edge om vooraf geschreven code op afstand naar een IoT Edge-apparaat te implementeren. Om dit te bereiken, moet u eerst uw Windows-apparaat gebruiken om een IoT Edge-apparaat te simuleren, waarna u er een module naar kunt implementeren.

In deze snelstart leert u de volgende zaken:

1. Een IoT Hub maken.
2. Een IoT Edge-apparaat registreren in uw IoT-hub.
3. De IoT Edge-runtime op uw apparaat installeren en starten.
4. Op afstand een module op een IoT Edge-apparaat implementeren en telemetrie naar IoT Hub verzenden.

![Zelfstudiearchitectuur][2]

De module die u in deze snelstart implementeert, is een gesimuleerde sensor waarmee temperatuur-, luchtvochtigheids- en drukgegevens worden gegenereerd. De andere Azure IoT Edge-zelfstudies bouwen voort op het werk dat u hier doet door modules te implementeren waarmee de gesimuleerde gegevens worden geanalyseerd voor zakelijke inzichten. 

>[!NOTE]
>De IoT Edge-runtime op Windows bevindt zich in [openbare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account][lnk-account] aan voordat u begint.

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
   az group create --name IoTEdgeResources --location westus
   ```

Een Windows-computer of virtuele machine die fungeert als uw IoT Edge-apparaat: 

* Gebruik een ondersteunde Windows-versie:
   * Windows 10 of hoger
   * Windows Server 2016 of hoger
* Als u een virtuele machine gebruikt, schakelt u [geneste virtualisatie][lnk-nested] in en dient u ten minste 2 GB geheugen toe te wijzen. 
* Installeer [Docker voor Windows][lnk-docker] en contoleer of deze wordt uitgevoerd.
* Configureer Docker voor het gebruiken van [Linux-containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Begin met de snelstart door uw IoT Hub met Azure CLI te maken. 

![IoT Hub maken][3]

Het gratis niveau van IoT Hub werkt voor deze snelstart. Als u in het verleden IoT Hub hebt gebruikt en al een gratis hub hebt gemaakt, kunt u die IoT-hub gebruiken. Elk abonnement biedt toegang tot slechts één gratis IoT-hub. 

Met de volgende code wordt een gratis **F1**-hub gemaakt in de resourcegroep **IoTEdgeResources**. Vervang *{hub_name}* door een unieke naam voor uw IoT-hub.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Als er een fout optreedt omdat er al één gratis hub in uw abonnement is, wijzigt u de SKU in **S1**. 

## <a name="register-an-iot-edge-device"></a>Een IoT Edge-apparaat registreren

Registreer een IoT Edge-apparaat bij uw net gemaakte IoT Hub.
![Een apparaat registreren][4]

Maak een apparaat-id voor uw gesimuleerde apparaat, zodat het met uw IoT-hub kan communiceren. Omdat IoT Edge-apparaten zich anders gedragen en anders kunnen worden beheerd dan typische IoT-apparaten, geeft u vanaf het begin aan dat dit een IoT Edge-apparaat is. 

1. Voer in Azure Cloud Shell de volgende opdracht in om een ​​apparaat met de naam **myEdgeDevice** in uw hub te maken.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

1. Haal de verbindingsreeks voor uw apparaat op, zodat uw fysieke apparaat aan de bijbehorende identiteit in IoT Hub wordt gekoppeld. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Kopieer de verbindingsreeks en sla deze op. U gebruikt deze waarde voor het configureren van de IoT Edge-runtime in de volgende sectie. 

## <a name="install-and-start-the-iot-edge-runtime"></a>De IoT Edge-runtime installeren en starten

Installeer en start de Azure IoT Edge-runtime op uw IoT Edge-apparaat. 
![Een apparaat registreren][5]

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. Deze bevat drie onderdelen. De **IoT Edge-beveiligingsdaemon** wordt telkens gestart wanneer een Edge-apparaat wordt opgestart en start het apparaat op door de IoT Edge-agent te starten. De **IoT Edge-agent** faciliteert de implementatie en bewaking van modules op het IoT Edge-apparaat, inclusief de IoT Edge-hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd. 

>[!NOTE]
>De installatiestappen in deze sectie zijn voorlopig nog handmatig, terwijl een installatiescript wordt ontwikkeld. 

Met de instructies in dit gedeelte wordt de IoT Edge-runtime met Linux-containers geconfigureerd. Als u Windows-containers wilt gebruiken, raadpleegt u [Azure IoT Edge-runtime installeren onder Windows voor gebruik met Windows-containers](how-to-install-iot-edge-windows-with-windows.md).

### <a name="download-and-install-the-iot-edge-service"></a>De IoT Edge-service downloaden en installeren

1. Voer PowerShell op uw IoT Edge-apparaat uit als administrator.

2. Download het IoT Edge-servicepakket.

   ```powershell
   Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
   Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
   Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
   rmdir C:\ProgramData\iotedge\iotedged-windows
   $sysenv = "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment"
   $path = (Get-ItemProperty -Path $sysenv -Name Path).Path + ";C:\ProgramData\iotedge"
   Set-ItemProperty -Path $sysenv -Name Path -Value $path
   ```

3. Installeer de vcruntime.

  ```powershell
  Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
  .\vc_redist.exe /quiet /norestart
  ```

4. Maak en start de IoT Edge-service.

   ```powershell
   New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
   Start-Service iotedge
   ```

5. Voeg firewall-uitzonderingen toe voor de poorten die door de IoT Edge-service worden gebruikt.

   ```powershell
   New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
   ```

6. Maak een nieuw bestand met de naam **iotedge.reg** en open het met een teksteditor. 

7. Voeg de volgende inhoud toe aan het bestand en sla het op. 

   ```input
   Windows Registry Editor Version 5.00
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
   "CustomSource"=dword:00000001
   "EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
   "TypesSupported"=dword:00000007
   ```

8. Navigeer naar uw bestand in Verkenner en dubbelklik erop om de wijzigingen in het Windows-register te importeren. 

### <a name="configure-the-iot-edge-runtime"></a>De IoT Edge-runtime configureren 

Configureer de runtime met uw IoT Edge-apparaatverbindingsreeks die u hebt gekopieerd toen u een nieuw apparaat registreerde. Configureer vervolgens het runtimenetwerk. 

1. Open het IoT Edge-configuratiebestand, dat zich bevindt in `C:\ProgramData\iotedge\config.yaml`. Dit bestand is beveiligd; voer daarom een ​​teksteditor zoals Notepad uit als administrator en gebruik vervolgens de editor om het bestand te openen. 

2. Zoek de sectie **provisioning** op en werk de waarde van **device_connection_string** bij met de reeks die u hebt gekopieerd van uw IoT Edge-apparaatgegevens. 

3. Haal in het PowerShell-venster van de beheerder de hostnaam voor uw IoT Edge-apparaat op en kopieer de uitvoer. 

   ```powershell
   hostname
   ```

4. Zoek in het configuratiebestand de sectie **Edge device hostname** op. Werk de waarde van **hostname** bij met de hostnaam die u hebt gekopieerd vanuit PowerShell.

3. Haal in het PowerShell-venster van de beheerder het IP-adres voor uw IoT Edge-apparaat op. 

   ```powershell
   ipconfig
   ```

4. Kopieer de waarde voor **IPv4 Address** in de sectie **vEthernet (DockerNAT)** van de uitvoer. 

5. Maak een omgevingsvariabele genaamd **IOTEDGE_HOST** en vervang *\<ip_address\>* door het IP-adres voor uw IoT Edge-apparaat. 

   ```powershell
   [Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<ip_address>:15580")
   ```

6. Zoek in het bestand `config.yaml` het gedeelte **Connect settings** op. Werk de waarden **management_uri** en **workload_uri** bij met uw IP-adres en de poorten die u in het vorige gedeelte hebt geopend. Vervang **\<GATEWAY_ADDRESS\>** met het DockerNAT-IP-adres dat u hebt gekopieerd. 

   ```yaml
   connect: 
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

7. Zoek de sectie **Listen settings** op en voeg dezelfde waarden toe voor **management_uri** en **workload_uri**. 

   ```yaml
   listen:
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

8. Zoek de sectie **Moby Container Runtime-instellingen** op en controleer of de waarde voor **netwerk** geen opmerkingen bevat en is ingesteld op **azure-iot-edge**

   ```yaml
   moby_runtime:
     docker_uri: "npipe://./pipe/docker_engine"
     network: "azure-iot-edge"
   ```

9. Sla het configuratiebestand op. 

10. Start de IoT Edge-service opnieuw in PowerShell.

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

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

## <a name="deploy-a-module"></a>Een module implementeren

Beheer uw Azure IoT Edge-apparaat vanuit de cloud om een module te implementeren waarmee telemetriegegevens worden verzonden naar IoT Hub.
![Een apparaat registreren][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

In deze snelstart hebt u een nieuw IoT Edge-apparaat gemaakt en de IoT Edge-runtime erop geïnstalleerd. Vervolgens hebt u Azure Portal gebruikt om een IoT Edge-module te pushen om te worden uitgevoerd op het apparaat zonder dat er wijzigingen in het apparaat zelf aangebracht hoefden te worden. In dit geval worden met de module die u hebt gepusht, omgevingsgegevens gemaakt die u voor de zelfstudies kunt gebruiken. 

Bevestig dat de module die vanuit de cloud is geïmplementeerd, op uw IoT Edge -apparaat wordt uitgevoerd. 

```powershell
iotedge list
```

   ![Drie modules op uw apparaat bekijken](./media/quickstart/iotedge-list-2.png)

Bekijk de berichten die vanaf de module tempSensor naar de cloud worden verzonden. 

```powershell
iotedge logs tempSensor -f
```

  ![De gegevens van uw module bekijken](./media/quickstart/iotedge-logs.png)

U kunt de berichten die worden ontvangen door uw IoT-hub ook bekijken met behulp van de [IoT Hub Explorer][lnk-iothub-explorer] of de [Azure IoT Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt doorgaan met de IoT Edge-zelfstudies, kunt u het apparaat gebruiken dat u hebt geregistreerd en ingesteld in deze snelstart. Anders kunt u de Azure-resources die u hebt gemaakt en de IoT Edge-runtime van uw apparaat verwijderen. 

### <a name="delete-azure-resources"></a>Azure-resources verwijderen

Als u uw virtuele machine en IoT-hub in een nieuwe resourcegroep hebt gemaakt, kunt u die groep en alle bijbehorende resources verwijderen. Als er iets in die resourcegroep staat dat u wilt behouden, verwijder dan alleen de afzonderlijke resources die u wilt opschonen. 

Verwijder de groep **IoTEdgeResources**. 

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>De IoT Edge-runtime verwijderen

Als u van plan bent om het IoT Edge-apparaat in de toekomst voor testdoeleinden te gebruiken, maar niet wilt dat de module tempSensor gegevens naar uw IoT-hub verzendt als die niet wordt gebruikt, gebruikt u de volgende opdracht om de IoT Edge-service te beëindigen. 

   ```powershell
   Stop-Service iotedge -NoWait
   ```

U kunt de service opnieuw starten wanneer u klaar om weer te gaan testen.

   ```powershell
   Start-Service iotedge
   ```

Als u de installaties van uw apparaat wilt verwijderen, gebruikt u de volgende opdrachten.  

Verwijder de IoT Edge-runtime.

   ```powershell
   cmd /c sc delete iotedge
   rm -r c:\programdata\iotedge
   ```

Wanneer de IoT Edge-runtime is verwijderd, worden de containers die deze heeft gemaakt gestopt. Ze worden echter niet van uw apparaat verwijderd. Geef alle containers weer.

   ```powershell
   docker ps -a
   ```

Verwijder de containers die door de IoT Edge-runtime op uw apparaat zijn gemaakt. Verander de naam van de container tempSensor als u die anders had genoemd. 

   ```powershell
   docker rm -f tempSensor
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```
   
## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een nieuw IoT Edge-apparaat gemaakt en de Azure IoT Edge-cloudinterface gebruikt om code te implementeren op het apparaat. U hebt nu een testapparaat waarmee ruwe gegevens over de omgeving worden gegenereerd. 

U kunt nu doorgaan met elke andere zelfstudie om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Sensorgegevens filteren met behulp van een Azure-functie](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png


<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
