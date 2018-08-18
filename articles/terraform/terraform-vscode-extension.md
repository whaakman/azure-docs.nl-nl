---
title: Azure Terraform VS Code-extensie | Microsoft Docs
description: In dit artikel leert u hoe u wilt installeren en gebruiken van de extensie Terraform in Visual Studio Code.
keywords: terraform, devops, virtuele machine, azure
author: v-mavick
ms.author: v-mavick
ms.date: 08/14/2018
ms.topic: article
ms.prod: vs-code
ms.openlocfilehash: 0c88879faae100372055479ad4edb8c36d8f557d
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190131"
---
# <a name="azure-terraform-vs-code-extension"></a>Azure Terraform VS Code-extensie

De Microsoft Azure Terraform Visual Studio Code (VS-Code)-extensie is ontworpen voor het verhogen van productiviteit van ontwikkelaars bij het ontwerpen, testen en met behulp van Terraform met Azure. De extensie biedt opdrachtondersteuning Terraform, graph resourcevisualisatie en CloudShell integratie binnen de VS Code.

## <a name="what-you-do"></a>Wat u allemaal doen

- Het open-source HashiCorp Terraform uitvoerbare bestand op uw computer installeren.
- Installeer de Terraform VS Code-extensie voor Azure op uw lokale installatie van de VS Code.

## <a name="what-you-learn"></a>Wat u leert

In deze zelfstudie leert u:

- Hoe Terraform kunt automatiseren en vereenvoudigen het inrichten van Azure-services.
- Het installeren en gebruiken van de Microsoft Terraform VS Code-extensie voor Azure-services.
- Het gebruik van VS Code te schrijven, plannen en uitvoeren van Terraform plannen.

## <a name="what-you-need"></a>Wat u nodig hebt

- Een computer met Windows 10, Linux of macOS 10.10 +.
- [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US).
- Een actief Azure-abonnement. [Activeren van een gratis 30-daagse proefversie Microsoft Azure-account](https://azure.microsoft.com/free/).
- Een installatie van de [Terraform](https://www.terraform.io/) open-source-hulpprogramma op uw lokale computer.
  
## <a name="prepare-your-dev-environment"></a>Uw ontwikkelomgeving voorbereiden

### <a name="install-git"></a>Git installeren

Als u wilt de oefeningen in het artikel hebt voltooid, moet u [Installeer Git](https://git-scm.com/).

### <a name="install-hashicorp-terraform"></a>Terraform van HashiCorp installeren

Volg de instructies op de HashiCorp [Terraform installeren](https://www.terraform.io/intro/getting-started/install.html) webpagina's worden behandeld:

- Downloaden van Terraform
- Terraform installeren
- Terraform controleren is correct geïnstalleerd

>[!Tip]
>Zorg ervoor dat u de instructies met betrekking tot het instellen van de variabele PATH system.

### <a name="install-nodejs"></a>Node.js installeren

Voor het gebruik van Terraform in de Cloud Shell, moet u [Installeer Node.js](https://nodejs.org/) 6.0 en hoger.

>[!NOTE]
>Om te controleren of Node.js is geïnstalleerd, opent u een terminal-venster en typ: `node -v`

### <a name="install-graphviz"></a>GraphViz installeren

Gebruik de Terraform functie visualiseren, moet u [installeren GraphViz](http://graphviz.org/).

>[!NOTE]
>Om te controleren of GraphViz is geïnstalleerd, opent u een terminal-venster en typ: `dot -V`

### <a name="install-the-azure-terraform-vs-code-extension"></a>De Azure Terraform VS Code-extensie installeren:

1. Start VS Code.
2. Klik op de **extensies** pictogram.

    ![Knop voor extensies](media/terraform-vscode-extension/tf-vscode-extensions-button.png)

3. Gebruik de **uitbreidingen voor zoeken in Marketplace** in het tekstvak om te zoeken naar de Azure Terraform-extensie:

    ![VS Code-extensies zoeken in Marketplace](media/terraform-vscode-extension/tf-search-extensions.png)

4. Klik op **Install**.

    >[!NOTE]
    >Wanneer u klikt op **installeren** voor de extensie van Azure Terraform, VS Code automatisch de Azure-accountextensie wordt geïnstalleerd. Azure-Account is een afhankelijkheidsbestand voor de Azure Terraform-extensie, die wordt gebruikt voor het uitvoeren van verificaties van Azure-abonnement en Azure-gerelateerde code-extensies.

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Controleer of dat de Terraform-extensie is geïnstalleerd in Visual Studio Code

1. Klik op het pictogram extensies.
2. Type `@installed` in het zoekvak.

    ![Geïnstalleerde extensies](media/terraform-vscode-extension/tf-installed-extensions.png)

De Azure Terraform-extensie wordt weergegeven in de lijst met geïnstalleerde extensies.

![Geïnstalleerde extensies van Terraform](media/terraform-vscode-extension/tf-installed-terraform-extension-button.png)

U kunt nu alle ondersteunde Terraform-opdrachten uitvoeren in uw Cloud Shell-omgeving van VS Code.

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>Oefening 1: Basic Terraform opdrachten overzicht

In deze oefening hebt u maken en uitvoeren van een eenvoudige Terraform-configuratiebestand dat een nieuwe Azure-resourcegroep inricht.

### <a name="prepare-a-test-plan-file"></a>Een plan testbestand voorbereiden

1. Selecteer in VS Code, **bestand > nieuw bestand** in de menubalk.
2. Navigeer naar de [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#) en kopieer de code in de **voorbeeld** codeblok:
3. Plak de gekopieerde code in het nieuwe bestand dat u hebt gemaakt in VS Code.

    ![Plak de code voorbeeld](media/terraform-vscode-extension/tf-paste-example-usage-code.png)

    >[!NOTE]
    >U kunt wijzigen de **naam** waarde van de resourcegroep, maar deze moet uniek zijn voor uw Azure-abonnement.

4. Selecteer in de menubalk **bestand > OpslaanAls**.
5. In de **OpslaanAls** dialoogvenster, navigeer naar de locatie van uw keuze en klik vervolgens op **nieuwe map**. (De naam van de nieuwe map wijzigen in een meer beschrijvende dan *nieuwe map*.)

    >[!NOTE]
    >In dit voorbeeld heet de map TERRAFORM-TEST-abonnement.

6. Zorg ervoor dat de nieuwe map is gemarkeerd (geselecteerd) en klik vervolgens op **Open**.
7. In de **OpslaanAls** dialoogvenster, wijzigt u de standaardnaam van het bestand moet *main.tf*.

    ![Opslaan als main.tf](media/terraform-vscode-extension/tf-save-as-main.png)

8. Klik op **Opslaan**.
- Selecteer in de menubalk **bestand > map openen**. Navigeer naar en selecteer de nieuwe map die u hebt gemaakt.

### <a name="run-terraform-init-command"></a>Uitvoeren van Terraform *init* opdracht

1. Start Visual Studio Code.
2. Selecteer in de menubalk van VS Code **bestand > map openen...**  en zoek en selecteer uw *main.tf* bestand.

    ![main.TF bestand](media/terraform-vscode-extension/tf-main-tf.png)

3. Selecteer in de menubalk **weergave > Command Palette... > Azure Terraform: Init**.
4. Na enkele ogenblikken wanneer u wordt gevraagd *wilt u Cloud Shell openen?* Klik op **OK**.

    ![Wilt u Cloud Shell openen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloud-shell.png)

5. De eerste keer dat u Cloud Shell via een nieuwe map, wordt u gevraagd voor het instellen van de web-App. Klik op **Open**.

    ![Eerste keer opstarten van Cloud Shell](media/terraform-vscode-extension/tf-first-launch-of-cloud-shell.png)

6. De pagina Welkom bij Azure Cloud Shell wordt geopend. Selecteer Bash of PowerShell.

    ![Welkom bij Azure Cloud Shell](media/terraform-vscode-extension/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >In dit voorbeeld is Bash (Linux) geselecteerd.

7. Als u een Azure storage-account nog niet hebt ingesteld, wordt het volgende scherm wordt weergegeven. Klik op **maken van de**.

    ![U hebt geen opslag gekoppeld](media/terraform-vscode-extension/tf-you-have-no-storage-mounted.png)

1. Azure Cloud Shell wordt gestart in de shell die u eerder hebt geselecteerd en geeft informatie weer voor het station van de cloud het zojuist hebt gemaakt voor u.

    ![Uw cloud-station is gemaakt](media/terraform-vscode-extension/tf-your-cloud-drive-has-been-created-in.png)

9. U mag de Cloud Shell nu afsluit
10. Selecteer in de menubalk **weergave** > **Command Palette** > **Azure Terraform: init**.

    ![Terraform is geïnitialiseerd](media/terraform-vscode-extension/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>Het plan visualiseren

Eerder in deze zelfstudie hebt u GraphViz geïnstalleerd. Terraform kunt GraphViz gebruiken voor het genereren van een visuele representatie van een configuratie of de uitvoering van plan. De Azure Terraform VS Code-extensie wordt geïmplementeerd voor deze functie via de *visualiseren* opdracht.

- Uit de **in de menubalk**, selecteer**weergave > Command Palette > Azure Terraform: visualiseren**.

    ![Het plan visualiseren](media/terraform-vscode-extension/tf-graph.png)

### <a name="run-terraform-plan-command"></a>Uitvoeren van Terraform *plan* opdracht

De Terraform *plan* opdracht wordt gebruikt om te controleren of het abonnement kan worden uitgevoerd voor een set wijzigingen gaan doen wat u bedoeld.

>[!NOTE]
>Terraform *plan* geen wijzigingen aanbrengen aan uw werkelijke Azure-resources. Om daadwerkelijk de wijzigingen die zijn opgenomen in uw abonnement, gebruiken we de Terraform *toepassen* opdracht.

- Selecteer in de menubalk **weergave** > **Command Palette** > **Azure Terraform: plan**.

    ![Terraform-plan](media/terraform-vscode-extension/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>Uitvoeren van Terraform *toepassen* opdracht

Nadat u hebt voldaan wordt aan met de resultaten van Terraform *plan*, kunt u uitvoeren de *toepassen* opdracht.

1. Selecteer in de menubalk **weergave** > **Command Palette** > **Azure Terraform: toepassing**.

    ![Terraform toepassen](media/terraform-vscode-extension/tf-terraform-apply.png)

2. Type *Ja*.

    ![Terraform Ja toepassen](media/terraform-vscode-extension/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>Controleer of dat uw abonnement Terraform is uitgevoerd

Om te zien als uw nieuwe Azure-resourcegroep is gemaakt:

1. Open Azure Portal.
2. Selecteer **resourcegroepen** in het navigatiedeelvenster links.

    ![Controleer of de nieuwe resource](media/terraform-vscode-extension/tf-verify-resource-group-created.png)

Uw nieuwe resourcegroep moet worden vermeld de **naam** kolom.

>[!NOTE]
>U kan uw Azure-Portal-venster geopend laten voorlopig; We gebruiken deze in de volgende stap.

### <a name="run-terraform-destroy-command"></a>Uitvoeren van Terraform *vernietigen* opdracht

1. Selecteer in de menubalk **weergave** > **Command Palette** > **Azure Terraform: vernietigen**.

    ![Terraform vernietigen](media/terraform-vscode-extension/tf-terraform-destroy.png)

2. Type *Ja*.

    ![Terraform vernietigen Ja](media/terraform-vscode-extension/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>Controleer of dat de resourcegroep is verwijderd

Om te bevestigen dat Terraform is vernietigd uw nieuwe resourcegroep:

1. Klik op **vernieuwen** in Azure portal *resourcegroepen* pagina.
2. De resourcegroep wordt niet meer worden weergegeven.

    ![Controleer of de resourcegroep is verwijderd](media/terraform-vscode-extension/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>Oefening 2: Terraform *compute* module

In deze oefening leert u hoe u de Terraform laden *compute* module in de VS Code-omgeving.

### <a name="clone-the-terraform-azurerm-compute-module"></a>Kloon de terraform-azurerm-compute-module

1. Gebruik [deze koppeling](https://github.com/Azure/terraform-azurerm-compute) voor toegang tot de module Terraform Azure Rm Compute op GitHub.
2. Klik op **klonen of downloaden**.

    ![Klonen of downloaden](media/terraform-vscode-extension/tf-clone-with-https.png)

    >[!NOTE]
    >In dit voorbeeld wordt de map heette *terraform-azurerm-compute*.

### <a name="open-the-folder-in-vs-code"></a>Open de map in VS Code

1. Start Visual Studio Code.
2. Uit de **in de menubalk**, selecteer **bestand > map openen** en navigeer naar en selecteer de map die u in de vorige stap hebt gemaakt.

    ![terraform-azurerm-compute-map](media/terraform-vscode-extension/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Initialiseren van Terraform

Voordat u kunt met behulp van de Terraform-opdrachten uit in de VS Code, u de plug-ins voor twee Azure-providers downloaden: willekeurige en azurerm.

1. In het deelvenster Terminal van VS Code IDE, type: `terraform init`

    ![terraform init opdracht](media/terraform-vscode-extension/tf-terraform-init-command.png)

2. Type `az login` en volg de aanwijzingen op het scherm instructies.

### <a name="module-test-lint"></a>Module test: *regel*

1. Uit de **in de menubalk**, selecteer **weergave > Command Palette > Azure Terraform: Test uitvoeren**.
2. Selecteer in de lijst met opties voor het type test, **regel**.

    ![Type van de test selecteren](media/terraform-vscode-extension/tf-select-type-of-test-lint.png)

3. Wanneer u wordt gevraagd *doen die u wilt openen CloudShell?* klikt u op **OK** en volg de aanwijzingen op het scherm instructies.

    ![Wilt u CloudShell openen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Wanneer u een uitvoert de **regel** of **end-to-end** testen, gebruikt Azure containerservice voor het inrichten van een testmachine om uit te voeren van de werkelijke test. Om deze reden duurt de testresultaten doorgaans enkele minuten moeten worden geretourneerd.

Na enkele ogenblikken ziet u een vermelding in de Terminal deelvenster vergelijkbaar met het volgende voorbeeld:

![De resultaten van regel](media/terraform-vscode-extension/tf-lint-test-results.png)

### <a name="module-test-end-to-end"></a>Module test: *end-to-end*

1. Uit de **in de menubalk**, selecteer **weergave > Command Palette > Azure Terraform: Test uitvoeren**.
2. Selecteer in de lijst met opties voor het type test, **end-to-end**.

    ![Type van de test selecteren](media/terraform-vscode-extension/tf-select-type-of-test-end-to-end.png)

3. Als u wordt gevraagd *doen die u wilt openen CloudShell?* klikt u op **OK** en volg de aanwijzingen op het scherm instructies.

    ![Wilt u CloudShell openen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Wanneer u een uitvoert de **regel** of **end-to-end** testen, gebruikt Azure containerservice voor het inrichten van een testmachine om uit te voeren van de werkelijke test. Om deze reden duurt de testresultaten doorgaans enkele minuten moeten worden geretourneerd.

Na enkele ogenblikken ziet u een vermelding in de Terminal deelvenster vergelijkbaar met het volgende voorbeeld:

![De resultaten van end-to-end](media/terraform-vscode-extension/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>Volgende stappen

U hebt gezien dat enkele van de manieren die terraform kunt vereenvoudigen de inrichting van Azure-services vanuit Visual Studio Code. U kunt nu, Bekijk enkele van deze resources:
- De [Terraform-Moduleregister](https://registry.terraform.io/) geeft een lijst van alle beschikbare Terraform-modules voor Azure en andere ondersteunde providers.

Voor elk van deze modules krijgt u de volgende informatie:

- Een beschrijving van de algemene mogelijkheden van de module en de bijbehorende kenmerken
- Een voorbeeld van gebruik
- Test configuraties die laten hoe u kunt maken zien, uitvoeren en testen van elke module op uw lokale dev-machine
- Een docker-bestand waarmee u kunt bouwen en lokaal uitvoeren van een module-ontwikkelomgeving.
