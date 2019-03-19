---
title: Valideren van een hub en spoke-netwerk met Terraform in Azure
description: Zelfstudie voor het valideren van de hub en spoke-topologie met alle virtuele netwerken met elkaar verbonden.
services: terraform
ms.service: azure
keywords: terraform, hub en spoke, netwerken, hybride netwerken, devops, virtuele machine, azure, vnet-peering
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 03/01/2019
ms.openlocfilehash: 157be65a19a1f790b911aa9d861c5f18fc8c0813
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58006170"
---
# <a name="tutorial-validate-a-hub-and-spoke-network-with-terraform-in-azure"></a>Zelfstudie: Valideren van een hub en spoke-netwerk met Terraform in Azure

In dit artikel hebt uitvoeren u de terraform bestanden die zijn gemaakt in het vorige artikel in deze reeks. Het resultaat is een validatie van de verbinding tussen de demo van virtuele netwerken.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * HCL (HashiCorp Language) gebruiken voor het implementeren van de Hub VNet in hub-spoke-topologie
> * Gebruik Terraform-abonnement om te controleren of de resources worden geïmplementeerd
> * Gebruik Terraform toepassen voor het maken van de resources in Azure
> * Controleer of de verbinding tussen de verschillende netwerken
> * Alle resources verwijderen met behulp van Terraform

## <a name="prerequisites"></a>Vereisten

1. [Maken van een hub- en -spokenetwerktopologie hybride met Terraform in Azure](./terraform-hub-spoke-introduction.md).
1. [On-premises virtueel netwerk maken met Terraform in Azure](./terraform-hub-spoke-on-prem.md).
1. [Een virtuele hub-netwerk maken met Terraform in Azure](./terraform-hub-spoke-hub-network.md).
1. [Een virtueel netwerkapparaat hub maken met Terraform in Azure](./terraform-hub-spoke-hub-nva.md).
1. [Een knooppunt virtuele netwerken maken met Terraform in Azure](./terraform-hub-spoke-spoke-network.md).

## <a name="verify-your-configuration"></a>Controleer de configuratie

Na het voltooien van de [vereisten](#prerequisites), controleren of de juiste configuratiebestanden weergegeven worden.

1. Blader naar de [Azure-portal](https://portal.azure.com).

1. Open [Azure Cloud Shell](/azure/cloud-shell/overview). Als u nog geen omgeving hebt geselecteerd, selecteert u **Bash** als uw omgeving.

    ![Cloud Shell-prompt](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. Ga naar de map `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Ga naar de nieuwe map:

    ```bash
    cd hub-spoke
    ```

1. Voer de `ls` opdracht uit om te controleren of de `.tf` configuratiebestanden in de vorige zelfstudies hebt gemaakt, worden weergegeven:

    ![Terraform demo-configuratiebestanden](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-config-files.png)

## <a name="deploy-the-resources"></a>De resources implementeren

1. De provider Terraform initialiseren:
    
    ```bash
    terraform init
    ```
    
    ![Van de voorbeeldresultaten van de opdracht "terraform init"](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-init.png)
    
1. Voer de `terraform plan` opdracht om te zien van het effect van de implementatie te bevestigen voordat kan worden uitgevoerd:

    ```bash
    terraform plan
    ```
    
    ![Van de voorbeeldresultaten van de opdracht "terraform plan"](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-plan.png)

1. De oplossing implementeert:

    ```bash
    terraform apply
    ```
    
    Voer `yes` wanneer hierom wordt gevraagd om te bevestigen van de implementatie.

    ![Van de voorbeeldresultaten van de opdracht 'terraform toepassen'](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-apply.png)
    
## <a name="test-the-hub-vnet-and-spoke-vnets"></a>Testen van de hub VNet en het knooppunt VNets

Deze sectie wordt beschreven hoe u test de verbinding van de gesimuleerde on-premises-omgeving met de hub VNet.

1. In de Azure-portal, blader naar de **on-premises-vnet-rg** resourcegroep.

1. In de **on-premises-vnet-rg** tabblad, selecteert u de virtuele machine met de naam **on-premises-vm**.

1. Selecteer **Verbinden**.

1. Naast de tekst **aanmelden met lokaal VM-account**, Kopieer de **ssh** opdracht naar het Klembord.

1. Uitvoeren vanaf een Linux-prompt `ssh` verbinding maken met de gesimuleerde on-premises omgeving. Gebruik het wachtwoord dat is opgegeven in de `on-prem.tf` parameterbestand.

1. Voer de `ping` opdracht test de verbinding met de jumpbox-VM in de hub VNet:

   ```bash
   ping 10.0.0.68
   ```

1. Voer de `ping` opdracht test de verbinding met de jumpbox-VM's in elk knooppunt:

   ```bash
   ping 10.1.0.68
   ping 10.2.0.68
   ```

1. Om af te sluiten de ssh-sessie op de **on-premises-vm** virtuele machine, voer `exit` en druk op &lt;Enter >.

## <a name="troubleshoot-vpn-issues"></a>Problemen met VPN

Zie het artikel voor informatie over het oplossen van VPN-fouten, [een hybride VPN-verbinding oplossen](/azure/architecture/reference-architectures/hybrid-networking/troubleshoot-vpn).

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer het niet meer nodig hebt, verwijdert u de resources in deze reeks zelfstudies hebt gemaakt.

1. De resources die zijn gedeclareerd in het plan verwijderen:

    ```bash
    terraform destroy
    ```

    Voer `yes` wanneer hierom wordt gevraagd de verwijdering van de resources te bevestigen.

1. Wijzig de mappen in de bovenliggende map:

    ```bash
    cd ..
    ```

1. Verwijder de `hub-scope` directory (met inbegrip van alle bestanden):

    ```bash
    rm -r hub-spoke
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Meer informatie over het gebruik van Terraform in Azure](/azure/terraform)