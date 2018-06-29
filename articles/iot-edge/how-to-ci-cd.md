---
title: Azure IoT rand continue integratie en continue implementatie | Microsoft Docs
description: Overzicht van de continue integratie en continue implementatie voor Azure IoT rand
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 62d8d770f6b4c3a62a2395eb8c1505dbc3835c28
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37047452"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continue integratie en continue implementatie naar Azure IoT rand

Dit artikel wordt beschreven hoe u de continue integratie en functies voor continue implementatie van Visual Studio Team Services (VSTS) en Microsoft Team Foundation Server (TFS) gebruiken kunt om te bouwen, testen en implementeren van toepassingen snel en efficiënt in uw Azure IoT-rand. 

In dit artikel leert u hoe:
* Maak en Controleer in een voorbeeld van een rand van de IoT-oplossing met eenheid tests.
* Azure IoT Edge-extensie voor uw VSTS installeren.
* Continue integratie (CI) naar de oplossing bouwen en uitvoeren van de eenheidstests configureren.
* Doorlopende implementatie configureren (CD) om de oplossing implementeert en antwoorden weer.

Het duurt 30 minuten om de stappen in dit artikel te voltooien.

![CI en CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Een voorbeeld-Edge van Azure IoT-oplossing met behulp van Visual Studio Code maken

In deze sectie maakt u een voorbeeld van een IoT-rand oplossing met controles die u als onderdeel van deze procedure kunt uitvoeren. Voer de stappen in voordat u de instructies in deze sectie, [ontwikkelen van een rand van de IoT-oplossing met meerdere modules in Visual Studio Code](tutorial-multiple-modules-in-vscode.md).

1. Typ in de VS Code opdracht palet en voer de opdracht **rand: nieuwe IoT oplossing**. Selecteer vervolgens uw werkruimtemap, geeft u de oplossingsnaam (de standaardnaam is **EdgeSolution**), en maak een C#-Module (**FilterModule**) als de eerste gebruikersmodule in deze oplossing. U moet ook de opslagplaats voor Dockerinstallatiekopieën opgeven voor de eerste module. De standaard-opslagplaats voor installatiekopieën is gebaseerd op een lokale Docker-register (`localhost:5000/filtermodule`). U moet het Azure Container register wijzigen (`<your container registry address>/filtermodule`) of Docker-Hub voor verdere continue integratie.

    ![Setup ACR](./media/how-to-ci-cd/acr.png)

2. Het venster tegenover Code laadt uw werkruimte rand van de IoT-oplossing. U kunt eventueel typt en uitvoeren **rand: module toevoegen IoT rand** meer modules toevoegen. Er is een `modules` map, een `.vscode` map en een deployment manifest sjabloonbestand in de hoofdmap. Alle gebruikers module codes zijn submappen onder de map `modules`. De `deployment.template.json` is de deployment manifest-sjabloon. Sommige parameters in dit bestand wordt geparseerd uit de `module.json`, die bestaat in de modulemap van elke.

3. Uw voorbeeld rand van de IoT-oplossing is nu gereed. De standaard C#-module fungeert als een module pipe-bericht. In de `deployment.template.json`, ziet u deze oplossing bevat twee modules. Het bericht wordt gegenereerd op basis van de `tempSensor` -module, en wordt het rechtstreeks worden doorgegeven `FilterModule`, vervolgens naar uw IoT-hub zijn verzonden. Vervang de volledige **Program.cs** bestand met de onderstaande inhoud. Voor meer informatie over dit codefragment kunt u verwijzen naar [maken van een project IoT rand C# module](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

    ```csharp
    namespace FilterModule
    {
        using System;
        using System.IO;
        using System.Runtime.InteropServices;
        using System.Runtime.Loader;
        using System.Security.Cryptography.X509Certificates;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;
        using System.Collections.Generic;     // for KeyValuePair<>
        using Microsoft.Azure.Devices.Shared; // for TwinCollection
        using Newtonsoft.Json;                // for JsonConvert

        public class MessageBody
        {
            public Machine machine { get; set; }
            public Ambient ambient { get; set; }
            public string timeCreated { get; set; }
        }
        public class Machine
        {
            public double temperature { get; set; }
            public double pressure { get; set; }
        }
        public class Ambient
        {
            public double temperature { get; set; }
            public int humidity { get; set; }
        }

        public class Program
        {
            static int counter;
            static int temperatureThreshold { get; set; } = 25;

            static void Main(string[] args)
            {
                Init().Wait();

                // Wait until the app unloads or is cancelled
                var cts = new CancellationTokenSource();
                AssemblyLoadContext.Default.Unloading += (ctx) => cts.Cancel();
                Console.CancelKeyPress += (sender, cpe) => cts.Cancel();
                WhenCancelled(cts.Token).Wait();
            }

            /// <summary>
            /// Handles cleanup operations when app is cancelled or unloads
            /// </summary>
            public static Task WhenCancelled(CancellationToken cancellationToken)
            {
                var tcs = new TaskCompletionSource<bool>();
                cancellationToken.Register(s => ((TaskCompletionSource<bool>)s).SetResult(true), tcs);
                return tcs.Task;
            }

            /// <summary>
            /// Initializes the ModuleClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init()
            {
                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessage, ioTHubModuleClient);
            }

            /// <summary>
            /// This method is called whenever the module is sent a message from the EdgeHub. 
            /// It just pipe the messages without any change.
            /// It prints all the incoming messages.
            /// </summary>
            static async Task<MessageResponse> FilterMessage(Message message, object userContext)
            {
                int counterValue = Interlocked.Increment(ref counter);

                var moduleClient = userContext as ModuleClient;
                if (moduleClient == null)
                {
                    throw new InvalidOperationException("UserContext doesn't contain " + "expected values");
                }

                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                var filteredMessage = filter(message);

                if (filteredMessage != null && !string.IsNullOrEmpty(messageString))
                {
                    var pipeMessage = new Message(messageBytes);
                    foreach (var prop in message.Properties)
                    {
                        pipeMessage.Properties.Add(prop.Key, prop.Value);
                    }
                    await moduleClient.SendEventAsync("output1", pipeMessage);
                    Console.WriteLine("Received message sent");
                }
                return MessageResponse.Completed;
            }

            public static Message filter(Message message)
            {
                var counterValue = Interlocked.Increment(ref counter);

                var messageBytes = message.GetBytes();
                var messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message {counterValue}: [{messageString}]");

                // Get message body
                var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                {
                    Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                        $"exceeds threshold {temperatureThreshold}");
                    var filteredMessage = new Message(messageBytes);
                    foreach (KeyValuePair<string, string> prop in message.Properties)
                    {
                        filteredMessage.Properties.Add(prop.Key, prop.Value);
                    }

                    filteredMessage.Properties.Add("MessageType", "Alert");
                    return filteredMessage;
                }
                return null;
            }
        }
    }
    ```

4. Maken van een .net Core eenheid testproject. Maak een nieuwe map in Verkenner tegenover Code **tests\FilterModuleTest** in uw werkruimte. Klik dan in VS-Code geïntegreerde terminal (**Ctrl + '**), voert u na de opdrachten voor het maken van een testproject xunit en verwijzing toevoegen aan de **FilterModule** project.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Mapstructuur](./media/how-to-ci-cd/add-test-project.png)

5. In de **FilterModuleTest** map bijwerken van de bestandsnaam van **UnitTest1.cs** naar **FilterModuleTest.cs**. Selecteer en open **FilterModuleTest.cs**, vervang de volledige code met onderstaande codefragment die de eenheidstests op basis van het project FilterModule bevat.

    ```csharp
    using Xunit;
    using FilterModule;
    using Newtonsoft.Json;
    using System;
    using System.IO;
    using System.Runtime.InteropServices;
    using System.Runtime.Loader;
    using System.Security.Cryptography.X509Certificates;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;

    namespace FilterModuleTest
    {
        public class FilterModuleTest
        {
            [Fact]
            public void filterLessThanThresholdTest()
            {
                var source = createMessage(25 - 1);
                var result = Program.filter(source);
                Assert.True(result == null);
            }

            [Fact]
            public void filterMoreThanThresholdAlertPropertyTest()
            {
                var source = createMessage(25 + 1);
                var result = Program.filter(source);
                Assert.True(result.Properties["MessageType"] == "Alert");
            }

            [Fact]
            public void filterMoreThanThresholdCopyPropertyTest()
            {
                var source = createMessage(25 + 1);
                source.Properties.Add("customTestKey", "customTestValue");
                var result = Program.filter(source);
                Assert.True(result.Properties["customTestKey"] == "customTestValue");
            }

            private Message createMessage(int temperature)
            {
                var messageBody = createMessageBody(temperature);
                var messageString = JsonConvert.SerializeObject(messageBody);
                var messageBytes = Encoding.UTF8.GetBytes(messageString);
                return new Message(messageBytes);
            }

            private MessageBody createMessageBody(int temperature)
            {
                var messageBody = new MessageBody
                {
                    machine = new Machine
                    {
                        temperature = temperature,
                        pressure = 0
                    },
                    ambient = new Ambient
                    {
                        temperature = 0,
                        humidity = 0
                    },
                    timeCreated = string.Format("{0:O}", DateTime.Now)
                };

                return messageBody;
            }
        }
    }
    ```

6. In de geïntegreerde terminal kunt u opgeven na opdrachten lokaal eenheidstests uitvoeren. 
    ```cmd
    dotnet test
    ```

    ![Testen van eenheden](./media/how-to-ci-cd/unit-test.png)

7. Deze projecten opslaan en vervolgens controleren in uw opslagplaats VSTS of TFS.
    

> [!NOTE]
> Voor meer informatie over het gebruik van VSTS code opslagplaatsen, Zie [delen van uw code met Visual Studio en VSTS Git](https://docs.microsoft.com/vsts/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Continue integratie configureren
In deze sectie maakt u een build-definitie die is geconfigureerd voor het automatisch uitvoeren als u wijzigingen in het voorbeeld rand van de IoT-oplossing inchecken en wordt automatisch uitvoeren van de UnitTests bevat.

1. Meld u aan bij uw account VSTS (**https://**_uw account_**. visualstudio.com**) en open het project waar u dit selectievakje is ingeschakeld in de voorbeeld-app.

    ![In-code](./media/how-to-ci-cd/init-project.png)

1. Ga naar [Azure IoT rand voor VSTS](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) op VSTS Marketplace. Klik op **vraag gratis** en volg de wizard voor het installeren van deze extensie aan uw account VSTS of downloaden naar uw TFS.

    ![De extensie installeren](./media/how-to-ci-cd/install-extension.png)

1. Open in uw VSTS de **bouwen &amp; Release** hub en in de **Builds** Kies **+ nieuwe definitie**. Of, als u al build definities hebt, kiest u de **+ nieuw** knop. 

    ![Nieuwe build](./media/how-to-ci-cd/add-new-build.png)

1. Als u wordt gevraagd, selecteert u de **VSTS Git** gegevensbrontype; selecteert u het project, de opslagplaats en de vertakking waar uw code bevindt. Kies **gaan**.

    ![VSTS git selecteren](./media/how-to-ci-cd/select-vsts-git.png)

1. In **Selecteer een sjabloon** venster kiezen **beginnen met een leeg proces**.

    ![Start leeg](./media/how-to-ci-cd/start-with-empty.png)

1. Klik op **+** aan de rechterkant van **fase 1** een taak toevoegen aan de fase. Zoek en selecteer **.Net Core**, en klik op **toevoegen** deze taak toevoegen aan de fase.

    ![Test voor DotNet](./media/how-to-ci-cd/add-dot-net-core.png)

1. Update de **weergavenaam** naar **dotnet test**, en in de **opdracht** vervolgkeuzelijst, selecteer **testen**. Voeg onder het pad naar de **pad naar projecten**.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![Test dotnet configureren](./media/how-to-ci-cd/dotnet-test.png)

1. Klik op **+** aan de rechterkant van **fase 1** een taak toevoegen aan de fase. Zoek en selecteer **Azure IoT rand**, en klik op **toevoegen** knop **tweemaal** deze taken toevoegen aan de fase.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. In de eerste taak van de rand van Azure IoT bijwerken de **weergavenaam** naar **Module bouwen en Push**, en in de **actie** vervolgkeuzelijst, selecteer **bouwen enPush**. In de **Module.json bestand** tekstvak onder pad aan toe te voegen. Kies vervolgens **Container registertype**, zorg ervoor dat u configureert en selecteert u het register dezelfde in uw code. Deze taak bouwt en push alle modules in de oplossing en publiceren naar de container-registersleutel die u hebt opgegeven. Als uw modules wordt gepusht naar verschillende registers, kunt u meerdere laten **Module bouwen en Push** taken.

    ```
    **/module.json
    ```

    ![Module Build en Push](./media/how-to-ci-cd/module-build-push.png)

1. In de tweede taak van de rand van Azure IoT bijwerken de **weergavenaam** naar **implementeren in IoT randapparaat**, en in de **actie** vervolgkeuzelijst, selecteer **implementeren naar IoT rand apparaat**. Selecteer uw Azure-abonnement en voer de naam van uw IoT-Hub. U kunt een IoT-rand implementatie-ID en de implementatie van prioriteit opgeven. U kunt ook implementeren op een of meer apparaten. Als u meerdere apparaten implementeert, moet u de voorwaarde voor het doel van apparaat opgeven. Als u wilt een apparaat om labels te gebruiken als de voorwaarde, moet u bijvoorbeeld bijwerken van de betreffende apparaten labels voordat de implementatie. 

    ![Implementeren naar rand](./media/how-to-ci-cd/deploy-to-edge.png)

1. Klik op de **proces** en zorg ervoor dat uw **Agent wachtrij** is **gehost Linux Preview**.

    ![Configureren](./media/how-to-ci-cd/configure-env.png)

1. Open de **Triggers** tabblad en schakel de **continue integratie** trigger. Zorg ervoor dat de vertakking met uw code is opgenomen.

    ![Trigger](./media/how-to-ci-cd/configure-trigger.png)

1. Sla de nieuwe build-definitie en een nieuwe build in de wachtrij. Klik op de **opslaan en wachtrij** knop.

1. Kies de koppeling naar de build in de berichtenbalk die wordt weergegeven. Of Ga naar de definitie voor de meest recente in de wachtrij build-taak maken.

    ![Ontwikkelen](./media/how-to-ci-cd/build-def.png)

1. Nadat de build is voltooid, ziet u de samenvatting voor elke taak en de resultaten in het logboekbestand live. 
    
    ![Voltooien](./media/how-to-ci-cd/complete.png)

1. U kunt teruggaan naar de Code van de VS en controleer of de IoT Hub apparaat explorer. Het apparaat aan de rand met de module moet beginnen met (Zorg ervoor dat u referenties van de registersleutel hebt toegevoegd aan de rand runtime).

    ![Rand uitgevoerd](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>Continue implementatie naar IoT Edge-apparaten

Zodat continue implementatie in feite moet u voor het instellen van de CI-taken met de randapparaten van juiste IoT, inschakelen van de **Triggers** voor uw vertakkingen in uw project. In klassieke DevOps uit veiligheidsoverwegingen bevat een project twee belangrijkste vertakkingen. De hoofdvertakking moet de stabiele versie van de code en de vertakking ontwikkelen bevat de meest recente codewijzigingen. Elke ontwikkelaar in het team moet vertakken vertakking ontwikkelen voor zijn of haar eigen functie vertakking als begonnen met de code, wat betekent dat alle doorvoeren bijwerken functie gebeurt vertakking uit de vertakking ontwikkelen. En elke pushed doorvoer via de CI-systeem moet worden getest. Nadat de code lokaal volledig getest, moet de functie vertakking naar de vertakking ontwikkelen via een pull-aanvraag worden samengevoegd. Wanneer de code op ontwikkelaars vertakking wordt getest via CI-systeem, kan het worden samengevoegd naar de hoofdvertakking via een pull-aanvraag.

Dus bij het implementeren van de rand van de IoT-apparaten, zijn er drie belangrijkste omgevingen.
- U kunt op de functie filiaal, gesimuleerde IoT randapparaat gebruiken op uw ontwikkelcomputer of implementeert op een fysieke rand van de IoT-apparaat.
- Vertakking op ontwikkelen, moet u op een fysieke IoT Edge-apparaat implementeren.
- Op de hoofdvertakking moet de doelapparaten IoT rand de productieapparaten.

## <a name="next-steps"></a>Volgende stappen

* Inzicht in de rand van de IoT-implementatie in [begrijpen IoT rand implementaties voor één apparaten of op grote schaal](module-deployment-monitoring.md)
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van de rand van de IoT-modules op grote schaal](how-to-deploy-monitor.md).
