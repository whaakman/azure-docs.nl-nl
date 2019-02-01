---
title: C# IoT Edge-module voor Azure Data Box Edge | Microsoft Docs
description: Informatie over het ontwikkelen van een C# IoT Edge-module die kan worden geïmplementeerd op de rand van uw gegevens in.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 01/31/2019
ms.author: alkohli
ms.openlocfilehash: d0b171f05bb2243da83509348b099bebcb44ebc7
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55508705"
---
# <a name="develop-a-c-iot-edge-module-to-move-files-on-data-box-edge-preview"></a>Ontwikkelen van een C# IoT Edge-module voor het verplaatsen van bestanden op gegevens in Edge (Preview)

In dit artikel begeleidt u stapsgewijs hoe u een IoT Edge-module voor implementatie met uw gegevens in het Edge-apparaat maakt. Azure Data Box Edge is een opslagoplossing waarmee u gegevens kunt verwerken en via een netwerk kunt verzenden naar Azure.

U kunt Azure IoT Edge-modules gebruiken met uw gegevens in Edge het transformeren van de gegevens die naar Azure verplaatst. In dit artikel wordt gebruikt in de module implementeert de logica voor het kopiëren van een bestand van een lokale share naar een cloud-share op uw gegevens in het Edge-apparaat.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Maak een container registry voor het opslaan en beheren van uw modules (Docker-installatiekopieën).
> * Maak een IoT Edge-module op uw gegevens in het Edge-apparaat te implementeren.

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 

## <a name="about-the-iot-edge-module"></a>Over de IoT Edge-module

Uw gegevens in het Edge-apparaat kunt implementeren en uitvoeren van IoT Edge modules. Edge-modules zijn in feite Docker-containers die een bepaalde taak uitvoeren, zoals opnemen een bericht van een apparaat, een bericht te transformeren of een bericht verzenden naar een IoT-Hub. In dit artikel maakt u een module waarmee bestanden worden gekopieerd van een lokale share naar een cloud-share op uw gegevens in het Edge-apparaat.

1. Bestanden worden geschreven naar de lokale share op uw gegevens in het Edge-apparaat.
2. De gebeurtenis bestand generator maakt een bestandgebeurtenis voor elk bestand geschreven naar de lokale share. De gebeurtenissen bestand worden vervolgens verzonden naar IoT Edge Hub (in IoT Edge-runtime).

   > [!IMPORTANT]
   > Het bestand gebeurtenissen worden gegenereerd voor de zojuist gemaakte bestanden. Het wijzigen van bestaande bestanden geen bestand gebeurtenissen gegenereerd.

3. De bestandgebeurtenis voor het maken van een bestand event-object dat ook een relatief pad voor het bestand bevat wordt verwerkt door de aangepaste IoT Edge-module. De module een absoluut pad met behulp van het relatieve bestandspad genereert en het bestand vanaf de lokale share gekopieerd naar de cloud-share. De module wordt het bestand vervolgens verwijderd uit de lokale share.

![De werking van Azure IoT Edge-module voor gegevens in het edge-apparaten](./media/data-box-edge-create-iot-edge-module/how-module-works.png)

Zodra het bestand zich in de cloud-share, automatisch wordt het geüpload naar uw Azure Storage-account.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, controleert u of u over het volgende beschikt:

- Een Data Box Edge-apparaat dat wordt uitgevoerd.

    - Het apparaat heeft ook een gekoppelde IoT-Hub-resource. Ga voor meer informatie naar [maken van een IoT Hub-resource](data-box-edge-deploy-configure-compute.md#create-an-iot-hub-resource) voor uw gegevens in Edge.
    - Het apparaat heeft Edge compute-rol is geconfigureerd. Ga voor meer informatie naar [berekeningsfunctie instellen](data-box-edge-deploy-configure-compute.md#set-up-compute-role) voor uw gegevens in het edge-apparaten.

- De volgende resources voor het ontwikkelen:

    - [Visual Studio Code](https://code.visualstudio.com/).
    - [De extensie C# voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
    - [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).
    - [.NET Core 2.1 SDK](https://www.microsoft.com/net/download).
    - [Docker CE](https://store.docker.com/editions/community/docker-ce-desktop-windows). U moet maken van een account om te downloaden en installeren van de software.

## <a name="create-a-container-registry"></a>Een containerregister maken

Een Azure-containerregister is een persoonlijk Docker-register in Azure waar u uw persoonlijke installatiekopieën van de Docker-container kunt opslaan en beheren. De twee populaire Docker-Registerservices beschikbaar in de cloud zijn Azure Container Registry en Docker Hub. In dit artikel maakt gebruik van het Containerregister.

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Selecteer **een resource maken > Containers > Container Registry**. Klik op **Create**.
3. Provide:

    1. Een unieke **registernaam** in Azure met 5 tot 50 alfanumerieke tekens.
    2. Kies een **abonnement**.
    3. Maak een nieuwe of kies een bestaande **resourcegroep**.
    4. Selecteer een **locatie**. Het is raadzaam dat deze locatie hetzelfde zijn als dat is gekoppeld aan de gegevens in het Edge-resource.
    5. Stel de **Gebruiker met beheerdersrechten** in op **Inschakelen**.
    6. De SKU ingesteld op **Basic**.

    ![Containerregister maken](./media/data-box-edge-create-iot-edge-module/create-container-registry-1.png)
 
4. Selecteer **Maken**.
5. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u **Toegangssleutels**.

    ![Toegangssleutel ophalen](./media/data-box-edge-create-iot-edge-module/get-access-keys-1.png)
 
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later het publiceren van de Docker-installatiekopie naar het register en de registerreferenties toevoegen aan de Azure IoT Edge-runtime.


## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren

De volgende stappen maakt u een IoT Edge-module-project op basis van de .NET Core-SDK 2.1. Het project maakt gebruik van Visual Studio Code en de Azure IoT Edge-extensie.

### <a name="create-a-new-solution"></a>Een nieuwe oplossing maken

Maak een C#-oplossingssjabloon die u met uw eigen code kunt aanpassen.

1. Selecteer in Visual Studio Code, **weergave > Command Palette** openen van het opdrachtenpalet van VS Code.
2. Voer in het opdrachtpalet de opdracht **Azure: Aanmelden** in, voer deze uit en volg de instructies om u aan te melden bij uw Azure-account. Als u al bent aangemeld, kunt u deze stap overslaan.
3. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Geef in het opdrachtpalet de volgende informatie op om de oplossing te maken:

    1. Selecteer de map waarin u de oplossing wilt maken.
    2. Geef een naam op voor de oplossing of houd de standaardnaam **EdgeSolution** aan.
    
        ![Nieuwe oplossing 1 maken](./media/data-box-edge-create-iot-edge-module/create-new-solution-1.png)

    3. Kies **C# Module** als de modulesjabloon.
    4. Vervang de naam van de standaard-module met de naam die u wilt toewijzen, in dit geval is het **FileCopyModule**.
    
        ![Nieuwe oplossing 2 maken](./media/data-box-edge-create-iot-edge-module/create-new-solution-2.png)

    5. Geef het containerregister dat u hebt gemaakt in de vorige sectie als de opslagplaats voor installatiekopieën voor uw eerste module. Vervang **localhost:5000** door de gekopieerde waarde voor de aanmeldingsserver.

        De laatste tekenreeks ziet eruit als `<Login server name>/<Module name>`. In dit voorbeeld wordt de tekenreeks is: `mycontreg2.azurecr.io/filecopymodule`.

        ![Nieuwe oplossing 3 maken](./media/data-box-edge-create-iot-edge-module/create-new-solution-3.png)

4. Ga naar **bestand > map openen**.

    ![Nieuwe oplossing 4 maken](./media/data-box-edge-create-iot-edge-module/create-new-solution-4.png)

5. Bladeren en wijs de **EdgeSolution** map die u eerder hebt gemaakt. Het venster VS Code geladen uw werkruimte IoT Edge-oplossing met de vijf onderdelen op het hoogste niveau. U niet bewerkt de **.vscode** map **.gitignore** bestand **.env** -bestand, en de **deployment.template.json** in dit artikel.
    
    Het enige onderdeel dat u is de map modules. Deze map bevat de C#-code voor uw module en de Docker-bestanden naar uw module als een containerinstallatiekopie maken.

    ![Nieuwe oplossing 5 maken](./media/data-box-edge-create-iot-edge-module/create-new-solution-5.png)

### <a name="update-the-module-with-custom-code"></a>De module bijwerken met aangepaste code

1. Open in de VS Code explorer **modules > CSharpModule > Program.cs**.
2. Aan de bovenkant van de **FileCopyModule naamruimte**, voeg de volgende using-instructies voor typen die later worden gebruikt. **Microsoft.Azure.Devices.Client.Transport.Mqtt** is een protocol voor het verzenden van berichten naar IoT Edge Hub.

    ```
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;
    using Newtonsoft.Json;
    ```
3. Voeg de **InputFolderPath** en **OutputFolderPath** variabele aan de klasse Program.

    ```
    class Program
        {
            static int counter;
            private const string InputFolderPath = "/home/LocalShare";
            private const string OutputFolderPath = "/home/CloudShare";
    ```

    > [!IMPORTANT]
    > Noteer de `InputFolderPath` en de `OutputFolderPath`. U moet deze paden opgeven wanneer u deze module implementeert.

4. Voeg de **MessageBody** klasse aan de klasse Program. Deze klassen bepalen het verwachte schema voor de hoofdtekst van inkomende berichten.

    ```
    /// <summary>
    /// The MessageBody class defines the expected schema for the body of incoming messages. 
    /// </summary>
    private class FileEvent
    {
        public string ChangeType { get; set; }

        public string ShareRelativeFilePath { get; set; }

        public string ShareName { get; set; }
    }
    ```

5. In de methode **Init** wordt met de code een object **ModuleClient** gemaakt en geconfigureerd. Dit object maakt de module verbinding maken met de lokale Azure IoT Edge-runtime MQTT-protocol gebruiken om te verzenden en ontvangen van berichten. De verbindingsreeks die wordt gebruikt in de Init-methode wordt aan de module geleverd door de IoT Edge-runtime. De code registreert een retouraanroep FileCopy voor het ontvangen van berichten van een IoT Edge hub via de **input1** eindpunt.

    ```
    /// <summary>
    /// Initializes the ModuleClient and sets up the callback to receive
    /// messages containing file event information
    /// </summary>
    static async Task Init()
    {
        MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
        ITransportSettings[] settings = { mqttSetting };

        // Open a connection to the IoT Edge runtime
        ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
        await ioTHubModuleClient.OpenAsync();
        Console.WriteLine("IoT Hub module client initialized.");

        // Register callback to be called when a message is received by the module
        await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FileCopy, ioTHubModuleClient);
    }
    ```

6. Voeg de code voor **FileCopy**.

    ```
            /// <summary>
        /// This method is called whenever the module is sent a message from the IoT Edge Hub. 
        /// This method deserializes the file event, extracts the corresponding relative file path, and creates the absolute input file path using the relative file path and the InputFolderPath.
        /// This method also forms the absolute output file path using the relative file path and the OutputFolderPath. It then copies the input file to output file and deletes the input file after the copy is complete.
        /// </summary>
        static async Task<MessageResponse> FileCopy(Message message, object userContext)
        {
            int counterValue = Interlocked.Increment(ref counter);

            try
            {
                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                if (!string.IsNullOrEmpty(messageString))
                {
                    var fileEvent = JsonConvert.DeserializeObject<FileEvent>(messageString);

                    string relativeFileName = fileEvent.ShareRelativeFilePath.Replace("\\", "/");
                    string inputFilePath = InputFolderPath + relativeFileName;
                    string outputFilePath = OutputFolderPath + relativeFileName;

                    if (File.Exists(inputFilePath))                
                    {
                        Console.WriteLine($"Moving input file: {inputFilePath} to output file: {outputFilePath}");
                        var outputDir = Path.GetDirectoryName(outputFilePath);
                        if (!Directory.Exists(outputDir))
                        {
                            Directory.CreateDirectory(outputDir);
                        }

                        File.Copy(inputFilePath, outputFilePath, true);
                        Console.WriteLine($"Copied input file: {inputFilePath} to output file: {outputFilePath}");
                        File.Delete(inputFilePath);
                        Console.WriteLine($"Deleted input file: {inputFilePath}");
                    } 
                    else
                    {
                        Console.WriteLine($"Skipping this event as input file doesn't exist: {inputFilePath}");   
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Caught exception: {0}", ex.Message);
                Console.WriteLine(ex.StackTrace);
            }

            Console.WriteLine($"Processed event.");
            return MessageResponse.Completed;
        }

    }
    ```

7. Sla dit bestand op.

## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In de vorige sectie hebt u een oplossing die het IoT Edge en code toegevoegd aan de FileCopyModule om bestanden te kopiëren van lokale share naar de cloud-share. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen.

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde Visual Studio Code-terminal in te voeren.

    `docker login <ACR login server> -u <ACR username>`

    Gebruik de aanmeldingsserver en de gebruikersnaam die u hebt gekopieerd uit uw containerregister. 

    ![Bouw en push IoT Edge-oplossing](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-1.png)

2. Wanneer u hierom wordt gevraagd om wachtwoord, voert u het wachtwoord. U kunt ook ophalen met de waarden voor de aanmeldingsserver, gebruikersnaam en het wachtwoord van de **toegangssleutels** in uw containerregister in Azure portal.
 
3. Zodra de referenties zijn opgegeven, kunt u uw module installatiekopie pushen naar uw Azure container registry. In de VS Code Explorer met de rechtermuisknop op de **module.json** bestand en selecteer **Build and Push IoT Edge-oplossing**.

    ![Bouw en push IoT Edge-oplossing](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-2.png)
 
    Wanneer u Visual Studio-Code voor het maken van uw oplossing zien, twee opdrachten worden uitgevoerd in de geïntegreerde terminal: docker-build- en docker push. Met deze twee opdrachten wordt uw code gebouwd, wordt de CSharpModule.dll in een container opgeslagen en wordt de code vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde.

    U wordt gevraagd om te kiezen het module-platform. Selecteer *amd64* overeenkomt met Linux.

    ![Platform selecteren](./media/data-box-edge-create-iot-edge-module/select-platform.png)

    > [!IMPORTANT] 
    > Alleen de Linux-modules worden ondersteund.

    U ziet mogelijk de volgende waarschuwing die u kunt negeren:

    *Program.cs(77,44): waarschuwing CS1998: Deze asynchrone methode beschikt niet over 'await' operators en synchroon worden uitgevoerd. Overweeg het gebruik van de operator 'await' om te wachten op een niet-blokkerende API-aanroepen, of 'await Task.Run(...)' aan het CPU-gebonden werken op een achtergrond-thread.*

4. U kunt het volledige adres van de containerinstallatiekopie, inclusief de tag, zien in de geïntegreerde terminal van VS Code. Het adres van de installatiekopie is gebaseerd op informatie die zich in de module.json-bestand met de indeling `<repository>:<version>-<platform>`. In dit artikel ziet dit eruit als `mycontreg2.azurecr.io/filecopymodule:0.0.1-amd64`.

## <a name="next-steps"></a>Volgende stappen

Als u wilt implementeren en uitvoeren van deze module voor gegevens in het edge-apparaten, Zie de stappen in [toevoegen van een aangepaste module](data-box-edge-deploy-configure-compute.md#add-a-custom-module).
