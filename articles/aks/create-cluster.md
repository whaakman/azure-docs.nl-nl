---
title: Een Azure Kubernetes Service (AKS)-cluster maken
description: Een cluster AKS maken met de CLI of Azure portal.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 304f3807a70179e4aab2ede80dc08a1aa85a2e51
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098903"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Een Azure Kubernetes Service (AKS)-cluster maken

Een Azure Kubernetes Service (AKS)-cluster kan worden gemaakt met de Azure CLI of Azure portal.

## <a name="azure-cli"></a>Azure-CLI

Gebruik de [az aks maken] [ az-aks-create] opdracht de cluster AKS maken.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

De volgende opties zijn beschikbaar met de `az aks create` opdracht. Zie de [Azure CLI verwijzing] [ az-aks-create] voor AKS voor meer informatie over elk van deze argumenten.

| Argument | Beschrijving | Vereist |
|---|---|:---:|
| `--name` `-n` | De bronnaam voor het beheerde cluster. | ja |
| `--resource-group` `-g` | Naam van de resourcegroep Azure Kubernetes Service. | ja |
| `--admin-username` `-u` | De gebruikersnaam voor de virtuele Linux-Machines.  Standaard: azureuser. | nee |
| `--aad-client-app-id` | (PREVIEW) De ID van een Azure Active Directory-clienttoepassing van het type 'Native'. | nee |
| `--aad-server-app-id` | (PREVIEW) De ID van de toepassing van een Azure Active Directory-server van het type 'Web-app /-API. | nee |
| `--aad-server-app-secret` | (PREVIEW) Het geheim van een Azure Active Directory-toepassing. | nee |
| `--aad-tenant-id` | (PREVIEW) De ID van een Azure Active Directory-tenant. | nee |
| `--admin-username` `-u` | Het gebruikersaccount te maken op het knooppunt VM's voor SSH-toegang.  Standaard: azureuser. | nee |
| ` --client-secret` | Het geheim dat is gekoppeld aan de service-principal. | nee |
| `--dns-name-prefix` `-p` | DNS-voorvoegsel voor het openbare ip-adres van clusters. | nee |
| `--dns-service-ip` | Een IP-adres wordt toegewezen aan de Kubernetes DNS-service. | nee |
| `--docker-bridge-address` | Een IP-adres en netmask toegewezen aan de Docker-brug. | nee |
| `--enable-addons` `-a` | Schakel de invoegtoepassingen Kubernetes in een door komma's gescheiden lijst. | nee |
| `--enable-rbac` `-r` | Schakel Kubernetes toegangsbeheer op basis van rollen. | nee |
| `--generate-ssh-keys` | SSH openbare en persoonlijke sleutelbestanden genereren indien deze ontbreken. | nee |
| `--kubernetes-version` `-k` | De versie van Kubernetes moet worden gebruikt voor het maken van het cluster, zoals '1.7.9' of '1.9.6'. | nee |
| `--locaton` `-l` | Locatie voor de automatisch gemaakte resourcegroep | nee |
| `--max-pods` `-m` | Het maximum aantal gehele product worden geïmplementeerd op een knooppunt. | nee |
| `--network-plugin` | De netwerk-invoegtoepassing voor Kubernetes te gebruiken. | nee |
| `--no-ssh-key` `-x` | Gebruik of maak een lokale SSH-sleutel niet. | nee |
| `--no-wait` | Wacht niet totdat de langlopende bewerking te voltooien. | nee |
| `--node-count` `-c` | Het standaardaantal knooppunten voor de knooppunt-adresgroepen.  Standaard: 3. | nee |
| `--node-osdisk-size` | De osDisk-grootte in GB aan knooppunt groep virtuele Machine. | nee |
| `--node-vm-size` `-s` | De grootte van de virtuele Machine.  Default: Standard_D1_v2. | nee |
| `--pod-cidr` | Een CIDR-notatie IP-adresbereik waaruit schil IP-adressen toewijzen bij kubenet wordt gebruikt. | nee |
| `--service-cidr` | Een CIDR-notatie IP-adresbereik waaruit de service-cluster IP-adressen toewijzen. | nee |
| `--service-principal` | Service-principal is gebruikt voor verificatie van de cluster. | nee |
| `--ssh-key-value` | SSH-sleutelbestand waarde of sleutel bestandspad.  Default: ~/.ssh/id_rsa.pub. | nee |
| `--tags` | Ruimte gescheiden labels in 'key [= value]' indeling. Gebruik '' bestaande labels wissen. | nee |
| `--vnet-subnet-id` | De ID van een subnet in een bestaand VNet waarin u voor het implementeren van het cluster. | nee |
| `--workspace-resource-id` | De resource-ID van een bestaande Log Analytics-werkruimte te gebruiken voor het opslaan van bewakingsgegevens. | nee |

## <a name="azure-portal"></a>Azure Portal

Zie voor instructies over het implementeren van een cluster AKS met de Azure-portal, de Azure Kubernetes Service (AKS) [Azure portal Quick Start][aks-portal-quickstart].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
