---
title: Maken van een cluster Azure Kubernetes Service (AKS)
description: Een AKS-cluster maken met de CLI of Azure portal.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5c133c61c989bf19be3e84287cb76a7d110dccc8
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440470"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Maken van een cluster Azure Kubernetes Service (AKS)

Een cluster Azure Kubernetes Service (AKS) kan worden gemaakt met de Azure CLI of Azure portal.

## <a name="azure-cli"></a>Azure-CLI

Gebruik de [az aks maken] [ az-aks-create] opdracht om de AKS-cluster te maken.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

De volgende opties zijn beschikbaar met de `az aks create` opdracht. Zie de [Azure CLI-verwijzing] [ az-aks-create] voor AKS voor meer informatie over elk van deze argumenten.

| Argument | Beschrijving | Vereist |
|---|---|:---:|
| `--name` `-n` | De naam van de resource voor het beheerde cluster. | ja |
| `--resource-group` `-g` | De naam van de resourcegroep van de Azure Kubernetes Service. | ja |
| `--admin-username` `-u` | Gebruikersnaam voor de virtuele Linux-Machines.  Standaard: azureuser. | nee |
| `--aad-client-app-id` | (PREVIEW) De ID van een Azure Active Directory-clienttoepassing van het type 'Native'. | nee |
| `--aad-server-app-id` | (PREVIEW) De ID van een Azure Active Directory-toepassing van het type 'Web-app/API'. | nee |
| `--aad-server-app-secret` | (PREVIEW) Het geheim van een Azure Active Directory-toepassing. | nee |
| `--aad-tenant-id` | (PREVIEW) De ID van een Azure Active Directory-tenant. | nee |
| `--admin-username` `-u` | Het gebruikersaccount te maken op knooppunt VM's voor SSH-toegang.  Standaard: azureuser. | nee |
| ` --client-secret` | Het geheim dat is gekoppeld aan de service-principal. | nee |
| `--dns-name-prefix` `-p` | DNS-voorvoegsel voor het openbare ip-adres van clusters. | nee |
| `--dns-service-ip` | Een IP-adres toegewezen aan de Kubernetes-DNS-service. | nee |
| `--docker-bridge-address` | Een IP-adres en netmasker die worden toegewezen aan de brug Docker. | nee |
| `--enable-addons` `-a` | Schakel de Kubernetes-invoegtoepassingen in een door komma's gescheiden lijst. | nee |
| `--enable-rbac` `-r` | Kubernetes rollen gebaseerd toegangsbeheer inschakelen. | nee |
| `--generate-ssh-keys` | SSH openbare en persoonlijke sleutelbestanden genereren als ontbreekt. | nee |
| `--kubernetes-version` `-k` | De versie van Kubernetes te gebruiken voor het maken van het cluster, zoals '1.7.9' of '1.9.6'. | nee |
| `--locaton` `-l` | Locatie voor de automatisch gemaakte resourcegroep | nee |
| `--max-pods` `-m` | Het maximale aantal schillen kan worden geïmplementeerd naar een knooppunt. | nee |
| `--network-plugin` | De netwerk-invoegtoepassing voor Kubernetes te gebruiken. | nee |
| `--no-ssh-key` `-x` | Gebruik of maak een lokale SSH-sleutel niet. | nee |
| `--no-wait` | Wacht niet tot de langdurige bewerking te voltooien. | nee |
| `--node-count` `-c` | Het standaardaantal knooppunten voor de groepen.  Standaard: 3. | nee |
| `--node-osdisk-size` | De osDisk grootte in GB aan knooppuntgroep virtuele Machine. | nee |
| `--node-vm-size` `-s` | De grootte van de virtuele Machine.  Default: Standard_D1_v2. | nee |
| `--pod-cidr` | Een CIDR-notatie IP-adresbereik van waaruit u IP-adressen toewijzen bij kubenet wordt gebruikt. | nee |
| `--service-cidr` | Een CIDR-notatie IP-adresbereik waaruit u wilt toewijzen van IP-adressen van de service-cluster. | nee |
| `--service-principal` | Service-principal wordt gebruikt voor verificatie van de cluster. | nee |
| `--ssh-key-value` | SSH-sleutelbestand waarde of sleutel bestandspad.  Default: ~/.ssh/id_rsa.pub. | nee |
| `--tags` | Ruimte gescheiden labels in 'key [= value]' indeling. Gebruik '' om te wissen van de bestaande tags. | nee |
| `--vnet-subnet-id` | De ID van een subnet in een bestaand VNet waarin het cluster te implementeren. | nee |
| `--workspace-resource-id` | De resource-ID van een bestaande Log Analytics-werkruimte te gebruiken voor het opslaan van gegevens. | nee |

## <a name="azure-portal"></a>Azure Portal

Zie voor instructies over het implementeren van een AKS-cluster met de Azure-portal, Azure Kubernetes Service (AKS) [Azure portal-snelstartgids][aks-portal-quickstart].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
