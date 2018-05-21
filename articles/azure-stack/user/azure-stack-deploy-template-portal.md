---
title: Sjablonen met behulp van de portal in Azure-Stack implementeren | Microsoft Docs
description: Informatie over het gebruik van de Azure-Stack-portal om sjablonen te implementeren.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 278f15271d3a5443102f5e387d3db1adb53fe7db
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Sjablonen met behulp van de Stack van Azure-portal implementeren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de portal gebruiken voor het implementeren van Azure Resource Manager-sjablonen op Azure-Stack.

Een sjabloon implementeren:

1. Aanmelden bij de portal, selecteert **nieuw**, en selecteer vervolgens **aangepaste**.
2. Selecteer **sjabloonimplementatie**.
3. Selecteer **template bewerken**, en plak de code van uw JSON-sjabloon in het codevenster. Selecteer **Opslaan**.
4. Selecteer **parameters bewerken**, geef waarden op voor de parameters die worden weergegeven, en selecteer vervolgens **OK**.
5. Selecteer **abonnement**. Kies het abonnement dat u wilt gebruiken en selecteer vervolgens **OK**.
6. Selecteer **resourcegroep**. Kies een bestaande resourcegroep of maak een nieuwe en selecteer vervolgens **OK**.
7. Selecteer **Maken**. Een nieuwe tegel op het dashboard houdt de voortgang van de sjabloonimplementatie van uw.

## <a name="next-steps"></a>Volgende stappen

[Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
