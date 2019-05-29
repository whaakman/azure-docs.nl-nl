---
title: 'Zelfstudie: een Azure Red Hat OpenShift-cluster schalen | Microsoft Docs'
description: Meer informatie over het schalen van een Microsoft Azure Red Hat OpenShift-cluster met behulp van de Azure CLI
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/06/2019
ms.openlocfilehash: b25e17e7064006a1421142dfcd32997cb4426e8e
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66305972"
---
# <a name="tutorial-scale-an-azure-red-hat-openshift-cluster"></a>Zelfstudie: Een Azure Red Hat OpenShift-cluster schalen

Deze zelfstudie is deel twee van een serie. U leert hoe u een Microsoft Azure Red Hat OpenShift cluster maken met de Azure CLI, de schaal vervolgens verwijderen voor het opschonen van resources.

In deel twee van de serie leert u het volgende:

> [!div class="checklist"]
> * Een Red Hat OpenShift-cluster schalen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * [Een Azure Red Hat OpenShift-cluster maken](tutorial-create-cluster.md)
> * Een Azure Red Hat OpenShift-cluster schalen
> * [Een Azure Red Hat OpenShift-cluster verwijderen](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Een cluster maken door de volgende de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

## <a name="step-1-sign-in-to-azure"></a>Stap 1: Aanmelden bij Azure

Als u de Azure CLI lokaal uitvoert, voert u `az login` zich aanmeldt bij Azure.

```bash
az login
```

Als u toegang tot meerdere abonnementen hebt, voert u `az account set -s {subscription ID}` vervangen `{subscription ID}` aan het abonnement dat u wilt gebruiken.

## <a name="step-2-scale-the-cluster-with-additional-nodes"></a>Stap 2: Het cluster met extra knooppunten schalen

Stel de variabele clusternaam op de naam van uw cluster vanuit een terminal Bash:

```bash
CLUSTER_NAME=yourclustername
```

Nu gaan we het cluster met vijf knooppunten met de Azure CLI te schalen:

```bash
az openshift scale --resource-group $CLUSTER_NAME --name $CLUSTER_NAME --compute-count 5
```

Na een paar minuten `az openshift scale` wordt voltooid en retourneert een JSON-document met clusterdetails over de schaal.

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster schalen

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [Een Azure Red Hat OpenShift-cluster verwijderen](tutorial-delete-cluster.md)