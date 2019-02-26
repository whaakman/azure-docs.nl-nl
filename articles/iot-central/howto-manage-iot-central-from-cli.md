---
title: IoT Central beheren vanuit Azure CLI | Microsoft Docs
description: IoT Central, beheren vanuit Azure CLI.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/07/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 9e5d842cece316bc9c53e1e8583f40a0f222b91d
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56808528"
---
# <a name="manage-iot-central-from-azure-cli"></a>IoT Central beheren vanuit Azure CLI

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

In plaats van het maken en beheren van IoT Central-toepassingen van de IoT Central [Toepassingsbeheer](https://aka.ms/iotcentral) pagina, kunt u [Azure CLI](/cli/azure/) voor het beheren van uw toepassingen.

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u liever de Azure CLI uitvoeren op uw lokale computer, raadpleegt u [Azure CLI installeren](/cli/azure/install-azure-cli). Wanneer u Azure CLI lokaal uitvoert, gebruikt u de **az login** opdracht uit om aan te melden bij Azure voordat u de opdrachten in dit artikel.

## <a name="create-an-application"></a>Een app maken

Gebruik de [az iotcentral app maken](/cli/azure/iotcentral/app#az-iotcentral-app-create) opdracht voor het maken van een IoT Central-toepassing in uw Azure-abonnement. Bijvoorbeeld:

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku S1 --template "iotc-demo@1.0.0" \
  --display-name "My Custom Display Name"
```

Deze opdrachten wordt eerst een resourcegroep maken in de regio voor de toepassing VS-Oost. De volgende tabel beschrijft de parameters gebruikt met de **az iotcentral app maken** opdracht:

| Parameter         | Description |
| ----------------- | ----------- |
| resource-group    | De resourcegroep die de toepassing bevat. Deze resourcegroep moet al bestaan in uw abonnement. |
| location          | Met deze opdracht maakt standaard gebruik van de locatie van de resourcegroep. Op dit moment kunt u een IoT Central-toepassing in de **VS-Oost**, **VS-West**, **Noord-Europa**, of **West-Europa** regio's. |
| naam              | De naam van de toepassing in Azure portal. |
| Subdomein         | Het subdomein in de URL van de toepassing. In het voorbeeld wordt de URL van de toepassing is https://mysubdomain.azureiotcentral.com. |
| sku               | Op dit moment de enige waarde die is **S1** (standard-laag). Zie [prijzen voor Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). |
| sjabloon          | De toepassingssjabloon te gebruiken. Zie de volgende tabel voor meer informatie: |
| display-name      | De naam van de toepassing, zoals weergegeven in de gebruikersinterface. |

**Sjablonen voor toepassingen**

| Naam van sjabloon            | Description |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Hiermee maakt u een lege toepassing die u kunt vullen met uw eigen apparaatsjablonen en apparaten. |
| iotc-demo@1.0.0          | Hiermee maakt u een toepassing die onder andere een vooraf gemaakte apparaatjabloon voor een gekoelde verkoopautomaat bevat. Gebruik deze sjabloon als u wilt beginnen met het verkennen van Azure IoT Central. |
| iotc-devkit-sample@1.0.0 | Hiermee maakt u een toepassing met apparaatsjablonen die u in staat stelt verbinding te maken met een MXChip- of Raspberry Pi-apparaat. Gebruik deze sjabloon als u een ontwikkelaar van het apparaat te experimenteren met een van deze apparaten. |

## <a name="view-your-applications"></a>Uw toepassingen bekijken

Gebruik de [az iotcentral applijst](/cli/azure/iotcentral/app#az-iotcentral-app-list) opdracht voor het weergeven van uw IoT Central-toepassingen en metagegevens bekijken.

## <a name="modify-an-application"></a>Wijzigen van een toepassing

Gebruik de [az iotcentral app-update](/cli/azure/iotcentral/app#az-iotcentral-app-update) opdracht voor het bijwerken van de metagegevens van een IoT Central-toepassing. Als u bijvoorbeeld de weergegeven naam van uw toepassing te wijzigen:

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Een toepassing verwijderen

Gebruik de [az iotcentral app verwijderen](/cli/azure/iotcentral/app#az-iotcentral-app-delete) opdracht voor het verwijderen van een IoT Central-toepassing. Bijvoorbeeld:

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u Azure IoT Central om toepassingen te beheren vanuit Azure CLI, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw toepassing beheren](howto-administer.md)
