---
title: De Azure Service Fabric Mesh CLI instellen | Microsoft Docs
description: Informatie over hoe u de Azure Service Fabric Mesh CLI instelt.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: f56bcf0c844545e8883175da2bd3f22afdcd19ea
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089524"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>De Service Fabric Mesh CLI instellen
De Service Fabric Mesh CLI is vereist voor het implementeren en beheren van resources in Service Fabric Mesh. 

Voor de preview is de Azure Service Fabric-NET CLI geschreven als een uitbreiding voor Azure CLI. U kunt deze installeren in de Azure Cloud Shell of een lokale installatie van Azure CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Als u liever de Azure CLI installeert en lokaal gebruikt, moet u versie 2.0.35 of hoger van Azure CLI installeren. Voer `az --version` uit om de versie te bekijken. Raadpleeg [Azure CLI 2.0 installeren][azure-cli-install] voor informatie over het installeren van of upgraden naar de nieuwste versie van de CLI.

Verwijder eventuele vorige installaties van de Azure Service Fabric Mesh CLI-module.

```azurecli-interactive
az extension remove --name mesh
```

Installeer de Azure Service Fabric Mesh CLI-uitbreidingsmodule met behulp van de volgende opdracht. 

```azurecli-interactive
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

U kunt ook uw [Windows-ontwikkelomgeving instellen](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

[azure-cli-install]: /cli/azure/install-azure-cli