---
title: Azure CLI-voorbeeldscript - Een Azure Event Grid-abonnement maken | Microsoft Docs
description: In het Azure CLI-script in dit onderwerp ziet u hoe u een Event Grid-abonnement op accountniveau kunt maken voor wijzigingen in de taakstatus.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: a3cff649001adf569f1454d16a2a97b32972ef00
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612613"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>CLI-voorbeeld: Een Azure Event Grid-abonnement maken 

In het Azure CLI-script in dit artikel ziet u hoe u een Event Grid-abonnement op accountniveau kunt maken voor wijzigingen in de taakstatus.

## <a name="prerequisites"></a>Vereisten 

- Installeer en gebruik de CLI lokaal. Voor dit artikel dient u gebruik te maken van Azure CLI, versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Momenteel werken niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref)-opdrachten in Azure Cloud Shell. U wordt aangeraden de CLI lokaal te gebruiken.

- [Een Azure Media Services-account maken](../create-account-cli-how-to.md).

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Volgende stappen

Zie [Azure CLI-voorbeelden](../cli-samples.md) voor meer voorbeelden.
