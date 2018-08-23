---
title: Sjablonen implementeren met behulp van de portal in Azure Stack | Microsoft Docs
description: Informatie over het gebruik van de Azure Stack-portal om sjablonen te implementeren.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 931c3d8beb9f2ed12228c74f09f84bbdee1798b8
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060785"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Sjablonen implementeren met behulp van de Azure Stack-portal

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De portal kunt u Azure Resource Manager-sjablonen implementeren met Azure Stack.

## <a name="to-deploy-a-template"></a>Om een sjabloon te implementeren

1. Aanmelden bij de portal, selecteer **nieuw**, en selecteer vervolgens **aangepaste**.
2. Selecteer **sjabloonimplementatie**.
3. Selecteer **template bewerken**, en plak de code van uw JSON-sjabloon in het code-venster. Selecteer **Opslaan**.
4. Selecteer **parameters bewerken**, geef waarden op voor de parameters die worden weergegeven, en selecteer vervolgens **OK**.
5. Selecteer **abonnement**. Kies het abonnement dat u wilt gebruiken, en selecteer vervolgens **OK**.
6. Selecteer **resourcegroep**. Kies een bestaande resourcegroep of maak een nieuw en selecteer vervolgens **OK**.
7. Selecteer **Maken**. Een nieuwe tegel op het dashboard houdt de voortgang van de sjabloonimplementatie van uw.

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
