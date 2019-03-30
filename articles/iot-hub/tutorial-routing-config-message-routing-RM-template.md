---
title: Berichtroutering configureren voor Azure IoT Hub met behulp van een Azure Resource Manager-sjabloon | Microsoft Docs
description: Berichtroutering configureren voor Azure IoT Hub met behulp van een Azure Resource Manager-sjabloon
author: robinsh
manager: philmeagit st
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 58b0039980f448c9bd953e9d59289c511ce95517
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663023"
---
# <a name="tutorial-use-an-azure-resource-manager-template-to-configure-iot-hub-message-routing"></a>Zelfstudie: Een Azure Resource Manager-sjabloon gebruiken om te configureren met het IoT Hub-berichtroutering

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="message-routing"></a>Berichtroutering

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

## <a name="download-the-template-and-parameters-file"></a>Download het sjabloon en parameters

Voor het tweede gedeelte van deze zelfstudie, die u kunt downloaden en uitvoeren van een toepassing met Visual Studio om berichten te verzenden naar de IoT Hub. Er is een map in dat het downloaden van de Azure Resource Manager-sjabloon en parameters in, evenals de Azure CLI en PowerShell-scripts bevat.

Ga verder en download de [Azure IoT C# voorbeelden](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) nu. Decomprimeer het bestand master.zip. De Resource Manager-sjabloon en het parameterbestand zijn in /iot-hub/Tutorials/Routing/SimulatedDevice/resources/als **template_iothub.json** en **template_iothub_parameters.json**.

## <a name="create-your-resources"></a>Uw resources maken

U gaat een Azure Resource Manager (DB)-sjabloon gebruiken om u te maken van al uw resources. De Azure CLI en PowerShell-scripts kunnen een paar regels worden uitgevoerd op een tijdstip. Een RM-sjabloon wordt geïmplementeerd in één stap. Dit artikel laat u de secties afzonderlijk om te begrijpen van elkaar. En vervolgens deze u hoe u de sjabloon implementeert ziet, en maken van het virtuele apparaat voor het testen van. Nadat de sjabloon wordt geïmplementeerd, kunt u de configuratie van de routering van het bericht weergeven in de portal.

Er zijn verschillende namen van voorbeeldresources die globaal uniek zijn, zoals de naam van de IoT Hub en de naam van het opslagaccount moeten. Als u wilt de naamgeving van de resources die eenvoudiger, die resourcenamen toe te voegen een willekeurige alfanumerieke waarde die is gegenereerd op basis van de huidige datum en tijd ingesteld. 

Als u de sjabloon bekijkt, ziet u waar variabelen zijn geconfigureerd voor deze resources die de parameter die wordt doorgegeven en samenvoegen *randomValue* naar de parameter. 

De volgende sectie wordt uitgelegd dat de parameters die worden gebruikt.

### <a name="parameters"></a>Parameters

De meeste van deze parameters hebben de standaardwaarden. Die eindigt op **_On** worden samengevoegd met *randomValue* zodat ze globaal uniek zijn. 

**randomValue**: Deze waarde is gegenereerd op basis van de huidige datum en tijd wanneer u de sjabloon implementeert. Dit veld is niet in het parameterbestand omdat deze wordt gegenereerd in de sjabloon zelf.

**abonnements-id**: Dit veld is ingesteld voor u om het abonnement waarin u de sjabloon implementeert. Dit veld is niet in het parameterbestand omdat deze is ingesteld voor u.

**IoTHubName_in**: Dit veld is de basis IoT Hub-naam, die wordt samengevoegd met de randomValue wereldwijd uniek zijn.

**Locatie**: Dit veld is de Azure-regio waarnaar u, zoals 'westus implementeert'.

**consumer_group**: Dit veld is de consumergroep instellen voor berichten die afkomstig is van het eindpunt van de routering. Het wordt gebruikt voor het filteren van resultaten in Azure Steam Analytics. Er is bijvoorbeeld de hele stroom zo krijgt u alles wat of als u gegevens die via met consumer_group ingesteld op **Contoso**, u kunt instellen van een Azure Stream Analytics stream (en de Power BI-rapport) om alleen die items weer te geven. Dit veld wordt gebruikt in deel 2 van deze zelfstudie.

**sku_name**: Dit veld is de schaalbaarheid voor de IoT-Hub. Deze waarde moet S1 of hoger; een gratis laag werkt niet voor deze zelfstudie, omdat dit meerdere eindpunten niet is toegestaan.

**sku_units**: Dit veld wordt met de **sku_name**, en is het aantal IoT Hub-eenheden die kunnen worden gebruikt.

**d2c_partitions**: Dit veld is het aantal partities die worden gebruikt voor de gebeurtenisstroom.

**storageAccountName_in**: Dit veld is de naam van het opslagaccount dat moet worden gemaakt. Berichten worden doorgestuurd naar een container in het opslagaccount. Dit veld wordt samengevoegd met de randomValue wereldwijd uniek te maken.

**storageContainerName**: Dit veld is de naam van de container waarin de berichten die zijn doorgestuurd naar het storage-account worden opgeslagen.

**storage_endpoint**: Dit veld is de naam voor het eindpunt van de storage-account die worden gebruikt door de routering van berichten.

**service_bus_namespace_in**: Dit veld is de naam van de Service Bus-naamruimte moet worden gemaakt. Deze waarde is samengevoegd met de randomValue wereldwijd uniek te maken.

**service_bus_queue_in**: Dit veld is de naam van de Service Bus-wachtrij gebruikt voor het routeren van berichten. Deze waarde is samengevoegd met de randomValue wereldwijd uniek te maken.

**AuthRules_sb_queue**: Dit veld is de autorisatieregels voor de service bus-wachtrij, gebruikt voor het ophalen van de verbindingsreeks voor de wachtrij.

### <a name="variables"></a>Variabelen

Deze waarden worden gebruikt in de sjabloon en zijn meestal afgeleid van parameters.

**queueAuthorizationRuleResourceId**: Dit veld is de ResourceId voor de verificatieregel voor de Service Bus-wachtrij. ResourceId wordt op zijn beurt gebruikt om op te halen van de verbindingsreeks voor de wachtrij.

**iotHubName**: Dit veld is de naam van de IoT-Hub nadat er randomValue samengevoegd. 

**storageAccountName**: Dit veld is de naam van de storage-account nadat er randomValue samengevoegd. 

**service_bus_namespace**: Dit veld is de naamruimte nadat er randomValue samengevoegd.

**service_bus_queue**: Dit veld is de naam van de Service Bus-wachtrij nadat er randomValue samengevoegd.

**sbVersion**: De versie van de Service Bus-API te gebruiken. In dit geval is het '2017-04-01'.

### <a name="resources-storage-account-and-container"></a>Resources: Storage-account en -container

De eerste resource hebt gemaakt, is het opslagaccount, samen met de container waarmee berichten worden gerouteerd. De container is een resource onder de storage-account. Er is een `dependsOn` component voor de storage-account waarvoor de storage-account worden gemaakt voordat de container.

Dit is in deze sectie eruit:

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2018-07-01",
    "location": "[parameters('location')]",
    "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
    },
    "kind": "Storage",
    "properties": {},
    "resources": [
        {
        "type": "blobServices/containers",
        "apiVersion": "2018-07-01",
        "name": "[concat('default/', parameters('storageContainerName'))]",
        "properties": {
            "publicAccess": "None"
            } ,
        "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
            ]
        }
    ]
}
```

### <a name="resources-service-bus-namespace-and-queue"></a>Resources: Service Bus-naamruimte en wachtrij

De tweede resource hebt gemaakt, is de Service Bus-naamruimte, samen met de Service Bus-wachtrij die berichten worden gerouteerd. De SKU is ingesteld op standaard. De API-versie wordt opgehaald uit de variabelen. Het is ook ingesteld op het activeren van de Service Bus-naamruimte wanneer deze in deze sectie (status: actief) implementeert. 

```json
{
    "type": "Microsoft.ServiceBus/namespaces",
    "comments": "The Sku should be 'Standard' for this tutorial.",
    "sku": {
        "name": "Standard",
        "tier": "Standard"
    },
    "name": "[variables('service_bus_namespace')]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "properties": {
        "provisioningState": "Succeeded",
        "metricId": "[concat('a4295411-5eff-4f81-b77e-276ab1ccda12:', variables('service_bus_namespace'))]",
        "serviceBusEndpoint": "[concat('https://', variables('service_bus_namespace'),'.servicebus.windows.net:443/')]",
        "status": "Active"
    },
    "dependsOn": []
}
```

Deze sectie maakt u de Service Bus-wachtrij. In dit gedeelte van het script bevat een `dependsOn` component die ervoor zorgt de naamruimte dat is gemaakt voordat de wachtrij.

```json
{
    "type": "Microsoft.ServiceBus/namespaces/queues",
    "name": "[concat(variables('service_bus_namespace'), '/', variables('service_bus_queue'))]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "scale": null,
    "properties": {},
    "dependsOn": [
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]"
    ]
}
```

### <a name="resources-iot-hub-and-message-routing"></a>Resources: IOT Hub en berichtroutering

Nu dat de storage-account en Service Bus-wachtrij hebt gemaakt, kunt u de IoT-Hub die berichten om ze te maken. De RM-sjabloon maakt gebruik van `dependsOn` van de EU, zodat deze niet wordt geprobeerd om het maken van de hub voordat de Service Bus-resources en het opslagaccount is gemaakt. 

Hier is het eerste deel van de IoT Hub-sectie. Dit deel van de sjabloon stelt u de afhankelijkheden en begint met de eigenschappen.

```json
{
    "apiVersion": "2018-04-01",
    "type": "Microsoft.Devices/IotHubs",
    "name": "[variables('IoTHubName')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces/queues', variables('service_bus_namespace'), variables('service_bus_queue'))]"
    ],
    "properties": {
        "eventHubEndpoints": {}
            "events": {
                "retentionTimeInDays": 1,
                "partitionCount": "[parameters('d2c_partitions')]"
                }
            },
```

De volgende sectie wordt de sectie voor het bericht een routeringsconfiguratie voor de Iot Hub. Eerst wordt de sectie voor de eindpunten. Dit deel van de sjabloon stelt u de routering-eindpunten voor de Service Bus-wachtrij en het opslagaccount, met inbegrip van de verbindingsreeksen.

Voor het maken van de verbindingsreeks voor de wachtrij, moet u de queueAuthorizationRulesResourcedId die is opgehaald inline. Voor het maken van de verbindingsreeks voor het opslagaccount ophalen van de primaire toegangssleutel en vervolgens worden gebruikt in de indeling voor de verbindingsreeks.

De eindpuntconfiguratie is ook waar u de blob-indeling instellen op `AVRO` of `JSON`.

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

 ```json
"routing": {
    "endpoints": {
        "serviceBusQueues": [
        {
            "connectionString": "[Concat('Endpoint=sb://',variables('service_bus_namespace'),'.servicebus.windows.net/;SharedAccessKeyName=',parameters('AuthRules_sb_queue'),';SharedAccessKey=',listkeys(variables('queueAuthorizationRuleResourceId'),variables('sbVersion')).primaryKey,';EntityPath=',variables('service_bus_queue'))]",
            "name": "[parameters('service_bus_queue_endpoint')]",
            "subscriptionId": "[parameters('subscriptionId')]", 
            "resourceGroup": "[resourceGroup().Name]"
        }
        ],
        "serviceBusTopics": [],
        "eventHubs": [],
        "storageContainers": [
            {
                "connectionString": 
                "[Concat('DefaultEndpointsProtocol=https;AccountName=',variables('storageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).keys[0].value)]",
                "containerName": "[parameters('storageContainerName')]",
                "fileNameFormat": "{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}",
                "batchFrequencyInSeconds": 100,
                "maxChunkSizeInBytes": 104857600,
                "encoding": "avro",
                "name": "[parameters('storage_endpoint')]",
                "subscriptionId": "[parameters('subscriptionId')]",
                "resourceGroup": "[resourceGroup().Name]"
            }
        ]
    },
```

Deze volgende sectie is voor de berichtroutes naar de eindpunten. Er is één set up voor elk eindpunt, zodat er één voor de Service Bus-wachtrij en één voor de container van het opslagaccount.

Houd er rekening mee dat de queryvoorwaarde voor de berichten worden gerouteerd naar de opslag is `level="storage"`, en de queryvoorwaarde voor de berichten die worden doorgestuurd naar de Service Bus-wachtrij is `level="critical"`.

```json
"routes": [
    {
        "name": "contosoStorageRoute",
        "source": "DeviceMessages",
        "condition": "level=\"storage\"",
        "endpointNames": [
            "[parameters('storage_endpoint')]"
            ],
        "isEnabled": true
    },
    {
        "name": "contosoSBQueueRoute",
        "source": "DeviceMessages",
        "condition": "level=\"critical\"",
        "endpointNames": [
            "[parameters('service_bus_queue_endpoint')]"
            ],
        "isEnabled": true
    }
],
```

Deze json ziet u de rest van de IoT Hub-sectie, met de standaard-gegevens en de SKU voor de hub.

```json
            "fallbackRoute": {
                "name": "$fallback",
                "source": "DeviceMessages",
                "condition": "true",
                "endpointNames": [
                    "events"
                ],
                "isEnabled": true
            }
        },
        "storageEndpoints": {
            "$default": {
                "sasTtlAsIso8601": "PT1H",
                "connectionString": "",
                "containerName": ""
            }
        },
        "messagingEndpoints": {
            "fileNotifications": {
                "lockDurationAsIso8601": "PT1M",
                "ttlAsIso8601": "PT1H",
                "maxDeliveryCount": 10
            }
        },
        "enableFileUploadNotifications": false,
        "cloudToDevice": {
            "maxDeliveryCount": 10,
            "defaultTtlAsIso8601": "PT1H",
            "feedback": {
                "lockDurationAsIso8601": "PT1M",
                "ttlAsIso8601": "PT1H",
                "maxDeliveryCount": 10
            }
        }
    },
    "sku": {
        "name": "[parameters('sku_name')]",
        "capacity": "[parameters('sku_units')]"
    }
}
```

### <a name="resources-service-bus-queue-authorization-rules"></a>Resources: Autorisatieregels voor Service Bus-wachtrij

De verificatieregel voor Service Bus-wachtrij wordt gebruikt om op te halen van de verbindingsreeks voor de Service Bus-wachtrij. Hierbij een `dependsOn` component om ervoor te zorgen dat deze niet is gemaakt voordat de Service Bus-naamruimte en de Service Bus-wachtrij.

```json
{
    "type": "Microsoft.ServiceBus/namespaces/queues/authorizationRules",
    "name": "[concat(variables('service_bus_namespace'), '/', variables('service_bus_queue'), '/', parameters('AuthRules_sb_queue'))]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "scale": null,
    "properties": {
        "rights": [
            "Send"
        ]
    },
    "dependsOn": [
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces/queues', variables('service_bus_namespace'), variables('service_bus_queue'))]"
    ]
},
```

### <a name="resources-consumer-group"></a>Resources: Consumentengroep

In deze sectie maakt u een groep consumenten voor de IoT Hub-gegevens in het tweede gedeelte van deze zelfstudie worden gebruikt door de Azure Stream Analytics.

```json
{
    "type": "Microsoft.Devices/IotHubs/eventHubEndpoints/ConsumerGroups",
    "name": "[concat(variables('iotHubName'), '/events/',parameters('consumer_group'))]",
    "apiVersion": "2018-04-01",
    "dependsOn": [
        "[concat('Microsoft.Devices/IotHubs/', variables('iotHubName'))]"
    ]
}
```

### <a name="resources-outputs"></a>Resources: Uitvoer

Als u verzenden van een waarde terug naar het script voor implementatie wilt moet worden weergegeven, gebruikt u een sectie uitvoer. Dit deel van de sjabloon retourneert de verbindingsreeks voor de Service Bus-wachtrij. Retourneert een waarde is niet vereist, is opgenomen als een voorbeeld van hoe resultaten worden geretourneerd naar de aanroepende script.

```json
"outputs": {
    "sbq_connectionString": {
      "type": "string",
      "value": "[Concat('Endpoint=sb://',variables('service_bus_namespace'),'.servicebus.windows.net/;SharedAccessKeyName=',parameters('AuthRules_sb_queue'),';SharedAccessKey=',listkeys(variables('queueAuthorizationRuleResourceId'),variables('sbVersion')).primaryKey,';EntityPath=',variables('service_bus_queue'))]"
    }
  }
```

## <a name="deploy-the-rm-template"></a>De RM-sjabloon implementeren

Als u wilt implementeren de sjabloon in Azure, de sjabloon en de parameterbestand uploaden naar Azure Cloud Shell en voer vervolgens een script om de sjabloon te implementeren. Open Azure Cloud Shell en meld u aan. In dit voorbeeld wordt PowerShell gebruikt.

Als u wilt de bestanden uploaden, selecteert u de **uploaden/downloaden van bestanden** pictogram in de menubalk en kies vervolgens uploaden.

![Cloud Shell in de menubalk met uploaden/downloaden van bestanden die zijn gemarkeerd](media/tutorial-routing-config-message-routing-RM-template/CloudShell_upload_files.png)

Gebruik de Verkenner die wordt weergegeven gebruiken om de bestanden op uw lokale schijf vinden en deze selecteren en kies vervolgens **Open**.

Nadat de bestanden zijn geüpload, wordt een dialoogvenster resultaten ziet er ongeveer als de volgende afbeelding.

![Cloud Shell in de menubalk met uploaden/downloaden van bestanden die zijn gemarkeerd](media/tutorial-routing-config-message-routing-RM-template/CloudShell_upload_results.png)

De bestanden zijn geüpload naar de share die wordt gebruikt door uw Cloud Shell-exemplaar. 

Voer het script om uit te voeren van de implementatie. De laatste regel van dit script wordt de variabele die is ingesteld om te worden geretourneerd, de verbindingsreeks van de Service Bus-wachtrij opgehaald.

Deze variabelen worden ingesteld in dit script.

**$RGName** is de naam van de resource waarop om de sjabloon te implementeren. Dit veld is gemaakt voordat u de sjabloon implementeert.

**$location** is de Azure-locatie moet worden gebruikt voor de sjabloon, zoals 'westus'.

**deploymentname** is een naam die u toewijst aan de implementatie om op te halen van de waarde van de bestaande variabele.

Hier volgt het PowerShell-script. Kopieer deze PowerShell-script en plak deze in de Cloud Shell-venster en vervolgens drukt u op Enter uit te voeren.

```powershell
$RGName="ContosoResources"
$location = "westus"
$deploymentname="contoso-routing"

# Remove the resource group if it already exists. 
#Remove-AzResourceGroup -name $RGName 
# Create the resource group.
New-AzResourceGroup -name $RGName -Location $location 

# Set a path to the parameter file. 
$parameterFile = "$HOME/template_iothub_parameters.json"
$templateFile = "$HOME/template_iothub.json"

# Deploy the template.
New-AzResourceGroupDeployment `
    -Name $deploymentname `
    -ResourceGroupName $RGName `
    -TemplateParameterFile $parameterFile `
    -TemplateFile $templateFile `
    -verbose

# Get the returning value of the connection string.
(Get-AzResourceGroupDeployment -ResourceGroupName $RGName -Name $deploymentname).Outputs.sbq_connectionString.value
```

Als u scriptfouten hebt, kunt u het script lokaal bewerken, opnieuw te uploaden naar de Cloud Shell en voer het script opnieuw uit. Nadat het script uitgevoerd is, kunt u doorgaan met de volgende stap.

## <a name="create-simulated-device"></a>Een gesimuleerd apparaat maken

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

## <a name="view-message-routing-in-the-portal"></a>Routering van berichten in de portal weergeven

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u beschikt over alle van de resources die u instelt en de berichtroutes zijn geconfigureerd, Ga naar de volgende zelfstudie voor meer informatie over het verwerken en de informatie over de gerouteerde berichten weer te geven.

> [!div class="nextstepaction"]
> [Deel 2: het bericht routering resultaten weergeven](tutorial-routing-view-message-routing-results.md)