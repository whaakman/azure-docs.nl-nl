---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: abb8b6bed6766ff0ea85eab1434014a057af4ca3
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68843184"
---
## <a name="publish-the-project-to-azure"></a>Het project naar Azure publiceren

In Visual Studio Code kunt u uw functieproject rechtstreeks in Azure publiceren. In dit proces maakt u een functie-app en de bijbehorende resources in uw Azure-abonnement. De functie-app biedt een context waar u uw functies kunt uitvoeren. Het project wordt in uw Azure-abonnement verpakt en geïmplementeerd in de nieuwe functie-app.

Visual Studio code maakt standaard alle Azure-resources die nodig zijn om uw functie-app te maken. De namen van deze resources zijn gebaseerd op de naam van de functie-app die u kiest. Als u volledige controle over de gemaakte resources nodig hebt, kunt u in plaats daarvan [publiceren met behulp van geavanceerde opties](../articles/azure-functions/functions-develop-vs-code.md#enable-publishing-with-advanced-create-options).

In deze sectie wordt ervan uitgegaan dat u een nieuwe functie-app in azure maakt.

> [!IMPORTANT]
> Als u in een bestaande functie-app publiceert, wordt de inhoud van die app in Azure overschreven.

1. Druk in Visual Studio code op F1 om het opdracht palet te openen. In het opdracht palet zoekt en selecteert u `Azure Functions: Deploy to function app...`.

1. Als u zich niet hebt aangemeld, wordt u gevraagd u aan **te melden bij Azure**. U kunt ook **een gratis Azure-account maken**. Nadat u zich hebt aangemeld vanuit de browser, gaat u terug naar Visual Studio code. 

1. Als u meerdere abonnementen hebt, **selecteert u een abonnement** voor de functie-app en kiest u **+ nieuwe functie-app maken in azure**.

1. Typ een unieke naam waarmee de functie-app wordt geïdentificeerd en druk op Enter. Geldige tekens voor de naam van en functie-app zijn `a-z`, `0-9` en `-`.

    Wanneer u op ENTER drukt, worden de volgende Azure-resources in uw abonnement gemaakt:

    * **[Resource groep](../articles/azure-resource-manager/resource-group-overview.md)** : Bevat alle gemaakte Azure-resources. De naam is gebaseerd op de naam van uw functie-app.
    * **[Opslag account](../articles/storage/common/storage-quickstart-create-account.md)** : Een standaard opslag account wordt gemaakt met een unieke naam die is gebaseerd op de naam van uw functie-app.
    * **[Hosting plan](../articles/azure-functions/functions-scale.md)** : Er wordt een verbruiks plan gemaakt in de regio vs-West om uw serverloze functie-app te hosten.
    * **Functie-app**: Uw project wordt geïmplementeerd naar en wordt uitgevoerd in deze nieuwe functie-app.

    Nadat de functie-app is gemaakt en het implementatiepakket is toegepast, wordt er een melding weergegeven. Selecteer **uitvoer weer geven** in deze melding om de resultaten voor het maken en implementeren weer te geven, inclusief de Azure-resources die u hebt gemaakt.

1. Terug in **Azure: In** het gebied functies vouwt u de nieuwe functie-app uit onder uw abonnement. Vouw **functies**uit, klik met de rechter muisknop op **http trigger**en kies vervolgens **functie-URL kopiëren**.

    ![Kopieer de functie-URL voor de nieuwe HTTP-trigger](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
