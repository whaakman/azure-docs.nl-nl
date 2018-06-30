---
title: Automatisch inrichten Azure IoT randapparaat met DP's - Windows | Microsoft Docs
description: Een gesimuleerd apparaat op uw Windows-machine voor het testen van automatische inrichting inrichting voor Azure IoT rand met inrichtingsservice apparaat gebruiken
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a973b248022cf3c0497f72bc2fcdd45a6527e65
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116022"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Maken en inrichten van een gesimuleerd apparaat van de rand van de TPM in Windows

Azure IoT Edge-apparaten kunnen worden automatisch-ingericht met behulp van de [apparaat inrichtingsservice](../iot-dps/index.yml) net als bij apparaten die niet rand ingeschakeld. Als u niet bekend met het proces van automatische inrichting bent, raadpleegt u de [automatische inrichting concepten](../iot-dps/concepts-auto-provisioning.md) voordat u doorgaat. 

In dit artikel wordt beschreven hoe u testen automatische inrichting op een gesimuleerd randapparaat met de volgende stappen: 

* Een exemplaar van IoT Hub apparaat inrichten Service (DP's) maken.
* Maak een gesimuleerd apparaat op uw Windows-machine met een gesimuleerde Trusted Platform Module (TPM) voor hardwarebeveiliging.
* Maak een afzonderlijke registratie voor het apparaat.
* Installeer de rand van de IoT-runtime en verbind het apparaat met IoT Hub.

## <a name="prerequisites"></a>Vereisten

* Een Windows-ontwikkelcomputer. In dit artikel maakt gebruik van Windows 10. 
* Een actieve IoT-Hub. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>De IoT Hub apparaat inrichtingsservice instellen

Een nieuw exemplaar van de inrichtingsservice van IoT Hub apparaat maken in Azure, en die koppelen aan uw IoT-hub. U kunt de instructies in [instellen van de IoT Hub DP's](../iot-dps/quick-setup-auto-provision.md).

Nadat u de apparaat-inrichtingsservice wordt uitgevoerd hebt, kopieert u de waarde van **ID-bereik** van de overzichtspagina. U kunt deze waarde gebruiken bij het configureren van de rand van de IoT-runtime. 

## <a name="simulate-a-tpm-device"></a>TPM-apparaat simuleren

Maak een gesimuleerd apparaat van de TPM op uw ontwikkelcomputer van Windows. Ophalen van de **registratie-ID** en **goedkeuringssleutel** voor uw apparaat, en ze gebruiken een afzonderlijke inschrijving-vermelding maken in DP's. 

Wanneer u een registratie in DP's maakt, hebt u de mogelijkheid om te declareren een **beginstatus apparaat Twin**. U kunt in de apparaat-twin labels instellen om apparaten te groeperen op een metriek die u nodig hebt in uw oplossing, zoals regio, omgeving, locatie of apparaat type. Deze tags worden gebruikt voor het maken van [automatische implementaties](how-to-deploy-monitor.md). 

Kies de SDK-taal die u gebruiken wilt voor het maken van het gesimuleerde apparaat en de stappen totdat u de afzonderlijke inschrijving hebt gemaakt. 

Wanneer u de afzonderlijke inschrijving maakt, selecteert u **inschakelen** om aan te geven dat deze virtuele machine is een **IoT randapparaat**.

Gesimuleerde apparaat en afzonderlijke registratiehandleidingen: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Opslaan na het maken van de afzonderlijke inschrijving de waarde van de **registratie-ID**. U kunt deze waarde gebruiken bij het configureren van de rand van de IoT-runtime. 

## <a name="install-the-iot-edge-runtime"></a>Installeren van de rand van de IoT-runtime

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. Onderdelen die hiervan zijn uitgevoerd in containers en kunnen u extra containers implementeren op het apparaat, zodat u kunt de code aan de rand uitvoeren. Op apparaten met Windows, kunt u gebruikmaken van containers Windows of Linux-containers. Kies het type van de containers die u wilt gebruiken en volg de stappen. Zorg ervoor dat voor het configureren van de rand van de IoT-runtime voor de automatische, geen handmatige inrichting. 

Volg de instructies voor het installeren van de rand van de IoT-runtime op het apparaat waarop de gesimuleerde TPM uit de vorige sectie. 

Kent uw DP's **ID-bereik** en apparaat **registratie-ID** voordat u begint met deze artikelen. 

* [Windows-containers](how-to-install-iot-edge-windows-with-windows.md)
* [Linux-containers](how-to-install-iot-edge-windows-with-linux.md)

## <a name="create-a-tpm-environment-variable"></a>Een TPM-omgevingsvariabele maken

Wijzig op de computer die wordt uitgevoerd van uw gesimuleerde apparaat, de **iotedge** registersleutels voor de service in te stellen van een omgevingsvariabele.

1. Van de **Start** menu openen **regedit**. 
2. Navigeer naar **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iotedge**. 
3. Selecteer **bewerken** > **nieuwe** > **waarde met meerdere tekenreeksen**. 
4. Voer de naam **omgeving**. 
5. Dubbelklik op de nieuwe variabele en de waardegegevens **IOTEDGE_USE_TPM_DEVICE = ON**. 
6. Klik op **OK** om uw wijzigingen op te slaan. 

## <a name="restart-the-iot-edge-runtime"></a>Opnieuw opstarten van de rand van de IoT-runtime

Start opnieuw op de rand van de IoT-runtime zodat deze alle configuratiewijzigingen die u hebt aangebracht op het apparaat opgehaald. 

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

Controleer of de rand van de IoT-runtime wordt uitgevoerd. 

   ```bash
   sudo systemctl status iotedge
   ```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

U kunt als de runtime is gestart, gaat u naar uw IoT-Hub en Zie dat het nieuwe apparaat automatisch is ingericht en is gereed om uit te voeren IoT rand modules. 

Controleer de status van de rand van de IoT-service.

```powershell
Get-Service iotedge
```

Raadpleeg de servicelogboeken van de laatste vijf minuten.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

Lijst met modules.

```powershell
iotedge list
```

## <a name="next-steps"></a>Volgende stappen

Het inschrijvingsproces Device inrichtingsservice kunt u de apparaat-ID en het apparaat twin labels op hetzelfde moment instellen, als u het nieuwe apparaat inricht. Deze waarden kunt u afzonderlijke apparaten of groepen van apparaten met behulp van automatische Apparaatbeheer als doel. Meer informatie over hoe [implementeren en bewaken IoT rand modules aan schalen met de Azure portal](how-to-deploy-monitor.md) of [met Azure CLI](how-to-deploy-monitor-cli.md)