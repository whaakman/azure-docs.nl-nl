---
title: Oplossing voor externe bewaking importeren pakket ADM - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een automatische inrichting management Pack importeren in de oplossingsverbetering voor externe controle
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 8fd6e733f3e80ba2a3ec632c088d070252e260cc
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52684990"
---
# <a name="import-an-automatic-device-management-package-into-your-remote-monitoring-solution-accelerator"></a>Importeren van een pakket voor automatische inrichting in de oplossingsverbetering voor externe controle

Een configuratie van automatische device management definieert de wijzigingen in de configuratie om te implementeren op een groep apparaten. In dit artikel wordt ervan uitgegaan dat een ontwikkelaar in uw organisatie al een configuratie van automatische device management is gemaakt. Zie voor meer informatie over hoe een ontwikkelaar een configuratie gemaakt, een van de volgende praktische artikelen van IoT-Hub:

- [Configureren en controleren van IoT-apparaten op schaal met Azure portal](../iot-hub/iot-hub-auto-device-config.md)
- [Configureren en controleren van IoT-apparaten op schaal met behulp van de Azure CLI](../iot-hub/iot-hub-auto-device-config-cli.md)

Een ontwikkelaar maakt en test de configuratie van een automatische device management in een ontwikkelomgeving. Wanneer u klaar bent, kunt u de configuratie importeren in de oplossingsverbetering voor externe controle.

## <a name="export-a-configuration"></a>Een configuratie exporteren

De Azure portal gebruiken voor het exporteren van de configuratie van automatische device management van uw ontwikkelomgeving:

1. Navigeer naar de IoT-hub die u kunt ontwikkelen en testen van uw IoT-apparaten in de Azure-portal. Klik op **IoT apparaatconfiguratie**:

    [![Configuratie van de IoT-apparaat](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-expanded.png#lightbox)

1. Klik op de configuratie die u wilt gebruiken. De **apparaat configuratiedetails** pagina:

    [![Configuratiedetails van de IoT-apparaat](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-expanded.png#lightbox)
1. Klik op **configuratiebestand downloaden**:

    [![Configuratiebestand downloaden](./media/iot-accelerators-remote-monitoring-import-adm-package/download-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/download-expanded.png#lightbox)

1. Het JSON-bestand opslaan als een lokaal bestand met de naam **configuration.json**.

U hebt nu een bestand met de configuratie van automatische device management. In de volgende sectie importeert u deze configuratie als een pakket in de oplossing voor externe controle.

## <a name="import-a-package"></a>Een pakket importeren

Volg onderstaande stappen voor het importeren van een configuratie van automatische device management als een pakket in uw oplossing:

1. Navigeer naar de **pakketten** pagina in de externe controle-Webgebruikersinterface: ![pakketten pagina](media/iot-accelerators-remote-monitoring-import-adm-package/packagepage.png)

1. Klik op **+ nieuw pakket**, kiest u **configuratie** als het pakkettype, en klik op **Bladeren** selecteren de **configuration.json** bestand dat u in de vorige sectie hebt opgeslagen:

    ![Configuratie selecteren](media/iot-accelerators-remote-monitoring-import-adm-package/uploadpackage.png)

1. Klik op **uploaden** het pakket toevoegen aan uw oplossing voor externe controle:

    ![Geüploade pakket](media/iot-accelerators-remote-monitoring-import-adm-package/uploadedpackage.png)

U hebt nu een configuratie van automatische device management als een pakket geüpload. Op de **implementaties** pagina, kunt u dit pakket implementeren op uw verbonden apparaten.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een configuratiepakket maken en importeren in van de oplossing voor externe controle, de volgende stap is te leren hoe u [beheren van apparaten die zijn verbonden met externe controle in bulk](iot-accelerators-remote-monitoring-bulk-configuration-update.md).
