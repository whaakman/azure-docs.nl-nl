---
title: SensorTag apparaat & Azure IoT Gateway - probleemoplossing | Microsoft Docs
description: Pagina voor probleemoplossing voor Intel NUC gateway
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-problemen, internet der dingen problemen
ROBOTS: NOINDEX
ms.assetid: 5f742c38-0e33-465a-9a0d-1e41e8d17187
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7e80951de55ade6b5140608dcff8ebb064f942ca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting"></a>Problemen oplossen

## <a name="hardware-issues"></a>Hardwareproblemen

### <a name="ti-sensortag-cannot-be-connected"></a>TI SensorTag kan niet worden verbonden.

Gebruik voor het oplossen van problemen met de netwerkverbinding SensorTag de [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Een probleem met de Intel NUC

Raadpleeg voor het oplossen van problemen met opstarten [problemen met het niet opstarten op Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).

Raadpleeg voor het oplossen van problemen met besturingssystemen [problemen met het besturingssysteem op Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).

Om andere problemen te verwijzen naar [knipperen Codes en een piepgeluid laat horen Codes voor Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).

## <a name="nodejs-package-issues"></a>Problemen met node.js-pakket

### <a name="no-response-during-gulp-tasks"></a>Geen reactie tijdens gulp taken

Als u problemen ondervindt met actieve taken gulp, kunt u toevoegen de `--verbose` optie voor foutopsporing. Probeer het huidige gulp taken beëindigd met behulp van `Ctrl + C`, en voer de volgende in het consolevenster opdracht om te zien foutopsporingsberichten. Gedetailleerde foutberichten ziet u in de console-uitvoer.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problemen met detectie van apparaat

Voor hulp bij het oplossen van veelvoorkomende problemen met de `discover-sensortag` opdracht, Controleer de [wiki-pagina](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>npm problemen

Voer de npm-pakket bijwerken met de volgende opdracht:

```bash
npm install -g npm
```

Als het probleem blijft optreden, laat u hierop een toelichting aan het einde van dit artikel of maak een GitHub-probleem in onze [voorbeeld opslagplaats](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).

## <a name="remote-debugging"></a>Foutopsporing op afstand
> Onderstaande instructies zijn bedoeld voor het opsporen van node.js-scripts die worden gebruikt in deze zelfstudie.
### <a name="run-the-sample-application-in-debug-mode"></a>De voorbeeldtoepassing in de foutopsporingsmodus uitvoeren

De voorbeeldtoepassing in de foutopsporingsmodus uitvoeren met de volgende opdracht:

```bash
gulp run --debug
```

Wanneer de engine voor foutopsporing klaar is, ziet u `Debugger listening on port 5858` in de console-uitvoer.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Visual Studio Code verbinding maken met het externe apparaat configureren

1. Open de **Debug** deelvenster aan de linkerkant.
2. Klik op de groene **foutopsporing starten** knop (F5). Hiermee opent u Visual Studio Code een `launch.json` bestand.
3. Update de `launch.json` bestand met de volgende inhoud. Vervang `[device hostname or IP address]` met de werkelijke IP-adres of de hostnaam apparaatnaam.

   ``` json
   {
     "version": "0.2.0",
     "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Configuratie van extern foutopsporing](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Koppelen aan de externe toepassing

Klik op de groene **foutopsporing starten** (F5) knop foutopsporing te starten.

Lees [JavaScript in VS-Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) voor meer informatie over het foutopsporingsprogramma.

![Voorbeeld van foutopsporing uitschakelen](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Problemen met Azure CLI

De Azure-opdrachtregelinterface (Azure CLI) is een preview-versie. Als u wilt zoeken naar oplossingen, kunt u de [Preview installeren handleiding](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Als u met het hulpprogramma fouten optreden, het bestand een [probleem](https://github.com/Azure/azure-cli/issues) in de **problemen** sectie van de GitHub-repo.

Als u hulp nodig hebt bij het oplossen van veelvoorkomende problemen, Controleer de [Leesmij](https://github.com/Azure/azure-cli/blob/master/README.rst).

Als u 'Kan een versie die voldoet aan de vereiste niet vinden' aan, voert u de volgende opdracht om te werken van pip naar de laatste versie.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problemen met de installatie van Python

### <a name="legacy-installation-issues-macos"></a>Verouderde installatieproblemen (Mac OS)

Wanneer u pip installeert, een machtigingsfout wordt gegenereerd wanneer oudere pakketten worden geïnstalleerd met **su** machtigingen. Deze situatie doet zich voor omdat een eerdere installatie van Python met brew (Mac OS) is niet volledig verwijderd. Sommige pakketten pip van een eerdere installatie zijn gemaakt door root, waardoor de machtiging fout optreedt. De oplossing is om te verwijderen die pakketten geïnstalleerd door de hoofdmap. Gebruik de volgende stappen uit om deze taak te voltooien:

1. Ga naar `/usr/local/lib/python2.7/site-packages`
2. Lijst met pakketten gemaakt door hoofdmap:`ls -l | grep root`
3. Verwijderen van pakketten uit stap 2:`sudo rm -rf {package name}`
4. Installeer Python.

## <a name="azure-iot-hub-issues"></a>Problemen met Azure IoT Hub

Als u uw Azure-IoT-hub met de Azure CLI met succes hebt ingericht en u een hulpprogramma moet voor het beheren van de apparaten die zijn verbonden met uw IoT-hub, kunt u de volgende hulpprogramma's.

### <a name="device-explorer"></a>Apparaat Explorer

[Apparaat Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) wordt uitgevoerd op uw lokale Windows-computer en verbinding maakt met uw IoT-hub in Azure. Er wordt gecommuniceerd met de volgende [IoT-hubeindpunten](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Identiteiten Apparaatbeheer voor het inrichten en beheren van apparaten geregistreerd bij uw IoT-hub.
- Apparaat-naar-cloud ontvangen u berichten van uw apparaat voor uw IoT-hub kunt bewaken.
- Cloud naar apparaat verzenden zodat u berichten naar uw apparaten van uw IoT-hub verzenden kunt.

Configureer uw IoT hub-verbindingsreeks vanuit dit hulpprogramma te gebruiken van de mogelijkheden ervan.

### <a name="iothub-explorer"></a>iothub explorer

[iothub explorer](https://github.com/Azure/iothub-explorer) is een voorbeeld meerdere platforms CLI-hulpprogramma om apparaatclients te beheren. Het hulpprogramma kunt u de apparaten in het identiteitenregister beheren, apparaat-naar-cloud-berichten worden gecontroleerd en cloud-naar-apparaatopdrachten verzenden.

Voer de volgende opdracht voor het installeren van de meest recente (voorlopige) versie van het hulpprogramma iothub explorer:

```bash
npm install -g iothub-explorer@latest
```

Als u meer informatie over alle iothub explorer-opdrachten en de bijbehorende parameters, voer de volgende opdracht:

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a>De Azure-portal

Een volledige CLI-ervaring helpt u bij het maken en beheren van alle Azure-resources. U kunt ook gebruik van de [Azure-portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) kunt inrichten, beheren en fouten opsporen in uw Azure-resources.

## <a name="azure-storage-issues"></a>Azure Storage-problemen

[Microsoft Azure Opslagverkenner (preview)](http://storageexplorer.com/) is een zelfstandige app van Microsoft die u gebruiken kunt voor het werken met Azure Storage-gegevens op Windows-, Mac OS- en Linux. Met dit hulpprogramma kunt u verbinding maken met uw tabel en de gegevens weergegeven in het. U kunt dit hulpprogramma om problemen met uw Azure Storage te gebruiken.
