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
ms.openlocfilehash: 7f4dced6f82622ba735b1b059f30d88830347fba
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625845"
---
## <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Uw cluster AKS voor het gebruik van Azure Dev spaties configureren

Open een opdrachtvenster en voer de volgende opdracht voor Azure CLI, met behulp van de resourcegroep die uw cluster AKS en de naam van uw cluster AKS bevat. De opdracht configureert het cluster met ondersteuning voor Azure Dev spaties.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

