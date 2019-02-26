---
title: Over het maken van een Kubernetes-cluster ingeschakeld voor Azure Dev ruimten met behulp van Azure Cloud Shell | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: article
description: Meer informatie over het snel maken van een Kubernetes-cluster ingeschakeld voor Azure Dev opslagruimten direct vanuit uw browser zonder iets te installeren.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: 81d8315224550791001610a9c32c2e2daee7e99d
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56823770"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Maak een Kubernetes-cluster met behulp van Azure Cloud Shell

U kunt [Azure Cloud Shell](/azure/cloud-shell) gebruiken om een cluster te maken voor Azure Dev Spaces met behulp van de **uitproberen** knop op deze pagina. Als u niet bent aangemeld, volg de aanwijzingen voor het aanmelden met een Azure-account en vervolgens typt u de opdrachten bij de Azure Cloud Shell-prompt wanneer deze wordt weergegeven.

## <a name="create-the-cluster"></a>Het cluster maken

Maak eerst de resourcegroep. Gebruik een van de momenteel ondersteunde regio's (EastUS, EastUS2, CentralUS, WestUS2, WestEurope, SoutheastAsia, CanadaCentral of CanadaEast).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Maak een Kubernetes-cluster met de volgende opdracht:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.10.9
```

Het duurt een paar minuten om het cluster te maken.  Als het gereed is wordt de uitvoer weergegeven in de JSON-indeling. Zoek naar `provisioningState` en controleer of er `Succeeded` staat.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Dev spaties](/azure/dev-spaces/) voor koppelingen naar volledige zelfstudies.
