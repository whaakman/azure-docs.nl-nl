---
title: Berichtroutering configureren met Azure IoT Hub (.NET) | Microsoft Docs
description: Berichtroutering configureren met Azure IoT Hub
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: ab354410ba3b0b37ae630a2b68daec63a9051555
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700822"
---
# <a name="tutorial-configure-message-routing-with-iot-hub"></a>Zelfstudie: berichtroutering configureren met IoT Hub

Met berichtroutering kunt u telemetriegegevens verzenden van uw IoT-apparaten naar ingebouwde, met Event Hub compatibele eindpunten of aangepaste eindpunten zoals blobopslag, Service Bus-wachtrij, Service Bus-onderwerp en Event Hubs. U kunt tijdens het configureren van berichtroutering routeringsregels maken om een route aan te passen die overeenkomt met een bepaalde regel. Zodra u deze hebt ingesteld, worden de inkomende gegevens automatisch door de IoT Hub doorgestuurd naar de eindpunten. 

In deze zelfstudie leert u hoe u routeringsregels kunt instellen en gebruiken met IoT Hub. U gaat berichten doorsturen van een IoT-apparaat naar een of meerdere services, met inbegrip van Blob Storage en Service Bus-wachtrij. Berichten in de Service Bus-wachtrij worden opgehaald door een logische app en via e-mail verzonden. Berichten waarvoor geen specifieke routering is ingesteld, worden naar het standaardeindpunt verzonden en weergegeven in een Power BI-visualisatie.

In deze zelfstudie voert u de volgende taken uit:

> [!div class="checklist"]
> * Stel met Azure CLI of PowerShell de basisresources in: een IoT Hub, een opslagaccount, een Service Bus-wachtrij en een gesimuleerd apparaat.
> * Configureer eindpunten en routes in IoT Hub voor het opslagaccount en Service Bus-wachtrij.
> * Maak een logische app die wordt geactiveerd en een e-mailbericht verzendt wanneer een bericht wordt toegevoegd aan de Service Bus-wachtrij.
> * Download een app die een IoT-apparaat aanzet tot het verzenden van berichten naar de hub voor de verschillende routeringsopties en voer deze uit.
> * Maak een Power BI-visualisatie voor gegevens die naar het standaardeindpunt worden verzonden.
> * Bekijk de resultaten...
> * .. .in de Service Bus-wachtrij en e-mailberichten.
> * ...in het opslagaccount.
> * ...in de Power BI-visualisatie.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

- Installeer [Visual Studio voor Windows](https://www.visualstudio.com/). 

- Een Power BI-account voor het analyseren van de Stream Analytics van het standaardeindpunt. ([Probeer Power BI gratis uit](https://app.powerbi.com/signupredirect?pbi_source=web).)

- Een Office 365-account voor het verzenden van melding via e-mail. 

Voor de instellingsstappen voor deze zelfstudie hebt u Azure CLI of Azure PowerShell nodig. 

Als u Azure CLI wilt gebruiken, is het raadzaam om bij de lokale installatie van Azure CLI de Azure Cloud Shell te gebruiken. Azure Cloud Shell is een gratis interactieve shell waarmee u de scripts van Azure CLI kunt uitvoeren. Er zijn vooraf algemene Azure-hulpprogramma's geïnstalleerd en geconfigureerd in Cloud Shell die u kunt gebruiken bij uw account, zodat u deze niet lokaal hoeft te installeren. 

Als PowerShell wilt gebruiken, moet u deze lokaal installeren via de onderstaande instructies. 

### <a name="azure-cloud-shell"></a>Azure Cloud Shell

U kunt Cloud Shell op verschillende manieren openen:

|  |   |
|-----------------------------------------------|---|
| Selecteer **Nu proberen** in de rechterbovenhoek van een codeblok. | ![Cloud Shell in dit artikel](./media/tutorial-routing/cli-try-it.png) |
| Open Cloud Shell in uw browser. | [![https://shell.azure.com/bash](./media/tutorial-routing/launchcloudshell.png)](https://shell.azure.com) |
| Klik op de knop **Cloud Shell** in het menu in de hoek rechtsboven in de [Azure Portal](https://portal.azure.com). |    ![Cloud Shell in de portal](./media/tutorial-routing/cloud-shell-menu.png) |
|  |  |

### <a name="using-azure-cli-locally"></a>Azure CLI lokaal gebruiken

Als u liever CLI lokaal gebruikt dan de Cloud-Shell, moet u over een Azure CLI-moduleversie 2.0.30.0 of hoger beschikken. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

### <a name="using-powershell-locally"></a>PowerShell lokaal gebruiken

Voor deze zelfstudie is moduleversie 5.7 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="set-up-resources"></a>Resources instellen

Voor deze zelfstudie hebt u een IoT Hub, een opslagaccount en een Service Bus-wachtrij nodig. Deze resources kunnen alle worden gemaakt met Azure CLI of Azure PowerShell. Gebruik dezelfde resourcegroep en -locatie voor alle resources. Vervolgens kunt u alles aan het einde in één stap verwijderen door de resourcegroep te verwijderen.

In de volgende secties wordt beschreven hoe u deze vereiste stappen kunt uitvoeren. Volg de instructies voor CLI *of* PowerShell.

1. Maak een [resourcegroep](../azure-resource-manager/resource-group-overview.md). 

    <!-- When they add the Basic tier, change this to use Basic instead of Standard. -->

2. Maak een IoT Hub in de S1-laag. Voeg een consumentengroep toe aan uw IoT Hub. De consumentengroep wordt gebruikt door de Azure Stream Analytics bij het ophalen van gegevens.

3. Maak een standaard V1-opslagaccount met Standard_LRS replicatie.

4. Maak een Service Bus-naamruimte en -wachtrij. 

5. Maak een apparaat-id voor het gesimuleerde apparaat dat berichten naar uw hub verzendt. Sla de sleutel op voor de testfase.

### <a name="azure-cli-instructions"></a>Instructies voor Azure CLI

De eenvoudigste manier om dit script gebruiken is om het te kopiëren en het in Cloud Shell te plakken. Ervan uitgaande dat u al bent aangemeld, wordt het script in één regel per keer uitgevoerd. 

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device 

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$RANDOM
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, so add a random number to the end.
storageAccountName=contosostorage$RANDOM
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
echo "$storageAccountKey"

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off 

# The Service Bus namespace must be globally unique, so add a random number to the end.
sbNameSpace=ContosoSBNamespace$RANDOM
echo "Service Bus namespace = " $sbNameSpace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNameSpace \
    --location $location
    
# The Service Bus queue name must be globally unique, so add a random number to the end.
sbQueueName=ContosoSBQueue$RANDOM
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNameSpace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

```

### <a name="powershell-instructions"></a>Instructies voor PowerShell

De eenvoudigste manier om dit script gebruiken is om [PowerShell ISE](/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise.md) te openen, het script naar het klembord te kopiëren en het gehele script in de scriptvenster te plakken. U kunt vervolgens de waarden voor de resourcenamen wijzigen (indien gewenst) en het gehele script uitvoeren. 

```azurepowershell-interactive
# Log into Azure account.
Login-AzureRMAccount

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"
$iotDeviceName = "Contoso-Test-Device"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, so add a random number to the end.
$iotHubName = "ContosoTestHub$(Get-Random)"
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzureRmIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzureRmIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random number to the end.
$storageAccountName = "contosostorage$(Get-Random)"
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
$storageContext = $storageAccount.Context 

# Create the container in the storage account.
New-AzureStorageContainer -Name $containerName `
    -Context $storageContext

# The Service Bus namespace must be globally unique,
#   so add a random number to the end.
$serviceBusNamespace = "ContosoSBNamespace$(Get-Random)"
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzureRmServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random number to the end.
$serviceBusQueueName  = "ContosoSBQueue$(Get-Random)"
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzureRmServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName 

```

Maak vervolgens een apparaat-id en sla de bijbehorende sleutel op voor later gebruik. Deze apparaat-id wordt gebruikt door de simulatietoepassing om berichten naar de IoT Hub te verzenden. Deze mogelijkheid is niet beschikbaar in PowerShell, maar u kunt het apparaat in [Azure Portal](https://portal.azure.com) maken.

1. Open [Azure Portal](https://portal.azure.com) en meld u aan bij uw Azure-account.

2. Klik op **Resourcegroepen** en selecteer uw resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**.

3. Klik in de lijst met resources op uw IoT Hub. In deze zelfstudie wordt gebruikgemaakt van **ContosoTestHub**. Selecteer **IoT-apparaten** in het deelvenster Hub.

4. Klik op **+ Toevoegen**. Vul in het deelvenster Apparaat toevoegen de apparaat-id in. In deze zelfstudie wordt gebruikgemaakt van **Contoso-testapparaat**. Laat het item voor sleutels leeg en schakel het selectievakje **Automatisch sleutels genereren** in. Zorg ervoor dat **Apparaat verbinden met IoT Hub** is ingeschakeld. Klik op **Opslaan**.

   ![Schermopname van het scherm Apparaat toevoegen.](./media/tutorial-routing/add-device.png)

5. Klik, nu deze is gemaakt, op het apparaat om de gegenereerde sleutels te zien. Klik op het pictogram Kopiëren voor de Primaire sleutel en sla deze op een locatie op zoals Kladblok voor de testfase van deze zelfstudie.

   ![Schermopname van de details van het apparaat, inclusief de sleutels.](./media/tutorial-routing/device-details.png)

## <a name="set-up-message-routing"></a>Berichtroutering instellen

U gaat berichten naar andere resources doorsturen op basis van eigenschappen die door het gesimuleerde apparaat aan het bericht zijn gekoppeld. Berichten die niet aangepast zijn doorgestuurd, worden verzonden naar het standaardeindpunt (berichten/gebeurtenissen). 

|waarde |Resultaat|
|------|------|
|niveau = 'opslag' |Schrijf naar Azure Storage.|
|niveau = 'kritiek' |Schrijf naar een Service Bus-wachtrij. Een logische app haalt het bericht op uit de wachtrij en gebruikt Office 365 om het bericht via e-mail te versturen.|
|standaardinstelling |Geef deze gegevens weer met Power BI.|

### <a name="routing-to-a-storage-account"></a>Doorsturen naar een opslagaccount 

Stel nu de routering in voor het opslagaccount. Definieer een eindpunt en stel vervolgens een route in voor dat eindpunt. Berichten waarvan de eigenschap **Niveau** is ingesteld op **Opslag** worden automatisch naar een opslagaccount geschreven.

1. Klik in [Azure Portal](https://portal.azure.com) op **Resourcegroepen** en selecteer vervolgens uw resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**. Klik op de IoT Hub in de lijst met resources. In deze zelfstudie wordt gebruikgemaakt van **ContosoTestHub**. Klik op **Eindpunten**. Klik in het deelvenster **Eindpunten** op **+ Toevoegen**. Voer de volgende informatie in:

   **Naam**: voer een naam in voor het eindpunt. In deze zelfstudie wordt gebruikgemaakt van **StorageContainer**.
   
   **Eindpunttype**: selecteer **Azure Storage Container** in de vervolgkeuzelijst.

   Klik op **Een container kiezen** om de lijst met opslagaccounts te bekijken. Selecteer uw opslagaccount. In deze zelfstudie wordt gebruikgemaakt van **contosostorage**. Selecteer daarna de container. In deze zelfstudie wordt gebruikgemaakt van **contosoresults**. Klik op **Selecteren** om terug te keren naar het deelvenster **Eindpunt toevoegen**. 
   
   ![Schermopname waarin het toevoegen van een eindpunt wordt weergegeven.](./media/tutorial-routing/add-endpoint-storage-account.png)
   
   Klik op **OK** om het toevoegen van een eindpunt te voltooien.
   
2. Klik op **Routes** op uw IoT Hub. U gaat een routeringsregel maken waarmee berichten worden doorgestuurd naar de opslagcontainer die u zojuist hebt toegevoegd als eindpunt. Klik op **+ Toevoegen** bovenaan het deelvenster Routes. Vul de velden in op het scherm. 

   **Naam**: voer een naam in voor uw routeringsregel. In deze zelfstudie wordt gebruikgemaakt van **StorageRule**.

   **Gegevensbron**: selecteer **Apparaatberichten** uit de vervolgkeuzelijst.

   **Eindpunt**: selecteer het eindpunt dat u net hebt ingesteld. In deze zelfstudie wordt gebruikgemaakt van **StorageContainer**. 
   
   **Querytekenreeks**: voer `level="storage"` in als querytekenreeks. 

   ![Schermopname waarin wordt weergegeven hoe een routeringsregel wordt gemaakt voor het opslagaccount.](./media/tutorial-routing/create-a-new-routing-rule-storage.png)
   
   Klik op **Opslaan**. Wanneer dit is voltooid, gaat u terug naar het deelvenster Routes, waar u uw nieuwe routeringsregel voor opslag kunt zien. Sluit het deelvenster Routes, waarna u terugkeert naar de pagina Resourcegroep.

### <a name="routing-to-a-service-bus-queue"></a>Routering naar een Service Bus-wachtrij 

Stel nu de routering in voor de Service Bus-wachtrij. Definieer een eindpunt en stel vervolgens een route in voor dat eindpunt. Berichten waarvan de eigenschap **Niveau** is ingesteld op **Kritiek** worden naar de Service Bus-wachtrij geschreven, die een logische app activeert waarmee vervolgens een e-mailbericht met de informatie wordt verzonden. 

1. Klik op de pagina Resourcegroep op uw IoT Hub en klik vervolgens op **Eindpunten**. Klik in het deelvenster **Eindpunten** op **+ Toevoegen**. Voer de volgende informatie in.

   **Naam**: voer een naam in voor het eindpunt. In deze zelfstudie wordt gebruikgemaakt van **CriticalQueue**. 

   **Eindpunttype**: selecteer **Service Bus-wachtrij** uit de vervolgkeuzelijst.

   **Service Bus-naamruimte**: selecteer de Service Bus-naamruimte voor deze zelfstudie in de vervolgkeuzelijst. In deze zelfstudie wordt gebruikgemaakt van **ContosoSBNamespace**.

   **Service Bus-wachtrij**: selecteer Service Bus-wachtrij uit de vervolgkeuzelijst. In deze zelfstudie wordt gebruikgemaakt van **contososbqueue**.

   ![Schermopname waarin het toevoegen van een eindpunt voor de Service Bus-wachtrij wordt weergegeven.](./media/tutorial-routing/add-endpoint-sb-queue.png)

   Klik op **OK** om het eindpunt op te slaan. Nadat deze voltooid, sluit u het deelvenster Eindpunten. 
    
2. Klik op **Routes** op uw IoT Hub. U gaat een routeringsregel maken waarmee berichten worden doorgestuurd naar de Service Bus-wachtrij die u zojuist hebt toegevoegd als eindpunt. Klik op **+ Toevoegen** bovenaan het deelvenster Routes. Vul de velden in op het scherm. 

   **Naam**: voer een naam in voor uw routeringsregel. In deze zelfstudie wordt gebruikgemaakt van **SBQueueRule**. 

   **Gegevensbron**: selecteer **Apparaatberichten** uit de vervolgkeuzelijst.

   **Eindpunt**: selecteer het eindpunt dat u net hebt ingesteld, **CriticalQueue**.

   **Querytekenreeks**: voer `level="critical"` in als querytekenreeks. 

   ![Schermopname waarin wordt weergegeven hoe een routeringsregel wordt gemaakt voor de Service Bus-wachtrij.](./media/tutorial-routing/create-a-new-routing-rule-sbqueue.png)
   
   Klik op **Opslaan**. Wanneer u naar het deelvenster Routes teruggaat, ziet u allebei uw nieuwe routeringsregels zoals deze hier zijn weergegeven.

   ![Schermopname waarin de routes zijn weergegeven die u zojuist hebt ingesteld.](./media/tutorial-routing/show-routing-rules-for-hub.png)

   Sluit het deelvenster Routes, waarna u terugkeert naar de pagina Resourcegroep.

## <a name="create-a-logic-app"></a>Een logische app maken  

De Service Bus-wachtrij moet worden gebruikt voor het ontvangen van berichten die zijn aangeduid als kritiek. Installeer een logische app die wordt geactiveerd om de Service Bus-wachtrij te bewaken en een e-mailbericht te verzenden wanneer een bericht wordt toegevoegd aan de wachtrij. 

1. Klik in de [Azure Portal](https://portal.azure.com) op **+ Een resource maken**. Typ **logische app** in het zoekvak en klik op Enter. Selecteer Logische app in de weergegeven zoekresultaten, klik dan op **Maken** om door te gaan naar het deelvenster **Logische app maken**. Vul de velden in. 

   **Naam**: dit veld is de naam van de logische app. In deze zelfstudie wordt gebruikgemaakt van **ContosoLogicApp**. 

   **Abonnement**: selecteer uw Azure-abonnement.

   **Resourcegroep**: klik op **Bestaande gebruiken** en selecteer uw resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**. 

   **Locatie**: gebruik uw locatie. In deze zelfstudie wordt gebruikgemaakt van **VS-West**. 

   **Log Analytics**: deze wisselknop moet worden uitgeschakeld. 

   ![Schermopname van het scherm Logische app maken.](./media/tutorial-routing/create-logic-app.png)

   Klik op **Create**.

4. Ga nu naar de logische app. De eenvoudigste manier om naar de logische app te gaan is door te klikken op **Resourcegroepen**, uw resourcegroep te selecteren (In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**) en vervolgens de logische app in de lijst met resources te selecteren. De pagina Ontwerpfunctie voor logic apps wordt weergegeven (misschien moet u naar rechts schuiven om de volledige pagina te zien). Schuif op de pagina Ontwerpfunctie voor logic apps omlaag totdat u de tegel waarin **Lege logische app +** ziet, en klik erop. 

5. Er wordt een lijst met connectors weergegeven. Selecteer **Service Bus**. 

   ![Schermafbeelding van de lijst met connectors.](./media/tutorial-routing/logic-app-connectors.png)

6. Er wordt een lijst met triggers weergegeven. Selecteer **Service Bus: wanneer een bericht wordt ontvangen in een wachtrij (automatisch voltooien)**. 

   ![Schermopname van de lijst met triggers voor de Service Bus.](./media/tutorial-routing/logic-app-triggers.png)

6. Vul op het volgende scherm de naam van de verbinding in. In deze zelfstudie wordt gebruikgemaakt van **ContosoConnection**. 

   ![Schermopname waarin het instellen van de verbinding van de Service Bus-wachtrij wordt weergegeven.](./media/tutorial-routing/logic-app-define-connection.png)

   Klik op Service Bus-naamruimte. In deze zelfstudie wordt gebruikgemaakt van **ContosoSBNamespace**. Wanneer u de naamruimte selecteert, vraagt de portal de Service Bus-naamruimte op voor het ophalen van de sleutels. Selecteer **RootManageSharedAccessKey** en klik op **Maken**. 
   
   ![Schermopname waarin de voltooiing van het instellen van de verbinding wordt weergegeven.](./media/tutorial-routing/logic-app-finish-connection.png)

7. Selecteer de naam van de wachtrij op het volgende scherm (in deze zelfstudie wordt gebruikgemaakt van **contososbqueue**) in de vervolgkeuzelijst. U kunt de standaardwaarden gebruiken voor de rest van de velden. 

   ![Schermopname waarin de wachtrijopties worden weergegeven.](./media/tutorial-routing/logic-app-queue-options.png)

7. Stel nu de actie in voor het verzenden van het e-mailbericht wanneer een bericht wordt ontvangen in de wachtrij. Klik in de ontwerpfunctie voor logische apps op **+ Nieuwe stap** om een stap toe te voegen en klik vervolgens op **Actie toevoegen**. Zoek in het deelvenster **Een actie kiezen** de optie **Office 365 Outlook** en klik erop. Selecteer op het scherm Triggers de optie **Office 365 Outlook - Een e-mail verzenden**.  

   ![Schermopname waarin de Office365-opties worden weergegeven.](./media/tutorial-routing/logic-app-select-outlook.png)

8. Meld u vervolgens aan bij uw Office 365-account voor het instellen van de verbinding. Geef de e-mailadressen op voor de ontvanger(s) van de e-mailberichten. Geef tevens het onderwerp op en het type berichten dat u wilt dat de ontvanger in de hoofdtekst zal zien. Vul voor het testen uw eigen e-mailadres in als ontvanger.

   Klik op **Dynamische inhoud toevoegen** om de inhoud weer te geven van het bericht dat u kunt opnemen. Selecteer **Inhoud**: hiermee wordt het bericht in het e-mailbericht opgenomen. 

   ![Schermopname van de e-mailopties voor de logische app.](./media/tutorial-routing/logic-app-send-email.png)

9. Klik op **Opslaan**. Sluit daarna de ontwerpfunctie voor de logische app af.

## <a name="set-up-azure-stream-analytics"></a>Azure Stream Analytics installeren

Als u de gegevens in een Power BI-visualisatie wilt zien, stelt u eerst een Stream Analytics-taak in om de gegevens op te halen. Houd er rekening mee dat alleen de berichten waarin het **niveau** **normaal** is, worden verzonden naar het standaardeindpunt en door de Stream Analytics-taak voor de Power BI-visualisatie worden opgehaald.

### <a name="create-the-stream-analytics-job"></a>De Stream Analytics-taak maken

1. Klik [in de Azure Portal](https://portal.azure.com) op **Een resource maken** > **Internet of Things** > **Stream Analytics-taak**.

2. Voer de volgende informatie in voor de taak.

   **Taaknaam**: de naam van de taak. De naam moet wereldwijd uniek zijn. In deze zelfstudie wordt gebruikgemaakt van **contosoJob**.

   **Resourcegroep**: gebruik dezelfde resourcegroep die wordt gebruikt door uw IoT Hub. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**. 

   **Locatie**: gebruik dezelfde locatie die u in het instellingsscript hebt gebruikt. In deze zelfstudie wordt gebruikgemaakt van **VS-West**. 

   ![Schermopname waarin wordt weergegeven hoe u de Stream analytics-taak kunt maken.](./media/tutorial-routing/stream-analytics-create-job.png)

3. Klik op **Maken** om de taak te maken. Als u wilt teruggaan naar de taak, klikt u op **Resourcegroepen**. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**. Selecteer de resourcegroep en klik op de Stream Analytics-taak in de lijst met resources. 

### <a name="add-an-input-to-the-stream-analytics-job"></a>Een invoer aan de Stream Analytics-taak toevoegen

1. Klik onder **Taaktopologie** op **Invoer**.

2. Klik in het deelvenster **Invoer** op **Stroominvoer toevoegen** en selecteer IoT Hub. Vul de volgende velden in op het scherm dat wordt weergegeven:

   **Invoeralias**: in deze zelfstudie wordt gebruikgemaakt **contosoinputs**.

   **Abonnement**: selecteer uw abonnement.

   **IoT Hub**: selecteer de IoT Hub. In deze zelfstudie wordt gebruikgemaakt van **ContosoTestHub**.

   **Eindpunt**: selecteer **Berichten**. (Als u Operations Monitoring selecteert, krijgt u de telemetriegegevens over de IoT Hub in plaats van de gegevens die u doorstuurt.) 

   **Naam van beleid voor gedeelde toegang**: selecteer **iothubowner**. In de portal wordt de sleutel van het beleid voor gedeelde toegang voor u ingevuld.

   **Consumentengroep**: selecteer de consumentengroep die u eerder hebt gemaakt. In deze zelfstudie wordt gebruikgemaakt van **contosoconsumers**.
   
   Accepteer de standaardwaarden voor de rest van de velden. 

   ![Schermopname waarin wordt weergegeven hoe u de invoerwaarden voor de Stream analytics-taak kunt instellen.](./media/tutorial-routing/stream-analytics-job-inputs.png)

5. Klik op **Opslaan**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Een uitvoer aan de Stream Analytics-taak toevoegen

1. Klik onder **Taaktopologie** op **Uitvoer**.

2. Klik in het deelvenster **Uitvoer** op **Toevoegen** en selecteer **Power BI**. Vul de volgende velden in op het scherm dat wordt weergegeven:

   **Uitvoeralias**: de alias die uniek is voor de uitvoer. In deze zelfstudie wordt gebruikgemaakt van **contosooutputs**. 

   **Naam van de gegevensset**: de naam van de gegevensset die moet worden gebruikt in Power BI. In deze zelfstudie wordt gebruikgemaakt van **contosodataset**. 

   **Tabelnaam**: de naam van de tabel die moet worden gebruikt in Power BI. In deze zelfstudie wordt gebruikgemaakt van **contosotable**.

   Accepteer de standaardwaarden voor de rest van de velden.

3. Klik op **Autoriseren** en meld u aan bij uw Power BI-account.

   ![Schermopname waarin wordt weergegeven hoe u de uitvoerwaarden voor de Stream analytics-taak kunt instellen.](./media/tutorial-routing/stream-analytics-job-outputs.png)

4. Klik op **Opslaan**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>De query van de Stream Analytics-taak configureren

1. Klik onder **Taaktopologie** op **Query**.

2. Vervang `[YourInputAlias]` door de invoeralias van de taak. In deze zelfstudie wordt gebruikgemaakt van **contosoinputs**.

3. Vervang `[YourOutputAlias]` door de uitvoeralias van de taak. In deze zelfstudie wordt gebruikgemaakt van **contosooutputs**.

   ![Schermopname waarin wordt weergegeven hoe u de query voor de Stream analytics-taak kunt instellen.](./media/tutorial-routing/stream-analytics-job-query.png)

4. Klik op **Opslaan**.

5. Sluit het deelvenster Query. Hiermee keert u terug naar de weergave van de resources in de resourcegroep. Klik op de Stream Analytics-taak. In deze zelfstudie heet deze taak **contosoJob**.

### <a name="run-the-stream-analytics-job"></a>Voer de Stream Analytics-taak uit

Klik in de Stream Analytics-taak op **Start** > **Nu** > **Start**. Zodra de taak kan worden gestart, wordt de taakstatus veranderd van **Gestopt** naar **In uitvoering**.

Als u het Power BI-rapport instelt, hebt u gegevens nodig, dus stelt u Power BI in nadat u het apparaat hebt gemaakt en de simulatietoepassing van het apparaat hebt ingesteld.

## <a name="run-simulated-device-app"></a>De app Gesimuleerd apparaat uitvoeren

Eerder in het scriptinstellingsgedeelte stelt u een apparaat in voor de simulatie via een IoT-apparaat. In deze sectie downloadt u een .NET-consoletoepassing die een apparaat simuleert dat apparaat-naar-cloud-berichten naar een IoT Hub verzendt. Deze toepassing verzendt berichten voor elk van de verschillende routeringsmethoden. 

Download de oplossing voor de [IoT-apparaatsimulatie](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Hiermee downloadt u een opslagplaats die meerdere toepassingen bevat. De oplossing die u zoekt bevindt zich in iot-hub/Tutorials/Routing/SimulatedDevice/.

Dubbelklik op het oplossingsbestand (SimulatedDevice.sln om de code in Visual Studio) te openen en open vervolgens Program.cs. Vervang `{iot hub hostname}` door de hostnaam van de IoT Hub. De indeling van de hostnaam van de IoT Hub is **{iot-hub-name} .azure-devices.net**. Voor deze zelfstudie is de naam van de hubhost **ContosoTestHub.azure devices.net**. Vervang vervolgens `{device key}` door de apparaatsleutel die u eerder hebt opgeslagen bij het instellen van het gesimuleerde apparaat. 

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Uitvoeren en testen 

Voer de consoletoepassing uit. Wacht enkele minuten. U kunt de berichten zien die op het scherm van de console van de toepassing worden verzonden.

Deze app verzendt elke seconde een nieuw apparaat-naar-cloud-bericht naar de IoT Hub. Het bericht bevat een JSON-geserialiseerd object met een apparaat-id, temperatuur, vochtigheid en berichtniveau, die als standaardwaarde `normal` heeft. Hiermee wordt een willekeurig niveau `critical` of `storage` toegewezen, zodat het bericht wordt doorgestuurd naar het opslagaccount of naar de Service Bus-wachtrij (die uw logische app aanzet tot het verzenden van een e-mailbericht). De standaardmeetwaarden (`normal`) worden weergegeven in het BI-rapport dat u daarna instelt.

Als alles juist is ingesteld, zou u op dit moment de volgende resultaten moeten zien:

1. U gaat e-mailberichten ontvangen over kritieke berichten. 

   ![Schermopname van de resulterende e-mailberichten.](./media/tutorial-routing/results-in-email.png)

   Dit betekent het volgende:

   * De routering naar de Service Bus-wachtrij werkt correct.
   * De logische app die het bericht uit de Service Bus-wachtrij ophaalt, werkt correct.
   * De connector van de logische app voor Outlook werkt goed. 

2. Klik in de [Azure Portal](https://portal.azure.com) op **Resourcegroepen** en selecteer uw resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**. Selecteer het opslagaccount, klik op **Blobs** en selecteer de container. In deze zelfstudie wordt gebruikgemaakt van **contosoresults**. U zou een map moeten zien, en u kunt inzoomen via de mappen totdat u een of meer bestanden ziet. Open een van deze bestanden; ze bevatten de vermeldingen die zijn doorgestuurd naar het opslagaccount. 

   ![Schermopname van de bestanden met resultaten in de opslag.](./media/tutorial-routing/results-in-storage.png)

Dit betekent het volgende:

   * De routering naar de Service Bus-wachtrij werkt correct.

Stel nu de Power BI-visualisatie in terwijl de toepassing nog steeds wordt uitgevoerd om de berichten te bekijken die afkomstig zijn van de standaardroutering. 

## <a name="set-up-the-power-bi-visualizations"></a>De Power BI-visualisaties instellen

1. Meld u aan bij uw [Power BI](https://powerbi.microsoft.com/)-account.

2. Ga naar **Werkruimten** en selecteer de werkruimte die u hebt ingesteld toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt. In deze zelfstudie wordt gebruikgemaakt van **Mijn werkruimte**. 

3. Klik op **Gegevenssets**.

   U zou de vermelde gegevensset moeten zien die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt. In deze zelfstudie wordt gebruikgemaakt van **contosodataset**. (Het kan 5-10 minuten duren voordat de gegevensset voor de eerste keer wordt weergegeven.)

4. Klik onder **ACTION** op het eerste pictogram om een rapport te maken.

   ![Schermopname van Power BI-werkruimte waarbij het pictogram Acties en Rapport zijn gemarkeerd.](./media/tutorial-routing/power-bi-actions.png)

5. Maak een lijndiagram om in realtime de temperatuur gedurende een bepaalde periode weer te geven.

   a. Voeg op de pagina voor rapporten maken een lijndiagram toe door te klikken op het lijndiagrampictogram.

   ![Schermopname van de visualisaties en velden.](./media/tutorial-routing/power-bi-visualizations-and-fields.png)

   b. Klap in het deelvenster **Velden** de tabel uit die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt. In deze zelfstudie wordt gebruikgemaakt van **contosotable**.

   c. Sleep **EventEnqueuedUtcTime** naar **As** in het deelvenster **Visualisaties**.

   d. Sleep **Temperatuur** naar **Waarden**.

   Er wordt een lijndiagram gemaakt. De x-as geeft de datum en tijd in UTC-tijdzone aan. De y-as geeft de temperatuur van de sensor aan.

7. Maak een ander lijndiagram om in realtime de vochtigheid gedurende een bepaalde periode weer te geven. Volg, als u het tweede diagram wilt instellen, dezelfde stappen als hierboven en plaats **EventEnqueuedUtcTime** op de x-as en **Vochtigheid** op de y-as.

   ![Schermopname van het laatste Power BI-rapport met de twee diagrammen.](./media/tutorial-routing/power-bi-report.png)

8. Klik op **Opslaan** om het rapport op te slaan.

U zou gegevens moeten kunnen zien in beide diagrammen. Dit betekent het volgende:

   * De routering naar het standaardeindpunt werkt correct.
   * De Azure Stream Analytics-taak stroomt correct.
   * De Power BI-visualisatie is juist ingesteld.

U kunt de diagrammen vernieuwen om de meest recente gegevens te bekijken door te klikken op de knop Vernieuwen bovenaan het Power BI-venster. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u alle resources die u hebt gemaakt, wilt verwijderen, verwijdert u de resourcegroep. Hiermee verwijdert u ook alle resources in de groep. In dit geval worden de IoT Hub, de Service Bus-naamruimte en -wachtrij, de logische app, het opslagaccount en de resourcegroep zelf verwijderd. 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Resources in de Power BI-visualisatie opschonen

Meld u aan bij uw [Power BI](https://powerbi.microsoft.com/)-account. Ga naar uw werkruimte. In deze zelfstudie wordt gebruikgemaakt van **Mijn werkruimte**. Als u de Power BI-visualisatie wilt verwijderen, gaat u naar Gegevenssets en klikt u op het prullenbakpictogram om de gegevensset te verwijderen. In deze zelfstudie wordt gebruikgemaakt van **contosodataset**. Wanneer u de gegevensset verwijdert, wordt het rapport ook verwijderd.

### <a name="clean-up-resources-using-azure-cli"></a>Resources opschonen met Azure CLI

U kunt de resourcegroep verwijderen met de opdracht [az group delete](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-delete).

```azurecli-interactive
az group delete --name $resourceGroup
```
### <a name="clean-up-resources-using-powershell"></a>Resources opschonen met PowerShell

U kunt de resourcegroep verwijderen met de opdracht [Remove-AzureRmResourceGroup](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup). $resourceGroup is helemaal aan het begin van deze zelfstudie ingesteld op **ContosoIoTRG1**.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $resourceGroup
```


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u met berichtroutering IoT Hub-berichten naar verschillende bestemmingen doorstuurt door de volgende taken uit te voeren.  

> [!div class="checklist"]
> * Stel met Azure CLI of PowerShell de basisresources in: een IoT Hub, een opslagaccount, een Service Bus-wachtrij en een gesimuleerd apparaat.
> * Configureer eindpunten en routes in IoT Hub voor het opslagaccount en Service Bus-wachtrij.
> * Maak een logische app die wordt geactiveerd en een e-mailbericht verzendt wanneer een bericht wordt toegevoegd aan de Service Bus-wachtrij.
> * Download een app die een IoT-apparaat aanzet tot het verzenden van berichten naar de hub voor de verschillende routeringsopties en voer deze uit.
> * Maak een Power BI-visualisatie voor gegevens die naar het standaardeindpunt worden verzonden.
> * Bekijk de resultaten...
> * .. .in de Service Bus-wachtrij en e-mailberichten.
> * ...in het opslagaccount.
> * ...in de Power BI-visualisatie.

Ga door naar de volgende zelfstudie voor informatie over het beheren van de toestand van een IoT-apparaat. 

> [!div class="nextstepaction"]
[Uw apparaten configureren vanaf een back-endservice](tutorial-device-twins.md)

 <!--  [Manage the state of a device](./tutorial-manage-state.md) -->