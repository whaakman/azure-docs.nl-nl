---
title: Azure IoT Edge worden uitgevoerd op Windows Server Virtual Machines | Microsoft Docs
description: Azure IoT Edge installatie-instructies op Windows Server Marketplace Virtual Machines
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: gregman
ms.openlocfilehash: be7479d3f042d6e64428a07e0509907b78595200
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159778"
---
# <a name="run-azure-iot-edge-on-windows-server-virtual-machines"></a>Azure IoT Edge worden uitgevoerd op Windows Server Virtual Machines
De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren.

Zie voor meer informatie over de werking van de IoT Edge-runtime en welke onderdelen zijn opgenomen, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md).

In dit artikel worden de stappen voor het uitvoeren van de Azure IoT Edge-runtime op een Windows Server 2019 virtuele machine met de [WindowsServer](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview) Azure Marketplace-aanbieding. Volg de instructies op [installeren van de Azure IoT Edge-runtime](how-to-install-iot-edge-windows.md) op Windows voor gebruik met andere versies.

> [!NOTE]
> IoT Edge-runtime op Windows Server is in [preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="deploy-from-the-azure-marketplace"></a>Implementeren in Azure Marketplace
1.  Navigeer naar de [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview) Azure Marketplace-aanbieding of door te zoeken naar 'Windows Server' op [Azure Marketplace](https://azuremarketplace.microsoft.com/)
2.  Selecteer **krijgen nu** 
3.  In **softwareabonnement**, zoek 'Windows Server 2019 Datacenter Server Core met Containers' en selecteer vervolgens **doorgaan** in het volgende dialoogvenster.
    * U kunt deze instructies ook gebruiken voor andere versies van Windows Server met Containers
4.  Eenmaal in de Azure-portal, selecteer **maken** en volg de wizard voor het implementeren van de virtuele machine. 
    *   Als het de eerste keer probeert een virtuele machine uit, is het gemakkelijkst om een wachtwoord te gebruiken en RDP en SSH inschakelen in het menu openbare binnenkomende poort. 
    *   Als u een resource-intensieve workload hebt, kunt u de grootte van de virtuele machine moeten opwaarderen door toe te voegen meer CPU's en/of geheugen.
5.  Wanneer de virtuele machine is geïmplementeerd, configureert u het om te verbinden met uw IoT-Hub:
    1.  Kopieer de verbindingsreeks van uw apparaat van uw IoT Edge-apparaat in uw IoT-Hub gemaakt (u kunt volgen de [Registreer een nieuwe Azure IoT Edge-apparaat vanuit de Azure-portal](how-to-register-device-portal.md) handleiding als u niet bekend bent met dit proces)
    1.  Selecteer uw zojuist gemaakte virtuele-machinebron in Azure portal en open de **RunCommand-** optie
    1.  Selecteer de **RunPowerShellScript** optie
    1.  Kopieer dit script in het opdrachtvenster met de apparaatverbindingsreeks: 
        ```powershell
        . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
        Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'
        ```
    1.  Voer het script voor het installeren van de Edge-runtime en stel de verbindingsreeks door te selecteren uit **uitvoeren**
    1.  Na een minuut of twee ziet u een bericht dat de Edge-runtime is geïnstalleerd en ingericht.


## <a name="deploy-from-the-azure-portal"></a>Implementeren vanuit Azure portal
1. Vanuit de Azure-portal, zoek naar 'Windows Server' en selecteer **Windows Server 2019 Datacenter** om te beginnen met de werkstroom voor het maken van virtuele machine. 
2. Van **een softwareabonnement selecteren** Kies 'Windows Server 2019 Datacenter Server Core met Containers', en selecteer vervolgens **maken**
3. Voltooi stap 5 in de 'implementeren vanuit de Azure Marketplace"bovenstaande instructies.

## <a name="deploy-from-azure-cli"></a>Implementeren vanuit Azure CLI
1. Als u van Azure CLI op uw bureaublad gebruikmaakt, starten door te dienen:

   ```azurecli-interactive
   az login
   ```
    
1. Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken:
   1. Uw abonnementen weergeven:
    
      ```azurecli-interactive
      az account list --output table
      ```
    
   1. Kopieer de abonnements-id-veld voor het abonnement dat u wilt gebruiken
   1. Voer deze opdracht met de ID die u hebt gekopieerd:
    
      ```azurecli-interactive 
      az account set -s {SubscriptionId}
      ```
    
1. Een nieuwe resourcegroep maken (of geef een bestaande in de volgende stappen):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```
    
1. Maak een nieuwe virtuele machine:

   ```azurecli-interactive
   az vm create -g IoTEdgeResources -n EdgeVM --image MicrosoftWindowsServer:WindowsServer:2019-Datacenter-Core-with-Containers:latest  --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```
   * Met deze opdracht wordt u gevraagd om een wachtwoord, maar u kunt de optie toevoegen `--admin-password` eenvoudig kunt instellen in een script
   * De Windows Server Core-installatiekopie heeft opdracht regel ondersteuning alleen met extern bureaublad, dus als u de volledige Bureaubladervaring, geef `MicrosoftWindowsServer:WindowsServer:2019-Datacenter-with-Containers:latest` als de afbeelding

1. De verbindingsreeks van het apparaat instellen (u kunt volgen de [een nieuw Azure IoT Edge-apparaat registreren bij Azure CLI](how-to-register-device-cli.md) handleiding als u niet bekend met dit proces bent):

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunPowerShellScript --script ". {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'"
   ```

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md).

Als u problemen met het Edge-runtime niet goed geïnstalleerd ondervindt, bekijk dan de [probleemoplossing](troubleshoot.md) pagina.

Als u wilt een bestaande installatie bijwerken naar de nieuwste versie van IoT Edge, Zie [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).

Meer informatie over het gebruik van Windows virtuele machines op de [documentatie voor Windows-Machines](https://docs.microsoft.com/azure/virtual-machines/windows/).

Als u wilt dat SSH in deze virtuele machine na de installatie, volgt u de [van OpenSSH voor Windows Server voor installatie](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse#installing-openssh-with-powershell) handleiding met behulp van extern bureaublad of externe powershell.
