---
title: Oplossing voor externe bewaking importeren Edge-pakket - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een IoT Edge-pakket importeren in de oplossingsverbetering voor externe controle
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/10/2018
ms.topic: conceptual
ms.openlocfilehash: 34222f396ed3c43932371aa9f64a459bb2a5dd0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61442936"
---
# <a name="import-an-iot-edge-package-into-your-remote-monitoring-solution-accelerator"></a>Een IoT Edge-pakket importeren in de oplossingsverbetering voor externe controle

Een manifest van de implementatie definieert de modules te implementeren naar een IoT Edge-apparaat. In dit artikel wordt ervan uitgegaan dat een ontwikkelaar in uw organisatie al manifest van de implementatie is gemaakt. Zie voor meer informatie over hoe een ontwikkelaar een manifest gemaakt, een van de volgende praktische artikelen van IoT Edge:

- [Azure IoT Edge-modules van de Azure-portal implementeren](../iot-edge/how-to-deploy-modules-portal.md)
- [Azure IoT Edge-modules met Azure CLI implementeren](../iot-edge/how-to-deploy-modules-cli.md)
- [Azure IoT Edge-modules van Visual Studio Code implementeren](../iot-edge/how-to-deploy-modules-vscode.md)

Een ontwikkelaar maakt en een manifest van de implementatie wordt getest in een ontwikkelomgeving. Wanneer u klaar bent, kunt u het manifest importeren in de oplossingsverbetering voor externe controle.

## <a name="export-a-manifest"></a>Exporteren van een manifest

De Azure portal gebruiken voor het exporteren van het manifest van de implementatie van uw ontwikkelomgeving:

1. Navigeer naar de IoT-hub die u kunt ontwikkelen en testen van uw IoT Edge-apparaten in de Azure-portal. Klik op **IoT Edge** en vervolgens **IoT Edge-implementaties**: ![IoT Edge](media/iot-accelerators-remote-monitoring-import-edge-package/iotedge.png)

1. Klik op de implementatie met de configuratie van de implementatie die u wilt gebruiken. De **implementatiedetails** pagina: ![Details van de IoT Edge-implementatie](media/iot-accelerators-remote-monitoring-import-edge-package/deploymentdetails.png)

1. Klik op **downloaden IoT Edge-manifest**:  ![Implementatie-manifest downloaden](media/iot-accelerators-remote-monitoring-import-edge-package/download.png)

1. Het JSON-bestand opslaan als een lokaal bestand met de naam **deploymentmanifest.json**.

U hebt nu een bestand met de implementatie van het manifest. In de volgende sectie importeert u dit manifest als een pakket in de oplossing voor externe controle.

## <a name="import-a-package"></a>Een pakket importeren

Volg de stappen hieronder om een manifest van de implementatie Edge importeren als een pakket in uw oplossing:

1. Navigeer naar de **pakketten** pagina in de externe controle-Webgebruikersinterface:  ![Pakketten pagina](media/iot-accelerators-remote-monitoring-import-edge-package/packagespage.png)

1. Klik op **+ nieuw pakket**, kiest u **Manifest van de rand** als het pakkettype, en klik op **Bladeren** selecteren de **deploymentmanifest.json** bestand u hebt opgeslagen in de vorige sectie:  ![Selecteer het manifest](media/iot-accelerators-remote-monitoring-import-edge-package/selectmanifest.png)

1. Klik op **uploaden** het pakket toevoegen aan uw oplossing voor externe controle:  ![Geüploade pakket](media/iot-accelerators-remote-monitoring-import-edge-package/uploadedpackage.png)

U hebt nu een IoT Edge-implementatie-manifest geüpload als een pakket. Op de **implementaties** pagina, kunt u dit pakket implementeren op uw verbonden IoT Edge-apparaten.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u modules naar een IoT Edge-apparaat van de oplossing voor externe controle implementeert, wordt de volgende stap is voor meer informatie over [IoT Edge](../iot-edge/about-iot-edge.md).
