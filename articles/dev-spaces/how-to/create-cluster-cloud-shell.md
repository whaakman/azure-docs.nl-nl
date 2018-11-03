---
title: Over het maken van een Kubernetes-cluster ingeschakeld voor Azure Dev ruimten met behulp van Azure Cloud Shell | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 10/04/2018
ms.topic: article
description: Meer informatie over het snel maken van een Kubernetes-cluster ingeschakeld voor Azure Dev opslagruimten direct vanuit uw browser zonder iets te installeren.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: 47c467e020a7a9253daa636352352d9a57dddf28
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978144"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Maak een Kubernetes-cluster met behulp van Azure Cloud Shell

U kunt [Azure Cloud Shell](/azure/cloud-shell) voor het maken van een cluster voor Azure Dev ruimten met behulp van de **uitproberen** knop op deze pagina. Als u niet bent aangemeld, volg de aanwijzingen voor het aanmelden met een Azure-account en vervolgens typt u de opdrachten bij de Azure Cloud Shell-prompt wanneer deze wordt weergegeven.

## <a name="create-the-cluster"></a>Het cluster maken

Maak eerst de resourcegroep. Gebruik een van de momenteel ondersteunde regio's (VS-Oost, EastUS2, CentralUS, WestUS2, Europa West, SoutheastAsia, CanadaCentral of CanadaEast).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Maak een Kubernetes-cluster met de volgende opdracht:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.3 --enable-addons http_application_routing
```

Het duurt een paar minuten om het cluster te maken.  Als u klaar bent, wordt de uitvoer wordt weergegeven in de JSON-indeling. Zoek naar `provisioningState` en controleer of er `Succeeded`.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Dev spaties](/azure/dev-spaces/) voor koppelingen naar volledige zelfstudies.
