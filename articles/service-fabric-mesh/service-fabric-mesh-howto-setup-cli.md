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
ms.openlocfilehash: cf899e236b7fa21a90a0784e1e2b4fce0feae055
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213432"
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

U kunt ook uw [Windows-ontwikkelomgeving](service-fabric-mesh-howto-setup-developer-environment-sdk.md) instellen.

[azure-cli-install]: /cli/azure/install-azure-cli