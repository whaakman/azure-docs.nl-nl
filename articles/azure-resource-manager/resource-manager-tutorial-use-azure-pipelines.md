---
title: Continue integratie met Azure-pijplijnen | Microsoft Docs
description: Leer hoe u continu bouwen, testen en implementeren van Azure Resource Manager-sjablonen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: carmonm
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 06/12/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 85dc0476da12bea64610b6910b0682fef00f4b5a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057790"
---
# <a name="tutorial-continuous-integration-of-azure-resource-manager-templates-with-azure-pipelines"></a>Zelfstudie: Continue integratie van Azure Resource Manager-sjablonen met Azure-pijplijnen

Informatie over het gebruik van Azure-pijplijnen doorlopend bouwen en implementeren van sjabloonprojecten voor Azure Resource Manager.

Azure DevOps biedt services ter ondersteuning van teams samen om werk plannen, samenwerken aan code ontwikkelen, en bouw en implementeer toepassingen voor ontwikkelaars. Ontwikkelaars kunnen werken in de cloud met Azure DevOps-Services. Azure DevOps biedt een geïntegreerde set met functies die u via uw webbrowser of IDE-client openen kunt. Azure pijplijn is een van deze functies. Azure pijplijnen is een volledig functionele continue integratie (CI) en continue levering (CD)-service. Het werkt met uw favoriete Git-provider en met de meeste grote cloud-services kunt implementeren. U kunt vervolgens de bouwen, testen en implementeren van uw code op Microsoft Azure, Google Cloud Platform of Amazon Web Services automatiseren.

In deze zelfstudie is ontworpen voor ontwikkelaars van de Azure Resource Manager-sjabloon die nieuwe Azure DevOps-Services en Azure-pijplijnen zijn. Als u al bekend met GitHub en DevOps bent, kunt u doorgaan met [maken van een pijplijn](#create-a-pipeline).

> [!NOTE]
> Kies een naam van het project. Wanneer u de zelfstudie doorloopt, vervangen door een van de **AzureRmPipeline** met de projectnaam van uw.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een GitHub-opslagplaats voorbereiden
> * Een Azure DevOps-project maken
> * Een Azure-pijplijn
> * Controleer of de pijplijn-implementatie
> * Bijwerken van de sjabloon en opnieuw implementeren
> * Resources opschonen

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* **Een GitHub-account**, waar u deze gebruiken om te maken van een opslagplaats voor uw sjablonen. Als u niet hebt, kunt u [maakt u er een gratis](https://github.com). Zie voor meer informatie over het gebruik van GitHub-opslagplaatsen [bouwen GitHub-opslagplaatsen](/azure/devops/pipelines/repos/github).
* **Installeer Git**. Maakt gebruik van deze zelfstudie instructie *Git Bash* of *Git Shell*. Zie voor instructies [Git installeren]( https://www.atlassian.com/git/tutorials/install-git).
* **Een organisatie Azure DevOps**. Als u niet hebt, kunt u een gratis maken. Zie [maken van een organisatie of project verzameling]( https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops).
* **[Visual Studio Code](https://code.visualstudio.com/) met de hulpprogramma's voor Resource Manager-extensie**. Zie [De extensie installeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="prepare-a-github-repository"></a>Een GitHub-opslagplaats voorbereiden

GitHub wordt gebruikt voor het opslaan van de broncode van uw project met inbegrip van Resource Manager-sjablonen. Zie voor andere ondersteunde opslagplaatsen [opslagplaatsen die worden ondersteund door Azure DevOps](/azure/devops/pipelines/repos/?view=azure-devops#supported-repository-types).

### <a name="create-a-github-repository"></a>Een GitHub-opslagplaats maken

Als u geen een GitHub-account hebt, raadpleegt u [vereisten](#prerequisites).

1. Aanmelden bij [GitHub](https://github.com).
2. Selecteer de installatiekopie van uw account in de rechterbovenhoek en selecteer vervolgens **uw opslagplaatsen**.

    ![Azure Resource Manager Azure DevOps Azure pijplijnen maken GitHub-opslagplaats](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-repository.png)

1. Selecteer **nieuw**, een groene knop.
1. In **naam van de opslagplaats**, voer een naam van de opslagplaats.  Bijvoorbeeld, **AzureRmPipeline-opslagplaats**. Vervang alle **AzureRmPipeline** met de projectnaam van uw. U kunt een selecteren **openbare** of **persoonlijke** voor deze zelfstudie te doorlopen. En selecteer vervolgens **opslagplaats maken**.
1. Noteer de URL. De opslagplaats-URL is de volgende indeling:

    ```url
    https://github.com/[YourAccountName]/[YourRepositoryName]
    ```

Deze opslagplaats wordt aangeduid als een *-opslagplaats voor externe*. Elk van de ontwikkelaars van hetzelfde project kunt klonen zijn of haar eigen *lokale opslagplaats*, en de wijzigingen naar de externe opslagplaats samen te voegen.

### <a name="clone-the-remote-repository"></a>De externe opslagplaats klonen

1. Open de Git Shell of Git Bash.  Zie [Vereisten](#prerequisites).
1. Controleer of de huidige map **github**.
1. Voer de volgende opdracht uit:

    ```bash
    git clone https://github.com/[YourAccountName]/[YourGitHubRepositoryName]
    cd [YourGitHubRepositoryName]
    mkdir CreateAzureStorage
    cd CreateAzureStorage
    pwd
    ```

    Vervang **[YourAccountName]** met uw GitHub-accountnaam en vervang **[YourGitHubRepositoryName]** met de naam van uw opslagplaats die u in de vorige procedure hebt gemaakt.

    De volgende schermafbeeldingen toont een voorbeeld.

    ![Azure Resource Manager-Azure DevOps Azure-pijplijnen maken GitHub bash](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-bash.png)

De **CreateAzureStorage** is de map waar de sjabloon is opgeslagen. De **pwd** opdracht geeft u het mappad. Het pad is waar u de sjabloon die u wilt opslaan in de volgende procedure.

### <a name="download-a-quickstart-template"></a>Een Quickstart-sjabloon downloaden

In plaats van het maken van een sjabloon, kunt u een [Quickstart-sjabloon]( https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json). Deze sjabloon maakt een Azure Storage-account.

1. Open Visual Studio code. Zie [Vereisten](#prerequisites).
2. Hiermee opent u de sjabloon met de volgende URL:

    ```URL
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Sla het bestand op als **azuredeploy.json** naar de **CreateAzureStorage** map. Zowel de naam van de map en de naam van het bestand worden gebruikt zoals in de pijplijn.  Als u deze namen wijzigt, moet u de namen die worden gebruikt in de pijplijn bijwerken.

### <a name="push-the-template-to-the-remote-repository"></a>De sjabloon naar de externe opslagplaats pushen

De azuredeploy.json is toegevoegd aan de lokale opslagplaats. U kunt vervolgens de sjabloon uploaden naar de externe opslagplaats.

1. Open *Git Shell* of *Git Bash*, als deze niet is geopend.
1. Wijzig de map naar de map CreateAzureStorage in uw lokale opslagplaats.
1. Controleer of de **azuredeploy.json** bestand is in de map.
1. Voer de volgende opdracht uit:

    ```bash
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    U kunt een waarschuwing over LF krijgen. U kunt de waarschuwing negeren. **master** is van de master-vertakking.  U wordt doorgaans een vertakking voor elke update. Ter vereenvoudiging van de zelfstudie gebruikt u de master-vertakking rechtstreeks.
1. Blader naar uw GitHub-opslagplaats vanuit een browser.  De URL is  **https://github.com/ [YourAccountName] / [YourGitHubRepository]** . U ziet de **CreateAzureStorage** map en **Azuredeploy.json** in de map.

Tot nu toe hebt u een GitHub-opslagplaats gemaakt en een sjabloon geüpload naar de opslagplaats.

## <a name="create-a-devops-project"></a>Een DevOps-project maken

Er is een DevOps-organisatie nodig voordat u verder kunt gaan met de volgende procedure.  Als u niet hebt, raadpleegt u [vereisten](#prerequisites).

1. Aanmelden bij [Azure DevOps](https://dev.azure.com).
1. Selecteer een DevOps-organisatie aan de linkerkant.

    ![Azure Resource Manager Azure DevOps Azure pijplijnen maken Azure DevOps-project](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-create-devops-project.png)

1. Selecteer **Project maken**. Als u geen projecten, wordt de pagina voor het project maken automatisch geopend.
1. Voer de volgende waarden in:

    * **Naam van het project**: Voer een naam van het project. U kunt de naam van het project dat u aan het begin van de zelfstudie hebt opgehaald.
    * **Versiebeheer**: Selecteer **Git**. U moet mogelijk om uit te breiden **Geavanceerd** om te zien **versiebeheer**.

    Gebruik de standaardwaarde voor de overige eigenschappen.
1. Selecteer **Project maken**.

Maak een serviceverbinding die wordt gebruikt voor het implementeren van projecten in Azure.

1. Selecteer **projectinstellingen** vanaf de onderkant van het menu links.
1. Selecteer **verbindingen** onder **pijplijnen**.
1. Selecteer **nieuwe serviceverbinding**, en selecteer vervolgens **AzureResourceManager**.
1. Voer de volgende waarden in:

    * **Verbindingsnaam**: Voer de naam van een verbinding. Bijvoorbeeld, **AzureRmPipeline-connectiviteit**. Noteer deze naam, moet u de naam bij het maken van uw pijplijn.
    * **Bereik niveau**: Selecteer **abonnement**.
    * **Abonnement**: Selecteer uw abonnement.
    * **Resourcegroep**: Laat dit leeg.
    * **Alle die leiden tot deze verbinding gebruiken toestaan**. (geselecteerd)
1. Selecteer **OK**.

## <a name="create-a-pipeline"></a>Een pijplijn maken

Tot nu toe als u de volgende taken hebt voltooid.  Als u de vorige secties overslaan omdat u bekend met GitHub en DevOps bent, moet u de taken uitvoeren voordat u doorgaat.

- Maak een GitHub-opslagplaats en sla [deze sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) naar de **CreateAzureStorage** map in de opslagplaats.
- Maken van een DevOps-project en maak een Azure Resource Manager serviceverbinding.

Een pijplijn maken met een stap om een sjabloon te implementeren:

1. Selecteer **pijplijnen** in het menu links.
1. Selecteer **nieuwe pijplijn**.
1. Uit de **Connect** tabblad **GitHub**. Als u wordt gevraagd, voert u uw GitHub-referenties en volg de instructies. Als u het volgende scherm ziet, selecteert u **selecteert alleen opslagplaatsen**, en controleer of uw opslagplaats is in de lijst voordat u **goedkeuren en installeren**.

    ![Azure Resource Manager Azure DevOps Azure pijplijnen alleen selecteren opslagplaatsen](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-only-select-repositories.png)

1. Uit de **Selecteer** tabblad, selecteert u de opslagplaats.  De standaardnaam is **[YourAccountName] / [YourGitHubRepositoryName]** .
1. Uit de **configureren** tabblad **Starter pijplijn**. Hier ziet u de **azure pipelines.yml** pijplijn-bestand met twee scriptstappen.
1. Vervang de **stappen** sectie met de volgende YAML:

    ```yaml
    steps:
    - task: AzureResourceGroupDeployment@2
      inputs:
        azureSubscription: '[YourServiceConnectionName]'
        action: 'Create Or Update Resource Group'
        resourceGroupName: '[EnterANewResourceGroupName]'
        location: 'Central US'
        templateLocation: 'Linked artifact'
        csmFile: 'CreateAzureStorage/azuredeploy.json'
        deploymentMode: 'Incremental'
    ```

    Het moet er als volgt uitzien:

    ![Azure Resource Manager Azure DevOps Azure pijplijnen yaml](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-yml.png)

    Breng de volgende wijzigingen aan:

    * **Azure-abonnement**: werkt de waarde bij met de serviceverbinding in de vorige procedure hebt gemaakt.
    * **actie**: de **maken of bijwerken resourcegroep** actie heeft 2 acties - 1. Maak een resourcegroep als de naam van een nieuwe resourcegroep wordt geleverd; 2. Implementeer de sjabloon die is opgegeven.
    * **resourceGroupName**: Geef de naam van een nieuwe resourcegroep. Bijvoorbeeld, **AzureRmPipeline-rg**.
    * **locatie**: Geef de locatie voor de resourcegroep.
    * **templateLocation**: wanneer **gekoppelde artefact** is opgegeven, wordt de taak wordt gekeken naar het sjabloonbestand rechtstreeks vanuit de opslagplaats met verbonden.
    * **csmFile** is het pad naar het sjabloonbestand. U hoeft niet te geven van een bestand met Sjabloonparameters omdat alle van de gedefinieerde parameters in de sjabloon standaardwaarden hebben.

    Zie voor meer informatie over de taak [Azure Resource Group Deployment-taak](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment)
1. Selecteer **Opslaan en uitvoeren**.
1. Selecteer **opslaan en uitvoeren** opnieuw. Een kopie van het YAML-bestand wordt opgeslagen in de opslagplaats van verbonden. U kunt het YAML-bestand bekijken door te bladeren naar uw opslagplaats.
1. Controleer of dat de pijplijn met succes is uitgevoerd.

    ![Azure Resource Manager Azure DevOps Azure pijplijnen yaml](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-status.png)

## <a name="verify-the-deployment"></a>De implementatie controleren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Open de resourcegroep. De naam is wat u hebt opgegeven in de pijplijn YAML-bestand.  U ziet één opslagaccount gemaakt.  Naam van het opslagaccount wordt gestart met **opslaan**.
1. Selecteer de naam van het opslagaccount om deze te openen.
1. Selecteer **eigenschappen**. U ziet dat de **SKU** is **Standard_LRS**.

    ![Azure Resource Manager Azure DevOps Azure pijplijnen portal verificatie](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-portal-verification.png)

## <a name="update-and-redeploy"></a>Bijwerken en opnieuw implementeren

Wanneer u de sjabloon bijwerken en de wijzigingen naar de externe opslagplaats forceren, werkt de pijplijn automatisch de resources, het storage-account in dit geval.

1. Open **azuredeploy.json** vanuit uw lokale opslagplaats in Visual Studio Code.
1. Update de **defaultValue** van **Opslagaccounttype** naar **Standard_GRS**. Zie de volgende schermafbeelding:

    ![Azure Resource Manager Azure DevOps Azure pijplijnen bijwerken yaml](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-update-yml.png)

1. Sla de wijzigingen op.
1. De wijzigingen naar de externe opslagplaats forceren door het uitvoeren van de volgende opdrachten vanuit Git Bash/Shell.

    ```bash
    git pull origin master
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    De eerste opdracht wordt de lokale opslagplaats met de externe opslagplaats gesynchroniseerd. Vergeet niet dat de pijplijn YAML-bestand is toegevoegd aan de externe opslagplaats.

    Met de master-vertakking van de externe opslagplaats bijgewerkt, wordt de pijplijn opnieuw geactiveerd.

Als u wilt controleren of de wijzigingen, kunt u de SKU van het storage-account controleren.  Zie [controleren of de implementatie](#verify-the-deployment).

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

U kunt ook de GitHub-opslagplaats en het Azure DevOps-project te verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Azure DevOps-pijplijn voor het implementeren van een Azure Resource Manager-sjabloon. Voor meer informatie over hoe u Azure-resources implementeert in meerdere regio's en hoe u veilige implementatiemethoden gebruikt, raadpleegt u

> [!div class="nextstepaction"]
> [Azure Deployment Manager gebruiken](./resource-manager-tutorial-deploy-vm-extensions.md)
