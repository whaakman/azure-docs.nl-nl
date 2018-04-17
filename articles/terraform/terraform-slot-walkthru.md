---
title: Terraform met Azure provider implementatiesites
description: Zelfstudie over het gebruik van Terraform met Azure provider implementatiesites
keywords: terraform, devops, Azure, implementatiesites virtuele machine
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 4/05/2018
ms.topic: article
ms.openlocfilehash: 3a018dbaf90801604b13efcf8bd7afb6dbc68659
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="use-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Terraform gebruiken voor het inrichten van infrastructuur met Azure implementatiesites

U kunt [Azure implementatiesites](/azure/app-service/web-sites-staged-publishing) wisselen tussen verschillende versies van uw app. Mogelijkheid kunt u de impact van verbroken implementaties te minimaliseren. 

In dit artikel ziet u een voorbeeld van het gebruik van implementatiesites door u door de implementatie van apps in twee stappen via GitHub en Azure. Een app wordt gehost in een productiesleuf. De tweede app wordt gehost in een faseringssleuf gewisseld. (De naam 'productie' en 'fasering' willekeurige zijn en mag elke gewenste die staat voor uw scenario). Nadat u uw implementatiesites hebt geconfigureerd, kunt u Terraform wisselen tussen de twee sleuven indien nodig.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **GitHub-account**: U moet een [GitHub](http://www.github.com) vertakken en gebruik de toets GitHub-repo-account.

## <a name="create-and-apply-the-terraform-plan"></a>Maken en toepassen van het plan Terraform

1. Blader naar de [Azure-portal](http://portal.azure.com).

1. Open [Azure-Cloud-Shell](/azure/cloud-shell/overview). Als u een omgeving eerder hebt geselecteerd, selecteert u **Bash** als uw omgeving.

    ![Shell-prompt voor cloud](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. Wijzig de mappen op de `clouddrive` directory.

    ```bash
    cd clouddrive
    ```

1. Maak een map met de naam `deploy`.

    ```bash
    mkdir deploy
    ```

1. Maak een map met de naam `swap`.

    ```bash
    mkdir swap
    ```

1. Gebruik de `ls` bash om te controleren of u beide directory's is gemaakt.

    ![Cloud-shell na het maken van mappen](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Wijzig de mappen op de `deploy` directory.

    ```bash
    cd deploy
    ```

1. Met behulp van de [vi editor](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html), maakt u een bestand met de naam `deploy.tf`. Dit bestand bevat de [Terraform configuratie](https://www.terraform.io/docs/configuration/index.html).

    ```bash
    vi deploy.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

    ```JSON
    # Configure the Azure provider
    provider "azurerm" { }

    resource "azurerm_resource_group" "slotDemo" {
        name = "slotDemoResourceGroup"
        location = "westus2"
    }

    resource "azurerm_app_service_plan" "slotDemo" {
        name                = "slotAppServicePlan"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        sku {
            tier = "Standard"
            size = "S1"
        }
    }

    resource "azurerm_app_service" "slotDemo" {
        name                = "slotAppService"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
    }

    resource "azurerm_app_service_slot" "slotDemo" {
        name                = "slotAppServiceSlotOne"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
        app_service_name    = "${azurerm_app_service.slotDemo.name}"
    }
    ```

1. Selecteer de Esc-toets om af te sluiten van de invoegmodus.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

1. Nu dat u het bestand hebt gemaakt, Controleer of de inhoud ervan.

    ```bash
    cat deploy.tf
    ```

1. Terraform initialiseren.

    ```bash
    terraform init
    ```

1. Het plan Terraform maken.

    ```bash
    terraform plan
    ```

1. Inrichten van de resources die zijn gedefinieerd in de `deploy.tf` configuratiebestand. (Bevestig de actie door te voeren `yes` bij de opdrachtprompt.)

    ```bash
    terraform apply
    ```

1. Sluit het venster Cloud Shell.

1. Selecteer in het hoofdmenu van de Azure portal **resourcegroepen**.

    ![De selectie 'Resourcegroepen' in de portal](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. Op de **resourcegroepen** tabblad **slotDemoResourceGroup**.

    ![Resourcegroep gemaakt door Terraform](./media/terraform-slot-walkthru/resource-group.png)

U ziet nu de resources die Terraform heeft gemaakt.

![Resources die zijn gemaakt door Terraform](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Vertakken van het testproject

Voordat u het maken en wisselen in en uit de implementatiesites testen kunt, moet u het testproject vanuit GitHub vertakken.

1. Blader naar de [geweldig terraform opslagplaats op GitHub](https://github.com/Azure/awesome-terraform).

1. Fork de **geweldig terraform** opslagplaats.

    ![De GitHub-repo-fantastisch terraform vertakken](./media/terraform-slot-walkthru/fork-repo.png)

1. Volg de prompts om te vertakken aan uw omgeving.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Implementeren vanuit GitHub op uw implementatiesites

Nadat u de test-repo-project vertakken, configureert u de implementatiesites via de volgende stappen uit:

1. Selecteer in het hoofdmenu van de Azure portal **resourcegroepen**.

1. Selecteer **slotDemoResourceGroup**.

1. Selecteer **slotAppService**.

1. Selecteer **implementatieopties**.

    ![Implementatie-opties voor een App Service-resource](./media/terraform-slot-walkthru/deployment-options.png)

1. Op de **Implementatieoptie** tabblad **bron kiezen**, en selecteer vervolgens **GitHub**.

    ![Implementatiebron selecteren](./media/terraform-slot-walkthru/select-source.png)

1. Nadat Azure de verbinding maakt en alle opties wordt weergegeven, selecteert u **autorisatie**.

1. Op de **autorisatie** tabblad **autoriseren**, en de referenties die Azure toegang nodig heeft tot uw GitHub-account op te geven. 

1. Nadat Azure uw GitHub-referenties gevalideerd, wordt een bericht wordt weergegeven en staat dat het verificatieproces is voltooid. Selecteer **OK** sluiten de **autorisatie** tabblad.

1. Selecteer **Kies uw organisatie** en uw organisatie.

1. Selecteer **Kies project**.

1. Op de **Kies project** tabblad de **geweldig terraform** project.

    ![Kies het project fantastisch terraform](./media/terraform-slot-walkthru/choose-project.png)

1. Selecteer **Kies vertakking**.

1. Op de **Kies vertakking** tabblad **master**.

    ![Kies de hoofdvertakking](./media/terraform-slot-walkthru/choose-branch-master.png)

1. Op de **Implementatieoptie** tabblad **OK**.

U hebt op dit moment de productiesite geïmplementeerd. Voer voor het implementeren van de faseringssleuf alle van de vorige stappen in deze sectie met de volgende wijzigingen aanbrengen:

- Selecteer in stap 3, de **slotAppServiceSlotOne** resource.

- Selecteer de werkvertakking in plaats van de hoofdvertakking in stap 13.

    ![Kies de werkvertakking](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Testen van de app-implementaties

In de vorige secties stelt u twee sleuven--**slotAppService** en **slotAppServiceSlotOne**--voor het implementeren van verschillende filialen in GitHub. Een voorbeeld bekijken we de web-apps om te valideren dat ze zijn geïmplementeerd.

Voer de volgende stappen uit twee keer. In stap 3, selecteert u **slotAppService** de eerste keer, en selecteer vervolgens **slotAppServiceSlotOne** de tweede keer.

1. Selecteer in het hoofdmenu van de Azure portal **resourcegroepen**.

1. Selecteer **slotDemoResourceGroup**.

1. Selecteer een **slotAppService** of **slotAppServiceSlotOne**.

1. Selecteer op de overzichtspagina **URL**.

    ![Selecteer de URL op het overzichtstabblad weergeven van de app.](./media/terraform-slot-walkthru/resource-url.png)

> [!NOTE]
> Het kan enkele minuten duren voordat Azure bouwen en implementeren van de site vanuit GitHub.
>
>

Voor de **slotAppService** web-app, ziet u een blauwe pagina met een paginatitel van **sleuf Demo-App 1**. Voor de **slotAppServiceSlotOne** web-app, ziet u een groene pagina met een paginatitel van **sleuf Demo-App 2**.

![Voorbeeld bekijken van de apps als u wilt testen dat ze correct zijn geïmplementeerd](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>De twee implementatiesites wisselen

Als u wilt testen wisselen van de twee implementatiesites, moet u de volgende stappen uitvoeren:
 
1. Overschakelen naar het browsertabblad met **slotAppService** (de app met de blauwe pagina). 

1. Terug naar de Azure-portal op een afzonderlijke tabblad.

1. Open Cloud-Shell.

1. Wijzig de mappen op de **clouddrive/wisselen** directory.

    ```bash
    cd clouddrive/swap
    ```

1. Maak een bestand met de naam met behulp van de editor vi `swap.tf`.

    ```bash
    vi swap.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

    ```JSON
    # Configure the Azure provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Selecteer de Esc-toets om af te sluiten van de invoegmodus.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

1. Terraform initialiseren.

    ```bash
    terraform init
    ```

1. Het plan Terraform maken.

    ```bash
    terraform plan
    ```

1. Inrichten van de resources die zijn gedefinieerd in de `swap.tf` configuratiebestand. (Bevestig de actie door te voeren `yes` bij de opdrachtprompt.)

    ```bash
    terraform apply
    ```

1. Nadat Terraform is voltooid wisselen van de sleuven, Ga terug naar de browser die wordt gerenderd de **slotAppService** web-app en vernieuw de pagina. 

De web-app in uw **slotAppServiceSlotOne** faseringssleuven heeft is gewisseld met de productiesite en wordt nu in het groen weergegeven. 

![De implementatiesites hebben zijn gewisseld](./media/terraform-slot-walkthru/slots-swapped.png)

Als u wilt terugkeren naar de oorspronkelijke productieversie van de app, pas de Terraform-plan dat u hebt gemaakt van de `swap.tf` configuratiebestand.

```bash
terraform apply
```

Nadat de app zijn uitgewisseld, ziet u de oorspronkelijke configuratie.