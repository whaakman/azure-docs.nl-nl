---
title: Maken van een Azure-IoT-Hub met behulp van een sjabloon (PowerShell) | Microsoft Docs
description: Het gebruik van een Azure Resource Manager-sjabloon te maken van een IoT-Hub met Azure PowerShell.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: d23d3824c477d3bba4e4900bee355376f1317f92
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59609167"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Een IoT-hub maken met Azure Resource Manager-sjabloon (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Informatie over het gebruik van een Azure Resource Manager-sjabloon om een IoT-Hub en een consumergroep te maken. Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen [Azure Resource Manager-documentatie](https://docs.microsoft.com/azure/azure-resource-manager/).

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

De Resource Manager-sjabloon die wordt gebruikt in deze Quick Start is afkomstig van [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/101-iothub-with-consumergroup-create/). Hier volgt een kopie van de sjabloon:

[!code-json[iothub-creation](~/quickstart-templates/101-iothub-with-consumergroup-create/azuredeploy.json)]

De sjabloon maakt een Azure-Iot-hub met drie eindpunten (Event hub, cloud-naar-apparaat en messaging) en een consumergroep. Zie voor meer voorbeelden van de sjabloon, [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Devices&pageNumber=1&sort=Popular). Het schema van de sjabloon voor Iot Hub kan worden gevonden [hier](https://docs.microsoft.com/azure/templates/microsoft.devices/iothub-allversions).

Er zijn verschillende methoden voor het implementeren van een sjabloon.  In deze zelfstudie gebruikt u Azure PowerShell.

Als u wilt het PowerShell-script uitvoeren, selecteert u **uitproberen** openen van de Azure Cloud shell. Plak het script, met de rechtermuisknop op de shell en selecteer vervolgens plakken:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$iotHubName = Read-Host -Prompt "Enter the IoT Hub name"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-iothub-with-consumergroup-create/azuredeploy.json" `
    -iotHubName $iotHubName
```

Zoals u in het PowerShell-script zien kunt, is de sjabloon die wordt gebruikt in de Azure Quickstart-sjablonen. Voor het gebruik van uw eigen, moet u eerst de sjabloonbestand uploaden naar de cloudshell en gebruik vervolgens de `-TemplateFile` overschakelen naar de bestandsnaam opgeven.  Zie voor een voorbeeld [implementeren van de sjabloon](../azure-resource-manager/resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell#deploy-the-template).

## <a name="next-steps"></a>Volgende stappen

Nu u hebt een IoT-hub met behulp van een Azure Resource Manager-sjabloon geïmplementeerd, kunt u verder te verkennen:

* Meer informatie over de mogelijkheden van de [REST API voor IoT Hub-resourceprovider][lnk-rest-api].
* Lezen [overzicht van Azure Resource Manager] [ lnk-azure-rm-overview] voor meer informatie over de mogelijkheden van Azure Resource Manager.
* Zie voor de JSON-syntaxis en de eigenschappen voor gebruik in sjablonen, [Microsoft.Devices resourcetypen](/azure/templates/microsoft.devices/iothub-allversions).

Zie de volgende artikelen voor meer informatie over het ontwikkelen van IoT-Hub:

* [Inleiding tot C SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/manage-resources-powershell.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
