---
title: Azure CLI-voorbeeldscript - Een asset publiceren | Microsoft Docs
description: Gebruik het Azure CLI-script om een asset te publiceren.
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
ms.openlocfilehash: 1ce85c18b93f22e9078e551deda74c6e6ff0b28b
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614919"
---
# <a name="cli-example-publish-an-asset"></a>CLI-voorbeeld: Een asset publiceren

In het Azure CLI-script in dit artikel ziet u hoe u een streaming-locator kunt maken en streaming-URL's kunt terugkrijgen. 

## <a name="prerequisites"></a>Vereisten 

- Installeer en gebruik de CLI lokaal. Voor dit artikel dient u gebruik te maken van Azure CLI, versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Momenteel werken niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref)-opdrachten in Azure Cloud Shell. U wordt aangeraden de CLI lokaal te gebruiken.

- [Een Azure Media Services-account maken](../create-account-cli-how-to.md).

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/publish-asset/Publish-Asset.sh "Publish an asset")]

## <a name="next-steps"></a>Volgende stappen

Zie [Azure CLI-voorbeelden](../cli-samples.md) voor meer voorbeelden.
