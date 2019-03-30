---
title: Weergave Azure IoT Hub bericht routeberekeningen (.NET) | Microsoft Docs
description: Azure IoT Hub bericht routering resultaten weergeven
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 7f6baec8fee2b046949e2d88b5fff7bb5db9b40b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662411"
---
# <a name="tutorial-part-2---view-the-routed-messages"></a>Zelfstudie: Deel 2: de gerouteerde berichten weergeven

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

## <a name="rules-for-routing-the-messages"></a>Regels voor het routeren van de berichten

Dit zijn de regels voor de routering van berichten; Deze zijn ingesteld in deel 1 van deze zelfstudie, zodat u ze werken in dit tweede deel.

|waarde |Resultaat|
|------|------|
|niveau = 'opslag' |Schrijf naar Azure Storage.|
|niveau = 'kritiek' |Schrijf naar een Service Bus-wachtrij. Een logische app haalt het bericht op uit de wachtrij en gebruikt Office 365 om het bericht via e-mail te versturen.|
|standaardinstelling |Geef deze gegevens weer met Power BI.|

Nu u de resources waarop de berichten die worden doorgestuurd, een app om berichten te verzenden naar de hub, uitvoeren en de routering in actie zien.

## <a name="create-a-logic-app"></a>Een logische app maken  

De Service Bus-wachtrij moet worden gebruikt voor het ontvangen van berichten die zijn aangeduid als kritiek. Installeer een logische app die wordt geactiveerd om de Service Bus-wachtrij te bewaken en een e-mailbericht te verzenden wanneer een bericht wordt toegevoegd aan de wachtrij.

1. In de [Azure-portal](https://portal.azure.com), selecteer **+ een resource maken**. Typ **logische app** in het zoekvak en klik op Enter. In de zoekresultaten weergegeven, selecteert u logische App en vervolgens **maken** om door te gaan naar de **logische app maken** deelvenster. Vul de velden in.

   **Naam**: Dit veld is de naam van de logische app. In deze zelfstudie wordt gebruikgemaakt van **ContosoLogicApp**.

   **Abonnement**: Selecteer uw Azure-abonnement.

   **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer de resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**.

   **Locatie**: Gebruik uw locatie. In deze zelfstudie wordt gebruikgemaakt van **US - west**.

   **Log Analytics**: Deze wisselknop moet worden uitgeschakeld.

   ![Het scherm van de logische App maken](./media/tutorial-routing-view-message-routing-results/create-logic-app.png)

   Selecteer **Maken**.

2. Ga nu naar de logische app. De eenvoudigste manier om aan de logische App is om te selecteren **resourcegroepen**, selecteer de resourcegroep (deze zelfstudie wordt gebruikgemaakt van **ContosoResources**), selecteer vervolgens de logische App in de lijst met resources. De pagina Ontwerpfunctie voor logic apps wordt weergegeven (misschien moet u naar rechts schuiven om de volledige pagina te zien). Op de pagina van de ontwerper van logische Apps, schuif omlaag totdat ziet u de tegel met de melding dat **lege logische App +** en selecteer deze. Het standaardtabblad is 'Voor u'. Als dit deelvenster leeg is, selecteert u **alle** om te zien van alle connectors en triggers beschikbaar.

3. Selecteer **Service Bus** in de lijst van connectors.

   ![De lijst van connectors](./media/tutorial-routing-view-message-routing-results/logic-app-connectors.png)

4. Er wordt een lijst met triggers weergegeven. Selecteer **wanneer een bericht wordt ontvangen in een wachtrij (automatisch voltooien) / Service Bus**.

   ![De lijst met triggers voor Service Bus](./media/tutorial-routing-view-message-routing-results/logic-app-triggers.png)

5. Vul op het volgende scherm de naam van de verbinding in. In deze zelfstudie wordt gebruikgemaakt van **ContosoConnection**.

   ![Instellen van de verbinding voor de Service Bus-wachtrij](./media/tutorial-routing-view-message-routing-results/logic-app-define-connection.png)

   Selecteer de Service Bus-naamruimte. In deze zelfstudie wordt gebruikgemaakt van **ContosoSBNamespace**. Wanneer u de naamruimte selecteert, vraagt de portal de Service Bus-naamruimte op voor het ophalen van de sleutels. Selecteer **RootManageSharedAccessKey** en selecteer **maken**.

   ![Instellen van de verbinding is voltooid](./media/tutorial-routing-view-message-routing-results/logic-app-finish-connection.png)

6. Selecteer de naam van de wachtrij op het volgende scherm (in deze zelfstudie wordt gebruikgemaakt van **contososbqueue**) in de vervolgkeuzelijst. U kunt de standaardwaarden gebruiken voor de rest van de velden.

   ![De wachtrij-opties](./media/tutorial-routing-view-message-routing-results/logic-app-queue-options.png)

7. Stel nu de actie in voor het verzenden van het e-mailbericht wanneer een bericht wordt ontvangen in de wachtrij. Selecteer in de ontwerper van logische Apps **+ nieuwe stap** om toe te voegen een stap, selecteert u vervolgens **alle** om alle van de beschikbare opties te zien. In de **een actie kiezen** deelvenster, zoek en selecteer **Office 365 Outlook**. Selecteer op het scherm triggers **e-mailbericht verzenden / Office 365 Outlook**.  

   ![De Office 365-opties](./media/tutorial-routing-view-message-routing-results/logic-app-select-outlook.png)

8. Meld u aan uw Office 365-account voor het instellen van de verbinding. Als dit een optreedt time-out, alleen probeer het opnieuw. Geef de e-mailadressen op voor de ontvanger(s) van de e-mailberichten. Geef tevens het onderwerp op en het type berichten dat u wilt dat de ontvanger in de hoofdtekst zal zien. Vul voor het testen uw eigen e-mailadres in als ontvanger.

   Selecteer **dynamische inhoud toevoegen** om weer te geven van de inhoud van het bericht dat u kunt opnemen. Selecteer **Inhoud**: hiermee wordt het bericht in het e-mailbericht opgenomen.

   ![De e-opties voor de logische app](./media/tutorial-routing-view-message-routing-results/logic-app-send-email.png)

9. Selecteer **Opslaan**. Sluit daarna de ontwerpfunctie voor de logische app af.

## <a name="set-up-azure-stream-analytics"></a>Azure Stream Analytics instellen

Als u de gegevens in een Power BI-visualisatie wilt zien, stelt u eerst een Stream Analytics-taak in om de gegevens op te halen. Houd er rekening mee dat alleen de berichten waarin het **niveau** **normaal** is, worden verzonden naar het standaardeindpunt en door de Stream Analytics-taak voor de Power BI-visualisatie worden opgehaald.

### <a name="create-the-stream-analytics-job"></a>De Stream Analytics-taak maken

1. In de [Azure-portal](https://portal.azure.com), selecteer **een resource maken** > **Internet of Things** > **Stream Analytics-taak**.

2. Voer de volgende informatie in voor de taak.

   **Taaknaam**: De naam van de taak. De naam moet wereldwijd uniek zijn. In deze zelfstudie wordt gebruikgemaakt van **contosoJob**.

   **Abonnement**: De Azure-abonnement die u voor de zelfstudie gebruikt.

   **Resourcegroep**: Gebruik dezelfde resourcegroep die wordt gebruikt door uw IoT Hub. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**.

   **Locatie**: Gebruik dezelfde locatie die u in het instellingsscript hebt gebruikt. In deze zelfstudie wordt gebruikgemaakt van **US - west**.

   ![De stream analytics-taak maken](./media/tutorial-routing-view-message-routing-results/stream-analytics-create-job.png)

3. Selecteer **maken** om de taak te maken. Als u wilt teruggaan naar het project, selecteer **resourcegroepen**. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**. Selecteer de resourcegroep en selecteer vervolgens de Stream Analytics-taak in de lijst met resources.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Een invoer aan de Stream Analytics-taak toevoegen

4. Onder **Taaktopologie**, selecteer **invoer**.

5. In de **invoer** venster **Stroominvoer toevoegen** en IoT-Hub selecteren. Vul de volgende velden in op het scherm dat wordt weergegeven:

   **Invoeralias**: In deze zelfstudie wordt gebruikgemaakt van **contosoinputs**.

   **IoT Hub selecteren bij uw abonnement**: Selecteer dit keuzerondje.

   **Abonnement**: Selecteer het Azure-abonnement u voor deze zelfstudie.

   **IoT Hub**: Selecteer de IoT Hub. In deze zelfstudie wordt gebruikgemaakt van **ContosoTestHub**.

   **Eindpunt**: Selecteer **Berichten**. (Als u Operations Monitoring selecteert, krijgt u de telemetriegegevens over de IoT Hub in plaats van de gegevens die u doorstuurt.) 

   **Naam van het gedeelde toegangsbeleid**: Selecteer **iothubowner**. In de portal wordt de sleutel van het beleid voor gedeelde toegang voor u ingevuld.

   **Consumentengroep**: Selecteer de consumergroep instellen in stap 1 van deze zelfstudie. In deze zelfstudie wordt gebruikgemaakt van **contosoconsumers**.
   
   Accepteer de standaardwaarden voor de rest van de velden. 

   ![Instellen van de invoer voor de stream analytics-taak](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-inputs.png)

6. Selecteer **Opslaan**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Een uitvoer aan de Stream Analytics-taak toevoegen

1. Onder **Taaktopologie**, selecteer **uitvoer**.

2. In de **uitvoer** venster **toevoegen**, en selecteer vervolgens **Power BI**. Vul de volgende velden in op het scherm dat wordt weergegeven:

   **Uitvoeralias**: De alias die uniek is voor de uitvoer. In deze zelfstudie wordt gebruikgemaakt van **contosooutputs**. 

   **Naam van gegevensset**: De naam van de gegevensset die moet worden gebruikt in Power BI. In deze zelfstudie wordt gebruikgemaakt van **contosodataset**. 

   **Tabelnaam**: De naam van de tabel die moet worden gebruikt in Power BI. In deze zelfstudie wordt gebruikgemaakt van **contosotable**.

   Accepteer de standaardwaarden voor de rest van de velden.

3. Selecteer **autoriseren**, en aanmelden bij uw Power BI-account. (Dit kan meer dan één keer duren).

   ![Instellen van de uitvoer voor de stream analytics-taak](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-outputs.png)

4. Selecteer **Opslaan**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>De query van de Stream Analytics-taak configureren

1. Selecteer **Query** onder **Taaktopologie**.

2. Vervang `[YourInputAlias]` door de invoeralias van de taak. In deze zelfstudie wordt gebruikgemaakt van **contosoinputs**.

3. Vervang `[YourOutputAlias]` door de uitvoeralias van de taak. In deze zelfstudie wordt gebruikgemaakt van **contosooutputs**.

   ![Instellen van de query voor de stream analytics-taak](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-query.png)

4. Selecteer **Opslaan**.

5. Sluit het deelvenster Query. U terugkeren naar de weergave van de resources in de resourcegroep. Selecteer de Stream Analytics-taak. In deze zelfstudie heet deze taak **contosoJob**.

### <a name="run-the-stream-analytics-job"></a>De Stream Analytics-taak uitvoeren

Selecteer in de Stream Analytics-taak, **Start** > **nu** > **Start**. Zodra de taak kan worden gestart, wordt de taakstatus veranderd van **Gestopt** naar **In uitvoering**.

Als u het Power BI-rapport instelt, hebt u gegevens nodig, dus stelt u Power BI in nadat u het apparaat hebt gemaakt en de simulatietoepassing van het apparaat hebt ingesteld.

## <a name="run-simulated-device-app"></a>Gesimuleerde apparaat-app uitvoeren

In het deel 1 van deze zelfstudie stelt u een apparaat simuleren met behulp van een IoT-apparaat. In deze sectie downloadt u de .NET-consoletoepassing die apparaat-naar-cloud-berichten verzenden naar een IoT-hub, apparaat simuleert ervan uitgaande dat u niet hebt al de app en de resources in deel 1 van deze zelfstudie downloaden.

Deze toepassing verzendt berichten voor elk van de methoden voor het doorsturen van berichten. Er is ook een map in de download de volledige Azure Resource Manager-sjabloon en parameterbestand, evenals de Azure CLI en PowerShell-scripts bevat.

Als u de bestanden niet uit de opslagplaats in stap 1 van deze zelfstudie downloaden, gaat u verder en download deze via [IoT Apparaatsimulatie](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Deze koppeling te selecteren, downloadt een opslagplaats met meerdere toepassingen in. de oplossing die u zoekt is iot-hub/Tutorials/Routing/IoT_SimulatedDevice.sln. 

Dubbelklik op het oplossingsbestand (IoT_SimulatedDevice.sln) te openen van de code in Visual Studio en opent u Program.cs. Vervang `{iot hub hostname}` door de hostnaam van de IoT Hub. De indeling van de hostnaam van de IoT Hub is **{iot-hub-name} .azure-devices.net**. Voor deze zelfstudie is de naam van de hubhost **ContosoTestHub.azure devices.net**. Vervang vervolgens `{device key}` door de apparaatsleutel die u eerder hebt opgeslagen bij het instellen van het gesimuleerde apparaat. 

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

   ![De resulterende e-mailberichten](./media/tutorial-routing-view-message-routing-results/results-in-email.png)

   Dit betekent dat de volgende instructies wordt voldaan. 

   * De routering naar de Service Bus-wachtrij werkt correct.
   * De logische app die het bericht uit de Service Bus-wachtrij ophaalt, werkt correct.
   * De connector van de logische app voor Outlook werkt goed. 

2. In de [Azure-portal](https://portal.azure.com), selecteer **resourcegroepen** en selecteer de resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**. Selecteer het opslagaccount, selecteer **Blobs**, selecteert u de Container. In deze zelfstudie wordt gebruikgemaakt van **contosoresults**. U zou een map moeten zien, en u kunt inzoomen via de mappen totdat u een of meer bestanden ziet. Open een van deze bestanden; ze bevatten de vermeldingen die zijn doorgestuurd naar het opslagaccount. 

   ![De resultaat-bestanden in de opslag](./media/tutorial-routing-view-message-routing-results/results-in-storage.png)

Dit betekent dat de volgende instructie is ingesteld op true.

   * De routering naar de Service Bus-wachtrij werkt correct.

Stel nu de Power BI-visualisatie in terwijl de toepassing nog steeds wordt uitgevoerd om de berichten te bekijken die afkomstig zijn van de standaardroutering.

## <a name="set-up-the-power-bi-visualizations"></a>De Power BI-visualisaties instellen

1. Meld u aan bij uw [Power BI](https://powerbi.microsoft.com/)-account.

2. Ga naar **Werkruimten** en selecteer de werkruimte die u hebt ingesteld toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt. In deze zelfstudie wordt gebruikgemaakt van **Mijn werkruimte**. 

3. Selecteer **gegevenssets**. Als u geen gegevenssets hebt, wacht een paar minuten en controleert u het opnieuw.

   U zou de vermelde gegevensset moeten zien die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt. In deze zelfstudie wordt gebruikgemaakt van **contosodataset**. (Het kan 5-10 minuten duren voordat de gegevensset voor de eerste keer wordt weergegeven.)

4. Onder **acties**, selecteert u het eerste pictogram om een rapport te maken.

   ![Power BI-werkruimte met acties en het rapport het pictogram gemarkeerd](./media/tutorial-routing-view-message-routing-results/power-bi-actions.png)

5. Maak een lijndiagram om in realtime de temperatuur gedurende een bepaalde periode weer te geven.

   * Toevoegen op de rapportpagina voor het maken van een lijndiagram hiervoor het pictogram lijndiagram.

     ![De velden en visualisaties](./media/tutorial-routing-view-message-routing-results/power-bi-visualizations-and-fields.png)

   * Klap in het deelvenster **Velden** de tabel uit die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt. In deze zelfstudie wordt gebruikgemaakt van **contosotable**.

   * Sleep **EventEnqueuedUtcTime** naar **As** in het deelvenster **Visualisaties**.

   * Sleep **Temperatuur** naar **Waarden**.

   Er wordt een lijndiagram gemaakt. De x-as geeft de datum en tijd in UTC-tijdzone aan. De y-as geeft de temperatuur van de sensor aan.

6. Maak een ander lijndiagram om in realtime de vochtigheid gedurende een bepaalde periode weer te geven. Volg, als u het tweede diagram wilt instellen, dezelfde stappen als hierboven en plaats **EventEnqueuedUtcTime** op de x-as en **Vochtigheid** op de y-as.

   ![De laatste Power BI-rapport met de twee grafieken](./media/tutorial-routing-view-message-routing-results/power-bi-report.png)

7. Selecteer **opslaan** het rapport wilt opslaan.

U zou gegevens moeten kunnen zien in beide diagrammen. Dit betekent dat de volgende instructies wordt voldaan:

   * De routering naar het standaardeindpunt werkt correct.
   * De Azure Stream Analytics-taak stroomt correct.
   * De Power BI-visualisatie is juist ingesteld.

U kunt vernieuwen de grafieken om te zien van de meest recente gegevens door de knop Vernieuwen boven aan het Power BI-venster te selecteren. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u verwijderen van alle resources die u via beide onderdelen van deze zelfstudie hebt gemaakt wilt, verwijdert u de resourcegroep. Hiermee verwijdert u ook alle resources in de groep. In dit geval worden de IoT Hub, de Service Bus-naamruimte en -wachtrij, de logische app, het opslagaccount en de resourcegroep zelf verwijderd. 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Resources in de Power BI-visualisatie opschonen

Meld u aan bij uw [Power BI](https://powerbi.microsoft.com/)-account. Ga naar uw werkruimte. In deze zelfstudie wordt gebruikgemaakt van **Mijn werkruimte**. Als u wilt verwijderen van de Power BI-visualisatie, gaat u naar gegevenssets en het Prullenbak kunt pictogram als u wilt verwijderen van de gegevensset. In deze zelfstudie wordt gebruikgemaakt van **contosodataset**. Wanneer u de gegevensset verwijdert, wordt het rapport ook verwijderd.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>De Azure CLI gebruiken voor het opschonen van resources

U kunt de resourcegroep verwijderen met de opdracht [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete). `$resourceGroup` is ingesteld op **ContosoResources** terug aan het begin van deze zelfstudie.

```azurecli-interactive
az group delete --name $resourceGroup
```

### <a name="use-powershell-to-clean-up-resources"></a>PowerShell gebruiken voor het opschonen van resources

U kunt de resourcegroep verwijderen met de opdracht [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup). `$resourceGroup` is ingesteld op **ContosoResources** terug aan het begin van deze zelfstudie.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie 2-onderdeel, hebt u geleerd hoe u routering van berichten naar IoT Hub-berichten naar andere bestemmingen te routeren door de volgende taken uit te.  

**Deel i Resources maken en instellen op Routering van berichten**
> [!div class="checklist"]
> * De resources--een IoT-hub, een storage-account, een Service Bus-wachtrij en een gesimuleerd apparaat maken.
> * De eindpunten en berichtroutes in IoT Hub configureren voor de storage-account en Service Bus-wachtrij.

**Deel II: Berichten verzenden naar de hub, gerouteerde resultaten weergeven**
> [!div class="checklist"]
> * Maak een logische app die wordt geactiveerd en een e-mailbericht verzendt wanneer een bericht wordt toegevoegd aan de Service Bus-wachtrij.
> * Download een app die een IoT-apparaat aanzet tot het verzenden van berichten naar de hub voor de verschillende routeringsopties en voer deze uit.
> * Maak een Power BI-visualisatie voor gegevens die naar het standaardeindpunt worden verzonden.
> * Bekijk de resultaten...
> * .. .in de Service Bus-wachtrij en e-mailberichten.
> * ...in het opslagaccount.
> * ...in de Power BI-visualisatie.

Ga door naar de volgende zelfstudie voor informatie over het beheren van de toestand van een IoT-apparaat. 

> [!div class="nextstepaction"]
> [Metrische gegevens en diagnostische gegevens instellen en gebruiken met een IoT Hub](tutorial-use-metrics-and-diags.md)
