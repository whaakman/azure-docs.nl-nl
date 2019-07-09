---
title: 'Zelfstudie: een Azure Red Hat OpenShift-cluster verwijderen | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een Azure Red Hat OpenShift-cluster met behulp van de Azure CLI verwijderen
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.topic: tutorial
ms.service: container-service
ms.date: 05/06/2019
ms.openlocfilehash: 0ad70f4c3681705377a350fee8b02a55c526f26c
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67669342"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-cluster"></a>Zelfstudie: Een Azure Red Hat OpenShift-cluster verwijderen

Dit is het einde van de zelfstudie. Wanneer u klaar bent met testen van de voorbeeld-cluster, volgt en alle bijbehorende resources verwijderd, zodat u niet betaalt voor wat u niet gebruikt.

In deel drie van de serie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster verwijderen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * [Een Azure Red Hat OpenShift-cluster maken](tutorial-create-cluster.md)
> * [De schaal van een Azure Red Hat OpenShift-cluster wijzigen](tutorial-scale-cluster.md)
> * Een Azure Red Hat OpenShift-cluster verwijderen

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Een cluster maken door de volgende de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

## <a name="step-1-sign-in-to-azure"></a>Stap 1: Aanmelden bij Azure

Als u de Azure CLI lokaal uitvoert, voert u `az login` zich aanmeldt bij Azure.

```bash
az login
```

Als u toegang tot meerdere abonnementen hebt, voert u `az account set -s {subscription ID}` vervangen `{subscription ID}` aan het abonnement dat u wilt gebruiken.

## <a name="step-2-delete-the-cluster"></a>Stap 2: Het cluster verwijderen

Open een terminal Bash en stel de variabele clusternaam op de naam van het cluster:

```bash
CLUSTER_NAME=yourclustername
```

Nu uw cluster verwijderen:

```bash
az openshift delete --resource-group $CLUSTER_NAME --name $CLUSTER_NAME
```

U wordt gevraagd of u wilt verwijderen van het cluster. Nadat u hebt bevestigd met `y`, het duurt enkele minuten om het cluster te verwijderen. Wanneer de opdracht is voltooid, wordt de hele resourcegroep en alle resources binnen deze, met inbegrip van het cluster worden verwijderd.

## <a name="deleting-a-cluster-using-the-azure-portal"></a>Verwijderen van een cluster met behulp van de Azure portal

U kunt ook de bijbehorende resourcegroep van het cluster via de online Azure-portal verwijderen. De naam van de resourcegroep is hetzelfde als de clusternaam van uw.

Op dit moment de `Microsoft.ContainerService/openShiftManagedClusters` -resource die is gemaakt bij het maken van het cluster is verborgen in de Azure-portal. In de `Resource group` weergave selectievakje `Show hidden types` om de resourcegroep weer te geven.

![Schermafbeelding van het selectievakje verborgen type](./media/aro-portal-hidden-type.png)

De resourcegroep verwijdert, worden alle gerelateerde resources die worden gemaakt wanneer u een Azure Red Hat OpenShift-cluster maakt.

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende geleerd:
> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster verwijderen

Meer informatie over het gebruik van OpenShift met de officiële [Red Hat OpenShift-documentatie](https://docs.openshift.com/aro/welcome/index.html)