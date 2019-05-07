---
title: Azure IoT Edge voor Linux installeren op Windows | Microsoft Docs
description: Azure IoT Edge-installatie-instructies voor Linux-containers op Windows 10, Windows Server en Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.openlocfilehash: bb47a1b828084673961a6d2c5657793b4437f294
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160573"
---
# <a name="use-iot-edge-on-windows-to-run-linux-containers"></a>Gebruik van IoT Edge op Windows om uit te voeren van Linux-containers

Test IoT Edge-modules voor Linux-apparaten met behulp van een Windows-computer. 

In een productiescenario voor, moeten Windows-apparaten alleen Windows-containers uitvoeren. Een veelvoorkomend ontwikkelingsscenario is echter een Windows-computer gebruiken om te maken van IoT Edge-modules voor Linux-apparaten. IoT Edge-runtime voor Windows kunt u uitvoeren van Linux-containers voor **testen en ontwikkeling** doeleinden. 

In dit artikel vindt u de stappen voor het installeren van de Azure IoT Edge-runtime met behulp van Linux-containers op uw Windows x64 (AMD/Intel) systeem. Zie voor meer informatie over het IoT Edge-runtime installatieprogramma, waaronder informatie over alle installatieparameters [Azure IoT Edge-runtime installeren op Windows](how-to-install-iot-edge-windows.md).

## <a name="prerequisites"></a>Vereisten

Gebruik deze sectie om te controleren of uw Windows-apparaat IoT Edge kan ondersteunen en voorbereidt een container-engine voordat de installatie. 

### <a name="supported-windows-versions"></a>Ondersteunde versies van Windows

Azure IoT Edge met Linux-containers kunt uitvoeren op de volgende versies van Windows: 
* Windows 10 jubileumupdate (build 14393) of hoger
* Windows Server 2016 of hoger

Zie voor meer informatie over wat inbegrepen in de meest recente versie van IoT Edge, [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Als u installeren van IoT Edge op een virtuele machine wilt, geneste virtualisatie inschakelen en ten minste 2 GB geheugen toewijzen. Hoe het inschakelen van geneste virtualisatie verschilt afhankelijk van de hypervisor het gebruik. Voor Hyper-V geneste virtuele machines van generatie 2 virtualisatie is standaard ingeschakeld. Voor VMWare is er een in-/ uitschakelen om in te schakelen van de functie op uw virtuele machine. 

### <a name="prepare-the-container-engine"></a>Voorbereiden van de container-engine 

Azure IoT Edge is afhankelijk van een [OCI-compatibele](https://www.opencontainers.org/) container-engine. Het grootste configuratie verschil tussen Windows en Linux-containers die wordt uitgevoerd op een Windows-machine is dat de installatie van de IoT Edge een Windows-container-runtime omvat, maar u uw eigen runtime voor Linux-containers opgeven moet voor de installatie van IoT Edge. 

Als u een Windows-machine kunt ontwikkelen en testen van containers voor Linux-apparaten instelt, kunt u [Docker Desktop](https://www.docker.com/docker-windows) als uw container-engine. U moet Docker installeren en configureer deze [Linux-containers gebruiken](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) voor de installatie van IoT Edge.  

Als uw IoT Edge-apparaat een Windows-computer is, controleert u of het voldoet aan de [systeemvereisten](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) voor Hyper-V.

## <a name="install-iot-edge-on-a-new-device"></a>IoT Edge op een nieuw apparaat installeren

>[!NOTE]
>Azure IoT Edge-softwarepakketten zijn afhankelijk van de licentievoorwaarden die zich in de pakketten (in de map LICENSE). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

Een PowerShell-script downloadt en installeert de Azure IoT Edge-daemon voor beveiliging. De daemon security start vervolgens de eerste dag van de twee runtimemodules, de IoT Edge-agent, waarmee externe implementaties van andere modules. 

Wanneer u de IoT Edge-runtime voor de eerste keer op een apparaat installeert, moet u voor het inrichten van het apparaat een identiteit van een IoT-hub. Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaatreeks voor verbindingen die worden geleverd door uw IoT-hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te stellen. 

U kunt meer lezen over de verschillende installatieopties en parameters in het artikel [Azure IoT Edge-runtime installeren op Windows](how-to-install-iot-edge-windows.md). Zodra u hebt de Docker-Desktop geïnstalleerd en geconfigureerd voor Linux-containers, de installatie van het belangrijkste verschil is het declareren van Linux met de **- ContainerOs** parameter. Bijvoorbeeld: 

1. Als u dat nog niet gedaan hebt, een nieuwe IoT Edge-apparaat registreren en de verbindingsreeks op te halen. Kopieer de verbindingsreeks voor later gebruik in deze sectie. U kunt deze stap uit met de volgende hulpprogramma's kunt uitvoeren:

   * [Azure Portal](how-to-register-device-portal.md)
   * [Azure-CLI](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. Voer PowerShell uit als beheerder.

3. De **implementeren IoTEdge** opdracht controleert of uw Windows-machine op een ondersteunde versie is, schakelt de functie voor containers en downloadt vervolgens de runtime moby (die niet wordt gebruikt voor Linux-containers) en de IoT Edge-runtime. De opdracht standaard ingesteld op Windows-containers, declareren dus Linux als het besturingssysteem van de gewenste container. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

4. Op dit moment IoT Core-apparaten kunnen automatisch opnieuw opgestart. Andere apparaten met Windows 10 of Windows Server mogelijk gevraagd om opnieuw te starten. Als dit het geval is, kunt u uw apparaat opnieuw. Zodra het apparaat gereed is, opnieuw uitvoeren van PowerShell als beheerder.

5. De **initialiseren IoTEdge** opdracht configureert de IoT Edge-runtime op uw computer. De standaardinstellingen van de opdracht voor het inrichten van handmatige met een verbindingsreeks voor het apparaat. Linux opnieuw als het besturingssysteem van de gewenste container declareren. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ContainerOs Linux
   ```

6. Geef desgevraagd de apparaatverbindingsreeks die u in stap 1 hebt opgehaald. De verbindingsreeks van het apparaat wordt gekoppeld aan het fysieke apparaat met een apparaat-ID van IoT-Hub. 

   De verbindingsreeks heeft de volgende notatie en niet tussen aanhalingstekens moet worden opgenomen: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Controleer de status van de IoT Edge-service. Deze moet worden weergegeven als het uitvoeren.  

```powershell
Get-Service iotedge
```

Bekijk Logboeken van de laatste 5 minuten. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Lijst met modules. Na een nieuwe installatie, de enige module ziet u het uitvoeren is **edgeAgent**. Nadat u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md), ziet u anderen. 

```powershell
iotedge list
```

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md).

Als u hebt met het IoT Edge niet goed geïnstalleerd problemen, bekijk de [probleemoplossing](troubleshoot.md) pagina.

Als u wilt een bestaande installatie bijwerken naar de nieuwste versie van IoT Edge, Zie [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).
