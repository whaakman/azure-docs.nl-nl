---
title: Over het maken van een Kubernetes-cluster ingeschakeld voor Azure Dev ruimten met behulp van Azure Cloud Shell | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 10/04/2018
ms.topic: article
description: Meer informatie over het snel maken van een Kubernetes-cluster ingeschakeld voor Azure Dev opslagruimten direct vanuit uw browser zonder iets te installeren.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: f10a84a602ce152d5c428525aa50f678b50c8b41
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48871567"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Maak een Kubernetes-cluster met behulp van Azure Cloud Shell

U kunt [Azure Cloud Shell](/azure/cloud-shell) voor het maken van een cluster voor Azure Dev ruimten met behulp van de **uitproberen** knop op deze pagina. Als u niet bent aangemeld, volg de aanwijzingen voor het aanmelden met een Azure-account en vervolgens typt u de opdrachten bij de Azure Cloud Shell-prompt wanneer deze wordt weergegeven.

## <a name="create-the-cluster"></a>Het cluster maken

Maak eerst de resourcegroep. Gebruik een van de momenteel ondersteunde regio's (VS-Oost, CentralUS, WestUS2, Europa West, CanadaCentral of CanadaEast).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Maak een Kubernetes-cluster met de volgende opdracht uit:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.2 --enable-addons http_application_routing
```

Het duurt een paar minuten om het cluster te maken.  Als u klaar bent, wordt de uitvoer wordt weergegeven in de JSON-indeling. Zoek naar `provisioningState` en controleer of er `Succeeded`.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Dev spaties](/azure/dev-spaces/) voor koppelingen naar volledige zelfstudies.