---
title: Azure CLI-voorbeeldscript - Een taak maken en verzenden | Microsoft Docs
description: In het Azure CLI-script in dit onderwerp ziet u hoe u een taak kunt verzenden naar een eenvoudige coderingstransformatie met behulp van de HTTPS-URL.
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
ms.openlocfilehash: 11e148181568f0c550b5eb094055c21c47c00cfc
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615820"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI-voorbeeld: Een taak maken en verzenden

In het Azure CLI-script in dit artikel ziet u hoe u een taak kunt maken en verzenden naar een eenvoudige coderingstransformatie met behulp van de HTTPS-URL.

## <a name="prerequisites"></a>Vereisten 

- Installeer en gebruik de CLI lokaal. Voor dit artikel dient u gebruik te maken van Azure CLI, versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Momenteel werken niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref)-opdrachten in Azure Cloud Shell. U wordt aangeraden de CLI lokaal te gebruiken.

- [Een Azure Media Services-account maken](../create-account-cli-how-to.md).

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-jobs/Create-Jobs.sh "Create and submit jobs")]

## <a name="next-steps"></a>Volgende stappen

Zie [Azure CLI-voorbeelden](../cli-samples.md) voor meer voorbeelden.
