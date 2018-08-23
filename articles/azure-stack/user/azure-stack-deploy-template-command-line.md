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
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 7814552256f17c5265bbeb4ce8c069dd8dca1bb2
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055738"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Sjablonen in Azure Stack via de opdrachtregel implementeren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De opdrachtregel gebruiken voor het implementeren van Azure Resource Manager-sjablonen in de Azure Stack Development Kit. Azure Resource Manager-sjablonen implementeren en inrichten van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking.

## <a name="before-you-begin"></a>Voordat u begint
 - [Installeren en verbinding maken met](azure-stack-version-profiles-azurecli2.md) met Azure Stack met Azure CLI
 - Download de bestanden *azuredeploy.json* en *azuredeploy.parameters.json* uit de [voorbeeldsjabloon voor storage-account maken](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).
 
## <a name="deploy-template"></a>Sjabloon implementeren
Navigeer naar de map waar deze bestanden zijn gedownload en voer de volgende opdracht uit om de sjabloon te implementeren:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Deze opdracht wordt de sjabloon geïmplementeerd naar de resourcegroep **cliRG** op de standaardlocatie van de Azure Stack POC.

## <a name="validate-template-deployment"></a>Sjabloonimplementatie valideren
Als u wilt zien deze resource-groep en storage-account, gebruik de volgende opdrachten:

    azure group list

    azure storage account list




