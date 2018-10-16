---
title: Azure Terraform-modulegenerator voor VS Code
description: Informatie over het gebruik van Yeoman voor het maken van een Terraform-basissjabloon.
services: terraform
ms.service: terraform
keywords: terraform, devops, virtual machine, azure, yeoman
author: v-mavick
manager: jeconnoc
ms.author: v-mavick
ms.topic: tutorial
ms.date: 09/12/2018
ms.openlocfilehash: 513b123c44cf2cd37cf81a91e0d2da9599eb1fcd
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396252"
---
# <a name="create-a-terraform-base-template-using-yeoman"></a>Een Terraform-basissjabloon maken met behulp van Yeoman

[Terraform](https://docs.microsoft.com/azure/terraform/
) biedt een manier om eenvoudig een infrastructuur te maken in Azure. [Yeoman](http://yeoman.io/) vereenvoudigt de taak van de module-ontwikkelaar enorm wat betreft het maken van Terraform-modules, en biedt daarnaast een uitstekend *best practices*-framework.

In dit artikel leert u hoe u de modulegenerator van Yeoman gebruikt om een Terraform-basissjabloon te maken.

## <a name="prerequisites"></a>Vereisten

- Een computer waarop Windows 10, Linux of macOS 10.10+ wordt uitgevoerd.
- **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.
- **Visual Studio Code**: we gebruiken [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) om bestanden te onderzoeken die zijn gemaakt door de Yeoman-generator. U kunt echter ook een andere code-editor gebruiken.
- **Terraform**: u hebt een installatie van [Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ) nodig om de module uit te voeren die door Yeoman is gemaakt.
- **Docker**: we gebruiken [Docker](https://www.docker.com/get-started) om de module uit te voeren die door de Yeoman-generator is gemaakt. (Als u dat wilt, mag u ook Ruby in plaats van Docker gebruiken om de voorbeeldmodule uit te voeren.)
- **Go-programmeertaal**: u hebt een installatie van [Go](https://golang.org/) nodig omdat de testscenario's die door Yeoman zijn gegenereerd, zijn geschreven in Go.

>[!NOTE]
>De meeste procedures in deze zelfstudie hebben betrekking op opdrachtregelvermeldingen. De hier beschreven stappen zijn van toepassing op alle besturingssystemen en opdrachtregelprogramma's. In onze voorbeelden hebben we gekozen voor PowerShell. U kunt echter ook een van de vele alternatieven kiezen, zoals Git Bash, Windows-opdrachtprompts of Linux- of macOS-opdrachtregelopdrachten.

## <a name="prepare-your-environment"></a>Uw omgeving voorbereiden

### <a name="install-nodejs"></a>Node.js installeren

Als u Terraform in de Cloud Shell wilt gebruiken, moet u [Node.js](https://nodejs.org/en/download/) 6.0+ installeren.

>[!NOTE]
>Als u wilt controleren of Node.js is geïnstalleerd, opent u een terminalvenster en voert u `node --version` in.

### <a name="install-yeoman"></a>Yeoman installeren

Voer `npm install -g yo` in vanaf een opdrachtprompt

![Yeoman installeren](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>De Yeoman-sjabloon voor de Terraform-module installeren

Voer `npm install -g generator-az-terra-module` in vanaf een opdrachtprompt.

![Generator-az-terra-module installeren](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

>[!NOTE]
>Als u wilt controleren of Yeoman is geïnstalleerd, voert u in een terminalvenster `yo --version` in.

### <a name="create-an-empty-folder-to-hold-the-yeoman-generated-module"></a>Een lege map maken om de door Yeoman gegenereerde module in op te slaan

De Yeoman-sjabloon genereert bestanden in de **huidige map**. Daarom moet u een map maken.

>[!Note]
>Deze lege map moet onder $GOPATH/src worden geplaatst. De instructies om dit te doen, vindt u [hier](https://github.com/golang/go/wiki/SettingGOPATH).

Vanaf een opdrachtprompt:

1. Navigeer naar de bovenliggende map waaronder u de nieuwe, lege map wilt maken.
1. Voer `mkdir <new-directory-name>` in.

    >[!NOTE]
    >Vervang <new-directory-name> door de naam van de nieuwe map. In dit voorbeeld hebben we de nieuwe map `GeneratorDocSample` genoemd.

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. Navigeer naar de nieuwe map door `cd <new directory's name>` te typen en op **Enter** te drukken.

    ![Naar de nieuwe map navigeren](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

    >[!NOTE]
    >Als u wilt controleren of deze map is leeg, voert u `ls` in. Er mogen geen bestanden worden weergegeven in de resulterende uitvoer van deze opdracht.

## <a name="create-a-base-module-template"></a>Een basismodulesjabloon maken

Vanaf een opdrachtprompt:

1. Voer `yo az-terra-module` in.

1. Volg de aanwijzingen op het scherm om de volgende informatie op te geven:

    - *Projectnaam van Terraform-module*

        ![Projectnaam](media/terraform-vscode-module-generator/ymg-project-name.png)       

        >[!NOTE]
        >In dit voorbeeld hebben we `doc-sample-module` ingevoerd.

    - *Wilt u het Docker-installatiekopiebestand opnemen?*

        ![Docker-installatiekopiebestand opnemen?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

        >[!NOTE]
        >Voer `y` in. Als u **n** selecteert, zal de gegenereerde modulecode alleen ondersteuning bieden voor uitvoering in de systeemeigen modus.

3. Voer `ls` in om de resulterende bestanden weer te geven die zijn gemaakt.

    ![Gemaakte bestanden weergeven](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>De gegenereerde modulecode controleren

1. Visual Studio Code starten

1. Selecteer **Bestand > Map openen** in de menubalk en selecteer de map die u hebt gemaakt.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Nu gaan we enkele bestanden bekijken die door de Yeoman-modulegenerator zijn gemaakt.

>[!Note]
>In dit artikel gebruiken we de bestanden main.tf, variables.tf en outputs.tf die door de Yeoman-modulegenerator zijn gemaakt. Als u uw eigen modules maakt, moet u deze bestanden echter aanpassen aan de functionaliteit van uw Terraform-module. Zie [Terratest in Terraform-modules](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster) voor een uitgebreidere bespreking van deze bestanden en hun gebruik.

### <a name="maintf"></a>main.tf

Definieert een module met de naam *random-shuffle*. De invoer is een *string_list*. De uitvoer is het aantal permutaties.

### <a name="variablestf"></a>variables.tf

Definieert de invoer- en uitvoervariabelen die door de module worden gebruikt.

### <a name="outputstf"></a>outputs.tf

Definieert het resultaat van de module. Hier is het de waarde die wordt geretourneerd door **random_shuffle**, een ingebouwde Terraform-module.

### <a name="rakefile"></a>Rakefile

Definieert de buildstappen. Deze stappen omvatten:

- **build**: valideert de indeling van het bestand main.tf.
- **eenheid**: het gegenereerde moduleraamwerk bevat geen code voor een eenheidstest. Als u een scenario voor een eenheidstest wilt opgeven, moet u hier code toevoegen.
- **e2e**: voert een end-to-end-test van de module uit.

### <a name="test"></a>test

- Testcases worden geschreven in Go.
- Alle codes in test zijn end-to-end-tests.
- End-to-end-tests proberen Terraform te gebruiken om alle items in te richten die zijn gedefinieerd onder **aansluiting** en vergelijken vervolgens de uitvoer in de code **template_output.go** met de vooraf gedefinieerde verwachte waarden.
- **Gopkg.lock** en **Gopkg.toml**: definiëren uw afhankelijkheden. 

## <a name="test-the-module-using-docker"></a>De module testen met Docker

>[!NOTE]
>In ons voorbeeld voeren we de module uit als een lokale module, dus buiten Azure om.

### <a name="confirm-docker-is-installed-and-running"></a>Controleren of Docker is geïnstalleerd en wordt uitgevoerd

Voer `docker version` in vanaf een opdrachtprompt.

![Docker-versie](media/terraform-vscode-module-generator/ymg-docker-version.png)

De resulterende uitvoer bevestigt dat Docker is geïnstalleerd.

Als u wilt controleren of Docker werkelijk wordt uitgevoerd, typt u `docker info`.

![Docker-info](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Een Docker-container instellen

1. Voer vanaf een opdrachtprompt het volgende in:

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    Het bericht **Gemaakt** wordt weergegeven.

    ![Gemaakt](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. Voer vanaf de opdrachtregel `docker image ls` in.

    Nu wordt de zojuist gemaakte module *terra-mod-example* vermeld.

    ![Resultaten van de opslagplaats](media/terraform-vscode-module-generator/ymg-repository-results.png)

    >[!NOTE]
    >De naam van de module, *terra-mod-example*, is opgegeven in de opdracht die u in stap 1 hierboven hebt ingevoerd.

1. Voer `docker run -it terra-mod-example /bin/sh` in.

    U werkt nu in Docker en u kunt het bestand weergeven door `ls` in te voeren.

    ![Docker-bestand weergeven](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

1. Voer `bundle install` in.

    Wacht tot het bericht **Bundel voltooid** wordt weergegeven en ga dan verder met de volgende stap.

1. Voer `rake build` in.

    ![Rake-build](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="perform-the-end-to-end-test"></a>De end-to-end-test uitvoeren

1. Voer `rake e2e` in.

1. Na enkele ogenblikken wordt het bericht **GESLAAGD** weergegeven.

    ![GESLAAGD](media/terraform-vscode-module-generator/ymg-pass.png)

1. Voer `exit` in om de end-to-end-test uit te voeren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De Azure Terraform Visual Studio Code-extensie installeren en gebruiken.](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
