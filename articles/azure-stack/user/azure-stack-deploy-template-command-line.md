---
title: Sjablonen implementeren met de opdrachtregel in Azure Stack | Microsoft Docs
description: Informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) om sjablonen te implementeren naar Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: fec5378b9165ca7e240f52c629c47cdda799ef51
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54155179"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Sjablonen in Azure Stack via de opdrachtregel implementeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De opdrachtregel gebruiken om Azure Resource Manager-sjablonen in de Azure Stack Development Kit-omgeving te implementeren. Azure Resource Manager-sjablonen implementeren en inrichten van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking.

## <a name="before-you-begin"></a>Voordat u begint

- [Installeren en verbinding maken met](azure-stack-version-profiles-azurecli2.md) met Azure Stack met Azure CLI.
- Download de bestanden *azuredeploy.json* en *azuredeploy.parameters.json* uit de [voorbeeldsjabloon voor storage-account maken](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Sjabloon implementeren

Navigeer naar de map waarin u deze bestanden zijn gedownload en voer de volgende opdracht uit om de sjabloon te implementeren:

```azurecli
az group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json
```

Deze opdracht wordt de sjabloon geïmplementeerd naar de resourcegroep **cliRG** op de standaardlocatie van de Azure Stack POC.

## <a name="validate-template-deployment"></a>Sjabloonimplementatie valideren

Als u wilt zien deze resource-groep en storage-account, gebruikt u de volgende CLI-opdrachten:

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het implementeren van sjablonen:

[Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
