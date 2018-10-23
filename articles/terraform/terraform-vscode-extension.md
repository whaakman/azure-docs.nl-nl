---
title: De Azure Terraform Visual Studio Code-extensie installeren en gebruiken
description: Lees hoe u de Azure Terraform-extensie in Visual Studio Code installeert en gebruikt.
services: terraform
ms.service: terraform
keywords: terraform, azure, devops, visual studio code, extensie
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/31/2018
ms.openlocfilehash: be0140606302335884d62ba60c875a049949b495
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353047"
---
# <a name="install-and-use-the-azure-terraform-visual-studio-code-extension"></a>De Azure Terraform Visual Studio Code-extensie installeren en gebruiken

De Microsoft Azure Terraform Visual Studio Code-extensie is ontworpen om de productiviteit van ontwikkelaars te verhogen wanneer zij Terraform met Azure schrijven, testen en gebruiken. De extensie biedt opdrachtondersteuning voor Terraform, visualisatie van resourcegrafieken en CloudShell-integratie in Visual Studio Code.

In dit artikel leert u het volgende:
> [!div class="checklist"]
> * Terraform gebruiken om de inrichting van Azure-services te automatiseren en te vereenvoudigen.
> * de Microsoft Terraform Visual Studio Code-extensie voor Azure-services installeren en gebruiken.
> * Visual Studio Code gebruiken om Terraform-plannen te schrijven, te plannen en uit te voeren.

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Terraform**: [Terraform installeren en configureren](/azure/virtual-machines/linux/terraform-install-configure).

- **Visual Studio Code**: installeer de versie van [Visual Studio Code](https://code.visualstudio.com/download) die bij uw omgeving past.

## <a name="prepare-your-dev-environment"></a>Uw ontwikkelaarsomgeving voorbereiden

### <a name="install-git"></a>Git installeren

U moet [Git installeren](https://git-scm.com/) om de oefeningen in het artikel te voltooien.

### <a name="install-hashicorp-terraform"></a>HashiCorp Terraform installeren

Volg de instructies op de HashiCorp-webpagina [Terraform installeren](https://www.terraform.io/intro/getting-started/install.html). Deze behandelen:

- Het downloaden van Terraform
- Het installeren van Terraform
- Het controleren of Terraform goed is geïnstalleerd

>[!Tip]
>Volg de instructies met betrekking tot het instellen van uw PATH-systeemvariabele.

### <a name="install-nodejs"></a>Node.js installeren

Als u Terraform in de Cloud Shell wilt gebruiken, moet u [Node.js](https://nodejs.org/) 6.0+ installeren.

>[!NOTE]
>U kunt controleren of Node.js is geïnstalleerd door een terminalvenster te openen en `node -v` in te voeren.

### <a name="install-graphviz"></a>GraphViz installeren

Als u de visualisatiefunctie van Terraform wilt gebruiken, moet u [GraphViz installeren](http://graphviz.org/).

>[!NOTE]
>U kunt controleren of GraphViz is geïnstalleerd door een terminalvenster te openen en `dot -V` in te voeren.

### <a name="install-the-azure-terraform-visual-studio-code-extension"></a>De Azure Terraform Visual Studio Code-extensie installeren

1. Start Visual Studio Code.

1. Selecteer **Extensies**.

    ![De knop Extensies](media/terraform-vscode-extension/tf-vscode-extensions-button.png)

1. Gebruik het tekstvak **Extensies zoeken in Marketplace** om de Azure Terraform-extensie te zoeken:

    ![Visual Studio Code-extensies zoeken in Marketplace](media/terraform-vscode-extension/tf-search-extensions.png)

1. Selecteer **Installeren**.

    >[!NOTE]
    >Wanneer u **Installeren** selecteert als Azure Terraform-extensie, wordt de Azure Account-extensie automatisch door Visual Studio Code geïnstalleerd. Azure Account is een afhankelijkheidsbestand voor de Azure Terraform-extensie, die wordt gebruikt om verificaties van Azure-abonnementen en aan Azure gerelateerde code-extensies uit te voeren.

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Controleren of de Terraform-extensie in Visual Studio Code is geïnstalleerd

1. Selecteer **Extensies**.

1. Voer `@installed` in het zoekvak in.

    ![Geïnstalleerde extensies](media/terraform-vscode-extension/tf-installed-extensions.png)

De Azure Terraform-extensie wordt in de lijst met geïnstalleerde extensies weergegeven.

![Geïnstalleerde Terraform-extensies](media/terraform-vscode-extension/tf-installed-terraform-extension-button.png)

U kunt nu alle ondersteunde Terraform-opdrachten in uw Cloud Shell-omgeving uitvoeren vanuit Visual Studio Code.

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>Oefening 1: overzicht van Terraform-basisopdrachten

In deze oefening gaat u een basisconfiguratiebestand van Terraform maken en uitvoeren waarmee een nieuwe Azure-resourcegroep wordt ingericht.

### <a name="prepare-a-test-plan-file"></a>Een testplanbestand voorbereiden

1. Selecteert **Bestand > Nieuw bestand** in de menubalk in Visual Studio Code.

1. Ga in uw browser naar de pagina [Terraform azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#) en kopieer de code in het codeblok **Gebruiksvoorbeeld**:

    ![Gebruiksvoorbeeld](media/terraform-vscode-extension/tf-azurerm-resource-group-example-usage.png)

1. Plak de gekopieerde code in het nieuwe bestand dat u in Visual Studio Code hebt gemaakt.

    ![Code van voorbeeldgebruik plakken](media/terraform-vscode-extension/tf-paste-example-usage-code.png)

    >[!NOTE]
    >U kunt de waarde **naam** van de resourcegroep wijzigen, maar dit moet wel een unieke naam voor uw Azure-abonnement zijn.

1. Selecteer **Bestand > Opslaan als** in de menubalk.

1. Navigeer in het dialoogvenster **Opslaan als** naar de gewenste locatie en selecteer vervolgens **Nieuwe map**. (Wijzig de naam van de nieuwe map in een naam die meer beschrijft dan alleen *Nieuwe map*.)

    >[!NOTE]
    >In dit voorbeeld noemen we de map TERRAFORM-TEST-PLAN.

1. Controleer of uw nieuwe map is gemarkeerd (geselecteerd) en selecteer **Openen**.

1. Wijzig de standaardnaam van het bestand in het dialoogvenster **Opslaan als** in *main.tf*.

    ![Opslaan als main.tf](media/terraform-vscode-extension/tf-save-as-main.png)

1. Selecteer **Opslaan**.
- In de menubalk selecteert u **Bestand > Map openen**. Navigeer naar de nieuwe map die u hebt gemaakt en selecteer deze map.

### <a name="run-terraform-init-command"></a>Voer de opdracht Terraform *init* uit

1. Start Visual Studio Code.

1. Ga naar de menubalk van Visual Studio Code, selecteer **Bestand > Map openen...** en zoek en selecteer het bestand *main.tf*.

    ![Het bestand main.tf](media/terraform-vscode-extension/tf-main-tf.png)

1. Selecteer **Weergeven > Opdrachtenpalet... > Azure Terraform: init** in de menubalk.

1. Zodra de bevestiging wordt weergegeven, selecteert u **OK**.

    ![Wilt u Cloud Shell openen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloud-shell.png)

1. Wanneer u Cloud Shell voor de eerste keer start vanuit een nieuwe map, wordt u gevraagd de webtoepassing in te stellen. Selecteer **Openen**.

    ![Eerste start van Cloud Shell](media/terraform-vscode-extension/tf-first-launch-of-cloud-shell.png)

1. De pagina Welkom bij Azure Cloud Shell wordt geopend. Selecteer Bash of PowerShell.

    ![Welkom bij Azure Cloud Shell](media/terraform-vscode-extension/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >In dit voorbeeld is Bash (Linux) geselecteerd.

1. Als u nog geen Azure-opslagaccount hebt ingesteld, wordt het volgende scherm weergegeven. Selecteer **Opslag maken**.

    ![U hebt geen opslag gekoppeld](media/terraform-vscode-extension/tf-you-have-no-storage-mounted.png)

1. Azure Cloud Shell wordt in de geselecteerde shell geopend en er wordt informatie weergegeven voor de cloudschijf die zojuist voor u is gemaakt.

    ![Uw cloudschijf is gemaakt](media/terraform-vscode-extension/tf-your-cloud-drive-has-been-created-in.png)

1. U kunt de Cloud Shell nu afsluiten

1. Selecteer **Weergeven** > **Opdrachtenpalet** > **Azure Terraform: init** in de menubalk.

    ![Terraform is geïnitialiseerd](media/terraform-vscode-extension/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>Het plan visualiseren

Eerder in deze zelfstudie hebt u GraphViz geïnstalleerd. Terraform kan GraphViz gebruiken om een visuele representatie te geven van ofwel een configuratie- of een uitvoeringsplan. Met de Azure Terraform Visual Studio Code-extensie wordt deze functie geïmplementeerd via de opdracht *visualiseren*.

- Selecteer **Weergeven > Opdrachtenpalet > Azure Terraform: Visualiseren** in de menubalk.

    ![Het plan visualiseren](media/terraform-vscode-extension/tf-graph.png)

### <a name="run-terraform-plan-command"></a>De opdracht *plannen* van Terraform uitvoeren

De Terraform-opdracht *plannen* wordt gebruikt om te controleren of het uitvoeringsplan voor een reeks wijzigingen doet wat u ervan verwacht.

>[!NOTE]
>Terraform *plannen* brengt geen wijzigingen aan in uw daadwerkelijke Azure-resources. Als u de wijzigingen in uw plan daadwerkelijk wilt doorvoeren, gebruiken we de Terraform-opdracht *toepassen*.

- Selecteer **Weergeven** > **Opdrachtenpalet** > **Azure Terraform: plan** in de menubalk.

    ![Terraform Plan](media/terraform-vscode-extension/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>De Terraform-opdracht *toepassen* uitvoeren

Als u tevreden bent met de resultaten van de Terraform-opdracht *plannen*, kunt u de opdracht *toepassen* uitvoeren.

1. Selecteer **Weergeven** > **Opdrachtenpalet** > **Azure Terraform: toepassen** in de menubalk.

    ![Terraform Apply](media/terraform-vscode-extension/tf-terraform-apply.png)

1. Voer `yes` in.

    ![Terraform Apply ja](media/terraform-vscode-extension/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>Controleer of uw Terraform-plan is uitgevoerd

U kunt als volgt zien of uw nieuwe Azure-resourcegroep is gemaakt:

1. Open Azure Portal.

1. Selecteer **Resourcegroepen** in het linkernavigatievenster.

    ![Bevestig uw nieuwe resource](media/terraform-vscode-extension/tf-verify-resource-group-created.png)

Uw nieuwe resourcegroep moet worden vermeld in de kolom **NAAM**.

>[!NOTE]
>U kunt het Azure Portal-venster voorlopig geopend laten; dit gebruiken we in de volgende stap.

### <a name="run-terraform-destroy-command"></a>De Terraform-opdracht *vernietigen* uitvoeren

1. Selecteer **Weergeven** > **Opdrachtenpalet** > **Azure Terraform: vernietigen** in de menubalk.

    ![Terraform vernietigen](media/terraform-vscode-extension/tf-terraform-destroy.png)

1. Voer *ja* in.

    ![Terraform vernietigen ja](media/terraform-vscode-extension/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>Controleer of uw resourcegroep is vernietigd

U kunt als volgt bevestigen dat Terraform uw nieuwe resourcegroep heeft vernietigd:

1. Selecteer **Vernieuwen** op de pagina **Resourcegroepen** in Azure Portal.

1. Uw resourcegroep wordt niet langer vermeld.

    ![Controleer of uw resourcegroep is vernietigd](media/terraform-vscode-extension/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>Oefening 2: de Terraform-module *berekenen*

In deze oefening leert u hoe u de Terraform-module *berekenen* in de Visual Studio Code-omgeving laadt.

### <a name="clone-the-terraform-azurerm-compute-module"></a>De module terraform-azurerm-compute klonen

1. Gebruik [deze koppeling](https://github.com/Azure/terraform-azurerm-compute) voor toegang tot de Terraform Azure Rm Compute-module in GitHub.

1. Selecteer **Klonen of downloaden**.

    ![Klonen of downloaden](media/terraform-vscode-extension/tf-clone-with-https.png)

    >[!NOTE]
    >In dit voorbeeld heeft onze map de naam *terraform-azurerm-compute* gekregen.

### <a name="open-the-folder-in-visual-studio-code"></a>Open de map in Visual Studio Code

1. Start Visual Studio Code.

1. Selecteer **Bestand > Map openen** in de menubalk en navigeer naar en selecteer de map die u hebt in de vorige stap hebt gemaakt.

    ![De map terraform-azurerm-compute](media/terraform-vscode-extension/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Terraform initialiseren

Voordat u de Terraform-opdrachten kunt gebruiken vanuit Visual Studio Code, moet u de invoegtoepassingen voor twee Azure-providers downloaden: random en azurerm.

1. Voer `terraform init` in het terminalvenster van de Visual Studio Code IDE in.

    ![De opdracht Terraform Init](media/terraform-vscode-extension/tf-terraform-init-command.png)

1. Voer `az login` in, druk op **<Invoeren** en volg de instructies op het scherm.

### <a name="module-test-lint"></a>Moduletest: *lint*

1. Selecteer **Weergeven > Opdrachtenpalet > Azure Terraform: Test uitvoeren** in de menubalk.

1. Selecteer **lint** in de lijst met testtype-opties.

    ![Type test selecteren](media/terraform-vscode-extension/tf-select-type-of-test-lint.png)

1. Als de bevestiging verschijnt, selecteert u **OK**, en volgt u de aanwijzingen op het scherm.

    ![Wilt u CloudShell openen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Wanneer u de test **lint** of **end-to-end** uitvoert, wordt door Azure een containerservice gebruikt om een testmachine in te richten om de daadwerkelijke test uit te voeren. Hierdoor worden uw testresultaten doorgaans binnen enkele minuten geretourneerd.

Na enkele ogenblikken ziet u een lijst in het Terminal-deelvenster dat vergelijkbaar is met het volgende voorbeeld:

![Linttestresultaten](media/terraform-vscode-extension/tf-lint-test-results.png)

### <a name="module-test-end-to-end"></a>Moduletest: *end-to-end*

1. Selecteer **Weergeven > Opdrachtenpalet > Azure Terraform: Test uitvoeren** in de menubalk.

1. Selecteer **end-to-end** in de lijst met testtype-opties.

    ![Type test selecteren](media/terraform-vscode-extension/tf-select-type-of-test-end-to-end.png)

1. Als de bevestiging verschijnt, selecteert u **OK**, en volgt u de aanwijzingen op het scherm.

    ![Wilt u CloudShell openen?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Wanneer u de test **lint** of **end-to-end** uitvoert, wordt door Azure een containerservice gebruikt om een testmachine in te richten om de daadwerkelijke test uit te voeren. Hierdoor worden uw testresultaten doorgaans binnen enkele minuten geretourneerd.

Na enkele ogenblikken ziet u een lijst in het Terminal-deelvenster dat vergelijkbaar is met het volgende voorbeeld:

![Resultaten van end-to-end-test](media/terraform-vscode-extension/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Lijst met de Terraform-modules die beschikbaar zijn voor Azure (en andere ondersteunde providers)](https://registry.terraform.io/)