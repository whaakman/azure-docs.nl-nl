---
title: 'Zelfstudie: met behulp van Azure IoT Hub bericht enrichments'
description: Zelfstudie over het gebruik van bericht enrichments voor Azure IoT Hub-berichten weergeven
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: e4906bf9f2aead69c315ddb7b2e3b10489378d87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66259072"
---
# <a name="tutorial-using-azure-iot-hub-message-enrichments-preview"></a>Zelfstudie: Met behulp van Azure IoT Hub bericht enrichments (preview)

*Bericht enrichments* is het vermogen van de IoT-Hub kunt u *stempel* berichten met aanvullende informatie voordat de berichten worden verzonden naar het opgegeven eindpunt. Eén reden voor het gebruik van bericht enrichments is om op te nemen van gegevens die kunnen worden gebruikt voor het vereenvoudigen van de downstream-verwerkingen. Bijvoorbeeld, het verrijkende telemetrieberichten van apparaten met een dubbele apparaattag belasting van klanten om te maken van de apparaatdubbel die API-voor deze informatie aanroepen verminderen. Zie voor meer informatie de [overzicht van bericht enrichments](iot-hub-message-enrichments-overview.md).

In deze zelfstudie gebruikt u de Azure CLI voor het instellen van de resources, met inbegrip van twee eindpunten die naar twee verschillende storage-containers verwijzen: **verrijkt** en **oorspronkelijke**. Vervolgens u gebruikt de [Azure-portal](https://portal.azure.com) configureren enrichments bericht moet worden toegepast op berichten die worden verzonden naar het eindpunt met alleen de **verrijkt** storage-container. U verzenden berichten naar de IoT-Hub die worden doorgestuurd naar beide storage-containers. Alleen de berichten verzonden naar het eindpunt voor de **verrijkt** opslagcontainer zal worden uitgebreid.

Hier volgen de taken die u wilt uitvoeren om deze zelfstudie te voltooien:

**Met behulp van IoT-Hub bericht enrichments**
> [!div class="checklist"]
> * Met de Azure CLI, maakt u de resources--een IoT-hub, een opslagaccount met twee eindpunten en de configuratie van de routering.
> * De Azure portal gebruiken voor het bericht enrichments configureren.
> * Een app die een IoT-apparaat verzenden van berichten naar de hub simuleert uitvoeren.
> * Bekijk de resultaten en controleer of dat het bericht enrichments werkt zoals verwacht.

## <a name="prerequisites"></a>Vereisten

* U hebt een abonnement op Azure nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

* Installeer [Visual Studio](https://www.visualstudio.com/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="retrieve-the-sample-code"></a>De voorbeeldcode halen

Download [IoT Apparaatsimulatie](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) en pak het uit. Deze opslagplaats bevat verschillende toepassingen, inclusief de functie die u gebruikt om berichten te verzenden naar de IoT hub.

Deze download bevat ook het script voor het maken van de resources die worden gebruikt voor het testen van bericht enrichments. Het script is in /azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/iothub_msgenrichment_cli.azcli. U kunt nu kijken naar het script en deze gebruiken. U kunt ook het script kopiëren rechtstreeks vanuit het artikel.

Wanneer u klaar bent om te testen, gebruikt u de toepassing Apparaatsimulatie uit deze download bericht te verzenden naar uw IoT-hub.

## <a name="set-up-and-configure-resources"></a>Instellen en configureren van resources

De Azure CLI-script configureert ook de twee routes naar de eindpunten die afzonderlijke storage-containers zijn naast het maken van de benodigde resources. Zie voor meer informatie over het configureren van de routering, de [routering zelfstudie](tutorial-routing.md). Nadat de resources zijn ingesteld, gebruikt u de [Azure-portal](https://portal.azure.com) bericht enrichments voor elk eindpunt configureren en ga vervolgens door met de stap testen.

> [!NOTE]
> Alle berichten worden gerouteerd naar beide eindpunten, maar alleen de berichten die naar het eindpunt met de geconfigureerde bericht enrichments zal worden uitgebreid.
>

U kunt het onderstaande script gebruiken of open het script in de map /resources van de gedownloade opslagplaats. Hier volgen de stappen die het script wordt uitgevoerd:

* Een IoT Hub maken.
* Een opslagaccount maken.
* Twee containers maken in de storage-account, één voor de verrijkt berichten en één voor berichten die niet worden uitgebreid.
* Instellen van de routering voor de twee verschillende opslagaccounts.
    * Een eindpunt voor elke container van het opslagaccount maken.
    * Een route voor elk van de containereindpunten van opslagaccounts maken.

Er zijn verschillende namen van voorbeeldresources die globaal uniek zijn, zoals de naam van de IoT Hub en de naam van het opslagaccount moeten. Als u het script gemakkelijker is uitgevoerd, de namen van voorbeeldresources worden toegevoegd met een willekeurige alfanumerieke waarde met de naam *randomValue*. De randomValue wordt gegenereerd zodra aan de bovenkant van het script en toegevoegd aan de namen van voorbeeldresources zo nodig in het script. Als u niet dat deze willekeurige wilt, kunt u dit instellen op een lege tekenreeks of op een specifieke waarde.

Als u dit nog niet hebt gedaan, opent u een [Cloud Shell-venster voor Bash.](https://shell.azure.com). Open het script in de uitgepakte opslagplaats, gebruikt u Ctrl + A om alles van het te selecteren en vervolgens op Ctrl + C om te kopiëren. U kunt ook de volgende CLI-script kopiëren of opent u het rechtstreeks in de cloudshell. Plak het script in de Azure cloud shell-venster met de rechtermuisknop op de opdrachtregel en selecteert u **plakken**. Het script wordt één instructie uitgevoerd op een tijdstip. Nadat het script niet meer werkt, selecteert u **Enter** om ervoor te zorgen dat deze de laatste opdracht wordt uitgevoerd. Het volgende codeblok ziet u het script dat wordt gebruikt, met opmerkingen waarin wordt uitgelegd wat het doet.

Hier vindt u de resources die zijn gemaakt door het script. **Verrijkt** betekent dat de resource voor berichten met enrichments is. **Oorspronkelijke** betekent dat de resource is voor berichten die niet worden uitgebreid.

| Name | Value |
|-----|-----|
| ResourceGroup | ContosoResourcesMsgEn |
| Containernaam | Origineel  |
| Containernaam | verrijkt  |
| De naam van de IoT-apparaat | Contoso-Test-Device |
| De naam van IoT Hub | ContosoTestHubMsgEn |
| De naam van opslagaccount | contosostorage |
| eindpunt 1 naam | ContosoStorageEndpointOriginal |
| eindpunt naam 2 | ContosoStorageEndpointEnriched|
| route Naam1 | ContosoStorageRouteOriginal |
| route naam 2 | ContosoStorageRouteEnriched |

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing rules.
subscriptionID=$(az account show --query id -o tsv)

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
location=westus2
resourceGroup=ContosoResourcesMsgEn
containerName1=original
containerName2=enriched
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique,
#   so add a random value to the end.
iotHubName=ContosoTestHubMsgEn$randomValue
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# You need a storage account that will have two containers
#   -- one for the original messages and
#   one for the enriched messages.
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
#    You need this to create the containers.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the containers in the storage account.
az storage container create --name $containerName1 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

az storage container create --name $containerName2 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
# If you are using Cloud Shell, you can scroll the window back up to retrieve this value.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

##### ROUTING FOR STORAGE #####

# You're going to have two routes and two endpoints.
# One points to container1 in the storage account
#   and includes all messages.
# The other points to container2 in the same storage account
#   and only includes enriched messages.

endpointType="azurestoragecontainer"
endpointName1="ContosoStorageEndpointOriginal"
endpointName2="ContosoStorageEndpointEnriched"
routeName1="ContosoStorageRouteOriginal"
routeName2="ContosoStorageRouteEnriched"

# for both endpoints, retrieve the messages going to storage
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)

# Create the routing endpoints and routes.
# Set the encoding format to either avro or json.

# This is the endpoint for container 1, for endpoint messages that are not enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName1 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName1 \
  --resource-group $resourceGroup \
  --encoding json

# This is the endpoint for container 2, for endpoint messages that are enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName2 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName2 \
  --resource-group $resourceGroup \
  --encoding json

# This is the route for messages that are not enriched.
# Create the route for the first storage endpoint.
az iot hub route create \
  --name $routeName1 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName1 \
  --enabled \
  --condition $condition

# This is the route for messages that are not enriched.
az iot hub route create \
  --name $routeName2 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName2 \
  --enabled \
  --condition $condition
```

Op dit moment de resources zijn klaar en de routering is geconfigureerd. U kunt de configuratie van de routering van het bericht in de portal weergeven en instellen van het bericht enrichments voor berichten naar de **verrijkt** storage-container.

### <a name="view-routing-and-configure-the-message-enrichments"></a>Routering en het bericht enrichments configureren

1. Ga naar uw IoT-Hub door te selecteren **resourcegroepen**, selecteer vervolgens de resourcegroep die is ingesteld voor deze zelfstudie (**ContosoResources_MsgEn**). De IoT-Hub niet vinden in de lijst en selecteer deze. Selecteer *berichtroutering** voor de Iot Hub.

   ![Selecteer berichtroutering](./media/tutorial-message-enrichments/select-iot-hub.png)

   Het bericht routering deelvenster bevat drie tabbladen-- **Routes**, **aangepaste eindpunten**, en **verrijken berichten**. U kunt de eerste twee tabbladen om te zien van de configuratie ingesteld door het script te bladeren. Gebruik het derde tabblad bericht enrichments toevoegen. Laten we verrijken berichten naar het eindpunt voor de storage-container met de naam **verrijkt**. Vul de naam en waarde, en selecteer vervolgens het eindpunt **ContosoStorageEndpointEnriched** in de vervolgkeuzelijst. Hier volgt een voorbeeld van het instellen van een verrijking die door de naam van de IoT Hub worden toegevoegd aan het bericht:

   ![Eerste verrijking toevoegen](./media/tutorial-message-enrichments/add-message-enrichments.png)

2. Deze waarden toevoegen aan de lijst voor het eindpunt ContosoStorageEndpointEnriched.

   | Name | Value | Eindpunt (vervolgkeuzelijst) |
   | ---- | ----- | -------------------------|
   | myIotHub | $iothubname | AzureStorageContainers > ContosoStorageEndpointEnriched |
   | Locatie apparaat | $twin.tags.location | AzureStorageContainers > ContosoStorageEndpointEnriched |
   |customerID | 6ce345b8-1e4a-411e-9398-d34587459a3a | AzureStorageContainers > ContosoStorageEndpointEnriched |

   > [!NOTE]
   > Als uw apparaat beschikt niet over een dubbel, wordt de waarde die u hier in plaatsen als een tekenreeks voor de waarde in het bericht enrichments worden geautoriseerd. Als u wilt zien van het apparaat informatie twin, gaat u naar uw hub in de portal, selecteert u vervolgens **IoT-apparaten**, selecteer uw apparaat en selecteer vervolgens **apparaatdubbel** aan de bovenkant van de pagina.
   >
   > U kunt de apparaatdubbel-informatie voor het toevoegen van tags (zoals locatie) en deze instellen op een specifieke waarde als u wilt bewerken. Zie [Understand and use device twins in IoT Hub](iot-hub-devguide-device-twins.md) (Apparaatdubbelen begrijpen en gebruiken in IoT Hub) voor meer informatie

3. Wanneer u klaar bent, is het deelvenster ziet die vergelijkbaar is met deze afbeelding:

   ![Tabel met alle enrichments toegevoegd](./media/tutorial-message-enrichments/all-message-enrichments.png)

4. Selecteer **toepassen** de wijzigingen op te slaan.

## <a name="send-messages-to-the-iot-hub"></a>Berichten verzenden naar de IoT-Hub

Nu dat de bericht-enrichments zijn geconfigureerd voor het eindpunt, voert u de toepassing Simulated Device berichten te verzenden naar de IoT-Hub. De hub is ingesteld met regels die de volgende taken uitvoeren:

* Berichten die worden doorgestuurd naar het opslageindpunt ContosoStorageEndpointOriginal zal niet worden uitgebreid en worden opgeslagen in de storage-container `original`.

* Berichten die worden doorgestuurd naar het opslageindpunt ContosoStorageEndpointEnriched worden verrijkt en die zijn opgeslagen in de storage-container `enriched`.

De gesimuleerde apparaattoepassing is een van de toepassingen in de uitgepakte download. De toepassing berichten verzendt voor elk van de methoden in voor het doorsturen van berichten de [routering zelfstudie](tutorial-routing.md); dit omvat Azure Storage.

Dubbelklik op het oplossingsbestand (IoT_SimulatedDevice.sln) te openen van de code in Visual Studio en opent u Program.cs. Vervang `{your hub name}` met de naam van de IoT-hub. De indeling van de IoT hub-hostnaam is **{naam van uw hub} .azure-devices.net**. Voor deze zelfstudie is de naam van de host hub **ContosoTestHubMsgEn.azure devices.net**. Vervang vervolgens `{device key}` door de sleutelwaarde u opgeslagen eerder wanneer het script voor het maken van de resources die worden uitgevoerd.

Als u geen sleutel voor het apparaat hebt, kunt u deze ophalen van de portal. Wanneer u zich aanmeldt, gaat u naar **resourcegroepen**, selecteer de resourcegroep en selecteer vervolgens uw IoT-Hub. Kijk onder **IoT-apparaten** voor uw testapparaat en selecteer uw apparaat. Selecteer het kopieerpictogram bij **primaire sleutel** naar het Klembord kopiëren.

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHubMsgEn.azure-devices.net";
        // This is the primary key for the device. This is in the portal.
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key.
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Uitvoeren en testen

Voer de consoletoepassing uit. Wacht enkele minuten. De berichten die worden verzonden, worden weergegeven op het scherm van de console van de toepassing.

Deze app verzendt elke seconde een nieuw apparaat-naar-cloud-bericht naar de IoT Hub. Het bericht bevat een JSON-geserialiseerd object met een apparaat-id, temperatuur, vochtigheid en berichtniveau, die als standaardwaarde `normal` heeft. Wordt een willekeurig toegewezen `critical` of `storage`, zodat het bericht moet worden doorgestuurd naar het storage-account of het standaardeindpunt. De berichten verzonden naar de **verrijkt** container in het opslagaccount dat zal worden uitgebreid.

Na aantal opslag e-mailberichten zijn verzonden, moet u de gegevens bekijken.

1. Selecteer **resourcegroepen**, Ga naar uw resourcegroep (ContosoResourcesMsgEn) en selecteer deze.

2. Selecteer uw opslagaccount (contosostorage). Selecteer vervolgens **Opslagverkenner (preview)** in het selectiedeelvenster aan de linkerkant.

   ![Selecteer Opslagverkenner](./media/tutorial-message-enrichments/select-storage-explorer.png)

   Selecteer **BLOBCONTAINERS** om te zien van de twee containers die kunnen worden gebruikt.

   ![Zie de containers in het storage-account](./media/tutorial-message-enrichments/show-blob-containers.png)

De berichten in de container met de naam **verrijkt** hebben de bericht-enrichments opgenomen in de berichten. De berichten in de container met de naam **oorspronkelijke** heeft de onbewerkte berichten met geen enrichments. Inzoomen op een van de containers totdat u naar de onderkant ophalen en opent u het meest recente berichtenbestand en vervolgens Doe hetzelfde voor de andere container om te controleren of er geen enrichments toegevoegd aan de berichten in die container.

Wanneer u berichten die zijn verrijkt bekijkt, ziet u de "Mijn IoT-Hub' met de naam van de hub, evenals de locatie en de klant-ID, als volgt:

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage","my IoT Hub":"contosotesthubmsgen3276","device location":"$twin.tags.location","customerID":"6ce345b8-1e4a-411e-9398-d34587459a3a"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

En hier niet aangepaste bericht is. "'Mijn IoT-Hub',"Locatie van het apparaat"en"customerID", niet worden weergegeven, omdat dit eindpunt geen enrichments heeft.

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u verwijderen van alle resources die u in deze zelfstudie hebt gemaakt wilt, verwijdert u de resourcegroep. Hiermee verwijdert u ook alle resources in de groep. In dit geval worden de IoT-hub, het opslagaccount en de resourcegroep zelf verwijderd.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>De Azure CLI gebruiken voor het opschonen van resources

U kunt de resourcegroep verwijderen met de opdracht [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete). `$resourceGroup` is ingesteld op **ContosoResources** terug aan het begin van deze zelfstudie.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt geconfigureerd en getest toe te voegen bericht enrichments naar IoT Hub-berichten met behulp van de volgende stappen uit:

**Met behulp van IoT-Hub bericht enrichments**
> [!div class="checklist"]
> * Met de Azure CLI, maakt u de resources--een IoT-hub, een opslagaccount met twee enendpoints en de configuratie van de routering.
> * De Azure portal gebruiken voor het bericht enrichments configureren.
> * Een app die een IoT-apparaat bericht naar de hub simuleert uitvoeren.
> * Bekijk de resultaten en controleer of dat het bericht enrichments werkt zoals verwacht.

Zie voor meer informatie over het bericht enrichments de [overzicht van bericht enrichments](iot-hub-message-enrichments-overview.md).

Zie de volgende artikelen voor meer informatie over het routeren van berichten:

* [Gebruik IoT Hub-berichtroutering apparaat-naar-cloud-berichten te verzenden naar verschillende eindpunten](iot-hub-devguide-messages-d2c.md)

* [Zelfstudie: IoT Hub-routering](tutorial-routing.md)