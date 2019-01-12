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
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 4a8d4ab4d93831abbd9489d9023dd9b6f5c66d6d
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246888"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Sjablonen implementeren met behulp van de Azure Stack-portal

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De portal kunt u Azure Resource Manager-sjablonen implementeren met Azure Stack.

## <a name="to-deploy-a-template"></a>Om een sjabloon te implementeren

1. Aanmelden bij de portal, selecteer **+ een resource maken**, en selecteer vervolgens **aangepaste**.
2. Selecteer **Sjabloonimplementatie**.
3. Selecteer **template bewerken**, en plak de code van uw JSON-sjabloon in het code-venster. Selecteer **Opslaan**.
4. Selecteer **parameters bewerken**, geef waarden op voor de parameters die worden weergegeven, en selecteer vervolgens **OK**.
5. Selecteer **abonnement**. Kies het abonnement dat u wilt gebruiken, en selecteer vervolgens **OK**.
6. Selecteer **resourcegroep**. Kies een bestaande resourcegroep of maak een nieuw en selecteer vervolgens **OK**.
7. Selecteer **Maken**. Een nieuwe tegel op het dashboard houdt de voortgang van de sjabloonimplementatie van uw.

## <a name="next-steps"></a>Volgende stappen

Zie het volgende artikel voor meer informatie over het implementeren van sjablonen:

- [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
