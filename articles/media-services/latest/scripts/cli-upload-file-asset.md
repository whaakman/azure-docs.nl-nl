---
title: Azure CLI-voorbeeldscript - Een bestand uploaden naar een container | Microsoft Docs
description: Gebruik het Azure CLI-script om een lokaal bestand te uploaden naar een opslagcontainer.
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
ms.openlocfilehash: 5120d938d137efef77eeb0b69a5bf571bd4c509b
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614494"
---
# <a name="cli-example-upload-a-local-file-to-a-container"></a>CLI-voorbeeld: Een lokaal bestand uploaden naar een container 

In het Azure CLI-script in dit artikel ziet u hoe u een lokaal bestand kunt uploaden naar een opslagcontainer.

## <a name="prerequisites"></a>Vereisten 

- Installeer en gebruik de CLI lokaal. Voor dit artikel dient u gebruik te maken van Azure CLI, versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Momenteel werken niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref)-opdrachten in Azure Cloud Shell. U wordt aangeraden de CLI lokaal te gebruiken.

- [Een Azure Media Services-account maken](../create-account-cli-how-to.md).

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/upload-file-asset/UploadFile-Asset.sh "Upload a file")]

## <a name="next-steps"></a>Volgende stappen

Zie [Azure CLI-voorbeelden](../cli-samples.md) voor meer voorbeelden.
