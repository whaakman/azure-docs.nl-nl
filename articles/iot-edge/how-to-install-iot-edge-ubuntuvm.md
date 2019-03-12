---
title: Azure IoT Edge op Ubuntu virtuele Machines uitvoeren | Microsoft Docs
description: Azure IoT Edge installatie-instructies op Ubuntu 16.04 Azure Marketplace Virtual Machines
author: gregman
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: gregman
ms.openlocfilehash: 456f7607786bd674cb6ede78d1164db033605799
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57528827"
---
# <a name="run-azure-iot-edge-on-ubuntu-virtual-machines"></a>Azure IoT Edge worden uitgevoerd op Ubuntu virtuele Machines

De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren.

Zie voor meer informatie over de werking van de IoT Edge-runtime en welke onderdelen zijn opgenomen, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md).

In dit artikel worden de stappen voor het uitvoeren van de Azure IoT Edge-runtime op een Ubuntu 16.04-Machine met behulp van de vooraf geconfigureerde [Azure IoT Edge op Ubuntu Azure Marketplace-aanbieding](https://aka.ms/azure-iot-edge-ubuntuvm). 

Bij de eerste keer opstarten, vooraf de nieuwste versie van de Azure IoT Edge-runtime installeert met de Azure IoT Edge op Ubuntu-VM. Dit omvat ook een script voor de verbindingsreeks instellen en start vervolgens opnieuw de runtime, dit kan op afstand worden geactiveerd via de virtuele machine van Azure portal of Azure vanaf de opdrachtregel, zodat u eenvoudig kunt configureren en verbinding maken met de IoT Edge-apparaat zonder dat een SSH- of extern wordt gestart bureaublad-sessiehost. Met dit script wacht voordat de verbindingsreeks instellen wanneer de IoT Edge-client is volledig geïnstalleerd zodat u niet hoeft te integreren in uw automation.

## <a name="deploy-from-the-azure-marketplace"></a>Implementeren in Azure Marketplace
1.  Navigeer naar de [Azure IoT Edge op Ubuntu](https://aka.ms/azure-iot-edge-ubuntuvm) Marketplace-aanbieding of door te zoeken naar 'Azure IoT Edge op Ubuntu' in [Azure Marketplace](https://azuremarketplace.microsoft.com/)
2.  Selecteer **IT krijgen nu** en vervolgens **doorgaan** in het volgende dialoogvenster.
3.  Eenmaal in de Azure-portal, selecteer **maken** en volg de wizard voor het implementeren van de virtuele machine. 
    *   Als het de eerste keer probeert een virtuele machine uit, is het gemakkelijkst om een wachtwoord te gebruiken en om in te schakelen van de SSH in het menu openbare binnenkomende poort. 
    *   Als u een resource-intensieve workload hebt, kunt u de grootte van de virtuele machine moeten opwaarderen door toe te voegen meer CPU's en/of geheugen.
4.  Wanneer de virtuele machine is geïmplementeerd, configureert u het om te verbinden met uw IoT-Hub:
    1.  Kopieer de verbindingsreeks van uw apparaat van uw IoT Edge-apparaat in uw IoT-Hub gemaakt (u kunt volgen de [Registreer een nieuwe Azure IoT Edge-apparaat vanuit de Azure-portal](how-to-register-device-portal.md) handleiding als u niet bekend bent met dit proces)
    1.  Selecteer uw zojuist gemaakte virtuele-machinebron in Azure portal en open de **RunCommand-** optie
    1.  Selecteer de **RunShellScript** optie
    1.  Voer het onderstaande script via het opdrachtvenster met de apparaatverbindingsreeks: `/etc/iotedge/configedge.sh “{device_connection_string}”`
    1.  Selecteer **uitvoeren**
    1.  Wacht even en het scherm moet Geef vervolgens een bericht weergegeven dat aangeeft van de verbindingsreeks correct is ingesteld.


## <a name="deploy-from-the-azure-portal"></a>Implementeren vanuit Azure Portal
Zoek 'Azure IoT Edge' van de Azure-portal en selecteer **Ubuntu Server 16.04 LTS + Azure IoT Edge-runtime** om te beginnen met de werkstroom voor het maken van virtuele machine. Van daaruit stappen 3 en 4 in de bovenstaande 'Implementeren vanuit de Azure Marketplace'-instructies.

## <a name="deploy-from-azure-cli"></a>Implementeren vanuit Azure CLI
1.  Als dit de eerste keer dat een virtuele machine van de CLI implementeren, moet u programmatische implementatie voor uw Azure-abonnement moet worden ingeschakeld:
    1. Open de [Azure IoT Edge op Ubuntu](https://aka.ms/azure-iot-edge-ubuntuvm) Marketplace-aanbieding
    1. Selecteer **IT krijgen nu** en **doorgaan** in het volgende dialoogvenster
    1. Selecteer **wilt implementeren via een programma? Aan de slag** aan de onderkant van het dialoogvenster in de portal
    1. Klik op de **inschakelen** knop in de **programmatische implementatie configureren** pagina en klik vervolgens op **opslaan**
1.  Als u van Azure CLI op uw bureaublad gebruikmaakt, starten door te dienen:

    ```azurecli-interactive
    az login
    ```
    
1.  Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken:
    1.  Uw abonnementen weergeven:
    
       ```azurecli-interactive
       az account list --output table
       ```
    
    1.  Kopieer de abonnements-id-veld voor het abonnement dat u wilt gebruiken
    1.  Voer deze opdracht met de ID die u zojuist hebt gekopieerd:
    
       ```azurecli-interactive 
       az account set -s {SubscriptionId}
       ```
    
1.  Een nieuwe resourcegroep maken (of geef een bestaande in de volgende stappen):

    ```azurecli-interactive
    az group create --name IoTEdgeResources --location westus2
    ```
    
1.  Maak een nieuwe virtuele machine:

    ```azurecli-interactive
    az vm create --resource-group IoTEdgeResources --name EdgeVM –-image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
    ```

1.  De verbindingsreeks van het apparaat instellen (u kunt volgen de [een nieuw Azure IoT Edge-apparaat registreren bij Azure CLI](how-to-register-device-cli.md) handleiding als u niet bekend met dit proces bent):

    ```azurecli-interactive
    az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunShellScript --script '/etc/iotedge/configedge.sh "{device_connection_string}"'
    ```

Als u wilt de SSH in deze virtuele machine na de installatie, gebruikt u het openbare IP-adres met de opdracht: `ssh azureuser@{publicIpAddress}`


## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md).

Als u hebt met het Edge-runtime niet goed geïnstalleerd problemen, bekijk de [probleemoplossing](troubleshoot.md) pagina.

Als u wilt een bestaande installatie bijwerken naar de nieuwste versie van IoT Edge, Zie [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).
