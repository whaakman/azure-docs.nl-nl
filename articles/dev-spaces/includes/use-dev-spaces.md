---
title: bestand opnemen
description: bestand opnemen
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 2563f7c36283521541562bcd88f973d86a6f672a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
## <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Uw cluster AKS voor het gebruik van Azure Dev spaties configureren

Open een opdrachtvenster en voert de volgende Azure CLI-opdrachten, met behulp van de resourcegroep die uw cluster AKS en de naam van uw cluster AKS bevat:

   ```cmd
   az extension add --name dev-spaces-preview 
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```
De eerste opdracht wordt een extensie in de Azure CLI ondersteuning toevoegen voor Azure Dev spaties geïnstalleerd en de tweede configureert uw cluster met ondersteuning voor Azure Dev spaties.
