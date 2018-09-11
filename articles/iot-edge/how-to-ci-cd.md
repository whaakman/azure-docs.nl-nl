---
title: Azure IoT Edge continue integratie en continue implementatie | Microsoft Docs
description: Overzicht van de continue integratie en continue implementatie voor Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5099ca70503ba2ed4ae8f4969a9199816c4986fb
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302568"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continue integratie en continue implementatie voor Azure IoT Edge

In dit artikel laat zien hoe u continue integratie en continue implementatiefuncties van Azure DevOps-Services en Microsoft Team Foundation Server (TFS) bouwen, testen en implementeren van toepassingen snel en efficiënt aan uw Azure IoT Edge kunt gebruiken. 

In dit artikel leert u hoe u:
* Maken en controleren in een voorbeeld van een IoT Edge-oplossing met eenheid tests.
* Azure IoT Edge-extensie voor uw Azure DevOps installeren.
* Continue integratie (CI) aan de oplossing bouwen en uitvoeren van de eenheidstests configureren.
* Configureer continue implementatie (CD) voor het implementeren van de oplossing en antwoorden weer.

Het duurt 30 minuten om de stappen in dit artikel te voltooien.

![CI en CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Een voorbeeld van Azure IoT Edge-oplossing met behulp van Visual Studio Code maken

In deze sectie maakt u een voorbeeld van een IoT Edge oplossing met eenheidstests dat kan worden uitgevoerd als onderdeel van het bouwproces. Voordat u de instructies in deze sectie, voer de stappen in [een IoT Edge-oplossing met meerdere modules in Visual Studio Code ontwikkelen](tutorial-multiple-modules-in-vscode.md).

1. Typ in het opdrachtenpalet van VS Code, en voer de opdracht **Edge: nieuwe IoT-Edge-oplossing**. Selecteer de werkruimtemap van uw, geeft u de oplossingsnaam (de standaardnaam is **EdgeSolution**), en maakt u een C#-Module (**FilterModule**) als de eerste gebruikersmodule in deze oplossing. U moet ook de opslagplaats voor Dockerinstallatiekopieën opgeven voor de eerste module. De standaard-opslagplaats voor installatiekopieën is gebaseerd op een lokale Docker-register (`localhost:5000/filtermodule`). U moet dit wijzigen in Azure Container Registry (`<your container registry address>/filtermodule`) of Docker-Hub voor verdere continue integratie.

    ![ACR instellen](./media/how-to-ci-cd/acr.png)

2. Het venster VS Code, de werkruimte van uw IoT Edge-oplossing wordt geladen. U kunt eventueel typt en uitvoeren **Edge: toevoegen aan IoT Edge module** om toe te voegen meer modules. Er is een `modules` map, een `.vscode` map en een manifest-sjabloonbestand van de implementatie in de hoofdmap. Alle gebruiker module codes zijn submappen onder de map `modules`. De `deployment.template.json` is het manifest sjabloon voor de implementatie. Sommige van de parameters in dit bestand wordt geparseerd uit de `module.json`, waar zich bevindt in de modulemap voor elke.

3. Uw voorbeeld IoT Edge-oplossing is nu gereed. De standaard C#-module fungeert als een pipe bericht-module. In de `deployment.template.json`, ziet u deze oplossing bevat twee modules. Het bericht wordt gegenereerd op basis van de `tempSensor` -module, en wordt rechtstreeks kan worden doorgesluisd `FilterModule`, vervolgens naar uw IoT-hub zijn verzonden. Vervang de gehele **Program.cs** bestand met de volgende inhoud. Voor meer informatie over dit codefragment kunt u verwijzen naar [maken van een IoT Edge C# module-project](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

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

4. Maken van een .net Core-eenheid test-project. Maak een nieuwe map in Verkenner van VS Code, **tests\FilterModuleTest** in uw werkruimte. Klik in VS Code geïntegreerde terminal (**Ctrl + '**), voert de volgende opdrachten voor het maken van een project xunit te testen en verwijzing toevoegen aan de **FilterModule** project.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Mapstructuur](./media/how-to-ci-cd/add-test-project.png)

5. In de **FilterModuleTest** map bijwerken van de bestandsnaam van **UnitTest1.cs** naar **FilterModuleTest.cs**. Selecteer en open **FilterModuleTest.cs**, vervang de volledige code met onderstaande codefragment, waarin u de eenheidstests op basis van de FilterModule-project.

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

6. In de geïntegreerde terminal, kunt u voert u de volgende opdrachten UnitTests lokaal uitvoeren. 
    ```cmd
    dotnet test
    ```

    ![Test jednotky](./media/how-to-ci-cd/unit-test.png)

7. Sla deze projecten en bekijk het in de opslagplaats van uw Azure DevOps of TFS.
    

> [!NOTE]
> Zie voor meer informatie over het gebruik van Azure-opslagplaatsen [deel van uw code met Visual Studio en Azure-opslagplaatsen](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Continue integratie configureren
In deze sectie maakt u een build-pijplijn die is geconfigureerd voor het automatisch uitgevoerd wanneer u op wijzigingen in het voorbeeld IoT Edge-oplossing controleren en deze automatisch de eenheidstests hierop worden uitgevoerd.

1. Meld u aan bij uw organisatie Azure DevOps (**https://**_uw account_**. visualstudio.com**) en open het project waarin u dit selectievakje is ingeschakeld in de voorbeeld-app.

    ![De code is ingecheckt](./media/how-to-ci-cd/init-project.png)

1. Ga naar [Azure IoT Edge voor Azure DevOps](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) op Azure DevOps-Marketplace. Klik op **gratis downloaden** en volg de wizard voor het installeren van deze extensie voor uw Azure DevOps-organisatie of downloaden naar uw TFS.

    ![De extensie installeren](./media/how-to-ci-cd/install-extension.png)

1. Open in uw Azure DevOps, de **bouwen &amp; Release** hub en klik in de **bouwt** tabblad **+ nieuwe pijplijn**. Of, als u bouwen van pijplijnen al hebt, kiest u de **+ nieuw** knop. 

    ![Nieuwe build](./media/how-to-ci-cd/add-new-build.png)

1. Als u hierom wordt gevraagd, selecteert u de **Azure DevOps Git** gegevensbrontype, selecteert u het project, de opslagplaats en de vertakking waar uw code zich bevindt. Kies **blijven**.

    ![Selecteer Azure DevOps git](./media/how-to-ci-cd/select-vsts-git.png)

1. In **selecteert u een sjabloon** venster, kiest u **beginnen met een lege proces**.

    ![Start leeg zijn](./media/how-to-ci-cd/start-with-empty.png)

1. Klik op **+** aan de rechterkant van **fase 1** een taak wordt toegevoegd met de fase. Zoek en selecteer **.Net Core**, en klikt u op **toevoegen** deze taak wordt toegevoegd met de fase.

    ![Test voor DotNet](./media/how-to-ci-cd/add-dot-net-core.png)

1. Update de **weergavenaam** naar **dotnet test**, en klik in de **opdracht** vervolgkeuzelijst **testen**. Voeg onder het pad naar de **pad naar projecten**.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![Dotnet test configureren](./media/how-to-ci-cd/dotnet-test.png)

1. Klik op **+** aan de rechterkant van **fase 1** een taak wordt toegevoegd met de fase. Zoek en selecteer **Azure IoT Edge**, en klikt u op **toevoegen** knop **tweemaal** deze taken toevoegen aan de fase.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. In de eerste Azure IoT Edge-taak bijwerken de **weergavenaam** naar **Module bouwen en Push**, en klik in de **actie** vervolgkeuzelijst **maken en pushen van**. In de **Module.json bestand** tekstvak onder het pad aan toe te voegen. Kies vervolgens **Type Containerregister**, zorg ervoor dat u configureert en selecteert u de dezelfde register in uw code. Deze taak bouwt en pushen van alle modules in de oplossing en publiceren naar het containerregister dat u hebt opgegeven. Als uw modules naar verschillende registers gepusht, kunt u meerdere hebben **Module bouwen en Push** taken.

    ```
    **/module.json
    ```

    ![Module-Build- en Push](./media/how-to-ci-cd/module-build-push.png)

1. In de tweede Azure IoT Edge-taak bijwerken de **weergavenaam** naar **implementeren in IoT Edge-apparaat**, en klik in de **actie** vervolgkeuzelijst **implementeren in IoT Edge apparaat**. Selecteer uw Azure-abonnement en voer de naam van uw IoT-Hub. U kunt een IoT Edge-implementatie-ID en de implementatie-prioriteit opgeven. U kunt ook kiezen voor implementatie op één of meerdere apparaten. Als u op meerdere apparaten implementeert, moet u de doelvoorwaarde apparaat opgeven. Bijvoorbeeld, als u wilt de apparaat-labels gebruiken als de voorwaarde, moet u de bijbehorende apparaten Tags voordat de implementatie bij te werken. 

    ![Implementeren naar rand](./media/how-to-ci-cd/deploy-to-edge.png)

1. Klik op de **proces** en zorg ervoor dat uw **Fronta agenta** is **gehost Linux Preview**.

    ![Configureren](./media/how-to-ci-cd/configure-env.png)

1. Open de **Triggers** tabblad en schakelt u de **continue integratie** trigger. Zorg ervoor dat de vertakking met uw code is opgenomen.

    ![Trigger](./media/how-to-ci-cd/configure-trigger.png)

1. Sla de nieuwe build-pijplijn en een nieuwe build in de wachtrij. Klik op de **opslaan en in de wachtrij** knop.

1. Kies de koppeling naar de build in de berichtenbalk die wordt weergegeven. Of Ga naar de pijplijn maken om te zien van de meest recente in de wachtrij build-taak.

    ![Build](./media/how-to-ci-cd/build-def.png)

1. Nadat de build is voltooid, ziet u het overzicht voor elke taak en de resultaten in het live logboekbestand. 
    
    ![Voltooid](./media/how-to-ci-cd/complete.png)

1. U kunt teruggaan naar VS Code en controleren van de IoT Hub apparaat-Verkenner. Het Edge-apparaat met de module moet beginnen met (Zorg ervoor dat u registerreferenties hebt toegevoegd aan de Edge-runtime).

    ![Edge uitgevoerd](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>Continue implementatie voor IoT Edge-apparaten

Om in te schakelen continue implementatie, in feite moet u het instellen van CI-taken met de juiste IoT Edge-apparaten inschakelen de **Triggers** voor uw vertakkingen in uw project. In een klassieke devops bevat een project twee belangrijkste vertakkingen. De master-vertakking moet de stabiele versie van de code en de vertakking ontwikkelen bevat de meest recente code hoeft te wijzigen. Elke ontwikkelaar in het team moet splitsen ontwikkelen vertakking voor zijn of haar eigen functie-vertakking als functie beginnen met het bijwerken van de code, wat betekent dat alle doorvoeringen gebeurt vertakkingen uit de vertakking ontwikkelen. En elke gepushte doorvoer moet worden getest via de CI-systeem. Nadat de code lokaal volledig getest, moet de functie filiaal worden samengevoegd op de vertakking ontwikkelen via een pull-aanvraag. Wanneer de code op de vertakking van de ontwikkelaar is getest via CI-systeem, kan deze kan worden samengevoegd op de master-vertakking via een pull-aanvraag.

Dus wanneer u implementeert op IoT Edge-apparaten, zijn er drie belangrijkste omgevingen.
- U kunt op de functie filiaal, gesimuleerde IoT Edge-apparaat gebruiken op uw ontwikkelcomputer of op een fysieke IoT Edge-apparaat implementeren.
- Ontwikkelen op vertakking, moet u op een fysiek IoT Edge-apparaat implementeren.
- Op de master-vertakking moet het doel IoT Edge-apparaten de productieapparaten.

## <a name="next-steps"></a>Volgende stappen

* Inzicht in de IoT Edge-implementatie in [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md)
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).
