---
title: Azure IoT Edge simuleren op Windows | Microsoft Docs
description: De Azure IoT Edge-runtime installeren op een gesimuleerd apparaat in Windows en de eerste module implementeren
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 06/08/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5bde54a65160c58d8bfba2f6c4c3b6a4317e46ed
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38540228"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Snelstart: Uw eerste IoT Edge-module van Azure Portal naar een Windows-apparaat implementeren - preview

Met Azure IoT Edge kunt u analyses en gegevensverwerking uitvoeren op uw apparaten, in plaats van dat u alle gegevens moet doorsturen naar de cloud. In de IoT Edge-zelfstudies kunt u zien hoe u verschillende typen modules implementeert, maar eerst hebt u een apparaat nodig dat u kunt testen. 

In deze snelstart leert u de volgende zaken:

1. Een IoT Hub maken
2. Een IoT Edge-apparaat registreren
3. De IoT Edge-runtime starten
4. Een module implementeren

![Zelfstudiearchitectuur][2]

De module die u in deze snelstart implementeert, is een gesimuleerde sensor waarmee temperatuur-, luchtvochtigheids- en drukgegevens worden gegenereerd. De andere Azure IoT Edge-zelfstudies bouwen voort op het werk dat u hier doet door modules te implementeren waarmee de gesimuleerde gegevens worden geanalyseerd voor zakelijke inzichten. 

>[!NOTE]
>De IoT Edge-runtime op Windows bevindt zich in [openbare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account][lnk-account] aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart wordt ervan uitgegaan dat u een computer of virtuele machine met Windows gebruikt om een IoT-apparaat te simuleren. Als u Windows uitvoert op een virtuele machine, schakelt u [geneste virtualisatie][lnk-nested] in en moet u ten minste 2 GB geheugen toewijzen. 

Zorg dat de volgende vereisten klaarstaan op de computer die u gebruikt voor een IoT Edge-apparaat:

1. Zorg ervoor dat u een ondersteunde versie van Windows gebruikt:
   * Windows 10 of hoger
   * Windows Server 2016 of hoger
2. Installeer [Docker voor Windows][lnk-docker] en contoleer of deze wordt uitgevoerd.
3. Configureer Docker voor het gebruiken van [Linux-containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Begin met de snelstart door uw IoT Hub in Azure Portal te maken.
![IoT Hub maken][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Een IoT Edge-apparaat registreren

Registreer een IoT Edge-apparaat bij uw net gemaakte IoT Hub.
![Een apparaat registreren][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

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
  $env:Path += ";C:\ProgramData\iotedge"
  SETX /M PATH "$env:Path"
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

  Zorg dat de omgevingsvariabele blijft behouden tijdens alle pogingen om opnieuw op te starten.

  ```powershell
  SETX /M IOTEDGE_HOST "http://<ip_address>:15580"
  ```

6. Zoek in het bestand `config.yaml` het gedeelte **Connect settings** op. Werk de waarden **management_uri** en **workload_uri** bij met uw IP-adres en de poorten die u in het vorige gedeelte hebt geopend. Vervang **\<GATEWAY_ADDRESS\>** door uw IP-adres. 

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

8. Zoek de sectie **Moby Container Runtime-instellingen**op en controleer of de waarde voor **netwerk** is ingesteld op `nat`.

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
    sort-object @{Expression="TimeCreated";Descending=$false}
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

U kunt het gesimuleerde apparaat dat u in deze snelstart hebt geconfigureerd, gebruiken om de IoT Edge-zelfstudies te testen. Als u wilt voorkomen dat de module tempSensor gegevens naar uw IoT-hub verzendt, gebruikt u de volgende opdracht om de IoT Edge-service te stoppen en de containers te verwijderen die op uw apparaat zijn gemaakt. Als u uw machine opnieuw als IoT Edge-apparaat wilt gebruiken, vergeet dan niet de service te starten. 

   ```powershell
   Stop-Service iotedge -NoWait
   docker rm -f $(docker ps -aq)
   ```

Als u de IoT-hub die u hebt gemaakt niet meer nodig hebt, kunt u de Azure-portal gebruiken om de resource en alle apparaten die eraan zijn gekoppeld te verwijderen. Navigeer naar de overzichtspagina van uw IoT Hub en selecteer **Verwijderen**. 

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een nieuw IoT Edge-apparaat gemaakt en de Azure IoT Edge-cloudinterface gebruikt om code te implementeren op het apparaat. U hebt nu een testapparaat waarmee ruwe gegevens over de omgeving worden gegenereerd. 

U kunt nu doorgaan met elke andere zelfstudie om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Sensorgegevens filteren met behulp van een Azure-functie](tutorial-deploy-function.md)

<!-- Images -->
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md
[lnk-account]: https://azure.microsoft.com/free
