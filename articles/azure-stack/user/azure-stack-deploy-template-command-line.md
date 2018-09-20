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
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: d40f9e8bf319f327879a71b23f9d9b82da924c2f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365431"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Sjablonen in Azure Stack via de opdrachtregel implementeren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

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

Als u wilt zien deze resource-groep en storage-account, gebruik de volgende opdrachten:

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het implementeren van sjablonen:

[Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)

