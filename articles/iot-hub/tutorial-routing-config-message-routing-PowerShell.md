---
title: Berichtroutering configureren voor Azure IoT Hub met behulp van Azure PowerShell | Microsoft Docs
description: Berichtroutering configureren voor Azure IoT Hub met behulp van Azure PowerShell
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 51e9bc85c2ee843aa096674a25a1f634bd08b838
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660923"
---
# <a name="tutorial-use-azure-powershell-to-configure-iot-hub-message-routing"></a>Zelfstudie: Azure PowerShell gebruiken voor het configureren van IoT Hub-berichtroutering

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="download-the-script-optional"></a>Download het script (optioneel)

Voor het tweede gedeelte van deze zelfstudie, die u kunt downloaden en uitvoeren van een toepassing met Visual Studio om berichten te verzenden naar de IoT Hub. Er is een map in de download de Azure Resource Manager-sjabloon en parameters in, evenals de Azure CLI en PowerShell-scripts bevat. 

Als u de klaar script weergeven wilt, downloadt u de [Azure IoT C# voorbeelden](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Decomprimeer het bestand master.zip. De Azure CLI-script is in /iot-hub/Tutorials/Routing/SimulatedDevice/resources/als **iothub_routing_psh.ps1**.

## <a name="create-your-resources"></a>Uw resources maken

Beginnen met het maken van de resources met PowerShell.

### <a name="use-powershell-to-create-your-base-resources"></a>PowerShell gebruiken om uw basis resources te maken

Er zijn verschillende namen van voorbeeldresources die globaal uniek zijn, zoals de naam van de IoT Hub en de naam van het opslagaccount moeten. Als u wilt dit eenvoudiger maken die namen van voorbeeldresources worden toegevoegd met een willekeurige alfanumerieke waarde, de zogenaamde *randomValue*. De randomValue wordt gegenereerd zodra aan de bovenkant van het script en toegevoegd aan de namen van voorbeeldresources zo nodig in het script. Als u niet dat deze willekeurige wilt, kunt u dit instellen op een lege tekenreeks of op een specifieke waarde. 

> [!IMPORTANT]
> De variabelen die zijn ingesteld in het eerste script worden ook gebruikt door de routering scriptuitvoering, dus alle van het script in dezelfde Cloud Shell-sessie. Als u een nieuwe sessie voor het uitvoeren van het script voor het instellen van de routering opent, worden worden enkele van de variabelen waarden ontbreekt. 
>

Kopieer en plak het onderstaande script in Cloud Shell en druk op Enter. Het script een regel wordt uitgevoerd op een tijdstip. De eerste sectie van het script wordt de basis resources maken voor deze zelfstudie, met inbegrip van de storage-account, IoT-Hub, Service Bus Namespace en Service Bus-wachtrij. Als u de zelfstudie doorloopt, elk blok van script Kopieer en plak deze in Cloud Shell uit te voeren.

```azurepowershell-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing rules.
$subscriptionID = (Get-AzContext).Subscription.Id

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves the first 6 digits of a random value.
$randomValue = "$(Get-Random)".Substring(0,6)

# Set the values for the resource names that don't have to be globally unique.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, 
#   so add a random value to the end.
$iotHubName = "ContosoTestHub" + $randomValue
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1 

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random value to the end.
$storageAccountName = "contosostorage" + $randomValue
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
# Retrieve the connection string from the context. 
$storageConnectionString = $storageAccount.Context.ConnectionString
Write-Host "storage connection string = " $storageConnectionString 

# Create the container in the storage account.
New-AzStorageContainer -Name $containerName `
    -Context $storageAccount.Context

# The Service Bus namespace must be globally unique,
#   so add a random value to the end.
$serviceBusNamespace = "ContosoSBNamespace" + $randomValue
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random value to the end.
$serviceBusQueueName  = "ContosoSBQueue" + $randomValue
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName  `
    -EnablePartitioning $False 
```

### <a name="create-a-simulated-device"></a>Een gesimuleerd apparaat maken

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

Nu dat de basis resources zijn ingesteld, kunt u de routering van berichten kunt configureren.

## <a name="set-up-message-routing"></a>Berichtroutering instellen

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

Gebruik voor het maken van een eindpunt routering [toevoegen AzIotHubRoutingEndpoint](/powershell/module/az.iothub/Add-AzIotHubRoutingEndpoint). Gebruik voor het maken van de berichten route voor het eindpunt [toevoegen AzIotHubRoute](/powershell/module/az.iothub/Add-AzIoTHubRoute).

### <a name="route-to-a-storage-account"></a>Doorsturen naar een opslagaccount 

Eerst, instellen van het eindpunt voor de storage-account en maakt u de bericht-route.

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

Deze variabelen worden ingesteld:

**resourceGroup**: Er zijn twee exemplaren van dit veld--van beide ingesteld op de resourcegroep.

**name**: Dit veld is de naam van de IoT-Hub waarop de routering van toepassing.

**endpointName**: Dit veld is de naam van het eindpunt. 

**endpointType**: Dit veld is het type eindpunt. Deze waarde moet worden ingesteld op `azurestoragecontainer`, `eventhub`, `servicebusqueue`, of `servicebustopic`. Voor uw doeleinden hier, ingesteld op `azurestoragecontainer`.

**abonnements-id**: Dit veld is ingesteld op de abonnements-id voor uw Azure-account.

**storageConnectionString**: Deze waarde wordt opgehaald uit het opslagaccount dat is ingesteld in het vorige script. Deze wordt gebruikt door de routering voor toegang tot het opslagaccount.

**containerName**: Dit veld is de naam van de container in het opslagaccount waarnaar de gegevens worden geschreven.

**Codering**: Stel dit veld in een `AVRO` of `JSON`. Hiermee wordt de indeling van de opgeslagen gegevens. De standaardwaarde is AVRO.

**routeName**: Dit veld is de naam van de route die u instelt. 

**Voorwaarde**: Dit veld is de query die wordt gebruikt om te filteren op de berichten die worden verzonden naar dit eindpunt. De queryvoorwaarde voor de berichten worden gerouteerd naar de opslag is `level="storage"`.

**Ingeschakeld**: Dit veld standaard ingesteld op `true`, waarmee wordt aangegeven dat de route van het bericht moet worden ingeschakeld nadat gemaakt.

Met dit script Kopieer en plak deze in uw Cloud Shell-venster.

```powershell
##### ROUTING FOR STORAGE #####

$endpointName = "ContosoStorageEndpoint"
$endpointType = "azurestoragecontainer"
$routeName = "ContosoStorageRoute"
$condition = 'level="storage"'
```

De volgende stap is het maken van de routering-eindpunt voor het opslagaccount. Ook geeft u de container waarin de resultaten worden opgeslagen. De container is gemaakt toen het storage-account is gemaakt.

```powershell
# Create the routing endpoint for storage.
# Specify 'AVRO' or 'JSON' for the encoding of the data.
Add-AzIotHubRoutingEndpoint `
  -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EndpointName $endpointName `
  -EndpointType $endpointType `
  -EndpointResourceGroup $resourceGroup `
  -EndpointSubscriptionId $subscriptionId `
  -ConnectionString $storageConnectionString  `
  -ContainerName $containerName `
  -Encoding AVRO
```

Maak vervolgens de bericht-route voor het opslageindpunt. De route bericht geeft aan waar de berichten die voldoen aan de queryspecificatie worden verzonden.

```powershell
# Create the route for the storage endpoint.
Add-AzIotHubRoute `
   -ResourceGroupName $resourceGroup `
   -Name $iotHubName `
   -RouteName $routeName `
   -Source DeviceMessages `
   -EndpointName $endpointName `
   -Condition $condition `
   -Enabled 
```

### <a name="route-to-a-service-bus-queue"></a>Routeren naar een Service Bus-wachtrij

Stel nu de routering in voor de Service Bus-wachtrij. Als u wilt ophalen van de verbindingsreeks voor de Service Bus-wachtrij, moet u een autorisatieregel die de juiste rechten heeft gedefinieerd. Het volgende script maakt u een autorisatieregel voor de Service Bus-wachtrij met de naam `sbauthrule`, en stelt u de rechten op `Listen Manage Send`. Zodra deze autorisatieregel is ingesteld, kunt u deze kunt gebruiken om op te halen van de verbindingsreeks voor de wachtrij.

```powershell
##### ROUTING FOR SERVICE BUS QUEUE #####

# Create the authorization rule for the Service Bus queue.
New-AzServiceBusAuthorizationRule `
  -ResourceGroupName $resourceGroup `
  -NamespaceName $serviceBusNamespace `
  -Queue $serviceBusQueueName `
  -Name "sbauthrule" `
  -Rights @("Manage","Listen","Send")
```

Nu de autorisatieregel gebruiken om op te halen van de Service Bus-wachtrij-sleutel. Deze autorisatieregel wordt gebruikt om op te halen van de verbindingsreeks later in het script.

```powershell
$sbqkey = Get-AzServiceBusKey `
    -ResourceGroupName $resourceGroup `
    -NamespaceName $serviceBusNamespace `
    -Queue $servicebusQueueName `
    -Name "sbauthrule"
```

Nu instellen van het eindpunt van de Routering en de route bericht voor de Service Bus-wachtrij. Deze variabelen worden ingesteld:

**endpointName**: Dit veld is de naam van het eindpunt. 

**endpointType**: Dit veld is het type eindpunt. Deze waarde moet worden ingesteld op `azurestoragecontainer`, `eventhub`, `servicebusqueue`, of `servicebustopic`. Voor uw doeleinden hier, ingesteld op `servicebusqueue`.

**routeName**: Dit veld is de naam van de route die u instelt. 

**Voorwaarde**: Dit veld is de query die wordt gebruikt om te filteren op de berichten die worden verzonden naar dit eindpunt. De queryvoorwaarde voor de berichten die worden doorgestuurd naar de Service Bus-wachtrij is `level="critical"`.

Hier volgt de Azure PowerShell voor de routering van berichten voor de Service Bus-wachtrij.

```powershell
$endpointName = "ContosoSBQueueEndpoint"
$endpointType = "servicebusqueue"
$routeName = "ContosoSBQueueRoute"
$condition = 'level="critical"'

# Add the routing endpoint, using the connection string property from the key.
Add-AzIotHubRoutingEndpoint `
  -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EndpointName $endpointName `
  -EndpointType $endpointType `
  -EndpointResourceGroup $resourceGroup `
  -EndpointSubscriptionId $subscriptionId `
  -ConnectionString $sbqkey.PrimaryConnectionString

# Set up the message route for the Service Bus queue endpoint.
Add-AzIotHubRoute `
   -ResourceGroupName $resourceGroup `
   -Name $iotHubName `
   -RouteName $routeName `
   -Source DeviceMessages `
   -EndpointName $endpointName `
   -Condition $condition `
   -Enabled 
```

### <a name="view-message-routing-in-the-portal"></a>Routering van berichten in de portal weergeven

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt de resources die u instelt en het berichtroutes zijn geconfigureerd, Ga naar de volgende zelfstudie voor meer informatie over het verzenden van berichten naar de IoT-hub en ze worden doorgestuurd naar de andere bestemmingen te zien. 

> [!div class="nextstepaction"]
> [Deel 2: het bericht routering resultaten weergeven](tutorial-routing-view-message-routing-results.md)