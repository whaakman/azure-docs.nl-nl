---
title: Sjablonen met Visual Studio in Azure-Stack implementeren | Microsoft Docs
description: Informatie over het implementeren van sjablonen met Visual Studio in Azure-Stack.
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 8fc32dc50d96d202dfc982cbdc52d8e479c3a3eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Sjablonen in Azure-Stack met Visual Studio implementeren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Visual Studio gebruiken voor het implementeren van Azure Resource Manager-sjablonen op de Azure-Stack development kit.

1. [Installeren en verbinding maken met](azure-stack-install-visual-studio.md) naar Azure-Stack met Visual Studio.
2. Open Visual Studio.
3. Klik op **bestand**, klikt u op **nieuw**, en in de **nieuw Project** dialoogvenster vak Klik **Azure-resourcegroep**.
4. Voer een **naam** voor het nieuwe project en klik vervolgens op **OK**.
5. In de **Azure-sjabloon selecteren** wijziging van het dialoogvenster de *sjablonen weergeven van deze locatie* vervolgkeuzelijst voor **Azure Stack Quickstart**
6. Klik op **101-create-storage-account**, en klik vervolgens op **OK**.  
7. In het nieuwe project kunt u een lijst met beschikbare sjablonen bekijken door het uitbreiden van de **sjablonen** knooppunt in de **Solution Explorer** deelvenster.
8. In de **Solution Explorer** deelvenster met de rechtermuisknop op de naam van uw project, klikt u op **implementeren**, klikt u vervolgens op **nieuwe implementatie**.
9. In de **implementeren in resourcegroep** het dialoogvenster de **abonnement** vervolgkeuzelijst, selecteer uw abonnement voor Microsoft Azure-Stack.
10. In de **resourcegroep** lijst, kiest u een bestaande resourcegroep of maak een nieuwe.
11. In de **locatie voor resourcegroep** lijst en klik vervolgens op een locatie kiezen **implementeren**.
12. In de **Parameters bewerken** dialoogvenster Geef waarden voor de parameters (die verschillen per sjabloon), en klik vervolgens op **opslaan**.

## <a name="next-steps"></a>Volgende stappen
[Sjablonen implementeren met de opdrachtregel](azure-stack-deploy-template-command-line.md)

[Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)

