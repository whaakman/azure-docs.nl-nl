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
ms.openlocfilehash: c30f4b9de279f8c02b7f6bc7fa7d9765972899b1
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50977428"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>De Service Fabric Mesh CLI instellen
De Service Fabric Mesh CLI (opdrachtregelinterface) is vereist voor het implementeren en beheren van resources in Service Fabric Mesh. 

Voor de preview is de Azure Service Fabric-NET CLI geschreven als een uitbreiding voor Azure CLI. U kunt deze installeren in de Azure Cloud Shell of een lokale installatie van Azure CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-service-fabric-mesh-cli-locally"></a>De Azure Service Fabric Mesh CLI lokaal installeren
Als u liever de Azure CLI installeert en lokaal gebruikt, moet u versie 2.0.43 of hoger van Azure CLI installeren. Voer `az --version` uit om de versie te bekijken. Raadpleeg [De Azure CLI installeren][azure-cli-install] voor informatie over het installeren van of upgraden naar de nieuwste versie van de CLI.

Installeer de Azure Service Fabric Mesh CLI-uitbreidingsmodule met behulp van de volgende opdracht. 

```azurecli-interactive
az extension add --name mesh
```

Voer de volgende opdracht uit voor het bijwerken van een bestaande Azure Service Fabric Mesh CLI-module.

```azurecli-interactive
az extension update --name mesh
```
## <a name="next-steps"></a>Volgende stappen

U kunt ook uw [Windows-ontwikkelomgeving](service-fabric-mesh-howto-setup-developer-environment-sdk.md) instellen.

Zoek antwoorden op [veelgestelde vragen en problemen](service-fabric-mesh-faq.md).

[azure-cli-install]: /cli/azure/install-azure-cli
