---
title: De Azure Service Fabric Mesh CLI instellen | Microsoft Docs
description: Informatie over hoe u de Azure Service Fabric Mesh CLI instelt.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/26/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 567f2afdea44f439779212c61fb3a129f4f979be
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281569"
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
az extension add --source https://meshcli.blob.core.windows.net/cli/mesh-0.9.1-py2.py3-none-any.whl
```

U kunt ook uw [Windows-ontwikkelomgeving](service-fabric-mesh-howto-setup-developer-environment-sdk.md) instellen.

[azure-cli-install]: /cli/azure/install-azure-cli