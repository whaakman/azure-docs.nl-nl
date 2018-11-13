---
title: CI/CD-pijplijn instellen met de build-taak van Azure Cosmos DB Emulator
description: Zelfstudie over het instellen van build- en releasewerkstroom in Azure DevOps met behulp van de build-taak van Cosmos DB Emulator
services: cosmos-db
keywords: Azure Cosmos DB Emulator
author: deborahc
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 11/02/2018
ms.author: dech
ms.openlocfilehash: 782975cfa548d214515761e45b8f79a2219831e2
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036968"
---
# <a name="set-up-a-cicd-pipeline-with-the-azure-cosmos-db-emulator-build-task-in-azure-devops"></a>Een CI/CD-pijplijn instellen met de build-taak van Azure Cosmos DB Emulator in Azure DevOps

Azure Cosmos DB Emulator biedt een lokale omgeving waarin de Azure Cosmos DB-service wordt geëmuleerd voor ontwikkelingsdoeleinden. Met behulp van de emulator kunt u uw toepassing lokaal ontwikkelen en testen, kosteloos en zonder een Azure-abonnement te maken. 

Met de build-taak van Azure Cosmos DB Emulator voor Azure DevOps kunt u hetzelfde doen in een CI-omgeving. Gebruik de build-taak voor het uitvoeren van tests met de emulator als onderdeel van uw build- en releasewerkstromen. De taak initialiseer een Docker-container waarin de emulator al wordt uitgevoerd en biedt een eindpunt dat kan worden gebruikt door de rest van de build-definitie. U kunt zo veel instanties van de emulator maken en starten als u nodig hebt. Elke instantie wordt uitgevoerd in een afzonderlijke container. 

In dit artikel leert u hoe u in Azure DevOps een CI-pijplijn instelt voor een ASP.NET-toepassing die de build-taak van Cosmos DB Emulator gebruikt voor het uitvoeren van tests. 

## <a name="install-the-emulator-build-task"></a>Build-taak van emulator installeren

U kunt de build-taak pas gebruiken als u deze eerst installeert in uw Azure DevOps-organisatie. Zoek de extensie **Azure Cosmos DB Emulator** in de [Marketplace](https://marketplace.visualstudio.com/items?itemName=azure-cosmosdb.emulator-public-preview) en klik op **Get it free.**

![Build-taak van Azure Cosmos DB Emulator zoeken en installeren in de Marketplace van Azure DevOps](./media/tutorial-setup-ci-cd/addExtension_1.png)

Kies vervolgens de organisatie waarin u de extensie installeren. 

> [!NOTE]
> Als u een extensie wilt installeren in een Azure DevOps-organisatie, moet u een accounteigenaar zijn of een beheerder van een projectverzameling. Als u niet bevoegd bent, maar wel lid bent van het account, kunt u extensies aanvragen. [Meer informatie.](https://docs.microsoft.com/azure/devops/marketplace/faq-extensions?view=vsts#install-request-assign-and-access-extensions)

![Een Azure DevOps-organisatie kiezen waarin u een extensie wilt installeren](./media/tutorial-setup-ci-cd/addExtension_2.png)

## <a name="create-a-build-definition"></a>Een build-definitie maken

Nu de extensie is geïnstalleerd, meldt u zich aan bij uw Azure DevOps-account en vindt u uw project vanuit het projectendashboard. U kunt een [build-pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav) aan uw project toevoegen of een bestaande build-pipeline wijzigen. Als u al een build-pipeline hebt, kunt u verdergaan met[Build-taak van de emulator toevoegen aan een build-definitie](#addEmulatorBuildTaskToBuildDefinition).

1. Om een nieuwe build-definitie te maken, gaat u naar het tabblad **Builds** in Azure DevOps. Selecteer **+New.** > **Nieuwe build-pipeline**

   ![Een nieuwe build-pipeline maken](./media/tutorial-setup-ci-cd/CreateNewBuildDef_1.png)

2. Selecteer de gewenste **bron**, **teamproject**, **opslagplaats**, en de **standaardvertakking voor handmatige en geplande builds**. Nadat u vereiste opties hebt gekozen, selecteert u **Doorgaan**

   ![Selecteer het teamproject, de opslagplaats en de vertakking voor de build-pipeline ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_2.png)

3. Selecteer ten slotte de gewenste sjabloon voor de build-pipeline. We selecteren de sjabloon **ASP.NET** voor deze zelfstudie. 

We hebben nu een build-pipeline die we kunnen instellen voor gebruik van de build-taak van Azure Cosmos DB Emulator. 

## <a name="addEmulatorBuildTaskToBuildDefinition"></a>De taak toevoegen aan een build-pipeline

1. Voordat u een taak toevoegt aan de build-pipeline, moet u een agenttaak toevoegen. Navigeer naar uw build-pipeline, selecteer **...** en kies **Een agenttaak toevoegen**.

1. Selecteer vervolgens het symbool **+** naast de agenttaak om de emulator-build-taak toe te voegen. Zoek naar **cosmos** in het zoekvak, selecteer **Azure Cosmos DB Emulator** en voeg deze toe aan de agent-taak. De build-taak start een container waarop al een exemplaar van de Cosmos DB-emulator wordt uitgevoerd. De Azure Cosmos DB Emulator-taak moet vóór alle andere taken worden geplaatst die ervan uitgaan dat de emulator wordt uitgevoerd.

   ![De Emulator-build-taak toevoegen aan de build-definitie](./media/tutorial-setup-ci-cd/addExtension_3.png)

In deze zelfstudie voegt u de taak toe aan het begin om ervoor te zorgen dat de emulator beschikbaar is voordat onze tests worden uitgevoerd.

## <a name="configure-tests-to-use-the-emulator"></a>Tests configureren voor gebruik van de emulator

Nu gaan we onze tests configureren voor het gebruik van de emulator. De build-taak van de emulator exporteert een omgevingsvariabele, 'CosmosDbEmulator.Endpoint', waarnaar taken verderop in de build-pijplijn eventueel aanvragen kunnen versturen. 

In deze zelfstudie gebruiken we de [taak VSTest](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/VsTestV2/README.md) om moduletests uit te voeren die zijn geconfigureerd via een **.runsettings**-bestand. Raadpleeg de [documentatie](https://docs.microsoft.com/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017) voor meer informatie over het configureren van de moduletests.

Hieronder ziet u een voorbeeld van een **.runsettings**-bestand met parameters die moeten worden doorgegeven aan de moduletests van een toepassing. De gebruikte variabele `authKey` is de [bekende sleutel](https://docs.microsoft.com/azure/cosmos-db/local-emulator#authenticating-requests) voor de emulator. `authKey` is de sleutel die wordt verwacht door de build-taak en deze moet dan ook zijn gedefinieerd in uw **.runsettings**-bestand.

```csharp
<RunSettings>
    <TestRunParameters>
    <Parameter name="endpoint" value="https://localhost:8081" />
    <Parameter name="authKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" />
    <Parameter name="database" value="ToDoListTest" />
    <Parameter name="collection" value="ItemsTest" />
  </TestRunParameters>
</RunSettings>
```

Via de eigenschap `TestContext` in het testproject van de toepassing wordt er verwezen naar de parameters bij `TestRunParameters`. Hier volgt een voorbeeld van een test die wordt uitgevoerd met Cosmos DB.

```csharp
namespace todo.Tests
{
    [TestClass]
    public class TodoUnitTests
    {
        public TestContext TestContext { get; set; }

        [TestInitialize()]
        public void Initialize()
        {
            string endpoint = TestContext.Properties["endpoint"].ToString();
            string authKey = TestContext.Properties["authKey"].ToString();
            Console.WriteLine("Using endpoint: ", endpoint);
            DocumentDBRepository<Item>.Initialize(endpoint, authKey);
        }
        [TestMethod]
        public async Task TestCreateItemsAsync()
        {
            var item = new Item
            {
                Id = "1",
                Name = "testName",
                Description = "testDescription",
                Completed = false,
                Category = "testCategory"
            };

            // Create the item
            await DocumentDBRepository<Item>.CreateItemAsync(item);
            // Query for the item
            var returnedItem = await DocumentDBRepository<Item>.GetItemAsync(item.Id, item.Category);
            // Verify the item returned is correct.
            Assert.AreEqual(item.Id, returnedItem.Id);
            Assert.AreEqual(item.Category, returnedItem.Category);
        }

        [TestCleanup()]
        public void Cleanup()
        {
            DocumentDBRepository<Item>.Teardown();
        }
    }
}
```

Navigeer naar de uitvoeringsopties in de taak VSTest. Geef bij **Settings file** aan dat de tests worden geconfigureerd via een **.runsettings**-bestand. Voeg ` -endpoint $(CosmosDbEmulator.Endpoint)` toe voor de optie **Override test run parameters**. Hiermee stelt u in dat de testtaak moet verwijzen naar het eindpunt van de build-taak van de emulator, en niet naar het eindpunt dat is gedefinieerd in het **.runsettings**-bestand.  

![Variabele voor eindpunt vervangen door eindpunt van build-taak van emulator](./media/tutorial-setup-ci-cd/addExtension_5.png)

## <a name="run-the-build"></a>De build uitvoeren

Nu gaan we de build **Opslaan en in de wachtrij zetten**. 

![De build opslaan en uitvoeren](./media/tutorial-setup-ci-cd/runBuild_1.png)

Als de build is gestart, ziet u dat de taak van Cosmos DB Emulator de Docker-installatiekopie gaat downloaden met de emulator geïnstalleerd. 

![De build opslaan en uitvoeren](./media/tutorial-setup-ci-cd/runBuild_4.png)

Als de build is voltooid, kijkt u of de tests zijn gelukt, en of ze allemaal vanuit de build-taak zijn uitgevoerd met de Cosmos DB Emulator.

![De build opslaan en uitvoeren](./media/tutorial-setup-ci-cd/buildComplete_1.png)

## <a name="next-steps"></a>Volgende stappen

Zie [De Azure Cosmos DB Emulator gebruiken voor lokaal ontwikkelen en testen](https://docs.microsoft.com/azure/cosmos-db/local-emulator) voor meer informatie over het gebruik van de emulator voor lokaal ontwikkelen en testen.

Zie [De Azure Cosmos DB Emulator-certificaten exporteren voor gebruik met Java, Python en Node.js](https://docs.microsoft.com/azure/cosmos-db/local-emulator-export-ssl-certificates) als u SSL-certificaten van de emulator wilt exporteren.
