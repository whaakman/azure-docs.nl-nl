---
title: Een cluster Kubernetes maken met Azure Kubernetes Service (AKS) en Terraform
description: Zelfstudie ter illustratie van het maken van een Kubernetes Cluster met Azure Kubernetes Service en Terraform
keywords: terraform, devops, virtuele machine, azure, kubernetes
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/11/2018
ms.topic: article
ms.openlocfilehash: bd00a0cc8446802a03570edd58949a46c0769101
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303751"
---
# <a name="create-a-kubernetes-cluster-with-azure-kubernetes-service-and-terraform"></a>Een cluster Kubernetes maken met Azure Kubernetes Service en Terraform
[Azure Kubernetes Service (AKS)](/azure/aks/) uw gehoste omgeving Kubernetes snel en eenvoudig te implementeren en beheren van beperkte toepassingen zonder container orchestration expertise beheert. Het verlicht ook de last van actieve bewerkingen en onderhoud door inrichten, upgraden en bronnen op aanvraag schalen mogelijk te maken, zonder uw toepassingen offline te brengen.

In deze zelfstudie leert u hoe de volgende taken uitvoeren bij het maken van een [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) cluster met behulp van [Terraform](http://terraform.io) en AKS:

> [!div class="checklist"]
> * Lijst met compatibele hardware (HashiCorp Language) gebruiken voor het definiëren van een cluster Kubernetes
> * Gebruik Terraform en AKS voor het maken van een cluster Kubernetes
> * Het hulpprogramma kubectl gebruiken om de beschikbaarheid van een cluster Kubernetes testen

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Configureer Terraform**: Volg de instructies in het artikel [Terraform en toegang tot Azure configureren](/azure/virtual-machines/linux/terraform-install-configure)

- **Azure service-principal**: Volg de aanwijzingen in de sectie van de **maken van de service-principal** sectie in het artikel [een Azure-service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal). Noteer de waarden voor de appId, displayName, wachtwoord en tenant.

## <a name="create-the-directory-structure"></a>Maak de mapstructuur
De eerste stap is het maken van de map waarin de Terraform configuratiebestanden voor de oefening.

1. Blader naar de [Azure-portal](http://portal.azure.com).

1. Open [Azure-Cloud-Shell](/azure/cloud-shell/overview). Als u een omgeving eerder hebt geselecteerd, selecteert u **Bash** als uw omgeving.

    ![Shell-prompt voor cloud](./media/terraform-create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. Wijzig de mappen op de `clouddrive` directory.

    ```bash
    cd clouddrive
    ```

1. Maak een map met de naam `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-k8s
    ```

1. Wijzig de mappen in de nieuwe map:

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>De Azure provider declareren
Maak de Terraform-configuratiebestand dat declareert de Azure-provider.

1. Maak een bestand met de naam in de Cloud-Shell `main.tf`.

    ```bash
    vi main.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

    ```JSON
    provider "azurerm" {
        version = "=1.5.0"
    }

    terraform {
        backend "azurerm" {}
    }

    ```

1. De invoegmodus afsluiten door te selecteren de **Esc** sleutel.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="define-a-kubernetes-cluster"></a>Een cluster Kubernetes definiëren
Maak de Terraform-configuratiebestand dat de bronnen voor het cluster Kubernetes declareert.

1. Maak een bestand met de naam in de Cloud-Shell `k8s.tf`.

    ```bash
    vi k8s.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

    ```JSON
    resource "azurerm_resource_group" "k8s" {
        name     = "${var.resource_group_name}"
        location = "${var.location}"
    }

    resource "azurerm_kubernetes_cluster" "k8s" {
        name                = "${var.cluster_name}"
        location            = "${azurerm_resource_group.k8s.location}"
        resource_group_name = "${azurerm_resource_group.k8s.name}"
        dns_prefix          = "${var.dns_prefix}"

        linux_profile {
            admin_username = "ubuntu"

            ssh_key {
            key_data = "${file("${var.ssh_public_key}")}"
            }
        }

        agent_pool_profile {
            name            = "default"
            count           = "${var.agent_count}"
            vm_size         = "Standard_D2"
            os_type         = "Linux"
            os_disk_size_gb = 30
        }

        service_principal {
            client_id     = "${var.client_id}"
            client_secret = "${var.client_secret}"
        }

        tags {
            Environment = "Development"
        }
    }
    ```

    De bovenstaande code wordt de naam van het cluster, de locatie en de resource_group_name ingesteld. Bovendien wordt de waarde dns_prefix - dat deel uitmaakt van de volledig gekwalificeerde domeinnaam (FQDN) gebruikt voor toegang tot het cluster - is ingesteld.

    De **linux_profile** record kunt u de instellingen configureren die ondersteuning bieden voor aanmeldingen bij gebruik van SSH worker-knooppunten.

    Met AKS betaalt u alleen voor worker-knooppunten. De **agent_pool_profile** record configureert u de details voor deze worker-knooppunten. De **agent_pool_profile record** bevat het aantal worker-knooppunten te maken en het type worker-knooppunten. Als u wilt opschalen of terugschroeven het cluster in de toekomst, wijzigt u de **aantal** waarde in deze record.

1. De invoegmodus afsluiten door te selecteren de **Esc** sleutel.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="declare-the-variables"></a>De variabelen declareren

1. Maak een bestand met de naam in de Cloud-Shell `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

    ```JSON
    variable "client_id" {}
    variable "client_secret" {}

    variable "agent_count" {
        default = 3
    }

    variable "ssh_public_key" {
        default = "~/.ssh/id_rsa.pub"
    }

    variable "dns_prefix" {
        default = "k8stest"
    }

    variable cluster_name {
        default = "k8stest"
    }

    variable resource_group_name {
        default = "azure-k8stest"
    }

    variable location {
        default = "Central US"
    }
    ```

1. De invoegmodus afsluiten door te selecteren de **Esc** sleutel.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="create-a-terraform-output-file"></a>Maak een Terraform-bestand voor uitvoer
[Terraform levert](https://www.terraform.io/docs/configuration/outputs.html) kunt u waarden die worden gemarkeerd voor de gebruiker definiëren wanneer Terraform een plan geldt en kunnen worden opgevraagd met de `terraform output` opdracht. In deze sectie maakt u een bestand voor uitvoer die toegang tot het cluster met verleent [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/).

1. Maak een bestand met de naam in de Cloud-Shell `output.tf`.

    ```bash
    vi output.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

    ```JSON
    output "client_key" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_key}"
    }

    output "client_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate}"
    }

    output "cluster_ca_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate}"
    }

    output "cluster_username" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.username}"
    }

    output "cluster_password" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.password}"
    }

    output "kube_config" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config_raw}"
    }

    output "host" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.host}"
    }
    ```

1. De invoegmodus afsluiten door te selecteren de **Esc** sleutel.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Azure-opslag instellen voor het opslaan van Terraform status
Terraform houdt status lokaal de `terraform.tfstate` bestand. Dit patroon werkt goed in een omgeving met één persoon. In een omgeving met praktischer meerdere personen, u moet echter status bijhouden op het gebruik van de server [Azure storage](/azure/storage/). In deze sectie kunt u de accountgegevens voor de benodigde opslag (accountnaam en accountsleutel) ophalen, en maken van een opslagcontainer waarin de Terraform statusinformatie wordt opgeslagen.

1. Selecteer in de Azure-portal **alle services** in het menu links.

1. Selecteer **opslagaccounts**.

1. Op de **opslagaccounts** tabblad, selecteert u de naam van het opslagaccount waarin Terraform voor het opslaan van de status is. U kunt bijvoorbeeld het storage-account gemaakt wanneer u de eerste keer Cloud Shell geopend.  De opslagaccountnaam die is gemaakt door Cloud Shell doorgaans begint met `cs` gevolgd door een willekeurige tekenreeks van letters en cijfers. **Vergeet niet de naam van het opslagaccount dat u selecteert, deze later nodig.**

1. Selecteer op het tabblad opslag account **toegangssleutels**.

    ![Storage-account menu](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. Noteer de **key1** **sleutel** waarde. (Het pictogram aan de rechterkant van de sleutel te selecteren wordt de waarde gekopieerd naar het Klembord.)

    ![Toegangssleutels voor opslag-account](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. In de Cloud-Shell, een container in uw Azure storage-account maken (vervangen door de &lt;YourAzureStorageAccountName > en &lt;YourAzureStorageAccountAccessKey > tijdelijke aanduidingen door de juiste waarden voor uw Azure storage-account ).

    ```bash
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Maken van het cluster Kubernetes
In deze sectie ziet u hoe u de `terraform init` opdracht voor het maken van de resources de configuratiebestanden die u hebt gemaakt in de vorige secties gedefinieerd.

1. In de Cloud-Shell initialiseren Terraform (vervangen door de &lt;YourAzureStorageAccountName > en &lt;YourAzureStorageAccountAccessKey > tijdelijke aanduidingen door de juiste waarden voor uw Azure storage-account).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    De `terraform init` opdracht geeft u het succes van het initialiseren van de back-end en provider-invoegtoepassing:

    ![Voorbeeld van resultaten voor 'terraform init'](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. Voer de `terraform plan` opdracht om het plan Terraform die, de Infrastructuurelementen definieert te maken. De opdracht wordt u gevraagd om twee waarden: **var.client_id** en **var.client_secret**. Voor de **var.client_id** variabele, voer de **appId** waarde gekoppeld aan uw service-principal. Voor de **var.client_secret** variabele, voer de **wachtwoord** waarde gekoppeld aan uw service-principal.

    ```bash
    terraform plan -out out.plan
    ```

    De `terraform plan` opdracht geeft u de resources die worden gemaakt tijdens het uitvoeren van de `terraform apply` opdracht:

    ![Voorbeeld van resultaten voor 'terraform plan'](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. Voer de `terraform apply` opdracht toe te passen van het plan om de Kubernetes-cluster te maken. Het maken van een cluster Kubernetes kan enkele minuten duren, wat resulteert in de Cloud Shell sessie time-out. Als de Cloud Shell sessietime-out optreedt, kunt u de stappen in de sectie ['Herstellen uit een Cloud-Shell time-out'](#recover-from-a-dloud-shell-timeout) waarmee u kunt de zelfstudie hebt voltooid.

    ```bash
    terraform apply out.plan
    ```

    De `terraform apply` opdracht geeft u de resultaten van het maken van de resources in uw configuratiebestanden gedefinieerd:

    ![Voorbeeld van resultaten 'terraform toepassen'](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. Selecteer in de Azure-portal **alle services** in het menu links om te zien van de resources die zijn gemaakt voor het nieuwe cluster met Kubernetese.

    ![Shell-prompt voor cloud](./media/terraform-create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Herstellen van een Cloud-Shell-time-out
Als de Cloud Shell sessietime-out optreedt, kunt u de volgende stappen uit om te herstellen uitvoeren:

1. Een Cloud-Shell-sessie starten.

1. Ga naar de map met de configuratiebestanden Terraform.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Voer de volgende opdracht uit:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Het cluster Kubernetes testen
De Kubernetes's kunnen worden gebruikt om te controleren of het nieuwe cluster.

1. De configuratie van de Kubernetes ophalen uit de status van de Terraform en sla het in een bestand dat kubectl kunnen lezen.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. Stel een omgevingsvariabele zodat kubectl neemt over de juiste configuratie.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Controleer of de status van het cluster.

    ```bash
    kubectl get nodes
    ```

    Ziet u de details van uw worker-knooppunten, en ze moeten alle hebben een status **gereed**, zoals wordt weergegeven in de volgende afbeelding:

    ![Het hulpprogramma kubectl kunt u de status van uw cluster Kubernetes controleren](./media/terraform-create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Terraform en AKS gebruiken om een Kubernetes-cluster te maken. Hier volgen enkele aanvullende resources waarmee u kunt meer informatie over Terraform op Azure: 

 [Terraform-Hub in Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure provider-documentatie](http://aka.ms/terraform)  
 [Terraform Azure provider bron](http://aka.ms/tfgit)  
 [Terraform Azure-modules](http://aka.ms/tfmodules)