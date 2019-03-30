---
title: Berichtroutering configureren voor Azure IoT Hub met behulp van de Azure CLI | Microsoft Docs
description: Berichtroutering configureren voor Azure IoT Hub met behulp van de Azure CLI
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 53a0645c42255d5f741d31c411ca70e1e009febc
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661578"
---
# <a name="tutorial-use-the-azure-cli-to-configure-iot-hub-message-routing"></a>Zelfstudie: De Azure CLI gebruiken voor het configureren van IoT Hub-berichtroutering

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="download-the-script-optional"></a>Download het script (optioneel)

Voor het tweede gedeelte van deze zelfstudie, die u kunt downloaden en uitvoeren van een toepassing met Visual Studio om berichten te verzenden naar de IoT Hub. Er is een map in de download de Azure Resource Manager-sjabloon en parameters in, evenals de Azure CLI en PowerShell-scripts bevat.

Als u de klaar script weergeven wilt, downloadt u de [Azure IoT C# voorbeelden](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Decomprimeer het bestand master.zip. De Azure CLI-script is in /iot-hub/Tutorials/Routing/SimulatedDevice/resources/als **iothub_routing_cli.azcli**.

## <a name="use-the-azure-cli-to-create-your-resources"></a>De Azure CLI gebruiken om uw resources te maken

Er zijn verschillende namen van voorbeeldresources die globaal uniek zijn, zoals de naam van de IoT Hub en de naam van het opslagaccount moeten. Als u wilt dit eenvoudiger maken die namen van voorbeeldresources worden toegevoegd met een willekeurige alfanumerieke waarde, de zogenaamde *randomValue*. De randomValue wordt gegenereerd zodra aan de bovenkant van het script en toegevoegd aan de namen van voorbeeldresources zo nodig in het script. Als u niet dat deze willekeurige wilt, kunt u dit instellen op een lege tekenreeks of op een specifieke waarde. 

> [!IMPORTANT]
> De variabelen die zijn ingesteld in het eerste script worden ook gebruikt door de routering scriptuitvoering, dus alle van het script in dezelfde Cloud Shell-sessie. Als u een nieuwe sessie voor het uitvoeren van het script voor het instellen van de routering opent, worden worden enkele van de variabelen waarden ontbreekt.
>

Kopieer en plak het onderstaande script in Cloud Shell en druk op Enter. Het script een regel wordt uitgevoerd op een tijdstip. De eerste sectie van het script wordt de basis resources maken voor deze zelfstudie, met inbegrip van de storage-account, IoT-Hub, Service Bus Namespace en Service Bus-wachtrij. Als u de rest van de zelfstudie doorloopt, elk blok van script Kopieer en plak deze in Cloud Shell uit te voeren.

> [!TIP]
> Een tip over foutopsporing: met dit script maakt gebruik van het symbool voortzetting (de backslash `\`) om het script beter leesbaar. Als u een probleem opgetreden bij het uitvoeren van het script hebt, zorg er dan voor dat er staan geen spaties na een van de backslash-tekens.
> 

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account. 
# This field is used when setting up the routing rules.
subscriptionID=$(az account show --query id)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# This command installs the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that 
#   don't have to be globally unique.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, 
#   so add a random value to the end.
iotHubName=ContosoTestHub$randomValue 
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub for the 'events' endpoint.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, 
#   so add a random value to the end.
storageAccountName=contosostorage$randomValue
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key. 
#    You need this to create the container.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"') 

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# The Service Bus namespace must be globally unique, 
#   so add a random value to the end.
sbNamespace=ContosoSBNamespace$randomValue
echo "Service Bus namespace = " $sbNamespace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNamespace \
    --location $location

# The Service Bus queue name must be globally unique, 
#   so add a random value to the end.
sbQueueName=ContosoSBQueue$randomValue
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNamespace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName
```

Nu dat de basis resources zijn ingesteld, kunt u de routering van berichten kunt configureren.

## <a name="set-up-message-routing"></a>Berichtroutering instellen

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

Gebruik voor het maken van een eindpunt routering [az iot hub routering-eindpunt maken](/cli/azure/iot/hub/routing-endpoint.md?view=azure-cli-latest#az-iot-hub-routing-endpoint-create). Gebruik voor het maken van de bericht-route voor het eindpunt [az iot hub route maken](/cli/azure/iot/hub/route?view=azure-cli-latest#az-iot-hub-route-create).

### <a name="route-to-a-storage-account"></a>Doorsturen naar een opslagaccount

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

Eerst, instellen van het eindpunt voor het opslagaccount en instellen van de route. 

Deze variabelen worden ingesteld:

**storageConnectionString**: Deze waarde wordt opgehaald uit het opslagaccount dat is ingesteld in het vorige script. Deze wordt gebruikt door de berichtroutering voor toegang tot het opslagaccount.

  **resourceGroup**: Er zijn twee exemplaren van de resourcegroep--stel ze in op de resourcegroep.

**eindpunt subscriptionID**: Dit veld is ingesteld op de Azure abonnements-id voor het eindpunt. 

**endpointType**: Dit veld is het type eindpunt. Deze waarde moet worden ingesteld op `azurestoragecontainer`, `eventhub`, `servicebusqueue`, of `servicebustopic`. Voor uw doeleinden hier, ingesteld op `azurestoragecontainer`.

**iotHubName**: Dit veld is de naam van de hub die de routering doet.

**containerName**: Dit veld is de naam van de container in het opslagaccount waarnaar de gegevens worden geschreven.

**Codering**: Dit veld is een `avro` of `json`. Dit geeft aan dat de indeling van de opgeslagen gegevens.

**routeName**: Dit veld is de naam van de route die u instelt. 

**endpointName**: Dit veld is de naam van het eindpunt. 

**Ingeschakeld**: Dit veld standaard ingesteld op `true`, waarmee wordt aangegeven dat de route van het bericht moet worden ingeschakeld nadat gemaakt.

**Voorwaarde**: Dit veld is de query die wordt gebruikt om te filteren op de berichten die worden verzonden naar dit eindpunt. De queryvoorwaarde voor de berichten worden gerouteerd naar de opslag is `level="storage"`.

Met dit script kopiëren en plak deze in uw Cloud Shell-venster en voer deze uit.

```azurecli
##### ROUTING FOR STORAGE ##### 

endpointName="ContosoStorageEndpoint"
endpointType="azurestoragecontainer"
routeName="ContosoStorageRoute"
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)
```

De volgende stap is het maken van de routering-eindpunt voor het opslagaccount. Ook geeft u de container waarin de resultaten worden opgeslagen. De container is eerder gemaakt wanneer het opslagaccount is gemaakt.

```azurecli
# Create the routing endpoint for storage.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName \
  --resource-group $resourceGroup \
  --encoding avro
```

Maak vervolgens de route voor het opslageindpunt. De route bericht geeft aan waar de berichten die voldoen aan de queryspecificatie worden verzonden. 

```azurecli
# Create the route for the storage endpoint.
az iot hub route create \
  --name $routeName \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName \
  --enabled \
  --condition $condition
```

### <a name="route-to-a-service-bus-queue"></a>Routeren naar een Service Bus-wachtrij

Stel nu de routering in voor de Service Bus-wachtrij. Als u wilt ophalen van de verbindingsreeks voor de Service Bus-wachtrij, moet u een autorisatieregel die de juiste rechten heeft gedefinieerd. Het volgende script maakt u een autorisatieregel voor de Service Bus-wachtrij met de naam `sbauthrule`, en stelt u de rechten op `Listen Manage Send`. Zodra deze autorisatieregel is gedefinieerd, kunt u deze kunt gebruiken om op te halen van de verbindingsreeks voor de wachtrij.

```azurecli
# Create the authorization rule for the Service Bus queue.
az servicebus queue authorization-rule create \
  --name "sbauthrule" \
  --namespace-name $sbNamespace \
  --queue-name $sbQueueName \
  --resource-group $resourceGroup \
  --rights Listen Manage Send \
  --subscription $subscriptionID
```

Nu de autorisatieregel gebruiken om op te halen van de verbindingsreeks voor de Service Bus-wachtrij.

```azurecli
# Get the Service Bus queue connection string.
# The "-o tsv" ensures it is returned without the default double-quotes.
sbqConnectionString=$(az servicebus queue authorization-rule keys list \
  --name "sbauthrule" \
  --namespace-name $sbNamespace \
  --queue-name $sbQueueName \
  --resource-group $resourceGroup \
  --subscription $subscriptionID \
  --query primaryConnectionString -o tsv)

# Show the Service Bus queue connection string.
echo "service bus queue connection string = " $sbqConnectionString
```

Nu instellen van het eindpunt van de Routering en de route bericht voor de Service Bus-wachtrij. Deze variabelen worden ingesteld:

**endpointName**: Dit veld is de naam van het eindpunt. 

**endpointType**: Dit veld is het type eindpunt. Deze waarde moet worden ingesteld op `azurestoragecontainer`, `eventhub`, `servicebusqueue`, of `servicebustopic`. Voor uw doeleinden hier, ingesteld op `servicebusqueue`.

**routeName**: Dit veld is de naam van de route die u instelt. 

**Voorwaarde**: Dit veld is de query die wordt gebruikt om te filteren op de berichten die worden verzonden naar dit eindpunt. De queryvoorwaarde voor de berichten die worden doorgestuurd naar de Service Bus-wachtrij is `level="critical"`.

Hier volgt de Azure CLI voor het eindpunt van de Routering en de route bericht voor de Service Bus-wachtrij.

```azurecli
endpointName="ContosoSBQueueEndpoint"
endpointType="ServiceBusQueue"
routeName="ContosoSBQueueRoute"
condition='level="critical"'

# Set up the routing endpoint for the Service Bus queue.
# This uses the Service Bus queue connection string.
az iot hub routing-endpoint create \
  --connection-string $sbqConnectionString \
  --endpoint-name $endpointName \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --resource-group $resourceGroup 

# Set up the message route for the Service Bus queue endpoint.
az iot hub route create --name $routeName \
  --hub-name $iotHubName \
  --source-type devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName \
  --enabled \
  --condition $condition
  ```

### <a name="view-message-routing-in-the-portal"></a>Routering van berichten in de portal weergeven

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt de resources die u instelt en het berichtroutes zijn geconfigureerd, Ga naar de volgende zelfstudie voor meer informatie over het verzenden van berichten naar de IoT-hub en ze worden doorgestuurd naar de andere bestemmingen te zien. 

> [!div class="nextstepaction"]
> [Deel 2: het bericht routering resultaten weergeven](tutorial-routing-view-message-routing-results.md)