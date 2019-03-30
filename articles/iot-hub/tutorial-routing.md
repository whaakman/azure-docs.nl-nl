---
title: Berichtroutering configureren voor Azure IoT Hub met behulp van de Azure CLI en Azure portal | Microsoft Docs
description: Berichtroutering configureren voor Azure IoT Hub met behulp van de Azure CLI en Azure portal
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/12/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 2f382c31c6bfb6ab71afd495c4c3f702715633c0
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661885"
---
# <a name="tutorial-use-the-azure-cli-and-azure-portal-to-configure-iot-hub-message-routing"></a>Zelfstudie: De Azure CLI en Azure portal gebruiken om te configureren met het IoT Hub-berichtroutering

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="use-the-azure-cli-to-create-the-base-resources"></a>De Azure CLI gebruiken voor de basis-resources maken

In deze zelfstudie maakt gebruik van de Azure CLI om de basis-resources te maken en gebruikt vervolgens de [Azure-portal](https://portal.azure.com) om weer te geven over het configureren van de routering van berichten en instellen van het virtuele apparaat voor het testen.

Er zijn verschillende namen van voorbeeldresources die globaal uniek zijn, zoals de naam van de IoT Hub en de naam van het opslagaccount moeten. Als u wilt dit eenvoudiger maken die namen van voorbeeldresources worden toegevoegd met een willekeurige alfanumerieke waarde, de zogenaamde *randomValue*. De randomValue wordt gegenereerd zodra aan de bovenkant van het script en toegevoegd aan de namen van voorbeeldresources zo nodig in het script. Als u niet dat deze willekeurige wilt, kunt u dit instellen op een lege tekenreeks of op een specifieke waarde.

Kopieer en plak het onderstaande script in Cloud Shell en druk op Enter. Het script een regel wordt uitgevoerd op een tijdstip. Hiermee maakt u de algemene resources voor deze zelfstudie, met inbegrip van de storage-account, IoT-Hub, Service Bus Namespace en Service Bus-wachtrij.

Een opmerking over foutopsporing: met dit script maakt gebruik van het symbool voortzetting (de backslash `\`) om het script beter leesbaar. Als u een probleem opgetreden bij het uitvoeren van het script hebt, zorg er dan voor dat er staan geen spaties na een van de backslash-tekens.

```azurecli-interactive
# This retrieves the subscription id of the account 
#   in which you're logged in.
# This field is used to set up the routing rules.
subscriptionID=$(az account show --query id)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# Set the values for the resource names that 
#   don't have to be globally unique.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults

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

```

Nu dat de basis resources zijn ingesteld, kunt u de routering van berichten de [Azure-portal](https://portal.azure.com).

## <a name="set-up-message-routing"></a>Berichtroutering instellen

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

### <a name="route-to-a-storage-account"></a>Doorsturen naar een opslagaccount

Stel nu de routering in voor het opslagaccount. Ga naar het deelvenster Berichtroutering en voeg een route toe. Definieer bij het toevoegen van de route een nieuw eindpunt voor de route. Nadat deze routering is ingesteld, waar berichten de **niveau** eigenschap is ingesteld op **opslag** automatisch naar een opslagaccount worden geschreven. 

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

1. In de [Azure-portal](https://portal.azure.com), selecteer **resourcegroepen**, selecteer vervolgens de resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**.

2. Selecteer de IoT-hub in de lijst met resources. In deze zelfstudie wordt gebruikgemaakt van **ContosoTestHub**.

3. Selecteer **berichtroutering**. In de **berichtroutering** venster +**toevoegen**. Op de **een Route toevoegen** venster +**toevoegen** naast het veld eindpunt om weer te geven van de ondersteunde eindpunten, zoals weergegeven in de volgende afbeelding:

   ![Beginnen met het toevoegen van een eindpunt voor een route](./media/tutorial-routing/message-routing-add-a-route-w-storage-ep.png)

4. Selecteer **Blob-opslag**. U ziet de **toevoegen van een opslageindpunt** deelvenster.

   ![Een eindpunt toevoegen](./media/tutorial-routing/message-routing-add-storage-ep.png)

5. Voer een naam in voor het eindpunt. Deze zelfstudie wordt gebruikgemaakt van **ContosoStorageEndpoint**.

6. Selecteer **een container kiezen**. Er wordt een lijst met opslagaccounts weergegeven. Selecteer het account dat u hebt ingesteld in de voorbereidende stappen. In deze zelfstudie wordt gebruikgemaakt van **contosostorage**. Er wordt een lijst met containers in die opslagaccount weergegeven. **Selecteer** de container u hebt ingesteld in de stappen voor gegevensvoorbereiding. In deze zelfstudie wordt gebruikgemaakt van **contosoresults**. U terugkeert naar de **toevoegen van een opslageindpunt** deelvenster en bekijken van de selecties die u hebt gemaakt.

7. Stel de codering voor AVRO of JSON. Gebruik ten behoeve van deze zelfstudie de standaardinstellingen voor de rest van de velden. Dit veld wordt grijs weergegeven als de geselecteerde regio biedt geen ondersteuning voor JSON-codering.,

   > [!NOTE]
   > U kunt de indeling van de blob-naam instellen met de **indeling van blobbestandsnaam**. De standaardwaarde is `{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}`. De indeling moet {iothub}, {partition}, {YYYY}, {MM}, {DD}, {HH}, en {mm} bevatten, in willekeurige volgorde.
   >
   > Als u bijvoorbeeld de indeling van de standaardnaam voor het blobbestand gebruikt, de naam van de hub ContosoTestHub is en de datum en tijd 30 oktober 2018 om 10:56 uur, dan ziet de blobnaam er als volgt uit: `ContosoTestHub/0/2018/10/30/10/56`.
   > 
   > De blobs worden geschreven in de Avro-indeling.
   >

8. Selecteer **maken** het opslageindpunt maken en toe te voegen aan de route. U gaat terug naar het deelvenster **Route toevoegen**.

9. Vul nu de rest van de informatie voor de routeringsquery in. Deze query specificeert de criteria voor het verzenden van berichten naar de opslagcontainer die u zojuist hebt toegevoegd als eindpunt. Vul de velden in op het scherm.

   **Naam**: Voer een naam in voor uw routeringsquery. Deze zelfstudie wordt gebruikgemaakt van **ContosoStorageRoute**.

   **Eindpunt**: Geeft het eindpunt weer dat u net hebt ingesteld.

   **Gegevensbron**: Selecteer **Telemetrieberichten apparaat** uit de vervolgkeuzelijst.

   **Route inschakelen**: Zorg ervoor dat dit veld is ingesteld op `enabled`.
   
   **Routeringsquery**: Voer `level="storage"` in als querytekenreeks.

   ![Een routering query voor de storage-account maken](./media/tutorial-routing/message-routing-finish-route-storage-ep.png)  

   Selecteer **Opslaan**. Wanneer dit is voltooid, gaat u terug naar het deelvenster Berichtroutering, waar u uw nieuwe routeringsquery voor opslag kunt zien. Sluit het deelvenster Routes, waarna u terugkeert naar de pagina Resourcegroep.

### <a name="route-to-a-service-bus-queue"></a>Routeren naar een Service Bus-wachtrij

Stel nu de routering in voor de Service Bus-wachtrij. Ga naar het deelvenster Berichtroutering en voeg een route toe. Definieer bij het toevoegen van de route een nieuw eindpunt voor de route. Nadat deze route is ingesteld, waar berichten de **niveau** eigenschap is ingesteld op **kritieke** worden geschreven naar de Service Bus-wachtrij, die een logische App, die vervolgens een e-mailbericht met de gegevens verzendt wordt geactiveerd.

1. Op de pagina van de resourcegroep, selecteert u uw IoT-hub en vervolgens **berichtroutering**.

2. In de **berichtroutering** venster +**toevoegen**.

3. Op de **een Route toevoegen** deelvenster Selecteer +**toevoegen** naast het veld eindpunt. Selecteer een **Service Bus-wachtrij**. Het deelvenster **Service Bus-eindpunt toevoegen** wordt weergegeven.

   ![Een service bus-eindpunt toevoegen](./media/tutorial-routing/message-routing-add-sbqueue-ep.png)

4. Vul de velden in:

   **Naam van het eindpunt**: Voer een naam in voor het eindpunt. Deze zelfstudie wordt gebruikgemaakt van **ContosoSBQueueEndpoint**.
   
   **Service Bus-naamruimte**: Gebruik de vervolgkeuzelijst om de service bus-naamruimte die u hebt ingesteld in de stappen voor gegevensvoorbereiding. In deze zelfstudie wordt gebruikgemaakt van **ContosoSBNamespace**.

   **Service Bus-wachtrij**: Gebruik de vervolgkeuzelijst om de Service Bus-wachtrij. In deze zelfstudie wordt gebruikgemaakt van **contososbqueue**.

5. Selecteer **maken** om toe te voegen van de Service Bus-wachtrij-eindpunt. U gaat terug naar het deelvenster **Route toevoegen**.

6. Vul nu de rest van de informatie voor de routeringsquery in. Deze query specificeert de criteria voor het verzenden van berichten naar de Service Bus-wachtrij die u zojuist hebt toegevoegd als eindpunt. Vul de velden in op het scherm. 

   **Naam**: Voer een naam in voor uw routeringsquery. Deze zelfstudie wordt gebruikgemaakt van **ContosoSBQueueRoute**. 

   **Eindpunt**: Geeft het eindpunt weer dat u net hebt ingesteld.

   **Gegevensbron**: Selecteer **Telemetrieberichten apparaat** uit de vervolgkeuzelijst.

   **Routeringsquery**: Voer `level="critical"` in als querytekenreeks. 

   ![Een routering query voor de Service Bus-wachtrij maken](./media/tutorial-routing/message-routing-finish-route-sbq-ep.png)

7. Selecteer **Opslaan**. Wanneer u teruggaat naar het deelvenster Routes, ziet u de twee nieuwe routeringsregels zoals deze hier zijn weergegeven.

   ![De routes die u zojuist hebt ingesteld](./media/tutorial-routing/message-routing-show-both-routes.png)

8. U kunt zien dat de aangepaste eindpunten door het selecteren van instellen de **aangepaste eindpunten** tabblad.

   ![Het aangepaste eindpunt dat u net hebt ingesteld](./media/tutorial-routing/message-routing-show-custom-endpoints.png)

9. Sluit het deelvenster Berichtroutering, waarna u terugkeert naar het deelvenster Resourcegroep.

## <a name="create-a-simulated-device"></a>Een gesimuleerd apparaat maken

[!INCLUDE [iot-hub-include-create- imulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt de resources die u instelt en het berichtroutes zijn geconfigureerd, Ga naar de volgende zelfstudie voor meer informatie over het verzenden van berichten naar de IoT-hub en ze worden doorgestuurd naar de andere bestemmingen te zien. 

> [!div class="nextstepaction"]
> [Deel 2: het bericht routering resultaten weergeven](tutorial-routing-view-message-routing-results.md)
