---
title: Sjablonen implementeren met Visual Studio in Azure Stack | Microsoft Docs
description: Leer hoe u sjablonen implementeren met Visual Studio in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: b0460faca3b9ac6d208a9edf474f6ebb7189cba8
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244032"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Sjablonen in Azure Stack met Visual Studio implementeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Visual Studio kunt u Azure Resource Manager-sjablonen implementeren met Azure Stack.

## <a name="to-deploy-a-template"></a>Om een sjabloon te implementeren

1. [Installeren en verbinding maken met](azure-stack-install-visual-studio.md) met Azure Stack met Visual Studio.
2. Open Visual Studio.
3. Selecteer **bestand**, en selecteer vervolgens **nieuw**. In **nieuw Project**, selecteer **Azure-resourcegroep**.
4. Voer een **naam** voor het nieuwe project en selecteer vervolgens **OK**.
5. In **Azure-sjabloon selecteren**, kies **Azure Stack Quickstart** uit de vervolgkeuzelijst.
6. Selecteer **101-create-storage-account**, en selecteer vervolgens **OK**.
7. Vouw in het nieuwe project, de **sjablonen** knooppunt in **Solution Explorer** om te zien van de beschikbare sjablonen.
8. In **Solution Explorer**, kies de naam van uw project en selecteer vervolgens **implementeren**. Selecteer **nieuwe implementatie**.
9. In **implementeren in resourcegroep**, gebruikt u de **abonnement** vervolgkeuzelijst om uw Microsoft Azure Stack-abonnement te selecteren.
10. Uit de **resourcegroep** lijst, kiest u een bestaande resourcegroep of maak een nieuwe.
11. Uit de **resourcegroeplocatie** lijst, kies een locatie en selecteer vervolgens **implementeren**.
12. In **Parameters bewerken**, geef waarden op voor de parameters (die variëren per sjabloon), en selecteer vervolgens **opslaan**.

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met de opdrachtregel](azure-stack-deploy-template-command-line.md)
* [Sjablonen ontwikkelen voor Azure Stack](azure-stack-develop-templates.md)
