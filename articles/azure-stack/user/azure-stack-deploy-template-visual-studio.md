---
title: Sjablonen met Visual Studio in Azure-Stack implementeren | Microsoft Docs
description: Informatie over het implementeren van sjablonen met Visual Studio in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 6cd722fedc0483e37ce6ee491d74a7c985111353
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605120"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Sjablonen in Azure-Stack met Visual Studio implementeren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt Visual Studio gebruiken voor het implementeren van Azure Resource Manager-sjablonen op Azure-Stack.

## <a name="to-deploy-a-template"></a>Om een sjabloon te implementeren

1. [Installeren en verbinding maken met](azure-stack-install-visual-studio.md) naar Azure-Stack met Visual Studio.
2. Open Visual Studio.
3. Selecteer **bestand**, en selecteer vervolgens **nieuw**. In **nieuw Project**, selecteer **Azure-resourcegroep**.
4. Voer een **naam** voor het nieuwe project en selecteer vervolgens **OK**.
5. In **Azure-sjabloon selecteren**, kies **Azure Stack Quickstart** uit de vervolgkeuzelijst.
6. Selecteer **101-create-storage-account**, en selecteer vervolgens **OK**.
7. Vouw in het nieuwe project de **sjablonen** knooppunt in **Solution Explorer** om te zien van de beschikbare sjablonen.
8. In **Solution Explorer**, kies de naam van uw project en selecteer vervolgens **implementeren**. Selecteer **nieuwe implementatie**.
9. In **implementeren in resourcegroep**, gebruiken de **abonnement** vervolgkeuzelijst om uw Microsoft Azure-Stack-abonnement te selecteren.
10. Van de **resourcegroep** lijst, kiest u een bestaande resourcegroep of maak een nieuwe.
11. Van de **locatie voor resourcegroep** lijst, kies een locatie en selecteer vervolgens **implementeren**.
12. In **Parameters bewerken**, geef waarden op voor de parameters (die verschillen per sjabloon), en selecteer vervolgens **opslaan**.

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met de opdrachtregel](azure-stack-deploy-template-command-line.md)
* [Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)
